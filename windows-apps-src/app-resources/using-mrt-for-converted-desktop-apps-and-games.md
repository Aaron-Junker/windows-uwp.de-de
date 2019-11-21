---
title: Verwenden von MRT für konvertierte Desktop-Apps und -Spiele
description: Indem Sie Ihre .NET- oder Win32-App oder Ihr Spiel als AppX-Paket verpacken, können Sie das Ressourcenverwaltungssystem nutzen, um App-Ressourcen zu laden, die auf den Laufzeitkontext zugeschnitten sind. In diesem Thema werden die Techniken detailliert beschrieben.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP, mrt, pri. Ressourcen, Spiele, Centennial, Desktop App Converter, mui, Satellitenassembly
ms.localizationpriority: medium
ms.openlocfilehash: 3367cfafb2f3a8e307fd26dc6d6c19f1ece0d17e
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254752"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Verwenden des Ressourcenverwaltungssystem für Windows 10 in älteren Apps oder Spielen

Apps und Spiele für .NET und Win32 werden häufig in verschiedene Sprachen übersetzt, um die Anzahl potenzieller Märkte zu vergrößern. Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md). By packaging your .NET or Win32 app or game as an MSIX or AppX package, you can leverage the Resource Management System to load app resources tailored to the run-time context. In diesem Thema werden die Techniken detailliert beschrieben.

Es gibt viele Möglichkeiten zum Lokalisieren einer herkömmlichen Win32-Anwendung, aber unter Windows 8 wurde ein neues [Ressourcenverwaltungssystem](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) eingeführt, das sich für alle Programmiersprachen und alle Anwendungstypen eignet und Funktionalität für mehr als eine einfache Lokalisierung bereitstellt. Dieses System wird im vorliegenden Artikel als „MRT” bezeichnet. Früher bedeutete dies „Modern Resource Technology“. Der Bestandteil „Modern“ wird jedoch nicht mehr verwendet. Der Ressourcen-Manager ist möglicherweise auch unter den Namen MRM (Modern Resource Manager, moderner Ressourcen-Manager) oder PRI (Package Resource Index, Paketressourcenindex) bekannt.

Combined with MSIX-based or AppX-based deployment (for example, from the Microsoft Store), MRT can automatically deliver the most-applicable resources for a given user / device which minimizes the download and install size of your application. Die Größenreduzierung kann bei Anwendungen mit einer großen Menge an lokalisiertem Inhalt erheblich sein, möglicherweise in einer Größenordnung von mehreren *Gigabytes* für AAA-Spiele. Weitere Vorteile von MRT sind lokalisierte Angebote in der Windows-Shell und im Microsoft Store sowie eine automatische Fallback-Logik für den Fall, dass die bevorzugte Sprache eines Benutzers nicht den verfügbaren Ressourcen entspricht.

Dieses Dokument beschreibt die allgemeine MRT-Architektur und stellt ein Handbuch für das Portieren bereit, sodass ältere Win32-Anwendungen mit minimalen Codeänderungen zu MRT verschoben werden können. Wenn der Wechsel zu MRT erfolgt sind, stehen Entwicklern weitere Vorteile zur Verfügung (z. B. die Möglichkeit, Ressourcen nach Skalierungsfaktor oder Systemdesign zu segmentieren). Beachten Sie, dass die MRT-basierte Lokalisierung sowohl für UWP-Anwendungen als auch für Win32-Anwendungen funktioniert, die von der Desktop-Brücke verarbeitet werden (auch bekannt als „Centennial”).

In vielen Fällen können Sie Ihre vorhandenen Lokalisierungsformate und Ihren Quellcode auch nach der Integration mit MRT noch verwenden, um Ressourcen zur Laufzeit aufzulösen und Downloadgrößen zu verringern – es ist kein „Ganz-oder-gar-nicht-Ansatz”. Die folgende Tabelle fasst die Aufgabe und die geschätzten Kosten bzw. den Nutzen jeder Phase zusammen. Diese Tabelle enthält nur Aufgaben, welche die Lokalisierung betreffen, also keine Aufgaben wie z. B. das Bereitstellen von Anwendungssymbolen mit hoher Auflösung oder hohem Kontrast. Weitere Informationen zum Bereitstellen von mehreren Ressourcen für Kacheln, Symbole usw. finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Beruflich</th>
<th>Vorteil</th>
<th>Geschätzte Kosten</th>
</tr>
<tr>
<td>Localize package manifest</td>
<td>Kaum Arbeitsaufwand erforderlich, damit der lokalisierte Inhalt in der Windows-Shell und im Microsoft Store angezeigt wird</td>
<td>Gering</td>
</tr>
<tr>
<td>Verwenden von MRT zum Erkennen und Suchen von Ressourcen</td>
<td>Voraussetzung für die Verringerung der Download- und Installationsgrößen; automatischer Fallback für Sprachen</td>
<td>Mittel</td>
</tr>
<tr>
<td>Erstellen von Ressourcenpaketen</td>
<td>Letzter Schritt für die Verringerung der Download- und Installationsgrößen</td>
<td>Gering</td>
</tr>
<tr>
<td>Migrieren von MRT-Ressourcenformaten und APIs</td>
<td>Erheblich geringere Dateigrößen (je nach vorhandener Ressourcentechnologie)</td>
<td>Groß</td>
</tr>
</table>

## <a name="introduction"></a>Einführung

Die meisten umfassenden Anwendungen enthalten Benutzeroberflächenelemente, auch als *Ressourcen* bezeichnet, die von dem Code der Anwendung entkoppelt werden (im Gegensatz zu *hartcodierten Werte*, die im Quellcode selbst verfasst werden). Es gibt verschiedene Gründe dafür, Ressourcen gegenüber hartcodierten Werten zu bevorzugen – wie beispielsweise die einfache Bearbeitung durch Nicht-Entwickler –, einer der wichtigsten Gründe ist jedoch, dass die App auf diese Weise verschiedene Darstellungen derselben logischen Ressource zur Laufzeit auswählen kann. Beispielsweise unterscheidet sich der auf einer Schaltfläche anzuzeigende Text (oder das in einem Symbol anzuzeigende Bild) abhängig von der/den Sprache(n), die ein Benutzer versteht, den Eigenschaften des Anzeigegeräts oder davon, ob eine Hilfstechnologie aktiviert ist.

Daher besteht der Hauptzweck von Ressourcenverwaltungstechnologien darin, zur Laufzeit eine Anfrage für einen logischen oder symbolischen *Ressourcennamen* (z. B. `SAVE_BUTTON_LABEL`) aus einer Reihe von möglichen *Kandidaten* (z. B. „Save”, „Speichern” oder „저장”) in den bestmöglichen tatsächlichen *Wert* (z. B. „Speichern”) zu übersetzen. MRT stellt eine solche Funktion bereit und ermöglicht es Anwendungen, Ressourcenkandidaten mithilfe von verschiedenen Attributen, den *Qualifizierern*, wie beispielsweise der Sprache des Benutzers, dem Skalierungsfaktor des Displays, dem ausgewählten Design des Benutzers und anderen Umgebungsfaktoren, zu identifizieren. MRT unterstützt auch benutzerdefinierte Qualifizierer für Anwendungen, die diese benötigen (beispielsweise könnte eine Anwendung für Benutzer, die sich mit einem Konto angemeldet haben, und für Gastbenutzer unterschiedliche grafische Ressourcen bereitstellen, ohne dass diese Prüfung explizit zu jedem Teil der Anwendung hinzugefügt wird). MRT funktioniert sowohl mit Zeichenfolgenressourcen als auch mit dateibasierten Ressourcen, wenn dateibasierte Ressourcen als Verweise auf die externen Daten (die Dateien selbst) implementiert werden.

### <a name="example"></a>Beispiel

Dies ist ein einfaches Beispiel für eine Anwendung mit Beschriftungen auf zwei Schaltflächen (`openButton` und `saveButton`) und einer PNG-Datei, die für ein Logo verwendet wird (`logoImage`). Die Beschriftungen werden ins Englische und ins Deutsche übersetzt, und das Logo ist für normale Desktopdisplays (Skalierungsfaktor 100 %) und hochauflösende Smartphones (Skalierungsfaktor 300 %) optimiert. Beachten Sie, dass dieses Diagramm eine allgemeine konzeptionelle Ansicht des Modells darstellt. Es entspricht nicht genau der Implementierung.

<p><img src="images\conceptual-resource-model.png"/></p>

In der Grafik verweist der Anwendungscode auf die drei logischen Ressourcennamen. Zur Laufzeit verwendet die Pseudo-Funktion `GetResource` MRT, um diese Ressourcennamen in der Ressourcentabelle (als PRI-Datei bezeichnet) zu suchen und basierend auf den Umgebungsbedingungen (der Sprache des Benutzers und dem Skalierungsfaktor des Displays) den am besten geeigneten Kandidaten zu finden. Bei Beschriftungen werden die Zeichenfolgen direkt verwendet. Beim Logobild werden die Zeichenfolgen als Dateinamen interpretiert, und die Dateien werden von der Festplatte gelesen. 

If the user speaks a language other than English or German, or has a display scale-factor other than 100% or 300%, MRT picks the "closest" matching candidate based on a set of fallback rules (see [Resource Management System](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) for more background).

Beachten Sie, dass MRT Ressourcen unterstützt, die auf mehrere Qualifizierer zugeschnitten sind – wenn beispielsweise das Logobild eingebetteten Text enthält, der auch lokalisiert werden muss, würde das Logo über vier Kandidaten verfügen: EN/Scale-100, DE/Scale-100, EN/Scale-300 and DE/Scale-300.

### <a name="sections-in-this-document"></a>Abschnitte in diesem Dokument

In den folgenden Abschnitten werden die allgemeinen Aufgaben dargestellt, die für die Integration von MRT in Ihre Anwendung erforderlich sind.

#### <a name="phase-0-build-an-application-package"></a>Phase 0: Erstellen eines Anwendungspakets

In diesem Abschnitt wird beschrieben, wie Sie Ihre vorhandene Desktopanwendung zu einem Anwendungspaket machen. In dieser Phase werden keine MRT-Features verwendet.

#### <a name="phase-1-localize-the-application-manifest"></a>Phase 1: Lokalisieren des Anwendungsmanifests

In diesem Abschnitt wird beschrieben, wie Sie das Manifest Ihrer Anwendung lokalisieren (damit es richtig in der Windows-Shell angezeigt wird), während noch Ihr älteres Ressourcenformat und Ihre älteren APIs verwendet werden, um Ressourcen zu packen und zu suchen. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2: Verwenden von MRT, um Ressourcen zu identifizieren und zu suchen

In diesem Abschnitt wird beschrieben, wie Sie Ihren Anwendungscode (und gegebenenfalls das Ressourcenlayout) so ändern, dass eine Suche von Ressourcen mithilfe von MRT möglich ist, während noch Ihre vorhandenen Ressourcenformate und Ihre älteren APIs verwendet werden, um die Ressourcen zu laden und zu verwenden. 

#### <a name="phase-3-build-resource-packs"></a>Phase 3: Erstellen von Ressourcenpaketen

In diesem Abschnitt werden die endgültigen Änderungen beschrieben, die erforderlich sind, um Ihre Ressourcen in separate *Ressourcenpakete* zu teilen, wodurch die Größe des Downloads (und der Installation) Ihrer App verringert wird.

### <a name="not-covered-in-this-document"></a>In diesem Dokument nicht behandelt

After completing Phases 0-3 above, you will have an application "bundle" that can be submitted to the Microsoft Store and that will minimize the download and install size for users by omitting the resources they don't need (eg, languages they don't speak). Weitere Verbesserungen hinsichtlich Größe der Anwendung und Funktionen können über einen letzten Schritt vorgenommen werden.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>Phase 4: Migrieren von MRT-Ressourcenformaten und APIs

Diese Phase kann in diesem Dokument nicht behandelt werden. Sie umfasst das Übertragen Ihrer Ressourcen (insbesondere Zeichenfolgen) von älteren Formate wie beispielsweise MUI DLLs oder.NET-Ressourcenassemblys in PRI-Dateien. Dies kann zu einer weiteren Platzeinsparung hinsichtlich der Größen für den Download und die Installation führen. Es ermöglicht darüber hinaus die Verwendung weiterer MRT-Features, z. B. das Verringern der Größen für den Download und die Installation von Bilddateien auf Grundlage eines Skalierungsfaktors, Barrierefreiheiteinstellungen usw.

## <a name="phase-0-build-an-application-package"></a>Phase 0: Erstellen eines Anwendungspakets

Bevor Sie Änderungen an den Ressourcen Ihrer Anwendung vornehmen, müssen Sie zunächst Ihre aktuelle Paketerstellungs- und Installationstechnologie durch die standardmäßige UWP-Paketerstellungs und Bereitstellungstechnologie ersetzen. Hierfür gibt es drei Möglichkeiten:

* If you have a large desktop application with a complex installer or you utilize lots of OS extensibility points, you can use the Desktop App Converter tool to generate the UWP file layout and manifest information from your existing app installer (for example, an MSI).
* If you have a smaller desktop application with relatively few files or a simple installer and no extensibility hooks, you can create the file layout and manifest information manually.
* If you're rebuilding from source and want to update your app to be a pure UWP application, you can create a new project in Visual Studio and rely on the IDE to do much of the work for you.

If you want to use the [Desktop App Converter](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw), see [Package a desktop application using the Desktop App Converter](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter) for more information on the conversion process. A complete set of Desktop Converter samples can be found on [the Desktop Bridge to UWP samples GitHub repo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

If you want to manually create the package, you will need to create a directory structure that includes all your application's files (executables and content, but not source code) and a package manifest file (.appxmanifest). An example can be found in [the Hello, World GitHub sample](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml), but a basic package manifest file that runs the desktop executable named `ContosoDemo.exe` is as follows, where the <span style="background-color: yellow">highlighted text</span> would be replaced by your own values.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap">
    <Identity Name="Contoso.Demo"
              Publisher="CN=Contoso.Demo"
              Version="1.0.0.0" />
    <Properties>
    <DisplayName>Contoso App</DisplayName>
    <PublisherDisplayName>Contoso, Inc</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>
    <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
    <Resources>
    <Resource Language="en-US" />
  </Resources>
    <Applications>
    <Application Id="ContosoDemo" Executable="ContosoDemo.exe" 
                 EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements DisplayName="Contoso Demo" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="Contoso Demo">
      </uap:VisualElements>
    </Application>
  </Applications>
    <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>
```

For more information about the package manifest file and package layout, see [App package manifest](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest).

Finally, if you're using Visual Studio to create a new project and migrate your existing code across, see [Create a "Hello, world" app](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal). You can include your existing code into the new project, but you will likely have to make significant code changes (particularly in the user interface) in order to run as a pure UWP app. Diese Änderungen werden in diesem Dokument nicht behandelt.

## <a name="phase-1-localize-the-manifest"></a>Phase 1: Localize the manifest

### <a name="step-11-update-strings--assets-in-the-manifest"></a>Step 1.1: Update strings & assets in the manifest

In Phase 0 you created a basic package manifest (.appxmanifest) file for your application (based on values provided to the converter, extracted from the MSI, or manually entered into the manifest) but it will not contain localized information, nor will it support additional features like high-resolution Start tile assets, etc.

To ensure your application's name and description are correctly localized, you must define some resources in a set of resource files, and update the package manifest to reference them.

#### <a name="creating-a-default-resource-file"></a>Erstellen eine Standardressourcendatei

Der erste Schritt ist das Erstellen einer Standardressourcendatei in der Standardsprache (z. B. Englisch (USA)). Sie können dies entweder manuell mit einem Text-Editor oder über den Ressourcen-Designer in Visual Studio vornehmen.

Wenn Sie die Ressourcen manuell erstellen möchten:

1. Erstellen Sie eine XML-Datei mit dem Namen `resources.resw`, und legen Sie sie in einem `Strings\en-us`-Unterordner Ihres Projekts ab. Use the appropriate BCP-47 code if your default language is not US English.
2. Fügen Sie in der XML-Datei den folgenden Inhalt hinzu. Ersetzten Sie dabei den <span style="background-color: yellow">hervorgehobenen Text</span> durch den entsprechenden Text in der Standardsprache Ihrer App.

> [!NOTE]
> There are restrictions on the lengths of some of these strings. Weitere Informationen finden Sie unter [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (English)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (English)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (English)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, USA</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (EN)</value>
  </data>
</root>
```

Wenn Sie den Designer in Visual Studio verwenden möchten:

1. Create the `Strings\en-us` folder (or other language as appropriate) in your project and add a **New Item** to the root folder of your project, using the default name of `resources.resw`. Be sure to choose **Resources File (.resw)** and not **Resource Dictionary** - a Resource Dictionary is a file used by XAML applications.
2. Geben Sie im Designer die folgenden Zeichenfolgen ein (verwenden Sie dieselben `Names`, ersetzen Sie die `Values` jedoch mit dem entsprechenden Text für Ihre Anwendung):

<img src="images\editing-resources-resw.png"/>

> [!NOTE]
> If you start with the Visual Studio designer, you can always edit the XML directly by pressing `F7`. Wenn Sie jedoch mit einer kleinen XML-Datei starten, *erkennt der Designer die Datei nicht*, weil viele zusätzliche Metadaten fehlen. Dies lässt sich durch Kopieren der XSD-Textbausteininformationen aus einer mit dem Designer erstellten Datei in die manuell bearbeitete XML-Datei beheben.

#### <a name="update-the-manifest-to-reference-the-resources"></a>Aktualisieren des Manifests, um auf die Ressourcen zu verweisen

After you have the values defined in the `.resw` file, the next step is to update the manifest to reference the resource strings. Auch in diesem Fall können Sie eine XML-Datei direkt bearbeiten oder dies durch den Manifest-Designer von Visual Studio vornehmen lassen.

Wenn Sie XML-Code direkt bearbeiten, öffnen Sie die Datei `AppxManifest.xml`, und nehmen Sie die folgenden Änderungen an den <span style="background-color: lightgreen">hervorgehoben Werten</span> vor – verwenden Sie *genau* diesen Text und keinen für Ihre Anwendung spezifischen. Es besteht keine Notwendigkeit, genau diese Ressourcennamen zu verwenden.&mdash;Sie können eigene Ressourcennamen verwenden&mdash;, die jedoch genau mit den Angaben in der Datei `.resw` übereinstimmen müssen. Diese Namen sollten den `Names` entsprechen, die Sie in der `.resw`-Datei erstellt haben, und als Präfix das `ms-resource:`-Schema und den `Resources/`-Namespace aufweisen. 

> [!NOTE]
> Many elements of the manifest have been omitted from this snippet - do not delete anything!

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package>
  <Properties>
    <DisplayName>ms-resource:Resources/PackageDisplayName</DisplayName>
    <PublisherDisplayName>ms-resource:Resources/PublisherDisplayName</PublisherDisplayName>
  </Properties>
  <Applications>
    <Application>
      <uap:VisualElements DisplayName="ms-resource:Resources/ApplicationDisplayName"
        Description="ms-resource:Resources/ApplicationDescription">
        <uap:DefaultTile ShortName="ms-resource:Resources/TileShortName">
          <uap:ShowNameOnTiles>
            <uap:ShowOn Tile="square150x150Logo" />
          </uap:ShowNameOnTiles>
        </uap:DefaultTile>
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

If you are using the Visual Studio manifest designer, open the .appxmanifest file and change the <span style="background-color: lightgreen">highlighted values</span> values in the **Application* tab and the *Packaging* tab:

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>Step 1.2: Build PRI file, make an MSIX package, and verify it's working

Sie sollten jetzt in der Lage sein, die `.pri`-Datei zu erstellen und die Anwendung bereitzustellen, um sicherzustellen, dass die richtigen Informationen (in Ihrer Standardsprache) im Startmenü angezeigt werden.

Wenn Sie in Visual Studio erstellen, drücken Sie einfach `Ctrl+Shift+B`, um das Projekt zu erstellen, und klicken Sie dann mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü `Deploy` aus.

If you're building manually, follow these steps to create a configuration file for `MakePRI` tool and to generate the `.pri` file itself (more information can be found in [Manual app packaging](/windows/msix/package/manual-packaging-root)):

1. Open a developer command prompt from the **Visual Studio 2017** or **Visual Studio 2019** folder in the Start menu.
2. Switch to the project root directory (the one that contains the .appxmanifest file and the **Strings** folder).
3. Geben Sie den folgenden Befehl ein, und ersetzen Sie dabei „contoso_demo.xml” mit einem für Ihr Projekt geeigneten Namen und „en-US” mit der Standardsprache Ihrer App (oder lassen Sie es nach Bedarf auf en-US). Note the XML file is created in the parent directory (**not** in the project directory) since it's not part of the application (you can choose any other directory you want, but be sure to substitute that in future commands).

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    Sie können `makepri createconfig /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/cf` sets the Configuration Filename (the output of this command)
      * `/dq` sets the Default Qualifiers, in this case the language `en-US`
      * `/pv` sets the Platform Version, in this case Windows 10
      * `/o` sets it to Overwrite the output file if it exists

4. Sie verfügen jetzt über eine Konfigurationsdatei. Führen Sie `MakePRI` erneut aus, um auf der Festplatte tatsächlich nach Ressourcen zu suchen und diese in einer PRI-Datei zu verpacken. Ersetzen Sie „contoso_demop.xml” mit dem XML-Dateinamen, den Sie im vorherigen Schritt verwendet haben, und legen Sie das übergeordnete Verzeichnis für Eingabe und Ausgabe fest: 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    Sie können `makepri new /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/pr` sets the Project Root (in this case, the current directory)
      * `/cf` sets the Configuration Filename, created in the previous step
      * `/of` sets the Output File 
      * `/mf` creates a Mapping File (so we can exclude files in the package in a later step)
      * `/o` sets it to Overwrite the output file if it exists

5. Sie verfügen jetzt über eine `.pri`-Datei mit den Standardsprachressourcen (z. B. en-US). Um sicherzustellen, dass es ordnungsgemäß funktioniert hat, können Sie den folgenden Befehl ausführen:

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    Sie können `makepri dump /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/if` sets the Input Filename 
      * `/of` sets the Output Filename (`.xml` will be appended automatically)
      * `/o` sets it to Overwrite the output file if it exists

6. Zuletzt können Sie `..\resources.xml` in einem Text-Editor öffnen und überprüfen, ob Ihre `<NamedResource>`-Werte (z. B. `ApplicationDescription` und `PublisherDisplayName`) zusammen mit den `<Candidate>`-Werten für die ausgewählte Standardsprache aufgeführt werden (am Anfang der Datei befinden sich auch weitere Inhalte; diese können Sie vorerst ignorieren).

You can open the mapping file `..\resources.map.txt` to verify it contains the files needed for your project (including the PRI file, which is not part of the project's directory). Besonders wichtig ist, dass die Zuordnungsdatei *keinen* Verweis auf Ihre `resources.resw`-Datei enthält, da der Inhalt dieser Datei bereits in die PRI-Datei eingebettet wurde. Sie enthält jedoch andere Ressourcen wie die Dateinamen Ihrer Bilder.

#### <a name="building-and-signing-the-package"></a>Erstellen und Signieren des Pakets 

Nachdem die PRI-Datei jetzt erstellt wurde, können Sie das Paket erstellen und signieren:

1. To create the app package, run the following command replacing `contoso_demo.appx` with the name of the MSIX/AppX file you want to create and making sure to choose a different directory for the file (this sample uses the parent directory; it can be anywhere but should **not** be the project directory).

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    Sie können `makeappx pack /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/m` sets the Manifest file to use
      * `/f` sets the mapping File to use (created in the previous step) 
      * `/p` sets the output Package name
      * `/o` sets it to Overwrite the output file if it exists

2. After the package is created, it must be signed. The easiest way to get a signing certificate is by creating an empty Universal Windows project in Visual Studio and copying the `.pfx` file it creates, but you can create one manually using the `MakeCert` and `Pvk2Pfx` utilities as described in [How to create an app package signing certificate](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate).

    > [!IMPORTANT]
    > If you manually create a signing certificate, make sure you place the files in a different directory than your source project or your package source, otherwise it might get included as part of the package, including the private key!

3. Verwenden Sie zum Signieren des Pakets den folgenden Befehl. Beachten Sie, dass der im Element `Identity` der Datei `AppxManifest.xml` angegebene `Publisher` mit dem `Subject` des Zertifikats übereinstimmen muss (hierbei handelt es sich **nicht** um das Element `<PublisherDisplayName>`; dieses ist der lokalisierte Anzeigename, der den Benutzern angezeigt wird). Ersetzen Sie wie gewohnt die `contoso_demo...`-Dateinamen mit den Namen für Ihr Projekt, und (**sehr wichtig**) stellen Sie sicher, dass die `.pfx`-Datei sich nicht im aktuellen Verzeichnis befindet (andernfalls wäre sie als Teil Ihres Pakets erstellt worden, einschließlich des privaten Signaturschlüssels!):

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    Sie können `signtool sign /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/fd` sets the File Digest algorithm (SHA256 is the default for AppX)
      * `/a` will Automatically select the best certificate
      * `/f` specifies the input File that contains the signing certificate

Zuletzt können Sie auf die Datei `.appx` doppelklicken, um diese zu installieren. Wenn Sie lieber die Befehlszeile verwenden, können Sie eine PowerShell-Eingabeaufforderung öffnen, zum Verzeichnis mit dem Paket wechseln, und Folgendes eingeben (wobei Sie `contoso_demo.appx` mit Ihrem Paketnamen ersetzen müssen):

```CMD
add-appxpackage contoso_demo.appx
```

Wenn Sie Fehlermeldungen erhalten, dass das Zertifikat nicht als vertrauenswürdig eingestuft wird, stellen Sie sicher, dass es zum lokalen Speicher hinzugefügt wurde (**nicht** zum Benutzerspeicher). Um das Zertifikat zum lokalen Speicher hinzufügen, können Sie entweder die Befehlszeile oder Windows Explorer verwenden.

Verwenden der Befehlszeile:

1. Run a Visual Studio 2017 or Visual Studio 2019 command prompt as an Administrator.
2. Wechseln Sie zu dem Verzeichnis mit der `.cer`-Datei (stellen Sie sicher, dass dies nicht das Verzeichnis Ihrer Quelle oder Ihres Projekts ist!)
3. Geben Sie folgenden Befehl ein, und ersetzen Sie dabei `contoso_demo.cer` mit Ihrem Dateinamen:
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    Sie können `certutil -addstore /?` ausführen, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `-addstore` adds a certificate to a certificate store
      * `TrustedPeople` indicates the store into which the certificate is placed

Verwenden von Windows Explorer:

1. Navigieren Sie zum Ordner mit der `.pfx`-Datei
2. Doppelklicken Sie auf die `.pfx`-Datei. Der **Zertifikatimport-Assistent** sollte angezeigt werden.
3. Choose `Local Machine` and click `Next`
4. Accept the User Account Control admin elevation prompt, if it appears, and click `Next`
5. Enter the password for the private key, if there is one, and click `Next`
6. Select `Place all certificates in the following store`
7. Klicken Sie auf `Browse`, und wählen Sie den Ordner `Trusted People` (**nicht** „Vertrauenswürdige Herausgeber”)
8. Click `Next` and then `Finish`

Versuchen Sie nach dem Hinzufügen des Zertifikats zum `Trusted People`-Speicher erneut, das Paket zu installieren.

Ihre App sollte jetzt in der Liste „Alle Apps” im Startmenü angezeigt werden, mit den richtigen Informationen aus der Datei `.resw` / `.pri`. Wenn Sie eine leere Zeichenfolge oder die Zeichenfolge `ms-resource:...` sehen, ist ein Fehler aufgetreten – überprüfen Sie Ihre Bearbeitungen, und stellen Sie sicher, dass sie richtig sind. Wenn Sie im Startmenü Ihrer App mit der rechten Maustaste klicken, können Sie sie als Kachel anheften und überprüfen, ob dort die richtigen Informationen angezeigt werden.

### <a name="step-13-add-more-supported-languages"></a>Schritt 1.3: Hinzufügen von mehr unterstützten Sprachen

After the changes have been made to the package manifest and the initial `resources.resw` file has been created, adding additional languages is easy.

#### <a name="create-additional-localized-resources"></a>Erstellen zusätzlicher lokalisierter Ressourcen

Erstellen Sie zunächst die Werte für die zusätzlich lokalisierten Ressourcen. 

Erstellen Sie im Ordner `Strings` zusätzliche Ordner für jede Sprache, die Sie unterstützen. Verwenden Sie dafür den entsprechenden BCP-47-Code (z. B. `Strings\de-DE`). Erstellen Sie in jedem dieser Ordner eine `resources.resw`-Datei (mithilfe von XML-Editor oder dem Visual Studio-Designer), die die Werte der übersetzten Ressourcen enthält. Es wird vorausgesetzt, dass die lokalisierten Zeichenfolgen bereits verfügbar sind. Sie müssen diese nur in die Datei `.resw` kopieren; der Übersetzungsschritt selbst wird in diesem Dokument nicht behandelt. 

Die Datei `Strings\de-DE\resources.resw` könnte beispielsweise wie folgt aussehen, der <span style="background-color: yellow">hervorgehobene Text</span> wurde von `en-US` geändert:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (German)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (German)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (German)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, DE</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (DE)</value>
  </data>
</root>
```

Für die folgenden Schritte gilt die Annahme, dass Sie sowohl für `de-DE` als auch für `fr-FR` Ressourcen hinzugefügt haben, dasselbe Muster kann jedoch für alle Sprachen befolgt werden.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>Update the package manifest to list supported languages

The package manifest must be updated to list the languages supported by the app. Der Desktop App Converter fügt die Standardsprache hinzu, die anderen müssen jedoch explizit hinzugefügt werden. Aktualisieren Sie beim direkten Bearbeiten der Datei `AppxManifest.xml` den `Resources`-Knoten wie folgt, fügen Sie dabei so viele Elemente hinzu, wie Sie benötigen, und ersetzen die <span style="background-color: yellow">entsprechenden Sprachen, die Sie unterstützen</span>. Stellen Sie dabei außerdem sicher, dass der erste Eintrag in der Liste die Standard- (Fallback-)Sprache ist. In diesem Beispiel ist der Standard Englisch (USA) mit zusätzlicher Unterstützung für Deutsch (Deutschland) und Französisch (Frankreich):

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Wenn Sie Visual Studio verwenden, sollte Sie nichts weiter tun müssen; wenn Sie `Package.appxmanifest` betrachten, sollten Sie den speziellen Wert <span style="background-color: yellow">x generate</span> sehen. Dieser bewirkt, dass der Buildprozess die Sprachen einfügt, die sich in Ihrem Projekt befinden (basierend auf den Ordnern, die mit den BCP-47 Codes benannt sind). Note that this is not a valid value for a real package manifest; it only works for Visual Studio projects:

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>Neuerstellen mit den lokalisierten Werten

Sie können Ihre Anwendung jetzt erneut erstellen und bereitstellen, und wenn Sie Ihre bevorzugte Sprache in Windows ändern, sollten die neu lokalisierten Werte im Startmenü angezeigt werden (Informationen zum Ändern Ihrer Sprache finden Sie unten).

Bei Visual Studio können Sie wieder einfach `Ctrl+Shift+B` zum Erstellen verwenden, und mit der rechten Maustaste auf das Projekt klicken, um es bereitzustellen (`Deploy`).

Wenn Sie das Projekt manuell erstellen, führen Sie die gleichen Schritte wie oben beschrieben aus, fügen Sie jedoch die zusätzlichen Sprachen getrennt durch Unterstriche zur Liste der Standardqualifizierer (`/dq`) hinzu, wenn Sie die Konfigurationsdatei erstellen. Beispiel: Unterstützung der Ressourcen für Englisch, Deutsch und Französisch, die im vorherigen Schritt hinzugefügt wurden:

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Dadurch wird eine PRI-Datei erstellt, die alle angegebenen Sprachen enthält, die Sie einfach zum Testen verwenden können. Wenn die Gesamtgröße der Ressourcen gering ist, oder Sie nur eine geringe Anzahl von Sprachen unterstützen, kann dies für den Versand Ihrer App ausreichend sein. Wenn Sie jedoch von einer möglichst geringen Installations- oder Downloadgröße Ihrer Ressource profitieren möchten, müssen Sie separate Sprachpakete erstellen.

#### <a name="test-with-the-localized-values"></a>Test mit den lokalisierten Werten

Zum Testen der neuen lokalisierten Änderungen müssen Sie einfach eine neue bevorzugte UI-Sprache zu Windows hinzufügen. Es ist nicht erforderlich, Language Packs herunterzuladen, das System neu zu starten, oder die gesamte Windows-UI in einer fremden Sprache anzuzeigen. 

1. Führen Sie die `Settings`-App aus (`Windows + I`).
2. Go to `Time & language`
3. Go to `Region & language`
4. Click `Add a language`
5. Geben (oder wählen) Sie die gewünschte Sprache ein (z. B. `Deutsch` oder `German`)
 * Wenn untergeordnete Sprachen vorhanden sind, wählen Sie die gewünschte Sprache aus (z. B. `Deutsch / Deutschland`)
6. Wählen Sie die neue Sprache in der Liste der Sprachen aus.
7. Click `Set as default`

Öffnen Sie nun das Startmenü und suchen Sie Ihre Anwendung, und die lokalisierten Werte für die ausgewählte Sprache (andere Apps können auch lokalisiert angezeigt werden) sollten angezeigt werden. Wenn Sie den lokalisierten Namen nicht sofort sehen, warten Sie einige Minuten, bis der Cache des Startmenüs aktualisiert wird. Um auf Ihre Muttersprache zurückzuwechseln, stellen Sie sie als Standardsprache in der Liste der Sprachen ein. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>Step 1.4: Localizing more parts of the package manifest (optional)

Other sections of the package manifest can be localized. Wenn Ihre Anwendung beispielsweise Dateierweiterungen bearbeitet, dann sollte sie eine `windows.fileTypeAssociation`-Erweiterung im Manifest aufweisen, die den <span style="background-color: lightgreen">grün hervorgehobenen Text gemäß der Abbildung verwendet</span> (da es auf Ressourcen verweisen wird), und den <span style="background-color: yellow">gelb hervorgehobenen Text</span> mit Informationen ersetzt, die für Ihre Anwendung spezifisch sind:

```xml
<Extensions>
  <uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="default">
      <uap:DisplayName>ms-resource:Resources/FileTypeDisplayName</uap:DisplayName>
      <uap:Logo>Assets\StoreLogo.png</uap:Logo>
      <uap:InfoTip>ms-resource:Resources/FileTypeInfoTip</uap:InfoTip>
      <uap:SupportedFileTypes>
        <uap:FileType ContentType="application/x-contoso">.contoso</uap:FileType>
      </uap:SupportedFileTypes>
    </uap:FileTypeAssociation>
  </uap:Extension>
</Extensions>
```

Darüber hinaus können Sie diese Informationen mit dem Visual Studio-Manifest-Designer anhand der Registerkarte `Declarations` hinzufügen. Achten Sie dabei auf die <span style="background-color: lightgreen">hervorgehobenen Werte</span>:

<p><img src="images\editing-declarations-info.png"/></p>

Fügen Sie jetzt die entsprechenden Ressourcennamen zu den einzelnen `.resw`-Dateien hinzu und ersetzen Sie somit den <span style="background-color: yellow">hervorgehobenen Text</span> mit dem entsprechenden Text Ihrer App (denken Sie daran, dass Sie diesen Vorgang für *jede unterstützte Sprache* durchführen müssen!):

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

Dies wird dann in Teilen der Windows-Shell angezeigt, z. B. im Datei-Explorer:

<p><img src="images\file-type-tool-tip.png"/></p>

Erstellen und testen Sie das Paket wie zuvor. Führen Sie dabei alle neuen Szenarien aus, die die neuen UI-Zeichenfolgen anzeigen sollten.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2: Verwenden von MRT, um Ressourcen zu identifizieren und zu suchen

Im vorherigen Abschnitt wurde gezeigt, wie MRT verwendet wird, um die Manifestdatei der App zu lokalisieren, damit der Name der App und andere Metadaten durch Windows-Shell richtig angezeigt werden. Keine Code-Änderungen waren dafür erforderlich. Es ist lediglich die Verwendung von `.resw`-Dateien und einigen weiteren Tools erforderlich. In diesem Abschnitt wird Ihnen gezeigt, wie Sie MRT zum Suchen von Ressourcen in Ihren vorhandenen Ressourcenformaten und Ihren vorhandenen Ressourcenbehandlungscode mit minimalen Änderungen verwenden.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Annahmen über vorhandenes Datei-Layout und Anwendungscode

Da es viele Möglichkeiten gibt, Win32-Desktop-Apps zu lokalisieren, wird dieses Dokument einige vereinfachende Annahmen über die vorhandene App-Struktur machen, die Sie zum Zuordnen Ihrer Umgebung benötigen. Möglicherweise müssen Sie einige Änderungen an Ihre vorhandene Codebasis oder Ressourcen-Layout vornehmen, um den Anforderungen der MRT zu entsprechen. Diese gehören nicht zum Umfang dieses Dokuments.

#### <a name="resource-file-layout"></a>Layout der Ressourcendatei

This article assumes your localized resources all have the same filenames (eg, `contoso_demo.exe.mui` or `contoso_strings.dll` or `contoso.strings.xml`) but that they are placed in different folders with BCP-47 names (`en-US`, `de-DE`, etc.). Es spielt keine Rolle, wie viele Ressourcendateien Sie haben, wie sie benannt sind, welche Dateiformate/verknüpften APIs sie aufweisen usw. Das einzig Wichtige ist, dass alle *logischen* Ressourcen den gleichen Dateinamen aufweisen (sie müssen jedoch in verschiedenen *physischen* Verzeichnissen abgelegt werden). 

Gegenbeispiel: wenn Ihre Anwendung eine flache Dateistruktur mit einem einzigen `Resources`-Verzeichnis verwendet, das die Dateien `english_strings.dll` und `french_strings.dll` enthält, würde sie sich nicht gut zu MRT zuordnen lassen. Eine bessere Struktur wäre ein `Resources`-Verzeichnis mit Unterverzeichnissen und den Dateien `en\strings.dll` und `fr\strings.dll`. Es ist auch möglich, den gleichen Basisdateinamen mit eingebetteten Qualifizierern zu verwenden, z. B. `strings.lang-en.dll` und `strings.lang-fr.dll`. Die Verwendung von Verzeichnissen mit Sprachcodes ist jedoch konzeptionell einfacher. Daher konzentrieren wir uns auf diese Vorgehensweise.

>[!NOTE]
> It is still possible to use MRT and the benefits of packaging even if you can't follow this file naming convention; it just requires more work.

Die Anwendung weist beispielweise eine Reihe von benutzerdefinierten Benutzeroberflächenbefehlen auf (die für Schaltflächenbeschriftungen usw. verwendet werden), die sich in einer einfachen Textdatei mit dem Namen <span style="background-color: yellow">ui.txt</span> befinden, die unter einem <span style="background-color: yellow">UICommands</span>-Ordner dargestellt wird:

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

#### <a name="resource-loading-code"></a>Ressourcenladecode

This article assumes that at some point in your code you want to locate the file that contains a localized resource, load it, and then use it. Die APIs, die zum Laden der Ressourcen verwendet werden und die APIs, die zum Extrahieren der Ressourcen verwendet werden, sind nicht wichtig. Im Pseudocode gibt es im Wesentlichen drei Schritte:

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT erfordert nur das Ändern der ersten beiden Schritte in diesem Prozess – wie Sie die besten Ressourcen festlegen und wie Sie sie finden. Es ist nicht erforderlich, das Laden oder die Verwendung der Ressourcen zu ändern (obwohl Ihnen dafür Hilftsmittel bereitgestellt, wenn Sie davon profitieren möchten).

Die Anwendung kann z. B. die Win32-API `GetUserPreferredUILanguages`, die CRT-Funktion `sprintf` und die Win32-API `CreateFile` verwenden, um die drei oben genannten Pseudocode-Funktionen zu ersetzen, und dann die Textdatei auf `name=value`-Paare manuell analysieren. (Die Details sind nicht wichtig; dies dient lediglich der Veranschaulichung der Tatsache, dass MRT keine Auswirkung auf die Techniken hat, die für die Verarbeitung der Ressourcen verwendet wird, nachdem sie gefunden wurden).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Schritt 2.1: Codeänderungen für die Verwendung von MRT, um Dateien zu suchen

Das Wechseln zwischen Codes für die Verwendung von MRT zum Suchen von Ressourcen ist nicht schwierig. Es erfordert die Verwendung einer Handvoll von WinRT-Typen und ein paar Codezeilen. Die Haupttypen, die Sie verwenden werden, lauten wie folgt:

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), das den derzeit aktiven Satz von Qualifiziererwerten enthält (Sprache, Skalierungsfaktor usw.).
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager), (die WinRT-Version, nicht die .NET-Version) der den Zugriff auf alle Ressourcen aus der PRI-Datei ermöglicht.
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap), die eine bestimmte Teilmenge der Ressourcen in der PRI-Datei (in diesem Beispiel stehen die dateibasierten Ressourcen im Vergleich zu den Zeichenfolgenressourcen) darstellt.
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), die eine logische Ressource mit allen möglichen Kandidaten darstellt.
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), die eine konkrete Kandidatenressource darstellt. 

In Pseudocode würden Sie eine bestimmte Ressource würden Sie wie folgt einen gegebenen Ressourcen-Dateinamen (z. B. `UICommands\ui.txt` im obigen Beispiel) auflösen:

<blockquote>
<pre>
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
</blockquote>
</pre>

Beachten Sie insbesondere, dass der Code **nicht** einen bestimmten Sprachordner anfordert – z. B. `UICommands\en-US\ui.txt` – auch wenn die Dateien dementsprechend auf dem Datenträger gespeichert sind. Stattdessen wird nach dem *logischen* Dateinamen `UICommands\ui.txt` gefragt und der Code ist von MRT abhängig, um die entsprechende Datei auf dem Datenträger in einem der Sprachverzeichnisse zu finden.

Ab dieser Stelle kann die Beispiel-App weiterhin `CreateFile` zum Laden der `absoluteFileName` und Analysieren der `name=value`-Paare wie zuvor verwenden; diese Logik muss in der App nicht geändert werden. Beim Schreiben in C# oder C++/CX ist der tatsächliche Code ist nicht viel komplizierter als dieser (und in der Tat können viele der temporären Variablen ausgelassen werden) – weitere Informationen finden Sie unten im Abschnitt **Laden der .NET-Ressourcen**. C++/WRL-basierte Anwendungen werden aufgrund der COM-basierten APIs auf niedriger Ebene zum Aktivieren und Aufrufen der WinRT-APIs komplexer sein, aber die grundlegenden Schritte sind identisch. Weitere Informationen finden Sie unten im Abschnitt **Laden von Win32-MUI-Ressourcen**.

#### <a name="loading-net-resources"></a>Laden von .NET-Ressourcen

Da .NET über einen integrierten Mechanismus für das Suchen und Laden von Ressourcen (als „Satellitenassemblys” bekannt) verfügt, muss für kein expliziter Code wie im obigen synthetischen Beispiel ersetzt werden – in. NET benötigen Sie lediglich die Ressourcen-DLL-Dateien in den entsprechenden Verzeichnissen, und sie werden automatisch für Sie gefunden. When an app is packaged as an MSIX or AppX using resource packs, the directory structure is somewhat different - rather than having the resource directories be subdirectories of the main application directory, they are peers of it (or not present at all if the user doesn't have the language listed in their preferences). 

Stellen Sie sich beispielsweise eine .NET-Anwendung mit dem folgenden Layout vor, in dem alle Dateien unter dem `MainApp`-Ordner vorhanden sind:

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

Nach der Konvertierung zu AppX wird das Layout etwa wie folgt aussehen, sofern `en-US` als die Standardsprache festgelegt wurde und der Benutzer sowohl Deutsch als auch Französisch in seiner Liste aufgeführt hat:

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

Da die lokalisierten Ressourcen in den Unterverzeichnissen unterhalb des Installationsorts der ausführbaren Datei nicht mehr vorhanden sind, schlägt die integrierte .NET-Ressource fehl. Zum Glück verfügt .NET über einen klar definierten Mechanismus für die Behandlung von fehlgeschlagenen Assembly-Ladeversuchen – das `AssemblyResolve`-Ereignis. Eine .NET-App, die MRT verwendet, muss für dieses Ereignis registriert werden und die fehlende Assembly für das .NET-Ressourcen-Subsystems bereitstellen. 

Ein präzises Beispiel für die Verwendung von WinRT-APIs für das Suchen von durch .NET verwendeten Satelliten-Assemblys lautet wie folgt: der dargestellte Code wird absichtlich komprimiert, um eine minimale Implementierung anzuzeigen, obwohl sie wie Sie sehen können fast genau dem oben genannten Pseudocode entspricht, mit übergebenen `ResolveEventArgs`, die den zu suchenden Namen der Assembly bereitstellt. Eine ausführbare Version dieses Codes (mit ausführlichen Kommentare und Fehlerbehandlung) finden Sie in der Datei `PriResourceRsolver.cs` im [der **.NET Assembly Resolver**-Beispiel auf GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo).

```csharp
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with AppX, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

Angesichts der oben aufgeführten Klasse, würden Sie Folgendes an einer beliebigen frühen Stelle im Startcode Ihrer Anwendung hinzufügen (bevor lokalisierte Ressourcen geladen werden müssten):

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

Die .NET-Laufzeit löst das Ereignis `AssemblyResolve` aus, wenn die Ressourcen-DLLs nicht gefunden werden kann. An diesem Punkt sucht der bereitgestellten Ereignishandler die gewünschte Datei über MRT und gibt die Assembly zurück.

> [!NOTE]
> If your app already has an `AssemblyResolve` handler for other purposes, you will need to integrate the resource-resolving code with your existing code.

#### <a name="loading-win32-mui-resources"></a>Laden von Win32-MUI-Ressourcen

Das Laden von Win32-MUI-Ressourcen ist im Wesentlichen identisch mit dem Laden von .NET-Satellitenassemblys, es wird jedoch entweder der Code C++/CX oder C++/WRL verwendet. Die Verwendung von C++/CX ermöglicht einen viel einfacheren Code, der dem oben aufgeführten Code C# sehr ähnelt, jedoch C++-Erweiterungen, Compiler-Switches und zusätzlichen Laufzeitaufwand verwendet, die Sie vielleicht vermeiden möchten. Wenn dies der Fall ist, stellt C++/WRL eine Lösung mit deutlich geringeren Auswirkungen dar – für den Preis von ausführlicherem Code. Wenn Sie jedoch mit der ATL-Programmierung (oder mit COM im Allgemeinen) vertraut sind, sollte WRL ihnen vertraut vorkommen. 

Die folgende Beispielfunktion veranschaulicht, wie C++/WRL verwendet werden kann, um eine bestimmte Ressourcen-DLL zu laden und ein `HINSTANCE` zurückzugeben, das zum Laden weiterer Ressourcen mithilfe von Win32-Ressourcen-APIs verwendet werden kann. Beachten Sie, dass dieser Code auf der aktuellen Sprache des Benutzers beruht – anders als bei dem C#-Beispiel, bei dem der `ResourceContext` explizit mit der Sprache initialisiert wird, die von der .NET-Laufzeit angefordert wird.

```cpp
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>Phase 3: Erstellen von Ressourcenpaketen

Jetzt, da Sie über ein umfangreiches Paket verfügen, das alle Ressourcen enthält, gibt es zwei Möglichkeiten, das Hauptpaket und Ressourcenpakete separat zu erstellen, um die Größen für den Download und die Installation zu verringern:

* Nehmen Sie ein vorhandenes umfangreiches Paket, und führen Sie es im [Bündel-Generator-Tool](https://www.microsoft.com/store/apps/9nblggh43pmq) aus, um automatisch Ressourcenpakete zu erstellen. Dies ist der bevorzugte Ansatz, wenn Sie über ein Buildsystem verfügen, das bereits ein umfangreiches Paket erstellt, und Sie dieses im Nachhinein verarbeiten möchten, um die Ressourcenpakete zu erstellen.
* Erzeugen Sie die einzelnen Ressourcenpakete direkt, und integrieren Sie sie in ein Bündel. Dies ist der bevorzugte Ansatz, wenn Sie mehr Kontrolle über Ihr Buildsystem haben und die Pakete direkt erstellen können.

### <a name="step-31-creating-the-bundle"></a>Schritt 3.1: Erstellen des Bündels

#### <a name="using-the-bundle-generator-tool"></a>Verwenden des Bündel-Generator-Tools

Um das Bündel-Generator-Tool zu verwenden, muss die für das Paket erstellte PRI-Konfigurationsdatei manuell aktualisiert werden, um den Abschnitt `<packaging>` zu entfernen.

If you're using Visual Studio, refer to [Ensure that resources are installed on a device regardless of whether a device requires them](https://docs.microsoft.com/en-us/previous-versions/dn482043(v=vs.140)) for information on how to build all languages into the main package by creating the files `priconfig.packaging.xml` and `priconfig.default.xml`.

Wenn Sie Dateien manuell bearbeiten, gehen Sie folgendermaßen vor: 

1. Erstellen Sie die Konfigurationsdatei auf die gleiche Weise wie zuvor, indem Sie den richtigen Pfad, den richtigen Dateinamen und die Sprachen ersetzen:

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. Öffnen Sie die erstellte `.xml`-Datei manuell, und löschen Sie den gesamten Abschnitt `&lt;packaging&rt;` (alles andere muss jedoch intakt bleiben):

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. Erstellen Sie die `.pri`-Datei und das `.appx`-Paket wie zuvor, indem Sie die aktualisierte Konfigurationsdatei und die entsprechenden Verzeichnis- und Dateinamen verwenden (oben finden Sie weitere Informationen zu diesen Befehlen):

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. AFter the package has been created, use the following command to create the bundle, using the appropriate directory and file names:

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

Now you can move to the final step, signing (see below).

#### <a name="manually-creating-resource-packages"></a>Manuelles Erstellen von Ressourcenpaketen

Das manuelle Erstellen von Ressourcenpaketem erfordert die Ausführung eines etwas anderen Satzes von Befehlen, um eigene `.pri`- und `.appx`-Dateien zu erstellen. Diese sind den Befehlen ähnlich, die oben im Zusammenhang mit der Erstellung von FAT-Paketen beschrieben wurden. Daher werden sie nur wenig erklärt. Hinweis: Alle Befehle gehen davon aus, dass das aktuelle Verzeichnis das Verzeichnis ist, in dem sich die Datei `AppXManifest.xml` befindet. Alle Dateien werden jedoch in das übergeordnete Verzeichnis platziert. (Sie können ein anderes Verzeichnis verwenden, wenn erforderlich, sollten das Projektverzeichnis jedoch nicht mit einer dieser Dateien kontaminieren.) Wie immer, ersetzen Sie die „Contoso“-Dateinamen durch eigene Dateinamen.

1. Verwenden Sie den folgenden Befehl, um eine Konfigurationsdatei zu erstellen, die **nur** die Standardsprache als den Standardqualifizierer nennt, in diesem Fall `en-US`:

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. Erstellen Sie eine `.pri`- und `.map.txt`-Standarddatei für das Hauptpaket sowie einen zusätzlichen Satz von Dateien für jede Sprache in Ihrem Projekt. Verwenden Sie hierzu den folgenden Befehl:

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. Verwenden Sie den folgenden Befehl, um das Hauptpaket zu erstellen (das den ausführbaren Code und die Standardsprachressourcen enthält). Wie immer, ändern Sie den Namen wie gewünscht. Sie sollten das Paket jedoch in einem eigenen Verzeichnis platzieren, um das Erstellen des Bündels zu vereinfachen (dieses Beispiel verwendet das Verzeichnis `..\bundle`):

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. Nachdem das Hauptpaket erstellt wurde, verwenden Sie den folgenden Befehl einmal für jede weitere Sprache (d. h., Sie wiederholen diesen Befehl für jede Sprachzuordnungsdatei, die im vorherigen Schritt generiert wurde). Auch hier sollten Sie die Ausgabe in einem separaten Verzeichnis platzieren (in demselben Verzeichnis wie das Hauptpaket). Beachten Sie, dass die Sprache **sowohl** in der Option `/f` als auch in der Option `/p` angegeben ist, sowie die Verwendung des neuen Arguments `/r` (das anzeigt, dass ein Ressourcenpaket gewünscht wird):

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. Kombinieren Sie alle Pakete aus dem Bündelverzeichnis in einer einzigen `.appxbundle`-Datei. Die neue Option `/d` gibt das Verzeichnis für alle Dateien im Bündel an (daher wurden die `.appx`-Dateien im vorherigen Schritt in ein eigenes Verzeichnis platziert):

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

The final step to building the package is signing.

### <a name="step-32-signing-the-bundle"></a>Schritt 3.2: Signieren des Bündels

Nachdem Sie die `.appxbundle`-Datei erstellt haben (entweder über das Bündel-Generator-Tool oder manuell), erhalten Sie eine einzelne Datei, die das Hauptpaket sowie alle Ressourcenpakete enthält. Der letzte Schritt besteht im Signieren der Datei, damit sie von Windows installiert wird:

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

This will produce a signed `.appxbundle` file that contains the main package plus all the language-specific resource packages. Wie bei einer Paketdatei werden durch Doppelklicken auf diese Datei die App und alle relevanten Sprachpakete installiert, abhängig von den Windows-Spracheinstellungen des Benutzers.

## <a name="related-topics"></a>Verwandte Themen

* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md)