---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Aktivieren Ihres Geräts für die Entwicklung
description: Konfigurieren Sie Ihr Windows 10-Gerät für die Entwicklung und das Debugging.
keywords: Erste Schritte Entwicklerlizenz Visual Studio, Entwicklerlizenz Gerät aktivieren
ms.date: 04/09/2019
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 9c1979f8e8232ee3bfd2e2961307608bf8da7836
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714150"
---
# <a name="enable-your-device-for-development"></a>Aktivieren Ihres Geräts für die Entwicklung

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>Aktivieren des Entwicklermodus, Querladen von Apps und Zugreifen auf andere Entwicklerfeatures

![Aktivieren deiner Geräte für die Entwicklung](images/developer-poster.png)

Wenn du deinen Computer für alltägliche Aktivitäten wie Spiele, Surfen im Web, E-Mails oder Office-Apps verwendest, musst du den Entwicklermodus *nicht* aktivieren. (In diesem Fall raten wir sogar davon ab.) Die restlichen Informationen auf dieser Seite sind dann für dich nicht relevant, und du kannst einfach mit dem fortfahren, womit du dich gerade beschäftigt hast. Danke für deinen Besuch!

Wenn du allerdings auf einem Computer zum ersten Mal Software mit Visual Studio schreibst, *musst* du den Entwicklermodus sowohl auf dem Entwicklungs-PC als auch auf allen Geräten aktivieren, die du zum Testen des Codes verwenden möchtest. Ist der Entwicklermodus beim Öffnen eines UWP-Projekts nicht aktiviert, wird entweder die Einstellungsseite **Für Entwickler** geöffnet oder das folgende Dialogfeld in Visual Studio angezeigt:

![Dialogfeld zum Aktivieren des Entwicklermodus in Visual Studio](images/latestenabledialog.png)

Sollte dieses Dialogfeld angezeigt werden, klicke auf **Für Entwickler-Einstellungen**, um die Einstellungsseite **Für Entwickler** zu öffnen.

> [!NOTE]
> Du kannst jederzeit zur Seite **Für Entwickler** navigieren, um den Entwicklermodus zu aktivieren oder zu deaktivieren. Gibt dazu einfach auf der Taskleiste im Cortana-Suchfeld „für Entwickler“ ein.

## <a name="accessing-settings-for-developers"></a>Zugreifen auf die Einstellungen für Entwickler

Gehe wie folgt vor, um den Entwicklermodus zu aktivieren oder auf andere Einstellungen zuzugreifen:

1.  Wähle im Dialogfeld **Für Entwickler** die gewünschte Zugriffsebene aus.
2.  Lesen Sie den Haftungsausschluss für die ausgewählte Einstellung, und klicken Sie dann auf **Ja** um die Änderung zu übernehmen.

> [!NOTE]
> Zum Aktivieren des Entwicklermodus benötigst du Administratorzugriff. Falls dein Gerät einer Organisation gehört, ist diese Option möglicherweise deaktiviert.

So sieht die Einstellungsseite auf Desktopgeräten aus:

![Navigieren Sie zu „Einstellungen > Update und Sicherheit“, und wählen Sie „Für Entwickler“ aus, um Ihre Optionen anzuzeigen.](images/devmode-pc-options.png)

## <a name="which-setting-should-i-choose-sideload-apps-or-developer-mode"></a>Welche Einstellung soll ich auswählen: Querladen von Apps oder Entwicklermodus?

 Sie können ein Gerät für die Entwicklung oder nur für das Querladen aktivieren.

-   *Microsoft Store-Apps* ist die Standardeinstellung. Lass diese Einstellung aktiviert, wenn du keine Apps entwickelst oder spezielle interne Apps verwendest, die von deinem Unternehmen bereitgestellt wurden.
-   *Querladen* dient zum Installieren und anschließenden Ausführen oder Testen einer App, die nicht vom Microsoft Store zertifiziert wurde. Hierzu zählen beispielsweise interne Unternehmens-Apps.
-   Im *Entwicklermodus* können Sie Apps querladen und Apps aus Visual Studio im Debugmodus ausführen.

Standardmäßig können nur UWP-Apps (Universelle Windows-Plattform) aus dem Microsoft Store installiert werden. Wenn Sie diese Einstellungen ändern, um die Entwicklerfeatures zu verwenden, kann hierdurch die Sicherheitsstufe Ihres Geräts geändert werden. Sie sollten keine Apps aus nicht überprüften Quellen installieren.

### <a name="sideload-apps"></a>Querladen von Apps

Die Einstellung für das Querladen von Apps wird normalerweise von Unternehmen oder Bildungseinrichtungen verwendet, die benutzerdefinierte Apps auf verwalteten Geräten installieren müssen, ohne den Microsoft Store zu nutzen, oder von anderen Benutzern, die Apps aus Microsoft-fremden Quellen ausführen müssen. In diesem Fall erzwingt die Organisation häufig eine Richtlinie, um die Einstellung *UWP-Apps* zu deaktivieren, wie oben in der Abbildung der Einstellungsseite gezeigt. Die Organisation stellt außerdem das erforderliche Zertifikat und den Installationsspeicherort zum Querladen von Apps bereit. Weitere Informationen finden Sie in den TechNet-Artikeln [Querladen von Branchen-Apps in Windows 10](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10) und [Erste Schritte mit der Bereitstellung von Apps in Microsoft Intune](https://docs.microsoft.com/intune/deploy-use/add-apps).

Spezifische Informationen für Gerätefamilien

-   Desktopgeräte: Du kannst ein App-Paket (APPX-Datei) sowie sämtliche Zertifikate, die zum Ausführen der App benötigt werden, mithilfe des Windows PowerShell-Skripts installieren, das mit dem Paket erstellt wird („Add-AppDevPackage.ps1“). Weitere Informationen finden Sie unter [Verpacken von UWP-Apps](../packaging/packaging-uwp-apps.md).

-   Mobilgeräte: Wenn das erforderliche Zertifikat bereits installiert ist, kannst du auf die Datei tippen, um alle APPX-Dateien zu installieren, die du per E-Mail oder auf einer SD-Karte erhalten hast.


Das **Querladen von Apps** ist eine sicherere Option als der Entwicklermodus, da Sie Apps ohne vertrauenswürdiges Zertifikat nicht auf dem Gerät installieren können.

> [!NOTE]
> Achten Sie beim Querladen von Apps darauf, dass diese von einer vertrauenswürdigen Quelle stammen. Wenn du eine quergeladene, nicht vom Microsoft Store zertifizierte App installierst, bestätigst du, dass du über alle erforderlichen Rechte zum Querladen dieser App verfügst und die alleinige Verantwortung für jegliche Schäden trägst, die durch das Installieren und Ausführen dieser App entstehen können. Weitere Informationen findest du in den [Datenschutzbestimmungen](https://go.microsoft.com/fwlink/?LinkId=521839) im Abschnitt „Windows“ &gt; „Microsoft Store“.


### <a name="developer-mode"></a>Entwicklermodus

Der Entwicklermodus ersetzt die Windows 8.1-Anforderungen durch eine Entwicklerlizenz.  Neben dem Querladen bietet der Entwicklermodus Debug- und zusätzliche Bereitstellungsoptionen. Hierzu gehört das Starten eines SSH-Diensts, der Bereitstellungen auf diesem Gerät ermöglicht. Um den Dienst zu beenden, müssen Sie den Entwicklermodus deaktivieren.

Wenn du den Entwicklermodus auf dem Desktop aktivierst, wird ein Featurepaket installiert, das Folgendes umfasst:
- Windows-Geräteportal: Nur wenn die Option **Geräteportal aktivieren** aktiviert ist, ist das Geräteportal aktiviert und Firewallregeln werden für es konfiguriert.
- Installiert und konfiguriert Firewallregeln für SSH-Dienste, die eine Remoteinstallation von Apps ermöglichen. Durch Aktivieren der **Gerätesuche** wird der SSH-Server aktiviert.


## <a name="additional-developer-mode-features"></a>Weitere Features im Entwicklermodus

Für jede Gerätefamilie können zusätzliche Entwicklerfeatures verfügbar sein. Diese Features sind nur verfügbar, wenn der Entwicklermodus auf dem Gerät aktiviert ist, und können sich abhängig von der verwendeten Betriebssystemversion unterscheiden.

Die folgende Abbildung zeigt Entwicklerfeatures für Windows 10:

![Optionen des Entwicklermodus](images/devmode-mob-options.png)

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Geräteportal

Weitere Informationen zum Geräteportal findest du in der [Übersicht über das Windows-Geräteportal](../debug-test-perf/device-portal.md).

Gerätespezifische Anweisungen zum Einrichten finden Sie in folgenden Artikeln:
- [Geräteportal für Windows-Desktop](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [Geräteportal für HoloLens](https://developer.microsoft.com/mixed-reality)
- [Geräteportal für IoT](https://developer.microsoft.com/windows/iot/docs/DevicePortal)
- [Geräteportal für Mobilgeräte](../debug-test-perf/device-portal-mobile.md)
- [Geräteportal für Xbox](../xbox-apps/device-portal-xbox.md)

Solltest du Probleme beim Aktivieren des Entwicklermodus oder des Geräteportals haben, findest du im Forum [Bekannte Probleme](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) entsprechende Problemumgehungen. Alternativ findest du unter [Fehler beim Installieren des Entwicklermoduspakets](#failure-to-install-developer-mode-package) zusätzliche Details sowie Informationen dazu, welche WSUS-KBs zugelassen werden müssen, um das Entwicklermoduspaket zu entsperren.

### <a name="ssh"></a>SSH

SSH-Dienste werden aktiviert, wenn du auf deinem Gerät die Gerätesuche aktivierst.  Dieses Feature wird verwendet, wenn es sich bei deinem Gerät um ein Remotebereitstellungsziel für UWP-Anwendungen handelt.   Die Namen der Dienste sind „SSH Server Broker“ und „SSH Server Proxy“.

> [!NOTE]
> Dies ist nicht die OpenSSH-Implementierung von Microsoft, die Sie auf [GitHub](https://github.com/PowerShell/Win32-OpenSSH) finden.  

Wenn Sie die SSH-Dienste nutzen möchten, können Sie die Gerätesuche aktivieren, um eine Pin-Kopplung zu ermöglichen. Wenn ein anderer SSH-Dienst ausgeführt werden soll, können Sie diesen auf einem anderen Anschluss einrichten oder die SSH-Dienste im Entwicklermodus deaktivieren. Zum Deaktivieren der SSH-Dienste muss die Gerätesuche deaktiviert werden.  

Die SSH-Anmeldung erfolgt über das Konto „DevToolsUser“, das ein Kennwort zur Authentifizierung akzeptiert.  Das Kennwort ist die PIN, die auf dem Gerät nach Betätigung der Schaltfläche „Koppeln“ für die Gerätesuche angezeigt wird. Es ist nur gültig, solange die PIN angezeigt wird.  Darüber hinaus wird ein SFTP-Subsystem für die manuelle Verwaltung des Ordners „DevelopmentFiles“ aktiviert, in dem lose Dateibereitstellungen aus Visual Studio installiert werden.

#### <a name="caveats-for-ssh-usage"></a>Tipps für die SSH-Verwendung
Da der vorhandene SSH-Server in Windows noch nicht mit dem Protokoll kompatibel ist, ist für die Verwendung eines SFTP- oder SSH-Clients möglicherweise eine spezielle Konfiguration erforderlich.  Das SFTP-Subsystem wird mit der Version 3 oder einer älteren Version betrieben, weshalb jeder Client, der eine Verbindung herstellen möchte, für einen älteren Server konfiguriert werden muss.  Auf älteren Geräten verwendet der SSH-Server `ssh-dss` für die Authentifizierung mit öffentlichem Schlüssel, was unter OpenSSH veraltet ist.  Für die Verbindungsherstellung mit solchen Geräten muss der SSH-Client manuell konfiguriert werden, um `ssh-dss` zu akzeptieren.  

### <a name="device-discovery"></a>Gerätesuche

Wenn Sie die Gerätesuche aktivieren, ist Ihr Gerät über mDNS für andere Geräte im Netzwerk sichtbar.  Dieses Feature ermöglicht auch das Abrufen der SSH-PIN zur Kopplung mit dem Gerät durch Betätigen der Schaltfläche „Koppeln“, die nach Aktivierung der Gerätesuche verfügbar gemacht wird.  Die folgende PIN-Eingabeaufforderung muss auf dem Bildschirm angezeigt werden, um die erste Bereitstellung von Visual Studio für das Gerät abzuschließen:  

![PIN-Kopplung](images/devmode-pc-pinpair.PNG)

Die Gerätesuche sollte nur aktiviert werden, wenn das Gerät ein Bereitstellungsziel sein soll. Wenn beispielsweise eine App über das Geräteportal zum Testen auf einem Smartphone bereitgestellt wird, müssen Sie die Gerätesuche auf dem Smartphone, jedoch nicht auf Ihrem Entwicklungscomputer aktivieren.

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Optimierungen für Windows-Explorer, Remotedesktop und PowerShell (nur Desktop)

 Auf der Desktopgerätefamilie finden Sie auf der Einstellungsseite **Für Entwickler** Verknüpfungen zu den Einstellungen, die Sie zum Optimieren Ihres PCs für Entwicklungsaufgaben verwenden können. Für jede Einstellung können Sie das Kontrollkästchen aktivieren und auf **Übernehmen** klicken, oder Sie können auf den Link **Einstellungen anzeigen** klicken, um die Einstellungsseite für diese Option zu öffnen.


## <a name="notes"></a>Anmerkungen
In früheren Versionen von Windows 10 Mobile enthielt das Menü „Entwicklereinstellungen“ die Option „Absturzabbilder”.  Diese Option wurde in das [Geräteportal](../debug-test-perf/device-portal.md) verschoben, damit sie nicht nur über USB, sondern auch per Remotezugriff verwendet werden kann.  

Es gibt verschiedene Tools, mit denen du eine App von einem Windows 10-PC aus auf einem Windows 10-Gerät bereitstellen kannst. Beide Geräte müssen über eine kabelgebundene oder drahtlose Verbindung mit dem gleichen Subnetz des Netzwerks verbunden sein oder über USB verbunden werden. Bei beiden Methoden wird lediglich das App-Paket (APPX-/APPXBUNDLE-Datei) installiert. Zertifikate werden nicht installiert.

-   Verwenden Sie das Tool für die Windows 10-Anwendungsbereitstellung (WinAppDeployCmd). Weitere Informationen zum [Tool WinAppDeployCmd](https://docs.microsoft.com/previous-versions/windows/apps/mt203806(v=vs.140)).
-   Über das [Geräteportal](../debug-test-perf/device-portal.md) kannst du von deinem Browser aus Bereitstellungen auf einem mobilen Gerät mit Windows 10 (ab Version 1511) durchführen. Im Geräteportal können Sie auf der Seite **[Apps](../debug-test-perf/device-portal.md#apps-manager)** ein App-Paket (APPX) hochladen und auf dem Gerät installieren.

## <a name="failure-to-install-developer-mode-package"></a>Fehler beim Installieren des Entwicklermoduspakets
Es kann vorkommen, dass der Entwicklermodus aufgrund von Netzwerk- oder Verwaltungsproblemen nicht ordnungsgemäß installiert wird. Das Entwicklermoduspaket ist für die **Remotebereitstellung** auf diesem PC (entweder über das Geräteportal von einem Browser aus oder unter Verwendung der Gerätesuche, um SSH zu aktivieren) erforderlich. Für die lokale Entwicklung wird es dagegen nicht benötigt.  Sollten diese Probleme auftreten, kannst du deine App immer noch mithilfe von Visual Studio oder von diesem Gerät aus auf einem anderen Gerät bereitstellen.

Im Forum zu den [bekannten Problemen](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) finden Sie entsprechende Problemumgehungen und vieles mehr.

> [!NOTE]
> Falls der Entwicklermodus nicht ordnungsgemäß installiert wird, kannst du uns Feedback senden. Wähle in der App **Feedback-Hub** die Option **Neues Feedback hinzufügen** sowie die Kategorie **Entwicklerplattform** und die Unterkategorie **Entwicklermodus** aus. Die Übermittlung von Feedback hilft Microsoft bei der Behebung des aufgetretenen Problems.

### <a name="failed-to-locate-the-package"></a>Das Paket konnte nicht gefunden werden

"Developer Mode package couldn’t be located in Windows Update. Fehlercode: 0x80004005. Weitere Informationen“   

Dieser Fehler kann aufgrund eines Netzwerkverbindungsproblems, aufgrund von Enterprise-Einstellungen oder weil das Paket nicht vorhanden ist, auftreten.

So beheben Sie dieses Problem:

1. Stellen Sie sicher, dass Ihr Computer mit dem Internet verbunden ist.
2. Wenn Sie einen in eine Domäne eingebundenen Computer verwenden, sprechen Sie mit dem Netzwerkadministrator. Das Entwicklermoduspaket ist standardmäßig in WSUS blockiert (genau wie alle anderen Features bei Bedarf).
2.1. Um das Entwicklermoduspaket in der aktuellen Version sowie in Vorgängerversionen zu entsperren, müssen die folgenden KBs in WSUS zugelassen werden: 4016509, 3180030, 3197985  
3. Suche unter „Einstellungen“ > „Updates und Sicherheit“ > „Windows-Updates“ nach Windows-Updates.
4. Vergewissere dich unter „Einstellungen“ > „System“ > „Apps und Features“ > „Optionale Features verwalten“ > „Feature hinzufügen“, dass das Windows-Entwicklermoduspaket vorhanden ist. Wenn es nicht vorhanden ist, kann Windows das richtige Paket für Ihren Computer nicht finden.

Nachdem Sie einen der oben genannten Schritte durchgeführt haben, deaktivieren Sie den Entwicklermodus, und aktivieren Sie ihn dann erneut, um die Korrektur zu überprüfen.


### <a name="failed-to-install-the-package"></a>Fehler beim Installieren des Pakets

"Developer Mode package failed to install. Fehlercode: 0x80004005. Weitere Informationen“

Dieser Fehler kann aufgrund von Inkompatibilitäten zwischen dem Build von Windows und dem Entwicklermoduspaket auftreten.

So beheben Sie dieses Problem:

1. Suche unter „Einstellungen“ > „Updates und Sicherheit“ > „Windows-Updates“ nach Windows-Updates.
2. Starten Sie Ihren Computer neu, um sicherzustellen, dass alle Updates angewendet wurden.


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>Verwenden von Gruppenrichtlinien oder Registrierungsschlüsseln zum Aktivieren von Geräten

Entwickler sollten meist die Einstellungs-Apps verwenden, um Geräte für das Debuggen zu aktivieren. In bestimmten Szenarien wie z. B. bei automatisierten Tests stehen weitere Möglichkeiten zum Aktivieren Ihres Windows 10-Desktopgeräts für die Entwicklung zur Verfügung.  Beachte, dass diese Schritte nicht dazu führen, dass der SSH-Server aktiviert wird oder das Gerät als Remotebereitstellungs- und Debuggingziel verwendet werden kann.

Sie können mithilfe von „gpedit.msc“ die Gruppenrichtlinien für die Geräteaktivierung festlegen (es sei denn, Sie verwenden Windows 10 Home). In Windows 10 Home müssen die Registrierungsschlüssel für die Geräteaktivierung direkt mithilfe von regedit oder von PowerShell-Befehlen festgelegt werden.

**Aktivieren des Geräts mithilfe von „gpedit“**

1.  Führen Sie **Gpedit.msc** aus.
2.  Navigieren Sie zu „Richtlinie für "Lokaler Computer" &gt; Computerkonfiguration &gt; Administrative Vorlagen &gt; Windows-Komponenten &gt; App-Paketbereitstellung.
3.  Bearbeiten Sie zum Aktivieren des Querladens die folgenden Richtlinien:

    -   **Installation aller vertrauenswürdigen Apps zulassen**

    - ODER

    Bearbeiten Sie zum Aktivieren des Entwicklermodus die Richtlinien, um beide zu aktivieren:

    -   **Installation aller vertrauenswürdigen Apps zulassen**
    -   **Entwicklung von UWP-Apps sowie die Installation aus einer integrierten Entwicklungsumgebung (IDE) zulassen**

4.  Starten Sie Ihren Computer neu.

**Aktivieren des Geräts mithilfe von „regedit“**

1.  Führen Sie **regedit** aus.
2.  Legen Sie diesen DWORD-Wert auf 1 fest, um das Querladen zu aktivieren.

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - ODER

    Um den Entwicklermodus zu aktivieren, legen Sie diese DWORD-Werte auf 1 fest:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**Aktivieren des Geräts mithilfe von PowerShell**

1.  Führen Sie PowerShell mit Administratorrechten aus.
2.  Führen Sie zum Aktivieren des Querladens diesen Befehl aus:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - ODER

    Führen Sie zum Aktivieren des Entwicklermodus diesen Befehl aus:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowDevelopmentWithoutDevLicense" /d "1"**

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Aktualisieren Ihres Geräts von Windows 8.1 auf Windows 10

Wenn Sie Apps auf Ihrem Windows 8.1-Gerät erstellen oder querladen, müssen Sie eine Entwicklerlizenz installieren. Beim Upgrade Ihres Geräts von Windows 8.1 auf Windows 10 bleiben diese Informationen erhalten. Führen Sie den folgenden Befehl aus, um diese Informationen von Ihrem aktualisierten Windows 10-Gerät zu entfernen. Dieser Schritt ist nicht erforderlich, wenn Sie ein Upgrade von Windows 8.1 direkt auf Windows 10, Version 1511 oder höher, ausführen.

**So hebst du die Registrierung einer Entwicklerlizenz auf:**

1.  Führen Sie PowerShell mit Administratorrechten aus.
2.  Führen Sie folgenden Befehl aus: **unregister-windowsdeveloperlicense**.

Danach müssen Sie Ihr Gerät wie in diesem Thema beschrieben für die Entwicklung aktivieren, damit Sie weiterhin auf diesem Gerät entwickeln können. Andernfalls erhalten Sie möglicherweise eine Fehlermeldung, wenn Sie Ihre App debuggen oder versuchen, ein Paket dafür zu erstellen. Hier ist ein Beispiel für diesen Fehler:

Fehler: DEP0700: Registrierung der App fehlgeschlagen.

## <a name="see-also"></a>Weitere Informationen

* [Deine erste App](your-first-app.md)
* [Veröffentlichen deiner UWP-App](https://docs.microsoft.com/windows/uwp/publish/)
* [Anleitungen zur Entwicklung von UWP App](https://docs.microsoft.com/windows/uwp/develop/)
* [Codebeispiele für UWP-Entwickler](https://developer.microsoft.com/windows/samples)
* [Was ist eine UWP-App?](universal-application-platform-guide.md)
* [Registrieren für ein Windows-Konto](sign-up.md)
