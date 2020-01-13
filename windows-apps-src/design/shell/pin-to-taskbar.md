---
Description: Ihre App kann programmgesteuert an die Taskleiste angeheftet werden und Sie können überprüfen, ob es aktuell ist angeheftet ist.
title: Anheften einer App an die Taskleiste
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Taskleiste, Taskleiste-Manager an primäre Kachel-Taskleiste anheften
ms.localizationpriority: medium
ms.openlocfilehash: 3aa259cd8c5c45ea99d83eaecb8e30fb0438aa8f
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684535"
---
# <a name="pin-your-app-to-the-taskbar"></a>Anheften einer App an die Taskleiste

Sie können Ihrer eigene App programmgesteuert an die Taskleiste anheften und [Ihre App an das Startmenü anheften](tiles-and-notifications/primary-tile-apis.md). Sie können ebenfalls überprüfen, ob Ihre App derzeit angeheftet ist, und ob die Taskleiste ein Anheften ermöglicht. 

![Taskleiste](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **** Erfordert das Fall Creators Update: Sie müssen als Ziel Insider SDK 16299 angeben und Insider-Builds 16299 oder höher ausführen, um die Taskleisten-API zu verwenden.

> **Wichtige APIs**: [TaskbarManager-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>Wenn bitten Sie den Benutzer Ihre App an die Taskleiste anzuheften? 

Mit der [TaskbarManager-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) können Sie die Benutzer auffordern, Ihre App an die Taskleiste anzuheften; der Benutzer muss die Anforderung genehmigen. Sie haben unter großem Aufwand eine herausragende App erstellt und haben jetzt die Möglichkeit, den Benutzer aufzufordern, diese an die Taskleiste anzuheften. Bevor wir uns den Code genauer ansehen, sollten Sie beim Entwurf der Oberfläche aber noch Folgendes bedenken:

* **Wir empfehlen**, eine unterbrechungsfreie und leicht ausblendbare UX in Ihrer App mit dem deutlichen Handlungsaufruf „An Taskleiste anheften“ zu entwerfen. Verwenden Sie zu diesem Zweck keine Dialogfelder und Flyouts. 
* **Wir empfehlen**, die Vorteile Ihrer App deutlich zu erläutern, bevor Sie den Benutzer auffordern, sie anzuheften.
* **Wir raten davon ab**, einen Benutzer zum Anheften Ihrer App aufzufordern, wenn die Kachel bereits angeheftet ist oder sie vom Gerät nicht unterstützt wird. (In diesem Artikel wird erläutert, wie festgelegt wird, ob ein Anheften unterstützt wird oder nicht).
* **Wir raten davon ab**, den Benutzer wiederholt zum Anheften Ihrer App aufzufordern (es wird ihm wahrscheinlich lästig).
* **Wir raten davon ab**, die Anheften-API ohne explizite Benutzerinteraktion aufzurufen, oder wenn die App minimiert/nicht geöffnet ist.


## <a name="1-check-whether-the-required-apis-exist"></a>1. Überprüfen Sie, ob die erforderlichen APIs vorhanden sind.

Wenn Ihre App ältere Versionen von Windows 10 unterstützt, müssen Sie überprüfen, ob die TaskbarManager-Klasse verfügbar ist. Sie können die [ApiInformation.IsTypePresent Methode](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_) zur Überprüfung verwenden. Wenn die TaskbarManager-Klasse nicht verfügbar sind, vermeiden Sie, Aufrufe an die APIs durchzuführen.

```csharp
if (ApiInformation.IsTypePresent("Windows.UI.Shell.TaskbarManager"))
{
    // Taskbar APIs exist!
}

else
{
    // Older version of Windows, no taskbar APIs
}
```


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. Überprüfen Sie, ob die Taskleiste vorhanden ist und ein Anheften ermöglicht

UWP-Apps können auf einer Vielzahl von Geräten ausgeführt werden. Nicht alle Geräte unterstützen die Taskleiste. Derzeit unterstützen nur Desktop-Geräte die Taskleiste. 

Auch wenn die Taskleiste verfügbar ist, kann eine Gruppenrichtlinie auf dem Computer des Benutzers das Anheften an die Taskleiste deaktivieren. Bevor Sie versuchen, Ihre App anzuheften, müssen Sie daher überprüfen, ob das Anheften an die Taskleiste unterstützt wird. Die [TaskbarManager.IsPinningAllowed-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) gibt „true” zurück, wenn die Taskleiste vorhanden ist ein Anheften ermöglicht. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> Wenn Sie die App nicht an die Taskleiste anheften möchten und nur wissen möchten, ob die Taskleiste verfügbar ist, verwenden Sie die [TaskbarManager.IsSupported-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported).


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. Überprüfen Sie, ob Ihre App derzeit an die Taskleiste angeheftet ist

Natürlich besteht kein Grund dafür, den Benutzer aufzufordern, die App an die Taskleiste anzuheften, wenn diese bereits angeheftet ist. Verwenden Sie die [TaskbarManager.IsCurrentAppPinnedAsync-Methode](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync), um zu überprüfen, ob die App bereits angeheftet ist, bevor Sie den Benutzer auffordern.

```csharp
// Check whether your app is currently pinned
bool isPinned = await TaskbarManager.GetDefault().IsCurrentAppPinnedAsync();

if (isPinned)
{
    // The app is already pinned--no point in asking to pin it again!
}
else 
{
    //The app is not pinned. 
}
```


##  <a name="4-pin-your-app"></a>4. Die App anheften

Wenn die Taskleiste vorhanden ist und ein Anheften zugelassen wird und Ihre App derzeit nicht angeheftet ist, sollten Sie einen Tipp anzeigen, damit Benutzer wissen, dass Ihre App angeheftet werden kann. Sie können Sie z. B. irgendwo ein Pinsymbol in der Benutzeroberfläche anzeigen, das die Benutzer anklicken können. 

Wenn der Benutzer auf Ihren Pin-Vorschlag klickt, rufen Sie die [TaskbarManager.RequestPinCurrentAppAsync-Methode](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync) auf. Diese Methode zeigt ein Dialogfeld an, das den Benutzer auffordert, zu bestätigen, dass sie Ihre App an die Taskleiste anheften möchten.

> [!IMPORTANT]
> Dies muss durch einen Vordergrund-UI-Thread aufgerufen werden, andernfalls wird eine Ausnahme ausgelöst.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![PIN-Dialogfeld](images/taskbar/pin-dialog.png)

Diese Methode gibt einen booleschen Wert zurück, der angibt, ob Ihre App jetzt an die Taskleiste angeheftet ist. Wenn die App bereits angeheftet war, wird die Methode sofort „true“ zurückgegeben, ohne dass dem Benutzer das Dialogfeld angezeigt wird. Wenn der Benutzer im Dialogfeld auf "Nein" klickt oder Ihre App nicht an die Taskleiste angeheftet werden kann, wird die Methode "false" zurückgegeben. Andernfalls hat der Benutzer auf „Ja“ geklickt und die App wurde angeheftet, und die API gibt „true“ zurück.


## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [Taskbarmanager-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Anheften einer APP an das Startmenü](tiles-and-notifications/primary-tile-apis.md)