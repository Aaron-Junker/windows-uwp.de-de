---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Testen von Surface Hub-Apps mit Visual Studio
description: Der Visual Studio-Simulator bietet eine Umgebung für das Entwerfen, Entwickeln, Debuggen und Testen von UWP-Apps, einschließlich Apps für Surface Hub.
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 37c7f9edbaee008b6e16ef2ca202ff5cbcf39ca2
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317507"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Testen von Surface Hub-Apps mit Visual Studio
Der Visual Studio-Simulator bietet eine Umgebung, in der Sie Apps für die universale Windows-Plattform (UWP) entwerfen, entwickeln, debuggen und testen können, einschließlich Apps, die Sie für Microsoft Surface Hub entwickelt haben. Der Simulator verwendet nicht die gleiche Benutzeroberfläche wie Surface Hub, aber es ist hilfreich beim Testen, wie Ihre app sieht aus, und mit dem Surface Hub-Bildschirmgröße und-Auflösung verhält.

Weitere Informationen zu den Simulator-Tool im Allgemeinen finden Sie unter [Ausführen von UWP-apps im Simulator](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Hinzufügen von Surface Hub-Auflösungen zum Simulator
So fügen Sie Surface Hub-Auflösungen zum Simulator hinzu:

1. Erstellen Sie eine Konfiguration für die 55" Surface Hub durch den folgenden XML-Code in eine Datei namens speichern *HardwareConfigurations-SurfaceHub55.xml*.  

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

2. Erstellen Sie eine Konfiguration für die 84" Surface Hub durch den folgenden XML-Code in eine Datei namens speichern *HardwareConfigurations-SurfaceHub84.xml*.

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

3. Kopieren Sie die beiden XML-Dateien in *C:\Program Files (x86) \Common Shared\Windows Simulator\\&lt;Versionsnummer&gt;\HardwareConfigurations*.

   > [!NOTE]
   > Zum Speichern von Dateien in diesen Ordner sind Administratorrechte erforderlich.

4. Führen Sie Ihre App im Visual Studio-Simulator aus. Klicken Sie in der Palette auf die Schaltfläche **Change Resolution**, und wählen Sie in der Liste eine Surface Hub-Konfiguration aus.

    ![Auflösungen des Visual Studio-Simulators](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [Tablet-Modus aktivieren](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) , die Erfahrung von einem Surface Hub besser zu simulieren.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Bereitstellen von apps aus Visual Studio auf einem Surface Hub-Gerät
Manuelles Bereitstellen einer app auf einem Surface Hub ist ein einfacher Prozess.

### <a name="enable-developer-mode"></a>Aktivieren des Entwicklermodus
Standardmäßig wird die Surface Hub nur apps aus dem Microsoft Store installiert. Um Apps, die von einer anderen Quelle signiert wurden, zu installieren, müssen Sie den Entwicklermodus aktivieren.

> [!NOTE]
> Nach der Entwicklermodus aktiviert wurde, müssen Sie dem Surface Hub zurückgesetzt werden soll, wenn Sie es noch Mal deaktivieren möchten. Durch das Zurücksetzen des Geräts werden alle lokalen Benutzerdateien und Konfigurationen gelöscht. Anschließend wird Windows neu installiert.

1. Öffnen Sie im **Startmenü** des Surface Hub die Einstellungs-App.

   > [!NOTE]
   > Zugriff auf die app "Einstellungen" auf dem Surface Hub sind Administratorrechte erforderlich.

2. Navigieren Sie zu **Update und Sicherheit \> für Entwickler**.

3. Wählen Sie **Entwicklermodus** aus, und akzeptieren Sie die Warnung.

### <a name="deploy-your-app-from-visual-studio"></a>Bereitstellen Ihrer App aus Visual Studio
Weitere Informationen zum Bereitstellungsprozess finden Sie in der Regel unter [bereitstellen und Debuggen von UWP-apps](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > [!NOTE]
   > Dieses Feature erfordert Visual Studio 2015 Update 1 oder höher, aber es wird empfohlen, dass Sie die neueste aktuellste Version von Visual Studio verwenden. Eine Instanz von Visual Studio auf dem neuesten Stand, werden Sie alle neuesten Entwicklungen und Sicherheitsupdates gibe.

1. Zur Auswahl eines Ziels navigieren Sie zur Dropdownliste mit Debugzielen neben der Schaltfläche **Debugging starten** und wählen **Remotecomputer** aus.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Dropdownliste der Debugziele in Visual Studio](images/vs-debug-target.png)

2. Geben Sie die IP-Adresse des Surface Hub ein. Stellen Sie sicher, dass der Authentifizierungsmodus **Universell** ausgewählt ist.

   > [!TIP] 
   > Nachdem Sie den Entwicklermodus aktiviert haben, finden Sie auf der Willkommensseite des Surface Hub-IP-Adresse.

3. Wählen Sie **starten (F5) Debuggen** bereitstellen und Debuggen Ihrer app auf dem Surface Hub oder drücken STRG + F5, um nur Ihre app bereitzustellen.

   > [!TIP]
   > Wenn es sich bei dem Surface Hub auf dem Begrüßungsbildschirm angezeigt wird, müssen schließen Sie es, indem Sie eine beliebige Schaltfläche auswählen.
