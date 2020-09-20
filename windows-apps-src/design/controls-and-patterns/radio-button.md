---
description: Erfahren Sie, wie Sie Optionsfelder verwenden, um Benutzern zu ermöglichen, eine Option aus einer Sammlung von zwei oder mehr sich gegenseitig ausschließenden, aber verwandten Optionen auszuwählen.
title: Richtlinien für Optionsfelder
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 06/10/2020
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ee933bd28594263e61e654b14b0541c6fa9ed41b
ms.sourcegitcommit: 875bd348608547e7a66fa4b460efe64b3246807e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/14/2020
ms.locfileid: "90080842"
---
# <a name="radio-buttons"></a>Optionsfelder

Mithilfe von Optionsfeldern können Benutzer eine Option aus einer Sammlung von zwei oder mehr sich gegenseitig ausschließenden, aber verwandten Optionen auswählen. Optionsfelder werden immer in Gruppen verwendet, und jede Option wird in der Gruppe durch ein Optionsfeld dargestellt.

Im Standardzustand ist in einer RadioButtons-Gruppe kein Optionsfeld ausgewählt. Das heißt, alle Optionsfelder sind deaktiviert. Sobald ein Benutzer ein Optionsfeld ausgewählt hat, kann die Auswahl nicht mehr aufgehoben werden, um die Gruppe wieder in ihren ursprünglichen Zustand ohne Auswahl zurück zu versetzen.

Das singuläre Verhalten einer Optionsfeldgruppe (RadioButtons) unterscheidet sie von [Kontrollkästchen](checkbox.md), die Mehrfachauswahl und das Aufheben der Auswahl (Deaktivieren) unterstützen.

:::image type="content" source="images/controls/radio-button.png" alt-text="Beispiel für eine RadioButtons-Gruppe mit einem aktivierten Optionsfeld.":::

**Abrufen der Windows-UI-Bibliothek**

| &nbsp; | &nbsp; |
| - | - |
| ![WinUI-Logo](images/winui-logo-64x64.png) | Das Steuerelement RadioButtons ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> **Windows-UI-Bibliotheks-APIs:** [RadioButtons-Klasse](/uwp/api/microsoft.ui.xaml.controls.radiobuttons), [SelectedItem-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex-Eigenschaft](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex), [SelectionChanged-Ereignis](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged)
>
> **Plattform-APIs:** [RadioButton-Klasse](/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [IsChecked-Eigenschaft](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked), [Checked-Ereignis](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked)

> [!IMPORTANT]
> **Vergleich von „RadioButtons“ und „RadioButton“**
>
>Es gibt zwei Möglichkeiten, Optionsfeldgruppen zu erstellen.
>
>- Beginnend mit WinUI 2.3 empfehlen wir das Steuerelement **[RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)** . Dieses Steuerelement vereinfacht das Layout, regelt Tastaturnavigation und Barrierefreiheit und unterstützt die Bindung an eine Datenquelle.
>- Sie können Gruppen einzelner **[RadioButton](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton)** -Steuerelemente verwenden. Wenn Ihre Anwendung nicht WinUI 2.3 oder höher verwendet, ist dies die einzige Option.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie Optionsfelder, um Benutzern zu ermöglichen, eine Auswahl aus zwei oder mehr Optionen, die sich gegenseitig ausschließen, zu treffen.

:::image type="content" source="images/radiobutton_basic.png" alt-text="Eine RadioButtons-Gruppe mit einem aktivierten Optionsfeld.":::

Verwenden Sie Optionsfelder, wenn Benutzer alle Optionen sehen müssen, bevor sie eine Auswahl treffen. Optionsfelder heben alle Optionen gleichermaßen hervor, was bedeutet, dass manche Optionen möglicherweise mehr Aufmerksamkeit auf sich ziehen, als eigentlich erforderlich oder wünschenswert ist.

Wenn nicht alle Optionen dieselbe Aufmerksamkeit benötigen, erwägen Sie die Verwendung anderer Steuerelemente. Um beispielsweise eine einzelne beste Option für die meisten Benutzer und in den meisten Situationen zu empfehlen, verwenden Sie ein [Kombinationsfeld](combo-box.md), um diese beste Option als Standardoption anzuzeigen.

:::image type="content" source="images/combo_box_collapsed.png" alt-text="Ein Kombinationsfeld mit angezeigter Standardoption.":::

Wenn es nur zwei mögliche Optionen gibt, die sich eindeutig als binäre Auswahl ausdrücken lassen, wie z. B. ein/aus oder ja/nein, kombinieren Sie sie in einem einzigen [Kontrollkästchen](checkbox.md) oder [Umschalter](toggles.md). Verwenden Sie beispielsweise ein einzelnes Kontrollkästchen für „Ich stimme zu“ anstelle von zwei Optionsfeldern für „Ich stimme zu“ und „Ich stimme nicht zu“.

Verwenden Sie nicht zwei Optionsfelder für eine einfache binäre Auswahl:

:::image type="content" source="images/radiobutton-vs-checkbox-rb.png" alt-text="Zwei Optionsfelder für die Darstellung einer binären Auswahl":::

Verwenden Sie stattdessen ein Kontrollkästchen:

:::image type="content" source="images/radiobutton-vs-checkbox-cb.png" alt-text="Ein Kontrollkästchen ist eine gute Alternative zum Anbieten einer binären Auswahl.":::

Wenn Benutzer mehrere Optionen auswählen können, verwenden Sie [Kontrollkästchen](checkbox.md).

:::image type="content" source="images/checkbox2.png" alt-text="Kontrollkästchen unterstützen Mehrfachauswahl":::

Wenn die Optionen des Benutzers in einem Wertebereich liegen (z. B. *10, 20, 30, ... 100*), verwenden Sie ein [Schieberegler](slider.md)-Steuerelement.

:::image type="content" source="images/controls/slider.png" alt-text="Ein Schieberegler-Steuerelement, das einen Wert aus einem Wertebereich anzeigt.":::

Wenn mehr als acht Optionen vorhanden sind, verwenden Sie ein [Kombinationsfeld](combo-box.md).

:::image type="content" source="images/combo_box_scroll.png" alt-text="Ein Listenfeld, das mehrere Optionen anzeigt.":::

Wenn die verfügbaren Optionen auf dem aktuellen Kontext einer App basieren oder anderweitig dynamisch variieren können, verwenden Sie ein Listensteuerelement.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="The XAML Controls Gallery app icon"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, <a href="xamlcontrolsgallery:/item/RadioButton">öffnen Sie sie, um das RadioButtons-Steuerelement in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="winui-radiobuttons-overview"></a>Übersicht über das WinUI-RadioButtons-Steuerelement

Das Tastaturzugriffs- und -navigationsverhalten wurden im [RadioButton](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)-Steuerelement optimiert. Diese Verbesserungen helfen sowohl Barrierefreiheits- als auch Tastaturbenutzern dabei, sich schneller und einfacher in der Liste der Optionen zu bewegen.

Zusätzlich zu diesen Verbesserungen wurde auch das visuelle Standardlayout einzelner Optionsfelder in einer RadioButtons-Gruppe durch automatische Einstellungen für Ausrichtung, Abstand und Ränder optimiert. Durch diese Optimierung entfällt die Notwendigkeit, diese Eigenschaften anzugeben, wie es bei Verwendung eines primitiven Gruppierungssteuerelements wie [StackPanel](../layout/layout-panels.md#stackpanel) oder [Grid](../layout/layout-panels.md#grid) erforderlich wäre.

### <a name="navigating-a-radiobuttons-group"></a>Navigieren in einer RadioButtons-Gruppe

Das Steuerelement `RadioButtons` verfügt über ein spezielles Navigationsverhalten, mit dem Tastaturbenutzer schneller und einfacher durch die Liste navigieren können.

#### <a name="keyboard-focus"></a>Tastaturfokus

Das `RadioButtons`-Steuerelement unterstützt zwei Zustände:

- Kein Optionsfeld ist ausgewählt.
- Ein Optionsfeld ist ausgewählt.

In den nächsten Abschnitten wird das Fokusverhalten des Steuerelements in den beiden Zuständen beschrieben.

##### <a name="no-radio-button-is-selected"></a>Kein Optionsfeld ist ausgewählt.

Wenn kein Optionsfeld ausgewählt ist, erhält das erste Optionsfeld in der Liste den Fokus.

> [!NOTE]
> Das Element, das den TAB-TASTEN-Fokus durch die erste Navigation per TAB-TASTE empfängt, wird nicht ausgewählt.

:::row:::
   :::column span="":::
     **_Liste ohne Registerkartenfokus; ohne Auswahl_**

     ![Liste ohne Registerkartenfokus und ohne ausgewähltes Element](images/radiobutton-no-selected-item-no-tab-focus.png)
   :::column-end:::
   :::column span="":::
      **_Liste mit anfänglichem Registerkartenfokus; ohne Auswahl_**

      ![Liste mit anfänglichem Registerkartenfokus und ohne ausgewähltes Element](images/radiobutton-no-selected-item-tab-focus.png)
   :::column-end:::
:::row-end:::

##### <a name="one-radio-button-is-selected"></a>Ein Optionsfeld ist ausgewählt.

Wenn ein Benutzer mittels TAB-TASTE in eine Liste wechselt, in der bereits ein Optionsfeld ausgewählt ist, erhält das ausgewählte Optionsfeld den Fokus.

:::row:::
   :::column span="":::
     **_Liste ohne Registerkartenfokus_**

     ![Liste ohne Registerkartenfokus und ausgewähltes Element](images/radiobutton-selected-item-no-tab-focus.png)
   :::column-end:::
   :::column span="":::
      **_Liste mit anfänglichem Registerkartenfokus_**

      ![Liste mit anfänglichem Registerkartenfokus und ausgewähltem Element](images/radiobutton-selected-item-tab-focus.png)
   :::column-end:::
:::row-end:::

#### <a name="keyboard-navigation"></a>Tastaturnavigation

Weitere Informationen zum allgemeinen Tastaturnavigationsverhalten finden Sie unter [Tastaturinteraktionen – Navigation](../input/keyboard-interactions.md#navigation).

Wenn ein Element in einer `RadioButtons`-Gruppe bereits den Fokus hält, kann der Benutzer mit den Pfeiltasten zwischen den Elementen innerhalb der Gruppe navigieren. Die NACH-UNTEN-TASTE und die NACH-OBEN-TASTE bewegen den Fokus zum „nächsten“ bzw. „vorherigen“ logischen Element, wie in Ihrem XAML-Markup definiert. Die NACH-LINKS-TASTE und die NACH-RECHTS-TASTE bewegen die Eingabemarke räumlich.

##### <a name="navigation-within-single-column-or-single-row-layouts"></a>Navigation in einspaltigen oder einzeiligen Layouts

Bei einem einspaltigen oder einzeiligen Layout führt die Tastaturnavigation zu folgendem Verhalten:

:::row:::
   :::column span="":::
     **_Einzelne Spalte_**

     ![Beispiel für die Tastaturnavigation in einer RadioButton-Gruppe mit einer einzigen Spalte](images/radiobutton-keyboard-navigation-single-column.png)

     Die NACH-OBEN-TASTE und die NACH-UNTEN-TASTE bewegen den Fokus zwischen Elementen.</br>Die NACH-LINKS-TASTE und die NACH-RECHTS-TASTE haben keine Funktion.
   :::column-end:::
   :::column span="":::
      **_Einzelne Zeile_**

      ![Beispiel für die Tastaturnavigation in einer RadioButton-Gruppe mit einer einzigen Zeile](images/radiobutton-keyboard-navigation-single-row.png)

      Die NACH-LINKS-TASTE und die NACH-OBEN-TASTE bewegen den Fokus zum vorherigen Element, und die NACH-RECHTS-TASTE und die NACH-UNTEN-TASTE bewegen den Fokus zum nächsten Element.
   :::column-end:::
:::row-end:::

##### <a name="navigation-within-multi-column-multi-row-layouts"></a>Navigieren in Layouts mit mehreren Spalten und Zeilen

In einem mehrspaltigen, mehrzeiligen Rasterlayout führt die Tastaturnavigation zu diesem Verhalten:

**_NACH-LINKS-TASTE bzw. NACH-RECHTS-TASTE_**

:::row:::
   :::column span="":::
      ![Beispiel für die horizontale Tastaturnavigation in einer RadioButtons-Gruppe mit mehreren Spalten oder Zeilen](images/radiobutton-keyboard-navigation-multi-column-row-1.png)

      

      Mit der NACH-LINKS-TASTE bzw. NACH-RECHTS-TASTE wird der Fokus zwischen Elementen in einer Zeile horizontal verschoben.
   :::column-end:::
   :::column span="":::
     ![Beispiel für horizontale Tastaturnavigation mit Fokus auf dem letzten Element in einer Spalte](images/radiobutton-keyboard-navigation-multi-column-row-3.png)

      Wenn der Fokus auf dem letzten Element in einer Spalte liegt und die NACH-RECHTS- oder NACH-LINKS-TASTE gedrückt wird, wird der Fokus auf das letzte Element in der nächsten oder vorherigen Spalte (falls vorhanden) verschoben.
   :::column-end:::
:::row-end:::

**_NACH-OBEN-TASTE bzw. NACH-UNTEN-TASTE_**

:::row:::
   :::column span="":::
      ![Beispiel für die vertikale Tastaturnavigation in einer RadioButtons-Gruppe mit mehreren Spalten oder Zeilen](images/radiobutton-keyboard-navigation-multi-column-row-2.png)

      Mit der NACH-OBEN-TASTE bzw. NACH-UNTEN-TASTE wird der Fokus zwischen Elementen in einer Spalte vertikal verschoben.
   :::column-end:::
   :::column span="":::
     ![Beispiel für vertikale Tastaturnavigation mit Fokus auf dem letzten Element in einer Spalte](images/radiobutton-keyboard-navigation-multi-column-row-4.png)

      Wenn der Fokus auf dem letzten Element in einer Spalte liegt und die NACH-UNTEN-TASTE gedrückt wird, wird der Fokus auf das erste Element in der nächsten Spalte (falls vorhanden) verschoben. Wenn der Fokus auf dem ersten Element in einer Spalte liegt und die NACH-OBEN-TASTE gedrückt wird, wird der Fokus auf das letzte Element in der vorherigen Spalte (falls vorhanden) verschoben.
   :::column-end:::
:::row-end:::

Weitere Informationen finden Sie unter [Tastaturinteraktionen](../input/keyboard-interactions.md#wrapping-homogeneous-list-and-grid-view-items).

###### <a name="wrapping"></a>Umbruch

Innerhalb der RadioButtons-Gruppe springt der Fokus nicht von der ersten Zeile oder Spalte zur letzten bzw. von der letzten Zeile oder Spalte zur ersten. Dies liegt daran, dass bei Verwendung einer Sprachausgabe das Gefühl für Grenzen und eine klare Festlegung von Anfang und Ende verloren geht, wodurch es für Benutzer mit Sehbehinderung schwierig wird, in der Liste zu navigieren.

Das `RadioButtons`-Steuerelement unterstützt auch keine Enumeration, da das Steuerelement eine angemessene Anzahl von Elementen enthalten soll (siehe [Ist dies das richtige Steuerelement?](#is-this-the-right-control)).

### <a name="selection-follows-focus"></a>Auswahl folgt Fokus

Wenn Sie die Tastatur verwenden, um zwischen Elementen in einer `RadioButtons`-Gruppe zu navigieren, wird beim Verschieben des Fokus von einem zum anderen Element jeweils das Element, das den Fokus erhält, ausgewählt, und das Element, das zuvor den Fokus besaß, wird deaktiviert.

:::row:::
   :::column span="":::
      **_Vor der Navigation mithilfe der Tastatur_**

      ![Beispiel für Fokus und Auswahl vor der Tastaturnavigation](images/radiobutton-two-selected-before-keyboard-navigation.png)

      Fokus und Auswahl vor der Tastaturnavigation.
   :::column-end:::
   :::column span="":::
     **_Nach der Navigation mithilfe der Tastatur_**

      ![Beispiel für Fokus und Auswahl nach der Tastaturnavigation](images/radiobutton-three-selected-after-keyboard-navigation.png)

      Fokus und Auswahl nach der Tastaturnavigation, wobei die NACH-UNTEN-TASTE den Fokus zu Optionsfeld 3 verschiebt, dieses auswählt und Optionsfeld 2 deaktiviert.
   :::column-end:::
:::row-end:::

Sie können den Fokus verschieben, ohne die Auswahl zu ändern, indem Sie zum Navigieren STRG+Pfeiltasten verwenden. Nachdem der Fokus verschoben wurde, können Sie die LEERTASTE verwenden, um das Element, das derzeit den Fokus besitzt, zu aktivieren.

#### <a name="navigating-with-xbox-gamepad-and-remote-control"></a>Navigation mit Xbox-Gamepad und Xbox-Fernbedienung

Wenn Sie ein Xbox-Gamepad oder eine Xbox-Fernbedienung verwenden, um zwischen Optionsfeldern zu navigieren, ist das Verhalten „Auswahl folgt Fokus“ deaktiviert, und der Benutzer muss die „A“-Taste drücken, um das Optionsfeld, das aktuell den Fokus hält, auszuwählen.

## <a name="accessibility-behavior"></a>Barrierefreiheitsverhalten

In der folgenden Tabelle wird beschrieben, wie die Sprachausgabe eine `RadioButtons`-Gruppe behandelt und was angekündigt wird. Dieses Verhalten hängt davon ab, wie ein Benutzer die Detaileinstellungen der Sprachausgabe festgelegt hat.

| Aktion | Ansage der Sprachausgabe |
|:--|:--|
| Fokus wechselt zu einem ausgewählten Element | „_Name_, RadioButton, ausgewählt, _x_ von _N_“ |
|Fokus wechselt zu einem deaktivierten Element<br> *(Bei der Navigation mit STRG-Pfeiltasten oder Xbox-Gamepad <br>, was bedeutet, dass die Auswahl nicht dem Fokus folgt.)* | „_Name_, RadioButton, nicht ausgewählt, _x_ von _N_“  |

> [!NOTE]
> Der _**Name**_, den die Sprachausgabe für jedes Element nennt, ist der Wert der angefügten Eigenschaft [AutomationProperties.Name](/uwp/api/windows.ui.xaml.automation.automationproperties.nameproperty), wenn diese für das Element verfügbar ist; andernfalls ist es der Wert, der von der [ToString](/dotnet/api/system.object.tostring?view=dotnet-uwp-10.0)-Methode des Elements zurückgegeben wird.
>
> _**x**_ ist die Nummer des aktuellen Elements. _**N**_ gibt die Gesamtzahl der Elemente in der Gruppe an.

## <a name="create-a-winui-radiobuttons-group"></a>Erstellen einer WinUI-RadioButtons-Gruppe

Das `RadioButtons`-Steuerelement verwendet ein Inhaltsmodell ähnlich einem [ItemsControl](/uwp/api/windows.ui.xaml.controls.itemscontrol)-Element. Das bedeutet, dass Sie folgende Aktionen ausführen können:

- Sie können das Steuerelement auffüllen, indem Sie Elemente direkt zur [Items](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.items)-Sammlung hinzufügen oder indem Sie Daten an seine [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource)-Eigenschaft binden.
- Sie können die Eigenschaften [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) oder [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem) verwenden, um die ausgewählte Option abzurufen und festzulegen.
- Sie können das Ereignis [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) so verarbeiten, dass es eine Aktion auslöst, wenn eine Option ausgewählt wird.

> [!TIP]
> In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt: `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using**-Anweisung hinzugefügt: `using muxc = Microsoft.UI.Xaml.Controls;`

Hier deklarieren Sie ein einfaches `RadioButtons`-Steuerelement mit drei Optionen. Mit der [Header](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.header)-Eigenschaft geben Sie eine Beschriftung für die Gruppe an, und mit der `SelectedIndex`-Eigenschaft legen Sie eine Standardoption fest.

```xaml
<muxc:RadioButtons Header="Background color"
                   SelectedIndex="0"
                   SelectionChanged="BackgroundColor_SelectionChanged">
    <x:String>Red</x:String>
    <x:String>Green</x:String>
    <x:String>Blue</x:String>
</muxc:RadioButtons>
```

Das Ergebnis sieht wie folgt aus:

:::image type="content" source="images/radiobuttons-default-group.png" alt-text="Eine Gruppe von drei Optionsfeldern":::

Um eine Aktion auszuführen, wenn der Benutzer eine Option auswählt, wird das Ereignis [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) verarbeitet. Hier ändern Sie die Hintergrundfarbe eines [Border](/uwp/api/windows.ui.xaml.controls.border)-Elements mit dem Namen „ExampleBorder“ (`<Border x:Name="ExampleBorder" Width="100" Height="100"/>`).

```csharp
private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    if (ExampleBorder != null && sender is muxc.RadioButtons rb)
    {
        string colorName = rb.SelectedItem as string;
        switch (colorName)
        {
            case "Red":
                ExampleBorder.Background = new SolidColorBrush(Colors.Red);
                break;
            case "Green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
        }
    }
}
```

> [!TIP]
> Sie können das ausgewählte Element auch von der [SelectionChangedEventArgs.AddedItems](/uwp/api/windows.ui.xaml.controls.selectionchangedeventargs.addeditems)-Eigenschaft abrufen. Es gibt nur ein ausgewähltes Element mit dem Index 0, sodass Sie das ausgewählte Element auf diese Weise abrufen können: `string colorName = e.AddedItems[0] as string;`.

### <a name="selection-states"></a>Auswahlzustände

Ein Optionsfeld hat zwei Zustände: „aktiviert“ und „deaktiviert“. Wenn eine Option in einer `RadioButtons`-Gruppe ausgewählt ist, können Sie ihren Wert aus der [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem)-Eigenschaft und ihre Position in der Sammlung aus der [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex)-Eigenschaft abrufen. Ein Optionsfeld kann deaktiviert werden, wenn ein Benutzer ein anderes Optionsfeld in derselben Gruppe auswählt, doch es kann nicht durch erneutes Auswählen deaktiviert werden. Sie können eine Optionsfeldgruppe jedoch programmgesteuert deaktivieren, indem Sie sie auf `SelectedItem = null` oder `SelectedIndex = -1` festlegen. (Wird `SelectedIndex` auf einen Wert außerhalb des Bereichs der `Items`-Sammlung festgelegt, erfolgt keine Auswahl.)

### <a name="radiobuttons-content"></a>Radiobuttons-Inhalt

Im vorherigen Beispiel haben Sie das `RadioButtons`-Steuerelement mit einfachen Zeichenfolgen aufgefüllt. Das Steuerelement stellte die Optionsschaltflächen zur Verfügung und verwendete die Zeichenfolgen als Beschriftung für die einzelnen Optionsschaltflächen.

Sie können das `RadioButtons`-Steuerelement jedoch mit einem beliebigen Objekt auffüllen. In der Regel möchten Sie, dass das Objekt eine Zeichenfolgendarstellung bereitstellt, die als Textbezeichnung verwendet werden kann. In einigen Fällen kann ein Bild anstelle von Text geeignet sein.

Hier werden [SymbolIcon](/uwp/api/windows.ui.xaml.controls.symbolicon)-Elemente verwendet, um das Steuerelement aufzufüllen.

```xaml
<muxc:RadioButtons Header="Choose the icon without an arrow">
    <SymbolIcon Symbol="Back"/>
    <SymbolIcon Symbol="Attach"/>
    <SymbolIcon Symbol="HangUp"/>
    <SymbolIcon Symbol="FullScreen"/>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-symbolicon.png" alt-text="Eine Gruppe von Optionsschaltflächen mit Symbolen":::

Sie können auch einzelne [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton)-Steuerelemente verwenden, um die `RadioButtons`-Elemente aufzufüllen. Dies ist ein Sonderfall, den wir zu einem späteren Zeitpunkt erörtern. Siehe [RadioButton-Steuerelemente in einer RadioButtons-Gruppe]().

Ein Vorteil der Verwendung beliebiger Objekte besteht darin, dass Sie das `RadioButtons`-Steuerelement an einen benutzerdefinierten Typ im Datenmodell binden können. Dies wird im nächsten Abschnitt veranschaulicht.

### <a name="data-binding"></a>Datenbindung

Das `RadioButtons`-Steuerelement unterstützt die Datenbindung an seine [ItemsSource](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.itemssource)-Eigenschaft. Dieses Beispiel veranschaulicht, wie Sie das Steuerelement an eine benutzerdefinierte Datenquelle binden können. Die Darstellung und Funktionalität dieses Beispiels ist identisch mit dem vorherigen Beispiel zum Ändern der Hintergrundfarbe, aber hier werden die Farbpinsel im Datenmodell gespeichert und nicht im `SelectionChanged`-Ereignishandler erstellt.

```xaml
 <muxc:RadioButtons Header="Background color"
                    SelectedIndex="0"
                    SelectionChanged="BackgroundColor_SelectionChanged"
                    ItemsSource="{x:Bind colorOptionItems}"/>
```

```c#
public sealed partial class MainPage : Page
{
    // Custom data item.
    public class ColorOptionDataModel
    {
        public string Label { get; set; }
        public SolidColorBrush ColorBrush { get; set; }

        public override string ToString()
        {
            return Label;
        }
    }

    List<ColorOptionDataModel> colorOptionItems;

    public MainPage1()
    {
        this.InitializeComponent();

        colorOptionItems = new List<ColorOptionDataModel>();
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Red", ColorBrush = new SolidColorBrush(Colors.Red) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Green", ColorBrush = new SolidColorBrush(Colors.Green) });
        colorOptionItems.Add(new ColorOptionDataModel()
            { Label = "Blue", ColorBrush = new SolidColorBrush(Colors.Blue) });
    }

    private void BackgroundColor_SelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        var option = e.AddedItems[0] as ColorOptionDataModel;
        ExampleBorder.Background = option?.ColorBrush;
    }
}
```

### <a name="radiobutton-controls-in-a-radiobuttons-group"></a>RadioButton-Steuerelemente in einer RadioButtons-Gruppe

Sie können einzelne [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton)-Steuerelemente verwenden, um die `RadioButtons`-Elemente aufzufüllen. Sie können dies tun, um Zugriff auf bestimmte Eigenschaften zu erhalten, wie z. B. `AutomationProperties.Name`; oder Sie verfügen vielleicht über vorhandenen `RadioButton`-Code, möchten aber das Layout und die Navigation von `RadioButtons` nutzen.

```xaml
<muxc:RadioButtons Header="Background color">
    <RadioButton Content="Red" Tag="red" AutomationProperties.Name="red"/>
    <RadioButton Content="Green" Tag="green" AutomationProperties.Name="green"/>
    <RadioButton Content="Blue" Tag="blue" AutomationProperties.Name="blue"/>
</muxc:RadioButtons>
```

Wenn Sie `RadioButton`-Steuerelemente in einer `RadioButtons`-Gruppe verwenden, weiß das `RadioButtons`-Steuerelement, wie das `RadioButton`-Steuerelement zu präsentieren ist, sodass am Ende nicht zwei Auswahlkreise angezeigt werden.

Es gibt jedoch einige Verhaltensweisen, die Sie kennen sollten. Es empfiehlt sich, den Zustand und die Ereignisse mit den einzelnen Steuerelementen oder mit der `RadioButtons`-Gruppe zu verarbeiten, aber nicht mit beiden, um Konflikte zu vermeiden.

Diese Tabelle enthält die zugehörigen Ereignisse und Eigenschaften für beide Steuerelemente.

|RadioButton  |RadioButtons  |
|---------|---------|
|[Checked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.checked), [Unchecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.unchecked), [Click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) |    [SelectionChanged](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectionchanged) |
|[IsChecked](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton.ischecked)  | [SelectedItem](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selecteditem), [SelectedIndex](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.selectedindex) |

Wenn Sie Ereignisse für ein einzelnes `RadioButton`-Steuerelement behandeln, z. B. `Checked` oder `Unchecked`, und auch das Ereignis `RadioButtons.SelectionChanged` behandeln, werden beide Ereignisse ausgelöst. Das `RadioButton`-Ereignis tritt zuerst ein, und dann das `RadioButtons.SelectionChanged`-Ereignis, was zu Konflikten führen könnte.

Die Eigenschaften `IsChecked`, `SelectedItem` und `SelectedIndex` bleiben synchronisiert. Eine Änderung an einer Eigenschaft aktualisiert die beiden anderen.

Die [RadioButton.GroupName](/uwp/api/windows.ui.xaml.controls.radiobutton.groupname)-Eigenschaft wird ignoriert. Die Gruppe wird vom `RadioButtons`-Steuerelement erstellt.

### <a name="defining-multiple-columns"></a>Definieren mehrerer Spalten

Standardmäßig ordnet das `RadioButtons`-Steuerelement seine Optionsfelder vertikal in einer einzelnen Spalte an. Sie können die Eigenschaft [MaxColumns](/uwp/api/microsoft.ui.xaml.controls.radiobuttons.maxcolumns) festlegen, um die Optionsfelder des Steuerelements in mehreren Spalten anzuordnen. (Wenn Sie dies tun, werden die Optionsfelder in spaltenweise absteigender Reihenfolge angeordnet, wobei die Elemente von oben nach unten und dann von links nach rechts ausgefüllt werden.)

```xaml
<muxc:RadioButtons Header="Options" MaxColumns="3">
    <x:String>Item 1</x:String>
    <x:String>Item 2</x:String>
    <x:String>Item 3</x:String>
    <x:String>Item 4</x:String>
    <x:String>Item 5</x:String>
    <x:String>Item 6</x:String>
</muxc:RadioButtons>
```

:::image type="content" source="images/radiobuttons-multi-column.png" alt-text="Optionsfelder in zwei dreispaltigen Gruppen.":::

> [!TIP]
> Um Elemente in einer einzigen horizontalen Zeile anzuordnen, setzen Sie `MaxColumns` gleich der Anzahl der Elemente in der Gruppe.

## <a name="create-your-own-radiobutton-group"></a>Erstellen einer eigenen Optionsfeldgruppe

> [!Important]
> Außer wenn sie eine ältere Version von WinUI verwenden, empfiehlt es sich, das WinUI-`RadioButtons`-Steuerelement zu verwenden, um `RadioButton`-Elemente zu gruppieren.

Optionsfelder funktionieren in Gruppen. Sie können einzelne [RadioButton](/uwp/api/Windows.UI.Xaml.Controls.RadioButton)-Steuerelemente auf eine von zwei Arten gruppieren:

- Platzieren Sie sie im gleichen übergeordneten Container.
- Legen Sie die [GroupName](/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName)-Eigenschaft für jedes Optionsfeld auf denselben Wert fest.

In diesem Beispiel wird die erste Gruppe von Optionsfeldern implizit gruppiert, da die Felder im selben StackPanel-Element enthalten sind. Die zweite Gruppe wird auf zwei StackPanel-Elemente aufgeteilt, damit sie mit `GroupName` explizit in eine einzelne Gruppe gruppiert werden.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 1 - implicit grouping -->
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="white" Checked="BGRadioButton_Checked"
                         IsChecked="True"/>
        </StackPanel>
    </StackPanel>

    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <!-- Group 2 - grouped by GroupName -->
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" Tag="green" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" Tag="yellow" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" Tag="blue" GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" Tag="white"  GroupName="BorderBrush"
                             Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="ExampleBorder"
            BorderBrush="#FFFFD700" Background="#FFFFFFFF"
            BorderThickness="10" Height="50" Margin="0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "green":
                ExampleBorder.Background = new SolidColorBrush(Colors.Green);
                break;
            case "blue":
                ExampleBorder.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "white":
                ExampleBorder.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && ExampleBorder != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "yellow":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "green":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "blue":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "white":
                ExampleBorder.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Diese beiden Gruppen von `RadioButton`-Steuerelementen sehen wie folgt aus:

:::image type="content" source="images/radio-button-groups.png" alt-text="Optionsfelder in zwei Gruppen":::

### <a name="radio-button-states"></a>Optionsfeldzustände

Ein Optionsfeld hat zwei Zustände: „aktiviert“ und „deaktiviert“. Wenn ein Optionsfeld aktiviert ist, ist die [IsChecked-Eigenschaft](/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) auf `true` festgelegt. Wenn ein Optionsfeld deaktiviert ist, ist die `IsChecked`-Eigenschaft auf `false` festgelegt. Ein Optionsfeld kann deaktiviert werden, wenn ein Benutzer ein anderes Optionsfeld in derselben Gruppe auswählt, doch es kann nicht durch erneutes Auswählen deaktiviert werden. Sie können ein Optionsfeld jedoch programmgesteuert durch Festlegen der `IsChecked`-Eigenschaft auf `false` deaktivieren.

### <a name="visuals-to-consider"></a>Zu erwägende visuelle Elemente

Der Standardabstand der einzelnen `RadioButton`-Steuerelemente unterscheidet sich von dem durch eine `RadioButtons`-Gruppe bereitgestellten Abstand. Um den `RadioButtons`-Abstand auf einzelne `RadioButton`-Steuerelemente anzuwenden, verwenden Sie einen `Margin`-Wert von `0,0,7,3`, wie hier gezeigt.

```xaml
<StackPanel>
    <StackPanel.Resources>
        <Style TargetType="RadioButton">
            <Setter Property="Margin" Value="0,0,7,3"/>
        </Style>
    </StackPanel.Resources>
    <TextBlock Text="Background"/>
    <RadioButton Content="Item 1"/>
    <RadioButton Content="Item 2"/>
    <RadioButton Content="Item 3"/>
</StackPanel>
```

Die folgenden Abbildungen zeigen die bevorzugten Abstände der Optionsfelder in einer Gruppe.

:::image type="content" source="images/radiobutton-layout.png" alt-text="Bild mit einer Gruppe von Optionsfeldern, vertikal angeordnet.":::

:::image type="content" source="images/radiobutton-redline.png" alt-text="Bild mit angezeigten Abstandsführungslinien für Optionsfelder.":::

> [!NOTE]
> Wenn Sie ein WinUI-RadioButtons-Steuerelement verwenden, sind Abstand, Ränder und Ausrichtung bereits optimiert.

## <a name="recommendations"></a>Empfehlungen

- Stellen Sie sicher, dass der Zweck und der aktuelle Status einer Gruppe von Optionsfeldern explizit ist.
- Begrenzen Sie die Textbezeichnung des Optionsfelds auf eine einzelne Zeile.
- Wenn die Textbezeichnung dynamisch ist, bedenken Sie, wie sich die Größe des Optionsfelds automatisch ändert sowie die Auswirkungen auf umliegende visuelle Elemente.
- Verwenden Sie die standardmäßige Schriftart, es sei denn, Sie müssen gemäß Ihren Markenrichtlinien eine andere verwenden.
- Platzieren Sie keine zwei Optionsfeldgruppen (RadioButtons) nebeneinander. Wenn sich zwei Optionsfeldgruppen direkt nebeneinander befinden, kann es für Benutzer schwierig sein, festzustellen, welche Optionsfelder zu welcher Gruppe gehören.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- Informationen, wie Sie alle XAML-Steuerelemente in ein interaktives Format bekommen, finden Sie im [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery).

## <a name="related-topics"></a>Zugehörige Themen

- [Schaltflächen](buttons.md)
- [Umschalter](toggles.md)
- [Kontrollkästchen](checkbox.md)
- [Listen und Kombinationsfelder](lists.md)
- [Schieberegler](slider.md)
- [RadioButtons-Klasse](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)
- [RadioButton-Klasse](/uwp/api/windows.ui.xaml.controls.radiobutton)
