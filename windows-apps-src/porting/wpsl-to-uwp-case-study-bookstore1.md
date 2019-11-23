---
ms.assetid: 2b63a4c8-b1c0-4c77-95ab-0b9549ba3c0e
description: In diesem Thema wird eine Fallstudie zum Portieren einer sehr einfachen Windows Phone Silverlight-App auf eine Windows 10 universelle Windows-Plattform-app (UWP) vorgestellt.
title: Windows Phone von Silverlight zur UWP-Fallstudie, Bookstore1
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1079d51aa0b013cd40ff585e5baabb61f940a745
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260098"
---
# <a name="windowsphone-silverlight-to-uwp-case-study-bookstore1"></a>Windows Phone von Silverlight zur UWP-Fallstudie: Bookstore1


In diesem Thema wird eine Fallstudie zum Portieren einer sehr einfachen Windows Phone Silverlight-App auf eine Windows 10 universelle Windows-Plattform-app (UWP) vorgestellt. Mit Windows 10 können Sie ein einzelnes App-Paket erstellen, das Ihre Kunden auf einer großen Bandbreite von Geräten installieren können, und das ist das, was wir in dieser Fallstudie tun werden. Weitere Informationen finden Sie unter [Anleitung für UWP-Apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

Die portierte App besteht aus einem **ListBox**-Element, das an ein Ansichtsmodell gebunden ist. Das Ansichtsmodell verfügt über eine Liste mit Büchern, für die Titel, Autor und Bucheinband angezeigt werden. Für die Bucheinbandbilder ist **Buildvorgang** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** festgelegt.

Die vorherigen Themen in diesem Abschnitt beschreiben die Unterschiede zwischen den Plattformen und bieten umfassende Informationen und Anleitungen zum Portierungsprozess für verschiedene Aspekte einer App, vom XAML-Markup über die Bindung an ein Ansichtsmodell bis hin zum Zugreifen auf Daten. Dieser Leitfaden soll anhand einer Fallstudie ergänzt werden, indem ein praktisches Beispiel vorgestellt wird. Bei den Fallstudien wird davon ausgegangen, dass Sie die Anleitung gelesen haben. Sie wird nicht wiederholt.

**Beachten** Sie   Wenn Sie\_Bookstore1Universal 10 in Visual Studio öffnen, wenn die Meldung "Visual Studio-Update erforderlich" angezeigt wird, führen Sie die Schritte zum Auswählen einer Versionsverwaltung für die Zielplattform in " [targetplatformversion](wpsl-to-uwp-troubleshooting.md)" aus.

## <a name="downloads"></a>Downloads

[Laden Sie die Bookstore1WPSL8 Windows Phone Silverlight-APP herunter](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1WPSL8).

[Laden Sie die Bookstore1Universal\_10 Windows 10-APP herunter](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10).

## <a name="the-windowsphone-silverlight-app"></a>Die Windows Phone Silverlight-App

So sieht „Bookstore1WPSL8“ aus – die App, die wir portieren werden. Es handelt sich dabei einfach um ein Listenfeld mit Büchern mit vertikalem Bildlauf unter der Überschrift des App-Namens und dem Seitentitel.

![Erscheinungsbild von „Bookstore1WPSL8“](images/wpsl-to-uwp-case-studies/c01-01-wpsl-how-the-app-looks.png)

## <a name="porting-to-a-windows10-project"></a>Portieren auf ein Windows 10-Projekt

Das Erstellen eines neuen Projekts in Visual Studio geht sehr schnell: Kopieren Sie Dateien aus „Bookstore1WPSL8“, und fügen Sie die kopierten Dateien in das neue Projekt ein. Erstellen Sie zunächst ein neues Projekt vom Typ „Leere Anwendung“ (Windows Universal). Nennen Sie Sie Bookstore1Universal\_10. Dabei handelt es sich um die Dateien, die von Bookstore1WPSL8 zu Bookstore1Universal\_10 kopiert werden.

-   Kopieren Sie den Ordner, der das Buch enthält, das das Buch enthält, das das Buch enthält (der Ordner ist \\\\coverimages). Vergewissern Sie sich nach dem Kopieren des Ordners im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf den kopierten Ordner, und klicken Sie dann auf **Zu Projekt hinzufügen**. Mit „Einschließen“ von Dateien oder Ordnern in einem Projekt meinen wir diesen Befehl. Klicken Sie jedes Mal, wenn Sie eine Datei oder einen Ordner kopieren, im **Projektmappen-Explorer** auf **Aktualisieren**, und schließen Sie dann die Datei oder den Ordner in das Projekt ein. Dies ist nicht für Dateien erforderlich, die Sie am Ziel ersetzen.
-   Kopieren Sie den Ordner mit der Quelldatei für das Ansichts Modell (der Ordner ist \\ViewModel).
-   Kopieren Sie „MainPage.xaml“, und ersetzen Sie die Datei am Ziel.

Wir können die Datei "App. XAML" und App.XAML.cs, die Visual Studio für uns generiert hat, im Windows 10-Projekt aufbewahren.

Bearbeiten Sie den Quell Code und die soeben kopierten Markup Dateien, und ändern Sie alle Verweise auf den Bookstore1WPSL8-Namespace in Bookstore1Universal\_10. Eine schnelle Möglichkeit dafür ist die Verwendung des Features **In Dateien ersetzen**. Im imperativen Code in der Ansichtsmodell-Quelldatei sind die folgenden Portierungsänderungen erforderlich:

-   Ändern Sie `System.ComponentModel.DesignerProperties` in `DesignMode`, und wenden Sie dann den Befehl **Auflösen** darauf an. Löschen Sie die `IsInDesignTool`-Eigenschaft, und fügen Sie mithilfe von IntelliSense den richtigen Eigenschaftsnamen hinzu: `DesignModeEnabled`.
-   Wenden Sie den Befehl **Auflösen** auf `ImageSource` an.
-   Wenden Sie den Befehl **Auflösen** auf `BitmapImage` an.
-   Löschen Sie using `System.Windows.Media;` und `using System.Windows.Media.Imaging;`.
-   Ändern Sie den Wert, der von der **Bookstore1Universal\_10. bookstoreviewmodel. appname** -Eigenschaft von "BOOKSTORE1WPSL8" in "Bookstore1Universal" zurückgegeben wird.

In „MainPage.xaml“ führen Sie die folgenden Portierungsänderungen aus:

-   Ändern Sie `phone:PhoneApplicationPage` in `Page` (einschließlich der Eigenschaftselementsyntax).
-   Löschen Sie die Namespacepräfix-Deklarationen `phone` und `shell`.
-   Ändern Sie „clr-namespace“ in der verbleibenden Namespacepräfix-Deklaration in „using“.

Wir können Markupkompilierungsfehler auf sehr einfache Weise beheben, wenn wir schnell Ergebnisse erhalten möchten, auch wenn dazu vorübergehend Markup entfernt werden muss. Wir sollten jedoch die Forderungen dokumentieren, die auf diese Weise entstehen. In diesem Fall:

1.  Löschen Sie im **Page**-Stammelement in der Datei **MainPage.xaml** den Codetext `SupportedOrientations="Portrait"`.
2.  Löschen Sie im **Page**-Stammelement in der Datei **MainPage.xaml** den Codetext `Orientation="Portrait"`.
3.  Löschen Sie im **Page**-Stammelement in der Datei **MainPage.xaml** den Codetext `shell:SystemTray.IsVisible="True"`.
4.  Löschen Sie in der `BookTemplate`-Datenvorlage die Verweise auf die `PhoneTextExtraLargeStyle`TextBlock`PhoneTextSubtleStyle`-Stile  ** und **.
5.  Löschen Sie in `TitlePanel` **StackPanel** die Verweise auf die `PhoneTextNormalStyle`TextBlock`PhoneTextTitle1Style`-Stile  ** und **.

Zuerst befassen wir uns mit der UI für die Familie der Mobilgeräte. Um die anderen Formfaktoren können wir uns später kümmern. Sie können die App jetzt erstellen und ausführen. Im Mobile-Emulator sieht sie wie hier dargestellt aus.

![UWP-App auf dem Mobilgerät mit Änderungen am ursprünglichen Quellcode](images/wpsl-to-uwp-case-studies/c01-02-mob10-initial-source-code-changes.png)

Die Ansicht und das Ansichtsmodell arbeiten ordnungsgemäß zusammen, und das **ListBox**-Element funktioniert. Wir müssen lediglich die Stile bearbeiten und dafür sorgen, dass die Bilder angezeigt werden.

## <a name="paying-off-the-debt-items-and-some-initial-styling"></a>Zurückzahlen der Schulden und erste Formatierungen

Standardmäßig werden alle Ausrichtungen unterstützt. Die Windows Phone Silverlight-App beschränkt sich explizit auf das Hochformat, sodass die Schulden Elemente \#1 und \#2 bezahlt werden, indem Sie das App-Paket Manifest im neuen Projekt aufrufen und das Hochformat in **unterstützten Ausrichtungen** **überprüfen.**

Für diese APP ist Item \#3 keine Schuld, weil die Statusleiste (früher als "Systemleiste" bezeichnet) standardmäßig angezeigt wird. Für Elemente \#4 und \#5 müssen wir vier universelle Windows-Plattform- **TextBlock** -Stile (UWP) suchen, die den von uns verwendeten Windows Phone Silverlight-Stilen entsprechen. Sie können die Windows Phone Silverlight-App im Emulator ausführen und diese nebeneinander mit der Abbildung im [Text](wpsl-to-uwp-porting-xaml-and-ui.md) Abschnitt vergleichen. Wenn Sie dies tun, und wenn Sie sich die Eigenschaften der Windows Phone Silverlight-System Stile ansehen, können wir diese Tabelle erstellen.

| Windows Phone Silverlight-Stilschlüssel | UWP-Stilschlüssel          |
|-------------------------------------|------------------------|
| PhoneTextExtraLargeStyle            | TitleTextBlockStyle    |
| PhoneTextSubtleStyle                | SubtitleTextBlockStyle |
| PhoneTextNormalStyle                | CaptionTextBlockStyle  |
| PhoneTextTitle1Style                | HeaderTextBlockStyle   |
 
Zum Festlegen dieser Stile können Sie sie einfach in den Markup-Editor eingeben oder die Visual Studio-XAML-Tools verwenden und sie festlegen, ohne etwas einzugeben. Klicken Sie dazu mit der rechten Maustaste auf einen **TextBlock** , und klicken Sie auf **Stil bearbeiten** &gt; **Ressource anwenden**. Klicken Sie dazu **mit der rechten Maustaste auf das** **Listenfeld** , und klicken Sie auf **zusätzliche Vorlagen bearbeiten** , &gt; **generierte Elemente bearbeiten (ItemTemplate)** zu bearbeiten.

Die Elemente sind mit einem weißen Hintergrund mit 80 % Deckkraft unterlegt, da der Standardstil des **ListBox**-Steuerelements den Hintergrund auf die `ListBoxBackgroundThemeBrush`-Systemressource festlegt. Legen Sie `Background="Transparent"` für das **ListBox**-Element fest, um den Hintergrund zu löschen. Zur Linksausrichtung der **TextBlock**-Elemente in der Elementvorlage bearbeiten Sie sie erneut wie oben beschrieben und legen unter **Margin** für beide `"9.6,0"`TextBlock **-Elemente den Wert**  fest.

Anschließend müssen wir aufgrund der [Änderungen im Zusammenhang mit Anzeigepixeln](wpsl-to-uwp-porting-xaml-and-ui.md) alle Dimensionen fester Größe, die wir noch nicht geändert haben (Ränder, Breite, Höhe, usw.), überprüfen und mit 0,8 multiplizieren. Die Bilder sollten z. B. von 70x70px in 56x56px geändert werden.

Lassen Sie uns jedoch die Bilder rendern, bevor wir die Ergebnisse Ihrer Formatierung anzeigen.

## <a name="binding-an-image-to-a-view-model"></a>Binden eines Bilds an ein Ansichtsmodell

In „Bookstore1WPSL8“ haben wir Folgendes vorgenommen:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(this.CoverImagePath, UriKind.Relative));
```

In „Bookstore1Universal“ verwenden wir das [URI-Schema](https://docs.microsoft.com/previous-versions/windows/apps/jj655406(v=win.10)) „ms-appx“. Damit der Rest unseres Codes beibehalten werden kann, können wir eine andere Überladung des **System.Uri**-Konstruktors verwenden, um das URI-Schema „ms-appx“ in einen Basis-URI einzufügen und den restlichen Pfad anzufügen. Hier sehen Sie ein Beispiel:

```csharp
    // this.BookCoverImagePath contains a path of the form "/Assets/CoverImages/one.png".
    return new BitmapImage(new Uri(new Uri("ms-appx://"), this.CoverImagePath));
```

## <a name="universal-styling"></a>Universelle Formatierung

Jetzt müssen wir nur noch einige abschließende Formatierungsanpassungen vornehmen und uns vergewissern, dass die App sowohl auf dem Desktop (und anderen Formfaktoren) sowie Mobilgeräten gut aussieht. Die Schritte finden Sie nachfolgend: Sie können die Links oben in diesem Thema verwenden, um die Projekte herunterzuladen und die Ergebnisse dieser Änderungen zwischen diesem Punkt und dem Ende der Fallstudie anzuzeigen.

-   Um den Abstand zwischen den Elementen zu verkleinern, suchen Sie in der Datei „MainPage.xaml“ nach der `BookTemplate`-Datenvorlage und löschen das `Margin`-Attribut aus dem **Grid**-Stammelement.
-   Wenn Sie für den Seitentitel etwas mehr Platz bereitstellen möchten, können Sie den unteren Rand für das `-5.6`TextBlock`0`-Element der Titelseite von **auf** zurücksetzen.
-   Jetzt müssen wir den Hintergrund von `LayoutRoot` auf den richtigen Standardwert festlegen, damit die App unabhängig vom Design bei der Ausführung auf allen Geräten entsprechend gut aussieht. Ändern Sie den Wert von `"Transparent"` in `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.

Bei einer komplexeren App müssten wir an diesem Punkt den Leitfaden unter [Portieren für Formfaktor und Benutzerfreundlichkeit](wpsl-to-uwp-form-factors-and-ux.md) verwenden und den Formfaktor für die einzelnen Geräte, auf denen die App jetzt ausgeführt werden kann, wirklich optimal nutzen. Bei dieser einfachen App können wir aber hier aufhören und uns ansehen, wie die App nach den letzten Formatierungsvorgängen aussieht. Tatsächlich sieht sie auf Mobilgeräten und Desktopgeräten gleich aus, auch wenn der Platz bei breiten Formfaktoren nicht bestmöglich genutzt wird (damit beschäftigen wir uns in einer späteren Fallstudie).

Informationen dazu, wie Sie das Design Ihrer App steuern, finden Sie unter [Designänderungen](wpsl-to-uwp-porting-xaml-and-ui.md).

![Die portierte Windows 10-App](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

Die portierte Windows 10-APP, die auf einem mobilen Gerät ausgeführt wird

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>Eine optionale Anpassung des Listenfelds für Mobilgeräte

Wenn die App auf einem Mobilgerät ausgeführt wird, ist der Hintergrund eines Listenfelds standardmäßig in beiden Designs hell. Vielleicht ist dies der von Ihnen bevorzugte Stil. Falls dies so ist, gibt es nichts mehr zu tun. Aber die Steuerelemente sind so konzipiert, dass Sie das Aussehen anpassen können, ohne ihr Verhalten zu ändern. Wenn das Listenfeld im dunklen Design dunkel sein soll, wie in der ursprünglichen App vorgesehen, hilft Ihnen [diese Anleitung](w8x-to-uwp-case-study-bookstore1.md) unter „Eine optionale Anpassung“ weiter.

## <a name="conclusion"></a>Abschluss

In dieser Fallstudie wurde der Prozess zum Portieren einer einfachen App gezeigt – einer zugegebenermaßen unrealistisch einfachen App. Beispielsweise können Listensteuerelemente für die Auswahl oder für die Herstellung eines Kontexts für die Navigation verwendet werden. Die App navigiert zu einer Seite mit weiteren Details zum ausgewählten Element. Diese bestimmte App führt keine Aktionen mit der Auswahl des Benutzers aus und verfügt nicht über Navigation. Dennoch diente die Fallstudie dazu, den Portierungsprozess vorzustellen und wichtige Techniken zu veranschaulichen, die Sie in echten UWP-Apps verwenden können.

Die nächste Fallstudie ist [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md), in der wir den Zugriff auf und die Anzeige von gruppierten Daten behandeln.
