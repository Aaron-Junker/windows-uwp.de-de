---
title: Xbox Live-sandboxes
description: Informationen Sie zu Sandkästen für die Entwicklung von Xbox Live.
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: ee284550a9b508a8d46556bf0353bd75d55014f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599475"
---
# <a name="xbox-live-sandboxes-intro"></a>Xbox Live-Sandboxes Einführung

In [Xbox Live-Dienstkonfiguration](xbox-live-service-configuration.md), es wurde erläutert, müssen Sie Informationen zu Ihrem Titel online in der Regel in konfigurieren [Partner Center](https://partner.microsoft.com/dashboard).  Diese Informationen umfassen Dinge wie Bestenlisten, die möchte, dass der Titel anzuzeigen, Erfolge, die Spieler entsperren können, der Vermittlung Konfiguration usw. an.

Wenn Sie Änderungen an der Dienstkonfiguration vornehmen, müssen diese von Partner Center veröffentlicht werden, bevor die Änderungen vom Rest des Xbox Live übernommen werden und nach Ihrem Titel angezeigt werden können.

Sie veröffentlichen, um eine Sandbox für die Entwicklung genannte.  Diese können Sie Änderungen an Ihrem Titel in einer isolierten Umgebung arbeiten.  Diese bieten mehrere Vorteile beschrieben, die der weiter unten.

Sind standardmäßig ein Xbox-Konsolen und Windows 10-PCs in der Sandbox Einzelhandel.

## <a name="benefits"></a>Vorteile

Entwicklung Sandboxes bieten einige Vorteile:


1. Sie können zu Änderungen an der ein Update für den Titel durchlaufen, ohne Auswirkungen auf die derzeit verfügbare Version verwenden.
2. Einige Tools funktionieren nur in einer Sandbox Entwicklung aus Sicherheitsgründen.
3. Einige Entwickler in Ihrem Team möglicherweise "Verzweigung" und Test-Dienst-konfigurationsänderungen möchten, ohne Auswirkungen auf Ihre primären Dienstkonfiguration für die in der Entwicklung.
4. Anderer Herausgeber nicht angezeigt, was Sie gerade arbeiten ohne Zugriff auf Ihre Sandbox.

Sie können auch **optional** Testkonten zu erstellen.  Sie können diese verwenden, wenn keine reguläre Xbox Live-Konto zum Testen der Titels des verwenden möchten, oder Sie mehrere Konten für Szenarios wie z. B. soziale Interaktion zu testen, benötigen (z. B.: eine Friend Statistiken anzeigen) oder Multiplayer.

Testkonten können nur Anmelden an Sandboxen für Entwicklung und werden in einem Abschnitt weiter unten erläutert werden.

## <a name="finding-out-about-your-sandbox"></a>Suchen Sie über Ihre Sandbox

Die große Mehrheit der Entwickler benötigen nur eine Sandbox.  Glücklicherweise ist eine Sandbox für Sie erstellt, wenn Sie einen Titel erstellen.

1. Erfahren Sie mehr über Ihre Sandbox um Partner Center hier: ![](images/getting_started/first_xbltitle_dashboard.png)

1. Klicken Sie dann auf den Titel auf: ![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. Zum Schluss klicken Sie auf die Dienste -> Xbox Live, im linken Menü ![](images/getting_started/first_xbltitle_leftnav.png)

1. Sie können jetzt Ihre Sandbox, die im folgenden aufgeführt sehen. ![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>Wie wirkt sich auf Ihre Sandbox Ihres Workflows

In der Regel arbeiten Sie mit Sandboxes auf folgende Weise:

1. (Einmalig) Wechseln Sie Ihrem PC oder deiner Xbox One mit Ihrer Entwicklung Sandbox ein.
2. (N-Mal) Wenn Sie Änderungen an der Dienstkonfiguration vornehmen, veröffentlichen Sie Änderungen auf Ihre Entwicklung Sandbox.  Änderungen an der Konfiguration von Service Dinge wie z. B. definieren Erfolge, Bestenlisten hinzufügen oder Ändern einer Sitzungsvorlage für Multiplayer-Spiele.
3. (Mehrfach) Wenn Sie für andere Teammitglieder arbeiten, können Sie ihnen Zugriff auf Ihre Sandbox gewähren
4. (Einmalig) Wenn Sie möchten etwas im Einzelhandel testen oder eine Unterbrechung an Ihre bevorzugte Xbox-Spiel zu spielen nutzen möchten, müssen Sie Ihre Sandbox wieder auf "RETAIL" zu wechseln.

Diese Szenarien werden im folgenden ausführlicher beschrieben.  Der Prozess besitzt einige Unterschiede auf PC, Konsole, und damit sind separate Abschnitte für die einzelnen vorhanden.

## <a name="switch-your-pcs-development-sandbox"></a>Der PC Entwicklung Sandbox wechseln

Wenn Sie Ihre PC Entwicklung Sandbox wechseln möchten, ist die empfohlene Methode dafür Windows Device Portal (WDP) verwenden.  Sie können dies auch über die Befehlszeile durchführen.  Wir werden beide Methoden beschrieben.

### <a name="windows-device-portal"></a>Windows Device Portal

Wenn Sie noch nicht WDP auf Ihrem PC aktiviert haben, befolgen Sie diese Anweisungen dazu. [Einrichten der Device-Portal auf der Windows-Desktop](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

Sobald Sie dies erledigt haben, öffnen Sie Windows-Dev-Portal, indem Sie die Verbindung herstellt, in Ihrem Webbrowser, wie in der oben genannten Artikel beschrieben.

Klicken Sie auf "Xbox Live", um den entsprechenden Abschnitt zu wechseln, wie unten dargestellt.

![](images/getting_started/wdp_switch_sandbox.png)

Und Sie Ihre Sandbox eingeben können, die Sie über die Schritte im erhalten haben die *suchen Sie Ihre Sandbox* , und klicken Sie auf "Ändern".

Um wieder auf "RETAIL" zu wechseln, können Sie im Einzelhandel hier eingeben.

### <a name="powershell-module"></a>PowerShell-Modul

[Xbox Live-PowerShell-Modul](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md) XboxlivePSModule enthält verschiedene Hilfsprogramme zum Ändern von Sandkästen auf dem PC oder -Konsole Xbox Live-Entwicklung zu unterstützen.

* Um es von Nutzen [PowerShell-Katalog](https://www.powershellgallery.com/packages/XboxlivePSModule), öffnen Sie ein PowerShell-Fenster:
    1. Herunterladen Sie und installieren Sie das Modul: `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. Einstieg mit `Import-Module XboxlivePSModule`
    3. Führen Sie Cmdlets, z. B. Set-XblSandbox XDKS.1 oder Get-XblSandbox

* Um es aus einer Zipdatei an nutzen [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools), einem PowerShell-Fenster öffnen
    1. Führen Sie `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1` aus.
    2. Führen Sie Cmdlets, z. B. Set-XblSandbox XDKS.1 oder Get-XblSandbox

### <a name="command-prompt-script"></a>Eingabeaufforderung-Skript

Laden Sie das Xbox Live-Tools-Paket auf [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) und Entpacken Sie es.  Sehen Sie eine SwitchSandbox.cmd Batch-Datei in ein.

Führen Sie es im Administratormodus, um Ihre Sandbox zu wechseln.  Das erste Argument ist der Sandbox.  Z. B. Wenn Sie mit der Sandbox XDKS.1 wechseln möchten, würden Sie tun:

```
SwitchSandbox.cmd XDKS.1
```

Um wieder auf "RETAIL" zu wechseln, geben Sie einfach, die als zweites Argument.

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Wechseln Sie Ihre Xbox One-Konsole Development-sandbox

### <a name="using-windows-dev-portal"></a>Mithilfe der Windows-Dev-Portal

Sie können im Windows Dev-Portal verwenden, die Sandbox auf der Konsole ändern.  Zu diesem Zweck rufen Sie "Home Dev" in der Konsole und aktivieren Sie ihn.

Danach können Sie die IP-Adresse in Ihrem Browser auf Ihrem PC für die Verbindung zur Konsole eingeben.  Sie können dann klicken Sie auf "Xbox Live" und geben Sie den Sandkasten in das Textfeld vorhanden.

### <a name="using-xbox-one-manager"></a>Mit dem ein Xbox-Manager

Xbox One-Manager können Sie bestimmte Aspekte der Konsole von Ihrem PC zu verwalten.  Dies schließt einen Neustart, installierten apps zu verwalten und ändern Ihre Sandbox.

Klicken Sie mit der rechten Maustaste auf die Konsole, die Sie verwenden möchten, ändern die Sandbox für, und wechseln zu "Einstellungen..."

Anschließend können Sie es eine Sandbox eingeben.

### <a name="using-xbox-one-console-ui"></a>Verwenden die Xbox One-Verwaltungskonsolen-Benutzeroberfläche

Wenn Sie Ihre Sandbox für die Entwicklung direkt in der Konsole ändern möchten, können Sie auf "Einstellungen" wechseln.  Fahren Sie mit "Developer-Einstellungen", und sehen Sie eine Option aus, um Ihre Sandbox zu ändern.

## <a name="sandbox-uses"></a>Sandbox verwendet.

### <a name="data-that-is-sandboxed"></a>Daten, die als sandkastenlösung bereitgestellt wird
Sie können die Sandbox-Funktionen verwenden, zum Verwalten des Zugriffs zwischen Entwicklern in Ihrem Team während des Entwicklungsprozesses.  Beispielsweise empfiehlt es sich um Daten zwischen Ihrem Entwicklungsteam und Tester zu isolieren.

Sandbox-Daten enthalten Folgendes:
- Erfolge, Bestenlisten und Statistiken für einen Benutzer.  Erfolge, die für einen Benutzer in einer Sandbox kumuliert übersetzen Sie nicht mit einem anderen Sandbox.
- Multiplayer-Spiele und Vermittlung.  Benutzer können keine Multiplayer-Spiele spielen Spiel mit einer anderen Person wird einer anderen Sandbox.
- Konfiguration des Diensts.  Wenn Sie einen Titel in einer Sandbox eine neue Auszeichnung hinzugefügt haben, ist es nicht in einer anderen Sandbox sichtbar.  Dies gilt für alle Dienstkonfigurationsdaten.

Nicht-Sandbox werden vorwiegend sozialen Informationen.  Also z. B., wenn ein Benutzer ein anderer Benutzer folgt, dass eine Sandbox-agnostisch ist.

### <a name="examples"></a>Beispiele
Einige Beispiele für werden unten bereitgestellt werden, um zu veranschaulichen einige der Vorteile von warum Sie möglicherweise mehrere Sandboxes verwenden möchten.

> **Hinweis**: Wenn Sie in der Xbox Creators-Programm sind, können Sie nur eine Sandbox haben.  Wenn Sie haben mehrere Sandboxes erstellen müssen, wenden Sie auf die ID@Xbox Programm.

#### <a name="service-config-isolation"></a>Config-Isolation von Diensten
Wie bereits erwähnt, ist die Dienstkonfiguration Sandbox-bestimmte.  Müssen Sie möglicherweise eine *Entwicklung* Sandbox und *Tests* Sandbox.  Wenn Sie einen Build der Titel des an Ihre Tester erteilen, würden Sie veröffentlichen Ihre [Dienstkonfiguration](xbox-live-service-configuration.md) auf die *Tests* Sandbox.

In der Zwischenzeit können Sie Erfolge hinzufügen, oder anderen Multiplayer-Sitzung Typen, für die Ihre *Entwicklung* Sandkasten ohne Auswirkungen auf die Dienstkonfiguration, die Ihre Tester angezeigt werden.

#### <a name="multiplayer"></a>Multiplayer
Nehmen Sie im obigen Beispiel mit einem *Entwicklung* und *Tests* Sandbox.  Vielleicht Ihre Dienstkonfiguration ist die gleiche Weise Sandboxes, aber Ihre Entwickler Multiplayer-Funktionen erstellen und Vermittlung miteinander testen möchten.  Auch testen Multiplayer-Ihre Tester.

In einem Fall wie folgt sollten Ihre Entwickler nicht die Xbox Live-Vermittlungsdienst, diese mit Tester abzugleichen, da diese separat Probleme Debuggen.  Eine gute Möglichkeit, dies zu verhindern, dass wäre für Ihre Entwickler in die *Entwicklung* Sandbox und Tester sich in einem separaten *Tests* Sandbox.  Auf diese Weise die beiden Gruppen isoliert.

## <a name="advanced"></a>Erweitert

Zur Vereinfachung der Entwicklung beginnen mit Ihrem Standard-Sandkasten aus, und fügen Sie neue Sandkästen mit Bedacht.

Wenn Sie Ihre Anforderungen wächst Zugriff und Isolation gefunden haben, sehen Sie [erweiterte Xbox Live-Sandboxes](advanced-xbox-live-sandboxes.md) Artikel.  
