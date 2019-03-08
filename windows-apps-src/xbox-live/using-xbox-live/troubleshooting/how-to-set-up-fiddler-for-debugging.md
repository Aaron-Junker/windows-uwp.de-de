---
title: Problembehandlung bei Xbox Live mithilfe von Fiddler
description: Erfahren Sie, wie Sie Fiddler verwenden, um die Xbox Live-Dienst-aufrufen zu beheben.
ms.assetid: 7d76e444-027b-4659-80d5-5b2bf56d199e
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Fiddler, Webdienstaufrufe, Problembehandlung
ms.localizationpriority: medium
ms.openlocfilehash: 84c6717a4f9f5aff9fd3ff1f68c870fdd9174865
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606715"
---
# <a name="troubleshooting-xbox-live-using-fiddler"></a>Problembehandlung bei Xbox Live mithilfe von Fiddler

Fiddler ist ein Web debugging Proxy, der alle HTTP- und HTTPS-Datenverkehr zwischen Ihrem Gerät und dem Internet protokolliert. Sie verwenden es, sich anzumelden, und überprüfen Sie den Verkehr zu und von der Xbox Live-Dienste und der vertrauenden Seite Partei-Webdienste, um zu verstehen und Debuggen Aufrufe des Webdiensts.

## <a name="for-windows-uwp-pc-apps"></a>Für Windows-PC für UWP-apps

1. Stellen Sie sicher, dass der aktuelle Benutzer der Administratorgruppe auf dem PC
1. Laden Sie Fiddler von [https://www.telerik.com/fiddler](https://www.telerik.com/fiddler)
1. Stellen Sie sicher, dass Sie die Version auswählen, die "für .NET 4 erstellt wurde."
1. Wechseln Sie nach Abschluss der Installation-Tools > Fiddler-Optionen, und Aktivieren von HTTPS CONNECTs erfassen und Entschlüsseln von HTTPS-Datenverkehr.  Die gesamte Kommunikation zwischen der Runtime und Xbox LIVE-Dienste werden mit SSL verschlüsselt.  Ohne diese Option werden keine brauchbaren Funktionen angezeigt.  Akzeptieren Sie alle Dialogfelder, die Fiddler angezeigt (5 Dialogfelder UAC einschließlich werden sollte)
1. Wechseln Sie zu "WinConfig", "Ausgenommen All" und "Änderungen speichern".  Andernfalls funktioniert die Fiddler nicht für Store-apps.
1. Nun, wenn Sie Ihre app ausführen, sollte die Anforderungen/Antworten zwischen der app, Laufzeit und das Xbox LIVE angezeigt werden.

Zum Debuggen von UWP-OS-Ebene aufrufen, die in der Regel ist nicht erforderlich, kann jedoch sein müssen hilfreich beim Debuggen von Anmelde- und in-Game-Ereignisse, Sie Fiddler zum Erfassen von WinHTTP-Datenverkehr zu konfigurieren.
Dies kann mit erfolgen:
```cpp
    netsh winhttp set proxy 127.0.0.1:8888 "<-loopback>"
```
in denen der Port 8888 den Port übereinstimmt, haben Sie Fiddlers-Tools konfiguriert | Optionen für | Registerkarte "Verbindungen" handelt es sich standardmäßig 8888.
Um dies rückgängig zu machen, führen Sie aus:
```cpp
    netsh winhttp reset proxy
```

## <a name="for-xbox-one-uwp-based-projects"></a>Für Xbox One-UWP-basierter Projekte

Führen Sie den folgenden Schritten [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/uwp-fiddler)

## <a name="for-xbox-one-xdk-based-projects"></a>Für Xbox One XDK-basierte Projekte

Im normalen Betrieb besteht das Risiko, dass die Kommunikation einer Konsole, die über einen Proxy kommuniziert, durch diesen verändert wird, sodass Spieler möglicherweise mogeln können. Daher werden Konsolen so entworfen, dass sie keine Kommunikation über einen Proxy zulassen. Wenn Sie Fiddler mit Ihrem Xbox One Dev Kit verwenden, müssen Sie einige spezielle Konfigurationsschritte für den Dev Kit ausführen, damit dieser den Fiddler-Proxy verwenden kann.

Fiddler ist Freeware und kann von der [Fiddler-Website](https://www.telerik.com/fiddler/) heruntergeladen werden.

Fiddler kann Auswirkungen auf den Netzwerkstatus haben, der von der Konsole gemeldet wird. Wenn eine Upstreamverbindung auf dem Computer deaktiviert wird, auf dem Fiddler ausgeführt wird, erkennt die Konsole diese Trennung möglicherweise erst nach Ablauf der Authentifizierung der Konsole. Wenn Sie Fiddler verwenden, müssen Sie die Verbindung zwischen der Konsole und dem Computer trennen, auf dem Fiddler ausgeführt wird, anstelle Fiddler zu verwenden, um eine Trennung zu simulieren Noch besser, verwenden Sie die Netzwerk-Spannung Tools simulieren also die Trennung der Verbindung für Testzwecke.

Installieren und aktivieren Fiddler, auf dem Entwicklungs-PC

Führen Sie diese Schritte zum Installieren und aktivieren Fiddler zum Überwachen von Datenverkehr aus dem Dev-Kit aus.

1. Installieren Sie Fiddler auf Ihrem Entwicklungscomputer, indem Sie die Anweisungen auf der [Fiddler-Website](https://www.telerik.com/fiddler/) befolgen.
1. Starten Sie Fiddler, und wählen Sie Fiddler-Optionen, aus dem Menü "Extras".
1. Wählen Sie die Registerkarte "Verbindungen", und stellen Sie sicher, dass auf die Remotecomputern "zulassen" die Verbindung das Kontrollkästchen aktiviert ist.
1. Klicken Sie auf OK, um die Änderung an den Einstellungen zu übernehmen. Ihnen wird nun ein Dialogfeld angezeigt, in dem Ihnen mitgeteilt wird, dass Fiddler neu gestartet werden muss, damit die Änderung wirksam wird, und Sie Ihre möglicherweise Ihre Firewall manuell konfigurieren müssen. Klicken Sie in diesem Dialogfeld auf OK, aber noch nicht neu Fiddler.
1. Konfigurieren Sie die erforderliche Firewallregel, um Remotecomputern das Herstellen von Verbindungen zu gestatten. Starten Sie das Windows-Firewall-Systemsteuerungsapplet. Klicken Sie auf Erweiterte Einstellungen, und klicken Sie dann eingehenden Sie Regel für Datenverkehr. Suchen Sie nach der Regel mit dem Namen "FiddlerProxy", und führen Sie einen Bildlauf nach rechts, überprüfen, dass jede der folgenden Einstellungen für diese Regel angezeigt wird.

| Einstellung          | Bevorzugter Wert                |
|------------------|--------------------------------|
| Name             | FiddlerProxy                   |
| Gruppe            | (einen Wert für die Gruppe nicht festgelegt) |
| Profil          | Alle                            |
| Enabled          | Ja                            |
| Aktion           | Zulassen                          |
| Außer Kraft setzen         | Nein                             |
| Programm          | Pfad zur fiddler.exe            |
| LocalAddress     | Beliebig                            |
| RemoteAddress    | Beliebig                            |
| Protokoll         | TCP                            |
| LocalPort        | Beliebig                            |
| RemotePort       | Beliebig                            |
| AllowedUsers     | Beliebig                            |
| AllowedComputers | Beliebig                            |


1. Konfigurieren Sie Fiddler zur Erfassung und HTTPS-Datenverkehr entschlüsseln.
    1. Um optimale Leistung zu aktivieren, legen Sie Fiddler zum Streaming-Modus zu verwenden, indem Sie auf die Schaltfläche "Stream" in der Schaltflächenleiste.
    1. Wählen Sie in Fiddler Fiddler-Optionen und Extras, dann HTTPS aus.
    1. Überprüfen Sie das Kontrollkästchen der Entschlüsselung von HTTPS-Datenverkehr. Wenn Sie eine Messagebox fragt, ob zum Konfigurieren von Windows das ZS-Zertifikat vertrauen, klicken Sie auf Nein.
    1. Klicken Sie auf die Schaltfläche Desktop Stammzertifikat zu exportieren.
1. Beenden Sie Fiddler, und starten Sie ihn erneut.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>So konfigurieren Sie ein Dev Kit für die Verwendung von Fiddler als dessen Proxy für das Internet
Fiddler-Konfiguration auf dem Dev-Kit wurde vereinfacht, von der Methode, die in früheren Versionen verwendet.

1. Kopieren Sie das Fiddler-Stammzertifikat, das Sie auf dem Desktop mit dem Dev-Kit als exportiert``` xs:\Microsoft\Cert\FiddlerRoot.cer```.  Sie können den folgenden Befehl verwenden:  ```xbcp [local Fiddler Root directory]\FiddlerRoot.cer xs:\Microsoft\Cert\FiddlerRoot.cer```
1. Erstellen Sie eine Textdatei namens ```ProxyAddress.txt```, mit der IP-Adresse oder Hostname des Entwicklungs-PC ausgeführt wird, Fiddler, und die Portnummer an, in dem Fiddler als ausschließlichen Inhalt in der Datei überwacht wird. Der Servername/IP-Adresse und Port wie folgt formatiert: "HOST: PORT". (In der Standardeinstellung verwendet Fiddler Port 8888). Z. B. "10.124.220.250:8888" oder "my_dev_pc.contoso.com:8888". Kopieren Sie diese Datei mit dem Dev-Kit als xs:\Microsoft\Fiddler\ProxyAddress.txt.  Sie können den folgenden Befehl verwenden:  ```xbcp [local Proxy Address file directory]\ProxyAddress.txt xs:\Microsoft\Fiddler\ProxyAddress.txt```
1. Starten Sie durch Eingabe des Dev-Kit neu ```xbreboot``` an der Eingabeaufforderung

### <a name="to-stop-using-fiddler"></a>So beenden Sie die Verwendung von Fiddler

Um als Proxy für das Internet mithilfe von Fiddler, und daher Ablaufverfolgung aller dem Dev-Kit des Netzwerkdatenverkehrs in Fiddler ausführen beenden, einfach umbenennen oder löschen Sie die FiddlerRoot.cer-Datei auf dem Dev-Kit, und Sie dann einen Neustart des Dev-Kits.

### <a name="how-it-works"></a>Funktionsweise

Das Vorhandensein von sowohl eine xs:\Microsoft\Cert\FiddlerRoot.cer und eine Datei xs:\Microsoft\Fiddler\ProxyAddress.txt beim Systemstart bewirkt, dass der Development Kit auf sich selbst um die Verwendung des Proxyservers in ProxyAddress.txt angegeben konfigurieren. Wenn entweder die Datei nicht vorhanden ist, klicken Sie dann das Dev-Kit nicht selbst über einen Fiddler-Proxy ausgeführt werden konfiguriert.

Jeder PC, auf dem Fiddler installiert ist, verwendet ein anderes Fiddler-Stammzertifikat. Wenn Sie mehr als ein PC zur Verfügung, die verwendet werden kann, um einen Fiddler-Proxy für Ihr Development Kit bereitzustellen, können Sie jedes PCs Fiddler-Stammzertifikat in das Verzeichnis Xs:\Microsoft\Cert kopieren, solange eine davon FiddlerRoot.cer heißt. Alle Zertifikate in das Verzeichnis Cert werden überprüft, wenn ein Fiddler-Proxy authentifiziert wird. Die Fiddler-Instanz auf ProxyAddress.txt beliebigen PC Adresse wird als Proxy dienen, solange das Zertifikat im Zertifikat Verzeichnis vorhanden ist.
