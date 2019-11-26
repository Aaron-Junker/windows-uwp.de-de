---
Description: Hier erfahren Sie, wie Sie die UI Ihrer App beim Ein- oder Ausblenden der Touch-Bildschirmtastatur anpassen können.
title: Reagieren auf das Vorhandensein der Bildschirmtastatur
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: Tastatur, Erreichbarkeit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktionen
ms.date: 07/13/2018
ms.topic: article
ms.openlocfilehash: c752a5df96c22b945865c0c3a465f22391aa54bc
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258278"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>Reagieren auf das Vorhandensein der Bildschirmtastatur

Hier erfahren Sie, wie Sie die UI Ihrer App beim Ein- oder Ausblenden der Touch-Bildschirmtastatur anpassen können.

### <a name="important-apis"></a>Wichtige APIs

- [AutomationPeer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)

![Bildschirmtastatur mit Standardlayout](images/keyboard/default.png)

<sup>Die Touchscreen-Tastatur im standardlayoutmodus</sup>

Die Bildschirmtastatur ermöglicht Texteingabe für Geräte, die Toucheingabe unterstützen. Texteingabesteuerelemente der Universellen Windows-Plattform (UWP) rufen standardmäßig die Bildschirmtastatur auf, wenn ein Benutzer auf ein bearbeitbares Eingabefeld tippt. Die Bildschirmtastatur bleibt in der Regel sichtbar, während der Benutzer zwischen Steuerelementen in einem Formular navigiert. Dieses Verhalten kann jedoch je nach Steuerelementtypen innerhalb des Formulars variieren.

Um das zugehörige Berührungs Tastatur Verhalten in einem benutzerdefinierten Texteingabe-Steuerelement zu unterstützen, das nicht von einem Standardtext Eingabe-Steuerelement abgeleitet ist, müssen Sie die <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> -Klasse verwenden, um die Steuerelemente für die Microsoft UI-Automatisierung verfügbar zu machen und die richtigen Steuerelement Muster Weitere Informationen finden Sie unter [Barrierefreiheit für den Tastaturzugriff](https://docs.microsoft.com/windows/uwp/design/accessibility/keyboard-accessibility) und [Benutzerdefinierte Automatisierungspeers](https://docs.microsoft.com/windows/uwp/design/accessibility/custom-automation-peers).

Wenn diese Unterstützung für Ihr benutzerdefiniertes Steuerelement hinzugefügt wurde, können Sie auf das Vorhandensein der Bildschirmtastatur entsprechend reagieren.

**Voraussetzung**

Dieses Thema baut auf [Tastaturinteraktionen](keyboard-interactions.md) auf.

Sie sollten über ein grundlegendes Verständnis der standardmäßigen Tastaturinteraktionen, das Behandeln von Tastatureingaben und Ereignissen und der Benutzeroberflächenautomatisierung verfügen.

Wenn Sie noch keine Erfahrung mit der Entwicklung von UWP-Apps (Universelle Windows-Plattform) haben, sollten Sie sich in den folgenden Themen zunächst mit den hier besprochenen Technologien vertraut machen.

- [Erste App erstellen](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
- Informationen zu Ereignissen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**Richtlinien für die Benutzer Leistung:**

Hilfreiche Tipps zum Entwerfen einer nützlichen und ansprechenden APP, die für Tastatureingaben optimiert ist, finden Sie unter [Tastatur Interaktionen](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions) .

## <a name="touch-keyboard-and-a-custom-ui"></a>Bildschirmtastatur und benutzerdefinierte Benutzeroberfläche

Im Folgenden finden Sie einige grundlegende Empfehlungen für benutzerdefinierte Steuerelemente für die Texteingabe.

- Zeigen Sie die Bildschirmtastatur während der gesamten Interaktion mit dem Formular an.

- Stellen Sie sicher, dass die benutzerdefinierten Steuerelemente den entsprechenden [AutomationControlType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) für die Benutzeroberflächen Automatisierung aufweisen, damit die Tastatur beibehalten wird, wenn sich der Fokus aus einem Texteingabefeld im Kontext des Text Eintrags verschiebt. Wenn Sie zum Beispiel wünschen, dass die Tastatur eingeblendet bleibt, wenn während des Texteingabeszenarios ein Menü geöffnet wird, muss das Menü den **AutomationControlType** „Menu“ aufweisen.

- Vermeiden Sie die Manipulation der Eigenschaften der Benutzeroberflächenautomatisierung zur Kontrolle der Bildschirmtastatur. Andere Werkzeuge für die Barrierefreiheit sind von der Genauigkeit der Eigenschaften der Benutzeroberflächenautomatisierung abhängig.

- Stellen Sie sicher, dass Benutzer das Eingabefeld, mit dem sie interagieren, immer sehen können.

    Da die Bildschirmtastatur einen großen Teil des Bildschirms verdeckt, stellt die UWP sicher, dass das Eingabefeld im Fokus eingeblendet, wie ein Benutzer durch die Steuerelemente des Formulars navigiert, einschließlich der Steuerelemente, die derzeit nicht in der Ansicht sind.

    Wenn Sie die Benutzeroberfläche anpassen, sollten Sie ein ähnliches Verhalten für die Darstellung der Fingereingabe Tastatur bereitstellen, indem Sie die [Anzeige](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) [und das](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) Ausblenden von Ereignissen verarbeiten, die durch das [**inputpane**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane) -Objekt

    ![Formular mit und ohne angezeigte Touch-Bildschirmtastatur](images/touch-keyboard-pan1.png)

    Manchmal müssen bestimmte UI-Elemente dauerhaft auf dem Bildschirm zu sehen sein. Gestalten Sie die UI so, dass sich die Formularsteuerelemente in einer verschiebbaren Region befinden und die wichtigen UI-Elemente statisch sind. Zum Beispiel:

    ![Formular mit Bereichen, die immer sichtbar sein sollen](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Behandeln von Showing- und Hiding-Ereignissen

Im folgenden finden Sie ein Beispiel für das Anfügen von Ereignis Handlern für die [Anzeige](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) und das Ausblenden von Ereignissen der [Touchscreen-Tastatur](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) .

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

- [Tastatur Interaktionen](keyboard-interactions.md)
- [Barrierefreiheit der Tastaturnavigation](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)
- [Benutzerdefinierte Automatisierungs-Peers](https://docs.microsoft.com/windows/uwp/accessibility/custom-automation-peers)

**Beispiele**

- [Beispiel für eine Berührungs Tastatur](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

**Archivbeispiele**

- [Eingabe: Beispiel für eine Touchscreen-Tastatur](https://code.msdn.microsoft.com/windowsapps/Touch-keyboard-sample-43532fda)
- [Reagieren auf das Aussehen der Bildschirmtastatur (Beispiel)](https://code.msdn.microsoft.com/windowsapps/keyboard-events-sample-866ba41c)
- [Beispiel für XAML-Textbearbeitung](https://code.msdn.microsoft.com/windowsapps/XAML-text-editing-sample-fb0493ad)
- [XAML-Barrierefreiheits Beispiel](https://code.msdn.microsoft.com/windowsapps/XAML-accessibility-sample-d63e820d)
