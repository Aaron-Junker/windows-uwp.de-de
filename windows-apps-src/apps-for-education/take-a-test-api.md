---
Description: Mit der JavaScript-API für die App „Prüfung“ von Microsoft können Sie zuverlässige Bewertungen durchführen. „Prüfung“ stellt einen sicheren Browser bereit, der die Lernenden daran hindert, während eines Tests andere Computer- oder Internet-Ressourcen zu verwenden.
title: JavaScript-API für Prüfung.
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
keywords: Windows 10, UWP, Bildung
ms.localizationpriority: medium
ms.openlocfilehash: bee8a04e3b4d57caf7da3e21f2be3c789d83be90
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627595"
---
# <a name="take-a-test-javascript-api"></a>JavaScript-API für Prüfung

[Prüfung](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) ist eine browserbasierte UWP-app, die gesperrten online Bewertungen zu Testzwecken anspruchsvolles rendert ermöglicht Lehrkräften auf der Bewertung konzentrieren statt eines sicheren Bereitstellen von Inhalten testumgebung. Um dies zu erreichen, wird eine JavaScript-API verwendet, die von jeder Web-Anwendung genutzt werden kann. Die API „Prüfung“ unterstützt den [Browser-API-Standard SBAC](https://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) zur Durchführung wichtiger allgemeiner Kernprüfungen.

Weitere Informationen zur App selbst finden Sie unter [Technische Referenz zur App „Prüfung“](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). Hilfe zur Problembehandlung finden Sie unter [Problembehandlung bei Microsoft Prüfung mithilfe der Ereignisanzeige](troubleshooting.md).

## <a name="reference-documentation"></a>Referenzdokumentation
Die Prüfungs-APIs gibt es in den folgenden Namespaces. Beachten Sie, dass alle APIs von einem globalen `SecureBrowser`-Objekt abhängen.

| Namespace | Beschreibung |
|-----------|-------------|
|[Security-namespace](#security-namespace)|Enthält APIs, mit denen Sie das Gerät zu Testzwecken sperren und eine Testumgebung erzwingen können. |

### <a name="security-namespace"></a>Sicherheitsnamespace

Der sicherheitsnamespace können Sie das Gerät sperren, überprüfen Sie die Liste der Benutzer- und Prozesse, Mac- und IP-Adressen zu erhalten und Löschen von zwischengespeicherten Webressourcen.

| Methode | Beschreibung   |
|--------|---------------|
|[Sperren](#lockDown) | Sperrt das Gerät für Testzwecke. |
|[isEnvironmentSecure](#isEnvironmentSecure) | Stellt fest, ob auf das Gerät noch der Sperrmodus-Kontext angewendet wird. |
|[getDeviceInfo](#getDeviceInfo) | Ruft Details zur Plattform ab, auf der die Testanwendung ausgeführt wird. |
|[examineProcessList](#examineProcessList)|Ruft die Liste der ausgeführten Benutzer- und Systemprozesse ab.|
|[Schließen](#close) | Schließt den Browser und entsperrt das Gerät. |
|[getPermissiveMode](#getPermissiveMode)|Überprüft, ob der eingeschränkte Modus aktiviert oder deaktiviert ist.|
|[setPermissiveMode](#setPermissiveMode)|Schaltet den eingeschränkten Modus ein oder aus.|
|[emptyClipBoard](#emptyClipBoard)|Löscht die Zwischenablage des Systems.|
|[getMACAddress](#getMACAddress)|Ruft die Liste der MAC-Adressen für das Gerät ab.|
|[getStartTime](#getStartTime) | Ruft die Zeit ab, zu der die Test-App gestartet wurde. |
|[getCapability](#getCapability) | Fragt ab, ob eine Funktion aktiviert oder deaktiviert ist. |
|[setCapability](#setCapability)|Aktiviert oder deaktiviert die angegebene Funktion.| 
|[isRemoteSession](#isRemoteSession) | Überprüft, ob die aktuelle Sitzung remote angemeldet ist. |
|[isVMSession](#isVMSession) | Überprüft, ob die aktuelle Sitzung auf einem virtuellen Computer ausgeführt wird. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>lockDown
Sperrt das Gerät. Wird auch zum Entsperren des Geräts verwendet. Die Test-Webanwendung führt diesen Aufruf aus, bevor Studenten mit dem Testen beginnen dürfen. Der Implementierer ist erforderlich, um alle notwendigen Aktionen zum Schutz der Testumgebung durchzuführen. Welche Schritte zum Sichern der Umgebung sind Geräte, die bestimmte und z. B., Aspekte, z. B. das Deaktivieren von Bildschirmaufnahmen, Voice Chat im sicheren Modus deaktivieren, deaktivieren die Zwischenablage des Systems, in einen Kioskmodus versetzt eingeben, deaktivieren Leerzeichen in OS x 10.7 und höher umfassen Geräte, usw. Testen der Anwendung können Sperren, bevor eine Bewertung beginnt und die Sperrung deaktiviert wird, wenn der Student, der die Bewertung abgeschlossen ist, und außerhalb des sicheren Test liegt.

**Die Syntax**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**Parameter**  
* `enable` - **"true"** führen Sie die Take a Test app über den Sperrbildschirm und Anwenden von Richtlinien, die in diesem erläutert [Dokument](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). **false** hält die Ausführung von „Prüfung“ über dem Sperrbildschirm an und beendet sie. Wirkungslos, wenn die App nicht gesperrt ist.  
* `onSuccess` -[optional] die Funktion, die aufgerufen wird, nachdem die Sperre wurde erfolgreich aktiviert oder deaktiviert wurde. Sie muss im Format `Function(Boolean currentlockdownstate)` vorliegen.  
* `onError` -[optional] die Funktion aufrufen, wenn die Sperrung-Vorgang fehlgeschlagen ist. Sie muss im Format `Function(Boolean currentlockdownstate)` vorliegen.  

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
Stellt fest, ob auf das Gerät noch der Sperrmodus-Kontext angewendet wird. Die Test-Webanwendung ruft diese Funktion auf, bevor Studenten mit dem Testen beginnen dürfen, sowie in regelmäßigen Abständen während des Tests.

**Die Syntax**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**Parameter**  
* `callback` – Die Funktion aufrufen, wenn diese Funktion abgeschlossen ist. Sie muss im Format `Function(String state)` vorliegen, wobei `state` eine JSON-Zeichenfolge ist, die zwei Felder enthält. Das erste Feld ist `secure`. Dieses Feld zeigt nur dann `true` an, wenn alle erforderlichen Sperren aktiviert (oder Features deaktiviert) wurden, um eine sichere Testumgebung zu ermöglichen, und wenn nichts beschädigt wurde, seitdem die App in den Sperrmodus gewechselt ist. Das andere Feld, `messageKey`, enthält weitere anbieterspezifische Details. Hier können Hersteller zusätzliche Informationen hinterlegen, die das boolesche Kennzeichen `secure` erweitern:

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
Ruft Details zur Plattform ab, auf der die Testanwendung ausgeführt wird. Wird verwendet, um die Informationen anzureichern, die vom Benutzer-Agent erkennbar waren.

**Die Syntax**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**Parameter**  
* `callback` – Die Funktion aufrufen, wenn diese Funktion abgeschlossen ist. Sie muss im Format `Function(String infoObj)` vorliegen, wobei `infoObj` eine JSON-Zeichenfolge ist, die mehrere Felder enthält. Die folgenden Felder müssen unterstützt werden:
    * `os` Stellt den Typ des Betriebssystems (z.B.: Windows, MacOS, Linux, iOS, Android usw..)
    * `name` die Namen der Betriebssystemversion, darstellt, sofern vorhanden (z. B.: Sierra Ubuntu).
    * `version` Stellt die Version des Betriebssystems (z.B.: 10.1, 10 pro usw..)
    * `brand` Stellt den sicheren Browser branding (z. B.: OAKS, CA, SmarterApp usw.)
    * `model` Stellt das Gerätemodell nur für mobile Geräte dar. /nicht verwendete für Desktopbrowser.

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
Ruft die Liste aller Prozesse ab, die auf dem Clientcomputer im Besitz des Benutzers ausgeführt werden. Die Testanwendung ruft diese Funktion auf, um die Liste zu überprüfen und mit einer Liste der Prozesse zu vergleichen, die während der Testphase auf die Blacklist gesetzt wurden. Dieser Aufruf sollte sowohl zu Beginn einer Prüfung sowie in regelmäßigen Abständen während der Prüfung ausgeführt werden. Wenn ein Prozess auf der Blacklist erkannt wird, sollte die Prüfung beendet werden, um die Integrität des Tests zu wahren.

**Die Syntax**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**Parameter**  
* `blacklistedProcessList` – Die Liste der Prozesse, die die Testen der Anwendung gesperrt ist.  
`callback` – Die Funktion aufrufen, nachdem die aktiven Prozesse gefunden wurden. Sie muss im Format `Function(String foundBlacklistedProcesses)` vorliegen, wobei `foundBlacklistedProcesses` das Format `"['process1.exe','process2.exe','processEtc.exe']"` hat. Sie ist leer, wenn keine Prozesse auf der Blacklist gefunden wurden. Wenn sie null ist, bedeutet dies, dass im ursprünglichen Funktionsaufruf ein Fehler aufgetreten ist.

**Anmerkung** Diese Liste enthält keine Systemprozesse.

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="close"/>

### <a name="close"></a>close
Schließt den Browser und entsperrt das Gerät. Die Testanwendung sollte diese Funktion aufrufen, wenn der Benutzer beschließt, den Browser zu beenden.

**Die Syntax**  
`void SecureBrowser.security.close(restart);`

**Parameter**  
* `restart` -Dieser Parameter wird ignoriert, jedoch muss angegeben werden.

**Hinweise** In Windows 10, Version 1607, muss das Gerät zunächst gesperrt werden. In späteren Versionen schließt diese Methode den Browser unabhängig davon, ob das Gerät gesperrt ist.

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
Die Testanwendung sollte diese Funktion aufrufen, um zu bestimmen, ob der eingeschränkte Modus aktiviert oder deaktiviert ist. Im eingeschränkten Modus lockert ein Browser erwartungsgemäß einige seiner strengen Sicherheitregeln, damit Hilfstechnologien mit dem sicheren Browser funktionieren. Browser, die die Benutzeroberflächen anderer Anwendungen strikt daran hindern, über diesen angezeigt zu werden, könnten dieses Verhalten im eingeschränkten Modus lockern. 

**Die Syntax**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**Parameter**  
* `callback` – Die Funktion, die aufgerufen wird, wenn dieser Aufruf abgeschlossen ist. Sie muss im Format `Function(Boolean permissiveMode)` vorliegen, wobei `permissiveMode` angibt, ob für den Browser aktuell der eingeschränkte Modus aktiviert ist. Wenn sie nicht definiert oder null ist, ist beim GET-Vorgang ein Fehler aufgetreten.

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
Die Testanwendung sollte diese Funktion aufrufen, um den eingeschränkten Modus zu aktivieren oder zu deaktivieren. Im eingeschränkten Modus lockert ein Browser erwartungsgemäß einige seiner strengen Sicherheitregeln, damit Hilfstechnologien mit dem sicheren Browser funktionieren. Browser, die die Benutzeroberflächen anderer Anwendungen strikt daran hindern, über diesen angezeigt zu werden, könnten dieses Verhalten im eingeschränkten Modus lockern. 

**Die Syntax**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**Parameter**  
* `enable` – Der boolescher Wert, der angibt, des Status des vorgesehenen einschränkend sein.  
* `callback` – Die Funktion, die aufgerufen wird, wenn dieser Aufruf abgeschlossen ist. Sie muss im Format `Function(Boolean permissiveMode)` vorliegen, wobei `permissiveMode` angibt, ob für den Browser aktuell der eingeschränkte Modus aktiviert ist. Wenn sie nicht definiert oder null ist, ist beim SET-Vorgang ein Fehler aufgetreten.

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
Löscht die Zwischenablage des Systems. Die Testanwendung sollte diese Funktion aufrufen, um zu erzwingen, dass alle Daten gelöscht werden, die unter Umständen in der Zwischenablage des Systems gespeichert sind. Dieser Vorgang wird auch von der Funktion **[lockDown](#lockDown)** ausgeführt.

**Die Syntax**  
`void SecureBrowser.security.emptyClipBoard();`

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
Ruft die Liste der MAC-Adressen für das Gerät ab. Die Testanwendung sollte diese Funktion aufrufen, um Unterstützung bei der Diagnose zu bieten. 

**Die Syntax**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**Parameter**  
* `callback` – Die Funktion, die aufgerufen wird, wenn dieser Aufruf abgeschlossen ist. Sie muss im Format `Function(String addressArray)` vorliegen, wobei `addressArray` das Format `"['00:11:22:33:44:55','etc']"` hat.

**"Hinweise"**  
Es ist schwierig, sich bei der Unterscheidung zwischen den Endnutzercomputern innerhalb der Testserver auf die Quell-IP-Adressen zu verlassen, da in Schulen für gewöhnlich Firewalls/NATs/Proxys verwendet werden. Anhand der MAC-Adressen kann die App für Diagnosezwecke zwischen den Endbenutzercomputern hinter einer gängigen Firewall unterscheiden.

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
Ruft die Zeit ab, zu der die Test-App gestartet wurde.

**Die Syntax**  
`DateTime SecureBrowser.settings.getStartTime();`

**zurück**  
Ein DateTime-Objekt, das den Zeitpunkt angibt, zu dem die Test-App gestartet wurde.

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
Fragt ab, ob eine Funktion aktiviert oder deaktiviert ist. 

**Die Syntax**  
`Object SecureBrowser.security.getCapability(String feature)`

**Parameter**  
`feature` -Die Zeichenfolge, um zu bestimmen, welche Funktion Abfrage. Gültige Funktionszeichenfolgen sind „screenMonitoring“, „printing“ und „textSuggestions“ (Groß-/Kleinschreibung spielt keine Rolle).

**Rückgabewert**  
Diese Funktion gibt ein JavaScript-Objekt oder Literalzeichen im folgenden Format zurück: `{<feature>:true|false}`. **true**, wenn die abgefragte Funktion aktiviert ist, **false**, wenn die Funktion nicht aktiviert oder die Funktionszeichenfolge ungültig ist.

**Anforderungen** Windows 10, Version 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
Aktiviert oder deaktiviert eine bestimmte Funktion des Browsers.

**Die Syntax**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**Parameter**  
* `feature` -Die Zeichenfolge, um zu bestimmen, welche Funktion festgelegt werden soll. Gültige Funktionszeichenfolgen sind `"screenMonitoring"`, `"printing"` und `"textSuggestions"` (Groß-/Kleinschreibung spielt keine Rolle).  
* `value` – Die gewünschte Einstellung für die Funktion. Muss entweder `"true"` oder `"false"` sein.  
* `onSuccess` -[optional] die Funktion, die aufgerufen wird, nachdem der Set-Vorgang erfolgreich abgeschlossen wurde. Sie muss im Format `Function(String jsonValue)` vorliegen, wobei *jsonValue* das Format `{<feature>:true|false|undefined}` hat.  
* `onError` -[optional] die Funktion, die aufgerufen wird, wenn die Set-Vorgang fehlgeschlagen ist. Sie muss im Format `Function(String jsonValue)` vorliegen, wobei *jsonValue* das Format `{<feature>:true|false|undefined}` hat.

**"Hinweise"**  
Wenn die Zielfunktion dem Browser nicht bekannt ist, übergibt diese Funktion den Wert `undefined` an die Rückruffunktion.

**Anforderungen** Windows 10, Version 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
Überprüft, ob die aktuelle Sitzung remote angemeldet ist.

**Die Syntax**  
`Boolean SecureBrowser.security.isRemoteSession();`

**Rückgabewert**  
**true**, wenn es sich bei der aktuellen Sitzung um eine Remotesitzung handelt, andernfalls **false**.

**Anforderungen an**  
Windows 10, Version 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
Überprüft, ob die aktuelle Sitzung auf einem virtuellen Computer ausgeführt wird.

**Die Syntax**  
`Boolean SecureBrowser.security.isVMSession();`

**Rückgabewert**  
**true**, wenn die aktuelle Sitzung auf einem virtuellen Computer ausgeführt wird, andernfalls **false**.

**"Hinweise"**  
Diese API-Prüfung erkennt nur VM Sitzungen, die in bestimmten Hypervisoren ausgeführt werden, die die entsprechenden APIs implementieren.

**Anforderungen an**  
Windows 10, Version 1709

---