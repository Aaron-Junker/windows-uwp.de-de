---
title: Verwenden von MRT für konvertierte Desktop-Apps und -Spiele
description: Indem Sie Ihre .NET- oder Win32-App oder Ihr Spiel als AppX-Paket verpacken, können Sie das Ressourcenverwaltungssystem nutzen, um App-Ressourcen zu laden, die auf den Laufzeitkontext zugeschnitten sind. In diesem Thema werden die erforderlichen Techniken detailliert beschrieben.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP, mrt, pri. Ressourcen, Spiele, Centennial, Desktop App Converter, mui, Satellitenassembly
ms.localizationpriority: medium
ms.openlocfilehash: 77cf9444e06920da0eae3ae430fe78c9f5a188ad
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682539"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Verwenden des Ressourcenverwaltungssystem für Windows 10 in älteren Apps oder Spielen

Apps und Spiele für .NET und Win32 werden häufig in verschiedene Sprachen übersetzt, um die Anzahl potenzieller Märkte zu vergrößern. Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md). Indem Sie Ihre .net-oder Win32-APP oder Ihr Spiel als msix-oder AppX-Paket Verpacken, können Sie das Ressourcen Verwaltungs System zum Laden von App-Ressourcen nutzen, die auf den Lauf Zeit Kontext zugeschnitten sind. In diesem Thema werden die erforderlichen Techniken detailliert beschrieben.

Es gibt viele Möglichkeiten zum Lokalisieren einer herkömmlichen Win32-Anwendung, aber unter Windows 8 wurde ein neues [Ressourcenverwaltungssystem](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) eingeführt, das sich für alle Programmiersprachen und alle Anwendungstypen eignet und Funktionalität für mehr als eine einfache Lokalisierung bereitstellt. Dieses System wird im vorliegenden Artikel als „MRT” bezeichnet. Früher bedeutete dies „Modern Resource Technology“. Der Bestandteil „Modern“ wird jedoch nicht mehr verwendet. Der Ressourcen-Manager ist möglicherweise auch unter den Namen MRM (Modern Resource Manager, moderner Ressourcen-Manager) oder PRI (Package Resource Index, Paketressourcenindex) bekannt.

In Kombination mit der msix-basierten oder der AppX-basierten Bereitstellung (z. b. aus dem Microsoft Store) kann MRT automatisch die am meisten anwendbaren Ressourcen für einen bestimmten Benutzer/Gerät liefern, wodurch die Download-und Installations Größe Ihrer Anwendung minimiert wird. Die Größenreduzierung kann bei Anwendungen mit einer großen Menge an lokalisiertem Inhalt erheblich sein, möglicherweise in einer Größenordnung von mehreren *Gigabytes* für AAA-Spiele. Weitere Vorteile von MRT sind lokalisierte Angebote in der Windows-Shell und im Microsoft Store sowie eine automatische Fallback-Logik für den Fall, dass die bevorzugte Sprache eines Benutzers nicht den verfügbaren Ressourcen entspricht.

Dieses Dokument beschreibt die allgemeine MRT-Architektur und stellt ein Handbuch für das Portieren bereit, sodass ältere Win32-Anwendungen mit minimalen Codeänderungen zu MRT verschoben werden können. Wenn der Wechsel zu MRT erfolgt sind, stehen Entwicklern weitere Vorteile zur Verfügung (z. B. die Möglichkeit, Ressourcen nach Skalierungsfaktor oder Systemdesign zu segmentieren). Beachten Sie, dass die MRT-basierte Lokalisierung sowohl für UWP-Anwendungen als auch für Win32-Anwendungen funktioniert, die von der Desktop-Brücke verarbeitet werden (auch bekannt als „Centennial”).

In vielen Fällen können Sie Ihre vorhandenen Lokalisierungsformate und Ihren Quellcode auch nach der Integration mit MRT noch verwenden, um Ressourcen zur Laufzeit aufzulösen und Downloadgrößen zu verringern – es ist kein „Ganz-oder-gar-nicht-Ansatz”. Die folgende Tabelle fasst die Aufgabe und die geschätzten Kosten bzw. den Nutzen jeder Phase zusammen. Diese Tabelle enthält nur Aufgaben, welche die Lokalisierung betreffen, also keine Aufgaben wie z. B. das Bereitstellen von Anwendungssymbolen mit hoher Auflösung oder hohem Kontrast. Weitere Informationen zum Bereitstellen von mehreren Ressourcen für Kacheln, Symbole usw. finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Work</th>
<th>Vorteil</th>
<th>Geschätzte Kosten</th>
</tr>
<tr>
<td>Lokalisieren des Paket Manifests</td>
<td>Kaum Arbeitsaufwand erforderlich, damit der lokalisierte Inhalt in der Windows-Shell und im Microsoft Store angezeigt wird</td>
<td>Kleine</td>
</tr>
<tr>
<td>Verwenden von MRT zum Erkennen und Suchen von Ressourcen</td>
<td>Voraussetzung für die Verringerung der Download- und Installationsgrößen; automatischer Fallback für Sprachen</td>
<td>Mittel</td>
</tr>
<tr>
<td>Erstellen von Ressourcenpaketen</td>
<td>Letzter Schritt für die Verringerung der Download- und Installationsgrößen</td>
<td>Kleine</td>
</tr>
<tr>
<td>Migrieren von MRT-Ressourcenformaten und APIs</td>
<td>Erheblich geringere Dateigrößen (je nach vorhandener Ressourcentechnologie)</td>
<td>Große</td>
</tr>
</table>

## <a name="introduction"></a>Einführung

Die meisten umfassenden Anwendungen enthalten Benutzeroberflächenelemente, auch als *Ressourcen* bezeichnet, die von dem Code der Anwendung entkoppelt werden (im Gegensatz zu *hartcodierten Werte*, die im Quellcode selbst verfasst werden). Es gibt verschiedene Gründe dafür, Ressourcen gegenüber hartcodierten Werten zu bevorzugen – wie beispielsweise die einfache Bearbeitung durch Nicht-Entwickler –, einer der wichtigsten Gründe ist jedoch, dass die App auf diese Weise verschiedene Darstellungen derselben logischen Ressource zur Laufzeit auswählen kann. Beispielsweise unterscheidet sich der auf einer Schaltfläche anzuzeigende Text (oder das in einem Symbol anzuzeigende Bild) abhängig von der/den Sprache(n), die ein Benutzer versteht, den Eigenschaften des Anzeigegeräts oder davon, ob eine Hilfstechnologie aktiviert ist.

Daher besteht der Hauptzweck von Ressourcenverwaltungstechnologien darin, zur Laufzeit eine Anfrage für einen logischen oder symbolischen *Ressourcennamen* (z. B. `SAVE_BUTTON_LABEL`) aus einer Reihe von möglichen *Kandidaten* (z. B. „Save”, „Speichern” oder „저장”) in den bestmöglichen tatsächlichen *Wert* (z. B. „Speichern”) zu übersetzen. MRT stellt eine solche Funktion bereit und ermöglicht es Anwendungen, Ressourcenkandidaten mithilfe von verschiedenen Attributen, den *Qualifizierern*, wie beispielsweise der Sprache des Benutzers, dem Skalierungsfaktor des Displays, dem ausgewählten Design des Benutzers und anderen Umgebungsfaktoren, zu identifizieren. MRT unterstützt auch benutzerdefinierte Qualifizierer für Anwendungen, die diese benötigen (beispielsweise könnte eine Anwendung für Benutzer, die sich mit einem Konto angemeldet haben, und für Gastbenutzer unterschiedliche grafische Ressourcen bereitstellen, ohne dass diese Prüfung explizit zu jedem Teil der Anwendung hinzugefügt wird). MRT funktioniert sowohl mit Zeichenfolgenressourcen als auch mit dateibasierten Ressourcen, wenn dateibasierte Ressourcen als Verweise auf die externen Daten (die Dateien selbst) implementiert werden.

### <a name="example"></a>Beispiel

Dies ist ein einfaches Beispiel für eine Anwendung mit Beschriftungen auf zwei Schaltflächen (`openButton` und `saveButton`) und einer PNG-Datei, die für ein Logo verwendet wird (`logoImage`). Die Beschriftungen werden ins Englische und ins Deutsche übersetzt, und das Logo ist für normale Desktopdisplays (Skalierungsfaktor 100 %) und hochauflösende Smartphones (Skalierungsfaktor 300 %) optimiert. Beachten Sie, dass dieses Diagramm eine allgemeine konzeptionelle Ansicht des Modells darstellt. Es entspricht nicht genau der Implementierung.

<p><img src="images\conceptual-resource-model.png"/></p>

In der Grafik verweist der Anwendungscode auf die drei logischen Ressourcennamen. Zur Laufzeit verwendet die Pseudo-Funktion `GetResource` MRT, um diese Ressourcennamen in der Ressourcentabelle (als PRI-Datei bezeichnet) zu suchen und basierend auf den Umgebungsbedingungen (der Sprache des Benutzers und dem Skalierungsfaktor des Displays) den am besten geeigneten Kandidaten zu finden. Bei Beschriftungen werden die Zeichenfolgen direkt verwendet. Beim Logobild werden die Zeichenfolgen als Dateinamen interpretiert, und die Dateien werden von der Festplatte gelesen. 

Wenn der Benutzer eine andere Sprache als Englisch oder Deutsch oder einen anderen Display scale-Faktor als 100% oder 300% spricht, wählt das MRT den "nächstgelegenen" übereinstimmenden Kandidaten basierend auf einem Satz von Fall Back Regeln aus (Weitere Hintergrundinformationen finden Sie unter [Resource Management System](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) ).

Beachten Sie, dass MRT Ressourcen unterstützt, die auf mehr als einen Qualifizierer zugeschnitten sind. wenn das Logobild z. b. eingebetteten Text enthielt, der auch lokalisiert werden musste, hätte das Logo vier Kandidaten: En/Scale-100, de/Scale-100, en/Scale-300 und de/Scale-300.

### <a name="sections-in-this-document"></a>Abschnitte in diesem Dokument

In den folgenden Abschnitten werden die allgemeinen Aufgaben dargestellt, die für die Integration von MRT in Ihre Anwendung erforderlich sind.

#### <a name="phase-0-build-an-application-package"></a>Phase 0: Erstellen eines Anwendungspakets

In diesem Abschnitt wird beschrieben, wie Sie Ihre vorhandene Desktopanwendung zu einem Anwendungspaket machen. In dieser Phase werden keine MRT-Features verwendet.

#### <a name="phase-1-localize-the-application-manifest"></a>Phase 1: Lokalisieren des Anwendungs Manifests

In diesem Abschnitt wird beschrieben, wie Sie das Manifest Ihrer Anwendung lokalisieren (damit es richtig in der Windows-Shell angezeigt wird), während noch Ihr älteres Ressourcenformat und Ihre älteren APIs verwendet werden, um Ressourcen zu packen und zu suchen. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2: Verwenden von MRT zum Erkennen und Suchen von Ressourcen

In diesem Abschnitt wird beschrieben, wie Sie Ihren Anwendungscode (und gegebenenfalls das Ressourcenlayout) so ändern, dass eine Suche von Ressourcen mithilfe von MRT möglich ist, während noch Ihre vorhandenen Ressourcenformate und Ihre älteren APIs verwendet werden, um die Ressourcen zu laden und zu verwenden. 

#### <a name="phase-3-build-resource-packs"></a>Phase 3: Erstellen von Ressourcenpaketen

In diesem Abschnitt werden die endgültigen Änderungen beschrieben, die erforderlich sind, um Ihre Ressourcen in separate *Ressourcenpakete* zu teilen, wodurch die Größe des Downloads (und der Installation) Ihrer App verringert wird.

### <a name="not-covered-in-this-document"></a>In diesem Dokument nicht behandelt

Nachdem Sie die oben aufgeführten Phasen 0-3 abgeschlossen haben, verfügen Sie über eine Anwendung "Bundle", die an die Microsoft Store übermittelt werden kann. Dadurch wird die Download-und Installations Größe für Benutzer minimiert, indem nicht benötigte Ressourcen ausgelassen werden (z. b. Sprachen, die Sie nicht benötigen). Weitere Verbesserungen hinsichtlich Größe der Anwendung und Funktionen können über einen letzten Schritt vorgenommen werden.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>Phase 4: Migrieren von MRT-Ressourcenformaten und APIs

Diese Phase kann in diesem Dokument nicht behandelt werden. Sie umfasst das Übertragen Ihrer Ressourcen (insbesondere Zeichenfolgen) von älteren Formate wie beispielsweise MUI DLLs oder.NET-Ressourcenassemblys in PRI-Dateien. Dies kann zu einer weiteren Platzeinsparung hinsichtlich der Größen für den Download und die Installation führen. Es ermöglicht darüber hinaus die Verwendung weiterer MRT-Features, z. B. das Verringern der Größen für den Download und die Installation von Bilddateien auf Grundlage eines Skalierungsfaktors, Barrierefreiheiteinstellungen usw.

## <a name="phase-0-build-an-application-package"></a>Phase 0: Erstellen eines Anwendungspakets

Bevor Sie Änderungen an den Ressourcen Ihrer Anwendung vornehmen, müssen Sie zunächst Ihre aktuelle Paketerstellungs- und Installationstechnologie durch die standardmäßige UWP-Paketerstellungs und Bereitstellungstechnologie ersetzen. Hierfür gibt es drei Möglichkeiten:

* Wenn Sie über eine große Desktop Anwendung mit einem komplexen Installer verfügen oder viele Betriebssystem-Erweiterbarkeits Punkte verwenden, können Sie mit dem Desktop App Converter-Tool das UWP-Datei Layout und die manifestressformationen aus dem vorhandenen APP-Installer (z. b. einer MSI-Datei) generieren.
* Wenn Sie über eine kleinere Desktop Anwendung mit relativ wenigen Dateien oder einem einfachen Installer und ohne Erweiterbarkeits Hooks verfügen, können Sie das Datei Layout und die manifestressformationen manuell erstellen.
* Wenn Sie die Neuerstellung von der Quelle ausführen und Ihre APP als reine UWP-Anwendung aktualisieren möchten, können Sie ein neues Projekt in Visual Studio erstellen und sich auf die IDE verlassen, um einen Großteil der Arbeit zu erledigen.

Wenn Sie den [Desktop-App Converter](https://aka.ms/converter)verwenden möchten, finden Sie weitere Informationen zum Konvertierungsprozess unter [Verpacken einer Desktop Anwendung mithilfe des Desktop-App-Konverters](https://aka.ms/converterdocs) . Ein kompletter Satz von Desktop konverterbeispielen finden Sie im [GitHub-Repository "Desktop Bridge to UWP Samples](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)".

Wenn Sie das Paket manuell erstellen möchten, müssen Sie eine Verzeichnisstruktur erstellen, die alle Dateien Ihrer Anwendung (ausführbare Dateien und Inhalt, aber keinen Quellcode) und eine Paket Manifest-Datei (. appxmanifest) enthält. Ein Beispiel finden Sie im Beispiel " [Hello, World GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml)", aber eine grundlegende Paket Manifest-Datei, in der die `ContosoDemo.exe` ausführbare Desktop Datei mit dem Namen ausgeführt wird, lautet wie folgt: der <span style="background-color: yellow">markierte Text</span> wird durch ihre eigenen Werte ersetzt.

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

Weitere Informationen zur Paket Manifest-Datei und zum Paket Layout finden Sie unter [App-Paket Manifest](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest).

Wenn Sie Visual Studio verwenden, um ein neues Projekt zu erstellen und Ihren vorhandenen Code zu migrieren, finden Sie weitere Informationen unter [Erstellen einer "Hello, World"-App](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal). Sie können Ihren vorhandenen Code in das neue Projekt einschließen, aber Sie müssen wahrscheinlich bedeutende Codeänderungen vornehmen (insbesondere in der Benutzeroberfläche), um als reine UWP-app ausgeführt werden zu können. Diese Änderungen werden in diesem Dokument nicht behandelt.

## <a name="phase-1-localize-the-manifest"></a>Phase 1: Lokalisieren des Manifests

### <a name="step-11-update-strings--assets-in-the-manifest"></a>Schritt 1,1: Aktualisieren von Zeichen folgen & Assets im Manifest

In Phase 0 haben Sie eine grundlegende Paket Manifest-Datei (. appxmanifest) für Ihre Anwendung erstellt (basierend auf den Werten, die für den Konverter bereitgestellt, aus der msi extrahiert oder manuell in das Manifest eingegeben werden), aber keine lokalisierten Informationen enthalten, und es wird auch nicht unterstützt. zusätzliche Features, wie z. b. hochauflösende Start-Kachel Ressourcen usw.

Um sicherzustellen, dass der Name und die Beschreibung Ihrer Anwendung ordnungsgemäß lokalisiert werden, müssen Sie einige Ressourcen in einem Satz von Ressourcen Dateien definieren und das Paket Manifest aktualisieren, um darauf zu verweisen.

#### <a name="creating-a-default-resource-file"></a>Erstellen eine Standardressourcendatei

Der erste Schritt ist das Erstellen einer Standardressourcendatei in der Standardsprache (z. B. Englisch (USA)). Sie können dies entweder manuell mit einem Text-Editor oder über den Ressourcen-Designer in Visual Studio vornehmen.

Wenn Sie die Ressourcen manuell erstellen möchten:

1. Erstellen Sie eine XML-Datei mit dem Namen `resources.resw`, und legen Sie sie in einem `Strings\en-us`-Unterordner Ihres Projekts ab. Verwenden Sie den entsprechenden bcp-47-Code, wenn die Standardsprache nicht Englisch (USA) ist.
2. Fügen Sie in der XML-Datei den folgenden Inhalt hinzu. Ersetzten Sie dabei den <span style="background-color: yellow">hervorgehobenen Text</span> durch den entsprechenden Text in der Standardsprache Ihrer App.

> [!NOTE]
> Es gibt Einschränkungen hinsichtlich der Längen einiger dieser Zeichen folgen. Weitere Informationen finden Sie unter [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

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

1. Erstellen Sie `Strings\en-us` im Projekt den Ordner (oder eine andere Sprache), und fügen Sie dem Stamm Ordner des Projekts mit `resources.resw`dem Standardnamen ein **Neues Element** hinzu. Achten Sie darauf, **Ressourcen Datei (. resw)** und kein **Ressourcen Wörterbuch** auszuwählen. ein Ressourcen Wörterbuch ist eine Datei, die von XAML-Anwendungen verwendet wird.
2. Geben Sie im Designer die folgenden Zeichenfolgen ein (verwenden Sie dieselben `Names`, ersetzen Sie die `Values` jedoch mit dem entsprechenden Text für Ihre Anwendung):

<img src="images\editing-resources-resw.png"/>

> [!NOTE]
> Wenn Sie mit dem Visual Studio-Designer beginnen, können Sie den XML-Code jederzeit direkt `F7`bearbeiten, indem Sie drücken. Wenn Sie jedoch mit einer kleinen XML-Datei starten, *erkennt der Designer die Datei nicht*, weil viele zusätzliche Metadaten fehlen. Dies lässt sich durch Kopieren der XSD-Textbausteininformationen aus einer mit dem Designer erstellten Datei in die manuell bearbeitete XML-Datei beheben.

#### <a name="update-the-manifest-to-reference-the-resources"></a>Aktualisieren des Manifests, um auf die Ressourcen zu verweisen

Nachdem Sie die Werte in der `.resw` Datei definiert haben, besteht der nächste Schritt darin, das Manifest zu aktualisieren, um auf die Ressourcen Zeichenfolgen zu verweisen. Auch in diesem Fall können Sie eine XML-Datei direkt bearbeiten oder dies durch den Manifest-Designer von Visual Studio vornehmen lassen.

Wenn Sie XML-Code direkt bearbeiten, öffnen Sie die Datei `AppxManifest.xml`, und nehmen Sie die folgenden Änderungen an den <span style="background-color: lightgreen">hervorgehoben Werten</span> vor – verwenden Sie *genau* diesen Text und keinen für Ihre Anwendung spezifischen. Es besteht keine Notwendigkeit, genau diese Ressourcennamen zu verwenden.&mdash;Sie können eigene Ressourcennamen verwenden&mdash;, die jedoch genau mit den Angaben in der Datei `.resw` übereinstimmen müssen. Diese Namen sollten den `Names` entsprechen, die Sie in der `.resw`-Datei erstellt haben, und als Präfix das `ms-resource:`-Schema und den `Resources/`-Namespace aufweisen. 

> [!NOTE]
> In diesem Code Ausschnitt wurden viele Elemente des Manifests ausgelassen. löschen Sie nichts.

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

Wenn Sie den Visual Studio-Manifest-Designer verwenden, öffnen Sie die. appxmanifest-Datei, und ändern Sie die Werte der <span style="background-color: lightgreen">markierten Werte</span> auf der Registerkarte **Anwendung* und der Registerkarte *Paket* Erstellung:

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>Schritt 1,2: Erstellen von PRI-Dateien, Erstellen eines msix-Pakets und überprüfen, ob es funktioniert

Sie sollten jetzt in der Lage sein, die `.pri`-Datei zu erstellen und die Anwendung bereitzustellen, um sicherzustellen, dass die richtigen Informationen (in Ihrer Standardsprache) im Startmenü angezeigt werden.

Wenn Sie in Visual Studio erstellen, drücken Sie einfach `Ctrl+Shift+B`, um das Projekt zu erstellen, und klicken Sie dann mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü `Deploy` aus.

Wenn Sie manuell erstellen, führen Sie die folgenden Schritte aus, um eine Konfigurations `MakePRI` Datei für das Tool zu `.pri` erstellen und die Datei selbst zu generieren (Weitere Informationen finden Sie unter [Manuelle App-Paket](/windows/msix/package/manual-packaging-root)Erstellung):

1. Öffnen Sie eine Developer-Eingabeaufforderung im Ordner **Visual Studio 2017** oder **Visual Studio 2019** im Startmenü.
2. Wechseln Sie zum Stammverzeichnis des Projekts (das Verzeichnis, das die appxmanifest-Datei und den Ordner " **Strings** " enthält).
3. Geben Sie den folgenden Befehl ein, und ersetzen Sie dabei „contoso_demo.xml” mit einem für Ihr Projekt geeigneten Namen und „en-US” mit der Standardsprache Ihrer App (oder lassen Sie es nach Bedarf auf en-US). Beachten Sie, dass die XML-Datei im übergeordneten Verzeichnis (**nicht** im Projektverzeichnis) erstellt wird, da Sie nicht Teil der Anwendung ist. (Sie können ein beliebiges anderes Verzeichnis auswählen, aber achten Sie darauf, dies in zukünftigen Befehlen zu ersetzen.)

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    Sie können `makepri createconfig /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/cf`Legt den Konfigurations Dateinamen fest (die Ausgabe dieses Befehls).
      * `/dq`legt die Standard Qualifizierer fest, in diesem Fall die Sprache`en-US`
      * `/pv`legt die Platt Form Version fest, in diesem Fall Windows 10.
      * `/o`legt fest, dass die Ausgabedatei überschrieben wird, falls vorhanden.

4. Sie verfügen jetzt über eine Konfigurationsdatei. Führen Sie `MakePRI` erneut aus, um auf der Festplatte tatsächlich nach Ressourcen zu suchen und diese in einer PRI-Datei zu verpacken. Ersetzen Sie „contoso_demop.xml” mit dem XML-Dateinamen, den Sie im vorherigen Schritt verwendet haben, und legen Sie das übergeordnete Verzeichnis für Eingabe und Ausgabe fest: 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    Sie können `makepri new /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/pr`Legt den Projektstamm fest (in diesem Fall das aktuelle Verzeichnis).
      * `/cf`Legt den Konfigurations Dateinamen fest, der im vorherigen Schritt erstellt wurde.
      * `/of`legt die Ausgabedatei fest. 
      * `/mf`erstellt eine Mapping-Datei (daher können in einem späteren Schritt Dateien im Paket ausgeschlossen werden).
      * `/o`legt fest, dass die Ausgabedatei überschrieben wird, falls vorhanden.

5. Sie verfügen jetzt über eine `.pri`-Datei mit den Standardsprachressourcen (z. B. en-US). Um sicherzustellen, dass es ordnungsgemäß funktioniert hat, können Sie den folgenden Befehl ausführen:

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    Sie können `makepri dump /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/if`Legt den Eingabe Dateinamen fest 
      * `/of`Legt den Ausgabe Dateinamen fest (`.xml` wird automatisch angehängt).
      * `/o`legt fest, dass die Ausgabedatei überschrieben wird, falls vorhanden.

6. Zuletzt können Sie `..\resources.xml` in einem Text-Editor öffnen und überprüfen, ob Ihre `<NamedResource>`-Werte (z. B. `ApplicationDescription` und `PublisherDisplayName`) zusammen mit den `<Candidate>`-Werten für die ausgewählte Standardsprache aufgeführt werden (am Anfang der Datei befinden sich auch weitere Inhalte; diese können Sie vorerst ignorieren).

Sie können die Mapping-Datei `..\resources.map.txt` öffnen, um zu überprüfen, ob Sie die Dateien enthält, die für Ihr Projekt erforderlich sind (einschließlich der PRI-Datei, die nicht Teil des Projektverzeichnisses ist). Besonders wichtig ist, dass die Zuordnungsdatei *keinen* Verweis auf Ihre `resources.resw`-Datei enthält, da der Inhalt dieser Datei bereits in die PRI-Datei eingebettet wurde. Sie enthält jedoch andere Ressourcen wie die Dateinamen Ihrer Bilder.

#### <a name="building-and-signing-the-package"></a>Erstellen und Signieren des Pakets 

Nachdem die PRI-Datei jetzt erstellt wurde, können Sie das Paket erstellen und signieren:

1. Um das App-Paket zu erstellen, führen Sie den `contoso_demo.appx` folgenden Befehl aus, indem Sie durch den Namen der zu erstellenden msix-/AppX-Datei ersetzen und sicherstellen, dass ein anderes Verzeichnis für die Datei ausgewählt wird (in diesem Beispiel wird das übergeordnete Verzeichnis verwendet. es kann an einer beliebigen **Stelle nicht** das Projektverzeichnis).

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    Sie können `makeappx pack /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/m`legt die zu verwendende Manifest-Datei fest.
      * `/f`legt die zu verwendende Mapping-Datei fest (die im vorherigen Schritt erstellt wurde). 
      * `/p`Legt den Namen des Ausgabe Pakets fest.
      * `/o`legt fest, dass die Ausgabedatei überschrieben wird, falls vorhanden.

2. Nachdem das Paket erstellt wurde, muss es signiert werden. Die einfachste Möglichkeit, ein Signaturzertifikat zu erhalten, besteht darin, ein leeres universelles Windows-Projekt in Visual Studio `.pfx` zu erstellen und die erstellte Datei zu kopieren. Sie können Sie `MakeCert` jedoch `Pvk2Pfx` mithilfe der-und-Hilfsprogramme manuell erstellen, wie unter [beschrieben. Erstellen eines App-Paket-Signatur Zertifikats](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)

    > [!IMPORTANT]
    > Wenn Sie ein Signaturzertifikat manuell erstellen, stellen Sie sicher, dass Sie die Dateien in einem anderen Verzeichnis als das Quell Projekt oder die Paketquelle platzieren. andernfalls wird es möglicherweise als Teil des Pakets eingeschlossen, einschließlich des privaten Schlüssels!

3. Verwenden Sie zum Signieren des Pakets den folgenden Befehl. Beachten Sie, dass der im Element `Identity` der Datei `AppxManifest.xml` angegebene `Publisher` mit dem `Subject` des Zertifikats übereinstimmen muss (hierbei handelt es sich **nicht** um das Element `<PublisherDisplayName>`; dieses ist der lokalisierte Anzeigename, der den Benutzern angezeigt wird). Ersetzen Sie wie gewohnt die `contoso_demo...`-Dateinamen mit den Namen für Ihr Projekt, und (**sehr wichtig**) stellen Sie sicher, dass die `.pfx`-Datei sich nicht im aktuellen Verzeichnis befindet (andernfalls wäre sie als Teil Ihres Pakets erstellt worden, einschließlich des privaten Signaturschlüssels!):

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    Sie können `signtool sign /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `/fd`Legt den File Digest-Algorithmus fest (SHA256 ist der Standardwert für AppX).
      * `/a`wählt automatisch das beste Zertifikat aus.
      * `/f`Gibt die Eingabedatei an, die das Signaturzertifikat enthält.

Zuletzt können Sie auf die Datei `.appx` doppelklicken, um diese zu installieren. Wenn Sie lieber die Befehlszeile verwenden, können Sie eine PowerShell-Eingabeaufforderung öffnen, zum Verzeichnis mit dem Paket wechseln, und Folgendes eingeben (wobei Sie `contoso_demo.appx` mit Ihrem Paketnamen ersetzen müssen):

```CMD
add-appxpackage contoso_demo.appx
```

Wenn Sie Fehlermeldungen erhalten, dass das Zertifikat nicht als vertrauenswürdig eingestuft wird, stellen Sie sicher, dass es zum lokalen Speicher hinzugefügt wurde (**nicht** zum Benutzerspeicher). Um das Zertifikat zum lokalen Speicher hinzufügen, können Sie entweder die Befehlszeile oder Windows Explorer verwenden.

Verwenden der Befehlszeile:

1. Führen Sie eine Visual Studio 2017-oder Visual Studio 2019-Eingabeaufforderung als Administrator aus.
2. Wechseln Sie zu dem Verzeichnis mit der `.cer`-Datei (stellen Sie sicher, dass dies nicht das Verzeichnis Ihrer Quelle oder Ihres Projekts ist!)
3. Geben Sie folgenden Befehl ein, und ersetzen Sie dabei `contoso_demo.cer` mit Ihrem Dateinamen:
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    Sie können `certutil -addstore /?` ausführen, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
      * `-addstore`Fügt einem Zertifikat Speicher ein Zertifikat hinzu.
      * `TrustedPeople`Gibt den Speicher an, in den das Zertifikat eingefügt wird.

Verwenden von Windows Explorer:

1. Navigieren Sie zum Ordner mit der `.pfx`-Datei
2. Doppelklicken Sie auf die `.pfx`-Datei. Der **Zertifikatimport-Assistent** sollte angezeigt werden.
3. Klicken `Local Machine` und klicken Sie auf`Next`
4. Akzeptieren Sie die Eingabeaufforderung für Administratorrechte der Benutzerkontensteuerung, sofern diese angezeigt wird, und klicken Sie auf`Next`
5. Geben Sie ggf. das Kennwort für den privaten Schlüssel ein, und klicken Sie auf`Next`
6. Wählen Sie`Place all certificates in the following store`
7. Klicken Sie auf `Browse`, und wählen Sie den Ordner `Trusted People` (**nicht** „Vertrauenswürdige Herausgeber”)
8. Klicken `Next` Sie auf und dann`Finish`

Versuchen Sie nach dem Hinzufügen des Zertifikats zum `Trusted People`-Speicher erneut, das Paket zu installieren.

Ihre App sollte jetzt in der Liste „Alle Apps” im Startmenü angezeigt werden, mit den richtigen Informationen aus der Datei `.resw` / `.pri`. Wenn Sie eine leere Zeichenfolge oder die Zeichenfolge `ms-resource:...` sehen, ist ein Fehler aufgetreten – überprüfen Sie Ihre Bearbeitungen, und stellen Sie sicher, dass sie richtig sind. Wenn Sie im Startmenü Ihrer App mit der rechten Maustaste klicken, können Sie sie als Kachel anheften und überprüfen, ob dort die richtigen Informationen angezeigt werden.

### <a name="step-13-add-more-supported-languages"></a>Schritt 1,3: Weitere unterstützte Sprachen hinzufügen

Nachdem die Änderungen am Paket Manifest vorgenommen und die ursprüngliche `resources.resw` Datei erstellt wurde, ist das Hinzufügen zusätzlicher Sprachen einfach.

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

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>Aktualisieren des Paket Manifests zum Auflisten unterstützter Sprachen

Das Paket Manifest muss aktualisiert werden, damit die von der APP unterstützten Sprachen aufgelistet werden. Der Desktop App Converter fügt die Standardsprache hinzu, die anderen müssen jedoch explizit hinzugefügt werden. Aktualisieren Sie beim direkten Bearbeiten der Datei `AppxManifest.xml` den `Resources`-Knoten wie folgt, fügen Sie dabei so viele Elemente hinzu, wie Sie benötigen, und ersetzen die <span style="background-color: yellow">entsprechenden Sprachen, die Sie unterstützen</span>. Stellen Sie dabei außerdem sicher, dass der erste Eintrag in der Liste die Standard- (Fallback-)Sprache ist. In diesem Beispiel ist der Standard Englisch (USA) mit zusätzlicher Unterstützung für Deutsch (Deutschland) und Französisch (Frankreich):

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Wenn Sie Visual Studio verwenden, sollte Sie nichts weiter tun müssen; wenn Sie `Package.appxmanifest` betrachten, sollten Sie den speziellen Wert <span style="background-color: yellow">x generate</span> sehen. Dieser bewirkt, dass der Buildprozess die Sprachen einfügt, die sich in Ihrem Projekt befinden (basierend auf den Ordnern, die mit den BCP-47 Codes benannt sind). Beachten Sie, dass es sich hierbei nicht um einen gültigen Wert für ein echtes Paket Manifest handelt. Dies funktioniert nur für Visual Studio-Projekte:

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
2. Gehe zu`Time & language`
3. Gehe zu`Region & language`
4. Sie`Add a language`
5. Geben (oder wählen) Sie die gewünschte Sprache ein (z. B. `Deutsch` oder `German`)
 * Wenn untergeordnete Sprachen vorhanden sind, wählen Sie die gewünschte Sprache aus (z. B. `Deutsch / Deutschland`)
6. Wählen Sie die neue Sprache in der Liste der Sprachen aus.
7. Sie`Set as default`

Öffnen Sie nun das Startmenü und suchen Sie Ihre Anwendung, und die lokalisierten Werte für die ausgewählte Sprache (andere Apps können auch lokalisiert angezeigt werden) sollten angezeigt werden. Wenn Sie den lokalisierten Namen nicht sofort sehen, warten Sie einige Minuten, bis der Cache des Startmenüs aktualisiert wird. Um auf Ihre Muttersprache zurückzuwechseln, stellen Sie sie als Standardsprache in der Liste der Sprachen ein. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>Schritt 1,4: Lokalisieren von mehr Teilen des Paket Manifests (optional)

Andere Abschnitte des Paket Manifests können lokalisiert werden. Wenn Ihre Anwendung beispielsweise Dateierweiterungen bearbeitet, dann sollte sie eine `windows.fileTypeAssociation`-Erweiterung im Manifest aufweisen, die den <span style="background-color: lightgreen">grün hervorgehobenen Text gemäß der Abbildung verwendet</span> (da es auf Ressourcen verweisen wird), und den <span style="background-color: yellow">gelb hervorgehobenen Text</span> mit Informationen ersetzt, die für Ihre Anwendung spezifisch sind:

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

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2: Verwenden von MRT zum Erkennen und Suchen von Ressourcen

Im vorherigen Abschnitt wurde gezeigt, wie MRT verwendet wird, um die Manifestdatei der App zu lokalisieren, damit der Name der App und andere Metadaten durch Windows-Shell richtig angezeigt werden. Keine Code-Änderungen waren dafür erforderlich. Es ist lediglich die Verwendung von `.resw`-Dateien und einigen weiteren Tools erforderlich. In diesem Abschnitt wird Ihnen gezeigt, wie Sie MRT zum Suchen von Ressourcen in Ihren vorhandenen Ressourcenformaten und Ihren vorhandenen Ressourcenbehandlungscode mit minimalen Änderungen verwenden.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Annahmen über vorhandenes Datei-Layout und Anwendungscode

Da es viele Möglichkeiten gibt, Win32-Desktop-Apps zu lokalisieren, wird dieses Dokument einige vereinfachende Annahmen über die vorhandene App-Struktur machen, die Sie zum Zuordnen Ihrer Umgebung benötigen. Möglicherweise müssen Sie einige Änderungen an Ihre vorhandene Codebasis oder Ressourcen-Layout vornehmen, um den Anforderungen der MRT zu entsprechen. Diese gehören nicht zum Umfang dieses Dokuments.

#### <a name="resource-file-layout"></a>Layout der Ressourcendatei

In diesem Artikel wird `contoso_demo.exe.mui` davon ausgegangen, dass Ihre lokalisierten Ressourcen alle die gleichen Dateinamen haben (z. b. oder `contoso_strings.dll` oder `contoso.strings.xml`), dass Sie aber in unterschiedlichen`en-US`Ordnern mit bcp-47-Namen (, `de-DE`usw.) abgelegt werden. Dabei spielt es keine Rolle, wie viele Ressourcen Dateien Sie besitzen, wie ihre Namen sind, welche Dateiformate/zugehörigen APIs vorhanden sind usw. Das einzige, was wichtig ist, ist, dass jede *logische* Ressource denselben Dateinamen hat (aber in einem anderen *physischen* Verzeichnis platziert wird). 

Gegenbeispiel: wenn Ihre Anwendung eine flache Dateistruktur mit einem einzigen `Resources`-Verzeichnis verwendet, das die Dateien `english_strings.dll` und `french_strings.dll` enthält, würde sie sich nicht gut zu MRT zuordnen lassen. Eine bessere Struktur wäre ein `Resources`-Verzeichnis mit Unterverzeichnissen und den Dateien `en\strings.dll` und `fr\strings.dll`. Es ist auch möglich, den gleichen Basisdateinamen mit eingebetteten Qualifizierern zu verwenden, z. B. `strings.lang-en.dll` und `strings.lang-fr.dll`. Die Verwendung von Verzeichnissen mit Sprachcodes ist jedoch konzeptionell einfacher. Daher konzentrieren wir uns auf diese Vorgehensweise.

>[!NOTE]
> Es ist weiterhin möglich, MRT und die Vorteile der Paket Erstellung zu verwenden, auch wenn Sie dieser Datei Benennungs Konvention nicht folgen können. Es erfordert nur noch mehr Arbeit.

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

In diesem Artikel wird davon ausgegangen, dass Sie zu einem bestimmten Zeitpunkt im Code die Datei suchen möchten, die eine lokalisierte Ressource enthält, Sie laden und dann verwenden können. Die APIs, die zum Laden der Ressourcen verwendet werden und die APIs, die zum Extrahieren der Ressourcen verwendet werden, sind nicht wichtig. Im Pseudocode gibt es im Wesentlichen drei Schritte:

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

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Schritt 2,1: Code Änderungen zur Verwendung von MRT zum Auffinden von Dateien

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

Da .NET über einen integrierten Mechanismus für das Suchen und Laden von Ressourcen (als „Satellitenassemblys” bekannt) verfügt, muss für kein expliziter Code wie im obigen synthetischen Beispiel ersetzt werden – in. NET benötigen Sie lediglich die Ressourcen-DLL-Dateien in den entsprechenden Verzeichnissen, und sie werden automatisch für Sie gefunden. Wenn eine App mithilfe von Ressourcen Paketen als msix oder AppX verpackt wird, unterscheidet sich die Verzeichnisstruktur etwas, anstatt dass die Ressourcen Verzeichnisse Unterverzeichnisse des Haupt Anwendungs Verzeichnisses sind, Sie sind Peers davon (oder gar nicht vorhanden, wenn der Benutzer die Sprache ist nicht in Ihren Einstellungen aufgeführt.) 

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

Ein präzises Beispiel für die Verwendung von WinRT-APIs für das Suchen von durch .NET verwendeten Satelliten-Assemblys lautet wie folgt: der dargestellte Code wird absichtlich komprimiert, um eine minimale Implementierung anzuzeigen, obwohl sie wie Sie sehen können fast genau dem oben genannten Pseudocode entspricht, mit übergebenen `ResolveEventArgs`, die den zu suchenden Namen der Assembly bereitstellt. Eine ausführbare Version dieses Codes (mit ausführlichen Kommentare und Fehlerbehandlung) finden Sie in der Datei `PriResourceRsolver.cs` im [der **.NET Assembly Resolver**-Beispiel auf GitHub](https://aka.ms/fvgqt4).

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
> Wenn Ihre APP bereits über einen `AssemblyResolve` Handler für andere Zwecke verfügt, müssen Sie den Code zur Ressourcen Auflösung in Ihren vorhandenen Code integrieren.

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

## <a name="phase-3-building-resource-packs"></a>Phase 3: Aufbauen von Ressourcen Paketen

Jetzt, da Sie über ein umfangreiches Paket verfügen, das alle Ressourcen enthält, gibt es zwei Möglichkeiten, das Hauptpaket und Ressourcenpakete separat zu erstellen, um die Größen für den Download und die Installation zu verringern:

* Nehmen Sie ein vorhandenes umfangreiches Paket, und führen Sie es im [Bündel-Generator-Tool](https://aka.ms/bundlegen) aus, um automatisch Ressourcenpakete zu erstellen. Dies ist der bevorzugte Ansatz, wenn Sie über ein Buildsystem verfügen, das bereits ein umfangreiches Paket erstellt, und Sie dieses im Nachhinein verarbeiten möchten, um die Ressourcenpakete zu erstellen.
* Erzeugen Sie die einzelnen Ressourcenpakete direkt, und integrieren Sie sie in ein Bündel. Dies ist der bevorzugte Ansatz, wenn Sie mehr Kontrolle über Ihr Buildsystem haben und die Pakete direkt erstellen können.

### <a name="step-31-creating-the-bundle"></a>Schritt 3,1: Erstellen des Pakets

#### <a name="using-the-bundle-generator-tool"></a>Verwenden des Bündel-Generator-Tools

Um das Bündel-Generator-Tool zu verwenden, muss die für das Paket erstellte PRI-Konfigurationsdatei manuell aktualisiert werden, um den Abschnitt `<packaging>` zu entfernen.

Wenn Sie Visual Studio verwenden, [Stellen Sie sicher, dass Ressourcen auf einem Gerät installiert sind, unabhängig davon, ob Sie von einem Gerät benötigt werden](https://docs.microsoft.com/en-us/previous-versions/dn482043(v=vs.140)) , um Informationen dazu zu erhalten, wie Sie alle Sprachen im `priconfig.packaging.xml` Hauptpaket erstellen, indem Sie die Dateien erstellen und `priconfig.default.xml` .

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

4. Nachdem das Paket erstellt wurde, verwenden Sie den folgenden Befehl, um das Paket mit den entsprechenden Verzeichnis-und Dateinamen zu erstellen:

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

Nun können Sie mit dem letzten Schritt fortfahren (siehe unten).

#### <a name="manually-creating-resource-packages"></a>Manuelles Erstellen von Ressourcenpaketen

Das manuelle Erstellen von Ressourcenpaketem erfordert die Ausführung eines etwas anderen Satzes von Befehlen, um eigene `.pri`- und `.appx`-Dateien zu erstellen. Diese sind den Befehlen ähnlich, die oben im Zusammenhang mit der Erstellung von FAT-Paketen beschrieben wurden. Daher werden sie nur wenig erklärt. Hinweis: Alle Befehle gehen davon aus, dass das aktuelle Verzeichnis das Verzeichnis ist `AppXManifest.xml` , das die Datei enthält, aber alle Dateien werden im übergeordneten Verzeichnis abgelegt (Sie können bei Bedarf ein anderes Verzeichnis verwenden, aber Sie sollten das Projektverzeichnis nicht mit einem der Diese Dateien). Wie immer, ersetzen Sie die „Contoso“-Dateinamen durch eigene Dateinamen.

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

Der letzte Schritt zum Entwickeln des Pakets ist das Signieren.

### <a name="step-32-signing-the-bundle"></a>Schritt 3,2: Signieren des Pakets

Nachdem Sie die `.appxbundle`-Datei erstellt haben (entweder über das Bündel-Generator-Tool oder manuell), erhalten Sie eine einzelne Datei, die das Hauptpaket sowie alle Ressourcenpakete enthält. Der letzte Schritt besteht im Signieren der Datei, damit sie von Windows installiert wird:

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

Dadurch wird eine signierte `.appxbundle` Datei erstellt, die das Hauptpaket sowie alle sprachspezifischen Ressourcen Pakete enthält. Wie bei einer Paketdatei werden durch Doppelklicken auf diese Datei die App und alle relevanten Sprachpakete installiert, abhängig von den Windows-Spracheinstellungen des Benutzers.

## <a name="related-topics"></a>Verwandte Themen

* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md)