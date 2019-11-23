---
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Tests im Zertifizierungskit für Windows-Apps
description: Das zertifizierungskit für Windows-apps enthält eine Reihe von Tests, mit denen sichergestellt werden kann, dass Ihre APP auf dem Microsoft Store veröffentlicht werden kann.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, App-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: 32ece54ef17c97b1cb16b3f0a706c86eb2858556
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257856"
---
# <a name="windows-app-certification-kit-tests"></a>Tests im Zertifizierungskit für Windows-Apps


Das [zertifizierungskit für Windows-apps](windows-app-certification-kit.md) enthält eine Reihe von Tests, mit denen sichergestellt werden kann, dass Ihre APP auf dem Microsoft Store veröffentlicht werden kann. Die Tests sind unten mit Ihren Kriterien, Details und empfohlenen Aktionen im Falle eines Fehlers aufgeführt.

## <a name="deployment-and-launch-tests"></a>Bereitstellungs- und Starttests

Überwachen die App während des Zertifizierungstests, um aufzuzeichnen, wann sie abstürzt oder sich aufhängt.

### <a name="background"></a>Hintergrund

Apps, die nicht mehr reagieren oder abstürzen, können zu einem Datenverlust und zu einer Beeinträchtigung des Benutzererlebnisses führen.

Apps müssen voll funktionsfähig sein, ohne Windows-Kompatibilitätsmodi, AppHelp-Meldungen oder andere Kompatibilitätspatches zu verwenden.

Apps dürfen DLLs nicht zum Laden in den HKEY\-lokalen\-Machine\\Software\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\AppInit\-DLLs-Registrierungsschlüssel auflisten.

### <a name="test-details"></a>Testdetails

Die App wird beim Zertifizierungstest durchgehend auf Flexibilität und Stabilität geprüft.

Das Zertifizierungskit für Windows-Apps ruft [**IApplicationActivationManager::ActivateApplication**](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication) auf, um Apps zu starten. Damit eine App mit **ActivateApplication** gestartet werden kann, muss die Benutzerkontensteuerung (UAC) aktiviert sein und die Bildschirmauflösung mindestens 1024 x 768 oder 768 x 1024 betragen. Ist eine dieser Bedingungen nicht erfüllt, fällt die App bei diesem Test durch.

### <a name="corrective-actions"></a>Maßnahmen

Stellen Sie sicher, dass die Benutzerkontensteuerung (UAC) auf dem Test-PC aktiviert ist.

Führen Sie den Test auf einem PC mit ausreichend großem Bildschirm aus.

Falls die App nicht gestartet werden kann, obwohl die Testplattform die Voraussetzungen für [**ActivateApplication**](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nf-shobjidl_core-iapplicationactivationmanager-activateapplication) erfüllt, können Sie das Problem mithilfe des Aktivierungsereignisprotokolls beheben. So finden Sie die Einträge im Ereignisprotokoll:

1.  Öffnen Sie eventvwr. exe, und navigieren Sie zum Anwendungs-und Dienst Protokoll\\Ordner Microsoft\\Windows\\immersive Shell.
2.  Filtern Sie die Ansicht entsprechend, um folgende Ereigniskennungen anzuzeigen: 5900-6000.
3.  Prüfen Sie die Protokolleinträge, um zu ermitteln, weshalb die App nicht gestartet wurde.

Führen Sie für die Datei mit dem Problem eine Problembehandlung durch, um das Problem zu identifizieren und zu beheben. Erstellen Sie die App neu, und wiederholen Sie den Test. Sie können auch überprüfen, ob im Protokollordner des Zertifizierungskits für Windows-Apps eine Dumpdatei erstellt wurde, die zum Debuggen Ihrer App verwendet werden kann.

## <a name="platform-version-launch-test"></a>Test der Funktionsfähigkeit auf Plattformversionen

Überprüft, ob die Windows-App auf einer zukünftigen Version des Betriebssystems ausgeführt werden kann. Dieser Test wurde bisher nur beim Desktop-App-Workflow angewendet, ist jetzt jedoch für die Workflows für den Store und die universelle Windows-Plattform (UWP) aktiviert.

### <a name="background"></a>Hintergrund

Die Informationen zur Betriebssystemversion weisen die Verwendung der Microsoft Store auf. Diese wurde von Apps häufig fälschlicherweise zum Überprüfen der Betriebssystemversion verwendet, damit Benutzern spezielle Funktionen für eine bestimmte Betriebssystemversion von der App bereitgestellt werden können.

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

-   **Framework-Abhängigkeits Regel**

    Mit diesem Test wird die Anforderung durchgesetzt, dass für die Apps geeignete Abhängigkeiten von der UWP bestehen. Für den Test tritt ein Fehler auf, wenn eine unzulässige Abhängigkeit besteht.

    Liegt ein Konflikt zwischen der Betriebssystemversion, in der die App verwendet wird, und den bestehenden Frameworkabhängigkeiten vor, schlägt der Test fehl. Der Test schlägt ebenfalls fehl, wenn sich die App auf eine Vorschauversion der Framework-DLLs bezieht.

-   **Überprüfung der prozessübergreifenden Kommunikation (IPC)**

    Dieser Test erzwingt die Anforderung, dass UWP-apps nicht außerhalb des App-Containers mit Desktop Komponenten kommunizieren. Die prozessübergreifende Kommunikation ist nur für quergeladene Apps vorgesehen. Apps, die für [**ActivatableClassAttribute**](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-activatableclassattribute) den Namen „DesktopApplicationPath“ angeben, bestehen diesen Test nicht.

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
-   [/SAFESEH Ausnahme Behandlungs Schutz](#binscope-2)
-   [Verhinderung von Datenausführung](#binscope-3)
-   [Adressraum Layout Randomization](#binscope-4)
-   [Frei gegebener PE-Abschnitt lesen/schreiben](#binscope-5)
-   [Appcontainercheck](#appcontainercheck)
-   [Executableimportscheck](#binscope-7)
-   [Wxcheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>AllowPartiallyTrustedCallersAttribute

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** APTCACheck Test failed (Fehler beim APTCACheck-Test.)

Das AllowPartiallyTrustedCallersAttribute-Attribut (kurz: APTCA-Attribut) ermöglicht den Zugriff auf vollständig vertrauenswürdigen Code aus teilweise vertrauenswürdigem Code in signierten Assemblys. Wenn Sie das APTCA-Attribut auf eine Assembly anwenden, können teilweise vertrauenswürdige Aufrufer diese Assembly aufrufen, solange die Assembly besteht. Dies kann ein Sicherheitsrisiko darstellen.

**Was Sie tun sollten, wenn Ihre APP mit diesem Test fehlschlägt**

Verwenden Sie das APTCA-Attribut nur dann für Assemblys mit starkem Namen, falls dies für das Projekt erforderlich ist und die Risiken bekannt sind. Falls das Attribut erforderlich ist, stellen Sie sicher, dass alle APIs durch geeignete Sicherheitsanforderungen für den Codezugriff geschützt sind. APTCA hat keine Auswirkung, wenn die Assembly Teil einer UWP-App (Universelle Windows-Plattform) ist.

**Rede**

Dieser Test wird nur für verwalteten Code (C#, .NET usw.) ausgeführt.

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>/SAFESEH Ausnahme Behandlungs Schutz

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** Fehler beim SafeSEHCheck-Test.

Ein Ausnahmehandler wird ausgeführt, wenn in der App eine Ausnahmebedingung auftritt – beispielsweise bei einem Fehler aufgrund einer Division durch Null. Da die Adresse des Ausnahmehandlers beim Aufrufen einer Funktion im Stapel gespeichert wird, besteht das Risiko eines Pufferüberlaufangriffs, sollte Schadsoftware den Stapel überschreiben.

**Was Sie tun sollten, wenn Ihre APP mit diesem Test fehlschlägt**

Aktivieren Sie beim Erstellen der App die Option "/SAFESEH" im Linker-Befehl. Diese Option ist in den Veröffentlichungskonfigurationen von Visual Studio standardmäßig aktiviert. Vergewissern Sie sich, dass diese Option in den Erstellungsanweisungen für alle alle ausführbaren Module Ihrer App aktiviert ist.

**Rede**

Für 64-Bit-Binärdateien oder für Binärdateien für den ARM-Chipsatz wird dieser Test nicht ausgeführt, da hier keine Ausnahmehandleradressen im Stapel gespeichert werden.

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>Verhinderung von Datenausführung

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** NXCheck Test failed (Fehler beim NXCheck-Test.)

Dieser Test stellt sicher, dass die App keinen Code ausführt, der in einem Datensegment gespeichert ist.

**Was Sie tun sollten, wenn Ihre APP mit diesem Test fehlschlägt**

Aktivieren Sie beim Erstellen der App die Option „/NXCOMPAT“ im Linker-Befehl. Diese Option ist in Linker-Versionen mit Unterstützung der Datenausführungsverhinderung (Data Execution Prevention, DEP) standardmäßig aktiviert.

**Rede**

Wir empfehlen, Apps auf einer DEP-fähigen CPU zu testen und alle DEP-bedingten Fehler zu beheben.

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>Adressraum Layout Randomization

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** Fehler beim DBCheck-Test.

Die Zufallsgestaltung des Adressraumlayouts (Address Space Layout Randomization, ASLR) lädt ausführbare Bilder in unvorhersehbare Speicherbereiche. Dadurch wird eine größere Hürde für Schadsoftware geschaffen, die erwartet, dass ein Programm an einer bestimmten virtuellen Adresse geladen wird. Ihre App und alle von der App verwendeten Komponenten müssen über ASLR-Unterstützung verfügen.

**Was Sie tun sollten, wenn Ihre APP mit diesem Test fehlschlägt**

Aktivieren Sie beim Erstellen der App die Option "/DYNAMICBASE" im Linker-Befehl. Vergewissern Sie sich, dass auch alle von der App verwendeten Module diese Linker-Option verwenden.

**Rede**

ASLR hat in der Regel keine Auswirkungen auf die Leistung. In einigen Szenarios ist auf 32-Bit-Systemen aber eine geringfügige Leistungsverbesserung zu beobachten. Es ist möglich, dass sich die Leistung bei einem stark belasteten System verschlechtert, bei dem viele Bilder an vielen unterschiedlichen Speicherbereichen geladen sind.

Dieser Test wird nur für Apps ausgeführt, die in nicht verwalteten Sprachen geschrieben wurden, z. B. mit C oder C++.

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>Frei gegebener PE-Abschnitt lesen/schreiben

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** Fehler beim SharedSectionsCheck-Test.

Binärdateien mit beschreibbaren Abschnitten, die als freigegeben gekennzeichnet sind, stellen eine Sicherheitsbedrohung dar. Erstellen Sie keine Apps mit freigegebenen beschreibbaren Abschnitten, wenn dies nicht notwendig ist. Verwenden Sie [**CreateFileMapping**](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-createfilemappinga) oder [**MapViewOfFile**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffile), um ein freigegebenes Speicherobjekt zu erstellen, das korrekt gesichert ist.

**Was Sie tun sollten, wenn Ihre APP mit diesem Test fehlschlägt**

Entfernen Sie sämtliche freigegebenen Abschnitte aus der App, und erstellen Sie freigegebene Speicherobjekte, indem Sie [**CreateFileMapping**](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-createfilemappinga) oder [**MapViewOfFile**](https://docs.microsoft.com/windows/desktop/api/memoryapi/nf-memoryapi-mapviewoffile) mit den passenden Sicherheitsattributen aufrufen und die App dann neu erstellen.

**Rede**

Dieser Test wird nur für Apps ausgeführt, die in nicht verwalteten Sprachen geschrieben wurden, z. B. mit C oder C++.

### <a name="appcontainercheck"></a>AppContainerCheck

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** Fehler beim AppContainerCheck-Test.

Der AppContainerCheck-Test prüft, ob das **appcontainer**-Bit im PE-Header einer ausführbaren Binärdatei gesetzt ist. Für Apps muss das **appcontainer**-Bit für alle EXE-Dateien und nicht verwalteten DLLs gesetzt sein, damit diese korrekt ausgeführt werden.

**Was Sie tun sollten, wenn Ihre APP mit diesem Test fehlschlägt**

Wenn der Test für eine systemeigene ausführbare Datei nicht erfolgreich ist, stellen Sie sicher, dass Sie zum Erstellen der Datei den aktuellen Compiler und Linker und für den Linker das Kennzeichen */appcontainer* verwenden.

Wenn eine verwaltete ausführbare Datei den Test nicht bestanden hat, stellen Sie sicher, dass Sie den neuesten Compiler und Linker (z. b. Microsoft Visual Studio) zum Erstellen der UWP-App verwendet haben.

**Rede**

Dieser Test wird für alle EXE-Dateien und nicht verwalteten DLLs ausgeführt.

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>Executableimportscheck

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** Fehler beim ExecutableImportsCheck-Test.

Ein portierbares ausführbares Image (Portable Executable, PE) besteht diesen Test nicht, wenn es in einen Abschnitt mit ausführbaren Code eingefügt wurde. Dies kann auftreten, wenn Sie für das PE-Image das Zusammenführen von „.rdata“ ermöglicht haben, indem Sie das Kennzeichen */merge* des Visual C++-Linkers auf */merge:.rdata=.text* festgelegt haben.

**Was Sie tun sollten, wenn Ihre APP mit diesem Test fehlschlägt**

Führen Sie die Importtabelle nicht in einem Abschnitt mit ausführbarem Code zusammen. Stellen Sie sicher, dass das Kennzeichen */merge* des Visual C++-Linkers nicht so eingestellt ist, dass der Abschnitt „.rdata“ in einem Codeabschnitt zusammengeführt wird.

**Rede**

Dieser Test wird für den gesamten Binärcode ausgeführt, außer für ausschließlich verwaltete Assemblys.

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>Wxcheck

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** WXCheck Test failed (Fehler beim WXCheck-Test).

Mit dieser Überprüfung können Sie sicherstellen, dass eine Binärdatei keine Seiten enthält, die als schreibbar und ausführbar gekennzeichnet sind. Dies kann vorkommen, wenn die Binärdatei über einen beschreibbaren und ausführbaren Abschnitt verfügt oder wenn der *sectionalignment* der Binärdatei kleiner als die *Seiten\-Größe*ist.

**Was Sie tun sollten, wenn Ihre APP mit diesem Test fehlschlägt**

Stellen Sie sicher, dass die Binärdatei nicht über einen beschreibbaren oder ausführbaren Abschnitt verfügt und dass der *sectionalignment* -Wert der Binärdatei mindestens gleich seiner *Seiten\-Größe*ist.

**Rede**

Dieser Test wird für alle EXE-Dateien und systemeigenen, nicht verwalteten DLLs ausgeführt.

Eine ausführbare Datei kann einen schreibbaren und ausführbaren Abschnitt enthalten, wenn bei ihrer Erstellung "Bearbeiten und Fortfahren" aktiviert wurden (/ZI). Bei Deaktivierung von „Bearbeiten und Fortfahren“ ist der ungültige Abschnitt nicht mehr enthalten.

*Page\-size* ist die *standardsectionalignment* für ausführbare Dateien.

### <a name="private-code-signing"></a>Private Codesignatur

Überprüft, ob innerhalb des App-Pakets Binärdateien für private Codesignaturen vorhanden sind.

### <a name="background"></a>Hintergrund

Signaturdateien für privaten Code sollten privat bleiben, da sie im Fall einer Gefährdung zu bösartigen Zwecken missbraucht werden könnten.

### <a name="test-details"></a>Testdetails

Überprüft, ob innerhalb des App-Pakets Dateien mit der Erweiterung ".pfx" oder ".snk", die darauf hinweisen, dass private Signaturschlüssel verwendet wurden.

### <a name="corrective-actions"></a>Maßnahmen

Entfernen Sie alle privaten Code Signatur Schlüssel (z. b. pfx-und SNK-Dateien) aus dem Paket.

## <a name="supported-api-test"></a>Test der unterstützten APIs

Testet die App, um festzustellen, ob nicht kompatible APIs verwendet werden.

### <a name="background"></a>Hintergrund

Apps müssen die APIs für UWP-Apps (Windows-Runtime oder unterstützte Win32-APIs) verwenden, um für die Microsoft Store zertifiziert zu werden. Dieser Test ermittelt auch Situationen, in denen eine verwaltete Binärdatei eine Abhängigkeit von einer Funktion außerhalb des genehmigten Profils aufweist.

### <a name="test-details"></a>Testdetails

-   Überprüft, ob jede Binärdatei innerhalb des App-Pakets keine Abhängigkeit von einer Win32-API hat, die für die UWP-App-Entwicklung nicht unterstützt wird, indem die Import Adress Tabelle der Binärdatei überprüft wird.
-   Stellt sicher, dass eine verwaltete Binärdatei des App-Pakets keine Abhängigkeit von einer Funktion außerhalb des genehmigten Profils aufweist.

### <a name="corrective-actions"></a>Maßnahmen

Stellen Sie sicher, dass die App als Releasebuild und nicht als Debugbuild kompiliert wurde.

> **Beachten Sie**  der Debugbuild einer APP schlägt diesen Test auch dann fehl, wenn die app nur [APIs für UWP-apps](https://docs.microsoft.com/uwp/)verwendet.

Überprüfen Sie die Fehlermeldungen, um die von der APP verwendete API zu identifizieren, die keine [API für UWP-apps](https://docs.microsoft.com/uwp/)ist.

> **Hinweis**  C++ apps, die in einer Debugkonfiguration erstellt werden, schlagen diesen Test auch dann fehl, wenn die Konfiguration nur APIs aus dem Windows SDK für UWP-Apps verwendet. Weitere Informationen finden [Sie unter Alternativen zu Windows-APIs in UWP-apps](https://msdn.microsoft.com/library/windows/apps/hh464945.aspx) .

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
<tr><th>Fehlermeldung</th><th>Anmerkungen</th></tr>
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
<p>„MakePRI.exe“ unterstützt eine Option mit dem Namen <strong>AutoMerge</strong>. Der Standardwert von <strong>AutoMerge</strong> ist <strong>Aus</strong>. Ist sie aktiviert, führt <strong>AutoMerge</strong> die App-Sprachpaketressourcen in einer einzelnen Datei „resources.pri“ zur Laufzeit zusammen. Dies wird für apps, die Sie über die Microsoft Store verteilen möchten, nicht empfohlen. Die Ressourcen. pri einer APP, die über den Microsoft Store verteilt wird, muss sich im Stammverzeichnis des App-Pakets befinden und alle sprach Verweise enthalten, die von der App unterstützt werden.</p>
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
<p>Die Bilddatei "{filename}" muss kleiner als 204800 Bytes sein.\*\*</p>
</td><td>
<p>Verringern Sie die Größe der angegebenen Bilder.</p>
</td></tr>
<tr><td>
<p>Die Datei "{filename}" darf keinen umgekehrten Karten Abschnitt enthalten.\*\*</p>
</td><td>
<p>Die umgekehrte Zuordnung wird zwar während des Debuggens mit F5 in Visual Studio beim Aufrufen von „makepri.exe“ generiert, sie kann jedoch entfernt werden, indem „makepri.exe“ beim Generieren einer PRI-Datei ohne den Parameter „/m“ ausgeführt wird.</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* gibt an, dass im Windows-zertifizierungskit für Windows-apps 3,3 ein Test für Windows 8.1 hinzugefügt wurde, und gilt nur, wenn Sie die entsprechende Version des Kits oder höher verwenden.</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>Branding-Validierung

UWP-apps sind vollständig und voll funktionsfähig. Apps, für die Standardbilder (aus Vorlagen oder SDK-Beispielen) verwendet werden, verfügen über eine schlechte Benutzeroberfläche und können im Store-Katalog nicht leicht identifiziert werden.

### <a name="test-details"></a>Testdetails

Bei diesem Test wird sichergestellt, dass die von der App verwendeten Bilder keine Standardbilder aus SDK-Beispielen oder aus Visual Studio sind.

### <a name="corrective-actions"></a>Maßnahmen

Ersetzen Sie Standardbilder durch eigene Bilder, die über Aussagekraft für Ihre App verfügen.

## <a name="debug-configuration-test"></a>Test der Debugkonfiguration

Testet die App, um sicherzustellen, dass es sich nicht um einen Debugbuild handelt.

### <a name="background"></a>Hintergrund

Um für die Microsoft Store zertifiziert zu werden, dürfen apps nicht für das Debuggen kompiliert werden, und Sie dürfen nicht auf Debugversionen einer ausführbaren Datei verweisen. Darüber hinaus müssen Sie den Code für die App optimiert erstellen, damit dieser Test bestanden wird.

### <a name="test-details"></a>Testdetails

Testen Sie die App, um sicherzustellen, dass es sich nicht um einen Debugbuild handelt und dass die App mit keinen Debugframeworks verknüpft ist.

### <a name="corrective-actions"></a>Maßnahmen

-   Erstellen Sie die APP als Releasebuild, bevor Sie Sie an den Microsoft Store senden.
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

Microsoft Store erfordert, dass alle Anwendungen, die Direct3D verwenden, ordnungsgemäß auf Featureebene 9\-1-Grafikkarten renderfähig sind oder fehlschlagen

Da Benutzer die Grafikhardware auf Ihrem Gerät nach der Installation der App ändern können, muss Ihre APP beim Start erkennen, ob die aktuelle Hardware die Mindestanforderungen erfüllt, wenn Sie eine minimale Featureebene über 9\-1 auswählen. Wenn die Mindestanforderungen nicht erfüllt sind, muss Ihre App dem Benutzer eine Meldung mit den Direct3D-Anforderungen anzeigen. Wird zudem eine App auf ein Gerät heruntergeladen, mit dem sie nicht kompatibel ist, sollte sie dies beim Start erkennen und dem Kunden eine Meldung bezüglich der erforderlichen Voraussetzungen anzeigen.

### <a name="test-details"></a>Testdetails

Der Test überprüft, ob die apps auf Featureebene 9\-1 korrekt dargestellt werden.

### <a name="corrective-action"></a>Maßnahmen

Stellen Sie sicher, dass Ihre APP auf Direct3D Featureebene 9\-1 korrekt gerendert wird, auch wenn Sie davon ausgehen, dass Sie auf einer höheren Funktionsebene ausgeführt wird Weitere Informationen finden Sie unter [Entwickeln für unterschiedliche Direct3D-Featureebenen](https://msdn.microsoft.com/library/windows/apps/hh994923.aspx).

### <a name="direct3d-trim-after-suspend"></a>Direct3D-Kürzung nach dem Anhalten

> **Beachten Sie**  dieser Test gilt nur für UWP-apps, die für Windows 8.1 und höher entwickelt wurden.

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

-   **ExclusiveTo-Attributtest:** Stellen Sie sicher, dass von UWP-Klassen keine Schnittstellen implementiert werden, die für eine andere Klasse als „ExclusiveTo” gekennzeichnet sind.
-   **Test auf Anordnung von Typen:** Stellen Sie sicher, dass die Metadaten für alle UWP-Typen in der WINMD-Datei enthalten sind, die im App-Paket über den längsten Namen mit Namespaceübereinstimmung verfügt.
-   **Test auf Groß-/Kleinschreibung von Typnamen:** Stellen Sie sicher, dass alle UWP-Typen im App-Paket eindeutige Namen aufweisen, bei denen die Groß-/Kleinschreibung nicht zu berücksichtigen ist. Vergewissern Sie sich außerdem, dass UWP-Typnamen im App-Paket nicht auch als Namespacename verwendet werden.
-   **Test auf Korrektheit des Typnamens:** Stellen Sie sicher, dass im globalen Namespace oder im Windows-Namespace der obersten Ebene keine UWP-Typen vorhanden sind.
-   **Test auf Korrektheit der allgemeinen Metadaten:** Stellen Sie sicher, dass der zum Generieren der Typen verwendete Compiler in Bezug auf die UWP-Spezifikationen auf dem neuesten Stand ist.
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

Überprüft, ob bei der Installation von Anwendungen keine Unterverzeichnisse erstellt werden, die länger als die maximale\-Pfad sind.

### <a name="background"></a>Hintergrund

Betriebssystemkomponenten (einschließlich der Eincheck Vorgänge, wwahost usw.) sind intern auf den maximalen\-Pfad für Dateisystem Pfade beschränkt und funktionieren für längere Pfade nicht ordnungsgemäß.

### <a name="test-details"></a>Testdetails

Mit dieser Option wird überprüft, ob der Pfad im App-Installationsverzeichnis den maximalen\-Pfad überschreitet.

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

* [Tests der Windows Desktop Bridge-App](windows-desktop-bridge-app-tests.md)
* [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 
