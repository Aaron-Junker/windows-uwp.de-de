---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: Diese Fallstudie baut auf den Informationen in Bookstore1 auf, beginnend mit einer universellen 8.1-App, die gruppierte Daten in einem SemanticZoom-Steuerelement anzeigt.
title: Windows Runtime 8.x zu UWP – Fallstudie Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fb3fdd324caa54c6965ae63485b5b4f264980d7e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259131"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Windows Runtime 8.x zu UWP – Fallstudie: Bookstore2


Diese Fallstudie baut auf den Informationen in [Bookstore1](w8x-to-uwp-case-study-bookstore1.md) auf und beginnt mit einer universellen 8.1-App, die gruppierte Daten in einem [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)-Steuerelement anzeigt. Im Ansichtsmodell stellt jede Instanz der **Author**-Klasse die Gruppe der vom betreffenden Autor verfassten Titel dar. In **SemanticZoom** können wir dann entweder die Bücherliste nach Autoren gruppiert anzeigen oder die Liste verkleinern, um eine Sprungliste der Autoren zu erhalten. Die Sprungliste ermöglicht eine wesentlich schnellere Navigation im Vergleich zum Blättern in der Bücherliste. Wir führen Sie durch die Schritte zum Portieren der APP auf eine Windows 10 universelle Windows-Plattform-app (UWP).

**Beachten** Sie   Wenn Sie Bookstore2Universal\_10 in Visual Studio öffnen, wenn die Meldung "Visual Studio-Update erforderlich" angezeigt wird, führen Sie die Schritte in " [targetplatformversion](w8x-to-uwp-troubleshooting.md)" aus.

## <a name="downloads"></a>Downloads

[Laden Sie die app "Bookstore2\_81 Universal 8,1" herunter](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2_81).

[Laden Sie die Bookstore2Universal\_10 Windows 10-APP herunter](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore2Universal_10).

## <a name="the-universal-81-app"></a>Die universelle 8.1-App

Im folgenden sehen Sie, was Bookstore2\_81 – die APP, die wir portieren werden – sieht wie folgt aus:. Über einen [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) mit horizontalem Bildlauf (Windows Phone: vertikalem Bildlauf) werden Bücher nach Autoren gruppiert angezeigt. Sie können die Liste auf die Sprungliste verkleinern und von dort aus wieder zurück zu einer beliebigen Gruppe navigieren. Die App besteht aus zwei Hauptteilen: dem Ansichtsmodell, das die gruppierte Datenquelle bereitstellt, und der Benutzeroberfläche, die an dieses Ansichtsmodell gebunden ist. Wie wir sehen werden, sind beide Teile leicht von der WinRT 8,1-Technologie zu Windows 10 portieren.

![Bookstore2\-81 unter Windows, Zoomansicht](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Bookstore2\_81 unter Windows, Zoomansicht
 

![Bookstore2\-81 unter Windows, Zoomansicht](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Bookstore2\_81 unter Windows, Zoomansicht

![Bookstore2\-81 auf Windows Phone, Zoomansicht](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Bookstore2\_81 auf Windows Phone, Zoomansicht

![Bookstore2\-81 auf Windows Phone, Zoomansicht](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Bookstore2\_81 auf Windows Phone, Zoomansicht

##  <a name="porting-to-a-windows10-project"></a>Portieren auf ein Windows 10-Projekt

Die Lösung Bookstore2\_81 ist ein universelles Universal App-Projekt in 8,1. Das Bookstore2\_81. Windows-Projekt erstellt das App-Paket für Windows 8.1, und das Bookstore2\_81. windowsphone-Projekt erstellt das App-Paket für Windows Phone 8,1. Bookstore2\_81. Shared ist das Projekt, das Quell Code, Markup Dateien und andere Assets und Ressourcen enthält, die von beiden anderen Projekten verwendet werden.

Genau wie bei der vorherigen Fallstudie ist die Option, die wir Unternehmen, [Wenn Sie über eine universelle 8,1-App verfügen](w8x-to-uwp-root.md), das Portieren des Inhalts des freigegebenen Projekts auf ein Windows 10-Ziel, das die universelle Gerätefamilie als Ziel hat.

Erstellen Sie zunächst ein neues Projekt vom Typ „Leere Anwendung“ (Windows Universal). Nennen Sie Sie Bookstore2Universal\_10. Dabei handelt es sich um die Dateien, die von Bookstore2\_81 nach Bookstore2Universal\_10 kopiert werden müssen.

**Aus dem freigegebenen Projekt**

-   Kopieren Sie den Ordner, der das Buch enthält, das das Buch enthält, das das Buch enthält (der Ordner ist \\\\coverimages). Vergewissern Sie sich nach dem Kopieren des Ordners im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf den kopierten Ordner, und klicken Sie dann auf **Zu Projekt hinzufügen**. Mit „Einschließen“ von Dateien oder Ordnern in einem Projekt meinen wir diesen Befehl. Klicken Sie jedes Mal, wenn Sie eine Datei oder einen Ordner kopieren, für jede Kopie im **Projektmappen-Explorer** auf **Aktualisieren**, und schließen Sie dann die Datei oder den Ordner in das Projekt ein. Dies ist nicht für Dateien erforderlich, die Sie am Ziel ersetzen.
-   Kopieren Sie den Ordner mit der Quelldatei für das Ansichts Modell (der Ordner ist \\ViewModel).
-   Kopieren Sie „MainPage.xaml“, und ersetzen Sie die Datei am Ziel.

**Aus dem Windows-Projekt**

-   Kopieren Sie „BookstoreStyles.xaml“. Diese wird als guter Ausgangspunkt verwendet, da alle Ressourcen Schlüssel in dieser Datei in einer Windows 10-App aufgelöst werden. einige der in der entsprechenden windowsphone-Datei werden nicht angezeigt.
-   Kopieren Sie SeZoUC.xaml und SeZoUC.xaml.cs. Wir beginnen mit der Windows-Version dieser Ansicht, die für breite Fenster geeignet ist, und passen sie später für kleinere Fenster und folglich kleinere Geräte an.

Bearbeiten Sie den Quell Code und die soeben kopierten Markup Dateien, und ändern Sie alle Verweise auf den Namespace Bookstore2\_81 auf Bookstore2Universal\_10. Eine schnelle Möglichkeit dafür ist die Verwendung des Features **In Dateien ersetzen**. Weder im Ansichtsmodell, noch in einem anderen imperativen Code sind Codeänderungen erforderlich. Wenn Sie jedoch leichter erkennen möchten, welche Version der app ausgeführt wird, ändern Sie den Wert, der von der **Bookstore2Universal\_10. bookstoreviewmodel. appname** -Eigenschaft zurückgegeben wird, von "Bookstore2\_81" in "Bookstore2Universal\_10".

Jetzt können Sie mit der Erstellung und Ausführung beginnen. Im folgenden wird erläutert, wie die neue UWP-App aussieht, nachdem Sie noch keine Arbeit mit Windows 10 hergestellt hat.

![Die Windows 10-App mit Änderungen am ursprünglichen Quellcode auf einem Desktopgerät, vergrößerte Ansicht](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

Die Windows 10-App mit anfänglichen Quell Codeänderungen, die auf einem Desktop Gerät ausgeführt werden, zoomansichts Ansicht

![Die Windows 10-App mit Änderungen am ursprünglichen Quellcode auf einem Desktopgerät, verkleinerte Ansicht](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

Windows 10-App mit anfänglichen Quell Codeänderungen, die auf einem Desktop Gerät ausgeführt werden, Zoomansicht

Das Ansichtsmodell und die vergrößerten sowie verkleinerten Ansichten arbeiten ordnungsgemäß zusammen, auch wenn man dies nicht auf Anhieb sieht. Ein Problem besteht darin, dass der [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) keinen Bildlauf durchführt. Dies liegt daran, dass in Windows 10 das Standardformat von [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) dazu veranlasst, dass es vertikal angeordnet wird (und die Entwurfs Richtlinien für Windows 10 empfehlen, dass es in neuen und in portierten apps auf diese Weise verwendet wird). Horizontale scrolleinstellungen in der Vorlage für benutzerdefinierte Elemente, die aus dem Projekt "Bookstore2\_81" kopiert wurden (für die 8,1-App entworfen), stehen jedoch in Konflikt mit den Einstellungen für den vertikalen Bildlauf im Standardformat von Windows 10, das als Ergebnis der Portierung von uns in eine Windows 10-App angewendet wird. Das zweite Problem besteht darin, dass sich die Benutzeroberfläche der App noch nicht anpasst, um eine bestmögliche Anzeige in verschieden großen Fenstern und auf kleinen Geräten zu ermöglichen. Drittens werden noch nicht die richtigen Stile und Pinsel verwendet, wodurch ein Großteil des Texts nicht sichtbar ist (einschließlich der Gruppenköpfe, auf die Sie zum Verkleinern klicken können). In den nächsten drei Abschnitten ([Designänderungen an semantischem Zoom und GridView](#semanticzoom-and-gridview-design-changes), [Adaptive UI](#adaptive-ui) und [Universelle Formatierung](#universal-styling)) werden wir diese drei Probleme beheben.

## <a name="semanticzoom-and-gridview-design-changes"></a>Designänderungen an SemanticZoom und GridView

Die Entwurfs Änderungen in Windows 10 für das [**semanticzoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) -Steuerelement werden im Abschnitt [semanticzoom-Änderungen](w8x-to-uwp-porting-xaml-and-ui.md)beschrieben. In diesem Abschnitt sind keine Überarbeitungen aufgrund der Änderungen erforderlich.

Die Änderungen an [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) werden im Abschnitt [Änderungen an „GridView/ListView”](w8x-to-uwp-porting-xaml-and-ui.md) beschrieben. Diese Änderungen erfordern einige geringfügige Anpassungen, die nachfolgend beschrieben werden.

-   Legen Sie in SeZoUC.xaml in der `ZoomedInItemsPanelTemplate``Orientation="Horizontal"` und `GroupPadding="0,0,0,20"` fest.
-   Löschen Sie in SeZoUC.xaml `ZoomedOutItemsPanelTemplate`, und entfernen Sie das `ItemsPanel`- Attribut aus der verkleinerten Ansicht.

Das war’s!

## <a name="adaptive-ui"></a>Adaptive UI

Nach dieser Änderung eignet sich das durch SeZoUC.xaml ermöglichte UI-Layout hervorragend zum Ausführen der App in einem breiten Fenster (nur auf Geräten mit großem Bildschirm möglich). Handelt es sich jedoch um ein schmales Fenster der App (auf kleinen Geräten, kann aber auch auf großen Geräten vorkommen), eignet sich die Benutzerfläche in der Windows Phone Store-App wohl am besten.

Wir können dazu auch das adaptive Visual State-Manager-Feature verwenden. Wir werden Eigenschaften für visuelle Elemente einrichten, sodass die Benutzeroberfläche mit den kleineren Vorlagen, die wir in der Windows Phone Store-App verwenden, standardmäßig im schmalen Zustand angeordnet wird. Dann werden wir erkennen, wann das Fenster der App breiter als eine bestimmte Größe ist bzw. dieser entspricht (gemessen in der Einheit [effektive Pixel](w8x-to-uwp-porting-xaml-and-ui.md)), und als Reaktion darauf die Eigenschaften der visuelle Elemente so ändern, dass wir ein größeres und breiteres Layout erhalten. Wir versetzen diese Eigenschaftsänderungen in einen visuellen Zustand und verwenden einen adaptiven Auslöser, um fortlaufend zu überwachen und zu bestimmen, ob der visuelle Zustand abhängig von der Breite des Fensters in effektiven Pixeln angewendet werden soll. Die Auslösung erfolgt hierbei anhand der Fensterbreite, aber sie kann auch anhand der Fensterhöhe erfolgen.

Eine minimale Fensterbreite von 548 Epx eignet sich in diesem Anwendungsfall, da dies der Größe des kleinsten Geräts entspricht, auf dem wir das breite Layout anzeigen können. Telefone sind in der Regel kleiner als 548 Epx, sodass wir auf solch einem kleinen Gerät das schmale Layout erhalten würden. Auf einem PC wird das Fenster standardmäßig so breit gestartet, dass der Wechsel zum breiten Zustand ausgelöst wird. Dort können Sie das Fenster schmaler ziehen, um zwei Spalten für Elemente der Größe 250 x 250 anzuzeigen. Ist es etwas schmaler, wird der Auslöser deaktiviert, der breite Ansichtszustand wird gelöscht, und das schmale Standardlayout wird aktiviert.

Welche Eigenschaften müssen wir also festlegen – und ändern – um diese beiden unterschiedlichen Layouts zu erreichen? Es gibt zwei Alternativen, für die jeweils ein anderer Ansatz erforderlich ist.

1.  Wir können zwei [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)-Steuerelemente in unserem Markup platzieren. Eine ist eine Kopie des Markups, die wir in der Windows-Runtime 8. x-App verwenden (mithilfe von [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) -Steuerelementen darin) und standardmäßig reduziert. Die andere Variante ist eine Kopie des Markups, das wir in der Windows Phone Store-App verwenden (mit [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)-Steuerelementen), und das standardmäßig eingeblendet sind. Der visuelle Zustand würde die Sichtbarkeitseigenschaften der beiden **SemanticZoom**-Steuerelemente verändern. Dies wäre keine schwierige Aufgabe, die Technik ist im Allgemeinen jedoch nicht besonders effizient. Wenn Sie sie verwenden, sollten Sie ein Profil Ihrer App erstellen und sicherstellen, dass sie Ihre Leistungsvorgaben noch erfüllt.
2.  Wir können einen einzelnen [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) mit [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView)-Steuerelementen verwenden. Zur Umsetzung unserer beiden Layouts würden wir im breiten visuellen Zustand die Eigenschaften der **ListView**-Steuerelemente ändern, einschließlich der darauf angewendeten Vorlagen, damit sie ebenso wie die [**GridView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) angeordnet werden. Das kann die Leistung verbessern, aber es gibt so viele feine Unterschiede zwischen den verschiedenen Stilen und Vorlagen der **GridView** und **ListView** und ihren verschiedenen Elementtypen, dass diese Lösung schwieriger umzusetzen ist. Diese Lösung ist zu diesem Zeitpunkt eng mit der Zuweisungsart der Standardstile und -vorlagen gekoppelt und daher für künftige Änderungen der Standards anfällig.

In dieser Fallstudie werden wir die erste Alternative untersuchen. Wenn Sie möchten, können Sie aber auch die zweite Variante testen und prüfen, ob diese besser für Sie geeignet ist. Führen Sie die folgenden Schritte aus, um die erste Alternative zu implementieren.

-   Legen Sie für den [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) im Markup Ihres neuen Projekts `x:Name="wideSeZo"` und `Visibility="Collapsed"` fest.
-   Kehren Sie zum Projekt Bookstore2\_81. windowsphone zurück, und öffnen Sie die Datei "*. XAML". Kopieren Sie das Elementmarkup für den [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom) dieser Datei, und fügen Sie es unmittelbar nach `wideSeZo` in Ihr neues Projekt ein. Geben Sie `x:Name="narrowSeZo"` für das Element ein, das Sie gerade eingefügt haben.
-   Jedoch benötigt `narrowSeZo` einige Stile, die wir noch nicht kopiert haben. In Bookstore2\_81. windowsphone kopieren Sie die beiden Stile (`AuthorGroupHeaderContainerStyle` und `ZoomedOutAuthorItemContainerStyle`) aus sezouc. XAML, und fügen Sie Sie in das neue Projekt in bookstorestyles. XAML ein.
-   Sie verfügen nun über zwei [**SemanticZoom**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.SemanticZoom)-Elemente in Ihrer neuen Datei „SeZoUC.xaml“. Umschließen Sie diese beiden Elemente in einem **Grid**.
-   Hängen Sie in „BookstoreStyles.xaml“ in Ihrem neuen Projekt das Wort `Wide` an diese drei Ressourcenschlüssel (und ihre Verweise in „SeZoUC.xaml“, jedoch nur auf die Verweise in `wideSeZo`) an: `AuthorGroupHeaderTemplate`, `ZoomedOutAuthorTemplate` und `BookTemplate`.
-   Öffnen Sie im Projekt Bookstore2\_81. windowsphone den Eintrag bookstorestyles. XAML. Kopieren Sie aus dieser Datei dieselben drei Ressourcen (oben erwähnt), die zwei Sprung Listen-Element Konverter und die Namespace-Präfix Deklaration Windows\_UI\_XAML-\_Steuerelemente\_primitive, und fügen Sie Sie in das neue Projekt ein.
-   Fügen Sie in „SeZoUC.xaml“ in Ihrem neuen Projekt dem oben hinzugefügten **Grid** das entsprechende Markup des Visual State-Managers hinzu.

```xml
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...

    </Grid>
```

## <a name="universal-styling"></a>Universelle Formatierung

Nun beheben wir einige Formatierungsprobleme, eines davon haben wir oben beim Kopieren aus dem alten Projekt kennengelernt.

-   Ändern Sie in „MainPage.xaml“ den Hintergrund von `LayoutRoot` in `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.
-   Legen Sie in „BookstoreStyles.xaml“ den Wert von `TitlePanelMargin` der Ressource auf `0` (oder den Wert, der Ihnen passend erscheint) fest.
-   Legen Sie in „SeZoUC.xaml“ den Rand von `wideSeZo` auf `0` (oder den Wert, der Ihnen passend erscheint) fest.
-   Entfernen Sie in BookstoreStyles.xaml das Rand-Attribut aus der `AuthorGroupHeaderTemplateWide`.
-   Löschen Sie das FontFamily-Attribut aus `AuthorGroupHeaderTemplate` und `ZoomedOutAuthorTemplate`.
-   In Bookstore2\_81 wurden die Ressourcen Schlüssel `BookTemplateTitleTextBlockStyle`, `BookTemplateAuthorTextBlockStyle`und `PageTitleTextBlockStyle` als Dereferenzierung verwendet, sodass ein einzelner Schlüssel in den beiden apps verschiedene Implementierungen besaß. Diese Dereferenzierung wird nicht mehr benötigt; wir können direkt auf die Systemstile verweisen. Ersetzen Sie daher diese Verweise in der gesamten App durch `TitleTextBlockStyle`, `CaptionTextBlockStyle` bzw. `HeaderTextBlockStyle`. Mit der Visual Studio-Funktion **In Dateien ersetzen** können Sie dieses Vorhaben schnell und fehlerfrei umsetzen. Dann können Sie diese drei ungenutzten Ressourcen löschen.
-   Ersetzen Sie in `AuthorGroupHeaderTemplate` das `PhoneAccentBrush`-Element durch `SystemControlBackgroundAccentBrush`, und legen Sie `Foreground="White"` für **TextBlock** fest, damit dies beim Ausführen mit der Mobilgerätfamilie richtig angezeigt wird.
-   Kopieren Sie in `BookTemplateWide` das Foreground-Attribut aus dem zweiten **TextBlock** in den ersten.
-   Ändern Sie in `ZoomedOutAuthorTemplateWide` den Verweis auf `SubheaderTextBlockStyle` (was nun ein wenig zu groß ist) in einen Verweis auf `SubtitleTextBlockStyle`.
-   Da die verkleinerte Ansicht (die Sprungliste) die vergrößerte Ansicht auf der neuen Plattform nicht mehr überlagert, können wir das `Background`-Attribut aus der verkleinerten Ansicht von `narrowSeZo` löschen.
-   Verschieben Sie `ZoomedInItemsPanelTemplate` von „SeZoUC.xaml“ in „BookstoreStyles.xaml“, damit sich alle Stile und Vorlagen in einer Datei befinden.

Nach den letzten Formatierungsvorgängen sieht die App folgendermaßen aus.

![Die portierte Windows 10-App auf einem Desktopgerät, vergrößerte Ansicht, zwei Fenstergrößen](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

Die portierte Windows 10-APP, die auf einem Desktop Gerät ausgeführt wird, Zoomansicht, zwei Fenstergrößen

![Die portierte Windows 10-App auf einem Desktopgerät, verkleinerte Ansicht, zwei Fenstergrößen](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

Die portierte Windows 10-APP, die auf einem Desktop Gerät ausgeführt wird, Zoomansicht, zwei Fenstergrößen

![Die portierte Windows 10-App auf einem mobilen Gerät, vergrößerte Ansicht](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

Die portierte Windows 10-APP, die auf einem mobilen Gerät ausgeführt wird, Zoomansicht

![Die portierte Windows 10-App auf einem mobilen Gerät, verkleinerte Ansicht](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

Die portierte Windows 10-APP, die auf einem mobilen Gerät ausgeführt wird, und Zoomansicht

## <a name="conclusion"></a>Abschluss

In dieser Fallstudie haben wir es mit einer aufwändigeren Benutzeroberfläche als im vorherigen Beispiel zu tun. Wie bei der vorherigen Fallstudie war für dieses Anzeigemodell kein großer Aufwand erforderlich, und unsere Anstrengungen konzentrierten sich in erster Linie auf die Umgestaltung der Benutzeroberfläche. Einige der Änderungen wurden dadurch erforderlich, dass wir zwei Projekte in eines kombinierten und dennoch viele Formfaktoren (letzten Endes mehr, als zuvor möglich waren) unterstützen wollten. Einige wenige Änderungen aufgrund von Änderungen erforderlich, die an der Plattform vorgenommen wurden.

Die nächsten Fallstudie ist [QuizGame](w8x-to-uwp-case-study-quizgame.md), in der wir den Zugriff auf und die Anzeige von gruppierten Daten behandeln.
