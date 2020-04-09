---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Testen von Surface Hub-Apps mit Visual Studio
description: Der Visual Studio-Simulator bietet eine Umgebung für das Entwerfen, Entwickeln, Debuggen und Testen von UWP-Apps, einschließlich Apps für Surface Hub.
ms.date: 10/26/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 37c7f9edbaee008b6e16ef2ca202ff5cbcf39ca2
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317507"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Testen von Surface Hub-Apps mit Visual Studio
Der Visual Studio-Simulator bietet eine Umgebung, in der Sie Apps für die universale Windows-Plattform (UWP) entwerfen, entwickeln, debuggen und testen können, einschließlich Apps, die Sie für Microsoft Surface Hub entwickelt haben. Der Simulator verwendet nicht dieselbe Benutzeroberfläche wie ein Surface Hub, ist jedoch hilfreich, um das Erscheinungsbild und Verhalten deiner App bei der Bildschirmgröße und -auflösung von Surface Hubs zu testen.

Weitere allgemeine Informationen zum Simulatortool findest du unter [Ausführen von UWP-Apps im Simulator](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Hinzufügen von Surface Hub-Auflösungen zum Simulator
So fügen Sie Surface Hub-Auflösungen zum Simulator hinzu:

1. Erstelle eine Konfiguration für den Surface Hub mit 55 Zoll, indem du den folgenden XML-Code in der Datei *HardwareConfigurations-SurfaceHub55.xml* speicherst.  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. Erstelle eine Konfiguration für den Surface Hub mit 84 Zoll, indem du den folgenden XML-Code in der Datei *HardwareConfigurations-SurfaceHub84.xml* speicherst.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. Kopiere die beiden XML-Dateien in *C:\Programme (x86)\Gemeinsame Dateien\Microsoft Shared\Windows Simulator\\&lt;Versionsnummer&gt;\HardwareConfigurations*.

   > [!NOTE]
   > Zum Speichern der Dateien in diesem Ordner sind Administratorrechte erforderlich.

4. Führen Sie Ihre App im Visual Studio-Simulator aus. Klicken Sie in der Palette auf die Schaltfläche **Change Resolution**, und wählen Sie in der Liste eine Surface Hub-Konfiguration aus.

    ![Auflösungen des Visual Studio-Simulators](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [Aktiviere den Tablet-Modus](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet), um das Verhalten auf einem Surface Hub besser zu simulieren.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Bereitstellen von Apps aus Visual Studio auf einem Surface Hub-Gerät
Das manuelle Bereitstellen einer App auf einem Surface Hub ist ein einfacher Vorgang.

### <a name="enable-developer-mode"></a>Aktivieren des Entwicklermodus
Standardmäßig installiert Surface Hub nur Apps aus dem Microsoft Store. Um Apps, die von einer anderen Quelle signiert wurden, zu installieren, müssen Sie den Entwicklermodus aktivieren.

> [!NOTE]
> Nachdem der Entwicklermodus aktiviert wurde, musst du den Surface Hub zurücksetzen, um ihn wieder deaktivieren zu können. Durch das Zurücksetzen des Geräts werden alle lokalen Benutzerdateien und Konfigurationen gelöscht. Anschließend wird Windows neu installiert.

1. Öffnen Sie im **Startmenü** des Surface Hub die Einstellungs-App.

   > [!NOTE]
   > Für den Zugriff auf die Einstellungs-App auf dem Surface Hub sind Administratorrechte erforderlich.

2. Navigiere zu **Update und Sicherheit \> Für Entwickler**.

3. Wählen Sie **Entwicklermodus** aus, und akzeptieren Sie die Warnung.

### <a name="deploy-your-app-from-visual-studio"></a>Bereitstellen Ihrer App aus Visual Studio
Weitere allgemeine Informationen zum Bereitstellungsvorgang findest du unter [Bereitstellen und Debuggen von UWP-Apps](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > [!NOTE]
   > Für dieses Feature ist Visual Studio 2015 Update 1 oder höher erforderlich, es wird jedoch empfohlen, die neueste Version von Visual Studio zu verwenden. Mit einer aktuellen Visual Studio-Instanz erhältst du alle aktuellen Entwicklungs- und Sicherheitsupdates.

1. Zur Auswahl eines Ziels navigieren Sie zur Dropdownliste mit Debugzielen neben der Schaltfläche **Debugging starten** und wählen **Remotecomputer** aus.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Dropdownliste der Debugziele in Visual Studio](images/vs-debug-target.png)

2. Geben Sie die IP-Adresse des Surface Hub ein. Stellen Sie sicher, dass der Authentifizierungsmodus **Universell** ausgewählt ist.

   > [!TIP] 
   > Nachdem du den Entwicklermodus aktiviert hast, wird die IP-Adresse des Surface Hub auf dem Begrüßungsbildschirm angezeigt.

3. Wähle **Debugging starten (F5)** aus, um die Bereitstellung und das Debugging deiner App auf dem Surface Hub auszuführen, oder drücke STRG+F5, um die App nur bereitzustellen.

   > [!TIP]
   > Sollte auf dem Surface Hub der Begrüßungsbildschirm angezeigt werden, kannst du ihn durch Auswählen einer beliebigen Schaltfläche schließen.
