---
Description: Erfahren Sie, wie sekundäre Kacheln an Taskleiste anheften.
title: Sekundäre Kacheln an Taskleiste anheften
label: Pin secondary tiles to taskbar
template: detail.hbs
ms.date: 11/28/2018
ms.topic: article
keywords: Windows 10, Uwp, an die Taskleiste, sekundären Kachel anheften anheften, sekundäre Kacheln auf der Taskleiste, Kontextmenü
ms.localizationpriority: medium
ms.openlocfilehash: 7ad322fe371b0e1f3605ffb4c29108a15bb28e0c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57591975"
---
# <a name="pin-secondary-tiles-to-taskbar"></a>Sekundäre Kacheln an Taskleiste anheften

Genau wie das Anheften von sekundären Kacheln starten, können Sie sekundäre Kacheln an die Taskleiste anheften, erteilen Ihren Benutzern schnellen Zugriff auf Inhalte in Ihrer app.

<img alt="Taskbar pinning" src="../images/taskbar/pin-secondary-ui.png" width="972"/>

> [!IMPORTANT]
> **Nur mit beschränktem Zugriff API**: Diese API ist ein Feature des eingeschränkten Zugriff. Um diese API verwenden zu können, wenden Sie sich an [ taskbarsecondarytile@microsoft.com ](mailto:taskbarsecondarytile@microsoft.com?Subject=Limited%20Access%20permission%20to%20use%20secondary%20tiles%20on%20taskbar).

> **Erfordert Update für Oktober 2018**: Sie müssen SDK 17763 ausgerichtet sein und ausgeführt werden Build 17763 oder höher auf an Taskleiste anheften.


## <a name="guidance"></a>Leitlinien

Eine sekundäre Kachel bietet eine konsistente und effiziente Methode für die Benutzer auf bestimmte Bereiche innerhalb einer app direkt zugreifen. Auch wenn ein Benutzer, ob "eine sekundäre Kachel an die Taskleiste anheften" auswählt, werden die fixierbare Bereiche in einer app durch Entwickler bestimmt. Weitere Informationen finden Sie unter [sekundäre Kachel Anleitungen](secondary-tiles-guidance.md).


## <a name="1-determine-if-api-exists-and-unlock-limited-access"></a>1. Bestimmen, ob die API vorhanden ist, und Entsperren von eingeschränktem Zugriff

Ältere Geräte müssen nicht die Taskleiste anheften von APIs (Wenn Sie ältere Versionen von Windows 10 entwickeln). Sie sollte nicht aus diesem Grund eine Schaltfläche "anheften" auf diesen Geräten anzeigen, die der Verknüpfung mit dem kann nicht.

Darüber hinaus ist dieses Feature unter eingeschränktem Zugriff gesperrt werden. Um Zugriff zu erhalten, wenden Sie sich an Microsoft. API-Aufrufe für  **[TaskbarManager.RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync#Windows_UI_Shell_TaskbarManager_RequestPinSecondaryTileAsync_Windows_UI_StartScreen_SecondaryTile_)**,  **[TaskbarManager.IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)**, und **[TaskbarManager.TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)** mit einem "Zugriff verweigert"-Ausnahme fehl. Apps sind zur Verwendung dieser API ohne Berechtigung nicht zulässig, und die API-Definition kann sich jederzeit ändern.

Verwenden der [ApiInformation.IsMethodPresent](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.ismethodpresent#Windows_Foundation_Metadata_ApiInformation_IsMethodPresent_System_String_System_String_) Methode, um zu bestimmen, ob die APIs vorhanden sind. Und dann die **[LimitedAccessFeatures](https://docs.microsoft.com/uwp/api/windows.applicationmodel.limitedaccessfeatures)** -API zum Testen die API zu entsperren.

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


## <a name="2-get-the-taskbarmanager-instance"></a>2. Rufen Sie die TaskbarManager-Instanz

UWP-Apps können auf einer Vielzahl von Geräten ausgeführt werden. Nicht alle Geräte unterstützen die Taskleiste. Derzeit unterstützen nur Desktop-Geräte die Taskleiste. Darüber hinaus möglicherweise Vorhandensein der Taskleiste stammen, und wechseln. Um zu überprüfen, ob die Taskleiste zurzeit vorhanden ist, rufen Sie die **[TaskbarManager.GetDefault](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.getdefault)** -Methode und die Überprüfung, die die zurückgegebene Instanz ist nicht null. Eine Schaltfläche "anheften" nicht angezeigt werden, wenn die Taskleiste nicht vorhanden ist.

Es wird empfohlen, enthalten in der Instanz für die Dauer eines einzelnen Vorgangs, wie das anheften, und klicken Sie dann eine neue Instanz das nächste Mal, das Sie einen anderen Vorgang müssen abrufen.

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


## <a name="3-check-whether-your-tile-is-currently-pinned-to-the-taskbar"></a>3. Überprüfen Sie, ob die Kachel, gegenwärtig an die Taskleiste angeheftet ist

Falls die Kachel bereits angeheftet ist, sollten Sie stattdessen eine Schaltfläche "Unpin" anzeigen. Sie können der **[IsSecondaryTilePinnedAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.issecondarytilepinnedasync)** -Methode überprüft, ob die derzeit die Kachel angeheftet ist (Benutzer können Sie lösen zu einem beliebigen Zeitpunkt). Bei dieser Methode übergeben Sie die **TileId** der Kachel, die Sie wissen möchten angeheftet ist.

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


## <a name="4-check-whether-pinning-is-allowed"></a>4. Überprüfen Sie, ob das Anheften von Zertifikaten zulässig ist

An die Taskleiste anheften, kann durch eine Gruppenrichtlinie deaktiviert werden. Die [TaskbarManager.IsPinningAllowed](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.ispinningallowed) Eigenschaft können Sie überprüfen, ob das Anheften von Zertifikaten zulässig ist.

Klickt der Benutzer auf die Schaltfläche "anheften", sollten Sie überprüfen, dass diese Eigenschaft, und wenn sie falsch ist, sollten Sie anzeigen, dass ein Meldungsdialogfeld informiert den Benutzer, den Anheften von Zertifikaten auf diesem Computer nicht zulässig ist.

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


## <a name="5-construct-and-pin-your-tile"></a>5. Erstellen und die Kachel anheften

Der Benutzer die Pin-Schaltfläche geklickt hat, und Sie haben festgestellt, dass die APIs vorhanden sind, Taskleiste vorhanden ist und Anheften von Zertifikaten zulässig ist... Zeit für die Pin!

Erstellen Sie zunächst die sekundäre Kachel genau so wie bei an Startmenü anheften. Sie können erfahren Sie mehr über die sekundäre kacheleigenschaften lesen [sekundäre Kacheln an Start anheften](secondary-tiles-pinning.md). Allerdings ist beim anheften an Taskleiste, zusätzlich zu den zuvor erforderlichen Eigenschaften können Square44x44Logo (Dies ist das Logo, die von der Taskleiste verwendet) ebenfalls erforderlich. Andernfalls wird eine Ausnahme ausgelöst werden.

Übergeben Sie dann auf die Kachel, um die **[RequestPinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.requestpinsecondarytileasync)** Methode. Da es unter eingeschränktem Zugriff handelt, wird dies kein Bestätigungsdialogfeld angezeigt und einen UI-Thread ist nicht erforderlich. Aber in der Zukunft bei Dies ist geöffnet einrichten über eingeschränktem Zugriff, Aufrufer erhalten ein Dialogfeld wird nicht mit eingeschränktem Zugriff und die erforderlich sein, die im UI-Thread zu verwenden.

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

Diese Methode gibt einen booleschen Wert, der angibt, ob die Kachel jetzt an die Taskleiste angeheftet wurde. Wenn die Kachel bereits angeheftet wurde, wird die Methode aktualisiert die vorhandene Kachel und gibt true zurück. Anheften von war nicht zulässig oder Taskleiste wird nicht unterstützt, gibt die Methode "false".


## <a name="enumerate-tiles"></a>Auflisten von Kacheln

Zum Anzeigen aller Kacheln, die Sie erstellt und sind immer noch angeheftet an einer beliebigen Stelle (Start, Taskleiste, oder beides) verwenden  **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.findallasync)**. Anschließend können Sie überprüfen, ob die an die Taskleiste und/oder starten diese Kacheln angeheftet werden. Wenn die Oberfläche nicht unterstützt wird, geben diese Methoden "false" zurück.

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

Um ein bereits angeheftete Kachel zu aktualisieren, können Sie die [ **SecondaryTile.UpdateAsync** ](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.updateasync) Methode wie in beschrieben [aktualisieren eine sekundäre Kachel](secondary-tiles-pinning.md#updating-a-secondary-tile).


## <a name="unpin-a-tile"></a>Lösen Sie eine Kachel

Ihre app sollte eine Schaltfläche "Unpin" bereitstellen, wenn die Kachel derzeit verknüpft ist. Um das Loslösen der Kachel, rufen Sie ganz einfach  **[TryUnpinSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.tryunpinsecondarytileasync)**, und übergeben Sie die **TileId** der sekundären Kachel soll nicht fixiert.

Diese Methode gibt einen booleschen Wert, der angibt, ob die Kachel nicht mehr an die Taskleiste angeheftet wurde. Wenn die Kachel wurde nicht im vornherein angeheftet hat, gibt dies auch "true" zurück. Wenn durch das lösen zulässig war nicht, gibt dies "false" zurück.

Wenn die Kachel nur an Taskleiste angeheftet wurde, dass die Kachel gelöscht wird, da er nicht mehr an einer beliebigen Stelle angeheftet ist.

```csharp
var taskbarManager = TaskbarManager.GetDefault();
if (taskbarManager != null)
{
    bool isUnpinned = await taskbarManager.TryUnpinSecondaryTileAsync("myTileId");
}
```


## <a name="delete-a-tile"></a>Löschen einer Kachel

Wenn Sie eine Kachel aus (Start, Taskleiste) lösen möchten, verwenden Sie die **[RequestDeleteAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.secondarytile.requestdeleteasync)** Methode.

Dies eignet sich für Fälle, in dem der Inhalt des Benutzers angeheftet nicht mehr gültig ist. Beispielsweise sollten wenn Ihrer app Sie ein Notebook zu starten und die Taskleiste anheften können, und der Benutzer dann das Notebook löscht, Sie einfach die mit dem Notebook verknüpfte Kachel löschen.

```csharp
// Initialize a secondary tile with the same tile ID you want removed.
// Or, locate it with FindAllAsync()
SecondaryTile toBeDeleted = new SecondaryTile(tileId);

// And then delete the tile
await toBeDeleted.RequestDeleteAsync();
```


## <a name="unpin-only-from-start"></a>Lösen Sie nur aus starten

Wenn Sie nur eine sekundäre Kachel von Anfang bleiben sie auf der Taskleiste lösen möchten, können Sie rufen die **[StartScreenManager.TryRemoveSecondaryTileAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager.tryremovesecondarytileasync)** Methode. Dadurch wird die Kachel auf ähnliche Weise gelöscht, wenn es nicht mehr mit jeder anderen Oberflächen verknüpft ist.

Diese Methode gibt einen booleschen Wert, der angibt, ob die Kachel nicht mehr an Start angeheftet wurde. Wenn die Kachel wurde nicht im vornherein angeheftet hat, gibt dies auch "true" zurück. Wenn es sich bei Ansichtsfilter war nicht zulässig, oder starten, wird nicht unterstützt, wird false zurückgegeben.

```csharp
await StartScreenManager.GetDefault().TryRemoveSecondaryTileAsync("myTileId");
```


## <a name="resources"></a>Ressourcen

* [TaskbarManager-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Sekundäre Kacheln an Startmenü anheften](secondary-tiles-pinning.md)