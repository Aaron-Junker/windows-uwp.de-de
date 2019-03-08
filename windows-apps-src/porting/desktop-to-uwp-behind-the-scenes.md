---
Description: Dieser Artikel beschäftigt sich eingehender damit, wie die Desktop-Brücke funktioniert.
title: Hintergrundinformationen zur Desktop-Brücke
ms.date: 05/25/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: a399fae9-122c-46c4-a1dc-a1a241e5547a
ms.localizationpriority: medium
ms.openlocfilehash: 644139f800caa2ead61ce19d63d4408c01575025
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603645"
---
# <a name="behind-the-scenes-of-your-packaged-desktop-application"></a>Hinter den Kulissen der gepackten desktop-Anwendung

Dieser Artikel enthält eine eingehendere Informationen dazu, was, Dateien und Registrierungseinträge repariert, geschieht Wenn Sie ein Windows-app-Paket für die desktop-Anwendung erstellen.

Ein wichtiges Ziel eines Pakets moderne ist getrennt von Anwendungsstatus von systemstatusschutz Beibehaltung der Kompatibilität mit anderen apps so weit wie möglich. Die Brücke erreicht dies, indem sie die Anwendung in einem Paket für die universelle Windows-Plattform (UWP) platziert und anschließend einige zur Laufzeit am Dateisystem und an der Registrierung vorgenommene Änderungen ermittelt und umleitet.

Pakete, die Sie für die desktop-Anwendung zu erstellen sind nur für Desktops, voll vertrauenswürdige Anwendungen und sind nicht virtualisierte oder Sandbox. Dies ermöglicht es ihnen, auf die gleiche Weise mit anderen Apps zu interagieren wie klassische Desktop-Anwendungen.

## <a name="installation"></a>Installation

App-Pakete werden mit der ausführbaren Datei *app_name.exe* unter *C:\Programme\WindowsApps\package_name* installiert. Jeder Paketordner enthält ein Manifest (AppxManifest.xml), das einen speziellen XML-Namespace für verpackte Apps enthält. In der Manifestdatei ist ein ```<EntryPoint>```-Element enthalten, das auf die vertrauenswürdige App verweist. Wenn die Anwendung gestartet wird, nicht in einem app-Container ausgeführt, aber stattdessen Ausführung als Benutzer wie üblich.

Nach der Bereitstellung werden Paketdateien als schreibgeschützt markiert und vom Betriebssystem gesperrt. Windows verhindert, dass Apps gestartet werden, wenn diese Dateien manipuliert werden.

## <a name="file-system"></a>Dateisystem

Um die app-Status enthalten, werden Änderungen, die die Anwendung AppData stellt erfasst. Alle Schreibvorgänge in den AppData-Ordner des Benutzers (z. B. *C:\Benutzer\Benutzername\AppData*), einschließlich Erstellungs-, Lösch- und Aktualisierungsvorgänge, werden direkt an einen privaten Speicherort pro Benutzer und App kopiert. Dies erstellt den Eindruck, dass die verpackte Anwendung eine der echten AppData bearbeitet wird, wenn tatsächlich eine private Kopie geändert werden. Durch eine derartige Umleitung von Schreibvorgängen kann das System alle von der App vorgenommenen Dateiänderungen nachverfolgen. Dadurch kann das System bereinigen Sie diese Dateien bei der Deinstallation der Anwendung, daher reduzieren-System "Rot" und Bereitstellen einer besseren Entfernung von Anwendungen für den Benutzer.

Zusätzlich zum Umleiten von AppData, werden die bekannten Windows-Ordnern ("System32", "Program Files (x86)", usw.) dynamisch mit entsprechenden Verzeichnissen im app-Paket zusammengeführt. Jedes verpackte Paket enthält im Stammverzeichnis einen Ordner mit dem Namen „VFS“. Alle Lesevorgänge für Verzeichnisse oder Dateien im VFS-Verzeichnis werden zur Laufzeit mit den jeweiligen nativen Entsprechungen zusammengeführt. Beispielsweise kann eine Anwendung enthalten *c:\Programme\Microsoft Files\WindowsApps\package_name\VFS\SystemX86\vc10.dll* wie Teil des app-Paket, aber die Datei angezeigt wird, installiert werden *C:\Windows\System32\ vc10.dll*.  Dies gewährleistet die Kompatibilität mit Desktopanwendungen, die davon ausgehen, dass sich Dateien an Speicherorten ohne Pakete befinden.

Schreibvorgänge in Dateien/Ordner im verpackten App-Paket sind nicht zulässig. Schreibvorgänge in Dateien und Ordner, die nicht Teil des Pakets sind, werden von der Brücke ignoriert und sind nur zulässig, wenn der Benutzer über entsprechende Berechtigungen verfügt.

### <a name="common-operations"></a>Allgemeine Vorgänge

Diese kurze Referenztabelle enthält allgemeine Dateisystemvorgänge und Informationen zur Verarbeitung durch die Brücke.

Vorgang | Ergebnis | Beispiel
:--- | :--- | :---
Lesen oder Enumerieren einer bekannten Windows-Datei oder eines bekannten Windows-Ordners | Eine dynamische Zusammenführung von *C:\Programme\Paketname\VFS\bekannter Ordner* mit der lokalen Systementsprechung | Der Lesevorgang für *C:\Windows\System32* gibt den Inhalt von *C:\Windows\System32* und den Inhalt von *C:\Programme\WindowsApps\Paketname\VFS\SystemX86* zurück.
Schreiben unter „AppData“ | Kopie bei Schreibvorgang an einem Speicherort pro Benutzer und App | Für „AppData“ ist dies in der Regel *C:\Benutzer\Benutzername\AppData*.  
Schreibvorgänge im Paket | Nicht zulässig Das Paket ist schreibgeschützt. | Schreibvorgänge unter *C:\Programme\WindowsApps\Paketname* sind nicht zulässig.
Schreibvorgänge außerhalb des Pakets | Zulässig, wenn der Benutzer über entsprechende Berechtigungen verfügt. | Ein Schreibvorgang in *C:\Windows\System32\foo.dll* ist zulässig, wenn das Paket nicht *C:\Programme\WindowsApps\Paketname\VFS\SystemX86\foo.dll* enthält und der Benutzer über entsprechende Berechtigungen verfügt.

### <a name="packaged-vfs-locations"></a>Gepackte VFS-Speicherorte

Der folgenden Tabelle können Sie entnehmen, wo Dateien, die zu Ihrem Paket gehören, für die App im System überlagert sind. Die Anwendung wird diese Dateien in den aufgelisteten Systemspeicherorte werden in der Tat die umgeleiteten Speicherorten innerhalb werden wahrnehmen *c:\Programme\Microsoft Files\WindowsApps\package_name\VFS*. Die FOLDERID-Speicherorte stammen von der [**KNOWNFOLDERID**](https://msdn.microsoft.com/library/windows/desktop/dd378457.aspx)-Konstante.

Systemspeicherort | Speicherort umgeleitet (unter ["PackageRoot"] \VFS\) | Gültig für Architekturen
 :--- | :--- | :---
FOLDERID_SystemX86 | SystemX86 | x86, amd64
FOLDERID_System | SystemX64 | amd64
FOLDERID_ProgramFilesX86 | ProgramFilesX86 | x86, amd6
FOLDERID_ProgramFilesX64 | ProgramFilesX64 | amd64
FOLDERID_ProgramFilesCommonX86 | ProgramFilesCommonX86 | x86, amd64
FOLDERID_ProgramFilesCommonX64 | ProgramFilesCommonX64 | amd64
FOLDERID_Windows | Windows | x86, amd64
FOLDERID_ProgramData | Gemeinsamer AppData-Ordner | x86, amd64
FOLDERID_System\catroot | AppVSystem32Catroot | x86, amd64
FOLDERID_System\catroot2 | AppVSystem32Catroot2 | x86, amd64
FOLDERID_System\drivers\etc | AppVSystem32DriversEtc | x86, amd64
FOLDERID_System\driverstore | AppVSystem32Driverstore | x86, amd64
FOLDERID_System\logfiles | AppVSystem32Logfiles | x86, amd64
FOLDERID_System\spool | AppVSystem32Spool | x86, amd64

## <a name="registry"></a>Registrierung

App-Pakete enthalten eine Datei namens „registry.dat“, die als logische Entsprechung für *HKLM\Software* in der tatsächlichen Registrierung dient. Zur Laufzeit führt diese virtuelle Registrierung den Inhalt dieser Struktur in der nativen Systemstruktur zusammen, um beide Strukturen in einer Ansicht darzustellen. Wenn „registry.dat“ beispielsweise einen einzelnen Schlüssel namens „Foo“ enthält, enthält ein Lesevorgang für *HKLM\Software* zur Laufzeit scheinbar ebenfalls „Foo“ (zusätzlich zu allen nativen Systemschlüsseln).

Nur Schlüssel unter *HKLM\Software* sind Teil des Pakets. Schlüssel unter *HKCU* oder in anderen Registrierungsbereichen sind nicht Teil des Pakets. Schreibvorgänge für Schlüssel oder Werte im Paket sind nicht zulässig. Schreibt in Schlüssel oder Werte nicht Teil des Pakets sind zulässig, solange der Benutzer zugreifen kann.

Alle Schreibvorgänge unter „HKCU“ entsprechen Kopie bei Schreibvorgang an einem privaten Speicherort pro Benutzer und App. In der Regel können Deinstallationsprogramme *HKEY_CURRENT_USER* nicht bereinigen, da die Bereitstellung von Registrierungsdaten für abgemeldete Benutzer aufgehoben wird und die Daten daher nicht verfügbar sind.

Alle Schreibvorgänge werden während des paketupgrades beibehalten und nur gelöscht, wenn die Anwendung vollständig entfernt wurde.

### <a name="common-operations"></a>Allgemeine Vorgänge

Diese kurze Referenztabelle enthält allgemeine Registrierungsvorgänge und Informationen zur Verarbeitung durch die Brücke.

Vorgang | Ergebnis | Beispiel
:--- | :--- | :---
Lesen oder Enumerieren von *HKLM\Software* | Eine dynamische Zusammenführung der Paketstruktur mit der lokalen Systementsprechung | Wenn „registry.dat“ einen einzelnen Schlüssel namens „Foo“ enthält, zeigt ein Lesevorgang von *HKLM\Software* zur Laufzeit die Inhalte von *HKLM\Software* und *HKLM\Software\Foo* an.
Schreibvorgänge unter „HKCU“ | Kopie bei Schreibvorgang an einem privaten Speicherort pro Benutzer und App | Identisch mit „AppData“ für Dateien
Schreibvorgänge im Paket | Nicht zulässig Das Paket ist schreibgeschützt. | Schreibvorgänge unter *HKLM\Software* sind nicht zulässig, wenn ein entsprechender Schlüssel/Wert in der Paketstruktur vorhanden ist.
Schreibvorgänge außerhalb des Pakets | Von der Brücke ignoriert. Zulässig, wenn der Benutzer über entsprechende Berechtigungen verfügt. | Schreibvorgänge unter *HKLM\Software* sind zulässig, sofern kein entsprechender Schlüssel/Wert in der Paketstruktur vorhanden ist und der Benutzer über entsprechende Zugriffsberechtigungen verfügt.

## <a name="uninstallation"></a>Deinstallation

Wenn ein Paket vom Benutzer deinstalliert wird, alle Dateien und Ordner befindet sich unter *c:\Programme\Microsoft Files\WindowsApps\package_name* entfernt werden, sowie die umgeleiteten Schreibvorgänge in AppData oder die Registrierung, die während der aufgezeichnet wurden die Prozess der paketerstellung.

## <a name="next-steps"></a>Nächste Schritte

**Hier finden Sie Antworten auf Ihre Fragen**

Haben Sie Fragen? Fragen Sie uns auf Stack Overflow. Unser Team überwacht diese [Tags](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Fragen Sie uns [hier](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Geben Sie Feedback oder Vorschläge für Features**

Weitere Informationen finden Sie unter [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
