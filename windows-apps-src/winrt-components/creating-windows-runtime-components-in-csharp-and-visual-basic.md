---
title: Komponenten für Windows-Runtime in C# und Visual Basic
description: Ab .NET 4,5 können Sie verwalteten Code verwenden, um eigene Windows-Runtime Typen zu erstellen, die in einer Windows-Runtime Komponente verpackt sind.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3ac5e08b328e8e094906d2e1793ea87fa2e66ae4
ms.sourcegitcommit: 91ac6db556fa2a49374fffb143f55fed864200ac
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/27/2020
ms.locfileid: "82173486"
---
# <a name="windows-runtime-components-with-c-and-visual-basic"></a>Komponenten für Windows-Runtime in C# und Visual Basic

Sie können verwalteten Code verwenden, um eigene Windows-Runtime Typen zu erstellen und Sie in einer Windows-Runtime Komponente zu verpacken. Sie können Ihre Komponente in universelle Windows-Plattform-Apps (UWP) verwenden, die in C++, JavaScript, Visual Basic oder c# geschrieben sind. In diesem Thema werden die Regeln zum Erstellen einer Komponente und einige Aspekte der .NET-Unterstützung für die Windows-Runtime erläutert. Im Allgemeinen ist diese Unterstützung allen .NET-Programmierern klar. Wenn Sie aber eine Komponente erstellen, die mit JavaScript oder C++ verwendet werden soll, müssen Sie auf die Unterschiede bei der Unterstützung der Windows-Runtime durch diese Sprachen achten.

Wenn Sie eine Komponente nur für UWP-Apps erstellen, die in Visual Basic oder c# geschrieben sind, und die Komponente keine UWP-Steuerelemente enthält, empfiehlt es sich, die **Klassen Bibliotheks** Vorlage anstelle der **Windows-Runtime Component** -Projektvorlage in Microsoft Visual Studio zu verwenden. Eine einfache Klassenbibliothek weist weniger Einschränkungen auf.

## <a name="declaring-types-in-windows-runtime-components"></a>Deklarieren von Typen in Windows-Runtime Komponenten

Intern können die Windows-Runtime Typen in der Komponente beliebige .NET-Funktionen verwenden, die in einer UWP-App zulässig sind. Weitere Informationen finden Sie unter [.net für UWP-apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0).

Extern können die Member ihrer Typen nur Windows-Runtime Typen für Ihre Parameter und Rückgabewerte verfügbar machen. In der folgenden Liste werden die Einschränkungen für .NET-Typen beschrieben, die von einer Windows-Runtime Komponente verfügbar gemacht werden.

- Die Felder, Parameter und Rückgabewerte aller öffentlichen Typen und Member in der Komponente müssen Windows-Runtime-Typen sein. Diese Einschränkung schließt die Windows-Runtime Typen ein, die Sie erstellen, sowie die Typen, die von der Windows-Runtime selbst bereitgestellt werden. Es enthält auch eine Reihe von .NET-Typen. Die Aufnahme dieser Typen ist Teil der Unterstützung, die .net bereitstellt, um die natürliche Verwendung der Windows-Runtime in verwaltetem Code zu ermöglichen &mdash; . Ihr Code scheint vertraute .NET-Typen anstelle der zugrunde liegenden Windows-Runtime Typen zu verwenden. Sie können z. b. die primitiven .NET-Typen, wie z. b. **Int32** und **Double**, bestimmte grundlegende Typen, z. b. **DateTimeOffset** und **URI**, und einige häufig verwendete generische Schnittstellentypen wie **IEnumerable &lt; T &gt; ** (IEnumerable (of T) in Visual Basic) und **IDictionary &lt; TKey, TValue &gt; **verwenden. Beachten Sie, dass die Typargumente dieser generischen Typen Windows-Runtime Typen sein müssen. Dies wird in den Abschnitten Weiterleiten von [Windows-Runtime Typen an verwalteten Code](#passing-windows-runtime-types-to-managed-code) und [übergeben von verwalteten Typen an den Windows-Runtime](#passing-managed-types-to-the-windows-runtime)weiter unten in diesem Thema erläutert.

- Öffentliche Klassen und Schnittstellen können Methoden, Eigenschaften und Ereignisse enthalten. Sie können Delegaten für Ihre Ereignisse deklarieren oder den **EventHandler &lt; T &gt; ** -Delegaten verwenden. Eine öffentliche Klasse oder Schnittstelle kann nicht folgende Aktionen ausführen:
    - generisch sein.
    - Implementieren Sie eine Schnittstelle, bei der es sich nicht um eine Windows-Runtime-Schnittstelle handelt (Sie können jedoch eigene Windows-Runtime Schnittstellen erstellen und implementieren).
    - Leiten Sie von Typen ab, die sich nicht im Windows-Runtime befinden, wie z. b **. System. Exception** und **System. EventArgs**.

- Alle öffentliche Typen müssen über einen Stammnamespace verfügen, der mit dem Assemblynamen übereinstimmt, und der Assemblyname darf nicht mit „Windows” beginnen.

    > **Tipp**: Standardmäßig entsprechen die Namespacenamen in Visual Studio-Projekten den Assemblynamen. In Visual Basic wird die Namespace-Anweisung für diesen Standardnamespace nicht im Code angezeigt.

- Öffentliche Strukturen können nur öffentliche Felder als Member enthalten, und diese Felder müssen Werttypen oder Zeichenfolgen sein.
- Öffentliche Klassen müssen **versiegelt** (**NotInheritable** in Visual Basic) sein. Wenn das Programmiermodell Polymorphie erfordert, können Sie eine öffentliche Schnittstelle erstellen und diese Schnittstelle in den Klassen implementieren, die polymorph sein müssen.

## <a name="debugging-your-component"></a>Debuggen der Komponente

Wenn sowohl Ihre UWP-App als auch die Komponente mit verwaltetem Code erstellt werden, können Sie beide gleichzeitig Debuggen.

Wenn Sie die Komponente als Teil einer UWP-App mit C++ testen, können Sie verwalteten und nativen Code gleichzeitig Debuggen. Die Vorgabe ist nur systemeigener Code.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>So debuggen Sie systemeigenen C++-Code und verwalteten Code
1.  Öffnen Sie das Kontextmenü für das Visual C++-Projekt, und wählen Sie **Eigenschaften** aus.
2.  Wählen Sie auf den Eigenschaftenseiten unter **Konfigurationseigenschaften** die Option **Debuggen** aus.
3.  Wählen Sie **Debuggertyp** aus, und ändern Sie dann in der Dropdownliste **Nur systemeigen** in **Gemischt (verwaltet und systemeigen)**. Klicken Sie auf **OK**.
4.  Legen Sie Haltepunkte im systemeigenen und verwalteten Code fest.

Wenn Sie die Komponente als Teil einer UWP-App mit JavaScript testen, befindet sich die Projekt Mappe standardmäßig im JavaScript-Debugmodus. In Visual Studio ist das gleichzeitige Debuggen von JavaScript und verwaltetem Code nicht möglich.

## <a name="to-debug-managed-code-instead-of-javascript"></a>So debuggen Sie verwalteten Code anstelle von JavaScript
1.  Öffnen Sie das Kontextmenü für das JavaScript-Projekt, und wählen Sie **Eigenschaften** aus.
2.  Wählen Sie auf den Eigenschaftenseiten unter **Konfigurationseigenschaften** die Option **Debuggen** aus.
3.  Wählen Sie **Debuggertyp** aus, und ändern Sie in der Dropdownliste **Nur Skript** in **Nur verwaltet**. Klicken Sie auf **OK**.
4.  Legen Sie Haltepunkte im verwaltetem Code fest, und debuggen Sie wie gewohnt.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Übergeben von Windows-Runtime Typen an verwalteten Code
Wie bereits im Abschnitt " [Deklarieren von Typen in Windows-Runtime Komponenten](#declaring-types-in-windows-runtime-components)" erwähnt, können bestimmte .NET-Typen in den Signaturen von Membern öffentlicher Klassen erscheinen. Dies ist Teil der Unterstützung, die .net bietet, um die natürliche Verwendung der Windows-Runtime in verwaltetem Code zu ermöglichen. Darin sind primitive Typen und einige Klassen und Schnittstellen enthalten. Wenn die Komponente von JavaScript oder von C++-Code verwendet wird, ist es wichtig zu wissen, wie die .NET-Typen dem Aufrufer angezeigt werden. Beispiele mit JavaScript finden Sie unter Exemplarische Vorgehensweise [: Erstellen einer c#-oder Visual Basic Windows-Runtime Komponente und Aufrufen dieser Komponente aus JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) . In diesem Abschnitt werden häufig verwendete Typen beschrieben.

In .net verfügen primitive Typen wie z. b. die **Int32** -Struktur über viele nützliche Eigenschaften und Methoden, z. b. die **tryparamese** -Methode. Im Gegensatz dazu haben primitive Typen und Strukturen in der Windows-Runtime nur Felder. Wenn Sie diese Typen an verwalteten Code übergeben, scheinen Sie .NET-Typen zu sein, und Sie können die Eigenschaften und Methoden von .NET-Typen wie gewohnt verwenden. Die folgende Liste fasst die Ersetzungen zusammen, die automatisch in der IDE vorgenommen werden:

-   Verwenden Sie für die Windows-Runtime **primitive Int32**, **Int64**, **Single**, **Double**, **Boolean**, **String** (eine unveränderliche Auflistung von Unicode-Zeichen), **enum**, **UInt32**, **UInt64**und **GUID**den Typ desselben Namens im System-Namespace.
-   Verwenden Sie für **UInt8**den Wert **System. Byte**.
-   Verwenden **Char16**Sie für Char16 **System. Char**.
-   Verwenden Sie für die **iinspectable** -Schnittstelle die **System. Object**-Schnittstelle.

Wenn c# oder Visual Basic ein sprach Schlüsselwort für einen dieser Typen bereitstellt, können Sie stattdessen das Language-Schlüsselwort verwenden.

Zusätzlich zu den primitiven Typen werden einige grundlegende, häufig verwendete Windows-Runtime Typen in verwaltetem Code als Ihre .NET-Entsprechungen angezeigt. Angenommen, Ihr JavaScript-Code verwendet die **Windows. Foundation. Uri** -Klasse, und Sie möchten Sie an eine c#-oder Visual Basic-Methode übergeben. Der entsprechende Typ in verwaltetem Code ist die .NET-Klasse **System. Uri** , und das ist der Typ, der für den Methoden Parameter verwendet werden soll. Sie können erkennen, wenn ein Windows-Runtime Typ als .NET-Typ angezeigt wird, da IntelliSense in Visual Studio beim Schreiben von verwaltetem Code den Windows-Runtime-Typ verbirgt und den entsprechenden .NET-Typ anzeigt. (In der Regel haben beiden Typen den gleichen Namen. Beachten Sie jedoch, dass die **Windows. Foundation. DateTime** -Struktur in verwaltetem Code als **System. DateTimeOffset** und nicht als **System. DateTime**angezeigt wird.

Bei einigen häufig verwendeten Auflistungs Typen erfolgt die Zuordnung zwischen den Schnittstellen, die von einem Windows-Runtime-Typ implementiert werden, und den Schnittstellen, die vom entsprechenden .NET-Typ implementiert werden. Wie bei den oben erwähnten Typen deklarieren Sie Parametertypen mithilfe des .net-Typs. Dies verbirgt einige Unterschiede zwischen den Typen und macht das Schreiben von .NET-Code natürlicher.

In der folgenden Tabelle sind die häufigsten dieser generischen Schnittstellentypen zusammen mit anderen allgemeinen Klassen- und Schnittstellenzuordnungen aufgeführt. Eine umfassende Liste der Windows-Runtime Typen, die von .net Maps zugeordnet werden, finden Sie unter .net-Zuordnungen [von Windows-Runtime Typen](net-framework-mappings-of-windows-runtime-types.md).

| Windows-Runtime                                  | .NET                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable &lt; T&gt;                              |
| IVector&lt;T&gt;                                 | IList &lt; T&gt;                                    |
| IVectorView&lt;T&gt;                             | Ilesonlylist &lt; T&gt;                            |
| IMap &lt; K, V&gt;                                 | IDictionary &lt; TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | Ileseronlydictionary &lt; TKey, TValue&gt;           |
| Ikeyvaluepair &lt; K, V&gt;                        | KeyValuePair &lt; TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVecinr                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

Wenn ein Typ mehrere Schnittstellen implementiert, können Sie jede Schnittstelle verwenden, die als Parametertyp oder Rückgabetyp eines Members implementiert wird. Beispielsweise können Sie das **Wörterbuch &lt; int &gt; , String** (**Dictionary (of Integer, String)** in Visual Basic) als **IDictionary &lt; int, String &gt; **, **ilesonlydictionary &lt; int, String &gt; **oder **IEnumerable &lt; System. Collections. Generic. KeyValuePair &lt; TKey, TValue &gt; &gt; **übergeben oder zurückgeben.

> [!IMPORTANT]
> JavaScript verwendet die Schnittstelle, die zuerst in der Liste der Schnittstellen angezeigt wird, die ein verwalteter Typ implementiert. Wenn Sie z. b. " **Dictionary &lt; int" &gt; , "String** " in JavaScript-Code zurückgeben, wird es als " **IDictionary int", "String" ( &lt; Zeichenfolge &gt; ** ) angezeigt Das bedeutet, dass die erste Schnittstelle Member enthalten muss, die in den nächsten Schnittstellen erscheinen, damit diese Member für JavaScript sichtbar sind.

In der Windows-Runtime werden **IMap &lt; k, v &gt; ** und **imapview &lt; K V &gt; ** mithilfe von "ikeyvaluepair" durchlaufen. Wenn Sie Sie an verwalteten Code übergeben, werden Sie als **IDictionary &lt; TKey, TValue &gt; ** und **ileseronlydictionary &lt; TKey, TValue &gt; **angezeigt. Daher verwenden Sie **System. Collections. Generic. KeyValuePair &lt; TKey, TValue &gt; ** , um Sie aufzulisten.

Die Darstellungsweise von Schnittstellen in verwaltetem Code wirkt sich auf die Darstellungsweise der Typen aus, die diese Schnittstellen implementieren. Beispielsweise implementiert die **PropertySet** -Klasse **IMap &lt; K, V &gt; **, das in verwaltetem Code als **IDictionary &lt; TKey, TValue &gt; **angezeigt wird. **PropertySet** wird so angezeigt, als ob es **IDictionary &lt; TKey, &gt; TValue** anstelle von **IMap &lt; K &gt; , V**implementiert hat, sodass es in verwaltetem Code über eine **Add** -Methode verfügt, die sich wie die **Add** -Methode in .net-Wörterbüchern verhält. Es scheint keine **Insert** -Methode zu haben. Dieses Beispiel finden Sie im Thema Exemplarische Vorgehensweise [zum Erstellen einer c#-oder Visual Basic Windows-Runtime Komponente und zum Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Übergeben von verwalteten Typen an die Windows-Runtime

Wie bereits im vorherigen Abschnitt erläutert, können einige Windows-Runtime Typen als .NET-Typen in den Signaturen der Member der Komponente oder in den Signaturen Windows-Runtime Members angezeigt werden, wenn Sie Sie in der IDE verwenden. Wenn Sie .NET-Typen an diese Member übergeben oder als Rückgabewerte der Member der Komponente verwenden, werden Sie im Code auf der anderen Seite als der entsprechende Windows-Runtime Typs angezeigt. Beispiele für die Auswirkungen, die dies haben kann, wenn Ihre Komponente aus JavaScript aufgerufen wird, finden Sie im Abschnitt "zurückgeben von verwalteten Typen aus der Komponente" unter Exemplarische Vorgehensweise zum [Erstellen einer c#-oder Visual Basic Windows-Runtime Komponente und zum Aufrufen dieser](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)Komponente über JavaScript.

## <a name="overloaded-methods"></a>Überladene Methoden

In der Windows-Runtime können Methoden überladen werden. Wenn Sie jedoch mehrere über Ladungen mit der gleichen Anzahl von Parametern deklarieren, müssen Sie das [**Windows. Foundation. Metadata. defaultoverloadattribute**](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) -Attribut auf nur eine dieser über Ladungen anwenden. Nur diese Überladung kann aus JavaScript aufgerufen werden. Im folgenden Code ist beispielsweise die Überladung, die **int** übernimmt (**Integer** in Visual Basic), die Standardüberladung.

```csharp
public string OverloadExample(string s)
{
    return s;
}

[Windows.Foundation.Metadata.DefaultOverload()]
public int OverloadExample(int x)
{
    return x;
}
```

```vb
Public Function OverloadExample(ByVal s As String) As String
    Return s
End Function

<Windows.Foundation.Metadata.DefaultOverload> _
Public Function OverloadExample(ByVal x As Integer) As Integer
    Return x
End Function
```

> Wichtig JavaScript ermöglicht das Übergeben eines beliebigen Werts an **OverloadExample**und wandelt den Wert in den Typ um, der für den-Parameter erforderlich ist. Sie können **OverloadExample** mit "42", "42" oder 42,3 aufrufen, aber alle diese Werte werden an die Standard Überladung geleitet. Die Standard Überladung im vorherigen Beispiel gibt 0, 42 bzw. 42 zurück.

Das Attribut **defauldeverloadattribue**kann nicht auf Konstruktoren angewendet werden. Alle Konstruktoren in einer Klasse müssen eine unterschiedliche Anzahl von Parametern aufweisen.

## <a name="implementing-istringable"></a>Implementieren von IStringable

Beginnend mit Windows 8.1 enthält die Windows-Runtime eine **istringable** -Schnittstelle, deren einzige Methode, **istringable. destring**, eine grundlegende Formatierungs Unterstützung bietet, die vergleichbar mit der von **Object. destring**bereitgestellten ist. Wenn Sie **istringable** in einem öffentlich verwalteten Typ implementieren möchten, der in eine Windows-Runtime Komponente exportiert wird, gelten die folgenden Einschränkungen:

-   Sie können die **istringable** -Schnittstelle nur in einer "Klasse implementiert"-Beziehung definieren, wie z. b. dem folgenden Code in c#:

    ```cs
    public class NewClass : IStringable
    ```

    Oder in Visual Basic-Code:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   Sie können **istringable** nicht in einer Schnittstelle implementieren.
-   Sie können einen Parameter nicht als Typ **istringable**deklarieren.
-   **Istringable** kann nicht der Rückgabetyp einer Methode, einer Eigenschaft oder eines Felds sein.
-   Sie können die **istringable** -Implementierung nicht aus Basisklassen ausblenden, indem Sie eine Methoden Definition wie die folgende verwenden:

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    Stattdessen muss die Implementierung **istringable. destring** die Basisklassen Implementierung immer überschreiben. Sie können eine Implementierung von " **destring** " nur ausblenden, indem Sie Sie auf einer stark typisierten Klasseninstanz aufrufen.

> [!NOTE]
> Unter einer Vielzahl von Bedingungen kann es bei Aufrufen von System eigenem Code zu einem verwalteten Typ, der **istringable** implementiert oder seine **destring** -Implementierung verbirgt, zu unerwartetem Verhalten führen.

## <a name="asynchronous-operations"></a>Asynchrone Vorgänge

Um eine asynchrone Methode in der Komponente zu implementieren, fügen Sie "Async" am Ende des Methoden namens hinzu und geben eine der Windows-Runtime-Schnittstellen zurück, die asynchrone Aktionen oder Vorgänge darstellen: **iasyncaction**, **iasyncactionwithprogress &lt; tprogress &gt; **, **iasyncoperation &lt; TResult &gt; **oder **iasyncoperationwithprogress &lt; TResult &gt; , tprogress**.

Sie können .net-Aufgaben (die [**Aufgaben**](/dotnet/api/system.threading.tasks.task) Klasse und die generische [**Aufgabe &lt; TResult &gt; **](/dotnet/api/system.threading.tasks.task-1) -Klasse) verwenden, um die asynchrone Methode zu implementieren. Sie müssen eine Aufgabe zurückgeben, die einen laufenden Vorgang darstellt, z. b. einen Task, der von einer asynchronen Methode zurückgegeben wird, die in c# oder Visual Basic geschrieben wurde, oder eine Aufgabe, die von der [**Task. Run**](/dotnet/api/system.threading.tasks.task.run) -Methode zurückgegeben wird. Wenn Sie für die Aufgabenerstellung einen Konstruktor verwenden, müssen Sie seine [Task.Start](/dotnet/api/system.threading.tasks.task.start)-Methode vor der Rückgabe aufrufen.

Eine Methode, die `await` ( `Await` in Visual Basic) verwendet, erfordert das `async` Schlüsselwort ( `Async` in Visual Basic). Wenn Sie eine solche Methode aus einer Windows-Runtime Komponente verfügbar machen, wenden Sie das- `async` Schlüsselwort auf den Delegaten an, den Sie an die **Run** -Methode übergeben.

Für asynchrone Aktionen und Vorgänge, die weder die Abbruch- noch die Fortschrittsberichterstattung unterstützen, können Sie mit der Erweiterungsmethode [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system) oder [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system) die Aufgabe in die entsprechende Schnittstelle umschließen. Beispielsweise implementiert der folgende Code eine asynchrone Methode, indem die **Task. Run &lt; TResult &gt; ** -Methode verwendet wird, um eine Aufgabe zu starten. Die " **asasyncoperation &lt; TResult &gt; ** "-Erweiterungsmethode gibt die Aufgabe als Windows-Runtime asynchronen Vorgang zurück.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return Task.Run<IList<string>>(async () =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    }).AsAsyncOperation();
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
     As IAsyncOperation(Of IList(Of String))

    Return Task.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function).AsAsyncOperation()
End Function
```

Der folgende JavaScript-Code zeigt, wie die-Methode mit einem [**winjs. Promise**](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10)) -Objekt aufgerufen werden kann. Die an die then-Methode übergebene Funktion wird ausgeführt, wenn der asynchrone Aufruf abgeschlossen ist. Der stringlist-Parameter enthält die Liste der Zeichen folgen, die von der **downloadasstringasync** -Methode zurückgegeben werden, und die Funktion übernimmt die erforderliche Verarbeitung.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Verwenden Sie für asynchrone Aktionen und Vorgänge, die die Abbruch-oder Fortschritts Berichterstattung unterstützen, die [**asyncinfo**](/dotnet/api/system.runtime.interopservices.windowsruntime) -Klasse, um eine gestartete Aufgabe zu generieren und die Abbruch-und Fortschritts Berichterstattungs Funktionen der Aufgabe mit den Abbruch-und Fortschritts Berichterstattungs Funktionen der entsprechenden Windows-Runtime-Schnittstelle zu verbinden. Ein Beispiel, das sowohl die Abbruch-als auch die Fortschritts Berichterstattung unterstützt, finden Sie unter Exemplarische Vorgehensweise: [Erstellen einer c#-oder Visual Basic Windows-Runtime Komponente und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Beachten Sie, dass Sie die Methoden der **asyncinfo** -Klasse auch dann verwenden können, wenn Ihre asynchrone Methode die Abbruch-oder Fortschritts Berichterstattung nicht unterstützt. Wenn Sie eine Visual Basic Lambda-Funktion oder eine anonyme c#-Methode verwenden, geben Sie keine Parameter für das Token und die [**iprogress &lt; t &gt; **](https://docs.microsoft.com/dotnet/api/system.iprogress-1) -Schnittstelle an. Wenn Sie eine C#-Lambda-Funktion verwenden, geben Sie einen Tokenparameter an, aber ignorieren Sie ihn. Das vorherige Beispiel, das die Methode asasyncoperation &lt; TResult verwendet &gt; , sieht wie folgt aus, wenn Sie stattdessen die Methode [**asyncinfo. Run &lt; TResult &gt; (Func &lt; CancellationToken, Task &lt; &gt; &gt; TResult**](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime)) verwenden.

```csharp
public static IAsyncOperation<IList<string>> DownloadAsStringsAsync(string id)
{
    return AsyncInfo.Run<IList<string>>(async (token) =>
    {
        var data = await DownloadDataAsync(id);
        return ExtractStrings(data);
    });
}
```

```vb
Public Shared Function DownloadAsStringsAsync(ByVal id As String) _
    As IAsyncOperation(Of IList(Of String))

    Return AsyncInfo.Run(Of IList(Of String))(
        Async Function()
            Dim data = Await DownloadDataAsync(id)
            Return ExtractStrings(data)
        End Function)
End Function
```

Wenn Sie eine asynchrone Methode erstellen, die optional die Abbruch-oder Fortschritts Berichterstattung unterstützt, sollten Sie über Ladungen hinzufügen, die keine Parameter für ein Abbruch Token oder die **iprogress &lt; t &gt; ** -Schnittstelle aufweisen.

## <a name="throwing-exceptions"></a>Auslösen von Ausnahmen

Sie können jeden Ausnahmetyp auslösen, der in .NET für Windows-Apps enthalten ist. Sie können keine eigenen öffentlichen Ausnahmetypen in einer Komponente für Windows-Runtime deklarieren, aber Sie können nicht öffentliche Typen deklarieren und auslösen.

Wenn die Komponente die Ausnahme nicht behandelt, wird eine entsprechende Ausnahme im Code ausgelöst, der die Komponente aufgerufen hat. Die Unterstützung der Windows-Runtime durch die aufrufende Sprache bestimmt, wie die Ausnahme dem Aufrufer dargestellt wird.

-   In JavaScript erscheint die Ausnahme als Objekt, in dem die Ausnahmemeldung durch eine Stapelüberwachung ersetzt ist. Wenn Sie Ihre App in Visual Studio debuggen, wird der Originaltext der Meldung im Ausnahmedialogfeld des Debuggers unter „WinRT Information" angezeigt. Sie können mit JavaScript-Code nicht auf den Originaltext der Meldung zugreifen.

    > **Tipp**:Momentan enthält die Stapelüberwachung den verwalteten Ausnahmetyp, aber es ist nicht empfehlenswert, diese zu untersuchen, um den Ausnahmetyp zu identifizieren. Verwenden Sie stattdessen einen HRESULT-Wert, wie weiter unten in diesem Abschnitt beschrieben.

-   In C++ erscheint die Ausnahme als Plattformausnahme. Wenn die HRESULT-Eigenschaft der verwalteten Ausnahme dem HRESULT einer bestimmten Platt Form Ausnahme zugeordnet werden kann, wird die spezifische Ausnahme verwendet. Andernfalls wird eine [**Platform:: COMException**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class) -Ausnahme ausgelöst. Der Meldungstext der verwalteten Ausnahme ist für C++-Code nicht verfügbar. Wenn eine bestimmte Plattformausnahme ausgelöst wurde, erscheint der Meldungstext für diesen Ausnahmetyp. Andernfalls wird kein Meldungstext ausgegeben. Siehe [Ausnahmen (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx).
-   In C# oder Visual Basic ist die Ausnahme eine normale verwaltete Ausnahme.

Wenn Sie in Ihrer Komponente eine Ausnahme auslösen, sollten Sie einen nicht öffentlichen Ausnahmetyp verwenden, dessen HResult-Eigenschaftswert speziell für Ihre Komponente gilt, damit die Ausnahme leichter von einem JavaScript- oder C++-Aufrufer verwaltet werden kann. Das HRESULT ist für einen JavaScript-Aufrufer über die Number-Eigenschaft des Ausnahme Objekts verfügbar, und für einen C++-Aufrufer über die [**COMException:: HRESULT**](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult) -Eigenschaft.

> [!NOTE]
> Verwenden Sie einen negativen Wert für HRESULT. Ein positiver Wert wird als Erfolg interpretiert und im JavaScript- oder C++-Aufrufer wird keine Ausnahme ausgelöst.

## <a name="declaring-and-raising-events"></a>Deklarieren und Auslösen von Ereignissen

Wenn Sie einen Typ deklarieren, um die Daten für das Ereignis aufzunehmen, leiten Sie diesen von „Object“ und nicht von „EventArgs“ ab, da „EventArgs“ kein Windows-Runtime-Typ ist. Verwenden Sie [**EventHandler &lt; TEventArgs &gt; **](https://docs.microsoft.com/dotnet/api/system.eventhandler-1) als Typ des Ereignisses, und verwenden Sie den Ereignis Argumenttyp als generisches Typargument. Rufen Sie das-Ereignis genau wie in einer .NET-Anwendung auf.

Wenn Ihre Komponente für Windows-Runtime von JavaScript oder C++ verwendet wird, folgt das Ereignis dem Windows-Runtime-Ereignismuster, das diese Sprachen erwarten. Wenn Sie die Komponente aus c# oder Visual Basic verwenden, wird das Ereignis als normales .NET-Ereignis angezeigt. Ein Beispiel finden Sie unter Exemplarische Vorgehensweise [: Erstellen einer c#-oder Visual Basic Windows-Runtime Komponente und Aufrufen dieser Komponente über JavaScript](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript).

Wenn Sie benutzerdefinierte Ereignisaccessoren implementieren (in Visual Basic ein Ereignis mit dem Schlüsselwort **Custom** deklarieren), müssen Sie in der Implementierung das Windows-Runtime-Ereignismuster verwenden. Siehe [benutzerdefinierte Ereignisse und Ereignisaccessoren in Windows-Runtime-Komponenten](custom-events-and-event-accessors-in-windows-runtime-components.md). Beachten Sie, dass bei der Behandlung des-Ereignisses aus c# oder Visual Basic-Code ein normales .NET-Ereignis angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine Komponente für Windows-Runtime für eigene Zwecke erstellt haben, stellen Sie möglicherweise fest, dass die Funktionen, die diese kapselt, für andere Entwickler nützlich sind. Sie haben zwei Optionen, um eine Komponente für die Verteilung an andere Entwickler zu packen. Siehe [Verteilen einer verwalteten Komponente für Windows-Runtime](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140)).

Weitere Informationen zu Visual Basic-und c#-sprach Features sowie zu .NET-Unterstützung für die Windows-Runtime finden Sie unter [Visual Basic-und c#-Sprachreferenz](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).

## <a name="related-topics"></a>Zugehörige Themen
* [.NET für UWP-Apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Exemplarische Vorgehensweise zum Erstellen einer Komponente für Windows-Runtime in C# oder Visual Basic und Aufrufen der Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
