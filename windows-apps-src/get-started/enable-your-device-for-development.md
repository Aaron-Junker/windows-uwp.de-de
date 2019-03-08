---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Aktivieren Ihres Geräts für die Entwicklung
description: Konfigurieren Sie Ihr Windows 10-Gerät für die Entwicklung und das Debugging.
keywords: Erste Schritte Entwicklerlizenz Visual Studio, Entwicklerlizenz Gerät aktivieren
ms.date: 05/30/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 90ff8e8c0d11e58d251e9c2f5bfb00f3b8aedf1a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57618955"
---
# <a name="enable-your-device-for-development"></a>Aktivieren Ihres Geräts für die Entwicklung

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>Aktivieren Sie den Entwicklermodus, laden Sie Apps quer und greifen Sie auf andere Entwicklerfeatures zu

![Aktivieren Sie Ihre Geräte für die Entwicklung](images/developer-poster.png)

Wenn Sie Ihren Computer für normale tägliche Aktivitäten wie Spiele, Surfen im Web, E-Mail oder Office-Apps verwenden, müssen und sollten Sie den Entwicklermodus *nicht* aktivieren. Die restlichen Informationen auf dieser Seite sind nicht von Bedeutung und Sie können beruhigt Ihre Arbeit fortsetzen. Vielen Dank für Ihren Besuch!

Wenn Sie allerdings Software schreiben und dazu Visual Studio zum ersten Mal auf einem Computer verwenden, *müssen* Sie den Entwicklermodus auf dem Entwicklungs-PC und auf allen Geräten aktivieren, die Sie zum Testen des Codes verwenden. Wenn Sie ein UWP-Projekt öffnen, während der Entwicklermodus nicht aktiviert ist, wird entweder die Seite mit den Einstellungen **Für Entwickler** oder dieses Dialogfeld in Visual Studio angezeigt:

![Dialogfeld zum Aktivieren des Entwicklermodus in Visual Studio](images/latestenabledialog.png)

Wenn dieses Dialogfeld angezeigt wird, klicken Sie auf **Einstellungen für Entwickler**, um die Einstellungsseite **Für Entwickler** zu öffnen.

> [!NOTE]
> Sie können jederzeit zur Seite **Für Entwickler** wechseln, um den Entwicklermodus zu aktivieren oder zu deaktivieren: Geben Sie einfach in der Taskleiste im Cortana-Suchfeld „für Entwickler“ ein.

## <a name="accessing-settings-for-developers"></a>Zugriff auf Entwicklereinstellungen

So aktivieren Sie den Entwicklermodus und greifen auf andere Einstellungen zu:

1.  Wählen Sie im Dialogfeld **Für Entwickler** die Zugriffsebene aus, die Sie benötigen.
2.  Lesen Sie den Haftungsausschluss für die ausgewählte Einstellung, und klicken Sie dann auf **Ja** um die Änderung zu übernehmen.

> [!NOTE]
> Für das Aktivieren des Entwicklermodus ist Administratorzugriff erforderlich. Wenn Ihr Gerät einem Unternehmen gehört, ist diese Option möglicherweise von Ihrer Organisation deaktiviert.

Hier sehen Sie die Einstellungsseite auf Desktopgeräten:

![Navigieren Sie zu „Einstellungen > Update und Sicherheit“, und wählen Sie „Für Entwickler“ aus, um Ihre Optionen anzuzeigen.](images/devmode-pc-options.png)

## <a name="which-setting-should-i-choose-sideload-apps-or-developer-mode"></a>Welche Einstellung soll ich auswählen: Querladen von Apps oder Entwicklermodus?

 Sie können ein Gerät für die Entwicklung oder nur für das Querladen aktivieren.

-   *Microsoft Store-apps* ist die Standardeinstellung. Wenn Sie keine Apps entwickeln oder spezielle interne Apps verwenden, die von Ihrem Unternehmen ausgestellt sind, sollten Sie diese Einstellung aktiviert lassen.
-   *Querladen* dient zum Installieren und Ausführen oder Testen einer App, die nicht vom Microsoft Store zertifiziert wurde. Hierzu zählen beispielsweise interne Unternehmens-Apps.
-   Im *Entwicklermodus* können Sie Apps querladen und Apps aus Visual Studio im Debugmodus ausführen.

Standardmäßig können Sie nur UWP-Apps (Universelle Windows-Plattform) aus dem Microsoft Store installieren. Wenn Sie diese Einstellungen ändern, um die Entwicklerfeatures zu verwenden, kann hierdurch die Sicherheitsstufe Ihres Geräts geändert werden. Sie sollten keine Apps aus nicht überprüften Quellen installieren.

### <a name="sideload-apps"></a>Querladen von Apps

Das Querladen von apps festlegen wird in der Regel von Unternehmen oder Schulen, die benutzerdefinierte apps auf verwalteten Geräten zu installieren, ohne Umweg über den Microsoft Store müssen oder Personen andere, die zum Ausführen von apps aus nicht-Microsoft-Quellen verwendet. In diesem Fall wird im Unternehmen häufig eine Richtlinie erzwungen, mit der die Einstellung *UWP-Apps* deaktiviert wird, wie oben in der Abbildung der Einstellungsseite dargestellt. Die Organisation stellt außerdem das erforderliche Zertifikat und den Installationsspeicherort zum Querladen von Apps bereit. Weitere Informationen finden Sie in den TechNet-Artikeln [Querladen von Branchen-Apps in Windows 10](https://technet.microsoft.com/library/mt269549.aspx) und [Erste Schritte mit der Bereitstellung von Apps in Microsoft Intune](https://technet.microsoft.com/library/dn646955.aspx).

Spezifische Informationen für Gerätefamilien

-   Desktopgeräte: Sie können ein app-Paket (.appx) installieren, und alle Zertifikate, die benötigt werden, um die app auszuführen, indem Sie dem Windows PowerShell-Skript ausführen, wird mit dem Paket ("Add-appdevpackage. ps1") erstellt. Weitere Informationen finden Sie unter [Verpacken von UWP-Apps](../packaging/packaging-uwp-apps.md).

-   Mobilgeräte: Wenn das erforderliche Zertifikat bereits installiert ist, können Sie die Datei, um alle an Sie gesendet werden, per e-Mail oder auf einer SD-Karte AppX-Datei installieren tippen.


Das **Querladen von Apps** ist eine sicherere Option als der Entwicklermodus, da Sie Apps ohne vertrauenswürdiges Zertifikat nicht auf dem Gerät installieren können.

> [!NOTE]
> Achten Sie beim Querladen von Apps darauf, dass diese von einer vertrauenswürdigen Quelle stammen. Wenn Sie eine quergeladene, nicht vom Microsoft Store zertifizierte App installieren, bestätigen Sie, dass Sie über alle erforderlichen Rechte zum Querladen dieser App verfügen und die alleinige Verantwortung für jegliche Schäden tragen, die durch das Installieren und Ausführen dieser App entstehen können. Weitere Informationen finden Sie in diesen [Datenschutzbestimmungen](https://go.microsoft.com/fwlink/?LinkId=521839) im Abschnitt zu Windows &gt; „Microsoft Store“.


### <a name="developer-mode"></a>Entwicklermodus

Der Entwicklermodus ersetzt die Windows 8.1-Anforderungen durch eine Entwicklerlizenz.  Neben dem Querladen bietet der Entwicklermodus Debug- und zusätzliche Bereitstellungsoptionen. Hierzu gehört das Starten eines SSH-Diensts, der Bereitstellungen auf diesem Gerät ermöglicht. Um den Dienst zu beenden, müssen Sie den Entwicklermodus deaktivieren.

Wenn Sie den Entwicklermodus auf dem Desktop aktivieren, wird ein Paket mit Funktionen installiert, das Folgendes enthält:
- Windows-Geräteportal. Nur wenn die Option **Geräteportal aktivieren** aktiviert ist, ist das Geräteportal aktiviert und Firewallregeln werden für es konfiguriert.
- Installiert und konfiguriert die Firewallregeln für SSH-Dienste, die eine Remote-Installation von Apps ermöglichen. Das Aktivieren der **Gerätesuche** aktiviert den SSH-Server.


## <a name="additional-developer-mode-features"></a>Weitere Features im Entwicklermodus

Für jede Gerätefamilie können zusätzliche Entwicklerfeatures verfügbar sein. Diese Features sind nur verfügbar, wenn der Entwicklermodus auf dem Gerät aktiviert ist, und können sich abhängig von der verwendeten Betriebssystemversion unterscheiden.

Diese Abbildung zeigt Entwicklerfeatures für Windows 10:

![Optionen des Entwicklermodus](images/devmode-mob-options.png)

### <a name="span-iddevice-discovery-and-pairingspandevice-portal"></a><span id="device-discovery-and-pairing"></span>Device-Portal

Weitere Informationen zum Device Portal finden Sie in der [Übersicht über das Windows-Geräteportal](../debug-test-perf/device-portal.md).

Gerätespezifische Anweisungen zum Einrichten finden Sie in folgenden Artikeln:
- [Für Desktop-Geräteportal](https://msdn.microsoft.com/windows/uwp/debug-test-perf/device-portal-desktop)
- [Geräteportal für HoloLens](https://developer.microsoft.com/windows/holographic/using_the_windows_device_portal)
- [Für IoT-Geräteportal](https://developer.microsoft.com/windows/iot/docs/DevicePortal)
- [Device-Portal für mobile Geräte](../debug-test-perf/device-portal-mobile.md)
- [Geräteportal für Xbox](../debug-test-perf/device-portal-xbox.md)

Wenn Sie Probleme beim Aktivieren des Entwicklermodus oder des Geräteportals haben, finden Sie im Forum [Bekannte Probleme](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) Problemumgehungen für diese Probleme. Suchen Sie andernfalls unter [Fehler beim Installieren des Entwicklermoduspakets](#failure-to-install-developer-mode-package) nach zusätzlichen Detail und welche WSUS KBs zugelassen werden, um das Paket im Entwicklermodus zu erlauben oder zu entsperren.

### <a name="ssh"></a>SSH

Wenn Sie die Gerätesuche auf Ihrem Gerät aktivieren, sind SSH-Dienste aktiviert.  Diese werden verwendet, wenn Ihr Gerät ein Remote-Bereitstellungsziel für UWP-Anwendungen ist.   Die Namen der Dienste sind „SSH Server Broker“ und „SSH Server Proxy“.

> [!NOTE]
> Dies ist nicht die OpenSSH-Implementierung von Microsoft, die Sie auf [GitHub](https://github.com/PowerShell/Win32-OpenSSH) finden.  

Wenn Sie die SSH-Dienste nutzen möchten, können Sie die Gerätesuche aktivieren, um eine Pin-Kopplung zu ermöglichen. Wenn ein anderer SSH-Dienst ausgeführt werden soll, können Sie diesen auf einem anderen Anschluss einrichten oder die SSH-Dienste im Entwicklermodus deaktivieren. Um die SSH-Dienste zu deaktivieren, deaktivieren Sie die Gerätesuche.  

Die SSH-Anmeldung erfolgt über das Konto "DevToolsUser", das ein Kennwort zur Authentifizierung akzeptiert.  Das Kennwort ist die PIN, die auf dem Gerät nach dem Drücken der Schaltfläche "Koppeln" unter Gerätesuche angezeigt wird und nur gültig ist, solange die PIN angezeigt wird.  Es wird ebenfalls ein SFTP-Subsystem für die manuelle Verwaltung des Ordners "developmentfiles" aktiviert, in dem die Bereitstellung loser Dateien von Visual Studio installiert sind.

#### <a name="caveats-for-ssh-usage"></a>Tipps für die SSH-Verwendung
Der vorhandene SSH-Server in Windows ist noch nicht mit dem Protokoll kompatibel, daher ist für die Verwendung eines SFTP- oder SSH-Clients möglicherweise eine spezielle Konfiguration erforderlich.  Das SFTP-Subsystem führt eine Version 3 oder älter aus, daher sollte jeder damit verbundene Client für einen älteren Server konfiguriert werden.  Der SSH-Server bei älteren Geräten verwendet `ssh-dss`für die Authentifizierung des öffentlichen Schlüssels, was unter OpenSSH veraltet ist.  Um eine Verbindung mit solchen Geräten herzustellen, muss der SSH-Client manuell konfiguriert werden, um `ssh-dss` zu akzeptieren.  

### <a name="device-discovery"></a>Gerätesuche

Wenn Sie die Gerätesuche aktivieren, ist Ihr Gerät über mDNS für andere Geräte im Netzwerk sichtbar.  Dieses Feature ermöglicht das Abrufen der SSH-PIN zur Kopplung mit dem Gerät durch Drücken der Schaltfläche "Koppeln", die verfügbar ist, wenn die Geräteermittlung aktiviert wird.  Diese PIN-Aufforderung muss auf dem Bildschirm angezeigt werden, um die erste Bereitstellung von Visual Studio für das Gerät abzuschließen.  

![PIN-Kopplung](images/devmode-pc-pinpair.PNG)

Die Gerätesuche sollte nur aktiviert werden, wenn das Gerät ein Bereitstellungsziel sein soll. Wenn beispielsweise eine App über das Geräteportal zum Testen auf einem Smartphone bereitgestellt wird, müssen Sie die Gerätesuche auf dem Smartphone, jedoch nicht auf Ihrem Entwicklungscomputer aktivieren.

### <a name="optimizations-for-windows-explorer-remote-desktop-and-powershell-desktop-only"></a>Optimierungen für Windows-Explorer, Remotedesktop und PowerShell (nur Desktop)

 Auf der Desktopgerätefamilie finden Sie auf der Einstellungsseite **Für Entwickler** Verknüpfungen zu den Einstellungen, die Sie zum Optimieren Ihres PCs für Entwicklungsaufgaben verwenden können. Für jede Einstellung können Sie das Kontrollkästchen aktivieren und auf **Übernehmen** klicken, oder Sie können auf den Link **Einstellungen anzeigen** klicken, um die Einstellungsseite für diese Option zu öffnen.


## <a name="notes"></a>Anmerkungen
In früheren Versionen von Windows 10 Mobile gab es die Option „Absturzabbild” im Menü "Entwicklereinstellungen".  Dieses Feature wurde auf das [Geräteportal](../debug-test-perf/device-portal.md) verschoben, damit sie per Remotezugriff, anstatt nur über USB verwendet werden kann.  

Es gibt verschiedene Tools, mit denen Sie eine App von einem Windows 10-PC auf einem Gerät bereitstellen können. Beide Geräte müssen über eine kabelgebundene oder drahtlose Verbindung mit dem gleichen Subnetz des Netzwerks verbunden sein oder über USB verbunden werden. Mit beiden aufgeführten Methoden wird nur das App-Paket (.appx/.appxbundle) installiert, nicht die Zertifikate.

-   Verwenden Sie das Tool für die Windows 10-Anwendungsbereitstellung (WinAppDeployCmd). Weitere Informationen zum [Tool WinAppDeployCmd](https://msdn.microsoft.com/library/windows/apps/mt203806.aspx).
-   Im [Geräteportal](../debug-test-perf/device-portal.md) ist die Bereitstellung aus Ihrem Browser auf einem mobilen Gerät unter Windows 10, Version 1511 oder höher, möglich. Im Geräteportal können Sie auf der Seite **[Apps](../debug-test-perf/device-portal.md#apps-manager)** ein App-Paket (APPX) hochladen und auf dem Gerät installieren.

## <a name="failure-to-install-developer-mode-package"></a>Fehler beim Installieren des Entwicklermoduspakets
In manchen Fällen wird der Entwicklermodus aufgrund von Problemen mit dem Netzwerk oder dem Administrator nicht ordnungsgemäß installiert. Das Entwicklermoduspaket ist für die **dezentrale** Bereitstellungen auf diesem PC erforderlich – verwenden Sie Device Portal über einen Browser oder Device Discovery SSH – aber nicht für die lokale Entwicklung.  Selbst wenn diese Probleme auftreten, können Sie Ihre App weiterhin mithilfe von Visual Studio oder von diesem Gerät auf ein anderes Gerät bereitstellen.

Im Forum zu den [bekannten Problemen](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) finden Sie entsprechende Problemumgehungen und vieles mehr.

> [!NOTE]
> Wenn Sie den Entwicklermodus nicht ordnungsgemäß installiert werden, empfehlen wir Ihnen in der Datei einer feedbackanforderung. In der **Feedback Hub** app, und wählen **neues Feedback übermitteln**, und wählen Sie die **Entwicklerplattform** Kategorie und die **Entwicklermodus** Unterkategorie. Senden von Feedback hilft Microsoft, die das Problem zu beheben, aufgetreten ist.

### <a name="failed-to-locate-the-package"></a>Das Paket konnte nicht gefunden werden

"Developer Mode package couldn’t be located in Windows Update. Error Code 0x80004005 Learn more"   

Dieser Fehler kann aufgrund eines Netzwerkverbindungsproblems, aufgrund von Enterprise-Einstellungen oder weil das Paket nicht vorhanden ist, auftreten.

So beheben Sie dieses Problem:

1. Stellen Sie sicher, dass Ihr Computer mit dem Internet verbunden ist.
2. Wenn Sie einen in eine Domäne eingebundenen Computer verwenden, sprechen Sie mit dem Netzwerkadministrator. Das Entwicklermoduspaket ist genau wie alle anderen Features standardmäßig in WSUS blockiert.
2.1. Zum Aufheben der Blockierung des Entwicklermodus-Pakets in der aktuellen und vorherigen Versionen sollten die folgenden KBs in WSUS zugelassen werden: 4016509, 3180030, 3197985  
3. Suchen nach Windows-Updates in den Einstellungen > Updates und Sicherheit > Windows-Updates.
4. Stellen Sie sicher, dass das Windows-Entwicklermoduspaket in den Einstellungen > System > Apps und Features > Optionale Features verwalten > Feature hinzufügen vorhanden ist. Wenn es nicht vorhanden ist, kann Windows das richtige Paket für Ihren Computer nicht finden.

Nachdem Sie einen der oben genannten Schritte durchgeführt haben, deaktivieren Sie den Entwicklermodus, und aktivieren Sie ihn dann erneut, um die Korrektur zu überprüfen.


### <a name="failed-to-install-the-package"></a>Fehler beim Installieren des Pakets

"Developer Mode package failed to install. Error Code 0x80004005 Learn more"

Dieser Fehler kann aufgrund von Inkompatibilitäten zwischen dem Build von Windows und dem Entwicklermoduspaket auftreten.

So beheben Sie dieses Problem:

1. Suchen nach Windows-Updates in den Einstellungen > Updates und Sicherheit > Windows-Updates.
2. Starten Sie Ihren Computer neu, um sicherzustellen, dass alle Updates angewendet wurden.


## <a name="use-group-policies-or-registry-keys-to-enable-a-device"></a>Verwenden von Gruppenrichtlinien oder Registrierungsschlüsseln zum Aktivieren von Geräten

Entwickler sollten meist die Einstellungs-Apps verwenden, um Geräte für das Debuggen zu aktivieren. In bestimmten Szenarien wie z. B. bei automatisierten Tests stehen weitere Möglichkeiten zum Aktivieren Ihres Windows 10-Desktopgeräts für die Entwicklung zur Verfügung.  Beachten Sie, dass diese Schritte nicht den SSH-Server aktivieren oder ermöglichen, dass das Gerät Ziel einer Remote-Bereitstellung und Debugging ist.

Sie können mithilfe von „gpedit.msc“ die Gruppenrichtlinien für die Geräteaktivierung festlegen (es sei denn, Sie verwenden Windows 10 Home). In Windows 10 Home müssen die Registrierungsschlüssel für die Geräteaktivierung direkt mithilfe von regedit oder von PowerShell-Befehlen festgelegt werden.

**Verwenden Sie Gpedit, um Ihr Gerät zu aktivieren.**

1.  Führen Sie **Gpedit.msc** aus.
2.  Navigieren Sie zu „Richtlinie für "Lokaler Computer" &gt; Computerkonfiguration &gt; Administrative Vorlagen &gt; Windows-Komponenten &gt; App-Paketbereitstellung.
3.  Bearbeiten Sie zum Aktivieren des Querladens die folgenden Richtlinien:

    -   **Installation aller vertrauenswürdigen Anwendungen zulassen**

    - ODER

    Bearbeiten Sie zum Aktivieren des Entwicklermodus die Richtlinien, um beide zu aktivieren:

    -   **Installation aller vertrauenswürdigen Anwendungen zulassen**
    -   **Ermöglicht die Entwicklung von UWP-apps und sie über eine integrierte Entwicklungsumgebung (IDE) installieren**

4.  Starten Sie Ihren Computer neu.

**Verwenden Sie "regedit", um Ihr Gerät aktivieren**

1.  Führen Sie **regedit** aus.
2.  Legen Sie diesen DWORD-Wert auf 1 fest, um das Querladen zu aktivieren.

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowAllTrustedApps**

    - ODER

    Um den Entwicklermodus zu aktivieren, legen Sie diese DWORD-Werte auf 1 fest:

    -   **HKLM\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock\\AllowDevelopmentWithoutDevLicense**

**Verwenden von PowerShell zum Aktivieren Ihres Geräts**

1.  Führen Sie PowerShell mit Administratorrechten aus.
2.  Führen Sie zum Aktivieren des Querladens diesen Befehl aus:

    -   **PS C:\\WINDOWS\\system32&gt; reg add "HKEY\_LOCAL\_MACHINE\\SOFTWARE\\Microsoft\\Windows\\CurrentVersion\\AppModelUnlock" /t REG\_DWORD /f /v "AllowAllTrustedApps" /d "1"**

    - ODER

    Führen Sie zum Aktivieren des Entwicklermodus diesen Befehl aus:

    -   **PS: C:\\WINDOWS\\"System32"&gt; Reg hinzufügen "HKEY\_lokalen\_Computer\\SOFTWARE\\Microsoft\\Windows\\" CurrentVersion "\\ AppModelUnlock"/ t / REG\_DWORD/f/v"AllowDevelopmentWithoutDevLicense"/ d"1"**

## <a name="upgrade-your-device-from-windows-81-to-windows-10"></a>Aktualisieren Ihres Geräts von Windows 8.1 auf Windows 10

Wenn Sie Apps auf Ihrem Windows 8.1-Gerät erstellen oder querladen, müssen Sie eine Entwicklerlizenz installieren. Beim Upgrade Ihres Geräts von Windows 8.1 auf Windows 10 bleiben diese Informationen erhalten. Führen Sie den folgenden Befehl aus, um diese Informationen von Ihrem aktualisierten Windows 10-Gerät zu entfernen. Dieser Schritt ist nicht erforderlich, wenn Sie ein Upgrade von Windows 8.1 direkt auf Windows 10, Version 1511 oder höher, ausführen.

**Beim Aufheben der Registrierung einer Entwicklerlizenz**

1.  Führen Sie PowerShell mit Administratorrechten aus.
2.  Führen Sie folgenden Befehl aus: **unregister-windowsdeveloperlicense**.

Danach müssen Sie Ihr Gerät wie in diesem Thema beschrieben für die Entwicklung aktivieren, damit Sie weiterhin auf diesem Gerät entwickeln können. Andernfalls erhalten Sie möglicherweise eine Fehlermeldung, wenn Sie Ihre App debuggen oder versuchen, ein Paket dafür zu erstellen. Hier ist ein Beispiel für diesen Fehler:

Fehler: DEP0700 : Fehler beim Registrieren der app.

## <a name="see-also"></a>Weitere Informationen

* [Ihre erste app](your-first-app.md)
* [Veröffentlichen Sie Ihre UWP-App](https://developer.microsoft.com/store/publish-apps).
* [Anleitungen zum Entwickeln von UWP-apps](https://developer.microsoft.com/windows/apps/develop)
* [Codebeispiele für UWP-Entwickler](https://developer.microsoft.com/windows/samples)
* [Was ist eine UWP-App?](universal-application-platform-guide.md)
* [Für Windows-Konto registrieren](sign-up.md)
