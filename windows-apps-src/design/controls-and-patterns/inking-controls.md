---
Description: Beschreibung der Freihandtools
title: Steuerelemente für Freihandeingaben
label: Inking Controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 97eae5f3-c16b-4aa5-b4a1-dd892cf32ead
ms.localizationpriority: medium
ms.openlocfilehash: 892e8e9bdeed562a83e566266a7391e9c24b2ad3
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081730"
---
# <a name="inking-controls"></a>Steuerelemente für Freihandeingaben



Es gibt zwei verschiedene Steuerelemente, die das Verknüpfen in UWP-Apps (universelle Windows-Plattform) erleichtern: [InkCanvas](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas) und [InkToolbar](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar).

Das InkCanvas-Steuerelement rendert Stifteingaben entweder als Freihandstrich (mit Standardeinstellungen für Farbe und Breite) oder als Radierstrich. Dieses Steuerelement ist eine transparente Überlagerung, die keine integrierte Benutzeroberfläche zum Ändern der Standardeigenschaften von Freihandstrichen enthält.

> [!NOTE]
> „InkCanvas“ kann zur Unterstützung ähnlicher Funktionen für Maus- und Toucheingabe konfiguriert werden.

Da das InkCanvas-Steuerelement keine Unterstützung für das Ändern der Standardeinstellungen von Freihandstrichen enthält, kann es mit einem InkToolbar-Steuerelement kombiniert werden. Das InkToolbar-Steuerelement enthält eine anpassbare und erweiterbare Sammlung von Schaltflächen, die Features für Freihandeingaben in einem verknüpften InkCanvas-Steuerelement aktivieren.

Das InkToolbar-Steuerelement enthält standardmäßig Schaltflächen zum Zeichnen, Löschen, Hervorheben sowie zum Anzeigen eines Lineals. Abhängig vom Feature werden in einem Flyout weitere Einstellungen und Befehle bereitgestellt, beispielsweise für Freihandfarbe, Strichstärke und das Löschen aller Freihandeingaben.

> [!NOTE]
> „InkToolbar“ unterstützt Stift- und Mauseingaben und kann zur Erkennung von Toucheingaben konfiguriert werden.

<img src="images/ink-tools-invoked-toolbar.png" width="300" alt="InkToolbar palette flyout">

> **Wichtige APIs:** [InkCanvas-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inkcanvas), [InkToolbar-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.inktoolbar), [InkPresenter-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter), [Windows.UI.Input.Inking](https://docs.microsoft.com/uwp/api/Windows.UI.Input.Inking)


## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie das InkCanvas-Steuerelement, wenn Sie in Ihrer App einfache Freihandfeatures ohne Freihandeinstellungen für den Benutzer bereitstellen müssen.

Standardmäßig werden Striche als Freihandeingabe gerendert, wenn die Stiftspitze (ein schwarzer Kugelschreiber mit einer Stärke von 2 Pixeln) verwendet wird, und als Radierer, wenn die Radiergummispitze verwendet wird. Falls keine Radiergummispitze vorhanden ist, kann das InkCanvas-Steuerelement so konfiguriert werden, dass Eingaben mit der Stiftspitze wie Radierstriche behandelt werden.

Kombinieren Sie das InkCanvas-Steuerelement mit einem InkToolbar-Steuerelement, um eine Benutzeroberfläche zum Aktivieren von Freihandfeatures und Festlegen grundlegender Freihandeigenschaften wie Strichgröße, Farbe und Form der Stiftspitze bereitzustellen.

> [!NOTE] 
> Verwenden Sie das zugrunde liegende [InkPresenter-Objekt](https://docs.microsoft.com/uwp/api/windows.ui.input.inking.inkpresenter), wenn Sie umfassendere Anpassungen am Rendering von Freihandeingaben für ein InkCanvas-Steuerelement vornehmen möchten.

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die <a href="xamlcontrolsgallery:/item/InkCanvas">App zu öffnen und InkCanvas in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

**Microsoft Edge**

Microsoft Edge verwendet „InkCanvas” und „InkToolbar” für **Webseitennotizen**.  
![Das InkCanvas-Steuerelement wird für Freihandeingaben in Microsoft Edge verwendet](images/ink-tools-edge.png)

**Windows Ink-Arbeitsbereich**

Die InkCanvas- und InkToolbar-Steuerelemente werden auch für den **Skizzenblock** und **Bildschirmskizzen** im **Windows Ink-Arbeitsbereich** verwendet.  
![InkToolbar-Steuerelement im Windows Ink-Arbeitsbereich](images/ink-tools-ink-workspace.png)

## <a name="create-an-inkcanvas-and-inktoolbar"></a>Erstellen eines InkCanvas- und InkToolbar-Steuerelements

Zum Hinzufügen eines InkCanvas-Steuerelements zu Ihrer App ist nur eine Markupzeile erforderlich:

```xaml
<InkCanvas x:Name="myInkCanvas"/>
```

> [!NOTE]
> Ausführliche Informationen zur Anpassung von „InkCanvas” mit „InkPresenter” finden Sie im Artikel [Zeichenstiftinteraktionen und Windows Ink in UWP-Apps](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions).

Das InkToolbar-Steuerelement muss in Verbindung mit einem InkCanvas-Steuerelement verwendet werden. Zum Einbinden eines InkToolbar-Steuerelements (mit allen integrierten Tools) in Ihre App ist nur eine zusätzliche Markupzeile erforderlich:

 ```xaml
<InkToolbar TargetInkCanvas="{x:Bind myInkCanvas}"/>
 ```

Dadurch wird das folgende InkToolbar-Steuerelement angezeigt:
<img src="images/ink-tools-uninvoked-toolbar.png" width="250" alt="Basic InkToolbar">

### <a name="built-in-buttons"></a>Integrierte Schaltflächen

Das InkToolbar-Steuerelement enthält die folgenden integrierten Schaltflächen:

**Stifte**

- Kugelschreiber – zeichnet mit einer runden Stiftspitze einen durchgehenden, undurchsichtigen Strich. Die Strichgröße hängt vom erkannten Stiftdruck ab.
- Bleistift – zeichnet mit einer runden Stiftspitze einen auslaufenden halbtransparenten Strich mit Textur (nützlich für überlagerte Schattierungseffekte). Die Strichfarbe (Dunkelheit) hängt vom erkannten Stiftdruck ab.
- Textmarker – zeichnet mit einer rechteckigen Stiftspitze einen halbtransparenten Strich.

Sie können sowohl die Farbpalette als auch die Größenattribute (Mindest-, Höchst- und Standardwerte) im Flyout für jeden Stift anpassen.

**Tool**

- Radierer – löscht alle Freihandstriche, die berührt werden. Beachten Sie, dass der gesamte Freihandstrich gelöscht wird, nicht nur der Teil unter dem Radiererstrich.

**Ein-/Ausschalten**

- Lineal – blendet das Lineal ein oder aus. Beim Zeichnen in der Nähe des Linealrands wird der Freihandstrich am Lineal ausgerichtet.  
 ![Dem InkToolbar-Steuerelement zugeordnetes visuelles Linealelement](images/inking-tools-ruler.png)

Obwohl dies die Standardkonfiguration ist, haben Sie die vollständige Kontrolle über die integrierten Schaltflächen, die im InkToolbar-Steuerelement für Ihre App enthalten sind.

### <a name="custom-buttons"></a>Benutzerdefinierte Schaltflächen

Das InkToolbar-Steuerelement besteht aus zwei unterschiedlichen Gruppen von Schaltflächentypen:

1. Eine Gruppe von „Toolschaltflächen“ mit den integrierten Schaltflächen zum Zeichnen, Radieren und Hervorheben. Hier werden benutzerdefinierte Stifte und Tools hinzugefügt.
> [!NOTE]
> Die ausgewählten Features schließen sich gegenseitig aus.

2. Eine Gruppe von „Umschaltflächen“ mit der integrierten Linealschaltfläche. Hier werden benutzerdefinierte Umschaltflächen hinzugefügt.
> [!NOTE]
> Die Features schließen sich nicht gegenseitig aus und können gleichzeitig mit anderen aktiven Tools verwendet werden.

Je nach Anwendung und erforderlicher Freihandfunktion können Sie dem InkToolbar-Steuerelement eine der folgenden Schaltflächen (die an die benutzerdefinierten Freihandfunktionen gebunden sind) hinzufügen:

- Benutzerdefinierter Stift: Ein Stift, für den die Farbpaletten- und Stiftspitzeneigenschaften der Freihandeingabe wie Form, Drehung und Größe von der Host-App definiert werden.
- Benutzerdefiniertes Tool: Ein Tool ohne Stift, das von der Host-App definiert wird.
- Benutzerdefiniertes Umschalten: Legt den Zustand eines durch die App definierten Features auf „aktiviert“ oder „deaktiviert“ fest. Wenn die Schaltfläche aktiviert ist, funktioniert das Feature in Verbindung mit dem aktiven Tool.

> [!NOTE]
> Die Anzeigereihenfolge der integrierten Schaltflächen kann nicht geändert werden. Die Standardreihenfolge in der Anzeige ist: Kugelschreiber, Stift, Textmarker, Radierer und Lineal. Benutzerdefinierte Stifte werden an den letzten Standardstift angefügt, benutzerdefinierte Tool-Schaltflächen werden zwischen der letzten Stiftschaltfläche und der Radiererschaltfläche hinzugefügt, und benutzerdefinierte Umschaltflächen werden nach der Linealschaltfläche hinzugefügt. (Benutzerdefinierte Schaltflächen werden in der Reihenfolge hinzugefügt, in der sie angegeben werden.)

Das InkToolbar-Steuerelement kann ein Element auf oberster Ebene sein, es wird jedoch in der Regel über eine Schaltfläche oder einen Befehl für die Freihandeingabe verfügbar gemacht. Wir empfehlen, die Glyphe EE56 aus der Schriftart „Segoe MLD2 Assets“ als Symbol auf oberster Ebene zu verwenden.

## <a name="inktoolbar-interaction"></a>InkToolbar-Interaktion

Alle integrierten Stift- und Toolschaltflächen enthalten ein Flyoutmenü, in dem Freihandeigenschaften sowie die Form und Größe der Stiftspitze festgelegt werden können. Eine „Erweiterungsglyphe” ![InkToolbar-Glyphe](images/ink-tools-glyph.png) wird auf der Schaltfläche angezeigt, um auf das Vorhandensein des Flyouts hinzuweisen.

Das Flyout wird angezeigt, wenn die Schaltfläche eines aktiven Tools erneut ausgewählt wird. Wenn die Farbe oder Größe geändert wird, wird das Flyout automatisch geschlossen, und die Freihandeingabe kann fortgesetzt werden. Für benutzerdefinierte Stifte und Tools kann das Standardflyoutmenü oder ein benutzerdefiniertes Flyoutmenü verwendet werden.

Der Radierer verfügt ebenfalls über ein Flyout mit dem Befehl **Freihand vollständig löschen**.  
![InkToolbar-Steuerelement mit aufgerufenem Radierer-Flyout](images/ink-tools-erase-all-ink.png)

 Informationen zur Anpassung und Erweiterbarkeit finden Sie im [einfachen Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk).

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Das InkCanvas-Steuerelement und die Freihandeingabe im Allgemeinen bieten bei der Verwendung eines aktiven Stifts die beste Benutzerfreundlichkeit. Es wird jedoch empfohlen, die Freihandeingabe mit Maus- und Toucheingabe (einschließlich des passiven Stifts) zu unterstützen, wenn dies für Ihre App erforderlich ist.
- Verwenden Sie ein InkToolbar-Steuerelement mit dem InkCanvas-Steuerelement, um grundlegende Freihandfeatures und -einstellungen bereitzustellen. Sowohl das InkCanvas- als auch das InkToolbar-Steuerelement können programmgesteuert angepasst werden.
- Das InkToolbar-Steuerelement und die Freihandeingabe im Allgemeinen bieten bei der Verwendung eines aktiven Stifts die beste Benutzerfreundlichkeit. Die Freihandeingabe mit Maus- und Toucheingabe kann aber unterstützt werden, wenn dies für Ihre App erforderlich ist.
- Bei Unterstützung der Freihandfunktion per Toucheingabe wird empfohlen, das Symbol „ED5F“ aus der Schriftart „Segoe MLD2 Assets“ für die Umschaltfläche mit einer QuickInfo „Schreiben durch Berühren“ zu verwenden.
- Für die Bereitstellung der Strichauswahl wird empfohlen, das Symbol „EF20“ aus der Schriftart „Segoe MLD2 Assets“ für die Toolschaltfläche mit einer QuickInfo „Auswahlwerkzeug“ zu verwenden.
- Wenn Sie mehrere InkCanvas-Steuerelemente verwenden, empfehlen wir die Verwendung eines einzelnen InkToolbar-Steuerelements zum Steuern der Freihandeingabe in allen Zeichenbereichen.
- Für eine optimale Leistung empfehlen wir, das Standardflyout zu ändern, anstatt ein benutzerdefiniertes Flyout für standardmäßige und benutzerdefinierte Tools zu erstellen.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- Im [einfachen Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/SimpleInk) werden acht Szenarien im Zusammenhang mit den Anpassungs- und Erweiterbarkeitsfunktionen der InkCanvas- und InkToolbar-Steuerelemente erläutert. Jedes Szenario bietet einen allgemeinen Überblick über gängige Situationen bei der Freihandeingabe und Steuerelementimplementierungen.
- In [Komplexes Freihandbeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ComplexInk) werden komplexere Szenarien für Freihandeingaben erläutert.
- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Stiftinteraktionen und Windows Ink in UWP-Apps](https://docs.microsoft.com/windows/uwp/design/input/pen-and-stylus-interactions)
- [Erkennen von Windows Ink-Strichen als Text und Formen](https://docs.microsoft.com/windows/uwp/design/input/convert-ink-to-text)
- [Speichern und Abrufen der Daten zu Windows Ink-Strichen](https://docs.microsoft.com/windows/uwp/design/input/save-and-load-ink)
