---
Description: Verwenden Sie visuelles Feedback Benutzern angezeigt, wenn deren Interaktionen mit einer UWP-app erkannt, interpretiert und verarbeitet werden.
title: Visuelles Feedback
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: Visuelles Feedback, Fokus-Feedback, Touch-Feedback, Kontaktvisualisierung, Eingabe, Interaktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 1f716b930a4d8b96a55a5741e5a520be624526fa
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66363541"
---
# <a name="guidelines-for-visual-feedback"></a>Richtlinien für visuelles Feedback

Zeigen Sie Benutzern durch visuelles Feedback, wenn ihre Interaktionen ermittelt, interpretiert und behandelt werden. Visuelles Feedback ist hilfreich für Benutzer und kann sie zur Interaktion ermutigen. Es weist auf erfolgreiche Interaktionen hin, was für den Benutzer das Gefühl der Kontrolle verstärkt. Darüber hinaus informiert es über den Systemstatus und verringert die Fehlerzahl.

> **Wichtige APIs:**  [**Windows.Devices.Input**](https://docs.microsoft.com/uwp/api/Windows.Devices.Input), [**Windows.UI.Input**](https://docs.microsoft.com/uwp/api/Windows.UI.Input), [**Windows.UI.Core**](https://docs.microsoft.com/uwp/api/Windows.UI.Core)

## <a name="recommendations"></a>Empfehlungen

- Versuchen Sie, Änderungen an einer Steuerelementvorlage auf ein Minimum zu beschränken, die direkt im Zusammenhang mit Ihrem Designziel stehen, da sich umfangreiche Änderungen auf die Leistung und Zugänglichkeit des Steuerelements und der Anwendung auswirken können. 
    - Unter [XAML-Formatvorlagen](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/xaml-styles) finden Sie weitere Informationen zum Anpassen der Eigenschaften eines Steuerelements, einschließlich der visuellen Zustandseigenschaften.
    - Unter [UserControl-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.usercontrol) finden Sie weitere Informationen zu Änderungen an einer Steuerelementvorlage.
    - Erwägen Sie, eine eigene benutzerdefinierte Steuerelementvorlage zu erstellen, wenn Sie umfangreiche Änderungen an einer Steuerelementvorlage vornehmen müssen. Ein Beispiel für eine benutzerdefinierte Steuerelementvorlage finden Sie unter [Beispiel für ein benutzerdefiniertes Bearbeitungssteuerelement](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl).
- Verwenden Sie keine Toucheingabevisualisierungen in Situationen, in denen diese die Verwendung der App beeinträchtigen könnten. Weitere Informationen finden Sie unter [**ShowGestureFeedback**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback).
- Zeigen Sie nur dann Feedback an, wenn dies absolut notwendig ist. Sorgen Sie dafür, dass die Benutzeroberfläche übersichtlich bleibt. Zeigen Sie nur dann visuelles Feedback an, wenn die darin enthaltenen Informationen sonst nirgends verfügbar sind.
- Versuchen Sie, die Verhaltensweisen der integrierten Windows-Gesten für visuelles Feedback nicht in erheblichem Umfang anzupassen, da dies eine inkonsistente und verwirrende Benutzerumgebung zur Folge haben kann.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung

Kontaktvisualisierungen sind besonders für Touchinteraktionen wichtig, bei denen Genauigkeit und Präzision gefragt sind. Ihre App sollte beispielsweise die Position, auf die getippt wurde, genau anzeigen, damit der Benutzer erkennen kann, ob er das Ziel verfehlt hat, wie weit er danebenliegt und welche Korrekturen erforderlich sind.

Durch die Verwendung der Standardsteuerelemente für die XAML-Plattform stellen Sie sicher, dass Ihre App auf allen Geräten und in allen Eingabesituationen ordnungsgemäß funktioniert. Wenn Ihre App benutzerdefinierte Interaktionen enthält, die angepasstes Feedback erfordern, müssen Sie sicherstellen, dass sich das Feedback für die jeweiligen Zwecke eignet, für alle unterstützten Eingabegeräte verfügbar ist und den Benutzer nicht von seiner eigentlichen Arbeit ablenkt. Dies kann insbesondere bei Spiel- oder Zeichnungs-Apps ein Problem sein, bei denen das visuelle Feedback mit wichtigen UI-Elementen kollidieren oder diese verdecken kann.

> [!Important]
> Das Interaktionsverhalten der integrierten Gesten sollte nicht geändert werden.

**Feedback auf Geräten**

Das visuelle Feedback ist im Allgemeinen vom Eingabegerät abhängig (Toucheingabe, Touchpad, Maus, Stift, Tastatur usw.). Das integrierte Feedback für die Maus z. B. beinhaltet normalerweise eine Bewegung und Änderung des Cursors, während für Touch- und Stifteingabe Berührungsvisualisierungen erforderlich sind und für die Eingabe und Navigation per Tastatur Fokusrechtecke und Hervorhebung verwendet werden.

Verwenden Sie [**ShowGestureFeedback**](https://docs.microsoft.com/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback), um das Feedbackverhalten für die Plattformgesten festzulegen.

Wenn Sie Anpassungen an der Feedback-UI vornehmen, muss das Feedback alle Eingabemodi unterstützen und für alle Modi geeignet sein.

Im Folgenden finden Sie einige Beispiele für integrierte Kontaktvisualisierungen in Windows.

| ![Touchfeedback](images/TouchFeedback.png) | ![Mausfeedback](images/MouseFeedback.png) | ![Stiftfeedback](images/PenFeedback.png) | ![Tastaturfeedback](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Touchvisualisierung | Maus-/Touchpadvisualisierung | Stiftvisualisierung | Tastaturvisualisierung |

## <a name="high-visibility-focus-visuals"></a>Visuelle Fokuselemente mit hoher Sichtbarkeit

Alle Windows-Apps zeigen ein stärker definiertes visuelles Fokuselement um interaktive Steuerelemente innerhalb der Anwendung herum. Diese neuen visuellen Fokuselemente können vollständig angepasst und auch gelöscht werden, wenn nötig.

Für das **10-Fuß-TV-Erlebnis**, das typisch für die Xbox und TV-Nutzung ist, unterstützt Windows **Einblendungen mit Fokus**, einen Lichteffekt, der den Rahmen fokussierbarer Elemente wie z. B. eine Schaltfläche animiert, wenn sie über Gamepads oder Tastatureingaben anfokussiert werden. Weitere Informationen finden Sie unter [Entwerfen für Xbox und Fernsehgeräte](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv#reveal-focus).

## <a name="color-branding--customizing"></a>Farbbranding und -anpassung

**Rahmeneigenschaften**

Es gibt zwei Elemente bei den visuellen Fokuselementen mit hoher Sichtbarkeit: der primäre und der sekundäre Rahmen. Der primäre Rahmen ist **2 Pixel** breit und verläuft an der *Außenseite* des sekundären Rahmens. Der sekundäre Rahmen ist **1 Pixel** breit und verläuft an der *Innenseite* des primären Rahmens.
![Hohe Transparenz Fokusindikator redlines](images/FocusRectRedlines.png)

Um die Breite der beiden Rahmentypen (primär oder sekundär) zu ändern, verwenden Sie **FocusVisualPrimaryThickness** bzw. **FocusVisualSecondaryThickness**:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Randbreiten visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusMargin.png)

Der Rand ist eine Eigenschaft des Typs [**Thickness**](https://docs.microsoft.com/dotnet/api/system.windows.thickness?redirectedfrom=MSDN) und kann daher so angepasst werden, dass er nur an bestimmten Seiten des Steuerelements angezeigt wird. Siehe unten: ![Hohe Transparenz Fokus visual Rand Stärke nur unten](images/FocusThicknessSide.png)

Der Rand ist der Abstand zwischen den visuellen Grenzen des Steuerelements und dem Beginn des *sekundären Rahmens* der visuellen Fokuselemente. Der standardmäßige Rand hat eine Breite von **1 Pixel** außerhalb der Grenzen des Steuerelements. Sie können diesen Rand pro Steuerelement bearbeiten, indem Sie die Eigenschaft **FocusVisualMargin** ändern:
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![Randunterschiede visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusPlusMinusMargin.png)

*Ein negativer Rand überträgt den Rahmen von der Mitte des Steuerelements, und ein positiver Rand wird näher den Rahmen verschieben, in der Mitte des Steuerelements.*

Um die visuellen Fokuselemente für ein Steuerelement vollständig zu deaktivieren, deaktivieren Sie einfach **UseSystemFocusVisuals**:
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

Die Breite, der Rand oder die vollständige Entfernung der visuellen Fokuselemente durch den App-Entwickler werden pro Steuerelement festgelegt.

**Farbformatierung**

Es gibt nur zwei Farbeigenschaften für die visuellen Fokuselemente; die primäre Rahmenfarbe und die sekundäre Rahmenfarbe. Diese Rahmenfarben für visuelle Fokuselemente können pro Steuerelement auf Seitenebene und global auf App-Ebene geändert werden:

Um App-weites Branding visueller Fokuselemente durchzuführen, überschreiben Sie die Systempinsel:
```XAML
<SolidColorBrush x:Key="SystemControlFocusVisualPrimaryBrush" Color="DarkRed"/>
<SolidColorBrush x:Key="SystemControlFocusVisualSecondaryBrush" Color="Pink"/>
```
![Änderungen der Farbe visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusRectColorChanges.png)

Um die Farben pro Steuerelement zu ändern, bearbeiten Sie einfach die Eigenschaften der visuellen Fokuselemente für das gewünschte Steuerelement:
```XAML
<Slider Width="200" FocusVisualPrimaryBrush="DarkRed" FocusVisualSecondaryBrush="Pink"/>
```

## <a name="related-articles"></a>Verwandte Artikel

**Für Designer**
* [Richtlinien für Verschiebung](guidelines-for-panning.md)

**Für Entwickler**
* [Benutzerdefinierte Benutzerinteraktionen](https://developer.microsoft.com/windows/design/inputs-devices)

**Beispiele**
* [Grundlegende Eingabebeispiel](https://go.microsoft.com/fwlink/p/?LinkID=620302)
* [Eingabebeispiel mit geringer Latenz](https://go.microsoft.com/fwlink/p/?LinkID=620304)
* [Beispiel für den Benutzerinteraktionsmodus](https://go.microsoft.com/fwlink/p/?LinkID=619894)
* [Beispiel für visuelle Fokuselemente](https://go.microsoft.com/fwlink/p/?LinkID=619895)

**Archivbeispiele**
* [Eingabe: XAML-benutzerbeispiel Eingabeereignisse](https://go.microsoft.com/fwlink/p/?linkid=226855)
* [Eingabe: Funktionen-gerätebeispiel](https://go.microsoft.com/fwlink/p/?linkid=231530)
* [Eingabe: Beispiel zu Leistungstests in Touch Treffer](https://go.microsoft.com/fwlink/p/?linkid=231590)
* [XAML Bildlauf, schwenken und Zoomen Beispiel](https://go.microsoft.com/fwlink/p/?linkid=251717)
* [Eingabe: Vereinfachte Freihand-Beispiel](https://go.microsoft.com/fwlink/p/?linkid=246570)
* [Eingabe: Beispiel für Windows 8-Gesten](https://go.microsoft.com/fwlink/p/?LinkId=264995)
* [Eingabe: Manipulationen und Beispiel für Bewegungen (C++)](https://go.microsoft.com/fwlink/p/?linkid=231605)
* [DirectX-Touch-Eingabe-Beispiel](https://go.microsoft.com/fwlink/p/?LinkID=231627)
 

 
