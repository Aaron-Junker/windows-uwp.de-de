---
title: Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic
description: Ab .NET Framework 4.5 können Sie mit verwaltetem Code eigene Windows-Runtime-Typen erstellen, die in einer Komponente für Windows-Runtime gepackt sind.
ms.assetid: A5672966-74DF-40AB-B01E-01E3FCD0AD7A
ms.date: 12/04/2018
ms.topic: article
dev_langs:
- csharp
- vb
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a20f8e85927015d6aa69dbbf4381cb2d7daafc36
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372031"
---
# <a name="creating-windows-runtime-components-in-c-and-visual-basic"></a>Erstellen von Komponenten für Windows-Runtime in C# und Visual Basic
Ab .NET Framework 4.5, können Sie verwalteten Code verwenden, um Ihre eigenen Windows-Runtime-Typen zu erstellen und Packen Sie sie in einer Windows-Runtime-Komponente. Sie können Ihre Komponente verwenden, in apps für universelle Windows-Plattform (UWP), die in C++, JavaScript, Visual Basic geschrieben werden oder C#. In diesem Thema werden die Regeln zum Erstellen einer Komponente und einige Aspekte der .NET Framework-Unterstützung für die Windows-Runtime erläutert. Im Allgemeinen ist diese Unterstützung allen .NET Framework-Programmierern klar. Wenn Sie aber eine Komponente erstellen, die mit JavaScript oder C++ verwendet werden soll, müssen Sie auf die Unterschiede bei der Unterstützung der Windows-Runtime durch diese Sprachen achten.

Wenn Sie eine Komponente für die Verwendung nur in UWP-apps erstellen, die in Visual Basic geschrieben werden oder C#, und die Komponente enthält keine UWP-Steuerelemente, und klicken Sie dann mithilfe von Onsider der **Klassenbibliothek** Vorlage anstelle von der **Windows Runtime-Komponente** in Microsoft Visual Studio-Projektvorlage. Eine einfache Klassenbibliothek weist weniger Einschränkungen auf.

## <a name="declaring-types-in-windows-runtime-components"></a>Deklarieren von Typen in Komponenten für Windows-Runtime

Intern können die Windows-Runtime-Typen in Ihrer Komponente alle .NET Framework-Funktionalitäten verwenden, die in einer UWP-app zulässig ist. Weitere Informationen finden Sie unter [.NET für UWP-apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0).

Extern können die Member Ihrer Typen verfügbar machen nur über die Windows-Runtime-Typen für ihre Parameter und Rückgabewerte. Die folgende Liste beschreibt die Einschränkungen für .NET Framework-Typen, die von einer Windows-Runtime-Komponente verfügbar gemacht werden.

- Die Felder, Parameter und Rückgabewerte aller öffentlichen Typen und Member in der Komponente müssen Windows-Runtime-Typen sein. Diese Einschränkung gilt auch für die Windows-Runtime-Typen, die Sie erstellen und die Typen an, die von der Windows-Runtime selbst bereitgestellt werden. Außerdem trifft sie auf einige .NET Framework-Typen zu. Die Aufnahme dieser Typen ist Teil der Unterstützung der .NET Framework stellt die normale Verwendung der Windows-Runtime in verwaltetem Code&mdash;Code bekannte .NET Framework-Typen anstelle der zugrunde liegenden Windows-Runtime-Typen zu verwenden scheint. Beispielsweise können Sie .NET Framework-primitive Typen wie z. B. **Int32** und **doppelte**, bestimmte grundlagentypen wie **DateTimeOffset** und **Uri** , sowie einige häufig verwendete generische Schnittstellentypen wie z. B. **"IEnumerable"&lt;T&gt;**  (IEnumerable (Of T) in Visual Basic) und **IDictionary&lt; TKey, TValue&gt;** . Beachten Sie, dass die Typargumente dieser generischen Typen mit Windows-Runtime-Typen sein müssen. Dies wird in den Abschnitten erläutert [übergeben von Windows-Runtime-Typen an verwalteten Code](#passing-windows-runtime-types-to-managed-code) und [übergeben von verwalteten Typen an Windows-Runtime](#passing-managed-types-to-the-windows-runtime)weiter unten in diesem Thema.

- Öffentliche Klassen und Schnittstellen können Methoden, Eigenschaften und Ereignisse enthalten. Sie können Delegaten für Ereignisse zu deklarieren, oder die **EventHandler&lt;T&gt;**  delegieren. Eine öffentliche Klasse oder Schnittstelle kann nicht:
    - generisch sein.
    - Implementieren einer Schnittstelle, die nicht auf einem Windows-Runtime-Schnittstelle ist (jedoch Sie erstellen Ihre eigenen Windows-Runtime-Schnittstellen und diese implementieren).
    - Ableiten von Typen, die nicht in der Windows-Runtime, z. B. **"System.Exception"** und **System.EventArgs**.

- Alle öffentliche Typen müssen über einen Stammnamespace verfügen, der mit dem Assemblynamen übereinstimmt, und der Assemblyname darf nicht mit „Windows” beginnen.

    > **Tipp**. Standardmäßig haben Visual Studio-Projekte für Namespacenamen, die Namen der Assembly übereinstimmen. In Visual Basic wird die Namespace-Anweisung für diesen Standardnamespace nicht im Code angezeigt.

- Öffentliche Strukturen können nur öffentliche Felder als Member enthalten, und diese Felder müssen Werttypen oder Zeichenfolgen sein.
- Öffentliche Klassen müssen **versiegelt** (**NotInheritable** in Visual Basic) sein. Erfordert das Programmiermodell Polymorphie, können Sie eine öffentliche Schnittstelle erstellen und implementieren diese Schnittstelle für die Klassen, die polymorph sein müssen.

## <a name="debugging-your-component"></a>Debuggen der Komponente

Wenn sowohl Ihre UWP-app und Ihre Komponente mit verwaltetem Code basieren, können Sie beide gleichzeitig Debuggen.

Wenn Sie Ihre Komponente als Teil einer UWP-app mit C++ testen, können Sie verwalteten und systemeigenen Code gleichzeitig Debuggen. Die Vorgabe ist nur systemeigener Code.

## <a name="to-debug-both-native-c-code-and-managed-code"></a>So debuggen Sie systemeigenen C++-Code und verwalteten Code
1.  Öffnen Sie das Kontextmenü für das Visual C++-Projekt, und wählen Sie **Eigenschaften** aus.
2.  Wählen Sie auf den Eigenschaftenseiten unter **Konfigurationseigenschaften** die Option **Debuggen** aus.
3.  Wählen Sie **Debuggertyp** aus, und ändern Sie dann in der Dropdownliste **Nur systemeigen** in **Gemischt (verwaltet und systemeigen)** . Klicken Sie auf **OK**.
4.  Legen Sie Haltepunkte im systemeigenen und verwalteten Code fest.

Wenn Sie Ihre Komponente als Teil einer UWP-app mit JavaScript testen, ist die Lösung wird standardmäßig in JavaScript-Debugmodus. In Visual Studio ist das gleichzeitige Debuggen von JavaScript und verwaltetem Code nicht möglich.

## <a name="to-debug-managed-code-instead-of-javascript"></a>So debuggen Sie verwalteten Code anstelle von JavaScript
1.  Öffnen Sie das Kontextmenü für das JavaScript-Projekt, und wählen Sie **Eigenschaften** aus.
2.  Wählen Sie auf den Eigenschaftenseiten unter **Konfigurationseigenschaften** die Option **Debuggen** aus.
3.  Wählen Sie **Debuggertyp** aus, und ändern Sie in der Dropdownliste **Nur Skript** in **Nur verwaltet**. Klicken Sie auf **OK**.
4.  Legen Sie Haltepunkte im verwaltetem Code fest, und debuggen Sie wie gewohnt.

## <a name="passing-windows-runtime-types-to-managed-code"></a>Übergeben von Windows-Runtime Typen an verwalteten Code
Wie bereits im Abschnitt erwähnt [Deklarieren von Typen in Windows-Runtime-Komponenten](#declaring-types-in-windows-runtime-components), bestimmte .NET Framework-Typen können in den Signaturen von Membern öffentlicher Klassen erscheinen. Dies ist Teil der Unterstützung, die .NET Framework bietet, um die natürliche Verwendung der Windows-Runtime in verwaltetem Code zu ermöglichen. Darin sind primitive Typen und einige Klassen und Schnittstellen enthalten. Wenn die Komponente über JavaScript oder von C++-Code verwendet wird, ist es wichtig zu wissen, wie die .NET Framework-Typen an den Aufrufer angezeigt werden. Finden Sie unter [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) Beispiele mit JavaScript. In diesem Abschnitt werden häufig verwendete Typen beschrieben.

In .NET Framework, Primitive Typen wie die **Int32** Struktur besitzen viele nützliche Eigenschaften und Methoden, wie z. B. die **TryParse** Methode. Im Gegensatz dazu haben primitive Typen und Strukturen in der Windows-Runtime nur Felder. Wenn Sie diese Typen an verwalteten Code übergeben, verhalten sie sich wie .NET Framework-Typen, und Sie können die Eigenschaften und Methoden der .NET Framework-Typen wie gewohnt verwenden. Die folgende Liste fasst die Ersetzungen zusammen, die automatisch in der IDE vorgenommen werden:

-   Für die Windows-Runtime-primitive **Int32**, **Int64**, **einzelne**, **doppelte**, **booleschen**,  **Zeichenfolge** (eine unveränderliche Auflistung von Unicode-Zeichen), **Enum**, **UInt32**, **UInt64**, und **Guid**, verwenden Sie den Typ mit dem gleichen Namen im System-Namespace.
-   Für **UInt8**, verwenden Sie **System.Byte**.
-   Für **Char16**, verwenden Sie **System.Char**.
-   Für die **"iinspectable"** Schnittstelle, verwenden Sie **"System.Object"** .

Wenn C# oder Visual Basic bietet ein Programmiersprachen-Schlüsselwort für alle Typen, dann können Sie stattdessen das Schlüsselwort verwenden.

Zusätzlich zu primitiven Typen erscheinen einige grundlegende, häufig verwendete Windows-Runtime-Typen in verwaltetem Code als ihre .NET Framework-Entsprechung. Nehmen wir beispielsweise an Ihre JavaScript-Code verwendet die **Windows.Foundation.Uri** -Klasse, und Sie möchten, zum Übergeben einer C# oder Visual Basic-Methode. Entspricht der Typ in verwaltetem Code von .NET Framework ist **System.Uri** -Klasse, und das ist der Typ, der für den Methodenparameter verwendet. Sie können feststellen, wann ein Windows-Runtime-Typ als .NET Framework-Typ erscheint, da IntelliSense in Visual Studio den Windows-Runtime-Typ ausblendet, wenn Sie verwalteten Code schreiben und den entsprechenden .NET Framework-Typ anzeigt. (In der Regel haben beiden Typen den gleichen Namen. Beachten Sie jedoch, dass die **Windows.Foundation.DateTime** Struktur wird in verwaltetem Code als **System.DateTimeOffset** und nicht als **System.DateTime**.)

Für einige häufig verwendete Sammlungstypen erfolgt die Zuordnung zwischen den Schnittstellen, die von einem Windows-Runtime-Typ implementiert werden, und den Schnittstellen, die vom entsprechenden .NET Framework-Typ implementiert werden. Wie bei den bereits erwähnten Typen deklarieren Sie Parametertypen, indem Sie den .NET Framework-Typ verwenden. Dadurch werden bestimmte Unterschiede zwischen den Typen ausgeblendet, und das Schreiben von .NET Framework-Code wirkt natürlicher.

In der folgenden Tabelle sind die häufigsten dieser generischen Schnittstellentypen zusammen mit anderen allgemeinen Klassen- und Schnittstellenzuordnungen aufgeführt. Eine vollständige Liste der Windows-Runtime-Typen, die vom .NET Framework zugeordnet werden soll, finden Sie unter [.NET Framework-Zuordnungen von Windows-Runtime-Typen](net-framework-mappings-of-windows-runtime-types.md).

| Windows-Runtime                                  | .NET Framework                                    |
|-|-|
| IIterable&lt;T&gt;                               | IEnumerable&lt;T&gt;                              |
| IVector&lt;T&gt;                                 | IList&lt;T&gt;                                    |
| IVectorView&lt;T&gt;                             | IReadOnlyList&lt;T&gt;                            |
| IMap&lt;K, V&gt;                                 | IDictionary&lt;TKey, TValue&gt;                   |
| IMapView&lt;K, V&gt;                             | IReadOnlyDictionary&lt;TKey, TValue&gt;           |
| IKeyValuePair&lt;K, V&gt;                        | KeyValuePair&lt;TKey, TValue&gt;                  |
| IBindableIterable                                | IEnumerable                                       |
| IBindableVecinr                                  | IList                                             |
| Windows.UI.Xaml.Data.INotifyPropertyChanged      | System.ComponentModel.INotifyPropertyChanged      |
| Windows.UI.Xaml.Data.PropertyChangedEventHandler | System.ComponentModel.PropertyChangedEventHandler |
| Windows.UI.Xaml.Data.PropertyChangedEventArgs    | System.ComponentModel.PropertyChangedEventArgs    |

Wenn ein Typ mehrere Schnittstellen implementiert, können Sie jede Schnittstelle verwenden, die als Parametertyp oder Rückgabetyp eines Members implementiert wird. Sie können z. B. übergeben oder Zurückgeben einer **Wörterbuch&lt;ganze Zahl, Zeichenfolge&gt;**  (**Dictionary (Of Integer, String)** in Visual Basic) als **IDictionary&lt;ganze Zahl, Zeichenfolge&gt;** , **IReadOnlyDictionary&lt;ganze Zahl, Zeichenfolge&gt;** , oder **"IEnumerable"&lt; System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;&gt;** .

> [!IMPORTANT]
> JavaScript verwendet die Schnittstelle, die zuerst in der Liste der Schnittstellen angezeigt wird, die ein verwalteter Typ implementiert. Wenn Sie zurückkehren, z. B. **Wörterbuch&lt;ganze Zahl, Zeichenfolge&gt;**  an JavaScript-Code, sie wird als **IDictionary&lt;ganze Zahl, Zeichenfolge&gt;**  unabhängig davon, welche Schnittstelle, die Sie als Rückgabetyp angeben. Das bedeutet, dass die erste Schnittstelle Member enthalten muss, die in den nächsten Schnittstellen erscheinen, damit diese Member für JavaScript sichtbar sind.

In der Windows-Runtime **IMap&lt;K, V&gt;**  und **IMapView&lt;K, V&gt;**  durchlaufen, indem Sie IKeyValuePair. Wenn Sie sie an verwalteten Code übergeben, werden sie als **IDictionary&lt;TKey, TValue&gt;**  und **IReadOnlyDictionary&lt;TKey, TValue&gt;** , sodass Sie verwenden, auf natürliche Weise **System.Collections.Generic.KeyValuePair&lt;TKey, TValue&gt;**  um diese aufzulisten.

Die Darstellungsweise von Schnittstellen in verwaltetem Code wirkt sich auf die Darstellungsweise der Typen aus, die diese Schnittstellen implementieren. Z. B. die **PropertySet** -Klasse implementiert **IMap&lt;K, V&gt;** , die angezeigt wird, in verwaltetem Code als **IDictionary&lt;TKey, TValue&gt;** . **PropertySet** scheint, als ob er implementiert **IDictionary&lt;TKey, TValue&gt;**  anstelle von **IMap&lt;K, V&gt;** in verwaltet Code anscheinend über eine **hinzufügen** -Methode, die wie verhält sich die **hinzufügen** -Methode in .NET Framework-Wörterbüchern. Es nicht angezeigt, damit ein **einfügen** Methode. Sie finden dieses Beispiel im Thema [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="passing-managed-types-to-the-windows-runtime"></a>Übergeben von verwalteten Typen an die Windows-Runtime

Wie bereits im vorherigen Abschnitt erwähnt, können einige Windows-Runtime-Typen als .NET Framework-Typen in den Signaturen von Komponentenmembern oder in den Signaturen von Windows-Runtime-Membern erscheinen, wenn Sie sie in der IDE verwenden. Wenn Sie .NET Framework-Typen an diese Member übergeben oder als Rückgabewerte von Komponentenmembern verwenden, werden sie dem Code auf der anderen Seite als der entsprechende Windows-Runtime-Typ dargestellt. Weitere Beispiele für die Auswirkungen, wenn die Komponente aus JavaScript aufgerufen wird, finden Sie unter im Abschnitt "Rückgabe von verwalteten Typen aus der Komponente" in [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

## <a name="overloaded-methods"></a>Überladene Methoden

In der Windows-Runtime können Methoden überladen werden. Wenn Sie mehrere Überladungen mit der gleichen Anzahl von Parametern deklarieren, Sie müssen jedoch anwenden, die [ **Windows.Foundation.Metadata.DefaultOverloadAttribute** ](/uwp/api/windows.foundation.metadata.defaultoverloadattribute) -Attribut auf nur eine dieser Überladungen. Nur diese Überladung kann aus JavaScript aufgerufen werden. Im folgenden Code ist beispielsweise die Überladung, die **int** übernimmt (**Integer** in Visual Basic), die Standardüberladung.

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

> [WICHTIG] JavaScript ermöglicht die Übergabe aller Werte, **OverloadExample**, und wandelt den Wert in den Typ, der durch den Parameter erforderlich ist. Rufen Sie **OverloadExample** mit "Zweiundvierzig", "42" oder 42,3 alle diese Werte werden an die Überladung übergeben. Die standardmäßige Überladung im vorherigen Beispiel gibt 0, 42 und 42 zurück.

Sie können nicht angewendet werden die **DefaultOverloadAttribut**e-Attribut an die Konstruktoren. Alle Konstruktoren in einer Klasse müssen eine unterschiedliche Anzahl von Parametern aufweisen.

## <a name="implementing-istringable"></a>Implementieren von IStringable

Ab Windows 8.1, die Windows-Runtime enthält eine **IStringable** Schnittstelle, deren einzige Methode, **IStringable.ToString**, eine grundlegende formatierungsunterstützung vergleichbar mit der gebotenen bietet **Object.ToString**. Wenn Sie sich entschließen, implementieren Sie **IStringable** in einem öffentlich verwalteten Typ, der in einer Windows-Runtime-Komponente exportiert wird, gelten die folgenden Einschränkungen:

-   Sie können definieren, die **IStringable** -Schnittstelle nur in einer "class Implements"-Beziehung, z. B. den folgenden Code in C#:

    ```cs
    public class NewClass : IStringable
    ```

    Oder in Visual Basic-Code:

    ```vb
    Public Class NewClass : Implements IStringable
    ```

-   Sie können nicht implementieren **IStringable** auf einer Schnittstelle.
-   Sie können einen Parameter vom Typ nicht deklarieren **IStringable**.
-   **IStringable** darf nicht der Rückgabetyp einer Methode, eine Eigenschaft oder ein Feld sein.
-   Sie können nicht ausgeblendet werden Ihre **IStringable** Implementierung von Basisklassen mithilfe eine Methodendefinition wie die folgende:

    ```cs
    public class NewClass : IStringable
    {
       public new string ToString()
       {
          return "New ToString in NewClass";
       }
    }
    ```

    Stattdessen die **IStringable.ToString** Implementierung muss die basisklassenimplementierung immer überschreiben. Sie können Ausblenden einer **ToString** Implementierung nur, indem sie auf einer stark typisierten Klasseninstanz aufrufen.

> [!NOTE]
> Unter einer Vielzahl von Bedingungen, Aufrufe von systemeigenem Code von einem verwalteten Typ, der implementiert **IStringable** oder blendet Sie aus der **ToString** Implementierung kann zu unerwartetem Verhalten führen.

## <a name="asynchronous-operations"></a>Asynchrone Vorgänge

Um eine asynchrone Methode in der Komponente zu implementieren, fügen Sie hinzu "Async" am Ende des Methodennamens, und zurückgeben Sie eines der Windows-Runtime-Schnittstellen, die asynchrone Aktionen oder Operationen darstellen: **IAsyncAction**, **IAsyncActionWithProgress&lt;TProgress&gt;** , **IAsyncOperation&lt;TResult&gt;** , oder **IAsyncOperationWithProgress&lt;TResult, TProgress&gt;** .

Können Sie .NET Framework-Aufgaben (die [ **Aufgabe** ](/dotnet/api/system.threading.tasks.task) -Klasse und generische [ **Aufgabe&lt;TResult&gt;**  ](/dotnet/api/system.threading.tasks.task-1) Klasse), die asynchrone Methode zu implementieren. Sie müssen eine Aufgabe, die eine laufende Operation, wie z. B. eine Aufgabe darstellt, die von einer asynchronen Methode in geschriebenen zurückgegeben wird zurückgeben C# oder Visual Basic oder eine Aufgabe, die von zurückgegeben wird das [ **"Task.Run"** ](/dotnet/api/system.threading.tasks.task.run) -Methode. Wenn Sie für die Aufgabenerstellung einen Konstruktor verwenden, müssen Sie seine [Task.Start](/dotnet/api/system.threading.tasks.task.start)-Methode vor der Rückgabe aufrufen.

Eine Methode, verwendet `await` (`Await` in Visual Basic) erfordert die `async` Schlüsselwort (`Async` in Visual Basic). Wenn Sie solche Methode einer Komponente für Windows-Runtime verfügbar machen, wenden Sie die `async` Schlüsselwort an den Delegaten, die Sie zum Übergeben der **ausführen** Methode.

Für asynchrone Aktionen und Vorgänge, die weder die Abbruch- noch die Fortschrittsberichterstattung unterstützen, können Sie mit der Erweiterungsmethode [WindowsRuntimeSystemExtensions.AsAsyncAction](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) oder [AsAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system?redirectedfrom=MSDN) die Aufgabe in die entsprechende Schnittstelle umschließen. Beispielsweise implementiert der folgende Code eine asynchrone Methode mithilfe der **"Task.Run"&lt;TResult&gt;**  Methode für den Aufgabenstart nutzt. Die **AsAsyncOperation&lt;TResult&gt;**  Erweiterungsmethode gibt die Aufgabe als asynchronen Vorgang Windows-Runtime zurück.

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

Der folgende JavaScript-Code zeigt, wie die Methode mithilfe von aufgerufen werden konnte eine [ **WinJS.Promise** ](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10)) Objekt. Die an die then-Methode übergebene Funktion wird ausgeführt, wenn der asynchrone Aufruf abgeschlossen ist. Der StringList-Parameter enthält die Liste der Zeichenfolgen, die von zurückgegeben wird das **DownloadAsStringAsync** -Methode, die Funktion übernimmt erforderliche Verarbeitung erforderlich ist.

```javascript
function asyncExample(id) {

    var result = SampleComponent.Example.downloadAsStringAsync(id).then(
        function (stringList) {
            // Place code that uses the returned list of strings here.
        });
}
```

Verwenden Sie für asynchrone Aktionen und Vorgänge, die Abbruch oder Fortschrittsberichterstattung unterstützt, die [ **AsyncInfo** ](/dotnet/api/system.runtime.interopservices.windowsruntime) Klasse, die eine begonnene Aufgabe zu generieren und zu verknüpfen, die die Abbruch- und fortschrittsberichterstattungs- die Funktionen des Tasks mit den Abbruch und Funktionen von der entsprechenden Windows-Runtime-Schnittstelle für statusberichterstellung. Ein Beispiel, Abbruch- und Fortschrittsberichterstattung unterstützt, finden Sie unter [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md).

Beachten Sie, dass Sie die Methoden verwenden, können die **AsyncInfo** Klasse, auch wenn die asynchrone Methode unterstützen einen Abbruch oder Fortschrittsberichterstattung nicht. Wenn Sie eine Visual Basic-Lambda-Funktion verwenden oder ein C# anonyme Methode, geben Sie keine Parameter für das Token und [ **IProgress&lt;T&gt;**  ](https://docs.microsoft.com/dotnet/api/system.iprogress-1?redirectedfrom=MSDN) Schnittstelle. Wenn Sie eine C#-Lambda-Funktion verwenden, geben Sie einen Tokenparameter an, aber ignorieren Sie ihn. Im vorherigen Beispiel verwendet die AsAsyncOperation&lt;TResult&gt; Methode sieht wie folgt aus, bei der Verwendung der [ **AsyncInfo.Run&lt;TResult&gt;(Func&lt; CancellationToken, Task&lt;TResult&gt;&gt;** ](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime?redirectedfrom=MSDN))-Überladungsmethode.

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

Wenn Sie eine asynchrone Methode, die optional die Abbruch- oder Fortschrittsberichterstattung unterstützt erstellen, erwägen Sie Überladungen, die keine Parameter für ein Abbruchtoken haben oder die **IProgress&lt;T&gt;**  -Schnittstelle.

## <a name="throwing-exceptions"></a>Auslösen von Ausnahmen

Sie können jeden Ausnahmetyp auslösen, der in .NET für Windows-Apps enthalten ist. Sie können keine eigenen öffentlichen Ausnahmetypen in einer Komponente für Windows-Runtime deklarieren, aber Sie können nicht öffentliche Typen deklarieren und auslösen.

Wenn die Komponente die Ausnahme nicht behandelt, wird eine entsprechende Ausnahme im Code ausgelöst, der die Komponente aufgerufen hat. Die Unterstützung der Windows-Runtime durch die aufrufende Sprache bestimmt, wie die Ausnahme dem Aufrufer dargestellt wird.

-   In JavaScript erscheint die Ausnahme als Objekt, in dem die Ausnahmemeldung durch eine Stapelüberwachung ersetzt ist. Wenn Sie Ihre App in Visual Studio debuggen, wird der Originaltext der Meldung im Ausnahmedialogfeld des Debuggers unter „WinRT Information" angezeigt. Sie können mit JavaScript-Code nicht auf den Originaltext der Meldung zugreifen.

    > **Tipp**. Derzeit wird die stapelüberwachung enthält den verwalteten Ausnahmetyp, aber nicht empfohlen, analysieren die Ablaufverfolgung aus, um den Ausnahmetyp zu identifizieren. Verwenden Sie stattdessen einen HRESULT-Wert, wie weiter unten in diesem Abschnitt beschrieben.

-   In C++ erscheint die Ausnahme als Plattformausnahme. Wenn HResult-Eigenschaft der verwalteten Ausnahme dem HRESULT des einer bestimmten plattformausnahme zugeordnet werden kann, wird die spezielle Ausnahme verwendet. andernfalls ein [ **Platform:: COMException** ](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class) Ausnahme ausgelöst. Der Meldungstext der verwalteten Ausnahme ist für C++-Code nicht verfügbar. Wenn eine bestimmte Plattformausnahme ausgelöst wurde, erscheint der Meldungstext für diesen Ausnahmetyp. Andernfalls wird kein Meldungstext ausgegeben. Siehe [Ausnahmen (C++/CX)](https://docs.microsoft.com/cpp/cppcx/exceptions-c-cx).
-   In C# oder Visual Basic ist die Ausnahme eine normale verwaltete Ausnahme.

Wenn Sie in Ihrer Komponente eine Ausnahme auslösen, sollten Sie einen nicht öffentlichen Ausnahmetyp verwenden, dessen HResult-Eigenschaftswert speziell für Ihre Komponente gilt, damit die Ausnahme leichter von einem JavaScript- oder C++-Aufrufer verwaltet werden kann. Das HRESULT ist verfügbar, um eine JavaScript-Aufrufer über das Ausnahmeobjekt Number-Eigenschaft und einer C++-Aufrufer über die [ **COMException:: HRESULT** ](https://docs.microsoft.com/cpp/cppcx/platform-comexception-class#hresult) Eigenschaft.

> [!NOTE]
> Verwenden Sie einen negativen Wert für Ihre HRESULT. Ein positiver Wert wird als Erfolg interpretiert und im JavaScript- oder C++-Aufrufer wird keine Ausnahme ausgelöst.

## <a name="declaring-and-raising-events"></a>Deklarieren und Auslösen von Ereignissen

Wenn Sie einen Typ deklarieren, um die Daten für das Ereignis aufzunehmen, leiten Sie diesen von „Object“ und nicht von „EventArgs“ ab, da „EventArgs“ kein Windows-Runtime-Typ ist. Verwenden Sie [ **EventHandler&lt;TEventArgs&gt;**  ](https://docs.microsoft.com/dotnet/api/system.eventhandler-1?redirectedfrom=MSDN) als Typ des Ereignisses, und verwenden Sie den Ereignisargumenttyp als das allgemeine Typargument. Lösen Sie das Ereignis genauso wie in einer .NET Framework-Anwendung aus.

Wenn Ihre Komponente für Windows-Runtime von JavaScript oder C++ verwendet wird, folgt das Ereignis dem Windows-Runtime-Ereignismuster, das diese Sprachen erwarten. Wenn Sie die Komponente in C# oder Visual Basic verwenden, erscheint das Ereignis als normales .NET Framework-Ereignis. Ein Beispiel finden Sie [Exemplarische Vorgehensweise: Erstellen einer einfachen Komponente in C# oder Visual Basic und Aufrufen dieser Komponente über JavaScript](/windows/uwp/winrt-components/walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript).

Wenn Sie benutzerdefinierte Ereignisaccessoren implementieren (in Visual Basic ein Ereignis mit dem Schlüsselwort **Custom** deklarieren), müssen Sie in der Implementierung das Windows-Runtime-Ereignismuster verwenden. Siehe [Benutzerdefinierte Ereignisse und Ereignisaccessoren in Komponenten für Windows-Runtime](custom-events-and-event-accessors-in-windows-runtime-components.md). Beachten Sie, dass das Ereignis auch dann als einfaches .NET Framework-Ereignis erscheint, wenn Sie C#- oder Visual Basic-Code verwenden.

## <a name="next-steps"></a>Nächste Schritte

Nachdem Sie eine Komponente für Windows-Runtime für eigene Zwecke erstellt haben, stellen Sie möglicherweise fest, dass die Funktionen, die diese kapselt, für andere Entwickler nützlich sind. Sie haben zwei Optionen, um eine Komponente für die Verteilung an andere Entwickler zu packen. Siehe [Verteilen einer verwalteten Komponente für Windows-Runtime](https://docs.microsoft.com/previous-versions/windows/apps/jj614475(v=vs.140)).

Weitere Informationen zu Visual Basic- und C#-Sprachfunktionen und zur .NET Framework-Unterstützung für die Windows-Runtime finden Sie unter [Visual Basic- und C#-Programmiersprachenreferenz](https://docs.microsoft.com/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).

## <a name="related-topics"></a>Verwandte Themen
* [.NET für UWP-apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Exemplarische Vorgehensweise: Erstellen einer einfachen Windows-Runtime-Komponente, und es über JavaScript aufrufen](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
