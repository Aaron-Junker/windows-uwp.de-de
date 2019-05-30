---
Description: Windows-Desktopanwendungen können sekundäre Kacheln dank der Desktop-Brücke anheften!
title: Sekundäre Kacheln von der Desktopanwendung anheften
label: Pin secondary tiles from desktop application
template: detail.hbs
ms.date: 05/25/2017
ms.topic: article
keywords: Windows 10, Desktop-Brücke, sekundäre Kacheln, anheften, Anheften, Schnellstart, Codebeispiel, Beispiel, Sekundärkachel, Desktopanwendung, Win32, Winforms, WPF
ms.localizationpriority: medium
ms.openlocfilehash: 7ca6471122ee1870a94ef0834a5eed8f83a4d4a7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362621"
---
# <a name="pin-secondary-tiles-from-desktop-application"></a>Sekundäre Kacheln von der Desktopanwendung anheften


Dank der [Desktop-Brück](https://developer.microsoft.com/windows/bridges/desktop) können Windows-Desktopanwendung (wie Win32, Windows Forms und WPF) sekundäre Kacheln anheften.

![Screenshot von sekundären Kacheln](images/secondarytiles.png)

> [!IMPORTANT]
> **Erfordert Fall Creators Update-** : Sie müssen SDK 16299 ausgerichtet sein und ausgeführt werden Build 16299 oder höher, um sekundäre Kacheln Anheften von Desktop-Brücke apps.

Das Hinzufügen einer sekundären Kachel aus Ihrer WPF- oder WinForms-Anwendung ist einer reinen UWP-App sehr ähnlich. Der einzige Unterschied besteht darin, dass Sie das Hauptfenster-Handle (HWND) angeben müssen. Der Grund dafür ist, dass Windows beim Anheften einer Kachel ein modales Dialogfeld anzeigt, das den Benutzer auffordert zu bestätigen, dass er die Kachel anheften möchte. Wenn die Desktop-Anwendung das SecondaryTile-Objekt nicht mit dem Besitzerfenster konfigurieren, kann Windows nicht feststellen, wo das Dialogfeld gezeichnet werden soll und der Vorgang schlägt fehl.


## <a name="package-your-app-with-desktop-bridge"></a>Packen Sie Ihre App mit der Desktop-Brücke

Wenn Sie Ihre App nicht mit der Desktop-Brücke gepackt haben [müssen Sie dies zuerst durchführen](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-root), um alle UWP-APIs verwenden zu können.


## <a name="enable-access-to-iinitializewithwindow-interface"></a>Aktivieren des Zugriffs auf die IInitializeWithWindow-Schnittstelle

Wenn die Anwendung in einer verwalteten Sprache wie z. B. C# oder Visual Basic geschrieben wurde, deklarieren Sie die IInitializeWithWindow-Schnittstelle im App-Code wie im folgenden C#-Beispiel mit dem [ComImport](https://docs.microsoft.com/dotnet/api/system.runtime.interopservices.comimportattribute?redirectedfrom=MSDN) und dem Guid-Attribut. In diesem Beispiel wird davon ausgegangen, dass Ihre Codedatei über eine using-Anweisung für den System.Runtime.InteropServices-Namespace verfügt.

```csharp
[ComImport]
[Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
[InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
public interface IInitializeWithWindow
{
    void Initialize(IntPtr hwnd);
}
```

Wenn Sie alternativ C++ verwenden, fügen Sie im Code einen Verweis auf die Headerdatei **shobjidl.h** hinzu. Diese Headerdatei enthält die Deklaration der *IInitializeWithWindow*-Schnittstelle.


## <a name="initialize-the-secondary-tile"></a>Initialisieren Sie die sekundäre Kachel

Initialisieren Sie ein neues sekundäres Kachelobjekt genau wie in einer normalen UWP-App. Weitere Informationen zum Erstellen und Anheften von Sekundärkacheln finden Sie unter [Sekundäre Kacheln anheften](secondary-tiles-pinning.md).

```csharp
// Initialize the tile with required arguments
SecondaryTile tile = new SecondaryTile(
    "myTileId5391",
    "Display name",
    "myActivationArgs",
    new Uri("ms-appx:///Assets/Square150x150Logo.png"),
    TileSize.Default);
```


## <a name="assign-the-window-handle"></a>Zuweisen des Fensterhandles

Dies ist der wichtigste Schritt für Desktopanwendung. Wandeln Sie das Objekt in ein [IInitializeWithWindow](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iinitializewithwindow)-Objekt um. Rufen Sie dann die [IInitializeWithWindow.Initialize](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iinitializewithwindow-initialize)-Methode auf, und übergeben Sie das Handle des Fensters, das der Besitzer aller modalen Dialoge sein soll. Im folgenden C#-Beispiel wird gezeigt, wie das Handle des Hauptfensters der App an die Methode übergeben wird.

```csharp
// Assign the window handle
IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)tile;
initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
```


## <a name="pin-the-tile"></a>Anheften der Kachel

Fordern Sie schließlich das Anheften der Kachel wie bei einer normalen UWP-App an.

```csharp
// Pin the tile
bool isPinned = await tile.RequestCreateAsync();

// TODO: Update UI to reflect whether user can now either unpin or pin
```


## <a name="send-tile-notifications"></a>Senden von Kachelbenachrichtigungen

> [!IMPORTANT]
> **April 2018 17134.81 oder höher erfordert**: Sie müssen Build 17134.81 oder höher zum Senden von Kacheln und infoanzeiger Benachrichtigungen an sekundäre Kacheln von Desktop-Brücke apps ausgeführt werden. Vor diesem x.81-Wartungsupdate würde beim Senden von Kachel- oder Signalbenachrichtigungen für sekundäre Kacheln von Desktop-Brücke-Apps die Ausnahme 0x80070490 *Element nicht gefunden* auftreten.

Das Senden von Kachel- oder Signalbenachrichtigungen ist identisch wie bei UWP-Apps. Weitere Informationen finden Sie unter [Senden einer lokalen Kachelbenachrichtigung](sending-a-local-tile-notification.md).


## <a name="resources"></a>Ressourcen

* [Vollständige Codebeispiel](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Übersicht über die sekundäre Kacheln](secondary-tiles.md)
* [PIN sekundäre Kacheln (UWP)](secondary-tiles-pinning.md)
* [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop)
* [Codebeispiele für die Desktop-Brücke](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)