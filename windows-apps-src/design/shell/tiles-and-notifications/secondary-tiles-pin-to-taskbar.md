---
description: Erfahren Sie, wie Sie sekundäre Kacheln an die Taskleiste anheften, um Ihren Benutzern einen schnellen Zugriff auf Inhalte in Ihrer APP zu bieten.
title: Anheften von sekundären Kacheln an die Taskleiste
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, an Taskleiste anheften, sekundäre Kachel, sekundäre Kacheln an Taskleiste anheften, Verknüpfung
ms.localizationpriority: medium
ms.openlocfilehash: 22f49fba21e4d3f997efee1a59123ab453e555ea
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220223"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Anheften von sekundären Kacheln an die Taskleiste

Wie beim anheften sekundärer Kacheln, können Sie sekundäre Kacheln an die Taskleiste anheften, sodass Ihre Benutzer schnell auf Inhalte in ihrer App zugreifen können.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **Eingeschränkte Zugriffs-API**: Diese API ist ein eingeschränktes Zugriffs Feature. Wenden Sie sich an, um diese API zu verwenden [taskbarsecondarytile@microsoft.com](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar) .

> **Erfordert das Update vom Oktober 2018**: Sie müssen das SDK 17763 als Ziel haben und Build 17763 oder höher ausführen, um an die Taskleiste anzuheften.


## <a name="guidance"></a>Anleitungen

Eine sekundäre Kachel bietet Benutzern eine konsistente und effiziente Möglichkeit, direkt auf bestimmte Bereiche innerhalb einer APP zuzugreifen. Obwohl ein Benutzer entscheidet, ob eine sekundäre Kachel an die Taskleiste angeheftet werden soll, werden die gepinbaren Bereiche in einer APP vom Entwickler bestimmt. Weitere Anleitungen finden Sie unter [Leitfaden für sekundäre Kacheln](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. ermitteln, ob die API vorhanden ist, und den eingeschränkten Zugriff aufheben

Ältere Geräte verfügen nicht über die Task leisten-anheften-APIs (wenn Sie auf ältere Versionen von Windows 10 abzielen). Daher sollten Sie auf diesen Geräten keine PIN-Schaltfläche anzeigen, die nicht fixiert werden kann.

Außerdem ist dieses Feature mit eingeschränktem Zugriff gesperrt. Wenden Sie sich an Microsoft, um Zugriff zu erhalten. API-Aufrufe an " **[taskbarmanager. requestpinsecondarytileasync](/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**", " **[taskbarmanager. issecondarytilepinnedasync](/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)**" und " **[taskbarmanager. tryunpinsecondarytileasync](/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** " schlagen mit einer Ausnahme vom Typ "Zugriff verweigert" fehl. Apps dürfen diese API ohne Berechtigungen nicht verwenden, und die API-Definition kann sich jederzeit ändern.

Verwenden Sie die [apiinformation. ismethodpresent](/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) -Methode, um zu bestimmen, ob die APIs vorhanden sind. Und verwenden Sie dann die **[limitedaccessfeatures](/uwp/api/windows.applicationmodel.limitedaccessfeatures)** -API, um die API zu entsperren.

```csharp
if (ApiInformation.IsMethodPresent("Windows.UI.Shell.TaskbarManager", "RequestPinSecondaryTileAsync"))
{
    // API present!
    // Unlock the pin to taskbar feature
    var result = LimitedAccessFeatures.TryUnlockFeature(
        "com.microsoft.windows.secondarytilemanagement",
        "<tokenFromMicrosoft>",
        "<publisher> has registered their use of com.microsoft.windows.secondarytilemanagement with Microsoft and agrees to the terms of use.");

    // If unlock succeeded
    if ((result.Status == LimitedAccessFeatureStatus.Available) ||
        (result.Status == LimitedAccessFeatureStatus.AvailableWithoutToken))
    {
        // Continue
    }
    else
    {
        // Don't show pin to taskbar button or call any of the below APIs
    }
}

else
{
    // Don't show pin to taskbar button or call any of the below APIs
}
```


## <a name="2-get-the-taskbarmanager-instance"></a>2. Get the taskbarmanager-Instanz

Windows-Apps können auf einer Vielzahl von Geräten ausgeführt werden. nicht alle unterstützen die Taskleiste. Derzeit unterstützen nur Desktop Geräte die Taskleiste. Außerdem kann es vorkommen, dass die Taskleiste vorhanden ist. Um zu überprüfen, ob die Taskleiste aktuell vorhanden ist, müssen Sie die **[taskbarmanager. GetDefault](/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** -Methode aufrufen und überprüfen, ob die zurückgegebene Instanz nicht NULL ist. Zeigen Sie keine PIN-Schaltfläche an, wenn die Taskleiste nicht vorhanden ist.

Es wird empfohlen, für die Dauer eines einzelnen Vorgangs auf der-Instanz festzulegen, z. b. mit dem Anhebens und anschließendes Abrufen einer neuen Instanz, wenn Sie das nächste Mal einen weiteren Vorgang ausführen müssen.

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();

if (taskbarManager != null)
{
    // Continue
}
else
{
    // Taskbar not present, don't display a pin button
}
```


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Überprüfen Sie, ob Ihre Kachel aktuell an die Taskleiste angeheftet ist.

Wenn die Kachel bereits angeheftet ist, sollten Sie stattdessen eine lösen-Schaltfläche anzeigen. Sie können die **[issecondarytilepinnedasync](/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** -Methode verwenden, um zu überprüfen, ob Ihre Kachel zurzeit fixiert ist (Benutzer können Sie jederzeit lösen). Bei dieser Methode übergeben Sie die **tileid** der Kachel, an die Sie wissen möchten, dass Sie fixiert ist.

```csharp
if (await taskbarManager.IsSecondaryTilePinnedAsync("myTileId"))
{
    // The tile is already pinned. Display the unpin button.
}

else 
{
    // The tile is not pinned. Display the pin button.
}
```


## <a name="4-check-whether-pinning-is-allowed"></a>4. Überprüfen Sie, ob angeheften zulässig sind

Das Anheften an die Taskleiste kann durch Gruppenrichtlinie deaktiviert werden. Mit der [taskbarmanager. ispinningallowed](/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) -Eigenschaft können Sie überprüfen, ob das Fixieren zulässig ist.

Wenn der Benutzer auf die Schaltfläche zum Anheften klickt, sollten Sie diese Eigenschaft überprüfen. wenn der Wert false ist, sollten Sie ein Meldungs Dialogfeld anzeigen, das den Benutzer informiert, dass das Anheften auf diesem Computer nicht zulässig

```csharp
TaskbarManager taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager == null)
{
    // Display message dialog informing user that taskbar is no longer present, and then hide the button
}

else if (taskbarManager.IsPinningAllowed == false)
{
    // Display message dialog informing user pinning is not allowed on this machine
}

else
{
    // Continue pinning
}
```


## <a name="5-construct-and-pin-your-tile"></a>5. erstellen und anheften der Kachel

Der Benutzer hat auf die Schaltfläche "Pin" geklickt, und Sie haben festgestellt, dass die APIs vorhanden sind, die Taskleiste vorhanden ist und angeheftet werden kann... Zeit bis zur PIN!

Erstellen Sie zuerst Ihre sekundäre Kachel wie beim anheulen an den Anfang. Weitere Informationen zu den Eigenschaften der sekundären Kachel finden Sie unter [anheften sekundärer Kacheln zum Starten](secondary-tiles-pinning.md). Beim anheften an die Taskleiste ist jedoch zusätzlich zu den zuvor erforderlichen Eigenschaften Square44x44Logo (das von der Taskleiste verwendete Logo) ebenfalls erforderlich. Andernfalls wird eine Ausnahme ausgelöst.

Übergeben Sie dann die Kachel an die **[requestpinsecondarytileasync](/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** -Methode. Da dieses unter eingeschränkten Zugriff ist, wird ein Bestätigungs Dialogfeld nicht angezeigt, und es wird kein UI-Thread benötigt. Doch in Zukunft, wenn dieser über eingeschränkten Zugriff geöffnet wird, erhalten Aufrufer, die keinen eingeschränkten Zugriff verwenden, ein Dialogfeld, das für die Verwendung des UI-Threads benötigt wird.

```csharp
// Initialize the tile (all properties below are required)
SecondaryTile tile = new SecondaryTile("myTileId");
tile.DisplayName = "PowerPoint 2016 (Remote)";
tile.Arguments = "app=powerpoint";
tile.VisualElements.Square44x44Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square44x44Logo.png");
tile.VisualElements.Square150x150Logo = new Uri("ms-appdata:///AppIcons/PowerPoint_Square150x150Logo.png");

// Pin it to the taskbar
bool isPinned = await taskbarManager.RequestPinSecondaryTileAsync(tile);
```

Diese Methode gibt einen booleschen Wert zurück, der angibt, ob Ihre Kachel nun an die Taskleiste angeheftet ist. Wenn die Kachel bereits angeheftet wurde, aktualisiert die Methode die vorhandene Kachel und gibt true zurück. Wenn das Anheften nicht zulässig ist oder die Taskleiste nicht unterstützt wird, gibt die Methode false zurück.


## <a name="enumerate-tiles"></a>Kacheln aufzählen

Wenn Sie alle Kacheln anzeigen möchten, die Sie erstellt haben und die immer noch an eine beliebige Stelle angeheftet sind (Start, Taskleiste oder beides), verwenden Sie **[findallasync](/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Anschließend können Sie überprüfen, ob diese Kacheln an die Taskleiste angeheftet sind und/oder gestartet werden. Wenn die Oberfläche nicht unterstützt wird, geben diese Methoden false zurück.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
var startScreenManager = StartScreenManager.GetDefault();

// Look through all tiles
foreach (SecondaryTile tile in await SecondaryTile.FindAllAsync())
{
    if (taskbarManager != null && await taskbarManager.IsSecondaryTilePinnedAsync(tile.TileId))
    {
        // Tile is pinned to the taskbar
    }

    if (startScreenManager != null && await startScreenManager.ContainsSecondaryTileAsync(tile.TileId))
    {
        // Tile is pinned to Start
    }
}
```


## <a name="update-a-tile"></a>Aktualisieren einer Kachel

Um eine bereits angeheftete Kachel zu aktualisieren, können Sie die Methode [**secondarytile. UpdateAsync**](/uwp/api/windows.ui.startscreen.secondarytile.updateasync) verwenden, wie unter [Aktualisieren einer sekundären Kachel](secondary-tiles-pinning.md#updating-a-secondary-tile)beschrieben.


## <a name="unpin-a-tile"></a>Lösen einer Kachel

Wenn die Kachel aktuell fixiert ist, sollte Ihre APP eine lösen-Schaltfläche bereitstellen. Um die Kachel zu lösen, nennen Sie einfach **[tryunpinsecondarytileasync](/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, und übergeben Sie dabei die **tileid** der sekundären Kachel, die Sie lösen möchten.

Diese Methode gibt einen booleschen Wert zurück, der angibt, ob Ihre Kachel nicht mehr an die Taskleiste angeheftet ist. Wenn Ihre Kachel nicht an erster Stelle fixiert wurde, wird auch true zurückgegeben. Wenn das lösen nicht zulässig war, wird false zurückgegeben.

Wenn die Kachel nur an die Taskleiste angeheftet wurde, wird die Kachel gelöscht, da Sie nicht mehr an beliebiger Stelle fixiert wird.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Löschen einer Kachel

Verwenden Sie die **[requestdeleteasync](/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** -Methode, wenn Sie eine Kachel von überall aus lösen möchten (Start, Taskleiste).

Dies ist für Fälle geeignet, in denen der vom Benutzer angeheftete Inhalt nicht mehr anwendbar ist. Wenn Sie mit Ihrer APP beispielsweise ein Notebook an den Start und die Taskleiste anheften und dann das Notebook löschen, sollten Sie einfach die dem Notebook zugeordnete Kachel löschen.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Nur vom Start lösen

Wenn Sie nur eine sekundäre Kachel aus dem Startmenü lösen möchten, während Sie auf der Taskleiste belassen wird, können Sie die Methode **[startscreenmanager. tryremovesecondarytileasync](/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** aufrufen. Auf diese Weise wird die Kachel gelöscht, wenn Sie nicht mehr an andere Oberflächen angeheftet ist.

Diese Methode gibt einen booleschen Wert zurück, der angibt, ob Ihre Kachel nicht mehr zum Starten fixiert ist. Wenn Ihre Kachel nicht an erster Stelle fixiert wurde, wird auch true zurückgegeben. Wenn das lösen nicht zulässig ist oder Start nicht unterstützt wird, wird false zurückgegeben.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Ressourcen

* [Taskbarmanager-Klasse](/uwp/api/windows.ui.shell.taskbarmanager)
* [Anheften von sekundären Kacheln an „Start“](secondary-tiles-pinning.md)
