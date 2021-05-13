---
title: Microsoft PowerToys
description: Microsoft PowerToys ist eine Reihe von Hilfsprogrammen für die Anpassung von Windows 10. Zu den Hilfsprogrammen gehören der Farbwähler (klicken Sie auf eine beliebige Stelle, um einen Farbwert zu übernehmen), FancyZones (Tastenkombinationen zur Positionierung von Fenstern in einem Rasterlayout), Datei-Explorer-Add-Ons (Vorschau von SVG- oder Markdown-Dateien), Bildgrößenänderung (Ändern der Größe eines oder mehrerer Bilder mit einem einfachen Rechtsklick), Tastatur-Manager (Umbelegung von Tasten oder Erstellen eigener Tastenkombinationen), PowerRename (massenhaftes Umbenennen mithilfe von Suchen und Ersetzen), PowerToys Run (Alt + Leertaste zum Starten von Anwendungen), eine Tastenkombinationsübersicht und vieles mehr.
ms.date: 12/02/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 5e7e88e8ff179ebbb63aa7369c22149b645c9838
ms.sourcegitcommit: afc4ff2c89f148d32073ab1cc42063ccdc573a8c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/11/2021
ms.locfileid: "98104581"
---
# <a name="microsoft-powertoys-utilities-to-customize-windows-10"></a>Microsoft PowerToys: Hilfsprogramme zur Anpassung von Windows 10

Microsoft PowerToys ist eine Sammlung von Hilfsprogrammen für Poweruser, mit der die Windows 10-Benutzeroberfläche zur Steigerung der Produktivität angepasst und optimiert werden kann.

> [!div class="nextstepaction"]
> [Installieren von PowerToys](install.md)

## <a name="processor-support"></a>Prozessorunterstützung

- **x64**: Unterstützt
- **x86**: In der Entwicklung (siehe [Problem #602](https://github.com/microsoft/PowerToys/issues/602))
- **ARM**: In der Entwicklung (siehe [Problem #490](https://github.com/microsoft/PowerToys/issues/490))

## <a name="current-powertoy-utilities"></a>Aktuelle PowerToy-Hilfsprogramme

Zu den derzeit verfügbaren Hilfsprogrammen gehören:

### <a name="color-picker"></a>Farbauswahl

:::row:::
    :::column:::
        [![Screenshot des Farbwählers](../images/pt-color-picker.png)](color-picker.md)
    :::column-end:::
    :::column span="2":::
        [Farbwähler](color-picker.md) ist ein systemweites Hilfsprogramm zur Farbauswahl, das mit <kbd>Win</kbd>+<kbd>Umschalt</kbd>+<kbd>C</kbd> aktiviert wird. Wählen Sie Farben aus einer beliebigen aktuell laufenden Anwendung aus. Der Farbwähler kopiert die Farbe automatisch in einem konfigurierbaren Format in die Zwischenablage. Farbwähler enthält darüber hinaus einen Editor, der einen Verlauf der zuvor ausgewählten Farben anzeigt und Ihnen die Optimierung der ausgewählten Farbe sowie das Kopieren verschiedener Zeichenfolgendarstellungen erlaubt. Dieser Code basiert auf dem [Color Picker von Martin Chrzan](https://github.com/martinchrzan/ColorPicker).
    :::column-end:::
:::row-end:::

### <a name="fancy-zones"></a>Fancy Zones

:::row:::
    :::column:::
        [![Screenshot von FancyZones](../images/pt-fancy-zones.png)](fancyzones.md)
    :::column-end:::
    :::column span="2":::
        [FancyZones](fancyzones.md) ist ein Fenster-Manager, der das Erstellen komplexer Fensterlayouts und das schnelle Positionieren von Fenstern in diesen Layouts einfach macht.
    :::column-end:::
:::row-end:::

### <a name="file-explorer-add-ons"></a>Datei-Explorer-Add-Ons

:::row:::
    :::column:::
        [![Screenshot von Datei-Explorer](../images/pt-file-explorer.png)](file-explorer.md)
    :::column-end:::
    :::column span="2":::
        [Datei-Explorer](file-explorer.md)-Add-Ons ermöglichen es dem Rendering des Vorschaubereichs im Datei-Explorer, SVG-Symboldateien (SVG) und Markdowndateien (MD) in der Vorschau anzuzeigen. Wählen Sie zum Aktivieren des Vorschaubereichs die Registerkarte „Ansicht“ im Datei-Explorer und dann „Vorschaubereich“ aus.
    :::column-end:::
:::row-end:::

### <a name="image-resizer"></a>Bildgrößenänderung

:::row:::
    :::column:::
        [![Screenshot der Bildgrößenänderung](../images/pt-image-resizer.png)](image-resizer.md)
    :::column-end:::
    :::column span="2":::
        Die [Bildgrößenänderung](image-resizer.md) ist eine Windows-Shellerweiterung für die schnelle Größenänderung von Bildern.  Ändern Sie mit einem einfachen Rechtsklick im Datei-Explorer sofort die Größe eines oder mehrerer Bilder. Dieser Code basiert auf dem [Image Resizer von Brice Lambson](https://github.com/bricelam/ImageResizer).
    :::column-end:::
:::row-end:::

### <a name="keyboard-manager"></a>Tastatur-Manager

:::row:::
    :::column:::
        [![Screenshot von Tastatur-Manager](../images/pt-keyboard-manager.png)](keyboard-manager.md)
    :::column-end:::
    :::column span="2":::
        Mit dem [Tastatur-Manager](keyboard-manager.md) können Sie die Tastatur so anpassen, dass sie produktiver wird, indem Sie die Tasten neu belegen und eigene Tastenkombinationen erstellen. Für dieses PowerToy ist Windows 10 1903 (Build 18362) oder höher erforderlich.
    :::column-end:::
:::row-end:::

### <a name="powerrename"></a>PowerRename

:::row:::
    :::column:::
        [![Screenshot von PowerRename](../images/pt-rename.png)](powerrename.md)
    :::column-end:::
    :::column span="2":::
        [PowerRename](powerrename.md) ermöglicht Ihnen das Umbenennen, Suchen und Ersetzen von Dateinamen als Massenvorgang. Es enthält erweiterte Funktionen, wie die Verwendung regulärer Ausdrücke, das Festlegen bestimmter Dateitypen als Ziel, die Vorschau der erwarteten Ergebnisse und die Möglichkeit, Änderungen rückgängig zu machen. Dieser Code basiert auf [SmartRename von Chris Davis](https://github.com/chrdavis/SmartRename).
    :::column-end:::
:::row-end:::

### <a name="powertoys-run"></a>PowerToys-Startprogramm

:::row:::
    :::column:::
        [![Screenshot des PowerToys-Startprogramms](../images/pt-run.png)](run.md)
    :::column-end:::
    :::column span="2":::
        Mithilfe des [PowerToys-Startprogramms](run.md) können Sie Ihre App im Handumdrehen suchen und starten – geben Sie einfach die Tastenkombination <kbd>Alt</kbd>+<kbd>Leertaste</kbd> ein, und beginnen Sie mit der Eingabe. Das Programm ist Open Source und für weitere Plug-Ins modular ausgelegt. Die Fensternavigation ist jetzt ebenfalls enthalten. Für dieses PowerToy ist Windows 10 1903 (Build 18362) oder höher erforderlich.
    :::column-end:::
:::row-end:::

### <a name="shortcut-guide"></a>Tastenkombinationsübersicht

:::row:::
    :::column:::
        [![Screenshot der Tastenkombinationsübersicht](../images/pt-shortcut-guide.png)](shortcut-guide.md)
    :::column-end:::
    :::column span="2":::
        Die [Windows-Tastenkombinationsübersicht](shortcut-guide.md) wird angezeigt, wenn ein Benutzer die Windows-Taste länger als eine Sekunde gedrückt hält, und zeigt die verfügbaren Tastenkombinationen für den aktuellen Zustand des Desktops an.
    :::column-end:::
:::row-end:::

## <a name="powertoys-video-walk-through"></a>Exemplarische Vorgehensweise zu PowerToys im Video

In diesem Video zeigt Clint Rutkas (Projektleiter von PowerToys), wie man die verschiedenen verfügbaren Hilfsprogramme installiert und verwendet. Außerdem gibt er einige Tipps, Informationen zum Mitmachen und mehr.

> [!VIDEO https://channel9.msdn.com/Shows/Tabs-vs-Spaces/PowerToys-Utilities-to-customize-Windows-10/player?format=ny]

## <a name="future-powertoy-utilities"></a>Kommende PowerToy-Hilfsprogramme

### <a name="experimental-powertoys"></a>Experimentelle PowerToys

Installieren Sie die Vorabversion der experimentellen Version von PowerToys, um die neuesten experimentellen Hilfsprogramme zu testen, darunter:

#### <a name="video-conference-mute-experimental"></a>Video Conference Mute (experimentell)

:::row:::
    :::column:::
        [![Screenshot von Video Conference Mute](../images/pt-video-conference-mute.png)](video-conference-mute.md)
    :::column-end:::
    :::column span="2":::
        [Video Conference Mute](video-conference-mute.md) ist eine Möglichkeit, sowohl Ihr Mikrofon als auch Ihre Kamera schnell „stumm“ zu schalten. Sie verwenden dazu <kbd>⊞ Win</kbd>+<kbd>N</kbd> während einer laufenden Telefonkonferenz, unabhängig von der Anwendung, die aktuell den Fokus hat. Dieses Hilfsprogramm ist nur in der [Vorabversion/experimentellen Version von PowerToys](https://github.com/microsoft/PowerToys/releases/) enthalten. Es erfordert Windows 10 1903 (Build 18362) oder höher.
    :::column-end:::
:::row-end:::

## <a name="known-issues"></a>Bekannte Probleme

Auf der Registerkarte [Issues](https://github.com/microsoft/PowerToys/issues) des PowerToys-Repositorys auf GitHub können Sie die bekannten Probleme durchsuchen oder ein neues Problem melden.

## <a name="contribute-to-powertoys-open-source"></a>Mitwirkung an PowerToys (Open Source)

PowerToys freut sich auf Ihre Beiträge! Das PowerToys-Entwicklungsteam freut sich, die Poweruser-Community als Partner zu gewinnen, um Tools zu entwickeln, die Benutzern die optimale Nutzung von Windows ermöglichen. Es gibt verschiedene Formen der Mitwirkung:

- Erstellen einer [technischen Spezifikation](https://codeburst.io/on-writing-tech-specs-6404c9791159)
- Einreichen eines [Entwurfskonzepts oder einer Empfehlung](https://www.microsoft.com/design/inclusive/)
- [Mitarbeit an der Dokumentation](/contribute/)
- Identifizieren und Beheben von Bugs im [Quellcode](https://github.com/microsoft/PowerToys/tree/master/src)
- [Programmieren neuer Features und PowerToy-Hilfsprogramme](https://github.com/microsoft/PowerToys/tree/master/doc/devdocs)

Bevor Sie mit der Arbeit an einem Feature beginnen, das Sie beitragen möchten, **lesen Sie den [Leitfaden für Mitwirkende](https://github.com/microsoft/PowerToys/blob/master/CONTRIBUTING.md)** . Das PowerToys-Team arbeitet gerne mit Ihnen zusammen, um den besten Ansatz herauszufinden, Sie bei der Entwicklung von Funktionen zu unterstützen und zu helfen, verschwendete oder doppelte Arbeit zu vermeiden.

## <a name="powertoys-release-notes"></a>Versionshinweise für PowerToys

Die PowerToys-[Versionshinweise](https://github.com/microsoft/PowerToys/releases/) sind auf der Installationsseite des GitHub-Repositorys aufgelistet. Im PowerToys-Wiki finden Sie darüber hinaus die [Versions-Checkliste](https://github.com/microsoft/PowerToys/wiki/Release-check-list) als Referenz.

## <a name="powertoys-history"></a>PowerToys-Geschichte

Diese Neuauflage wurde durch das [PowerToys-Projekt aus der Windows 95-Ära](https://en.wikipedia.org/wiki/Microsoft_PowerToys) inspiriert und bietet Powerusern die Möglichkeit, die Windows 10-Shell effizienter zu nutzen und sie für einzelne Workflows anzupassen.  Eine gute Übersicht der Windows 95-PowerToys finden Sie [hier](https://socket3.wordpress.com/2016/10/22/using-windows-95-powertoys/).

## <a name="powertoys-roadmap"></a>PowerToys-Roadmap

PowerToys ist ein Rapid-Incubation Open Source-Team mit dem Ziel, Powerusern die Möglichkeit zu geben, die Windows 10-Shell effizienter zu nutzen und sie für einzelne Workflows anzupassen. Die Arbeitsprioritäten werden konsistent überprüft, neu bewertet und angepasst, um die Produktivität unserer Benutzer zu verbessern.

- [Neue Spezifikationen für mögliche PowerToys](https://github.com/microsoft/PowerToys/wiki/Specs)
- [Backlog-Prioritätsliste](https://github.com/microsoft/PowerToys/wiki/Roadmap#backlog-priority-list-in-order)
- [Version 1.0-Strategiespezifikation](https://github.com/microsoft/PowerToys/wiki/Version-1.0-Strategy), Februar 2020
