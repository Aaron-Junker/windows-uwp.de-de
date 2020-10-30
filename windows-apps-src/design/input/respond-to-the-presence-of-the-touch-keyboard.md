---
description: Hier erfahren Sie, wie Sie die UI Ihrer App beim Ein- oder Ausblenden der Touch-Bildschirmtastatur anpassen können.
title: Reagieren auf die Anzeige der Bildschirmtastatur
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: Tastatur, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktionen
ms.date: 09/24/2020
ms.topic: article
ms.openlocfilehash: c3431fcafb86428ce5eddb8ea7a6b0187b64a5e5
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93035083"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>Reagieren auf die Anzeige der Bildschirmtastatur

Hier erfahren Sie, wie Sie die UI Ihrer App beim Ein- oder Ausblenden der Touch-Bildschirmtastatur anpassen können.

### <a name="important-apis"></a>Wichtige APIs

- [AutomationPeer](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](/uwp/api/Windows.UI.ViewManagement.InputPane)

![Touch-Bildschirmtastatur mit Standardlayout](images/keyboard/default.png)

<sup>Die Touchscreen-Tastatur im standardlayoutmodus</sup>

Die Bildschirmtastatur ermöglicht Texteingabe für Geräte, die Toucheingabe unterstützen. Texteingabe-Steuerelemente für Windows-apps rufen standardmäßig die Touchscreen-Tastatur auf, wenn ein Benutzer auf ein bearbeitbares Eingabefeld tippt. Die Bildschirmtastatur bleibt in der Regel sichtbar, während der Benutzer zwischen Steuerelementen in einem Formular navigiert. Dieses Verhalten kann jedoch je nach Steuerelementtypen innerhalb des Formulars variieren.

Um das entsprechende Verhalten der Bildschirmtastatur in einem benutzerdefinierten Texteingabe-Steuerelement zu unterstützen, das nicht von einem standardmäßigen Texteingabe-Steuerelement abgeleitet ist, müssen Sie die <a href="/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a>-Klasse verwenden, um die Steuerelemente für die Microsoft-Benutzeroberflächenautomatisierung verfügbar zu machen und die richtigen Steuerelementmuster der Benutzeroberflächenautomatisierung implementieren. Weitere Informationen finden Sie unter [Barrierefreiheit für den Tastaturzugriff](../accessibility/keyboard-accessibility.md) und [Benutzerdefinierte Automatisierungspeers](../accessibility/custom-automation-peers.md).

Wenn diese Unterstützung für Ihr benutzerdefiniertes Steuerelement hinzugefügt wurde, können Sie auf das Vorhandensein der Bildschirmtastatur entsprechend reagieren.

**Voraussetzungen:**

Dieses Thema baut auf [Tastaturinteraktionen](keyboard-interactions.md) auf.

Sie sollten über ein grundlegendes Verständnis der standardmäßigen Tastaturinteraktionen, das Behandeln von Tastatureingaben und Ereignissen und der Benutzeroberflächenautomatisierung verfügen.

Wenn Sie mit der Entwicklung von Windows-apps noch nicht vertraut sind, können Sie sich diese Themen ansehen, um sich mit den hier behandelten Technologien vertraut zu machen.

- [Erstellen der ersten App](../../get-started/your-first-app.md)
- Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](../../xaml-platform/events-and-routed-events-overview.md).

**Richtlinien für die Benutzer Leistung:**

Hilfreiche Tipps zum Entwerfen einer nützlichen und ansprechenden APP, die für Tastatureingaben optimiert ist, finden Sie unter [Tastatur Interaktionen](./keyboard-interactions.md) .

## <a name="touch-keyboard-and-a-custom-ui"></a>Bildschirmtastatur und benutzerdefinierte Benutzeroberfläche

Im Folgenden finden Sie einige grundlegende Empfehlungen für benutzerdefinierte Steuerelemente für die Texteingabe.

- Zeigen Sie die Bildschirmtastatur während der gesamten Interaktion mit dem Formular an.

- Stellen Sie sicher, dass Ihre benutzerdefinierten Steuerelemente über den richtigen [AutomationControlType](/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) für die Benutzeroberflächenautomatisierung verfügen, um zu gewährleisten, dass die Tastatur eingeblendet bleibt, wenn der Fokus während der Texteingabe vom Texteingabefeld verschoben wird. Wenn Sie zum Beispiel wünschen, dass die Tastatur eingeblendet bleibt, wenn während des Texteingabeszenarios ein Menü geöffnet wird, muss das Menü den **AutomationControlType** „Menu“ aufweisen.

- Vermeiden Sie die Manipulation der Eigenschaften der Benutzeroberflächenautomatisierung zur Kontrolle der Bildschirmtastatur. Andere Werkzeuge für die Barrierefreiheit sind von der Genauigkeit der Eigenschaften der Benutzeroberflächenautomatisierung abhängig.

- Stellen Sie sicher, dass Benutzer das Eingabefeld, mit dem sie interagieren, immer sehen können.

    Da die touchtastatur einem großen Teil des Bildschirms entspricht, stellt Windows sicher, dass das Eingabefeld mit dem Fokus in die Ansicht wechselt, wenn ein Benutzer durch die Steuerelemente im Formular navigiert, einschließlich der Steuerelemente, die derzeit nicht in der Ansicht angezeigt werden.

    Beim Anpassen der Benutzeroberfläche können Sie ein ähnliches Verhalten für die Darstellung der Bildschirmtastatur bereitstellen, indem Sie die [Showing](/uwp/api/windows.ui.viewmanagement.inputpane.showing)- und [Hiding](/uwp/api/windows.ui.viewmanagement.inputpane.hiding)-Ereignisse behandeln, die vom [**InputPane**](/uwp/api/Windows.UI.ViewManagement.InputPane)-Objekt zur Verfügung gestellt werden.

    ![Formular mit und ohne angezeigte Bildschirmtastatur](images/touch-keyboard-pan1.png)

    Manchmal müssen bestimmte UI-Elemente dauerhaft auf dem Bildschirm zu sehen sein. Gestalten Sie die UI so, dass sich die Formularsteuerelemente in einer verschiebbaren Region befinden und die wichtigen UI-Elemente statisch sind. Beispiel:

    ![Formular mit Bereichen, die immer sichtbar sein sollen](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Behandeln von Showing- und Hiding-Ereignissen

Im folgenden finden Sie ein Beispiel für das Anfügen von Ereignis Handlern für die [Anzeige](/uwp/api/windows.ui.viewmanagement.inputpane.showing) und das Ausblenden von Ereignissen der [Touchscreen-Tastatur](/uwp/api/windows.ui.viewmanagement.inputpane.hiding) .

```csharp
using Windows.UI.ViewManagement;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Foundation;
using Windows.UI.Xaml.Navigation;

namespace SDKTemplate
{
    /// <summary>
    /// Sample page to subscribe show/hide event of Touch Keyboard.
    /// </summary>
    public sealed partial class Scenario2_ShowHideEvents : Page
    {
        public Scenario2_ShowHideEvents()
        {
            this.InitializeComponent();
        }

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Subscribe to Showing/Hiding events
            currentInputPane.Showing += OnShowing;
            currentInputPane.Hiding += OnHiding;
        }

        protected override void OnNavigatedFrom(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Unsubscribe from Showing/Hiding events
            currentInputPane.Showing -= OnShowing;
            currentInputPane.Hiding -= OnHiding;
        }

        void OnShowing(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Showing";
        }
        
        void OnHiding(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Hiding";
        }
    }
}
```

```cppwinrt
...
#include <winrt/Windows.UI.ViewManagement.h>
...
private:
    winrt::event_token m_showingEventToken;
    winrt::event_token m_hidingEventToken;
...
Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Subscribe to Showing/Hiding events
    m_showingEventToken = inputPane.Showing({ this, &Scenario2_ShowHideEvents::OnShowing });
    m_hidingEventToken = inputPane.Hiding({ this, &Scenario2_ShowHideEvents::OnHiding });
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Unsubscribe from Showing/Hiding events
    inputPane.Showing(m_showingEventToken);
    inputPane.Hiding(m_hidingEventToken);
}

void Scenario2_ShowHideEvents::OnShowing(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Showing");
}

void Scenario2_ShowHideEvents::OnHiding(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Hiding");
}
```

```cpp
#include "pch.h"
#include "Scenario2_ShowHideEvents.xaml.h"

using namespace SDKTemplate;
using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Navigation;

Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto inputPane = InputPane::GetForCurrentView();
    // Subscribe to Showing/Hiding events
    showingEventToken = inputPane->Showing +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnShowing);
    hidingEventToken = inputPane->Hiding +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnHiding);
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(NavigationEventArgs^ e)
{
    auto inputPane = Windows::UI::ViewManagement::InputPane::GetForCurrentView();
    // Unsubscribe from Showing/Hiding events
    inputPane->Showing -= showingEventToken;
    inputPane->Hiding -= hidingEventToken;
}

void Scenario2_ShowHideEvents::OnShowing(InputPane^ /*sender*/, InputPaneVisibilityEventArgs^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Showing";
}

void Scenario2_ShowHideEvents::OnHiding(InputPane^ /*sender*/, InputPaneVisibilityEventArgs ^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Hiding";
}
```

## <a name="related-articles"></a>Verwandte Artikel

- [Tastaturinteraktionen](keyboard-interactions.md)
- [Barrierefreiheit der Tastaturnavigation](../accessibility/keyboard-accessibility.md)
- [Benutzerdefinierte Automatisierungspeers](../accessibility/custom-automation-peers.md)

### <a name="samples"></a>Beispiele

- [Beispiel für eine Berührungs Tastatur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

### <a name="archive-samples"></a>Archivbeispiele

- [Eingabe: Beispiel für die Bildschirmtastatur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
- [Beispiel für die Reaktion auf die Anzeige der Bildschirmtastatur](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Responding%20to%20the%20appearance%20of%20the%20on-screen%20keyboard%20sample)
- [Beispiel für die XAML-Textbearbeitung](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
- [XAML-Beispiel für Barrierefreiheit](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
