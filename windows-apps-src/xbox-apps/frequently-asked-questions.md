---
title: Häufig gestellte Fragen
description: Wenn die Dinge nicht erwartungsgemäß funktionieren, finden Sie auf dieser Seite häufig gestellte Fragen zu UWP auf Xbox.
ms.date: 03/29/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 265fe827-bd4a-48d4-b362-8793b9b25705
ms.localizationpriority: medium
ms.openlocfilehash: 7da3b6a6507f2652eb2357ab80897d9ba35d7484
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170734"
---
# <a name="frequently-asked-questions"></a>Häufig gestellte Fragen

Funktioniert etwas nicht wie erwartet? Auf dieser Seite finden Sie häufig gestellte Fragen. Lesen Sie außerdem das Thema [Bekannte Probleme](known-issues.md) und besuchen Sie das Forum zum [Entwickeln von universellen Windows-Apps](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

### <a name="why-arent-my-games-and-apps-working"></a>Warum funktionieren meine Spiele und apps nicht?

Wenn Ihre Spiele und apps nicht funktionieren oder Sie keinen Zugriff auf den Store oder Live Dienste haben, werden Sie wahrscheinlich im Entwicklermodus ausgeführt. Um herauszufinden, in welchem Modus Sie sich gerade befinden, drücken Sie auf dem Controller die **Start** Schaltfläche. Wenn Sie auf diese Weise zu dev Home anstatt zur privatarbeit im Einzelhandel gelangen, sind Sie im Entwicklermodus. Wenn Sie spielen möchten, können Sie Dev Home öffnen und wieder in den Einzelhandelsmodus wechseln, indem Sie die Schaltfläche **Leave developer mode** verwenden.

### <a name="why-cant-i-connect-to-my-xbox-one-using-visual-studio"></a>Warum kann ich mit Visual Studio keine Verbindung mit meiner Xbox One herstellen?

Vergewissern Sie sich zunächst, dass der Entwicklermodus und nicht der Einzelhandelsmodus aktiviert ist. Im Einzelhandelsmodus können Sie keine Verbindung mit der Xbox One herstellen. Um herauszufinden, in welchem Modus Sie sich gerade befinden, drücken Sie auf dem Controller die **Start** Schaltfläche. Wenn Sie anstelle von dev Home Gold/Live-Inhalte sehen, befinden Sie sich im Einzelhandels Modus, und Sie müssen die Entwicklermodus-Aktivierungs-app ausführen, um in den Entwicklermodus zu wechseln.

> [!NOTE]
> Damit Sie eine App bereitstellen können, muss ein Benutzer angemeldet sein.

Weitere Informationen finden Sie unter [Beheben von Bereitstellungsfehlern](#fixing-deployment-failures) weiter unten auf dieser Seite.

### <a name="how-do-i-switch-between-retail-mode-and-developer-mode"></a>Wie wechsle ich zwischen Einzelhandels- und Entwicklermodus?

Folgen Sie den Anweisungen zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md), um mehr über diese Modi zu erfahren.

### <a name="how-do-i-know-if-i-am-in-retail-mode-or-developer-mode"></a>Woran erkenne ich, ob ich mich im Einzelhandels- oder im Entwicklermodus befinde?

Folgen Sie den Anweisungen zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md), um mehr über diese Modi zu erfahren. 

Um herauszufinden, in welchem Modus Sie sich gerade befinden, drücken Sie auf dem Controller die **Start** Schaltfläche. 
- Wenn Sie auf die Entwickler-Startseite gelangen, sind Sie im Entwicklermodus.
- Wenn Sie Gold/Live-Inhalte sehen, befinden Sie sich im Einzelhandel.

### <a name="will-my-games-and-apps-still-work-if-i-activate-developer-mode"></a>Funktionieren meine Spiele und Apps auch, wenn ich den Entwicklermodus aktiviere?

Ja, Sie können vom Entwicklermodus in den Einzelhandelsmodus wechseln, in dem Sie Ihre Spiele spielen können. Weitere Informationen finden Sie auf der Seite zur [Aktivierung des Xbox One-Entwicklermodus](devkit-activation.md). 

### <a name="can-i-develop-and-publish-x86-apps-for-xbox"></a>Kann ich x86-Apps für die Xbox entwickeln und veröffentlichen?
Xbox unterstützt nicht mehr die x86-App-Entwicklung oder x86-App-Übermittlungen im Store. 

### <a name="will-i-lose-my-games-and-apps-or-saved-changes"></a>Gehen meine Spiele und Apps oder gespeicherte Änderungen verloren?

Wenn Sie das Entwicklerprogramm verlassen, gehen dabei installierte Spiele und Apps nicht verloren. Auch wenn Sie online waren, als Sie diese abgespielt haben, werden Ihre gespeicherten Spiele alle in Ihrem Live Account Cloud-Profil gespeichert, sodass Sie Sie nicht verlieren.

### <a name="how-do-i-leave-the-developer-program"></a>Wie verlasse ich das Entwicklerprogramm?

Weitere Informationen dazu, wie Sie das Entwicklerprogramm verlassen, finden Sie im Thema zur [Deaktivierung des Xbox One-Entwicklermodus](devkit-deactivation.md).

### <a name="i-sold-my-xbox-one-and-left-it-in-developer-mode-how-do-i-deactivate-developer-mode"></a>Ich habe meine Xbox One verkauft und im Entwicklermodus belassen. Wie deaktiviere ich den Entwicklermodus?

Wenn Sie keinen Zugriff mehr auf Ihre Xbox One haben, können Sie Sie im Windows Partner Center deaktivieren. Weitere Informationen finden Sie im Abschnitt **Deaktivieren der Konsole mithilfe von Partner Center** des Themas [Xbox One Developer Mode Deactivation](devkit-deactivation.md#deactivate-your-console-using-partner-center) . 

### <a name="i-left-the-developer-program-using-partner-center-but-im-in-still-developer-mode-what-do-i-do"></a>Ich habe das Entwicklerprogramm mit Partner Center verlassen, bin aber im Entwicklermodus. Wie gehe ich vor?

Starten Sie Dev Home, und wählen Sie die Schaltfläche **Leave developer mode** aus. Dadurch wird die Konsole im Einzelhandelsmodus neu gestartet. 

### <a name="can-i-publish-my-app"></a>Kann ich meine App veröffentlichen?

Wenn Sie über ein [Entwicklerkonto](https://developer.microsoft.com/store/register)verfügen, können Sie Apps über Partner Center [veröffentlichen](../publish/index.md) . UWP-apps, die auf einer Einzel Konsole mit dem Einzelhandel erstellt und getestet werden, durchlaufen denselben Erfassungs-, Überprüfungs-und Veröffentlichungsprozess, den Windows heute durchführt, mit zusätzlichen Überprüfungen, die den heutigen Xbox One-Standards entsprechen.

### <a name="can-i-publish-my-game"></a>Kann ich mein Spiel veröffentlichen?

Sie können UWP und die Xbox One im Entwicklermodus verwenden, um Ihre Spiele auf der Xbox One zu erstellen und zu testen. Zum Veröffentlichen von UWP-spielen müssen Sie sich bei [ID@XBOX](https://www.xbox.com/Developers/id) dem [Xbox Live Creators-Programm](https://developer.microsoft.com/games/xbox/xboxlive/creator)registrieren oder Teil dieses Programms sein. Weitere Informationen finden Sie unter [Übersicht über das Entwicklerprogramm](https://developer.microsoft.com/games/xbox/docs/xboxlive/get-started/developer-program-overview.html).

### <a name="will-the-standard-game-engines-work"></a>Können die standardmäßigen Spielengines verwendet werden?

Informationen hierzu finden Sie auf der Seite [Bekannte Probleme](known-issues.md) für diese Version.

### <a name="what-capabilities-and-system-resources-are-available-to-uwp-games-on-xbox-one"></a>Welche Funktionen und Systemressourcen sind für UWP-Spiele auf Xbox One verfügbar? 

Informationen hierzu finden Sie unter [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md).

### <a name="if-i-create-a-directx-12-uwp-game-will-it-run-on-my-xbox-one-in-developer-mode"></a>Kann ein DirectX 12-UWP-Spiel, das ich erstellt habe, auf meiner Xbox One im Entwicklermodus ausgeführt werden?

Informationen hierzu finden Sie unter [Systemressourcen für UWP-Apps und -Spiele auf Xbox One](system-resource-allocation.md).

### <a name="will-the-entire-uwp-api-surface-be-available-on-xbox"></a>Wird die gesamte UWP-API-Oberfläche auf Xbox verfügbar sein?

Informationen hierzu finden Sie auf der Seite [Bekannte Probleme](known-issues.md) für diese Version.

### <a name="fixing-deployment-failures"></a>Beheben von Bereitstellungsfehlern

Wenn Sie Ihre APP nicht aus Visual Studio bereitstellen können, können Sie diese Schritte möglicherweise unterstützen, um das Problem zu beheben. Wenn Sie nicht weiterkommen, bitten Sie im Forum um Hilfe.

> [!NOTE]
> Damit Sie eine App bereitstellen können, muss ein Benutzer angemeldet sein. Wenn Sie die Fehlermeldung 0x87e10008 erhalten, vergewissern Sie sich, dass ein Benutzer angemeldet ist, und versuchen Sie es noch einmal.

Wenn Visual Studio keine Verbindung mit Ihrer Xbox One herstellen kann:

1. Stellen Sie sicher, dass Sie sich im Entwicklermodus befinden (wie weiter oben auf dieser Seite erläutert).
2. Vergewissern Sie sich, dass der Entwicklungscomputer richtig eingerichtet wurde. Haben Sie *alle* Anweisungen in [Erste Schritte bei der Entwicklung von UWP-Apps auf Xbox One](getting-started.md) befolgt? 

3. Wenn Sie dies noch nicht getan haben, lesen Sie das Thema [Einrichten der Entwicklungsumgebung](development-environment-setup.md) und das Thema [Einführung in Xbox One-Tools](introduction-to-xbox-tools.md) .

4. Vergewissern Sie sich, dass Sie von Ihrem Entwicklungscomputer aus einen Pingbefehl an die IP-Adresse Ihrer Konsole senden können.
  > [!NOTE]
  > Es wird empfohlen, eine Kabelverbindung zu Ihrer Konsole zu verwenden, um die bestmögliche Leistung bei der Bereitstellung zu erzielen.

5. Stellen Sie sicher, dass Sie das universelle (unverschlüsselte Protokoll) in der Dropdown Liste Authentifizierung auf der Registerkarte **Debuggen** verwenden. Weitere Informationen finden Sie unter [Setup für die Entwicklungsumgebung](development-environment-setup.md).


### <a name="if-im-building-an-app-using-htmljavascript-how-do-i-enable-gamepad-navigation"></a>Wie aktiviere ich die Gamepad-Navigation, wenn ich eine App mithilfe von HTML/JavaScript erstelle?

TVHelpers ist ein Satz von JavaScript- und XAML-/C#-Beispielen und -Bibliotheken, mit denen Sie großartige Xbox One-Inhalte und TV-Inhalte in JavaScript und C# erstellen können. TVJS ist eine Bibliothek, mit der Sie Premium-UWP-Apps für Xbox One erstellen können. TVJS bietet u. a. Unterstützung für die automatische Controllernavigation, Rich-Media-Wiedergabe und Suche. Sie können TVJS mit einer gehosteten Web-App genauso einfach wie mit einer gepackten UWP-Web-App mit vollständigem Zugriff auf Windows-Runtime-APIs verwenden.

Weitere Informationen finden Sie unter dem [TVHelpers](https://github.com/Microsoft/TVHelpers) tvhilfsprojekt und dem Projekt- [wiki](https://github.com/Microsoft/TVHelpers/wiki).

## <a name="see-also"></a>Weitere Informationen
- [Bekannte Probleme mit UWP auf Xbox One](known-issues.md)
- [UWP auf Xbox One](index.md)
- [UWP auf Xbox One](index.md)
