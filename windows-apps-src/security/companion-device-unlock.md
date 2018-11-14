---
title: Entsperren von Windows mit Windows Hello-Begleitgeräten (IoT)
description: Ein Windows Hello-Begleitgerät ist ein Gerät, das in Verbindung mit dem Windows10-Desktopgerät zur Verbesserung der Benutzerauthentifizierung verwendet werden kann. Mit dem Windows Hello-Begleitgeräteframework kann ein Begleitgerät umfangreiche Funktionen für Windows Hello bereitstellen, auch wenn Biometrie nicht verfügbar ist (beispielsweise, wenn das Windows10-Desktopgerät über keine Kamera für die Gesichtsauthentifizierung oder kein Fingerabdrucklesegerät verfügt).
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, Sicherheit
ms.assetid: 89f3d331-20cd-457b-83e8-1a22aaab2658
ms.localizationpriority: medium
ms.openlocfilehash: 1bf1ae02a1c1937c02b6c750872dc784d28cd8e0
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "6201355"
---
# <a name="windows-unlock-with-windows-hello-companion-iot-devices"></a>Entsperren von Windows mit Windows Hello-Begleitgeräten (IoT)

Ein Windows Hello-Begleitgerät ist ein Gerät, das in Verbindung mit dem Windows10-Desktopgerät zur Verbesserung der Benutzerauthentifizierung verwendet werden kann. Mit dem Windows Hello-Begleitgeräteframework kann ein Begleitgerät umfangreiche Funktionen für Windows Hello bereitstellen, auch wenn Biometrie nicht verfügbar ist (beispielsweise, wenn das Windows10-Desktopgerät über keine Kamera für die Gesichtsauthentifizierung oder kein Fingerabdrucklesegerät verfügt).

> **Hinweis** Das Windows Hello-Begleitgeräteframework ist ein spezielles Feature und nicht für alle App-Entwickler verfügbar. Damit dieses Framework verwendet werden kann, muss Ihre App speziell von Microsoft bereitgestellt werden und die eingeschränkte Funktion *SecondaryAuthenticatorFactor* muss im Manifest angegeben sein. Um eine Genehmigung zu erhalten, wenden Sie sich an [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com).

## <a name="introduction"></a>Einführung

> Eine Videoübersicht finden Sie in der Sitzung [Windows Unlock with IoT Devices](https://channel9.msdn.com/Events/Build/2016/P491) von Build 2016 in Channel 9.

> Codebeispiele finden Sie im [Windows Hello-Begleitgeräteframework-Github-Repository](https://github.com/Microsoft/companion-device-framework).

### <a name="use-cases"></a>Anwendungsfälle

Das Windows Hello-Begleitgeräteframework kann auf unterschiedliche Weise verwendet werden, um eine erstklassige Windows-Entsperrung mit einem Begleitgerät zu erstellen. Beispiele:

- Benutzer können Ihr Begleitgerät per USB am PC anschließen, auf die Schaltfläche des Begleitgeräts tippen und den PC automatisch entsperren.
- Benutzer können in ihrer Tasche ein Smartphone mit sich führen, das bereits über Bluetooth mit dem PC gekoppelt ist. Durch Drücken der LEERTASTE des PCs wird eine Benachrichtigung an ihr Smartphone gesendet. Diese muss zum Entsperren des PCs einfach nur bestätigt werden.
- Benutzer können das Begleitgerät an ein NFC-Lesegerät halten und den PC so entsperren.
- Benutzer können ein Fitness-Armband tragen, das den Träger bereits authentifiziert hat. Wenn sich der Benutzer dem PC nähert und eine spezielle Geste (beispielsweise Klatschen) ausführt, wird der PC entsperrt.

### <a name="biometric-enabled-windows-hello-companion-devices"></a>Biometriefähige Windows Hello-Begleitgeräte

Falls das Begleitgerät über Biometrieunterstützung verfügt, ist das [Windows-Biometrieframework](https://msdn.microsoft.com/library/windows/hardware/mt608302(v=vs.85).aspx) in manchen Fällen eine bessere Lösung als das Windows Hello-Begleitgeräteframework. Wenden Sie sich an [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com), und wir helfen Ihnen bei der Auswahl des richtigen Ansatzes.

### <a name="components-of-the-solution"></a>Komponenten der Lösung

Das folgende Diagramm zeigt die Komponenten der Lösung und wer jeweils für deren Erstellung zuständig ist.

![Frameworkübersicht](images/companion-device-1.png)

Das Windows Hello-Begleitgeräteframework wird als Dienst unter Windows implementiert, der in diesem Artikel als Begleitauthentifizierungsdienst bezeichnet wird. Dieser Dienst generiert ein Entsperrtoken, das durch einen auf dem Windows Hello-Begleitgerät gespeicherten HMAC-Schlüssel geschützt werden muss. Dadurch wird sichergestellt, dass für den Zugriff auf das Entsperrtoken das Windows Hello-Begleitgerät benötigt wird. Pro Tupel (PC, Windows-Benutzer) ist jeweils ein eindeutiges Entsperrtoken vorhanden.

Die Integration des Windows Hello-Begleitgeräteframeworks erfordert Folgendes:

- Eine aus dem Windows Store heruntergeladene, für das Windows Hello-Begleitgerät bestimmte Begleitgeräte-App für die [universelle Windows-Plattform (UWP)](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide). 
- Die Möglichkeit, auf dem Windows Hello-Begleitgerät zwei 256-Bit-HMAC-Schlüssel zu erstellen und damit HMAC (mit SHA-256) zu generieren.
- Ordnungsgemäß konfigurierte Sicherheitsreinstellungen auf dem Windows10-Desktopgerät. Bevor Windows Hello-Begleitgeräte eingebunden werden können, muss diese PIN für den Begleitauthentifizierungsdienst eingerichtet werden. Die Benutzer müssen über „Einstellungen“ > „Konten“ > „Anmeldeoptionen“ eine PIN einrichten.

Zusätzlich zu den oben genannten Anforderungen ist die Windows Hello-Begleitgeräte-App für Folgendes zuständig:

- Benutzeroberfläche und Branding der ersten Registrierung und spätere Aufhebung der Registrierung des Windows Hello-Begleitgeräts
- Ausführung im Hintergrund, Erkennung des Windows Hello-Begleitgeräts, Kommunikation mit dem Windows Hello-Begleitgerät und Begleitauthentifizierungsdienst
- Fehlerbehandlung

In der Regel werden Begleitgeräte mit einer Ersteinrichtungs-App (beispielsweise für die Ersteinrichtung eines Fitnessarmbands) ausgeliefert. Die in diesem Dokument beschriebenen Funktionen können Teil dieser App sein, sodass keine separate App erforderlich ist.  

### <a name="user-signals"></a>Benutzersignale

Jedes Windows Hello-Begleitgerät muss mit einer App kombiniert werden, die drei Benutzersignale unterstützt. Bei diesen Signalen kann es sich um eine Aktion oder um eine Geste handeln.

- **Absichtssignal**: Ermöglicht dem Benutzer, seine Entsperrabsicht deutlich zu machen (beispielsweise durch Drücken einer Taste auf dem Windows Hello-Begleitgerät). Das Absichtssignal muss auf dem **Windows Hello-Begleitgerät** erfasst werden.
- **Benutzeranwesenheitssignal**: Bestätigt die Anwesenheit des Benutzers. Das Windows Hello-Begleitgerät kann beispielsweise eine PIN anfordern, bevor es zum Entsperren des PCs verwendet werden kann (nicht zu verwechseln mit einer PC-PIN), oder es ist unter Umständen ein Tastendruck erforderlich.
- **Mehrdeutigkeitsvermeidungssignal**: Gibt an, welches Windows10-Desktopgerät der Benutzer entsperren möchte, wenn dem Windows Hello-Begleitgerät mehrere Optionen zur Verfügung stehen.

Von diesen Benutzersignalen kann eine beliebige Anzahl zu einem einzelnen Signal kombiniert werden. Benutzeranwesenheits- und Absichtssignale sind bei jeder Verwendung erforderlich.

### <a name="registration-and-future-communication-between-a-pc-and-windows-hello-companion-devices"></a>Registrierung und zukünftige Kommunikation zwischen einem PC und Windows Hello-Begleitgeräten

Damit ein Windows Hello-Begleitgerät in das Windows Hello-Begleitgeräteframework eingebunden werden kann, muss es zunächst beim Framework registriert werden. Für den Registrierungsprozess ist allein die Windows Hello-Begleitgeräte-App zuständig.

Bei der Beziehung zwischen dem Windows Hello-Begleitgerät und dem Windows10-Desktopgerät kann es sich um eine 1:n-Beziehung handeln. (Ein Begleitgerät kann also für viele Windows10-Desktopgeräte verwendet werden.) Jedes Windows Hello-Begleitgerät kann jedoch nur für einen einzelnen Benutzer auf dem jeweiligen Windows10-Desktopgerät verwendet werden.   

Damit ein Windows Hello-Begleitgerät mit einem PC kommunizieren kann, müssen sich beide auf einen Transport einigen. Die Entscheidung wird von der Windows Hello-Begleitgeräte-App getroffen. Das Windows Hello-Begleitgeräteframework schränkt weder den Transporttyp (USB, NFC, WLAN, Bluetooth, BLE usw.) noch das Protokoll zwischen Windows Hello-Begleitgerät und Windows Hello-Begleitgeräte-App des Windows10-Desktopgeräts ein. Es schlägt jedoch bestimmte Sicherheitsaspekte für die Transportschicht vor (wie in diesem Dokument im Abschnitt „Sicherheitsanforderungen“ beschrieben). Für die Erfüllung dieser Anforderungen ist der Geräteanbieter verantwortlich. Sie werden nicht vom Framework bereitgestellt.


## <a name="user-interaction-model"></a>Benutzerinteraktionsmodell

### <a name="windows-hello-companion-device-app-discovery-installation-and-first-time-registration"></a>Erkennung, Installation und erstmalige Registrierung der Windows Hello-Begleitgeräte-App

Im Anschluss sehen Sie einen typischen Benutzerworkflow:

- Der Benutzer richtet die PIN auf jedem der Windows10-Zieldesktopgeräte ein, die er mit dem Windows Hello-Begleitgerät entsperren möchte.
- Der Benutzer führt auf dem Windows10-Desktopgerät die Windows Hello-Begleitgeräte-App aus, um sein Windows Hello-Begleitgerät bei dem Windows10-Desktopgerät zu registrieren.

Hinweise:

- Es empfiehlt sich, die Erkennung, das Herunterladen und das Starten der Windows Hello-Begleitgeräte-App zu optimieren und nach Möglichkeit zu automatisieren, sodass die App beispielsweise aufseiten des Windows10-Desktopgeräts durch Tippen auf das Windows Hello-Begleitgerät auf einem NFC-Lesegerät heruntergeladen werden kann. Dies ist allerdings Aufgabe des Windows Hello-Begleitgeräts und der Windows Hello-Begleitgeräte-App.
- In einer Unternehmensumgebung kann die Windows Hello-Begleitgeräte-App per MDM bereitgestellt werden.
- Fehlermeldungen, die unter Umständen im Rahmen der Registrierung auftreten, müssen dem Benutzer von der Windows Hello-Begleitgeräte-App angezeigt werden.

### <a name="registration-and-de-registration-protocol"></a>Protokoll für Registrierung und Registrierungsaufhebung

Das folgende Diagramm zeigt, wie das Windows Hello-Begleitgerät bei der Registrierung mit dem Begleitauthentifizierungsdienst interagiert.  

![Registrierungsfluss](images/companion-device-2.png)

In unserem Protokoll werden zwei Schlüssel verwendet:

- Geräteschlüssel (**devicekey**): Dient zum Schutz von Entsperrtoken, die der PC zum Entsperren von Windows benötigt.
- Authentifizierungsschlüssel (**authkey**): Dient zur gegenseitigen Authentifizierung von Windows Hello-Begleitgerät und Begleitauthentifizierungsdienst.

Der Geräteschlüssel und die Authentifizierungsschlüssel werden bei der Registrierung zwischen Windows Hello-Begleitgeräte-App und dem Windows Hello-Begleitgerät ausgetauscht. Zum Schutz der Schlüssel müssen die Windows Hello-Begleitgeräte-App und das Windows Hello-Begleitgerät daher einen sicheren Transport verwenden.

Beachten Sie außerdem Folgendes: Im obigen Diagramm ist zwar zu sehen, dass zwei HMAC-Schlüssel auf dem Windows Hello-Begleitgerät generiert werden, diese können jedoch auch von der App generiert und zur Speicherung an das Windows Hello-Begleitgerät gesendet werden.

### <a name="starting-authentication-flows"></a>Starten von Authentifizierungsflüssen

Der Anmeldefluss für Windows10-Desktopgeräte kann vom Benutzer (durch ein Absichtssignal) über das Windows Hello-Begleitgeräteframework auf zwei Arten gestartet werden:

- Durch Öffnen des Deckels (Laptop) oder durch Drücken der LEERTASTE oder Wischen nach oben (PC)
- Durch eine Geste oder eine Aktion auf dem Windows Hello-Begleitgerät

Welche Option als Ausgangspunkt verwendet wird, liegt beim Windows Hello-Begleitgerät. Das Windows Hello-Begleitgeräteframework informiert die Begleitgeräte-App, wenn der erste Fall eintritt. Im zweiten Fall muss die Windows Hello-Begleitgeräte-App das Begleitgerät abfragen, um zu ermitteln, ob das Ereignis erfasst wurde. Dadurch wird sichergestellt, dass das Windows Hello-Begleitgerät vor dem erfolgreichen Entsperren das Absichtssignal erfasst.

### <a name="windows-hello-companion-device-credential-provider"></a>Anmeldeinformationsanbieter für Windows Hello-Begleitgeräte

Windows10 verfügt über einen neuen Anmeldeinformationsanbieter zur Behandlung sämtlicher Windows Hello-Begleitgeräte.

Der Anmeldeinformationsanbieter für Windows Hello-Begleitgeräte startet die Begleitgerät-Hintergrundaufgabe mittels Aktivierung eines Triggers. Der Trigger wird erstmals festgelegt, wenn der PC aktiviert und ein Sperrbildschirm angezeigt wird. Das zweite Mal erfolgt, wenn der PC die Anmeldebenutzeroberfläche anzeigt und der Anmeldeinformationsanbieter für Windows Hello-Begleitgeräte die ausgewählte Kachel ist.

Die Hilfsbibliothek für die Windows Hello-Begleitgeräte-App lauscht auf die Änderung des Sperrbildschirmstatus und sendet das Ereignis, das der Windows Hello-Begleitgerät-Hintergrundaufgabe entspricht.

Sollten mehrere Windows Hello-Begleitgerät-Hintergrundaufgaben vorhanden sein, wird der PC durch die erste Hintergrundaufgabe entsperrt, die den Authentifizierungsprozess abgeschlossen hat. Alle übrigen Authentifizierungsaufrufe werden vom Begleitgerät-Authentifizierungsdienst ignoriert.

Die Vorgänge aufseiten des Windows Hello-Begleitgeräts werden von der Windows Hello-Begleitgeräte-App abgewickelt und verwaltet. Das Windows Hello-Begleitgeräteframework hat keinen Einfluss auf diesen Teil der Benutzererfahrung. Genauer gesagt: Der Begleitauthentifizierungsanbieter informiert die Windows Hello-Begleitgeräte-App (mittels der dazugehörigen Hintergrund-App) über Zustandsänderungen der Anmeldebenutzeroberfläche (wie Einblendung des Sperrbildschirms oder Ausblendung des Sperrbildschirms durch Drücken der LEERTASTE), und die Windows Hello-Begleitgeräte-App muss daraufhin mit einer entsprechenden Benutzererfahrung reagieren (beispielsweise, indem nach dem Drücken der LEERTASTE und Ausblenden des Sperrbildschirms über USB nach dem Gerät gesucht wird).

Das Windows Hello-Begleitgeräteframework stellt eine Reihe (lokalisierter) Text- und Fehlermeldungen für die Windows Hello-Begleitgeräte-App zur Verfügung. Diese werden im Vordergrund des Sperrbildschirms (oder auf der Anmeldebenutzeroberfläche) angezeigt. Weitere Informationen finden Sie im Abschnitt „Umgang mit Meldungen und Fehlern“.

### <a name="authentication-protocol"></a>Authentifizierungsprotokoll

Nachdem die mit einer Windows Hello-Begleitgeräte-App verknüpfte Hintergrundaufgabe per Trigger gestartet wurde, muss sie die Unterstützung des Windows Hello-Begleitgeräts bei der Überprüfung eines durch den Begleitauthentifizierungsdienst berechneten HMAC-Werts und bei der Berechnung zweier HMAC-Werte anfordern:
- Dienst-HMAC überprüfen = HMAC(Authentifizierungsschlüssel, Dienst-Nonce || Geräte-Nonce || Sitzungs-Nonce).
- Berechnen Sie den HMAC des Geräteschlüssels mit einer Nonce.
- Berechnen Sie den HMAC des Authentifizierungsschlüssels mit dem ersten HMAC-Wert, verkettet mit einer durch den Begleitauthentifizierungsdienst generierten Nonce.

Der zweite berechnete Wert wird vom Dienst nicht nur zur Authentifizierung des Geräts, sondern auch zur Verhinderung von Replay-Angriffen im Transportkanal verwendet.

![Registrierungsfluss](images/companion-device-3.png)

## <a name="lifecycle-management"></a>Lebenszyklusverwaltung

### <a name="register-once-use-everywhere"></a>Einmalige Registrierung, ortsunabhängige Verwendung

Ohne Back-End-Server müssen Benutzer ihr Windows Hello-Begleitgerät separat bei jedem Windows10-Desktopgerät registrieren.

Eine Begleitgerätehersteller oder OEM kann einen Webdienst implementieren, um den Registrierungszustand für mehrere Windows10-Desktopgeräte oder mobile Geräte des Benutzers zur Verfügung zu stellen. Ausführlichere Informationen finden Sie im Abschnitt „Roaming, Sperrung und Filterdienst“.

### <a name="pin-management"></a>PIN-Verwaltung

Bevor ein Begleitgerät verwendet werden kann, muss auf dem Windows10-Desktopgerät eine PIN eingerichtet werden. Dadurch wird sichergestellt, dass der Benutzer über eine Ausweichmöglichkeit verfügt, falls das Windows Hello-Begleitgerät nicht funktioniert. Die PIN wird von Windows verwaltet und den Apps gegenüber nie offengelegt. Sie kann unter „Einstellungen“ > „Konten“ > „Anmeldeoptionen“ geändert werden.

### <a name="management-and-policy"></a>Verwaltung und Richtlinie

Benutzer können ein Windows Hello-Begleitgerät von einem Windows10-Desktopgerät entfernen, indem sie die Windows Hello-Begleitgeräte-App auf dem entsprechenden Desktopgerät ausführen.

Unternehmen stehen zum Steuern des Windows Hello-Begleitgeräteframeworks zwei Optionen zur Verfügung:

- Aktivieren oder Deaktivieren des Features
- Definieren einer Whitelist mit Windows Hello-Begleitgeräten, die das Windows-App-Schließfach verwenden dürfen

Das Windows Hello-Begleitgeräteframework unterstützt keine zentralisierte Inventarisierung verfügbarer Begleitgeräte und keine Methode zur weiteren Filterung der zulässigen Instanzen eines Windows Hello-Begleitgerätetyps. (So kann beispielsweise nicht festgelegt werden, dass nur Begleitgeräte mit einer Seriennummer zwischen X und Y zulässig sind.) App-Entwickler können solche Funktionen jedoch über einen eigenen Dienst bereitstellen. Ausführlichere Informationen finden Sie im Abschnitt „Roaming, Sperrung und Filterdienst“.

### <a name="revocation"></a>Sperrung

Die Remoteentfernung eines Begleitgeräts von einem bestimmten Windows10-Desktopgerät wird vom Windows Hello-Begleitgeräteframework nicht unterstützt. Stattdessen können Benutzer das Windows Hello-Begleitgerät über die auf dem Windows10-Desktopgerät ausgeführte Windows Hello-Begleitgeräte-App entfernen.

Begleitgerätehersteller können jedoch einen Dienst erstellen, der eine Remotesperrung ermöglicht. Ausführlichere Informationen finden Sie im Abschnitt „Roaming, Sperrung und Filterdienst“.

### <a name="roaming-and-filter-services"></a>Roaming- und Filterdienste

Begleitgerätehersteller können einen Webdienst implementieren, der für folgende Szenarien verwendet werden kann:

- Filterdienst für Unternehmen: Ein Unternehmen kann die Windows Hello-Begleitgeräte, die in seiner Umgebung verwendet werden können, auf eine kleine Gruppe von Geräten eines bestimmten Herstellers beschränken. So kann das Unternehmen Contoso beispielsweise beim HerstellerX 10.000Begleitgeräte des ModellsY bestellen und sicherstellen, dass in der Contoso-Domäne ausschließlich diese Geräte (und kein anderes Gerätemodell des HerstellersX) verwendet werden können.
- Inventarisierung: Ein Unternehmen kann eine Liste mit vorhandenen Begleitgeräten erstellen, die in einer Unternehmensumgebung verwendet werden.
- Echtzeitsperrung: Wenn ein Mitarbeiter sein Begleitgerät als verloren oder gestohlen meldet, kann dieses Gerät über den Webdienst gesperrt werden.
- Roaming: Ein Benutzer muss sein Begleitgerät lediglich einmal registrieren und kann es danach für alle seine Desktopgeräte und mobilen Geräte unter Windows10 verwenden.

Zur Implementierung dieser Features muss die Windows Hello-Begleitgeräte-App bei der Registrierung und Verwendung mit dem Webdienst kommunizieren. Die Windows Hello-Begleitgeräte-App kann zur Optimierung in Anmeldeszenarien mit Zwischenspeicherung verwendet werden, sodass beispielsweise nur einmal täglich mit dem Webdienst kommuniziert werden muss. (Dadurch verlängert sich allerdings die Zeit für die Sperrung auf bis zu einen Tag.)  

## <a name="windows-hello-companion-device-framework-api-model"></a>API-Modell des Windows Hello-Begleitgeräteframeworks

### <a name="overview"></a>Übersicht

Eine Windows Hello-Begleitgeräte-App muss zwei Komponenten umfassen: eine Vordergrund-App mit Benutzeroberfläche zum Registrieren und Aufheben der Registrierung des Geräts sowie eine Hintergrundaufgabe für die Authentifizierung.

Der gesamte API-Fluss sieht wie folgt aus:

1. Registrieren Sie das Windows Hello-Begleitgerät.
    * Überprüfen Sie, ob sich das Gerät in Reichweite befindet, und fragen Sie dessen Funktionen ab (falls erforderlich).
    * Generieren Sie zwei HMAC-Schlüssel (entweder aufseiten des Begleitgeräts oder aufseiten der App).
    * Rufen Sie „RequestStartRegisteringDeviceAsync“ auf.
    * Rufen Sie „FinishRegisteringDeviceAsync“ auf.
    * Stellen Sie sicher, dass die Windows Hello-Begleitgeräte-App die HMAC-Schlüssel speichert (sofern unterstützt) und die Kopien verwirft.
2. Registrieren Sie die Hintergrundaufgabe.
3. Warten Sie auf das passende Ereignis in der Hintergrundaufgabe.
    * WaitingForUserConfirmation: Warten Sie auf dieses Ereignis, wenn zum Starten des Authentifizierungsablaufs eine Benutzeraktion/-geste auf dem Windows Hello-Begleitgerät erforderlich ist.
    * CollectingCredential: Warten Sie auf dieses Ereignis, wenn der Start des Authentifizierungsablaufs auf einer Benutzeraktion/-geste auf dem PC (beispielsweise Drücken der LEERTASTE) beruht.
    * Anderer Trigger (beispielsweise eine Smartcard): Fragen Sie den aktuellen Authentifizierungszustand ab, um die richtigen APIs aufzurufen.
4. Halten Sie den Benutzer durch Aufrufen von „ShowNotificationMessageAsync“ über Fehlermeldungen oder nächste Schritte auf dem Laufenden. Rufen Sie diese API erst auf, nachdem ein Absichtssignal erfasst wurde.
5. Entsperrung
    * Vergewissern Sie sich, dass Absichts- und Benutzeranwesenheitssignale erfasst wurden.
    * Rufen Sie „StartAuthenticationAsync“ auf.
    * Kommunizieren Sie mit dem Begleitgerät, um die erforderlichen HMAC-Vorgänge auszuführen.
    * Rufen Sie „FinishAuthenticationAsync“ auf.
6. Heben Sie auf Wunsch des Benutzers die Registrierung eines Windows Hello-Begleitgeräts auf (etwa, wenn der Benutzer sein Begleitgerät verloren hat).
    * Führen Sie das Windows Hello-Begleitgerät für den angemeldeten Benutzer mittels „FindAllRegisteredDeviceInfoAsync“ auf.
    * Heben Sie die Registrierung mithilfe von „UnregisterDeviceAsync“ auf.

### <a name="registration-and-de-registration"></a>Registrierung und Registrierungsaufhebung

Die Registrierung erfordert zwei API-Aufrufe an den Begleitauthentifizierungsdienst: „RequestStartRegisteringDeviceAsync“ und „FinishRegisteringDeviceAsync“.

Bevor diese Aufrufe vorgenommen werden können, muss die Windows Hello-Begleitgeräte-App überprüfen, ob das Windows Hello-Begleitgerät verfügbar ist. Wenn das Windows Hello-Begleitgerät für die Generierung der HMAC-Schlüssel (Authentifizierungs- und Geräteschlüssel) zuständig ist, muss die Windows Hello-Begleitgeräte-App außerdem das Begleitgerät zur Generierung der Schlüssel auffordern, bevor einer der beiden obigen Aufrufe durchgeführt wird. Wenn die Windows Hello-Begleitgeräte-App für die Generierung der HMAC-Schlüssel zuständig ist, muss dieser Schritt vor den beiden obigen Aufrufen durchgeführt werden.

Im Zuge des ersten API-Aufrufs (RequestStartRegisteringDeviceAsync) muss die Windows Hello-Begleitgeräte-App außerdem die Gerätefunktionen ermitteln (beispielsweise, ob die Möglichkeit zum sicheren Speichern von HMAC-Schlüsseln besteht) und sie ggf. im Rahmen des API-Aufrufs übergeben. Wenn mit der gleichen Windows Hello-Begleitgeräte-App mehrere Versionen des gleichen Begleitgeräts verwaltet werden und sich diese Funktionen ändern (und dies mithilfe einer Geräteabfrage ermittelt werden muss), empfiehlt es sich, die entsprechende Abfrage vor dem ersten API-Aufruf durchzuführen.   

Die erste API (RequestStartRegisteringDeviceAsync) gibt ein Handle zurück, das von der zweiten API (FinishRegisteringDeviceAsync) verwendet wird. Der erste Aufruf für die Registrierung startet die PIN-Abfrage, um sicherzustellen, dass der Benutzer anwesend ist. Ist keine PIN eingerichtet, ist dieser Aufruf nicht erfolgreich. Über einen Aufruf von „KeyCredentialManager.IsSupportedAsync“ kann die Windows Hello-Begleitgeräte-App auch abfragen, ob eine PIN eingerichtet ist. Bei dem Aufruf von „RequestStartRegisteringDeviceAsync“ kann auch ein Fehler auftreten, falls die Verwendung des Windows Hello-Begleitgeräts per Richtlinie deaktiviert wurde.

Das Ergebnis des ersten Aufrufs wird über die Enumeration „SecondaryAuthenticationFactorRegistrationStatus“ zurückgegeben:

```C#
{
    Failed = 0,         // Something went wrong in the underlying components
    Started,            // First call succeeded
    CanceledByUser,     // User cancelled PIN prompt
    PinSetupRequired,   // PIN is not set up
    DisabledByPolicy,   // Companion device framework or this app is disabled
}
```

Mit dem zweiten Aufruf (FinishRegisteringDeviceAsync) wird die Registrierung abgeschlossen. Im Rahmen des Registrierungsprozesses kann die Windows Hello-Begleitgeräte-App Konfigurationsdaten des Begleitgeräts mit dem Begleitauthentifizierungsdienst speichern. Für diese Daten gilt eine Größenbeschränkung von 4KB. Die Daten stehen der Windows Hello-Begleitgeräte-App bei der Authentifizierung zur Verfügung. Diese Daten können etwa zum Herstellen einer Verbindung mit dem Windows Hello-Begleitgerät verwendet werden (beispielsweise im Falle einer MAC-Adresse). Ein weiteres Verwendungsbeispiel für Konfigurationsdaten wäre die Verwendung des PCs als Speicherort, falls das Windows Hello-Begleitgerät über keine Speichermöglichkeit verfügt. Beachten Sie, dass in den Konfigurationsdaten gespeicherte vertrauliche Daten mit einem Schlüssel verschlüsselt werden müssen, der nur dem Windows Hello-Begleitgerät bekannt ist. Wenn Konfigurationsdaten von einem Windows-Dienst gespeichert werden, stehen sie der Windows Hello-Begleitgeräte-App außerdem benutzerprofilübergreifend zur Verfügung.

Die Windows Hello-Begleitgeräte-App kann „AbortRegisteringDeviceAsync“ aufrufen, um die Registrierung abzubrechen und einen Fehlercode zu übergeben. Der Begleitauthentifizierungsdienst protokolliert den Fehler in den Telemetriedaten. Ein gutes Beispiel für diesen Aufruf wäre etwa, wenn die Registrierung aufgrund eines Windows Hello-Begleitgerätefehlers nicht abgeschlossen werden konnte (beispielsweise, weil die HMAC Schlüssel nicht gespeichert werden können oder die Bluetooth-Verbindung unterbrochen wurde).

Die Windows Hello-Begleitgeräte-App muss dem Benutzer eine Option zur Verfügung stellen, über die er die Registrierung des Windows Hello-Begleitgeräts bei seinem Windows10-Desktopgerät aufheben kann (beispielsweise, wenn er sein Begleitgerät verloren oder eine neuere Version gekauft hat). Bei Verwendung dieser Option muss die Windows Hello-Begleitgeräte-App „UnregisterDeviceAsync“ aufrufen. Dieser Aufruf durch die Windows Hello-Begleitgeräte-App sorgt dafür, dass der Begleitgeräte-Authentifizierungsdienst aufseiten des PCs alle Daten (einschließlich der HMAC-Schlüssel) löscht, die mit der speziellen Geräte- und App-ID zusammenhängen. Dieser API-Aufruf versucht nicht, HMAC-Schlüssel aus der Windows Hello-Begleitgeräte-App oder aufseiten des Begleitgeräts zu löschen. Dies muss in der Windows Hello-Begleitgeräte-App implementiert werden.

Sämtliche Fehlermeldungen der Registrierungs- und Registrierungsaufhebungsphase müssen von der Windows Hello-Begleitgeräte-App angezeigt werden.

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.UI.Popups;

namespace SecondaryAuthFactorSample
{
    public class DeviceRegistration
    {

        public void async OnRegisterButtonClick()
        {
            //
            // Pseudo function, the deviceId should be retrieved by the application from the device
            //
            string deviceId = await ReadSerialNumberFromDevice();

            IBuffer deviceKey = CryptographicBuffer.GenerateRandom(256/8);
            IBuffer mutualAuthenticationKey = CryptographicBuffer.GenerateRandom(256/8);

            SecondaryAuthenticationFactorRegistration registrationResult =
                await SecondaryAuthenticationFactorRegistration.RequestStartRegisteringDeviceAsync(
                    deviceId,  // deviceId: max 40 wide characters. For example, serial number of the device
                    SecondaryAuthenticationFactorDeviceCapabilities.SecureStorage |
                        SecondaryAuthenticationFactorDeviceCapabilities.HMacSha256 |
                        SecondaryAuthenticationFactorDeviceCapabilities.StoreKeys,
                    "My test device 1", // deviceFriendlyName: max 64 wide characters. For example: John's card
                    "SAMPLE-001", // deviceModelNumber: max 32 wide characters. The app should read the model number from device.
                    deviceKey,
                    mutualAuthenticationKey);

            switch(registerResult.Status)
            {
            case SecondaryAuthenticationFactorRegistrationStatus.Started:
                //
                // Pseudo function:
                // The app needs to retrieve the value from device and set into opaqueBlob
                //
                IBuffer deviceConfigData = ReadConfigurationDataFromDevice();

                if (deviceConfigData != null)
                {
                    await registrationResult.Registration.FinishRegisteringDeviceAsync(deviceConfigData); //config data limited to 4096 bytes
                    MessageDialog dialog = new MessageDialog("The device is registered correctly.");
                    await dialog.ShowAsync();
                }
                else
                {
                    await registrationResult.Registration.AbortRegisteringDeviceAsync("Failed to connect to the device");
                    MessageDialog dialog = new MessageDialog("Failed to connect to the device.");
                    await dialog.ShowAsync();
                }
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.CanceledByUser:
                MessageDialog dialog = new MessageDialog("You didn't enter your PIN.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.PinSetupRequired:
                MessageDialog dialog = new MessageDialog("Please setup PIN in settings.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.DisabledByPolicy:
                MessageDialog dialog = new MessageDialog("Your enterprise prevents using this device to sign in.");
                await dialog.ShowAsync();
                break;
            }
        }

        public void async UpdateDeviceList()
        {
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticationFactorDeviceFindScope.User);

            if (deviceInfoList.Count > 0)
            {
                foreach (SecondaryAuthenticationFactorInfo deviceInfo in deviceInfoList)
                {
                    //
                    // Add deviceInfo.FriendlyName and deviceInfo.DeviceId into a combo box
                    //
                }
            }
        }

        public void async OnUnregisterButtonClick()
        {
            string deviceId;
            //
            // Read the deviceId from the selected item in the combo box
            //
            await SecondaryAuthenticationFactorRegistration.UnregisterDeviceAsync(deviceId);
        }
    }
}
```

### <a name="authentication"></a>Authentifizierung

Die Authentifizierung erfordert zwei API-Aufrufe an den Begleitauthentifizierungsdienst: „StartAuthenticationAsync“ und „FinishAuthencationAsync“.

Die erste Initiierungs-API gibt ein Handle zurück, das dann von der zweiten API verwendet wird.  Der erste Aufruf gibt unter anderem eine Nonce zurück, für die (nach Verkettung mit anderen Elementen) ein HMAC-Vorgang mit dem auf dem Windows Hello-Begleitgerät gespeicherten Geräteschlüssel durchgeführt werden muss. Der zweite Aufruf gibt die Ergebnisse des HMAC-Vorgangs mit dem Geräteschlüssel zurück und kann in eine erfolgreiche Authentifizierung (Anzeige des Desktops) münden.

Bei der ersten Initiierungs-API (StartAuthenticationAsync) kann ein Fehler auftreten, falls das Windows Hello-Begleitgerät nach der ursprünglichen Registrierung durch eine Richtlinie deaktiviert wurde. Ein Fehler kann außerdem auftreten, wenn der API-Aufruf außerhalb des Zustands „WaitingForUserConfirmation“ oder „CollectingCredential“ erfolgt. (Weitere Informationen finden Sie weiter unten in diesem Abschnitt.) Darüber hinaus kann ein Fehler auftreten, wenn der Aufruf von einer nicht registrierten Begleitgeräte-App stammt. Die Enumeration „SecondaryAuthenticationFactorAuthenticationStatus“ fasst die möglichen Ergebnisse zusammen:

```C#
{
    Failed = 0,                     // Something went wrong in the underlying components
    Started,
    UnknownDevice,                  // Companion device app is not registered with framework
    DisabledByPolicy,               // Policy disabled this device after registration
    InvalidAuthenticationStage,     // Companion device framework is not currently accepting
                                    // incoming authentication requests
}
```

Beim zweiten API-Aufruf (FinishAuthencationAsync) kann ein Fehler auftreten, wenn die Nonce aus dem ersten Aufruf nicht mehr gültig ist. (Dies ist nach 20Sekunden der Fall.) Die Enumeration „SecondaryAuthenticationFactorFinishAuthenticationStatus“ erfasst die möglichen Ergebnisse.

```C#
{
    Failed = 0,     // Something went wrong in the underlying components
    Completed,      // Success
    NonceExpired,   // Nonce is expired
}
```

Das Timing der beiden API-Aufrufe („StartAuthenticationAsync“ und „FinishAuthencationAsync“) muss sich an der Erfassung der Absichts-, Benutzeranwesenheits- und Mehrdeutigkeitsvermeidungssignale durch das Windows Hello-Begleitgerät orientieren. (Weitere Informationen finden Sie unter „Benutzersignale“.) So darf der zweite Aufruf beispielsweise erst nach Verfügbarkeit des Absichtssignals übermittelt werden. Anders ausgedrückt: Der PC darf nicht entsperrt werden, wenn der Benutzer dies nicht ausdrücklich beabsichtigt. Um es noch etwas deutlicher zu machen: Wenn die Entsperrung des PCs darauf beruht, dass sich ein Bluetooth-Gerät in Reichweite befindet, muss ein explizites Absichtssignal erfasst werden. Andernfalls wird der PC jedes Mal entsperrt, wenn der Benutzer auf dem Weg in die Küche daran vorbeiläuft. Darüber hinaus ist die im ersten Aufruf zurückgegebene Nonce nur für 20Sekunden gültig und läuft dann ab. Daher darf der erste Aufruf erst erfolgen, wenn die Windows Hello-Begleitgeräte-App davon ausgehen kann, dass das Begleitgerät vorhanden ist (also etwa per USB angeschlossen oder an das NFC-Lesegerät gehalten wird). Achten Sie bei Verwendung von Bluetooth darauf, weder den Akku des PCs noch andere laufende Bluetooth-Aktivitäten zu beeinträchtigen, während geprüft wird, ob sich das Windows Hello-Begleitgerät in Reichweite befindet. Falls ein Benutzeranwesenheitssignal (etwa die Eingabe einer PIN) erforderlich ist, empfiehlt es sich zudem, den ersten Authentifizierungsaufruf erst nach Erfassung dieses Signals durchzuführen.

Dank des Windows Hello-Begleitgeräteframeworks kann die Windows Hello-Begleitgeräte-App die beiden obigen Aufrufe zu einem geeigneten Zeitpunkt durchführen, da immer genau bekannt ist, in welcher Phase des Authentifizierungsablaufs sich der Benutzer befindet. Zu diesem Zweck informiert das Windows Hello-Begleitgeräteframework die App-Hintergrundaufgabe über Sperrzustandsänderungen.

![Begleitgerätefluss](images/companion-device-4.png)

Im Anschluss finden Sie Details zu den einzelnen Zuständen:

| Zustand                         | Beschreibung                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| WaitingForUserConfirmation    | Dieses Zustandsänderungs-Benachrichtigungsereignis wird ausgelöst, wenn der Sperrbildschirm eingeblendet wird (beispielsweise, weil der Benutzer WINDOWS+L gedrückt hat). Es wird davon abgeraten, Fehlermeldungen in Bezug auf Probleme bei der Suche nach einem Gerät in diesem Zustand anzufordern. Im Allgemeinen empfiehlt es sich, nur dann Meldungen anzuzeigen, wenn das Absichtssignal vorliegt. Die Windows Hello-Begleitgeräte-App muss in diesem Zustand den ersten API-Aufruf für die Authentifizierung durchführen, wenn das Begleitgerät das Absichtssignal (beispielsweise Halten an ein NFC-Lesegerät, Drücken einer Taste des Begleitgeräts oder Ausführen einer bestimmten Geste – etwa Klatschen) erfasst und die Hintergrundaufgabe der Windows Hello-Begleitgeräte-App vom Begleitgerät einen Hinweis dafür erhält, dass das Absichtssignal erkannt wurde. Ansonsten gilt: Wenn die Windows Hello-Begleitgeräte-App darauf wartet, dass der Authentifizierungsablauf durch den PC gestartet wird (indem der Benutzer auf dem Sperrbildschirm nach oben wischt oder die LEERTASTE drückt), muss die Windows Hello-Begleitgeräte-App auf den nächsten Zustand (CollectingCredential) warten.     |
| CollectingCredential          | Dieses Zustandsänderungs-Benachrichtigungsereignis wird ausgelöst, wenn der Benutzer den Laptopdeckel öffnet, eine beliebige Taste auf der Tastatur drückt oder auf dem Sperrbildschirm nach oben wischt. Wenn der Start der Absichtssignalerfassung auf den obigen Aktionen basiert, muss die Windows Hello-Begleitgeräte-App mit der Erfassung des Signals beginnen (etwa mithilfe eines Popupfensters auf dem Begleitgerät, in dem der Benutzer gefragt wird, ob er den PC entsperren möchte). Dies ist ein guter Zeitpunkt für die Anzeige von Fehlermeldungen, falls die Windows Hello-Begleitgeräte-App ein Benutzeranwesenheitssignal auf dem Begleitgerät benötigt (etwa durch Eingabe einer PIN auf dem Windows Hello-Begleitgerät).                                                                                                                                                                                                                                                                                                                                            |
| SuspendingAuthentication      | Wenn die Windows Hello-Begleitgeräte-App diesen Zustand empfängt, bedeutet das, dass der Begleitauthentifizierungsdienst keine Authentifizierungsanforderungen mehr akzeptiert.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| CredentialCollected           | Dieser Zustand bedeutet, dass eine andere Windows Hello-Begleitgeräte-App die zweite API aufgerufen hat und der Begleitauthentifizierungsdienst die übermittelten Daten überprüft. An diesem Punkt akzeptiert der Begleitauthentifizierungsdienst keine weiteren Authentifizierungsanforderungen, es sei denn, die Überprüfung der aktuell übermittelten Anforderung ist nicht erfolgreich. Die Windows Hello-Begleitgeräte-App sollte auf den nächsten Zustand warten.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| CredentialAuthenticated       | Dieser Zustand bedeutet, dass die übermittelten Anmeldeinformationen akzeptiert wurden. „CredentialAuthenticated“ verfügt über die Geräte-ID des erfolgreichen Windows Hello-Begleitgeräts. Diese muss von der Windows Hello-Begleitgeräte-App überprüft werden, um zu ermitteln, ob das ihr zugeordnete Gerät erfolgreich war. Falls nicht, darf die Windows Hello-Begleitgeräte-App keine nachfolgenden Authentifizierungsabläufe (wie etwa eine Erfolgsmeldung auf dem Begleitgerät oder eine Vibration des Geräts) durchführen. Falls die übermittelten Anmeldeinformationen nicht akzeptiert wurden, ändert sich der Zustand in „CollectingCredential“.                                                                                                                                                                                                                                                                                                                                                                                       |
| StoppingAuthentication        | Die Authentifizierung war erfolgreich, und dem Benutzer wurde der Desktop angezeigt. Jetzt kann die Beendigung der Hintergrundaufgabe erzwungen werden. Heben Sie vor dem Beenden der Hintergrundaufgabe ausdrücklich die Registrierung des StageEvent-Handlers auf. Dadurch wird die Hintergrundaufgabe schnell beendet.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |



Windows Hello-Begleitgeräte-Apps dürfen die beiden Authentifizierungs-APIs nur in den ersten beiden Zuständen aufrufen. Windows Hello-Begleitgeräte-Apps müssen ermitteln, in welchem Szenario das Ereignis ausgelöst wird. Es gibt zwei Möglichkeiten: beim Entsperren oder nach dem Entsperren. Derzeit wird nur das Entsperrszenario unterstützt. In späteren Versionen werden unter Umständen auch Szenarien nach dem Entsperren unterstützt. Die Enumeration „SecondaryAuthenticationFactorAuthenticationScenario“ erfasst diese beiden Optionen:

```C#
{
    SignIn = 0,         // Running under lock screen mode
    CredentialPrompt,   // Running post unlock
}
```

Vollständiges Codebeispiel:

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using System.Threading;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public sealed class AuthenticationTask : IBackgroundTask
    {
        private string _deviceId;
        private static AutoResetEvent _exitTaskEvent = new AutoResetEvent(false);
        private static IBackgroundTaskInstance _taskInstance;
        private BackgroundTaskDeferral _deferral;

        private void Authenticate()
        {
            int retryCount = 0;

            while (retryCount < 3)
            {
                //
                // Pseudo code, the svcAuthNonce should be passed to device or generated from device
                //
                IBuffer svcAuthNonce = CryptographicBuffer.GenerateRandom(256/8);

                SecondaryAuthenticationFactorAuthenticationResult authResult = await
                    SecondaryAuthenticationFactorAuthentication.StartAuthenticationAsync(
                        _deviceId,
                        svcAuthNonce);
                if (authResult.Status != SecondaryAuthenticationFactorAuthenticationStatus.Started)
                {
                    SecondaryAuthenticationFactorAuthenticationMessage message;
                    switch (authResult.Status)
                    {
                        case SecondaryAuthenticationFactorAuthenticationStatus.DisabledByPolicy:
                            message = SecondaryAuthenticationFactorAuthenticationMessage.DisabledByPolicy;
                            break;
                        case SecondaryAuthenticationFactorAuthenticationStatus.InvalidAuthenticationStage:
                            // The task might need to wait for a SecondaryAuthenticationFactorAuthenticationStageChangedEvent
                            break;
                        default:
                            return;
                    }

                    // Show error message. Limited to 512 characters wide
                    await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(null, message);
                    return;
                }

                //
                // Pseudo function:
                // The device calculates and returns sessionHmac and deviceHmac
                //
                await GetHmacsFromDevice(
                    authResult.Authentication.ServiceAuthenticationHmac,
                    authResult.Authentication.DeviceNonce,
                    authResult.Authentication.SessionNonce,
                    out deviceHmac,
                    out sessionHmac);
                if (sessionHmac == null ||
                    deviceHmac == null)
                {
                    await authResult.Authentication.AbortAuthenticationAsync(
                        "Failed to read data from device");
                    return;
                }

                SecondaryAuthenticationFactorFinishAuthenticationStatus status =
                    await authResult.Authentication.FinishAuthencationAsync(deviceHmac, sessionHmac);
                if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.NonceExpired)
                {
                    retryCount++;
                    continue;
                }
                else if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.Completed)
                {
                    // The credential data is collected and ready for unlock
                    return;
                }
            }
        }

        public void OnAuthenticationStageChanged(
            object sender,
            SecondaryAuthenticationFactorAuthenticationStageChangedEventArgs args)
        {
            // The application should check the args.StageInfo.Stage to determine what to do in next. Note that args.StageInfo.Scenario will have the scenario information (SignIn vs CredentialPrompt).

            switch(args.StageInfo.Stage)
            {
            case SecondaryAuthenticationFactorAuthenticationStage.WaitingForUserConfirmation:
                // Show welcome message
                await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(
                    null,
                    SecondaryAuthenticationFactorAuthenticationMessage.WelcomeMessageSwipeUp);
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CollectingCredential:
                // Authenticate device
                Authenticate();
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CredentialAuthenticated:
                if (args.StageInfo.DeviceId = _deviceId)
                {
                    // Show notification on device about PC unlock
                }
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.StoppingAuthentication:
                // Quit from background task
                _exitTaskEvent.Set();
                break;
            }

            Debug.WriteLine("Authentication Stage = " + args.StageInfo.AuthenticationStage.ToString());
        }

        //
        // The Run method is the entry point of a background task.
        //
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            _taskInstance = taskInstance;
            _deferral = taskInstance.GetDeferral();

            // Register canceled event for this task
            taskInstance.Canceled += TaskInstanceCanceled;

            // Find all device registred by this application
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticationFactorDeviceFindScope.AllUsers);

            if (deviceInfoList.Count == 0)
            {
                // Quit the task silently
                return;
            }
            _deviceId = deviceInfoList[0].DeviceId;
            Debug.WriteLine("Use first device '" + _deviceId + "' in the list to signin");

            // Register AuthenticationStageChanged event
            SecondaryAuthenticationFactorRegistration.AuthenticationStageChanged += OnAuthenticationStageChanged;

            // Wait the task exit event
            _exitTaskEvent.WaitOne();

            _deferral.Complete();
        }

        void TaskInstanceCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _exitTaskEvent.Set();
        }
    }
}
```

### <a name="register-a-background-task"></a>Registrieren einer Hintergrundaufgabe

Wenn die Windows Hello-Begleitgeräte-App das erste Begleitgerät registriert, muss sie auch die Hintergrundaufgabenkomponente registrieren, die Authentifizierungsinformationen zwischen Gerät und Begleitgeräte-Authentifizierungsdienst übergibt.

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public class BackgroundTaskManager
    {
        // Register background task
        public static async Task<IBackgroundTaskRegistration> GetOrRegisterBackgroundTaskAsync(
            string bgTaskName,
            string taskEntryPoint)
        {
            // Check if there's an existing background task already registered
            var bgTask = (from t in BackgroundTaskRegistration.AllTasks
                          where t.Value.Name.Equals(bgTaskName)
                          select t.Value).SingleOrDefault();
            if (bgTask == null)
            {
                BackgroundAccessStatus status =
                    BackgroundExecutionManager.RequestAccessAsync().AsTask().GetAwaiter().GetResult();

                if (status == BackgroundAccessStatus.Denied)
                {
                    Debug.WriteLine("Background Execution is denied.");
                    return null;
                }

                var taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = bgTaskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger(new SecondaryAuthenticationFactorAuthenticationTrigger());
                bgTask = taskBuilder.Register();
                // Background task is registered
            }

            bgTask.Completed += BgTask_Completed;
            bgTask.Progress += BgTask_Progress;

            return bgTask;
        }
    }
}
```

### <a name="errors-and-messages"></a>Fehler und Meldungen

Das Windows Hello-Begleitgeräteframework muss den Benutzer mittels Feedback über Erfolg oder Misserfolg der Anmeldung informieren. Das Windows Hello-Begleitgeräteframework stellt eine Reihe (lokalisierter) Text- und Fehlermeldungen für die Windows Hello-Begleitgeräte-App zur Verfügung. Diese werden auf der Anmeldebenutzeroberfläche angezeigt.

![Begleitgerätefehler](images/companion-device-5.png)

Windows Hello-Begleitgeräte-Apps können auf der Anmeldebenutzeroberfläche mithilfe von „ShowNotificationMessageAsync“ Meldungen für den Benutzer anzeigen. Rufen Sie diese API auf, wenn ein Absichtssignal verfügbar ist. Beachten Sie, dass ein Absichtssignal immer aufseiten des Windows Hello-Begleitgeräts erfasst werden muss.

Es gibt zwei Arten von Meldungen: Anleitungen und Fehler.

Anleitungsmeldungen dienen dazu, den Benutzer über die Vorgehensweise zum Starten des Entsperrprozesses zu informieren. Diese Meldungen werden dem Benutzer lediglich einmal (bei der erstmaligen Geräteregistrierung) auf dem Sperrbildschirm angezeigt. Diese Nachrichten werden auch weiterhin auf dem Sperrbildschirm angezeigt.

Fehlermeldungen werden immer angezeigt und werden angezeigt, nachdem ein Absichtssignal angezeigt wird. Angesichts der Tatsache, dass vor dem Anzeigen von Meldungen für den Benutzer ein Absichtssignal erfasst werden muss und der Benutzer seine Absicht nur über eines der Windows Hello-Begleitgeräte zum Ausdruck bringt, darf es nicht dazu kommen, dass mehrere Windows Hello-Begleitgeräte um die Anzeige von Fehlermeldungen konkurrieren. Daher pflegt das Windows Hello-Begleitgeräteframework keine Warteschlange. Wenn ein Aufrufer eine Fehlermeldung anfordert, wird sie fünf Sekunden lang angezeigt. Während dieser fünf Sekunden werden alle anderen Anforderungen zum Anzeigen einer Fehlermeldung verworfen. Nach Ablauf der fünf Sekunden hat ein anderer Aufrufer die Möglichkeit, eine Fehlermeldung anzuzeigen. Das Blockieren des Fehlerkanals durch Aufrufer ist nicht zulässig.

Folgende Anleitungs- und Fehlermeldungen stehen zur Verfügung. Der Gerätename ist ein Parameter und wird von der Begleitgeräte-App als Teil von „ShowNotificationMessageAsync“ übergeben.

**Anleitung**

- „Wischen Sie nach oben oder drücken Sie die LEERTASTE, um sich mit *Gerätename* anzumelden.“
- „Einrichtung des Begleitgeräts. Bitte warten Sie oder verwenden Sie eine andere Anmeldeoption.“
- „Berühren Sie *Gerätename* zur Anmeldung an das NFC-Lesegerät.“
- „*Gerätename* wird gesucht ...“
- „Schließen Sie *Gerätename* zur Anmeldung an einen USB-Anschluss an.“

**Fehler**

- „Anmeldeanweisungen finden Sie auf *Gerätename*.“
- „Aktivieren Sie Bluetooth, um *Gerätename* für die Anmeldung zu verwenden.“
- „Aktivieren Sie NFC, um *Gerätename* für die Anmeldung zu verwenden.“
- „Stellen Sie eine WLAN-Verbindung her, um *Gerätename* für die Anmeldung zu verwenden.“
- „Tippen Sie erneut auf *Gerätename*.“
- „In Ihrem Unternehmen ist keine Anmeldung mit *Gerätename* möglich. Verwenden Sie eine andere Anmeldeoption.“
- „Tippen Sie zur Anmeldung auf *Gerätename*.“
- „Legen Sie Ihren Finger zur Anmeldung auf *Gerätename*.“
- „Wischen Sie zur Anmeldung mit Ihrem Finger über *Gerätename*.“
- „Anmeldung mit *Gerätename* nicht möglich. Verwenden Sie eine andere Anmeldeoption.“
- „Es ist ein Fehler aufgetreten. Verwenden Sie eine andere Anmeldeoption, und richten Sie *Gerätename* erneut ein.“
- „Versuchen Sie es noch mal.“
- „Sagen Sie Ihre gesprochene Passphrase in *Gerätename*.“
- „Bereit für die Anmeldung mit *Gerätename*.“
- „Verwenden Sie zuerst eine andere Anmeldeoption. Dann können Sie *Gerätename* verwenden, um sich anzumelden.“

### <a name="enumerating-registered-devices"></a>Aufzählen registrierter Geräte

Die Windows Hello-Begleitgeräte-App kann durch Aufrufen von „FindAllRegisteredDeviceInfoAsync“ die registrierten Begleitgeräte aufzählen. Diese API unterstützt zwei Abfragetypen (definiert durch die Enumeration „SecondaryAuthenticationFactorDeviceFindScope“):

```C#
{
    User = 0,
    AllUsers,
}
```

Der erste Bereich gibt die Liste der Geräte für den angemeldeten Benutzer zurück. Der zweite gibt die Liste für alle Benutzer auf dem PC zurück. Der erste Bereich muss beim Aufheben der Registrierung verwendet werden, um zu verhindern, dass die Registrierung für ein Windows Hello-Begleitgerät eines anderen Benutzers aufgehoben wird. Der zweite muss bei der Authentifizierung oder Registrierung verwendet werden: Bei der Registrierung kann diese Enumeration dazu beitragen, eine doppelte Registrierung des gleichen Windows Hello-Begleitgeräts durch die App zu vermeiden.

Hinweis: Selbst wenn die App diese Überprüfung nicht durchführt, wird eine mehrmalige Registrierung des gleichen Windows Hello-Begleitgeräts durch den PC verhindert. Wenn zum Zeitpunkt der Authentifizierung der Bereich „AllUsers“ verwendet wird, kann die Windows Hello-Begleitgeräte-App den Wechsel des Benutzerflusses besser unterstützen: BenutzerA wird angemeldet, während BenutzerB angemeldet ist. (Dazu müssen beide Benutzer die Windows Hello-Begleitgeräte-App installiert haben, BenutzerA muss seine Begleitgeräte beim PC registriert haben, und auf dem PC muss der Sperr- oder der Anmeldebildschirm angezeigt werden.)

## <a name="security-requirements"></a>Sicherheitsanforderungen

Der Begleitauthentifizierungsdienst bietet folgende Sicherheitsvorkehrungen:

- Schadsoftware auf einem Windows10-Desktopgerät, die als mittlerer Benutzer oder App-Container ausgeführt wird, kann über das Windows Hello-Begleitgerät nicht unbemerkt auf (im Rahmen von Windows Hello gespeicherte) Benutzeranmeldeinformationsschlüssel auf dem PC zugreifen.
- Ein böswilliger Benutzer auf einem Windows10-Desktopgerät kann kein Windows Hello-Begleitgerät eines anderen Benutzers auf dem Windows10-Desktopgerät verwenden, um unbemerkt auf dessen Benutzeranmeldeinformationsschlüssel (auf dem gleichen Windows10-Desktopgerät) zuzugreifen.
- Schadsoftware auf dem Windows Hello-Begleitgerät kann nicht unbemerkt auf die Benutzeranmeldeinformationsschlüssel auf dem Windows10-Desktopgerät zugreifen und auch keine Funktionen und keinen Code nutzen, die speziell für das Windows Hello-Begleitgeräteframework entwickelt wurden.
- Ein böswilliger Benutzer kann ein Windows10-Desktopgerät nicht durch Abfangen und späteres Wiedergeben von Datenverkehr zwischen Windows Hello-Begleitgerät und Windows10-Desktopgerät entsperren. Die Verwendung von Nonce, Authentifizierungsschlüssel und HMAC in unserem Protokoll schützt zuverlässig vor Replay-Angriffen.
- Schadsoftware oder ein böswilliger Benutzer auf einem nicht autorisierten PC kann nicht über ein Windows Hello-Begleitgerät auf den PC eines regulären Benutzers zugreifen. Dies wird durch die gegenseitige Authentifizierung zwischen Begleitauthentifizierungsdienst und Windows Hello-Begleitgerät mit Authentifizierungsschlüssel und HMAC in unserem Protokoll erreicht.

Der Schlüssel für die oben aufgeführten Sicherheitsvorkehrungen liegt im Schutz der HMAC Schlüssel vor unbefugtem Zugriff sowie in der Überprüfung der Benutzeranwesenheit. Genauer gesagt müssen folgende Anforderungen erfüllt werden:

- Schutz des Windows Hello-Begleitgeräts vor Klonversuchen
- Schutz vor Abhörversuchen, wenn HMAC-Schlüssel zur Registrierung an den PC gesendet werden
- Verfügbarkeit des Benutzeranwesenheitssignals
