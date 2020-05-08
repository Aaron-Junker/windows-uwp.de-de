---
Description: Sie können Ihre APP Programm gesteuert an die Taskleiste anheften, und Sie können überprüfen, ob Sie zurzeit fixiert ist.
title: Anheften einer App an die Taskleiste
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Taskleiste, Task leisten-Manager, an Taskleiste anheften, primäre Kachel
ms.localizationpriority: medium
ms.openlocfilehash: 44ef6430398960e13fe5eebb40a52d022df6f0d2
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970655"
---
# <a name="pin-your-app-to-the-taskbar"></a>Anheften einer App an die Taskleiste

Sie können Ihre eigene APP Programm gesteuert an die Taskleiste anheften, genauso wie Sie [Ihre APP an das Startmenü anheften](tiles-and-notifications/primary-tile-apis.md)können. Und Sie können überprüfen, ob Ihre APP zurzeit fixiert ist, und ob die Taskleiste das Anheften zulässt. 

![Taskleiste](images/taskbar/taskbar.png)

> [!IMPORTANT]
> **Erfordert das Fall Creators Update**: Sie müssen das SDK 16299 als Ziel verwenden und Build 16299 oder höher ausführen, um die Task leisten-APIs zu verwenden.

> **Wichtige APIs**: [taskbarmanager-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) 


## <a name="when-should-you-ask-the-user-to-pin-your-app-to-the-taskbar"></a>Wann sollten Sie den Benutzer bitten, Ihre APP an die Taskleiste anzuheften? 

Mit der [taskbarmanager-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager) können Sie den Benutzer bitten, Ihre APP an die Taskleiste anzuheften. der Benutzer muss die Anforderung genehmigen. Sie haben viel Mühe, eine prominente APP zu entwickeln, und jetzt können Sie den Benutzer bitten, ihn an die Taskleiste anzuheften. Bevor wir uns jedoch mit dem Code beschäftigen, sind folgende Punkte zu beachten, wenn Sie Ihre Erfahrungen entwerfen:

* **Erstellen Sie eine nicht** unterbrechungsfreie und leicht zu löschende Benutzeroberfläche in Ihrer APP mit dem Befehl "an Taskleiste anheften". Vermeiden Sie zu diesem Zweck die Verwendung von Dialogfeldern und Flyouts. 
* Erläutern Sie den Wert Ihrer **App eindeutig,** bevor Sie den Benutzer bitten, ihn anzuheften.
* Bitten Sie einen Benutzer **nicht** , Ihre APP anzuheften, wenn die Kachel bereits angeheftet ist oder das Gerät Sie nicht unterstützt. (In diesem Artikel wird erläutert, wie Sie feststellen können, ob anheften unterstützt wird.
* Bitten Sie den Benutzer **nicht** wiederholt, Ihre APP anzuheften (Sie werden wahrscheinlich benachrichtigt).
* Nennen Sie die PIN-API **nicht** ohne explizite Benutzerinteraktion oder wenn Ihre APP minimiert/nicht geöffnet ist.


## <a name="1-check-whether-the-required-apis-exist"></a>1. Überprüfen Sie, ob die erforderlichen APIs vorhanden sind

Wenn Ihre APP ältere Versionen von Windows 10 unterstützt, müssen Sie überprüfen, ob die taskbarmanager-Klasse verfügbar ist. Sie können diese Überprüfung mithilfe der [apiinformation. istypeer Present-Methode](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation#Windows_Foundation_Metadata_ApiInformation_IsTypePresent_System_String_) durchführen. Wenn die taskbarmanager-Klasse nicht verfügbar ist, sollten Sie keine Aufrufe der APIs ausführen.

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


## <a name="2-check-whether-taskbar-is-present-and-allows-pinning"></a>2. Überprüfen Sie, ob die Taskleiste vorhanden und das Anheften zulässt.

Windows-Apps können auf einer Vielzahl von Geräten ausgeführt werden. nicht alle unterstützen die Taskleiste. Derzeit unterstützen nur Desktop Geräte die Taskleiste. 

Auch wenn die Taskleiste verfügbar ist, kann eine Gruppenrichtlinie auf dem Computer des Benutzers das Anheften der Taskleiste deaktivieren. Bevor Sie also versuchen, Ihre APP anzuheften, müssen Sie überprüfen, ob das Anheften an die Taskleiste unterstützt wird. Die [taskbarmanager. ispinningallowed-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsPinningAllowed) gibt true zurück, wenn die Taskleiste vorhanden ist und das Anheften ermöglicht. 

```csharp
// Check if taskbar allows pinning (Group Policy can disable it, or some device families don't have taskbar)
bool isPinningAllowed = TaskbarManager.GetDefault().IsPinningAllowed;
```

> [!NOTE]
> Wenn Sie Ihre APP nicht an die Taskleiste anheften möchten und nur herausfinden möchten, ob die Taskleiste verfügbar ist, verwenden Sie die [taskbarmanager. IsSupported-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsSupported).


## <a name="3-check-whether-your-app-is-currently-pinned-to-the-taskbar"></a>3. Überprüfen Sie, ob Ihre APP zurzeit an die Taskleiste angeheftet ist.

Natürlich gibt es keinen Punkt, an dem der Benutzer aufgefordert wird, die APP an die Taskleiste anzuheften, wenn Sie bereits angeheftet ist. Sie können die [taskbarmanager. iscurrentapppinnedasync-Methode](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.IsCurrentAppPinnedAsync) verwenden, um zu überprüfen, ob die APP bereits angeheftet wurde, bevor der Benutzer gefragt wird.

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


##  <a name="4-pin-your-app"></a>4. anheften Ihrer APP

Wenn die Taskleiste vorhanden ist und das Anheften zulässig ist und Ihre APP derzeit nicht fixiert ist, sollten Sie einen feinen Tipp anzeigen, um den Benutzern mitzuteilen, dass Sie Ihre APP anheften können. Beispielsweise können Sie an einer beliebigen Stelle in der Benutzeroberfläche ein Pin-Symbol anzeigen, auf das der Benutzer klicken kann. 

Wenn der Benutzer auf die Benutzeroberfläche für den PIN-Vorschlag klickt, rufen Sie die [taskbarmanager. requestpincurrentappasync-Methode](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager.RequestPinCurrentAppAsync)auf. Diese Methode zeigt ein Dialogfeld an, in dem der Benutzer aufgefordert wird, zu bestätigen, dass die APP an die Taskleiste angeheftet werden soll.

> [!IMPORTANT]
> Dies muss von einem Benutzeroberflächen Thread im Vordergrund aufgerufen werden. andernfalls wird eine Ausnahme ausgelöst.

```csharp
// Request to be pinned to the taskbar
bool isPinned = await TaskbarManager.GetDefault().RequestPinCurrentAppAsync();
```

![PIN-Dialogfeld](images/taskbar/pin-dialog.png)

Diese Methode gibt einen booleschen Wert zurück, der angibt, ob Ihre APP nun an die Taskleiste angeheftet ist. Wenn Ihre APP bereits angeheftet wurde, gibt die Methode sofort true zurück, ohne das Dialogfeld für den Benutzer anzuzeigen. Wenn der Benutzer im Dialogfeld auf "Nein" klickt oder die APP an die Taskleiste angehefnt ist, gibt die Methode "false" zurück. Andernfalls hat der Benutzer auf Ja geklickt, und die APP wurde fixiert, und die API gibt true zurück.


## <a name="resources"></a>Ressourcen

* [Vollständiges Codebeispiel auf GitHub](https://github.com/WindowsNotifications/quickstart-pin-to-taskbar)
* [Taskbarmanager-Klasse](https://docs.microsoft.com/uwp/api/windows.ui.shell.taskbarmanager)
* [Anheften einer APP an das Startmenü](tiles-and-notifications/primary-tile-apis.md)
