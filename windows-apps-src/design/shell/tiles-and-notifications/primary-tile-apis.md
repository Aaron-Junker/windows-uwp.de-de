---
Description: Sie können die primäre Kachel Ihrer eigenen App programmgesteuert an die Startseite anheften, genauso wie Sie sekundäre Kacheln anheften können. Und Sie können prüfen, ob sie derzeit angeheftet ist.
title: Primäre Kachel-APIs
label: Primary tile API's
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, StartScreenManager, primäre Kachel anheften, primäre Kachel-APIs, überprüfen, ob die Kachel angeheftet ist, Live-Kachel
ms.localizationpriority: medium
ms.openlocfilehash: 04d7c66b358a3a465522ad3b56d8ae926358ae57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57596195"
---
# <a name="primary-tile-apis"></a>Primärkachel-APIs
 

Mit Primärkachel-APIs können Sie überprüfen, ob die App auf der Startseite angeheftet ist und anfragen, ob Ihre App an die primäre Kachel der App angeheftet werden kann.

> [!IMPORTANT]
> **Erfordert Creators Update**: Sie müssen SDK 15063 ausgerichtet und Build 15063 oder höher, um die primäre Kachel APIs verwenden ausgeführt werden.

> **Wichtige APIs**: [**StartScreenManager class**](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager), [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_), [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)


## <a name="when-to-use-primary-tile-apis"></a>Verwendung von Primärkachel-APIs

Sie haben großen Aufwand betrieben, um die primäre Kachel Ihrer App benutzerfreundlich zu gestalten, und jetzt haben Sie die Möglichkeit, den Benutzer aufzufordern, sie an die Startseite anzuheften. Bevor wir uns den Code genauer ansehen, sollten Sie beim Entwurf der Oberfläche aber noch Folgendes bedenken:

* **Wir empfehlen**, eine unterbrechungsfreie und leicht ausblendbare UX in Ihrer App mit dem deutlichen Handlungsaufruf „Live-Kachel anheften“ zu entwerfen.
* **Wir empfehlen**, die Vorteile der Live-Kachel Ihrer App deutlich zu erläutern, bevor Sie den Benutzer auffordern, sie anzuheften.
* **Wir raten davon ab**, einen Benutzer zum Anheften der Kachel Ihrer App aufzufordern, wenn die Kachel bereits angeheftet ist oder sie vom Gerät nicht unterstützt wird (weitere Informationen folgen).
* **Wir raten davon ab**, den Benutzer wiederholt zum Anheften der Kachel Ihrer App aufzufordern (es wird ihm wahrscheinlich lästig).
* **Wir raten davon ab**, die Anheften-API ohne explizite Benutzerinteraktion aufzurufen, oder wenn die App minimiert/nicht geöffnet ist.


## <a name="checking-whether-the-apis-exist"></a>Überprüfen, ob die APIs vorhanden sind

Wenn Ihre App ältere Versionen von Windows 10 unterstützt, müssen Sie überprüfen, ob diese Primärkachel-APIs verfügbar sind. Verwenden Sie dazu ApiInformation. Wenn die Primärkachel-APIs nicht verfügbar sind, vermeiden Sie, Aufrufe an die APIs durchzuführen.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.StartScreen.StartScreenManager"))
{
    // Primary tile API's supported!
}
else
{
    // Older version of Windows, no primary tile API's
}
```


## <a name="check-if-start-supports-your-app"></a>Überprüfen, ob Start die App unterstützt

Abhängig vom aktuellen Startmenü und dem Typ Ihrer App wird das Anheften Ihrer App an die aktuelle Startseite möglicherweise nicht unterstützt. Nur Desktop und Mobile unterstützen das Anheften der primären Kachel an die Startseite. Bevor Sie eine Anheften-UI anzeigen oder Anheften-Code ausführen, müssen Sie daher zunächst prüfen, ob Ihre App für die aktuelle Startseite überhaupt unterstützt wird. Wenn sie nicht unterstützt wird, fordern Sie den Benutzer nicht zum Anheften der Kachel auf.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>Überprüfen, ob die primäre Kachel derzeit angeheftet ist

Um festzustellen, ob die primäre Kachel derzeit an die Startseite angeheftet ist, verwenden Sie die Methode [ContainsAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_).

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>Anheften Ihrer primären Kachel

Wenn die primäre Kachel aktuell nicht angeheftet ist und die Kachel von der Startseite unterstützt wird, sollten Sie Benutzern den Tipp anzeigen, dass die primäre Kachel angeheftet werden kann.

> [!NOTE]
> Sie müssen diese API über eine UI-Thread aufrufen, während Ihre app im Vordergrund, und Sie diese API nur aufrufen sollten, nachdem der Benutzer absichtlich angefordert hat die primäre Kachel angeheftet werden (z. B. nach dem der Benutzer auf Ja, Ihre Tipp zum Anheften der Kachel geklickt haben).

Klickt der Benutzer auf Ihre Schaltfläche, um die primäre Kachel anzuheften, würden Sie dann die Methode [RequestAddAppListEntryAsync](https://docs.microsoft.com/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) aufrufen, um anzufordern, dass die Kachel an die Startseite angeheftet wird. Dadurch wird ein Dialogfeld angezeigt, das den Benutzer auffordert zu bestätigen, dass Ihre Kachel an die Startseite angeheftet werden soll.

Dadurch wird ein boolescher Wert zurückgegeben, der angibt, ob die Kachel jetzt an die Startseite angeheftet ist. Wenn die Kachel bereits angeheftet war, wird sofort „true“ zurückgegeben, ohne dass dem Benutzer das Dialogfeld angezeigt wird. Wenn der Benutzer im Dialogfeld auf „Nein“ klickt oder das Anheften Ihrer Kachel an die Startseite nicht unterstützt wird, wird „false“ zurückgegeben. Andernfalls hat der Benutzer auf „Ja“ geklickt und die Kachel wurde angeheftet, und die API gibt „true“ zurück.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// And pin it to Start
bool isPinned = await StartScreenManager.GetDefault().RequestAddAppListEntryAsync(entry);
```


## <a name="resources"></a>Ressourcen

* [Vollständige Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-pin-primary-tile)
* ["An Taskleiste anheften"](../pin-to-taskbar.md)
* [Kacheln, Signale und Benachrichtigungen](index.md)
* [Kachel "Adaptive"-Dokumentation](create-adaptive-tiles.md)
