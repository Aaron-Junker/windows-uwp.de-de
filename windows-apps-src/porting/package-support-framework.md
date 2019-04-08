---
Description: Beheben von Problemen, die die desktop-Anwendung aus der Ausführung in einem Container MSIX verhindern
Search.Product: eADQiWindows 10XVcnh
title: Beheben von Problemen, die die desktop-Anwendung aus der Ausführung in einem Container MSIX verhindern
ms.date: 07/02/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 80f9c8bad9445bd9cfef9b09c00f99929fda37aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590725"
---
# <a name="apply-runtime-fixes-to-an-msix-package-by-using-the-package-support-framework"></a>Anwenden von Korrekturen der Common Language Runtime zu einem Paket MSIX mithilfe des Paket-Support-Framework

Das Paket-Support-Framework ist ein open-Source-Kit, mit dem Sie die anwenden Korrekturen an der vorhandenen win32-Anwendung, wenn Sie keinen Zugriff auf den Quellcode haben, damit es in einem Container MSIX ausgeführt werden kann. Das Paket-Support-Framework hilft Ihrer Anwendung, die die empfohlenen Vorgehensweisen der modernen Laufzeitumgebung.

Weitere Informationen finden Sie unter [Unterstützung Paketframework](https://docs.microsoft.com/windows/msix/package-support-framework-overview).

Dieses Handbuch unterstützt Sie beim Anwendungskompatibilitätsprobleme zu identifizieren, und zum Suchen, anwenden und zum Erweitern der Laufzeit Korrekturen, die ihnen zu begegnen.

<a id="identify" />

## <a name="identify-packaged-application-compatibility-issues"></a>Identifizieren von Anwendungskompatibilitätsproblemen für App-Pakete

Erstellen Sie zunächst ein Paket für Ihre Anwendung ein. Installieren Sie es anschließend, führen Sie es, und beobachten Sie das Verhalten. Sie erhalten möglicherweise Fehlermeldungen, die Sie ein Kompatibilitätsproblem bestimmen können. Sie können auch [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) um Probleme zu identifizieren.  Häufig auftretende Probleme im Zusammenhang mit Anwendung-Annahmen bezüglich der Arbeit Verzeichnis und das Programm Berechtigungen.

### <a name="using-process-monitor-to-identify-an-issue"></a>Monitor "Prozess" verwenden, um ein Problem zu identifizieren.

[Monitor "Prozess"](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) ist ein leistungsfähiges Dienstprogramm ermöglicht Ihnen, eine app Datei und Vorgänge in der geräteidentitätsregistrierung und ihre Ergebnisse.  Dies hilft Ihnen, Probleme mit der Anwendungskompatibilität zu verstehen.  Nach dem Öffnen von Process Monitor, Hinzufügen eines Filters (Filter > Filter...), nur die Ereignisse aus der ausführbaren Datei der Anwendung enthält.

![ProcMon App-Filter](images/desktop-to-uwp/procmon_app_filter.png)

Es wird eine Liste der Ereignisse angezeigt. Für viele dieser Fälle wird das Wort **Erfolg** erscheint in der **Ergebnis** Spalte.

![ProcMon-Ereignisse](images/desktop-to-uwp/procmon_events.png)

Optional können Sie Ereignisse, um nur der Fehlschläge zeigt nur filtern.

![Ausschließen von ProcMon-Erfolg](images/desktop-to-uwp/procmon_exclude_success.png)

Wenn Sie einen Dateisystem-Access-Fehler vermuten, suchen Sie nach fehlerhaften Ereignisse, die unter der "System32" / SysWOW64 oder der Dateipfad. Filter können auch hier zu helfen. Starten Sie am unteren Rand der Liste aus, und führen Sie einen Bildlauf nach oben. Fehler, die am unteren Rand der Liste angezeigt werden, sind die zuletzt aufgetreten. Achten Sie die meisten auf Fehler, die Zeichenfolgen wie "Zugriff verweigert" enthalten und "Pfad/Name nicht gefunden", und ignorieren Sie Dinge, die nicht verdächtige aussehen. Die [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/) gibt es zwei Probleme. Sie können diese Probleme in der Liste sehen, die in der folgenden Abbildung angezeigt wird.

![ProcMon "config.txt"](images/desktop-to-uwp/procmon_config_txt.png)

In das erste Problem, das in diesem Bild angezeigt wird, kann die Anwendung nicht aus der Datei "Config.txt" zu lesen, die im Pfad "C:\Windows\SysWOW64" befindet. Es ist unwahrscheinlich, dass die Anwendung versucht wird, direkt auf diesen Pfad verweisen. In den meisten Fällen versucht, mithilfe eines relativen Pfads aus dieser Datei zu lesen, und "System32/SysWOW64" wird standardmäßig im Verzeichnis der Anwendung arbeiten. Dies deutet darauf hin, dass die Anwendung erwartet die aktuellen Arbeitsverzeichnis an einer beliebigen Stelle im Paket festgelegt werden. Suchen in die AppX-Datei, sehen wir, dass die Datei im gleichen Verzeichnis wie die ausführbare Datei vorhanden ist.

![App "config.txt"](images/desktop-to-uwp/psfsampleapp_config_txt.png)

Das zweite Problem wird in der folgenden Abbildung angezeigt.

![ProcMon-Protokolldatei](images/desktop-to-uwp/procmon_logfile.png)

In dieser Ausgabe die Anwendung Fehler beim Schreiben eine log-Datei auf den entsprechenden Paketpfad. Dies würde wird empfohlen, ein Datei-Umleitung-Fixup hilfreich sein können.

<a id="find" />

## <a name="find-a-runtime-fix"></a>Suchen nach einer Lösung für die Common Language runtime

Die PSF enthält Runtime-Updates, die Sie jetzt, wie z. B. die Datei Umleitung Fixup verwenden können.

### <a name="file-redirection-fixup"></a>Datei Umleitung Fixup

Können Sie die [Datei Umleitung Fixup](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) umleiten versucht, schreiben oder Lesen von Daten in einem Verzeichnis, die über eine Anwendung nicht, die in einem MSIX-Container ausgeführt wird.

Beispielsweise wenn Ihre Anwendung in eine Protokolldatei, die im selben Verzeichnis wie Ihre Anwendungen, die ausführbare Datei ist schreibt, dann können Sie die [Datei Umleitung Fixup](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/fixups/FileRedirectionFixup) betreffende Protokolldatei in einem anderen Speicherort, z. B. den Datenspeicher des lokalen app zu erstellen.

### <a name="runtime-fixes-from-the-community"></a>Runtime-Fixes aus der community

Achten Sie darauf, überprüfen die Communitybeiträge zu unserer [GitHub](https://github.com/Microsoft/MSIX-PackageSupportFramework) Seite. Es ist möglich, dass andere Entwickler haben ein Problem mit Ihrer vergleichbar gelöst, und eine Lösung für die Common Language Runtime freigegeben haben.

## <a name="apply-a-runtime-fix"></a>Eine Korrektur der Laufzeit anwenden

Sie können eine vorhandene Common Language Runtime-Fehlerbehebung mit ein paar einfache Tools aus dem Windows SDK, und klicken Sie mit folgenden Schritten anwenden.

> [!div class="checklist"]
> * Erstellen Sie einen Paket-Layout-Ordner
> * Herunterladen von Dateien Paketframework-Unterstützung
> * Zu Ihrem Paket hinzufügen
> * Bearbeiten des Paketmanifests
> * Erstellen einer Konfigurationsdatei

Kehren Sie wir über die verschiedenen Aufgaben.

### <a name="create-the-package-layout-folder"></a>Erstellen Sie den Paket-Layout-Ordner

Wenn Sie bereits über eine .msix oder AppX-Datei verfügen, können Sie seinen Inhalt in einem Layoutordner entpacken, die als Stagingbereich für das Paket verwendet wird. Hierzu können Sie über eine Eingabeaufforderung MakeAppx-Tool basierend auf dem Installationspfad des SDK verwenden, handelt es sich, in dem Sie das Tool makeappx.exe auf Ihrem Windows 10-PC finden: X86: C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\bin\x86\makeappx.exe X64: C:\Programme\Microsoft Dateien (x86) \Windows Kits\10\bin\x64\makeappx.exe

```ps
makeappx unpack /p PSFSamplePackage_1.0.60.0_AnyCPU_Debug.msix /d PackageContents

```

Dadurch werden etwa Ihnen, die wie folgt aussieht.

![Paketlayout](images/desktop-to-uwp/package_contents.png)

Wenn Sie keine Datei .msix (oder AppX-Datei) für den Einstieg haben, können Sie die Paketordner und Dateien von Grund auf neu erstellen.

### <a name="get-the-package-support-framework-files"></a>Herunterladen von Dateien Paketframework-Unterstützung

Sie können das PSF Nuget-Paket mithilfe des eigenständigen Nuget-Befehlszeilentools oder über Visual Studio abrufen.

#### <a name="get-the-package-by-using-the-command-line-tool"></a>Rufen Sie das Paket mit dem Befehlszeilentool

Installieren des Nuget-Befehlszeilentools von diesem Speicherort: https://www.nuget.org/downloads. Klicken Sie dann über die Nuget-Befehlszeile, führen Sie diesen Befehl ein:

```ps
nuget install Microsoft.PackageSupportFramework
```

#### <a name="get-the-package-by-using-visual-studio"></a>Abrufen des Pakets mit Visual Studio

Klicken Sie in Visual Studio mit der rechten Maustaste in Ihrem lösungs- oder Projektknotens, und wählen Sie einen der Befehle aus Nuget-Pakete verwalten.  Suchen Sie nach **Microsoft.PackageSupportFramework** oder **PSF** auf das Paket auf Nuget.org zu finden. Anschließend installieren Sie es.

### <a name="add-the-package-support-framework-files-to-your-package"></a>Die Unterstützung von Paketframework-Dateien zu Ihrem Paket hinzufügen

Fügen Sie die erforderlichen 32-Bit- und 64-Bit-PSF-DLLs und ausführbaren Dateien auf das Paketverzeichnis hinzu. Orientieren Sie sich an der folgenden Tabelle. Sie sollten auch alle Fixes von Common Language Runtime enthalten, die Sie benötigen. In unserem Beispiel benötigen wir die Datei Umleitung Common Language Runtime-Lösung.

| Ausführbare Datei der Anwendung ist x64 | Ausführbare Datei der Anwendung ist x86 |
|-------------------------------|-----------|
| [PSFLauncher64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |  [PSFLauncher32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfLauncher/readme.md) |
| [PSFRuntime64.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) | [PSFRuntime32.dll](https://github.com/Microsoft/MSIX-PackageSupportFramework/tree/master/PsfRuntime/readme.md) |
| [PSFRunDll64.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) | [PSFRunDll32.exe](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/PsfRunDll/readme.md) |

Der Inhalt des Pakets sollte in etwa wie folgt aussehen.

![Binärdateien von Paketen](images/desktop-to-uwp/package_binaries.png)

### <a name="modify-the-package-manifest"></a>Bearbeiten des Paketmanifests

Öffnen Sie Ihr Paketmanifest in einem Text-Editor, und legen Sie dann die `Executable` Attribut der `Application` -Element auf den Namen der ausführbaren Datei PSF Startprogramm.  Wenn Sie die Architektur Ihrer Anwendung kennen, wählen Sie die entsprechende Version, PSFLauncher32.exe oder PSFLauncher64.exe.  Wenn dies nicht der Fall ist, PSFLauncher32.exe funktioniert in allen Fällen.  Hier sehen Sie ein Beispiel.

```xml
<Package ...>
  ...
  <Applications>
    <Application Id="PSFSample"
                 Executable="PSFLauncher32.exe"
                 EntryPoint="Windows.FullTrustApplication">
      ...
    </Application>
  </Applications>
</Package>
```

### <a name="create-a-configuration-file"></a>Erstellen einer Konfigurationsdatei

Erstellen Sie einen Dateinamen ``config.json``, und speichern Sie die Datei in den Stammordner des Pakets. Ändern Sie die deklarierte app-ID der Datei "config.JSON" ein, um auf die ausführbare Datei zu verweisen, die Sie soeben ersetzt. Verwenden die Informationen, die Sie von der Verwendung von Monitor "Prozess" erzielt, können Sie auch das Arbeitsverzeichnis als auch verwenden die Datei Umleitung Fixup zum Umleiten von Lese-/Schreibvorgänge auf .log-Dateien im Verzeichnis "PSFSampleApp" Relative Paket.

```json
{
    "applications": [
        {
            "id": "PSFSample",
            "executable": "PSFSampleApp/PSFSample.exe",
            "workingDirectory": "PSFSampleApp/"
        }
    ],
    "processes": [
        {
            "executable": "PSFSample",
            "fixups": [
                {
                    "dll": "FileRedirectionFixup.dll",
                    "config": {
                        "redirectedPaths": {
                            "packageRelative": [
                                {
                                    "base": "PSFSampleApp/",
                                    "patterns": [
                                        ".*\\.log"
                                    ]
                                }
                            ]
                        }
                    }
                }
            ]
        }
    ]
}
```

Es folgt eine Anleitung für das Schema "config.JSON":

| Array | key | Wert |
|-------|-----------|-------|
| applications | id |  Verwenden Sie den Wert der `Id` Attribut der `Application` -Element im Paketmanifest. |
| applications | ausführbare Datei | Die Paket-Relative Pfad der ausführbaren Datei, die Sie starten möchten. In den meisten Fällen können Sie diesen Wert aus der Paketmanifestdatei abrufen, bevor Sie sie ändern. Es ist der Wert des der `Executable` Attribut der `Application` Element. |
| applications | WorkingDirectory | (Optional) Ein Paket relativen Pfad als das Arbeitsverzeichnis der Anwendung verwenden, die gestartet wird. Wenn Sie diesen Wert nicht festlegen, um das Betriebssystem verwendet den `System32` Verzeichnis als Arbeitsverzeichnis der Anwendung. |
| Prozesse | ausführbare Datei | In den meisten Fällen werden diese den Namen des der `executable` oben mit dem Pfad und Dateierweiterungen entfernt konfiguriert. |
| Korrekturen | DLL | Paket relativen Pfad, den der Fixup,.msix/.appx geladen. |
| Korrekturen | Konfigurationsdatei | (Optional) Steuert das Verhalten der Fixup-Verteilerliste. Das genaue Format der dieser Wert variiert pro Fixup-von-Fixup wie jeder Korrektur dieses "Blob" interpretieren kann, wie er möchte. |

Die `applications`, `processes`, und `fixups` Schlüssel sind Arrays. Das bedeutet, dass Sie die Datei "config.json" verwenden können, um mehr als eine Anwendung, Prozess- und Fixup-DLL anzugeben.

### <a name="package-and-test-the-app"></a>Paket und Testen Sie die App

Als Nächstes erstellen Sie ein Paket.

```ps
makeappx pack /d PackageContents /p PSFSamplePackageFixup.msix
```

Klicken Sie dann signieren.

```ps
signtool sign /a /v /fd sha256 /f ExportedSigningCertificate.pfx PSFSamplePackageFixup.msix
```

Weitere Informationen finden Sie unter [Vorgehensweise: Erstellen Sie ein paketsignaturzertifikat](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate) und [Signieren ein Pakets mithilfe von "SignTool"](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/how-to-sign-a-package-using-signtool)

Installieren Sie das Paket mithilfe von PowerShell.

>[!NOTE]
> Denken Sie daran, zuerst das Paket zu deinstallieren.

```ps
powershell Add-AppPackage .\PSFSamplePackageFixup.msix
```

Führen Sie die Anwendung, und beobachten Sie das Verhalten mit Common Language Runtime-Fix angewendet.  Wiederholen Sie die Diagnose und die paketerstellung Schritte nach Bedarf.

### <a name="use-the-trace-fixup"></a>Verwenden Sie die Ablaufverfolgung Fixup

Alternatives Verfahren zum Diagnostizieren von Problemen der verpackten Anwendung-Kompatibilität werden der Fixup-Ablaufverfolgung verwenden. Diese DLL ist in der PSF enthalten und bietet eine detaillierte Diagnose Übersicht der app-Verhalten, ähnlich wie Process Monitor.  Es ist speziell auf Probleme mit der Anwendungskompatibilität anzuzeigen.  Verwenden, den der Fixup-Ablaufverfolgung, die DLL zum Paket hinzufügen. fügen das folgende Fragment auf Ihre "config.JSON", und klicken Sie dann Packen und installieren Sie die Anwendung.

```json
{
    "dll": "TraceFixup.dll",
    "config": {
        "traceLevels": {
            "filesystem": "allFailures"
        }
    }
}
```

Fehler, die betrachtet werden können, "erwartet" filtert der Fixup-Ablaufverfolgung, in der Standardeinstellung.  Beispielsweise können Anwendungen versuchen, bedingungslos eine Datei löschen, ohne zu überprüfen, um festzustellen, ob sie vorhanden ist, wird ignoriert. das Ergebnis. Dies hat die unglückliche Konsequenz, die einige unerwartete Fehler herausgefiltert abrufen können, damit im obigen Beispiel wir uns entscheiden um alle Fehler bei der Filesystem-Funktionen zu erhalten. Wir tun dies, da von vor, denen beim Lesen aus der Datei "config.txt" schlägt fehl bekannt mit "der Nachricht Datei wurde nicht gefunden". Dies ist ein Fehler, der häufig eingehalten und nicht in der Regel davon ausgegangen, dass nicht erwartet werden. In der Praxis ist es wahrscheinlich am besten beginnen Sie mit dem Filter nur auf unerwartete Fehler, und klicken Sie dann Fallback auf alle Fehler liegt ein Problem, das noch nicht identifiziert werden kann.

Standardmäßig ruft die Ausgabe der Ablaufverfolgung Fixup für den angefügten Debugger gesendet werden. In diesem Beispiel wir sind nicht auf einen Debugger anfügen, und stattdessen verwenden die [DebugView](https://docs.microsoft.com/en-us/sysinternals/downloads/debugview) Programm von SysInternals, um seine Ausgabe anzuzeigen. Nach dem Ausführen der app, sehen die gleichen Fehler wie zuvor, wir das würde uns auf die gleichen Fixes für die Common Language Runtime zeigen.

![TraceShim-Datei wurde nicht gefunden.](images/desktop-to-uwp/traceshim_filenotfound.png)

![TraceShim Zugriff verweigert](images/desktop-to-uwp/traceshim_accessdenied.png)

## <a name="debug-extend-or-create-a-runtime-fix"></a>Debuggen Sie, erweitern Sie oder erstellen Sie eine Lösung für die Common Language runtime

Sie können Visual Studio verwenden, zum Debuggen einer Lösung für die Common Language Runtime, erweitern eine Lösung für die Common Language Runtime oder von Grund auf neu erstellen. Sie müssen sich all dies erfolgreich ist.

> [!div class="checklist"]
> * Fügen Sie ein paketerstellungsprojekt
> * Projekt für die Common Language Runtime-Lösung hinzufügen
> * Fügen Sie ein Projekt, das das ausführbare PSF-Startprogramm wird gestartet.
> * Konfigurieren Sie das Packaging-Projekt

Wenn Sie fertig sind, wird Ihre Projektmappe etwa wie folgt aussehen.

![Abgeschlossene Lösung](images/desktop-to-uwp/runtime-fix-project-structure.png)

Sehen wir uns auf jedes Projekt in diesem Beispiel.

| Projekt | Zweck |
|-------|-----------|
| DesktopApplicationPackage | Dieses Projekt basiert auf der [paketerstellungsprojekt für Windows-Anwendung](desktop-to-uwp-packaging-dot-net.md) und das Paket MSIX ausgegeben. |
| Runtimefix | Dies ist ein C++ Dynamic-Linked-Bibliotheksprojekt, das eine oder mehrere Ersatzfunktionen enthält, die als die Common Language Runtime-Lösung dienen. |
| PSFLauncher | Dies ist eine leere C++-Projekt. Dieses Projekt ist ein Ort der Common Language Runtime verteilbare Dateien von Framework-Pakets Unterstützung gesammelt werden sollen. Es gibt eine ausführbare Datei aus. Diese ausführbare Datei wird als erstes, die ausgeführt wird, wenn Sie die Projektmappe zu starten. |
| WinFormsDesktopApplication | Dieses Projekt enthält den Quellcode einer Desktopanwendung. |

Um ein vollständiges Beispiel anzuzeigen, die alle von diesen Typen von Projekten enthält, finden Sie unter [PSFSample](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/samples/PSFSample/).

Betrachten Sie die Schritte zum Erstellen und Konfigurieren jedes dieser Projekte in der Projektmappe aus.

### <a name="create-a-package-solution"></a>Erstellen Sie eine Paket-Projektmappe

Wenn Sie eine Lösung für die desktop-Anwendung noch nicht, erstellen Sie ein neues **leere Projektmappe** in Visual Studio.

![Leere Projektmappe](images/desktop-to-uwp/blank-solution.png)

Sie sollten auch alle Anwendungsprojekte hinzuzufügen, was man.

### <a name="add-a-packaging-project"></a>Fügen Sie ein paketerstellungsprojekt

Wenn Sie noch keinem **Paketerstellungsprojekt für Windows-Anwendungen**erstellen, und fügen sie der Projektmappe.

![Paket-Projektvorlage](images/desktop-to-uwp/package-project-template.png)

Weitere Informationen zu paketerstellungsprojekt für Windows-Anwendung, finden Sie unter [Packen der Anwendung mit Visual Studio](desktop-to-uwp-packaging-dot-net.md).

In **Projektmappen-Explorer**mit der rechten Maustaste auf das Projekt zum Verpacken, wählen Sie **bearbeiten**, und fügen Sie diese dann an das Ende der Projektdatei:

```xml
<Target Name="PSFRemoveSourceProject" AfterTargets="ExpandProjectReferences" BeforeTargets="_ConvertItems">
<ItemGroup>
  <FilteredNonWapProjProjectOutput Include="@(_FilteredNonWapProjProjectOutput)">
  <SourceProject Condition="'%(_FilteredNonWapProjProjectOutput.SourceProject)'=='<your runtime fix project name goes here>'" />
  </FilteredNonWapProjProjectOutput>
  <_FilteredNonWapProjProjectOutput Remove="@(_FilteredNonWapProjProjectOutput)" />
  <_FilteredNonWapProjProjectOutput Include="@(FilteredNonWapProjProjectOutput)" />
</ItemGroup>
</Target>
```

### <a name="add-project-for-the-runtime-fix"></a>Projekt für die Common Language Runtime-Lösung hinzufügen

Fügen Sie ein C++ **Dynamic Link Library (DLL)** Projekt der Projektmappe.

![Korrektur der Laufzeitbibliothek](images/desktop-to-uwp/runtime-fix-library.png)

Mit der rechten Maustaste das, das Projekt, und wählen Sie dann **Eigenschaften**.

Finden Sie in den Eigenschaftenseiten die **Standard der Sprache C++** ein, und wählen Sie dann in der Dropdownliste neben dem Feld, das **ISO C ++ 17-Standard (/ Std: c ++ 17)** Option.

![ISO 17 Option](images/desktop-to-uwp/iso-option.png)

Mit der rechten Maustaste in des Projekts, und wählen Sie dann im Kontextmenü der **Nuget-Pakete verwalten** Option. Sicherstellen, dass die **Paketquelle** Option wird festgelegt, um **alle** oder **nuget.org**.

Klicken Sie auf dem Symbol "Einstellungen" neben dem Feld.

Suchen Sie nach der *PSF** Nuget-Paket, und installieren Sie es für dieses Projekt.

![NuGet-Paket](images/desktop-to-uwp/psf-package.png)

Wenn Sie Debuggen, oder eine vorhandene Common Language Runtime-Lösung erweitern möchten, fügen Sie die Runtime-Fix-Dateien, die Sie anhand der Anleitungen, die in beschriebenen erhalten die [Suchen nach einer Lösung für die Common Language Runtime](#find) Abschnitt dieses Handbuchs.

Wenn Sie eine neue Lösung erstellen möchten, hinzuzufügen Sie nicht alles zu diesem Projekt noch. Wir unterstützen Sie die richtigen Dateien dieses Projekts weiter unten in diesem Handbuch hinzufügen. Jetzt werden wir das Einrichten Ihrer Projektmappe weiterhin.

### <a name="add-a-project-that-starts-the-psf-launcher-executable"></a>Fügen Sie ein Projekt, das das ausführbare PSF-Startprogramm wird gestartet.

Fügen Sie ein C++ **leeres Projekt** Projekt der Projektmappe.

![Leeres Projekt](images/desktop-to-uwp/blank-app.png)

Hinzufügen der **PSF** Nuget-Paket auf dieses Projekt, indem Sie die gleichen Anweisungen, die im vorherigen Abschnitt beschrieben.

Öffnen Sie die Eigenschaftenseiten für das Projekt, und klicken Sie in der **allgemeine** legen Sie die Seite "Einstellungen" die **Zielname** Eigenschaft ``PSFLauncher32`` oder ``PSFLauncher64`` abhängig von der Architektur Ihrer Anwendung.

![PSF Startprogramm-Referenz](images/desktop-to-uwp/shim-exe-reference.png)

Fügen Sie einen Projektverweis auf die Common Language Runtime Fix-Projekt in der Projektmappe hinzu.

![Korrektur-Runtime-Referenz](images/desktop-to-uwp/reference-fix.png)

Mit der rechten Maustaste in der Referenz, und klicken Sie dann in der **Eigenschaften** Fenster diese Werte gelten.

| Eigenschaft | Wert |
|-------|-----------|
| Lokale Kopie | True |
| Lokale Satellitenassemblys kopieren | True |
| Verweisassemblyausgabe | True |
| Bibliothekabhängigkeiten verknüpfen | False |
| Bibliothekabhängigkeitseingaben Link | False |

### <a name="configure-the-packaging-project"></a>Konfigurieren Sie das Packaging-Projekt

Klicken sie im Paketprojekt mit der rechten Maustaste auf den Ordner **Anwendungen**, und wählen Sie dann **Verweis hinzufügen** aus.

![Hinzufügen des Projektverweises](images/desktop-to-uwp/add-reference-packaging-project.png)

Wählen Sie das PSF-Startprogramm-Projekt und Ihrem desktop-App-Projekt, und wählen Sie dann die **OK** Schaltfläche.

![Desktopprojekt](images/desktop-to-uwp/package-project-references.png)

>[!NOTE]
> Wenn Sie nicht den Quellcode der Anwendung haben, wählen Sie einfach das PSF Startprogramm-Projekt. Wir zeigen Ihnen wie die ausführbare Datei zu verweisen, wenn Sie eine Konfigurationsdatei erstellen.

In der **Anwendungen** Knoten mit der rechten Maustaste der PSF Startprogramm-Anwendung, und wählen Sie dann **als Einstiegspunkt festlegen**.

![Als Einstiegspunkt festlegen](images/desktop-to-uwp/set-startup-project.png)

Fügen Sie die Datei ``config.json`` zu Ihrem Projekt verpacken, kopieren Sie anschließend den folgenden Json-Text in die Datei einfügen. Legen Sie die **Paketaktion** Eigenschaft **Content**.

```json
{
    "applications": [
        {
            "id": "",
            "executable": "",
            "workingDirectory": ""
        }
    ],
    "processes": [
        {
            "executable": "",
            "fixups": [
                {
                    "dll": "",
                    "config": {
                    }
                }
            ]
        }
    ]
}
```

Geben Sie einen Wert für die einzelnen Schlüssel. Verwenden Sie diese Tabelle als Leitfaden.

| Array | key | Wert |
|-------|-----------|-------|
| applications | id |  Verwenden Sie den Wert der `Id` Attribut der `Application` -Element im Paketmanifest. |
| applications | ausführbare Datei | Die Paket-Relative Pfad der ausführbaren Datei, die Sie starten möchten. In den meisten Fällen können Sie diesen Wert aus der Paketmanifestdatei abrufen, bevor Sie sie ändern. Es ist der Wert des der `Executable` Attribut der `Application` Element. |
| applications | WorkingDirectory | (Optional) Ein Paket relativen Pfad als das Arbeitsverzeichnis der Anwendung verwenden, die gestartet wird. Wenn Sie diesen Wert nicht festlegen, um das Betriebssystem verwendet den `System32` Verzeichnis als Arbeitsverzeichnis der Anwendung. |
| Prozesse | ausführbare Datei | In den meisten Fällen werden diese den Namen des der `executable` oben mit dem Pfad und Dateierweiterungen entfernt konfiguriert. |
| Korrekturen | DLL | Paket relativen Pfad, den der Fixup-DLL zu laden. |
| Korrekturen | Konfigurationsdatei | (Optional) Steuert das Verhalten für der Fixup-DLL. Das genaue Format der dieser Wert variiert pro Fixup-von-Fixup wie jeder Korrektur dieses "Blob" interpretieren kann, wie er möchte. |

Wenn Sie fertig sind, Ihre ``config.json`` Datei sieht etwa wie folgt.

```json
{
  "applications": [
    {
      "id": "DesktopApplication",
      "executable": "DesktopApplication/WinFormsDesktopApplication.exe",
      "workingDirectory": "WinFormsDesktopApplication"
    }
  ],
  "processes": [
    {
      "executable": ".*App.*",
      "fixups": [ { "dll": "RuntimeFix.dll" } ]
    }
  ]
}

```

>[!NOTE]
> Die `applications`, `processes`, und `fixups` Schlüssel sind Arrays. Das bedeutet, dass Sie die Datei "config.json" verwenden können, um mehr als eine Anwendung, Prozess- und Fixup-DLL anzugeben.

### <a name="debug-a-runtime-fix"></a>Debuggen einer Lösung für die Common Language runtime

Drücken Sie in Visual Studio F5, um den Debugger zu starten.  Zunächst, die beginnt, ist die PSF Startprogramm-Anwendung, die wodurch wiederum die Ziel-desktop-Anwendung gestartet wird.  Um die Ziel-desktop-Anwendung zu debuggen, müssen Sie dazu manuell an den desktop-Anwendung-Prozess anfügen **Debuggen**->**an den Prozess anhängen**, und wählen Sie dann die Anwendung der Prozess. Um das Debuggen einer .NET-Anwendung mit einer Lösung für native-Runtime DLL zuzulassen, wählen Sie verwalteten und systemeigenen Code-Typen, die (Debuggen im gemischten Modus).  

Nachdem Sie diese eingerichtet haben, können Sie Breakpoints neben den Codezeilen in der desktop-App-Code und das laufzeitprojekt für die Korrektur festlegen. Wenn Sie nicht den Quellcode der Anwendung haben, werden Sie können Haltepunkte nur neben den Codezeilen in Ihrem Runtime Fix-Projekt festlegen.

>[!NOTE]
> Obwohl Visual Studio die einfachste Entwicklung bietet und Debuggen, einige Einschränkungen bestehen, also weiter unten in diesem Handbuch erörtern andere Debugverfahren wir, die Sie anwenden können.

## <a name="create-a-runtime-fix"></a>Erstellen Sie eine Lösung für die Common Language runtime

Wenn nicht vorhanden ist, noch eine Laufzeit für das Problem beheben, aufgelöst werden soll, können Sie eine neue Laufzeit-Lösung erstellen, durch das Schreiben von Ersatzfunktionen und einschließlich Konfigurationsdaten, sinnvoll. Sehen wir uns die einzelnen Teile an.

### <a name="replacement-functions"></a>Ersatzfunktionen

Ermitteln Sie zuerst die Funktion, die Aufrufe schlagen fehl, wenn Ihre Anwendung in einem MSIX-Container ausgeführt wird. Anschließend können Sie Ersatzfunktionen erstellen, die den Laufzeit-Manager stattdessen aufrufen soll. Dies bietet Ihnen die Möglichkeit der Implementierung einer Funktion mit dem Verhalten ersetzen, die die Regeln der modernen Laufzeitumgebung entspricht.

Öffnen Sie in Visual Studio das Common Language Runtime-Fix-Projekt, das Sie oben in dieser Anleitung erstellt haben.

Deklarieren Sie die ``FIXUP_DEFINE_EXPORTS`` Makro und fügen dann eine Include-Anweisung für die `fixup_framework.h` am oberen Rand jedes. CPP-Datei, in dem Sie die Funktionen der Laufzeit beheben hinzufügen möchten.

```c++
#define FIXUP_DEFINE_EXPORTS
#include <fixup_framework.h>
```

>[!IMPORTANT]
>Stellen Sie sicher, dass die `FIXUP_DEFINE_EXPORTS` Makro wird vor der Include-Anweisung angezeigt.

Erstellen Sie eine Funktion, die die gleiche Signatur der Funktion hat, verfügt über Verhalten, die Sie ändern möchten. Hier ist eine Beispielfunktion, die ersetzt die `MessageBoxW` Funktion.

```c++
auto MessageBoxWImpl = &::MessageBoxW;
int WINAPI MessageBoxWFixup(
    _In_opt_ HWND hwnd,
    _In_opt_ LPCWSTR,
    _In_opt_ LPCWSTR caption,
    _In_ UINT type)
{
    return MessageBoxWImpl(hwnd, L"SUCCESS: This worked", caption, type);
}

DECLARE_FIXUP(MessageBoxWImpl, MessageBoxWFixup);
```

Der Aufruf von `DECLARE_FIXUP` ordnet die `MessageBoxW` Funktion, um Ihr neue Ersetzungsfunktion. Wenn Ihre Anwendung versucht, rufen Sie die `MessageBoxW` -Funktion, es wird die Ersetzungsfunktion stattdessen aufzurufen.

#### <a name="protect-against-recursive-calls-to-functions-in-runtime-fixes"></a>Schutz vor rekursive Aufrufe von Funktionen in der Common Language Runtime-Fehlerbehebungen

Sie können optional auch anwenden, die `reentrancy_guard` Typ, der Ihre Funktionen, die für rekursive Aufrufe von Funktionen in der Common Language Runtime-Problembehebungen zu schützen.

Beispielsweise können Sie eine Ersatz-Funktion für erzeugen die `CreateFile` Funktion. Die Implementierung aufrufen kann die `CopyFile` -Funktion, aber die Implementierung von der `CopyFile` Funktionsaufruf kann die `CreateFile` Funktion. Dies kann zu einem unendlichen rekursiven Zyklus von Aufrufen an die `CreateFile` Funktion.

Weitere Informationen zu `reentrancy_guard` finden Sie unter [authoring.md](https://github.com/Microsoft/MSIX-PackageSupportFramework/blob/master/Authoring.md)

### <a name="configuration-data"></a>Konfigurationsdaten

Wenn Sie Konfigurationsdaten Laufzeit beheben hinzufügen möchten, erwägen sie die ``config.json``. Auf diese Weise können Sie die `FixupQueryCurrentDllConfig` , ganz einfach, Daten zu analysieren. In diesem Beispiel analysiert einen Boolean und String-Wert aus dieser Konfigurationsdatei.

```c++
if (auto configRoot = ::FixupQueryCurrentDllConfig())
{
    auto& config = configRoot->as_object();

    if (auto enabledValue = config.try_get("enabled"))
    {
        g_enabled = enabledValue->as_boolean().get();
    }

    if (auto logPathValue = config.try_get("logPath"))
    {
        g_logPath = logPathValue->as_string().wstring();
    }
}
```

## <a name="other-debugging-techniques"></a>Andere Debugverfahren

Visual Studio Sie das einfachste entwickeln und Debuggen erhalten, gibt es jedoch einige Einschränkungen.

Zunächst F5-debugging führt die Anwendung durch Bereitstellen von lose Dateien aus dem Paket Layout-Ordnerpfad, installieren, sondern aus einem .msix / AppX-Paket.  Der Ordner "Layout" muss in der Regel nicht die gleichen sicherheitseinschränkungen wie einem installierten Paket-Ordner. Daher kann es nicht Pfad Zugriff Denial Paketfehlern vor dem Anwenden einer Lösung für die Common Language Runtime reproduziert werden.

Um dieses Problem zu beheben, verwenden Sie .msix / AppX-Paket-Bereitstellung anstelle von F5 lose Datei Bereitstellung.  Erstellen Sie eine .msix / AppX-Paketdatei, die Verwendung der [MakeAppx](https://docs.microsoft.com/en-us/windows/desktop/appxpkg/make-appx-package--makeappx-exe-) Hilfsprogramm aus dem Windows SDK, wie oben beschrieben. Oder, in Visual Studio, mit der rechten Maustaste in Ihrer Anwendung-Projektknoten und wählen **Store**->**App-Pakete erstellen**.

Ein weiteres Problem mit Visual Studio ist, dass es keine integrierten Unterstützung für das Anfügen an alle untergeordneten Prozesse, die vom Debugger gestartet.   Dies erschwert es zum Debuggen von Logik, die während des Starts der Anwendung, die nach dem Start von Visual Studio manuell angefügt werden muss.

Um dieses Problem zu beheben, verwenden Sie einen Debugger, die von untergeordneten Prozess anfügen unterstützt.  Beachten Sie, dass es im Allgemeinen nicht möglich, einen just-in-Time (JIT)-Debugger an die Zielanwendung anzufügen.  Grund dafür ist die meisten Techniken für JIT-Kompilierung Starten des Debuggers anstelle der Ziel-app, über den ImageFileExecutionOptions-Registrierungsschlüssel.  Dies verfehlt den detouring Mechanismus PSFLauncher.exe FixupRuntime.dll in die Ziel-app einfügen.  WinDbg, enthalten der [Debugging-Tools für Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index), und erhalten Sie von der [Windows SDK](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk), unterstützt untergeordneten Prozess angefügt.  Es unterstützt jetzt auch direkt [starten und Debuggen eine UWP-app](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugging-a-uwp-app-using-windbg#span-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanspan-idlaunchinganddebuggingauwpappspanlaunching-and-debugging-a-uwp-app).

Starten Sie zum Starten der Ziel-Anwendung als untergeordneter Prozess zu debuggen, ``WinDbg``.

```ps
windbg.exe -plmPackage PSFSampleWithFixup_1.0.59.0_x86__7s220nvg1hg3m -plmApp PSFSample
```

Auf der ``WinDbg`` auffordern, untergeordneten Debuggen zu aktivieren, und legen die geeigneten Haltepunkte.

```ps
.childdbg 1
g
```

(wird ausgeführt, bis die Anwendung startet und unterbricht den Debugger)

```ps
sxe ld fixup.dll
g
```

(führen Sie erst der Fixup-DLL geladen wird)

```ps
bp ...
```

>[!NOTE]
> [PLMDebug](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/plmdebug) kann auch das Anfügen eines Debuggers an eine app beim Start verwendet werden, und ist auch im enthalten die [Debugging-Tools für Windows](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/index).  Es ist jedoch komplexer, als der direkte Unterstützung nun von WinDbg verwenden.

## <a name="support-and-feedback"></a>Support und Feedback

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
