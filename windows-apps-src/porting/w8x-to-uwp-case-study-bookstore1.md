---
title: Windows Runtime 8.x zu UWP – Fallstudie, Bookstore1
ms.assetid: e4582717-afb5-4cde-86bb-31fb1c5fc8f3
description: Dieses Thema enthält eine Fallstudie, der eine sehr einfache Universal 8.1-app für eine app für Windows 10 universelle Windows-Plattform (UWP) portieren.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 5e0b8ffbebdca920bc6cc584dd735062a364d1d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627935"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore1"></a>Windows-Runtime 8.x zu UWP-Fallstudie: Bookstore1


Dieses Thema enthält eine Fallstudie, der eine sehr einfache Universal 8.1-app für eine app für Windows 10 universelle Windows-Plattform (UWP) portieren. Eine Universal 8.1-app ist eine, die eine app-Paket für Windows 8.1 und einer anderen app-Paket für Windows Phone 8.1 erstellt. Unter Windows 10 können Sie eine einzelne app-Paket erstellen, die Ihre Kunden können auf eine Vielzahl von Geräten installieren, und das ist in dieser Fallstudie dazu. Weitere Informationen finden Sie unter [Anleitung für UWP-Apps](https://msdn.microsoft.com/library/windows/apps/dn894631).

Die portierte App besteht aus einem **ListBox**-Element, das an ein Ansichtsmodell gebunden ist. Das Ansichtsmodell verfügt über eine Liste mit Büchern, für die Titel, Autor und Bucheinband angezeigt werden. Für die Bucheinbandbilder ist **Buildvorgang** auf **Inhalt** und **In Ausgabeverzeichnis kopieren** auf **Nicht kopieren** festgelegt.

Die vorherigen Themen in diesem Abschnitt beschreiben die Unterschiede zwischen den Plattformen und bieten umfassende Informationen und Anleitungen zum Portierungsprozess für verschiedene Aspekte einer App, vom XAML-Markup über die Bindung an ein Ansichtsmodell bis hin zum Zugreifen auf Daten. Dieser Leitfaden soll anhand einer Fallstudie ergänzt werden, indem ein praktisches Beispiel vorgestellt wird. Bei den Fallstudien wird davon ausgegangen, dass Sie die Anleitung gelesen haben. Sie wird nicht wiederholt.

**Beachten Sie**    beim Öffnen von Bookstore1Universal\_10 in Visual Studio, wenn die Meldung "Visual Studio-Update erforderlich", klicken Sie dann die Schritte in [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Downloads

[Herunterladen der Bookstore1\_81 Universal 8.1 app](https://go.microsoft.com/fwlink/?linkid=532946).

[Herunterladen der Bookstore1Universal\_10 Windows 10-app](https://go.microsoft.com/fwlink/?linkid=532950).

## <a name="the-universal-81-app"></a>Die universelle 8.1-App

Hier ist welche Bookstore1\_81 – die app, die wir an Port jetzt – wie folgt aussieht. Es handelt sich dabei einfach um ein Listenfeld mit Büchern mit vertikalem Bildlauf unter der Überschrift des App-Namens und dem Seitentitel.

![wie bookstore1\-81 sieht unter Windows](images/w8x-to-uwp-case-studies/c01-01-win81-how-the-app-looks.png)

Bookstore1\_81 für Windows

![wie bookstore1\-81 sieht auf Windows Phone](images/w8x-to-uwp-case-studies/c01-02-wp81-how-the-app-looks.png)

Bookstore1\_81 auf Windows Phone

##  <a name="porting-to-a-windows10-project"></a>Portieren auf einem Windows 10-Projekt

Die Bookstore1\_81-Lösung ist ein 8.1 universelle App-Projekt, und sie diese Projekte enthält.

-   Bookstore1\_81.Windows. Dies ist das Projekt, das das app-Paket für Windows 8.1-builds.
-   Bookstore1\_81.WindowsPhone. Dies ist das Projekt, das das App-Paket für Windows Phone 8.1 erstellt.
-   Bookstore1\_81.Shared. Dies ist das Projekt, das den Quellcode, die Markupdateien und andere Assets und Ressourcen enthält, die von den beiden anderen Projekten verwendet werden.

Für diese Fallstudie verwenden wir die üblichen Optionen, die unter [Bei einer Universal 8.1-App](w8x-to-uwp-root.md) in Bezug auf die zu unterstützenden Geräte beschrieben sind. Die Entscheidung hier ist ein einfaches Beispiel: Diese app weist dieselben Funktionen auf und führt den größten Teil davon mit den gleichen Code, in der Windows 8.1 und Windows Phone 8.1-Formularen. Daher müssen wir port den Inhalt des freigegebenen Projekts (und was sonst noch von anderen Projekten wir müssen) auf Windows 10, die Ziel die universellen Gerätefamilie (eine, die Sie auf die größtmögliche Palette an Geräten installieren können).

Es ist eine sehr schnell Aufgabe erstellen Sie ein neues Projekt in Visual Studio Kopieren von Dateien über darauf aus Bookstore1\_81, und schließen Sie die kopierten Dateien in das neue Projekt. Erstellen Sie zunächst ein neues Projekt vom Typ „Leere Anwendung“ (Windows Universal). Nennen Sie sie Bookstore1Universal\_10. Hierbei handelt es sich um die Dateien kopiert aus Bookstore1\_81 bis Bookstore1Universal\_10.

**Über das freigegebene Projekt**

-   Kopieren Sie den Ordner mit den Buch Cover PNG-Dateien (der Ordner ist \\Assets\\CoverImages). Vergewissern Sie sich nach dem Kopieren des Ordners im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf den kopierten Ordner, und klicken Sie dann auf **Zu Projekt hinzufügen**. Mit „Einschließen“ von Dateien oder Ordnern in einem Projekt meinen wir diesen Befehl. Klicken Sie jedes Mal, wenn Sie eine Datei oder einen Ordner kopieren, für jede Kopie im **Projektmappen-Explorer** auf **Aktualisieren**, und schließen Sie dann die Datei oder den Ordner in das Projekt ein. Dies ist nicht für Dateien erforderlich, die Sie am Ziel ersetzen.
-   Kopieren Sie den Ordner, die die Quelldatei des Ansicht-Modell enthält (der Ordner ist \\"ViewModel").
-   Kopieren Sie „MainPage.xaml“, und ersetzen Sie die Datei am Ziel.

**Aus dem Windows-Projekt**

-   Kopieren Sie „BookstoreStyles.xaml“. Wir verwenden beziehungsproblemen wie ein guter Ausgangspunkt, da die Ressourcenschlüssel in dieser Datei in einer Windows 10-app aufgelöst werden; Einige der Werte in die entsprechende WindowsPhone-Datei nicht der Fall ist.

Bearbeiten Sie die Quelldateien von Code und Markup, das Sie gerade kopiert haben, und ändern Sie alle Verweise auf die Bookstore1\_81 Namespace Bookstore1Universal\_10. Eine schnelle Möglichkeit dafür ist die Verwendung des Features **In Dateien ersetzen**. Weder im Ansichtsmodell, noch in einem anderen imperativen Code sind Codeänderungen erforderlich. Aber, nur um ihn leichter sehen, welche Version der app ausgeführt wird, ändern Sie den Rückgabewert von der **Bookstore1Universal\_10.BookstoreViewModel.AppName** Eigenschaft von "BOOKSTORE1\_81" " BOOKSTORE1UNIVERSAL\_10".

Jetzt können Sie mit der Erstellung und Ausführung beginnen. Sieht wie unsere neue UWP-app nach danach keine explizite Arbeit noch, um es auf Windows 10 portieren.

![Die Windows 10-App mit Änderungen am ursprünglichen Quellcode](images/w8x-to-uwp-case-studies/c01-03-desk10-initial-source-code-changes.png)

Die Windows 10-app mit Änderungen am ersten Quellcode auf einem Desktopgerät ausgeführt wird

![Die Windows 10-App mit Änderungen am ursprünglichen Quellcode](images/w8x-to-uwp-case-studies/c01-04-mob10-initial-source-code-changes.png)

Die Windows 10-app mit Änderungen am ersten Quellcode auf einem mobilen Gerät ausgeführt wird

Die Ansicht und das Ansichtsmodell arbeiten ordnungsgemäß zusammen, und das **ListBox**-Element funktioniert. Wir müssen lediglich die Formatierung ändern. Auf einem mobilen Gerät mit hellem Design können wir den Rahmen des Listenfelds sehen, der sich jedoch einfach ausblenden lässt. Und die Schrift ist zu groß, sodass wir die verwendeten Stile ändern. Darüber hinaus sollte die App helle Farben verwenden, wenn sie auf einem Desktopgerät ausgeführt wird, damit sie standardmäßig aussieht. Wir werden dies also ändern.

## <a name="universal-styling"></a>Universelle Formatierung

Die Bookstore1\_81 app verwendet zwei unterschiedliche Ressourcenverzeichnisse (BookstoreStyles.xaml) die Stile auf die Betriebssysteme Windows 8.1 und Windows Phone 8.1 anzupassen. Keiner dieser beiden BookstoreStyles.xaml-Dateien enthält genau die Stile, die für die Windows 10-app erforderlich. Die gute Nachricht ist jedoch, dass unsere Vorstellungen tatsächlich sehr viel einfacher sind als diese beiden. In den nächsten Schritten werden wir daher hauptsächlich unsere Projektdateien und das Markup entfernen und vereinfachen. Die Schritte finden Sie nachfolgend: Sie können die Links oben in diesem Thema verwenden, um die Projekte herunterzuladen und die Ergebnisse dieser Änderungen zwischen diesem Punkt und dem Ende der Fallstudie anzuzeigen.

-   Um den Abstand zwischen den Elementen zu verkleinern, suchen Sie die `BookTemplate`-Datenvorlage in „MainPage.xaml“, und löschen Sie `Margin="0,0,0,8"` aus dem Stamm-**Grid**.
-   Auch in `BookTemplate`gibt es Verweise auf `BookTemplateTitleTextBlockStyle` und `BookTemplateAuthorTextBlockStyle`. Bookstore1\_81 verwendet diese Schlüssel als eine Dereferenzierung, sodass ein einzelner Schlüssel verschiedene Implementierungen in beiden apps haben. Diese Dereferenzierung wird nicht mehr benötigt; wir können direkt auf die Systemstile verweisen. Ersetzen Sie daher diese Verweise durch `TitleTextBlockStyle` bzw. `SubtitleTextBlockStyle`.
-   Jetzt müssen wir den Hintergrund von `LayoutRoot` auf den richtigen Standardwert festlegen, damit die App unabhängig vom Design bei der Ausführung auf allen Geräten entsprechend gut aussieht. Ändern Sie den Wert von `"Transparent"` in `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.
-   Ändern Sie in `TitlePanel` den Verweis auf `TitleTextBlockStyle` (was nun ein wenig zu groß ist) in einen Verweis auf `CaptionTextBlockStyle`. `PageTitleTextBlockStyle` wird von einem anderen Bookstore1\_81 Dereferenzierung, die wir nicht mehr benötigen. Ändern Sie dies, sodass stattdessen auf `HeaderTextBlockStyle` verwiesen wird.
-   Für **ListBox** müssen keine speziellen Background-, Style- oder ItemContainerStyle-Werte mehr festgelegt werden. Löschen Sie daher diese drei Attribute und deren Werte aus dem Markup. Wir möchten den Rand von **ListBox** ausblenden und fügen daher `BorderBrush="{x:Null}"` hinzu.
-   Wir verweisen nicht mehr auf eine Ressource in der **ResourceDictionary**-Datei namens „BookstoreStyles.xaml“. Sie können alle diese Ressourcen löschen. Löschen Sie jedoch nicht die Datei „BookstoreStyles.xaml“ selbst. Diese können wir noch ein letztes Mal verwenden, wie Sie im nächsten Abschnitt sehen werden.

Nach den letzten Formatierungsvorgängen sieht die App folgendermaßen aus.

![Die fast portierte Windows 10-App](images/w8x-to-uwp-case-studies/c01-05-desk10-almost-ported.png)

Die fast portiert Windows 10-app ausgeführt wird, auf einem Desktopgerät

![Die fast portierte Windows 10-App](images/w8x-to-uwp-case-studies/c01-06-mob10-almost-ported.png)

Die fast portiert Windows 10-app auf einem mobilen Gerät ausgeführt wird

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

Der portierten Windows 10-app auf einem mobilen Gerät ausgeführt wird

## <a name="conclusion"></a>Abschluss

In dieser Fallstudie wurde der Prozess zum Portieren einer einfachen App gezeigt – einer zugegebenermaßen unrealistisch einfachen App. Beispielsweise kann ein Listenfeld für die Auswahl oder die Herstellung eines Navigationskontexts verwendet werden; die App navigiert zu einer Seite mit weiteren Details zum ausgewählten Element. Diese bestimmte App führt keine Aktionen mit der Auswahl des Benutzers aus und verfügt nicht über Navigation. Dennoch diente die Fallstudie dazu, den Portierungsprozess vorzustellen und wichtige Techniken zu veranschaulichen, die Sie in echten UWP-Apps verwenden können.

Wir konnten uns auch davon überzeugen, dass das Portieren von Ansichtsmodellen in der Regel ein reibungsloser Prozess ist. Die Benutzeroberfläche und die Formfaktorunterstützung sind Aspekte, die beim Portieren mit höherer Wahrscheinlichkeit unsere Aufmerksamkeit erfordern.

Die nächste Fallstudie ist [Bookstore2](w8x-to-uwp-case-study-bookstore2.md), in der wir den Zugriff auf und die Anzeige von gruppierten Daten behandeln.
