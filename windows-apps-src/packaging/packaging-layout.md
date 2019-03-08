---
title: Paketerstellung mit dem Verpackungslayout
description: Das Verpackungslayout ist ein Dokument, das die Verpackungsstruktur der App beschreibt. Es gibt die Bündel einer App („primär” und „optional”), die Pakete in den Bündeln sowie die Dateien in den Paketen an.
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, verpackung, paketlayout, bestandspaket
ms.localizationpriority: medium
ms.openlocfilehash: 3e54b74cf3052fdeb5b70cc90f59ab0ea59aef76
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627655"
---
# <a name="package-creation-with-the-packaging-layout"></a>Paketerstellung mit dem Verpackungslayout  

Mit der Einführung von Bestandspaketen stehen Entwicklern nun die Tools zur Verfügung, um mehr Pakete zusätzlich zu mehr Pakettypen zu erstellen. Wenn eine App größer und komplexer wird, besteht sie oft aus mehreren Paketen, und es wird immer schwieriger, diese Pakete zu verwalten (besonders wenn Sie außerhalb von Visual Studio arbeiten und Mapping-Dateien verwenden). Zur Vereinfachung der Verwaltung der Verpackungsstruktur einer App können Sie das von MakeAppx.exe unterstützte Verpackungslayout verwenden. 

Das Verpackungslayout ist ein XML-Dokument, das die Verpackungsstruktur der App beschreibt. Es gibt die Bündel einer App („primär” und „optional”), die Pakete in den Bündeln sowie die Dateien in den Paketen an. Dateien können aus verschiedenen Ordnern, Laufwerken und Netzwerkadressen ausgewählt werden. Platzhalter können zum Auswählen oder Ausschließen von Dateien verwendet werden.

Nachdem das Verpackungslayout für eine App eingerichtet wurde, wird es zusammen mit MakeAppx.exe verwendet, um alle Pakete für eine App in einem einzigen Befehlszeilenaufruf zu erstellen. Das Verpackungslayout kann bearbeitet werden, um die Paketstruktur entsprechend Ihren Bereitstellungsanforderungen anzupassen. 


## <a name="simple-packaging-layout-example"></a>Beispiel für ein einfaches Verpackungslayout

Hier sehen Sie ein Beispiel für ein einfaches Verpackungslayout:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>
    
    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>

  </PackageFamily>
</PackagingLayout>
```

Analysieren wir dieses Beispiel, um zu verstehen, wie es funktioniert.

### <a name="packagefamily"></a>PackageFamily
Erstellt dieses Paket Layout eine einzelnen Flatfile-app-Paketdatei mit x X64-Architektur und ein "Medien" Asset-Paket. 

Das **PackageFamily**-Element wird verwendet, um ein App-Bündel zu definieren. Sie müssen das **ManifestPath**-Attribut verwenden, um für das Bündel eine **AppxManifest**-Datei bereitzustellen, wobei diese **AppxManifest**-Datei der **AppxManifest**-Datei für das Architekturpaket des Bündels entsprechen sollte. Das **ID**-Attribut muss ebenfalls angegeben werden. Dies wird während der Erstellung des Pakets zusammen mit MakeAppx.exe verwendet, damit Sie nur dieses Paket erstellen können, wenn Sie möchten, und das ist der Dateiname des resultierenden Pakets. Das **FlatBundle**-Attribut wird verwendet, um zu beschreiben, welche Art von Bündel Sie erstellen möchten, wobei **true** für ein flaches Bündel (über das Sie hier mehr lesen können), und **false** für ein klassisches Bündel steht. Das **ResourceManager**-Attribut wird verwendet, um anzugeben, ob die Ressourcenpakete innerhalb dieses Bündels MRT verwenden, um auf die Dateien zuzugreifen. Dies ist standardmäßig **true**, ab Windows 10, Version 1803, ist es jedoch noch nicht fertig, daher muss dieses Attribut auf **false** gesetzt werden.


### <a name="package-and-assetpackage"></a>Package und AssetPackage
Innerhalb der **PackageFamily** sind die Pakete, die ein App-Bündel enthält bzw. Verweise definiert. Hier wird das Architekturpaket (auch als Hauptpaket bezeichnet) mit dem **Package**-Element, und das Bestandspaket mit dem **AssetPackage**-Element definiert. Das Architekturpaket muss angeben, für welche Architektur das Paket vorgesehen ist, entweder „x64”, „x86”, „arm” oder „neutral”. Sie können (optional) auch direkt eine **AppxManifest**-Datei speziell für dieses Paket bereitstellen, indem Sie das **ManifestPath**-Attribut erneut verwenden. Wenn keine **AppxManifest**-Datei bereitgestellt wird, wird eine aus der für die **PackageFamily** bereitgestellten **AppxManifest**-Datei automatisch generiert. 

Standardmäßig wird für jedes Paket im Bündel eine **AppxManifest**-Datei generiert. Für das Bestandspaket können Sie auch das **AllowExecution**-Attribut festlegen. Dieses Attribut auf **false** (den Standardwert) festzulegen hilft, die Veröffentlichungszeit Ihrer App zu verkürzen, da die Virenprüfung bei Paketen, die nicht ausgeführt werden müssen, den Veröffentlichungsprozess nicht blockiert (weitere Informationen hierzu finden Sie unter [Einführung zu Bestandspaketen](asset-packages.md)). 

### <a name="files"></a>Dateien
Innerhalb jeder Paketdefinition können Sie das **Datei**-Element verwenden, um Dateien auszuwählen, die in diesem Paket enthalten sein sollen. Das **SourcePath**-Attribut gibt an, wo sich die Dateien lokal befinden. Sie können Dateien aus verschiedenen Ordnern (durch Angabe von relativen Pfaden), verschiedenen Laufwerken (durch Angabe von absoluten Pfaden) oder sogar Netzwerkfreigaben auswählen (beispielsweise durch Angabe von `\\myshare\myapp\*`). **DestinationPath** gibt an, wo die Dateien innerhalb des Pakets, relativ zum Paketstammverzeichnis, gespeichert werden. **ExcludePath** kann verwendet werden (anstelle der beiden anderen Attribute), um Dateien von denen auszuschließen, die von den **SourcePath**-Attributen anderer **File**-Elemente innerhalb des gleichen Pakets ausgewählt wurden.

Jedes **File**-Element kann zum Auswählen mehrerer Dateien mithilfe von Platzhaltern verwendet werden. Im Allgemeinen können einzelne Platzhalter (`*`) innerhalb des Pfades überall und beliebig oft verwendet werden. Ein Platzhalter entspricht jedoch nur den Dateien in einem Ordner und nicht in Unterordnern. Beispielsweise kann `C:\MyGame\*\*` in **SourcePath** zum Auswählen der Dateien `C:\MyGame\Audios\UI.mp3` und `C:\MyGame\Videos\intro.mp4` verwendet werden, es kann jedoch nicht `C:\MyGame\Audios\Level1\warp.mp3` auswählen. Der doppelte Platzhalter (`**`) kann auch anstelle von Ordner- oder Dateinamen verwendet werden, um beliebige Zeichenfolgen rekursiv zu ersetzen (jedoch nicht neben Teilnamen). Beispielsweise kann `C:\MyGame\**\Level1\**``C:\MyGame\Audios\Level1\warp.mp3` und `C:\MyGame\Videos\Bonus\Level1\DLC1\intro.mp4` auswählen. Platzhalter können auch verwendet werden, um Dateinamen im Rahmen des Verpackungsprozesses direkt zu ändern, wenn die Platzhalter an verschiedenen Stellen zwischen Quelle und Ziel verwendet werden. Wenn zum Beispiel `C:\MyGame\Audios\*` für **SourcePath** und `Sound\copy_*` für **DestinationPath** steht, kann `C:\MyGame\Audios\UI.mp3` ausgewählt werden und wird im Paket als `Sound\copy_UI.mp3` angezeigt. Im Allgemeinen muss die Anzahl der einfachen Platzhalter und doppelten Platzhalter für **SourcePath** und **DestinationPath** eines einzelnen **File**-Elements identisch sein.


## <a name="advanced-packaging-layout-example"></a>Beispiel für ein erweitertes Verpackungslayout

Hier sehen Sie ein Beispiel für ein komplizierteres Verpackungslayout:

```xml
<PackagingLayout xmlns="http://schemas.microsoft.com/appx/makeappx/2017">
  <!-- Main game -->
  <PackageFamily ID="MyGame" FlatBundle="true" ManifestPath="C:\mygame\appxmanifest.xml" ResourceManager="false">
    
    <!-- x64 code package-->
    <Package ID="x64" ProcessorArchitecture="x64">
      <Files>
        <File DestinationPath="*" SourcePath="C:\mygame\*"/>
        <File ExcludePath="*C:\mygame\*.txt"/>
      </Files>
    </Package>

    <!-- Media asset package -->
    <AssetPackage ID="Media" AllowExecution="false">
      <Files>
        <File DestinationPath="Media\**" SourcePath="C:\mygame\media\**"/>
      </Files>
    </AssetPackage>
    
    <!-- English resource package -->
    <ResourcePackage ID="en">
      <Files>
        <File DestinationPath="english\**" SourcePath="C:\mygame\english\**"/>
      </Files>
      <Resources Default="true">
        <Resource Language="en"/>
      </Resources>
    </ResourcePackage>

    <!-- French resource package -->
    <ResourcePackage ID="fr">
      <Files>
        <File DestinationPath="french\**" SourcePath="C:\mygame\french\**"/>
      </Files>
      <Resources>
        <Resource Language="fr"/>
      </Resources>
    </ResourcePackage>
  </PackageFamily>

  <!-- DLC in the related set -->
  <PackageFamily ID="DLC" Optional="true" ManifestPath="C:\DLC\appxmanifest.xml">
    <Package ID="DLC.x86" Architecture="x86">
      <Files>
        <File DestinationPath="**" SourcePath="C:\DLC\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- DLC not part of the related set -->
  <PackageFamily ID="Themes" Optional="true" RelatedSet="false" ManifestPath="C:\themes\appxmanifest.xml">
    <Package ID="Themes.main" Architecture="neutral">
      <Files>
        <File DestinationPath="**" SourcePath="C:\themes\**"/>
      </Files>
    </Package>
  </PackageFamily>

  <!-- Existing packages that need to be included/referenced in the bundle -->
  <PrebuiltPackage Path="C:\prebuilt\DLC2.appxbundle" />

</PackagingLayout>
```

Dieses Beispiel unterscheidet sich vom einfachen Beispiel durch das Hinzufügen der Elemente **ResourcePackage** und **Optional**.

Ressourcenpakete können mit dem **ResourcePackage**-Element angegeben werden. Innerhalb von **ResourcePackage** muss das **Resources**-Element für die Angabe der Ressourcenbezeichner des Ressourcenpakets verwendet werden. Die Ressourcenbezeichner sind die Ressourcen, die vom Ressourcenpaket unterstützt werden. Hier sehen wir, dass zwei Ressourcenpakete definiert sind, die jeweils die sprachspezifischen Dateien für Englisch und Französisch enthalten. Ein Ressourcenpaket kann mehrere Bezeichner haben. Fügen Sie hierzu ein weiteres **Resource**-Element in **Resources** hinzu. Außerdem muss eine Standardressource für die Ressourcendimension angegeben werden, wenn die Dimension (Sprache, Skalierung und dxfl) vorhanden ist. Hier sehen wir, dass Englisch die Standardsprache ist, was für Benutzer, die Französisch nicht als Systemsprache festgelegt haben, bedeutet, dass sie das englische Ressourcenpaket herunterladen und zur Anzeige in Englisch wechseln müssen.


Optionale Pakete besitzen jeweils ihre eigenen, eindeutigen Paketfamiliennamen und müssen mit **PackageFamily**-Elementen definiert werden, wenn das **Optional**-Attribut als **true** angegeben wird. Das **RelatedSet**-Attribut wird verwendet, um anzugeben, ob sich das optionale Paket innerhalb des zugehörigen Sets befindet (standardmäßig ist dieser Wert „true”), und ob das optionale Paket mit dem primären Paket aktualisiert werden soll.

Die **PrebuiltPackage** Element wird verwendet, um Pakete hinzufügen, die nicht definiert sind, im Paket Layout enthalten oder auf die verwiesen wird in die app-Bundle-Dateien erstellt werden sollen. In diesem Fall wird eine andere DLC optionalen Pakets hier eingefügt wird, so, dass die primären app-Paketdatei kann darauf verwiesen wird und der zugehörigen Gruppe angehören.


## <a name="build-app-packages-with-a-packaging-layout-and-makeappxexe"></a>Erstellen von App-Paketen mit einem Verpackungslayout und MakeAppx.exe
Sobald Sie das Verpackungslayout für Ihre App haben, können Sie MakeAppx.exe verwenden, um die Pakete Ihrer App zu erstellen. Verwenden Sie den folgenden Befehl, um alle im Verpackungslayout definierten Pakete zu erstellen:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /op OutputPackages\
```

Wenn Sie jedoch Ihre App aktualisieren, und einige Pakete keine geänderten Dateien enthalten, können Sie nur die Pakete erstellen, die geändert wurden. Bei Verwendung des einfachen Beispiels für ein Verpackungslayout auf dieser Seite und beim Erstellen des x64-Architekturpakets würde der Befehl folgendermaßen aussehen:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "x64" /ip PreviousVersion\ /op OutputPackages\ /iv
```

Mit dem `/id`-Kennzeichen können die zu erstellenden Pakete aus dem Verpackungslayout entsprechend dem **ID**-Attribut im Layout ausgewählt werden. `/ip` wird verwendet, um anzugeben, wo sich in diesem Fall die früheren Versionen der Pakete befinden. Die vorherige Version muss angegeben werden, da die app-Paketdatei noch immer benötigt, um auf die vorherige Version von verweisen die **Media** Paket. Mit dem `/iv`-Kennzeichen wird die Version der zu erstellenden Pakete automatisch erhöht (anstatt die Version in der **AppxManifest**-Datei zu ändern). Sie können auch die Switches `/pv` und `/bv` verwenden, um eine Paketversion (für alle zu erstellenden Pakete) bzw. eine Bündelversion (für alle zu erstellenden Bündel), direkt bereitzustellen.
Verwenden Sie diesen Befehl, wenn Sie mithilfe des erweiterten Verpackungslayoutbeispiels auf dieser Seite nur das optionale Bündel **Designs** und das App-Paket **Themes.main**, auf das es verweist, erstellen möchten:

``` example 
MakeAppx.exe build /f PackagingLayout.xml /id "Themes" /op OutputPackages\ /bc /nbp
```

Das `/bc`-Kennzeichen wird verwendet, um anzugeben, dass die untergeordneten Elemente des **Designs**-Bündels ebenfalls erstellt werden sollen (in diesem Fall wird **Themes.main** erstellt). Das `/nbp`-Kennzeichen gibt an, dass das übergeordnete Element des **Designs**-Bündels nicht erstellt werden soll. Das übergeordnete Element des **Designs**, ist ein optionaler app-Bundle, ist die primäre app-Bundle: **MyGame**. In der Regel muss für ein optionales Paket in einem zugehörigen Set auch das primäre App-Bündel erstellt werden, damit das optionale Paket installiert werden kann, da auf das optionale Paket auch im primären App-Bündel verwiesen wird, wenn es sich in einem zugehörigen Set befindet (um die Versionierung zwischen primärem und optionalem Paket zu gewährleisten). Das folgende Diagramm veranschaulicht die über- und untergeordnete Beziehung zwischen Paketen.

![Verpackungslayout-Diagramm](images/packaging-layout.png)
