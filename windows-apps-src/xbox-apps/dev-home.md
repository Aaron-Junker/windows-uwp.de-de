---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Startseite für Entwickler in der Konsole (dev Home)
description: Erfahren Sie, wie die Entwickler-Start Tools-Funktion auf dem Xbox One Console Development Kit die Entwickler Produktivität unterstützt.
ms.date: 08/09/2017
ms.topic: article
keywords: Windows 10, UWP
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 291b25e962aa8ac37705fd0db544f138036949d2
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636610"
---
# <a name="developer-home-on-the-console-dev-home"></a>Startseite für Entwickler in der Konsole (dev Home)
   
  
Dev Home ist ein Tool, das auf dem Xbox One Development Kit basiert, das Entwicklern die Produktivität erleichtern soll. Dev Home bietet Funktionen zum Verwalten und Konfigurieren Ihres Development Kits, zum Verwalten von Benutzern, zum Starten installierter Titel und zum Durchführen von Aufzeichnungen und Ablauf Verfolgungen. In zukünftigen Versionen werden wir die Funktionalität weiter erweitern, um zusätzliche Features basierend auf Ihrem Feedback zu aktivieren und die Erweiterbarkeit und das Hinzufügen eigener Tools zu ermöglichen.   
   
  
Wir freuen uns über Ihr Feedback zu dev Home und den Szenarien, die Sie am meisten interessiert, wenn Sie es unterstützen. Geben Sie Ihre Kommentare über die unter **Feedback senden** im Hauptmenü der APP oder über den Entwicklerkonto-Manager (Dam) beschriebenen Methoden ein.   
   
  
So starten Sie dev Home bei der Wiederherstellung von November 2015 oder höher:  
 
   1. Öffnen Sie das Handbuch, indem Sie nach links navigieren, oder Doppelklicken Sie auf die Schaltfläche Nexus.  
   1. Nach unten zu **Einstellungen** (das Zahnrad Symbol)   
   1. **Alle Einstellungen** auswählen  
   1. Wählen Sie auf der Standard **Entwickler** Seite **Developer Home** (Start Symbol) aus.   

 ![Screenshot der Seite "Entwickler" in "Einstellungen".](images/dev_home_icons.png)   
  
Wählen Sie unter frühere Wiederherstellungen auf der rechten Seite des Startbildschirms in den **angezeigten Inhalten** die Kachel dev-Home aus, oder zeigen Sie die Anwendungsliste in Xbox One-Manager an, und starten Sie **dev Home**.   
 ![Screenshot der Registerkarte "Anwendungen", die mit hervorgehobener Startoption angezeigt wird.](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Benutzeroberfläche  
   
  
Der Header der Benutzeroberfläche von dev Home enthält die folgenden wichtigen Informationen zur Entwicklungs Konsole:   
 
   *  **Konsolen-IP:** Die aktuelle IP-Adresse der Konsole.   
   *  **Konsolen Name:** Der aktuelle Hostname der Konsole.  
   *  **Sandbox:** Der Name der Sandbox, in der sich die Konsole befindet.  
   *  **Betriebssystemversion:** Die aktuelle Wiederherstellungs Version, die in der-Konsole ausgeführt wird.
   *  Aktuelle Systemzeit.   

   
  
Der Rest der Benutzeroberfläche von dev Home ist in die folgenden Seiten unterteilt. Weitere Informationen zu den Tools auf diesen Seiten finden Sie in den jeweiligen Themen.   
 
   *  [Startseite](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Einstellungen](devhome-settings.md)  
   *  [Medien Erfassung](devhome-capture.md)  
   *  [Netzwerk](devhome-networking.md)  
   *  [Leistung](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Hauptmenü  
   
  
Durch Drücken der **Menü** Schaltfläche auf dem Controller können Sie auf das Hauptmenü zugreifen, das die Konfiguration des App-Arbeitsbereichs, die Möglichkeit zum Verwalten von Anmelde Informationen für den Zugriff auf Netzwerkadressen und Informationen zur Bereitstellung von Feedback zur APP ermöglicht.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Snap-in-UX  
   
  
Mehrere vorhandene und anstehende Tools in dev Home (z. b. Netzwerk und multiplayerfunktionen) sind so konzipiert, dass Sie bei der Ausführung ihres Titels an die Seite geleitet werden, sodass Sie beim Testen problemlos auf die Tools zugreifen können.   
   
  
Um auf den Snap-Modus zuzugreifen, markieren Sie den Titel des entsprechenden Tools, klicken Sie auf dem Controller auf die Schaltfläche **Ansicht** , **und wählen Sie** im Kontextmenü die Option ausrichten aus:  
 ![Screenshot der Entwicklungs Startseite mit hervorgehobener Snap-Option.](images/dev_home_4.png)   
  
Dev Home wird rechts angedockt. Sie können den Kontext umschalten, indem Sie wie gewohnt auf die Schaltfläche Nexus doppeltippen.  
 ![Screenshot der Entwicklungs Startseite, die anzeigt, dass Sie während des Tests auf die Tools zugreifen können.](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Anpassen von Dev Home  
   
  
Dev Home kann angepasst und personalisiert werden. Sie können die APP so konfigurieren, dass Sie Ihrem Workflow entspricht, und diese dann als Arbeitsbereich speichern. Dieser Arbeitsbereich kann exportiert und importiert werden, sodass Sie das Layout nach Bedarf auf andere Konsolen kopieren können. Diese Optionen finden Sie im Hauptmenü unter **Arbeitsbereich**. Die exportierte Datei befindet sich auf dem temporären Laufwerk des Systems im `Dev Home\Workspaces` Verzeichnis.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Ändern der Größe und Neuanordnen von Tools  
   
  
Um die Größe oder Position eines Tools zu ändern, verwenden Sie die Kontextmenü Schaltfläche (Schaltfläche anzeigen auf dem Controller), während der Titel den Fokus besitzt. Wählen Sie im Kontextmenü **verschieben** oder **Größe ändern**aus.   
 ![Screenshot der Entwicklungs Startseite, die auf der rechten Seite mit hervorgehobener Verschiebungs Option angezeigt wird.](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Ändern der Designfarbe und des Hintergrundbilds  
   
  
Im Hauptmenü können Sie den **Arbeitsbereich** auswählen und dann Design **Farbe ändern**. Wählen Sie eine neue Farbe aus, und klicken Sie auf **Speichern** , um die Design Farbe zu aktualisieren   
 ![Screenshot der Entwicklungs Startseite, die den Arbeitsbereich anzeigt und Design Farboptionen ändern ausgewählt hat.](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Festlegen der Standardanwendung für ein Paket  
   
  
Wenn ein Paket mehrere Anwendungen enthält, können Sie mit dev Home festlegen, dass die Standardanwendung gestartet wird. Markieren Sie das Paket im Start Programm, und **Klicken Sie auf die Schaltfläche** , um die Liste der verfügbaren Anwendungen zu öffnen. Markieren Sie den Wert, den Sie als Standard festlegen möchten, und klicken Sie auf die Schaltfläche **Ansicht** , und wählen Sie dann im Kontextmenü die Option **als Standard festlegen** aus.   
 ![Screenshot der Entwicklungs Startseite mit hervorgehobener Option "als Standard festlegen".](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Verwenden von dev Home zum Registrieren und starten von Titeln über eine Netzwerkfreigabe  
   
  
Über das Start Programm können Sie am unteren Rand der Liste installierte apps und Spiele die Option **Spiel von einer Netzwerkfreigabe registrieren** auswählen, um eine lose Dateiversion eines Titels Remote auszuführen.   
 ![Screenshot der Entwicklungs Startseite mit hervorgehobener Option "Spiel von einer Netzwerkfreigabe registrieren".](images/dev_home_8.png)   
  
Sie können dann den Netzwerkpfad zur appxmanifest.xml Datei für den Titel eingeben, den Sie registrieren möchten. Dev Home versucht, den Titel mithilfe vorhandener Anmelde Informationen für diese Freigabe zu registrieren. Falls erforderlich, werden neue Anmelde Informationen für das Netzwerk angezeigt. Wenn Sie auf zusätzliche Freigaben zugreifen müssen (z. b. um auf symbolisch verknüpfte Ressourcen auf einem separaten Server zuzugreifen), müssen Sie diese über die folgende Option hinzufügen.   
   
  
Sie können diese gespeicherten Anmelde Informationen (und weitere hinzufügen) in der-Konsole über die Option **Netzwerk Anmelde Informationen verwalten** im Hauptmenü verwalten.   
 ![Screenshot der dev-Startseite mit hervorgehobener Option "Netzwerk Anmelde Informationen verwalten"](images/dev_home_9.png)   
  
Sie können die Anmelde Informationen auf der Konsole anzeigen, Anmelde Informationen bearbeiten, indem Sie den Pfad der Anmelde Informationen auswählen und auf **eine** Schaltfläche klicken und eine der Anmelde Informationen entfernen, indem Sie den Link entfernen auswählen und auf **eine** Schaltfläche klicken.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>In diesem Abschnitt  
  
[Startseite (dev Home)](devhome-home.md)  


&nbsp;&nbsp;Ermöglicht den schnellen Zugriff auf die Aufgaben, die routinemäßig in einer Entwicklungs Konsole ausgeführt werden. 
  
  
[Xbox Live page (dev Home)](devhome-live.md)  


&nbsp;&nbsp;Erfasst multiplayerinformationen und zeigt den aktuellen Status des Xbox Live-Dienstanbieter an. 
  
  
[Seite "Einstellungen" (dev-Startseite)](devhome-settings.md)  


&nbsp;&nbsp;Bietet Zugriff auf verschiedene Einstellungen für die Entwicklungs Konsole. 
  
  
[Seite "Medien Erfassung" (dev-Startseite)](devhome-capture.md)  


&nbsp;&nbsp;Auf der Seite **Medien Erfassung** von dev Home wird ein Video des Titels erfasst, der derzeit in der Konsole ausgeführt wird. 
  
  
[Netzwerkseite (dev-Startseite)](devhome-networking.md)  


&nbsp;&nbsp;Simuliert verschiedene Netzwerkbedingungen zu Problem Behandlungszwecken. 
  
  
[Seite "Leistung" (dev-Startseite)](devhome-performance.md)  


&nbsp;&nbsp;Simuliert verschiedene Datenträger Aktivitäten und CPU-Nutzungsbedingungen zu Problem Behandlungszwecken. 
 