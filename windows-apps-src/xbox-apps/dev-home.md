---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Entwickler-Startseite auf der Konsole (Dev Home)
description: Enthält Informationen über die Dev Home-App für Xbox One.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, UWP
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620845"
---
# <a name="developer-home-on-the-console-dev-home"></a>Entwickler-Startseite auf der Konsole (Dev Home)
   
  
Dev Home ist ein Tool im Xbox One Development Kit, das die Produktivität von Entwicklern unterstützen soll. Dev Home bietet Funktionen zum Verwalten und Konfigurieren Ihres Development Kits, zum Verwalten von Benutzern, zum Starten von installierten Titeln sowie zur Erfassung und Ablaufverfolgung. In zukünftigen Versionen werden wir den Funktionsumfang basierend auf Ihrem Feedback um zusätzliche Funktionen erweitern sowie eine Erweiterung um benutzereigene Tools ermöglichen.   
   
  
Wir sind sehr an Ihrem Feedback zu Dev Home und den Szenarien interessiert, für die Sie am dringendsten Unterstützung benötigen. Geben Sie Ihre Kommentare im Hauptmenü der App gemäß der unter **Feedback senden** beschriebenen Methoden oder über den Developer Account Manager (DAM) ein.   
   
  
So starten Sie Dev Home nach der Wiederherstellung am November 2015 oder später:  
 
   1. Öffnen Sie auf dem Startbildschirm links das Menü oder doppelklicken Sie auf die Schaltfläche „Nexus“.  
   1. Wechseln Sie nach unten zu **Einstellungen** (das Zahnradsymbol).   
   1. Wählen Sie **Alle Einstellungen** aus.  
   1. Wählen Sie auf der **Entwickler**-Standardseite den **Entwickler-Startbildschirm** (das Symbol für den Startbildschirm) aus.   

 ![](images/dev_home_icons.png)   
  
Wählen Sie in früheren Wiederherstellungen die Dev Home-Kachel rechts auf dem Startbildschirm unter den **ausgewählten Inhalten** aus oder zeigen Sie die Anwendungsliste im Xbox One-Manager an, und starten Sie **Dev Home**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Benutzeroberfläche  
   
  
Die Kopfzeile der Dev Home-Benutzeroberfläche enthält die folgenden wichtigen Informationen „auf einen Blick“ zur Entwicklungskonsole:   
 
   *  **IP-Konsole:** Die aktuelle IP-Adresse der Konsole.   
   *  **Name der Konsole:** Der aktuelle Hostname der Konsole.  
   *  **Sandkasten:** Der Name der Sandbox, der in die Konsole ist.  
   *  **Betriebssystemversion:** Die aktuelle Version von Site Recovery, die in der Konsole ausgeführt wird.
   *  Die aktuelle Systemzeit.   

   
  
Die übrige Dev Home-Benutzeroberfläche ist in die folgenden Seiten unterteilt. Weitere Informationen zu den Tools auf diesen Seiten finden Sie in den jeweiligen Themen.   
 
   *  [Home](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Einstellungen](devhome-settings.md)  
   *  [Medien erfassen](devhome-capture.md)  
   *  [Netzwerk](devhome-networking.md)  
   *  [Leistung](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Hauptmenü  
   
  
Durch Drücken der **Menü**-Schaltfläche auf dem Controller können Sie auf das Hauptmenü zugreifen, in dem Sie den App-Workspace konfigurieren sowie Anmeldeinformationen für den Zugriff auf Netzwerkorte und Informationen zur Bereitstellung von Feedback in der App verwalten können.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Andockmodus UX  
   
  
Verschiedene vorhandene und neue Tools in Dev Home wie Netzwerke und Multiplayer sind so konzipiert, dass Sie auf der Seite angedockt werden können, während Sie den Titel ausführen. Auf diese Weise haben Sie einfachen Zugriff auf Tools beim Testen.   
   
  
Um den Andockmodus zu verwenden, markieren Sie den Titel des entsprechenden Tools, drücken Sie die Schaltfläche **Ansicht** auf Ihrem Controller, und wählen Sie im Kontextmenü **Andocken**.  
 ![](images/dev_home_4.png)   
  
Dev Home wird rechts angedockt. Sie können den Kontext umschalten, indem Sie wie gewohnt auf die Schaltfläche „Nexus“ doppeltippen.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Anpassen von Dev Home  
   
  
Dev Home kann angepasst und personalisiert werden. Sie können die App Ihrem Workflow entsprechend konfigurieren und anschließend als Arbeitsbereich speichern. Dieser Arbeitsbereich kann exportiert und importiert werden, sodass Sie nach Bedarf das Layout auf andere Konsolen kopieren können. Diese Optionen finden Sie im Hauptmenü unter **Arbeitsbereich**. Die exportierte Datei wird auf dem Scratch-Systemlaufwerk im `Dev Home\Workspaces` Verzeichnis abgelegt.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Ändern der Größe und Neuanordnen von Tools  
   
  
Um die Größe oder Position eines Tools zu ändern, verwenden Sie die Kontextmenüschaltfläche (Schaltfläche „Anzeigen“ auf dem Controller), während der Titel im Fokus steht. Wählen Sie im Kontextmenü **Verschieben** oder **Größe ändern**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Ändern der Designfarbe und des Hintergrundbilds  
   
  
Sie können im Hauptmenü **Arbeitsbereich** und dann **Designfarbe ändern**auswählen. Wählen Sie eine neue Farbe und klicken Sie dann auf **Speichern**, um die Designfarbe zum Hervorheben des Fokus zu aktualisieren.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Festlegen der Standardanwendung für ein Paket  
   
  
Wenn ein Paket mehrere Anwendungen enthält, können Sie mit Dev Home die Standardanwendung festlegen, die gestartet werden soll. Markieren Sie das Paket im Startprogramm, und drücken Sie die Schaltfläche **A**, um die Liste der verfügbaren Anwendungen zu öffnen. Markieren Sie die Anwendung, die Sie als Standardanwendung festlegen möchten. Drücken Sie im Kontextmenü die **Ansicht**-Taste, und wählen Sie dann **als Standard festlegen**.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Verwenden von Dev Home zum Registrieren und Starten von Titeln aus einer Netzwerkfreigabe  
   
  
Sie können im Startprogramm unten bei den installierten Apps und der Spieleliste die Option **Spiele aus einer Netzwerkfreigabe registrieren** auswählen, um die lose Dateiversion eines Titels extern auszuführen.   
 ![](images/dev_home_8.png)   
  
Anschließend haben Sie die Möglichkeit, den Netzwerkpfad zur Datei „appxmanifest.xml“ für den Titel einzugeben, den Sie registrieren möchten. Dev Home versucht dann, den Titel unter Verwendung aller vorhandenen Anmeldeinformationen für diese Freigabe zu registrieren und fordert Sie ggf. zur Eingabe neuer Netzwerkanmeldeinformationen auf. Wenn Sie auf zusätzliche Freigaben (z. B. für den Zugriff symbolisch verknüpfter Ressourcen auf einem separaten Server) zugreifen müssen, müssen Sie diese Anmeldeinformationen unter Anwendung der untenstehenden Option hinzufügen.   
   
  
Sie können diese gespeicherten Anmeldeinformationen auf der Konsole über die Option **Netzwerkanmeldeinformationen verwalten** des Hauptmenüs verwalten (und weitere Anmeldeinformationen hinzufügen).   
 ![](images/dev_home_9.png)   
  
Die aktuell auf der Konsole verfügbaren Anmeldeinformationen können angezeigt werden. Darüber hinaus können Sie die Anmeldeinformationen bearbeiten: Wählen Sie dazu den Pfad zu den Anmeldeinformationen aus, klicken Sie auf die Schaltfläche **A** und entfernen Sie bestimmte Anmeldeinformationen, indem Sie den „Entfernen“-Link auswählen und auf die Schaltfläche **A** klicken.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>Inhalt dieses Abschnitts  
  
[Auf der Startseite (Dev-Startseite)](devhome-home.md)  


&nbsp;&nbsp;Bietet schnellen Zugriff auf die Aufgaben, die für eine entwicklungskonsole regelmäßig ausgeführt werden. 
  
  
[Xbox Live-Seite (Dev-Startseite)](devhome-live.md)  


&nbsp;&nbsp;Multiplayer-Informationen erfasst und zeigt den aktuellen Status des Xbox Live-Diensts. 
  
  
[Seite "Einstellungen" (Dev-Startseite)](devhome-settings.md)  


&nbsp;&nbsp;Bietet Zugriff auf verschiedene Einstellungen für die entwicklungskonsole. 
  
  
[Medienaufzeichnung Seite (Dev-Startseite)](devhome-capture.md)  


&nbsp;&nbsp;Die **medienaufzeichnung** Dev-Startseite auf der Seite erfasst Video des Titels, der derzeit auf der Konsole ausgeführt wird. 
  
  
[Seite "Netzwerke" (Dev-Startseite)](devhome-networking.md)  


&nbsp;&nbsp;Simuliert den verschiedenen Netzwerke Bedingungen für die Problembehandlung. 
  
  
[Seite "Leistung" (Dev-Startseite)](devhome-performance.md)  


&nbsp;&nbsp;Simuliert den verschiedenen Datenträgeraktivität und CPU-Auslastung Bedingungen für die Problembehandlung. 
 