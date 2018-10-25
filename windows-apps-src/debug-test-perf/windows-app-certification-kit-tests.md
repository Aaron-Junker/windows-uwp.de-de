---
author: PatrickFarley
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Tests im Zertifizierungskit für Windows-Apps
description: Das Zertifizierungskit für Windows-Apps enthält eine Reihe von Tests, die sicherstellen, dass Ihre app im Microsoft Store veröffentlicht werden kann.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, Uwp, app-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: 49ecc472c8c1d4adebd8376fce9d2d5e6e2a955e
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/25/2018
ms.locfileid: "5516266"
---
# <a name="windows-app-certification-kit-tests"></a>Tests im Zertifizierungskit für Windows-Apps


Das [Zertifizierungskit für Windows-Apps](windows-app-certification-kit.md) enthält eine Reihe von Tests, mit deren Hilfe, stellen Sie sicher, dass Ihre app an den Microsoft Store veröffentlicht werden kann. Die Tests sind unten aufgeführt, mit ihren Suchkriterien, Details, und empfohlene Maßnahmen bei einem Fehler.

## <a name="deployment-and-launch-tests"></a>Bereitstellungs- und Starttests

Überwachen die App während des Zertifizierungstests, um aufzuzeichnen, wann sie abstürzt oder sich aufhängt.

### <a name="background"></a>Hintergrund

Apps, die nicht mehr reagieren oder abstürzen, können zu einem Datenverlust und zu einer Beeinträchtigung des Benutzererlebnisses führen.

Apps müssen voll funktionsfähig sein, ohne Windows-Kompatibilitätsmodi, AppHelp-Meldungen oder andere Kompatibilitätspatches zu verwenden.

Apps dürfen im Registrierungsschlüssel HKEY\-LOCAL\-MACHINE\\Software\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\AppInit\-DLLs keine DLLs zum Laden auflisten.

### <a name="test-details"></a>Testdetails

Die App wird beim Zertifizierungstest durchgehend auf Flexibilität und Stabilität geprüft.

Das Zertifizierungskit für Windows-Apps ruft [**IApplicationActivationManager::ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903) auf, um Apps zu starten. Damit eine App mit **ActivateApplication** gestartet werden kann, muss die Benutzerkontensteuerung (UAC) aktiviert sein und die Bildschirmauflösung mindestens 1024x768 oder 768x1024 betragen. Ist eine dieser Bedingungen nicht erfüllt, fällt die App bei diesem Test durch.

### <a name="corrective-actions"></a>Maßnahmen

Stellen Sie sicher, dass die Benutzerkontensteuerung (UAC) auf dem Test-PC aktiviert ist.

Führen Sie den Test auf einem PC mit ausreichend großem Bildschirm aus.

Falls die App nicht gestartet werden kann, obwohl die Testplattform die Voraussetzungen für [**ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903) erfüllt, können Sie das Problem mithilfe des Aktivierungsereignisprotokolls beheben. So finden Sie die Einträge im Ereignisprotokoll:

1.  Öffnen Sie die Datei „eventvwr.exe“, und navigieren Sie zum Ordner „Application and Services Log\\Microsoft\\Windows\\Immersive-Shell“.
2.  Filtern Sie die Ansicht entsprechend, um folgende Ereigniskennungen anzuzeigen: 5900-6000.
3.  Prüfen Sie die Protokolleinträge, um zu ermitteln, weshalb die App nicht gestartet wurde.

Führen Sie für die Datei mit dem Problem eine Problembehandlung durch, um das Problem zu identifizieren und zu beheben. Erstellen Sie die App neu, und wiederholen Sie den Test. Sie können auch überprüfen, ob im Protokollordner des Zertifizierungskits für Windows-Apps eine Dumpdatei erstellt wurde, die zum Debuggen Ihrer App verwendet werden kann.

## <a name="platform-version-launch-test"></a>Test der Funktionsfähigkeit auf Plattformversionen

Überprüft, ob die Windows-App auf einer zukünftigen Version des Betriebssystems ausgeführt werden kann. Dieser Test wurde bisher nur beim Desktop-App-Workflow angewendet, ist jetzt jedoch für die Workflows für den Store und die universelle Windows-Plattform (UWP) aktiviert.

### <a name="background"></a>Hintergrund

Verwendung der betriebssystemversionsinformationen für den Microsoft Store ist eingeschränkt. Diese wurde von Apps häufig fälschlicherweise zum Überprüfen der Betriebssystemversion verwendet, damit Benutzern spezielle Funktionen für eine bestimmte Betriebssystemversion von der App bereitgestellt werden können.

### <a name="test-details"></a>Testdetails

Das Zertifizierungskit für Windows-Apps verwendet „HighVersionLie“, um zu ermitteln, wie die App die Betriebssystemversion prüft. Wenn die App abstürzt, besteht sie diesen Test nicht.

### <a name="corrective-action"></a>Maßnahmen

Apps sollten dies anhand von API-Funktionen zur Versionsabfrage überprüfen. Weitere Informationen finden Sie unter [Version des Betriebssystems](https://msdn.microsoft.com/library/windows/desktop/ms724832).

## <a name="background-tasks-cancellation-handler-validation"></a>Überprüfung des Abbruchhandlers für Aufgaben

Dieser Test stellt sicher, dass die App über einen Abbruchhandler für deklarierte Hintergrundaufgaben verfügt. Es muss eine dedizierte Funktion vorhanden sein, die beim Abbruch der Aufgabe aufgerufen wird. Dieser Test wird nur für bereitgestellte Apps durchgeführt.

### <a name="background"></a>Hintergrund

Windows-Apps können einen Prozess registrieren, der im Hintergrund ausgeführt wird. Eine E-Mail-App kann z.B. von Zeit zu Zeit einen Pingbefehl an einen Server senden. Wenn das Betriebssystem diese Ressourcen jedoch benötigt, wird die Hintergrundaufgabe abgebrochen, und Apps sollten diesen Abbruch problemlos behandeln. Apps ohne Abbruchhandler stürzen u.U. ab oder werden nicht geschlossen, wenn der Benutzer versucht, die App zu schließen.

### <a name="test-details"></a>Testdetails

Die App wird gestartet und angehalten, und der Teil der App, der sich nicht im Hintergrund befindet, wird beendet. Anschließend werden die Hintergrundaufgaben im Zusammenhang mit dieser App abgebrochen. Der Zustand der App wird überprüft, und wenn die App noch ausgeführt, besteht sie diesen Test nicht.

### <a name="corrective-action"></a>Maßnahmen

Fügen Sie Ihrer App den Abbruchhandler hinzu. Weitere Informationen finden Sie unter [Unterstützen der App mit Hintergrundaufgaben](https://msdn.microsoft.com/library/windows/apps/Mt299103).

## <a name="app-count"></a>App-Anzahl

Dieser Test stellt sicher, dass ein App-Paket (APPX, App-Bündel) genau eine Anwendung enthält. Dies wurde im Kit zu einem eigenständigen Test geändert.

### <a name="background"></a>Hintergrund

Dieser Test wurde gemäß der Store-Richtlinie implementiert.

### <a name="test-details"></a>Testdetails

Für Windows Phone8.1-Apps wird mit dem Test geprüft, ob die Gesamtzahl der APPX-Pakete im Bündel kleiner als &lt; 512 ist, ob nur ein Hauptpaket im Bündel vorhanden ist und ob die Architektur des Hauptpakets im Bündel als ARM oder neutral gekennzeichnet ist.

Für Windows10-Apps wird mit dem Test geprüft, ob die Revisionsnummer in der Version des Bündels auf 0 festgelegt ist.

### <a name="corrective-action"></a>Maßnahmen

Stellen Sie sicher, dass App-Paket und -Bündel die weiter oben in den Testdetails angegebenen Anforderungen erfüllen.

## <a name="app-manifest-compliance-test"></a>Test der Erfüllung technischer Anforderungen für das App-Manifest

Überprüft die Inhalte des App-Manifests auf Korrektheit.

### <a name="background"></a>Hintergrund

Apps müssen ein korrekt formatiertes App-Manifest besitzen.

### <a name="test-details"></a>Testdetails

Überprüft das App-Manifest, um sicherzustellen, dass der Inhalt der Beschreibung in den [App-Paketanforderungen](https://msdn.microsoft.com/library/windows/apps/Mt148525) entspricht.

-   **Dateierweiterungen und Protokolle**

    Von der App können die Dateierweiterungen deklariert werden, die ihr zugeordnet werden sollen. Bei nicht korrekter Verwendung wird von einer App u.U. eine große Anzahl von Dateierweiterungen deklariert, von denen die meisten nicht verwendet werden. Darunter leidet die Benutzerfreundlichkeit. Mit diesem Test wird eine Überprüfung der Anzahl von Dateierweiterungen durchgeführt, die einer App zugeordnet werden können.

-   **Frameworkabhängigkeitsregel**

    Mit diesem Test wird die Anforderung durchgesetzt, dass für die Apps geeignete Abhängigkeiten von der UWP bestehen. Für den Test tritt ein Fehler auf, wenn eine unzulässige Abhängigkeit besteht.

    Liegt ein Konflikt zwischen der Betriebssystemversion, in der die App verwendet wird, und den bestehenden Frameworkabhängigkeiten vor, schlägt der Test fehl. Der Test schlägt ebenfalls fehl, wenn sich die App auf eine Vorschauversion der Framework-DLLs bezieht.

-   **Überprüfung der prozessübergreifenden Kommunikation (Inter-process Communication, IPC)**

    Bei diesem Test wird die Anforderung durchgesetzt, die UWP-apps nicht außerhalb des app-Containers mit Desktopkomponenten kommunizieren. Die prozessübergreifende Kommunikation ist nur für quergeladene Apps vorgesehen. Apps, die für [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) den Namen „DesktopApplicationPath“ angeben, bestehen diesen Test nicht.

### <a name="corrective-action"></a>Maßnahmen

Gleichen Sie das App-Manifest mit den [App-Paketanforderungen](https://msdn.microsoft.com/library/windows/apps/Mt148525) ab.

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
-   [/SafeSEH-Ausnahmebehandlungsschutz](#binscope-2)
-   [Datenausführungsverhinderung](#binscope-3)
-   [Zufallsgestaltung des Adressraumlayouts](#binscope-4)
-   [Lesen/Schreiben des freigegebenen PE-Abschnitts](#binscope-5)
-   [AppContainerCheck](#appcontainercheck)
-   [ExecutableImportsCheck](#binscope-7)
-   [WXCheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>AllowPartiallyTrustedCallersAttribute

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** APTCACheck Test failed (Fehler beim APTCACheck-Test.)

Das AllowPartiallyTrustedCallersAttribute-Attribut (kurz: APTCA-Attribut) ermöglicht den Zugriff auf vollständig vertrauenswürdigen Code aus teilweise vertrauenswürdigem Code in signierten Assemblys. Wenn Sie das APTCA-Attribut auf eine Assembly anwenden, können teilweise vertrauenswürdige Aufrufer diese Assembly aufrufen, solange die Assembly besteht. Dies kann ein Sicherheitsrisiko darstellen.

**Was ist zu tun, wenn die App den Test nicht besteht?**

Verwenden Sie das APTCA-Attribut nur dann für Assemblys mit starkem Namen, falls dies für das Projekt erforderlich ist und die Risiken bekannt sind. Falls das Attribut erforderlich ist, stellen Sie sicher, dass alle APIs durch geeignete Sicherheitsanforderungen für den Codezugriff geschützt sind. APTCA hat keine Auswirkung, wenn die Assembly Teil einer UWP-App (Universelle Windows-Plattform) ist.

**Anmerkungen**

Dieser Test wird nur für verwalteten Code (C#, .NET usw.) ausgeführt.

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>/SafeSEH-Ausnahmebehandlungsschutz

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** SafeSEHCheck Test failed (Fehler beim SafeSEHCheck-Test).

Ein Ausnahmehandler wird ausgeführt, wenn in der App eine Ausnahmebedingung auftritt – beispielsweise bei einem Fehler aufgrund einer Division durch Null. Da die Adresse des Ausnahmehandlers beim Aufrufen einer Funktion im Stapel gespeichert wird, besteht das Risiko eines Pufferüberlaufangriffs, sollte Schadsoftware den Stapel überschreiben.

**Was ist zu tun, wenn die App den Test nicht besteht?**

Aktivieren Sie beim Erstellen der App die Option "/SAFESEH" im Linker-Befehl. Diese Option ist in den Veröffentlichungskonfigurationen von VisualStudio standardmäßig aktiviert. Vergewissern Sie sich, dass diese Option in den Erstellungsanweisungen für alle alle ausführbaren Module Ihrer App aktiviert ist.

**Anmerkungen**

Für 64-Bit-Binärdateien oder für Binärdateien für den ARM-Chipsatz wird dieser Test nicht ausgeführt, da hier keine Ausnahmehandleradressen im Stapel gespeichert werden.

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>Datenausführungsverhinderung

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** NXCheck Test failed (Fehler beim NXCheck-Test.)

Dieser Test stellt sicher, dass die App keinen Code ausführt, der in einem Datensegment gespeichert ist.

**Was ist zu tun, wenn die App den Test nicht besteht?**

Aktivieren Sie beim Erstellen der App die Option „/NXCOMPAT“ im Linker-Befehl. Diese Option ist in Linker-Versionen mit Unterstützung der Datenausführungsverhinderung (Data Execution Prevention, DEP) standardmäßig aktiviert.

**Anmerkungen**

Wir empfehlen, Apps auf einer DEP-fähigen CPU zu testen und alle DEP-bedingten Fehler zu beheben.

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>Zufallsgestaltung des Adressraumlayouts

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** DBCheck Test failed (Fehler beim DBCheck-Test.)

Die Zufallsgestaltung des Adressraumlayouts (Address Space Layout Randomization, ASLR) lädt ausführbare Bilder in unvorhersehbare Speicherbereiche. Dadurch wird eine größere Hürde für Schadsoftware geschaffen, die erwartet, dass ein Programm an einer bestimmten virtuellen Adresse geladen wird. Ihre App und alle von der App verwendeten Komponenten müssen über ASLR-Unterstützung verfügen.

**Was ist zu tun, wenn die App den Test nicht besteht?**

Aktivieren Sie beim Erstellen der App die Option "/DYNAMICBASE" im Linker-Befehl. Vergewissern Sie sich, dass auch alle von der App verwendeten Module diese Linker-Option verwenden.

**Anmerkungen**

ASLR hat in der Regel keine Auswirkungen auf die Leistung. In einigen Szenarios ist auf 32-Bit-Systemen aber eine geringfügige Leistungsverbesserung zu beobachten. Es ist möglich, dass sich die Leistung bei einem stark belasteten System verschlechtert, bei dem viele Bilder an vielen unterschiedlichen Speicherbereichen geladen sind.

Dieser Test wird nur für Apps ausgeführt, die in nicht verwalteten Sprachen geschrieben wurden, z.B. mit C oder C++.

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>Lesen/Schreiben des freigegebenen PE-Abschnitts

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** SharedSectionsCheck Test failed (Fehler beim SharedSectionsCheck-Test).

Binärdateien mit beschreibbaren Abschnitten, die als freigegeben gekennzeichnet sind, stellen eine Sicherheitsbedrohung dar. Erstellen Sie keine Apps mit freigegebenen beschreibbaren Abschnitten, wenn dies nicht notwendig ist. Verwenden Sie [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) oder [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761), um ein freigegebenes Speicherobjekt zu erstellen, das korrekt gesichert ist.

**Was ist zu tun, wenn die App den Test nicht besteht?**

Entfernen Sie sämtliche freigegebenen Abschnitte aus der App, und erstellen Sie freigegebene Speicherobjekte, indem Sie [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) oder [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761) mit den passenden Sicherheitsattributen aufrufen und die App dann neu erstellen.

**Anmerkungen**

Dieser Test wird nur für Apps ausgeführt, die in nicht verwalteten Sprachen geschrieben wurden, z.B. mit C oder C++.

### <a name="appcontainercheck"></a>AppContainerCheck

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** AppContainerCheck Test failed (Fehler beim AppContainerCheck-Test).

Der AppContainerCheck-Test prüft, ob das **appcontainer**-Bit im PE-Header einer ausführbaren Binärdatei gesetzt ist. Für Apps muss das **appcontainer**-Bit für alle EXE-Dateien und nicht verwalteten DLLs gesetzt sein, damit diese korrekt ausgeführt werden.

**Was ist zu tun, wenn die App den Test nicht besteht?**

Wenn der Test für eine systemeigene ausführbare Datei nicht erfolgreich ist, stellen Sie sicher, dass Sie zum Erstellen der Datei den aktuellen Compiler und Linker und für den Linker das Kennzeichen */appcontainer* verwenden.

Wenn eine verwaltete ausführbare Datei der Test fehlschlägt, stellen Sie sicher, dass Sie den aktuellen Compiler und Linker wie beispielsweise Microsoft Visual Studio verwendet, um die UWP-app erstellen.

**Anmerkungen**

Dieser Test wird für alle EXE-Dateien und nicht verwalteten DLLs ausgeführt.

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>ExecutableImportsCheck

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** ExecutableImportsCheck Test failed (Fehler beim ExecutableImportsCheck-Test).

Ein portierbares ausführbares Image (Portable Executable, PE) besteht diesen Test nicht, wenn es in einen Abschnitt mit ausführbaren Code eingefügt wurde. Dies kann auftreten, wenn Sie für das PE-Image das Zusammenführen von „.rdata“ ermöglicht haben, indem Sie das Kennzeichen */merge* des Visual C++-Linkers auf */merge:.rdata=.text* festgelegt haben.

**Was ist zu tun, wenn die App den Test nicht besteht?**

Führen Sie die Importtabelle nicht in einem Abschnitt mit ausführbarem Code zusammen. Stellen Sie sicher, dass das Kennzeichen */merge* des Visual C++-Linkers nicht so eingestellt ist, dass der Abschnitt „.rdata“ in einem Codeabschnitt zusammengeführt wird.

**Anmerkungen**

Dieser Test wird für den gesamten Binärcode ausgeführt, außer für ausschließlich verwaltete Assemblys.

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>WXCheck

**Fehlermeldung des Zertifizierungskits für Windows-Apps:** WXCheck Test failed (Fehler beim WXCheck-Test).

Mit dieser Überprüfung können Sie sicherstellen, dass eine Binärdatei keine Seiten enthält, die als schreibbar und ausführbar gekennzeichnet sind. Dies kann vorkommen, wenn die Binärdatei einen schreibbaren und ausführbaren Abschnitt enthält oder wenn *SectionAlignment* der Binärdatei kleiner als *PAGE\-SIZE* ist.

**Was ist zu tun, wenn die App den Test nicht besteht?**

Stellen Sie sicher, dass die Binärdatei keinen schreibbaren oder ausführbaren Abschnitt enthält und dass der *SectionAlignment*-Wert der Binärdatei mindestens seiner *PAGE\-SIZE* entspricht.

**Anmerkungen**

Dieser Test wird für alle EXE-Dateien und systemeigenen, nicht verwalteten DLLs ausgeführt.

Eine ausführbare Datei kann einen schreibbaren und ausführbaren Abschnitt enthalten, wenn bei ihrer Erstellung "Bearbeiten und Fortfahren" aktiviert wurden (/ZI). Bei Deaktivierung von „Bearbeiten und Fortfahren“ ist der ungültige Abschnitt nicht mehr enthalten.

*PAGE\-SIZE* ist der Standardwert von *SectionAlignment* für ausführbare Dateien.

### <a name="private-code-signing"></a>Private Codesignatur

Überprüft, ob innerhalb des App-Pakets Binärdateien für private Codesignaturen vorhanden sind.

### <a name="background"></a>Hintergrund

Signaturdateien für privaten Code sollten privat bleiben, da sie im Fall einer Gefährdung zu bösartigen Zwecken missbraucht werden könnten.

### <a name="test-details"></a>Testdetails

Überprüft, ob innerhalb des App-Pakets Dateien mit der Erweiterung ".pfx" oder ".snk", die darauf hinweisen, dass private Signaturschlüssel verwendet wurden.

### <a name="corrective-actions"></a>Maßnahmen

Entfernen Sie alle Signaturschlüssel für privaten Code (z.B. PFX- und SNK-Dateien) aus dem Paket.

## <a name="supported-api-test"></a>Test der unterstützten APIs

Testet die App, um festzustellen, ob nicht kompatible APIs verwendet werden.

### <a name="background"></a>Hintergrund

Apps müssen die APIs für UWP-apps (Windows-Runtime- oder unterstützte Win32-APIs), um für den Microsoft Store zertifiziert zu werden verwenden. Dieser Test ermittelt auch Situationen, in denen eine verwaltete Binärdatei eine Abhängigkeit von einer Funktion außerhalb des genehmigten Profils aufweist.

### <a name="test-details"></a>Testdetails

-   Stellt sicher, dass jeder Binärdatei innerhalb des app-Pakets keine Abhängigkeit von einer Win32-API aufweist, die nicht für die Entwicklung von UWP-Apps unterstützt wird, indem Sie die Importadressentabelle der Binärdatei.
-   Stellt sicher, dass eine verwaltete Binärdatei des App-Pakets keine Abhängigkeit von einer Funktion außerhalb des genehmigten Profils aufweist.

### <a name="corrective-actions"></a>Maßnahmen

Stellen Sie sicher, dass die App als Releasebuild und nicht als Debugbuild kompiliert wurde.

> **Hinweis:** das Debugbuild einer App diesen Test nicht, auch wenn die app nur [APIs für UWP-apps](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)verwendet.

Überprüfen Sie die Fehlermeldungen, um der API zu identifizieren, die app verwendet, die eine [API für UWP-apps](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx)nicht ist.

> **Hinweis:** C++-apps, die unter einer Debugkonfiguration erstellt wurden, auch wenn die Konfiguration nur APIs aus dem Windows SDK für UWP-apps verwendet diesen Test nicht. Siehe [alternativen zu Windows-APIs in UWP-apps](http://go.microsoft.com/fwlink/p/?LinkID=244022) für Weitere Informationen.

## <a name="performance-tests"></a>Leistungstests

Die App muss schnell auf Benutzerinteraktionen und Systembefehle reagieren, um Benutzern eine schnelle und flüssige Benutzeroberfläche zu bieten.

Die Merkmale des Computers, auf dem der Test ausgeführt wird, können die Testergebnisse beeinflussen. Die Schwellenwerte des Leistungstests für die App-Zertifizierung sind so festgelegt, dass Computer mit geringem Energieverbrauch die Erwartungen der Kunden an eine schnelle und flüssige Benutzeroberfläche erfüllen. Um die Leistung Ihrer App zu ermitteln, empfehlen wir, die App auf einem PC mit geringem Energieverbrauch (z.B. einem PC mit Intel Atom-Prozessor) bei einer Auflösung von mindestens 1366x768 und mit einem herkömmlichen Festplattenlaufwerk (im Gegensatz zu einem Festkörperlaufwerk) zu testen.

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
<p>In der tatsächlichen Meldung enthält „{Bildname}“ den Namen des Bilds mit dem Fehler.</p>
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
<p>Weitere Informationen finden Sie unter <a href="https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx">Reaktionsfähiges Design für UWP-Apps (Universelle Windows-Plattform) – Grundlagen</a> und <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">Richtlinien für App-Ressourcen</a>.</p>
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
<p>„MakePRI.exe“ unterstützt eine Option mit dem Namen <strong>AutoMerge</strong>. Der Standardwert von <strong>AutoMerge</strong> ist <strong>Aus</strong>. Ist sie aktiviert, führt <strong>AutoMerge</strong> die App-Sprachpaketressourcen in einer einzelnen Datei „resources.pri“ zur Laufzeit zusammen. Wird dies nicht für apps empfohlen, die Sie über den Microsoft Store vertreiben möchten. Die Datei "Resources.pri" einer App, die über den Microsoft Store vertriebenen müssen im Stammverzeichnis des app Pakets werden und alle Sprachverweise, die von der app unterstützten enthalten.</p>
</td></tr>
<tr><td>
<p>Die Zeichenfolge „{string}“ entspricht nicht der Längenbeschränkung von maximal {number} Zeichen.</p>
</td><td>
<p>Weitere Informationen finden Sie unter <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">App-Paketanforderungen</a>.</p>
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
<p>Weitere Informationen finden Sie unter <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">App-Paketanforderungen</a>.</p>
</td></tr>
<tr><td>
<p>In der Datei „resources.pri” ist keine Standardressource angegeben.</p>
</td><td>
<p>Weitere Informationen finden Sie unter <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">Richtlinien für App-Ressourcen</a>.</p>
<p>In der Standardbuildkonfiguration nimmt Visual Studio nur Bildressourcen mit der Skalierung 200% in das App-Paket auf, wenn ein Bündel generiert wird, andere Ressourcen werden im Ressourcenpaket abgelegt. Stellen Sie sicher, dass Sie entweder Bildressourcen mit der Skalierung 200% einschließen oder Ihr Projekt für die Aufnahme der vorhandenen Ressourcen konfigurieren.</p>
</td></tr>
<tr><td>
<p>In der Datei „resources.pri“ ist kein Ressourcenwert angegeben.</p>
</td><td>
<p>Stellen Sie sicher, dass für das App-Manifest gültige Ressourcen in „resources.pri“ definiert sind.</p>
</td></tr>
<tr><td>
<p>Die Bilddatei „{Dateiname}“ muss kleiner als 204.800Bytes sein.\*\*</p>
</td><td>
<p>Verringern Sie die Größe der angegebenen Bilder.</p>
</td></tr>
<tr><td>
<p>Die Datei „{Dateiname}“ darf keinen Abschnitt mit umgekehrter Zuordnung enthalten.\*\*</p>
</td><td>
<p>Die umgekehrte Zuordnung wird zwar während des Debuggens mit F5 in VisualStudio beim Aufrufen von „makepri.exe“ generiert, sie kann jedoch entfernt werden, indem „makepri.exe“ beim Generieren einer PRI-Datei ohne den Parameter „/m“ ausgeführt wird.</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* bedeutet, dass der Version3.3 des Zertifizierungskits für Windows-Apps für Windows 8.1 ein Test hinzugefügt wurde, der nur bei der Verwendung dieser oder einer höheren Version des Kits anwendbar ist.</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>Branding-Validierung

UWP-apps sollten vollständig und betriebsbereit sein. Apps, für die Standardbilder (aus Vorlagen oder SDK-Beispielen) verwendet werden, verfügen über eine schlechte Benutzeroberfläche und können im Store-Katalog nicht leicht identifiziert werden.

### <a name="test-details"></a>Testdetails

Bei diesem Test wird sichergestellt, dass die von der App verwendeten Bilder keine Standardbilder aus SDK-Beispielen oder aus VisualStudio sind.

### <a name="corrective-actions"></a>Maßnahmen

Ersetzen Sie Standardbilder durch eigene Bilder, die über Aussagekraft für Ihre App verfügen.

## <a name="debug-configuration-test"></a>Test der Debugkonfiguration

Testet die App, um sicherzustellen, dass es sich nicht um einen Debugbuild handelt.

### <a name="background"></a>Hintergrund

Um für den Microsoft Store zertifiziert zu werden, müssen apps nicht zum Debuggen kompiliert werden und sie nicht auf die Debugversionen einer ausführbaren Datei verweisen. Darüber hinaus müssen Sie den Code für die App optimiert erstellen, damit dieser Test bestanden wird.

### <a name="test-details"></a>Testdetails

Testen Sie die App, um sicherzustellen, dass es sich nicht um einen Debugbuild handelt und dass die App mit keinen Debugframeworks verknüpft ist.

### <a name="corrective-actions"></a>Maßnahmen

-   Erstellen Sie die app als Releasebuild, bevor Sie sie an den Microsoft Store übermitteln.
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

Testet MicrosoftDirect3D-Apps, um sicherzustellen, dass sie auf Geräten mit älterer Grafikhardware nicht abstürzen.

### <a name="background"></a>Hintergrund

Microsoft Store müssen alle Anwendungen mit Direct3D richtig gerendert bzw. ordnungsgemäß Feature Grafikkarten der Featureebene 9\-1-Karten.

Da die Benutzer die Grafikhardware ihrer Geräte nach der Installation der App ändern können, muss Ihre App für den Fall, dass Sie eine Featureebene höher als 9\-1 verwenden, beim Start erkennen, ob die aktuelle Hardware die Mindestanforderungen erfüllt. Wenn die Mindestanforderungen nicht erfüllt sind, muss Ihre App dem Benutzer eine Meldung mit den Direct3D-Anforderungen anzeigen. Wird zudem eine App auf ein Gerät heruntergeladen, mit dem sie nicht kompatibel ist, sollte sie dies beim Start erkennen und dem Kunden eine Meldung bezüglich der erforderlichen Voraussetzungen anzeigen.

### <a name="test-details"></a>Testdetails

Bei diesem Test wird überprüft, ob Apps unter der Featureebene9\-1 richtig gerendert werden.

### <a name="corrective-action"></a>Maßnahmen

Stellen Sie sicher, dass die App unter der Direct3D-Featureebene9\-1 richtig gerendert wird. Dies gilt auch, wenn die App für die Ausführung auf einer höheren Featureebene bestimmt ist. Weitere Informationen finden Sie unter [Entwickeln für unterschiedliche Direct3D-Featureebenen](http://go.microsoft.com/fwlink/p/?LinkID=253575).

### <a name="direct3d-trim-after-suspend"></a>Direct3D-Kürzung nach dem Anhalten

> **Hinweis:** dieser Test gilt nur für UWP-apps und höher für Windows8.1 entwickelt wurden.

### <a name="background"></a>Hintergrund

Wenn von der App auf ihrem jeweiligen Direct3D-Gerät [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) nicht aufgerufen wird, gibt sie keinen Speicher frei, der für die vorherigen 3D-Arbeitsschritte zugeordnet wurde. Dies erhöht das Risiko, dass Apps aufgrund von unzureichendem Systemspeicher beendet werden.

### <a name="test-details"></a>Testdetails

Apps werden auf die Einhaltung der d3d-Anforderungen überprüft. Außerdem wird sichergestellt, dass Apps beim Anhalterückruf (Suspend) eine neue [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346)-API aufrufen.

### <a name="corrective-action"></a>Maßnahmen

Von der App sollte die [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346)-API für ihre [**IDXGIDevice3**](https://msdn.microsoft.com/library/windows/desktop/Dn280345)-Schnittstelle vor jedem Anhaltevorgang aufgerufen werden.

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

Es wird sichergestellt, dass von Anwendungen während der Installation keine Unterverzeichnisse erstellt werden, die länger als MAX\-PATH sind.

### <a name="background"></a>Hintergrund

Komponenten des Betriebssystems (z.B. Trident, WWAHost usw.) sind für Dateisystempfade intern auf den MAX\-PATH-Wert begrenzt und funktionieren nicht ordnungsgemäß, wenn längere Pfade verwendet werden.

### <a name="test-details"></a>Testdetails

Es wird überprüft, ob für Pfade im Installationsverzeichnis der App der MAX\-PATH-Wert überschritten wird.

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

* [Tests für Windows Desktop-Brücke-Apps](windows-desktop-bridge-app-tests.md)
* [Microsoft Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 
