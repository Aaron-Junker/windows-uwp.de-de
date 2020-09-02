---
ms.assetid: D20C8E01-4E78-4115-A2E8-07BB3E67DDDC
description: In diesem Artikel wird beschrieben, wie Sie auf die Taschenlampe eines Geräts zugreifen und diese verwenden, sofern vorhanden. Die Taschenlampenfunktion wird unabhängig von der Kamera des Geräts und der Blitzfunktion der Kamera verwaltet.
title: Kameraunabhängige Taschenlampe
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 281cede94ee587cc86509a9f32ed34857a5ae620
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89362923"
---
# <a name="camera-independent-flashlight"></a>Kameraunabhängige Taschenlampe



In diesem Artikel wird beschrieben, wie Sie auf die Taschenlampe eines Geräts zugreifen und diese verwenden, sofern vorhanden. Die Taschenlampenfunktion wird unabhängig von der Kamera des Geräts und der Blitzfunktion der Kamera verwaltet. Neben dem Abrufen eines Verweises auf die Leuchte und dem Anpassen ihrer Einstellungen erfahren Sie in diesem Artikel auch, wie die Leuchtenressourcen korrekt freigegeben werden, wenn sie nicht verwendet wird. Außerdem wird beschrieben, wie Sie ermitteln können, wann sich die Verfügbarkeit der Leuchte ändert, falls sie von einer anderen App verwendet wird.

## <a name="get-the-devices-default-lamp"></a>Abrufen der Standardleuchte des Geräts

Um die Standardleuchte des Geräts abzurufen, rufen Sie [**Lamp.GetDefaultAsync**](/uwp/api/windows.devices.lights.lamp.getdefaultasync) auf. Die Leuchten-APIs sind im [**Windows.Devices.Lights**](/uwp/api/Windows.Devices.Lights)-Namespace zu finden. Achten Sie darauf, dass Sie eine „using“-Direktive für diesen Namespace hinzufügen, bevor Sie versuchen, auf diese APIs zuzugreifen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLightsNamespace":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetDeclareLamp":::


:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetGetDefaultLamp":::

Wenn das zurückgegebene Objekt **null** ist, wird die **Lamp**-API auf dem Gerät nicht unterstützt. Einige Geräte unterstützen möglicherweise die **Lamp**-API nicht, auch wenn auf dem Gerät tatsächlich eine Leuchte vorhanden ist.

## <a name="get-a-specific-lamp-using-the-lamp-selector-string"></a>Abrufen einer bestimmten Leuchte mithilfe der Leuchtenauswahlzeichenfolge

Einige Geräte verfügen möglicherweise über mehrere Leuchten. Um eine Liste der auf dem Gerät verfügbaren Leuchten zu erhalten, rufen Sie die Geräteauswahlzeichenfolge ab, indem Sie [**GetDeviceSelector**](/uwp/api/windows.devices.lights.lamp.getdeviceselector) aufrufen. Diese Auswahlzeichenfolge kann dann an [**DeviceInformation.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync) übergeben werden. Diese Methode wird verwendet, um viele verschiedene Arten von Geräten aufzulisten. Mit der Auswahlzeichenfolge kann die Methode angewiesen werden, dass nur Geräte mit Leuchten zurückgegeben werden sollen. Das von **FindAllAsync** zurückgegeben [**DeviceInformationCollection**](/uwp/api/Windows.Devices.Enumeration.DeviceInformationCollection)-Objekt ist eine Sammlung von [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-Objekten, die die auf dem Gerät verfügbaren Leuchten darstellen. Wählen Sie eines der Objekte in der Liste aus, und übergeben Sie dann die [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id)-Eigenschaft an [**Lamp.FromIdAsync**](/uwp/api/windows.devices.lights.lamp.fromidasync), um einen Verweis auf die angeforderte Leuchte abzurufen. In diesem Beispiel wird die **GetFirstOrDefault**-Erweiterungsmethode aus dem **System.Linq**-Namespace zum Auswählen des **DeviceInformation**-Objekts verwendet, wobei die [**EnclosureLocation.Panel**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel)-Eigenschaft den Wert **Back** aufweist. Dadurch wird eine Leuchte auf der Rückseite des Gerätegehäuses ausgewählt, sofern vorhanden.

Beachten Sie, dass sich die [**DeviceInformation**](/uwp/api/Windows.Devices.Enumeration.DeviceInformation)-APIs im [**Windows.Devices.Enumeration**](/uwp/api/Windows.Devices.Enumeration)-Namespace befinden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetEnumerationNamespace":::

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetGetLampWithSelectionString":::

## <a name="adjust-lamp-settings"></a>Anpassen der Leuchteneinstellungen

Nachdem Sie über eine Instanz der [**Lamp**](/uwp/api/Windows.Devices.Lights.Lamp)-Klasse verfügen, aktivieren Sie die Leuchte, indem Sie die [**IsEnabled**](/uwp/api/windows.devices.lights.lamp.isenabled)-Eigenschaft auf **true** setzen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsOn":::

Schalten Sie die Leuchte aus, indem Sie die [**IsEnabled**](/uwp/api/windows.devices.lights.lamp.isenabled)-Eigenschaft auf **false** setzen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsOff":::

Einige Geräte haben Leuchten, die Farbwerte unterstützen. Überprüfen Sie, ob eine Leuchte Farben unterstützt, indem Sie die [**IsColorSettable**](/uwp/api/windows.devices.lights.lamp.iscolorsettable)-Eigenschaft überprüfen. Wenn dieser Wert **true** ist, können Sie die Farbe der Leuchte mit der [**Color**](/uwp/api/windows.devices.lights.lamp.color)-Eigenschaft festlegen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetLampSettingsColor":::

## <a name="register-to-be-notified-if-the-lamp-availability-changes"></a>Registrierung für Benachrichtigung, wenn sich die Leuchtenverfügbarkeit ändert

Zugriff auf die Leuchte wird der letzten App gewährt, die den Zugriff angefordert hat. Wenn also eine andere App gestartet wird, die eine Leuchtenressource anfordert, die Ihre App gerade verwendet, kann Ihre App die Leuchte erst wieder steuern, wenn die andere App die Ressource freigegeben hat. Um eine Benachrichtigung zu erhalten, wenn sich die Verfügbarkeit der Leuchte ändert, registrieren Sie einen Handler für das [**Lamp.AvailabilityChanged**](/uwp/api/windows.devices.lights.lamp.availabilitychanged)-Ereignis.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetAvailabilityChanged":::

Überprüfen Sie im Handler für das Ereignis, ob es sich bei der Eigenschaftenänderung, die das Ereignis ausgelöst hat, um die [**LampAvailabilityChanged.IsAvailable**](/uwp/api/windows.devices.lights.lampavailabilitychangedeventargs.isavailable)-Eigenschaft handelte. In diesem Beispiel wird ein Umschalter zum An- und Ausschalten der Leuchte je nach Verfügbarkeit der Leuchte aktiviert oder deaktiviert.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetAvailabilityChangedHandler":::

## <a name="properly-dispose-of-the-lamp-resource-when-not-in-use"></a>Ordnungsgemäßes Freigeben der Leuchtenressourcen, wenn diese nicht verwendet wird

Wenn Sie die Leuchte nicht mehr verwenden, sollten Sie sie deaktivieren und [**Lamp.Close**](/uwp/api/windows.devices.lights.lamp.close) aufrufen, um die Ressource freizugeben und anderen Apps den Zugriff auf die Leuchte zu ermöglichen. Diese Eigenschaft ist der **Dispose**-Methode zugewiesen, wenn Sie C# verwenden. Wenn Sie sich für [**AvailabilityChanged**](/uwp/api/windows.devices.lights.lamp.availabilitychanged) registriert haben, sollten Sie die Registrierung des Handlers aufheben, wenn Sie die Leuchtenressource freigeben. Der richtige Stelle in Ihrem Code für das Freigeben der Leuchtenressource ist von Ihrer App abhängig. Um den Leuchtenzugriff auf eine einzige Seite zu beschränken, geben Sie die Ressource im [**OnNavigatingFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatingfrom)-Ereignis frei.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/Lamp/cs/MainPage.xaml.cs" id="SnippetDisposeLamp":::

## <a name="related-topics"></a>Verwandte Themen
- [Medienwiedergabe](media-playback.md)

 
