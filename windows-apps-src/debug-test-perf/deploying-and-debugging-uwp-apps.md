---
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: Dieser Artikel führt Sie Schritt für Schritt durch die Ausrichtung Ihrer Apps auf verschiedene Bereitstellungs- und Debugziele.
title: Bereitstellen und Debuggen von UWP (Universelle Windows-Plattform)-Apps
ms.date: 4/8/2019
ms.topic: article
keywords: windows 10, uwp, debuggen, testen, leistung
ms.localizationpriority: medium
ms.openlocfilehash: c210f84c4fc3d07ba5a3d81eef059e17fdf9f308
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244436"
---
# <a name="deploying-and-debugging-uwp-apps"></a>Bereitstellen und Debuggen von UWP-Apps

Dieser Artikel führt Sie Schritt für Schritt durch die Ausrichtung Ihrer Apps auf verschiedene Bereitstellungs- und Debugziele.

Microsoft Visual Studio ermöglicht Ihnen das Bereitstellen und Debuggen Ihre apps (Universelle Windows Plattform) auf einer Vielzahl von Windows 10-Geräte. Das Erstellen und Registrieren der App auf dem Zielgerät wird von Visual Studio abgewickelt.

## <a name="picking-a-deployment-target"></a>Auswählen eines Bereitstellungsziels

Zur Auswahl eines Ziels navigieren Sie zur Dropdownliste mit Debugzielen neben der Schaltfläche **Debugging starten**, und wählen Sie das Ziel aus, auf dem Sie Ihre App bereitstellen möchten. Nachdem Sie das Ziel ausgewählt haben, wählen Sie entweder **Debugging starten (F5)** aus, um das Bereitstellen und Debuggen in einem Schritt auszuführen, oder drücken Sie **STRG+F5**, um nur die Bereitstellung auf dem Ziel auszuführen.

![Liste mit Zielgeräten für das Debuggen](images/debug-device-target-list.png)

- Mit **Simulator** wird die App in einer simulierten Umgebung auf dem aktuellen Entwicklungscomputer bereitgestellt. Diese Option ist nur verfügbar wenn Ihrer app **Mindestversion. Version** ist kleiner als oder gleich der für das Betriebssystem auf dem Entwicklungscomputer.
- Mit **Lokaler Computer** wird die App auf dem aktuellen Entwicklungscomputer bereitgestellt. Diese Option ist nur verfügbar wenn Ihrer app **Mindestversion. Version** ist kleiner als oder gleich der für das Betriebssystem auf dem Entwicklungscomputer.
- Über **Remotecomputer** können Sie ein Remoteziel angeben, auf dem die App bereitgestellt werden soll. Weitere Informationen zur Bereitstellung auf einem Remotecomputer finden Sie unter [Angeben eines Remotegeräts](#specifying-a-remote-device).
- Mit **Gerät** wird die App auf einem über USB verbundenen Gerät bereitgestellt. Das Gerät muss für Entwickler entsperrt sein und über einen entsperrten Bildschirm verfügen.
- Bei einem **Emulator**-Ziel wird die App in einem Emulator gestartet und bereitgestellt, wobei die Konfiguration im Namen angegeben ist. Emulatoren sind nur auf Hyper-V-Computer mit Windows 8.1 aktiviert oder über.

## <a name="debugging-deployed-apps"></a>Debuggen von bereitgestellten Apps

Visual Studio kann über den Menübefehl **Debuggen**, **An Prozess anhängen** an alle ausgeführten Prozesse in einer UWP-App angehängt werden. Für das Anhängen an einen laufenden Prozess ist das ursprüngliche Visual Studio-Projekt nicht erforderlich. Das Laden der [Symbole](#symbols) des Prozesses hilft beim Debuggen eines Prozesses ohne den ursprünglichen Code erheblich.  

Darüber hinaus kann über den Menübefehl **Debuggen**, **Andere**, **Installierte App-Pakete debuggen** jedes installierten App-Paket angehängt und debuggt werden.

![Dialogfeld „Installiertes App-Paket debuggen"](images/gs-debug-uwp-apps-002.png)

Bei der Auswahl von **Eigenen Code zunächst nicht starten, sondern debuggen** wird der Visual Studio-Debugger an Ihre UWP-App angehängt, sobald Sie ihn starten. Dies bietet eine effektive Möglichkeit zum Debuggen von Steuerelementpfaden über [verschiedene Startmethoden](../xbox-apps/automate-launching-uwp-apps.md) (z. B. die Protokollaktivierung mit benutzerdefinierten Parametern).  

UWP-Apps können unter Windows 8.1 oder höher entwickelt und kompiliert werden. Sie müssen jedoch unter Windows 10 ausgeführt werden. Wenn Sie eine UWP-App auf einem PC unter Windows 8.1 entwickeln, können Sie eine auf einem anderen Windows 10-Gerät ausgeführte UWP-App remote debuggen. Der Host und der Zielcomputer müssen sich im selben LAN befinden. Laden Sie hierzu auf beiden Computern die [Remotetools für Visual Studio](https://www.visualstudio.com/downloads/) herunter, und installieren Sie sie. Die installierte Version muss der installierten Version von Visual Studio entsprechen. Die ausgewählte Architektur der Auswahl (x86, x64) muss mit der Architektur der Ziel-App übereinstimmen.

## <a name="package-layout"></a>Paketlayout

Ab Visual Studio 2015 Update 3 haben wir die Option für Entwickler festlegen den Layout-Pfad für ihre UWP-apps hinzugefügt. Hiermit wird festgelegt, an welchen Ort auf dem Datenträger das Paketlayout beim Erstellen der App kopiert wird. In der Standardeinstellung wird diese Eigenschaft relativ zum Stammverzeichnis des Projekts festgelegt. Wenn Sie diese Eigenschaft nicht ändern, entspricht das Verhalten dem der früheren Versionen von Visual Studio.

Diese Eigenschaft kann in den **Debug**-Eigenschaften des Projekts geändert werden.

Wenn Sie beim Erstellen eines Pakets für die App alle Layoutdateien in dieses einschließen möchten, müssen Sie die Projekteigenschaft `<IncludeLayoutFilesInPackage>true</IncludeLayoutFilesInPackage>` hinzufügen.

So fügen Sie diese Eigenschaft hinzu

1. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **Projekt entladen** aus.
2. Klicken Sie mit der rechten Maustaste auf das Projekt, und wählen Sie dann **[Projektname].xxproj bearbeiten** aus (XXPROJ lautet je nach Projektsprache unterschiedlich).
3. Fügen Sie die Eigenschaft hinzu, und laden Sie das Projekt erneut.

## <a name="specifying-a-remote-device"></a>Festlegen eines Remotegeräts

### <a name="c-and-microsoft-visual-basic"></a>C# und Microsoft Visual Basic

Wenn Sie einen Remotecomputer für C#- oder Microsoft Visual Basic-Apps angeben möchten, wählen Sie in der Dropdownliste mit Debugzielen **Remotecomputer** aus. Das Dialogfeld **Remoteverbindungen** wird angezeigt. Hier können Sie eine IP-Adresse angeben oder ein erkanntes Gerät auswählen. Standardmäßig ist der Authentifizierungsmodus **Universell** ausgewählt. Lesen Sie den Abschnitt [Authentifizierungsmodi](#authentication-modes), um den geeigneten Authentifizierungsmodus zu ermitteln.

![Dialogfeld „Remoteverbindungen“](images/debug-remote-connections.png)

Um zu diesem Dialogfeld zurückzukehren, können Sie die Projekteigenschaften öffnen und zur Registerkarte **Debuggen** navigieren. Wählen Sie dort neben **Remotecomputer** die Option **Suchen** aus.

![Registerkarte „Debuggen“](images/debug-remote-machine-config.png)

Wenn Sie eine App auf einem Remote-PC bereitstellen möchten, der noch nicht das Creators-Update enthält, müssen Sie auch die Visual Studio-Remotetools auf den Ziel-PC herunterladen und dort installieren. Eine vollständige Anleitung finden Sie unter [Anweisungen zu Remote-PCs](#remote-pc-instructions).  Ab dem Creators Update unterstützt ein PC auch Remotebereitstellung.  

### <a name="c-and-javascript"></a>C++ und JavaScript

So geben Sie einen Remotecomputer Ziel für eine C++ oder JavaScript-UWP-app an:

1. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und klicken Sie dann auf **Eigenschaften**.
2. Navigieren Sie zu den Einstellungen für **Debuggen**, und wählen Sie unter **Zu startender Debugger** die Option **Remotecomputer** aus.
3. Geben Sie den **Computernamen** ein (oder klicken Sie auf **Suchen**, um einen Namen zu suchen), und legen Sie die Eigenschaft **Authentifizierungstyp** fest.

![Seiten mit den Eigenschaften für das Debuggen](images/debug-property-pages.png)

Nachdem der Computer angegeben wurde, können Sie in der Dropdownliste mit Debugzielen **Remotecomputer** auswählen, um zum angegebenen Computer zurückzukehren. Es kann jeweils nur ein Remotecomputer ausgewählt werden.

### <a name="remote-pc-instructions"></a>Anweisungen für Remote-PCs

> [!NOTE]
> Diese Anleitung ist nur für ältere Versionen von Windows 10 erforderlich.  Ab dem Creators-Update kann ein PC wie eine Xbox behandelt werden.  Dazu müssen Sie die Geräteerkennung im PC-Entwicklermodusmenü aktivieren und die universelle Authentifizierung für PIN-Kopplung und Verbindung mit dem PC verwenden.

Für die Bereitstellung auf einem Remote-PC, der noch nicht das Creators-Update enthält, müssen auf dem Ziel-PC die Remotetools für Visual Studio installiert sein. Der remote-PC muss auch eine Version von Windows, die größer als oder gleich auf Ihre apps ausgeführt werden **Mindestversion. Version** Eigenschaft. Nachdem Sie die Remotetools installiert haben, müssen Sie den Remotedebugger auf dem Ziel-PC starten.

Suchen Sie dazu im Menü **Start** nach **Remotedebugger**, öffnen Sie den Debugger, und erlauben Sie ihm die Konfiguration Ihrer Firewalleinstellungen, wenn Sie dazu aufgefordert werden. Standardmäßig wird der Debugger mit Windows-Authentifizierung gestartet. Somit werden die Benutzeranmeldeinformationen benötigt, falls der angemeldete Benutzer nicht auf beiden PCs identisch ist.

So ändern Sie es **keine Authentifizierung**in die **Remotedebugger**, wechseln Sie zu **Tools**  - &gt; **Optionen**, und Legen Sie sie dann auf **keine Authentifizierung**. Nach dem Einrichten des Remotedebuggers müssen Sie zudem sicherstellen, dass das Hostgerät auf [Entwicklermodus](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) gesetzt wurde. Anschließend können Sie die Bereitstellung über Ihren Entwicklungscomputer ausführen.

Weitere Informationen finden Sie im [Visual Studio Download Center](https://www.visualstudio.com/downloads/).

## <a name="passing-command-line-debug-arguments"></a>Übergeben von Debugargumenten in der Befehlszeile

In Visual Studio 2017 können Sie beim Starten des Debuggens von UWP-Anwendungen Debugargumente in der Befehlszeile übergeben. Sie können auf die Debugargumente für die Befehlszeile mit dem *Args*-Parameter in der **OnLaunched**-Methode der [**Application**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.application)-Klasse zugreifen. Öffnen Sie die Projekteigenschaften, und navigieren Sie zur Registerkarte **Debuggen**, um Debugargumente für die Befehlszeile anzugeben.

> [!NOTE]
> Diese Registerkarte ist in Visual Studio 2017 (Version 15.1) für C#, VB und C++ verfügbar. JavaScript ist in neueren Versionen von Visual Studio 2017 verfügbar. Debugargumente in der Befehlszeile sind für alle Bereitstellungstypen mit Ausnahme des Simulators verfügbar.

Für C#- und VB-UWP-Projekte wird das Feld **Befehlszeilenargumente:** unter **Startoptionen** angezeigt.

![Befehlszeilenargumente](images/command-line-arguments.png)

Für C++- und JS-UWP-Projekte wird das Feld **Befehlszeilenargumente** in den **Debugeigenschaften** angezeigt.

![C++- und JS-Befehlszeilenargumente](images/command-line-arguments-cpp.png)

Nach dem Festlegen der Befehlszeilenargumente können Sie auf den Wert des Arguments in der **OnLaunched**-Methode der App zugreifen. Die *Args* des [**LaunchActivatedEventArgs**](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.activation.launchactivatedeventargs)-Objekts enthalten die **Arguments**-Eigenschaft, deren Wert auf den Text im Feld **Befehlszeilenargumente** festgelegt ist.

![C++- und JS-Befehlszeilenargumente](images/command-line-arguments-debugging.png)

## <a name="authentication-modes"></a>Authentifizierungsmodi

Für die Bereitstellung auf Remotecomputern gibt es drei Authentifizierungsmodi:

- **Universell (unverschlüsseltes Protokoll)**: Verwenden Sie diesen Authentifizierungsmodus aus, wenn Sie auf einem Remotegerät bereitstellen. Derzeit können dies IoT-, Xbox- und HoloLens-Geräte sein oder PCs mit Creators-Update (oder neuer). „Universell (unverschlüsseltes Protokoll)“ sollte nur in vertrauenswürdigen Netzwerken verwendet werden. Die Debuggingverbindung ist anfällig für böswillige Benutzer, die zwischen dem Entwicklungs- und dem Remotecomputer übertragene Daten abfangen und manipulieren könnten.
- **Windows**: Dieser Authentifizierungsmodus wird ausschließlich für eine remote-PC (Desktop oder Laptop) verwendet werden die Visual Studio Remote Tools ausführen. Verwenden Sie diesen Authentifizierungsmodus, wenn Sie Zugriff auf die Anmeldeinformationen des beim Zielcomputer angemeldeten Benutzers haben. Dies ist der sicherste Kanal für die Remotebereitstellung.
- **Keine**: Dieser Authentifizierungsmodus wird ausschließlich für eine remote-PC (Desktop oder Laptop) verwendet werden die Visual Studio Remote Tools ausführen. Verwenden Sie diese Authentifizierungsmoduseinstellung, wenn Sie einen Testcomputer in einer Umgebung eingerichtet haben, bei der die Anmeldung über ein Testkonto erfolgte und keine Anmeldeinformationen eingegeben werden können. Die Remotedebuggereinstellungen müssen so festgelegt sein, dass sie den Modus „Keine Authentifizierung“ akzeptieren.

## <a name="advanced-remote-deployment-options"></a>Erweiterte Remotebereitstellungsoptionen

Vorliegen von der Version von Visual Studio 2015 Update 3 und Windows 10 Anniversary Update-Optionen neue erweiterte Remotebereitstellung für bestimmte Windows 10-Geräte. Die erweiterten Optionen für Remotebereitstellung finden Sie im Menü **Debuggen** für die Projekteigenschaften.

Zu den neuen Eigenschaften zählen:

- Bereitstellungstyp
- Paketregistrierungspfad
- Beibehalten aller Dateien auf dem Gerät – auch wenn diese nicht mehr Teil des Layouts sind

### <a name="requirements"></a>Anforderungen

Um die erweiterten Remotebereitstellungsoptionen verwenden zu können, müssen folgende Anforderungen erfüllt sein:

- Haben Sie Visual Studio 2015 Update 3 oder eine höhere Visual Studio Version oder höher installiert mit Windows 10-Tools 1.4.1 (enthält das Windows 10 Anniversary Update SDK) Es wird empfohlen, dass Sie die neueste Version von Visual Studio mit Updates verwenden, um sicherzustellen, dass Sie alle erhalten die neueste Funktionen für Entwicklung und Sicherheit.
- Ziel ist ein Xbox-Remotegerät mit Windows 10 Anniversary Update oder ein PC mit Windows 10 Creators Update.
- Verwenden des universellen Authentifizierungsmodus

### <a name="properties-pages"></a>Eigenschaftenseiten

Bei einer C#- oder Visual Basic-UWP-App wird die Eigenschaftenseite wie folgt aussehen.

![CS- oder VB-Eigenschaften](images/advanced-remote-deploy-cs.png)

Für eine C++-UWP-App wird die Eigenschaftenseite wie folgt aussehen.

![Cpp-Eigenschaften](images/advanced-remote-deploy-cpp.png)

### <a name="copy-files-to-device"></a>Dateien auf Gerät kopieren

Mit **Dateien auf Gerät kopieren** werden die Dateien über das Netzwerk physisch auf das Remotegerät übertragen. Das Paketlayout für das **Layout Ordnerpfad** wird kopiert und registriert. Die auf das Gerät kopierten Dateien werden in Visual Studio weiter mit denen des Visual Studio-Projekts synchronisiert. Es gibt jedoch auch die Option, **alle Dateien auf dem Gerät beizubehalten – auch wenn diese nicht mehr Teil des Layouts sind**. Diese Option bedeutet, dass alle Dateien, die zuvor auf das Remotegerät kopiert wurden und nicht mehr Teil Ihres Projekts sind, auf dem Remotegerät verbleiben.

Der beim **Kopieren von Dateien auf das Gerät** angegebene **Paketregistrierungspfad** ist der physische Speicherort auf dem Remotegerät, auf das die Dateien kopiert werden. Dieser Pfad kann als beliebiger relativer Pfad angegeben werden. Der Speicherort, an dem die Dateien bereitgestellt werden, ist relativ zum Stammverzeichnis der Entwicklungsdateien und daher je nach Zielgerät unterschiedlich. Es ist sinnvoll, diesen Pfad anzugeben, wenn mehrere Entwickler ein Gerät verwenden und an Paketen mit identischer Build-Abweichung arbeiten.

> [!NOTE]
> **Dateien auf Gerät kopieren** wird derzeit für Xbox mit Windows 10 Anniversary Update und PCs mit Windows 10 Creators Update unterstützt.

Auf dem Remotegerät wird das Layout an folgenden Standardspeicherort kopiert: `\\MY-DEVKIT\DevelopmentFiles\PACKAGE-REGISTRATION-PATH`

### <a name="register-layout-from-network"></a>Registrieren des Layouts über das Netzwerk

Wenn Sie das Layout über das Netzwerk registrieren möchten, können Sie ein Paketlayout für eine Netzwerkfreigabe erstellen, um anschließend das Layout auf dem Remotegerät direkt über das Netzwerk zu registrieren. Hierzu müssen Sie einen Ordnerpfad für das Layout (eine Netzwerkfreigabe) angeben, auf den vom Remotegerät aus zugegriffen werden kann. Die Eigenschaft **Layout Ordnerpfad** ist der relative Pfad zum Visual Studio-PC, während es sich bei der Eigenschaft **Paketregistrierungspfad** um denselben Pfad handelt, der jedoch relativ zum Remotegerät angegeben wurde.

Um das Layout erfolgreich über das Netzwerk zu registrieren, müssen Sie zunächst für **Layout Ordnerpfad** einen freigegebenen Netzwerkordner angeben. Klicken Sie hierzu im Datei-Explorer mit der rechten Maustaste auf den Ordner, und wählen Sie **Freigeben für > Bestimmte Personen** sowie anschließend die Benutzer aus, für die der Ordner freigegeben werden soll. Wenn Sie versuchen, das Layout über das Netzwerk zu registrieren, werden Sie aufgefordert, Anmeldeinformationen anzugeben. Damit wird sichergestellt, dass Sie die Registrierung als Benutzer mit Zugriff auf die Freigabe durchführen.

Hilfe hierzu bieten die folgenden Beispiele:

- Beispiel 1 (lokaler Layoutordner, auf den als Netzwerkfreigabe zugegriffen werden kann):
  - **Ordnerpfad des Layouts** = `D:\Layouts\App1`
  - **Paketregistrierungspfad** = `\\NETWORK-SHARE\Layouts\App1`

- Beispiel 2 (Netzwerk-Layoutordner):
  - **Ordnerpfad des Layouts** = `\\NETWORK-SHARE\Layouts\App1`
  - **Paketregistrierungspfad** = `\\NETWORK-SHARE\Layouts\App1`

Wenn Sie das Layout zunächst über das Netzwerk registrieren, werden Ihre Anmeldeinformationen auf dem Zielgerät zwischengespeichert, sodass Sie sich nicht immer wieder anmelden müssen. Um zwischengespeicherte Anmeldeinformationen zu entfernen, verwenden Sie [WinAppDeployCmd.exe Tool](https://msdn.microsoft.com/windows/uwp/packaging/install-universal-windows-apps-with-the-winappdeploycmd-tool) (Teil des Windows 10-SDKs) mit dem Befehl **Deletecreds**.

Beim Registrieren des Geräts über das Netzwerk können Sie **Alle Dateien auf dem Gerät beibehalten** nicht auswählen, da keine Dateien physisch auf das Remotegerät kopiert werden.

> [!NOTE]
> **Registrieren des Layouts über das Netzwerk** wird derzeit für Xbox mit Windows 10 Anniversary Update und PCs mit Windows 10 Creators Update unterstützt.

Auf dem Remotegerät, das Layout wird am folgenden Standardspeicherort abhängig von die Gerätefamilie registriert: `Xbox: \\MY-DEVKIT\DevelopmentFiles\XrfsFiles` -Hierbei handelt es sich um einen Symlink zu den **paketregistrierungspfad** PC keine symbolische Verknüpfung verwendet und stattdessen direkt registriert die **paketregistrierungspfad**

## <a name="debugging-options"></a>Debugoptionen

Unter Windows 10, die startleistung von UWP-apps verbessert wird, indem Sie proaktiv zu starten und Anhalten von apps dann in eine Technik namens [Vorabstart der](https://msdn.microsoft.com/library/windows/apps/Mt593297). Viele Apps sind in diesem Modus sofort funktionsfähig, das Verhalten einiger Apps muss jedoch möglicherweise angepasst werden. Um das Debuggen von Problemen in diesen Codepfaden zu erleichtern, können Sie das Debuggen der App von Visual Studio im Vorabstartmodus starten.

Das Debuggen unterstützt wird sowohl von Visual Studio-Projekt (**Debuggen**  - &gt; **andere Debugziele**  - &gt; **universelle Debuggen Windows-App der Vorabstart**), und für apps, die bereits auf dem Computer installierten (**Debuggen**  - &gt; **andere Debugziele**  - &gt; **Debuggen Installed App Package** durch Auswählen der **aktivieren-app mit der Vorabstart** Kontrollkästchen). Weitere Informationen finden Sie unter [Debuggen des UWP-Vorabstarts](https://go.microsoft.com/fwlink/p/?LinkId=717245).

Sie können die folgenden Bereitstellungsoptionen auf der Eigenschaftenseite **Debuggen** des Startprojekts festlegen:

- **Lokales Netzwerkloopback zulassen**

  Aus Sicherheitsgründen darf eine UWP-App, die mit der Standardmethode installiert wurde, keine Netzwerkaufrufe an das Gerät senden, auf dem sie installiert ist. Für die bereitgestellte App erstellt die Visual Studio-Bereitstellung standardmäßig eine Ausnahme von dieser Regel. Diese Ausnahme macht es möglich, Kommunikationsverfahren auf einem einzelnen Computer zu testen. Vor der Übermittlung Ihrer-app für den Microsoft Store, sollten Sie die app ohne die Ausnahme testen.

  So entfernen Sie die Netzwerkloopback-Ausnahme aus der App

  - Auf der C# und Visual Basic **Debuggen** auf der Seite Deaktivieren der **lokales netzwerkloopback zulassen** Kontrollkästchen.
  - Legen Sie den Wert **Lokales Netzwerkloopback zulassen** auf der Eigenschaftenseite **Debuggen** für JavaScript und C++ auf **Nein** fest.

- **Eigenen Code zunächst nicht starten, sondern debuggen/Anwendung starten**

  So konfigurieren Sie die Bereitstellung für das automatische Starten einer Debugsitzung beim Starten der App

  - Auf der C# und Visual Basic **Debuggen** auf der Seite auf die **nicht starten sondern Debuggen meinen Code** Kontrollkästchen.
  - Legen Sie den Wert **Anwendung starten** auf der Eigenschaftenseite **Debuggen** für JavaSCript und C++ auf **Ja** fest.

## <a name="symbols"></a>Sonderzeichen

Symboldateien enthalten eine Vielzahl von Daten, die sehr hilfreich beim Debuggen von Code sind (z. B. Variablen, Funktionsnamen und Adressen von Einsprungspunkten). Mit diesen Daten können Sie die Ausnahmen und Ausführungsreihenfolge von Aufruflisten besser überblicken. Über den [Microsoft-Symbolserver](https://msdl.microsoft.com/download/symbols) stehen Symbole für die meisten Windows-Varianten zur Verfügung. Für schnellere Offline-Lookups können Sie jedoch auch unter [Herunterladen von Windows-Symbolpaketen](https://aka.ms/winsymbols) heruntergeladen werden.

Wählen Sie zum Festlegen von Symboloptionen für Visual Studio **Extras > Optionen** aus, und navigieren Sie im Dialogfeld zu **Debuggen > Symbole**.

![Optionen (Dialogfeld)](images/gs-debug-uwp-apps-004.png)

Legen Sie die **Sympath**-Variable auf dem Speicherort des Symbolpakets fest, um die Symbole in einer Debugsitzung mit [WinDbg](#windbg) zu laden. Der folgende Befehl lädt beispielsweise Symbole vom Microsoft-Symbolserver und speichert sie dann im Verzeichnis C:\Symbols zwischen:

```cmd
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

Mit `‘;’` als Trennzeichen oder dem Befehl `.sympath+` können Sie mehrere Pfade hinzufügen. Mehr zu erweiterten Symbolvorgängen mit WinDbg finden Sie unter [Öffentliche und private Symbole](https://msdn.microsoft.com/library/windows/hardware/ff553493).

## <a name="windbg"></a>WinDbg

WinDbg ist ein leistungsstarker Debugger, der als Teil der Debugtools für Windows bereitgestellt wird (Letzteres ist des [Windows SDK](https://go.microsoft.com/fwlink/p/?LinkID=271979)). Die Windows-SDK-Installation ermöglicht die Installation der Debugtools für Windows als eigenständiges Produkt. WinDbg ist beim Debuggen von systemeigenem Code sehr hilfreich. Es wird jedoch nicht für Apps empfohlen, die in verwaltetem Code oder in HTML5 geschrieben wurden.

Um WinDbg mit UWP-Apps zu verwenden, müssen Sie zunächst die Prozesslebensdauer-Verwaltung (Process Lifetime Management, PLM) für Ihr App-Paket mit PLMDebug deaktivieren (siehe [Test- und Debugtools für die Prozesslebensdauer-Verwaltung (PLM)](testing-debugging-plm.md).

```cmd
plmdebug /enableDebug [PackageFullName] ""C:\Program Files\Debugging Tools for Windows (x64)\WinDbg.exe\" -server npipe:pipe=test"
```

Im Gegensatz zu Visual Studio arbeitet der Großteil der Kernfunktionen von WinDbg mit Befehlen im Befehlsfenster. Mit den Befehlen können Sie den Ausführungszustand anzeigen, Benutzermodus-Speicherabbilder prüfen und in verschiedensten Modi debuggen.

Einer der am häufigsten in WinDbg verwendeten Befehle ist `!analyze -v`. Er wird verwendet, um ausführliche Informationen zur aktuellen Ausnahme abzurufen. Diese Informationen umfassen unter anderem:

- FAULTING_IP: Anweisungszeiger zum Zeitpunkt des Fehlers
- EXCEPTION_RECORD: Adresse, Code und Flags der aktuellen Ausnahme
- STACK_TEXT: Stapelüberwachung vor der Ausnahme

Eine vollständige Liste aller WinDbg-Befehle finden Sie unter [Debuggerbefehle](https://msdn.microsoft.com/library/ff540507).

## <a name="related-topics"></a>Verwandte Themen

- [Test- und Debugtools für die Prozesslebensdauer-Verwaltung (PLM)](testing-debugging-plm.md)
- [Debugging, Tests und Leistung](index.md)
