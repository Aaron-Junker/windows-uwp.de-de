---
Description: Mit der JavaScript-API für die App „Prüfung“ von Microsoft können Sie zuverlässige Bewertungen durchführen. „Prüfung“ stellt einen sicheren Browser bereit, der die Lernenden daran hindert, während eines Tests andere Computer- oder Internet-Ressourcen zu verwenden.
title: JavaScript-API für Prüfung.
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Bildung
ms.localizationpriority: medium
ms.openlocfilehash: 2eeb190fc95e46a95813affd432948d38c0328a4
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218393"
---
# <a name="take-a-test-javascript-api"></a>JavaScript-API für Prüfung

[Test](/education/windows/take-tests-in-windows-10) ist eine browserbasierte UWP-APP, die gesperrte Online Bewertungen für Tests mit hoher Beteiligung rendert, sodass Dozenten sich auf die Bewertungs Inhalte konzentrieren können, anstatt eine sichere Testumgebung bereitzustellen. Um dies zu erreichen, wird eine JavaScript-API verwendet, die von jeder Webanwendung verwendet werden kann. Die Take-a-Test-API unterstützt den [SBAC-Browser-API-Standard](https://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) für allgemeine Kern Tests.

Weitere Informationen zur APP selbst finden Sie in der [technischen Referenz zum Erstellen einer Test-App](/education/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) . Hilfe zur Problembehandlung finden Sie unter [Problembehandlung bei Microsoft Prüfung mithilfe der Ereignisanzeige](troubleshooting.md).

## <a name="reference-documentation"></a>Referenzdokumentation
Die Take a Test-APIs sind in den folgenden Namespaces vorhanden. Beachten Sie, dass alle APIs von einem globalen Objekt abhängig sind `SecureBrowser` .

| Namespace | BESCHREIBUNG |
|-----------|-------------|
|[Security-Namespace](#security-namespace)|Enthält APIs, mit denen Sie das Gerät zum Testen Sperren und eine Testumgebung erzwingen können. |

### <a name="security-namespace"></a>Sicherheitsnamespace

Der Security-Namespace ermöglicht das Sperren des Geräts, das Überprüfen der Liste der Benutzer-und System Prozesse, das Abrufen von Mac-und IP-Adressen und das Löschen von zwischengespeicherten Webressourcen.

| Methode | BESCHREIBUNG   |
|--------|---------------|
|[Sperr](#lockDown) | Sperrt das Gerät zum Testen. |
|[isEnvironmentSecure](#isEnvironmentSecure) | Stellt fest, ob auf das Gerät noch der Sperrmodus-Kontext angewendet wird. |
|[getdebug-Info](#getDeviceInfo) | Ruft Details zur Plattform ab, auf der die Testanwendung ausgeführt wird. |
|[examineprocesslist](#examineProcessList)|Ruft die Liste der laufenden Benutzer-und System Prozesse ab.|
|[close](#close) | Schließt den Browser und entsperrt das Gerät. |
|[getpermissivemode](#getPermissiveMode)|Überprüft, ob der einschränkend Modus ein-oder ausgeschaltet ist.|
|[setpermissivemode](#setPermissiveMode)|Schaltet den Modus für den Modus ein oder aus.|
|[EmptyClipboard](#emptyClipBoard)|Löscht die System Zwischenablage.|
|[getMACAddress](#getMACAddress)|Ruft die Liste der MAC-Adressen für das Gerät ab.|
|[getstarttime](#getStartTime) | Ruft den Zeitpunkt ab, zu dem die Test-App gestartet wurde. |
|[getcapability](#getCapability) | Fragt ab, ob eine Funktion aktiviert oder deaktiviert ist. |
|[setcapability](#setCapability)|Aktiviert oder deaktiviert die angegebene Funktion.| 
|[IsRemoteSession](#isRemoteSession) | Überprüft, ob die aktuelle Sitzung Remote angemeldet ist. |
|[isvmsession](#isVMSession) | Überprüft, ob die aktuelle Sitzung auf einem virtuellen Computer ausgeführt wird. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>Sperr
Sperrt das Gerät. Wird auch zum Entsperren des Geräts verwendet. Die Test-Webanwendung ruft diesen Aufruf vor, damit Schüler und Studenten mit dem Testen beginnen können. Der Implementierer muss alle erforderlichen Maßnahmen ergreifen, um die Testumgebung zu sichern. Die Schritte zum Sichern der Umgebung sind gerätespezifisch und umfassen beispielsweise Aspekte wie das Deaktivieren von Bildschirmaufnahmen, das Deaktivieren des sprach Chats im sicheren Modus, das Löschen der Zwischenablage des Systems, das Wechseln in einen Kiosk Modus, das Deaktivieren von Leerzeichen in OSX 10.7 +-Geräten usw. Die Testanwendung aktiviert die Sperrung, bevor eine Bewertung beginnt, und deaktiviert die Sperrung, wenn der Student die Bewertung abgeschlossen hat und aus dem sicheren Test besteht.

**Syntax**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**Parameter**  
* `enable` - " **true** ", um die app "Take-a-Test" über dem Sperrbildschirm auszuführen und die in diesem [Dokument](/education/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396)erläuterten Richtlinien anzuwenden. **false** beendet die Ausführung von Take-a-Test über dem Sperrbildschirm und schließt Sie, es sei denn, die APP ist nicht gesperrt. in diesem Fall hat dies keine Auswirkungen.  
* `onSuccess` -[optional] die Funktion, die aufgerufen wird, nachdem die Sperrung erfolgreich aktiviert oder deaktiviert wurde. Sie muss das Format aufweisen `Function(Boolean currentlockdownstate)` .  
* `onError` -[optional] die Funktion, die aufgerufen werden soll, wenn der Sperr Vorgang fehlgeschlagen ist. Sie muss das Format aufweisen `Function(Boolean currentlockdownstate)` .  

**Anforderungen**  
Windows 10, Version 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
Stellt fest, ob auf das Gerät noch der Sperrmodus-Kontext angewendet wird. Die Test-Webanwendung wird dies aufrufen, bevor die Schüler/Studenten in der Regel in regelmäßigen Abständen getestet werden können.

**Syntax**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**Parameter**  
* `callback` -Die Funktion, die aufgerufen wird, wenn diese Funktion abgeschlossen ist. Sie muss das Format aufweisen, `Function(String state)` bei dem es `state` sich um eine JSON-Zeichenfolge mit zwei Feldern handelt. Das erste ist das `secure` Feld, das nur angezeigt wird, `true` Wenn alle erforderlichen Sperren aktiviert (oder Features deaktiviert) wurden, um eine sichere Testumgebung zu aktivieren, und keines dieser Elemente kompromittiert wurde, seit die app in den Sperrmodus wechselt. Das andere Feld, `messageKey` , enthält weitere Details oder Informationen, die Hersteller spezifisch sind. Die Absicht besteht darin, den Anbietern zu ermöglichen, zusätzliche Informationen hinzufügen, die das boolesche `secure` Flag erweitern:

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**Anforderungen**  
Windows 10, Version 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getdebug-Info
Ruft Details zur Plattform ab, auf der die Testanwendung ausgeführt wird. Hiermit werden alle Informationen erweitert, die vom Benutzer-Agent erkennbar sind.

**Syntax**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**Parameter**  
* `callback` -Die Funktion, die aufgerufen wird, wenn diese Funktion abgeschlossen ist. Sie muss das Format aufweisen, `Function(String infoObj)` bei dem es `infoObj` sich um eine JSON-Zeichenfolge mit mehreren Feldern handelt. Die folgenden Felder müssen unterstützt werden:
    * `os` stellt den Betriebs Systemtyp dar (z. b. Windows, macOS, Linux, Ios, Android usw.).
    * `name` stellt den Namen des Betriebssystem Releases dar (z. b. Sierra, Ubuntu).
    * `version` stellt die Betriebssystemversion dar (z. b. 10,1, 10 pro usw.)
    * `brand` stellt das sichere Browser Branding dar (z. b. "Eichen", "ca", "smarterapp" usw.).
    * `model` stellt das Gerätemodell nur für mobile Geräte dar. NULL/nicht verwendet für Desktop Browser.

**Anforderungen**  
Windows 10, Version 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineprocesslist
Ruft die Liste aller Prozesse ab, die auf dem Client Computer ausgeführt werden, der sich im Besitz des Benutzers befindet. Die Testanwendung ruft diese auf, um die Liste zu überprüfen, und vergleicht sie mit einer Liste von Prozessen, die während des Testzeitraums als schwarz aufgelistet angesehen wurden. Dieser Aufruf sollte sowohl zu Beginn einer Bewertung als auch regelmäßig aufgerufen werden, während der Student die Bewertung durch nimmt. Wenn ein schwarz aufgelisteter Prozess erkannt wird, sollte die Bewertung beendet werden, um die Test Integrität beizubehalten.

**Syntax**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**Parameter**  
* `blacklistedProcessList` : Die Liste der Prozesse, die von der Testanwendung aufgelistet wurden.  
`callback` -Die Funktion, die aufgerufen werden soll, sobald die aktiven Prozesse gefunden wurden. Sie muss das folgende Format aufweisen: `Function(String foundBlacklistedProcesses)` `foundBlacklistedProcesses` `"['process1.exe','process2.exe','processEtc.exe']"` . Er ist leer, wenn keine blackaufgelisteten Prozesse gefunden wurden. Wenn der Wert NULL ist, weist dies darauf hin, dass im ursprünglichen Funktions aufrufein Fehler aufgetreten ist.

**Anmerkung** Diese Liste enthält keine Systemprozesse.

**Anforderungen**  
Windows 10, Version 1709

---

<span id="close"/>

### <a name="close"></a>schließen
Schließt den Browser und entsperrt das Gerät. Die Testanwendung sollte diese aufrufen, wenn der Benutzer zum Beenden des Browsers entscheidet.

**Syntax**  
`void SecureBrowser.security.close(restart);`

**Parameter**  
* `restart` -Dieser Parameter wird ignoriert, muss jedoch angegeben werden.

**Hinweise** In Windows 10, Version 1607, muss das Gerät zunächst gesperrt werden. In späteren Versionen schließt diese Methode den Browser, unabhängig davon, ob das Gerät gesperrt ist.

**Anforderungen**  
Windows 10, Version 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getpermissivemode
Die Testwebanwendung sollte diese aufrufen, um zu bestimmen, ob der Modus für den Modus ein-oder ausgeschaltet ist. Im Zuordnungs Modus wird erwartet, dass ein Browser einige seiner strengen sicherheitshooks entspannt, damit Hilfstechnologien mit dem sicheren Browser funktionieren. Beispielsweise kann es vorkommen, dass Browser, die andere Anwendungs Benutzeroberflächen nicht in der obigen Darstellung verhindern, diese auch im Modus "Modus" lockern möchten. 

**Syntax**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**Parameter**  
* `callback` -Die Funktion, die aufgerufen wird, wenn dieser Aufruf abgeschlossen wird. Sie muss das folgende Format aufweisen: `Function(Boolean permissiveMode)` Where `permissiveMode` gibt an, ob sich der Browser derzeit im berechtigenden Modus befindet. Wenn der Wert nicht definiert oder NULL ist, ist ein Fehler beim Get-Vorgang aufgetreten.

**Anforderungen**  
Windows 10, Version 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setpermissivemode
Die Test-Webanwendung sollte diese aufrufen, um den Modus für den Modus ein-oder auszuschalten. Im Zuordnungs Modus wird erwartet, dass ein Browser einige seiner strengen sicherheitshooks entspannt, damit Hilfstechnologien mit dem sicheren Browser funktionieren. Beispielsweise kann es vorkommen, dass Browser, die andere Anwendungs Benutzeroberflächen nicht in der obigen Darstellung verhindern, diese auch im Modus "Modus" lockern möchten. 

**Syntax**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**Parameter**  
* `enable` : Der boolesche Wert, der den vorgesehenen Status des Zustands des Zustands angibt.  
* `callback` -Die Funktion, die aufgerufen wird, wenn dieser Aufruf abgeschlossen wird. Sie muss das folgende Format aufweisen: `Function(Boolean permissiveMode)` Where `permissiveMode` gibt an, ob sich der Browser derzeit im berechtigenden Modus befindet. Wenn Sie nicht definiert oder NULL ist, ist ein Fehler im Set-Vorgang aufgetreten.

**Anforderungen**  
Windows 10, Version 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>EmptyClipboard
Löscht die System Zwischenablage. Die Testanwendung sollte dies aufrufen, um zu erzwingen, dass alle Daten, die in der Zwischenablage des Systems gespeichert werden, gelöscht werden. Die **[Lockdown](#lockDown)** -Funktion führt auch diesen Vorgang aus.

**Syntax**  
`void SecureBrowser.security.emptyClipBoard();`

**Anforderungen**  
Windows 10, Version 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
Ruft die Liste der MAC-Adressen für das Gerät ab. Die Testanwendung sollte diese zur Unterstützung der Diagnose aufrufen. 

**Syntax**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**Parameter**  
* `callback` -Die Funktion, die aufgerufen wird, wenn dieser Aufruf abgeschlossen wird. Sie muss das folgende Format aufweisen: `Function(String addressArray)` `addressArray` `"['00:11:22:33:44:55','etc']"` .

**Anmerkungen**  
Es ist schwierig, sich auf Quell-IP-Adressen zu verlassen, um zwischen den Endbenutzer Computern innerhalb der Testserver zu unterscheiden, da Firewalls/NATs/Proxys in den Schulen häufig verwendet werden. Die Mac-Adressen ermöglichen der APP, Endclient Computer hinter einer gemeinsamen Firewall zu Diagnose Zwecken zu unterscheiden.

**Anforderungen**  
Windows 10, Version 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getstarttime
Ruft den Zeitpunkt ab, zu dem die Test-App gestartet wurde.

**Syntax**  
`DateTime SecureBrowser.security.getStartTime();`

**Return**  
Ein DateTime-Objekt, das den Zeitpunkt angibt, zu dem die Testanwendung gestartet wurde.

**Anforderungen**  
Windows 10, Version 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getcapability
Fragt ab, ob eine Funktion aktiviert oder deaktiviert ist. 

**Syntax**  
`Object SecureBrowser.security.getCapability(String feature)`

**Parameter**  
`feature` : Die Zeichenfolge, mit der bestimmt wird, welche Abfrage Abfrage möglich ist Gültige Funktions Zeichenfolgen sind "screenmonitoring", "Printing" und "Text Vorschläge" (ohne Beachtung der Groß-/Kleinschreibung).

**Rückgabewert**  
Diese Funktion gibt entweder ein JavaScript-Objekt oder ein Literalformat mit dem folgenden Format zurück: `{<feature>:true|false}` . **true** , wenn die abgefragte Funktion aktiviert ist, **false** , wenn die Funktion nicht aktiviert ist oder die Funktions Zeichenfolge ungültig ist.

**Anforderungen** Windows 10, Version 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setcapability
Aktiviert oder deaktiviert eine bestimmte Funktion im Browser.

**Syntax**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**Parameter**  
* `feature` : Die Zeichenfolge, die festlegt, welche Funktion festgelegt werden soll. Gültige Funktions Zeichenfolgen sind `"screenMonitoring"` , `"printing"` und `"textSuggestions"` (Groß-/Kleinschreibung nicht beachtet).  
* `value` : Die beabsichtigte Einstellung für das Feature. Er muss entweder `"true"` oder sein `"false"` .  
* `onSuccess` -[optional] die Funktion, die aufgerufen wird, nachdem der Set-Vorgang erfolgreich abgeschlossen wurde. Der Wert muss in der Form lauten, `Function(String jsonValue)` in der *JsonValue* folgendes Format hat: `{<feature>:true|false|undefined}` .  
* `onError` -[optional] die Funktion, die aufgerufen wird, wenn der Set-Vorgang fehlgeschlagen ist. Der Wert muss in der Form lauten, `Function(String jsonValue)` in der *JsonValue* folgendes Format hat: `{<feature>:true|false|undefined}` .

**Anmerkungen**  
Wenn die Zielfunktion dem Browser unbekannt ist, übergibt diese Funktion den Wert `undefined` an die Rückruffunktion.

**Anforderungen** Windows 10, Version 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>IsRemoteSession
Überprüft, ob die aktuelle Sitzung Remote angemeldet ist.

**Syntax**  
`Boolean SecureBrowser.security.isRemoteSession();`

**Rückgabewert**  
**true** , wenn die aktuelle Sitzung Remote ist, andernfalls **false**.

**Anforderungen**  
Windows 10, Version 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isvmsession
Überprüft, ob die aktuelle Sitzung innerhalb eines virtuellen Computers ausgeführt wird.

**Syntax**  
`Boolean SecureBrowser.security.isVMSession();`

**Rückgabewert**  
**true** , wenn die aktuelle Sitzung auf einem virtuellen Computer ausgeführt wird, andernfalls **false**.

**Anmerkungen**  
Mit dieser API-Überprüfung können nur VM-Sitzungen erkannt werden, die in bestimmten Hypervisoren ausgeführt werden, die die entsprechenden APIs implementieren.

**Anforderungen**  
Windows 10, Version 1709

---
