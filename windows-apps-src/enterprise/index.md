---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: Diese Roadmap enthält eine Übersicht über wichtige Unternehmensfeatures für Windows 10-Apps und Apps für die Universelle Windows-Plattform (UWP).
title: Enterprise
ms.date: 01/16/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 0a95529f40ef5bb1cbf112c91c385e6621620a01
ms.sourcegitcommit: 7a8aea567b26283c71420e0d305d78f675e1fba7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125683"
---
# <a name="enterprise"></a>Enterprise

Dieser Artikel enthält eine Übersicht über die wichtigsten Unternehmensfeatures, die von der Universellen Windows-Plattform (UWP) für Windows 10-Apps bereitgestellt werden. Ein Video, in dem einige dieser Features vorgestellt werden, finden Sie unter [Rapidly Construct LOB Applications with UWP and Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502) (Schnelles Erstellen von Branchenanwendungen mit UWP und Visual Studio).

## <a name="feature-highlights"></a>Featurehighlights

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

Windows Template Studio ist eine Visual Studio 2019-Erweiterung, die mit einer assistentenbasierten Oberfläche die Erstellung neuer UWP-Apps (Universelle Windows-Plattform) beschleunigt. Das resultierende UWP-Projekt ist wohlgeformter lesbarer Code, der die neuesten Windows 10-Features enthält und dabei bewährte Muster und bewährte Methoden implementiert.

![Windows Template Studio](images/windows-template-studio.png)

Weitere Informationen finden Sie unter [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio).

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>Steuerelemente zum Erstellen von Benutzeroberflächen im Desktopstil

Wir haben neue UWP-XAML-Steuerelemente veröffentlicht, mit denen die Lücke zwischen der Benutzeroberfläche einer herkömmlichen Desktopanwendung und einer UWP-Benutzeroberfläche gefüllt wird.

Mit den neuen Steuerelementen [MenuBar](/windows/uwp/design/controls-and-patterns/menus), [DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button), [SplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) und [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) können Sie Befehle beispielsweise flexibler verfügbar machen. Mit [EditableComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) können Benutzer Werte eingeben, die nicht in einer vordefinierten Liste mit Optionen aufgeführt sind.

![MenuBar](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>Steuerelemente zur Unterstützung von Unternehmensszenarien

[DataGridView](https://docs.microsoft.com/windows/communitytoolkit/controls/datagrid) ermöglicht das flexible Anzeigen einer Sammlung mit Daten in Zeilen und Spalten.

[TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) ermöglicht eine Hierarchieauflistung mit Knoten, die das Aus- und Einblenden von geschachtelten Elementen erlauben. Das Steuerelement kann verwendet werden, um eine Ordnerstruktur oder geschachtelte Beziehungen zwischen Elementen in der Benutzeroberfläche zu veranschaulichen.

![DataGrid-Steuerelement](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Windows-UI-Bibliothek

Die Windows-UI-Bibliothek umfasst eine Reihe von NuGet-Paketen, mit denen Steuerelemente und andere Benutzeroberflächenelemente für UWP-Apps bereitgestellt werden. Darüber hinaus wird die Abwärtskompatibilität mit früheren Versionen von Windows 10 ermöglicht, damit Ihre App auch funktioniert, wenn Benutzer nicht über das aktuelle Betriebssystem verfügen.

![Windows-UI-Bibliothek](images/win-ui.png)

Informationen hierzu finden Sie unter [Windows UI Library (Preview Version)](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek (Vorschauversion)).

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications-xaml-islands"></a>UWP-Steuerelemente in Desktopanwendungen (XAML-Inseln)

Windows 10 ermöglicht Ihnen jetzt die Verwendung von UWP-Steuerelementen in WPF-, Windows Forms- und C++-Win32-Desktopanwendungen mithilfe eines Features namens *XAML-Inseln*. Dies bedeutet, dass Sie das Aussehen, das Erscheinungsbild und die Funktionalität Ihrer vorhandenen Desktopanwendungen mit den aktuellen Windows 10-Benutzeroberflächenfeatures erweitern können, die nur über UWP-Steuerelemente verfügbar sind, z. B. Windows Ink und Steuerelemente, die das Fluent Design-System unterstützen. Dieses Feature wird als „XAML-Inseln“ bezeichnet.

Informationen hierzu finden Sie unter [UWP-Steuerelemente in Desktopanwendungen](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls).

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

.NET Standard 2.0 enthält über 20.000 APIs mehr als .NET Standard 1.x. Dies ist eine deutliche Vereinfachung beim Migrieren von vorhandenen .NET Framework-Bibliotheken und ihrer anschließenden Nutzung für verschiedene .NET-Anwendungen, z. B. Ihre UWP-Anwendung.

![net-standard](images/dot-net-standard-project-template.png)

Weitere Informationen finden Sie unter [Teilen von Code zwischen einer Desktop-App und einer UWP-App](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>SQL Server-Konnektivität

Ihre App kann sich direkt mit einer SQL Server-Datenbank verbinden und dann Daten über Klassen im Namespace [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient) speichern und abrufen.

Informationen hierzu finden Sie unter [Verwenden einer SQL Server-Datenbank in einer UWP-App](https://docs.microsoft.com/windows/uwp/data-access/sql-server-databases).

<a id="MSIX" />

### <a name="msix-deployment"></a>MSIX-Bereitstellung

MSIX ist ein Paketformat für Windows-Apps, das die besten Features von MSI, AppX, App-V und ClickOnce kombiniert, um eine moderne und zuverlässige Paketerstellung für alle Windows-Apps zu ermöglichen. Beim MSIX-Paket wird die Funktionalität von vorhandenen App-Paketen und Installationsdateien beibehalten, und es werden moderne Verpackungs- und Bereitstellungsfeatures für Win32-, WPF- und Windows Forms-Apps ermöglicht. 

![Symbol „MSIX“](images/MSIX-App-Package.ico)

Informationen hierzu finden Sie in der [MSIX-Dokumentation](https://docs.microsoft.com/windows/msix/).

<a id="distribution" />

## <a name="security"></a>Sicherheit

Windows 10 umfasst eine Suite mit Sicherheitsfeatures für App-Entwickler zum Schutz der Identität der Benutzer, der Sicherheit von Unternehmensnetzwerken und auf Geräten gespeicherten Unternehmensdaten. Neu bei Windows 10 ist Microsoft Passport, eine einfach bereitzustellende, alternative zweistufige Authentifizierungsmethode, die über eine PIN oder Windows Hello zugänglich ist. Dies sorgt für Sicherheit im Unternehmen und unterstützt eine Authentifizierung auf Basis von Fingerabdrücken, Gesichtserkennung und Irisscans.

| Thema | Beschreibung |
|-------|-------------|
| [Einführung in die Entwicklung sicherer Windows-Apps](https://docs.microsoft.com/windows/uwp/security/intro-to-secure-windows-app-development) | Dieser einführende Artikel erläutert verschiedene Windows-Sicherheitsfeatures, die auf verschiedenen Stufen verfügbar sind, d. h. Authentifizierung, In-Flight-Daten und At-Rest-Daten. Außerdem wird hier beschrieben, wie Sie diese Stufen in Ihren Apps integrieren können. Er umfasst eine Vielzahl an Themen und enthält in erster Linie weitere Informationen für App-Architekten zu den Windows-Features, die die Entwicklung von Universellen Windows-Plattform-Apps beschleunigen. |
| [Authentifizierung und Benutzeridentität](https://docs.microsoft.com/windows/uwp/security/authentication-and-user-identity) | UWP-Apps verfügen über verschiedene Optionen zur Benutzerauthentifizierung, die in diesem Artikel beschrieben werden. Für Unternehmen wird dringend das neue Microsoft Passport-Feature empfohlen. Microsoft Passport ersetzt Kennwörter mit der sicheren zweistufigen Authentifizierung (Two-Factor Authentication, 2FA), indem vorhandene Anmeldeinformationen überprüft und gerätespezifische Anmeldeinformationen erstellt werden, die eine Benutzergeste (entweder Biometrie- oder PIN-basiert) schützen, und schafft so eine bequeme und sichere Umgebung. |
| [Kryptografie](https://docs.microsoft.com/windows/uwp/security/cryptography) | Der Abschnitt „Kryptografie“ bietet eine Übersicht über die für UWP-Apps verfügbaren Kryptografie-Features. Die Artikel reichen von einführenden exemplarischen Vorgehensweisen zum einfachen Verschlüsseln sensibler Daten bis hin zu erweiterten Themen, z.B. Bearbeiten von kryptografischen Schlüsseln und Arbeiten mit MACs, Hashes und Signaturen. |
| [Windows Information Protection (WIP)](wip-hub.md) | Dies ist ein Übersichtsthema mit umfassenden Informationen für Entwickler zum Zusammenhang zwischen der Windows Information Protection (WIP) und Dateien, Puffern, der Zwischenablage, dem Netzwerk, Hintergrundaufgaben und dem Schutz von Daten bei Sperre. |

## <a name="data-binding-and-databases"></a>Datenbindung und Datenbanken

Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten aus externen Quellen wie Datenbanken anzeigen und diese Daten optional synchronisieren kann. Mit der Datenbindung können Sie Datenaspekte von Benutzeroberflächenaspekten trennen, was zu einem einfacheren konzeptionellen Modell und besserer Lesbarkeit, Testbarkeit und Wartung Ihrer App führt.

| Thema | Beschreibung |
|-------|-------------|
| [Übersicht über Datenbindung](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-quickstart) | In diesem Thema erfahren Sie, wie Sie in einer UWP-App (Universelle Windows-Plattform) ein Steuerelement (oder ein anderes Benutzeroberflächenelement) an ein einzelnes Element oder ein Elementsteuerelement an eine Sammlung von Elementen binden. Darüber hinaus wird erläutert, wie Sie die Anzeige von Elementen steuern, eine Detailansicht auf Grundlage einer Auswahl implementieren und Daten für die Anzeige umwandeln. |
| [Entity Framework 7 für UWP](https://docs.microsoft.com/windows/uwp/data-access/entity-framework-7-with-sqlite-for-csharp-apps) | Das Durchführen komplexer Abfragen für große Datensätze ist mit Entity Framework 7, mit Unterstützung für UWP, wesentlich einfacher. In dieser exemplarischen Vorgehensweise wird eine UWP-App erstellt, die auf grundlegende Daten der lokalen SQLite-Datenbank mithilfe von Entity Framework zugreift. |
| [Lokale SQLite-Datenbank](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Dieses Video enthält ein umfassendes Entwicklerhandbuch zur Verwendung von SQLite, der empfohlenen Lösung für lokale App-Datenbanken. Laden Sie die neueste Version für UWP unter [SQLite](https://www.sqlite.org/download.html) herunter oder verwenden Sie die Version, die im Lieferumfang des Windows 10 SDK enthalten ist. |

## <a name="networking-and-data-serialization"></a>Netzwerke und Datenserialisierung

Branchenspezifische Apps müssen häufig mit Daten auf einer Vielzahl von anderen Systemen kommunizieren oder diese speichern. Dies erfolgt in der Regel durch Herstellen einer Verbindung mit einem Netzwerkdienst (mithilfe von Protokollen wie REST oder SOAP) und durch anschließendes Serialisieren bzw. Deserialisieren von Daten in ein gemeinsames Format. Die Arbeit mit Netzwerken und die Datenserialisierung in UWP-Apps sind mit WPF-, WinForms- und ASP.NET-Anwendungen vergleichbar. Weitere Informationen finden Sie in den folgenden Artikeln.

| Thema | Beschreibung |
|-------|-------------|
| [Grundlagen zum Netzwerk](https://docs.microsoft.com/windows/uwp/networking/networking-basics) | In dieser exemplarischen Vorgehensweise werden grundlegende, für alle UWP-Apps relevante Netzwerkkonzepte erläutert, unabhängig von den verwendeten Kommunikationsprotokollen.  |
| [Welche Netzwerktechnologie?](https://docs.microsoft.com/windows/uwp/networking/which-networking-technology) | Eine kurze Übersicht über die Netzwerktechnologien, die für UWP-Apps zur Verfügung stehen, mit Vorschlägen zum Auswählen der Technologien, die für Ihre App am besten geeignet sind. |
| [XML- und SOAP-Serialisierung](https://docs.microsoft.com/dotnet/framework/serialization/xml-and-soap-serialization) | Bei der XML-Serialisierung werden Objekte in einen XML-Datenstrom konvertiert, der einer bestimmten Sprache der XML-Schemadefinition (XSD) entspricht. Sie können zum Konvertieren zwischen XML und einer stark typisierten Klasse die systemeigene [XDocument](https://docs.microsoft.com/dotnet/api/system.xml.linq.xdocument)-Klasse oder eine externe Bibliothek verwenden. |
| [JSON-Serialisierung](https://docs.microsoft.com/uwp/api/Windows.Data.Json) | Die JSON-Serialisierung (JavaScript Object Notation) ist ein gängiges Format für die Kommunikation mit REST-APIs. [JSON.NET von Newtonsoft](https://www.newtonsoft.com/json) wird vollständig für UWP-Apps unterstützt. |

## <a name="devices"></a>Geräte

Für die Integration in branchenspezifischen Tools, z.B. Drucker, Strichcodescanner oder Smartcardleser, ist es möglicherweise erforderlich, externe Geräte oder Sensoren in Ihrer App zu integrieren. Hier folgen einige Beispiele zu Features, die Sie mithilfe der in diesem Abschnitt beschriebenen Technologie zur App hinzufügen können.

| Thema  | Beschreibung |
|--------|-------------|
| [Auflisten von Geräten](https://docs.microsoft.com/windows/uwp/devices-sensors/enumerate-devices) | In diesem Artikel wird erläutert, wie mit dem [Windows.Devices.Enumeration](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration)-Namespace nach Geräten gesucht werden kann, die intern mit dem System verbunden, extern verbunden oder über Drahtlos- oder Netzwerkprotokolle entdeckt werden können. Beginnen Sie mit diesem Artikel, wenn Sie eine App erstellen, die mit Geräten arbeitet. |
| [Drucken und Scannen](https://docs.microsoft.com/windows/uwp/devices-sensors/printing-and-scanning) | Beschreibt das Drucken und Scannen von Ihrer App aus, z.B. Herstellen einer Verbindung und Arbeiten mit Unternehmensgeräten wie POS-Systemen (Point-of-Sale), Belegdruckern und Einzugsscannern mit hoher Kapazität. |
| [Bluetooth](https://docs.microsoft.com/windows/uwp/devices-sensors/bluetooth) | Neben herkömmlichen Bluetooth-Verbindungen zum Senden und Empfangen von Daten oder Steuern von Geräten kann unter Windows 10 Bluetooth Low Energy (BTLE) zum Senden oder Empfangen von Beacons im Hintergrund verwendet werden. Verwenden Sie diese zum Anzeigen von Benachrichtigungen oder Aktivieren von Funktionen, wenn sich ein Benutzer in der Nähe eines bestimmten Orts befindet oder diesen verlässt. |
| [Im Unternehmen freigegebener Speicher](enterprise-shared-storage.md) | Erfahren Sie, wie Daten in Gerätesperrszenarien innerhalb derselben App zwischen App-Instanzen oder zwischen Apps freigegeben werden können. |

## <a name="device-targeting"></a>Ausrichten an Geräte

Viele Benutzer bringen in der heutigen Zeit ihre eigenen Telefone oder Tablets zur Arbeit mit, die unterschiedliche Formfaktoren und Bildschirmgrößen aufweisen. Mit der Universellen Windows-Plattform (UWP) können Sie branchenspezifische Apps entwickeln, die problemlos auf allen Arten von Geräten ausgeführt werden können, u.a. Desktop-PCs und PPI-Displays, und können so die Reichweite Ihrer Apps und die Effizienz des Codes maximieren.

| Thema | Beschreibung |
|-------|-------------|
| [Anleitung für UWP-Apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) | In dieser Anleitung können Sie sich mit der UWP-Plattform unter Windows 10 vertraut machen. Sie erhalten u.a. Informationen dazu, was eine Gerätefamilie ist, wie Sie entscheiden, auf welche Ihre Apps abzielen sollen, Informationen zu neuen UI-Steuerelementen und Bereichen, mit denen Sie Ihre Benutzeroberfläche für verschiedene Geräte-Formfaktoren anpassen können, sowie zur für Ihre App verfügbaren API-Oberfläche und wie Sie diese steuern können. |
| [Adaptives XAML-UI-Codebeispiel](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) | Dieses Codebeispiel zeigt die möglichen Layoutoptionen und Steuerelemente für Ihre App, unabhängig von der Art des Geräts, und veranschaulicht eine Interaktion mit den Bereichen, um das gewünschte Layout zu erzielen. Neben der Reaktion der Steuerelemente und der App selbst auf verschiedene Formfaktoren, werden die verschiedenen Methoden zum Erzielen einer adaptiven Benutzeroberfläche aufgezeigt. |
| [Xamarin-Thema](/xamarin/) | Xamarin für Telefone |

## <a name="deployment"></a>Bereitstellung

Es stehen verschiedene Optionen für die Verteilung von Apps für die Benutzer in Ihrer Organisation mithilfe von MSIX-Paketen zur Verfügung. Sie können eine auf dem App-Installer basierende Bereitstellung konfigurieren, Geräteverwaltungstools wie Microsoft Endpoint Configuration Manager und Microsoft Intune verwenden, im Microsoft Store für Unternehmen veröffentlichen oder Apps Geräte querladen. Sie können Ihre Apps auch der Öffentlichkeit zur Verfügung stellen, indem Sie sie im Microsoft Store veröffentlichen.

| Thema | Beschreibung |
|-------|-------------|
| [MSIX-Dokumentation](https://docs.microsoft.com/windows/msix/) | MSIX ist ein Paketformat für Windows-Apps, das die besten Features von MSI, AppX, App-V und ClickOnce kombiniert, um eine moderne und zuverlässige Paketerstellung zu ermöglichen. |
| [Verteilen von branchenspezifischen Apps an Unternehmen](https://docs.microsoft.com/windows/uwp/publish/distribute-lob-apps-to-enterprises) | Informieren Sie sich über die Optionen für die Verteilung von Branchenanwendungen, ohne die Apps der Öffentlichkeit auf breiter Basis zugänglich zu machen, z. B. der auf App-Installer basierenden Bereitstellung, Microsoft Endpoint Configuration Manager und Microsoft Intune sowie der Veröffentlichung im Microsoft Store für Unternehmen. |
| [Querladen von Apps](https://docs.microsoft.com/windows/deploy/sideload-apps-in-windows-10) | Wenn Sie eine App querladen, stellen Sie ein signiertes App-Paket auf einem Gerät bereit. Das Signieren, Hosten und Bereitstellen dieser Apps wird beibehalten. Der Prozess zum Querladen von Apps ist für Windows 10 optimiert.             |
| [Veröffentlichen von Apps im Microsoft Store](https://developer.microsoft.com/store/publish-apps) | Im einheitlichen Microsoft Store können Sie Ihre gesamten Apps für alle Windows-Geräte verwalten und veröffentlichen. Passen Sie die Verfügbarkeit Ihrer App mit marktspezifischen Preisen, Steuerelementen für Verteilung und Sichtbarkeit und weiteren Optionen an. |

## <a name="enterprise-uwp-samples"></a>Beispiele für UWP-Apps für Unternehmen

| Thema |  Beschreibung |
|------ |--------------|
| [VanArsdel-Bestandsbeispiel](https://github.com/Microsoft/InventorySample) | Eine UWP-Beispiel-App, mit der Branchenszenarien veranschaulicht werden. Das Beispiel basiert auf dem Erstellen und Verwalten von Kunden, Bestellungen und Produkten für das fiktive Unternehmen VanArsdel. |
| [Beispieldatenbank für Kundenbestellung](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Eine UWP-Beispiel-App, mit der hilfreiche Features für Entwickler in Unternehmen veranschaulicht werden, z. B. AAD-Authentifizierung (Azure Active Directory), Benutzeroberflächen-Steuerelemente (inklusive des Datenrasters), Sqlite- und SQL Azure-Datenbankintegration, Entity Framework und API-Clouddienste. Das Beispiel basiert auf dem Erstellen und Verwalten von Kundenkonten, Bestellungen und Produkten für das fiktive Unternehmen Contoso. |

## <a name="patterns-and-practices"></a>Muster und Methoden

Eine Codebasis für umfangreiche Apps im Unternehmen kann schwer zu handhaben sein. Prism ist ein Framework zum Erstellen von lose gekoppelten, verwaltbaren und testbaren XAML-Anwendungen in WPF, der Universellen Windows-Plattform unter Windows 10 und in Xamarin Forms. Prism stellt eine Implementierung einer Sammlung von Entwurfsmustern bereit, die beim Schreiben von gut strukturierten und verwaltbaren XAML-Anwendungen hilfreich sind, u.a. MVVM, Einfügen von Abhängigkeiten, Befehle und EventAggregator.

Weitere Informationen zu Prism finden Sie im [GitHub-Repository](https://github.com/PrismLibrary/Prism).

 

 
