---
title: Exemplarische Vorgehensweise zum C# Erstellen eines-oder-Visual Basic Windows-Runtime Komponente und zum Aufrufen dieser Komponente über JavaScript
description: In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie .NET Framework mit Visual Basic oder C# verwenden, um eigene Windows-Runtime-Typen zu erstellen, die in einer Komponente für Windows-Runtime verpackt sind, und wie Sie die Komponente in Ihrer universellen Windows-App aufrufen, die mit JavaScript für Windows erstellt wurde.
ms.assetid: 1565D86C-BF89-4EF3-81FE-35367DB8D671
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0429d5eafb2ade013e54f40dc0f2803e7a5e08f2
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340628"
---
# <a name="walkthrough-of-creating-a-c-or-visual-basic-windows-runtime-component-and-calling-it-from-javascript"></a>Exemplarische Vorgehensweise zum C# Erstellen eines-oder-Visual Basic Windows-Runtime Komponente und zum Aufrufen dieser Komponente über JavaScript

In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie .NET Framework mit Visual Basic oder C# verwenden, um eigene Windows-Runtime-Typen zu erstellen, die in einer Komponente für Windows-Runtime verpackt sind, und wie Sie die Komponente in Ihrer universellen Windows-App aufrufen, die mit JavaScript für Windows erstellt wurde.

Visual Studio erleichtert das Hinzufügen einer Komponente für Windows-Runtime, die mit C# oder Visual Basic geschrieben wurde, zu Ihrer App, und das Erstellen von Windows-Runtime-Typen, die Sie von JavaScript aus aufrufen können. Intern können Ihre Windows-Runtime-Typen jede .NET Framework-Funktion verwenden, die in einer universellen Windows-App zulässig ist. (Weitere Informationen finden Sie unter [Windows-Runtime Komponenten mit C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) und [Übersicht über .net für UWP-apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0).) Extern können die Member ihres Typs nur Windows-Runtime Typen für Ihre Parameter und Rückgabewerte verfügbar machen. Wenn Sie die Projekt Mappe erstellen, erstellt Visual Studio Ihre .NET Framework Windows-Runtime Komponenten Projekt und führt dann einen Buildschritt aus, der eine Windows-Metadatendatei (. winmd) erstellt. Hierbei handelt es sich um Ihre Komponente für Windows-Runtime, die Visual Studio in Ihre App einschließt.

> **Hinweis**  Der .NET Framework ordnet einige häufig verwendete .NET Framework Typen, z. b. primitive Datentypen und Auflistungs Typen, Ihren Windows-Runtime Entsprechungen automatisch zu. Diese .NET Framework-Typen können in der öffentlichen Schnittstelle einer Komponente für Windows-Runtime verwendet werden und werden Benutzern der Komponente als die entsprechenden Windows-Runtime-Typen angezeigt. Weitere Informationen finden Sie [unter Windows-Runtime-Komponenten C# mit und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md).

In dieser exemplarischen Vorgehensweise werden die folgenden Aufgaben veranschaulicht. Nachdem Sie den ersten Abschnitt abgeschlossen haben, in dem die Windows-App mit JavaScript einrichtet wird, können Sie die restlichen Abschnitte in beliebiger Reihenfolge abschließen.

## <a name="prerequisites"></a>Voraussetzungen:

-   Windows 10
-   Microsoft Visual Studio 2015 oder Microsoft Visual Studio Community 2015

## <a name="creating-a-simple-windows-runtime-class"></a>Erstellen eine einfache Windows-Runtime-Klasse

In diesem Abschnitt wird eine universelle Windows-App erstellt, die mit JavaScript für Windows erstellt C# wurde, und es wird eine Visual Basic-oder Windows-Runtime Komponenten Projekt Es wird gezeigt, wie ein verwalteter Windows-Runtime-Typ definiert, eine Instanz des Typs aus JavaScript erstellt und statische sowie Instanzmember aufgerufen werden. Die visuelle Darstellung der Beispiel-App ist absichtlich einfach, um den Fokus auf der Komponente zu behalten. Sie können die Darstellung gerne verschönern.

1.  Erstellen Sie in Visual Studio ein neues JavaScript-Projekt: Wählen Sie in der Menüleiste **Datei, neu und Projekt**aus. Wählen Sie im Abschnitt **Installierte Vorlagen** des Dialogfelds **Neues Projekt** die Option **JavaScript** und dann **Windows** sowie **Universelle** aus. (Wenn Windows nicht verfügbar ist, stellen Sie sicher, dass Sie Windows 8 oder höher verwenden.) Wählen Sie die Vorlage **leere Anwendung** aus, und geben Sie als Projektname SampleApp ein.
2.  Erstellen Sie das Komponenten Projekt: Öffnen Sie in Projektmappen-Explorer das Kontextmenü für die SampleApp-Projekt Mappe, und wählen Sie **Hinzufügen**und dann **Neues Projekt** aus C# , um der Projekt Mappe ein neues Projekt oder Visual Basic Projekt hinzuzufügen. Wählen Sie im Abschnitt **Installierte Vorlagen** des Dialogfelds **Neues Projekt hinzufügen** die Option **Visual Basic** oder **Visual C#** und dann **Windows** und **Universelle** aus. Wählen Sie die Vorlage **Komponente für Windows-Runtime** aus, und geben Sie **SampleComponent** als Projektnamen ein.
3.  Ändern Sie den Namen der Klasse in **Example**. Beachten Sie, dass die Klasse standardmäßig mit **public sealed** (**Public NotInheritable** in Visual Basic) gekennzeichnet ist. Die Windows-Runtime-Klassen, die Sie von Ihrer Komponente aus bereitstellen, müssen versiegelt sein.
4.  Fügen Sie der Klasse zwei einfache Member hinzu, eine **static**-Methode (**Shared**-Methode in Visual Basic) und eine Instanzeigenschaft:

    > [!div class="tabbedCodeSnippets"]
    > ```csharp
    > namespace SampleComponent
    > {
    >     public sealed class Example
    >     {
    >         public static string GetAnswer()
    >         {
    >             return "The answer is 42.";
    >         }
    >
    >         public int SampleProperty { get; set; }
    >     }
    > }
    > ```
    > ```vb
    > Public NotInheritable Class Example
    >     Public Shared Function GetAnswer() As String
    >         Return "The answer is 42."
    >     End Function
    >
    >     Public Property SampleProperty As Integer
    > End Class
    > ```

5.  Optional: Um IntelliSense für die neu hinzugefügten Elemente zu aktivieren, öffnen Sie in Projektmappen-Explorer das Kontextmenü für das Projekt SampleComponent, und wählen Sie dann **Erstellen**aus.
6.  Öffnen Sie im Projektmappen-Explorer im JavaScript-Projekt das Kontextmenü für **Verweise**, und wählen Sie dann **Verweis hinzufügen** aus, um den **Verweis-Manager** zu öffnen. Wählen Sie **Projekte** und dann **Projektmappe** aus. Aktivieren Sie das Kontrollkästchen für das Projekt „SampleComponent“, und wählen Sie **OK** aus, um einen Verweis hinzuzufügen.

## <a name="call-the-component-from-javascript"></a>Aufrufen der Komponente über JavaScript

Um den Windows-Runtime-Typ aus JavaScript zu verwenden, fügen Sie den folgenden Code in der anonymen Funktion in der Datei „default.js“ (im Ordner „js“ des Projekts) hinzu, die von der Visual Studio-Vorlage bereitgestellt wird. Sie sollten ihn nach dem Ereignishandler „app.oncheckpoint“ und vor dem Aufruf von „app.start“ einfügen.

```javascript
var ex;

function basics1() {
   document.getElementById('output').innerHTML =
        SampleComponent.Example.getAnswer();

    ex = new SampleComponent.Example();

   document.getElementById('output').innerHTML += "<br/>" +
       ex.sampleProperty;

}

function basics2() {
    ex.sampleProperty += 1;
    document.getElementById('output').innerHTML += "<br/>" +
        ex.sampleProperty;
}
```

Beachten Sie, dass der erste Buchstaben der einzelnen Membernamen von Großbuchstaben in Kleinbuchstaben geändert wird. Diese Transformation ist Teil der Unterstützung, die JavaScript bereitstellt, um die natürliche Verwendung von Windows-Runtime zu ermöglichen. Für Namespaces und Klassennamen wird die Pascal-Schreibweise verwendet. Für Membernamen wird mit Ausnahme von Ereignisnamen, die nur aus Kleinbuchstaben bestehen, die Camel-Schreibweise verwendet. Informationen finden Sie unter [Verwenden der Windows-Runtime in JavaScript](https://docs.microsoft.com/scripting/jswinrt/using-the-windows-runtime-in-javascript). Die Regeln für die Camel-Schreibweise können verwirrend sein. Normalerweise wird eine Reihe von anfänglichen Großbuchstaben als Kleinbuchstaben angezeigt, aber wenn ein Kleinbuchstabe drei Großbuchstaben folgt, werden nur die ersten beiden Buchstaben in Kleinbuchstaben angezeigt: Ein Element mit dem Namen „IDStringKind“ wird z. B. als „idStringKind“ angezeigt. In Visual Studio können Sie Ihr Komponentenprojekt für Windows-Runtime erstellen und dann IntelliSense in Ihrem JavaScript-Projekt verwenden, um die richtige Groß- und Kleinschreibung anzuzeigen.

In ähnlicher Weise unterstützt .NET Framework die natürliche Verwendung der Windows-Runtime in verwaltetem Code. Dies wird in den nachfolgenden Abschnitten dieses Artikels und in den Artikeln [Windows-Runtime Komponenten mit C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) und [.NET Framework Unterstützung für UWP-apps und Windows-Runtime](https://docs.microsoft.com/dotnet/standard/cross-platform/support-for-windows-store-apps-and-windows-runtime)erläutert.

## <a name="create-a-simple-user-interface"></a>Erstellen einer einfachen Benutzeroberfläche

Öffnen Sie in Ihrem JavaScript-Projekt die Datei „default.HTML“, und aktualisieren Sie den Text wie im folgenden Code dargestellt. Dieser Code enthält den vollständigen Satz von Steuerelementen für die Beispiel-App und gibt die Funktionsnamen für die Klickereignisse an.

> **Hinweis**  Wenn Sie die APP zum ersten Mal ausführen, werden nur die Schaltflächen Schaltflächen basics1 und Basics2 unterstützt.

```html
<body>
            <div id="buttons">
            <button id="button1" >Basics 1</button>
            <button id="button2" >Basics 2</button>

            <button id="runtimeButton1">Runtime 1</button>
            <button id="runtimeButton2">Runtime 2</button>

            <button id="returnsButton1">Returns 1</button>
            <button id="returnsButton2">Returns 2</button>

            <button id="events1Button">Events 1</button>

            <button id="btnAsync">Async</button>
            <button id="btnCancel" disabled="disabled">Cancel Async</button>
            <progress id="primeProg" value="25" max="100" style="color: yellow;"></progress>
        </div>
        <div id="output">
        </div>
</body>
```

Öffnen Sie in Ihrem JavaScript-Projekt im Ordner „css“ die Datei „default.css“. Ändern Sie den Textabschnitt wie dargestellt, und fügen Sie Formate hinzu, um das Layout von Schaltflächen und die Anordnung des Ausgabetexts zu steuern.

```css
body
{
    -ms-grid-columns: 1fr;
    -ms-grid-rows: 1fr 14fr;
    display: -ms-grid;
}

#buttons {
    -ms-grid-rows: 1fr;
    -ms-grid-columns: auto;
    -ms-grid-row-align: start;
}
#output {
    -ms-grid-row: 2;
    -ms-grid-column: 1;
}
```

Fügen Sie jetzt Ereignislistener-Registrierungscode hinzu, indem Sie dem processAll-Aufruf in „app.onactivated“ in „default.js“ eine THEN-Klausel hinzufügen. Ersetzen Sie die vorhandene Codezeile, die „setPromise“ aufruft, durch folgenden Code:

```javascript
args.setPromise(WinJS.UI.processAll().then(function () {
    var button1 = document.getElementById("button1");
    button1.addEventListener("click", basics1, false);
    var button2 = document.getElementById("button2");
    button2.addEventListener("click", basics2, false);
}));
```

Dies ist eine bessere Möglichkeit, Ereignisse zu HTML-Steuerelementen hinzuzufügen, als das Hinzufügen eines Klickereignishandlers direkt im HTML-Code. Weitere Informationen finden Sie unter [Erstellen einer "Hello, World"-app (JS)](/windows/uwp/get-started/create-a-hello-world-app-js-uwp).

## <a name="build-and-run-the-app"></a>Erstellen und Ausführen der App

Ändern Sie vor dem Erstellen die Zielplattform für alle Projekte auf ARM, x64 oder x86 – je nachdem, was für Ihren Computer erforderlich ist.

Drücken Sie zum Erstellen und Ausführen der Projektmappe die F5-TASTE. (Falls ein Laufzeitfehler angezeigt wird, der besagt, dass „SampleComponent“ nicht definiert ist, fehlt der Verweis auf das Klassenbibliotheksprojekt.)

Visual Studio kompiliert zunächst die Klassenbibliothek und führt dann eine MSBuild-Aufgabe aus, die [Winmdexp.exe (Windows-Runtime-Metadatenexporttool)](https://docs.microsoft.com/dotnet/framework/tools/winmdexp-exe-windows-runtime-metadata-export-tool) ausführt, um Ihre Komponente für Windows-Runtime zu erstellen. Die Komponente wird in eine WINMD-Datei eingeschlossen, die den verwalteten Code und die Windows-Metadaten enthält, die den Code beschreiben. „WinMdExp.exe“ generiert Build-Fehlermeldungen, wenn Sie Code schreiben, der in einer Komponente für Windows-Runtime ungültig ist, und die Fehlermeldungen werden in der Visual Studio-IDE angezeigt. Visual Studio fügt die Komponente zum App-Paket (APPX-Datei) für Ihre universelle Windows-App hinzu und generiert das entsprechende Manifest.

Wählen Sie die Schaltfläche „Basics1“ aus, um dem Ausgabebereich den Rückgabewert aus der statischen GetAnswer-Methode zuzuweisen, eine Instanz der Example-Klasse zu erstellen und den Wert der SampleProperty-Eigenschaft im Ausgabebereich anzuzeigen. Die Ausgabe ist hier dargestellt:

``` syntax
"The answer is 42."
0
```

Wählen Sie Schaltfläche „Basics2“ aus, um den Wert der SampleProperty-Eigenschaft zu erhöhen und den neuen Wert im Ausgabebereich anzuzeigen. Primitive Typen wie Zeichenfolgen und Zahlen können als Parameter- und Rückgabetypen verwendet werden und zwischen verwaltetem Code und JavaScript übergeben werden. Da Zahlen in JavaScript im Gleitkommaformat mit doppelter Genauigkeit gespeichert werden, werden sie in numerische .NET Framework-Typen konvertiert.

> **Hinweis**  standard mäßig können Sie Haltepunkte nur im JavaScript-Code festlegen. Informationen zum Debuggen C# von Visual Basic oder Code finden Sie unter C# Erstellen von Windows-Runtime Komponenten in und Visual Basic.

Wechseln Sie von der App zu Visual Studio, und drücken Sie Umschalt+F5, um den Debugmodus zu beenden und die App zu schließen.

## <a name="using-the-windows-runtime-from-javascript-and-managed-code"></a>Verwenden von Windows-Runtime aus JavaScript und verwaltetem Code

Windows-Runtime kann aus JavaScript oder verwaltetem Code aufgerufen werden. Windows-Runtime-Objekte können zwischen den beiden übergeben werden, und Ergebnisse können auf beiden Seiten behandelt werden. Allerdings unterscheiden sich die Verwendungsmöglichkeiten der Windows-Runtime-Typen in den beiden Umgebungen in einigen Details, da JavaScript und .NET Framework Windows-Runtime unterschiedlich unterstützen. Das folgende Beispiel veranschaulicht diese Unterschiede mithilfe der [Windows.Foundation.Collections.PropertySet](https://docs.microsoft.com/uwp/api/windows.foundation.collections.propertyset)-Klasse. In diesem Beispiel erstellen Sie eine Instanz der PropertySet-Sammlung in verwaltetem Code und registrieren einen Ereignishandler zum Nachverfolgen der Änderungen in der Sammlung. Dann fügen Sie JavaScript-Code hinzu, der die Auflistung abruft, einen eigenen Ereignishandler registriert und die Auflistung verwendet. Zuletzt fügen Sie eine Methode hinzu, die Änderungen an der Sammlung von verwaltetem Code aus vornimmt und das Behandeln einer verwalteten Ausnahme durch JavaScript zeigt.

> **Wichtig**  In diesem Beispiel wird das Ereignis im UI-Thread ausgelöst. Wenn Sie das Ereignis über einen Hintergrundthread auslösen, z. B. in einem asynchronen Aufruf, müssen Sie einige zusätzliche Arbeiten ausführen, damit JavaScript das Ereignis behandelt. Weitere Informationen finden Sie unter [Auswerfen von Ereignissen in Windows-Runtime-Komponenten](raising-events-in-windows-runtime-components.md).

Fügen Sie im SampleComponent-Projekt eine neue **public sealed**-Klasse (**Public NotInheritable**-Klasse in Visual Basic) mit dem Namen „PropertySetStats“ hinzu. Die Klasse umschließt eine PropertySet-Auflistung und behandelt das MapChanged-Ereignis. Der Ereignishandler verfolgt die Anzahl der aufgetretenen Änderungen jeder Art, und die DisplayStats-Methode erstellt einen Bericht, der in HTML formatiert ist. Beachten Sie die zusätzliche **using**-Anweisung (**Imports**-Anweisung in Visual Basic). Achten Sie darauf, diese Anweisung den vorhandenen **using**-Anweisungen hinzuzufügen, anstatt sie zu überschreiben.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using Windows.Foundation.Collections;
>
> namespace SampleComponent
> {
>     public sealed class PropertySetStats
>     {
>         private PropertySet _ps;
>         public PropertySetStats()
>         {
>             _ps = new PropertySet();
>             _ps.MapChanged += this.MapChangedHandler;
>         }
>
>         public PropertySet PropertySet { get { return _ps; } }
>
>         int[] counts = { 0, 0, 0, 0 };
>         private void MapChangedHandler(IObservableMap<string, object> sender,
>             IMapChangedEventArgs<string> args)
>         {
>             counts[(int)args.CollectionChange] += 1;
>         }
>
>         public string DisplayStats()
>         {
>             StringBuilder report = new StringBuilder("<br/>Number of changes:<ul>");
>             for (int i = 0; i < counts.Length; i++)
>             {
>                 report.Append("<li>" + (CollectionChange)i + ": " + counts[i] + "</li>");
>             }
>             return report.ToString() + "</ul>";
>         }
>     }
> }
> ```
> ```vb
> Imports System.Text
>
> Public NotInheritable Class PropertySetStats
>     Private _ps As PropertySet
>     Public Sub New()
>         _ps = New PropertySet()
>         AddHandler _ps.MapChanged, AddressOf Me.MapChangedHandler
>     End Sub
>
>     Public ReadOnly Property PropertySet As PropertySet
>         Get
>             Return _ps
>         End Get
>     End Property
>
>     Dim counts() As Integer = {0, 0, 0, 0}
>     Private Sub MapChangedHandler(ByVal sender As IObservableMap(Of String, Object),
>         ByVal args As IMapChangedEventArgs(Of String))
>
>         counts(CInt(args.CollectionChange)) += 1
>     End Sub
>
>     Public Function DisplayStats() As String
>         Dim report As New StringBuilder("<br/>Number of changes:<ul>")
>         For i As Integer = 0 To counts.Length - 1
>             report.Append("<li>" & CType(i, CollectionChange).ToString() &
>                           ": " & counts(i) & "</li>")
>         Next
>         Return report.ToString() & "</ul>"
>     End Function
> End Class
> ```

Der Ereignishandler folgt dem vertrauten .NET Framework Ereignis Muster, mit dem Unterschied, dass der Absender des Ereignisses (in diesem Fall das PropertySet-Objekt) in die iobservablemap @ no__t-0string, Object @ no__t-1-Schnittstelle (iobservablemap (of String, Object) in Visual umgewandelt wird. Basic), bei dem es sich um eine Instanziierung der Windows-Runtime-Schnittstelle [iobservablemap @ no__t-3K, V @ no__t-4](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IObservableMap_K_V_)handelt. (Sie können den Absender ggf. in seinen Typ umwandeln.) Außerdem werden die Ereignis Argumente als Schnittstelle und nicht als Objekt dargestellt.

Fügen Sie in der Datei „default.js“ die Runtime1-Funktion wie gezeigt hinzu: Dieser Code erstellt ein PropertySetStats-Objekt, ruft die PropertySet-Auflistung ab und fügt einen eigenen Ereignishandler, die OnMapChanged-Funktion, für die Behandlung des MapChanged-Ereignisses hinzu. Nach dem Ändern der Auflistung ruft „runtime1“ die DisplayStats-Methode auf, um eine Zusammenfassung der Änderungstypen anzuzeigen.

```javascript
var propertysetstats;

function runtime1() {
    document.getElementById('output').innerHTML = "";

    propertysetstats = new SampleComponent.PropertySetStats();
    var propertyset = propertysetstats.propertySet;

    propertyset.addEventListener("mapchanged", onMapChanged);

    propertyset.insert("FirstProperty", "First property value");
    propertyset.insert("SuperfluousProperty", "Unnecessary property value");
    propertyset.insert("AnotherProperty", "A property value");

    propertyset.insert("SuperfluousProperty", "Altered property value")
    propertyset.remove("SuperfluousProperty");

    document.getElementById('output').innerHTML +=
        propertysetstats.displayStats();
}

function onMapChanged(change) {
    var result
    switch (change.collectionChange) {
        case Windows.Foundation.Collections.CollectionChange.reset:
            result = "All properties cleared";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemInserted:
            result = "Inserted " + change.key + ": '" +
                change.target.lookup(change.key) + "'";
            break;
        case Windows.Foundation.Collections.CollectionChange.itemRemoved:
            result = "Removed " + change.key;
            break;
        case Windows.Foundation.Collections.CollectionChange.itemChanged:
            result = "Changed " + change.key + " to '" +
                change.target.lookup(change.key) + "'";
            break;
        default:
            break;
     }

     document.getElementById('output').innerHTML +=
         "<br/>" + result;
}
```

Die Art und Weise der Behandlung von Windows-Runtime-Ereignissen in JavaScript unterscheidet sich von der Art und Weise der Behandlung im .NET Framework-Code. Der JavaScript-Ereignishandler verwendet nur ein Argument. Wenn Sie dieses Objekt im Visual Studio-Debugger anzeigen, ist die erste Eigenschaft der Absender. Die Member der Ereignisargumentschnittstelle werden ebenfalls direkt auf diesem Objekt angezeigt.

Drücken Sie zum Ausführen der App die F5-TASTE. Wenn die Klasse nicht versiegelt ist, erhalten Sie die Fehlermeldung wie „Das Exportieren des nicht versiegelten Typs 'SampleComponent.Example' wird derzeit nicht unterstützt. Markieren Sie ihn als versiegelt.“

Wählen Sie die Schaltfläche **Runtime1** aus. Der Ereignishandler zeigt Änderungen an, wenn Elemente hinzugefügt oder geändert werden, und am Ende wird die DisplayStats-Methode aufgerufen, um eine Zusammenfassung der Anzahl zu erzeugen. Wechseln Sie zu Visual Studio zurück, und drücken Sie Umschalt+F5, um den Debugmodus zu beenden und die App zu schließen.

Um der „PropertySet“-Sammlung zwei weitere Elemente aus dem verwalteten Code hinzuzufügen, fügen Sie den folgenden Code der „PropertySetStats“-Klasse hinzu:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public void AddMore()
> {
>     _ps.Add("NewProperty", "New property value");
>     _ps.Add("AnotherProperty", "A property value");
> }
> ```
> ```vb
> Public Sub AddMore()
>     _ps.Add("NewProperty", "New property value")
>     _ps.Add("AnotherProperty", "A property value")
> End Sub
> ```

In diesem Code wird ein weiterer Unterschied bei der Verwendung von Windows-Runtime Typen in zwei Umgebungen deutlich. Wenn Sie diesen Code selbst eingeben, werden Sie feststellen, dass IntelliSense die Insert-Methode, die Sie im JavaScript-Code verwendet haben, nicht anzeigt. Stattdessen wird die Add-Methode angezeigt, die häufig bei Auflistungen in .NET Framework zu sehen ist. Das liegt daran, dass einige häufig verwendete Sammlungsschnittstellen unterschiedliche Namen aber ähnliche Funktionalität in Windows-Runtime und .NET Framework haben. Wenn Sie diese Schnittstellen in verwaltetem Code verwenden, werden sie als die jeweiligen .NET Framework-Entsprechung angezeigt. Dies wird in [Windows-Runtime Komponenten mit C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)erläutert. Wenn Sie die gleichen Schnittstellen in JavaScript verwenden, besteht der einzige Unterschied zu Windows-Runtime darin, dass Großbuchstaben am Anfang der Membernamen zu Kleinbuchstaben werden.

Fügen Sie zum Schluss zum Aufrufen der AddMore-Methode mit der Ausnahmebehandlung die Runtime2-Funktion zu „default.js“ hinzu.

```javascript
function runtime2() {
   try {
      propertysetstats.addMore();
    }
   catch(ex) {
       document.getElementById('output').innerHTML +=
          "<br/><b>" + ex + "<br/>";
   }

   document.getElementById('output').innerHTML +=
       propertysetstats.displayStats();
}
```

Fügen Sie den Ereignishandlercode für die Registrierung auf die gleiche Weise wie zuvor hinzu.

```javascript
var runtimeButton1 = document.getElementById("runtimeButton1");
runtimeButton1.addEventListener("click", runtime1, false);
var runtimeButton2 = document.getElementById("runtimeButton2");
runtimeButton2.addEventListener("click", runtime2, false);
```

Drücken Sie zum Ausführen der App die F5-TASTE. Wählen Sie **Runtime1** und dann **Runtime2** aus. Der JavaScript-Ereignishandler meldet die erste Änderung an die Sammlung. Die zweite Änderung verfügt jedoch über einen doppelten Schlüssel. Benutzer von .NET Framework-Wörterbüchern erwarten, dass die Add-Methode eine Ausnahme auslöst, was auch geschieht. JavaScript behandelt die .NET Framework-Ausnahme.

> **Hinweis**  Sie können die Ausnahme Meldung nicht aus JavaScript-Code anzeigen. Der Meldungstext wird durch eine Ablaufverfolgung ersetzt. Weitere Informationen finden Sie unter "Auslösen von Ausnahmen" unter Erstellen von Windows-Runtime C# Komponenten in und Visual Basic.

Im Gegensatz dazu wird der Wert des Elements geändert, wenn JavaScript die Insert-Methode mit einem doppelten Schlüssel aufruft. Dieser Unterschied im Verhalten besteht aus den verschiedenen Methoden, mit denen JavaScript und der .NET Framework die Windows-Runtime unterstützen, wie in [Windows-Runtime C# Komponenten mit und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md)erläutert.

## <a name="returning-managed-types-from-your-component"></a>Zurückgeben von verwalteten Typen von der Komponente

Wie zuvor erwähnt, können Sie systemeigene Windows-Runtime-Typen frei zwischen dem JavaScript-Code und dem C#- oder Visual Basic-Code übergeben. In den meisten Fällen sind die Namen und Membernamen in beiden Fällen identisch (die Namen der Member beginnen in JavaScript lediglich mit Kleinbuchstaben). Im vorherigen Abschnitt schien die PropertySet-Klasse allerdings andere Member im verwalteten Code zu haben. (In JavaScript haben Sie z. b. die Insert-Methode aufgerufen, und im .NET Framework Code haben Sie die Add-Methode aufgerufen.) In diesem Abschnitt wird erläutert, wie sich diese Unterschiede auf .NET Framework an JavaScript weiter gegebenen Typen auswirken.

Zusätzlich zum Zurückgeben von Windows-Runtime-Typen, die Sie in der Komponente erstellt oder von JavaScript an die Komponente übergeben haben, können Sie einen verwalteten Typ, der in verwaltetem Code erstellt wurde, an JavaScript so zurückgeben, als wäre er der entsprechende Windows-Runtime-Typ. Auch im ersten, einfachen Beispiel einer Laufzeitklasse waren die Parameter und die Rückgabetypen der Member Visual Basic- oder primitive C#-Typen, die .NET Framework-Typen entsprechen. Fügen Sie den folgenden Code zur „Example“-Klasse hinzu, um dies für Sammlungen zu demonstrieren und eine Methode zu erstellen, die ein generisches Wörterbuch mit Zeichenfolgen zurückgibt, die durch Ganzzahlen indiziert sind:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static IDictionary<int, string> GetMapOfNames()
> {
>     Dictionary<int, string> retval = new Dictionary<int, string>();
>     retval.Add(1, "one");
>     retval.Add(2, "two");
>     retval.Add(3, "three");
>     retval.Add(42, "forty-two");
>     retval.Add(100, "one hundred");
>     return retval;
> }
> ```
> ```vb
> Public Shared Function GetMapOfNames() As IDictionary(Of Integer, String)
>     Dim retval As New Dictionary(Of Integer, String)
>     retval.Add(1, "one")
>     retval.Add(2, "two")
>     retval.Add(3, "three")
>     retval.Add(42, "forty-two")
>     retval.Add(100, "one hundred")
>     Return retval
> End Function
> ```

Beachten Sie, dass das Wörterbuch als Schnittstelle zurückgegeben werden muss, die von [Dictionary&lt;TKey, TValue&gt;](https://docs.microsoft.com/dotnet/api/system.collections.generic.dictionary-2) implementiert wird und die einer Windows-Runtime-Schnittstelle zugeordnet ist. In diesem Fall ist die Schnittstelle „IDictionary&lt;int, string&gt;“ („IDictionary(Of Integer, String)“ in Visual Basic). Wenn der Windows-Runtime-Typ „IMap&lt;int, string&gt;“ an verwalteten Code übergeben wird, wird er als „IDictionary&lt;int, string&gt; angezeigt. Der umgekehrte Fall gilt, wenn der verwaltete Typ an JavaScript übergeben wird.

**Wichtig**  Wenn ein verwalteter Typ mehrere Schnittstellen implementiert, verwendet JavaScript die Schnittstelle, die zuerst in der Liste angezeigt wird. Wenn Sie beispielsweise „Dictionary&lt;int, string&gt;“ an JavaScript-Code zurückgeben, wird „IDictionary&lt;int, string&gt;“ angezeigt, unabhängig davon, welche Schnittstelle Sie als Rückgabetyp angeben. Das bedeutet, dass die erste Schnittstelle Member enthalten muss, die in den nächsten Schnittstellen erscheinen, damit diese Member für JavaScript sichtbar sind.

 

Fügen Sie zum Testen der neuen Methode und Verwenden des Wörterbuchs die Funktionen „returns1“ und „returns2“ zu „default.js“ hinzu:

```javascript
var names;

function returns1() {
    names = SampleComponent.Example.getMapOfNames();
    document.getElementById('output').innerHTML = showMap(names);
}

var ct = 7;

function returns2() {
    if (!names.hasKey(17)) {
        names.insert(43, "forty-three");
        names.insert(17, "seventeen");
    }
    else {
        var err = names.insert("7", ct++);
        names.insert("forty", "forty");
    }
    document.getElementById('output').innerHTML = showMap(names);
}

function showMap(map) {
    var item = map.first();
    var retval = "<ul>";

    for (var i = 0, len = map.size; i < len; i++) {
        retval += "<li>" + item.current.key + ": " + item.current.value + "</li>";
        item.moveNext();
    }
    return retval + "</ul>";
}
```

Fügen Sie den Ereignisregistrierungscode dem gleichen THEN-Block hinzu, dem Sie auch den anderen Ereignisregistrierungscode hinzugefügt haben:

```javascript
var returnsButton1 = document.getElementById("returnsButton1");
returnsButton1.addEventListener("click", returns1, false);
var returnsButton2 = document.getElementById("returnsButton2");
returnsButton2.addEventListener("click", returns2, false);
```

Zu diesem JavaScript-Code gibt es einige interessante Aspekte. Zunächst einmal enthält er eine showMap-Funktion, um den Inhalt des Wörterbuchs in HTML-Code anzuzeigen. Beachten Sie im Code für „showMap“ das Iterationsmuster. In .NET Framework gibt es keine First-Methode für die generische IDictionary-Schnittstelle, und die Größe wird durch eine Count-Eigenschaft anstatt durch eine Size-Methode zurückgegeben. Für JavaScript entspricht „IDictionary&lt;int, string&gt;“ dem Windows-Runtime-Typ „IMap&lt;int, string&gt;“. (Siehe die [IMap&lt;K,V&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.Collections.IMap_K_V_)-Schnittstelle.)

In der returns2-Funktion ruft JavaScript wie in früheren Beispielen die Insert-Methode („Insert“ in JavaScript) auf, um dem Wörterbuch Elemente hinzuzufügen.

Drücken Sie zum Ausführen der App die F5-TASTE. Um den anfänglichen Inhalt des Wörterbuchs zu erstellen und anzuzeigen, wählen Sie die Schaltfläche **Returns1** aus. Um zwei weitere Einträge zum Wörterbuch hinzuzufügen, wählen Sie die Schaltfläche **Returns2** aus. Beachten Sie, dass die Einträge in der Reihenfolge der Einfügung angezeigt werden, wie Sie dies von „Dictionary&lt;TKey, TValue&gt;“ erwarten würden. Wenn sie sortiert werden sollen, können Sie „SortedDictionary&lt;int, string&gt;“ von „GetMapOfNames“ zurückgeben. (Die PropertySet-Klasse, die in früheren Beispielen verwendet wurde, hat eine andere interne Organisation als „Dictionary&lt;TKey, TValue&gt;“.)

JavaScript ist natürlich keine stark typisierte Sprache, sodass das Verwenden stark typisierter generischer Collections zu überraschenden Ergebnissen führen kann. Wählen Sie erneut die Schaltfläche **Returns2** aus. JavaScript wandelt die „7“ pflichtgemäß in eine numerische 7 um, und die numerische 7 wird in ct in einer Zeichenfolge gespeichert. Außerdem wird die Zeichenfolge „forty“ in „null“ umgewandelt. Aber das ist erst der Anfang. Wählen Sie die Schaltfläche **Returns2** noch einige weitere Male aus. Im verwalteten Code würde die Add-Methode Ausnahmen aufgrund doppelter Schlüssel auch dann generieren, wenn die Werte in die richtigen Typen umgewandelt wurden. Im Gegensatz dazu aktualisiert die Insert-Methode den Wert, der einem bestehenden Schlüssel zugeordnet ist, und gibt einen booleschen Wert zurück, der angibt, ob ein neuer Schlüssel zum Wörterbuch hinzugefügt wurde. Deshalb ändert sich der dem Schlüssel 7 zugeordnete Wert ständig.

Ein weiteres unerwartetes Verhalten: Wenn Sie eine nicht zugewiesene JavaScript-Variable als Zeichen folgen Argument übergeben, erhalten Sie die Zeichenfolge "nicht definiert". Kurz gesagt sollten Sie mit Bedacht vorgehen, wenn Sie .NET Framework-Sammlungstypen an Ihren JavaScript-Code übergeben.

> **Beachten**Sie   Wenn Sie große Mengen an Text verketten müssen, können Sie ihn effizienter ausführen, indem Sie den Code in eine .NET Framework Methode verschieben und die StringBuilder-Klasse verwenden, wie in der showmap-Funktion gezeigt.

Obwohl Sie Ihre eigenen generischen Typen aus einer Komponente für Windows-Runtime nicht verfügbar machen können, können Sie generische .NET Framework-Collections für Windows-Runtime-Klassen zurückgeben, indem Sie Code wie den folgenden verwenden:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> public static object GetListOfThis(object obj)
> {
>     Type target = obj.GetType();
>     return Activator.CreateInstance(typeof(List<>).MakeGenericType(target));
> }
> ```
> ```vb
> Public Shared Function GetListOfThis(obj As Object) As Object
>     Dim target As Type = obj.GetType()
>     Return Activator.CreateInstance(GetType(List(Of )).MakeGenericType(target))
> End Function
> ```

„List&lt;T&gt;“ implementiert „IList&lt;T&gt;“, der als Windows-Runtime-Typ „IVector&lt;T&gt;“ in JavaScript angezeigt wird.

## <a name="declaring-events"></a>Deklarieren von Ereignissen


Sie können Ereignisse mit dem standardmäßigen .NET Framework-Ereignismuster oder mit anderen Mustern deklarieren, die von Windows-Runtime verwendet werden. .NET Framework unterstützt die Äquivalenz zwischen dem System.EventHandler&lt;TEventArgs&gt;-Delegaten und dem EventHandler&lt;T&gt;-Delegaten von Windows-Runtime, sodass sich die Verwendung von „EventHandler&lt;TEventArgs&gt;“ gut zum Implementieren des .NET Framework-Standardmusters eignet. Fügen Sie zur Demonstration der Funktionsweise das folgende Klassenpaar dem „SampleComponent“-Projekt hinzu:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> namespace SampleComponent
> {
>     public sealed class Eventful
>     {
>         public event EventHandler<TestEventArgs> Test;
>         public void OnTest(string msg, long number)
>         {
>             EventHandler<TestEventArgs> temp = Test;
>             if (temp != null)
>             {
>                 temp(this, new TestEventArgs()
>                 {
>                     Value1 = msg,
>                     Value2 = number
>                 });
>             }
>         }
>     }
>
>     public sealed class TestEventArgs
>     {
>         public string Value1 { get; set; }
>         public long Value2 { get; set; }
>     }
> }
> ```
> ```vb
> Public NotInheritable Class Eventful
>     Public Event Test As EventHandler(Of TestEventArgs)
>     Public Sub OnTest(ByVal msg As String, ByVal number As Long)
>         RaiseEvent Test(Me, New TestEventArgs() With {
>                             .Value1 = msg,
>                             .Value2 = number
>                             })
>     End Sub
> End Class
>
> Public NotInheritable Class TestEventArgs
>     Public Property Value1 As String
>     Public Property Value2 As Long
> End Class
> ```

Wenn Sie ein Ereignis in Windows-Runtime verfügbar machen, erbt die „EventArgs“-Klasse von „System.Object“. Sie erbt anders als in .NET Framework nicht von System.EventArgs, da EventArgs kein Windows-Runtime-Typ ist.

Wenn Sie benutzerdefinierte Ereignisaccessoren für das Ereignis deklarieren (**Custom**-Schlüsselwort in Visual Basic), müssen Sie das Windows-Runtime-Ereignismuster verwenden. Siehe [benutzerdefinierte Ereignisse und Ereignisaccessoren in Windows-Runtime-Komponenten](custom-events-and-event-accessors-in-windows-runtime-components.md).

Fügen Sie die events1-Funktion zu „default.js“ hinzu, um das Testereignis zu behandeln. Die „events1“Funktion erstellt eine Ereignishandlerfunktion für das Testereignis und ruft sofort die „OnTest-Methode“ zum Auslösen des Ereignisses auf. Wenn Sie einen Haltepunkt im Textbereich des Ereignishandlers platzieren, können Sie sehen, dass das Objekt, das an den einzelnen Parameter übergeben wird, das Quellobjekt und beide Member von „TestEventArgs“ enthält.

```javascript
var ev;

function events1() {
   ev = new SampleComponent.Eventful();
   ev.addEventListener("test", function (e) {
       document.getElementById('output').innerHTML = e.value1;
       document.getElementById('output').innerHTML += "<br/>" + e.value2;
   });
   ev.onTest("Number of feet in a mile:", 5280);
}
```

Fügen Sie den Ereignisregistrierungscode dem gleichen THEN-Block hinzu, dem Sie auch den anderen Ereignisregistrierungscode hinzugefügt haben:

```javascript
var events1Button = document.getElementById("events1Button");
events1Button.addEventListener("click", events1, false);
```

## <a name="exposing-asynchronous-operations"></a>Bereitstellen asynchroner Vorgänge


.NET Framework verfügt über eine Vielzahl von Tools für die asynchrone und parallele Verarbeitung – basierend auf der Aufgabe und auf generischen [Task&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task-1)-Klassen. Um die aufgabenbasierte asynchrone Verarbeitung in einer Komponente für Windows-Runtime verfügbar zu machen, verwenden Sie die Windows-Runtime-Schnittstellen [IAsyncAction](https://docs.microsoft.com/windows/desktop/api/windows.foundation/nn-windows-foundation-iasyncaction), [IAsyncActionWithProgress&lt;TProgress&gt;](https://docs.microsoft.com/previous-versions/br205784(v=vs.85)), [IAsyncOperation&lt;TResult&gt;](https://docs.microsoft.com/previous-versions/br205802(v=vs.85)) und [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://docs.microsoft.com/previous-versions/br205807(v=vs.85)). (In Windows-Runtime geben Vorgänge Ergebnisse zurück, Aktionen aber nicht.)

In diesem Abschnitt wird ein abbrechbarer asynchroner Vorgang veranschaulicht, der den Status meldet und Ergebnisse zurückgibt. Die GetPrimesInRangeAsync-Methode verwendet die [AsyncInfo](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime)-Klasse, um eine Aufgabe zu generieren und die Abbruch- und Statusmeldungsfeatures mit einem WinJS.Promise-Objekt zu verbinden. Fügen Sie der Beispielklasse zunächst die GetPrimesInRangeAsync-Methode hinzu:

> [!div class="tabbedCodeSnippets"]
> ```csharp
> using System.Runtime.InteropServices.WindowsRuntime;
> using Windows.Foundation;
>
> public static IAsyncOperationWithProgress<IList<long>, double>
> GetPrimesInRangeAsync(long start, long count)
> {
>     if (start < 2 || count < 1) throw new ArgumentException();
>
>     return AsyncInfo.Run<IList<long>, double>((token, progress) =>
>
>         Task.Run<IList<long>>(() =>
>         {
>             List<long> primes = new List<long>();
>             double onePercent = count / 100;
>             long ctProgress = 0;
>             double nextProgress = onePercent;
>
>             for (long candidate = start; candidate < start + count; candidate++)
>             {
>                 ctProgress += 1;
>                 if (ctProgress >= nextProgress)
>                 {
>                     progress.Report(ctProgress / onePercent);
>                     nextProgress += onePercent;
>                 }
>                 bool isPrime = true;
>                 for (long i = 2, limit = (long)Math.Sqrt(candidate); i <= limit; i++)
>                 {
>                     if (candidate % i == 0)
>                     {
>                         isPrime = false;
>                         break;
>                     }
>                 }
>                 if (isPrime) primes.Add(candidate);
>
>                 token.ThrowIfCancellationRequested();
>             }
>             progress.Report(100.0);
>             return primes;
>         }, token)
>     );
> }
> ```
> ```vb
> Imports System.Runtime.InteropServices.WindowsRuntime
>
> Public Shared Function GetPrimesInRangeAsync(ByVal start As Long, ByVal count As Long)
> As IAsyncOperationWithProgress(Of IList(Of Long), Double)
>
>     If (start < 2 Or count < 1) Then Throw New ArgumentException()
>
>     Return AsyncInfo.Run(Of IList(Of Long), Double)( _
>         Function(token, prog)
>             Return Task.Run(Of IList(Of Long))( _
>                 Function()
>                     Dim primes As New List(Of Long)
>                     Dim onePercent As Long = count / 100
>                     Dim ctProgress As Long = 0
>                     Dim nextProgress As Long = onePercent
>
>                     For candidate As Long = start To start + count - 1
>                         ctProgress += 1
>
>                         If ctProgress >= nextProgress Then
>                             prog.Report(ctProgress / onePercent)
>                             nextProgress += onePercent
>                         End If
>
>                         Dim isPrime As Boolean = True
>                         For i As Long = 2 To CLng(Math.Sqrt(candidate))
>                             If (candidate Mod i) = 0 Then
>                                 isPrime = False
>                                 Exit For
>                             End If
>                         Next
>
>                         If isPrime Then primes.Add(candidate)
>
>                         token.ThrowIfCancellationRequested()
>                     Next
>                     prog.Report(100.0)
>                     Return primes
>                 End Function, token)
>         End Function)
> End Function
> ```

„GetPrimesInRangeAsync“ ist eine sehr einfache Primzahlensuche – das ist beabsichtigt. Hier liegt der Schwerpunkt auf der Implementierung eines asynchronen Vorgangs. Einfachheit ist also wichtig, und eine langsame Implementierung ist von Vorteil, wenn wir den Abbruch demonstrieren. Getprimesinrangeasync findet PRIMES durch Brute-Force: Ein Kandidat wird durch alle ganzen Zahlen dividiert, die kleiner oder gleich der Quadratwurzel sind, statt nur die Primzahlen zu verwenden. Schrittweises Durchlaufen des Codes:

-   Führen Sie vor dem Start eines asynchronen Vorgangs Aufgaben wie z. B. das Überprüfen von Parametern und das Auslösen von Ausnahmen bei einer ungültigen Eingabe aus.
-   Der zentrale Punkt dieser Implementierung sind die Methode [AsyncInfo.Run&lt;TResult, TProgress&gt;(Func&lt;CancellationToken, IProgress&lt;TProgress&gt;, Task&lt;TResult&gt;](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.windowsruntime)&gt;) und der Delegat, der den einzigen Parameter der Methode darstellt. Der Delegat muss ein Abbruchtoken und eine Schnittstelle für Statusmeldungen akzeptieren und muss eine gestartete Aufgabe zurückgeben, die diese Parameter verwendet. Wenn JavaScript die GetPrimesInRangeAsync-Methode aufruft, treten die folgenden Schritte auf (nicht unbedingt in der hier angegebenen Reihenfolge):

    -   Das [WinJS.Promise](https://docs.microsoft.com/previous-versions/windows/apps/br211867(v=win.10))-Objekt stellt Funktionen zum Verarbeiten der zurückgegebenen Ergebnisse, zum Reagieren auf einen Abbruch und zum Behandeln von Statusberichten bereit.
    -   Die AsyncInfo.Run-Methode erstellt eine Abbruchquelle und ein Objekt, das die IProgress&lt;T&gt;-Schnittstelle implementiert. Sie übergibt ein [CancellationToken](https://docs.microsoft.com/dotnet/api/system.threading.cancellationtoken)-Token aus der Abbruchquelle und die [IProgress&lt;T&gt;](https://docs.microsoft.com/dotnet/api/system.iprogress-1)-Schnittstelle an den Delegaten.

        > **Hinweis**  Wenn das Promise-Objekt keine Funktion bereitstellt, um auf den Abbruch zu reagieren, übergibt asyncinfo. Run trotzdem ein abbrechbares Token, und der Abbruch kann weiterhin auftreten. Wenn das Promise-Objekt keine Funktion zum Behandeln von Statusaktualisierungen bereitstellt, stellt „AsyncInfo.Run“ dennoch ein Objekt bereit, das „ IProgress&lt;T&gt;“ implementiert, aber seine Berichte werden ignoriert.

    -   Der Delegat verwendet die Methode [Task.Run&lt;TResult&gt;(Func&lt;TResult&gt;, CancellationToken](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task.run#System_Threading_Tasks_Task_Run__1_System_Func___0__System_Threading_CancellationToken_)), um eine gestartete Aufgabe zu erstellen, die das Token und die Statusschnittstelle verwendet. Der Delegat für die gestartete Aufgabe wird durch eine Lambdafunktion bereitgestellt, die das gewünschte Ergebnis berechnet. Mehr dazu später.
    -   Die AsyncInfo.Run-Methode erstellt ein Objekt, das die Schnittstelle [IAsyncOperationWithProgress&lt;TResult, TProgress&gt;](https://docs.microsoft.com/uwp/api/Windows.Foundation.IAsyncOperationWithProgress_TResult_TProgress_) implementiert, den Windows-Runtime-Abbruchmechanismus mit der Tokenquelle verbindet und die Statusberichtsfunktion des Promise-Objekts mit der IProgress&lt;T&gt;-Schnittstelle verbindet.
    -   Die IAsyncOperationWithProgress&lt;TResult, TProgress&gt;-Schnittstelle wird an JavaScript zurückgegeben.

-   Die Lambdafunktion, die durch die gestartete Aufgabe dargestellt wird, verwendet keine Argumente. Da es sich um eine Lambdafunktion handelt, ist der Zugriff auf das Token und die IProgress-Schnittstelle möglich. Jedes Mal, wenn eine Kandidatenzahl ausgewertet wird, geschieht Folgendes:

    -   Die Lambdafunktion überprüft, ob der nächste Prozentpunkt des Status erreicht wurde. Ist dies der Fall, ruft die Lambdafunktion die IProgress&lt;T&gt;.Report-Methode auf, und der Prozentsatz wird an die Funktion übergeben, die das Promise-Objekt für Statusberichte angegeben hat.
    -   Verwendet das Abbruchtoken zum Auslösen einer Ausnahme, wenn der Vorgang abgebrochen wurde. Wenn die [IAsyncInfo.Cancel](https://docs.microsoft.com/uwp/api/windows.foundation.iasyncinfo.cancel)-Methode (die von der IAsyncOperationWithProgress&lt;TResult, TProgress&gt;-Schnittstelle geerbt wird) aufgerufen wurde, sorgt die von der AsyncInfo.Run-Methode eingerichtete Verbindung dafür, dass das Abbruchtoken benachrichtigt wird.
-   Wenn die Lambdafunktion die Liste mit den Primzahlen zurückgibt, wird die Liste an die Funktion übergeben, die das WinJS.Promise-Objekt für die Verarbeitung der Ergebnisse angegeben hat.

Zum Erstellen der JavaScript-Zusage und zum Einrichten des Abbruchmechanismus fügen Sie die Funktionen „AsyncRun“ und „AsyncCancel“ zu „default.js“ hinzu.

```javascript
var resultAsync;
function asyncRun() {
    document.getElementById('output').innerHTML = "Retrieving prime numbers.";
    btnAsync.disabled = "disabled";
    btnCancel.disabled = "";

    resultAsync = SampleComponent.Example.getPrimesInRangeAsync(10000000000001, 2500).then(
        function (primes) {
            for (i = 0; i < primes.length; i++)
                document.getElementById('output').innerHTML += " " + primes[i];

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function () {
            document.getElementById('output').innerHTML += " -- getPrimesInRangeAsync was canceled. -- ";

            btnCancel.disabled = "disabled";
            btnAsync.disabled = "";
        },
        function (prog) {
            document.getElementById('primeProg').value = prog;
        }
    );
}

function asyncCancel() {    
    resultAsync.cancel();
}
```

Vergessen Sie nicht den Ereignisregistrierungscode (der gleiche wie zuvor).

```javascript
var btnAsync = document.getElementById("btnAsync");
btnAsync.addEventListener("click", asyncRun, false);
var btnCancel = document.getElementById("btnCancel");
btnCancel.addEventListener("click", asyncCancel, false);
```

Durch den Aufruf der asynchronen GetPrimesInRangeAsync-Methode erstellt die AsyncRun-Funktion ein WinJS.Promise-Objekt. Die then-Methode des Objekts verwendet drei Funktionen, die die zurückgegebenen Ergebnisse verarbeiten, auf Fehler reagieren (einschließlich Abbruch) und Statusberichte behandeln. In diesem Beispiel werden die zurückgegebenen Ergebnisse im Ausgabebereich angezeigt. Bei Abbruch oder Abschluss werden die Schaltflächen, die den Vorgang starten und abbrechen, zurückgesetzt. Die Statusberichte aktualisieren das Statussteuerelement.

Die asyncCancel-Funktion ruft einfach die cancel-Methode des WinJS.Promise-Objekts auf.

Drücken Sie zum Ausführen der App die F5-TASTE. Wählen Sie zum Starten des asynchronen Vorgangs die Schaltfläche **Async** aus. Was als Nächstes passiert, hängt von der Geschwindigkeit des Computers ab. Wenn die Statusleiste den Abschluss erreicht, bevor Sie auch nur blinzeln können, erhöhen Sie die Startzahl, die an „GetPrimesInRangeAsync“ übergeben wird, um den Faktor zehn oder ein Vielfaches davon. Sie können die Dauer des Vorgangs optimieren, indem Sie die Anzahl der Testzahlen erhöhen oder verringern, allerdings hat das Hinzufügen von Nullen (0) in der Mitte der Startzahl einen größeren Einfluss. Um den Vorgang abzubrechen, wählen Sie die Schaltfläche **Cancel Async** aus.

## <a name="related-topics"></a>Verwandte Themen

* [Übersicht über .net für UWP-apps](https://docs.microsoft.com/previous-versions/windows/apps/br230302(v=vs.140))
* [.Net für UWP-apps](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
* [Exemplarische Vorgehensweise zum Erstellen einer Komponente für Windows-Runtime in C# oder Visual Basic und Aufrufen der Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md)
