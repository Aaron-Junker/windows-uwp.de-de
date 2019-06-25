---
description: Eine Art von Pinsel, der eine durchscheinende Textur erzeugt.
title: Acryl-Material
template: detail.hbs
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
pm-contact: yulikl
design-contact: rybick
dev-contact: jevansa
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 4731ab089189a8a03656281d1a9a6da6e4d24e89
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/13/2019
ms.locfileid: "65984259"
---
# <a name="acrylic-material"></a>Acryl-Material

![Herobild](images/header-acrylic.svg)

Acryl ist eine Art von [Pinsel](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Media.Brush), der eine durchscheinende Textur erzeugt. Sie können Acryl auf App-Oberflächen anwenden, um Tiefe hinzuzufügen und eine visuelle Hierarchie herzustellen.  <!-- By allowing user-selected wallpaper or colors to shine through, acrylic keeps users in touch with the OS personalization they've chosen. -->

> **Wichtige APIs:** [AcrylicBrush-Klasse](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.media.acrylicbrush), [Background-Eigenschaft](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.control.Background)

:::row:::
    :::column:::
        Acrylic in light theme
        ![Acrylic in light theme](images/Acrylic_LightTheme_Base.png)
    :::column-end:::
    :::column:::
        Acrylic in dark theme
        ![Acrylic in dark theme](images/Acrylic_DarkTheme_Base.png)
    :::column-end:::
:::row-end:::

## <a name="acrylic-and-the-fluent-design-system"></a>Acryl und das Fluent Design-System

 Mit dem Fluent Design-System erstellen Sie moderne Oberflächen, die Licht, Tiefe, Bewegung, Material und Skalierungsmöglichkeiten beinhalten. Acryl ist eine Komponente des Fluent Design-Systems, die Ihrer App physische Struktur (Material) und Tiefe hinzufügt. Weitere Informationen finden Sie in der [Übersicht über Fluent Design für UWP](/windows/apps/fluent-design-system).

 ## <a name="video-summary"></a>Videozusammenfassung

> [!VIDEO https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Fall-Creators-Update/WinDev002/player]

## <a name="examples"></a>Beispiele

:::row:::
    :::column span:::
![Ein Bild](images/XAML-controls-gallery-app-icon.png)
    :::column-end:::
    :::column span="2":::
**XAML-Steuerelementekatalog**<br>
Wenn Sie die XAML-Steuerelementekatalog-App installiert haben, klicken Sie <a href="xamlcontrolsgallery:/item/Acrylic">hier</a>, um die App zu öffnen und Acryl in Aktion zu sehen.

<a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a><br>
<a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a>
    :::column-end:::
:::row-end:::

## <a name="acrylic-blend-types"></a>Acrylmischungen
Die auffälligste Eigenschaft von Acryl ist seine Transparenz. Es gibt zwei Acrylmischungen, die beeinflussen, welche Inhalte durch das Material sichtbar sind:
 - **Hintergrund-Acryl** zeigt den Desktophintergrund und andere Fenster, die sich hinter der derzeit aktiven App befinden. Dabei wird Tiefe zwischen den Fenstern der Anwendung hinzugefügt, während die Personalisierungseinstellungen des Benutzers angewendet werden.
 - **In-App-Acryl** fügt Tiefenwirkung innerhalb des App-Frames hinzu, wodurch sowohl Fokus als auch Hierarchie erzeugt werden.

 ![Hintergrund-Acryl](images/BackgroundAcrylic_DarkTheme.png)

 ![In-App-Acryl](images/AppAcrylic_DarkTheme.png)

 Seien Sie beim Schichten mehrerer Acryloberflächen vorsichtig: Mehrere Schichten von Hintergrund-Acryl können ablenkende optische Täuschungen verursachen.

## <a name="when-to-use-acrylic"></a>Wann sollte Acryl verwendet werden?

* Verwenden Sie In-App-Acryl zur Unterstützung der Benutzeroberfläche, z. B. auf Oberflächen, die beim Scrollen oder während der Interaktion mit ihnen Inhalte überlappen können.
* Verwenden Sie Hintergrund-Acryl für kurzlebige Benutzeroberflächenelemente wie Kontextmenüs, Flyouts und einfach ausblendbare Benutzeroberflächenelemente.<br />Die Verwendung von Acryl in kurzlebigen Szenarios hilft dabei, eine visuelle Beziehung zu dem Inhalt aufrechtzuerhalten, von dem das kurzlebige Benutzeroberflächenelement ausgelöst wurde.

Bei Verwendung von In-App-Acryl auf Navigationsoberflächen sollten Sie erwägen, den Inhalt unter dem Acrylbereich auszudehnen, um den Flow in Ihrer App zu verbessern. „NavigationView“ erledigt dies automatisch für Sie. Um einen Streifenbildungseffekt zu vermeiden, versuchen Sie nicht, mehrere Acrylelemente Rand an Rand zu platzieren, da dies zu einem unerwünschten Rand zwischen den beiden weichgezeichneten Oberflächen führen kann. Acryl ist ein Tool zum Erzeugen von visueller Harmonie in Ihren Designs, bei inkorrekter Verwendung können sich jedoch visuelle Störungen ergeben.

Berücksichtigen Sie die folgenden Verwendungsmuster, um zu entscheiden, wie sich Acryl am besten in Ihre App integrieren lässt:

### <a name="horizontal-navigation-or-commanding"></a>Horizontale Navigation oder Steuerung

Wenn sich „NavigationView“ in Ihrer App nicht verwenden lässt und Sie Acryl selbst hinzufügen möchten, empfehlen wir die Verwendung von relativ durchscheinendem Acryl mit einer Farbton-Deckkraft von 60 %.
 - Wenn der Bereich als Überlagerung von anderen Inhalten der App geöffnet wird, sollte dies [60 % In-App-Acryl](#acrylic-theme-resources) sein.

![Karten-App mit horizontaler In-App-Steuerung](images/Maps_In_App_Acrylic_1.png)

Zusätzlich erhält Ihre App dadurch, dass sich Ihr Inhalt unter dem oben liegenden Acryl ausdehnt oder scrollt, eine immersivere und nahtlosere Erfahrung.

### <a name="vertical-panes"></a>Vertikale Bereiche

Für vertikale Bereiche oder Oberflächen, die dabei helfen, Inhalte Ihrer App abzuteilen, empfehlen wir, dass Sie einen nicht transparenten Hintergrund anstelle von Acryl verwenden. Wenn sich Ihre vertikalen Bereiche auf bzw. vor Inhalten öffnen, wie in den Modi **Compact** oder **Minimal** von „NavigationView “, sollten Sie In-App-Acryl verwenden, um den Kontext der Seite beizubehalten, wenn der Benutzer diesen Bereich geöffnet hat.

### <a name="transient-surfaces"></a>Kurzlebige Oberflächen

Für Apps mit Menü-Flyouts, nicht modalen Popups oder einfach ausblendbaren Bereichen empfiehlt sich die Verwendung von Hintergrund-Acryl.

![E-Mail-App-Muster mit einem Informations-Flyout](images/Mail_TransientContextMenu.png)

Viele unserer Steuerelemente verwenden standardmäßig Acryl. [MenuFlyouts](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/menus), [AutoSuggestBox](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/auto-suggest-box), [ComboBox](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.combobox) und ähnliche Steuerelemente mit einfach ausblendbaren Popups verwenden alle das kurzlebige Acryl, wenn sie aufgerufen werden.

> [!Note]
> Das Rendern von Acryloberflächen ist GPU-intensiv, wodurch der Energieverbrauch des Geräts erhöht und die Akkulaufzeit verkürzt werden kann. Acryleffekte werden automatisch deaktiviert, wenn Geräte in den Stromsparmodus versetzt werden, und die Benutzer können Acryleffekte für alle Apps wahlweise deaktivieren.

## <a name="usability-and-adaptability"></a>Benutzerfreundlichkeit und Anpassungsfähigkeit
Acryl passt seine Darstellung automatisch an eine Vielzahl von Geräten und Kontexten an.

Im Modus mit hohem Kontrast wird Benutzern anstelle von Acryl weiterhin die vertraute Hintergrundfarbe ihrer Wahl angezeigt. Darüber hinaus werden sowohl das Hintergrund- als auch das In-App-Acryl in den folgenden Fällen als Volltonfarben angezeigt:
 - Bei Deaktivierung der Transparenz in „Einstellungen“ > „Personalisierung“ > „Farbe“
 - Bei aktivem Stromsparmodus
 - Bei Ausführen der App auf Low-End-Hardware

Darüber hinaus werden in den folgenden Fällen nur beim Hintergrund-Acryl Durchsichtigkeit und Textur durch eine Volltonfarbe ersetzt:
 - Bei Deaktivierung eines App-Fensters auf dem Desktop
 - Bei Ausführen der UWP-App auf einem Telefon, der Xbox, HoloLens oder einem Tablet

### <a name="legibility-considerations"></a>Hinweise zur Lesbarkeit
Es muss sichergestellt werden, dass Texte, die in der App dargestellt werden, [die Kontrastverhältnisse erfüllen](../accessibility/accessible-text-requirements.md). Wir haben die Acrylzusammensetzung optimiert, damit Texte in Schwarz oder Weiß mit hoher Farbauflösung oder in Mittelgrau auf Acryl die Kontrastverhältnisse erfüllen. Die von der Plattform bereitgestellten Designressourcen nehmen standardmäßig kontrastierende Färbungen mit 80 % Deckkraft an. Beim Platzieren von Textkörper mit hoher Farbauflösung auf Acryl können Sie die Farbton-Deckkraft verringern und gleichzeitig die Lesbarkeit erhalten. Im dunklen Modus kann die Farbton-Deckkraft 70 % betragen, während das Acryl im hellen Modus ein Kontrastverhältnis von 50 % Deckkraft erfüllt.

Es wird davon abgeraten, akzentfarbigen Text auf Ihren Acryloberflächen zu platzieren, da diese Kombinationen bei einem Schriftgrad von 15px wahrscheinlich nicht die Mindestanforderungen an das Kontrastverhältnis erfüllen. Platzieren Sie möglichst keine [Hyperlinks](../controls-and-patterns/hyperlinks.md) über Acrylelementen. Vergessen Sie darüber hinaus nicht die Auswirkungen auf die Lesbarkeit, wenn Sie den Acrylfarbton oder die Deckkraftstufe auf Werte außerhalb der von der Designressource bereitgestellten Plattformstandardeinstellungen einstellen möchten.

## <a name="acrylic-theme-resources"></a>Acryl-Designressourcen
Mit der neuen Designressource „XAML AcrylicBrush“ oder der vordefinierten „AcrylicBrush“ können Sie Acryl ganz einfach auf die Oberflächen Ihrer App anwenden. Zunächst müssen Sie entscheiden, ob Sie In-App- oder Hintergrund-Acryl verwenden möchten. Prüfen Sie die zuvor in diesem Artikel beschriebenen allgemeinen App-Muster auf Empfehlungen.

Wir haben eine Sammlung von Pinsel-Designressourcen sowohl für Hintergrund- als auch In-App-Acrylarten erstellt, die das Design der App berücksichtigen und nach Bedarf wieder auf Volltonfarben zurückgreifen. Ressourcen mit der Benennung *AcrylicWindow* stellen Hintergrund-Acryl dar, während sich *AcrylicElement* auf In-App-Acryl bezieht.

<table>
    <tr>
        <th align="center">Ressourcenschlüssel</th>
        <th align="center">Farbton-Deckkraft</th>
        <th align="center"><a href="color.md">Fallbackfarbe</a> </th>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowBrush, SystemControlAcrylicElementBrush <br/> SystemControlChromeLowAcrylicWindowBrush, SystemControlChromeLowAcrylicElementBrush <br/> SystemControlBaseHighAcrylicWindowBrush, SystemControlBaseHighAcrylicElementBrush <br/> SystemControlBaseLowAcrylicWindowBrush, SystemControlBaseLowAcrylicElementBrush <br/> SystemControlAltHighAcrylicWindowBrush, SystemControlAltHighAcrylicElementBrush <br/> SystemControlAltLowAcrylicWindowBrush, SystemControlAltLowAcrylicElementBrush </td>
        <td align="center"> 80 % </td>
        <td> ChromeMedium <br/> ChromeLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltHigh <br/><br/> AltLow </td>
    </tr>
    </tr>
        <td> <b>Empfohlene Verwendung:</b> Hierbei handelt es sich um allgemeine Acrylressourcen, die sich in einer Vielzahl von Verwendungsarten gut einsetzen lassen. Wenn Ihre App sekundären Text in der Farbe „AltMedium“ mit einer kleineren Textgröße als 18px verwendet, platzieren Sie eine Acrylressource von 80 % hinter dem Text, um die <a href="../accessibility/accessible-text-requirements.md">Anforderungen an das Kontrastverhältnis zu erfüllen</a>. </td>
    </tr>
    <tr>
        <td> SystemControlAcrylicWindowMediumHighBrush, SystemControlAcrylicElementMediumHighBrush <br/> SystemControlBaseHighAcrylicWindowMediumHighBrush, SystemControlBaseHighAcrylicElementMediumHighBrush </td>
        <td align="center"> 70 % </td>
        <td> ChromeMedium <br/><br/> BaseHigh </td>
    </tr>
    <tr>
        <td> <b>Empfohlene Verwendung:</b> Wenn Ihre App sekundären Text in der Farbe AltMedium mit einer Textgröße von 18px oder mehr verwendet, können Sie diese 70 % durchscheinenden Acrylressourcen hinter dem Text platzieren. Wir empfehlen die Verwendung dieser Ressourcen in den oberen horizontalen Navigations- und Steuerungsbereichen in Ihrer App.  </td>
    </tr>
    <tr>
        <td> SystemControlChromeHighAcrylicWindowMediumBrush, SystemControlChromeHighAcrylicElementMediumBrush <br/> SystemControlChromeMediumAcrylicWindowMediumBrush, SystemControlChromeMediumAcrylicElementMediumBrush <br/> SystemControlChromeMediumLowAcrylicWindowMediumBrush, SystemControlChromeMediumLowAcrylicElementMediumBrush <br/> SystemControlBaseHighAcrylicWindowMediumBrush, SystemControlBaseHighAcrylicElementMediumBrush <br/> SystemControlBaseMediumLowAcrylicWindowMediumBrush, SystemControlBaseMediumLowAcrylicElementMediumBrush <br/> SystemControlAltMediumLowAcrylicWindowMediumBrush, SystemControlAltMediumLowAcrylicElementMediumBrush  </td>
        <td align="center"> 60 % </td>
        <td> ChromeHigh <br/><br/> ChromeMedium <br/><br/> ChromeMediumLow <br/><br/> BaseHigh <br/><br/> BaseLow <br/><br/> AltMediumLow </td>
    </tr>
    <tr>
        <td> <b>Empfohlene Verwendung:</b> Wenn nur primärer Text der Farbe AltHigh über Acryl platziert wird, kann Ihre App diese Ressourcen von 60 % verwenden. Es wird empfohlen, den <a href="../controls-and-patterns/navigationview.md">vertikalen Navigationsbereich</a> Ihrer App, d. h. das Hamburger-Menü, mit 60 % Acryl zu zeichnen. </td>
    </tr>
</table>

Zusätzlich zu farbneutralem Acryl haben wir Ressourcen hinzugefügt, mit denen sich das Acryl mithilfe von benutzerdefinierten Akzentfarben färben lässt. Wir empfehlen, farbiges Acryl sparsam zu verwenden. Platzieren Sie für die bereitgestellten Varianten „dark1“ und „dark2“ weißen bzw. hellfarbigen Text der Textfarbe des dunklen Designs entsprechend über diesen Ressourcen.
<table>
    <tr>
        <th align="center">Ressourcenschlüssel</th>
        <th align="center">Farbton-Deckkraft</th>
        <th align="center"><a href="color.md">Farbton und Fallbackfarben</a> </th>
    </tr>
    <tr>
        <td> SystemControlAccentAcrylicWindowAccentMediumHighBrush, SystemControlAccentAcrylicElementAccentMediumHighBrush  </td>
        <td align="center"> 70 % </td>
        <td> SystemAccentColor </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark1AcrylicWindowAccentDark1Brush, SystemControlAccentDark1AcrylicElementAccentDark1Brush  </td>
        <td align="center"> 80 % </td>
        <td> SystemAccentColorDark1 </td>
    </tr>
    <tr>
        <td> SystemControlAccentDark2AcrylicWindowAccentDark2MediumHighBrush, SystemControlAccentDark2AcrylicElementAccentDark2MediumHighBrush  </td>
        <td align="center"> 70 % </td>
        <td> SystemAccentColorDark2 </td>
    </tr>
</table>


Um eine bestimmte Oberfläche zu zeichnen, wenden Sie eines der oben genannten Designressourcen auf Elementhintergründe an. Verfahren Sie dabei genauso, wie Sie die anderen Pinselressourcen anwenden würden.

```xaml
<Grid Background="{ThemeResource SystemControlAcrylicElementBrush}">
```

## <a name="custom-acrylic-brush"></a>Benutzerdefinierter Acrylpinsel
Sie können dem Acryl Ihrer App einen Farbton hinzufügen, um das Branding anzuzeigen oder ein optisches Gleichgewicht mit anderen Elementen auf der Seite zu erzeugen. Um Farbe und keine Graustufen anzuzeigen, müssen Sie Ihre eigenen Acrylpinsel mithilfe der folgenden Eigenschaften definieren.
 - **TintColor**: die Überlagerungsschicht der Farbe/des Farbtons. Sie sollten sowohl den RGB-Farbwert als auch die Deckkraft des Alphakanals angeben.
 - **TintOpacity**: die Deckkraft der Farbtonschicht. Wir empfehlen 80 % Deckkraft als Ausgangspunkt, obwohl verschiedene Farben bei anderer Durchsichtigkeit möglicherweise ansprechender aussehen.
 - **TintLuminosityOpacity**: steuert die Höhe der Sättigung, die zulässig ist, durch die Acryloberfläche des Hintergrunds.
 - **BackgroundSource**: die Kennzeichnung zum Festlegen, ob Sie Hintergrund- oder In-App Acryl verwenden möchten.
 - **FallbackColor**: die Volltonfarbe, durch die Acryl im Stromsparmodus ersetzt wird. Im Fall von Hintergrund-Acryl ersetzt die Fallbackfarbe das Acryl auch, wenn Ihre App nicht im aktiven Desktopfenster angezeigt wird oder wenn die App auf dem Telefon oder der Xbox ausgeführt wird.

![Acrylmuster für das helle Design](images/CustomAcrylic_Swatches_LightTheme.png)

![Acrylmuster für das dunkle Design](images/CustomAcrylic_Swatches_DarkTheme.png)

![Deckkraft der Leuchtdichte im Vergleich zur Farbton-Deckkraft](images/LuminosityVersusTint.png)

Um einen Acrylpinsel hinzuzufügen, definieren Sie die Ressourcen für das dunkle und das helle Design und für das Design mit hohem Kontrast. Beachten Sie, dass wir bei hohem Kontrast die Verwendung eines „SolidColorBrush“ mit demselben X:Key wie für den dunklen/hellen AcrylicBrush empfehlen.

> [!Note] 
> Wenn Sie keinen „TintLuminosityOpacity“-Wert angeben, passt das System seinen Wert automatisch auf Grundlage Ihrer Werte für „TintColor“ und „TintOpacity“ an.

```xaml
<ResourceDictionary.ThemeDictionaries>
    <ResourceDictionary x:Key="Default">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FF7F0000"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="HighContrast">
        <SolidColorBrush x:Key="MyAcrylicBrush"
            Color="{ThemeResource SystemColorWindowColor}"/>
    </ResourceDictionary>

    <ResourceDictionary x:Key="Light">
        <AcrylicBrush x:Key="MyAcrylicBrush"
            BackgroundSource="HostBackdrop"
            TintColor="#FFFF0000"
            TintOpacity="0.8"
            TintLuminosityOpacity="0.5"
            FallbackColor="#FFFF7F7F"/>
    </ResourceDictionary>
</ResourceDictionary.ThemeDictionaries>
```

Das folgende Beispiel zeigt, wie Sie AcrylicBrush in Code deklarieren. Wenn Ihre App mehrere Betriebssystemziele unterstützt, müssen Sie sicherstellen, dass diese API auf dem Gerät des Benutzers verfügbar ist.

```csharp
if (Windows.Foundation.Metadata.ApiInformation.IsTypePresent("Windows.UI.Xaml.Media.XamlCompositionBrushBase"))
{
    Windows.UI.Xaml.Media.AcrylicBrush myBrush = new Windows.UI.Xaml.Media.AcrylicBrush();
    myBrush.BackgroundSource = Windows.UI.Xaml.Media.AcrylicBackgroundSource.HostBackdrop;
    myBrush.TintColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.FallbackColor = Color.FromArgb(255, 202, 24, 37);
    myBrush.TintOpacity = 0.6;

    grid.Fill = myBrush;
}
else
{
    SolidColorBrush myBrush = new SolidColorBrush(Color.FromArgb(255, 202, 24, 37));

    grid.Fill = myBrush;
}
```

## <a name="extend-acrylic-into-the-title-bar"></a>Ausdehnen von Acryl in die Titelleiste

Um Ihrem App-Fenster ein nahtloses Aussehen zu verleihen, können Sie im Acryl im Titelleistenbereich verwenden. In diesem Beispiel wird Acryl in die Titelleiste ausgedehnt, indem die [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar) der Eigenschaften des Objekts [ButtonBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonBackgroundColor) und [ButtonInactiveBackgroundColor](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewTitleBar.ButtonInactiveBackgroundColor) auf [Colors.Transparent](https://docs.microsoft.com/uwp/api/Windows.UI.Colors.Transparent) festgelegt werden.

```csharp
private void ExtendAcrylicIntoTitleBar()
{
    CoreApplication.GetCurrentView().TitleBar.ExtendViewIntoTitleBar = true;
    ApplicationViewTitleBar titleBar = ApplicationView.GetForCurrentView().TitleBar;
    titleBar.ButtonBackgroundColor = Colors.Transparent;
    titleBar.ButtonInactiveBackgroundColor = Colors.Transparent;
}
```

Dieser Code kann in die [OnLaunched](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application#Windows_UI_Xaml_Application_OnLaunched_Windows_ApplicationModel_Activation_LaunchActivatedEventArgs_)-Methode Ihre App eingefügt werden (_App.xaml.cs_), nach dem Aufruf von [Window.Activate](https://docs.microsoft.com/uwp/api/windows.ui.xaml.window.Activate) wie hier gezeigt, oder auf der ersten Seite Ihrer App.

```csharp
// Call your extend acrylic code in the OnLaunched event, after
// calling Window.Current.Activate.
protected override void OnLaunched(LaunchActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;

    // Do not repeat app initialization when the Window already has content,
    // just ensure that the window is active
    if (rootFrame == null)
    {
        // Create a Frame to act as the navigation context and navigate to the first page
        rootFrame = new Frame();

        rootFrame.NavigationFailed += OnNavigationFailed;

        if (e.PreviousExecutionState == ApplicationExecutionState.Terminated)
        {
            //TODO: Load state from previously suspended application
        }

        // Place the frame in the current Window
        Window.Current.Content = rootFrame;
    }

    if (e.PrelaunchActivated == false)
    {
        if (rootFrame.Content == null)
        {
            // When the navigation stack isn't restored navigate to the first page,
            // configuring the new page by passing required information as a navigation
            // parameter
            rootFrame.Navigate(typeof(MainPage), e.Arguments);
        }
        // Ensure the current window is active
        Window.Current.Activate();

        // Extend acrylic
        ExtendAcrylicIntoTitleBar();
    }
}
```

Darüber hinaus müssen Sie den Titel der App, der normalerweise automatisch in der Titelleiste angezeigt wird, mit einem TextBlock zeichnen, der `CaptionTextBlockStyle` verwendet. Weitere Informationen finden Sie unter [Anpassen der Titelleiste](../shell/title-bar.md).

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen
* Verwenden Sie Acryl als Hintergrundmaterial von nicht primären App-Oberflächen wie Navigationsbereichen.
* Dehnen Sie das Acryl auf mindestens einen Rand der App aus, um durch eine dezente Vermischung mit der Umgebung der App eine nahtlose Oberfläche bereitzustellen.
* Platzieren Sie kein Desktop-Acryl auf großen Hintergrundoberflächen Ihrer-App – dies verletzt die eigentliche Absicht von Acryl, das primär für kurzlebige Oberflächen verwendet wird.
* Platzieren Sie In-App- und Hintergrund-Acryl nicht direkt nebeneinander, um visuelle Spannung an den Rändern zu vermeiden.
* Platzieren Sie nicht mehrere Acrylbereiche mit demselben Farbton und derselben Deckkraft nebeneinander, da dies einen unerwünschten, sichtbaren Rand erzeugt.
* Platzieren Sie keinen akzentfarbigen Text über Acryloberflächen.

## <a name="how-we-designed-acrylic"></a>Unser Ansatz beim Entwurf von Acryl

Wir haben die Hauptkomponenten von Acryl optimiert, um seine individuelle Darstellung und einzigartigen Eigenschaften zu erhalten. Wir haben damit begonnen, flachen Oberflächen mithilfe von Durchsichtigkeit, Weichzeichnung und Rauschen visuelle Tiefe und Dimension hinzuzufügen. Dann haben wir eine Ausschluss-Mischmodus-Ebene hinzugefügt, um den Kontrast und die Lesbarkeit der auf dem Acrylhintergrund platzierten UI sicherzustellen. Zuletzt haben wir Farbtöne hinzugefügt, um Personalisierungen zu ermöglichen. Zusammen ergeben diese Ebenen ein neues, einsatzbereites Material.

![Acrylzusammensetzung](images/AcrylicRecipe_Diagram.jpg)
<br/>Das Acryl setzt sich folgendermaßen zusammen: Hintergrund, Weichzeichnung, Ausschluss-Mischung, Überlagerung der Farbe/des Farbtons, Rauschen


## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

[**Reveal Highlight**](reveal.md)
