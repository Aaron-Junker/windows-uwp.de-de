---
title: Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One
description: Einrichten des Computers und der Xbox One für die UWP-Entwicklung
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 19756730177485c6d16ad9a42ff1174eba8ca3b9
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820308"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One

Führen Sie die folgenden Schritte **sorgfältig** aus, um Ihren PC und Xbox One erfolgreich für die Entwicklung für die Universelle Windows-Plattform einzurichten. Lesen Sie nach der Einrichtung die Seite [UWP für Xbox One](index.md), um mehr über den Entwicklermodus auf Xbox One und das Erstellen von UWP-Apps zu erfahren. 

## <a name="before-you-start"></a>Bevor Sie beginnen

Bevor Sie beginnen, müssen Sie die folgenden Schritte ausführen:
-   Richten Sie einen PC mit der neuesten Version von Windows 10.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2019.

    > [!NOTE]
    > Visual Studio 2019 is required if you are using the Windows 10, build 15063 SDK. -->

- Sorgen Sie für mindestens 5 GB freien Speicherplatz auf Ihrer Xbox One.

## <a name="setting-up-your-development-pc"></a>Einrichten des Entwicklungs-PCs

1.  Installieren Sie Visual Studio 2015 Update 3, Visual Studio 2017 oder Visual Studio-2019.

    Wenn Sie Visual Studio 2015 Update 3 installieren, stellen Sie sicher, dass Sie auswählen **benutzerdefinierte** installieren, und wählen Sie die **Entwicklungstools für universelle Windows-App** Kontrollkästchen – es ist nicht Teil der standardmäßigen zu installieren. Achten Sie als C++-Entwickler darauf, **Benutzerdefinierte Installation** auszuwählen. Wählen Sie **C++** aus.

    Wenn Sie Visual Studio 2017 oder Visual Studio-2019 installieren, stellen Sie sicher, dass Sie auswählen, die **Universal Windows Platform Development** arbeitsauslastung. Wenn Sie in C++-Entwickler sind die **Zusammenfassung** auf der rechten Seite unter **universelle Windows-Plattform-Entwicklung**, stellen Sie sicher, dass Sie auswählen, die **C++ Universal Windows Platform-Tools** Kontrollkästchen. Es ist nicht Teil der Standardinstallation.

    Weitere Informationen finden Sie unter [Ihrer UWP auf Xbox-Entwicklungsumgebung einrichten](development-environment-setup.md).

2.  Installieren Sie das neueste [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

3.  Aktivieren Sie den Entwicklermodus für den Entwicklungs-PC (**Settings / Update und Sicherheit / für Entwickler bzw. verwenden Sie die Funktionen für Entwickler / Entwicklermodus**).

Nachdem Ihr Entwicklungs-PC nun bereit ist, können Sie sich dieses Video ansehen oder weiterlesen, um zu erfahren, wie Sie Ihre Xbox One für die Entwicklung einrichten und darauf eine UWP-App erstellen und bereitstellen.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Einrichten Ihrer Xbox One-Konsole

1.  Aktivieren Sie den Entwicklermodus auf der Xbox One. Die app herunterladen, erhalten Sie den Aktivierungscode und geben Sie ihn in das **verwalten deiner Xbox One Konsolen** Seite im Partner Center-app-Entwicklerkonto. Weitere Informationen finden Sie unter [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md). 

2.  Öffnen der **Dev Mode Aktivierung** app, und wählen **Switch und Neustart**. Herzlichen Glückwunsch! Ihre Xbox One befindet sich nun im Entwicklermodus.
  
  > [!NOTE]
  > Ihre Einzelhandelsspiele und -Apps werden im Entwicklermodus nicht ausgeführt, die von Ihnen erstellten Apps oder Spiele werden jedoch ausgeführt. Wechseln Sie zurück in den Einzelhandelsmodus, um Ihre Lieblingsspiele und -Apps auszuführen.
    
  > [!NOTE]
  > Damit Sie eine App auf der Xbox One-Konsole im Entwicklermodus bereitstellen können, muss ein Benutzer an der Konsole angemeldet sein. Sie können entweder ein vorhandenes Xbox Live-Konto verwenden oder ein neues Konto für Ihre Konsole im Entwicklermodus erstellen. 

## <a name="creating-your-first-project-in-visual-studio"></a>Erstellen Ihr erste Projekt in Visual Studio

Weitere Informationen finden Sie unter [Ihrer UWP auf Xbox-Entwicklungsumgebung einrichten](development-environment-setup.md).

1.  **Für C#** : Erstellen Sie ein neues universelles Windows-Projekt, und klicken Sie in der **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften**. Wählen Sie die **Debuggen** Registerkarte **Zielgerät** zu **Remotecomputer**, geben Sie die IP-Adresse oder den Hostnamen, der Ihre Xbox One-Konsole in der **Remotecomputer**  Feld, und wählen **universell (unverschlüsseltes Protokoll)** in die **Authentifizierungsmodus** Dropdown-Liste.   

    Die IP-Adresse Ihrer Xbox One finden Sie, indem Sie Dev Home auf der Konsole starten (die große Kachel auf der rechten Seite der Startseite) und in der oberen linken Ecke suchen. Weitere Informationen zu Dev Home finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).  

2.  **Für C++ und HTML/Javascript-Projekte**: Sie folgen einen ähnlichen Pfad wie C# Projekte, aber in den Projekteigenschaften finden Sie unter den **Debuggen** Registerkarte **Remotecomputer** in der Debugger an die Dropdown-Liste zu öffnen, geben Sie die IP-Adresse oder den Hostnamen des die Konsole in der **Computername** Feld, und wählen **universell (unverschlüsseltes Protokoll)** in die **Authentifizierungstyp** Feld.

3. Wählen Sie **X64** aus der Dropdownliste links neben die grüne Wiedergabeschaltfläche in der oberen Menüleiste.
   
4.  Wenn Sie F5 drücken, wird Ihre App erstellt, und die Bereitstellung auf der Xbox One wird gestartet.
  
5.  Wenn Sie diesen Vorgang zum ersten Mal durchführen, fordert Visual Studio Sie zur Eingabe einer PIN für Ihre Xbox One auf. Erhalten Sie eine PIN durch Starten des Dev-Startseite für Ihre Xbox One und Auswählen der **zeigen Visual Studio-Pin** Schaltfläche.
  
6.  Nach der Kopplung wird die Bereitstellung der App gestartet. Beim ersten Mal kann diese Bereitstellung ein wenig langsam sein (da alle Tools auf Ihre Xbox kopiert werden müssen). Wenn dieser Vorgang jedoch länger als wenige Minuten dauert, ist wahrscheinlich ein Problem aufgetreten. Stellen Sie sicher, dass Sie alle oben genannten Schritte ausgeführt haben – haben Sie den **Authentifizierungsmodus** auf **Universell** festgelegt? Vergewissern Sie sich außerdem, dass Sie eine drahtgebundene Netzwerkverbindung mit der Xbox One verwenden.  

7. Lehnen Sie sich nun ganz entspannt zurück. Viel Spaß mit Ihrer ersten App auf der Konsole!  

## <a name="thats-it"></a>Das ist alles!

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Siehe auch  
- [Häufig gestellte Fragen](frequently-asked-questions.md)  
- [Bekannte Probleme mit der UWP auf Xbox-Entwicklerprogramm](known-issues.md)
- [UWP auf Xbox One](index.md) 
