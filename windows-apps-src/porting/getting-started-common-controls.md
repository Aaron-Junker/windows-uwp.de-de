---
ms.assetid: E2B73380-D673-48C6-9026-96976D745017
description: Erste Schritte mit allgemeinen Steuerelementen
title: Erste Schritte mit allgemeinen Steuerelementen
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: a64dbd6a9530f81c55b0d4b52e4c0fd55c4b9956
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372828"
---
# <a name="getting-started-common-controls"></a>Erste Schritte: Allgemeine Steuerelemente


## <a name="common-controls-list"></a>Liste „Allgemeine Steuerelemente“

Im vorherigen Abschnitt haben Sie mit nur zwei Steuerelementen gearbeitet: Schaltflächen und Textblöcken. Es gibt selbstverständlich viele weitere Steuerelemente, die Ihnen zur Verfügung stehen. Hier sind einige gängige Steuerelemente, die Sie in Ihren Apps und unter iOS verwenden können. Die iOS-Steuerelemente sind in alphabetischer Reihenfolge neben den entsprechenden UWP-Steuerelementen aufgeführt.

Das Besondere bei UWP-Steuerelementen ist, dass sie den Typ des Geräts erkennen, auf dem sie ausgeführt werden, und ihre Darstellung und Funktionalität entsprechend ändern können. Wenn Ihr Projekt z. B. das [**DatePicker**](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10))-Steuerelement verwendet, ist dieses in der Lage, seine Darstellung und sein Verhalten auf einem Desktopcomputer im Vergleich zu einem Telefon entsprechend zu optimieren und anzupassen. Sie müssen nichts weiter unternehmen: die Steuerelemente passen sich automatisch zur Laufzeit an.

| iOS-Steuerelement (Klasse/Protokoll) | Entsprechendes UWP-Steuerelement |
|------------------------------|--------------------------------------|
| Aktivitätsanzeige (**UIActivityIndicatorView**) | [**ProgressRing**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing) <br/> Siehe auch [Schnellstart: Hinzufügen von Statussteuerelementen](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Anzeigenbanner-Ansicht (**ADBannerView**) und Anzeigenbanner-Ansichtsdelegat (**ADBannerViewDelegate**) | [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) <br/> Siehe auch [Anzeigen von Werbung in Ihrer App](../monetize/display-ads-in-your-app.md) |
| Schaltfläche (UIButton) | [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) (Schaltfläche) <br/> Siehe auch [Schnellstart: Hinzufügen von Schaltflächen-Steuerelemente](https://docs.microsoft.com/previous-versions/windows/apps/jj153346(v=win.10)) |
| Datumsauswahl (UIDatePicker) | [DatePicker](https://docs.microsoft.com/previous-versions/windows/apps/br211681(v=win.10)) |
| Bildansicht (UIImageView) | [Image](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) <br/> Siehe auch [Image und ImageBrush](https://docs.microsoft.com/windows/uwp/controls-and-patterns/images-imagebrushes) |
| Bezeichnung (UILabel) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/> Siehe auch [Schnellstart: Anzeigen von Text](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Kartenansicht (MKMapView) und Kartenansichtsdelegat (MKMapViewDelegate) | Finden Sie unter [Bing Maps für UWP-apps](https://go.microsoft.com/fwlink/p/?LinkId=263496) |
| Navigationscontrollerdelegat (UINavigationController) und Navigationscontrollerdelegat (UINavigationControllerDelegate) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/> Siehe auch [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Seitensteuerung (UIPageControl) | [Seite](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/> Siehe auch [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Auswahlansicht (UIPickerView) und Auswahlansichtsdelegat (UIPickerViewDelegate) | [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) <br/> Siehe auch [Hinzufügen von Kombinationsfeldern und Listenfeldern](https://docs.microsoft.com/previous-versions/windows/apps/hh780616(v=win.10)) |
| Statusleiste (UIProgressView) | [ProgressBar](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar) <br/> Siehe auch [Schnellstart: Hinzufügen von Statussteuerelementen](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10)) |
| Bildlaufansicht (UIScrollView) und Bildlaufansichtsdelegat (UIScrollViewDelegate) | [ScrollViewer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer) <br/>  Siehe auch [Beispiele zu Bildlauf, Verschieben und Zoomen von Extensible Application Markup Language (XAML)](https://go.microsoft.com/fwlink/p/?LinkId=238577) |
| Suchleiste (UISearchBar) und Suchleistendelegat (UISearchBarDelegate) | Siehe [Hinzufügen von Suchfunktionen zu einer App](https://docs.microsoft.com/previous-versions/windows/apps/jj130767(v=win.10)) <br/>  Siehe auch [Schnellstart: Hinzufügen von Suchfunktionen zu einer app](https://docs.microsoft.com/previous-versions/windows/apps/hh868180(v=win.10)) |
| Segmentiertes Steuerelement (UISegmentedControl) | Keine |
| Schieberegler (UISlider) | [Schieberegler](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) <br/>  Siehe auch [So wird's gemacht: Hinzufügen eines Schiebereglers](https://docs.microsoft.com/previous-versions/windows/apps/hh868197(v=win.10)) |
| Controller für geteilte Ansicht (UISplitViewController) und Controllerdelegat für geteilte Ansicht (UISplitViewControllerDelegate) | Keine |
| Schalter (UISwitch) | [ToggleSwitch](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ToggleSwitch) <br/>  Siehe auch [So wird's gemacht: Hinzufügen von Umschaltern](https://docs.microsoft.com/previous-versions/windows/apps/hh868198(v=win.10)) |
| Registerkartenleisten-Controller (UITabBarController) und Registerkartenleisten-Controllerdelegat (UITabBarControllerDelegate) | Keine |
| Tabellenansichtscontroller (UITableViewController), Tabellenansicht (UITableView), Tabellenansichtsdelegat (UITableViewDelegate) und Tabellenzelle (UITableViewCell) | [ListView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView) <br/>  Siehe auch [Schnellstart: Hinzufügen von ListView- und GridView-Steuerelementen](https://docs.microsoft.com/previous-versions/windows/apps/hh780650(v=win.10)) |
| Textfeld (UITextField) und Textfelddelegat (UITextFieldDelegate) | [TextBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) <br/>  Siehe auch [Anzeigen und Bearbeiten von Text](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/text-controls) |
| Textansicht (UITextView) und Textansichtsdelegat (UITextViewDelegate) | [TextBlock](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) <br/>  Siehe auch [Schnellstart: Anzeigen von Text](https://docs.microsoft.com/previous-versions/windows/apps/hh700392(v=win.10)) |
| Ansicht (UIView) und Ansichtscontroller (UIViewController) | [Seite](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) <br/>  Siehe auch [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |
| Webansicht (UIWebView) und Webansichtsdelegat (UIWebViewDelegate) | [WebView](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) <br/>  Siehe auch [Beispiel für XAML-WebView-Steuerelement](https://go.microsoft.com/fwlink/p/?LinkId=238582) |
| Fenster (UIWindow) | [Frame](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Frame) <br/>  Siehe auch [Navigation](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) |

Unter [Steuerelementliste](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/) finden Sie noch mehr Steuerelemente.

**Beachten Sie**  finden Sie eine Liste der Steuerelemente für die UWP-apps mit JavaScript und HTML, [Liste Steuerelemente](https://docs.microsoft.com/previous-versions/windows/apps/hh465453(v=win.10)).

### <a name="next-step"></a>Nächster Schritt

[Erste Schritte: Navigation](getting-started-navigation.md)

## <a name="related-topics"></a>Verwandte Themen

* [build 2014: Was ist XAML-Benutzeroberfläche und Steuerelemente?](https://go.microsoft.com/fwlink/p/?LinkID=397897)
* [build 2014: Entwickeln von Apps mit der allgemeinen XAML UI-Frameworks](https://go.microsoft.com/fwlink/p/?LinkID=397898)
* [build 2014: Mithilfe von Visual Studio zum Erstellen von XAML Converged Apps](https://go.microsoft.com/fwlink/p/?LinkID=397876)
