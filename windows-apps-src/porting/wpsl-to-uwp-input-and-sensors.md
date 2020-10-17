---
description: Erfahren Sie, wie Sie Windows Phone Silverlight für ein e/a-, Geräte-und App-Modell auf UWP portieren.
title: Portieren von Windows Phone Silverlight auf UWP für e/a-, Geräte-und App-Modell
ms.assetid: bf9f2c03-12c1-49e4-934b-e3fa98919c53
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 85f0bfaa42c6357a468f2c3babbd4a89925d2e4c
ms.sourcegitcommit: 0c4bbaf1c119a84002748cdcf02e1449835559c3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/16/2020
ms.locfileid: "92133013"
---
#  <a name="porting-windowsphone-silverlight-to-uwp-for-io-device-and-app-model"></a>Portieren von Windows Phone Silverlight zu UWP - E/A, Gerät und App-Modell


Im vorherigen Thema ging es um das [Portieren von XAML und UI](wpsl-to-uwp-porting-xaml-and-ui.md).

Code, der in das Gerät selbst integriert und auf dessen Sensoren abgestimmt ist, umfasst auch Eingaben vom und Ausgaben an den Benutzer. Auch die Datenverarbeitung kann einbezogen werden. Aber dieser Code wird in der Regel nicht als UI-Ebene oder Datenebene betrachtet. Dieser Code enthält die Integration in Vibrationscontroller, Beschleunigungsmesser, Gyroskop, Mikrofon und Lautsprecher (überschneiden sich mit Spracherkennung und Sprachsynthese), (geografischen) Standort und Eingabemodalitäten, z. B. Touch, Maus, Tastatur und Stift.

## <a name="application-lifecycle-process-lifetime-management"></a>App-Lebenszyklus (Prozesslebensdauer-Verwaltung)

Ihre Windows Phone Silverlight-App enthält Code zum Speichern und Wiederherstellen des App-Zustands und Anzeigemodus, um die Markierung als veraltet und die anschließende erneute Aktivierung zu unterstützen. Der App-Lebenszyklus von UWP-Apps (Universelle Windows-Plattform) weist starke Parallelen zu dem von Windows Phone Silverlight-Apps auf, da beide mit dem Ziel entworfen werden, die verfügbaren Ressourcen für die vom Benutzer zur Anzeige im Vordergrund ausgewählte App zu maximieren. Sie werden feststellen, dass Ihr Code sich dem neuen System recht problemlos anpasst.

**Hinweis**    Durch Drücken der Schaltfläche "Hardware **zurück** " wird automatisch eine Windows Phone Silverlight-App beendet. Eine UWP-App wird durch Drücken der Hardwaretaste **Zurück** auf einem Mobilgerät dagegen *nicht* automatisch beendet. Stattdessen wird sie erst angehalten und dann ggf. beendet. Diese Details sind für eine App, die entsprechend auf App-Lebenszyklusereignisse reagiert, jedoch transparent.

Ein so genanntes „Entprellfenster“ ist der Zeitraum von der Deaktivierung der App bis zum Auslösen des Anhalteereignisses durch das System. Für eine UWP-App gibt es kein Entprellfenster. Das Anhalteereignis wird ausgelöst, sobald eine App inaktiv wird.

Weitere Informationen finden Sie unter [App Lifecycle](../launch-resume/app-lifecycle.md).

## <a name="camera"></a>Kamera

Im Code der Windows Phone Silverlight-Kameraaufnahme wird die **Microsoft.Devices.Camera**-, **Microsoft.Devices.PhotoCamera**- oder **Microsoft.Phone.Tasks.CameraCaptureTask**-Klasse verwendet. Zum Portieren dieses Codes zur universellen Windows-Plattform (UWP) können Sie die [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture)-Klasse verwenden. Ein Codebeispiel finden Sie im Thema [**CapturePhotoToStorageFileAsync**](/uwp/api/windows.media.capture.mediacapture.capturephototostoragefileasync). Mit dieser Methode können Sie ein Foto in einer Speicherdatei aufnehmen. Dazu müssen die **microphone**- und **webcam**- [**Gerätefunktionen**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability) im App-Paketmanifest festgelegt werden.

Eine weitere Möglichkeit ist die [**CameraCaptureUI**](/uwp/api/Windows.Media.Capture.CameraCaptureUI)-Klasse, für die ebenfalls die **Mikrofon**- und **Webcam**- [**Funktionen**](/uwp/schemas/appxpackage/uapmanifestschema/element-devicecapability) erforderlich sind.

Foto-Apps werden für UWP-Apps nicht unterstützt.

## <a name="detecting-the-platform-your-app-is-running-on"></a>Erkennen der Plattform, auf der Ihre App ausgeführt wird

Die Herangehensweise an die Ausrichtung von Apps ändert sich mit Windows 10. Das neue konzeptionelle Modell besteht darin, dass eine App auf die Universelle Windows-Plattform (UWP) ausgerichtet ist und auf allen Windows-Geräten ausgeführt wird. Dann besteht die Möglichkeit, Funktionen hervorzuheben, die exklusiv für bestimmte Gerätefamilien angeboten werden. Bei Bedarf besteht auch die Möglichkeit, die App auf eine oder mehrere bestimmte Gerätefamilien zu beschränken. Weitere Informationen zu Gerätefamilien – und wie Sie entscheiden, auf welche Sie eine App ausrichten sollten – finden Sie unter [Anleitung für UWP-Apps](../get-started/universal-application-platform-guide.md).

**Hinweis**    Es wird empfohlen, die Betriebssystem-oder Gerätefamilie nicht zu verwenden, um das vorhanden sein von Features zu erkennen. Das Identifizieren des aktuellen Betriebssystems oder der Gerätefamilie ist in der Regel nicht die beste Möglichkeit, um festzustellen, ob ein bestimmtes Feature für das Betriebssystem oder die Gerätefamilie vorhanden ist. Anstatt das Betriebssystem oder die Gerätefamilie (und Versionsnummer) zu ermitteln, sollten Sie das Vorhandensein des Features selbst überprüfen (siehe [Bedingte Kompilierung und adaptiver Code](wpsl-to-uwp-porting-to-a-uwp-project.md)). Wenn ein bestimmtes Betriebssystem oder eine bestimmte Gerätefamilie erforderlich ist, sollten Sie darauf achten, es bzw. sie als unterstützte Mindestversion zu verwenden, anstatt den Test nur für diese Version zu entwerfen.

Zum Anpassen der Benutzeroberfläche Ihrer App für verschiedene Geräte gibt es mehrere empfohlene Möglichkeiten. Verwenden Sie weiterhin Elemente mit automatischer Größenanpassung und dynamische Layoutbereiche. Verwenden Sie in Ihrem XAML-Markup weiterhin Größen in der Einheit „effektive Pixel“ (früher „Anzeigepixel“), damit sich die Benutzeroberfläche an verschiedene Auflösungen und Skalierungsfaktoren anpasst (siehe [Anzeigepixel/Effektive Pixel, Abstand zum Bildschirm und Skalierungsfaktoren](wpsl-to-uwp-porting-xaml-and-ui.md)). Verwenden Sie außerdem die adaptiven Auslöser und Setter des Visual State-Managers zum Anpassen der Benutzeroberfläche an die Fenstergröße (siehe [Anleitung für UWP-Apps](../get-started/universal-application-platform-guide.md)).

Bei einem Szenario, in dem das Erkennen der Gerätefamilie unvermeidbar ist, können Sie so vorgehen. In diesem Beispiel verwenden wir die [**AnalyticsVersionInfo**](/uwp/api/Windows.System.Profile.AnalyticsVersionInfo)-Klasse, um zu einer für die jeweilige Mobilgerätefamilie angepassten Seite zu navigieren – falls diese vorhanden ist – und wir stellen sicher, dass andernfalls eine Umleitung auf eine Standardseite erfolgt.

```csharp
   if (Windows.System.Profile.AnalyticsInfo.VersionInfo.DeviceFamily == "Windows.Mobile")
        rootFrame.Navigate(typeof(MainPageMobile), e.Arguments);
    else
        rootFrame.Navigate(typeof(MainPage), e.Arguments);
```

Ihre App kann auch anhand der aktiven Ressourcenauswahlfaktoren die Gerätefamilie ermitteln, auf der sie ausgeführt wird. Im folgenden Beispiel wird gezeigt, wie dies imperativ durchgeführt wird, und im Thema [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.qualifiervalues) wird der gängigere Anwendungsfall für die Klasse beim Laden der gerätefamilienspezifischen Ressourcen basierend auf dem Gerätefamilienfaktor beschrieben.

```csharp
var qualifiers = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
string deviceFamilyName;
bool isDeviceFamilyNameKnown = qualifiers.TryGetValue("DeviceFamily", out deviceFamilyName);
```

Siehe auch [Bedingte Kompilierung und adaptiver Code](wpsl-to-uwp-porting-to-a-uwp-project.md).

## <a name="device-status"></a>Gerätestatus

Eine Windows Phone Silverlight-App kann die **Microsoft.Phone.Info.DeviceStatus**-Klasse verwenden, um Infos über das Gerät, auf dem die App ausgeführt wird, zu erhalten. Es gibt kein direktes UWP-Äquivalent für den **Microsoft.Phone.Info**-Namespace. Sie finden hier aber einige Eigenschaften und Ereignisse, die Sie in einer UWP-App verwenden können, anstatt die Member der **DeviceStatus-Klasse** aufzurufen.

| Windows Phone Silverlight                                                               | UWP                                                                                                                                                                                                                                                                                                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **ApplicationCurrentMemoryUsage**-Eigenschaft und **ApplicationCurrentMemoryUsageLimit**-Eigenschaft | [							Eigenschaften **MemoryManager.AppMemoryUsage**](/uwp/api/windows.system.memorymanager.appmemoryusage) und [**AppMemoryUsageLimit**](/uwp/api/windows.system.memorymanager.appmemoryusagelimit)                                                                                                                                    |
| **ApplicationPeakMemoryUsage**-Eigenschaft                                                 | Verwenden Sie das Tool zur Erstellung von Arbeitsspeicherprofilen in Visual Studio. Weitere Informationen finden Sie unter [Analysieren der Speicherauslastung](/visualstudio/welcome-to-visual-studio-2015?view=vs-2015).                                                                                                                                                                          |
| **DeviceFirmwareVersion**-Eigenschaft                                                      | [**EasClientDeviceInformation.SystemFirmwareVersion**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemfirmwareversion)-Eigenschaft (nur Familie der Desktopgeräte)                                                                                                                                                                             |
| **DeviceHardwareVersion**-Eigenschaft                                                      | [**EasClientDeviceInformation.SystemHardwareVersion**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemhardwareversion)-Eigenschaft (nur Familie der Desktopgeräte)                                                                                                                                                                             |
| **DeviceManufacturer**-Eigenschaft                                                         | [**EasClientDeviceInformation.SystemManufacturer**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemmanufacturer)-Eigenschaft (nur Familie der Desktopgeräte)                                                                                                                                                                                |
| **DeviceName**-Eigenschaft                                                                 | [**EasClientDeviceInformation.SystemProductName**](/uwp/api/windows.security.exchangeactivesyncprovisioning.easclientdeviceinformation.systemproductname)-Eigenschaft (nur Familie der Desktopgeräte)                                                                                                                                                                                 |
| **DeviceTotalMemory**-Eigenschaft                                                          | Keine Entsprechung                                                                                                                                                                                                                                                                                                                      |
| **IsKeyboardDeployed**-Eigenschaft                                                         | Keine Entsprechung Diese Eigenschaft enthält Infos zu Hardwaretastaturen für Mobilgeräte, die nur selten verwendet werden.                                                                                                                                                                                                        |
| **IsKeyboardPresent**-Eigenschaft                                                          | Keine Entsprechung Diese Eigenschaft enthält Infos zu Hardwaretastaturen für Mobilgeräte, die nur selten verwendet werden.                                                                                                                                                                                                        |
| **KeyboardDeployedChanged**-Ereignis                                                       | Keine Entsprechung Diese Eigenschaft enthält Infos zu Hardwaretastaturen für Mobilgeräte, die nur selten verwendet werden.                                                                                                                                                                                                        |
| **PowerSource**-Eigenschaft                                                                | Keine Entsprechung                                                                                                                                                                                                                                                                                                                      |
| **PowerSourceChanged**-Ereignis                                                            | Behandeln Sie das [**RemainingChargePercentChanged**](/uwp/api/windows.phone.devices.power.battery.remainingchargepercentchanged)-Ereignis (nur Familie der Mobilgeräte). Das Ereignis wird ausgelöst, wenn der Wert der [**RemainingChargePercent**](/uwp/api/windows.phone.devices.power.battery.remainingchargepercent)-Eigenschaft (nur Familie der Mobilgeräte) um 1 % verkleinert wird. |

## <a name="location"></a>Standort

Wenn eine App, für die im App-Paketmanifest die Positionsfunktion deklariert wird, unter Windows 10 ausgeführt wird, fordert das System die Zustimmung des Endbenutzers an. Falls in Ihrer App eine eigene benutzerdefinierte Aufforderung zur Zustimmung oder eine Schaltfläche zum Aktivieren/Deaktivieren angezeigt wird, sollten Sie sie entfernen, damit Endbenutzer nur eine Aufforderung erhalten.

## <a name="orientation"></a>Orientation

Die Entsprechung der UWP-App für die Eigenschaften **PhoneApplicationPage.SupportedOrientations** und **Orientation** ist das [**uap:InitialRotationPreference**](/uwp/schemas/appxpackage/uapmanifestschema/element-uap-splashscreen)-Element im App-Paketmanifest. Wählen Sie die Registerkarte **Anwendung** aus, falls sie nicht bereits ausgewählt wurde, und aktivieren Sie unter **Unterstützte Drehungen** ein oder mehrere Kontrollkästchen, um Ihre Präferenzen anzugeben.

Sie können die Benutzeroberfläche Ihrer UWP-App aber beliebig ausrichten, damit sie unabhängig von Geräteausrichtung und Bildschirmgröße gut aussieht. Mehr dazu finden Sie im folgenden Thema [Portieren für Formfaktor und Benutzerfreundlichkeit](wpsl-to-uwp-form-factors-and-ux.md).

Das nächste Thema lautet [Portieren von Unternehmen und Datenebenen](wpsl-to-uwp-business-and-data.md).