---
title: Exemplarische Vorgehensweise zum Erstellen einer Komponente für Windows-Runtime in C++/CX und Aufrufen der Komponente über JavaScript oder C#
description: In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie eine einfache Komponenten-DLL-Datei für Windows-Runtime erstellen, die von JavaScript, C# oder Visual Basic aufgerufen werden kann.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c7f0e1ba5c78ce41a5326d3643b5afe80f380b3c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155174"
---
# <a name="walkthrough-of-creating-a-ccx-windows-runtime-component-and-calling-it-from-javascript-or-c"></a>Exemplarische Vorgehensweise zum Erstellen einer Komponente für Windows-Runtime in C++/CX und Aufrufen der Komponente über JavaScript oder C#

> [!NOTE]
> Dieses Thema enthält Informationen, die Sie beim Verwalten Ihrer C++/CX-Anwendung unterstützen. Es wird jedoch empfohlen, [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) für neue Anwendungen zu verwenden. C++/WinRT ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet. Informationen zum Erstellen einer Windows-Runtime-Komponente mithilfe von C++/WinRT finden Sie unter [Windows-Runtime-Komponenten mit C++/WinRT](./create-a-windows-runtime-component-in-cppwinrt.md).

In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie eine einfache Komponenten-DLL-Datei für Windows-Runtime erstellen, die von JavaScript, C# oder Visual Basic aufgerufen werden kann. Bevor Sie mit dieser exemplarischen Vorgehensweise beginnen, stellen Sie sicher, dass Sie Konzepte wie die abstrakte binäre Schnittstelle (ABI), Verweisklassen und Visual C++-Komponentenerweiterungen kennen, die das Verwenden von Verweisklassen vereinfachen. Weitere Informationen finden Sie unter [Windows-Runtime Komponenten mit C++/CX](creating-windows-runtime-components-in-cpp.md) und [Visual C++-Sprachreferenz (C++/CX)](/cpp/cppcx/visual-c-language-reference-c-cx).

## <a name="creating-the-c-component-dll"></a>Erstellen der C++-Komponenten-DLL-Datei
In diesem Beispiel erstellen wir zunächst das Komponentenprojekt. Sie können aber auch das JavaScript-Projekt zuerst erstellen. Die Reihenfolge spielt keine Rolle.

Beachten Sie, dass die Hauptklasse der Komponente Beispiele für Eigenschafts- und Methodendefinitionen sowie eine Ereignisdeklaration enthält. Diese werden nur aus Gründen der Anschaulichkeit bereitgestellt. Sie sind nicht erforderlich, und in diesem Beispiel ersetzen wir den gesamten generierten Code durch unseren eigenen Code.

### <a name="to-create-the-c-component-project"></a>**So erstellen Sie das C++-Komponentenprojekt**
1. Wählen Sie in der Visual Studio-Menüleiste **Datei, neu und Projekt**aus.

2. Erweitern Sie im Dialogfeld **Neues Projekt** im linken Bereich **Visual C++**, und wählen Sie dann den Knoten für universelle Windows-Apps aus.

3. Wählen Sie im mittleren Bereich **Windows-Runtime Komponente** aus, und benennen Sie das Projekt mit WinRT \_ cpp.

4. Klicken Sie auf die Schaltfläche **OK** .

## <a name="to-add-an-activatable-class-to-the-component"></a>**So fügen Sie der Komponente eine aktivierbare Klasse hinzu**
Eine aktivierbare Klasse kann vom Clientcode mithilfe eines **new**-Ausdrucks (**New** in Visual Basic oder **ref new** in C++) erstellt werden. In der Komponente können Sie sie als **public ref class sealed** deklarieren. Die Class1.h- und CPP-Dateien verfügen bereits über eine Verweisklasse. Sie können den Namen ändern, aber in diesem Beispiel verwenden wir den Standardnamen – Class1. Sie können zusätzliche Verweisklassen oder Standardklassen in der Komponente definieren, falls sie benötigt werden. Weitere Informationen zu Verweisklassen finden Sie unter [Typsystem (C++/CX)](/cpp/cppcx/type-system-c-cx).

Fügen Sie \# Class1. h diese include-Direktiven hinzu:

```cpp
#include <collection.h>
#include <ppl.h>
#include <amp.h>
#include <amp_math.h>
```

„collection.h“ ist die Headerdatei für konkrete C++-Klassen wie die Platform::Collections::Vector-Klasse und die Platform::Collections::Map-Klasse, die sprachunabhängige, durch die Windows-Runtime definierte Schnittstellen implementieren. Die AMP-Header werden verwendet, um Berechnungen auf der GPU auszuführen. Sie haben keine Windows-Runtime-Entsprechungen. Das ist in Ordnung, da sie privat sind. Im Allgemeinen sollten Sie aus Leistungsgründen ISO-C++-Code und Standardbibliotheken intern innerhalb der Komponente verwenden. Nur die Windows-Runtime-Schnittstelle muss in Windows-Runtime-Typen ausgedrückt werden.

## <a name="to-add-a-delegate-at-namespace-scope"></a>So fügen Sie einen Delegaten im Gültigkeitsbereich des Namespaces hinzu
Ein Delegat ist ein Konstrukt, das die Parameter und den Rückgabetyp für Methoden definiert. Ein Ereignis ist eine Instanz eines bestimmten Delegattyps, und eine Ereignishandlermethode, die das Ereignis abonniert, muss über die Signatur verfügen, die im Delegat angegeben wird. Der folgende Code definiert einen Delegattyp, der „int“ erhält und „void“ zurückgibt. Der Code deklariert danach ein öffentliches Ereignis dieses Typs. Dadurch kann der Clientcode Methoden bereitstellen, die beim Auslösen des Ereignisses aufgerufen werden.

Fügen Sie die folgende Delegatdeklaration im Gültigkeitsbereich des Namespaces in „Class1.h“ unmittelbar vor der Class1-Deklaration hinzu.

```cpp
public delegate void PrimeFoundHandler(int result);
```

Wenn der Code beim Einfügen in Visual Studio nicht korrekt ausgerichtet wird, drücken Sie STRG+K+D, um den Einzug für die gesamte Datei zu korrigieren.

## <a name="to-add-the-public-members"></a>So fügen Sie öffentliche Member hinzu.
Die Klasse stellt drei öffentliche Methoden und ein öffentliches Ereignis bereit. Die erste Methode ist synchron, da sie immer sehr schnell ausgeführt wird. Da die anderen beiden Methoden einige Zeit in Anspruch nehmen können, sind sie asynchron, damit sie den UI-Thread nicht blockieren. Diese Methoden geben IAsyncOperationWithProgress und IAsyncActionWithProgress zurück. „IAsyncOperationWithProgress“ definiert eine asynchrone Methode, die ein Ergebnis zurückgibt, und „IAsyncActionWithProgress“ definiert eine asynchrone Methode, die „void“ zurückgibt. Diese Schnittstellen ermöglichen auch, dass der Clientcode Updates zum Status des Vorgangs erhält.

```cpp
public:

        // Synchronous method.
        Windows::Foundation::Collections::IVector<double>^  ComputeResult(double input);

        // Asynchronous methods
        Windows::Foundation::IAsyncOperationWithProgress<Windows::Foundation::Collections::IVector<int>^, double>^
            GetPrimesOrdered(int first, int last);
        Windows::Foundation::IAsyncActionWithProgress<double>^ GetPrimesUnordered(int first, int last);

        // Event whose type is a delegate "class"
        event PrimeFoundHandler^ primeFoundEvent;

```
## <a name="to-add-the-private-members"></a>So fügen Sie private Member hinzu
Die Klasse enthält drei private Member: zwei Hilfsmethoden für die numerischen Berechnungen und ein CoreDispatcher-Objekt, das verwendet wird, um die Ereignisaufrufe von Workerthreads zurück an den UI-Thread zu marshallen.

```cpp
private:
        bool is_prime(int n);
        Windows::UI::Core::CoreDispatcher^ m_dispatcher;
```

### <a name="to-add-the-header-and-namespace-directives"></a>So fügen Sie die Header- und Namespacedirektiven hinzu
1. Fügen Sie in „Class1.cpp“ die folgenden #include-Direktiven hinzu:

```cpp
#include <ppltasks.h>
#include <concurrent_vector.h>
```

2. Fügen Sie jetzt diese using-Anweisungen hinzu, um die erforderlichen Namespaces abzurufen:

```cpp
using namespace concurrency;
using namespace Platform::Collections;
using namespace Windows::Foundation::Collections;
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
```

## <a name="to-add-the-implementation-for-computeresult"></a>So fügen Sie die Implementierung für ComputeResult hinzu
Fügen Sie in „Class1.cpp“ die folgende Methodenimplementierung hinzu. Diese Methode wird synchron auf dem aufrufenden Thread ausgeführt, ist aber sehr schnell, da sie C++ AMP verwendet, um die Berechnung auf der GPU zu parallelisieren. Weitere Informationen finden Sie in der C++ AMP-Übersicht. Die Ergebnisse werden an einen konkreten Platform::Collections::Vector<T>-Typ angefügt, der bei der Rückgabe implizit in einen Windows::Foundation::Collections::IVector<T>-Typ konvertiert wird.

```cpp
//Public API
IVector<double>^ Class1::ComputeResult(double input)
{
    // Implement your function in ISO C++ or
    // call into your C++ lib or DLL here. This example uses AMP.
    float numbers[] = { 1.0, 10.0, 60.0, 100.0, 600.0, 10000.0 };
    array_view<float, 1> logs(6, numbers);

    // See http://msdn.microsoft.com/library/hh305254.aspx
    parallel_for_each(
        logs.extent,
        [=] (index<1> idx) restrict(amp)
    {
        logs[idx] = concurrency::fast_math::log10(logs[idx]);
    }
    );

    // Return a Windows Runtime-compatible type across the ABI
    auto res = ref new Vector<double>();
    int len = safe_cast<int>(logs.extent.size());
    for(int i = 0; i < len; i++)
    {      
        res->Append(logs[i]);
    }

    // res is implicitly cast to IVector<double>
    return res;
}
```
## <a name="to-add-the-implementation-for-getprimesordered-and-its-helper-method"></a>So fügen Sie die Implementierung für „GetPrimesOrdered“ und die Hilfsmethode hinzu
Fügen Sie die Implementierungen für „GetPrimesOrdered“ und die Hilfsmethode „is_prime“ in „Class1.cpp“ hinzu. „GetPrimesOrdered“ verwendet eine cConcurrent_vector-Klasse und eine parallel_for-Funktionsschleife zur Aufteilung der Arbeit und um die maximalen Ressourcen des Computers zu verwenden, auf dem das Programm ausgeführt wird, um Ergebnisse zu erzielen. Nachdem die Ergebnisse berechnet, gespeichert und sortiert wurden, werden sie einem Platform::Collections::Vector<T>-Typ hinzugefügt und als „Windows::Foundation::Collections::IVector<T>“ an den Clientcode zurückgegeben.

Beachten Sie den Code für den Statusbericht, der es dem Client ermöglicht, den Benutzer durch eine Statusanzeige oder ein anderes UI-Element darüber zu informieren, wie lange der Vorgang noch dauern wird. Statusberichte sind allerdings nicht kostenlos. Ein Ereignis muss auf der Komponentenseite ausgelöst und im UI-Thread behandelt werden, und der Statuswert muss bei jeder Iteration gespeichert werden. Eine Möglichkeit, die Kosten zu minimieren, ist das Begrenzen der Häufigkeit, mit der ein Statusereignis ausgelöst wird. Wenn die Kosten weiterhin unerschwinglich sind oder Sie die Länge des Vorgangs nicht einschätzen können, können Sie einen Statusring verwenden, der anzeigt, dass ein Vorgang ausgeführt wird, der aber nicht die restliche Zeit bis zum Abschluss anzeigt.

```cpp
// Determines whether the input value is prime.
bool Class1::is_prime(int n)
{
    if (n < 2)
        return false;
    for (int i = 2; i < n; ++i)
    {
        if ((n % i) == 0)
            return false;
    }
    return true;
}

// This method computes all primes, orders them, then returns the ordered results.
IAsyncOperationWithProgress<IVector<int>^, double>^ Class1::GetPrimesOrdered(int first, int last)
{
    return create_async([this, first, last]
    (progress_reporter<double> reporter) -> IVector<int>^ {
        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }
        // Perform the computation in parallel.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        parallel_for(first, last + 1, [this, &primes, &operation,
            range, &lastPercent, reporter](int n) {

                // Increment and store the number of times the parallel
                // loop has been called on all threads combined. There
                // is a performance cost to maintaining a count, and
                // passing the delegate back to the UI thread, but it's
                // necessary if we want to display a determinate progress
                // bar that goes from 0 to 100%. We can avoid the cost by
                // setting the ProgressBar IsDeterminate property to false
                // or by using a ProgressRing.
                if(InterlockedIncrement(&operation) % 100 == 0)
                {
                    reporter.report(100.0 * operation / range);
                }

                // If the value is prime, add it to the local vector.
                if (is_prime(n)) {
                    primes.push_back(n);
                }
        });

        // Sort the results.
        std::sort(begin(primes), end(primes), std::less<int>());        
        reporter.report(100.0);

        // Copy the results to a Vector object, which is
        // implicitly converted to the IVector return type. IVector
        // makes collections of data available to other
        // Windows Runtime components.
        return ref new Vector<int>(primes.begin(), primes.end());
    });
}
```

## <a name="to-add-the-implementation-for-getprimesunordered"></a>So fügen Sie die Implementierung für „GetPrimesUnordered“ hinzu
Der letzte Schritt zum Erstellen der C++-Komponente ist das Hinzufügen der Implementierung für „GetPrimesUnordered“ in „Class1.cpp“. Diese Methode gibt jedes einzelne Ergebnis sofort beim Auffinden zurück – ohne zu warten, bis alle Ergebnisse gefunden wurden. Jedes Ergebnis wird im Ereignishandler zurückgegeben und auf der Benutzeroberfläche in Echtzeit angezeigt. Beachten Sie auch hier, dass ein Statusbericht verwendet wird. Diese Methode verwendet auch die Hilfsmethode „is_prime“.

```cpp
// This method returns no value. Instead, it fires an event each time a
// prime is found, and passes the prime through the event.
// It also passes progress info.
IAsyncActionWithProgress<double>^ Class1::GetPrimesUnordered(int first, int last)
{

    auto window = Windows::UI::Core::CoreWindow::GetForCurrentThread();
    m_dispatcher = window->Dispatcher;


    return create_async([this, first, last](progress_reporter<double> reporter) {

        // Ensure that the input values are in range.
        if (first < 0 || last < 0) {
            throw ref new InvalidArgumentException();
        }

        // In this particular example, we don't actually use this to store
        // results since we pass results one at a time directly back to
        // UI as they are found. However, we have to provide this variable
        // as a parameter to parallel_for.
        concurrent_vector<int> primes;
        long operation = 0;
        long range = last - first + 1;
        double lastPercent = 0.0;

        // Perform the computation in parallel.
        parallel_for(first, last + 1,
            [this, &primes, &operation, range, &lastPercent, reporter](int n)
        {
            // Store the number of times the parallel loop has been called  
            // on all threads combined. See comment in previous method.
            if(InterlockedIncrement(&operation) % 100 == 0)
            {
                reporter.report(100.0 * operation / range);
            }

            // If the value is prime, pass it immediately to the UI thread.
            if (is_prime(n))
            {                
                // Since this code is probably running on a worker
                // thread, and we are passing the data back to the
                // UI thread, we have to use a CoreDispatcher object.
                m_dispatcher->RunAsync( CoreDispatcherPriority::Normal,
                    ref new DispatchedHandler([this, n, operation, range]()
                {
                    this->primeFoundEvent(n);

                }, Platform::CallbackContext::Any));

            }
        });
        reporter.report(100.0);
    });
}
```

## <a name="creating-a-javascript-client-app-visual-studio-2017"></a>Erstellen einer JavaScript-Client-app (Visual Studio 2017)

Wenn Sie einen c#-Client erstellen möchten, können Sie diesen Abschnitt überspringen.

> [!NOTE]
> Universelle Windows-Plattform-Projekte (UWP) werden in Visual Studio 2019 nicht unterstützt. Weitere Informationen finden Sie [unter JavaScript und typescript in Visual Studio 2019](/visualstudio/javascript/javascript-in-vs-2019?view=vs-2019#projects). Zum Befolgen dieses Abschnitts wird die Verwendung von Visual Studio 2017 empfohlen. Weitere Informationen finden Sie [unter JavaScript in Visual Studio 2017](/visualstudio/javascript/javascript-in-vs-2017).

### <a name="to-create-a-javascript-project"></a>So erstellen Sie ein JavaScript-Projekt
1. Öffnen Sie in Projektmappen-Explorer (in Visual Studio 2017; siehe **Hinweis** oben) das Kontextmenü für den Projektmappenknoten, und wählen Sie **hinzufügen, neues Projekt**aus.

2. Erweitern Sie „JavaScript“ (kann unter **Andere Sprachen** verschachtelt sein), und wählen Sie **Leere App (universelle Windows-App)** aus.

3. Akzeptieren Sie den Standardnamen &mdash; App1 &mdash; durch Auswählen der Schaltfläche **OK** .

4. Öffnen Sie das Kontextmenü für den Projektknoten „App1“, und wählen Sie **Als Startprojekt festlegen** aus.

5. Fügen Sie einen Projektverweis zu WinRT_CPP hinzu:

6. Öffnen Sie dann das Kontextmenü für den Knoten „Verweise“, und wählen Sie **Verweis hinzufügen** aus.

7. Wählen Sie im linken Bereich des Dialogfelds „Verweis-Manager“ die Option **Projekte** aus, und wählen Sie dann **Lösung** aus.

8. Wählen Sie im mittleren Bereich „WinRT_CPP“ und dann die Schaltfläche **OK** aus.

## <a name="to-add-the-html-that-invokes-the-javascript-event-handlers"></a>So fügen Sie den HTML-Code hinzu, der die JavaScript-Ereignishandler aufruft
Fügen Sie den HTML-Code in das <body> Knoten der Seite default.html:

```HTML
<div id="LogButtonDiv">
     <button id="logButton">Logarithms using AMP</button>
 </div>
 <div id="LogResultDiv">
     <p id="logResult"></p>
 </div>
 <div id="OrderedPrimeButtonDiv">
     <button id="orderedPrimeButton">Primes using parallel_for with sort</button>
 </div>
 <div id="OrderedPrimeProgress">
     <progress id="OrderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="OrderedPrimeResultDiv">
     <p id="orderedPrimes">
         Primes found (ordered):
     </p>
 </div>
 <div id="UnorderedPrimeButtonDiv">
     <button id="ButtonUnordered">Primes returned as they are produced.</button>
 </div>
 <div id="UnorderedPrimeDiv">
     <progress id="UnorderedPrimesProgressBar" value="0" max="100"></progress>
 </div>
 <div id="UnorderedPrime">
     <p id="unorderedPrimes">
         Primes found (unordered):
     </p>
 </div>
 <div id="ClearDiv">
     <button id="Button_Clear">Clear</button>
 </div>
```

## <a name="to-add-styles"></a>So fügen Sie Formate hinzu
Entfernen Sie in „default.css“ das body-Format, und fügen Sie dann diese Formate hinzu:

```css
#LogButtonDiv {
border: orange solid 1px;
-ms-grid-row: 1; /* default is 1 */
-ms-grid-column: 1; /* default is 1 */
}
#LogResultDiv {
background: black;
border: red solid 1px;
-ms-grid-row: 1;
-ms-grid-column: 2;
}
#UnorderedPrimeButtonDiv, #OrderedPrimeButtonDiv {
border: orange solid 1px;
-ms-grid-row: 2;   
-ms-grid-column:1;
}
#UnorderedPrimeProgress, #OrderedPrimeProgress {
border: red solid 1px;
-ms-grid-column-span: 2;
height: 40px;
}
#UnorderedPrimeResult, #OrderedPrimeResult {
border: red solid 1px;
font-size:smaller;
-ms-grid-row: 2;
-ms-grid-column: 3;
-ms-overflow-style:scrollbar;
}
```

## <a name="to-add-the-javascript-event-handlers-that-call-into-the-component-dll"></a>So fügen Sie die JavaScript-Ereignishandler hinzu, die die Komponenten-DLL aufrufen
Fügen Sie am Ende der Datei „default.js“ die folgenden Funktionen hinzu. Diese Funktionen werden aufgerufen, wenn die Schaltflächen auf der Hauptseite ausgewählt werden. Beachten Sie, wie JavaScript die C++-Klasse aktiviert und dann ihre Methoden aufruft und die Rückgabewerte zum Auffüllen der HTML-Beschriftungen verwendet.

```JavaScript
var nativeObject = new WinRT_CPP.Class1();

function LogButton_Click() {

    var val = nativeObject.computeResult(0);
    var result = "";

    for (i = 0; i < val.length; i++) {
        result += val[i] + "<br/>";
    }

    document.getElementById('logResult').innerHTML = result;
}

function ButtonOrdered_Click() {
    document.getElementById('orderedPrimes').innerHTML = "Primes found (ordered): ";

    nativeObject.getPrimesOrdered(2, 10000).then(
        function (v) {
            for (var i = 0; i < v.length; i++)
                document.getElementById('orderedPrimes').innerHTML += v[i] + " ";
        },
        function (error) {
            document.getElementById('orderedPrimes').innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("OrderedPrimesProgressBar");
            progressBar.value = p;
        });
}

function ButtonUnordered_Click() {
    document.getElementById('unorderedPrimes').innerHTML = "Primes found (unordered): ";
    nativeObject.onprimefoundevent = handler_unordered;

    nativeObject.getPrimesUnordered(2, 10000).then(
        function () { },
        function (error) {
            document.getElementById("unorderedPrimes").innerHTML += " " + error.description;
        },
        function (p) {
            var progressBar = document.getElementById("UnorderedPrimesProgressBar");
            progressBar.value = p;
        });
}

var handler_unordered = function (n) {
    document.getElementById('unorderedPrimes').innerHTML += n.target.toString() + " ";
};

function ButtonClear_Click() {

    document.getElementById('logResult').innerHTML = "";
    document.getElementById("unorderedPrimes").innerHTML = "";
    document.getElementById('orderedPrimes').innerHTML = "";
    document.getElementById("UnorderedPrimesProgressBar").value = 0;
    document.getElementById("OrderedPrimesProgressBar").value = 0;
}
```

Fügen Sie Code zum Hinzufügen von Ereignis-Listenern durch Ersetzen des vorhandenen Aufrufs von „WinJS.UI.processAll“ in „app.onactivated“ in „default.js“ durch den folgenden Code hinzu, der die Ereignisregistrierung in einem „then“-Block implementiert. Eine ausführliche Erläuterung hierzu finden Sie unter [Erstellen einer "Hello, World"-app (JS)](../get-started/create-a-hello-world-app-js-uwp.md).

```JavaScript
args.setPromise(WinJS.UI.processAll().then( function completed() {
    var logButton = document.getElementById("logButton");
    logButton.addEventListener("click", LogButton_Click, false);
    var orderedPrimeButton = document.getElementById("orderedPrimeButton");
    orderedPrimeButton.addEventListener("click", ButtonOrdered_Click, false);
    var buttonUnordered = document.getElementById("ButtonUnordered");
    buttonUnordered.addEventListener("click", ButtonUnordered_Click, false);
    var buttonClear = document.getElementById("Button_Clear");
    buttonClear.addEventListener("click", ButtonClear_Click, false);
}));
```

Drücken Sie F5, um die App auszuführen.

## <a name="creating-a-c-client-app"></a>Erstellen einer C#-Client-App

### <a name="to-create-a-c-project"></a>So erstellen Sie ein C#-Projekt
1. Öffnen Sie im Projektmappen-Explorer das Kontextmenü für den Lösungsknoten, und wählen Sie dann **Hinzufügen, Neues Projekt** aus.

2. Erweitern Sie Visual C# (kann unter **Andere Sprachen** verschachtelt sein), wählen Sie **Windows** und dann **universelle** im linken Bereich, und schließlich **Leere App** im mittleren Bereich aus.

3. Nennen Sie diese App „CS_Client“, und wählen Sie dann die Schaltfläche **OK** aus.

4. Öffnen Sie das Kontextmenü für den Projektknoten „CS_Client“, und wählen Sie **Als Startprojekt festlegen** aus.

5. Fügen Sie einen Projektverweis zu WinRT_CPP hinzu:

   - Öffnen Sie das Kontextmenü für den Knoten **Verweise** , und wählen Sie **Verweis hinzufügen**aus.

   - Klicken Sie im linken Bereich des Dialog Felds **Verweis-Manager** auf **Projekte** , und wählen Sie dann Projekt Mappe **aus.**

   - Wählen Sie im mittleren Bereich WinRT_CPP aus, und klicken Sie dann auf die Schaltfläche **OK** .

## <a name="to-add-the-xaml-that-defines-the-user-interface"></a>So fügen Sie den XAML-Code hinzu, der die Benutzeroberfläche definiert
Kopieren Sie den folgenden Code in das Grid-Element in „MainPage.xaml“.

```xaml
<ScrollViewer>
            <StackPanel Width="1400">

                <Button x:Name="Button1" Width="340" Height="50"  Margin="0,20,20,20" Content="Synchronous Logarithm Calculation" FontSize="16" Click="Button1_Click_1"/>
                <TextBlock x:Name="Result1" Height="100" FontSize="14"></TextBlock>
            <Button x:Name="PrimesOrderedButton" Content="Prime Numbers Ordered" FontSize="16" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesOrderedButton_Click_1"></Button>
            <ProgressBar x:Name="PrimesOrderedProgress" IsIndeterminate="false" Height="40"></ProgressBar>
                <TextBlock x:Name="PrimesOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>
            <Button x:Name="PrimesUnOrderedButton" Width="340" Height="50" Margin="0,20,20,20" Click="PrimesUnOrderedButton_Click_1" Content="Prime Numbers Unordered" FontSize="16"></Button>
            <ProgressBar x:Name="PrimesUnOrderedProgress" IsIndeterminate="false" Height="40" ></ProgressBar>
            <TextBlock x:Name="PrimesUnOrderedResult" MinHeight="100" FontSize="10" TextWrapping="Wrap"></TextBlock>

            <Button x:Name="Clear_Button" Content="Clear" HorizontalAlignment="Left" Margin="0,20,20,20" VerticalAlignment="Top" Width="341" Click="Clear_Button_Click" FontSize="16"/>
        </StackPanel>
</ScrollViewer>
```

## <a name="to-add-the-event-handlers-for-the-buttons"></a>So fügen Sie die Ereignishandler für die Schaltflächen hinzu
Öffnen Sie die Datei „MainPage.xaml.cs“ im Projektmappen-Explorer. (Die Datei befindet sich möglicherweise unter „MainPage.xaml“.) Fügen Sie eine using-Direktive für „System.Text“ und dann den Ereignishandler für die Logarithmusberechnung in der „MainPage“-Klasse hinzu.

```csharp
private void Button1_Click_1(object sender, RoutedEventArgs e)
{
    // Create the object
    var nativeObject = new WinRT_CPP.Class1();

    // Call the synchronous method. val is an IList that
    // contains the results.
    var val = nativeObject.ComputeResult(0);
    StringBuilder result = new StringBuilder();
    foreach (var v in val)
    {
        result.Append(v).Append(System.Environment.NewLine);
    }
    this.Result1.Text = result.ToString();
}
```

Fügen Sie den Ereignishandler für das sortierte Ergebnis hinzu:

```csharp
async private void PrimesOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (ordered): ");

    PrimesOrderedResult.Text = sb.ToString();

    // Call the asynchronous method
    var asyncOp = nativeObject.GetPrimesOrdered(2, 100000);

    // Before awaiting, provide a lambda or named method
    // to handle the Progress event that is fired at regular
    // intervals by the asyncOp object. This handler updates
    // the progress bar in the UI.
    asyncOp.Progress = (asyncInfo, progress) =>
        {
            PrimesOrderedProgress.Value = progress;
        };

    // Wait for the operation to complete
    var asyncResult = await asyncOp;

    // Convert the results to strings
    foreach (var result in asyncResult)
    {
        sb.Append(result).Append(" ");
    }

    // Display the results
    PrimesOrderedResult.Text = sb.ToString();
}
```

Fügen Sie den Ereignishandler für das unsortierte Ergebnis und für die Schaltfläche hinzu, die die Ergebnisse löscht, sodass Sie den Code erneut ausführen können.

```csharp
private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
{
    var nativeObject = new WinRT_CPP.Class1();

    StringBuilder sb = new StringBuilder();
    sb.Append("Primes found (unordered): ");
    PrimesUnOrderedResult.Text = sb.ToString();

    // primeFoundEvent is a user-defined event in nativeObject
    // It passes the results back to this thread as they are produced
    // and the event handler that we define here immediately displays them.
    nativeObject.primeFoundEvent += (n) =>
    {
        sb.Append(n.ToString()).Append(" ");
        PrimesUnOrderedResult.Text = sb.ToString();
    };

    // Call the async method.
    var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

    // Provide a handler for the Progress event that the asyncResult
    // object fires at regular intervals. This handler updates the progress bar.
    asyncResult.Progress += (asyncInfo, progress) =>
        {
            PrimesUnOrderedProgress.Value = progress;
        };
}

private void Clear_Button_Click(object sender, RoutedEventArgs e)
{
    PrimesOrderedProgress.Value = 0;
    PrimesUnOrderedProgress.Value = 0;
    PrimesUnOrderedResult.Text = "";
    PrimesOrderedResult.Text = "";
    Result1.Text = "";
}
```

## <a name="running-the-app"></a>Ausführen der App
Wählen Sie das C#-Projekt oder das JavaScript-Projekt als Startprojekt aus, indem Sie das Kontextmenü für den Projektknoten im Projektmappen-Explorer öffnen und **Als Startprojekt festlegen** auswählen. Drücken Sie dann F5 zum Ausführen mit Debugging oder STRG+F5 zum Ausführen ohne Debugging.

## <a name="inspecting-your-component-in-object-browser-optional"></a>Untersuchen der Komponente im Objektkatalog (optional)
Im Objektkatalog können Sie alle Windows-Runtime-Typen untersuchen, die in WINMD-Dateien definiert werden. Dazu zählen die Typen im Plattformnamespace und Standardnamespace. Da die Typen im „Platform::Collections“-Namespace aber in der Headerdatei „collections.h“ und nicht in einer WINMD-Datei definiert werden, werden sie nicht im Objektkatalog angezeigt.

### <a name="to-inspect-a-component"></a>**So untersuchen Sie eine Komponente**
1. Wählen Sie auf der Menüleiste **Ansicht, Objektkatalog** (STRG+ALT+J) aus.

2. Erweitern Sie im linken Bereich des Objektkatalog den Knoten WinRT \_ cpp, um die Typen und Methoden anzuzeigen, die für die Komponente definiert sind.

## <a name="debugging-tips"></a>Tipps zum Debuggen
Laden Sie für einen besseren Debugvorgang die Debuggingsymbole von den öffentlichen Microsoft-Symbolservern herunter:

### <a name="to-download-debugging-symbols"></a>**So laden Sie Debuggingsymbole herunter**
1. Wählen Sie in der Menüleiste Extras **, Optionen**aus.

2. Erweitern Sie im Dialogfeld **Optionen** die Option **Debuggen** , und wählen Sie **Symbole** aus.

3. Wählen Sie **Microsoft-Symbolserver** und dann die Schaltfläche **OK** aus.

Beim ersten Mal kann das Herunterladen der Symbole einige Zeit dauern. Geben Sie für eine bessere Leistung beim nächsten Drücken von F5 ein lokales Verzeichnis an, in dem Sie die Symbole zwischenspeichern.

Beim Debuggen einer JavaScript-Lösung mit einer Komponenten-DLL können Sie den Debugger so einstellen, dass er das schrittweise Ausführen von Skripts oder das schrittweise Ausführen von systemeigenem Code in der Komponente, aber nicht beides gleichzeitig ermöglicht. Um die Einstellung zu ändern, öffnen Sie im Projektmappen-Explorer das Kontextmenü für den JavaScript-Projektknoten, und wählen Sie **Eigenschaften, Debuggen, Debuggertyp** aus.

Achten Sie darauf, entsprechende Funktionen im Paket-Designer auszuwählen. Sie können den Paket-Designer durch Öffnen der Datei „Package.appxmanifest“ öffnen. Wenn Sie z. B. versuchen, programmgesteuert auf Dateien im Ordner „Bilder“ zuzugreifen, stellen Sie sicher, dass das Kontrollkästchen **Bildbibliothek** im Bereich **Funktionen** des Paket-Designers aktiviert ist.

Wenn der JavaScript-Code die öffentlichen Eigenschaften oder Methoden in der Komponente nicht erkennt, stellen Sie sicher, dass Sie in JavaScript Camel Casing verwenden. Auf die `ComputeResult`-C++-Methode muss beispielsweise in JavaScript als `computeResult` verwiesen werden.

Wenn Sie ein C++-Komponentenprojekt für Windows-Runtime aus einer Projektmappe entfernen, müssen Sie den Projektverweis auch aus dem JavaScript-Projekt manuell entfernen. Andernfalls werden nachfolgende Debug- oder Buildvorgänge verhindert. Bei Bedarf können Sie dann einen Assemblyverweis zur DLL-Datei hinzufügen.

## <a name="related-topics"></a>Zugehörige Themen
* [Komponenten für Windows-Runtime mit C++/CX](creating-windows-runtime-components-in-cpp.md)