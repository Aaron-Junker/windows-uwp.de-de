---
Description: Erstellen Sie eine moderne Windows-App-Paket für Ihre vorhandenen Windows Forms-, WPF- oder Win32-Anwendung oder ein Spiel. Fügen Sie moderne Funktionen für Windows 10-Benutzer, und vereinfachen Sie die Bereitstellung und monetarisierung.
Search.Product: eADQiWindows 10XVcnh
title: Desktopanwendungen packen
ms.date: 09/05/2018
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 74373c24-f948-43bb-aa85-01e2e8e87162
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 70a9b6046e3b7be9ac84678ac21c0c9f89a4a7b2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627825"
---
# <a name="package-desktop-applications-desktop-bridge"></a>Paket-desktop-Anwendungen (Desktop-Brücke)

Nutzen Sie die vorhandene desktop-Anwendung, und fügen Sie moderne Funktionen für Windows 10-Benutzern. Sorgen Sie über die Verteilung über den Microsoft Store für eine größerer Reichweite in internationalen Märkten. Sie können Ihre Anwendung auf viel einfachere Weise verdienen, durch die Nutzung von Funktionen, die direkt in den Store integriert. Natürlich müssen Sie den Store nicht verwenden. Sie können auch Ihre vorhandenen Vertriebskanäle nutzen.

![Desktop-Brücke](images/desktop-to-uwp/desktop-bridge-4.png)

Wenn Sie ein Paket für die desktop-Anwendung erstellen, erhalten Ihre Anwendung eine Identität, und diese Identität, die desktop-Anwendung hat Zugriff auf Windows Universal Platform (UWP) APIs. Sie können diese für moderne und ansprechende Benutzeroberflächen wie z. B. Live-Kacheln und Benachrichtigungen verwenden.  Verwenden Sie einfache bedingten Kompilierung und Runtime überprüft, um die UWP-Code nur ausgeführt, wenn Ihre Anwendung unter Windows 10 ausgeführt wird.

Abgesehen von den Code, mit denen Sie sich Windows 10-benutzererfahrungen light, Ihrer Anwendung bleibt unverändert, und Sie können weiterhin auf Ihre vorhandenen Windows 7-, Windows Vista oder Windows XP-Benutzer, die Basisklasse bereit. Unter Windows 10, Ihre Anwendung weiterhin in voller Vertrauenswürdigkeit ausgeführt Benutzermodus, wie es heute tut.

>[!IMPORTANT]
>Die Fähigkeit zum Erstellen einer Windows-app-Paket für die desktop-Anwendung (auch bekannt als die Desktop-Brücke) wurde in Windows 10 Version 1607 eingeführt, und es kann nur in Projekten, die Windows 10 Anniversary Update (10.0; als Ziel verwendet werden Build 14393) oder eine neuere Version in Visual Studio.

> [!NOTE]
> Sehen Sie sich <a href="https://mva.microsoft.com/en-US/training-courses/developers-guide-to-the-desktop-bridge-17373?l=oZG0B1WhD_8406218965/">diese Reihe</a> von kurzen Videos an, die von der Microsoft Virtual Academy veröffentlicht wurden. Diese Videos durchlaufen Sie den gesamten Prozess für den Einsatz von Ihrem desktop-Anwendung auf die universelle Windows-Plattform (UWP).

## <a name="benefits"></a>Vorteile

Hier sind einige Gründe für die Erstellung eines Windows-App-Pakets für Ihre Desktop-Anwendung:

:heavy_check_mark: **Optimierte Bereitstellung**. Apps und Spiele, die die Brücke verwenden, profitieren von einer hervorragenden Bereitstellung. Diese Funktion gewährleistet, dass Benutzer zuverlässig eine Anwendung zu installieren und aktualisieren können. Wenn ein Benutzer die App deinstalliert, wird sie vollständig entfernt, ohne dass irgendwelche Spuren zurückbleiben. Dadurch wird der Zeitaufwand zum Erstellen des Setups und zum Bereitstellen von Updates verringert.

:heavy_check_mark: **Automatische Updates und Lizenzierung**. Ihre Anwendung kann an den Microsoft Store-integrierten lizenzierungs- und Funktionen für automatische Update teilnehmen. Automatische Updates stellen einen sehr zuverlässigen und effizienten Mechanismus dar, da nur die geänderten Teile von Dateien heruntergeladen werden.

:heavy_check_mark: **Höhere Reichweite und vereinfachte Monetarisierung**. Wenn Sie Ihre Apps über den Microsoft Store vertreiben, können Sie Millionen Windows 10-Benutzer erreichen, die Apps, Spiele und In-App-Käufe mit lokalen Zahlungsoptionen erwerben können.

:heavy_check_mark: **Hinzufügen von UWP-Features**.  Sie können in Ihrem eigenen Tempo Ihrem App-Paket UWP-Features hinzufügen, beispielsweise eine XAML-Benutzeroberfläche, Aktualisierungen von Live-Kacheln, UWP-Hintergrundaufgaben, App-Dienste und vieles mehr.

:heavy_check_mark: **Erweiterte Anwendungsmöglichkeiten auf allen Geräten**. Wenn Sie die Brücke verwenden, können Sie Ihren Code nach und nach auf die universelle Windows-Plattform migrieren, um jedes Windows 10-Gerät zu erreichen, einschließlich Smartphones, Xbox One und HoloLens.

Eine vollständige Liste der Vorteile finden Sie unter [Desktop-Brücke](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="prepare"></a>Vorbereiten

Bereiten Sie Ihre Anwendung zuerst im Artikel vor [Vorbereiten Ihrer desktop-app-Paket](desktop-to-uwp-prepare.md), und klicken Sie dann eines der Probleme, die auf Ihre Anwendung angewendet werden soll, bevor Sie ein Windows-app-Paket dafür erstellen. Sie können keine viele Änderungen an Ihrer Anwendung vornehmen, bevor Sie das Paket erstellen zu können. Es gibt jedoch einige Situationen, in denen möglicherweise die Anwendung zu optimieren, bevor Sie ein Paket zu erstellen.

<a id="convert" />

## <a name="package"></a>Paket

Hier sind einige Tools für die Erstellung eines Windows-App-Pakets für Ihre App.

### <a name="desktop-app-converter"></a>Desktop App Converter

Zwar taucht der Begriff „Konverter“ im Namen dieses Tools auf, es konvertiert aber Ihre App aber nicht wirklich. Ihre Anwendung bleibt unverändert. Das Tool generiert ein Windows-App-Paket für Sie. Es kann in Fällen sehr nützlich sein, Ihre Anwendung viele systemmodifizierungen durchführt, oder wenn Sie haben Ungewissheit im Hinblick auf Ihrer Installer Funktionsweise.

Der Desktop App Converter übersetzt die Aktionen des Installationsprogramms für die virtuelle Datei und die Registrierung-System, die die gepackte Version der Anwendung verwendet werden. Der Desktop App Converter erledigt für Sie auch einige zusätzliche Aufgaben. Nachfolgend sind einige aufgelistet.

:heavy_check_mark: Automatisches Registrieren Ihrer Preview-Handler, Miniaturansichthandler, Eigenschaftenhandler, Firewall-Regeln, URL-Kennzeichen.

:heavy_check_mark: Register Datei automatisch replikationsdatentyp-Zuordnungen, mit denen Benutzer Gruppierung von Dateien mithilfe der **Art** Spalte im Datei-Explorer.

:heavy_check_mark: Registriert Ihre öffentlichen COM-Server an.

:heavy_check_mark: Generiert ein Zertifikat, das Sie verwenden können, um die Anwendung auszuführen.

:heavy_check_mark: Ihre Anwendung für App-Pakete desktop-Anwendung und Microsoft Store-Anforderungen wird überprüft.

Ein weiterer hervorragender Grund für den Desktop App Converter ist, wenn Sie Ihre Anwendung beibehalten, indem Sie eine andere Entwicklungsumgebung als Visual Studio. Auch wenn Ihre Anwendung nicht über ein Installationsprogramm verfügt, können Sie den Desktop App Converter.

Finden Sie unter [Packen eine desktop-Anwendung über den Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md)

### <a name="visual-studio"></a>Visual Studio

Wenn Sie die Anwendung mithilfe von Visual Studio verwalten und Ihre Anwendung nicht über ein Installationsprogramm verfügt oder das Installationsprogramm nicht alle komplizierten Aufgaben ausführen kann, sollten Sie stattdessen Visual Studio verwenden.

Visual Studio erleichtert das Erstellen des Pakets ganz erheblich. Sie müssen nur Ihr Paketprojekt hinzufügen, auf das Desktopprojekt verweisen und F5 drücken, um Ihre App zu debuggen. Es sind keine manuellen Optimierungsmethoden mehr erforderlich. Das neue optimierte Design ist eine enorme Verbesserung über die Benutzeroberfläche, die in der vorherigen Version von Visual Studio verfügbar war. Hier sind einige andere Aufgaben, die es für Sie erledigen kann.

:heavy_check_mark: Generieren Sie automatisch visueller Objekte.

:heavy_check_mark: Nehmen Sie Änderungen in Ihr Manifest mit einem visuellen Designer an.

:heavy_check_mark: Generieren Sie das Paket mithilfe ein Assistenten.

:heavy_check_mark: Ganz einfach über das Zuweisen einer Identität für Ihre Anwendung aus einem Namen, den Sie bereits in reserviert haben [Partner Center](https://partner.microsoft.com/dashboard).

Finden Sie unter [Packen eine desktop-Anwendung mit Visual Studio](desktop-to-uwp-packaging-dot-net.md)

### <a name="third-party-installer"></a>Installationsprogramme von Drittanbietern.

 Mehrere beliebte Produkte von Drittanbietern und Installationsprogramme unterstützen jetzt die Möglichkeit zum Packen einer desktop-Anwendung. Sie können zum Generieren von MSI-Installationsprogrammen oder verpackten App-Paketen mit nur wenigen Klicks verwenden. Obwohl wir keine Dokumentation zur Verwendung dieser Tools bereitstellen, finden Sie auf unseren Websites zusätzliche Informationen.

#### <a name="advanced-installer"></a>Erweiterter Installer

Caphyon bietet ein kostenloses, GUI-basiertes Desktop-App-Verpackungstool an, das Ihnen hilft, ein Windows-App-Paket für Ihre Anwendung mit nur wenigen Klicks zu erstellen. Sie können ein der Installationsprogramme verwenden. auch solche, die im unbeaufsichtigten Modus ausgeführt, und führt eine Überprüfung überprüfen, um festzustellen, ob die Anwendung für die paketerstellung geeignet ist.
Der Desktop App Converter kann auch mit Hyper-V und [VMware](https://www.vmware.com/) integriert werden. Dies bedeutet, dass Sie Ihren eigenen virtuellen Computern verwenden können, ohne ein entsprechendes [Docker](https://docs.docker.com/)-Bild herunterladen zu müssen, das über 3 GB groß sein kann.

<img width="20%" src="images/desktop-to-uwp/Advanced_Installer_Vertical.png">

Sie können den [Erweiterten Installer](https://www.advancedinstaller.com/) verwenden, um eine MSI-Datei und [Windows-App-Pakete](https://www.advancedinstaller.com/uwp-app-package.html) von vorhandenen Projekten zu generieren. Sie können den erweiterten Installer auch verwenden, um Windows-App-Pakete zu importieren, die Sie mithilfe des Microsoft Desktop App Converters generieren. Nach dem Importieren können Sie sie mithilfe der visuellen Tools verwalten, die speziell für UWP-Apps entwickelt wurden.

Der erweiterte Installer bietet auch eine Erweiterung für Visual Studio 2017 und 2015, die zum [erstellen und debuggen von Desktop-Brücke-Apps](https://www.advancedinstaller.com/debug-desktop-bridge-apps.html) verwendet werden können.

In diesem [Video](https://www.youtube.com/watch?v=cmLKgn04Vfg&feature=youtu.be) finden Sie einen kurzen Überblick.

> [!TIP]
> Sehen Sie sich daher die zuletzt veröffentlichte [Advanced Installer Express Edition](https://www.advancedinstaller.com/express-edition.html) an.

#### <a name="cloudhouse-compatibility-containers"></a>Cloudhouse Compatibility-Container

Unternehmenskunden, die über Branchenanwendungen verfügen, die mit Windows 10 und 10 S nicht kompatibel sind, können mit den Compatibility-Containern von Cloudhouse Windows XP- und 7-Apps auf Windows 10 ausführen und dann konvertieren, damit diese auf der universellen Windows-Plattform (UWP) über den Microsoft Store für Unternehmen oder Microsoft InTune bereitgestellt werden, ohne den Quellcode zu ändern. Registrieren Sie sich für eine [kostenlose Testversion](https://www.cloudhouse.com/free-trial).

<img width="20%" src="images/desktop-to-uwp/cloudhouse-container-logo.png">

Cloudhouse bietet eine automatische Packager für Verpackung LOB-Anwendungen in [Kompatibilität Container](https://docs.cloudhouse.com/37613-overview/266723-compatibility-containers-for-applications) unter den Betriebssystemen, auf denen die apps läuft heute (z. B.: Windows XP), und klicken Sie dann [Vorbereiten für die Konvertierung](https://docs.cloudhouse.com/37613-overview/266725-compatibility-containers-for-desktop-bridge?from_search=17883905) zu UWP. Der Container wird anschließend in das neue Windows-App-Paketformat mithilfe des Desktop App Converter-Tools von Microsoft konvertiert.

Der Auto Packager verwendet Installationsdaten/Datenerfassungen und Laufzeitanalysen, um einen Container für die Anwendung zu erstellen. Dieser enthält die Anwendungsdateien, Registrierungsdaten, Laufzeiten, Abhängigkeiten sowie ein notwendiges Umleitungsmodul, mit dem die Anwendung unter Windows 10 ausgeführt werden kann. Der Container bietet Isolation für die Anwendung und die Laufzeiten, damit sie nicht die anderen Anwendungen, die auf dem Gerät des Benutzers ausgeführt werden, beeinflussen oder mit diesen in Konflikt geraten.

Erfahren Sie in unserem [Versionsblog](https://www.cloudhouse.com/resources/release-solution-to-get-any-line-of-business-app-to-uwp) mehr darüber, wie Sie über den Microsoft Store für Unternehmen Branchenanwendungen bereitstellen können.

#### <a name="firegiant"></a>FireGiant

Die [FireGiant MSIX Erweiterung](https://www.firegiant.com/products/wix-expansion-pack/msix) können Sie die Windows-app-Pakete und die MSI-Pakete gleichzeitig aus dem gleichen WiX-Quellcode erstellen. Jedes Mal, wenn Sie erstellt haben, können Sie Windows 10 mit einer Windows-app-Paket und früheren Versionen von Windows mit MSI abzielen.

<img width="20%" src="images/desktop-to-uwp/FG3rdPartyLogo.png">

Die Erweiterung FireGiant MSIX werden statische Analyse und intelligente Emulation des Ihre WiX-Projekte verwendet, um Windows-app-Pakete der Mehraufwand Datenträger Speicherplatz und Runtime für Container oder virtuellen Computern zu erstellen.

Da die Erweiterung FireGiant MSIX Installationsprogramm konvertiert und deswegen nicht ausführen, können Sie Ihre WiX-Installationsprogramms verwalten, ohne wiederholt in Windows-app-Pakete zu konvertieren. Alle Benutzer der verschiedenen Versionen von Windows erhalten die neuesten Verbesserungen und brauchen sich darum zu kümmern, ob MSI- und Windows-App-Pakete identisch sind.

Finden Sie in diesem [video](https://www.youtube.com/watch?v=AFBpdBiAYQE) und sehen Sie, wie in ein paar Codezeilen FireGiant CEO Rob Mensching eine Appx (Windows-app-Paket)-Version des Tools beliebter Open-Source-7-Zip-Komprimierung erstellt, und klicken Sie dann, wie er sowohl Windows-Anwendung verbessert und MSI-Pakete mit den Änderungen in der gleichen WiX-Quellcode.

#### <a name="installaware"></a>InstallAware

Installieren Sie**Aware** mit einer [Erfolgsbilanz](https://www.installaware.com/press-room.htm), die sofort die Innovationen von Microsoft, Builds [Windows-App-Pakete (Desktop-Brücke)](https://www.installaware.com/appx-builder.htm), App-V (Application Virtualization), MSI (Windows Installer), und EXE-Datei (systemeigener Code) Pakete aus einer einzigen Quelle unterstützt.

<img width="20%" src="images/desktop-to-uwp/installaware.png">

Install**Aware** bietet kostenlose Install**Aware**-Erweiterungen für Visual Studio 2012-2017. Sie Können sie verwenden, um Windows-App-Pakete mit einem einzigen Klick in die [Visual Studio-Symbolleiste](https://www.installaware.com/visual-studio-installer-2015.htm) zu erstellen.

Sie können außerdem mit Package**Aware** (Snapshot-freie Setup-Erfassung) oder dem Database Import Wizard (für alle MSI-Installationsprogramme und MSM-Merge-Module) jedes Setup importieren, auch dann, wenn Sie nicht den Quellcode für das Setup haben. Sie können [GUI-Tools](https://www.installaware.com/scripting-two-way-integrated-ide.htm) zur visuellen oder skriptgesteuerten Pflege und Erweiterung Ihrer Importe verwenden.

[Erweiterte APPX-Erstellungsoptionen](https://www.installaware.com/mhtml5/desktop/appx.htm) ermöglichen die Unterstützung von Microsoft Store-Beiträgen oder die Erstellung von signierten Windows-App-Paket-Binärdateien für die Verteilung per Querladen. Sie können sogar **WSA**-Installer-Pakete (Windows Server-Anwendungen) für Bereitstellungen auf **Nano Server** erstellen – und zwar über einen einzigen Quellcode und mit vollständiger Unterstützung für die [Befehlszeile Automatisierung](https://www.installaware.com/scripting-automation-interface.htm) (zusätzlich zu einer GUI).

Install**Aware** bietet außerdem eine [Open-Source-Version](https://www.installaware.com/gnu.asp) einer **APPX-Builder-Bibliothek**, sowie ein Beispiel-Befehlszeile-Applet, unter der GNU Affero GPL-Lizenz. Diese sind für die Verwendung mit Open-Source-Plattformen wie WiX entwickelt worden.

#### <a name="installshield"></a>InstallShield

InstallShield bietet eine einzige Lösung zur Entwicklung von MSI- und EXE-Installationsprogrammen, Erstellung von UWP-Paketen und WSA-Paketen und zur Virtualisierung von Anwendungen mit nur minimalem Aufwand für das Skripting, Erstellen von Code und Überarbeiten der Anwendung.

<img width="20%" src="images/desktop-to-uwp/InstallShield-logo.jpg">

Scannen Sie Ihr InstallShield-Projekt in wenigen Sekunden und sparen Sie sich stundenlanges Nachforschen. Erkennen Sie automatisch potenzielle Kompatibilitätsprobleme zwischen der Anwendung und den UWP- und WSA-Paketen.

Bereiten Sie für den Microsoft Store vor und vereinfachen Sie die Softwareinstallation unter Windows 10 durch die Erstellung von UWP-App-Paketen aus Ihren vorhandenen InstallShield-Projekten. Erstellen von Windows-Installer- und UWP-App-Pakete für alle gewünschten Bereitstellungsszenarien Ihrer Kunden. Unterstützung von Nano Server- und Windows Server 2016-Bereitstellungen durch die Erstellung von WSA-Paketen aus Ihren vorhandenen InstallShield-Projekten.

Entwickeln Sie Ihre Installation in Modulen für eine einfachere Bereitstellung und Wartung, und fügen Sie anschließend die Komponenten und die Abhängigkeiten zum Erstellungszeitpunkt in einem einzigen UWP-App-Paket für den Microsoft Store hinzu. Für die direkte Bereitstellung außerhalb des Stores bündeln Sie Ihre UWP-App-Pakete und anderen Abhängigkeiten zusammen mit einem Installationsprogramm oder einem erweiterten UI-Installer.

Erfahren Sie mehr in diesem [E-Book](https://na01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fresources.flexerasoftware.com%2Fweb%2Fpdf%2FeBook-IS-Your-Fast-Track-to-Profit.pdf&data=02%7C01%7Cnormesta%40microsoft.com%7C86b9a00bc8e345c2ac6208d4ba464802%7C72f988bf86f141af91ab2d7cd011db47%7C1%7C1%7C636338258409706554&sdata=IAYNp9nFc8B5ayxwrs%2FQTWowUmOda6p%2Fn%2BjdHea257M%3D&reserved=0).

#### <a name="pace-suite"></a>PACE Suite

[PACE Suite](https://pacesuite.com/) ist ein Tool zum Verpacken von Anwendung, das Sie verwenden können, um Ihre Desktop-Apps zur universellen Windows-Plattform zu bringen.

<img width="20%" src="images/desktop-to-uwp/PACE.png">

Mit PACE Suite müssen Sie keine speziellen Paketumgebung vorbereiten oder zusätzliche Windows SDK-Komponenten installieren. PACE Suite kann Windows-App-Pakete in Ihrer normalen Umgebung unter Windows 10 oder Windows Server 2016 unabhängig voneinander erstellen. Sehen Sie sich das [dargestellte Beispiel](https://pacesuite.com/convert-exe-to-appx/) an und erfahren Sie, wie PACE Suite das erneute Verpacken des Installationsprogramms auf einem Windows-App-Paket durchführt.

Abgesehen vom Erstellen von Windows-App-Paketen können Sie ebenfalls PACE Suite verwenden, um Windows Installer-Pakete (MSI), Patches (MSP), Transformationen (MST) und App-V-Pakete zu erstellen. Für MSI-Authoring hilft PACE Suite bei der Verwaltung von Aktualisierungen, Berechtigungseinstellungen, benutzerdefinierten Aktionen, Skripts und anderem. Sie können auch die Anwendungen direkt auf den System Center Configuration Manager veröffentlichen.

Informationen zur Überprüfung aller Anwendungspaketfunktionen finden Sie unter [PACE Suite-Features](https://pacesuite.com/features/).

#### <a name="rad-studio"></a>RAD Studio

Mehr unter [RAD Studio by Embarcadero](https://www.embarcadero.com/products/rad-studio/windows-10-store-desktop-bridge)

#### <a name="raypack-studio"></a>RayPack Studio

Raynet des-Lösung verpacken [RayPack Studio](https://raynet.de/Raynet-Products/RayPackStudio), unterstützt die Erstellung von Paketen für desktop-Anwendungen als eine von mehreren möglichen Ergebnisse der Konvertierung effizient und leicht zu konfigurieren und das Neuverpacken von Framework.

<img width="20%" src="images/desktop-to-uwp/RaynetLogo_v3.png">

Vorhandene virtuelle Umgebungen (VMware Workstation, Hyper-V) können verwendet werden, um automatisierte/Massenkonvertierungsaufgaben ohne langwierige Umgebungseinrichtung durchzuführen. Eine Komponente des Studios ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)) ist in der Lage, Prüfungs- und Kompatibilitätstests vor der Konvertierung durchzuführen, um sicherzustellen, dass die Software zur Konvertierung geeignet ist. Darüber hinaus können Benutzer jetzt umfassende Kollisions- und Kompatibilitätschecks mit verschiedenen Windows 10-Editionen, einschließlich Anniversary und Creators Update, durchführen.

Neben der Erstellung von Softwarepaketen für Windows 10 im APPX/UWP-Format kann RayPack Studio auch zum Erstellen von klassischen Windows Installer (MSI)-Paketen, Patches (MSP), Transformationen (MST) und App-V-Paketen verwendet werden. Diese Lösung enthält darüber hinaus eine Reihe von Softwareprodukten und Komponenten für die Erstellung professioneller Softwarepakete in Unternehmen. Neben Softwareverpackung und -virtualisierung berücksichtigt RayPack Studio alle weiteren verpackungsrelevanten Aufgaben: Konflikt- und Kompatibilitätsprüfungen von Softwareanwendungen und -paketen ([RayQC Advanced](https://raynet.de/Raynet-Products/RayQCad)), Software-Evaluierung ([RayEval](https://raynet.de/Raynet-Products/RayEval)) und Qualitätssicherung ([RayQC](https://raynet.de/Raynet-Products/RayQC)).

Mithilfe von [RayFlow](https://raynet.de/Raynet-Products/RayFlow), dem Unternehmens-Workflowsystem von Raynet, können Benutzer über den gesamten Unternehmensanwendungs-Lebenszyklus hinweg effizient an der Software arbeiten, von der Paketbestellung über die Evaluierung, Analyse, Verpackung, Qualitätssicherung und Benutzerakzeptanztests bis hin zur Bereitstellung. Alle Pakete und Formate können direkt in SCCM oder anderen Lösungen gespeichert und bereitgestellt werden. Der gesamte Anwendungslebenszyklusprozess wird von RayFlow nachverfolgt und verwaltet. Darüber hinaus können beliebige Bestellsysteme, wie z. B. ServiceNow, integriert werden. Mit seinen Tools für Dienstanbieter beliefert Raynet Softwareverpackungsfactorys weltweit.

Überzeugen Sie sich selbst und holen Sie sich die [kostenlose Testversion](https://raynet.de/contact?init=license) von Raynet RayPack Studio und RayFlow. Weitere Informationen finden Sie unter [www.raynet.de](https://raynet.de/home).

**Verwandte Links**:

* Raynet: [https://raynet.de/home](https://raynet.de/home)
* RayPack Studio: [https://raynet.de/Raynet-Products/RayPackStudio](https://raynet.de/Raynet-Products/RayPackStudio)
* RayFlow: [https://raynet.de/Raynet-Products/RayFlow](https://raynet.de/Raynet-Products/RayFlow)
* RayEval: [https://raynet.de/Raynet-Products/RayEval](https://raynet.de/Raynet-Products/RayEval)
* RayQC: [https://raynet.de/Raynet-Products/RayQC](https://raynet.de/Raynet-Products/RayQC)
* RayQC Advanced: [https://raynet.de/Raynet-Products/RayQCad](https://raynet.de/Raynet-Products/RayQCad)
* Kostenlose Testversion: [https://raynet.de/contact?init=license](https://raynet.de/contact?init=license)

### <a name="manual-packaging"></a>Manuelles Verpacken

Eine letzte Möglichkeit ist können Sie Ihre Anwendung konvertieren, ohne Verwendung dieser Tools. Wenn Sie eine präzise Steuerung der Konvertierung bevorzugen, können Sie eine Manifestdatei erstellen und anschließend das **MakeAppx.exe**-Tool ausführen, um das Windows-App-Paket zu erstellen.

Finden Sie unter [manuell Packen von einer Desktopanwendung](desktop-to-uwp-manual-conversion.md).

## <a name="integrate"></a>integrieren

Wenn Ihre Anwendung Sie in das System integrieren muss (z. B.: Einrichten der Firewall-Regeln) Dinge im Paketmanifest der Anwendung und die Beschreibung des Systems erledigt den Rest. Für die meisten dieser Aufgaben müssen Sie gar keinen Code schreiben. Mit ein wenig XML in das Manifest können Sie Aktionen wie das Starten eines Prozesses, wenn der Benutzer anmeldet, integrieren Ihre Anwendung in Datei-Explorer und fügen Ihrer Anwendung eine Liste der print-Ziele, die in anderen apps angezeigt werden.

Finden Sie unter [integrieren Sie Ihre App-Pakete Desktopanwendung mit Windows 10](desktop-to-uwp-extensions.md).

## <a name="enhance"></a>Verbessern

Nachdem Sie Ihre App verpackt haben, können Sie Features wie Live-Kacheln und Push-Benachrichtigungen nutzen. Einige dieser Funktionen kann die Engagement-Ebene der Anwendung erheblich verbessern und sie nur sehr wenig Zeit zum Hinzufügen von Kosten. Für einige Erweiterungen ist etwas mehr Code erforderlich.

Weitere Informationen finden Sie unter [Verbessern Sie Ihre Desktopanwendung für Windows 10](desktop-to-uwp-enhance.md).

## <a name="extend"></a>Erweitern

Einige Windows 10-Umgebungen (z. B. eine touch-fähige UI-Seite) müssen sich in einem Modern-App-Container befinden. In der Regel sollten Sie zuerst ermitteln, ob Sie Ihre Umgebung über die [Erweiterung](desktop-to-uwp-enhance.md) Ihre vorhandene Desktopanwendung mit UWP-APIs hinzufügen können. Wenn Sie eine UWP-Komponente zu verwenden, um die Funktionalität erzielen, müssen, können Sie Ihrer Projektmappe ein UWP-Projekt hinzufügen und app-Dienste für die Kommunikation zwischen Ihrer desktop-Anwendung und die UWP-Komponente verwenden.

Weitere Informationen finden Sie unter [Erweitern Ihrer Desktopanwendung mit modernen UWP-Komponenten](desktop-to-uwp-extend.md).

## <a name="migrate"></a>Migrieren

Obwohl kein Tool existiert, das eine Desktopanwendung in eine UWP-App konvertieren kann, können Sie einen Großteil Ihres vorhandenen Codes wiederverwenden und so die Kosten der Programmierung verringern. Sie können dies tun, indem Sie so viel Geschäftslogik wie möglich in die .NET Standard 2.0-Bibliotheken verschieben.

.NET Standard 2.0 umfasst eine massive Erhöhung der Anzahl der .NET-APIs sowie der Kompatibilitätsshim für Ihre bevorzugten NuGet-Pakete und Bibliotheken von Drittanbietern.

Sie können den Code in .NET Standardbibliotheken migrieren und so eine universelle Windows-Plattform (UWP)-App erstellen, um alle Windows 10-Geräten zu erreichen.

Weitere Informationen finden Sie unter [Teilen von Code zwischen einer Desktop-App und einer UWP-App](desktop-to-uwp-migrate.md)


## <a name="test"></a>Test

Zum Testen der Anwendung in einer realistischen Umgebung, wie Sie für die Verteilung vorbereiten, empfiehlt es sich zum Signieren Ihrer Anwendung, und installieren Sie es. Mehr unter [Tests für Ihre App](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-debug#test-your-app).

>[!IMPORTANT]
> Wenn Sie Ihre Anwendung in der Microsoft Store veröffentlichen möchten, stellen Sie sicher, dass Ihre Anwendung auf Geräten, auf denen Windows 10 im S-Modus ausgeführt, ordnungsgemäß ausgeführt wird. Dies ist eine Store-Anforderung. Weitere Informationen finden Sie unter [Testen Ihrer Windows-App für Windows 10 im S Modus](desktop-to-uwp-test-windows-s.md).

## <a name="validate"></a>Überprüfen

Um Ihrer Anwendung wird auf dem Microsoft Store veröffentlichten oder erfahren, wie die beste Chance geben [zertifizierten Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666), überprüfen und lokal testen, bevor Sie sie zur Zertifizierung einreichen.

Wenn Sie die DAC zum Verpacken der app verwenden, können Sie die neue ``-Verify`` Flag, um Ihr Paket für die App-Pakete desktop-Anwendung und die Store-Anforderungen überprüfen. Weitere Informationen finden Sie unter [Verpacken einer App, Signieren der App, und Vorbereiten der App für die Übermittlung an den Store](desktop-to-uwp-run-desktop-app-converter.md#optional-parameters).

Wenn Sie Visual Studio verwenden, können Sie überprüfen, dass Ihre Anwendung aus der **App-Pakete erstellen** Assistenten. Weitere Informationen finden Sie unter [Erstellen einer App-Paketuploaddatei](../packaging/packaging-uwp-apps.md#create-an-app-package-upload-file).

Um das Tool manuell auszuführen, lesen Sie sich [Zertifizierungskit für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md) durch.

Informationen zur Liste der Tests, die die Windows-Apps-Zertifizierung für die Überprüfung Ihrer App verwendet, finden Sie unter [App-Tests für Windows-Desktop-Brücke](../debug-test-perf/windows-desktop-bridge-app-tests.md).

## <a name="distribute"></a>Vertreiben

Sie können Ihre Anwendung verteilen, indem Sie sie dem Microsoft Store zu veröffentlichen oder durch querladen auf anderen Systemen.

Finden Sie unter [eine desktop-app-Pakete verteilen](desktop-to-uwp-distribute.md).

## <a name="support-and-feedback"></a>Support und Feedback

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

## <a name="in-this-section"></a>Inhalt dieses Abschnitts

| Thema | Beschreibung |
|-------|-------------|
| [Vorbereiten Sie eine app-Paket](desktop-to-uwp-prepare.md) | Enthält eine Liste von Elementen, die vor dem Verpacken Ihrer Desktop-App überprüft werden sollten. |
| [Verpacken einer app über den Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md) | Beinhaltet Informationen zum Ausführen von Desktop App Converter. |
| [Packen Sie manuell eine desktop-Anwendung](desktop-to-uwp-manual-conversion.md) | Enthält Informationen zum manuellen Erstellen eines App-Pakets und -Manifests. |
| [Packen einer desktop-Anwendung mithilfe von Visual Studio](desktop-to-uwp-packaging-dot-net.md)| Erfahren Sie, wie Sie die desktop-Anwendung mit Visual Studio-Paket. |
| [Integrieren Sie Ihre desktop-Anwendung mit Windows 10](desktop-to-uwp-extensions.md) | Integrieren Sie Ihre Anwendung mit Windows 10 mithilfe von Aufgaben in der Paketmanifestdatei, der Ihr Projekt für das Verpacken, die beschreibt. |
| [Erweitern Sie Ihre desktop-Anwendung für Windows 10](desktop-to-uwp-enhance.md)| Verwenden Sie UWP-APIs, um moderne Umgebungen für Windows 10-Benutzer hinzuzufügen. |
| [Eine gepackte desktop-Anwendung zur UWP-APIs](desktop-to-uwp-supported-api.md) | Informieren Sie UWP-APIs für Ihre App-Pakete Desktopanwendung verwenden. |
| [Erweitern Sie die desktop-Anwendung mit modernen UWP-Komponenten](desktop-to-uwp-extend.md)| Fügen Sie erweiterte Funktionen hinzu, die in einem UWP-App-Container ausgeführt werden müssen. Mithilfe von app-Dienste, eine Verbindung mit dem UWP-Prozess die desktop-Anwendung.|
| [Ausführen, Debuggen und Testen einer App-Pakete desktop-Anwendung](desktop-to-uwp-debug.md) | Erläutert die Optionen für das Debuggen der verpacken App. |
| [Verteilen einer App-Pakete desktop-Anwendung ](desktop-to-uwp-distribute.md) | Sehen Sie, wie Sie die konvertierte Anwendung für Benutzer verteilen können.  |
| [Bekannte Issues(desktop-to-uwp-known-issues.md) | Führt bekannte Probleme mit der paketerstellung von desktopanwendungen. |
