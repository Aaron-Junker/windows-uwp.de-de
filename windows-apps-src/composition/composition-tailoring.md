---
title: Anpassen der Komposition
description: Verwenden Sie die Kompositions-APIs zum Anpassen der Benutzeroberflächenautomatisierungs und Optimieren der Leistung zu ermöglichen, benutzereinstellungen und die Eigenschaften des Geräts.
ms.date: 07/16/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: bcc9a6d89a143d8fd03d73dbd83b832ed9513ee2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644415"
---
# <a name="tailoring-effects--experiences-using-windows-ui"></a>Anpassen von Auswirkungen und Erfahrungen mit Windows-Benutzeroberfläche

Windows-Benutzeroberfläche bietet viele ansprechende Effekte, Animationen und bedeutet, dass um diese zu unterscheiden. Erfüllt die Erwartungen der Benutzer für Leistung und anpassbarkeit ist jedoch immer noch ein wichtiger Bestandteil von erfolgreiche Anwendungen erstellen. Die universelle Windows-Plattform unterstützt eine umfangreiche und verschiedene-Familie von Geräten, die unterschiedlichen Features und Funktionen haben. Um eine inklusive-Erfahrung für alle Benutzer zu gewährleisten, müssen Sie die Skalierung Ihrer Anwendungen auf Geräten sicher und benutzereinstellungen berücksichtigen. Anpassen der Benutzeroberfläche, bieten eine effiziente Möglichkeit, nutzen Sie die funktionsmöglichkeiten eines Geräts und eine angenehme und inklusive benutzererfahrung sicherzustellen.

Anpassen der Benutzeroberfläche ist eine Kategorie umfasst Aufgaben für Leistung und ansprechende Benutzeroberfläche in Bezug auf den folgenden Bereichen:

- Unter Beachtung der und zur Anpassung an den benutzereinstellungen für Effekte
- Sicherheitsszenarios unverzichtbar benutzereinstellungen für Animationen
- Optimieren der Benutzeroberfläche für den angegebenen Hardwarefunktionen

Hier wird beschrieben, wie Sie Ihre Effekten und Animationen mit der visuellen Ebene in den Bereichen, die oben genannten anpassen, aber es gibt viele andere Methoden zum Anpassen der Anwendung, damit eine hervorragende endbenutzererfahrung. Anleitungen-Dokumentation finden Sie Anleitungen zum [Anpassen Ihrer Benutzeroberfläche](/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design) für verschiedene Geräte und [Erstellen von reaktionsfähigen UI](/windows/uwp/design/layout/responsive-design).

## <a name="user-effects-settings"></a>Auswirkungen von benutzereinstellungen

Benutzer können ihre Windows-Benutzeroberfläche für eine Vielzahl von Gründen anpassen, welche die Anwendungen berücksichtigen sollten, und sich anpassen. Ein Bereich, den Endbenutzer steuern können, ändert die Typen von Auswirkungen, die sie sehen, die in ihrem System verwendet.

### <a name="transparency-effects-settings"></a>Einstellungen für die Auswirkungen von Transparenz

Eine solche Einstellung für die Auswirkung, die Benutzer anpassen können, ist das Transparenzeffekte ein-und aktivieren. Diese finden Sie in der Einstellungs-app unter Personalisierung > Farben, oder über die app "Einstellungen" > erleichterte Bedienung > anzeigen.

![In den Einstellungen die Option "Transparenz"](images/tailoring-transparency-setting.png)

Wenn aktiviert, werden unwirksam, die Transparenz verwendet wie erwartet angezeigt werden. Dies gilt für den Blog, HostBackdropBrush oder jedes benutzerdefinierten Effekts-Diagramm, das nicht vollständig deckend ist.

Wenn deaktiviert, wird Blog Material automatisch auf eine Volltonfarbe zurückgreifen, da XAMLs-acrylpinsel standardmäßig auf dieses Ereignis überwacht wurde. Hier sehen wir die Rechner-Anwendung entsprechend Fallback auf eine Volltonfarbe Wenn Transparenzeffekte nicht aktiviert sind:

![Rechner mit Blog](images/tailoring-acrylic.png)
![-Rechner mit Antworten auf die Transparenzeinstellungen Blog](images/tailoring-acrylic-fallback.png)

Für jegliche benutzerdefinierten wirkt sich die Anwendung muss jedoch auf reagieren die [UISettings.AdvancedEffectsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) Eigenschaft oder [AdvancedEffectsEnabledChanged](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.advancedeffectsenabledchanged) Ereignis und Switch out die Auswirkungen/Wirkung Diagramm ein Effekts verwendet wird, das keine Transparenz aufweist. Ein Beispiel dafür ist unten:

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

## <a name="animations-settings"></a>Einstellungen für Animationen

Anwendungen sollten auf ähnliche Weise Lauschen und reagieren auf die [UISettings.AnimationsEnabled](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.uisettings.animationsenabled) Eigenschaft, um sicherzustellen, dass Animationen sind aktiviert oder deaktiviert basierend auf benutzereinstellungen in den Einstellungen > erleichterte Bedienung > anzeigen.

![Animationen-Option in den Einstellungen](images/tailoring-animations-setting.png)

```cs
public MainPage()
{
   var uisettings = new UISettings();
   bool animationsEnabled = uisettings.AnimationsEnabled;
   // TODO respond to animations settings
}

```

## <a name="leveraging-the-capabilities-api"></a>Nutzung der API-Funktionen

Durch die Nutzung der [CompositionCapabilities](/uwp/api/windows.ui.composition.compositioncapabilities) -APIs können Sie erkennen die Komposition verfügbar sind und leistungsstarke auf angegebene Hardware-features und passen Sie den Entwurf, um sicherzustellen, dass Endbenutzer erhalten eine leistungsfähige und ansprechende Benutzeroberfläche auf einem das Gerät. Die-APIs bieten die Möglichkeit zur Überprüfung von Hardware-Systemfunktionen, um die ordnungsgemäße Skalierung für eine Vielzahl von Formfaktoren Effekt zu implementieren. Dies erleichtert Ihnen die Anwendung, um eine schöne erstellen und die nahtlose End-benutzererfahrung entsprechend anpassen.

Diese API stellt die Methoden "und" einen Ereignislistener, die mit der Skalierung von Entscheidungen für die Benutzeroberfläche der Anwendung wirksam werden kann. Die Funktion erkennt, wie gut das System komplexer Zusammensetzung und Renderingvorgängen verarbeiten kann und dann die Informationen in einem einfach zu nutzenden-Modell für Entwickler nutzen können.

### <a name="using-composition-capabilities"></a>Mithilfe von Komposition-Funktionen

Die CompositionCapabilities-Funktionalität wird bereits für Funktionen wie Blog-Material, genutzt wird, in denen das Material auf eine weitere leistungsstarke Auswirkung je nach Szenario und Hardware zurückgegriffen.

Die API für vorhandenen Code in wenigen einfachen Schritten kann hinzugefügt werden.

1. Abrufen des funktionalitätsobjekts in Ihrer Anwendung-Konstruktor.

    ```cs
    _capabilities = CompositionCapabilities.GetForCurrentView();
    ```

1. Registrieren Sie einen Funktionen-changed-Ereignis-Listener für Ihre app.

    ```cs
    _capabilities.Changed += HandleCapabilitiesChanged;
    ```

1. Fügen Sie Inhalt an die Rückruf-Ereignismethode unterschiedlicher Funktionen behandelt. Dies kann oder möglicherweise nicht der nächste Schritt unten ähnelt.
1. Wenn Effekte verwenden zu können, überprüfen Sie zunächst die Capabilities-Objekt. Bedingungsüberprüfungen in Betracht, oder wechseln Sie Steueranweisungen, je nachdem, wie Sie die Auswirkungen anpassen möchten.

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

Vollständiges Codebeispiel finden Sie auf die [Windows UI-Github-Repository](https://github.com/Microsoft/WindowsUIDevLabs/tree/master/SampleGallery/Samples/SDK%2015063/CompCapabilities).

## <a name="fast-vs-slow-effects"></a>Schnelle oder langsame Effekte

Aufgrund des Feedbacks aus der bereitgestellten [AreEffectsSupported](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectssupported) und [AreEffectsFast](/uwp/api/windows.ui.composition.compositioncapabilities.areeffectsfast) -Methoden in der CompositionCapabilities-API kann die Anwendung kann entscheiden, zu teuer oder nicht unterstützten Effekte für austauschen andere Auswirkungen ihrer Wahl, die für das Gerät optimiert sind. Einige Effekte bekanntermaßen Weitere ressourcenintensiv als andere konsistent sein und sollten sparsam eingesetzt werden und andere Effekte können flexibler verwendet werden. Für alle Effekte sollte jedoch Sorgfalt verwendet werden beim Verketten und animieren, die als einige Szenarien oder Kombinationen von die Leistungsmerkmale des Diagramms Auswirkungen ändern können. Im folgenden finden Sie einige Faustregel Leistungsmerkmale für die einzelnen Auswirkungen:

- Effekte, die bekannt sind, haben Auswirkungen auf die hohe Leistung sind wie folgt: Bildbearbeitungstools, Volumeschattenkopie-Maske, BackDropBrush, HostBackDropBrush und Ebene Visual. Diese werden nicht empfohlen, für die low-End-Geräten [(die Funktionsebene 9.1-9.3)](https://msdn.microsoft.com/library/windows/desktop/ff476876(v=vs.85).aspx), und sollte mit Bedacht auf high-End-Geräten verwendet werden.
- Effekte mit Auswirkungen auf die mittlere Leistung enthalten Farbmatrix, bestimmte Blend Auswirkungen BlendModes (Helligkeit, Farbe, Sättigung und Hue), SpotLight, SceneLightingEffect und (je nach Szenario) BorderEffect. Diese Effekte auf low-End-Geräten mit bestimmten Szenarien funktioniert möglicherweise, jedoch Sorgfalt beim Verketten und animieren, verwendet werden soll. Empfehlen Sie einschränken der Verwendung auf zwei oder weniger, und bei Übergängen nur animieren.
- Alle anderen Auswirkungen haben Auswirkungen auf die mit niedriger Leistung und in alle plausible Szenarien beim animieren und verketten.

## <a name="related-articles"></a>Verwandte Artikel

- [Reaktionsfähige Designtechniken UWP](https://docs.microsoft.com/windows/uwp/design/layout/responsive-design)
- [UWP Gerät anpassen](https://docs.microsoft.com/windows/uwp/design/layout/screen-sizes-and-breakpoints-for-responsive-design)
