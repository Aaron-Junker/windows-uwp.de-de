---
title: Häufig gestellte Fragen
description: Häufig gestellte Fragen zu UWP auf Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 38b5144d9b70e6e2825ef11a64be8873ec5b7d63
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57614555"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Funktioniert etwas nicht wie erwartet? Auf dieser Seite finden Sie häufig gestellte Fragen. Lesen Sie außerdem das Thema [Bekannte Probleme](known-issues.md) und besuchen Sie das Forum zum [Entwickeln von universellen Windows-Apps](https://go.microsoft.com/fwlink/?linkid=839446). 

### <a name="why-arent-my-games-and-apps-working"></a>Warum funktionieren meine Spiele und Apps nicht?

Wenn Ihre Spiele und Apps nicht funktionieren oder Sie keinen Zugriff auf den Store oder die Live-Dienste haben, ist vermutlich der Entwicklermodus aktiviert. Um festzustellen, in welchem Modus Sie sich gerade befinden, drücken Sie die **Startbildschirmtaste** auf dem Controller. Wenn Sie bei diesem Vorgang zu Dev Home anstatt zum Einzelhandel-Startbildschirm weitergeleitet werden, befinden Sie sich im Entwicklermodus. Wenn Sie spielen möchten, können Sie Dev Home öffnen und wieder in den Einzelhandelsmodus wechseln, indem Sie die Schaltfläche **Leave developer mode** verwenden.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Warum kann ich über Visual Studio keine Verbindung zu meiner Xbox One herstellen?

Vergewissern Sie sich zunächst, dass der Entwicklermodus und nicht der Einzelhandelsmodus aktiviert ist. Im Einzelhandelsmodus können Sie keine Verbindung mit der Xbox One herstellen. Um festzustellen, in welchem Modus Sie sich gerade befinden, drücken Sie die **Startbildschirmtaste** auf dem Controller. Wenn anstelle von Dev Home Gold-/Live-Inhalte angezeigt werden, befinden sich im Einzelhandelsmodus und müssen die DevMode-Aktivierungs-App ausführen, um in den Entwicklermodus zu wechseln.

> [!NOTE]
> Damit Sie eine App bereitstellen können, muss ein Benutzer angemeldet sein.

Weitere Informationen finden Sie unter [Beheben von Bereitstellungsfehlern](#fixing-deployment-failures) weiter unten auf dieser Seite.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>Wie wechsle ich zwischen Einzelhandels- und Entwicklermodus?

Folgen Sie den Anweisungen zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md), um mehr über diese Modi zu erfahren.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>Woran erkenne ich, ob ich mich im Einzelhandels- oder im Entwicklermodus befinde?

Folgen Sie den Anweisungen zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md), um mehr über diese Modi zu erfahren. 

Um festzustellen, in welchem Modus Sie sich gerade befinden, drücken Sie die **Startbildschirmtaste** auf dem Controller. 
- Wenn Sie bei diesem Vorgang zu Dev Home weitergeleitet werden, befinden Sie sich im Entwicklermodus.
- Wenn Gold-/Live-Inhalte angezeigt werden, befinden Sie sich im Einzelhandelsmodus.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>Funktionieren meine Spiele und Apps auch, wenn ich den Entwicklermodus aktiviere?

Ja, Sie können vom Entwicklermodus in den Einzelhandelsmodus wechseln, in dem Sie Ihre Spiele spielen können. Weitere Informationen finden Sie auf der Seite zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md). 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>Kann x86-Apps für Xbox entwickeln und veröffentlichen?
Xbox unterstützt die Entwicklung von x86 Apps oder deren Übermittlung an den Store nicht mehr. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>Gehen meine Spiele und Apps oder gespeicherte Änderungen verloren?

Wenn Sie das Entwicklerprogramm verlassen, gehen dabei installierte Spiele und Apps nicht verloren. Außerdem: Wenn Sie beim Spielen online waren, werden Ihre gespeicherten Spiele im Cloudprofil Ihres Live-Kontos gespeichert und gehen nicht verloren.

### <a name="how-do-i-leave-the-developer-program"></a>Wie verlasse ich das Entwicklerprogramm?

Weitere Informationen dazu, wie Sie das Entwicklerprogramm verlassen, finden Sie im Thema zur [Deaktivierung des Xbox One-Entwicklermodus](devkit-deactivation.md).

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>Ich habe meine Xbox One verkauft und im Entwicklermodus belassen. Wie deaktiviere ich den Entwicklermodus?

Wenn Sie keinen Zugriff mehr auf Ihre Xbox One haben, können Sie es im Partner Center von Windows deaktivieren. Weitere Informationen finden Sie unter der **deaktivieren Sie die Konsole mit der Partner Center** im Abschnitt der [Xbox eine Developer-Modus Deaktivierung](devkit-deactivation.md#deactivate-your-console-using-partner-center) Thema. 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>Bin ich immer noch den Entwicklermodus, aber ich links im Developer-Programm, das über Partner Center. Wie gehe ich vor?

Starten Sie Dev Home, und wählen Sie die Schaltfläche **Leave developer mode** aus. Dadurch wird die Konsole im Einzelhandelsmodus neu gestartet. 

### <a name="can-i-publish-my-app"></a>Kann ich meine App veröffentlichen?

Sie können [Veröffentlichen von apps](../publish/index.md) über Partner Center, wenn Sie haben eine [Developer-Konto](https://developer.microsoft.com/store/register). Die auf einer Xbox One im Einzelhandelsmodus erstellten und getesteten UWP-Apps durchlaufen denselben Aufnahme-, Überprüfungs- und Veröffentlichungsprozess, den Windows heutzutage durchführt. Mithilfe zusätzlicher Überprüfungen werden die aktuellen Xbox One-Standards erfüllt.

### <a name="can-i-publish-my-game"></a>Kann ich mein Spiel veröffentlichen?

Sie können UWP und die Xbox One im Entwicklermodus verwenden, um Ihre Spiele auf der Xbox One zu erstellen und zu testen. Um UWP-Spiele zu veröffentlichen, müssen Sie sich bei [ID@XBOX](https://www.xbox.com/Developers/id) registrieren oder Teil des [Xbox Live Creators-Programms](https://developer.microsoft.com/games/xbox/xboxlive/creator) sein. Weitere Informationen finden Sie unter [Übersicht über das Entwicklerprogramm](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html).

### <a name="will-the-standard-game-engines-work"></a>Können die standardmäßigen Spielengines verwendet werden?

Informationen hierzu finden Sie auf der Seite [Bekannte Probleme](known-issues.md) für diese Version.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Welche Funktionen und Systemressourcen sind für UWP-Spiele auf Xbox One verfügbar? 

Informationen hierzu finden Sie unter [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>Kann ein DirectX 12-UWP-Spiel, das ich erstellt habe, auf meiner Xbox One im Entwicklermodus ausgeführt werden?

Informationen hierzu finden Sie unter [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>Wird die gesamte UWP-API-Oberfläche auf Xbox verfügbar sein?

Informationen hierzu finden Sie auf der Seite [Bekannte Probleme](known-issues.md) für diese Version.

### <a name="fixing-deployment-failures"></a>Beheben von Bereitstellungsfehlern

Wenn die Bereitstellung Ihrer App in Visual Studio fehlschlägt, können Ihnen die folgenden Schritte beim Beheben des Problems helfen. Wenn Sie nicht weiterkommen, bitten Sie im Forum um Hilfe.

> [!NOTE]
> Damit Sie eine App bereitstellen können, muss ein Benutzer angemeldet sein. Wenn Sie die Fehlermeldung 0x87e10008 erhalten, vergewissern Sie sich, dass ein Benutzer angemeldet ist, und versuchen Sie es noch einmal.

Wenn Visual Studio keine Verbindung mit Ihrer Xbox One herstellen kann:

1. Stellen Sie sicher, dass Sie sich im Entwicklermodus befinden (wie weiter oben auf dieser Seite erläutert).
2. Vergewissern Sie sich, dass der Entwicklungscomputer richtig eingerichtet wurde. Haben Sie *alle* Anweisungen in [Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One](getting-started.md) befolgt? 

3. Ist dies nicht der Fall, lesen Sie sich das Thema zur [Einrichtung der Entwicklungsumgebung](development-environment-setup.md) und das Thema zur [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md) durch.

4. Vergewissern Sie sich, dass Sie von Ihrem Entwicklungscomputer aus einen Pingbefehl an die IP-Adresse Ihrer Konsole senden können.
  > [!NOTE]
  > Es wird empfohlen, eine Kabelverbindung zu Ihrer Konsole zu verwenden, um die bestmögliche Leistung bei der Bereitstellung zu erzielen.

5. Stellen Sie sicher, dass in der Dropdownliste für die Authentifizierung auf der Registerkarte **Debuggen** die Option „Universell (unverschlüsseltes Protokoll)“ ausgewählt ist. Weitere Informationen finden Sie unter [Einrichtung der Entwicklungsumgebung](development-environment-setup.md).


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Wie kann ich beim Erstellen einer App mit HTML/JavaScript die Gamepad-Navigation aktivieren?

TVHelpers ist ein Satz von JavaScript- und XAML-/C#-Beispielen und -Bibliotheken, mit denen Sie großartige Xbox One-Inhalte und TV-Inhalte in JavaScript und C# erstellen können. TVJS ist eine Bibliothek, mit der Sie Premium-UWP-Apps für Xbox One erstellen können. TVJS bietet u. a. Unterstützung für die automatische Controllernavigation, Rich-Media-Wiedergabe und Suche. Sie können TVJS mit einer gehosteten Web-App genauso einfach wie mit einer gepackten UWP-Web-App mit vollständigem Zugriff auf Windows-Runtime-APIs verwenden.

Weitere Informationen finden Sie im Projekt [TVHelpers](https://github.com/Microsoft/TVHelpers) und im Projekt [wiki](https://github.com/Microsoft/TVHelpers/wiki).

## <a name="see-also"></a>Siehe auch
- [Bekannte Probleme mit der UWP für Xbox One](known-issues.md)
- [UWP auf Xbox One](index.md)
- [UWP auf Xbox One](index.md)
