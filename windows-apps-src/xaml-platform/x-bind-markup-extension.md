---
description: 'Die XBIND-Markup Erweiterung ist eine leistungsstarke Alternative zur Bindung. XBIND-New für Windows 10: wird in kürzerer Zeit und weniger Speicher als bei der Bindung ausgeführt und unterstützt ein besseres Debuggen.'
title: XBIND-Markup Erweiterung
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a25797f50ee76542b8f9543cb76453d2916368ac
ms.sourcegitcommit: 82d202478ab4d3011c5ddd2e852958c34336830d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/22/2019
ms.locfileid: "72715857"
---
# <a name="xbind-markup-extension"></a>{x:Bind}-Markup Erweiterung

**Beachten Sie**  für allgemeine Informationen zur Verwendung der Datenbindung in der APP mit **{x:Bind}** (und für einen gesamten Vergleich zwischen **{x:Bind}** und **{Binding}** ). Weitere Informationen finden Sie unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

Die **{x:Bind}** -Markup Erweiterung – New für Windows 10 – stellt eine Alternative zu **{Binding}** dar. **{x:Bind}** wird in kürzerer Zeit und weniger Arbeitsspeicher als **{Binding}** ausgeführt und unterstützt ein besseres Debuggen.

Zum Zeitpunkt der XAML-Kompilierung wird **{x:Bind}** in Code konvertiert, der einen Wert aus einer Eigenschaft in einer Datenquelle erhält, und für die im Markup angegebene Eigenschaft festgelegt wird. Das Bindungs Objekt kann optional so konfiguriert werden, dass Änderungen am Wert der Datenquellen Eigenschaft beobachtet und auf der Grundlage dieser Änderungen (`Mode="OneWay"`) aktualisiert werden. Sie kann auch optional so konfiguriert werden, dass Änderungen, die in Ihrem eigenen Wert abgelegt werden, an die Quell Eigenschaft (`Mode="TwoWay"`) zurückgelegt werden.

Die von " **{x:Bind}** " und " **{Binding}** " erstellten Bindungs Objekte sind größtenteils funktional äquivalent. **{X:Bind}** führt jedoch speziellen Code aus, der zur Kompilierzeit generiert wird, und **{Binding}** verwendet die allgemeine Überprüfung der Lauf Zeit Objekte. Folglich haben **{x:Bind}** -Bindungen (häufig als kompilierte Bindungen bezeichnet) eine hohe Leistung, stellen die Kompilierzeit Überprüfung ihrer Bindungs Ausdrücke bereit und unterstützen das Debuggen, indem Sie in den Code Dateien, die wird als partielle Klasse für die Seite generiert. Diese Dateien befinden sich in Ihrem `obj` Ordner mit Namen wie (für C#)`<view name>.g.cs`.

> [!TIP]
> **{x:Bind}** hat einen Standardmodus von " **OneTime**" im Gegensatz zu " **{Binding}** ", der den Standardmodus " **OneWay**" hat. Dies wurde aus Leistungsgründen gewählt, da die Verwendung von **OneWay** bewirkt, dass mehr Code generiert und die Änderungs Erkennung verarbeitet wird. Sie können einen Modus für die Verwendung der OneWay-oder TwoWay-Bindung explizit angeben. Sie können auch " [x:defaultbindmode](x-defaultbindmode-attribute.md) " verwenden, um den Standardmodus für " **{x:Bind}** " für ein bestimmtes Segment der Markup Struktur zu ändern. Der angegebene Modus gilt für alle **{x:Bind}** -Ausdrücke auf diesem Element und seinen untergeordneten Elementen, die nicht explizit einen Modus als Teil der Bindung angeben.

**Beispiel-apps, die {x:Bind} veranschaulichen**

-   [{x:Bind}-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [Quiz Spiel](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [Beispiel für XAML-UI-Grundlagen](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>Verwendung von XAML-Attributen

``` syntax
<object property="{x:Bind}" .../>
-or-
<object property="{x:Bind propertyPath}" .../>
-or-
<object property="{x:Bind bindingProperties}" .../>
-or-
<object property="{x:Bind propertyPath, bindingProperties}" .../>
-or-
<object property="{x:Bind pathToFunction.functionName(functionParameter1, functionParameter2, ...), bindingProperties}" .../>
```

| Laufzeit | Beschreibung |
|------|-------------|
| _PropertyPath_ | Eine Zeichenfolge, die den Eigenschafts Pfad für die Bindung angibt. Weitere Informationen finden Sie weiter unten im Abschnitt [Eigenschafts Pfad](#property-path) . |
| _bindingproperties_ |
| _propName_=_Wert_\[, _propName_=_Wert_\]* | Eine oder mehrere Bindungseigenschaften, die mithilfe einer Name-Wert-Paar-Syntax angegeben werden. |
| _propName_ | Der Zeichen folgen Name der Eigenschaft, die für das Bindungs Objekt festgelegt werden soll. Beispiel: "Konverter". |
| _value_ | Der Wert, auf den die Eigenschaft festgelegt werden soll. Die Syntax des Arguments hängt davon ab, welche Eigenschaft festgelegt wird. Im folgenden finden Sie ein Beispiel für die Verwendung eines _propName_ -=_Werts_ , bei dem der Wert selbst eine Markup Erweiterung ist: `Converter={StaticResource myConverterClass}`. Weitere Informationen finden Sie unter [Eigenschaften, die Sie unten im Abschnitt {x:Bind} festlegen können](#properties-that-you-can-set-with-xbind) . |

## <a name="examples"></a>Beispiele

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

In diesem XAML-Beispiel wird **{x:Bind}** mit der **ListView. ItemTemplate** -Eigenschaft verwendet. Notieren Sie sich die Deklaration eines **x:datatype** -Werts.

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Eigenschafts Pfad

*PropertyPath* legt den **Pfad** für einen **{x:Bind}** -Ausdruck fest. **Path** ist ein Eigenschafts Pfad, der den Wert der Eigenschaft, untergeordneten Eigenschaft, des Felds oder der Methode angibt, an die Sie binden (die Quelle). Sie können den Namen der **Pfad** Eigenschaft explizit erwähnen: `{x:Bind Path=...}`. Oder Sie können es weglassen: `{x:Bind ...}`.

### <a name="property-path-resolution"></a>Eigenschafts Pfad Auflösung

**{x:Bind}** verwendet nicht den **DataContext** als Standard Quelle – sondern verwendet die Seite oder das Benutzer Steuerelement selbst. Daher wird im Code Behind der Seite oder des Benutzer Steuer Elements nach Eigenschaften, Feldern und Methoden gesucht. Um das Ansichts Modell für **{x:Bind}** verfügbar zu machen, werden Sie in der Regel dem Code Behind für die Seite oder das Benutzer Steuerelement neue Felder oder Eigenschaften hinzufügen. Die Schritte in einem Eigenschafts Pfad werden durch Punkte (.) getrennt, und Sie können mehrere Trennzeichen einschließen, um nachfolgende untergeordnete Eigenschaften zu durchlaufen. Verwenden Sie das Punkt Trennzeichen, unabhängig von der Programmiersprache, mit der das an gebundene Objekt implementiert wird.

Beispiel: in einer Seite sucht **Text = "{x:Bind Employee. FirstName}"** nach einem **Employee** -Member auf der Seite und dann nach einem **FirstName** -Element für das Objekt, das von **Employee**zurückgegeben wird. Wenn Sie ein Items-Steuerelement an eine Eigenschaft binden, die abhängige Mitarbeiter eines Mitarbeiters enthält, kann der Eigenschafts Pfad "Employee. Dependents" lauten, und die Element Vorlage des Items-Steuer Elements würde die Anzeige der Elemente in "Dependents" übernehmen.

" C++ **{X:Bind}** " kann für/CX nicht an private Felder und Eigenschaften in der Seite oder im Datenmodell gebunden werden – Sie benötigen eine öffentliche Eigenschaft, damit Sie gebunden werden kann. Die Oberfläche für die Bindung muss als CX-Klassen/-Schnittstellen verfügbar gemacht werden, damit wir die relevanten Metadaten erhalten können. Das **\[bindbare\]** Attribut sollte nicht benötigt werden.

Bei **x:Bind**muss **ElementName = xxx** nicht als Teil des Bindungs Ausdrucks verwendet werden. Stattdessen können Sie den Namen des Elements als ersten Teil des Pfads für die Bindung verwenden, da benannte Elemente Felder innerhalb der Seite oder des Benutzer Steuer Elements werden, die die Stamm Bindungs Quelle darstellen. 


### <a name="collections"></a>Sammlungen

Wenn es sich bei der Datenquelle um eine Auflistung handelt, kann ein Eigenschafts Pfad Elemente in der Auflistung nach Position oder Index angeben. Beispiel: "Teams\[0\]. "Players", wobei das Literale "\[\]" das "0" einschließt, das das erste Element in einer NULL indizierten Auflistung anfordert.

Um einen Indexer zu verwenden, muss das Modell **IList&lt;t&gt;** oder **IVector&lt;t&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. (Beachten Sie, dass die Indexer-Syntax von Iread onlylist&lt;t&gt; und ivectorview&lt;t&gt; nicht unterstützt wird.) Wenn der Typ der indizierten Eigenschaft " **INotifyCollectionChanged** " oder " **iobservablevector** " unterstützt und die Bindung OneWay oder TwoWay ist, registriert Sie diese Schnittstellen und lauscht auf Änderungs Benachrichtigungen. Die Änderungs Erkennungs Logik wird basierend auf allen Sammlungs Änderungen aktualisiert, auch wenn sich dies nicht auf den spezifischen indizierten Wert auswirkt. Dies liegt daran, dass die Abhör Logik in allen Instanzen der Auflistung üblich ist.

Wenn es sich bei der Datenquelle um ein Wörterbuch oder eine Zuordnung handelt, kann ein Eigenschafts Pfad Elemente in der Auflistung nach dem Zeichen folgen Namen angeben. Beispielsweise **&lt;TextBlock Text = "{x:Bind Players\[' John Smith '\]"/&gt;** nach einem Element im Wörterbuch mit dem Namen "John Smith". Der Name muss in Anführungszeichen eingeschlossen werden, und es können entweder einfache oder doppelte Anführungszeichen verwendet werden. Hat (^) kann zum Escapezeichen in Zeichen folgen verwendet werden. Es ist normalerweise am einfachsten, Alternative Anführungszeichen zu verwenden, die für das XAML-Attribut verwendet werden. (Beachten Sie, dass die Indexer-Syntax von Iread onlydictionary&lt;t&gt; und imapview&lt;t&gt; nicht unterstützt wird.)

Um einen Zeichenfolgenindexer zu verwenden, muss das Modell **IDictionary&lt;String, t&gt;** oder **IMap&lt;String, t&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. Wenn der Typ der indizierten Eigenschaft **iobservablemap** unterstützt und die Bindung OneWay oder TwoWay ist, registriert Sie die Änderungs Benachrichtigungen für diese Schnittstellen und lauscht darauf. Die Änderungs Erkennungs Logik wird basierend auf allen Sammlungs Änderungen aktualisiert, auch wenn sich dies nicht auf den spezifischen indizierten Wert auswirkt. Dies liegt daran, dass die Abhör Logik in allen Instanzen der Auflistung üblich ist.

### <a name="attached-properties"></a>Angefügte Eigenschaften

Zum Binden an [angefügte Eigenschaften](./attached-properties-overview.md)müssen Sie die Klassen-und Eigenschaftsnamen in Klammern nach dem Punkt einfügen. Beispiel **: Text = "{x:Bind Button22. ( Grid. Row)} "** . Wenn die Eigenschaft nicht in einem XAML-Namespace deklariert ist, müssen Sie einen XML-Namespace als Präfix zuweisen, den Sie einem Code Namespace am Anfang des Dokuments zuordnen sollten.

### <a name="casting"></a>Zauber

Kompilierte Bindungen sind stark typisiert und lösen den Typ jedes einzelnen Schritts in einem Pfad auf. Wenn der zurückgegebene Typ nicht über den Member verfügt, tritt zum Zeitpunkt der Kompilierung ein Fehler auf. Sie können eine Umwandlung angeben, um den tatsächlichen Typ des Objekts zu binden. Im folgenden Fall ist **obj** eine Eigenschaft vom Typ Object, enthält jedoch ein Textfeld, sodass wir entweder **Text = "{x:Bind ((TextBox) obj)" verwenden können. Text} "** oder **Text =" {x:Bind obj. (TextBox. Text)} "** .
Das Feld **groups3** in **Text = "{x:Bind ((Data: sampledatagroup) groups3\[0\]). Title} "** ist ein Wörterbuch von Objekten, sodass Sie es in" **Daten: sampledatagroup**"umwandeln müssen. Beachten Sie die Verwendung der XML- **Daten:** Namespace-Präfix für die Zuordnung des Objekt Typs zu einem Code Namespace, der nicht Teil des standardmäßigen XAML-Namespace ist.

_Hinweis: die C#Umwandlungs Syntax im-Format ist flexibler als die Syntax der angefügten Eigenschaft, und es wird die empfohlene Syntax in Zukunft angezeigt._

## <a name="functions-in-binding-paths"></a>Funktionen in Bindungs Pfaden

Ab Windows 10 unterstützt Version 1607, **{x:Bind}** die Verwendung einer Funktion als Blatt Schritt des Bindungs Pfads. Dies ist eine leistungsstarke Funktion für die Datenbindung, die mehrere Szenarien im Markup ermöglicht. Weitere Informationen finden Sie unter [Funktions Bindungen](../data-binding/function-bindings.md) .

## <a name="event-binding"></a>Ereignis Bindung

Die Ereignis Bindung ist eine eindeutige Funktion für die kompilierte Bindung. Sie ermöglicht es Ihnen, den Handler für ein Ereignis mit einer Bindung anzugeben, anstatt eine Methode im Code Behind zu verwenden. Beispiel: **Click = "{x:Bind rootframe. GoForward}"** .

Bei Ereignissen darf die Ziel Methode nicht überladen werden und muss außerdem folgende Aktionen ausführen:

- Entspricht der Signatur des Ereignisses.
- Oder verfügen über keine Parameter.
- Oder haben die gleiche Anzahl von Parametern von Typen, die von den Typen der Ereignis Parameter zugewiesen werden können.

In generiertem Code-Behind verarbeitet die kompilierte Bindung das-Ereignis und leitet es an die-Methode im Modell weiter. dabei wird der Pfad des Bindungs Ausdrucks ausgewertet, wenn das Ereignis auftritt. Dies bedeutet, dass im Gegensatz zu Eigenschafts Bindungen Änderungen am Modell nicht nachverfolgt werden.

Weitere Informationen zur Zeichen folgen Syntax für einen Eigenschafts Pfad finden Sie unter [Syntax für Eigenschafts Pfade](property-path-syntax.md). Beachten Sie dabei die hier beschriebenen Unterschiede für **{x:Bind}** .

## <a name="properties-that-you-can-set-with-xbind"></a>Eigenschaften, die Sie mit {x:Bind} festlegen können

**{x:Bind}** wird mit der Platzhalter Syntax *bindingproperties* veranschaulicht, da mehrere Lese-/Schreibeigenschaften in der Markup Erweiterung festgelegt werden können. Die Eigenschaften können in beliebiger Reihenfolge mit durch Trennzeichen getrennten *propName* -=*Wert* -Paaren festgelegt werden. Beachten Sie, dass Sie keine Zeilenumbrüche in den Bindungs Ausdruck einschließen können. Einige der Eigenschaften erfordern Typen, die keine Typkonvertierung aufweisen, sodass diese Markup Erweiterungen ihrer eigenen geschachtelten innerhalb von **{x:Bind}** erfordern.

Diese Eigenschaften funktionieren ähnlich wie die Eigenschaften der [**Bindungs**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) Klasse.

| Eigenschaft | Beschreibung |
|----------|-------------|
| **Path** | Weitere Informationen finden Sie oben im Abschnitt [Eigenschafts Pfad](#property-path) . |
| **Erer** | Gibt das Konverterobjekt an, das von der Bindungs-Engine aufgerufen wird. Der Konverter kann in XAML festgelegt werden, jedoch nur, wenn Sie auf eine Objektinstanz verweisen, die Sie in einem [{statikresource}-Markup Erweiterungs](staticresource-markup-extension.md) Verweis auf dieses Objekt im Ressourcen Wörterbuch zugewiesen haben. |
| **Converterlanguage** | Gibt die Kultur an, die vom Konverter verwendet werden soll. (Wenn Sie **converterlanguage** festlegen, sollten Sie auch **Converter**festlegen.) Die Kultur wird als Standard basierter Bezeichner festgelegt. Weitere Informationen finden Sie unter [**converterlanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Gibt den Konverterparameter an, der in der Konverterlogik verwendet werden kann. (Wenn Sie **ConverterParameter** festlegen, sollten Sie auch **Converter**festlegen.) Bei den meisten Konvertern wird eine einfache Logik verwendet, die alle benötigten Informationen aus dem übergebenen Wert zum Konvertieren erhält und keinen **ConverterParameter** -Wert benötigt. Der **ConverterParameter** -Parameter ist für moderate erweiterte konverterimplementierungen vorgesehen, die über mehr als eine Logik verfügen, die einen Schlüssel aus den in **ConverterParameter**übergebenen Schlüsseln entfernt. Sie können einen Konverter schreiben, der andere Werte als Zeichen folgen verwendet. Dies ist jedoch nicht üblich. Weitere Informationen finden Sie unter Hinweise in [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter) . |
| **FallbackValue** | Gibt einen Wert an, der angezeigt wird, wenn die Quelle oder der Pfad nicht aufgelöst werden kann. |
| **Mode** | Gibt den Bindungs Modus als eine der folgenden Zeichen folgen an: "OneTime", "OneWay" oder "TwoWay". Der Standardwert ist "OneTime". Beachten Sie, dass sich dies von der Standardeinstellung für " **{Binding}** " unterscheidet, die in den meisten Fällen "OneWay" lautet. |
| **TargetNullValue** | Gibt einen Wert an, der angezeigt wird, wenn der Quellwert aufgelöst wird, aber explizit **null**ist. |
| **Bindback** | Gibt eine Funktion an, die für die umgekehrte Richtung einer bidirektionalen Bindung verwendet werden soll. |
| **UpdateSourceTrigger** | Gibt an, wann Änderungen in TwoWay-Bindungen vom-Steuerelement auf das Modell zurückgelegt werden sollen. Der Standardwert für alle Eigenschaften außer TextBox. Text ist PropertyChanged; TextBox. Text ist LostFocus.|

> [!NOTE]
> Wenn Sie Markup von **{Binding}** in **{x:Bind}** umrechnen, beachten Sie die Unterschiede in den Standardwerten für die **Mode** -Eigenschaft.
> [**x:defaultbindmode**](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) kann verwendet werden, um den Standardmodus für x:Bind für ein bestimmtes Segment der Markup Struktur zu ändern. Der ausgewählte Modus wendet alle x:Bind-Ausdrücke für dieses Element und seine untergeordneten Elemente an, die nicht explizit einen Modus als Teil der Bindung angeben. OneTime ist leistungsfähiger als OneWay, da die Verwendung von OneWay dazu führt, dass mehr Code generiert und die Änderungs Erkennung verarbeitet wird.

## <a name="remarks"></a>Anmerkungen

Da **{x:Bind}** generierten Code verwendet, um seine Vorteile zu erzielen, sind zum Zeitpunkt der Kompilierung Typinformationen erforderlich. Dies bedeutet, dass Sie keine Bindung an Eigenschaften haben, bei denen der Typ nicht im Voraus bekannt ist. Aus diesem Grund können Sie " **{x:Bind}** " nicht mit der **DataContext** -Eigenschaft verwenden, die vom Typ " **Object**" ist und auch zur Laufzeit geändert werden kann.

Wenn Sie " **{x:Bind}** " mit Datenvorlagen verwenden, müssen Sie den Typ angeben, an den gebunden wird, indem Sie einen **x:datatype** -Wert festlegen, wie im Abschnitt " [Beispiele](#examples) " gezeigt. Sie können auch den Typ auf eine Schnittstelle oder einen Basis Klassentyp festlegen und dann bei Bedarf Umwandlungen verwenden, um einen vollständigen Ausdruck zu formulieren.

Kompilierte Bindungen sind abhängig von der Codegenerierung. Wenn Sie also **{x:Bind}** in einem Ressourcen Wörterbuch verwenden, muss das Ressourcen Wörterbuch über eine Code-Behind-Klasse verfügen. Ein Codebeispiel finden Sie [unter Ressourcen Wörterbücher mit {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind) .

Seiten und Benutzer Steuerelemente, die kompilierte Bindungen einschließen, verfügen im generierten Code über eine Eigenschaft "Bindungen". Dies schließt die folgenden Methoden ein:

- **Update ()** : die Werte aller kompilierten Bindungen werden aktualisiert. Für alle unidirektionalen/bidirektionalen Bindungen werden die Listener verknüpft, um Änderungen zu erkennen.
- **Initialisieren ()** : Wenn die Bindungen nicht bereits initialisiert wurden, wird Update () aufgerufen, um die Bindungen zu initialisieren.
- **Stoptracking ()** : Hiermit werden alle Listener, die für unidirektionale und bidirektionale Bindungen erstellt wurden, entfernt. Sie können mit der Update ()-Methode erneut initialisiert werden.

> [!NOTE]
> Ab Windows 10, Version 1607, bietet das XAML-Framework einen integrierten booleschen Wert für den Sichtbarkeits Konverter. Der Konverter ordnet dem **sichtbaren** Enumerationswert **true** zu, und **false** wird reduziert **, sodass Sie** eine Sichtbarkeits Eigenschaft an einen booleschen Wert binden können, ohne einen Konverter zu erstellen. Beachten Sie, dass es sich hierbei nicht um eine Funktion der Funktions Bindung, sondern nur um die Eigenschaften Bindung handelt Um den integrierten Konverter zu verwenden, muss die minimale SDK-Zielversion der APP 14393 oder höher sein. Sie können Sie nicht verwenden, wenn Ihre APP auf frühere Versionen von Windows 10 abzielt. Weitere Informationen zu Ziel Versionen finden Sie unter [Version Adaptive Code](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Tipp**   Wenn Sie eine einzelne geschweifte Klammer für einen Wert angeben müssen, z. b. in " [**path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) " oder " [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter)", stellen Sie ihm einen umgekehrten Schrägstrich voran: `\{`. Alternativ dazu können Sie auch die gesamte Zeichenfolge einschließen, die die geschweiften Klammern enthält, die in einem sekundären Anführungszeichen, z. b. `ConverterParameter='{Mix}'`.

[**Konvertersprache**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) und **konvertersprache** beziehen sich alle auf das [**Szenario, in**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)dem ein Wert oder ein Typ von der Bindungs Quelle in einen Typ oder Wert konvertiert wird, der mit der Bindungs Ziel Eigenschaft kompatibel ist. Weitere Informationen und Beispiele finden Sie im Abschnitt "Datenkonvertierungen" unter [Datenbindung ausführlich](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

**{x:Bind}** ist nur eine Markup Erweiterung, ohne dass solche Bindungen Programm gesteuert erstellt oder bearbeitet werden können. Weitere Informationen zu Markup Erweiterungen finden Sie unter [Übersicht über XAML](xaml-overview.md).

