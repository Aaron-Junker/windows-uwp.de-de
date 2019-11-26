---
title: Windows Runtime 8.x zu UWP – Fallstudie, Bookstore1
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: In diesem Thema wird eine Fallstudie zum Portieren einer sehr einfachen Universal 8,1-app in eine Windows 10 universelle Windows-Plattform-app (UWP) vorgestellt.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ecbc4d3fcdff9d06469e7a274fd56ef453a81e02
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260131"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Windows Runtime 8.x zu UWP – Fallstudie, Bookstore1


In diesem Thema wird eine Fallstudie zum Portieren einer sehr einfachen Universal 8,1-app in eine Windows 10 universelle Windows-Plattform-app (UWP) vorgestellt. Eine Universal 8,1-App erstellt ein App-Paket für Windows 8.1 und ein anderes app-Paket für Windows Phone 8,1. Mit Windows 10 können Sie ein einzelnes App-Paket erstellen, das Ihre Kunden auf einer großen Bandbreite von Geräten installieren können, und das ist das, was wir in dieser Fallstudie tun werden. Weitere Informationen finden Sie unter [Anleitung für UWP-Apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

Die portierte App besteht aus einem **ListBox**-Element, das an ein Ansichtsmodell gebunden ist. Das Ansichtsmodell verfügt über eine Liste mit Büchern, für die Titel, Autor und Bucheinband angezeigt werden. Für die Bucheinbandbilder ist **Buildvorgang** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** festgelegt.

Die vorherigen Themen in diesem Abschnitt beschreiben die Unterschiede zwischen den Plattformen und bieten umfassende Informationen und Anleitungen zum Portierungsprozess für verschiedene Aspekte einer App, vom XAML-Markup über die Bindung an ein Ansichtsmodell bis hin zum Zugreifen auf Daten. Dieser Leitfaden soll anhand einer Fallstudie ergänzt werden, indem ein praktisches Beispiel vorgestellt wird. Bei den Fallstudien wird davon ausgegangen, dass Sie die Anleitung gelesen haben. Sie wird nicht wiederholt.

**Beachten** Sie   Wenn Sie Bookstore1Universal\_10 in Visual Studio öffnen, wenn die Meldung "Visual Studio-Update erforderlich" angezeigt wird, führen Sie die Schritte in " [targetplatformversion](w8x-to-uwp-troubleshooting.md)" aus.

## <a name="downloads"></a>Downloads

[Laden Sie die app "Bookstore1\_81 Universal 8,1" herunter](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1_81).

[Laden Sie die Bookstore1Universal\_10 Windows 10-APP herunter](https://codeload.github.com/MicrosoftDocs/windows-topic-specific-samples/zip/Bookstore1Universal_10).

## <a name="the-universal-81-app"></a>Die universelle 8.1-App

Im folgenden sehen Sie, was Bookstore1\_81 – die APP, die wir portieren werden – sieht wie folgt aus:. Es handelt sich dabei einfach um ein Listenfeld mit Büchern mit vertikalem Bildlauf unter der Überschrift des App-Namens und dem Seitentitel.

![So sieht bookstore1\-81 unter Windows aus](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

Bookstore1\_81 unter Windows

![So sieht bookstore1\-81 unter Windows Phone aus](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

Bookstore1\_81 Windows Phone

##  <a name="porting-to-a-windows10-project"></a>Portieren auf ein Windows 10-Projekt

Die Lösung Bookstore1\_81 ist 8,1 ein universelles Universal-App-Projekt, das diese Projekte enthält.

-   Bookstore1\_81. Windows. Dies ist das Projekt, mit dem das App-Paket für Windows 8.1 erstellt wird.
-   Bookstore1\_81. windowsphone. Dies ist das Projekt, das das App-Paket für Windows Phone 8.1 erstellt.
-   Bookstore1\_81. shared. Dies ist das Projekt, das den Quellcode, die Markupdateien und andere Assets und Ressourcen enthält, die von den beiden anderen Projekten verwendet werden.

Für diese Fallstudie verwenden wir die üblichen Optionen, die unter [Bei einer Universal 8.1-App](w8x-to-uwp-root.md) in Bezug auf die zu unterstützenden Geräte beschrieben sind. Die Entscheidung hier ist ein einfaches Beispiel: diese APP verfügt über dieselben Features und hat größtenteils denselben Code, sowohl in der Windows 8.1 als auch in Windows Phone 8,1-Formularen. Wir portieren also den Inhalt des freigegebenen Projekts (und alles, was wir aus den anderen Projekten benötigen) auf ein Windows 10-Gerät, das die universelle Gerätefamilie als Ziel hat (eine, die Sie auf den verschiedensten Geräten installieren können).

Es ist eine sehr schnelle Aufgabe, ein neues Projekt in Visual Studio zu erstellen, Dateien aus Bookstore1\_81 zu kopieren und die kopierten Dateien in das neue Projekt einzubeziehen. Erstellen Sie zunächst ein neues Projekt vom Typ „Leere Anwendung“ (Windows Universal). Nennen Sie Sie Bookstore1Universal\_10. Dabei handelt es sich um die Dateien, die von Bookstore1\_81 nach Bookstore1Universal\_10 kopiert werden müssen.

**Aus dem freigegebenen Projekt**

-   Kopieren Sie den Ordner, der das Buch enthält, das das Buch enthält, das das Buch enthält (der Ordner ist \\\\coverimages). Vergewissern Sie sich nach dem Kopieren des Ordners im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf den kopierten Ordner, und klicken Sie dann auf **Zu Projekt hinzufügen**. Mit „Einschließen“ von Dateien oder Ordnern in einem Projekt meinen wir diesen Befehl. Klicken Sie jedes Mal, wenn Sie eine Datei oder einen Ordner kopieren, für jede Kopie im **Projektmappen-Explorer** auf **Aktualisieren**, und schließen Sie dann die Datei oder den Ordner in das Projekt ein. Dies ist nicht für Dateien erforderlich, die Sie am Ziel ersetzen.
-   Kopieren Sie den Ordner mit der Quelldatei für das Ansichts Modell (der Ordner ist \\ViewModel).
-   Kopieren Sie „MainPage.xaml“, und ersetzen Sie die Datei am Ziel.

**Aus dem Windows-Projekt**

-   Kopieren Sie „BookstoreStyles.xaml“. Diese wird als guter Ausgangspunkt verwendet, da alle Ressourcen Schlüssel in dieser Datei in einer Windows 10-App aufgelöst werden. einige der in der entsprechenden windowsphone-Datei werden nicht angezeigt.

Bearbeiten Sie den Quell Code und die soeben kopierten Markup Dateien, und ändern Sie alle Verweise auf den Namespace Bookstore1\_81 auf Bookstore1Universal\_10. Eine schnelle Möglichkeit dafür ist die Verwendung des Features **In Dateien ersetzen**. Weder im Ansichtsmodell, noch in einem anderen imperativen Code sind Codeänderungen erforderlich. Wenn Sie jedoch leichter erkennen möchten, welche Version der app ausgeführt wird, ändern Sie den Wert, der von der **Bookstore1Universal\_10. bookstoreviewmodel. appname** -Eigenschaft zurückgegeben wird, von "BOOKSTORE1\_81" in "Bookstore1Universal\_10".

Jetzt können Sie mit der Erstellung und Ausführung beginnen. Im folgenden wird erläutert, wie die neue UWP-App aussieht, nachdem Sie noch keine explizite Arbeit für die Portierung von Windows 10 ausgeführt hat.

![Die Windows 10-App mit Änderungen am ursprünglichen Quellcode](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

Windows 10-App mit anfänglichen Quell Codeänderungen, die auf einem Desktop Gerät ausgeführt werden

![Die Windows 10-App mit Änderungen am ursprünglichen Quellcode](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

Windows 10-App mit anfänglichen Quell Codeänderungen, die auf einem mobilen Gerät ausgeführt werden

Die Ansicht und das Ansichtsmodell arbeiten ordnungsgemäß zusammen, und das **ListBox**-Element funktioniert. Wir müssen lediglich die Formatierung ändern. Auf einem mobilen Gerät mit hellem Design können wir den Rahmen des Listenfelds sehen, der sich jedoch einfach ausblenden lässt. Und die Schrift ist zu groß, sodass wir die verwendeten Stile ändern. Darüber hinaus sollte die App helle Farben verwenden, wenn sie auf einem Desktopgerät ausgeführt wird, damit sie standardmäßig aussieht. Wir werden dies also ändern.

## <a name="universal-styling"></a>Universelle Formatierung

Die Bookstore1\_81-App verwendete zwei verschiedene Ressourcen Wörterbücher (bookstorestyles. XAML), um Ihre Stile an die Windows 8.1-und Windows Phone 8,1-Betriebssysteme anzupassen. Keines dieser beiden Dateien der Datei "bookstorestyles. XAML" enthält genau die Stile, die wir für unsere Windows 10-App benötigen. Die gute Nachricht ist jedoch, dass unsere Vorstellungen tatsächlich sehr viel einfacher sind als diese beiden. In den nächsten Schritten werden wir daher hauptsächlich unsere Projektdateien und das Markup entfernen und vereinfachen. Die Schritte finden Sie nachfolgend: Sie können die Links oben in diesem Thema verwenden, um die Projekte herunterzuladen und die Ergebnisse dieser Änderungen zwischen diesem Punkt und dem Ende der Fallstudie anzuzeigen.

-   Um den Abstand zwischen den Elementen zu verkleinern, suchen Sie die `BookTemplate`-Datenvorlage in „MainPage.xaml“, und löschen Sie `Margin="0,0,0,8"` aus dem Stamm-**Grid**.
-   Auch in `BookTemplate`gibt es Verweise auf `BookTemplateTitleTextBlockStyle` und `BookTemplateAuthorTextBlockStyle`. Bookstore1\_81 verwendete diese Schlüssel als Dereferenzierung, sodass ein einzelner Schlüssel in den beiden apps über verschiedene Implementierungen verfügte. Diese Dereferenzierung wird nicht mehr benötigt; wir können direkt auf die Systemstile verweisen. Ersetzen Sie daher diese Verweise durch `TitleTextBlockStyle` bzw. `SubtitleTextBlockStyle`.
-   Jetzt müssen wir den Hintergrund von `LayoutRoot` auf den richtigen Standardwert festlegen, damit die App unabhängig vom Design bei der Ausführung auf allen Geräten entsprechend gut aussieht. Ändern Sie den Wert von `"Transparent"` in `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.
-   Ändern Sie in `TitlePanel` den Verweis auf `TitleTextBlockStyle` (was nun ein wenig zu groß ist) in einen Verweis auf `CaptionTextBlockStyle`. `PageTitleTextBlockStyle` ist eine weitere Bookstore1\_81-Dereferenzierung, die wir nicht mehr benötigen. Ändern Sie dies, sodass stattdessen auf `HeaderTextBlockStyle` verwiesen wird.
-   Für **ListBox** müssen keine speziellen Background-, Style- oder ItemContainerStyle-Werte mehr festgelegt werden. Löschen Sie daher diese drei Attribute und deren Werte aus dem Markup. Wir möchten den Rand von **ListBox** ausblenden und fügen daher `BorderBrush="{x:Null}"` hinzu.
-   Wir verweisen nicht mehr auf eine Ressource in der **ResourceDictionary**-Datei namens „BookstoreStyles.xaml“. Sie können alle diese Ressourcen löschen. Löschen Sie jedoch nicht die Datei „BookstoreStyles.xaml“ selbst. Diese können wir noch ein letztes Mal verwenden, wie Sie im nächsten Abschnitt sehen werden.

Nach den letzten Formatierungsvorgängen sieht die App folgendermaßen aus.

![Die fast portierte Windows 10-App](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

Die fast portierte Windows 10-APP, die auf einem Desktop Gerät ausgeführt wird

![Die fast portierte Windows 10-App](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

Die fast portierte Windows 10-APP, die auf einem mobilen Gerät ausgeführt wird

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

Die portierte Windows 10-APP, die auf einem mobilen Gerät ausgeführt wird

## <a name="conclusion"></a>Fazit

In dieser Fallstudie wurde der Prozess zum Portieren einer einfachen App gezeigt – einer zugegebenermaßen unrealistisch einfachen App. Beispielsweise kann ein Listenfeld für die Auswahl oder die Herstellung eines Navigationskontexts verwendet werden; die App navigiert zu einer Seite mit weiteren Details zum ausgewählten Element. Diese bestimmte App führt keine Aktionen mit der Auswahl des Benutzers aus und verfügt nicht über Navigation. Dennoch diente die Fallstudie dazu, den Portierungsprozess vorzustellen und wichtige Techniken zu veranschaulichen, die Sie in echten UWP-Apps verwenden können.

Wir konnten uns auch davon überzeugen, dass das Portieren von Ansichtsmodellen in der Regel ein reibungsloser Prozess ist. Die Benutzeroberfläche und die Formfaktorunterstützung sind Aspekte, die beim Portieren mit höherer Wahrscheinlichkeit unsere Aufmerksamkeit erfordern.

Die nächste Fallstudie ist [Bookstore2](w8x-to-uwp-case-study-bookstore2.md), in der wir den Zugriff auf und die Anzeige von gruppierten Daten behandeln.
