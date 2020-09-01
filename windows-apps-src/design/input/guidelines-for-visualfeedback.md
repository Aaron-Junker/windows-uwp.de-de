---
Description: Verwenden Sie visuelles Feedback, um Benutzer anzuzeigen, wenn ihre Interaktionen mit einer Windows-App erkannt, interpretiert und behandelt werden.
title: Visuelles Feedback
ms.assetid: bf2f3672-95f0-4c8c-9a72-0934f2d3b767
label: Visual feedback
template: detail.hbs
keywords: Visuelles Feedback, Fokus-Feedback, Touch-Feedback, Kontaktvisualisierung, Eingabe, Interaktion
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9ced83ca771f4954f8e42dc42e0882d1a5b7c6b1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172454"
---
# <a name="guidelines-for-visual-feedback"></a>Richtlinien für visuelles Feedback

Zeigen Sie Benutzern durch visuelles Feedback, wenn ihre Interaktionen ermittelt, interpretiert und behandelt werden. Visuelles Feedback ist hilfreich für Benutzer und kann sie zur Interaktion ermutigen. Es weist auf erfolgreiche Interaktionen hin, was für den Benutzer das Gefühl der Kontrolle verstärkt. Darüber hinaus informiert es über den Systemstatus und verringert die Fehlerzahl.

> **Wichtige APIs**:  [**Windows. Devices. Input**](/uwp/api/Windows.Devices.Input), [**Windows. UI. Input**](/uwp/api/Windows.UI.Input), [**Windows. UI. Core**](/uwp/api/Windows.UI.Core)

## <a name="recommendations"></a>Empfehlungen

- Versuchen Sie, die Änderungen an einer Steuerelement Vorlage auf jene zu beschränken, die sich direkt auf Ihre Entwurfs Absicht beziehen, da umfangreiche Änderungen sich auf die Leistung und den Zugriff sowohl auf die Leistung als auch auf die Anwendung auswirken können. 
    - Weitere Informationen zum Anpassen der Eigenschaften eines Steuer Elements, einschließlich der visuellen Zustands Eigenschaften, finden Sie unter [XAML-Stile](../controls-and-patterns/xaml-styles.md) .
    - Ausführliche Informationen zum vornehmen von Änderungen an einer Steuerelement Vorlage finden Sie in der [UserControl-Klasse](/uwp/api/windows.ui.xaml.controls.usercontrol) .
    - Es empfiehlt sich, ein eigenes benutzerdefiniertes Steuerelement zu erstellen, wenn Sie wesentliche Änderungen an einer Steuerelement Vorlage vornehmen müssen. Ein Beispiel für ein benutzerdefiniertes Steuerelement mit Vorlagen finden Sie unter Beispiel für [benutzerdefiniertes Bearbeitungs Steuer](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)Element.
- Verwenden Sie keine Toucheingabevisualisierungen in Situationen, in denen diese die Verwendung der App beeinträchtigen könnten. Weitere Informationen finden Sie unter [**ShowGestureFeedback**](/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback).
- Zeigen Sie nur dann Feedback an, wenn dies absolut notwendig ist. Sorgen Sie dafür, dass die Benutzeroberfläche übersichtlich bleibt. Zeigen Sie nur dann visuelles Feedback an, wenn die darin enthaltenen Informationen sonst nirgends verfügbar sind.
- Versuchen Sie, die Verhaltensweisen der integrierten Windows-Gesten für visuelles Feedback nicht in erheblichem Umfang anzupassen, da dies eine inkonsistente und verwirrende Benutzerumgebung zur Folge haben kann.

## <a name="additional-usage-guidance"></a>Weitere Hinweise zur Verwendung

Kontaktvisualisierungen sind besonders für Touchinteraktionen wichtig, bei denen Genauigkeit und Präzision gefragt sind. Ihre App sollte beispielsweise die Position, auf die getippt wurde, genau anzeigen, damit der Benutzer erkennen kann, ob er das Ziel verfehlt hat, wie weit er danebenliegt und welche Korrekturen erforderlich sind.

Durch die Verwendung der Standardsteuerelemente für die XAML-Plattform stellen Sie sicher, dass Ihre App auf allen Geräten und in allen Eingabesituationen ordnungsgemäß funktioniert. Wenn Ihre App benutzerdefinierte Interaktionen enthält, die angepasstes Feedback erfordern, müssen Sie sicherstellen, dass sich das Feedback für die jeweiligen Zwecke eignet, für alle unterstützten Eingabegeräte verfügbar ist und den Benutzer nicht von seiner eigentlichen Arbeit ablenkt. Dies kann insbesondere bei Spiel- oder Zeichnungs-Apps ein Problem sein, bei denen das visuelle Feedback mit wichtigen UI-Elementen kollidieren oder diese verdecken kann.

> [!Important]
> Das Interaktionsverhalten der integrierten Gesten sollte nicht geändert werden.

**Feedback auf allen Geräten**

Das visuelle Feedback ist im Allgemeinen vom Eingabegerät abhängig (Toucheingabe, Touchpad, Maus, Stift, Tastatur usw.). Das integrierte Feedback für die Maus z. B. beinhaltet normalerweise eine Bewegung und Änderung des Cursors, während für Touch- und Stifteingabe Berührungsvisualisierungen erforderlich sind und für die Eingabe und Navigation per Tastatur Fokusrechtecke und Hervorhebung verwendet werden.

Verwenden Sie [**ShowGestureFeedback**](/uwp/api/windows.ui.input.gesturerecognizer.showgesturefeedback), um das Feedbackverhalten für die Plattformgesten festzulegen.

Wenn Sie Anpassungen an der Feedback-UI vornehmen, muss das Feedback alle Eingabemodi unterstützen und für alle Modi geeignet sein.

Im Folgenden finden Sie einige Beispiele für integrierte Kontaktvisualisierungen in Windows.

| ![Touchfeedback](images/TouchFeedback.png) | ![Mausfeedback](images/MouseFeedback.png) | ![Stiftfeedback](images/PenFeedback.png) | ![Tastaturfeedback](images/KeyboardFeedback.png) |
| --- | --- | --- | --- |
| Touchvisualisierung | Maus-/Touchpadvisualisierung | Stiftvisualisierung | Tastaturvisualisierung |

## <a name="high-visibility-focus-visuals"></a>Visuelle Fokuselemente mit hoher Sichtbarkeit

Alle Windows-Apps zeigen ein stärker definiertes visuelles Fokuselement um interaktive Steuerelemente innerhalb der Anwendung herum. Diese neuen visuellen Fokuselemente können vollständig angepasst und auch gelöscht werden, wenn nötig.

Für die **10-Fuß-** Darstellung, die typisch für die Verwendung von Xbox und TV ist, unterstützt Windows den **Fokus**, einen Beleuchtungs Effekt, der den Rahmen von Fokus baren Elementen animiert, wie z. b. eine Schaltfläche, wenn Sie den Fokus über Gamepad oder Tastatureingaben erhalten. Weitere Informationen finden Sie unter [Entwerfen für Xbox und TV](../devices/designing-for-tv.md#reveal-focus).

## <a name="color-branding--customizing"></a>Farbbranding und -anpassung

### <a name="border-properties"></a>Rahmeneigenschaften

Es gibt zwei Elemente bei den visuellen Fokuselementen mit hoher Sichtbarkeit: der primäre und der sekundäre Rahmen. Der primäre Rahmen ist **2 Pixel** breit und verläuft an der *Außenseite* des sekundären Rahmens. Der sekundäre Rahmen ist **1 Pixel** breit und verläuft an der *Innenseite* des primären Rahmens.
![Redlines visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusRectRedlines.png)

Um die Breite der beiden Rahmentypen (primär oder sekundär) zu ändern, verwenden Sie **FocusVisualPrimaryThickness** bzw. **FocusVisualSecondaryThickness**:
```XAML
<Slider Width="200" FocusVisualPrimaryThickness="5" FocusVisualSecondaryThickness="2"/>
```
![Randbreiten visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusMargin.png)

Der Rand ist eine Eigenschaft des Typs [**Thickness**](/dotnet/api/system.windows.thickness) und kann daher so angepasst werden, dass er nur an bestimmten Seiten des Steuerelements angezeigt wird. Siehe unten: ![ hohe Sichtbarkeit Fokus visuelle Rand Stärke nur unten](images/FocusThicknessSide.png)

Der Rand ist das Leerzeichen zwischen den visuellen Begrenzungen des Steuer Elements und dem Anfang des *sekundären*Rahmens der Fokus Visuals. Der Standard Rand ist **1 px** von den Steuerelement Grenzen entfernt. Sie können diesen Rand auf der Basis der einzelnen Steuerelemente bearbeiten, indem Sie die **focevisualmargin** -Eigenschaft ändern:
```XAML
<Slider Width="200" FocusVisualMargin="-5"/>
```
![Randunterschiede visueller Fokuselemente mit hoher Sichtbarkeit](images/FocusPlusMinusMargin.png)

*Ein negativer Rand verschiebt den Rahmen weiter weg von der Mitte des Steuerelements. Ein positiver Rand verschiebt den Rahmen näher zur Mitte des Steuerelements.*

Um die visuellen Fokuselemente für ein Steuerelement vollständig zu deaktivieren, deaktivieren Sie einfach **UseSystemFocusVisuals**:
```XAML
<Slider Width="200" UseSystemFocusVisuals="False"/>
```

Die Breite, der Rand oder die vollständige Entfernung der visuellen Fokuselemente durch den App-Entwickler werden pro Steuerelement festgelegt.

### <a name="color-properties"></a>Farbeigenschaften

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

### <a name="for-designers"></a>Für Designer

- [Richtlinien für Verschiebung](guidelines-for-panning.md)

### <a name="for-developers"></a>Für Entwickler

- [Benutzerdefinierte Benutzerinteraktionen](../layout/index.md)

### <a name="samples"></a>Proben

- [Einfaches Eingabebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BasicInput)
- [Eingabebeispiel mit geringer Latenz](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/LowLatencyInput)
- [Beispiel für den Benutzerinteraktionsmodus](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/UserInteractionMode)
- [Beispiel für visuelle Fokuselemente](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlFocusVisuals)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für XAML-Benutzereingabeereignisse](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20XAML%20user%20input%20events%20sample)
- [Eingabe: Beispiel für Gerätefunktionen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%23%5D-Windows%208%20app%20samples/C%23/Windows%208%20app%20samples/Input%20Device%20capabilities%20sample%20(Windows%208))
- [Eingabe: Beispiel für Fingereingabe-Treffertests](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20desktop%20samples/%5BC%2B%2B%5D-Windows%208%20desktop%20samples/C%2B%2B/Windows%208%20desktop%20samples/Input%20Touch%20hit%20testing%20sample)
- [Beispiel für XAML-scrollen, Schwenken und Zoomen](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Universal%20Windows%20app%20samples/111487-Universal%20Windows%20app%20samples/XAML%20scrolling%2C%20panning%2C%20and%20zooming%20sample)
- [Eingabe: vereinfachtes Freihandbeispiel](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Simplified%20ink%20sample)
- [Eingabe: Beispiel für Windows 8-Bewegungen](/samples/browse/?redirectedfrom=MSDN-samples)
- [Eingabe: Manipulationen und Gesten (Beispiel)](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Input%20Gestures%20and%20manipulations%20with%20GestureRecognizer)
- [Beispiel für die DirectX-Fingereingabe](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/DirectX%20touch%20input%20sample%20(Windows%208))
 

 