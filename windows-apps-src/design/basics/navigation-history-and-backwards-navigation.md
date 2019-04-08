---
Description: Erfahren Sie, wie rückwärts Navigation für das Durchlaufen des Benutzers Navigationsverlauf in einer UWP-app zu implementieren.
title: Navigationsverlauf und Rückwärtsnavigation (Windows-Apps)
template: detail.hbs
op-migration-status: ready
ms.date: 06/21/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c74d4ebd08dfeddfb4a0149cffcd7bb845ceff11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595045"
---
# <a name="navigation-history-and-backwards-navigation-for-uwp-apps"></a>Navigationsverlauf und Rückwärtsnavigation für UWP-Apps

> **Wichtige APIs:** ["Backrequested"-Ereignis](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager.BackRequested), ["systemnavigationmanager"-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Core.SystemNavigationManager), [OnNavigatedTo](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_)

Die universelle Windows-Plattform (UWP) enthält ein einheitliches System zur Rückwärtsnavigation, mit dem der Navigationsverlauf des Benutzers innerhalb einer App und je nach Gerät von App zu App durchlaufen werden kann.

Um die Rückwärtsnavigation in Ihrer App zu implementieren, platzieren Sie einen [Zurück](#Back-button)-Button in der oberen linken Ecke der Benutzeroberfläche Ihrer App. Wenn Ihre App das [NavigationView](../controls-and-patterns/navigationview.md)-Steuerelement verwendet, können Sie die integrierte [Zurück-Schaltfläche von NavigationView](../controls-and-patterns/navigationview.md#backwards-navigation) verwenden.

Der Benutzer erwartet, dass durch Drücken der Zurück-Schaltfläche die vorherige Seite im Navigationsverlauf der App aufgerufen wird. Sie können entscheiden, welche Navigationsaktionen dem Navigationsverlauf hinzugefügt werden sollen und wie auf das Drücken der Zurück-Schaltfläche reagiert werden soll.

## <a name="back-button"></a>Zurück-Schaltfläche

Verwenden Sie zum Erstellen einer Schaltfläche "zurück" die [Schaltfläche](../controls-and-patterns/buttons.md) steuern Sie mit der `NavigationBackButtonNormalStyle` formatieren aus, und platzieren Sie die Schaltfläche mit den in der oberen linken Ecke von Ihrer app Benutzeroberfläche (Weitere Informationen finden Sie unter den folgenden XAML-Codebeispielen).

![Zurück-Schaltfläche oben links in der App-Oberfläche](images/back-nav/BackEnabled.png)

```xaml
<Button Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Wenn Ihre App eine obere [CommandBar](../controls-and-patterns/app-bars.md) hat, wird das 44 Pixel hohe Button-Steuerelement nicht sehr gut neben 48 Pixel hohe AppBarButtons passen. Um jedoch Inkonsistenzen zu vermeiden, richten Sie den oberen Teil der Schaltfläche innerhalb der 48 Pixel-Grenzen aus.

![Zurück-Schaltfläche in der oberen Befehlsleiste](images/back-nav/CommandBar.png)

```xaml
<Button VerticalAlignment="Top" HorizontalAlignment="Left" 
Style="{StaticResource NavigationBackButtonNormalStyle}"/>
```

Um UI-Elemente zu minimieren, die sich in Ihrer App bewegen, zeigen Sie eine deaktivierte Zurück-Schaltfläche an, wenn sich nichts im Backstack befindet (siehe Codebeispiel unten). Wenn Sie, dass Ihre app niemals einen Backstack verfügen erwarten, müssen Sie jedoch nicht die Schaltfläche "zurück" anzeigen.

![Zustände der Zurück-Schaltfläche](images/back-nav/BackDisabled.png)

## <a name="code-example"></a>Codebeispiel

Das folgende Codebeispiel demonstriert, wie man das Rückwärtsnavigationsverhalten mit einer Zurück-Schaltfläche implementiert. Der Code reagiert auf das Button-Ereignis [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.Click) und deaktiviert/aktiviert die Sichtbarkeit der Schaltfläche in [**OnNavigatedTo**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page.onnavigatedto#Windows_UI_Xaml_Controls_Page_OnNavigatedTo_Windows_UI_Xaml_Navigation_NavigationEventArgs_), das beim Navigieren zu einer neuen Seite aufgerufen wird. Das Codebeispiel behandelt auch Eingaben von Hardware- und Softwaresystem-Zurück-Schaltflächen, indem es einen Listener für das [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested)-Ereignis registriert.

```xaml
<!-- MainPage.xaml -->
<Page x:Class="AppName.MainPage">
...
<Button x:Name="BackButton" Click="Back_Click" Style="{StaticResource NavigationBackButtonNormalStyle}"/>
...
<Page/>
```

Code-Behind:

```csharp
// MainPage.xaml.cs
public MainPage()
{
    KeyboardAccelerator GoBack = new KeyboardAccelerator();
    GoBack.Key = VirtualKey.GoBack;
    GoBack.Invoked += BackInvoked;
    KeyboardAccelerator AltLeft = new KeyboardAccelerator();
    AltLeft.Key = VirtualKey.Left;
    AltLeft.Invoked += BackInvoked;
    this.KeyboardAccelerators.Add(GoBack);
    this.KeyboardAccelerators.Add(AltLeft);
    // ALT routes here
    AltLeft.Modifiers = VirtualKeyModifiers.Menu;
}

protected override void OnNavigatedTo(NavigationEventArgs e)
{
    BackButton.IsEnabled = this.Frame.CanGoBack;
}

private void Back_Click(object sender, RoutedEventArgs e)
{
    On_BackRequested();
}

// Handles system-level BackRequested events and page-level back button Click events
private bool On_BackRequested()
{
    if (this.Frame.CanGoBack)
    {
        this.Frame.GoBack();
        return true;
    }
    return false;
}

private void BackInvoked (KeyboardAccelerator sender, KeyboardAcceleratorInvokedEventArgs args)
{
    On_BackRequested();
    args.Handled = true;
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"

#include "winrt/Windows.System.h"
#include "winrt/Windows.UI.Xaml.Controls.h"
#include "winrt/Windows.UI.Xaml.Input.h"
#include "winrt/Windows.UI.Xaml.Navigation.h"

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::UI::Xaml;

namespace winrt::PageNavTest::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();

        Windows::UI::Xaml::Input::KeyboardAccelerator goBack;
        goBack.Key(Windows::System::VirtualKey::GoBack);
        goBack.Invoked({ this, &MainPage::BackInvoked });
        Windows::UI::Xaml::Input::KeyboardAccelerator altLeft;
        altLeft.Key(Windows::System::VirtualKey::Left);
        altLeft.Invoked({ this, &MainPage::BackInvoked });
        KeyboardAccelerators().Append(goBack);
        KeyboardAccelerators().Append(altLeft);
        // ALT routes here.
        altLeft.Modifiers(Windows::System::VirtualKeyModifiers::Menu);
    }

    void MainPage::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
    {
        BackButton().IsEnabled(Frame().CanGoBack());
    }

    void MainPage::Back_Click(IInspectable const&, RoutedEventArgs const&)
    {
        On_BackRequested();
    }

    // Handles system-level BackRequested events and page-level back button Click events.
    bool MainPage::On_BackRequested()
    {
        if (Frame().CanGoBack())
        {
            Frame().GoBack();
            return true;
        }
        return false;
    }

    void MainPage::BackInvoked(Windows::UI::Xaml::Input::KeyboardAccelerator const& sender,
        Windows::UI::Xaml::Input::KeyboardAcceleratorInvokedEventArgs const& args)
    {
        args.Handled(On_BackRequested());
    }
}
```

Wir behandeln oben rückwärts Navigation für eine einzelne Seite. Sie können zur Navigation in jede Seite behandeln, wenn Sie bestimmte Seiten aus der Rückwärtsnavigation ausschließen möchten, oder auf Seitenebene Code vor dem Anzeigen der Seite ausgeführt werden soll.

Um Abwärtskompatibilität Navigation für eine ganze app behandeln möchten, registrieren Sie einen globalen Listener für die [ **BackRequested** ](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.BackRequested) Ereignis in der `App.xaml` Code-Behind-Datei.

App.xaml Code-Behind:

```csharp
// App.xaml.cs
Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested += App_BackRequested;
Frame rootFrame = Window.Current.Content;
rootFrame.PointerPressed += On_PointerPressed;

private void App_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    e.Handled = On_BackRequested();
}

private void On_PointerPressed(object sender, PointerRoutedEventArgs e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender as UIElement).Properties.PointerUpdateKind == PointerUpdateKind.XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled = On_BackRequested();
    }
}

private bool On_BackRequested()
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame.CanGoBack)
    {
        rootFrame.GoBack();
        return true;
    }
    return false;
}
```

```cppwinrt
// App.cpp
#include <winrt/Windows.UI.Core.h>
#include "winrt/Windows.UI.Input.h"
#include "winrt/Windows.UI.Xaml.Input.h"

#include "App.h"
#include "MainPage.h"

using namespace winrt;
...

    Windows::UI::Core::SystemNavigationManager::GetForCurrentView().BackRequested({ this, &App::App_BackRequested });
    Frame rootFrame{ nullptr };
    auto content = Window::Current().Content();
    if (content)
    {
        rootFrame = content.try_as<Frame>();
    }
    rootFrame.PointerPressed({ this, &App::On_PointerPressed });
...

void App::App_BackRequested(IInspectable const& /* sender */, Windows::UI::Core::BackRequestedEventArgs const& e)
{
    e.Handled(On_BackRequested());
}

void App::On_PointerPressed(IInspectable const& sender, Windows::UI::Xaml::Input::PointerRoutedEventArgs const& e)
{
    bool isXButton1Pressed =
        e.GetCurrentPoint(sender.as<UIElement>()).Properties().PointerUpdateKind() == Windows::UI::Input::PointerUpdateKind::XButton1Pressed;

    if (isXButton1Pressed)
    {
        e.Handled(On_BackRequested());
    }
}

// Handles system-level BackRequested events.
bool App::On_BackRequested()
{
    if (Frame().CanGoBack())
    {
        Frame().GoBack();
        return true;
    }
    return false;
}
```

## <a name="optimizing-for-different-device-and-form-factors"></a>Optimierung für unterschiedliche Geräte- und Formfaktoren

Diese Anleitung für das Rückwärtsnavigationsdesign gilt für alle Geräte. Verschiedene Geräte und Formfaktoren können jedoch von der Optimierung profitieren. Dies hängt auch von der Hardware-Zurück-Taste ab, der von verschiedenen Shells unterstützt wird.

- **Telefon/Tablet**: Eine Hardware- oder Softwarefehler-zurück-Schaltfläche ist immer vorhanden, die auf mobilen und Tablet, aber es wird empfohlen, eine in-app die Schaltfläche "zurück" aus Gründen der Übersichtlichkeit zu zeichnen.
- **Desktop/Hub**: Zeichnen Sie die Schaltfläche "zurück" in-app auf der oberen linken Ecke von Ihrer app Benutzeroberfläche.
- **Xbox/TV**: Zeichnen Sie eine Schaltfläche "zurück", nicht für sie unnötige UI-Elemente hinzufügen. Verlassen Sie sich stattdessen auf die Gamepad-Taste B, um rückwärts zu navigieren.

Wenn Ihre App auf mehreren Geräten läuft, [erstellen Sie einen benutzerdefinierten visuellen Trigger für die Xbox](../devices/designing-for-tv.md#custom-visual-state-trigger-for-xbox), um die Sichtbarkeit der Schaltfläche umzuschalten. Das NavigationView-Steuerelement schaltet automatisch die Sichtbarkeit der Schaltfläche „Zurück” um, wenn Ihre App auf der Xbox läuft. 

Wir empfehlen, die folgenden Eingaben für die Rückwärtsnavigation zu unterstützen. (Beachten Sie, dass einige dieser Eingaben vom System-BackRequested nicht unterstützt werden und durch separate Ereignisse behandelt werden müssen.

| Input | Ereignis |
| --- | --- |
| Windows-Rücktaste | BackRequested |
| Hardware-Zurück-Taste | BackRequested |
| Shell-Tablet-Modus Zurück-Schaltfläche | BackRequested |
| VirtualKey.XButton1 | PointerPressed |
| VirtualKey.GoBack | KeyboardAccelerator.BackInvoked |
| ALT + NACH-LINKS | KeyboardAccelerator.BackInvoked |

Die oben aufgeführten Codebeispiele zeigen, wie man mit diesen Eingaben umgeht.

## <a name="system-back-behavior-for-backward-compatibilities"></a>System-Rückwärtsverhalten für Rückwärtskompatibilitäten

Bisher nutzten UWP-Apps [AppViewBackButtonVisibility](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility) für die Rückwärtsnavigation. Die API werden weiterhin unterstützt werden, um Abwärtskompatibilität zu gewährleisten, jedoch nicht mehr empfohlen auf ["appviewbackbuttonvisibility"](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility). Stattdessen sollte Ihre App eine eigene Zurück-Schaltfläche in der App darstellen.

Wenn Ihre app weiterhin mit ["appviewbackbuttonvisibility"](https://docs.microsoft.com/uwp/api/windows.ui.core.appviewbackbuttonvisibility), und klicken Sie dann die Benutzeroberfläche des Systems die System-zurück-Schaltfläche in der Titelleiste gerendert wird. (Die Darstellung und das Benutzer Interaktionen für die Schaltfläche "zurück" sind nicht von der vorherigen Builds.)

![Titelleiste über die Schaltfläche "zurück"](images/nav-back-pc.png)

### <a name="system-back-bar"></a>System nach hinten Balken

> [!NOTE]
> "System nach hinten Balken" ist nur eine Beschreibung, kein offizieller Name.

Das System nach hinten ist ein "Band", die zwischen dem Band Registerkarte und der app-Inhaltsbereich eingefügt wird. Das Band läuft über die Breite der App und enthält die Zurück-Schaltfläche auf der linken Seite. Das Band enthält eine vertikale Gesamthöhe des 32 Pixel, um sicherzustellen, dass ausreichend Touch-Zielgröße für die Schaltfläche "zurück".

Die systemeigene Rückwärtsnavigationsleiste wird dynamisch angezeigt, abhängig von der Sichtbarkeit der Zurück-Schaltfläche. Wenn die zurück-Schaltfläche sichtbar ist, wird das System nach hinten Leiste eingefügt werden, verschieben app-Inhalte nach unten x 32 Pixel unterhalb der Registerkarte. Wenn die Schaltfläche "zurück" ausgeblendet ist, auf dem System nach hinten Leiste dynamisch entfernt werden kann, mehr app-Inhalte x 32 Pixel, um das Band Registerkarte zu erfüllen. Um zu vermeiden, müssen Ihrer app-UI-Schicht, hoch- oder Herunterskalieren, zeichnen empfiehlt eine [in-app-Schaltfläche "zurück"](#back-button).

[Title bar Anpassungen](../shell/title-bar.md) übernommen wird, sowohl auf der Registerkarte "app" als auch auf das System wieder Leiste. Wenn Ihre app Hintergrund- und Vordergrundfarben Farbeigenschaften mit angibt [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar), und klicken Sie dann die Farben auf der Registerkarte "und" System Rückseite angewendet werden Leiste.

## <a name="guidelines-for-custom-back-navigation-behavior"></a>Richtlinien für das benutzerdefinierte Verhalten der Rückwärtsnavigation

Wenn Sie Ihre eigene Zurück-Stapelnavigation bereitstellen möchten, sollte die Benutzeroberfläche mit anderen Apps konsistent sein. Wir empfehlen, die folgenden Muster für Navigationsaktionen zu verwenden:

<div class="mx-responsive-img">
<table>
<thead>
<tr class="header">
<th align="left">Navigationsaktion</th>
<th align="left">Zum Navigationsverlauf hinzufügen?</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="vertical-align:top;"><strong>Seite, um die Seite "," andere "Peer Groups"</strong></td>
<td style="vertical-align:top;"><strong>"Ja"</strong>
<p>In der folgenden Abbildung navigiert der Benutzer von Ebene 1 der App zu Ebene 2. Dabei werden Peer-Gruppen durchlaufen, sodass die Navigation dem Navigationsverlauf hinzugefügt wird.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly1.png" alt="Navigation across peer groups" /></p>
<p>In der nächsten Abbildung navigiert der Benutzer zwischen zwei Peer-Gruppen derselben Ebene. Er durchläuft erneut Peer-Gruppen, sodass die Navigation dem Navigationsverlauf hinzugefügt wird.</p>
<p><img src="images/back-nav/nav-pagetopage-diffpeers-imageonly2.png" alt="Navigation across peer groups" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Seite zu Seite gleich peer-Gruppe nicht auf dem Bildschirm Navigationselement</strong>
<p>Der Benutzer navigiert von einer Seite zu einer anderen mit derselben Peer-Gruppe. Es ist nicht auf dem Bildschirm Navigationselement (z. B. <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>), die direkte Navigation auf beiden Seiten bereitstellt.</p></td>
<td style="vertical-align:top;"><strong>"Ja"</strong>
<p>In der folgenden Abbildung wird der Benutzer navigiert zwischen zwei Seiten in der gleichen Peer-Gruppe und die Navigation zum Navigationsverlauf hinzugefügt werden sollen.</p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-noosnavelement.png" alt="Navigation within a peer group" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Seite zu Seite derselben Peer-Gruppe, mit einer auf dem Bildschirm Navigationselement</strong>
<p>Der Benutzer navigiert von einer Seite zu einer anderen in derselben Peer-Gruppe. Beide Seiten werden in das gleiche Navigationselement, dargestellt, wie z. B. <a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview">NavigationView</a>.</p></td>
<td style="vertical-align:top;"><strong>Es abhängig ist.</strong>
<p>Ja, zum Navigationsverlauf, zwei wichtige Ausnahmen hinzufügen. Fügen Sie Wenn Sie erwarten, dass Benutzer Ihrer App wechseln Sie zwischen den Seiten in der Peer-Gruppe häufig oder wenn Sie die Navigationshierarchie beibehalten möchten, klicken Sie dann keine dem Navigationsverlauf. Wenn der Benutzer in diesem Fall die Zurück-Schaltfläche drückt, wird wieder die letzte Seite aufgerufen, die geöffnet war, bevor der Benutzer zur aktuellen Peer-Gruppe navigierte. </p>
<p><img src="images/back-nav/nav-pagetopage-samepeer-yesosnavelement.png" alt="Navigation across peer groups when a navigation element is present" /></p></td>
</tr>
<tr class="even">
<td style="vertical-align:top;"><strong>Eine vorübergehende Benutzeroberfläche anzeigen</strong>
<p>Die App zeigt ein Popupfenster oder ein untergeordnetes Fenster an, z. B. ein Dialogfeld, einen Begrüßungsbildschirm oder eine Bildschirmtastatur. Anderenfalls wechselt die App in einen speziellen Modus, z. B. den Mehrfachauswahlmodus.</p></td>
<td style="vertical-align:top;"><strong>Nein</strong>
<p>Wenn der Benutzer die Zurück-Schaltfläche drückt, sollte die vorübergehende Benutzeroberfläche geschlossen werden (durch Ausblenden der Bildschirmtastatur, Abbrechen des Dialogfelds usw.) und zur Seite zurückgekehrt werden, die die vorübergehende Benutzeroberfläche aufgerufen hat.</p>
<p><img src="images/back-nav/back-transui.png" alt="Showing a transient UI" /></p></td>
</tr>
<tr class="odd">
<td style="vertical-align:top;"><strong>Auflisten von Elementen</strong>
<p>Die App zeigt die Inhalte für ein Bildschirmelement an, z. B. die Details für das ausgewählte Element in der Master/Details-Liste.</p></td>
<td style="vertical-align:top;"><strong>Nein</strong>
<p>Das Aufzählen von Elementen ist mit der Navigation innerhalb einer Peer-Gruppe vergleichbar. Wenn der Benutzer die Zurück-Schaltfläche drückt, sollte zu der Seite navigiert werden, die vor der aktuellen Seite angezeigt wurde, die die Elementenaufzählung enthält.</p>
<p><img src="images/back-nav/nav-enumerate.png" alt="Iterm enumeration" /></p></td>
</tr>
</tbody>
</table>
</div>

### <a name="resuming"></a>Fortsetzen

Wenn der Benutzer zu einer anderen App wechselt und zu Ihrer App zurückkehrt, wird empfohlen, zur letzten Seite im Navigationsverlauf zurückzukehren.

## <a name="related-articles"></a>Verwandte Artikel

- [Navigationsgrundlagen](navigation-basics.md)
