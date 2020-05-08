---
Description: Dank der Desktop Bridge können Windows-Desktop Anwendungen sekundäre Kacheln anheften.
title: Heften Sie sekundäre Kacheln aus der Desktop Anwendung
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, Desktop Bridge, sekundäre Kacheln, PIN, Pinning, Schnellstart, Codebeispiel, Beispiel, secondarytile, Desktop Anwendung, Win32, WinForms, WPF
ms.localizationpriority: medium
ms.openlocfilehash: 7ddcd96eadbb6d2edbc3a72fa58ff3cc8931a09b
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730365"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>Heften Sie sekundäre Kacheln aus der Desktop Anwendung


Dank der [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)können Windows-Desktop Anwendungen (z. b. Win32, Windows Forms und WPF) sekundäre Kacheln anheften.

![Screenshot der sekundären Kacheln](images/secondarytiles.png)

> [!IMPORTANT]
> **Erfordert das Fall Creators Update**: Sie müssen das SDK 16299 als Ziel festlegen und Build 16299 oder höher ausführen, um sekundäre Kacheln aus Desktop Bridge-apps anzuheften.

Das Hinzufügen einer sekundären Kachel aus Ihrer WPF-oder WinForms-Anwendung ähnelt einer reinen UWP-app. Der einzige Unterschied besteht darin, dass Sie das Hauptfenster handle (HWND) angeben müssen. Dies liegt daran, dass Windows beim anheften einer Kachel ein modales Dialogfeld anzeigt, das den Benutzer auffordert, zu bestätigen, ob die Kachel angeheftet werden soll. Wenn die Desktop Anwendung das secondarytile-Objekt nicht mit dem Besitzer Fenster konfiguriert, weiß Windows nicht, wo das Dialogfeld gezeichnet werden soll, und der Vorgang schlägt fehl.


## <a name="package-your-app-with-desktop-bridge"></a>Verpacken Ihrer APP mit Desktop Bridge

Wenn Sie Ihre APP nicht mit der Desktop Bridge gepackt haben, [müssen Sie dies zunächst tun](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root) , bevor Sie eine Windows-Runtime-APIs verwenden können.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>Aktivieren des Zugriffs auf die iinitializewithwindow-Schnittstelle

Wenn Ihre Anwendung in einer verwalteten Sprache wie z. b. c# oder Visual Basic geschrieben ist, deklarieren Sie die iinitializewithwindow-Schnittstelle mit dem [ComImport](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute) -und GUID-Attribut im Code der APP, wie im folgenden c#-Beispiel gezeigt. In diesem Beispiel wird davon ausgegangen, dass Ihre Codedatei über eine using-Anweisung für den System.Runtime.InteropServices-Namespace verfügt.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

Wenn Sie C++ verwenden, fügen Sie alternativ einen Verweis auf die Header Datei " **shobjidl. h** " in Ihrem Code hinzu. Diese Headerdatei enthält die Deklaration der *IInitializeWithWindow*-Schnittstelle.


## <a name="initialize-the-secondary-tile"></a>Initialisieren der sekundären Kachel

Initialisieren Sie ein neues sekundäres Kachel Objekt genau wie bei einer normalen UWP-app. Weitere Informationen zum Erstellen und fixieren sekundärer Kacheln finden Sie unter Anheften von [sekundären Kacheln](secondary-tiles-pinning.md).

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>Fenster Handle zuweisen

Dies ist der wichtigste Schritt für Desktop Anwendungen. Wandeln Sie das Objekt in ein [iinitializewithwindow](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow) -Objekt um. Aufrufen Sie dann die [iinitializewithwindow. Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize) -Methode, und übergeben Sie das Handle des Fensters, das Sie als Besitzer für das modale Dialogfeld festlegen möchten. Im folgenden c#-Beispiel wird gezeigt, wie das Handle des Hauptfensters Ihrer APP an die-Methode übergeben wird.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>Anheften der Kachel

Fordern Sie schließlich an, die Kachel wie eine normale UWP-App anzuheften.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>Senden von Kachel Benachrichtigungen

> [!IMPORTANT]
> **Erfordert die Version 17134,81 oder höher von April 2018**: Sie müssen Build 17134,81 oder höher ausführen, um Kachel-oder Badge-Benachrichtigungen an sekundäre Kacheln von Desktop Bridge-apps zu senden. Vor diesem. 81-Wartungsupdate würde eine Ausnahme 0x80070490- *Element nicht gefunden* auftreten, wenn Kachel-oder Badge-Benachrichtigungen an sekundäre Kacheln von Desktop Bridge-apps gesendet werden.

Das Senden von Kachel-oder Badge-Benachrichtigungen ist identisch mit UWP-apps. Weitere Informationen finden Sie unter [Senden einer lokalen Kachel Benachrichtigung](sending-a-local-tile-notification.md) .


## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Übersicht über sekundäre Kacheln](secondary-tiles.md)
* [Anheften von sekundären Kacheln (UWP)](secondary-tiles-pinning.md)
* [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop)
* [Desktop Bridge-Codebeispiele](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)