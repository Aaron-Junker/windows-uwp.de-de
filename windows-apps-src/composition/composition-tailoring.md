---
title: Komposition-Anpassung
description: Verwenden Sie die Kompositions-APIs, um Ihre Benutzeroberfläche anzupassen, die Leistung zu optimieren und die Benutzereinstellungen und Gerätemerkmale zu erfüllen.
ms.date: 07/16/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 3d4aa82f70e9bad7a60a97b6b28f28f3dfd008c9
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166404"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Anpassen von Effekten & Erfahrungen mithilfe der Windows-Benutzeroberfläche

Die Windows-Benutzeroberfläche bietet viele schöne Effekte, Animationen und Mittel für die Unterscheidung. Es ist jedoch immer noch ein erforderlicher Bestandteil der Erstellung erfolgreicher Anwendungen, die Benutzer Erwartungen an Leistung und Anpassbarkeit zu erfüllen. Der universelle Windows-Plattform unterstützt eine große, unterschiedlichste Gerätefamilie mit unterschiedlichen Features und Funktionen. Um für alle Benutzer ein inklusives Verhalten bereitzustellen, müssen Sie sicherstellen, dass Ihre Anwendungen Geräte übergreifend skaliert werden und die Benutzereinstellungen beachtet werden. Die Anpassung der Benutzeroberfläche bietet eine effiziente Möglichkeit, um die Funktionen eines Geräts zu nutzen und eine angenehme und inklusivbenutzer Oberfläche sicherzustellen.

Die Benutzeroberflächen Anpassung ist eine umfassende Kategorie, die die Arbeit für leistungsstarke, schöne Benutzeroberflächen in Bezug auf die folgenden Bereiche umfasst:

- Berücksichtigen und Anpassen von Benutzereinstellungen für Effekte
- Benutzereinstellungen für Animationen werden untersucht
- Optimieren der Benutzeroberfläche für die angegebenen Hardwarefunktionen

Hier erfahren Sie, wie Sie Ihre Effekte und Animationen in den oben aufgeführten Bereichen mit der visuellen Schicht anpassen, aber es gibt noch viele weitere Möglichkeiten, Ihre Anwendung anzupassen, um eine gute Benutzerumgebung zu gewährleisten. Leitfäden finden Sie unter so [passen Sie die Benutzeroberfläche](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) für verschiedene Geräte an und [Erstellen reaktionsfähige Benutzer](../design/layout/responsive-design.md)Oberflächen.

## <a name="user-effects-settings"></a>Benutzer Effekteinstellungen

Benutzer können Ihre Windows-Darstellung aus einer Vielzahl von Gründen anpassen, die von Anwendungen beachtet und angepasst werden müssen. Ein Bereich, der von Endbenutzern gesteuert werden kann, ist die Änderung der Art von Effekten, die im gesamten System angezeigt werden.

### <a name="transparency-effects-settings"></a>Einstellungen für Transparenzeffekte

Eine solche Effekteinstellung, die Benutzer anpassen können, ist das Aktivieren/Deaktivieren von Transparenz Effekten. Diese finden Sie in der App "Einstellungen" unter Personalization > Colors, oder über die app "Einstellungen" > vereinfachen des Zugriffs > Anzeige.

![Transparenz Option in "Einstellungen"](images/tailoring-transparency-setting.png)

Wenn diese aktiviert ist, werden alle Effekte, die Transparenz verwenden, erwartungsgemäß angezeigt. Dies gilt für "Acryl", "hostbackdropbrush" oder ein beliebiges, nicht vollständig undurchsichtiges Diagramm.

Wenn Sie ausgeschaltet ist, greift das Acrylmaterial automatisch auf eine voll Tonfarbe zurück, da der Acryl Pinsel von XAML standardmäßig auf dieses Ereignis lauscht. Hier sehen wir, dass die Rechner-APP entsprechend auf eine voll Tonfarbe zurückgreift, wenn Transparenzeffekte nicht aktiviert sind:

![Rechner mit einem Acryl ](images/tailoring-acrylic.png)
 ![ Rechner mit Acryl, der auf Transparenz Einstellungen reagiert](images/tailoring-acrylic-fallback.png)

Allerdings muss die Anwendung für benutzerdefinierte Effekte auf die [uisettings. advancedeffectsenabled](/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabled) -Eigenschaft oder das [advancedeffectsenabledchanged](/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) -Ereignis reagieren und das Effect/Effect-Diagramm auslagern, um einen Effekt zu verwenden, der keine Transparenz aufweist. Ein Beispiel hierfür finden Sie unten:

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool advancedEffects = uisettings.AdvancedEffectsEnabled;
   uisettings.AdvancedEffectsEnabledChanged += Uisettings_AdvancedEffectsEnabledChanged;
}

private void Uisettings_AdvancedEffectsEnabledChanged(UISettings sender, object args)
{
    // TODO respond to sender.AdvancedEffectsEnabled
}
```

## <a name="animations-settings"></a>Animations Einstellungen

Ebenso sollten Anwendungen die [uisettings. animationsenabled](/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) -Eigenschaft überwachen und darauf reagieren, um sicherzustellen, dass Animationen basierend auf den Benutzereinstellungen in den Einstellungen > einfache Zugriffs > angezeigt werden.

![Animations Option in "Einstellungen"](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Nutzen der Funktionen-API

Durch die Nutzung der APIs für die [compositionperformance](/uwp/api/windows.ui.composition.compositioncapabilities) können Sie erkennen, welche Kompositions Features auf der jeweiligen Hardware verfügbar und leistungsfähig sind, und den Entwurf anpassen, um sicherzustellen, dass Endbenutzer auf jedem Gerät eine leistungsfähige und schöne Benutzer Funktionalität erhalten. Mithilfe der APIs können Sie die Hardwaresystem Funktionen überprüfen, um die Skalierung von ordnungsgemäßen Effekten über eine Vielzahl von Formfaktoren hinweg zu implementieren. Dies erleichtert es, die Anwendung entsprechend anzupassen, um eine schöne und nahtlose Endbenutzer Funktion zu erstellen.

Diese API stellt Methoden und einen Ereignislistener bereit, die verwendet werden können, um die Skalierung von Entscheidungen für die Benutzeroberfläche der Anwendung Die Funktion erkennt, wie gut das System komplexe Kompositions-und Renderingvorgänge verarbeiten kann, und gibt dann die Informationen in einem leicht verwendbaren Modell zurück, das von Entwicklern verwendet werden kann.

### <a name="using-composition-capabilities"></a>Verwenden von Kompositions Funktionen

Die Funktion "compositionfunktionen" wird bereits für Features wie das Acrylmaterial genutzt, wobei die Materialien je nach Szenario und Hardware auf einen leistungsfähigere Effekt zurückgreifen.

Die API kann in wenigen einfachen Schritten zu vorhandenem Code hinzugefügt werden.

1. Erwerben Sie das Funktionen-Objekt im Konstruktor Ihrer Anwendung.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registrieren Sie einen Ereignislistener für Ihre APP.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Fügen Sie der Ereignis Rückruf Methode Inhalt hinzu, um verschiedene Funktionsebenen zu verarbeiten. Dies entspricht möglicherweise dem nächsten nachfolgenden Schritt.
1. Wenn Sie Effekte verwenden, überprüfen Sie zuerst das Objekt "Funktionen". Verwenden Sie ggf. bedingte Überprüfungen oder Schalter Steuerungs Anweisungen, je nachdem, wie Sie die Effekte anpassen möchten.

    ```cs
    if (_capabilities.AreEffectsSupported())
    {
        // Add incremental effects updates here

        if (_capabilities.AreEffectsFast())
        {
            // Add more advanced effects here where applicable
        }
    }
    ```

Vollständigen Beispielcode finden Sie im [GitHub](https://github.com/microsoft/WindowsCompositionSamples/tree/master/SampleGallery/Samples/SDK 15063/CompCapabilities)-Repository der Windows-Benutzeroberfläche.

## <a name="fast-vs-slow-effects"></a>Schnelle im Vergleich zu langsamen Effekten

Basierend auf dem Feedback der bereitgestellten [areeffectysupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) -Methode und der [areeffecungfast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) -Methode in der compositionmethods-API kann die Anwendung sich entscheiden, teure oder nicht unterstützte Auswirkungen auf andere Effekte ihrer Wahl auszutauschen, die für das Gerät optimiert sind. Einige Effekte sind so bekannt, dass Sie ständig ressourcenintensiver als andere sind und sparsam verwendet werden sollten. andere Effekte können auch mehr frei genutzt werden. Für alle Effekte sollte jedoch Vorsicht beim Verketten und animieren verwendet werden, da einige Szenarien oder Kombinationen die Leistungsmerkmale des Effekt Diagramms ändern können. Im folgenden finden Sie eine Reihe von Thumb-Leistungsmerkmalen für einzelne Effekte:

- Effekte, die bekanntermaßen hohe Leistung haben, sind wie folgt – Gaußscher Weichzeichner, Schatten Maske, backdropbrush, hostbackdropbrush und ebenenvisualisierung. Diese werden für Low-End-Geräte [(Funktionsebene 9.1-9.3)](/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro)nicht empfohlen und sollten auf High-End-Geräten umsichtig verwendet werden.
- Zu den Effekten mit mittlerer Leistungs Beeinträchtigung zählen Farb Matrix, bestimmte Blend-Effekte blendmodes (Helligkeit, Farbe, Sättigung und Farbton), Spotlight, scenelightingeffect und (je nach Szenario) bordereffect. Diese Auswirkungen können mit bestimmten Szenarien auf Low-End-Geräten verwendet werden. bei der Verkettung und Animation sollte jedoch Vorsicht geboten werden. Es empfiehlt sich, die Verwendung auf zwei oder weniger zu beschränken und nur über Übergänge zu animieren.
- Alle anderen Effekte haben geringe Auswirkungen auf die Leistung und funktionieren in allen sinnvollen Szenarien, wenn Animationen und Verkettung verwendet werden.

## <a name="related-articles"></a>Verwandte Artikel

- [Reaktionsfähige UWP-Entwurfs Techniken](../design/layout/responsive-design.md)
- [UWP-Geräte Anpassung](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md)