---
title: Xbox Live-sandboxes
description: Einführung in die Xbox Live-Sandbox
ms.assetid: e7daf845-e6cb-4561-9dfa-7cfba882f494
ms.date: 10/30/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3f2e5496c9aab2629591a121850685e7f5a1b2fe
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590225"
---
# <a name="xbox-live-sandboxes-introduction"></a>Einführung in die Xbox Live-Sandbox

In der [Xbox Live-Dienstkonfiguration](xbox-live-service-configuration-creators.md) Artikel wurde erläutert, müssen Sie Informationen zu Ihrem Titel konfigurieren [Partner Center](https://partner.microsoft.com/dashboard). Diese Informationen umfassen z. B. die Statistiken, Bestenlisten, Lokalisierung und mehr. Änderungen an Ihrer Xbox Live-Dienstkonfiguration müssen von Partner Center in Ihre Entwicklung Sandbox veröffentlicht werden, bevor die Änderungen vom Rest des Xbox Live übernommen werden und in der Titelleiste Ihres zugegriffen werden können.

Eine Sandbox für die Entwicklung können Sie Änderungen an Ihrem Titel in einer isolierten Umgebung arbeiten. Sandboxes bieten mehrere Vorteile:

1. Sie können zu Änderungen an der ein Update für den Titel durchlaufen, ohne Auswirkungen auf die Version, die in der Produktion aktiv ist.
2. Aus Gründen der Sicherheit können einige Tools nur in einer Sandbox für die Entwicklung.
3. Anderer Herausgeber nicht angezeigt, was Sie gerade arbeiten ohne Zugriff auf Ihre Sandbox.

Standardmäßig sind in der Sandbox RETAIL Xbox One-Konsolen und Windows 10-PCs. Sie müssen Ihre PC bzw. Xbox One mit der Entwicklung-Sandbox auf diese Version von der Xbox Live-Dienstkonfiguration zu wechseln. Es ist wichtig, daran denken, die das Gerät zurück an den Sandkasten im Einzelhandel zu ändern, wenn müssen Sie etwas zu im Einzelhandel testen oder eine Unterbrechung an Ihre bevorzugten Xbox Live zu spielen Spiele nutzen möchten.

## <a name="finding-out-about-your-sandbox"></a>Suchen Sie über Ihre sandbox

Eine Sandbox ist für Sie erstellt, wenn Sie einen Titel erstellen. Sie finden die Sandkasten-ID öffnen Sie Ihr Produkt in **Partner Center** und navigieren Sie zu **Services** > **Xbox Live**. Die **Sandkasten-ID** wird am oberen Rand der Seite aufgeführt.

![](../images/getting_started/devcenter_sandbox_id.png)

## <a name="switch-your-pcs-development-sandbox"></a>Der PC Entwicklung Sandbox wechseln
Sie können Ihren PC in der Sandbox für die Entwicklung mithilfe von Unity, Windows Device Portal (WPD) oder über die Befehlszeile wechseln.

### <a name="unity"></a>Unity

#### <a name="prerequisites"></a>Voraussetzungen
Muss Folgendes ausgeführt werden, bevor Sie in der Sandbox Entwicklung in Unity wechseln können:

1. [Konfigurieren von Xbox Live in Unity](configure-xbox-live-in-unity.md)

#### <a name="switch-sandboxes"></a>Sandboxes wechseln
Die integrierten in Xbox Live-Konfiguration können Sie zwischen Ihrer Entwicklungs- und RETAIL Sandboxes ganz einfach umschalten. Um zu starten, wechseln Sie zu **Xbox Live** > **Konfiguration** im Menü. Sehen Sie in die aktuelle Sandbox die **Modus Entwicklerkonfiguration** Abschnitt.

1. Wenn **Entwicklermodus** sagt **aktiviert**, sind Sie derzeit in der Entwicklung Sandbox Ihres Spiels zugeordnet. Klicken Sie auf die **wechseln Sie zurück zum Modus für im Einzelhandel** Schaltfläche ausgelagert werden sollen.
2. Wenn **Entwicklermodus** sagt **deaktiviert**, sind Sie derzeit in der Sandbox Einzelhandel. Klicken Sie auf die **wechseln Sie zu den Entwicklermodus** Schaltfläche einfügen.

![XBL aktiviert](../images/unity/unity-xbl-dev-mode.PNG)

### <a name="windows-device-portal"></a>Windows Device Portal

#### <a name="prerequisites"></a>Voraussetzungen
Die folgenden Anforderungen ausgeführt werden, bevor Sie Ihre Sandbox in Windows Device Portal (WPD) wechseln:

1. [Einrichten der Device-Portal auf der Windows-Desktop](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

#### <a name="switch-sandboxes"></a>Sandboxes wechseln

1. Open **Windows Dev-Portal** durch Herstellen einer Verbindung in Ihrem Webbrowser, wie beschrieben in der [Setup Device Portal auf der Windows-Desktop](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop) Artikel.
2. Klicken Sie auf **Xbox Live**.
3. Geben Sie Ihre Entwicklung Sandbox in das Textfeld ein, und klicken Sie auf **ändern**.

![](../images/getting_started/wdp_switch_sandbox.png)

Um wieder auf "RETAIL" zu wechseln, können Sie im Einzelhandel hier eingeben.

### <a name="command-line"></a>Befehlszeile

#### <a name="prerequisites"></a>Voraussetzungen
Muss Folgendes ausgeführt werden, bevor Sie in der Sandbox "Entwicklung" über Befehlszeile wechseln können:

1. Laden Sie das Xbox Live-Tools-Paket auf [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) und Entpacken Sie es.

#### <a name="switch-sandboxes"></a>Sandboxes wechseln
1. Führen Sie die Batchdatei SwitchSandbox.cmd in **Administratormodus**.

Führen Sie es im Administratormodus, um Ihre Sandbox zu wechseln. Das erste Argument ist der Sandbox. Wenn Sie mit der Sandbox MJJSQH.58 wechseln möchten, würden Sie z. B. diesen Befehl verwenden:

```cmd
SwitchSandbox.cmd MJJSQH.58
```

Um wieder auf "RETAIL" zu wechseln, geben Sie einfach, die als zweites Argument.

```cmd
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Wechseln Sie Ihre Xbox One-Konsole Development-sandbox

### <a name="using-xbox-dev-portal"></a>Verwenden von Xbox Dev-Portal

Sie können im Xbox-Dev-Portal verwenden, die Sandbox auf der Konsole ändern. Wechseln Sie zu diesem Zweck auf [Dev-Startseite](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) auf Ihrer Konsole und [aktivieren Sie das Device Portal](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-xbox). Nachdem Sie das Xbox Dev-Portal geöffnet haben:

2. Klicken Sie auf **Xbox Live**.
3. Geben Sie Ihre Entwicklung Sandbox in das Textfeld ein, und Küken **ändern**.

![](../images/getting_started/xdp_switch_sandbox.png)

### <a name="using-xbox-one-console-ui"></a>Verwenden die Xbox One-Verwaltungskonsolen-Benutzeroberfläche

Können Sie [Dev-Startseite](https://docs.microsoft.com/windows/uwp/xbox-apps/dev-home) , die Sandbox auf Ihrer Konsole direkt zu ändern:

1. Klicken Sie auf **Änderung Sandbox**auf **Schnellaktionen**.
2. Geben Sie den Sandkasten-ID, und klicken Sie dann auf **speichern und starten Sie**.

### <a name="sign-in-with-the-xbox-app"></a>Melden Sie sich mit der Xbox-App

Nachdem Sie Ihrem Entwicklungs-PC mit der richtigen Sandbox Titel Ihres gewechselt haben, sollten Sie stellen Sie sicher, dass Sie für Xbox Live mit einem geeignete-Konto angemeldet sind. Dies ist möglich durch Anmelden bei der [Xbox Live-App](https://www.xbox.com/en-US/xbox-app). Sobald Ihre Entwicklungsumgebung gestartet wird, mit der gewünschten Sandbox die Xbox-App werden Benutzeranmeldung mit den gleichen Einschränkungen wie alle anderen Xbox Live-service auf die Sandbox ausgeführt wird. Dadurch wird es nützlich, um sicherzustellen, dass Sie ein gültiges Konto für den Sandkasten verwenden.
