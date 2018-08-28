---
author: GrantMeStrength
ms.assetid: 03A74239-D4B6-4E41-B2FA-6C04F225B844
title: Hier erfahren Sie, wie Sie eine „Hallo Welt“-App (XAML) erstellen
description: Verwenden Sie XAML (Extensible Application Markup Language) mit C# zum Erstellen einer einfachen „Hallo Welt“-App, die auf die universelle Windows Plattform (UWP) unter Windows10 abzielt.
ms.author: jken
ms.date: 03/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP, erste App, Hallo Welt
ms.localizationpriority: medium
ms.openlocfilehash: 950b2f3fac44c8350a51fd5c1b7071f05c92d746
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/28/2018
ms.locfileid: "2886466"
---
# <a name="create-a-hello-world-app-xaml"></a>Erstellen der App „Hello, world“ (XAML)

In diesem Lernprogramm erfahren Sie, wie Sie XAML und C# zum Erstellen einer einfachen „Hello, World“-App für die universelle Windows-Plattform (UWP) unter Windows10 verwenden. Mit nur einem Projekt in Microsoft Visual Studio können Sie eine App erstellen, die auf allen Geräten mit Windows10 ausgeführt werden kann.

Hier erfahren Sie Folgendes:

-   Erstellen Sie ein neues **Visual Studio 2017**-Projekt für **Windows 10** und die **UWP**.
-   Schreiben Sie XAML zum Ändern der UI auf der Startseite.
-   Führen Sie das Projekt auf dem lokalen Desktop in Visual Studio aus.
-   Verwenden Sie einen SpeechSynthesizer, um die App sprechen zu lassen, wenn Sie auf eine Schaltfläche klicken.


## <a name="before-you-start"></a>Vorbereitung

-   [Was ist eine universelle Windows-App?](universal-application-platform-guide.md)
-   [Visual Studio 2017 (und Windows 10) herunterladen](https://developer.microsoft.com/windows/downloads). Hier erfahren Sie weitere Informationen über die [Vorbereitung](get-set-up.md).
-   Außerdem wird davon ausgegangen, dass Sie das Standardfensterlayout in Visual Studio verwenden. Wenn Sie das Standardlayout ändern, können Sie es im Menü **Fenster** mit dem Befehl **Fensterlayout zurücksetzen** wiederherstellen.

> [!NOTE]
> In diesem Lernprogramm wird Visual Studio Community 2017 verwendet. Wenn Sie eine andere Version von Visual Studio verwenden, kann das Programm für Sie etwas anders aussehen.

## <a name="video-summary"></a>Video-Zusammenfassung

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Writing-Your-First-Windows-10-App/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>



## <a name="step-1-create-a-new-project-in-visual-studio"></a>Schritt 1: Erstellen eines neuen Projekts in Visual Studio

1.  Starten Sie Visual Studio 2017.

2.  Wählen Sie im Menü **Datei** **Neu > Projekt** um das Dialogfeld *Neues Projekt* zu öffnen.

3.  Wählen Sie aus der Liste der Vorlagen auf der linken Seite, **Installed > Visual c# > universellen Windows** die Liste der UWP-Projektvorlagen anzeigen.

    (Wenn keine universellen Vorlagen angezeigt werden, fehlen möglicherweise die Komponenten zum Erstellen von UWP-Apps. Sie können die Installation wiederholen und UWP-Unterstützung hinzufügen, indem Sie im Dialogfeld *Neues Projekt* auf **Visual Studio-Installer öffnen** klicken. Finden Sie unter [Vorbereiten](get-set-up.md).)

    ![So wiederholen Sie den Installationsvorgang](images/win10-cs-install.png)

4.  Wählen Sie die Vorlage **Leere App (universelle Windows-App)** aus, und geben Sie „HelloWorld“ als **Name** ein. Wählen Sie **OK** aus.

    ![Das Fenster für ein neues Projekt](images/win10-cs-01.png)

> [!NOTE]
> Wenn Sie Visual Studio zum ersten Mal verwenden, wird möglicherweise das Dialogfeld „Einstellungen“ angezeigt, in dem Sie aufgefordert werden, **Entwicklermodus** zu aktivieren. Der Entwicklermodus ist eine spezielle Einstellung, die bestimmte Features ermöglicht, z.B. die Berechtigung zum Ausführen von Apps, direkt und nicht nur aus dem Store. Weitere Informationen finden Sie in [Aktivieren Ihres Geräts für die Entwicklung](enable-your-device-for-development.md). Wählen Sie **Entwicklermodus** aus, klicken Sie auf **Ja**, und schließen Sie das Dialogfeld, um mit dem Lernprogramm fortzufahren.

 ![Aktivieren des Dialogfelds für Entwicklermodus](images/win10-cs-00.png)

5.  Das Dialogfeld für die Zielversion/mindestens erforderliche Version wird angezeigt. Die Standardeinstellungen sind für dieses Lernprogramm in Ordnung, wählen Sie daher **OK** aus, um das Projekt zu erstellen.

    ![Das Fenster „Projektmappen-Explorer“](images/win10-cs-02.png)

6.  Wenn das neue Projekt geöffnet wird, werden die Dateien im Bereich **Projektmappen-Explorer** auf der rechten Seite angezeigt. Möglicherweise müssen Sie die Registerkarte **Projektmappen-Explorer** anstelle der Registerkarte **Eigenschaften** auswählen, um die Dateien anzuzeigen.

    ![Das Fenster „Projektmappen-Explorer“](images/win10-cs-03.png)

**Leere App (universelle Windows-App)** ist zwar nur eine Minimalvorlage, umfasst aber trotzdem eine Reihe von Dateien. Diese Dateien werden für alle UWP-Apps mit C# benötigt. Sie sind Teil jedes Projekts, das Sie mit Visual Studio erstellen.


### <a name="whats-in-the-files"></a>Inhalt der Dateien

Doppelklicken Sie zum Anzeigen und Bearbeiten einer Datei im Projekt im **Projektmappen-Explorer** auf die gewünschte Datei. Erweitern Sie eine XAML-Datei genau wie einen Ordner, um die zugeordnete Codedatei anzuzeigen. XAML-Dateien werden in einer geteilten Ansicht geöffnet, die sowohl die Entwurfsoberfläche als auch den XAML-Editor enthält.
> [!NOTE]
> Was ist XAML? Extensible Application Markup Language (XAML) ist die Sprache, die zum Definieren der Benutzeroberfläche Ihrer App verwendet wird. Sie kann manuell eingegeben oder mit den Visual Studio-Entwicklungstools erstellt wurden. Eine XAML-Datei verfügt über eine CodeBehind-Datei („.xaml.cs“), die die Logik enthält. Zusammen bilden XAML und CodeBehind eine vollständige Klasse. Weitere Informationen finden Sie in der [XAML-Übersicht](https://msdn.microsoft.com/library/windows/apps/Mt185595).

*„App.xaml“ und „App.xaml.cs“*

-   In „App.xaml“ deklarieren Sie Ressourcen, die in der gesamten App zur Anwendung kommen.
-   „App.xaml.cs“ ist die CodeBehind-Datei für „App.xaml“. Sie enthält wie alle CodeBehind-Seiten einen Konstruktor, der die `InitializeComponent`-Methode aufruft. Die `InitializeComponent`-Methode wird nicht von Ihnen geschrieben. Sie wird von Visual Studio generiert und dient in erster Linie dazu, die in der XAML-Datei deklarierten Elemente zu initialisieren.
-   „App.xaml.cs“ ist der Einstiegspunkt für Ihre App.
-   „App.xaml.cs“ enthält außerdem Methoden zum Behandeln der Aktivierung und Unterbrechung der App.

*MainPage.xaml*

-   In „MainPage.xaml“ definieren Sie die Benutzeroberfläche für Ihre App. Sie können Elemente direkt per XAML-Markup hinzufügen oder die Designtools von Visual Studio verwenden.
-   „MainPage.xaml.cs“ ist die CodeBehind-Seite für „MainPage.xaml“. Hier fügen Sie Ihre App-Logik und Ereignishandler hinzu.
-   Zusammen definieren diese beiden Dateien im `HelloWorld`-Namespace eine neue Klasse mit dem Namen `MainPage`, die von [**Page**](https://msdn.microsoft.com/library/windows/apps/BR227503) erbt.

*Package.appxmanifest*
-   Eine Manifestdatei, die Ihre App beschreibt (Name, Beschreibung, Kachel, Startseite usw.)
-   Umfasst eine Liste der Dateien, die Ihre App enthält.

*Ein Satz mit Logobildern*
-   „Assets/Square150x150Logo.scale-200.png“ stellt Ihre App im Startmenü dar.
-   „Assets/StoreLogo.png“ stellt Ihre App im Microsoft Store dar.
-   „Assets/SplashScreen.scale-200.png“ ist der Begrüßungsbildschirm, der beim Start der App angezeigt wird.

## <a name="step-2-adding-a-button"></a>Schritt 2: Hinzufügen von Schaltflächen

### <a name="using-the-designer-view"></a>Mithilfe der Entwurfsansicht

Fügen wir nun der Seite eine Schaltfläche hinzu. In diesem Lernprogramm verwenden Sie lediglich einige der zuvor aufgeführten Dateien: „App.xaml“, „MainPage.xaml“ und „MainPage.xaml.cs“.

1.  Doppelklicken Sie auf die Datei **MainPage.xaml**, um sie in der Entwurfsansicht zu öffnen.

    Sie werden feststellen, dass eine grafische Ansicht im oberen Teil des Bildschirms und die XAML-Codeansicht darunter vorhanden ist. Sie können jeweils Änderungen vornehmen, wir verwenden jetzt jedoch die grafische Ansicht.

    ![Das Fenster „Projektmappen-Explorer“](images/win10-cs-04.png)

2.  Klicken Sie auf die vertikale Registerkarte **Toolbox** auf der linken Seite, um die Liste der UI-Steuerelemente zu öffnen. (Sie können auf das Reißzweckensymbol in der Titelleiste klicken, damit sie sichtbar bleibt.)

    ![Das Fenster „Projektmappen-Explorer“](images/win10-cs-05.png)

3.  Erweitern Sie **Häufig verwendete XAML-Steuerelemente**, und ziehen Sie die **Schaltfläche** in die Mitte der Design-Canvas.

    ![Das Fenster „Projektmappen-Explorer“](images/win10-cs-06.png)

    Wenn Sie das Fenster mit dem XAML-Code betrachten, sehen Sie, dass die Schaltfläche auch dort hinzugefügt wurde:

 ```XAML
<Button x:name="button" Content="Button" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
 ```

4.  Ändern Sie den Text der Schaltfläche.

    Klicken Sie in der XAML-Codeansicht, und ändern Sie den Inhalt von „Schaltfläche“ in „Hello, World!“.

```XAML
<Button x:name="button" Content="Hello, world!" HorizontalAlignment="Left" Margin = "152,293,0,0" VerticalAlignment="Top"/>
```

Beachten Sie, wie die in der Design-Canvas angezeigte Schaltfläche aktualisiert wird, um den neuen Text anzuzeigen.

![Das Fenster „Projektmappen-Explorer“](images/win10-cs-07.png)

## <a name="step-3-start-the-app"></a>Schritt3: Starten der App


Sie haben nun eine sehr einfache App erstellt. Dies ist ein guter Zeitpunkt zum Erstellen, Bereitstellen und Starten Ihrer App, um sie in Aktion zu sehen. Sie können Ihre App auf dem lokalen Computer, in einem Simulator oder Emulator oder auf einem Remotegerät debuggen. Dies ist das Zielgerätmenü in Visual Studio.

![Dropdownliste mit Zielgeräten zum Debuggen Ihrer App](images/uap-debug.png)

### <a name="start-the-app-on-a-desktop-device"></a>Starten der App auf einem Desktop-Gerät

Standardmäßig wird die App auf dem lokalen Computer ausgeführt. Das Menü mit den Zielgeräten enthält mehrere Optionen zum Debuggen Ihrer App auf Geräten der Desktopfamilie.

-   **Simulator**
-   **Lokaler Computer**
-   **Remotecomputer**

**So beginnen Sie mit dem Debuggen auf dem lokalen Computer**

1.  Stellen Sie sicher, dass auf der **Standardsymbolleiste** im Menü mit den Zielgeräten (![Menü „Debuggen starten“](images/startdebug-full.png)) die Option **Lokaler Computer** ausgewählt ist. (Dies ist die Standardeinstellung.)
2.  Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen starten** (![Schaltfläche „Debuggen starten“](images/startdebug-sm.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen starten**.

   oder

   Drücken Sie F5.

Die App wird in einem Fenster geöffnet, und zuerst wird ein standardmäßiger Begrüßungsbildschirm angezeigt. Der Begrüßungsbildschirm setzt sich aus einem Bild (SplashScreen.png) und einer Hintergrundfarbe (in der Manifestdatei der App angegeben) zusammen.

Nach dem Ausblenden des Begrüßungsbildschirms wird Ihre App angezeigt. Sie sieht ungefähr wie folgt aus:

![Erster App-Bildschirm](images/win10-cs-08.png)

Drücken Sie die WINDOWS-TASTE, um das Menü **Start** zu öffnen, und zeigen Sie alle Apps an. Beachten Sie, dass beim lokalen Bereitstellen der App dem Menü **Start** die dazugehörige Kachel hinzugefügt wird. Wenn Sie die App später erneut ausführen möchten (nicht im Debugmodus), tippen oder klicken Sie im Menü **Start** auf die Kachel.

Viel zu bieten hat die App zwar noch nicht, aber trotzdem: Herzlichen Glückwunsch! Sie haben Ihre erste UWP-App erstellt!

**So beenden Sie das Debuggen**

   Klicken Sie auf der Symbolleiste auf die Schaltfläche **Debuggen beenden** (![Schaltfläche „Debuggen beenden“](images/stopdebug.png)).

   oder

   Klicken Sie im Menü **Debuggen** auf **Debuggen beenden**.

   oder

   Schließen Sie das App-Fenster.

## <a name="step-4-event-handlers"></a>Schritt4: Ereignishandler

„Ereignishandler“ klingt kompliziert, dies ist jedoch nur ein anderer Namen für den Code, der aufgerufen wird, wenn ein Ereignis auftritt (z.B. wenn der Benutzer auf die Schaltfläche klickt).

1.  Beenden Sie die Ausführung der App, sofern nicht bereits geschehen.

2.  Doppelklicken Sie auf das Schaltflächen-Steuerelement auf die Design-Canvas, damit Visual Studio einen Ereignishandler für die Schaltfläche erstellt.

  Natürlich können Sie den gesamten Code auch manuell erstellen. Oder Sie klicken zum Auswählen auf die Schaltfläche und suchen im Bereich **Eigenschaften** unten rechts. Wenn Sie zu **Ereignisse** (kleiner Gewitterblitz) wechseln, können Sie den Namen des Ereignishandlers hinzufügen.

3.  Bearbeiten Sie den Ereignishandlercode in *MainPage.xaml.cs*, der CodeBehind-Seite. An dieser Stelle wird die Sache interessant. Der Standard-Ereignishandler sieht wie folgt aus:

```C#
private void Button_Click(object sender, RoutedEventArgs e)
{

}
```

  Wir ändern ihn, damit er wie folgt aussieht:

```C#
private async void Button_Click(object sender, RoutedEventArgs e)
        {
            MediaElement mediaElement = new MediaElement();
            var synth = new Windows.Media.SpeechSynthesis.SpeechSynthesizer();
            Windows.Media.SpeechSynthesis.SpeechSynthesisStream stream = await synth.SynthesizeTextToStreamAsync("Hello, World!");
            mediaElement.SetSource(stream, stream.ContentType);
            mediaElement.Play();
        }
```

Geben Sie unbedingt auch das **async**-Schlüsselwort an, oder Sie erhalten beim Versuch, die App auszuführen, einen Fehler.

### <a name="what-did-we-just-do"></a>Was haben wir gerade gemacht?

Dieser Code verwendet Windows-APIs zum Erstellen eines Sprachsyntheseobjekts und gibt dann zu sprechenden Text an. (Weitere Informationen zur Verwendung von SpeechSynthesis finden Sie in den Dokumenten zum [SpeechSynthesis-Namespace](https://msdn.microsoft.com/library/windows/apps/windows.media.speechsynthesis.aspx).)

Wenn Sie die App ausführen und auf die Schaltfläche klicken, sagt Ihr Computer (oder das Handy) wörtlich „Hello, World!“.


## <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch, Sie haben Ihre erste App für Windows10 und die UWP erstellt!

Informationen dazu, wie Sie XAML für die Gestaltung der Steuerelemente in Ihrer App verwenden, finden Sie im [Rasterlernprogramm](../design/layout/grid-tutorial.md). Sie können auch direkt mit den [nächsten Schritten](learn-more.md) fortfahren.

## <a name="see-also"></a>Weitere Informationen

* [Ihre erste App](your-first-app.md)
* [Veröffentlichen Sie Ihre UWP-App](https://developer.microsoft.com/store/publish-apps).
* [Anleitungen zur Entwicklung von UWP-Apps](https://developer.microsoft.com/windows/apps/develop)
* [Codebeispiele für UWP-Entwickler](https://developer.microsoft.com/windows/samples)
* [Was ist eine universelle Windows-App?](universal-application-platform-guide.md)
* [Für Windows-Konto anmelden](sign-up.md)
