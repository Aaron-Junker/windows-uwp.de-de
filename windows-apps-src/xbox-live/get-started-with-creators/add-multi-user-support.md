---
title: Hinzufügen von Unterstützung für mehrere Benutzer zu Ihr Unity-Spiel
description: Hinzufügen von Unterstützung für mehrere Benutzer zu Ihr Unity-Spiel mithilfe von der Xbox Live Unity-Plug-in
ms.assetid: ''
ms.date: 07/14/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Unity, mehrere Benutzer
ms.localizationpriority: medium
ms.openlocfilehash: 483a0e966be756de483bf7e2ab8458647397687b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613735"
---
# <a name="add-multi-user-support-to-your-unity-game"></a>Hinzufügen von Unterstützung für mehrere Benutzer zu Ihr Unity-Spiel
> [!IMPORTANT]
> Die Xbox Live Unity-Plug-in unterstützt keine Erfolge oder online Multiplayer-Spiele und wird nur empfohlen, für die [Xbox Live Creators-Programm](../developer-program-overview.md) Member.

Unterstützung für mehrere Benutzer wird jetzt von der Xbox Live Unity-Plug-In für Szenarien unterstützt, bei denen mehrere lokale Unternehmen. Jeder Spieler kann ihre eigenen Xbox Live-Konto für Statistiken und melden Sie sich verwenden. Ihr Spiel kann auch Gäste für Benutzer aktivieren, denen keine Xbox Live-Konten zu spielen. Dieses Feature wird nur auf der Xbox-Konsolen unterstützt.

Dieses Tutorial führt Sie durch Hinzufügen von Unterstützung für mehrere Benutzer zu den verschiedenen Xbox Live prefabs (Vorlagen).

> [!IMPORTANT]
> Szenarien für mehrere Benutzer werden nur auf Xbox-Konsolen unterstützt. Diese Funktion funktioniert nicht auf PC.

## <a name="prerequisites"></a>Voraussetzungen
Müssen Sie befolgt haben die [Einstieg](configure-xbox-live-in-unity.md) Tutorial zum Aktivieren und konfigurieren die Xbox Live.

## <a name="adding-and-signing-in-multiple-xbox-live-users"></a>Hinzufügen, und melden Sie sich mehrere Xbox Live-Benutzer

1. Von der **Assets** > **Xbox Live** > **Prefabs** Ordner, ziehen Sie in Ihrer Szene so viele **XboxLiveUser** prefab Instanzen wie Spieler fallen. In diesem Tutorial werden zwei Spieler also zwei **XboxLiveUser** Instanzen werden in der Szene hinzugefügt werden. Wir werden bezeichnen sie als **XboxLiveUser** und **XboxLiveUser2** der Einfachheit halber.

2. Ordnungsgemäß Hinzufügen der Benutzer ein, und melden Sie sich, diese im, fügen Sie zwei Instanzen der **UserProfile** , um der Szene eine für jede prefab **XboxLiveUser**. Stellen Sie sicher, dass eine Instanz von der `XboxLiveServices` prefab in der Szene. Stellen Sie außerdem sicher, dass die beiden bewegt **UserProfile** Objekte, die in der Szene kann sie voneinander entfernt. Da diese Prefabs der Eventsystem Unity verwenden, stellen Sie sicher, dass Sie eine Instanz von der `EventSystem` in der Szene.

    ![Hierarchie der Unterstützung für mehrere Benutzer in Xbox Live Unity-Plug-in-Tutorial-Projekt](../images/unity/MUA-Tutorial-Hierarchy.png)

    ![Spiele-Szene von Unterstützung für mehrere Benutzer in Xbox Live Unity-Plug-in-Tutorial-Projekt](../images/unity/MUA-Tutorial-GameScene.png)

3. Weisen Sie eine Instanz der **XboxLiveUser** prefabs (Vorlagen) auf die Szene jedes der **UserProfile** Objekte.

    ![UserProfile-Prefab für die Unterstützung für mehrere Benutzer](../images/unity/user-profile-for-mua.png)

4. Da mehrere Benutzer Anwendungen nur für Xbox One-Geräte unterstützt werden, Hinzufügen von Domänencontroller-Unterstützung, um die **UserProfile** Objekte ist erforderlich. Auf jedem **UserProfile** -Objekt ist ein Feld namens `InputControllerButton` , können Sie angeben, den Joystick und Schaltfläche Zahlen jedes **UserProfile** überwacht werden soll.

In diesem Tutorial verwenden wir `joystick 1 button 0` für die **UserProfile** , dem Spieler 1 zugewiesen ist und `joystick 2 button 0` für Player-2 und die zweite **UserProfile** spielobjekt. Damit weisen Sie die `A` Schaltfläche für die Interaktion mit **UserProfile** für zwei Controller.

> [!Note]
> Sehen Sie sich, Weitere Informationen zur Unterstützung von Xbox-Controller in den Xbox Live Unity-Plug-in: [Hinzufügen von Domänencontroller-Unterstützung zu Xbox Live prefabs (Vorlagen)](add-controller-support-to-xbox-live-prefabs.md)

5. Führen Sie die Szene im Editor, und drücken, führen Sie auf die Schaltflächen, um sicherzustellen, dass alles richtig konfiguriert ist.

    ![Unterstützung für mehrere Benutzer im Unity-Editor](../images/unity/run-example-mua.png)

## <a name="building-and-testing-the-uwp"></a>Erstellen und testen die UWP

1. Nachdem die Schritte, die am beschrieben die [entwickeln Creators Titel mit Unity](configure-xbox-live-in-unity.md) Tutorial öffnen Sie die exportierte UWP-Projektmappe in Visual Studio.

2. Unter dem Projekt UWP Ihres Spiels, klicken Sie mit der rechten Maustaste auf die `package.appxmanifest.xml` Datei, und wählen **Ansichtscode**.

3. Unter den `<Properties></Properties>` Abschnitt, fügen Sie die Eingabe von mehreren Benutzern für die app ermöglicht Folgendes: `<uap:SupportedUsers>multiple</uap:SupportedUsers>`

4. Um auf der Xbox zu testen, führen Sie in der Dokumentation unter den [Ihrer UWP auf Xbox-Entwicklungsumgebung einrichten](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/development-environment-setup) Tutorial.

## <a name="using-the-other-xbox-live-prefabs-with-multiple-users"></a>Verwenden die anderen Xbox Live prefabs (Vorlagen) mit mehreren Benutzern

In der **Beispiele** Ordner des Plug-Ins, besteht eine **MultiUserExample** Szene, die zeigt, wie die verschiedenen prefabs (Vorlagen) die **XboxLiveUser** Instanzen, um bestimmte anzeigen Informationen für jeden Benutzer.
