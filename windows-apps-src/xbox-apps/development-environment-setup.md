---
title: Einrichten der Umgebung für die UWP-Entwicklung auf Xbox
description: Schritte zum Einrichten und Testen der Umgebung für die UWP-Entwicklung auf Xbox
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.localizationpriority: medium
ms.openlocfilehash: 844aaf8ff1e85836683bf752d301c1254066ea5a
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258740"
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>Einrichten der Umgebung für die UWP-Entwicklung auf Xbox

Die Umgebung für die UWP-Entwicklung (Universelle Windows-Plattform) auf Xbox besteht aus einem Entwicklungscomputer, der über ein lokales Netzwerk mit einer Xbox One-Konsole verbunden ist.
The development PC requires Visual Studio 2015 Update 3, Visual Studio 2017, or Visual Studio 2019.
The development PC also requires Windows 10, the Windows 10 SDK build 14393 or later, and a range of supporting tools.

In diesem Artikel werden die Schritte zum Einrichten und Testen der Entwicklungsumgebung beschrieben.

## <a name="visual-studio-setup"></a>Einrichten von Visual Studio

1. Install Visual Studio 2015 Update 3, Visual Studio 2017, or Visual Studio 2019. Weitere Informationen und Downloads für die Installation finden Sie unter [Downloads und Tools für Windows 10](https://dev.windows.com/downloads). We recommend that you use the latest version of Visual Studio so that you can receive the latest updates for developers and security.


2. If you're installing Visual Studio 2017 or Visual Studio 2019, make sure that you choose the **Universal Windows Platform development** workload. Wenn Sie ein C++-Entwickler sind, stellen Sie sicher, dass Sie auch das Kontrollkästchen  **	UWP-Tools für C++** im Bereich **Zusammenfassung** rechts unter **Entwicklung für die universelle Windows-Plattform** wählen. Es ist nicht Teil der Standardinstallation.

    ![Install Visual Studio 2019](images/development-environment-setup-1.png)

    Wenn Sie Visual Studio 2015 Update 3 installieren, stellen Sie sicher, dass das Kontrollkästchen **Entwicklungstools für universelle Windows-Apps** aktiviert ist.

    ![Installieren von Visual Studio 2015 Update 2](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Einrichten des Windows 10 SDK

Installieren Sie das aktuelle Windows 10 SDK. Dies ist in der Installation von Visual Studio enthalten, aber wenn Sie es separat herunterladen möchten, finden Sie weitere Informationen unter [Windows 10 SDK](https://developer.microsoft.com/windows/downloads/windows-10-sdk).


## <a name="enabling-developer-mode"></a>Aktivieren des Entwicklermodus

Bevor Sie Apps von Ihrem Entwicklungs-PC bereitstellen können, müssen Sie den Entwicklermodus aktivieren. Navigieren Sie unter **Einstellungen** zu **Update und Sicherheit** / **Für Entwickler**, und wählen Sie unter **Entwicklerfunktionen verwenden** die Option **Entwicklermodus**.

## <a name="setting-up-your-xbox-one"></a>Einrichten Ihrer Xbox One

Bevor Sie eine App auf Ihrer Xbox One bereitstellen können, muss ein Benutzer auf der Konsole angemeldet sein. Sie können entweder ein vorhandenes Xbox Live-Konto verwenden oder ein neues Konto für Ihre Konsole im Entwicklermodus erstellen. 

## <a name="create-your-first-app"></a>Erstellen Ihrer ersten App

1. Stellen Sie sicher, dass sich der Entwicklungscomputer in demselben lokalen Netzwerk wie die gewünschte Xbox One befindet. Dies bedeutet normalerweise, dass beide denselben Router verwenden und sich im gleichen Subnetz befinden. Es wird eine drahtgebundene Netzwerkverbindung empfohlen.

2. Stellen Sie sicher, dass sich die Xbox One Konsole im Entwicklermodus befindet.  Weitere Informationen finden Sie unter [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md).

3. Legen Sie die Programmiersprache fest, die Sie für Ihre UWP-App verwenden möchten.

4. Wählen Sie auf Ihrem Entwicklungs-PC in Visual Studio **Neu/Projekt** aus.

5. Wählen Sie im Fenster **Neues Projekt** **Windows Universal/Leere App (Universal Windows)** aus.

### <a name="starting-a-c-project"></a>Starten eines C#-Projekts

  ![Dialogfeld „Neues Projekt“](images/development-environment-setup-2.png)

1. Wählen Sie im Dialogfeld **Neues universelles Windows-Projekt** Build 14393 oder höher im Dropdownmenü **Mindestens erforderliche Version** aus. Wählen Sie das aktuelle SDK im Dropdownmenü **Zielversion** aus. Wenn das Dialogfeld **Entwicklermodus** angezeigt wird, klicken Sie auf **OK**. Es wird eine neue leere App erstellt.

2. Konfigurieren Sie die Entwicklungsumgebung für das Remotedebuggen:

    a. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Eigenschaften**.

    b. Ändern Sie auf der Registerkarte **Debuggen** die Option **Plattform** in **x64**. (x86 wird nicht mehr als Plattform auf Xbox unterstützt.)

    c. Ändern Sie unter **Startoptionen** das **Zielgerät** in **Remotecomputer**.

    d. Geben Sie in **Remotecomputer** die System-IP-Adresse oder den Hostnamen der Xbox One Konsole ein. Informationen zum Ermitteln der IP-Adresse oder des Hostnamens finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).

    e. Wählen Sie in der Dropdownliste **Authentifizierungsmodus** den Eintrag **Universell (unverschlüsseltes Protokoll)** aus.

    ![C#-Eigenschaftenseiten für „Leere App“](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>Starten eines C++-Projekts

  ![C++-Projekt](images/development-environment-setup-3.png)

1. Wählen Sie im Dialogfeld **Neues universelles Windows-Projekt** Build 14393 oder höher im Dropdownmenü **Mindestens erforderliche Version** aus. Wählen Sie das aktuelle SDK im Dropdownmenü **Zielversion** aus. Wenn das Dialogfeld **Entwicklermodus** angezeigt wird, klicken Sie auf **OK**. Es wird eine neue leere App erstellt.

2. Konfigurieren Sie die Entwicklungsumgebung für das Remotedebuggen:

   a. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Eigenschaften**.

   b. Ändern Sie **Zu startender Debugger** auf der Registerkarte **Debuggen** in **Remotecomputer**.

   c. Geben Sie in **Computername** die System-IP-Adresse oder den Hostnamen der Xbox One-Konsole ein. Informationen zum Ermitteln der IP-Adresse oder des Hostnamens finden Sie unter [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md).

   d. Wählen Sie in der Dropdownliste **Authentifizierungstyp** den Eintrag **Universell (unverschlüsseltes Protokoll)** aus.

   e. Wählen Sie in der Dropdownliste **Plattform** die Option **x64** aus.

    ![C++-Eigenschaftenseiten für „Leere App“](images/development-environment-setup-4.png)

### <a name="pin-pair-your-device-with-visual-studio"></a>Koppeln des Geräts per PIN mit Visual Studio

1. Speichern Sie die Einstellungen, und stellen Sie sicher, dass sich die Xbox One-Konsole im Entwicklermodus befindet.

2. Drücken Sie im geöffneten Projekt in Visual Studio F5.

3. Wenn dies Ihre erste Bereitstellung ist, werden Sie in einem Dialogfeld in Visual Studio aufgefordert, Ihr Gerät per PIN zu koppeln.

    a. Um eine PIN abzurufen, öffnen Sie auf der Startseite der Xbox One Konsole **Dev Home**.

    b. Wählen Sie auf der Registerkarte **Startseite** unter **Schnelle Aktionen** die Option **Visual Studio-Pin anzeigen** aus.
  
    ![Dialogfeld „Mit Visual Studio koppeln“](images/development-environment-setup-5.png)

    c. Geben Sie im Dialogfeld **Mit Visual Studio koppeln** die PIN ein. Die folgende PIN ist nur ein Beispiel und stimmt nicht mit Ihrer PIN überein.

    ![Dialogfeld „Mit Visual Studio per PIN koppeln“](images/devhome_pin.png)

    d. Gegebenenfalls auftretende Bereitstellungsfehler werden im Fenster **Ausgabe** angezeigt.

Herzlichen Glückwunsch! Sie haben Ihre erste UWP-App auf Xbox erfolgreich erstellt und bereitgestellt!

## <a name="see-also"></a>Weitere Informationen:
- [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md)  
- [Downloads and tools for Windows 10](https://developer.microsoft.com/windows/downloads)  
- [Windows-Insider-Programm](https://insider.windows.com/)  
- [Introduction to Xbox One tools](introduction-to-xbox-tools.md) 
- [UWP auf Xbox One](index.md)