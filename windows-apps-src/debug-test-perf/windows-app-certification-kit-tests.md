---
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Tests im Zertifizierungskit für Windows-Apps
description: Die Windows-Zertifizierungskit für Apps enthält eine Reihe von Tests, die helfen können, stellen Sie sicher, dass Ihre app auf dem Microsoft Store veröffentlicht werden kann.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, app-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: 0a7cf1e89c91f9ad53777aa21af1d43e070c4fc8
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362232"
---
# <a name="windows-app-certification-kit-tests"></a>Tests im Zertifizierungskit für Windows-Apps


Die [Windows App Certification Kit](windows-app-certification-kit.md) enthält eine Reihe von Tests, die dabei helfen, sicherzustellen, dass Ihre app ist bereit für den Microsoft Store veröffentlicht werden. Die Tests sind unten aufgeführt, mit den Kriterien, die Informationen und empfohlene Maßnahmen bei einem Fehler.

## <a name="deployment-and-launch-tests"></a>Bereitstellungs- und Starttests

Überwachen die App während des Zertifizierungstests, um aufzuzeichnen, wann sie abstürzt oder sich aufhängt.

### <a name="background"></a>Hintergrund

Apps, die nicht mehr reagieren oder abstürzen, können zu einem Datenverlust und zu einer Beeinträchtigung des Benutzererlebnisses führen.

Apps müssen voll funktionsfähig sein, ohne Windows-Kompatibilitätsmodi, AppHelp-Meldungen oder andere Kompatibilitätspatches zu verwenden.

Apps müssen nicht aufgeführt DLLs beim Laden in der zu öffnende HKEY\-lokalen\-Computer\\Software\\Microsoft\\Windows NT\\"CurrentVersion"\\Windows\\AppInit \-DLLs-Registrierungsschlüssel.

### <a name="test-details"></a>Testdetails

Die App wird beim Zertifizierungstest durchgehend auf Flexibilität und Stabilität geprüft.

Das Zertifizierungskit für Windows-Apps ruft [**IApplicationActivationManager::ActivateApplication**](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication) auf, um Apps zu starten. Damit eine App mit **ActivateApplication** gestartet werden kann, muss die Benutzerkontensteuerung (UAC) aktiviert sein und die Bildschirmauflösung mindestens 1024 x 768 oder 768 x 1024 betragen. Ist eine dieser Bedingungen nicht erfüllt, fällt die App bei diesem Test durch.

### <a name="corrective-actions"></a>Maßnahmen

Stellen Sie sicher, dass die Benutzerkontensteuerung (UAC) auf dem Test-PC aktiviert ist.

Führen Sie den Test auf einem PC mit ausreichend großem Bildschirm aus.

Falls die App nicht gestartet werden kann, obwohl die Testplattform die Voraussetzungen für [**ActivateApplication**](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication) erfüllt, können Sie das Problem mithilfe des Aktivierungsereignisprotokolls beheben. So finden Sie die Einträge im Ereignisprotokoll:

1.  Eventvwr.exe und navigieren Sie zu der Anwendung und Services Log\\Microsoft\\Windows\\Immersive-Shell-Ordner.
2.  Filtern Sie die Ansicht, um die Ereignis-Ids anzuzeigen: 5900-6000.
3.  Prüfen Sie die Protokolleinträge, um zu ermitteln, weshalb die App nicht gestartet wurde.

Führen Sie für die Datei mit dem Problem eine Problembehandlung durch, um das Problem zu identifizieren und zu beheben. Erstellen Sie die App neu, und wiederholen Sie den Test. Sie können auch überprüfen, ob im Protokollordner des Zertifizierungskits für Windows-Apps eine Dumpdatei erstellt wurde, die zum Debuggen Ihrer App verwendet werden kann.

## <a name="platform-version-launch-test"></a>Test der Funktionsfähigkeit auf Plattformversionen

Überprüft, ob die Windows-App auf einer zukünftigen Version des Betriebssystems ausgeführt werden kann. Dieser Test wurde bisher nur beim Desktop-App-Workflow angewendet, ist jetzt jedoch für die Workflows für den Store und die universelle Windows-Plattform (UWP) aktiviert.

### <a name="background"></a>Hintergrund

Version der Betriebssysteminformationen wurde Nutzung für den Microsoft Store eingeschränkt. Diese wurde von Apps häufig fälschlicherweise zum Überprüfen der Betriebssystemversion verwendet, damit Benutzern spezielle Funktionen für eine bestimmte Betriebssystemversion von der App bereitgestellt werden können.

### <a name="test-details"></a>Testdetails

Das Zertifizierungskit für Windows-Apps verwendet „HighVersionLie“, um zu ermitteln, wie die App die Betriebssystemversion prüft. Wenn die App abstürzt, besteht sie diesen Test nicht.

### <a name="corrective-action"></a>Maßnahmen

Apps sollten dies anhand von API-Funktionen zur Versionsabfrage überprüfen. Weitere Informationen finden Sie unter [Version des Betriebssystems](https://docs.microsoft.com/windows/desktop/SysInfo/operating-system-version).

## <a name="background-tasks-cancellation-handler-validation"></a>Überprüfung des Abbruchhandlers für Aufgaben

Dieser Test stellt sicher, dass die App über einen Abbruchhandler für deklarierte Hintergrundaufgaben verfügt. Es muss eine dedizierte Funktion vorhanden sein, die beim Abbruch der Aufgabe aufgerufen wird. Dieser Test wird nur für bereitgestellte Apps durchgeführt.

### <a name="background"></a>Hintergrund

Windows-Apps können einen Prozess registrieren, der im Hintergrund ausgeführt wird. Eine E-Mail-App kann z. B. von Zeit zu Zeit einen Pingbefehl an einen Server senden. Wenn das Betriebssystem diese Ressourcen jedoch benötigt, wird die Hintergrundaufgabe abgebrochen, und Apps sollten diesen Abbruch problemlos behandeln. Apps ohne Abbruchhandler stürzen u. U. ab oder werden nicht geschlossen, wenn der Benutzer versucht, die App zu schließen.

### <a name="test-details"></a>Testdetails

Die App wird gestartet und angehalten, und der Teil der App, der sich nicht im Hintergrund befindet, wird beendet. Anschließend werden die Hintergrundaufgaben im Zusammenhang mit dieser App abgebrochen. Der Zustand der App wird überprüft, und wenn die App noch ausgeführt, besteht sie diesen Test nicht.

### <a name="corrective-action"></a>Maßnahmen

Fügen Sie Ihrer App den Abbruchhandler hinzu. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](https://docs.microsoft.com/windows/uwp/launch-resume/support-your-app-with-background-tasks).

## <a name="app-count"></a>App-Anzahl

Dieser Test stellt sicher, dass ein App-Paket (APPX, App-Bündel) genau eine Anwendung enthält. Dies wurde im Kit zu einem eigenständigen Test geändert.

### <a name="background"></a>Hintergrund

Dieser Test wurde gemäß der Store-Richtlinie implementiert.

### <a name="test-details"></a>Testdetails

Für Windows Phone 8.1-Apps wird mit dem Test geprüft, ob die Gesamtzahl der APPX-Pakete im Bündel kleiner als &lt; 512 ist, ob nur ein Hauptpaket im Bündel vorhanden ist und ob die Architektur des Hauptpakets im Bündel als ARM oder neutral gekennzeichnet ist.

Für Windows 10-Apps wird mit dem Test geprüft, ob die Revisionsnummer in der Version des Bündels auf 0 festgelegt ist.

### <a name="corrective-action"></a>Maßnahmen

Stellen Sie sicher, dass App-Paket und -Bündel die weiter oben in den Testdetails angegebenen Anforderungen erfüllen.

## <a name="app-manifest-compliance-test"></a>Test der Erfüllung technischer Anforderungen für das App-Manifest

Überprüft die Inhalte des App-Manifests auf Korrektheit.

### <a name="background"></a>Hintergrund

Apps müssen ein korrekt formatiertes App-Manifest besitzen.

### <a name="test-details"></a>Testdetails

Überprüft das App-Manifest, um sicherzustellen, dass der Inhalt der Beschreibung in den [App-Paketanforderungen](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) entspricht.

-   **Dateierweiterungen und Protokolle**

    Von der App können die Dateierweiterungen deklariert werden, die ihr zugeordnet werden sollen. Bei nicht korrekter Verwendung wird von einer App u. U. eine große Anzahl von Dateierweiterungen deklariert, von denen die meisten nicht verwendet werden. Darunter leidet die Benutzerfreundlichkeit. Mit diesem Test wird eine Überprüfung der Anzahl von Dateierweiterungen durchgeführt, die einer App zugeordnet werden können.

-   **Framework-abhängigkeitsregel**

    Mit diesem Test wird die Anforderung durchgesetzt, dass für die Apps geeignete Abhängigkeiten von der UWP bestehen. Für den Test tritt ein Fehler auf, wenn eine unzulässige Abhängigkeit besteht.

    Liegt ein Konflikt zwischen der Betriebssystemversion, in der die App verwendet wird, und den bestehenden Frameworkabhängigkeiten vor, schlägt der Test fehl. Der Test schlägt ebenfalls fehl, wenn sich die App auf eine Vorschauversion der Framework-DLLs bezieht.

-   **Überprüfung der prozessübergreifenden Kommunikation (IPC)**

    Dieser Test setzt den Bedarf an, den dass die UWP-apps nicht außerhalb des app-Containers auf Desktop-Komponenten kommunizieren. Die prozessübergreifende Kommunikation ist nur für quergeladene Apps vorgesehen. Apps, die für [**ActivatableClassAttribute**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-activatableclassattribute) den Namen „DesktopApplicationPath“ angeben, bestehen diesen Test nicht.

### <a name="corrective-action"></a>Maßnahmen

Gleichen Sie das App-Manifest mit den [App-Paketanforderungen](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements) ab.

## <a name="windows-security-features-test"></a>Test der Windows-Sicherheitsfeatures

### <a name="background"></a>Hintergrund

Durch Ändern der standardmäßigen Windows-Sicherheitsvorkehrungen können Kunden einem erhöhten Risiko ausgesetzt werden.

### <a name="test-details"></a>Testdetails

Testet die Sicherheit der App durch Ausführen des [BinScope Binary Analyzers](#binscope-binary-analyzer-tests).

Bei den BinScope Binary Analyzer-Tests werden die Binärdateien der App auf Code- und Erstellungsmerkmale untersucht, die die App weniger anfällig für Angriffe oder für die Verwendung als Angriffsmittel machen.

Die BinScope-Tests des Analyzers für Binärdateien prüfen, ob folgende sicherheitsrelevante Features korrekt verwendet werden:

-   BinScope-Tests des Analyzers für Binärdateien
-   Private Codesignatur

### <a name="binscope-binary-analyzer-tests"></a>BinScope-Tests des Analyzers für Binärdateien

Die [BinScope-Tests des Analyzers für Binärdateien](https://www.microsoft.com/en-us/download/details.aspx?id=44995) untersuchen die Binärdateien der App auf Code- und Erstellungsmerkmale, die die App weniger anfällig für Angriffe oder für die Verwendung als Angriffsmittel machen.

Die BinScope-Tests des Analyzers für Binärdateien prüfen, ob die sicherheitsrelevanten Features korrekt verwendet werden:

-   [AllowPartiallyTrustedCallersAttribute](#binscope-1)
-   [/ SafeSEH-Ausnahmebehandlungsschutz](#binscope-2)
-   [Datenausführungsverhinderung](#binscope-3)
-   [Address Space Layout Randomization](#binscope-4)
-   [Lese-/Schreibzugriff freigegebenen PE-Abschnitts](#binscope-5)
-   [AppContainerCheck](#appcontainercheck)
-   [ExecutableImportsCheck](#binscope-7)
-   [WXCheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>AllowPartiallyTrustedCallersAttribute

**Windows-Zertifizierungskit für Apps-Fehlermeldung:** Fehler bei APTCACheck Test

Das AllowPartiallyTrustedCallersAttribute-Attribut (kurz: APTCA-Attribut) ermöglicht den Zugriff auf vollständig vertrauenswürdigen Code aus teilweise vertrauenswürdigem Code in signierten Assemblys. Wenn Sie das APTCA-Attribut auf eine Assembly anwenden, können teilweise vertrauenswürdige Aufrufer diese Assembly aufrufen, solange die Assembly besteht. Dies kann ein Sicherheitsrisiko darstellen.

**Erläutert, wie bei Ihrer app dieser Test schlägt fehl.**

Verwenden Sie das APTCA-Attribut nur dann für Assemblys mit starkem Namen, falls dies für das Projekt erforderlich ist und die Risiken bekannt sind. Falls das Attribut erforderlich ist, stellen Sie sicher, dass alle APIs durch geeignete Sicherheitsanforderungen für den Codezugriff geschützt sind. APTCA hat keine Auswirkung, wenn die Assembly Teil einer UWP-App (Universelle Windows-Plattform) ist.

**"Hinweise"**

Dieser Test wird nur für verwalteten Code (C#, .NET usw.) ausgeführt.

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>/ SafeSEH-Ausnahmebehandlungsschutz

**Windows-Zertifizierungskit für Apps-Fehlermeldung:** Fehler bei SafeSEHCheck Test

Ein Ausnahmehandler wird ausgeführt, wenn in der App eine Ausnahmebedingung auftritt – beispielsweise bei einem Fehler aufgrund einer Division durch Null. Da die Adresse des Ausnahmehandlers beim Aufrufen einer Funktion im Stapel gespeichert wird, besteht das Risiko eines Pufferüberlaufangriffs, sollte Schadsoftware den Stapel überschreiben.

**Erläutert, wie bei Ihrer app dieser Test schlägt fehl.**

Aktivieren Sie beim Erstellen der App die Option "/SAFESEH" im Linker-Befehl. Diese Option ist in den Veröffentlichungskonfigurationen von Visual Studio standardmäßig aktiviert. Vergewissern Sie sich, dass diese Option in den Erstellungsanweisungen für alle alle ausführbaren Module Ihrer App aktiviert ist.

**"Hinweise"**

Für 64-Bit-Binärdateien oder für Binärdateien für den ARM-Chipsatz wird dieser Test nicht ausgeführt, da hier keine Ausnahmehandleradressen im Stapel gespeichert werden.

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>Datenausführungsverhinderung

**Windows-Zertifizierungskit für Apps-Fehlermeldung:** Fehler bei NXCheck Test

Dieser Test stellt sicher, dass die App keinen Code ausführt, der in einem Datensegment gespeichert ist.

**Erläutert, wie bei Ihrer app dieser Test schlägt fehl.**

Aktivieren Sie beim Erstellen der App die Option „/NXCOMPAT“ im Linker-Befehl. Diese Option ist in Linker-Versionen mit Unterstützung der Datenausführungsverhinderung (Data Execution Prevention, DEP) standardmäßig aktiviert.

**"Hinweise"**

Wir empfehlen, Apps auf einer DEP-fähigen CPU zu testen und alle DEP-bedingten Fehler zu beheben.

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>Address Space Layout Randomization

**Windows-Zertifizierungskit für Apps-Fehlermeldung:** Fehler bei DBCheck Test

Die Zufallsgestaltung des Adressraumlayouts (Address Space Layout Randomization, ASLR) lädt ausführbare Bilder in unvorhersehbare Speicherbereiche. Dadurch wird eine größere Hürde für Schadsoftware geschaffen, die erwartet, dass ein Programm an einer bestimmten virtuellen Adresse geladen wird. Ihre App und alle von der App verwendeten Komponenten müssen über ASLR-Unterstützung verfügen.

**Erläutert, wie bei Ihrer app dieser Test schlägt fehl.**

Aktivieren Sie beim Erstellen der App die Option "/DYNAMICBASE" im Linker-Befehl. Vergewissern Sie sich, dass auch alle von der App verwendeten Module diese Linker-Option verwenden.

**"Hinweise"**

ASLR hat in der Regel keine Auswirkungen auf die Leistung. In einigen Szenarios ist auf 32-Bit-Systemen aber eine geringfügige Leistungsverbesserung zu beobachten. Es ist möglich, dass sich die Leistung bei einem stark belasteten System verschlechtert, bei dem viele Bilder an vielen unterschiedlichen Speicherbereichen geladen sind.

Dieser Test wird nur für Apps ausgeführt, die in nicht verwalteten Sprachen geschrieben wurden, z. B. mit C oder C++.

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>Lese-/Schreibzugriff freigegebenen PE-Abschnitts

**Windows-Zertifizierungskit für Apps-Fehlermeldung:** SharedSectionsCheck Test ist fehlgeschlagen.

Binärdateien mit beschreibbaren Abschnitten, die als freigegeben gekennzeichnet sind, stellen eine Sicherheitsbedrohung dar. Erstellen Sie keine Apps mit freigegebenen beschreibbaren Abschnitten, wenn dies nicht notwendig ist. Verwenden Sie [**CreateFileMapping**](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-createfilemappinga) oder [**MapViewOfFile**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffile), um ein freigegebenes Speicherobjekt zu erstellen, das korrekt gesichert ist.

**Erläutert, wie bei Ihrer app dieser Test schlägt fehl.**

Entfernen Sie sämtliche freigegebenen Abschnitte aus der App, und erstellen Sie freigegebene Speicherobjekte, indem Sie [**CreateFileMapping**](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-createfilemappinga) oder [**MapViewOfFile**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffile) mit den passenden Sicherheitsattributen aufrufen und die App dann neu erstellen.

**"Hinweise"**

Dieser Test wird nur für Apps ausgeführt, die in nicht verwalteten Sprachen geschrieben wurden, z. B. mit C oder C++.

### <a name="appcontainercheck"></a>AppContainerCheck

**Windows-Zertifizierungskit für Apps-Fehlermeldung:** AppContainerCheck Test ist fehlgeschlagen.

Der AppContainerCheck-Test prüft, ob das **appcontainer**-Bit im PE-Header einer ausführbaren Binärdatei gesetzt ist. Für Apps muss das **appcontainer**-Bit für alle EXE-Dateien und nicht verwalteten DLLs gesetzt sein, damit diese korrekt ausgeführt werden.

**Erläutert, wie bei Ihrer app dieser Test schlägt fehl.**

Wenn der Test für eine systemeigene ausführbare Datei nicht erfolgreich ist, stellen Sie sicher, dass Sie zum Erstellen der Datei den aktuellen Compiler und Linker und für den Linker das Kennzeichen */appcontainer* verwenden.

Wenn eine verwaltete ausführbare Datei der Test fehlschlägt, stellen Sie sicher, dass Sie über aktuelle Compiler und Linker, z. B. Microsoft Visual Studio verwendet, um die UWP-app zu erstellen.

**"Hinweise"**

Dieser Test wird für alle EXE-Dateien und nicht verwalteten DLLs ausgeführt.

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>ExecutableImportsCheck

**Windows-Zertifizierungskit für Apps-Fehlermeldung:** ExecutableImportsCheck Test ist fehlgeschlagen.

Ein portierbares ausführbares Image (Portable Executable, PE) besteht diesen Test nicht, wenn es in einen Abschnitt mit ausführbaren Code eingefügt wurde. Dies kann auftreten, wenn Sie für das PE-Image das Zusammenführen von „.rdata“ ermöglicht haben, indem Sie das Kennzeichen */merge* des Visual C++-Linkers auf */merge:.rdata=.text* festgelegt haben.

**Erläutert, wie bei Ihrer app dieser Test schlägt fehl.**

Führen Sie die Importtabelle nicht in einem Abschnitt mit ausführbarem Code zusammen. Stellen Sie sicher, dass das Kennzeichen */merge* des Visual C++-Linkers nicht so eingestellt ist, dass der Abschnitt „.rdata“ in einem Codeabschnitt zusammengeführt wird.

**"Hinweise"**

Dieser Test wird für den gesamten Binärcode ausgeführt, außer für ausschließlich verwaltete Assemblys.

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>WXCheck

**Windows-Zertifizierungskit für Apps-Fehlermeldung:** WXCheck Test ist fehlgeschlagen.

Mit dieser Überprüfung können Sie sicherstellen, dass eine Binärdatei keine Seiten enthält, die als schreibbar und ausführbar gekennzeichnet sind. Dies kann auftreten, wenn die Binärdatei einen writable- und ausführbaren Abschnitt aufweist oder wenn der Binärdatei *SectionAlignment* ist kleiner als *Seite\-Größe*.

**Erläutert, wie bei Ihrer app dieser Test schlägt fehl.**

Stellen Sie sicher, dass die Binärdatei einen Abschnitt geschrieben oder ausführbare Datei, und dass keine der Binärdatei *SectionAlignment* Wert ist mindestens gleich der *Seite\-Größe*.

**"Hinweise"**

Dieser Test wird für alle EXE-Dateien und systemeigenen, nicht verwalteten DLLs ausgeführt.

Eine ausführbare Datei kann einen schreibbaren und ausführbaren Abschnitt enthalten, wenn bei ihrer Erstellung "Bearbeiten und Fortfahren" aktiviert wurden (/ZI). Bei Deaktivierung von „Bearbeiten und Fortfahren“ ist der ungültige Abschnitt nicht mehr enthalten.

*Seite\-Größe* ist die Standardeinstellung *SectionAlignment* für ausführbare Dateien.

### <a name="private-code-signing"></a>Private Codesignatur

Überprüft, ob innerhalb des App-Pakets Binärdateien für private Codesignaturen vorhanden sind.

### <a name="background"></a>Hintergrund

Signaturdateien für privaten Code sollten privat bleiben, da sie im Fall einer Gefährdung zu bösartigen Zwecken missbraucht werden könnten.

### <a name="test-details"></a>Testdetails

Überprüft, ob innerhalb des App-Pakets Dateien mit der Erweiterung ".pfx" oder ".snk", die darauf hinweisen, dass private Signaturschlüssel verwendet wurden.

### <a name="corrective-actions"></a>Maßnahmen

Entfernen Sie alle Signaturschlüssel für privaten Code (z. B. PFX- und SNK-Dateien) aus dem Paket.

## <a name="supported-api-test"></a>Test der unterstützten APIs

Testet die App, um festzustellen, ob nicht kompatible APIs verwendet werden.

### <a name="background"></a>Hintergrund

Apps müssen die APIs für UWP-apps (Windows-Runtime oder unterstützten Win32-APIs), für den Microsoft Store-Zertifizierung verwenden. Dieser Test ermittelt auch Situationen, in denen eine verwaltete Binärdatei eine Abhängigkeit von einer Funktion außerhalb des genehmigten Profils aufweist.

### <a name="test-details"></a>Testdetails

-   Stellt sicher, dass jede Binärdatei in der app-Paket eine Abhängigkeit auf eine Win32-API nicht, die nicht für die Entwicklung von UWP-Apps durch Aktivieren der Importadresstabelle der Binärdatei unterstützt wird.
-   Stellt sicher, dass eine verwaltete Binärdatei des App-Pakets keine Abhängigkeit von einer Funktion außerhalb des genehmigten Profils aufweist.

### <a name="corrective-actions"></a>Maßnahmen

Stellen Sie sicher, dass die App als Releasebuild und nicht als Debugbuild kompiliert wurde.

> **Beachten Sie**  Debugbuild einer Anwendung können dieser Test schlägt fehl, selbst wenn die app nur verwendet [-APIs für UWP-apps](https://docs.microsoft.com/uwp/).

Überprüfen Sie den Fehlermeldungen zum Identifizieren der-API verwendet die app, die keine [-API für UWP-apps](https://docs.microsoft.com/uwp/).

> **Beachten Sie**  C++-apps, die in einem Debug-Konfiguration erstellt werden können dieser Test schlägt fehl, selbst wenn die Konfiguration nur die APIs aus dem Windows SDK für UWP-apps verwendet. Angezeigt wird, [Alternativen zu Windows-APIs in UWP-apps](https://go.microsoft.com/fwlink/p/?LinkID=244022) für Weitere Informationen.

## <a name="performance-tests"></a>Leistungstests

Die App muss schnell auf Benutzerinteraktionen und Systembefehle reagieren, um Benutzern eine schnelle und flüssige Benutzeroberfläche zu bieten.

Die Merkmale des Computers, auf dem der Test ausgeführt wird, können die Testergebnisse beeinflussen. Die Schwellenwerte des Leistungstests für die App-Zertifizierung sind so festgelegt, dass Computer mit geringem Energieverbrauch die Erwartungen der Kunden an eine schnelle und flüssige Benutzeroberfläche erfüllen. Um die Leistung Ihrer App zu ermitteln, empfehlen wir, die App auf einem PC mit geringem Energieverbrauch (z. B. einem PC mit Intel Atom-Prozessor) bei einer Auflösung von mindestens 1366 x 768 und mit einem herkömmlichen Festplattenlaufwerk (im Gegensatz zu einem Festkörperlaufwerk) zu testen.

### <a name="bytecode-generation"></a>Generierung von Bytecode

Als Leistungsoptimierungsmaßnahme zum Beschleunigen der JavaScript-Ausführungszeit wird von JavaScript-Dateien mit JS-Erweiterung Bytecode generiert, wenn die App bereitgestellt wird. Für JavaScript-Vorgänge können die Zeiträume für den Startvorgang und die laufende Ausführung so erheblich verkürzt werden.

### <a name="test-details"></a>Testdetails

Die App-Bereitstellung wird daraufhin überprüft, ob alle JS-Dateien in Bytecode konvertiert wurden.

### <a name="corrective-action"></a>Maßnahmen

Wenn bei diesem Test Fehler ermittelt werden, sollten Sie bei der Behandlung dieses Problems folgende Schritte berücksichtigen:

-   Stellen Sie sicher, dass die Ereignisprotokollierung aktiviert ist.
-   Stellen Sie sicher, dass alle JavaScript-Dateien in Bezug auf die Syntax gültig sind.
-   Vergewissern Sie sich, dass alle vorherigen Versionen der App deinstalliert wurden.
-   Schließen Sie die identifizierten Dateien aus dem App-Paket aus.

### <a name="optimized-binding-references"></a>Optimierte Bindungsverweise

Beim Verwenden von Bindungen sollte WinJS.Binding.optimizeBindingReferences auf "true" festgelegt werden, um die Speicherauslastung zu optimieren.

### <a name="test-details"></a>Testdetails

Überprüfen Sie den Wert von "WinJS.Binding.optimizeBindingReferences".

### <a name="corrective-action"></a>Maßnahmen

Legen Sie „WinJS.Binding.optimizeBindingReferences“ im JavaScript der App auf **true** fest.

## <a name="app-manifest-resources-test"></a>Test der App-Manifestressourcen

### <a name="app-resources-validation"></a>App-Ressourcenüberprüfung

Die App wird ggf. nicht installiert, wenn die im App-Manifest deklarierten Zeichenfolgen oder Bilder falsch sind. Wenn die App mit diesen Fehlern installiert wird, werden das Logo der App oder andere von der App verwendete Bilder möglicherweise nicht richtig angezeigt.

### <a name="test-details"></a>Testdetails

Prüft die im App-Manifest definierten Ressourcen, um sicherzustellen, dass sie vorhanden und gültig sind.

### <a name="corrective-action"></a>Maßnahmen

Orientieren Sie sich an der folgenden Tabelle.

<table>
<tr><th>Fehlermeldung</th><th>Kommentare</th></tr>
<tr><td>
<p>Das Bild "{Bildname}" definiert sowohl einen Scale- als auch einen TargetSize-Qualifizierer. Es darf jedoch jeweils nur ein Qualifizierer definiert sein.</p>
</td><td>
<p>Sie können Bilder für unterschiedliche Auflösungen anpassen.</p>
<p>In der tatsächlichen Meldung enthält „{image name}“ den Namen des Bilds mit dem Fehler.</p>
<p> Stellen Sie sicher, dass für jedes Bild entweder „Scale” oder „TargetSize” als Qualifizierer definiert ist.</p>
</td></tr>
<tr><td>
<p>Das Bild "{Bildname}" überschreitet die Größenbeschränkung von {Größenangabe}.</p>
</td><td>
<p>Stellen Sie sicher, dass alle Bilder der App den Größenbeschränkungen entsprechen.</p>
<p>In der tatsächlichen Meldung enthält „{image name}“ den Namen des Bilds mit dem Fehler.</p>
</td></tr>
<tr><td>
<p>Das Bild "{Bildname}" fehlt im Paket.</p>
</td><td>
<p>Ein erforderliches Bild fehlt.</p>
<p>In der tatsächlichen Meldung enthält "{Bildname}“ den Namen des fehlenden Bilds.</p>
</td></tr>
<tr><td>
<p>Das Bild "{Bildname}" ist keine gültige Bilddatei.</p>
</td><td>
<p>Stellen Sie sicher, dass alle Bilder der App den Einschränkungen für Dateiformattypen entsprechen.</p>
<p>In der tatsächlichen Meldung enthält „{image name}“ den Namen des ungültigen Bilds.</p>
</td></tr>
<tr><td>
<p>Das Bild „BadgeLogo“ enthält einen ungültigen ABGR-Wert {value} an der Position (x, y). Das Pixel muss weiß (##FFFFFF) oder transparent (00######) sein.</p>
</td><td>
<p>Das Signallogo ist ein Bild, das neben der Signalbenachrichtigung angezeigt wird, um die App auf dem Sperrbildschirm zu identifizieren. Dieses Bild muss einfarbig sein (es darf nur weiße und transparente Pixel enthalten).</p>
<p>In der tatsächlichen Meldung enthält {Wert} den Namen des ungültigen Farbwerts im Bild.</p>
</td></tr>
<tr><td>
<p>Das Bild „BadgeLogo“ enthält einen ABGR-Wert {Wert} an der Position (x, y), der für ein Bild mit hohem Kontrast (Weiß) ungültig ist. Das Pixel muss (##2A2A2A) oder dunkler oder transparent (00######) sein.</p>
</td><td>
<p>Das Signallogo ist ein Bild, das neben der Signalbenachrichtigung angezeigt wird, um die App auf dem Sperrbildschirm zu identifizieren.   Da das Signallogo bei hohem Kontrast (Weiß) auf einem weißen Hintergrund angezeigt wird, muss es sich um eine dunkle Version des normalen Signallogos handeln. Bei hohem Kontrast (Weiß) darf das Signallogo nur Pixel enthalten, die dunkler als (##2A2A2A) oder transparent sind.</p>
<p>In der tatsächlichen Meldung enthält {Wert} den Namen des ungültigen Farbwerts im Bild.</p>
</td></tr>
<tr><td>
<p>Für das Bild muss mindestens eine Variante ohne TargetSize-Qualifizierer definiert sein. Sie müssen einen Scale-Qualifizierer definieren oder „Scale” und „TargetSize” nicht angeben. In diesem Fall wird „Scale-100” verwendet.</p>
</td><td>
<p>Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design">Reaktionsfähiges Design für UWP-Apps (Universelle Windows-Plattform) – Grundlagen</a> und <a href="https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data">Richtlinien für App-Ressourcen</a>.</p>
</td></tr>
<tr><td>
<p>Das Paket enthält keine Datei „resources.pri”.</p>
</td><td>
<p>Wenn das App-Manifest lokalisierbaren Inhalt enthält, müssen Sie sicherstellen, dass das Paket der App eine gültige Datei „resources.pri“ enthält.</p>
</td></tr>
<tr><td>
<p>Die Datei „resources.pri“ muss eine Ressourcenzuordnung enthalten, bei der der Name dem Paketnamen „{vollständiger Paketname}“ entspricht.</p>
</td><td>
<p>Dieser Fehler wird angezeigt, wenn das Manifest geändert wird und der Name der Ressourcenzuordnung in „resources.pri“ dem Paketnamen im Manifest nicht mehr entspricht.</p>
<p>In der tatsächlichen Meldung enthält „{vollständiger Paketname}“ den Paketnamen, den „resources.pri“ enthalten muss.</p>
<p>Um diesen Fehler zu beheben, müssen Sie die Datei „resources.pri“ neu erstellen. Am besten erstellen Sie dazu das App-Paket neu.</p>
</td></tr>
<tr><td>
<p>Für die Datei „resources.pri“ darf „Automatisch zusammenführen“ nicht aktiviert sein.</p>
</td><td>
<p>„MakePRI.exe“ unterstützt eine Option mit dem Namen <strong>AutoMerge</strong>. Der Standardwert von <strong>AutoMerge</strong> ist <strong>Aus</strong>. Ist sie aktiviert, führt <strong>AutoMerge</strong> die App-Sprachpaketressourcen in einer einzelnen Datei „resources.pri“ zur Laufzeit zusammen. Wir nicht für apps zu empfehlen, die Sie über den Microsoft Store verteilen möchten. Die resources.pri einer App, die über den Microsoft Store verteilt werden muss im Stammverzeichnis des app Paket und alle Verweise, die von der app unterstützte Sprache.</p>
</td></tr>
<tr><td>
<p>Die Zeichenfolge „{string}“ entspricht nicht der Längenbeschränkung von maximal {number} Zeichen.</p>
</td><td>
<p>Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/windows/uwp/publish/app-package-requirements">App-Paketanforderungen</a>.</p>
<p>In der tatsächlichen Meldung wird „{string}“ durch die Zeichenfolge mit dem Fehler ersetzt, und {number} enthält die maximale Länge.</p>
</td></tr>
<tr><td>
<p>Die Zeichenfolge „{string}“ darf keine führenden/nachgestellten Leerzeichen enthalten.</p>
</td><td>
<p>Das Schema für die Elemente im App-Manifest lässt führende oder nachgestellte Leerzeichen nicht zu.</p>
<p>In der tatsächlichen Meldung wird „{string}“ durch die Zeichenfolge mit dem Fehler ersetzt.</p>
<p>Stellen Sie sicher, dass keiner der lokalisierten Werte der Manifestfelder in „resources.pri“ führende oder nachgestellte Leerzeichen enthält.</p>
</td></tr>
<tr><td>
<p>Die Zeichenfolge darf nicht leer sein (Länge größer 0 (null)).</p>
</td><td>
<p>Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/windows/uwp/publish/app-package-requirements">App-Paketanforderungen</a>.</p>
</td></tr>
<tr><td>
<p>In der Datei „resources.pri” ist keine Standardressource angegeben.</p>
</td><td>
<p>Weitere Informationen finden Sie unter <a href="https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data">Richtlinien für App-Ressourcen</a>.</p>
<p>In der Standardbuildkonfiguration nimmt Visual Studio nur Bildressourcen mit der Skalierung 200 % in das App-Paket auf, wenn ein Bündel generiert wird, andere Ressourcen werden im Ressourcenpaket abgelegt. Stellen Sie sicher, dass Sie entweder Bildressourcen mit der Skalierung 200 % einschließen oder Ihr Projekt für die Aufnahme der vorhandenen Ressourcen konfigurieren.</p>
</td></tr>
<tr><td>
<p>In der Datei „resources.pri“ ist kein Ressourcenwert angegeben.</p>
</td><td>
<p>Stellen Sie sicher, dass für das App-Manifest gültige Ressourcen in „resources.pri“ definiert sind.</p>
</td></tr>
<tr><td>
<p>Die Bilddatei {Filename} muss kleiner als 204800 Bytes sein.\*\*</p>
</td><td>
<p>Verringern Sie die Größe der angegebenen Bilder.</p>
</td></tr>
<tr><td>
<p>Die {Filename}-Datei muss einen Abschnitt für die umgekehrte Zuordnung nicht enthalten.\*\*</p>
</td><td>
<p>Die umgekehrte Zuordnung wird zwar während des Debuggens mit F5 in Visual Studio beim Aufrufen von „makepri.exe“ generiert, sie kann jedoch entfernt werden, indem „makepri.exe“ beim Generieren einer PRI-Datei ohne den Parameter „/m“ ausgeführt wird.</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* Gibt an, dass ein Test für Windows 8.1 in den Windows App Certification Kit 3.3 hinzugefügt wurde und ist gilt nur, wenn diese Version des Kits oder höher.</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>Branding-Validierung

UWP-apps komplett und voll funktionstüchtig sein sollten. Apps, für die Standardbilder (aus Vorlagen oder SDK-Beispielen) verwendet werden, verfügen über eine schlechte Benutzeroberfläche und können im Store-Katalog nicht leicht identifiziert werden.

### <a name="test-details"></a>Testdetails

Bei diesem Test wird sichergestellt, dass die von der App verwendeten Bilder keine Standardbilder aus SDK-Beispielen oder aus Visual Studio sind.

### <a name="corrective-actions"></a>Maßnahmen

Ersetzen Sie Standardbilder durch eigene Bilder, die über Aussagekraft für Ihre App verfügen.

## <a name="debug-configuration-test"></a>Test der Debugkonfiguration

Testet die App, um sicherzustellen, dass es sich nicht um einen Debugbuild handelt.

### <a name="background"></a>Hintergrund

Für die Zertifizierung für die Microsoft Store-apps müssen nicht kompiliert werden, zum Debuggen, und sie müssen nicht auf die Debugversionen einer ausführbaren Datei verweisen. Darüber hinaus müssen Sie den Code für die App optimiert erstellen, damit dieser Test bestanden wird.

### <a name="test-details"></a>Testdetails

Testen Sie die App, um sicherzustellen, dass es sich nicht um einen Debugbuild handelt und dass die App mit keinen Debugframeworks verknüpft ist.

### <a name="corrective-actions"></a>Maßnahmen

-   Erstellen Sie die app als ein Releasebuild erstellt, bevor Sie sie an den Microsoft Store übermitteln.
-   Stellen Sie sicher, dass Sie die richtige .NET Framework-Version installiert haben.
-   Stellen Sie sicher, dass die App nicht über Links zu Debugversionen eines Frameworks verfügt und dass die Erstellung mit einer Releaseversion erfolgt. Wenn diese App .NET-Komponenten enthält, sollten Sie sich vergewissern, dass Sie die richtige Version des .NET-Frameworks installiert haben.

## <a name="file-encoding-test"></a>Test der Dateicodierung

### <a name="utf-8-file-encoding"></a>UTF-8-Dateicodierung

### <a name="background"></a>Hintergrund

HTML-, CSS- und JavaScript-Dateien müssen im UTF-8-Format codiert sein und über eine entsprechende Bytereihenfolgemarke (BOM) verfügen, um von der Bytecode-Zwischenspeicherung profitieren und bestimmte Laufzeitfehlerbedingungen vermeiden zu können.

### <a name="test-details"></a>Testdetails

Testet den Inhalt der App-Pakete, um sicherzustellen, dass darin die richtige Dateicodierung verwendet wird.

### <a name="corrective-action"></a>Maßnahmen

Öffnen Sie die betroffene Datei, und wählen Sie in Visual Studio im Menü **Datei** die Option **Speichern unter**. Wählen Sie neben der Schaltfläche **Speichern** das Dropdownsteuerelement aus, und wählen Sie **Mit Codierung speichern** aus. Wählen Sie im Dialogfeld mit den erweiterten Speicheroptionen die Option **Unicode (UTF-8 mit Signatur)** aus, und klicken Sie auf **OK**.

## <a name="direct3d-feature-level-test"></a>Test der Direct3D-Featureebene

### <a name="direct3d-feature-level-support"></a>Test auf Unterstützung der Direct3D-Featureebene

Testet Microsoft Direct3D-Apps, um sicherzustellen, dass sie auf Geräten mit älterer Grafikhardware nicht abstürzen.

### <a name="background"></a>Hintergrund

Microsoft Store müssen alle Anwendungen ordnungsgemäß gerendert, oder Fehler ordnungsgemäß auf mit Direct3D-Funktionsebene 9\-1 Grafikkarten.

Da Benutzer die Grafikhardware in ihr Gerät ändern können nach der Installation der app bei der Auswahl einer minimale Funktionsebene größer als 9\-1, Ihre app muss erkennen beim Start, und zwar unabhängig davon, ob die aktuelle Hardware die Mindestanforderungen erfüllt. Wenn die Mindestanforderungen nicht erfüllt sind, muss Ihre App dem Benutzer eine Meldung mit den Direct3D-Anforderungen anzeigen. Wird zudem eine App auf ein Gerät heruntergeladen, mit dem sie nicht kompatibel ist, sollte sie dies beim Start erkennen und dem Kunden eine Meldung bezüglich der erforderlichen Voraussetzungen anzeigen.

### <a name="test-details"></a>Testdetails

Der Test wird überprüft, ob es sich bei den apps genau auf Funktionsebene 9 gerendert werden\-1.

### <a name="corrective-action"></a>Maßnahmen

Stellen Sie sicher, dass Ihre app richtig auf Direct3D-Funktionsebene 9 gerendert\-1, selbst wenn Sie es auf eine höhere Funktionsebene erwarten. Weitere Informationen finden Sie unter [Entwickeln für unterschiedliche Direct3D-Featureebenen](https://go.microsoft.com/fwlink/p/?LinkID=253575).

### <a name="direct3d-trim-after-suspend"></a>Direct3D-Kürzung nach dem Anhalten

> **Beachten Sie**  dieser Test gilt nur für UWP-apps entwickelt für Windows 8.1 und höher.

### <a name="background"></a>Hintergrund

Wenn von der App auf ihrem jeweiligen Direct3D-Gerät [**Trim**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim) nicht aufgerufen wird, gibt sie keinen Speicher frei, der für die vorherigen 3D-Arbeitsschritte zugeordnet wurde. Dies erhöht das Risiko, dass Apps aufgrund von unzureichendem Systemspeicher beendet werden.

### <a name="test-details"></a>Testdetails

Apps werden auf die Einhaltung der d3d-Anforderungen überprüft. Außerdem wird sichergestellt, dass Apps beim Anhalterückruf (Suspend) eine neue [**Trim**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim)-API aufrufen.

### <a name="corrective-action"></a>Maßnahmen

Von der App sollte die [**Trim**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nf-dxgi1_3-idxgidevice3-trim)-API für ihre [**IDXGIDevice3**](https://docs.microsoft.com/windows/desktop/api/dxgi1_3/nn-dxgi1_3-idxgidevice3)-Schnittstelle vor jedem Anhaltevorgang aufgerufen werden.

## <a name="app-capabilities-test"></a>Test der App-Funktionen

### <a name="special-use-capabilities"></a>Sonderfunktionen

### <a name="background"></a>Hintergrund

Sonderfunktionen sind für sehr spezielle Szenarien vorgesehen. Diese Funktionen dürfen nur mit Unternehmenskonten genutzt werden.

### <a name="test-details"></a>Testdetails

Es wird überprüft, ob von der App die folgenden Funktionen deklariert werden:

-   EnterpriseAuthentication
-   SharedUserCertificates
-   DocumentsLibrary

Falls eine oder mehrere dieser Funktionen deklariert werden, wird Benutzern während des Tests eine Warnung angezeigt.

### <a name="corrective-actions"></a>Maßnahmen

Sie können erwägen, die Sonderfunktion zu entfernen, wenn diese für die App nicht erforderlich ist. Die Verwendung dieser Funktionen unterliegt außerdem einer weiteren Prüfung anhand der Richtlinie für die Aufnahme.

## <a name="windows-runtime-metadata-validation"></a>Windows-Runtime-Metadatenvalidierung

### <a name="background"></a>Hintergrund

Es wird sichergestellt, dass die Komponenten, die als Teil einer App mitgeliefert werden, mit dem UWP-Typsystem kompatibel sind.

### <a name="test-details"></a>Testdetails

Es wird überprüft, ob die **WINMD**-Dateien im Paket den UWP-Regeln entsprechen.

### <a name="corrective-actions"></a>Maßnahmen

-   **ExclusiveTo attributtest:** Stellen Sie sicher, dass UWP Klassen keine Schnittstellen implementiert werden, die als ExclusiveTo einer anderen Klasse markiert sind.
-   **Geben Sie Speicherort Test:** Stellen Sie sicher, dass sich die Metadaten für alle UWP-Typen in der Winmd-Datei mit dem längsten übereinstimmenden Namespace-Namen im app-Paket ab.
-   **Groß-/Kleinschreibung Namenstest Typ:** Stellen Sie sicher, dass alle UWP-Typen eindeutige Namen mit Groß-/Kleinschreibung im App-Paket aufweisen. Vergewissern Sie sich außerdem, dass UWP-Typnamen im App-Paket nicht auch als Namespacename verwendet werden.
-   **Geben Sie Namen Richtigkeit Test:** Stellen Sie sicher, dass im globalen Namespace oder im Windows-Namespace der obersten Ebene keine UWP-Typen vorhanden sind.
-   **Allgemeine Metadaten Richtigkeit Test:** Stellen Sie sicher, dass der Compiler, die, den Sie verwenden, um Ihre Typen generieren, auf dem neuesten Stand mit den UWP-Spezifikationen.
-   **Eigenschaftentest:** Stellen Sie sicher, dass alle Eigenschaften einer UWP-Klasse über eine get-Methode verfügen (set-Methoden sind optional). Vergewissern Sie sich, dass der Typ des Rückgabewerts der get-Methode für alle Eigenschaften von UWP-Typen jeweils mit dem Typ des Eingabeparameters der set-Methode übereinstimmt.

## <a name="package-sanity-tests"></a>Tests für die Paketintegrität

### <a name="platform-appropriate-files-test"></a>Test auf für Plattform geeignete Dateien

Apps, von denen gemischte Binärdateien installiert werden, stürzen je nach der Prozessorarchitektur von Benutzern möglicherweise ab oder werden nicht richtig ausgeführt.

### <a name="background"></a>Hintergrund

Bei diesem Test werden die Binärdateien in einem App-Paket auf Architekturkonflikte geprüft. Ein App-Paket sollte keine Binärdateien enthalten, die unter der im Manifest angegebenen Prozessorarchitektur nicht verwendet werden können. Das Einfügen von nicht unterstützten Binärdateien kann zum Absturz der App oder zu einem unnötigen Anstieg der Paketgröße einer App führen.

### <a name="test-details"></a>Testdetails

Es wird überprüft, ob die Bitanzahl jeder einzelnen Datei im PE-Header korrekt ist, wenn Querverweise mit der Prozessorarchitekturdeklaration des App-Pakets eingerichtet werden.

### <a name="corrective-action"></a>Maßnahmen

Befolgen Sie diese Richtlinien, um sicherzustellen, dass Ihr App-Paket nur Dateien enthält, die von der im App-Manifest angegebenen Architektur unterstützt werden:

-   Wenn die Zielprozessorarchitektur der App den Prozessortyp "Neutral" aufweist, kann die App keine x86-, x64- oder ARM-Binärdateien oder -Abbilddateien enthalten.

-   Wenn die Zielprozessorarchitektur der App den Prozessortyp "x86" aufweist, darf das App-Paket nur x86-Binärdateien oder -Abbilddateien enthalten. Wenn das Paket x64- oder ARM-Binärtypen oder -Imagetypen enthält, tritt beim Test ein Fehler auf.

-   Wenn die Zielprozessorarchitektur der App den Prozessortyp "x64" aufweist, muss das App-Paket x64-Binärdateien oder -Abbilddateien enthalten. Beachten Sie, dass das Paket in diesem Fall auch x86-Dateien enthalten kann. Für die Hauptoberfläche der App sollte jedoch die x64-Binärdatei genutzt werden.

    Falls das Paket jedoch ARM-Binärdateien oder -Abbilddateien oder ausschließlich x86-Binärdateien oder -Abbilddateien enthält, tritt beim Test ein Fehler auf.

-   Wenn die Zielprozessorarchitektur der App den Prozessortyp "ARM" aufweist, darf das App-Paket nur ARM-Binärdateien oder -Abbilddateien enthalten. Wenn das Paket x64- oder x86-Binärdateien oder -Abbilddateien enthält, tritt beim Test ein Fehler auf.

### <a name="supported-directory-structure-test"></a>Test der unterstützten Verzeichnisstruktur

Überprüft, dass Anwendungen nicht erstellen, werden Unterverzeichnisse im Rahmen der Installation, die länger als der HÖCHSTWERT\-Pfad.

### <a name="background"></a>Hintergrund

OS-Komponenten (einschließlich Trident, WWAHost usw.) sind auf maximal intern beschränkt\-Pfad für die System-Dateipfade und funktionieren nicht ordnungsgemäß für längere Pfade.

### <a name="test-details"></a>Testdetails

Stellt sicher, dass kein Pfad innerhalb der app-Installationsverzeichnis MAX überschreitet\-Pfad.

### <a name="corrective-action"></a>Maßnahmen

Verwenden Sie eine kürzere Verzeichnisstruktur bzw. kürzere Dateinamen.

## <a name="resource-usage-test"></a>Test für die Ressourcennutzung

### <a name="winjs-background-task-test"></a>Test der WinJS-Hintergrundaufgabe

Mit dem Test der WinJS-Hintergrundaufgabe wird sichergestellt, dass JavaScript-Apps über die richtigen close-Anweisungen verfügen, damit die Apps nicht unnötig Akkuenergie verbrauchen.

### <a name="background"></a>Hintergrund

Apps mit JavaScript-Hintergrundaufgaben müssen "Close()" als letzte Anweisung der Hintergrundaufgabe aufrufen. Ist dies bei Apps nicht der Fall, kann das System unter Umständen nicht in den verbundenen Standbymodus zurückkehren, was zu einer schnelleren Entleerung des Akkus führen kann.

### <a name="test-details"></a>Testdetails

Wenn für Apps im Manifest keine Hintergrundaufgabe angegeben ist, gilt der Test als bestanden. Andernfalls wird beim Testen die JavaScript-Hintergrundaufgabendatei analysiert, die im App-Paket angegeben ist, und nach einer Close()-Anweisung gesucht. Wird diese gefunden, gilt der Test als bestanden. Falls nicht, gilt der Test als nicht bestanden.

### <a name="corrective-action"></a>Maßnahmen

Aktualisieren Sie den JavaScript-Hintergrundcode so, dass „Close()” richtig aufgerufen wird.


## <a name="related-topics"></a>Verwandte Themen

* [Tests für Windows Desktop Bridge-app](windows-desktop-bridge-app-tests.md)
* [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 
