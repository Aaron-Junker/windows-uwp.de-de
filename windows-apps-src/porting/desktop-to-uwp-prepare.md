---
Description: Dieser Artikel beschreibt Dinge, die Sie vor dem Verpacken Ihrer desktop-Anwendung wissen müssen. Sie müssen möglicherweise nicht viel tun, um Ihre App für die Verpackung vorzubereiten.
Search.Product: eADQiWindows 10XVcnh
title: Vorbereitung zum Packen einer desktop-Anwendung (Desktop-Brücke)
ms.date: 05/18/20188
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.localizationpriority: medium
ms.openlocfilehash: 49238ba1b72cf3daf46fb076432a478e9f381bc6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57600895"
---
# <a name="prepare-to-package-a-desktop-application"></a>Packen eine desktop-Anwendung vorbereiten

In diesem Artikel sind Punkte aufgeführt, die Sie vor dem Verpacken von Desktop-Apps wissen sollten. Möglicherweise nicht viel, rufen Sie die Anwendung bereit für den Prozess der paketerstellung zu tun, aber wenn keines der folgenden Elemente bezieht sich auf Ihre Anwendung müssen Sie sie vor dem Packen. Denken Sie daran, dass Lizenzierung und automatische Updates im Rahmen des Microsoft Store erfolgen, sodass Sie alle mit diesen Aufgaben verbundenen Features aus Ihrer Codebasis entfernen können.

>[!IMPORTANT]
>Die Fähigkeit zum Erstellen einer Windows-app-Paket für die desktop-Anwendung (auch bekannt als die Desktop-Brücke) wurde in Windows 10 Version 1607 eingeführt, und es kann nur in Projekten, die Windows 10 Anniversary Update (10.0; als Ziel verwendet werden Build 14393) oder eine neuere Version in Visual Studio.

+ __Die Anwendung erfordert eine Version von .NET 4.6.2 vor__. Sie müssen sicherstellen, dass Ihre Anwendung auf .NET 4.6.2 ausgeführt wird. Sie können keine Versionen erfordern oder weiterverteilen, die älter als 4.6.2 sind. Dies ist die .NET-Version, die im Windows 10 Anniversary Update ausgeliefert wurde. Überprüfen, dass Ihre Anwendung in dieser Version funktioniert, wird sichergestellt, dass Ihre Anwendung weiterhin mit zukünftigen Updates von Windows 10 kompatibel sein.  Wenn Ihre Anwendung .NET Framework 4.0 oder höher ausgerichtet ist, es wird erwartet, die auf .NET 4.6.2 ausgeführt werden, jedoch sollten Sie immer noch testen.

+ __Ihre Anwendung immer ausgeführt wird, mit erhöhter Sicherheitsberechtigungen__. Die Anwendung muss während der Ausführung als interaktiver Benutzer funktioniert. Benutzer, die Ihre Anwendung aus dem Microsoft Store zu installieren, möglicherweise nicht Systemadministratoren, so dass Ihre Anwendung bedeutet, dass mit erhöhten Rechten ausgeführt werden, die sie nicht ordnungsgemäß für Standardbenutzer laufen. Apps, die für erhöhte Rechte für einen beliebigen Teil ihrer Funktionalität benötigen, werden nicht im Microsoft Store akzeptiert.

+ __Die Anwendung erfordert ein Kernel-Modus-Treiber oder einen Windows-Dienst__. Die Brücke eignet sich für eine App, ein Kernelmodustreiber oder ein Windows-Dienst, die unter einem Systemkonto ausgeführt werden müssen, werden jedoch nicht unterstützt. Verwenden Sie anstelle eines Windows-Diensts eine [Hintergrundaufgabe](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Ihre App-Module werden im Prozess für Prozesse geladen, die nicht im Windows-App-Paket enthalten sind__. Dies ist nicht zulässig. Prozessinterne Erweiterungen, beispielsweise [Shellerweiterungen](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), werden nicht unterstützt. Wenn zwei Apps in einem Paket enthalten sind, können Sie jedoch die prozessübergreifende Kommunikation zwischen diesen verwenden.

+ __Ihre Anwendung verwendet ein benutzerdefiniertes Application User Model ID (AUMID)__. Wenn der Prozess erfordert [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) um eine eigene AUMID festzulegen, es möglicherweise verwenden Sie nur die AUMID für sie von der Anwendung Modell Umgebung/Windows-app-Paket generiert. Sie können keine benutzerdefinierten Anwendungsbenutzermodell-IDs definieren.

+ __Die Anwendung ändert die Registrierungsstruktur von HKEY_LOCAL_MACHINE (HKLM)__. Bei jedem Versuch Ihrer Anwendung zum Erstellen eines Schlüssels HKLM oder öffnen Sie eine für die Änderung führt zu einem Zugriff-verweigert-Fehler. Denken Sie daran, dass die Anwendung ihre eigene private virtualisierte Ansicht der Registrierung, sodass das Konzept einer Registrierungsstruktur für Benutzer und computerweite (das ist was HKLM ist) nicht gilt. Sie müssen stattdessen eine andere Möglichkeit finden, um das zu erreichen, wozu Sie HKLM verwendet haben, beispielsweise Schreiben in HKEY_CURRENT_USER (HKCU).

+ __Ihre Anwendung verwendet einen Registrierungsunterschlüssel Ddeexec als Mittel zum Starten von einer anderen app__. Verwenden Sie stattdessen einen der DelegateExecute-Verbhandler genau wie von den verschiedenen Activatable*-Erweiterungen in dem [App-Paketmanifest](https://msdn.microsoft.com/library/windows/apps/br211474.aspx) konfiguriert.

+ __Die Anwendung schreibt, zu dem Ordner "AppData" oder in die Registrierung mit dem Ziel Freigeben von Daten mit einer anderen app__. Nach der Konvertierung wird AppData an den lokalen App-Datenspeicher weitergeleitet, der ein privater Speicher für jede UWP-App ist.

  Alle Einträge, die Ihre Anwendung in der Registrierungsstruktur HKEY_LOCAL_MACHINE schreibt werden in einer isolierten Binärdatei umgeleitet, und alle Einträge, die die Anwendung in der Registrierungsstruktur HKEY_CURRENT_USER schreibt in eine private pro Benutzer, pro-app-Speicherort platziert werden. Weitere Informationen zur Datei- und Registrierungsumleitung finden Sie unter [Hintergrundinformationen zur Desktop-Brücke](desktop-to-uwp-behind-the-scenes.md).  

  Verwenden Sie verschiedene Methoden für die prozessübergreifende Datenfreigabe. Weitere Informationen finden Sie unter [Speichern und Abrufen von Einstellungen und anderen App-Daten](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Ihre Anwendung schreibt in das Installationsverzeichnis für Ihre app__. Beispielsweise schreibt Ihre Anwendung in eine Protokolldatei, die Sie in demselben Verzeichnis wie die EXE-Datei eingefügt. Dies wird nicht unterstützt. Sie müssen daher einen anderen Speicherort wählen, z. B. den lokalen App-Datenspeicher.

+ __Die Anwendungsinstallation muss der Benutzer eingreifen__. Das Installationsprogramm der Anwendung muss in der Lage, im Hintergrund ausgeführt, und es muss alle erforderlichen Komponenten, die auf standardmäßig auf einem sauberen Betriebssystemimage nicht installieren.

+ __Ihre Anwendung verwendet das aktuelle Arbeitsverzeichnis__. Zur Laufzeit erhalten die Paket-desktop-Anwendung keine der gleichen Arbeitsverzeichnis, das Sie zuvor angegeben haben, auf Ihrem Desktop. LNK-Verknüpfung. Sie müssen Ihre CWD zur Laufzeit zu ändern, wenn das richtige Verzeichnis für Ihre Anwendung ordnungsgemäß wichtig ist.

+ __Die Anwendung erfordert UIAccess__. Wenn Ihre Anwendung `UIAccess=true` für das `requestedExecutionLevel`-Element im UAC-Manifest angibt, wird die Konvertierung in eine UWP-App derzeit nicht unterstützt. Weitere Informationen finden Sie unter [Sicherheit bei der Benutzeroberflächenautomatisierung – Übersicht](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Ihre Anwendung verfügbar macht, COM-Objekte__. Prozesse und Erweiterungen innerhalb des Pakets können OLE & COM-Server sowohl im Prozess und Out-of-Process (OOP) registrieren und verwenden.  Der Creators Update stellt verpackte COM-Unterstützung bereit, die die Möglichkeit bietet, OLE & OOP-COM-Server zu registrieren, die außerhalb des Pakets sichtbar sind.  Weitere Informationen finden Sie unter [COM-Server und OLE-Dokument-Support für Desktop-Brücke](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97).

   Die verpackte COM-Unterstützung funktioniert mit den vorhandenen COM-APIs. Sie funktioniert aber nicht bei Anwendungserweiterungen, die vom direkten Lesen der Registrierung abhängig sind, da der Speicherort für die verpackte COM privat ist.

+ __Ihre Anwendung verfügbar macht GAC-Assemblys für die Verwendung durch andere Prozesse__. In der aktuellen Version darf nicht Ihre Anwendung GAC-Assemblys für die Verwendung von Prozessen, stammen von ausführbaren Dateien, die externe auf Ihrem Windows-app-Paket verfügbar machen. Prozesse aus dem Paket können GAC-Assemblys wie gewohnt registrieren und verwenden, aber sie sind nicht extern sichtbar. Dies bedeutet, dass Interoperabilitätsszenarien wie OLE nicht funktionieren, wenn sie von externen Prozessen aufgerufen werden.

+ __Ihre Anwendung ist C-Laufzeitbibliotheken (CRT) in eine nicht unterstützte Weise verknüpfen__. Die Microsoft C/C++-Laufzeitbibliothek enthält Routinen für die Programmierung für das Microsoft Windows-Betriebssystem. Diese Routinen automatisieren viele allgemeine Programmieraufgaben, die von den Programmiersprachen C# und C++ nicht bereitgestellt werden. Wenn Ihre Anwendung C/C++-Laufzeitbibliothek nutzt, müssen Sie sicherstellen, dass er auf eine unterstützte Weise verknüpft ist.

    Visual Studio 2017 unterstützt die statische und dynamische Verknüpfung, damit Ihr Code gängige DLL-Dateien verwenden kann, und statische Verknüpfung, um die Bibliothek direkt in Ihren Code, mit der aktuellen Version der CRT, zu verknüpfen. Wenn möglich, empfehlen wir die Anwendung verwenden, die dynamische Verknüpfung mit Visual Studio 2017.

    Unterstützung in früheren Versionen von Visual Studio variiert. Weitere Informationen finden Sie in der folgenden Tabelle:

    <table>
    <th>Version von Visual Studio</td><th>Dynamische Verknüpfung</th><th>Statische Verknüpfung</th></th>
    <tr><td>2005 (VC 8)</td><td>Nicht unterstützt.</td><td>Unterstützt</td>
    <tr><td>2008 (VC 9)</td><td>Nicht unterstützt.</td><td>Unterstützt</td>
    <tr><td>2010 (VC 10)</td><td>Unterstützt</td><td>Unterstützt</td>
    <tr><td>2012 (VC 11)</td><td>Unterstützt</td><td>Nicht unterstützt.</td>
    <tr><td>2013 (VC 12)</td><td>Unterstützt</td><td>Nicht unterstützt.</td>
    <tr><td>2015 und 2017 (VC 14)</td><td>Unterstützt</td><td>Unterstützt</td>
    </table>

    Hinweis: In allen Fällen müssen Sie eine Verknüpfung auf die neueste öffentlich verfügbaren CRT.

+ __Die Anwendung installiert und lädt Sie Assemblys aus dem Windows-Seite-an-Seite-Ordner__. Beispielsweise wird Ihre Anwendung C-Laufzeitbibliotheken VC8 oder VC9 verwendet und sie wird dynamisch verknüpfen aus Windows-Seite-an-Seite-Ordner, was bedeutet, dass Ihrem Code allgemeine DLL-Dateien aus einem freigegebenen Ordner verwendet wird. Dies wird nicht unterstützt. Sie müssen diese statisch verknüpfen, indem sie eine Verknüpfung mit den verteilbaren Bibliotheksdateien direkt in den Code erstellen.

+ __Ihre Anwendung verwendet eine Abhängigkeit im Ordner "System32" / SysWOW64__. Damit diese DLL-Dateien funktionieren, müssen Sie sie im virtuellen Dateisystem Ihres Windows-App-Pakets einschließen. Dadurch wird sichergestellt, dass die Anwendung verhält, als wären die DLLs im installiert die **"System32"**/**SysWOW64** Ordner. Erstellen Sie im Stammordner des Pakets einen Ordner mit dem Namen **VFS**. Erstellen Sie innerhalb dieses Ordners die Ordner **SystemX64** und **SystemX86**. Platzieren Sie die 32-Bit-Version Ihrer DLL-Datei im Ordner **SystemX86** und die 64-Bit-Version im Ordner **SystemX64**.

+ __Ihre App verwendet ein VCLibs-Frameworkpaket__. Die VCLibs-Bibliotheken können direkt aus dem Microsoft Store installiert werden, wenn sie im Windows-App-Paket als Abhängigkeit definiert sind. Beispielsweise wenn Ihre Anwendung Dev11 VCLibs-Pakete verwendet, nehmen Sie folgende Änderung zum Anwendungsmanifest-Paket: Unter den `<Dependencies>` Knoten hinzufügen:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Während der Installation aus dem Microsoft Store wird die korrekte Version des VCLibs-Frameworks (x86 oder x64) vor der Installation der App installiert.  
Die Abhängigkeiten werden nicht installiert, wenn die Anwendung durch Sideloaden installiert ist. Um die Abhängigkeiten manuell auf dem Computer zu installieren, müssen Sie das passende VCLibs-Frameworkpaket für die Desktop-Brücke herunterladen und installieren. Weitere Informationen über diese Szenarien finden Sie unter [Verwenden der Visual C++-Laufzeitbibliotheken in einem Centennial-Projekt](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

  **Frameworkpakete**:

  * [VC-14.0-Framework-Pakete für Desktop-Brücke](https://www.microsoft.com/download/details.aspx?id=53175)
  * [VC-12.0-Framework-Pakete für Desktop-Brücke](https://www.microsoft.com/download/details.aspx?id=53176)
  * [VC-11.0-Framework-Pakete für Desktop-Brücke](https://www.microsoft.com/download/details.aspx?id=53340)


+ __Die Anwendung enthält eine benutzerdefinierte Sprungliste__. Bei der Verwendung von Sprunglisten müssen mehrere Probleme und Vorsichtsmaßnahme berücksichtigt werden.

    - __Die Architektur Ihrer app entspricht nicht dem Betriebssystem.__  Sprunglisten derzeit nicht ordnungsgemäß, wenn die Anwendung und Betriebssystem-Architekturen nicht übereinstimmen (z. B. x X86 Anwendung X64 unter Windows). Zu diesem Zeitpunkt ist gibt es keine problemumgehung außer die Anwendung auf die entsprechende Architektur neu kompilieren.

    - __Ihre Anwendung erstellt wird, Listeneinträge springen und ruft [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) oder [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. Legen Sie „AppID“ nicht programmgesteuert im Code fest. Andernfalls werden die Sprunglisteneinträge nicht angezeigt. Wenn Ihre Anwendung eine benutzerdefinierte Id benötigt wird, geben Sie es mithilfe der Manifestdatei an. Finden Sie unter [manuell Packen von einer Desktopanwendung](desktop-to-uwp-manual-conversion.md) Anweisungen. Die App-ID für die Anwendung ist im Abschnitt *YOUR_PRAID_HERE* angegeben.

    - __Die Anwendung fügt einen Sprung Liste Shell-Link, der eine ausführbare Datei in Ihrem Paket verweist__. Sie können ausführbare Dateien in Ihrem Paket nicht direkt aus einer Sprungliste starten (mit Ausnahme des absoluten Pfads einer eigenen EXE-Datei einer App). Registrieren Sie stattdessen einen app-Ausführung-Alias (der können die App-Pakete desktop-Anwendung über ein Schlüsselwort gestartet wird, als wäre er auf dem Pfad), und legen Sie den Zielpfad für den Link stattdessen an den Alias. Weitere Informationen darüber, wie Sie die Erweiterung AppExecutionAlias verwenden, finden Sie unter [integrieren Sie Ihre desktop-Anwendung mit Windows 10](desktop-to-uwp-extensions.md). Hinweis: Wenn Ressourcen des Links in der Sprungliste der ursprünglichen EXE-Datei entsprechen müssen, müssen Sie Ressourcen wie etwa das Symbol mithilfe von [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) und den Anzeigenamen mit „PKEY_Title“ festlegen (wie bei anderen benutzerdefinierten Einträgen).

    - __Die Anwendung fügt einen Sprung Listeneinträge, die auf Ressourcen in der app-Paket von absolute Pfade verweist__. Der Installationspfad einer Anwendung möglicherweise ändern, wenn Pakete, die aktualisiert werden, ändern den Speicherort der Ressourcen (z. B. Symbole, Dokumente, ausführbare Datei und So weiter). Wenn Sprung Listeneinträge auf solche Objekte absolute Pfade verweisen, die Anwendung sollte die Sprungliste in regelmäßigen Abständen (z. B. Starten Sie auf die Anwendung) aktualisieren um sicherzustellen, dass Pfade ordnungsgemäß aufgelöst werden. Sie können stattdessen auch die UWP-[**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx)-APIs verwenden. Diese APIs ermöglichen den Verweis auf Zeichenfolgen- und Bildressourcen mithilfe des relativen ms-resource-URI-Paketschemas (das zudem Sprache, DPI und hohen Kontrast berücksichtigt).

+ __Die Anwendung gestartet wird, ein Hilfsprogramm zum Durchführen von Aufgaben__. Vermeiden Sie das Starten von Befehlshilfsprogrammen wie PowerShell und Cmd.exe. Wenn Benutzer Ihre Anwendung auf einem System, die die Windows 10 S ausgeführt wird installieren, klicken Sie dann Ihre Anwendung sie überhaupt gestartet nicht. Dies kann Ihrer Anwendung aus der Übermittlung an den Microsoft Store blockiert werden, da alle für den Microsoft Store eingereichte apps mit Windows 10 s kompatibel sein müssen

Das Starten eines Hilfsprogramms kann oft eine bequeme Methode für das Abrufen von Informationen aus dem Betriebssystem, Zugreifen auf die Registrierung oder das Zugreifen auf Systemfunktionen bereitstellen. Sie können jedoch stattdessen UWP-APIs verwenden, um diese Aufgaben auszuführen. Diese APIs sind leistungsfähiger, da sie nicht benötigen, eine separate ausführbare Datei ausgeführt, aber noch wichtiger ist, dass sie lassen Sie die Anwendung von außerhalb des Pakets zu erreichen. Die app Design bleibt konsistent mit der Isolation, Vertrauenswürdigkeit und Sicherheit, die im Lieferumfang von einer Anwendungs, die Sie verpackt haben, und die Anwendung verhält sich wie erwartet auf Systemen mit Windows 10 s

+ __Die Anwendung hostet,-add-ins, -Plug-ins oder Erweiterungen__.   In vielen Fällen werden Erweiterungen im COM-Stil wahrscheinlich weiterhin funktionieren, sofern die Erweiterung nicht verpackt wurde und sie als vertrauenswürdig installiert wurde. Das ist da die Installationsprogramme ihre voll vertrauenswürdigem-Funktionen verwenden können, ändern Sie die Registrierung und Erweiterungsdateien platzieren, wo die hostanwendung erwartet wird, um Sie zu finden.

   Allerdings, wenn diese Erweiterungen werden gepackt und als ein Windows-app-Paket installiert ist, funktionieren sie nicht, da jedes Paket (der hostanwendung und der Erweiterung) voneinander isoliert werden. Weitere Informationen wie Anwendungen sind isoliert aus dem System, finden Sie unter [hinter den Kulissen der Desktop-Brücke](desktop-to-uwp-behind-the-scenes.md).

 Alle Anwendungen und Erweiterungen, die Benutzer auf einem System mit Windows 10 S installieren, müssen als Windows-App-Pakete installiert werden. Wenn Sie beabsichtigen, Ihrer Erweiterungen Verpacken oder Sie Contributors Verpacken sie empfehlen möchten, berücksichtigen Sie also wie Sie die Kommunikation zwischen das Host-Anwendungspaket und alle Erweiterungspakete erleichtern können. Eine Möglichkeit wäre die Verwendung eines [App-Dienstes](../launch-resume/app-services.md).

+ __Ihre Anwendung generiert Code__. Ihre Anwendung kann Generieren von Code, der er verwendet im Arbeitsspeicher, aber generiert das Schreiben von Code auf den Datenträger vermeiden, da die Windows-App-Zertifizierungsprozess diesen Code vor der Übermittlung der app nicht überprüfen kann. Darüber hinaus nicht apps, die für das Schreiben von Code auf dem Datenträger ordnungsgemäß auf Systemen mit Windows 10 s ausgeführt Dies kann Ihrer Anwendung aus der Übermittlung an den Microsoft Store blockiert werden, da alle für den Microsoft Store eingereichte apps mit Windows 10 s kompatibel sein müssen

>[!IMPORTANT]
> Nachdem Sie Ihr Windows-app-Paket erstellt haben, testen Sie Ihre Anwendung aus, um sicherzustellen, dass sie ordnungsgemäß auf Systemen funktioniert, auf denen Windows 10 s ausgeführt Alle für den Microsoft Store eingereichte apps muss kompatibel mit Windows 10 S.-Apps, die nicht kompatibel sind, wird nicht im Store akzeptiert werden. Weitere Informationen finden Sie unter [Testen Ihrer Windows-App für Windows 10 S](desktop-to-uwp-test-windows-s.md).

## <a name="next-steps"></a>Nächste Schritte

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Erstellen Sie ein Windows-app-Paket für Ihre desktop-app**

Weitere Informationen erhalten Sie unter [Erstellen eines Windows-App-Pakets](desktop-to-uwp-root.md#convert).
