---
description: Sie können die primäre Kachel ihrer eigenen App Programm gesteuert anheften, so wie Sie sekundäre Kacheln anheften können. Und Sie können überprüfen, ob es zurzeit fixiert ist.
title: Primäre Kachel-APIs
label: Primary tile API's
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP, startscreenmanager, primär Kachel anheften, primäre Kachel-APIs, überprüfen, ob Kachel fixiert, Live-Kachel
ms.localizationpriority: medium
ms.openlocfilehash: 83cf11d80ffcd03148cbe5e784aaad5836357796
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029693"
---
# <a name="primary-tile-apis"></a>Primäre Kachel-APIs
 

Mit primären Kachel-APIs können Sie überprüfen, ob Ihre APP aktuell an den Start angeheftet ist, und die primäre Kachel Ihrer APP anheften.

> [!IMPORTANT]
> **Erfordert Creators Update** : Sie müssen das SDK 15063 als Ziel verwenden und Build 15063 oder höher ausführen, um die primären Kachel-APIs zu verwenden.

> **Wichtige APIs** : [**startscreenmanager-Klasse**](/uwp/api/windows.ui.startscreen.startscreenmanager), " [kontainsapplistentryasync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_)", " [requestaddapplistentryasync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) "


## <a name="when-to-use-primary-tile-apis"></a>Verwendungszwecke von primären Kachel-APIs

Sie haben viel Mühe gemacht, um eine hervorragende benutzerfreundliche Darstellung der primären Kachel Ihrer APP zu entwerfen. nun haben Sie die Möglichkeit, den Benutzer zu bitten, ihn anzuheften. Bevor wir uns jedoch mit dem Code beschäftigen, sind folgende Punkte zu beachten, wenn Sie Ihre Erfahrungen entwerfen:

* Erstellen **Sie** eine nicht unterbrechungsfreie und leicht zu löschende Benutzeroberfläche in Ihrer APP mit dem Befehl "Live-Kachel anheften".
* Erläutern Sie den Wert der Live-Kachel Ihrer **App eindeutig,** bevor Sie den Benutzer bitten, ihn anzuheften.
* Bitten Sie einen Benutzer **nicht** , die Kachel Ihrer APP anzuheften, wenn die Kachel bereits angeheftet ist oder das Gerät Sie nicht unterstützt (Weitere Informationen finden Sie folgen).
* Bitten Sie den Benutzer **nicht** wiederholt, die Kachel Ihrer APP anzuheften (Sie werden wahrscheinlich benachrichtigt).
* Nennen Sie die PIN-API **nicht** ohne explizite Benutzerinteraktion oder wenn Ihre APP minimiert/nicht geöffnet ist.


## <a name="checking-whether-the-apis-exist"></a>Es wird überprüft, ob die API vorhanden ist.

Wenn Ihre APP ältere Versionen von Windows 10 unterstützt, müssen Sie überprüfen, ob diese primären Kachel-APIs verfügbar sind. Hierzu verwenden Sie apiinformation. Wenn die primären Kachel-APIs nicht verfügbar sind, sollten Sie keine Aufrufe der APIs ausführen.

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


## <a name="check-if-start-supports-your-app"></a>Überprüfen, ob Start ihre App unterstützt

Abhängig vom aktuellen Startmenü und ihrer Art von App wird das Fixieren der APP an den aktuellen Startbildschirm möglicherweise nicht unterstützt. Nur Desktop-und Mobilgeräte Unterstützung zum Starten der primären Kachel. Daher müssen Sie vor dem Anzeigen der PIN-Benutzeroberfläche oder der Ausführung eines PIN-Codes zunächst überprüfen, ob Ihre APP sogar für den aktuellen Start Bildschirm unterstützt wird. Wenn dies nicht unterstützt wird, fordern Sie den Benutzer nicht auf, die Kachel anzuheften.

```csharp
// Get your own app list entry
// (which is always the first app list entry assuming you are not a multi-app package)
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if Start supports your app
bool isSupported = StartScreenManager.GetDefault().SupportsAppListEntry(entry);
```


## <a name="check-whether-youre-currently-pinned"></a>Überprüfen Sie, ob Sie zurzeit fixiert sind

Wenn Sie feststellen möchten, ob Ihre primäre Kachel aktuell anheftet ist, verwenden [Sie die Methode](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_ContainsAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) "" von "".

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// Check if your app is currently pinned
bool isPinned = await StartScreenManager.GetDefault().ContainsAppListEntryAsync(entry);
```


##  <a name="pin-your-primary-tile"></a>Anheften der primären Kachel

Wenn Ihre primäre Kachel derzeit nicht fixiert ist und ihre Kachel vom Start unterstützt wird, können Sie Benutzern einen Tipp anzeigen, dass Sie Ihre primäre Kachel anheften können.

> [!NOTE]
> Sie müssen diese API von einem UI-Thread aus anrufen, während sich Ihre APP im Vordergrund befindet, und Sie sollten diese API nur dann anrufen, wenn der Benutzer absichtlich angefordert hat, dass die primäre Kachel angeheftet wurde (z. b. Nachdem der Benutzer auf Ja geklickt hat, um den Tipp zum Fixieren der Kachel zu erhalten).

Wenn der Benutzer auf die Schaltfläche klickt, um die primäre Kachel anzuheften, können Sie die [requestaddapplistentryasync](/uwp/api/windows.ui.startscreen.startscreenmanager#Windows_UI_StartScreen_StartScreenManager_RequestAddAppListEntryAsync_Windows_ApplicationModel_Core_AppListEntry_) -Methode aufrufen, um anzufordern, dass Ihre Kachel an den Start angeheftet werden soll. Dadurch wird ein Dialogfeld angezeigt, in dem der Benutzer gefragt wird, ob die Kachel gestartet werden soll.

Dadurch wird ein boolescher Wert zurückgegeben, der angibt, ob die Kachel jetzt zum Starten angeheftet ist. Wenn die Kachel bereits angeheftet wurde, wird sofort true zurückgegeben, ohne dem Benutzer den Dialog anzuzeigen. Wenn der Benutzer im Dialogfeld auf Nein klickt oder die Kachel an Start nicht unterstützt wird, wird false zurückgegeben. Andernfalls hat der Benutzer auf Ja geklickt, und die Kachel wurde fixiert, und die API gibt true zurück.

```csharp
// Get your own app list entry
AppListEntry entry = (await Package.Current.GetAppListEntriesAsync())[0];

// And pin it to Start
bool isPinned = await StartScreenManager.GetDefault().RequestAddAppListEntryAsync(entry);
```


## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-pin-primary-tile)
* [An Taskleiste anheften](../pin-to-taskbar.md)
* [Kacheln, Signale und Benachrichtigungen](index.md)
* [Dokumentation zu adaptiver Kachel](create-adaptive-tiles.md)
