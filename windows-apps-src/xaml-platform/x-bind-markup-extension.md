---
description: Die Markuperweiterung xBind ist eine leistungsstarke Alternative Bindung an. xBind - ausgeführt wird, neu für Windows 10 - schneller und weniger Arbeitsspeicher als Bindung und eine bessere Debuggen unterstützt.
title: xBind-Markuperweiterung
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 4a6f182ab5f34f7bbb99e54626001126b3741522
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320275"
---
# <a name="xbind-markup-extension"></a>{x:Bind}-Markuperweiterung

**Beachten Sie**  allgemeine Informationen zum Verwenden der Datenbindung in Ihrer app mit **{X: Bind}** (und eine allumfassende Vergleich zwischen **{X: Bind}** und **{Binding}** ), finden Sie unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

Die **{X: Bind}** Markuperweiterung – neues für Windows 10 – ist eine Alternative zum **{Binding}** . **{X: Bind}**  Ausführung schneller und weniger Arbeitsspeicher als **{Binding}** und unterstützt eine bessere Debuggingunterstützung.

Zur XAML-Kompilierungszeit wird **{X: Bind}** in Code konvertiert, der den Wert aus einer Eigenschaft aus einer Datenquelle abruft und diesen in der im Markup angegeben Eigenschaft festlegt. Das Binding-Objekt kann optional konfiguriert werden, um Änderungen am Wert der Datenquelleneigenschaft zu beobachten und sich basierend auf diesen Änderungen (`Mode="OneWay"`) zu aktualisieren. Es kann optional auch konfiguriert werden, um Änderungen am eigenen Wert per Push zurück an die Quelleigenschaft (`Mode="TwoWay"`) zu senden.

Die von **{x:Bind}** und **{Binding}** erstellten Bindungsobjekte sind von der Funktionsweise her größtenteils identisch. **{x:Bind}** führt allerdings speziellen Code aus, der zur Kompilierzeit generiert wird, und **{Binding}** verwendet eine allgemeine Laufzeitobjektüberprüfung. Folglich bieten **{x:Bind}** -Bindungen (häufig als kompilierte Bindungen bezeichnet) eine hervorragende Leistung, stellen die Validierung Ihrer Bindungsausdrücke bei der Kompilierung bereit und unterstützen das Debuggen, indem Sie die Möglichkeit erhalten, Haltepunkte in den Codedateien festzulegen, die als Teilklasse für die Seite generiert werden. Diese Dateien befinden sich in Ihrem `obj`-Ordner und weisen Namen wie `<view name>.g.cs` auf (für C#).

> [!TIP]
> **{x:Bind}** verfügt über einen Standardmodus von **OneTime**, im Gegensatz zu **{Binding}** , wo der Standardmodus **OneWay** ist. Diese wurde aus Leistungsgründen so gewählt, da **OneWay** bewirkt, dass mehr Code generiert wird, um die Änderungserkennung zu verknüpfen und zu behandeln. Sie können für einen Modus explizit angeben, dass OneWay- oder TwoWay-Bindung verwendet wird. [x:DefaultBindMode](x-defaultbindmode-attribute.md) kann auch verwendet werden, um für ein bestimmtes Segment der Markupstruktur den Standardmodus für **{x:Bind}** zu ändern. Der angegebene Modus gilt für alle **{x:Bind}** -Ausdrücke in diesem Element und seinen untergeordneten Elementen, die nicht explizit einen Bindungsmodus festlegen.

**Beispiel-apps, die veranschaulichen, {X: Bind}**

-   [{X: Bind}-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [Grundlagen zu XAML-UI-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=619992)

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

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

| Begriff | Beschreibung |
|------|-------------|
| _propertyPath_ | Eine Zeichenfolge, die den Eigenschaftspfad für die Bindung angibt. Weitere Informationen finden Sie unten im Abschnitt [Eigenschaftspfad](#property-path). |
| _bindingProperties_ |
| _propName_=_value_\[, _propName_=_value_\]* | Mindestens eine Bindungseigenschaft, die mithilfe einer Name-Wert-Paarsyntax angegeben wird. |
| _propName_ | Der Zeichenfolgenname der für das Binding-Objekt festzulegenden Eigenschaft. Beispiel: „Konverter“. |
| _value_ | Der für die Eigenschaft festzulegende Wert. Die Syntax des Arguments hängt von der festgelegten Eigenschaft ab. Hier ein Beispiel für eine _propName_=_value_-Syntax, bei der der Wert selbst eine Markuperweiterung ist: `Converter={StaticResource myConverterClass}`. Weitere Informationen finden Sie unten im Abschnitt [Mit {x:Bind} festzulegende Eigenschaften](#properties-that-you-can-set-with-xbind). |

## <a name="examples"></a>Beispiele

```XAML
<Page x:Class="QuizGame.View.HostView" ... >
    <Button Content="{x:Bind Path=ViewModel.NextButtonText, Mode=OneWay}" ... />
</Page>
```

Dieser beispielhafte XAML-Code verwendet **{x:Bind}** mit einer **ListView.ItemTemplate**-Eigenschaft. Beachten Sie die Deklaration eines **x:DataType**-Werts.

```XAML
  <DataTemplate x:Key="SimpleItemTemplate" x:DataType="data:SampleDataGroup">
    <StackPanel Orientation="Vertical" Height="50">
      <TextBlock Text="{x:Bind Title}"/>
      <TextBlock Text="{x:Bind Description}"/>
    </StackPanel>
  </DataTemplate>
```

## <a name="property-path"></a>Eigenschaftspfad

*PropertyPath* legt **Path** für einen **{x:Bind}** -Ausdruck fest. **Path** ist ein Eigenschaftspfad, der den Wert der Eigenschaft, der Untereigenschaft, des Felds oder der Methode angibt, an die bzw. das Sie binden (die Quelle). Sie können den Namen der **Path**-Eigenschaft explizit erwähnen: `{x:Bind Path=...}`. Sie können diesen aber auch weglassen: `{x:Bind ...}`.

### <a name="property-path-resolution"></a>Auflösung des Eigenschaftspfads

**{x:Bind}** verwendet nicht den **DataContext** als Standardquelle. Stattdessen wird die Seite oder das Benutzersteuerelement selbst verwendet. So sieht der CodeBehind-Abschnitt der Seite oder des Benutzersteuerelements für Eigenschaften, Felder und Methoden aus. Um das Ansichtsmodell für **{x:Bind}** verfügbar zu machen, sollten Sie in der Regel neue Felder oder Eigenschaften zum CodeBehind-Abschnitt der Seite oder des Benutzersteuerelements hinzufügen. Die Schritte in einem Eigenschaftspfad werden durch Punkte getrennt (.), und Sie können mehrere Trennzeichen angeben, um aufeinander folgende untergeordnete Eigenschaften zu durchlaufen. Verwenden Sie unabhängig von der verwendeten Programmiersprache einen Punkt als Trennzeichen, um das Objekt zu implementieren, an das die Bindung erfolgt.

So sucht **Text="{x:Bind Employee.FirstName}"** z. B. auf einer Seite nach einem **Employee**-Mitglied auf der Seite und dann nach einem **FirstName**-Mitglied für das von **Employee** zurückgegebene Objekt. Wenn Sie ein ItemsControl-Element an eine Eigenschaft binden, die die abhängigen Elemente des Mitarbeiters enthält, kann der Eigenschaftspfad „Employee.Dependents“ lauten, und die Elementvorlage des ItemsControl-Elements wäre für die Anzeige der Elemente „Dependents“ verantwortlich.

Für C++/CX kann **{x:Bind}** keine Bindungen an private Felder und Eigenschaften im Seiten- oder Datenmodell durchführen – Sie benötigen eine öffentliche Eigenschaft, damit die Bindung möglich ist. Der Oberflächenbereich für die Bindung muss als CX-Klassen/-Schnittstellen verfügbar gemacht werden, damit die relevanten Metadaten abgerufen werden können. Die **\[Bindable\]** Attribut sollte nicht erforderlich.

Mit **x:Bind** müssen Sie **ElementName=xxx** nicht als Teil des Bindungsausdrucks verwenden. Stattdessen können der Name des Elements als der erste Teil des Pfads für die Bindung Sie da benannte Elemente Felder in die Seite oder das Benutzersteuerelement werden, die die Stamm-Bindungsquelle darstellt. 


### <a name="collections"></a>Auflistungen

Wenn die Datenquelle eine Auflistung ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihrer Position oder ihres Indexes angeben. Z. B. "Teams\[0\]. Dessen Mitglieder", in dem das Literal"\[\]"schließt"0", der das erste Element in einer Sammlung mit Null indizierte anfordert.

Um einen Indexer verwenden zu können, muss das Modell **IList&lt;T&gt;** oder **IVector&lt;T&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. (Beachten Sie, IReadOnlyList&lt;T&gt; und IVectorView&lt;T&gt; die Indexersyntax nicht unterstützt.) Wenn der Typ der indizierten Eigenschaft **INotifyCollectionChanged** oder **IObservableVector** unterstützt und die Bindung OneWay oder TwoWay ist, registriert und lauscht er auf Änderungsbenachrichtigungen an diesen Schnittstellen. Die Änderungserkennungslogik wird basierend auf allen Auflistungsänderungen aktualisiert, auch wenn sie keine Auswirkungen auf den entsprechenden indizierten Wert hat. Dies geschieht, da die Überwachungslogik für alle Instanzen der Auflistung identisch ist.

Wenn die Datenquelle ein Wörterbuch oder eine Karte ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihres Zeichenfolgennamens angeben. Z. B. **&lt;TextBlock Text = "{X: Bind Spieler\["John Smith"\]" /&gt;** sucht ein Element im Wörterbuch mit dem Namen "John Smith". Der Name muss in Anführungszeichen gesetzt werden. Dabei können einfache oder doppelte Anführungszeichen verwendet werden. Das Caret-Symbol (^) kann als Escapezeichen für Anführungszeichen innerhalb von Zeichenfolgen verwendet werden. Es ist normalerweise am einfachsten, Anführungszeichen zu verwenden, die von denen für das XAML-Attribut verwendeten abweichen. (Beachten Sie, IReadOnlyDictionary&lt;T&gt; und IMapView&lt;T&gt; die Indexersyntax nicht unterstützt.)

Um einen Zeichenfolgen-Indexer verwenden zu können, muss das Modell **IDictionary&lt;string, T&gt;** oder **IMap&lt;string T&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. Wenn der Typ der indizierten Eigenschaft **IObservableMap** unterstützt und die Bindung OneWay oder TwoWay ist, wird er registriert und überwacht Benachrichtigungen auf diesen Schnittstellen. Die Änderungserkennungslogik wird basierend auf allen Auflistungsänderungen aktualisiert, auch wenn sie keine Auswirkungen auf den entsprechenden indizierten Wert hat. Dies geschieht, da die Überwachungslogik für alle Instanzen der Auflistung identisch ist.

### <a name="attached-properties"></a>Angefügte Eigenschaften

Um an angefügte Eigenschaften zu binden, müssen Sie den Klassen- und den Eigenschaftsnamen nach dem Punkt in Klammern setzen. Beispiel: **Text="{x:Bind Button22.(Grid.Row)}"** . Wenn die Eigenschaft nicht in einem Xaml-Namespace deklariert wird, müssen Sie ihr einen XML-Namespace als Präfix voranstellen, den Sie einem Codenamespace am Anfang des Dokuments zuordnen sollten.

### <a name="casting"></a>Umwandlung

Kompilierte Bindungen sind stark typisiert und lösen den Typ der einzelnen Schritte in einem Pfad auf. Wenn der zurückgegebene Typ nicht über den Member verfügt, erzeugt er zur Kompilierzeit einen Fehler. Sie können eine Umwandlung angeben, um der Bindung den tatsächlichen Typ des Objekts mitzuteilen. Im folgenden Fall ist **obj** eine Eigenschaft eines Typobjekts, enthält aber ein Textfeld, sodass wir **Text="{x:Bind ((TextBox)obj).Text}"** oder **Text="{x:Bind obj.(TextBox.Text)}"** verwenden können.
Die **groups3** Feld **Text = "{X: Bind ((data:SampleDataGroup) groups3\[0\]). Title} "** ist ein Wörterbuch von Objekten, damit Sie sie zum umwandeln müssen **Daten: SampleDataGroup**. Beachten Sie die Verwendung des **data:** -XML-Namespacepräfix für die Zuordnung des Objekttyps zu einem Code-Namespace, der nicht Teil des Standard-XAML-Namespace ist.

_Hinweis: Die C#-Syntax lautet: Umwandlung ist flexibler als die Syntax der angefügten Eigenschaft, und ist die empfohlene Syntax weiterleiten._

## <a name="functions-in-binding-paths"></a>Funktionen in Bindungspfaden

Ab Windows 10, Version 1607, unterstützt **{x: Bind}** die Verwendung einer Funktion als blattbildenden Schritt des Bindungspfades. Dies ist ein leistungsstarkes Feature für die Datenbindung, die verschiedene Szenarien im Markup unterstützt. Finden Sie unter [Funktion Bindungen](../data-binding/function-bindings.md) Details.

## <a name="event-binding"></a>Ereignisbindung

Ereignisbindung ist ein besonderes Feature der kompilierte Bindung. Damit können Sie den Handler für ein Ereignis mit einer Bindung angeben. Es muss also keine Methode im CodeBehind-Abschnitt sein. Zum Beispiel: **Click="{x:Bind rootFrame.GoForward}"** .

Bei Ereignissen muss die Zielmethode nicht überladen werden und muss Folgendes erfüllen:

- Sie muss mit der Signatur des Ereignisses übereinstimmen
- ODER keine Parameter enthalten
- ODER die gleiche Anzahl von Parametern von Typen enthalten, die alle von den Typen der Ereignisparameter zugeordnet werden können.

Im generierten CodeBehind-Abschnitt verarbeitet die kompilierte Bindung das Ereignis und leitet es an die Methode für das Modell weiter. Dabei wird der Pfad des Bindungsausdrucks ausgewertet, wenn das Ereignis eintritt. Dies bedeutet, dass – im Gegensatz zu Eigenschaftsbindungen – keine Änderungen am Modell nachverfolgt werden.

Weitere Informationen zur Zeichenfolgensyntax für einen Eigenschaftspfad finden Sie unter [Property-path-Syntax](property-path-syntax.md). Beachten Sie die Unterschiede, die hier für **{x:Bind}** beschrieben werden.

## <a name="properties-that-you-can-set-with-xbind"></a>Mit {x:Bind} festzulegende Eigenschaften

**{x:Bind}** wird mit der *bindingProperties*-Platzhaltersyntax angegeben, da in der Markuperweiterung mehrere Lese-/Schreibeigenschaften einer Binding-Klasse festgelegt werden können. Die Eigenschaften können in beliebiger Reihenfolge mithilfe von durch Kommas getrennten Paaren *propName*=*value* angegeben werden. Beachten Sie, dass Sie in den Bindungsausdruck keine Zeilenumbrüche einschließen können. Für einige Eigenschaften sind Typen erforderlich, die keine Typkonvertierung enthalten, sodass für diese eigene innerhalb von **{x:Bind}** geschachtelte Markuperweiterungen erforderlich sind.

Diese Eigenschaften funktionieren ähnlich wie die Eigenschaften der [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)-Klasse.

| Eigenschaft | Beschreibung |
|----------|-------------|
| **Pfad** | Weitere Informationen finden Sie weiter oben im Abschnitt [Eigenschaftspfad](#property-path). |
| **Converter** | Gibt das Konverterobjekt an, das vom Bindungsmodul aufgerufen wird. Der Konverter kann in XAML festgelegt werden, sofern Sie dabei auf eine Objektinstanz verweisen, die Sie in einer [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) zugewiesen haben. Verweisen Sie anschließend im Ressourcenwörterbuch auf dieses Objekt. |
| **ConverterLanguage** | Gibt die Kultur an, die vom Konverter verwendet werden soll. (Wenn Sie festlegen, sind **ConverterLanguage** sollten Sie auch festlegen werden **Konverter**.) Die Kultur wird als eine standardbasierte-ID festgelegt. Weitere Informationen finden Sie unter [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Gibt den Konverterparameter an, der in der Konverterlogik verwendet werden kann. (Wenn Sie festlegen, sind **ConverterParameter** sollten Sie auch festlegen werden **Konverter**.) Die meisten Konverter verwenden Sie einfachen Logik, die alle benötigten Informationen aus dem übergebenen Wert zu konvertierenden abrufen und müssen keine **ConverterParameter** Wert. Der **ConverterParameter**-Parameter ist für etwas weiter fortgeschrittene Konverterimplementierungen mit mehr als einer Logik gedacht, die überprüft, was in **ConverterParameter** übergeben wird. Sie können auch einen Konverter schreiben, der keine Zeichenfolgenwerte verwendet. Dies ist jedoch sehr ungewöhnlich. Weitere Informationen finden Sie in den „Anmerkungen“ zu [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter). |
| **FallbackValue** | Gibt einen Wert an, der angezeigt wird, wenn die Quelle oder der Pfad nicht aufgelöst werden können. |
| **Modus** | Gibt den Bindungsmodus, als eine der folgenden Zeichenfolgen: "OneTime", "OneWay" oder "TwoWay". Der Standard lautet "OneTime". Beachten Sie, dass dieser vom Standardwert für **{Binding}** abweicht, der in den meisten Fällen "OneWay" lautet. |
| **TargetNullValue** | Gibt einen Wert an, der angezeigt wird, wenn der Quellwert aufgelöst werden kann, aber explizit **null** ist. |
| **BindBack** | Gibt eine Funktion an, die für die umgekehrter Richtung einer bidirektionalen Bindung verwendet wird. |
| **UpdateSourceTrigger** | Gibt an, wann in TwoWay-Bindungen Änderungen vom Steuerelement zum Modell zurückgegeben werden. Der Standardwert für alle Eigenschaften außer TextBox.Text ist PropertyChanged. TextBox.Text wird LostFocus.|

> [!NOTE]
> Wenn Sie Markup von **{Binding}** in **{x:Bind}** konvertieren, beachten Sie die unterschiedlichen Standardwerte der **Mode**-Eigenschaft.
 
> [**X: DefaultBindMode** ](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute) können verwendet werden, um den Standardmodus für X: Bind für ein bestimmtes Segment des der Markup-Struktur ändern. Der ausgewählte Modus gilt für alle x:Bind-Ausdrücke in diesem Element und seinen untergeordneten Elementen, die nicht explizit einen Bindungsmodus festlegen. OneTime ist leistungsfähiger als OneWay, da die Verwendung von OneWay bewirkt, dass mehr Code generiert wird, um die Änderungserkennung zu verknüpfen und zu behandeln.

## <a name="remarks"></a>Hinweise

Da **{x:Bind}** generierten Code für die optimale Nutzung verwendet, sind zur Kompilierzeit Typinformationen erforderlich. Dies bedeutet, dass Sie nur an Eigenschaften binden können, für die Sie den Typ vorab kennen. Aus diesem Grund können Sie **{x:Bind}** nicht mit der **DataContext**-Eigenschaft verwenden, die vom Typ **Object** ist und außerdem zur Laufzeit geändert werden kann.

Bei Verwendung **{X: Bind}** mit Datenvorlagen, Sie müssen angeben, den Typ an gebunden wird, durch Festlegen einer **Datentyp: X** Wert, siehe die [Beispiele](#examples) Abschnitt. Sie können den Typ auch auf eine Schnittstelle oder einen Basisklassentyp festlegen und dann ggf. Umwandlungen verwenden, um einen vollständigen Ausdruck zu formulieren.

Kompilierte Bindungen hängen von der Codegenerierung ab. Wenn Sie daher **{x:Bind}** in einem Ressourcenwörterbuch verwenden, muss das Ressourcenwörterbuch über eine CodeBehind-Klasse verfügen. Ein Codebeispiel finden Sie unter [Ressourcenwörterbücher mit {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind).

Bei Seiten und Benutzersteuerelementen, die kompilierte Bindungen umfassen, befindet sich im generierten Code eine "Bindings"-Eigenschaft. Dazu gehören folgende Methoden:

- **Update()** - Hiermit werden die Werte aller kompilierten Bindungen aktualisiert. Für alle unidirektionalen und bidirektionalen Bindungen werden zur Erkennung von Änderungen Listener eingehängt.
- **Initialize()** : Wenn die Bindungen nicht bereits initialisiert wurden, wird zur Initialisierung der Bindungen Update() aufgerufen
- **StopTracking()** - Hängt die für uni- und bidirektionale Bindungen erstellen Listener aus. Sie können mit der Methode Update() erneut initialisiert werden.

> [!NOTE]
> Ab Windows 10, Version 1607, wird über das XAML-Framework ein integrierter Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand bereitgestellt. Der Konverter verknüpft **true** mit dem Enumerationswert **Visible** und **false** mit dem Wert **Collapsed**, sodass Sie eine Visibility-Eigenschaft an einen booleschen Wert binden können, ohne einen Konverter zu erstellen. Beachten Sie, dass dies keine Funktionsbindung ist, sondern nur eine Eigenschaftsbindung. Für die Verwendung des integrierten Konverters muss die SDK-Zielversion der App mindestens 14393 lauten. Die Verwendung ist nicht möglich, wenn Ihre App für frühere Versionen von Windows 10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

**Tipp**    bei Bedarf an einzelnen geschweiften Klammern nach einem Wert wie z. B. in [ **Pfad** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) oder [ **ConverterParameter** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter), einen umgekehrten Schrägstrich voranstellen: `\{`. Setzen Sie alternativ die gesamte Zeichenfolge mit den geschweiften Klammern, für die Escapezeichen verwendet werden müssen, in weitere Anführungszeichen, z. B. `ConverterParameter='{Mix}'`.

[**Konverter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter), [ **ConverterLanguage** ](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) und **ConverterLanguage** beziehen sich auf das Szenario der ein Wert konvertiert wird, oder geben Sie aus der die Bindungsquelle in einen Typ oder Wert, der mit die Bindungsziel-Eigenschaft kompatibel ist. Weitere Informationen und Beispiele finden Sie im Abschnitt „Datenkonvertierungen“ unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

**{x:Bind}** ist nur eine Markuperweiterung ohne Methode zum programmgesteuerten Erstellen oder Ändern dieser Bindungen. Weitere Informationen zu Markuperweiterungen finden Sie in der [XAML-Übersicht](xaml-overview.md).

