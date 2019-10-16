---
description: 'Die XBIND-Markup Erweiterung ist eine leistungsstarke Alternative zur Bindung. XBIND-New für Windows 10: wird in kürzerer Zeit und weniger Speicher als bei der Bindung ausgeführt und unterstützt ein besseres Debuggen.'
title: xBind-Markuperweiterung
ms.assetid: 529FBEB5-E589-486F-A204-B310ACDC5C06
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: eeb56dce1984afd67e7a44bbfce1453a1d9f531a
ms.sourcegitcommit: 3360db6bc975516e01913d3d73599c964a411052
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/04/2019
ms.locfileid: "70296989"
---
# <a name="xbind-markup-extension"></a>{x:Bind}-Markuperweiterung

**Hinweis**  allgemeine Informationen zur Verwendung der Datenbindung in Ihrer APP mit **{x:Bind}** (und für einen gesamten Vergleich zwischen **{x:Bind}** und **{Binding}** ) finden Sie [unter Ausführliche Informationen zur Datenbindung](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

Die **{x:Bind}** -Markup Erweiterung – New für Windows 10 – stellt eine Alternative zu **{Binding}** dar. **{x:Bind}** wird in kürzerer Zeit und weniger Arbeitsspeicher als **{Binding}** ausgeführt und unterstützt ein besseres Debuggen.

Zur XAML-Kompilierungszeit wird **{X: Bind}** in Code konvertiert, der den Wert aus einer Eigenschaft aus einer Datenquelle abruft und diesen in der im Markup angegeben Eigenschaft festlegt. Das Binding-Objekt kann optional konfiguriert werden, um Änderungen am Wert der Datenquelleneigenschaft zu beobachten und sich basierend auf diesen Änderungen (`Mode="OneWay"`) zu aktualisieren. Es kann optional auch konfiguriert werden, um Änderungen am eigenen Wert per Push zurück an die Quelleigenschaft (`Mode="TwoWay"`) zu senden.

Die von **{x:Bind}** und **{Binding}** erstellten Bindungsobjekte sind von der Funktionsweise her größtenteils identisch. **{x:Bind}** führt allerdings speziellen Code aus, der zur Kompilierzeit generiert wird, und **{Binding}** verwendet eine allgemeine Laufzeitobjektüberprüfung. Folglich bieten **{x:Bind}** -Bindungen (häufig als kompilierte Bindungen bezeichnet) eine hervorragende Leistung, stellen die Validierung Ihrer Bindungsausdrücke bei der Kompilierung bereit und unterstützen das Debuggen, indem Sie die Möglichkeit erhalten, Haltepunkte in den Codedateien festzulegen, die als Teilklasse für die Seite generiert werden. Diese Dateien befinden sich in Ihrem `obj`-Ordner und weisen Namen wie `<view name>.g.cs` auf (für C#).

> [!TIP]
> **{x:Bind}** verfügt über einen Standardmodus von **OneTime**, im Gegensatz zu **{Binding}** , wo der Standardmodus **OneWay** ist. Diese wurde aus Leistungsgründen so gewählt, da **OneWay** bewirkt, dass mehr Code generiert wird, um die Änderungserkennung zu verknüpfen und zu behandeln. Sie können für einen Modus explizit angeben, dass OneWay- oder TwoWay-Bindung verwendet wird. [x:DefaultBindMode](x-defaultbindmode-attribute.md) kann auch verwendet werden, um für ein bestimmtes Segment der Markupstruktur den Standardmodus für **{x:Bind}** zu ändern. Der angegebene Modus gilt für alle **{x:Bind}** -Ausdrücke in diesem Element und seinen untergeordneten Elementen, die nicht explizit einen Bindungsmodus festlegen.

**Beispiel-apps, die {x:Bind} veranschaulichen**

-   [{x:Bind}-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=619989)
-   [QuizGame](https://github.com/microsoft/Windows-appsample-networkhelper)
-   [Beispiel für XAML-UI-Grundlagen](https://go.microsoft.com/fwlink/p/?linkid=619992)

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
| _bindingproperties_ |
| _propName_=-_Wert_\[, _propName_=-_Wert_\]* | Mindestens eine Bindungseigenschaft, die mithilfe einer Name-Wert-Paarsyntax angegeben wird. |
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

Für C++/CX kann **{x:Bind}** keine Bindungen an private Felder und Eigenschaften im Seiten- oder Datenmodell durchführen – Sie benötigen eine öffentliche Eigenschaft, damit die Bindung möglich ist. Der Oberflächenbereich für die Bindung muss als CX-Klassen/-Schnittstellen verfügbar gemacht werden, damit die relevanten Metadaten abgerufen werden können. **Das\[bindbare\]** Attribut sollte nicht benötigt werden.

Mit **x:Bind** müssen Sie **ElementName=xxx** nicht als Teil des Bindungsausdrucks verwenden. Stattdessen können Sie den Namen des Elements als ersten Teil des Pfads für die Bindung verwenden, da benannte Elemente Felder innerhalb der Seite oder des Benutzer Steuer Elements werden, die die Stamm Bindungs Quelle darstellen. 


### <a name="collections"></a>Auflistungen

Wenn die Datenquelle eine Auflistung ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihrer Position oder ihres Indexes angeben. Beispiel: "Teams\[0.\] "Players", wobei das Literale "\[\]" den Wert "0" einschließt, der das erste Element in einer NULL indizierten Auflistung anfordert.

Um einen Indexer verwenden zu können, muss das Modell **IList&lt;T&gt;** oder **IVector&lt;T&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. (Beachten Sie, dass die Indexer&lt;-&gt; Syntax von ilesonlylist t und ivectorview&lt;t&gt; nicht unterstützt wird.) Wenn der Typ der indizierten Eigenschaft **INotifyCollectionChanged** oder **IObservableVector** unterstützt und die Bindung OneWay oder TwoWay ist, registriert und lauscht er auf Änderungsbenachrichtigungen an diesen Schnittstellen. Die Änderungserkennungslogik wird basierend auf allen Auflistungsänderungen aktualisiert, auch wenn sie keine Auswirkungen auf den entsprechenden indizierten Wert hat. Dies geschieht, da die Überwachungslogik für alle Instanzen der Auflistung identisch ist.

Wenn die Datenquelle ein Wörterbuch oder eine Karte ist, kann der Eigenschaftspfad Elemente in der Auflistung anhand ihres Zeichenfolgennamens angeben. Beispiel **\[\]&gt; : TextBlock Text = "{x:Bind Players ' John Smith '"/sucht nach einem Element im Wörterbuch mit dem Namen "John Smith". &lt;** Der Name muss in Anführungszeichen gesetzt werden. Dabei können einfache oder doppelte Anführungszeichen verwendet werden. Das Caret-Symbol (^) kann als Escapezeichen für Anführungszeichen innerhalb von Zeichenfolgen verwendet werden. Es ist normalerweise am einfachsten, Anführungszeichen zu verwenden, die von denen für das XAML-Attribut verwendeten abweichen. (Beachten Sie, dass&lt;ilesonlydictionary t&gt; und imapview&lt;t&gt; die Indexersyntax nicht unterstützen.)

Um einen Zeichenfolgen-Indexer verwenden zu können, muss das Modell **IDictionary&lt;string, T&gt;** oder **IMap&lt;string T&gt;** für den Typ der Eigenschaft implementieren, die indiziert werden soll. Wenn der Typ der indizierten Eigenschaft **IObservableMap** unterstützt und die Bindung OneWay oder TwoWay ist, wird er registriert und überwacht Benachrichtigungen auf diesen Schnittstellen. Die Änderungserkennungslogik wird basierend auf allen Auflistungsänderungen aktualisiert, auch wenn sie keine Auswirkungen auf den entsprechenden indizierten Wert hat. Dies geschieht, da die Überwachungslogik für alle Instanzen der Auflistung identisch ist.

### <a name="attached-properties"></a>Angefügte Eigenschaften

Zum Binden an [angefügte Eigenschaften](./attached-properties-overview.md)müssen Sie die Klassen-und Eigenschaftsnamen in Klammern nach dem Punkt einfügen. Beispiel: **Text="{x:Bind Button22.(Grid.Row)}"** . Wenn die Eigenschaft nicht in einem Xaml-Namespace deklariert wird, müssen Sie ihr einen XML-Namespace als Präfix voranstellen, den Sie einem Codenamespace am Anfang des Dokuments zuordnen sollten.

### <a name="casting"></a>Umwandlung

Kompilierte Bindungen sind stark typisiert und lösen den Typ der einzelnen Schritte in einem Pfad auf. Wenn der zurückgegebene Typ nicht über den Member verfügt, erzeugt er zur Kompilierzeit einen Fehler. Sie können eine Umwandlung angeben, um der Bindung den tatsächlichen Typ des Objekts mitzuteilen. Im folgenden Fall ist **obj** eine Eigenschaft eines Typobjekts, enthält aber ein Textfeld, sodass wir **Text="{x:Bind ((TextBox)obj).Text}"** oder **Text="{x:Bind obj.(TextBox.Text)}"** verwenden können.
Das **groups3** -Feld im **Text = "{x:Bind ((Data: sampledatagroup)\[groups3\]0). Title} "** ist ein Wörterbuch von Objekten, sodass Sie es in" **Daten: sampledatagroup**"umwandeln müssen. Beachten Sie die Verwendung des **data:** -XML-Namespacepräfix für die Zuordnung des Objekttyps zu einem Code-Namespace, der nicht Teil des Standard-XAML-Namespace ist.

_Hinweis: Die C#Umwandlungs Syntax im-Format ist flexibler als die Syntax der angefügten Eigenschaft, und es wird die empfohlene Syntax in Zukunft angezeigt._

## <a name="functions-in-binding-paths"></a>Funktionen in Bindungspfaden

Ab Windows 10, Version 1607, unterstützt **{x: Bind}** die Verwendung einer Funktion als blattbildenden Schritt des Bindungspfades. Dies ist eine leistungsstarke Funktion für die Datenbindung, die mehrere Szenarien im Markup ermöglicht. Weitere Informationen finden Sie unter [Funktions Bindungen](../data-binding/function-bindings.md) .

## <a name="event-binding"></a>Ereignisbindung

Ereignisbindung ist ein besonderes Feature der kompilierte Bindung. Damit können Sie den Handler für ein Ereignis mit einer Bindung angeben. Es muss also keine Methode im CodeBehind-Abschnitt sein. Zum Beispiel: **Klicken Sie auf = "{x:Bind rootframe. GoForward}"** .

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
| **Path** | Weitere Informationen finden Sie weiter oben im Abschnitt [Eigenschaftspfad](#property-path). |
| **Erer** | Gibt das Konverterobjekt an, das vom Bindungsmodul aufgerufen wird. Der Konverter kann in XAML festgelegt werden, sofern Sie dabei auf eine Objektinstanz verweisen, die Sie in einer [{StaticResource}-Markuperweiterung](staticresource-markup-extension.md) zugewiesen haben. Verweisen Sie anschließend im Ressourcenwörterbuch auf dieses Objekt. |
| **Converterlanguage** | Gibt die Kultur an, die vom Konverter verwendet werden soll. (Wenn Sie **converterlanguage** festlegen, sollten Sie auch **Converter**festlegen.) Die Kultur wird als Standard basierter Bezeichner festgelegt. Weitere Informationen finden Sie unter [**ConverterLanguage**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage). |
| **ConverterParameter** | Gibt den Konverterparameter an, der in der Konverterlogik verwendet werden kann. (Wenn Sie **ConverterParameter** festlegen, sollten Sie auch **Converter**festlegen.) Bei den meisten Konvertern wird eine einfache Logik verwendet, die alle benötigten Informationen aus dem übergebenen Wert zum Konvertieren erhält und keinen **ConverterParameter** -Wert benötigt. Der **ConverterParameter**-Parameter ist für etwas weiter fortgeschrittene Konverterimplementierungen mit mehr als einer Logik gedacht, die überprüft, was in **ConverterParameter** übergeben wird. Sie können auch einen Konverter schreiben, der keine Zeichenfolgenwerte verwendet. Dies ist jedoch sehr ungewöhnlich. Weitere Informationen finden Sie in den „Anmerkungen“ zu [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter). |
| **FallbackValue** | Gibt einen Wert an, der angezeigt wird, wenn die Quelle oder der Pfad nicht aufgelöst werden können. |
| **Modus** | Gibt den Bindungs Modus als eine der folgenden Zeichen folgen an: "OneTime", "OneWay" oder "TwoWay". Der Standard lautet "OneTime". Beachten Sie, dass dieser vom Standardwert für **{Binding}** abweicht, der in den meisten Fällen "OneWay" lautet. |
| **TargetNullValue** | Gibt einen Wert an, der angezeigt wird, wenn der Quellwert aufgelöst werden kann, aber explizit **null** ist. |
| **Bindback** | Gibt eine Funktion an, die für die umgekehrter Richtung einer bidirektionalen Bindung verwendet wird. |
| **UpdateSourceTrigger** | Gibt an, wann in TwoWay-Bindungen Änderungen vom Steuerelement zum Modell zurückgegeben werden. Der Standardwert für alle Eigenschaften außer TextBox. Text ist PropertyChanged; TextBox. Text ist LostFocus.|

> [!NOTE]
> Wenn Sie Markup von **{Binding}** in **{x:Bind}** konvertieren, beachten Sie die unterschiedlichen Standardwerte der **Mode**-Eigenschaft.
** 
> [X:DefaultBindMode](https://docs.microsoft.com/windows/uwp/xaml-platform/x-defaultbindmode-attribute)** können verwendet werden, um den Standardmodus für X: Bind für ein bestimmtes Segment des der Markup-Struktur ändern. Der ausgewählte Modus gilt für alle x:Bind-Ausdrücke in diesem Element und seinen untergeordneten Elementen, die nicht explizit einen Bindungsmodus festlegen. OneTime ist leistungsfähiger als OneWay, da die Verwendung von OneWay bewirkt, dass mehr Code generiert wird, um die Änderungserkennung zu verknüpfen und zu behandeln.

## <a name="remarks"></a>Hinweise

Da **{x:Bind}** generierten Code für die optimale Nutzung verwendet, sind zur Kompilierzeit Typinformationen erforderlich. Dies bedeutet, dass Sie nur an Eigenschaften binden können, für die Sie den Typ vorab kennen. Aus diesem Grund können Sie **{x:Bind}** nicht mit der **DataContext**-Eigenschaft verwenden, die vom Typ **Object** ist und außerdem zur Laufzeit geändert werden kann.

Wenn Sie " **{x:Bind}** " mit Datenvorlagen verwenden, müssen Sie den Typ angeben, an den gebunden wird, indem Sie einen **x:datatype** -Wert festlegen, wie im Abschnitt " [Beispiele](#examples) " gezeigt. Sie können den Typ auch auf eine Schnittstelle oder einen Basisklassentyp festlegen und dann ggf. Umwandlungen verwenden, um einen vollständigen Ausdruck zu formulieren.

Kompilierte Bindungen hängen von der Codegenerierung ab. Wenn Sie daher **{x:Bind}** in einem Ressourcenwörterbuch verwenden, muss das Ressourcenwörterbuch über eine CodeBehind-Klasse verfügen. Ein Codebeispiel finden Sie unter [Ressourcenwörterbücher mit {x:Bind}](../data-binding/data-binding-in-depth.md#resource-dictionaries-with-x-bind).

Bei Seiten und Benutzersteuerelementen, die kompilierte Bindungen umfassen, befindet sich im generierten Code eine "Bindings"-Eigenschaft. Dazu gehören folgende Methoden:

- **Update()** - Hiermit werden die Werte aller kompilierten Bindungen aktualisiert. Für alle unidirektionalen und bidirektionalen Bindungen werden zur Erkennung von Änderungen Listener eingehängt.
- **Initialize()** : Wenn die Bindungen nicht bereits initialisiert wurden, wird zur Initialisierung der Bindungen Update() aufgerufen
- **StopTracking()** - Hängt die für uni- und bidirektionale Bindungen erstellen Listener aus. Sie können mit der Methode Update() erneut initialisiert werden.

> [!NOTE]
> Ab Windows 10, Version 1607, wird über das XAML-Framework ein integrierter Konverter für die Konvertierung eines booleschen Operanden in einen Sichtbarkeitszustand bereitgestellt. Der Konverter verknüpft **true** mit dem Enumerationswert **Visible** und **false** mit dem Wert **Collapsed**, sodass Sie eine Visibility-Eigenschaft an einen booleschen Wert binden können, ohne einen Konverter zu erstellen. Beachten Sie, dass dies keine Funktionsbindung ist, sondern nur eine Eigenschaftsbindung. Für die Verwendung des integrierten Konverters muss die SDK-Zielversion der App mindestens 14393 lauten. Die Verwendung ist nicht möglich, wenn Ihre App für frühere Versionen von Windows 10 bestimmt ist. Weitere Informationen zu Zielversionen finden Sie unter [Versionsadaptiver Code](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Tipp   :`\{`Wenn Sie eine einzelne geschweifte Klammer für einen Wert angeben müssen, z. b. in " [**path**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.path) " oder " [**ConverterParameter**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterparameter)", stellen Sie einen umgekehrten Schrägstrich vor. Setzen Sie alternativ die gesamte Zeichenfolge mit den geschweiften Klammern, für die Escapezeichen verwendet werden müssen, in weitere Anführungszeichen, z. B. `ConverterParameter='{Mix}'`.

[**Konvertersprache**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converterlanguage) und **konvertersprache** beziehen sich alle auf das [**Szenario, in**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.data.binding.converter)dem ein Wert oder ein Typ von der Bindungs Quelle in einen Typ oder Wert konvertiert wird, der mit der Bindungs Ziel Eigenschaft kompatibel ist. Weitere Informationen und Beispiele finden Sie im Abschnitt „Datenkonvertierungen“ unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

**{x:Bind}** ist nur eine Markuperweiterung ohne Methode zum programmgesteuerten Erstellen oder Ändern dieser Bindungen. Weitere Informationen zu Markuperweiterungen finden Sie in der [XAML-Übersicht](xaml-overview.md).

