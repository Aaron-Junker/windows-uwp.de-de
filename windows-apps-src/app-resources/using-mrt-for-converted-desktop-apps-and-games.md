---
title: Verwendung von MRT für konvertierte Desktop-Apps und Spiele
description: Indem Sie Ihre .NET- oder Win32-App oder Ihr Spiel als MSIX- oder AppX-Paket verpacken, können Sie das Ressourcenverwaltungssystem nutzen, um App-Ressourcen zu laden, die auf den Laufzeitkontext zugeschnitten sind. In diesem Thema werden die Techniken detailliert beschrieben.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP, MRT, PRI. Ressourcen, Spiele, Centennial, Desktop-App Converter, MUI, Satellitenassembly
ms.localizationpriority: medium
ms.openlocfilehash: b86cbcfcc5a6c6284b993dcad1325b108b1ab353
ms.sourcegitcommit: 6cb20dca1cb60b4f6b894b95dcc2cc3a166165ad
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/01/2020
ms.locfileid: "91636490"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Verwenden des Ressourcenverwaltungssystems für Windows 10 in älteren Apps oder Spielen

.Net-und Win32-apps und-Spiele werden häufig in verschiedenen Sprachen lokalisiert, um den gesamten adressierbaren Markt zu erweitern. Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md). Indem Sie Ihre .NET- oder Win32-App oder Ihr Spiel als MSIX- oder AppX-Paket verpacken, können Sie das Ressourcenverwaltungssystem nutzen, um App-Ressourcen zu laden, die auf den Laufzeitkontext zugeschnitten sind. In diesem Thema werden die Techniken detailliert beschrieben.

Es gibt zahlreiche Möglichkeiten, eine herkömmliche Win32-Anwendung zu lokalisieren, aber mit Windows 8 wurde ein [neues Ressourcen Verwaltungssystem](/previous-versions/windows/apps/jj552947(v=win.10)) eingeführt, das über Programmiersprachen hinweg über Anwendungs Typen hinweg funktioniert und Funktionen über die einfache Lokalisierung hinweg bereitstellt. Dieses System wird in diesem Thema als "MRT" bezeichnet. In der Vergangenheit war das "moderne Ressourcen Technologie", aber der Begriff "Modern" wurde eingestellt. Der Ressourcen-Manager wird möglicherweise auch als MRM (Modern Ressourcen-Manager) oder PRI (Paket Ressourcen Index) bezeichnet.

In Kombination mit der msix-basierten oder der AppX-basierten Bereitstellung (z. b. aus dem Microsoft Store) kann MRT automatisch die am meisten anwendbaren Ressourcen für einen bestimmten Benutzer/Gerät bereitzustellen, wodurch die Download-und Installations Größe Ihrer Anwendung minimiert wird. Diese Größenreduzierung kann für Anwendungen mit einer großen Menge lokalisierter Inhalte, vielleicht in der Reihenfolge der verschiedenen *Gigabyte* für AAA-Spiele, von Bedeutung sein. Zu den zusätzlichen Vorteilen von MRT zählen lokalisierte Auflistungen in der Windows-Shell und die Microsoft Store automatische Fall Back Logik, wenn die bevorzugte Sprache eines Benutzers nicht Ihren verfügbaren Ressourcen entspricht.

Dieses Dokument beschreibt die allgemeine Architektur von MRT und bietet einen Leitfaden zum Portieren, mit dem Sie ältere Win32-Anwendungen mit minimalen Codeänderungen in MRT verschieben können. Sobald der Umstieg auf das MRT erfolgt ist, sind zusätzliche Vorteile (z. b. die Möglichkeit zum Segmentieren von Ressourcen nach Skalierungsfaktor oder Systemdesign) für den Entwickler verfügbar. Beachten Sie, dass die MRT-basierte Lokalisierung sowohl für UWP-Anwendungen als auch für Win32-Anwendungen funktioniert, die von der Desktop Bridge (auch "Centennial") verarbeitet werden

In vielen Situationen können Sie weiterhin vorhandene Lokalisierungs Formate und Quellcode verwenden, während Sie mit MRT zum Auflösen von Ressourcen zur Laufzeit und zum Minimieren der Downloadgrößen verwenden. es handelt sich hierbei nicht um einen all-oder Nothing-Ansatz. In der folgenden Tabelle werden die Arbeit und die geschätzten Kosten/Vorteile der einzelnen Phasen zusammengefasst. Diese Tabelle enthält keine Aufgaben ohne Lokalisierung, wie z. b. das Bereitstellen von Anwendungen mit hoher Auflösung oder hohem Kontrast. Weitere Informationen zum Bereitstellen mehrerer Assets für Kacheln, Symbole usw. finden [Sie unter Anpassen von Ressourcen für Sprache, Skalierung, hohen Kontrast und andere Qualifizierer](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Arbeit</th>
<th>Vorteil</th>
<th>Geschätzte Kosten</th>
</tr>
<tr>
<td>Lokalisieren des Paket Manifests</td>
<td>Erforderliche minimale Arbeitsschritte, die erforderlich sind, damit Ihre lokalisierten Inhalte in der Windows-Shell und in der Microsoft Store angezeigt werden</td>
<td>Small</td>
</tr>
<tr>
<td>Verwenden von MRT zum Ermitteln und Auffinden von Ressourcen</td>
<td>Erforderliche Komponente zum Minimieren der Download-und Installationsgrößen Automatischer sprach Fall Back</td>
<td>Medium</td>
</tr>
<tr>
<td>Erstellen von Ressourcen Paketen</td>
<td>Letzter Schritt zum Minimieren der Download-und Installationsgrößen</td>
<td>Small</td>
</tr>
<tr>
<td>Migrieren zu MRT-Ressourcen Formaten und-APIs</td>
<td>Erheblich kleinere Dateigrößen (abhängig von der vorhandenen Ressourcen Technologie)</td>
<td>Large</td>
</tr>
</table>

## <a name="introduction"></a>Einführung

Die meisten nicht trivialen Anwendungen enthalten Benutzeroberflächen Elemente, die als *Ressourcen* bezeichnet werden, die vom Code der Anwendung entkoppelt sind (im Gegensatz zu *hart codierten Werten* , die im Quellcode selbst erstellt werden). Es gibt mehrere Gründe, Ressourcen über hart codierte Werte zu bevorzugen, z. b. einfache Bearbeitung durch nicht-Entwickler, aber einer der Hauptgründe dafür ist, dass die Anwendung zur Laufzeit verschiedene Darstellungen derselben logischen Ressource auswählen kann. Beispielsweise kann der Text, der auf einer Schaltfläche angezeigt werden soll (oder das Bild, das in einem Symbol angezeigt werden soll), abhängig von der Sprache (n), die der Benutzer versteht, den Merkmalen des Anzeige Geräts oder davon, ob für den Benutzer Hilfstechnologien aktiviert sind, unterschiedlich sein.

Der primäre Zweck einer Ressourcen Verwaltungs Technologie besteht daher darin, zur Laufzeit eine Anforderung für einen logischen oder symbolischen *Ressourcennamen* (z. `SAVE_BUTTON_LABEL` b.) in den bestmöglichen tatsächlichen *Wert* (z. b. "Save") aus einer Reihe möglicher *Kandidaten* (z. b. "Save", "Save" oder "저장") zu übersetzen. MRT bietet eine solche Funktion und ermöglicht Anwendungen die Identifizierung von Ressourcen Kandidaten mithilfe einer Vielzahl von Attributen, die als *Qualifizierer*bezeichnet werden, z. b. die Sprache des Benutzers, den Skalierungsfaktor für die Anzeige, das ausgewählte Design des Benutzers und andere Umgebungsfaktoren. MRT unterstützt sogar benutzerdefinierte Qualifizierer für Anwendungen, die diese benötigen (z. b. kann eine Anwendung verschiedene Grafik Ressourcen für Benutzer bereitstellen, die sich mit einem Konto im Vergleich zu Gastbenutzern angemeldet haben, ohne diese Prüfung explizit zu jedem Teil Ihrer Anwendung hinzuzufügen). MRT funktioniert sowohl mit Zeichen folgen Ressourcen als auch mit dateibasierten Ressourcen, in denen dateibasierte Ressourcen als Verweise auf die externen Daten (die Dateien selbst) implementiert werden.

### <a name="example"></a>Beispiel

Im folgenden finden Sie ein einfaches Beispiel für eine Anwendung, die über Text Bezeichnungen auf zwei Schaltflächen ( `openButton` und `saveButton` ) und eine PNG-Datei für ein Logo ( `logoImage` ) verfügt. Die Text Bezeichnungen sind in Englisch und Deutsch lokalisiert, und das Logo ist für normale Desktop Anzeige (100% Skalierungsfaktor) und High-Resolution-Telefone (300% Skalierungsfaktor) optimiert. Beachten Sie, dass dieses Diagramm eine grundlegende konzeptionelle Ansicht des Modells darstellt. Er wird nicht exakt der Implementierung zugeordnet.

:::image type="content" source="images\conceptual-resource-model.png" alt-text="Screenshot einer Quell Code Bezeichnung, einer Bezeichnung für eine Nachschlage Tabelle und einer Datei auf der Festplatte.&quot;:::

In der Grafik verweist der Anwendungscode auf die drei logischen Ressourcennamen. Zur Laufzeit verwendet die `GetResource` Pseudo Funktion MRT, um die Ressourcennamen in der Ressourcen Tabelle (als PRI-Datei bezeichnet) anzuzeigen und den am besten geeigneten Kandidaten basierend auf den Umgebungsbedingungen (die Sprache des Benutzers und den Skalierungsfaktor der Anzeige) zu finden. Im Fall der Bezeichnungen werden die Zeichen folgen direkt verwendet. Im Fall des logobilds werden die Zeichen folgen als Dateinamen interpretiert, und die Dateien werden vom Datenträger gelesen. 

Wenn der Benutzer eine andere Sprache als Englisch oder Deutsch oder einen anderen Display scale-Faktor als 100% oder 300% spricht, wählt das MRT den &quot;nächstgelegenen" übereinstimmenden Kandidaten basierend auf einem Satz von Fall Back Regeln aus (Weitere Hintergrundinformationen finden Sie unter [Resource Management System](/previous-versions/windows/apps/jj552947(v=win.10)) ).

Beachten Sie, dass MRT Ressourcen unterstützt, die auf mehr als einen Qualifizierer zugeschnitten sind. wenn das Logobild z. b. eingebetteten Text enthielt, der auch lokalisiert werden musste, hätte das Logo vier Kandidaten: en/Scale-100, de/Scale-100, en/Scale-300 und de/Scale-300.

### <a name="sections-in-this-document"></a>Abschnitte in diesem Dokument

In den folgenden Abschnitten werden die Aufgaben auf hoher Ebene erläutert, die für die Integration von MRT in Ihre Anwendung erforderlich sind.

#### <a name="phase-0-build-an-application-package"></a>Phase 0: Erstellen eines Anwendungspakets

In diesem Abschnitt wird beschrieben, wie Sie Ihre vorhandene Desktop Anwendung als Anwendungspaket aufbauen. In dieser Phase werden keine MRT-Features verwendet.

#### <a name="phase-1-localize-the-application-manifest"></a>Phase 1: Lokalisieren des Anwendungs Manifests

In diesem Abschnitt wird beschrieben, wie Sie das Manifest Ihrer Anwendung lokalisieren (sodass es in der Windows-Shell ordnungsgemäß angezeigt wird), während Sie weiterhin Ihr Legacy Ressourcen Format und die API verwenden, um Ressourcen zu verpacken und zu suchen. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2: Verwenden von MRT zum Ermitteln und Auffinden von Ressourcen

In diesem Abschnitt wird beschrieben, wie Sie Ihren Anwendungscode (und möglicherweise Ressourcen Layout) ändern, um Ressourcen mithilfe von MRT zu finden, während Sie weiterhin vorhandene Ressourcen Formate und APIs verwenden, um die Ressourcen zu laden und zu nutzen. 

#### <a name="phase-3-build-resource-packs"></a>Phase 3: Erstellen von Ressourcen Paketen

In diesem Abschnitt werden die letzten Änderungen erläutert, die erforderlich sind, um Ihre Ressourcen in separate *Ressourcen Pakete*aufzuteilen und so die Download-und Installations Größe Ihrer APP zu minimieren.

### <a name="not-covered-in-this-document"></a>In diesem Dokument nicht behandelt

Nachdem Sie die oben aufgeführten Phasen 0-3 abgeschlossen haben, verfügen Sie über eine Anwendung "Bundle", die an die Microsoft Store übermittelt werden kann. Dadurch wird die Download-und Installations Größe für Benutzer minimiert, indem nicht benötigte Ressourcen ausgelassen werden (z. b. Sprachen, die Sie nicht benötigen). Weitere Verbesserungen bei der Anwendungs Größe und-Funktionalität können durch einen abschließenden Schritt vorgenommen werden.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>Phase 4: Migrieren zu MRT-Ressourcen Formaten und-APIs

Diese Phase geht über den Rahmen dieses Dokuments hinaus. Dazu gehört das Verschieben Ihrer Ressourcen (insbesondere von Zeichen folgen) aus Legacy Formaten wie MUI-DLLs oder .net-Ressourcenassemblys in PRI-Dateien. Dies kann zu weiteren Speicherplatz Einsparungen für Download & Installationsgrößen führen. Außerdem ermöglicht es die Verwendung anderer MRT-Features, z. b. das Minimieren des Downloads und der Installation von Bilddateien, basierend auf dem Skalierungsfaktor, den Barrierefreiheits Einstellungen usw.

## <a name="phase-0-build-an-application-package"></a>Phase 0: Erstellen eines Anwendungspakets

Bevor Sie Änderungen an den Ressourcen der Anwendung vornehmen, müssen Sie zunächst die aktuelle Verpackungs-und Installations Technologie durch die standardmäßige UWP-Verpackungs-und Bereitstellungs Technologie ersetzen. Hierzu stehen drei Möglichkeiten zur Verfügung:

* Wenn Sie über eine große Desktop Anwendung mit einem komplexen Installer verfügen oder viele Betriebssystem-Erweiterbarkeits Punkte verwenden, können Sie mit dem Desktop App Converter-Tool das UWP-Datei Layout und die manifestressformationen aus dem vorhandenen APP-Installer (z. b. einer MSI-Datei) generieren.
* Wenn Sie über eine kleinere Desktop Anwendung mit relativ wenigen Dateien oder einem einfachen Installer und ohne Erweiterbarkeits Hooks verfügen, können Sie das Datei Layout und die manifestressformationen manuell erstellen.
* Wenn Sie die Neuerstellung von der Quelle ausführen und Ihre APP als reine UWP-Anwendung aktualisieren möchten, können Sie ein neues Projekt in Visual Studio erstellen und sich auf die IDE verlassen, um einen Großteil der Arbeit zu erledigen.

Wenn Sie den [Desktop-App Converter](https://www.microsoft.com/store/p/desktopappconverter/9nblggh4skzw)verwenden möchten, finden Sie weitere Informationen zum Konvertierungsprozess unter [Verpacken einer Desktop Anwendung mithilfe des Desktop-App-Konverters](/windows/msix/desktop/desktop-to-uwp-run-desktop-app-converter) . Ein kompletter Satz von Desktop konverterbeispielen finden Sie im [GitHub-Repository "Desktop Bridge to UWP Samples](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)".

Wenn Sie das Paket manuell erstellen möchten, müssen Sie eine Verzeichnisstruktur erstellen, die alle Dateien Ihrer Anwendung (ausführbare Dateien und Inhalt, aber keinen Quellcode) und eine Paket Manifest-Datei (. appxmanifest) enthält. Ein Beispiel finden Sie im Beispiel " [Hello, World GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml)", aber eine grundlegende Paket Manifest-Datei, in der die ausführbare Desktop Datei mit dem Namen ausgeführt wird `ContosoDemo.exe` , lautet wie folgt: der <span style="background-color: yellow">markierte Text</span> wird durch ihre eigenen Werte ersetzt.

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

Weitere Informationen zur Paket Manifest-Datei und zum Paket Layout finden Sie unter [App-Paket Manifest](/uwp/schemas/appxpackage/appx-package-manifest).

Wenn Sie Visual Studio verwenden, um ein neues Projekt zu erstellen und Ihren vorhandenen Code zu migrieren, finden Sie weitere Informationen unter [Erstellen einer "Hello, World"-App](../get-started/create-a-hello-world-app-xaml-universal.md). Sie können Ihren vorhandenen Code in das neue Projekt einschließen, aber Sie müssen wahrscheinlich bedeutende Codeänderungen vornehmen (insbesondere in der Benutzeroberfläche), um als reine UWP-app ausgeführt werden zu können. Diese Änderungen werden in diesem Dokument nicht behandelt.

## <a name="phase-1-localize-the-manifest"></a>Phase 1: Lokalisieren des Manifests

### <a name="step-11-update-strings--assets-in-the-manifest"></a>Schritt 1,1: Aktualisieren von Zeichen folgen & Assets im Manifest

In Phase 0 haben Sie eine grundlegende Paket Manifest-Datei (. appxmanifest) für Ihre Anwendung erstellt (basierend auf den Werten, die für den Konverter bereitgestellt werden, die aus der msi extrahiert oder manuell in das Manifest eingegeben wurden), Sie enthalten jedoch keine lokalisierten Informationen und unterstützen auch keine zusätzlichen Features wie hochauflösende Start Kachel Ressourcen usw.

Um sicherzustellen, dass der Name und die Beschreibung Ihrer Anwendung ordnungsgemäß lokalisiert werden, müssen Sie einige Ressourcen in einem Satz von Ressourcen Dateien definieren und das Paket Manifest aktualisieren, um darauf zu verweisen.

#### <a name="creating-a-default-resource-file"></a>Erstellen einer Standard Ressourcen Datei

Der erste Schritt besteht darin, eine Standard Ressourcen Datei in der Standardsprache (z. b. US-Englisch) zu erstellen. Dies ist entweder manuell mit einem Text-Editor oder über den Ressourcen-Designer in Visual Studio möglich.

Wenn Sie die Ressourcen manuell erstellen möchten:

1. Erstellen Sie eine XML-Datei mit dem Namen, `resources.resw` und platzieren Sie Sie in einem `Strings\en-us` Unterordner des Projekts. Verwenden Sie den entsprechenden bcp-47-Code, wenn die Standardsprache nicht Englisch (USA) ist.
2. Fügen Sie in der XML-Datei den folgenden Inhalt hinzu, wobei der <span style="background-color: yellow">markierte Text</span> durch den entsprechenden Text für Ihre APP in Ihrer Standardsprache ersetzt wird.

> [!NOTE]
> Es gibt Einschränkungen hinsichtlich der Längen einiger dieser Zeichen folgen. Weitere Informationen finden Sie unter [visualelements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

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

Wenn Sie den Designer in Visual Studio verwenden möchten, gehen Sie wie folgt vor:

1. Erstellen Sie `Strings\en-us` im Projekt den Ordner (oder eine andere Sprache), und fügen Sie dem Stamm Ordner des Projekts mit dem Standardnamen ein **Neues Element** hinzu `resources.resw` . Achten Sie darauf, **Ressourcen Datei (. resw)** und kein **Ressourcen Wörterbuch** auszuwählen. ein Ressourcen Wörterbuch ist eine Datei, die von XAML-Anwendungen verwendet wird.
2. Geben Sie mithilfe des Designers die folgenden Zeichen folgen ein (verwenden Sie dasselbe, `Names` aber ersetzen Sie `Values` mit dem entsprechenden Text für Ihre Anwendung):

:::image type="content" source="images\editing-resources-resw.png" alt-text="Screenshot einer Quell Code Bezeichnung, einer Bezeichnung für eine Nachschlage Tabelle und einer Datei auf der Festplatte.&quot;:::

In der Grafik verweist der Anwendungscode auf die drei logischen Ressourcennamen. Zur Laufzeit verwendet die `GetResource` Pseudo Funktion MRT, um die Ressourcennamen in der Ressourcen Tabelle (als PRI-Datei bezeichnet) anzuzeigen und den am besten geeigneten Kandidaten basierend auf den Umgebungsbedingungen (die Sprache des Benutzers und den Skalierungsfaktor der Anzeige) zu finden. Im Fall der Bezeichnungen werden die Zeichen folgen direkt verwendet. Im Fall des logobilds werden die Zeichen folgen als Dateinamen interpretiert, und die Dateien werden vom Datenträger gelesen. 

Wenn der Benutzer eine andere Sprache als Englisch oder Deutsch oder einen anderen Display scale-Faktor als 100% oder 300% spricht, wählt das MRT den &quot;nächstgelegenen" :::

> [!NOTE]
> Wenn Sie mit dem Visual Studio-Designer beginnen, können Sie den XML-Code jederzeit direkt bearbeiten, indem Sie drücken `F7` . Wenn Sie jedoch mit einer minimalen XML-Datei beginnen, *erkennt der Designer die Datei nicht* , da viele zusätzliche Metadaten fehlen. Sie können dieses Problem beheben, indem Sie die Textbausteine-XSD-Informationen aus einer vom Designer generierten Datei in Ihre Hand bearbeitete XML-Datei kopieren.

#### <a name="update-the-manifest-to-reference-the-resources"></a>Aktualisieren Sie das Manifest, um auf die Ressourcen zu verweisen.

Nachdem Sie die Werte in der Datei definiert haben `.resw` , besteht der nächste Schritt darin, das Manifest zu aktualisieren, um auf die Ressourcen Zeichenfolgen zu verweisen. Sie können eine XML-Datei auch direkt bearbeiten oder auf den Visual Studio-Manifest-Designer zurückgreifen.

Wenn Sie XML direkt bearbeiten, öffnen Sie die `AppxManifest.xml` Datei, und nehmen Sie die folgenden Änderungen an den <span style="background-color: lightgreen">markierten Werten</span> vor: Verwenden Sie diesen *exakten* Text, nicht für die Anwendung spezifische Text. Es ist nicht erforderlich, diese exakten Ressourcennamen zu verwenden, &mdash; die Sie selbst auswählen können &mdash; . Sie müssen jedoch genau den Namen der Datei auswählen, die Sie auswählen `.resw` . Diese Namen sollten mit dem versehen `Names` werden, das Sie in der `.resw` Datei erstellt haben, dem das `ms-resource:` Schema und den Namespace vorangestellt sind `Resources/` . 

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

:::image type="content" source="images\editing-application-info.png" alt-text="Screenshot einer Quell Code Bezeichnung, einer Bezeichnung für eine Nachschlage Tabelle und einer Datei auf der Festplatte.&quot;:::

In der Grafik verweist der Anwendungscode auf die drei logischen Ressourcennamen. Zur Laufzeit verwendet die `GetResource` Pseudo Funktion MRT, um die Ressourcennamen in der Ressourcen Tabelle (als PRI-Datei bezeichnet) anzuzeigen und den am besten geeigneten Kandidaten basierend auf den Umgebungsbedingungen (die Sprache des Benutzers und den Skalierungsfaktor der Anzeige) zu finden. Im Fall der Bezeichnungen werden die Zeichen folgen direkt verwendet. Im Fall des logobilds werden die Zeichen folgen als Dateinamen interpretiert, und die Dateien werden vom Datenträger gelesen. 

Wenn der Benutzer eine andere Sprache als Englisch oder Deutsch oder einen anderen Display scale-Faktor als 100% oder 300% spricht, wählt das MRT den &quot;nächstgelegenen" :::

:::image type="content" source="images\editing-packaging-info.png" alt-text="Screenshot einer Quell Code Bezeichnung, einer Bezeichnung für eine Nachschlage Tabelle und einer Datei auf der Festplatte.&quot;:::

In der Grafik verweist der Anwendungscode auf die drei logischen Ressourcennamen. Zur Laufzeit verwendet die `GetResource` Pseudo Funktion MRT, um die Ressourcennamen in der Ressourcen Tabelle (als PRI-Datei bezeichnet) anzuzeigen und den am besten geeigneten Kandidaten basierend auf den Umgebungsbedingungen (die Sprache des Benutzers und den Skalierungsfaktor der Anzeige) zu finden. Im Fall der Bezeichnungen werden die Zeichen folgen direkt verwendet. Im Fall des logobilds werden die Zeichen folgen als Dateinamen interpretiert, und die Dateien werden vom Datenträger gelesen. 

Wenn der Benutzer eine andere Sprache als Englisch oder Deutsch oder einen anderen Display scale-Faktor als 100% oder 300% spricht, wählt das MRT den &quot;nächstgelegenen" :::

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>Schritt 1,2: Erstellen von PRI-Dateien, Erstellen eines msix-Pakets und Überprüfen der Funktionsweise

Sie sollten jetzt in der Lage sein, die Datei zu erstellen `.pri` und die Anwendung bereitzustellen, um zu überprüfen, ob die korrekten Informationen (in der Standardsprache) im Startmenü angezeigt werden.

Wenn Sie in Visual Studio erstellen, drücken Sie einfach, `Ctrl+Shift+B` um das Projekt zu erstellen, und klicken Sie dann mit der rechten Maustaste auf das Projekt, und wählen Sie `Deploy` aus dem Kontextmenü aus.

Wenn Sie manuell erstellen, führen Sie die folgenden Schritte aus, um eine Konfigurationsdatei für `MakePRI` das Tool zu erstellen und die `.pri` Datei selbst zu generieren (Weitere Informationen finden Sie unter [Manuelle App-Paket](/windows/msix/package/manual-packaging-root)Erstellung):

1. Öffnen Sie eine Developer-Eingabeaufforderung im Ordner **Visual Studio 2017** oder **Visual Studio 2019** im Startmenü.
2. Wechseln Sie zum Stammverzeichnis des Projekts (das Verzeichnis, das die appxmanifest-Datei und den Ordner " **Strings** " enthält).
3. Geben Sie den folgenden Befehl ein, und ersetzen Sie "contoso_demo.xml" durch einen Namen, der für Ihr Projekt geeignet ist, und "en-US" mit der Standardsprache Ihrer APP (oder behalten Sie ggf. "en-US" bei). Beachten Sie, dass die XML-Datei im übergeordneten Verzeichnis (**nicht** im Projektverzeichnis) erstellt wird, da Sie nicht Teil der Anwendung ist. (Sie können ein beliebiges anderes Verzeichnis auswählen, aber achten Sie darauf, dies in zukünftigen Befehlen zu ersetzen.)

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    Sie können eingeben, `makepri createconfig /?` um zu sehen, was jeder Parameter hat, aber zusammengefasst:
      * `/cf` Legt den Konfigurations Dateinamen fest (die Ausgabe dieses Befehls).
      * `/dq` legt die Standard Qualifizierer fest, in diesem Fall die Sprache `en-US`
      * `/pv` legt die Platt Form Version fest, in diesem Fall Windows 10.
      * `/o` legt fest, dass die Ausgabedatei überschrieben wird, falls vorhanden.

4. Sie verfügen nun über eine Konfigurationsdatei, führen Sie `MakePRI` erneut aus, um den Datenträger tatsächlich nach Ressourcen zu durchsuchen und Sie in einer PRI-Datei zu verpacken. Ersetzen Sie "contoso_demop.xml" durch den XML-Dateinamen, den Sie im vorherigen Schritt verwendet haben, und geben Sie das übergeordnete Verzeichnis sowohl für die Eingabe als auch für die Ausgabe an: 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    Sie können eingeben, `makepri new /?` um zu sehen, was jeder Parameter hat, aber kurz gesagt:
      * `/pr` Legt den Projektstamm fest (in diesem Fall das aktuelle Verzeichnis).
      * `/cf` Legt den Konfigurations Dateinamen fest, der im vorherigen Schritt erstellt wurde.
      * `/of` legt die Ausgabedatei fest. 
      * `/mf` erstellt eine Mapping-Datei (daher können in einem späteren Schritt Dateien im Paket ausgeschlossen werden).
      * `/o` legt fest, dass die Ausgabedatei überschrieben wird, falls vorhanden.

5. Nun verfügen Sie über eine `.pri` Datei mit den Standardsprachen Ressourcen (z. b. en-US). Sie können den folgenden Befehl ausführen, um zu überprüfen, ob er ordnungsgemäß funktioniert:

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    Sie können eingeben, `makepri dump /?` um zu sehen, was jeder Parameter hat, aber kurz gesagt:
      * `/if` Legt den Eingabe Dateinamen fest 
      * `/of` Legt den Ausgabe Dateinamen fest ( `.xml` wird automatisch angehängt).
      * `/o` legt fest, dass die Ausgabedatei überschrieben wird, falls vorhanden.

6. Schließlich können Sie `..\resources.xml` in einem Text-Editor öffnen und überprüfen, ob die Werte (z. b `<NamedResource>` `ApplicationDescription` `PublisherDisplayName` . und) zusammen mit den `<Candidate>` Werten für die gewünschte Standardsprache aufgeführt werden (es gibt noch weitere Inhalte am Anfang der Datei).

Sie können die Mapping-Datei öffnen `..\resources.map.txt` , um zu überprüfen, ob Sie die Dateien enthält, die für Ihr Projekt erforderlich sind (einschließlich der PRI-Datei, die nicht Teil des Projektverzeichnisses ist). Wichtig ist, dass die Mapping-Datei *keinen* Verweis auf die `resources.resw` Datei enthält, da der Inhalt der Datei bereits in die PRI-Datei eingebettet wurde. Sie enthält jedoch andere Ressourcen wie die Dateinamen der Bilder.

#### <a name="building-and-signing-the-package"></a>Entwickeln und Signieren des Pakets 

Nachdem die PRI-Datei erstellt wurde, können Sie das Paket erstellen und signieren:

1. Um das App-Paket zu erstellen, führen Sie den folgenden Befehl aus, `contoso_demo.appx` indem Sie durch den Namen der zu erstellenden msix/. AppX-Datei ersetzen und sicherstellen, dass Sie ein anderes Verzeichnis für die Datei auswählen (in diesem Beispiel wird das übergeordnete Verzeichnis verwendet, es kann sich an einem beliebigen Ort befinden, sollte aber **nicht** das Projektverzeichnis sein)

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    Sie können eingeben, `makeappx pack /?` um zu sehen, was jeder Parameter hat, aber kurz gesagt:
      * `/m` legt die zu verwendende Manifest-Datei fest.
      * `/f` legt die zu verwendende Mapping-Datei fest (die im vorherigen Schritt erstellt wurde). 
      * `/p` Legt den Namen des Ausgabe Pakets fest.
      * `/o` legt fest, dass die Ausgabedatei überschrieben wird, falls vorhanden.

2. Nachdem das Paket erstellt wurde, muss es signiert werden. Die einfachste Möglichkeit, ein Signaturzertifikat zu erhalten, besteht darin, ein leeres universelles Windows-Projekt in Visual Studio zu erstellen und die `.pfx` erstellte Datei zu kopieren. Sie können Sie jedoch mithilfe der `MakeCert` -und-Hilfsprogramme manuell erstellen, `Pvk2Pfx` wie unter [Erstellen eines App-Paket-Signatur Zertifikats](/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate)beschrieben.

    > [!IMPORTANT]
    > Wenn Sie ein Signaturzertifikat manuell erstellen, stellen Sie sicher, dass Sie die Dateien in einem anderen Verzeichnis als das Quell Projekt oder die Paketquelle platzieren. andernfalls wird es möglicherweise als Teil des Pakets eingeschlossen, einschließlich des privaten Schlüssels!

3. Verwenden Sie den folgenden Befehl, um das Paket zu signieren. Beachten Sie, dass der, der `Publisher` im- `Identity` Element von angegeben `AppxManifest.xml` `Subject` ist, dem des Zertifikats entsprechen muss (Dies ist **nicht** das- `<PublisherDisplayName>` Element, das der lokalisierte Anzeige Name ist, der Benutzern angezeigt werden soll). Ersetzen Sie die Dateinamen wie üblich `contoso_demo...` durch die Namen, die für das Projekt geeignet sind, und (**sehr wichtig**) stellen Sie sicher, dass `.pfx` sich die Datei nicht im aktuellen Verzeichnis befindet (andernfalls hätte Sie als Teil des Pakets erstellt, einschließlich des privaten Signatur Schlüssels!):

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    Sie können eingeben, `signtool sign /?` um zu sehen, was jeder Parameter hat, aber kurz gesagt:
      * `/fd` Legt den File Digest-Algorithmus fest (SHA256 ist der Standardwert für. AppX).
      * `/a` wählt automatisch das beste Zertifikat aus.
      * `/f` Gibt die Eingabedatei an, die das Signaturzertifikat enthält.

Schließlich können Sie nun auf die Datei doppelklicken, `.appx` um Sie zu installieren, oder wenn Sie die Befehlszeile bevorzugen, können Sie eine PowerShell-Eingabeaufforderung öffnen, zu dem Verzeichnis wechseln, das das Paket enthält, und Folgendes eingeben ( `contoso_demo.appx` durch ihren Paketnamen ersetzen):

```CMD
add-appxpackage contoso_demo.appx
```

Wenn Sie Fehlermeldungen erhalten, dass das Zertifikat nicht vertrauenswürdig ist, stellen Sie sicher, dass es dem Computerspeicher (**nicht** dem Benutzerspeicher) hinzugefügt wird. Zum Hinzufügen des Zertifikats zum Computerspeicher können Sie entweder die Befehlszeile oder den Windows-Explorer verwenden.

So verwenden Sie die Befehlszeile:

1. Führen Sie eine Visual Studio 2017-oder Visual Studio 2019-Eingabeaufforderung als Administrator aus.
2. Wechseln Sie zu dem Verzeichnis, das die `.cer` Datei enthält (stellen Sie sicher, dass dies außerhalb der Quell-oder Projekt Verzeichnisse liegt!)
3. Geben Sie den folgenden Befehl ein, und ersetzen `contoso_demo.cer` Sie durch den Dateinamen:
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    Sie können ausführen, `certutil -addstore /?` um zu sehen, was jeder Parameter hat, aber kurz gesagt:
      * `-addstore` Fügt einem Zertifikat Speicher ein Zertifikat hinzu.
      * `TrustedPeople` Gibt den Speicher an, in den das Zertifikat eingefügt wird.

So verwenden Sie Windows-Explorer:

1. Navigieren Sie zu dem Ordner, der die Datei enthält. `.pfx`
2. Doppelklicken Sie auf die `.pfx` Datei, und der **Zertifikat Import-Assistent** sollte angezeigt werden.
3. `Local Machine`Klicken und klicken Sie auf`Next`
4. Akzeptieren Sie die Eingabeaufforderung für Administratorrechte der Benutzerkontensteuerung, sofern diese angezeigt wird, und klicken Sie auf `Next`
5. Geben Sie ggf. das Kennwort für den privaten Schlüssel ein, und klicken Sie auf `Next`
6. `Place all certificates in the following store` auswählen
7. Klicken Sie `Browse` , und wählen Sie den `Trusted People` Ordner (**nicht** "Vertrauenswürdige Herausgeber") aus.
8. Klicken Sie auf `Next` und dann `Finish`

Nachdem Sie das Zertifikat zum Speicher hinzugefügt `Trusted People` haben, versuchen Sie erneut, das Paket zu installieren.

Sie sollten nun sehen, dass Ihre APP im Startmenü der Liste "alle apps" mit den korrekten Informationen aus der Datei angezeigt wird `.resw`  /  `.pri` . Wenn eine leere Zeichenfolge oder die Zeichenfolge angezeigt wird, `ms-resource:...` ist ein Fehler aufgetreten. Überprüfen Sie die Änderungen, und stellen Sie sicher, dass Sie korrekt sind. Wenn Sie im Startmenü mit der rechten Maustaste auf Ihre APP klicken, können Sie Sie als Kachel anheften und überprüfen, ob dort auch die richtigen Informationen angezeigt werden.

### <a name="step-13-add-more-supported-languages"></a>Schritt 1,3: Hinzufügen von weiteren unterstützten Sprachen

Nachdem die Änderungen am Paket Manifest vorgenommen und die ursprüngliche `resources.resw` Datei erstellt wurde, ist das Hinzufügen zusätzlicher Sprachen einfach.

#### <a name="create-additional-localized-resources"></a>Erstellen zusätzlicher lokalisierter Ressourcen

Erstellen Sie zunächst die zusätzlichen lokalisierten Ressourcen Werte. 

`Strings`Erstellen Sie im Ordner zusätzliche Ordner für jede Sprache, die Sie unterstützen, indem Sie den entsprechenden bcp-47-Code (z `Strings\de-DE` . b.) verwenden. Erstellen Sie in jedem dieser Ordner eine `resources.resw` Datei (entweder mit einem XML-Editor oder dem Visual Studio-Designer), die die übersetzten Ressourcen Werte enthält. Es wird vorausgesetzt, dass Sie die lokalisierten Zeichen folgen bereits an einem Ort verfügbar sind, und Sie müssen Sie nur in die `.resw` Datei kopieren. in diesem Dokument wird der Übersetzungs Schritt nicht behandelt. 

Die Datei könnte z. b. `Strings\de-DE\resources.resw` wie folgt aussehen, wobei der <span style="background-color: yellow">markierte Text</span> von geändert wird `en-US` :

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

In den folgenden Schritten wird davon ausgegangen, dass Sie Ressourcen für und hinzugefügt `de-DE` `fr-FR` haben, aber das gleiche Muster kann für jede Sprache befolgt werden.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>Aktualisieren des Paket Manifests zum Auflisten unterstützter Sprachen

Das Paket Manifest muss aktualisiert werden, damit die von der APP unterstützten Sprachen aufgelistet werden. Der Desktop App Converter fügt die Standardsprache hinzu, aber die anderen müssen explizit hinzugefügt werden. Wenn Sie die `AppxManifest.xml` Datei direkt bearbeiten, aktualisieren Sie den `Resources` Knoten wie folgt, und fügen Sie so viele Elemente hinzu, wie Sie benötigen, und ersetzen Sie dabei die <span style="background-color: yellow">entsprechenden Sprachen, die Sie unterstützen</span> , und stellen Sie sicher, dass der erste Eintrag in der Liste die Standardsprache (Fallback) ist. In diesem Beispiel lautet der Standardwert Englisch (USA) mit zusätzlicher Unterstützung für Deutsch (Deutschland) und Französisch (Frankreich):

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Wenn Sie Visual Studio verwenden, sollten Sie nichts tun. Wenn Sie sich ansehen `Package.appxmanifest` , sollte der Wert für die spezielle <span style="background-color: yellow">x-Generierung</span> angezeigt werden, der bewirkt, dass der Buildprozess die im Projekt gefundenen Sprachen einfügt (basierend auf den Ordnern mit bcp-47-Codes). Beachten Sie, dass es sich hierbei nicht um einen gültigen Wert für ein echtes Paket Manifest handelt. Dies funktioniert nur für Visual Studio-Projekte:

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>Neuerstellung mit den lokalisierten Werten

Nun können Sie die Anwendung erneut erstellen und bereitstellen. Wenn Sie Ihre bevorzugte Sprache in Windows ändern, sollten die neu lokalisierten Werte im Startmenü angezeigt werden (Anweisungen zum Ändern der Sprache finden Sie unten).

Für Visual Studio können Sie auch nur `Ctrl+Shift+B` zum Erstellen und mit der rechten Maustaste auf das Projekt zu verwenden `Deploy` .

Wenn Sie das Projekt manuell erstellen, führen Sie die gleichen Schritte wie oben aus, und fügen Sie die durch Unterstriche getrennten zusätzlichen Sprachen der Standard qualifiziererliste () hinzu, `/dq` Wenn Sie die Konfigurationsdatei erstellen. So unterstützen Sie z. b. die im vorherigen Schritt hinzugefügten Ressourcen für Englisch, Deutsch und Französisch:

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Dadurch wird eine PRI-Datei erstellt, die alle angegebenen languagesenthält, die Sie problemlos für Tests verwenden können. Wenn die Gesamtgröße Ihrer Ressourcen gering ist oder nur eine kleine Anzahl von Sprachen unterstützt wird, ist dies möglicherweise für Ihre Versand-App akzeptabel. Dies ist nur der Fall, wenn Sie die Vorteile der Minimierung der Installations-/Downloadgröße für Ihre Ressourcen benötigen, die Sie benötigen, um die zusätzliche Arbeit beim Aufbau separater Sprachpakete zu erledigen.

#### <a name="test-with-the-localized-values"></a>Testen mit den lokalisierten Werten

Um die neuen lokalisierten Änderungen zu testen, fügen Sie Windows einfach eine neue bevorzugte UI-Sprache hinzu. Es ist nicht erforderlich, Sprachpakete herunterzuladen, das System neu zu starten oder die gesamte Windows-Benutzeroberfläche in einer Fremdsprache anzuzeigen. 

1. Ausführen der `Settings` app ( `Windows + I` )
2. Besuchen Sie `Time & language`.
3. Besuchen Sie `Region & language`.
4. Klicken Sie auf `Add a language`.
5. Geben Sie die gewünschte Sprache ein (oder wählen Sie Sie `Deutsch` aus). `German`
 * Wenn eine unter Sprache vorhanden ist, wählen Sie die gewünschte Option aus (z. b. `Deutsch / Deutschland` ).
6. Wählen Sie in der Liste Sprache die neue Sprache aus.
7. Klicken Sie auf `Set as default`.

Öffnen Sie jetzt das Startmenü, und suchen Sie nach der Anwendung, und die lokalisierten Werte für die ausgewählte Sprache sollten angezeigt werden (andere apps werden möglicherweise auch lokalisiert). Wenn der lokalisierte Name nicht sofort angezeigt wird, warten Sie einige Minuten, bis der Cache des Startmenüs aktualisiert wird. Wenn Sie zu ihrer eigenen Sprache zurückkehren möchten, legen Sie die Standardsprache in der Sprachliste ab. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>Schritt 1,4: Lokalisieren von mehr Teilen des Paket Manifests (optional)

Andere Abschnitte des Paket Manifests können lokalisiert werden. Wenn Ihre Anwendung z. b. Dateierweiterungen verarbeitet, sollte Sie eine `windows.fileTypeAssociation` Erweiterung im Manifest aufweisen, wobei der <span style="background-color: lightgreen">grün markierte Text</span> genau wie gezeigt verwendet wird (da er auf Ressourcen verweist) und den <span style="background-color: yellow">gelben markierten Text</span> durch spezifische Informationen für Ihre Anwendung ersetzt:

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

Sie können diese Informationen auch mit dem Visual Studio-Manifest-Designer hinzufügen, indem Sie `Declarations` auf der Registerkarte die <span style="background-color: lightgreen">markierten Werte</span>beachten:

:::image type="content" source="images\editing-declarations-info.png" alt-text="Screenshot einer Quell Code Bezeichnung, einer Bezeichnung für eine Nachschlage Tabelle und einer Datei auf der Festplatte.&quot;:::

In der Grafik verweist der Anwendungscode auf die drei logischen Ressourcennamen. Zur Laufzeit verwendet die `GetResource` Pseudo Funktion MRT, um die Ressourcennamen in der Ressourcen Tabelle (als PRI-Datei bezeichnet) anzuzeigen und den am besten geeigneten Kandidaten basierend auf den Umgebungsbedingungen (die Sprache des Benutzers und den Skalierungsfaktor der Anzeige) zu finden. Im Fall der Bezeichnungen werden die Zeichen folgen direkt verwendet. Im Fall des logobilds werden die Zeichen folgen als Dateinamen interpretiert, und die Dateien werden vom Datenträger gelesen. 

Wenn der Benutzer eine andere Sprache als Englisch oder Deutsch oder einen anderen Display scale-Faktor als 100% oder 300% spricht, wählt das MRT den &quot;nächstgelegenen" :::

Fügen Sie nun den einzelnen Dateien die entsprechenden Ressourcennamen hinzu `.resw` , und ersetzen Sie dabei den <span style="background-color: yellow">markierten Text</span> durch den entsprechenden Text für Ihre APP (denken Sie daran, dies für *jede unterstützte Sprache*zu tun!):

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

Diese werden dann in Teilen der Windows-Shell angezeigt, z. b. im Datei-Explorer:

:::image type="content" source="images\file-type-tool-tip.png" alt-text="Screenshot einer Quell Code Bezeichnung, einer Bezeichnung für eine Nachschlage Tabelle und einer Datei auf der Festplatte.&quot;:::

In der Grafik verweist der Anwendungscode auf die drei logischen Ressourcennamen. Zur Laufzeit verwendet die `GetResource` Pseudo Funktion MRT, um die Ressourcennamen in der Ressourcen Tabelle (als PRI-Datei bezeichnet) anzuzeigen und den am besten geeigneten Kandidaten basierend auf den Umgebungsbedingungen (die Sprache des Benutzers und den Skalierungsfaktor der Anzeige) zu finden. Im Fall der Bezeichnungen werden die Zeichen folgen direkt verwendet. Im Fall des logobilds werden die Zeichen folgen als Dateinamen interpretiert, und die Dateien werden vom Datenträger gelesen. 

Wenn der Benutzer eine andere Sprache als Englisch oder Deutsch oder einen anderen Display scale-Faktor als 100% oder 300% spricht, wählt das MRT den &quot;nächstgelegenen":::

Erstellen und testen Sie das Paket wie zuvor, und üben Sie alle neuen Szenarien aus, in denen die neuen UI-Zeichen folgen angezeigt werden.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2: Verwenden von MRT zum Ermitteln und Auffinden von Ressourcen

Im vorherigen Abschnitt wurde gezeigt, wie Sie mit MRT die Manifest-Datei Ihrer APP lokalisieren, damit die Windows-Shell den APP-Namen und andere Metadaten korrekt anzeigen kann. Hierfür sind keine Codeänderungen erforderlich. Es war lediglich die Verwendung von `.resw` Dateien und einigen zusätzlichen Tools erforderlich. In diesem Abschnitt erfahren Sie, wie Sie mit MRT Ressourcen in Ihren vorhandenen Ressourcen Formaten suchen und Ihren vorhandenen Code zur Ressourcenverarbeitung mit minimalen Änderungen verwenden.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Annahmen über vorhandenes Datei Layout & Anwendungscode

Da es viele Möglichkeiten gibt, Win32-Desktop-Apps zu lokalisieren, werden in diesem Whitepaper einige vereinfachende Annahmen über die Struktur der vorhandenen Anwendung erläutert, die Sie Ihrer spezifischen Umgebung zuordnen müssen. Möglicherweise müssen Sie einige Änderungen an Ihrer vorhandenen Codebasis oder auf dem Ressourcen Layout vornehmen, um den Anforderungen von MRT gerecht zu werden. Diese werden größtenteils nicht in diesem Dokument behandelt.

#### <a name="resource-file-layout"></a>Layout der Ressourcen Datei

In diesem Artikel wird davon ausgegangen, dass Ihre lokalisierten Ressourcen alle die gleichen Dateinamen haben (z. b. `contoso_demo.exe.mui` oder `contoso_strings.dll` oder `contoso.strings.xml` ), dass Sie aber in unterschiedlichen Ordnern mit bcp-47-Namen ( `en-US` , `de-DE` usw.) abgelegt werden. Dabei spielt es keine Rolle, wie viele Ressourcen Dateien Sie besitzen, wie ihre Namen sind, welche Dateiformate/zugehörigen APIs vorhanden sind usw. Das einzige, was wichtig ist, ist, dass jede *logische* Ressource denselben Dateinamen hat (aber in einem anderen *physischen* Verzeichnis platziert wird). 

Beispiel: Wenn die Anwendung eine Flatfile-Struktur mit einem einzelnen Verzeichnis verwendet, `Resources` das die Dateien `english_strings.dll` und enthält `french_strings.dll` , wird das MRT nicht gut zugeordnet. Eine bessere Struktur wäre ein `Resources` Verzeichnis mit Unterverzeichnissen und Dateien `en\strings.dll` und `fr\strings.dll` . Es ist auch möglich, denselben Basis Dateinamen zu verwenden, aber mit eingebetteten Qualifizierern, wie z. b. `strings.lang-en.dll` und `strings.lang-fr.dll` , aber die Verwendung von Verzeichnissen mit den Sprachcodes ist konzeptionell einfacher, und das ist der Schwerpunkt.

>[!NOTE]
> Es ist weiterhin möglich, MRT und die Vorteile der Paket Erstellung zu verwenden, auch wenn Sie dieser Datei Benennungs Konvention nicht folgen können. Es erfordert nur noch mehr Arbeit.

Die Anwendung kann z. b. über einen Satz benutzerdefinierter Benutzeroberflächen Befehle (für Schaltflächen Bezeichnungen usw.) in einer einfachen Textdatei mit dem Namen " <span style="background-color: yellow">ui.txt</span>" verfügen, die sich unter dem Ordner " <span style="background-color: yellow">uicommands</span> " befinden:

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

#### <a name="resource-loading-code"></a>Code zum Laden von Ressourcen

In diesem Artikel wird davon ausgegangen, dass Sie zu einem bestimmten Zeitpunkt im Code die Datei suchen möchten, die eine lokalisierte Ressource enthält, Sie laden und dann verwenden können. Die APIs, die zum Laden der Ressourcen verwendet werden, sowie die APIs, die zum Extrahieren der Ressourcen verwendet werden, sind nicht von Bedeutung. In Pseudocode sind im Wesentlichen drei Schritte erforderlich:

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

MRT erfordert nur, dass die ersten beiden Schritte in diesem Prozess geändert werden. dabei wird erläutert, wie Sie die besten Kandidaten Ressourcen ermitteln und wie Sie Sie finden. Es ist nicht erforderlich, dass Sie ändern, wie Sie die Ressourcen laden oder verwenden (obwohl diese Funktionen bereitstellen, wenn Sie Sie nutzen möchten).

Die Anwendung kann z. b. die Win32-API `GetUserPreferredUILanguages` , die CRT `sprintf` -Funktion und die Win32-API verwenden, `CreateFile` um die drei oben genannten Pseudo Code Funktionen zu ersetzen, und dann die Textdatei mit der Suche nach Paaren manuell analysieren `name=value` . (Die Details sind nicht wichtig. Dies soll lediglich veranschaulichen, dass MRT keine Auswirkung auf die Techniken hat, die zum Verarbeiten von Ressourcen verwendet werden, sobald Sie gefunden wurden.)

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Schritt 2,1: Code Änderungen zur Verwendung von MRT zum Auffinden von Dateien

Das Wechseln Ihres Codes zur Verwendung von MRT zum Suchen von Ressourcen ist nicht schwierig. Hierfür müssen einige WinRT-Typen und einige Codezeilen verwendet werden. Die wichtigsten Typen, die Sie verwenden werden, lauten wie folgt:

* [Resourcecontext](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), der den derzeit aktiven Satz von Qualifiziererwerten kapselt (Sprache, Skalierungsfaktor usw.)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (die WinRT-Version, nicht die .NET-Version), die den Zugriff auf alle Ressourcen aus der PRI-Datei ermöglicht
* [ResourceMap](/uwp/api/windows.applicationmodel.resources.core.resourcemap): stellt eine bestimmte Teilmenge der Ressourcen in der PRI-Datei dar (in diesem Beispiel die dateibasierten Ressourcen im Vergleich zu den Zeichen folgen Ressourcen).
* " [Namedresource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource)", die eine logische Ressource und alle möglichen Kandidaten darstellt.
* [Resourcecandidate](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), das eine einzelne konkrete Kandidaten Ressource darstellt 

In Pseudo Code sieht die Art und Weise, wie Sie einen bestimmten Ressourcen Dateinamen auflösen würden (wie `UICommands\ui.txt` im obigen Beispiel), wie folgt aus:

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

Beachten Sie insbesondere, dass der Code einen bestimmten Sprachordner **nicht** anfordert `UICommands\en-US\ui.txt` , auch wenn die Dateien auf dem Datenträger vorhanden sind. Stattdessen wird der *logische* Dateiname angefordert, `UICommands\ui.txt` und die entsprechende Datei auf dem Datenträger wird von MRT in einem der sprach Verzeichnisse gefunden.

Von hier aus kann die Beispiel-App weiterhin verwenden, `CreateFile` um zu laden `absoluteFileName` und die `name=value` Paare wie zuvor zu analysieren. keine dieser Logik muss in der APP geändert werden. Wenn Sie in c# oder C++/CX schreiben, ist der tatsächliche Code nicht viel komplizierter als dies (und tatsächlich können viele der zwischen Variablen vereinfacht werden). Weitere Informationen finden Sie unten im Abschnitt zum **Laden von .NET-Ressourcen**. C++/WRL-based Anwendungen sind aufgrund der com-basierten Low-Level-APIs, die zum Aktivieren und Abrufen der WinRT-APIs verwendet werden, komplexer. die folgenden grundlegenden Schritte sind jedoch identisch. Weitere Informationen finden Sie unten im Abschnitt zum **Laden von Win32-MUI-Ressourcen**.

#### <a name="loading-net-resources"></a>Laden von .NET-Ressourcen

Da .net über einen integrierten Mechanismus zum Suchen und Laden von Ressourcen verfügt (als "Satellitenassemblys" bezeichnet), gibt es keinen expliziten Code, der wie im obigen synthetischen Beispiel ersetzt werden kann. in .net benötigen Sie lediglich die Ressourcen-DLLs in den entsprechenden Verzeichnissen, die automatisch für Sie gespeichert werden. Wenn eine App mithilfe von Ressourcen Paketen als msix-oder AppX-Paket verpackt wird, unterscheidet sich die Verzeichnisstruktur gering voneinander, anstatt dass die Ressourcen Verzeichnisse Unterverzeichnisse des Haupt Anwendungs Verzeichnisses sind. Sie sind Peers davon (oder gar nicht vorhanden, wenn der Benutzer nicht die Sprache in Ihren Einstellungen aufgelistet hat). 

Stellen Sie sich z. b. eine .NET-Anwendung mit folgendem Layout vor, in der alle Dateien unter dem Ordner vorhanden sind `MainApp` :

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

Nach der Konvertierung in AppX sieht das Layout in etwa wie folgt aus, wobei angenommen wird, dass es sich um `en-US` die Standardsprache handelt und der Benutzer sowohl Deutsch als auch Französisch in der Sprachliste aufgeführt hat:

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

Da die lokalisierten Ressourcen in den Unterverzeichnissen unter dem Installationsort der hauptausführ baren Datei nicht mehr vorhanden sind, kann die integrierte .NET-Ressourcen Auflösung nicht ausgeführt werden. Glücklicherweise verfügt .net über einen klar definierten Mechanismus zur Behandlung fehlerhafter assemblyauslastungs Versuche-das- `AssemblyResolve` Ereignis. Eine .net-APP, die MRT verwendet, muss sich für dieses Ereignis registrieren und die fehlende Assembly für das .NET-Ressourcen Subsystem bereitstellen. 

Ein Beispiel für die Verwendung der WinRT-APIs zum Auffinden von Satellitenassemblys, die von .NET verwendet werden, lautet wie folgt: der Code wird in der dargestellten Form absichtlich komprimiert, um eine minimale Implementierung anzuzeigen. Sie können jedoch sehen, dass er dem obigen Pseudo Code genau zugeordnet wird. dabei wird der Name der Assembly bereitgestellt, die `ResolveEventArgs` Sie suchen müssen. Eine ausführbare Version dieses Codes (mit ausführlichen Kommentaren und Fehlerbehandlung) finden Sie in der Datei `PriResourceRsolver.cs` im [Beispiel zum .net-Assemblyresolver auf GitHub **.NET Assembly Resolver** ](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DotNetSatelliteAssemblyDemo).

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

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with .appx, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

Bei der oben genannten Klasse würden Sie am Anfang des Start Codes Ihrer Anwendung Folgendes hinzufügen (bevor lokalisierte Ressourcen geladen werden müssen):

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

Die .NET-Laufzeit gibt das `AssemblyResolve` Ereignis immer dann aus, wenn die Ressourcen-DLLs nicht gefunden werden können. der bereitgestellte Ereignishandler findet die gewünschte Datei über MRT und gibt die Assembly zurück.

> [!NOTE]
> Wenn Ihre APP bereits über einen `AssemblyResolve` Handler für andere Zwecke verfügt, müssen Sie den Code zur Ressourcen Auflösung in Ihren vorhandenen Code integrieren.

#### <a name="loading-win32-mui-resources"></a>Laden von Win32-MUI-Ressourcen

Das Laden von Win32-MUI-Ressourcen ist im Grunde das gleiche wie das Laden von .net-Satellitenassemblys, stattdessen aber entweder C++/CX oder C++/WRL Code Die Verwendung von C++/CX bietet viel einfacheren Code, der eng mit dem obigen c#-Code übereinstimmt. er verwendet jedoch C++-Spracherweiterungen, compilerswitches und zusätzliche Lauf Zeit Übersichten, die Sie möglicherweise vermeiden möchten. Wenn dies der Fall ist, bietet die Verwendung von C++/WRL eine weitaus geringere Auswirkung auf Kosten von ausführlicheren Code. Wenn Sie jedoch mit der ATL-Programmierung (bzw. com im allgemeinen) vertraut sind, sollte WRL vertraut sein. 

Die folgende Beispiel Funktion zeigt, wie C++/WRL verwendet wird, um eine bestimmte Ressourcen-DLL zu laden und eine zurückzugeben `HINSTANCE` , die zum Laden weiterer Ressourcen mithilfe der üblichen Win32-Ressourcen-APIs verwendet werden kann. Beachten Sie, dass im Gegensatz zum c#-Beispiel, in dem der explizit `ResourceContext` mit der von der .NET-Laufzeit angeforderten Sprache initialisiert wird, dieser Code von der aktuellen Sprache des Benutzers abhängig ist.

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

## <a name="phase-3-building-resource-packs"></a>Phase 3: aufbauen von Ressourcen Paketen

Nun, da Sie über ein "Fat Pack" verfügen, das alle Ressourcen enthält, gibt es zwei Pfade zum Entwickeln von separaten Haupt Paketen und Ressourcen Paketen, um die Download-und Installationsgrößen zu minimieren:

* Erstellen Sie ein vorhandenes FAT-Paket, und führen Sie es über [das Paket Generator Tool](https://www.microsoft.com/store/apps/9nblggh43pmq) aus, um automatisch Ressourcen Pakete zu erstellen. Dies ist der bevorzugte Ansatz, wenn Sie über ein Buildsystem verfügen, das bereits ein Fat-Pack erstellt, und Sie es zur Generierung der Ressourcen Pakete bereitstellen möchten.
* Erstellen Sie die einzelnen Ressourcen Pakete direkt, und erstellen Sie Sie in einem Paket. Dies ist der bevorzugte Ansatz, wenn Sie mehr Kontrolle über Ihr Buildsystem haben und die Pakete direkt erstellen können.

### <a name="step-31-creating-the-bundle"></a>Schritt 3,1: Erstellen des Pakets

#### <a name="using-the-bundle-generator-tool"></a>Verwenden des Bundle Generator-Tools

Um das Bundle Generator-Tool zu verwenden, muss die PRI-Konfigurationsdatei, die für das Paket erstellt wurde, manuell aktualisiert werden, um den Abschnitt zu entfernen `<packaging>` .

Wenn Sie Visual Studio verwenden, [Stellen Sie sicher, dass Ressourcen auf einem Gerät installiert sind, unabhängig davon, ob Sie von einem Gerät benötigt werden](/previous-versions/dn482043(v=vs.140)) , um Informationen dazu zu erhalten, wie Sie alle Sprachen in das Hauptpaket erstellen, indem Sie die Dateien `priconfig.packaging.xml` und erstellen `priconfig.default.xml` .

Wenn Sie Dateien manuell bearbeiten, führen Sie die folgenden Schritte aus: 

1. Erstellen Sie die Konfigurationsdatei auf die gleiche Weise wie zuvor, und ersetzen Sie dabei den richtigen Pfad, den Dateinamen und die Sprachen:

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. Öffnen Sie die erstellte `.xml` Datei manuell, und löschen Sie den gesamten `&lt;packaging&rt;` Abschnitt (behalten Sie aber alle anderen Elemente bei):

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. Erstellen `.pri` Sie die Datei und das `.appx` Paket wie zuvor, und verwenden Sie dabei die aktualisierte Konfigurationsdatei und die entsprechenden Verzeichnis-und Dateinamen (Weitere Informationen zu diesen Befehlen finden Sie weiter oben):

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. Nachdem das Paket erstellt wurde, verwenden Sie den folgenden Befehl, um das Paket mit den entsprechenden Verzeichnis-und Dateinamen zu erstellen:

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

Nun können Sie mit dem letzten Schritt fortfahren (siehe unten).

#### <a name="manually-creating-resource-packages"></a>Manuelles Erstellen von Ressourcen Paketen

Das manuelle Erstellen von Ressourcen Paketen erfordert eine etwas andere Reihe von Befehlen, um `.pri` separate `.appx` Dateien und Dateien zu erstellen. diese sind mit den oben verwendeten Befehlen zum Erstellen von FAT-Paketen vergleichbar. es wird also eine minimale Erläuterung gegeben. Hinweis: bei allen Befehlen wird davon ausgegangen, dass das aktuelle Verzeichnis das Verzeichnis ist, das die `AppXManifest.xml` Datei enthält, aber alle Dateien werden im übergeordneten Verzeichnis abgelegt (Sie können ggf. ein anderes Verzeichnis verwenden, aber Sie sollten das Projektverzeichnis nicht mit diesen Dateien verschmutzen). Ersetzen Sie die Dateinamen "Configuration Manager" wie immer durch ihre eigenen Dateinamen.

1. Verwenden Sie den folgenden Befehl, um eine Konfigurationsdatei zu erstellen, die **nur** die Standardsprache als Standard Qualifizierer benennt: in diesem Fall `en-US` :

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. Erstellen Sie einen Standard `.pri` -und eine- `.map.txt` Datei für das Hauptpaket sowie einen zusätzlichen Satz von Dateien für jede Sprache, die in Ihrem Projekt gefunden wird, mit dem folgenden Befehl:

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. Verwenden Sie den folgenden Befehl, um das Hauptpaket (das den ausführbaren Code und die Standard Sprachressourcen enthält) zu erstellen. Ändern Sie den Namen wie immer in einem separaten Verzeichnis, um das Erstellen des Pakets zu einem späteren Zeitpunkt zu vereinfachen (in diesem Beispiel wird das `..\bundle` Verzeichnis verwendet):

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. Nachdem das Hauptpaket erstellt wurde, verwenden Sie den folgenden Befehl für jede weitere Sprache (d. r. Wiederholen Sie diesen Befehl für jede sprach Zuordnungs Datei, die Sie im vorherigen Schritt erstellt haben). Die Ausgabe sollte sich wieder in einem separaten Verzeichnis befinden (das gleiche wie das Hauptpaket). Beachten Sie, dass die Sprache **sowohl** in der `/f` -Option als auch in der- `/p` Option und die Verwendung des neuen `/r` Arguments (das angibt, dass ein Ressourcenpaket gewünscht ist) angegeben wird:

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. Kombinieren Sie alle Pakete aus dem Paket Verzeichnis in einer einzelnen `.appxbundle` Datei. Die neue `/d` Option gibt das Verzeichnis an, das für alle Dateien im Paket verwendet werden soll (aus diesem Grund `.appx` werden die Dateien im vorherigen Schritt in ein separates Verzeichnis eingefügt):

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

Der letzte Schritt zum Entwickeln des Pakets ist das Signieren.

### <a name="step-32-signing-the-bundle"></a>Schritt 3,2: Signieren des Pakets

Nachdem Sie die `.appxbundle` Datei (entweder über das Bundle Generator-Tool oder manuell) erstellt haben, verfügen Sie über eine einzelne Datei, die das Hauptpaket und alle Ressourcen Pakete enthält. Der letzte Schritt besteht darin, die Datei zu signieren, sodass Sie von Windows installiert wird:

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

Dadurch wird eine signierte `.appxbundle` Datei erstellt, die das Hauptpaket sowie alle sprachspezifischen Ressourcen Pakete enthält. Auf die Anwendung kann genauso wie auf eine Paketdatei zum Installieren der APP und auf der Grundlage der Windows-Spracheinstellungen des Benutzers ein Doppelklick erfolgen.

## <a name="related-topics"></a>Zugehörige Themen

* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md)