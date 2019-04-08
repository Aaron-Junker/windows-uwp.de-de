---
title: Verwenden von MRT für konvertierte Desktop-Apps und -Spiele
description: Indem Sie Ihre .NET- oder Win32-App oder Ihr Spiel als AppX-Paket verpacken, können Sie das Ressourcenverwaltungssystem nutzen, um App-Ressourcen zu laden, die auf den Laufzeitkontext zugeschnitten sind. In diesem Thema werden die erforderlichen Techniken detailliert beschrieben.
ms.date: 10/25/2017
ms.topic: article
keywords: Windows 10, UWP, mrt, pri. Ressourcen, Spiele, Centennial, Desktop App Converter, mui, Satellitenassembly
ms.localizationpriority: medium
ms.openlocfilehash: 287c22cbd50f1b69f505bbddd445740fe9422c31
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597015"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Verwenden des Ressourcenverwaltungssystem für Windows 10 in älteren Apps oder Spielen

## <a name="overview"></a>Übersicht

Apps und Spiele für .NET und Win32 werden häufig in verschiedene Sprachen übersetzt, um die Anzahl potenzieller Märkte zu vergrößern. Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md). Indem Sie Ihre .NET- oder Win32-App oder Ihr Spiel als AppX-Paket verpacken, können Sie das Ressourcenverwaltungssystem nutzen, um App-Ressourcen zu laden, die auf den Laufzeitkontext zugeschnitten sind. In diesem Thema werden die erforderlichen Techniken detailliert beschrieben.

Es gibt viele Möglichkeiten zum Lokalisieren einer herkömmlichen Win32-Anwendung, aber unter Windows 8 wurde ein neues [Ressourcenverwaltungssystem](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx) eingeführt, das sich für alle Programmiersprachen und alle Anwendungstypen eignet und Funktionalität für mehr als eine einfache Lokalisierung bereitstellt. Dieses System wird im vorliegenden Artikel als „MRT” bezeichnet. Früher bedeutete dies „Modern Resource Technology“. Der Bestandteil „Modern“ wird jedoch nicht mehr verwendet. Der Ressourcen-Manager ist möglicherweise auch unter den Namen MRM (Modern Resource Manager, moderner Ressourcen-Manager) oder PRI (Package Resource Index, Paketressourcenindex) bekannt.

In Kombination mit einer AppX-basierten Bereitstellung (z. B. über den Microsoft Store) kann MRT automatisch die am besten anwendbaren Ressourcen für einen bestimmten Benutzer/ein bestimmtes Gerät bereitstellen, wodurch die Größe des Downloads und der Installationsumfang der Anwendung verringert werden. Die Größenreduzierung kann bei Anwendungen mit einer großen Menge an lokalisiertem Inhalt erheblich sein, möglicherweise in einer Größenordnung von mehreren *Gigabytes* für AAA-Spiele. Weitere Vorteile von MRT sind lokalisierte Angebote in der Windows-Shell und im Microsoft Store sowie eine automatische Fallback-Logik für den Fall, dass die bevorzugte Sprache eines Benutzers nicht den verfügbaren Ressourcen entspricht.

Dieses Dokument beschreibt die allgemeine MRT-Architektur und stellt ein Handbuch für das Portieren bereit, sodass ältere Win32-Anwendungen mit minimalen Codeänderungen zu MRT verschoben werden können. Wenn der Wechsel zu MRT erfolgt sind, stehen Entwicklern weitere Vorteile zur Verfügung (z. B. die Möglichkeit, Ressourcen nach Skalierungsfaktor oder Systemdesign zu segmentieren). Beachten Sie, dass die MRT-basierte Lokalisierung sowohl für UWP-Anwendungen als auch für Win32-Anwendungen funktioniert, die von der Desktop-Brücke verarbeitet werden (auch bekannt als „Centennial”).

In vielen Fällen können Sie Ihre vorhandenen Lokalisierungsformate und Ihren Quellcode auch nach der Integration mit MRT noch verwenden, um Ressourcen zur Laufzeit aufzulösen und Downloadgrößen zu verringern – es ist kein „Ganz-oder-gar-nicht-Ansatz”. Die folgende Tabelle fasst die Aufgabe und die geschätzten Kosten bzw. den Nutzen jeder Phase zusammen. Diese Tabelle enthält nur Aufgaben, welche die Lokalisierung betreffen, also keine Aufgaben wie z. B. das Bereitstellen von Anwendungssymbolen mit hoher Auflösung oder hohem Kontrast. Weitere Informationen zum Bereitstellen von mehreren Ressourcen für Kacheln, Symbole usw. finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Work</th>
<th>Vorteil</th>
<th>Geschätzte Kosten</th>
</tr>
<tr>
<td>Lokalisieren des AppX-Manifests</td>
<td>Kaum Arbeitsaufwand erforderlich, damit der lokalisierte Inhalt in der Windows-Shell und im Microsoft Store angezeigt wird</td>
<td>Klein</td>
</tr>
<tr>
<td>Verwenden von MRT zum Erkennen und Suchen von Ressourcen</td>
<td>Voraussetzung für die Verringerung der Download- und Installationsgrößen; automatischer Fallback für Sprachen</td>
<td>Mittel</td>
</tr>
<tr>
<td>Erstellen von Ressourcenpaketen</td>
<td>Letzter Schritt für die Verringerung der Download- und Installationsgrößen</td>
<td>Klein</td>
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

Wenn der Benutzer eine andere Sprache als Englisch oder Deutsch spricht oder einen anderen Skalierungsfaktor als 100 % oder 300 % verwendet, wählt MRT auf Grundlager einer Reihe von Fallbackregeln den Kandidaten mit den größten Übereinstimmung (weitere Hintergrundinformationen dazu im [Thema **Ressourcenverwaltungssystem** auf MSDN](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx)). 

Beachten Sie, dass MRT unterstützt Ressourcen, die auf mehr als einem Qualifizierer zugeschnitten sind – z. B. wenn das Logobild eingebettetem Text, die enthalten lokalisiert werden auch benötigt, das Logo vier Kandidaten müssten: EN/Skalierung: 100, DE/Skalierung-100, EN/Skalierung-300 und Skalierung: DE: 300.

### <a name="sections-in-this-document"></a>Abschnitte in diesem Dokument

In den folgenden Abschnitten werden die allgemeinen Aufgaben dargestellt, die für die Integration von MRT in Ihre Anwendung erforderlich sind.

**Phase 0: Erstellen eines Anwendungspakets**

In diesem Abschnitt wird beschrieben, wie Sie Ihre vorhandene Desktopanwendung zu einem Anwendungspaket machen. In dieser Phase werden keine MRT-Features verwendet.

**Phase 1: Lokalisieren des Anwendungsmanifests**

In diesem Abschnitt wird beschrieben, wie Sie das Manifest Ihrer Anwendung lokalisieren (damit es richtig in der Windows-Shell angezeigt wird), während noch Ihr älteres Ressourcenformat und Ihre älteren APIs verwendet werden, um Ressourcen zu packen und zu suchen. 

**Phase 2: Verwenden Sie einen MRT zu identifizieren, und suchen Sie nach Ressourcen**

In diesem Abschnitt wird beschrieben, wie Sie Ihren Anwendungscode (und gegebenenfalls das Ressourcenlayout) so ändern, dass eine Suche von Ressourcen mithilfe von MRT möglich ist, während noch Ihre vorhandenen Ressourcenformate und Ihre älteren APIs verwendet werden, um die Ressourcen zu laden und zu verwenden. 

**Phase 3: Erstellen von Ressourcenpaketen**

In diesem Abschnitt werden die endgültigen Änderungen beschrieben, die erforderlich sind, um Ihre Ressourcen in separate *Ressourcenpakete* zu teilen, wodurch die Größe des Downloads (und der Installation) Ihrer App verringert wird.

### <a name="not-covered-in-this-document"></a>In diesem Dokument nicht behandelt

Nach Abschluss der oben beschriebenen Phasen 0 bis 3 verfügen Sie über ein „Anwendungsbündel”, das an den Microsoft Store übermittelt werden kann und das den Download- und Installationsumfang für Benutzer verringert, indem die nicht benötigten Ressourcen ausgelassen werden (z. B. nicht verwendete Sprachen). Weitere Verbesserungen hinsichtlich Größe der Anwendung und Funktionen können über einen letzten Schritt vorgenommen werden. 

**Phase 4: Migrieren Sie zu MRT-Resource-Formate und -APIs**

Diese Phase kann in diesem Dokument nicht behandelt werden. Sie umfasst das Übertragen Ihrer Ressourcen (insbesondere Zeichenfolgen) von älteren Formate wie beispielsweise MUI DLLs oder.NET-Ressourcenassemblys in PRI-Dateien. Dies kann zu einer weiteren Platzeinsparung hinsichtlich der Größen für den Download und die Installation führen. Es ermöglicht darüber hinaus die Verwendung weiterer MRT-Features, z. B. das Verringern der Größen für den Download und die Installation von Bilddateien auf Grundlage eines Skalierungsfaktors, Barrierefreiheiteinstellungen usw.

- - -

## <a name="phase-0-build-an-application-package"></a>Phase 0: Erstellen eines Anwendungspakets

Bevor Sie Änderungen an den Ressourcen Ihrer Anwendung vornehmen, müssen Sie zunächst Ihre aktuelle Paketerstellungs- und Installationstechnologie durch die standardmäßige UWP-Paketerstellungs und Bereitstellungstechnologie ersetzen. Hierfür gibt es drei Möglichkeiten:

0. Wenn Sie über eine große Desktopanwendung mit einem komplexen Installationsprogramm verfügen oder viele Erweiterungspunkte für das Betriebssystem verwenden, können Sie das Tool Desktop App Converter verwenden, um das Layout der UWP-Datei und die Manifestinformationen aus dem vorhandenen App-Installationsprogramm (z. B. eine MSI-Datei) zu generieren.
0. Wenn Sie über eine kleinere Desktopanwendung mit relativ wenigen Dateien oder einem einfachen Installationsprogramm und keinen Hooks für die Erweiterbarkeit verfügen, können Sie das Dateilayout und die Manifestinformationen manuell erstellen.
0. Wenn Sie eine Neuerstellung aus der Quelle vornehmen und Ihre App so aktualisieren möchten, dass sie eine „reine” UWP-Anwendung ist, können Sie ein neues Projekt in Visual Studio erstellen und die IDE einen Großteil der Arbeit durchführen lassen

Wenn Sie verwenden möchten. die [Desktop App Converter](https://aka.ms/converter), finden Sie in [der **Desktop, UWP Bridge: Desktop App Converter** Thema bei MSDN](https://aka.ms/converterdocs) für Weitere Informationen zu konvertieren. Einen vollständigen Satz an Desktop Converter-Beispielen finden Sie im [GitHub-Repository **Desktop-Brücke-zu-UWP – Beispiele**](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

Wenn Sie das Paket manuell erstellen möchten, müssen Sie eine Verzeichnisstruktur erstellen, die alle Dateien Ihrer Anwendung (ausführbare Dateien und Inhalte, aber keinen Quellcode) und eine `AppXManifest.xml`-Datei enthält. Ein Beispiel finden Sie im [GitHub-Beispiel **Hello, World**](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml); eine einfache `AppXManifest.xml` -Datei, die die ausführbare Desktopdatei mit dem Namen `ContosoDemo.exe` ausführt, würde folgendermaßen aussehen, wobei der <span style="background-color: yellow">hervorgehobene Text</span> mit Ihren eigenen Werte ersetzt würde:

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap"&gt;
    &lt;Identity Name="<span style="background-color: yellow">Contoso.Demo</span>"
              Publisher="<span style="background-color: yellow">CN=Contoso.Demo</span>"
              Version="<span style="background-color: yellow">1.0.0.0</span>" /&gt;
    &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: yellow">Contoso App</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: yellow">Contoso, Inc</span>&lt;/PublisherDisplayName&gt;
    &lt;Logo&gt;Assets\StoreLogo.png&lt;/Logo&gt;
  &lt;/Properties&gt;
    &lt;Dependencies&gt;
    &lt;TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" /&gt;
  &lt;/Dependencies&gt;
    &lt;Resources&gt;
    &lt;Resource Language="<span style="background-color: yellow">en-US</span>" /&gt;
  &lt;/Resources&gt;
    &lt;Applications&gt;
    &lt;Application Id="<span style="background-color: yellow">ContosoDemo</span>" Executable="<span style="background-color: yellow">ContosoDemo.exe</span>" 
                 EntryPoint="Windows.FullTrustApplication"&gt;
    &lt;uap:VisualElements DisplayName="<span style="background-color: yellow">Contoso Demo</span>" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="<span style="background-color: yellow">Contoso Demo</span>"&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
    &lt;Capabilities&gt;
    &lt;rescap:Capability Name="runFullTrust" /&gt;
  &lt;/Capabilities&gt;
&lt;/Package&gt;
</pre>
</blockquote>

Weitere Informationen zur Datei `AppXManifest.xml` und dem Paketlayout finden Sie im [Thema **App-Paketmanifest** auf MSDN](https://msdn.microsoft.com/en-us/library/windows/apps/br211474.aspx).

Wenn Sie zum Erstellen eines neuen Projekts und zum Migrieren des vorhandenen Codes Visual Studio verwenden, können Sie weitere Informationen in der [MSDN-Dokumentation für die Erstellung eines neuen UWP-Projekts](https://msdn.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal) finden. Sie können den vorhandenen Code in das neue Projekt einfügen, Sie müssen aber wahrscheinlich noch erhebliche Änderungen am Code vornehmen (insbesondere in der Benutzeroberfläche), damit die App als „reine” UWP-App ausgeführt wird. Diese Änderungen werden in diesem Dokument nicht behandelt.

---

## <a name="phase-1-localize-the-application-manifest"></a>Phase 1: Lokalisieren des Anwendungsmanifests

### <a name="step-11-update-strings--assets-in-the-appxmanifest"></a>Schritt 1.1: Aktualisieren von Zeichenfolgen und Objekte in die appxmanifest-Datei

In Phase 0 haben Sie eine einfache `AppXManifest.xml`-Datei für Ihre Anwendung erstellt (basierend auf dem Converter bereitgestellten Werten, die aus der MSI-Datei extrahiert oder manuell in das Paketmanifest eingegeben wurden). Diese enthält jedoch weder lokalisierte Informationen noch werden zusätzliche Features wie Ressourcen für Startkacheln mit hoher Auflösung usw. unterstützt. 

Um sicherzustellen, dass der Name und die Beschreibung Ihrer Anwendung richtig lokalisiert sind, müssen Sie einige Ressourcen in einer Reihe von Ressourcendateien definieren und AppX-Manifest aktualisieren, damit diese Datei auf sie verweist.

**Erstellen einer Standardressourcendatei**

Der erste Schritt ist das Erstellen einer Standardressourcendatei in der Standardsprache (z. B. Englisch (USA)). Sie können dies entweder manuell mit einem Text-Editor oder über den Ressourcen-Designer in Visual Studio vornehmen.

Wenn Sie die Ressourcen manuell erstellen möchten:

0. Erstellen Sie eine XML-Datei mit dem Namen `resources.resw`, und legen Sie sie in einem `Strings\en-us`-Unterordner Ihres Projekts ab. 
 * Verwenden Sie den entsprechenden Code BCP-47, wenn die Standardsprache nicht Englisch (USA) ist. 
0. Fügen Sie in der XML-Datei den folgenden Inhalt hinzu. Ersetzten Sie dabei den <span style="background-color: yellow">hervorgehobenen Text</span> durch den entsprechenden Text in der Standardsprache Ihrer App.

[!Note] Es gibt Einschränkungen für die Länge von einigen dieser Zeichenfolgen. Weitere Informationen finden Sie unter [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements?branch=live).

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, USA</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (EN)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

Wenn Sie den Designer in Visual Studio verwenden möchten:

0. Erstellen der `Strings\en-us` Ordner (oder andere Sprache nach Bedarf) in Ihrem Projekt und fügen eine **neues Element** in den Stammordner des Projekts, verwenden Sie den Standardnamen des `resources.resw`
 * Wählen Sie **Ressourcendatei (.resw)** und nicht **Ressourcenverzeichnis** – ein Ressourcenverzeichnis ist eine Datei, die von XAML-Anwendungen verwendet wird.
0. Geben Sie im Designer die folgenden Zeichenfolgen ein (verwenden Sie dieselben `Names`, ersetzen Sie die `Values` jedoch mit dem entsprechenden Text für Ihre Anwendung):

<img src="images\editing-resources-resw.png"/>

Hinweis: Wenn Sie mit dem Visual Studio-Designer starten, können Sie die XML-Datei jederzeit durch Drücken von `F7` bearbeiten. Wenn Sie jedoch mit einer kleinen XML-Datei starten, *erkennt der Designer die Datei nicht*, weil viele zusätzliche Metadaten fehlen. Dies lässt sich durch Kopieren der XSD-Textbausteininformationen aus einer mit dem Designer erstellten Datei in die manuell bearbeitete XML-Datei beheben. 

**Aktualisieren Sie das Manifest, um die Ressourcen zu verweisen.**

Wenn Sie die Werte in der `.resw` -Datei definiert haben, besteht der nächste Schritt darin, das Manifest zu aktualisieren, um auf die Ressourcenzeichenfolgen zu verweisen. Auch in diesem Fall können Sie eine XML-Datei direkt bearbeiten oder dies durch den Manifest-Designer von Visual Studio vornehmen lassen.

Wenn Sie XML-Code direkt bearbeiten, öffnen Sie die Datei `AppxManifest.xml`, und nehmen Sie die folgenden Änderungen an den <span style="background-color: lightgreen">hervorgehoben Werten</span> vor – verwenden Sie *genau* diesen Text und keinen für Ihre Anwendung spezifischen. Es besteht keine Notwendigkeit, genau diese Ressourcennamen zu verwenden.&mdash;Sie können eigene Ressourcennamen verwenden&mdash;, die jedoch genau mit den Angaben in der Datei `.resw` übereinstimmen müssen. Diese Namen sollten den `Names` entsprechen, die Sie in der `.resw`-Datei erstellt haben, und als Präfix das `ms-resource:`-Schema und den `Resources/`-Namespace aufweisen. 

*Hinweis: viele Elemente des Manifests wurde weggelassen, anhand dieses Codeausschnitts – löschen Sie nicht alles!*

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;Package&gt;
  &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PackageDisplayName</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PublisherDisplayName</span>&lt;/PublisherDisplayName&gt;
  &lt;/Properties&gt;
  &lt;Applications&gt;
    &lt;Application&gt;
      &lt;uap:VisualElements DisplayName="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDisplayName</span>"
        Description="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDescription</span>"&gt;
        &lt;uap:DefaultTile ShortName="<span style="background-color: lightgreen">ms-resource:Resources/TileShortName</span>"&gt;
          &lt;uap:ShowNameOnTiles&gt;
            &lt;uap:ShowOn Tile="square150x150Logo" /&gt;
          &lt;/uap:ShowNameOnTiles&gt;
        &lt;/uap:DefaultTile&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
&lt;/Package&gt;
</pre>
</blockquote>

Wenn Sie den Manifest-Designer von Visual Studio verwenden, öffnen Sie die Datei `Package.appxmanifest`, und ändern Sie die <span style="background-color: lightgreen">hervorgehoben Werte</span> auf der Registerkarte `Application` und der Registerkarte `Packaging`:

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-appx-package-and-verify-its-working"></a>Schritt 1.2: PRI-Datei erstellen, stellen Sie ein AppX-Paket und überprüfen Sie, ob es funktioniert

Sie sollten jetzt in der Lage sein, die `.pri`-Datei zu erstellen und die Anwendung bereitzustellen, um sicherzustellen, dass die richtigen Informationen (in Ihrer Standardsprache) im Startmenü angezeigt werden. 

Wenn Sie in Visual Studio erstellen, drücken Sie einfach `Ctrl+Shift+B`, um das Projekt zu erstellen, und klicken Sie dann mit der rechten Maustaste auf das Projekt, und wählen Sie im Kontextmenü `Deploy` aus. 

Führen Sie bei einer manuellen Erstellung die folgenden Schritte aus, um eine Konfigurationsdatei für `MakePRI`-Tool zu erstellen und die `.pri`-Datei selbst zu generieren (weitere Informationen finden Sie im [Thema **Manuelles Verpacken von Apps** auf MSDN](https://docs.microsoft.com/en-us/windows/uwp/packaging/manual-packaging-root)):

0. Öffnen Sie eine Developer-Eingabeaufforderung über den Ordner `Visual Studio 2015` im Startmenü.
0. Wechseln Sie zum Stammverzeichnis des Projekts (d. h. zu demjenigen, dass die Datei `AppxManifest.xml` und den Ordner `Strings` enthält).
0. Geben Sie den folgenden Befehl ein, und ersetzen Sie dabei „contoso_demo.xml” mit einem für Ihr Projekt geeigneten Namen und „en-US” mit der Standardsprache Ihrer App (oder lassen Sie es nach Bedarf auf en-US). Beachten Sie, dass die XML-Datei im übergeordneten Verzeichnis (**nicht** im Projektverzeichnis) erstellt wird, da sie nicht Teil der Anwendung ist. (Sie können ein beliebiges anderes Verzeichnis auswählen, müssen dies aber in zukünftigen Befehlen verwenden.)

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
```

0. Sie können `makepri createconfig /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
 * `/cf` Legt den Dateinamen der Konfiguration (die Ausgabe dieses Befehls)
 * `/dq` Legt die Standard-Qualifizierer, in diesem Fall die Sprache `en-US`
 * `/pv` Legt die Version der Plattform, in diesem Fall Windows 10
 * `/o` Legt fest, damit die Ausgabedatei überschrieben, falls vorhanden
0. Sie verfügen jetzt über eine Konfigurationsdatei. Führen Sie `MakePRI` erneut aus, um auf der Festplatte tatsächlich nach Ressourcen zu suchen und diese in einer PRI-Datei zu verpacken. Ersetzen Sie „contoso_demop.xml” mit dem XML-Dateinamen, den Sie im vorherigen Schritt verwendet haben, und legen Sie das übergeordnete Verzeichnis für Eingabe und Ausgabe fest: 

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. Sie können `makepri new /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
 * `/pr` Legt das Stammverzeichnis des Projekts (in diesem Fall wird das aktuelle Verzeichnis)
 * `/cf` Legt den Dateinamen Konfiguration im vorherigen Schritt erstellt haben
 * `/of` Legt die Ausgabedatei 
 * `/mf` erstellt eine Zuordnungsdatei, (also wir Ausschließen von Dateien in das Paket in einem späteren Schritt können)
 * `/o` Legt fest, damit die Ausgabedatei überschrieben, falls vorhanden
0. Sie verfügen jetzt über eine `.pri`-Datei mit den Standardsprachressourcen (z. B. en-US). Um sicherzustellen, dass es ordnungsgemäß funktioniert hat, können Sie den folgenden Befehl ausführen:

    `makepri dump /if ..\resources.pri /of ..\resources /o`
0. Sie können `makepri dump /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
 * `/if` Legt den Dateinamen der Eingabe fest 
 * `/of` Legt den Dateinamen für die Ausgabe fest (`.xml` werden automatisch angefügt)
 * `/o` Legt fest, damit die Ausgabedatei überschrieben, falls vorhanden
0. Zuletzt können Sie `..\resources.xml` in einem Text-Editor öffnen und überprüfen, ob Ihre `<NamedResource>`-Werte (z. B. `ApplicationDescription` und `PublisherDisplayName`) zusammen mit den `<Candidate>`-Werten für die ausgewählte Standardsprache aufgeführt werden (am Anfang der Datei befinden sich auch weitere Inhalte; diese können Sie vorerst ignorieren).

Wenn Sie möchten, können Sie die Zuordnungsdatei `..\resources.map.txt` öffnen, um zu überprüfen, ob sie die für Ihr Projekt benötigten Dateien enthält (einschließlich der PRI-Datei, die nicht Teil des Projektverzeichnisses ist). Besonders wichtig ist, dass die Zuordnungsdatei *keinen* Verweis auf Ihre `resources.resw`-Datei enthält, da der Inhalt dieser Datei bereits in die PRI-Datei eingebettet wurde. Sie enthält jedoch andere Ressourcen wie die Dateinamen Ihrer Bilder.

**Erstellen und Signieren des Pakets**

Nachdem die PRI-Datei jetzt erstellt wurde, können Sie das Paket erstellen und signieren:

0. Um das App-Paket zu erstellen, führen Sie den folgenden Befehl aus, und ersetzen Sie dabei `contoso_demo.appx` mit dem Namen der AppX Datei, die Sie erstellen möchten. Wählen Sie ein anderes Verzeichnis für die `.AppX`-Datei (in diesem Beispiel wird das übergeordnete Verzeichnis verwendet; sie können jedes beliebige Verzeichnis verwenden, jedoch **nicht** das Projektverzeichnis):

    `makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o`
0. Sie können `makeappx pack /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
 * `/m` Legt die Manifest-Datei an
 * `/f` Legt die Zuordnung zu verwendende Datei an (im vorherigen Schritt erstellt) 
 * `/p` Legt die Ausgabe Paketname
 * `/o` Legt fest, damit die Ausgabedatei überschrieben, falls vorhanden
0. Wenn das Paket erstellt wurde, muss es signiert werden. Die einfachste Möglichkeit, ein Signaturzertifikat abrufen ist, indem Sie ein leeres universelle Windows-Projekt in Visual Studio erstellen, und kopieren die `.pfx` -Datei erstellt, aber Sie können manuell mit erstellen die `MakeCert` und `Pvk2Pfx` Hilfsprogramme wie beschrieben in <a href="https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832.aspx"> Vorgehensweise: erstellen ein app-Pakets, das Signaturzertifikat</a>, ein Thema auf MSDN. 
 * **Wichtig:** Wenn Sie manuell ein Signaturzertifikat erstellen, stellen Sie sicher, dass Sie die Dateien in einem anderen Verzeichnis platziert haben, als Ihre Source-Projekt oder Ihrer Paketquelle, andernfalls als Bestandteil des Pakets, einschließlich des privaten Schlüssels abrufen kann!
0. Verwenden Sie zum Signieren des Pakets den folgenden Befehl. Beachten Sie, dass der im Element `Identity` der Datei `AppxManifest.xml` angegebene `Publisher` mit dem `Subject` des Zertifikats übereinstimmen muss (hierbei handelt es sich **nicht** um das Element `<PublisherDisplayName>`; dieses ist der lokalisierte Anzeigename, der den Benutzern angezeigt wird). Ersetzen Sie wie gewohnt die `contoso_demo...`-Dateinamen mit den Namen für Ihr Projekt, und (**sehr wichtig**) stellen Sie sicher, dass die `.pfx`-Datei sich nicht im aktuellen Verzeichnis befindet (andernfalls wäre sie als Teil Ihres Pakets erstellt worden, einschließlich des privaten Signaturschlüssels!):

    `signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx`
0. Sie können `signtool sign /?` eingeben, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
 * `/fd` legt der Dateidigest-Algorithmus (SHA256 ist die Standardeinstellung für AppX)
 * `/a` wird automatisch das beste Zertifikat auswählen.
 * `/f` Gibt die Eingabedatei, die das Signaturzertifikat enthält.

Zuletzt können Sie auf die Datei `.appx` doppelklicken, um diese zu installieren. Wenn Sie lieber die Befehlszeile verwenden, können Sie eine PowerShell-Eingabeaufforderung öffnen, zum Verzeichnis mit dem Paket wechseln, und Folgendes eingeben (wobei Sie `contoso_demo.appx` mit Ihrem Paketnamen ersetzen müssen):

```CMD
    add-appxpackage contoso_demo.appx
```

Wenn Sie Fehlermeldungen erhalten, dass das Zertifikat nicht als vertrauenswürdig eingestuft wird, stellen Sie sicher, dass es zum lokalen Speicher hinzugefügt wurde (**nicht** zum Benutzerspeicher). Um das Zertifikat zum lokalen Speicher hinzufügen, können Sie entweder die Befehlszeile oder Windows Explorer verwenden.

Verwenden der Befehlszeile:

0. Führen Sie eine Visual Studio 2015-Befehlszeile als Administrator aus.
0. Wechseln Sie zu dem Verzeichnis mit der `.cer`-Datei (stellen Sie sicher, dass dies nicht das Verzeichnis Ihrer Quelle oder Ihres Projekts ist!)
0. Geben Sie folgenden Befehl ein, und ersetzen Sie dabei `contoso_demo.cer` mit Ihrem Dateinamen:

    `certutil -addstore TrustedPeople contoso_demo.cer`
0. Sie können `certutil -addstore /?` ausführen, um zu sehen, was die einzelnen Parameter bedeuten. Kurz zusammengefasst:
 * `-addstore` Fügt ein Zertifikat in einen Zertifikatspeicher
 * `TrustedPeople` Gibt an, den Speicher, in dem das Zertifikat abgelegt ist

Verwenden von Windows Explorer:

0. Navigieren Sie zum Ordner mit der `.pfx`-Datei
0. Doppelklicken Sie auf die `.pfx`-Datei. Der **Zertifikatimport-Assistent** sollte angezeigt werden.
0. Wählen Sie `Local Machine` , und klicken Sie auf `Next`
0. Akzeptieren Sie die Eingabeaufforderung zur berechtigungserweiterung User Account Control-Administrator aus, wenn es angezeigt wird, und klicken Sie auf `Next`
0. Geben Sie das Kennwort für den privaten Schlüssel, sofern vorhanden, und klicken Sie auf `Next`
0. Wählen Sie `Place all certificates in the following store`
0. Klicken Sie auf `Browse`, und wählen Sie den Ordner `Trusted People` (**nicht** „Vertrauenswürdige Herausgeber”)
0. Klicken Sie auf `Next` und klicken Sie dann `Finish`

Versuchen Sie nach dem Hinzufügen des Zertifikats zum `Trusted People`-Speicher erneut, das Paket zu installieren.

Ihre App sollte jetzt in der Liste „Alle Apps” im Startmenü angezeigt werden, mit den richtigen Informationen aus der Datei `.resw` / `.pri`. Wenn Sie eine leere Zeichenfolge oder die Zeichenfolge `ms-resource:...` sehen, ist ein Fehler aufgetreten – überprüfen Sie Ihre Bearbeitungen, und stellen Sie sicher, dass sie richtig sind. Wenn Sie im Startmenü Ihrer App mit der rechten Maustaste klicken, können Sie sie als Kachel anheften und überprüfen, ob dort die richtigen Informationen angezeigt werden.

### <a name="step-13-add-more-supported-languages"></a>Schritt 1.3: Fügen Sie weitere unterstützten Sprachen hinzu

Nachdem die Änderungen am AppX-Manifest vorgenommen und die ursprüngliche `resources.resw`-Datei erstellt wurde, ist es ganz einfache, zusätzliche Sprachen hinzuzufügen.

**Erstellen Sie zusätzliche lokalisierte Ressourcen**

Erstellen Sie zunächst die Werte für die zusätzlich lokalisierten Ressourcen. 

Erstellen Sie im Ordner `Strings` zusätzliche Ordner für jede Sprache, die Sie unterstützen. Verwenden Sie dafür den entsprechenden BCP-47-Code (z. B. `Strings\de-DE`). Erstellen Sie in jedem dieser Ordner eine `resources.resw`-Datei (mithilfe von XML-Editor oder dem Visual Studio-Designer), die die Werte der übersetzten Ressourcen enthält. Es wird vorausgesetzt, dass die lokalisierten Zeichenfolgen bereits verfügbar sind. Sie müssen diese nur in die Datei `.resw` kopieren; der Übersetzungsschritt selbst wird in diesem Dokument nicht behandelt. 

Die Datei `Strings\de-DE\resources.resw` könnte beispielsweise wie folgt aussehen, der <span style="background-color: yellow">hervorgehobene Text</span> wurde von `en-US` geändert:

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, DE</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (DE)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

Für die folgenden Schritte gilt die Annahme, dass Sie sowohl für `de-DE` als auch für `fr-FR` Ressourcen hinzugefügt haben, dasselbe Muster kann jedoch für alle Sprachen befolgt werden.

**Aktualisieren Sie die AppX-Manifests auf Liste unterstützter Sprachen**

AppX-Manifest muss aktualisiert werden, um die von der App unterstützten Sprachen aufzuführen. Der Desktop App Converter fügt die Standardsprache hinzu, die anderen müssen jedoch explizit hinzugefügt werden. Aktualisieren Sie beim direkten Bearbeiten der Datei `AppxManifest.xml` den `Resources`-Knoten wie folgt, fügen Sie dabei so viele Elemente hinzu, wie Sie benötigen, und ersetzen die <span style="background-color: yellow">entsprechenden Sprachen, die Sie unterstützen</span>. Stellen Sie dabei außerdem sicher, dass der erste Eintrag in der Liste die Standard- (Fallback-)Sprache ist. In diesem Beispiel ist der Standard Englisch (USA) mit zusätzlicher Unterstützung für Deutsch (Deutschland) und Französisch (Frankreich):

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">EN-US</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">DE-DE</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">FR-FR</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

Wenn Sie Visual Studio verwenden, sollte Sie nichts weiter tun müssen; wenn Sie `Package.appxmanifest` betrachten, sollten Sie den speziellen Wert <span style="background-color: yellow">x generate</span> sehen. Dieser bewirkt, dass der Buildprozess die Sprachen einfügt, die sich in Ihrem Projekt befinden (basierend auf den Ordnern, die mit den BCP-47 Codes benannt sind). Beachten Sie, dass dies kein gültiger Wert für echte Appx-Manifeste ist. Es funktioniert nur für Visual Studio-Projekte:

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">x-generate</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

**Mit den lokalisierten Werten erneut erstellen.**

Sie können Ihre Anwendung jetzt erneut erstellen und bereitstellen, und wenn Sie Ihre bevorzugte Sprache in Windows ändern, sollten die neu lokalisierten Werte im Startmenü angezeigt werden (Informationen zum Ändern Ihrer Sprache finden Sie unten).

Bei Visual Studio können Sie wieder einfach `Ctrl+Shift+B` zum Erstellen verwenden, und mit der rechten Maustaste auf das Projekt klicken, um es bereitzustellen (`Deploy`).

Wenn Sie das Projekt manuell erstellen, führen Sie die gleichen Schritte wie oben beschrieben aus, fügen Sie jedoch die zusätzlichen Sprachen getrennt durch Unterstriche zur Liste der Standardqualifizierer (`/dq`) hinzu, wenn Sie die Konfigurationsdatei erstellen. Beispiel: Unterstützung der Ressourcen für Englisch, Deutsch und Französisch, die im vorherigen Schritt hinzugefügt wurden:

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Dadurch wird eine PRI-Datei erstellt, die alle angegebenen Sprachen enthält, die Sie einfach zum Testen verwenden können. Wenn die Gesamtgröße der Ressourcen gering ist, oder Sie nur eine geringe Anzahl von Sprachen unterstützen, kann dies für den Versand Ihrer App ausreichend sein. Wenn Sie jedoch von einer möglichst geringen Installations- oder Downloadgröße Ihrer Ressource profitieren möchten, müssen Sie separate Sprachpakete erstellen.

**Testen Sie mit die lokalisierte Werte**

Zum Testen der neuen lokalisierten Änderungen müssen Sie einfach eine neue bevorzugte UI-Sprache zu Windows hinzufügen. Es ist nicht erforderlich, Language Packs herunterzuladen, das System neu zu starten, oder die gesamte Windows-UI in einer fremden Sprache anzuzeigen. 

0. Führen Sie die `Settings`-App aus (`Windows + I`).
0. Gehe zu `Time & language`
0. Gehe zu `Region & language`
0. Klicken Sie auf `Add a language`
0. Geben (oder wählen) Sie die gewünschte Sprache ein (z. B. `Deutsch` oder `German`)
 * Wenn untergeordnete Sprachen vorhanden sind, wählen Sie die gewünschte Sprache aus (z. B. `Deutsch / Deutschland`)
0. Wählen Sie die neue Sprache in der Liste der Sprachen aus.
0. Klicken Sie auf `Set as default`

Öffnen Sie nun das Startmenü und suchen Sie Ihre Anwendung, und die lokalisierten Werte für die ausgewählte Sprache (andere Apps können auch lokalisiert angezeigt werden) sollten angezeigt werden. Wenn Sie den lokalisierten Namen nicht sofort sehen, warten Sie einige Minuten, bis der Cache des Startmenüs aktualisiert wird. Um auf Ihre Muttersprache zurückzuwechseln, stellen Sie sie als Standardsprache in der Liste der Sprachen ein. 

### <a name="step-14-localizing-more-parts-of-the-appx-manifest-optional"></a>Schritt 1.4: Lokalisieren von mehreren Teilen des AppX-Manifests (optional)

Andere Abschnitte des AppX-Manifests können lokalisiert werden. Wenn Ihre Anwendung beispielsweise Dateierweiterungen bearbeitet, dann sollte sie eine `windows.fileTypeAssociation`-Erweiterung im Manifest aufweisen, die den <span style="background-color: lightgreen">grün hervorgehobenen Text gemäß der Abbildung verwendet</span> (da es auf Ressourcen verweisen wird), und den <span style="background-color: yellow">gelb hervorgehobenen Text</span> mit Informationen ersetzt, die für Ihre Anwendung spezifisch sind:

<blockquote>
<pre>
&lt;Extensions&gt;
  &lt;uap:Extension Category="windows.fileTypeAssociation"&gt;
    &lt;uap:FileTypeAssociation Name="default"&gt;
      &lt;uap:DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeDisplayName</span>&lt;/uap:DisplayName&gt;
      &lt;uap:Logo&gt;<span style="background-color: yellow">Assets\StoreLogo.png</span>&lt;/uap:Logo&gt;
      &lt;uap:InfoTip&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeInfoTip</span>&lt;/uap:InfoTip&gt;
      &lt;uap:SupportedFileTypes&gt;
        &lt;uap:FileType ContentType="<span style="background-color: yellow">application/x-contoso</span>"&gt;<span style="background-color: yellow">.contoso</span>&lt;/uap:FileType&gt;
      &lt;/uap:SupportedFileTypes&gt;
    &lt;/uap:FileTypeAssociation&gt;
  &lt;/uap:Extension&gt;
&lt;/Extensions&gt;
</pre>
</blockquote>

Darüber hinaus können Sie diese Informationen mit dem Visual Studio-Manifest-Designer anhand der Registerkarte `Declarations` hinzufügen. Achten Sie dabei auf die <span style="background-color: lightgreen">hervorgehobenen Werte</span>:

<p><img src="images\editing-declarations-info.png"/></p>

Fügen Sie jetzt die entsprechenden Ressourcennamen zu den einzelnen `.resw`-Dateien hinzu und ersetzen Sie somit den <span style="background-color: yellow">hervorgehobenen Text</span> mit dem entsprechenden Text Ihrer App (denken Sie daran, dass Sie diesen Vorgang für *jede unterstützte Sprache* durchführen müssen!):

<blockquote>
<pre>
... existing content...

&lt;data name="FileTypeDisplayName"&gt;
  &lt;value&gt;<span style="background-color: yellow">Contoso Demo File</span>&lt;/value&gt;
&lt;/data&gt;
&lt;data name="FileTypeInfoTip"&gt;
  &lt;value&gt;<span style="background-color: yellow">Files used by Contoso Demo App</span>&lt;/value&gt;
&lt;/data&gt;
</pre>
</blockquote>

Dies wird dann in Teilen der Windows-Shell angezeigt, z. B. im Datei-Explorer:

<p><img src="images\file-type-tool-tip.png"/></p>

Erstellen und testen Sie das Paket wie zuvor. Führen Sie dabei alle neuen Szenarien aus, die die neuen UI-Zeichenfolgen anzeigen sollten.

- - -

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Phase 2: Verwenden von MRT zum Erkennen und Suchen von Ressourcen

Im vorherigen Abschnitt wurde gezeigt, wie MRT verwendet wird, um die Manifestdatei der App zu lokalisieren, damit der Name der App und andere Metadaten durch Windows-Shell richtig angezeigt werden. Keine Code-Änderungen waren dafür erforderlich. Es ist lediglich die Verwendung von `.resw`-Dateien und einigen weiteren Tools erforderlich. In diesem Abschnitt wird Ihnen gezeigt, wie Sie MRT zum Suchen von Ressourcen in Ihren vorhandenen Ressourcenformaten und Ihren vorhandenen Ressourcenbehandlungscode mit minimalen Änderungen verwenden.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Annahmen über vorhandenes Datei-Layout und Anwendungscode

Da es viele Möglichkeiten gibt, Win32-Desktop-Apps zu lokalisieren, wird dieses Dokument einige vereinfachende Annahmen über die vorhandene App-Struktur machen, die Sie zum Zuordnen Ihrer Umgebung benötigen. Möglicherweise müssen Sie einige Änderungen an Ihre vorhandene Codebasis oder Ressourcen-Layout vornehmen, um den Anforderungen der MRT zu entsprechen. Diese gehören nicht zum Umfang dieses Dokuments.

**Ressource Dateilayout**

Dieses Whitepaper setzt voraus, dass all Ihre lokalisierten Ressourcen die gleichen Dateinamen aufweisen (z. B. `contoso_demo.exe.mui` oder `contoso_strings.dll` oder `contoso.strings.xml`), dass sie jedoch in unterschiedlichen Ordnern mit BCP-47 Namen (`en-US`, `de-DE` usw.) abgelegt wurden. Es spielt keine Rolle, wie viele Ressourcendateien, die Sie, wie ihre Namen verwendet werden, welche ihre-Dateiformate / verknüpften APIs usw. sind. Das einzige, der wichtig ist, die alle *logische* Ressource weist den gleichen Dateinamen (jedoch in einem anderen platziert *physischen* Verzeichnis). 

Gegenbeispiel: wenn Ihre Anwendung eine flache Dateistruktur mit einem einzigen `Resources`-Verzeichnis verwendet, das die Dateien `english_strings.dll` und `french_strings.dll` enthält, würde sie sich nicht gut zu MRT zuordnen lassen. Eine bessere Struktur wäre ein `Resources`-Verzeichnis mit Unterverzeichnissen und den Dateien `en\strings.dll` und `fr\strings.dll`. Es ist auch möglich, den gleichen Basisdateinamen mit eingebetteten Qualifizierern zu verwenden, z. B. `strings.lang-en.dll` und `strings.lang-fr.dll`. Die Verwendung von Verzeichnissen mit Sprachcodes ist jedoch konzeptionell einfacher. Daher konzentrieren wir uns auf diese Vorgehensweise.

**Hinweis:** Es ist weiterhin möglich, MRT und die Vorteile von AppX-Paketen zu verwenden, selbst wenn Sie diese Dateinamenskonvention nicht befolgen können – es macht nur mehr Arbeit.

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

**Code zum Laden von Ressourcen**

In diesem Whitepaper wird angenommen, dass Sie an einer bestimmten Stelle in Ihrem Code die Datei finden möchten, die eine lokalisierte Ressource enthält, und dass Sie diese laden und verwenden möchten. Die APIs, die zum Laden der Ressourcen verwendet werden und die APIs, die zum Extrahieren der Ressourcen verwendet werden, sind nicht wichtig. Im Pseudocode gibt es im Wesentlichen drei Schritte:

```
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
```

MRT erfordert nur das Ändern der ersten beiden Schritte in diesem Prozess – wie Sie die besten Ressourcen festlegen und wie Sie sie finden. Es ist nicht erforderlich, das Laden oder die Verwendung der Ressourcen zu ändern (obwohl Ihnen dafür Hilftsmittel bereitgestellt, wenn Sie davon profitieren möchten).
 
Die Anwendung kann z. B. die Win32-API `GetUserPreferredUILanguages`, die CRT-Funktion `sprintf` und die Win32-API `CreateFile` verwenden, um die drei oben genannten Pseudocode-Funktionen zu ersetzen, und dann die Textdatei auf `name=value`-Paare manuell analysieren. (Die Details sind nicht wichtig; dies dient lediglich der Veranschaulichung der Tatsache, dass MRT keine Auswirkung auf die Techniken hat, die für die Verarbeitung der Ressourcen verwendet wird, nachdem sie gefunden wurden).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Schritt 2.1: Änderungen am Code MRT verwenden, um Dateien zu suchen.

Das Wechseln zwischen Codes für die Verwendung von MRT zum Suchen von Ressourcen ist nicht schwierig. Es erfordert die Verwendung einer Handvoll von WinRT-Typen und ein paar Codezeilen. Die Haupttypen, die Sie verwenden werden, lauten wie folgt:

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), das den derzeit aktiven Satz von Qualifiziererwerten enthält (Sprache, Skalierungsfaktor usw.).
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager), (die WinRT-Version, nicht die .NET-Version) der den Zugriff auf alle Ressourcen aus der PRI-Datei ermöglicht.
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap), die eine bestimmte Teilmenge der Ressourcen in der PRI-Datei (in diesem Beispiel stehen die dateibasierten Ressourcen im Vergleich zu den Zeichenfolgenressourcen) darstellt.
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), die eine logische Ressource mit allen möglichen Kandidaten darstellt.
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), die eine konkrete Kandidatenressource darstellt. 

In Pseudocode würden Sie eine bestimmte Ressource würden Sie wie folgt einen gegebenen Ressourcen-Dateinamen (z. B. `UICommands\ui.txt` im obigen Beispiel) auflösen:

```
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
```

Beachten Sie insbesondere, dass der Code **nicht** einen bestimmten Sprachordner anfordert – z. B. `UICommands\en-US\ui.txt` – auch wenn die Dateien dementsprechend auf dem Datenträger gespeichert sind. Stattdessen wird nach dem *logischen* Dateinamen `UICommands\ui.txt` gefragt und der Code ist von MRT abhängig, um die entsprechende Datei auf dem Datenträger in einem der Sprachverzeichnisse zu finden.

Ab dieser Stelle kann die Beispiel-App weiterhin `CreateFile` zum Laden der `absoluteFileName` und Analysieren der `name=value`-Paare wie zuvor verwenden; diese Logik muss in der App nicht geändert werden. Beim Schreiben in C# oder C++/CX ist der tatsächliche Code ist nicht viel komplizierter als dieser (und in der Tat können viele der temporären Variablen ausgelassen werden) – weitere Informationen finden Sie unten im Abschnitt **Laden der .NET-Ressourcen**. C++/WRL-basierte Anwendungen werden aufgrund der COM-basierten APIs auf niedriger Ebene zum Aktivieren und Aufrufen der WinRT-APIs komplexer sein, aber die grundlegenden Schritte sind identisch. Weitere Informationen finden Sie unten im Abschnitt **Laden von Win32-MUI-Ressourcen**.

**Das Laden von Ressourcen für .NET**

Da .NET über einen integrierten Mechanismus für das Suchen und Laden von Ressourcen (als „Satellitenassemblys” bekannt) verfügt, muss für kein expliziter Code wie im obigen synthetischen Beispiel ersetzt werden – in. NET benötigen Sie lediglich die Ressourcen-DLL-Dateien in den entsprechenden Verzeichnissen, und sie werden automatisch für Sie gefunden. Wenn eine App als ein AppX-Paket anhand von Ressourcenpaketen verpackt wird, ist die Verzeichnisstruktur etwas anders – statt die Ressourcenverzeichnisse als Unterverzeichnisse des Hauptverzeichnisses der Anwendung festzulegen, werden sie als Peers vom Hauptverzeichnis festgelegt (oder sie sind gar nicht vorhanden, wenn der Benutzer die Sprache nicht in seinen Präferenzen aufgelistet hat). 

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

```C#
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

```C#
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

Die .NET-Laufzeit löst das Ereignis `AssemblyResolve` aus, wenn die Ressourcen-DLLs nicht gefunden werden kann. An diesem Punkt sucht der bereitgestellten Ereignishandler die gewünschte Datei über MRT und gibt die Assembly zurück.

**Hinweis:** Wenn Ihre App bereits einen `AssemblyResolve`-Handler für andere Zwecke besitzt, müssen Sie den ressourcenauflösenden Code in den vorhandenen Code integrieren.

**Das Laden von Win32-MUI-Ressourcen**

Das Laden von Win32-MUI-Ressourcen ist im Wesentlichen identisch mit dem Laden von .NET-Satellitenassemblys, es wird jedoch entweder der Code C++/CX oder C++/WRL verwendet. Die Verwendung von C++/CX ermöglicht einen viel einfacheren Code, der dem oben aufgeführten Code C# sehr ähnelt, jedoch C++-Erweiterungen, Compiler-Switches und zusätzlichen Laufzeitaufwand verwendet, die Sie vielleicht vermeiden möchten. Wenn dies der Fall ist, stellt C++/WRL eine Lösung mit deutlich geringeren Auswirkungen dar – für den Preis von ausführlicherem Code. Wenn Sie jedoch mit der ATL-Programmierung (oder mit COM im Allgemeinen) vertraut sind, sollte WRL ihnen vertraut vorkommen. 

Die folgende Beispielfunktion veranschaulicht, wie C++/WRL verwendet werden kann, um eine bestimmte Ressourcen-DLL zu laden und ein `HINSTANCE` zurückzugeben, das zum Laden weiterer Ressourcen mithilfe von Win32-Ressourcen-APIs verwendet werden kann. Beachten Sie, dass dieser Code auf der aktuellen Sprache des Benutzers beruht – anders als bei dem C#-Beispiel, bei dem der `ResourceContext` explizit mit der Sprache initialisiert wird, die von der .NET-Laufzeit angefordert wird.

```CPP
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

0. Nehmen Sie ein vorhandenes umfangreiches Paket, und führen Sie es im [Bündel-Generator-Tool](https://aka.ms/bundlegen) aus, um automatisch Ressourcenpakete zu erstellen. Dies ist der bevorzugte Ansatz, wenn Sie über ein Buildsystem verfügen, das bereits ein umfangreiches Paket erstellt, und Sie dieses im Nachhinein verarbeiten möchten, um die Ressourcenpakete zu erstellen.
0. Erzeugen Sie die einzelnen Ressourcenpakete direkt, und integrieren Sie sie in ein Bündel. Dies ist der bevorzugte Ansatz, wenn Sie mehr Kontrolle über Ihr Buildsystem haben und die Pakete direkt erstellen können.

### <a name="step-31-creating-the-bundle"></a>Schritt 3.1: Erstellen das Paket

**Verwenden das Bundle-Generator-tool**

Um das Bündel-Generator-Tool zu verwenden, muss die für das Paket erstellte PRI-Konfigurationsdatei manuell aktualisiert werden, um den Abschnitt `<packaging>` zu entfernen.

Wenn Sie Visual Studio verwenden, finden Sie im [Thema **Sicherstellen, dass Ressourcen auf einem Gerät installiert sind...** auf MSDN](https://msdn.microsoft.com/en-us/library/dn482043.aspx) Informationen dazu, wie Sie alle Sprachen in das Hauptpaket integrieren, indem Sie die Dateien `priconfig.packaging.xml` und `priconfig.default.xml` erstellen.

Wenn Sie Dateien manuell bearbeiten, gehen Sie folgendermaßen vor: 

0. Erstellen Sie die Konfigurationsdatei auf die gleiche Weise wie zuvor, indem Sie den richtigen Pfad, den richtigen Dateinamen und die Sprachen ersetzen:

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o`
0. Öffnen Sie die erstellte `.xml`-Datei manuell, und löschen Sie den gesamten Abschnitt `&lt;packaging&rt;` (alles andere muss jedoch intakt bleiben):

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8" standalone="yes" ?&gt; 
&lt;resources targetOsVersion="10.0.0" majorVersion="1"&gt;
  &lt;!-- Packaging section has been deleted... --&gt;
  &lt;index root="\" startIndexAt="\"&gt;
    &lt;default&gt;
    ...
    ...
</pre>
</blockquote>

0. Erstellen Sie die `.pri`-Datei und das `.appx`-Paket wie zuvor, indem Sie die aktualisierte Konfigurationsdatei und die entsprechenden Verzeichnis- und Dateinamen verwenden (oben finden Sie weitere Informationen zu diesen Befehlen):

```CMD
makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
```

0. Nachdem das Paket erstellt wurde, verwenden Sie den folgenden Befehl, um das Bündel zu erstellen, indem Sie das richtige Verzeichnis und die richtigen Dateinamen verwenden:

    `BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo`

Nun können Sie mit dem letzten Schritt fortfahren, der Signierung (siehe unten).

**Manuelles Erstellen von Ressourcenpaketen**

Das manuelle Erstellen von Ressourcenpaketem erfordert die Ausführung eines etwas anderen Satzes von Befehlen, um eigene `.pri`- und `.appx`-Dateien zu erstellen. Diese sind den Befehlen ähnlich, die oben im Zusammenhang mit der Erstellung von FAT-Paketen beschrieben wurden. Daher werden sie nur wenig erklärt. Hinweis: Alle Befehle wird davon ausgegangen, dass das aktuelle Verzeichnis des Verzeichnisses ist, enthält die `AppXManifest.xml` Datei, aber alle Dateien in das übergeordnete Verzeichnis (können ein anderes Verzeichnis aus, wenn erforderlich, aber Sie das Verzeichnis des Projekts mit einem der verunreinigen sollte nicht platziert werden Diese Dateien). Wie immer, ersetzen Sie die „Contoso“-Dateinamen durch eigene Dateinamen.

0. Verwenden Sie den folgenden Befehl, um eine Konfigurationsdatei zu erstellen, die **nur** die Standardsprache als den Standardqualifizierer nennt, in diesem Fall `en-US`:

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o`
0. Erstellen Sie eine `.pri`- und `.map.txt`-Standarddatei für das Hauptpaket sowie einen zusätzlichen Satz von Dateien für jede Sprache in Ihrem Projekt. Verwenden Sie hierzu den folgenden Befehl:

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. Verwenden Sie den folgenden Befehl, um das Hauptpaket zu erstellen (das den ausführbaren Code und die Standardsprachressourcen enthält). Wie immer, ändern Sie den Namen wie gewünscht. Sie sollten das Paket jedoch in einem eigenen Verzeichnis platzieren, um das Erstellen des Bündels zu vereinfachen (dieses Beispiel verwendet das Verzeichnis `..\bundle`):

    `makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o`
0. Nachdem das Hauptpaket erstellt wurde, verwenden Sie den folgenden Befehl einmal für jede weitere Sprache (d. h., Sie wiederholen diesen Befehl für jede Sprachzuordnungsdatei, die im vorherigen Schritt generiert wurde). Auch hier sollten Sie die Ausgabe in einem separaten Verzeichnis platzieren (in demselben Verzeichnis wie das Hauptpaket). Beachten Sie, dass die Sprache **sowohl** in der Option `/f` als auch in der Option `/p` angegeben ist, sowie die Verwendung des neuen Arguments `/r` (das anzeigt, dass ein Ressourcenpaket gewünscht wird):

    `makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o`
0. Kombinieren Sie alle Pakete aus dem Bündelverzeichnis in einer einzigen `.appxbundle`-Datei. Die neue Option `/d` gibt das Verzeichnis für alle Dateien im Bündel an (daher wurden die `.appx`-Dateien im vorherigen Schritt in ein eigenes Verzeichnis platziert):

    `makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o`

Der letzte Schritt beim Erstellen des Pakets ist das Signieren.

### <a name="step-32-signing-the-bundle"></a>Schritt 3.2: Das Paket signieren

Nachdem Sie die `.appxbundle`-Datei erstellt haben (entweder über das Bündel-Generator-Tool oder manuell), erhalten Sie eine einzelne Datei, die das Hauptpaket sowie alle Ressourcenpakete enthält. Der letzte Schritt besteht im Signieren der Datei, damit sie von Windows installiert wird:

    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle

Dies erzeugt einen signiertes `.appxbundle` -Datei, die das Hauptpaket und alle ressourcenpakete für die sprachspezifischen enthält. Wie bei einer Paketdatei werden durch Doppelklicken auf diese Datei die App und alle relevanten Sprachpakete installiert, abhängig von den Windows-Spracheinstellungen des Benutzers.

## <a name="related-topics"></a>Verwandte Themen

* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md)