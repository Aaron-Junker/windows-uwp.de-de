---
title: Windows Runtime 8.x zu UWP – Fallstudie, Bookstore1
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: This topic presents a case study of porting a very simple Universal 8.1 app to a Windows 10 Universal Windows Platform (UWP) app.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ecbc4d3fcdff9d06469e7a274fd56ef453a81e02
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260131"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Windows Runtime 8.x zu UWP – Fallstudie, Bookstore1


This topic presents a case study of porting a very simple Universal 8.1 app to a Windows 10 Universal Windows Platform (UWP) app. A Universal 8.1 app is one that builds one app package for Windows 8.1, and a different app package for Windows Phone 8.1. With Windows 10, you can create a single app package that your customers can install onto a wide range of devices, and that's what we'll do in this case study. Weitere Informationen finden Sie unter [Anleitung für UWP-Apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

Die portierte App besteht aus einem **ListBox**-Element, das an ein Ansichtsmodell gebunden ist. Das Ansichtsmodell verfügt über eine Liste mit Büchern, für die Titel, Autor und Bucheinband angezeigt werden. Für die Bucheinbandbilder ist **Buildvorgang** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** festgelegt.

Die vorherigen Themen in diesem Abschnitt beschreiben die Unterschiede zwischen den Plattformen und bieten umfassende Informationen und Anleitungen zum Portierungsprozess für verschiedene Aspekte einer App, vom XAML-Markup über die Bindung an ein Ansichtsmodell bis hin zum Zugreifen auf Daten. Dieser Leitfaden soll anhand einer Fallstudie ergänzt werden, indem ein praktisches Beispiel vorgestellt wird. Bei den Fallstudien wird davon ausgegangen, dass Sie die Anleitung gelesen haben. Sie wird nicht wiederholt.

**Note**   When opening Bookstore1Universal\_10 in Visual Studio, if you see the message "Visual Studio update required", then follow the steps in [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Downloads

[Download the Bookstore1\_81 Universal 8.1 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1_81).

[Download the Bookstore1Universal\_10 Windows 10 app](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10).

## <a name="the-universal-81-app"></a>Die universelle 8.1-App

Here’s what Bookstore1\_81—the app that we're going to port—looks like. Es handelt sich dabei einfach um ein Listenfeld mit Büchern mit vertikalem Bildlauf unter der Überschrift des App-Namens und dem Seitentitel.

![how bookstore1\-81 looks on windows](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

Bookstore1\_81 on Windows

![how bookstore1\-81 looks on windows phone](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

Bookstore1\_81 on Windows Phone

##  <a name="porting-to-a-windows10-project"></a>Porting to a Windows 10 project

The Bookstore1\_81 solution is an 8.1 Universal App project, and it contains these projects.

-   Bookstore1\_81.Windows. This is the project that builds the app package for Windows 8.1.
-   Bookstore1\_81.WindowsPhone. Dies ist das Projekt, das das App-Paket für Windows Phone 8.1 erstellt.
-   Bookstore1\_81.Shared. Dies ist das Projekt, das den Quellcode, die Markupdateien und andere Assets und Ressourcen enthält, die von den beiden anderen Projekten verwendet werden.

Für diese Fallstudie verwenden wir die üblichen Optionen, die unter [Bei einer Universal 8.1-App](w8x-to-uwp-root.md) in Bezug auf die zu unterstützenden Geräte beschrieben sind. The decision here is a simple one: this app has the same features, and does so mostly with the same code, in both its Windows 8.1 and Windows Phone 8.1 forms. So, we'll port the contents of the Shared project (and anything else we need from the other projects) to a Windows 10 that targets the Universal device family (one that you can install onto the widest range of devices).

It's a very quick task to create a new project in Visual Studio, copy files over to it from Bookstore1\_81, and include the copied files in the new project. Erstellen Sie zunächst ein neues Projekt vom Typ „Leere Anwendung“ (Windows Universal). Name it Bookstore1Universal\_10. These are the files to copy over from Bookstore1\_81 to Bookstore1Universal\_10.

**From the Shared project**

-   Copy the folder containing the book cover image PNG files (the folder is \\Assets\\CoverImages). Vergewissern Sie sich nach dem Kopieren des Ordners im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf den kopierten Ordner, und klicken Sie dann auf **Zu Projekt hinzufügen**. Mit „Einschließen“ von Dateien oder Ordnern in einem Projekt meinen wir diesen Befehl. Klicken Sie jedes Mal, wenn Sie eine Datei oder einen Ordner kopieren, für jede Kopie im **Projektmappen-Explorer** auf **Aktualisieren**, und schließen Sie dann die Datei oder den Ordner in das Projekt ein. Dies ist nicht für Dateien erforderlich, die Sie am Ziel ersetzen.
-   Copy the folder containing the view model source file (the folder is \\ViewModel).
-   Kopieren Sie „MainPage.xaml“, und ersetzen Sie die Datei am Ziel.

**From the Windows project**

-   Kopieren Sie „BookstoreStyles.xaml“. We'll use this one as a good starting-point because all the resource keys in this file will resolve in a Windows 10 app; some of those in the equivalent WindowsPhone file will not.

Edit the source code and markup files that you just copied and change any references to the Bookstore1\_81 namespace to Bookstore1Universal\_10. Eine schnelle Möglichkeit dafür ist die Verwendung des Features **In Dateien ersetzen**. Weder im Ansichtsmodell, noch in einem anderen imperativen Code sind Codeänderungen erforderlich. But, just to make it easier to see which version of the app is running, change the value returned by the **Bookstore1Universal\_10.BookstoreViewModel.AppName** property from "BOOKSTORE1\_81" to "BOOKSTORE1UNIVERSAL\_10".

Jetzt können Sie mit der Erstellung und Ausführung beginnen. Here's how our new UWP app looks after having done no explicit work yet to port it to Windows 10.

![Die Windows 10-App mit Änderungen am ursprünglichen Quellcode](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

The Windows 10 app with initial source code changes running on a Desktop device

![Die Windows 10-App mit Änderungen am ursprünglichen Quellcode](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

The Windows 10 app with initial source code changes running on a Mobile device

Die Ansicht und das Ansichtsmodell arbeiten ordnungsgemäß zusammen, und das **ListBox**-Element funktioniert. Wir müssen lediglich die Formatierung ändern. Auf einem mobilen Gerät mit hellem Design können wir den Rahmen des Listenfelds sehen, der sich jedoch einfach ausblenden lässt. Und die Schrift ist zu groß, sodass wir die verwendeten Stile ändern. Darüber hinaus sollte die App helle Farben verwenden, wenn sie auf einem Desktopgerät ausgeführt wird, damit sie standardmäßig aussieht. Wir werden dies also ändern.

## <a name="universal-styling"></a>Universelle Formatierung

The Bookstore1\_81 app used two different resource dictionaries (BookstoreStyles.xaml) to tailor its styles to the Windows 8.1 and Windows Phone 8.1 operating systems. Neither of those two BookstoreStyles.xaml files contains exactly the styles we need for our Windows 10 app. Die gute Nachricht ist jedoch, dass unsere Vorstellungen tatsächlich sehr viel einfacher sind als diese beiden. In den nächsten Schritten werden wir daher hauptsächlich unsere Projektdateien und das Markup entfernen und vereinfachen. Die Schritte finden Sie nachfolgend: Sie können die Links oben in diesem Thema verwenden, um die Projekte herunterzuladen und die Ergebnisse dieser Änderungen zwischen diesem Punkt und dem Ende der Fallstudie anzuzeigen.

-   Um den Abstand zwischen den Elementen zu verkleinern, suchen Sie die `BookTemplate`-Datenvorlage in „MainPage.xaml“, und löschen Sie `Margin="0,0,0,8"` aus dem Stamm-**Grid**.
-   Auch in `BookTemplate`gibt es Verweise auf `BookTemplateTitleTextBlockStyle` und `BookTemplateAuthorTextBlockStyle`. Bookstore1\_81 used those keys as an indirection so that a single key had different implementations in the two apps. Diese Dereferenzierung wird nicht mehr benötigt; wir können direkt auf die Systemstile verweisen. Ersetzen Sie daher diese Verweise durch `TitleTextBlockStyle` bzw. `SubtitleTextBlockStyle`.
-   Jetzt müssen wir den Hintergrund von `LayoutRoot` auf den richtigen Standardwert festlegen, damit die App unabhängig vom Design bei der Ausführung auf allen Geräten entsprechend gut aussieht. Ändern Sie den Wert von `"Transparent"` in `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.
-   Ändern Sie in `TitlePanel` den Verweis auf `TitleTextBlockStyle` (was nun ein wenig zu groß ist) in einen Verweis auf `CaptionTextBlockStyle`. `PageTitleTextBlockStyle` is another Bookstore1\_81 indirection that we don't need any longer. Ändern Sie dies, sodass stattdessen auf `HeaderTextBlockStyle` verwiesen wird.
-   Für **ListBox** müssen keine speziellen Background-, Style- oder ItemContainerStyle-Werte mehr festgelegt werden. Löschen Sie daher diese drei Attribute und deren Werte aus dem Markup. Wir möchten den Rand von **ListBox** ausblenden und fügen daher `BorderBrush="{x:Null}"` hinzu.
-   Wir verweisen nicht mehr auf eine Ressource in der **ResourceDictionary**-Datei namens „BookstoreStyles.xaml“. Sie können alle diese Ressourcen löschen. Löschen Sie jedoch nicht die Datei „BookstoreStyles.xaml“ selbst. Diese können wir noch ein letztes Mal verwenden, wie Sie im nächsten Abschnitt sehen werden.

Nach den letzten Formatierungsvorgängen sieht die App folgendermaßen aus.

![Die fast portierte Windows 10-App](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

The almost-ported Windows 10 app running on a Desktop device

![Die fast portierte Windows 10-App](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

The almost-ported Windows 10 app running on a Mobile device

## <a name="an-optional-adjustment-to-the-list-box-for-mobile-devices"></a>Eine optionale Anpassung des Listenfelds für Mobilgeräte

Wenn die App auf einem Mobilgerät ausgeführt wird, ist der Hintergrund eines Listenfelds standardmäßig in beiden Designs hell. Dies ist möglicherweise genau der Stil, den Sie bevorzugen, sodass Sie in diesem Fall nichts weiter unternehmen müssen als aufzuräumen: Löschen Sie dazu die Ressourcenwörterbuchdatei „BookstoreStyles.xaml“ aus dem Projekt, und entfernen Sie das Markup, das sie in „MainPage.xaml“ zusammenführt.

Aber die Steuerelemente sind so konzipiert, dass Sie das Aussehen anpassen können, ohne ihr Verhalten zu ändern. Wenn das Listenfeld durch das dunkle Design dunkel gestaltet werden soll – so wie in der ursprünglichen App vorgesehen – dann finden Sie in diesem Abschnitt eine Beschreibung dafür.

Die Änderung, die wir vornehmen, muss sich nur dann auf die App auswirken, wenn sie auf mobilen Geräten ausgeführt wird. Daher verwenden wir einen leicht angepassten Listenfeldstil, wenn die App auf der Mobilgerätefamilie ausgeführt wird, für alle anderen Geräte verwenden wir weiterhin den Standardstil. Dazu erstellen wir eine Kopie von „BookstoreStyles.xaml“ und geben ihr einen speziellen MRT-qualifizierten Namen, sodass die Kopie nur auf mobilen Geräten geladen wird.

Fügen Sie ein neues **ResourceDictionary**-Projektelement hinzu, und geben Sie ihm den Namen „BookstoreStyles.DeviceFamily Mobile.xaml“. Sie haben jetzt zwei Dateien, die beide den logischen Namen "BookstoreStyles.xaml" haben. (Dies ist der Name, den Sie in Ihrem Markup und Code verwenden.) Die Dateien haben jedoch unterschiedliche physische Namen, sodass sie verschiedene Markups enthalten können. Sie können dieses MRT-qualifizierte Benennungsschema für jede XAML-Datei verwenden. Beachten Sie jedoch, dass sich alle XAML-Dateien mit demselben logischen Namen eine einzelne CodeBehind-Datei („xaml.cs“) teilen, sofern vorhanden.

Bearbeiten Sie eine Kopie der Steuerelementvorlage für das Listenfeld, und speichern Sie sie mit dem Schlüssel von `BookstoreListBoxStyle` im neuen Ressourcenwörterbuch „BookstoreStyles.DeviceFamily-Mobile.xaml“. Jetzt nehmen wir einfache Änderungen an drei der Setter vor.

-   Ändern Sie im Vordergrundsetter den Wert in `"{x:Null}"`. Beachten Sie, dass das direkte Festlegen einer Eigenschaft auf `"{x:Null}"` für ein Element identisch mit der Einstellung `null` im Code ist. Der Wert `"{x:Null}"` in einem Setter hat jedoch einen einzigartigen Effekt: Der Setter im Standardstil (für dieselbe Eigenschaft) wird überschrieben, und der Standardwert der Eigenschaft wird für das Zielelement wiederhergestellt.
-   Ändern Sie im Hintergrundsetter den Wert in `"Transparent"`, um den hellen Hintergrund zu entfernen.
-   Suchen Sie im Vorlagensetter den visuellen Zustand mit dem Namen `Focused`, und löschen Sie das Storyboard, damit sie ein leeres Tag erhalten.
-   Löschen Sie alle anderen Setter aus dem Markup.

Kopieren Sie schließlich `BookstoreListBoxStyle` in „BookstoreStyles.xaml“, und löschen Sie die drei Setter, damit sie ein leeres Tag erhalten. Dies führen wir auf anderen Geräten als mobilen Geräten durch, sodass der Verweis auf „BookstoreStyles.xaml“ und `BookstoreListBoxStyle` weiterhin aufgelöst wird, aber keine Auswirkungen hat.

![Die portierte Windows 10-App](images/w8x-to-uwp-case-studies/c01-07-mob10-ported.png)

The ported Windows 10 app running on a Mobile device

## <a name="conclusion"></a>Abschluss

In dieser Fallstudie wurde der Prozess zum Portieren einer einfachen App gezeigt – einer zugegebenermaßen unrealistisch einfachen App. Beispielsweise kann ein Listenfeld für die Auswahl oder die Herstellung eines Navigationskontexts verwendet werden; die App navigiert zu einer Seite mit weiteren Details zum ausgewählten Element. Diese bestimmte App führt keine Aktionen mit der Auswahl des Benutzers aus und verfügt nicht über Navigation. Dennoch diente die Fallstudie dazu, den Portierungsprozess vorzustellen und wichtige Techniken zu veranschaulichen, die Sie in echten UWP-Apps verwenden können.

Wir konnten uns auch davon überzeugen, dass das Portieren von Ansichtsmodellen in der Regel ein reibungsloser Prozess ist. Die Benutzeroberfläche und die Formfaktorunterstützung sind Aspekte, die beim Portieren mit höherer Wahrscheinlichkeit unsere Aufmerksamkeit erfordern.

Die nächste Fallstudie ist [Bookstore2](w8x-to-uwp-case-study-bookstore2.md), in der wir den Zugriff auf und die Anzeige von gruppierten Daten behandeln.
