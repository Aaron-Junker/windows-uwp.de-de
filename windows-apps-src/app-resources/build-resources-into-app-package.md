---
Description: Einige Arten von Apps (mehrsprachige Wörterbücher, Übersetzungstools usw.) müssen das Standardverhalten von einem App-Paket überschreiben und Ressourcen im App-Paket und nicht in separaten Ressourcenpaketen integrieren. In diesem Thema wird erläutert, wie das geht.
title: Erstellen von Ressourcen in Ihrem App-Paket
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: b975dcf88ecd26dc5a24d602c117b779fa2aada6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174524"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>Integrieren von Ressourcen im App-Paket und nicht in einem Ressourcenpaket

Einige Arten von apps (mehrsprachige Wörterbücher, Übersetzungstools usw.) müssen das Standardverhalten einer APP Bundle überschreiben und Ressourcen in das App-Paket erstellen, anstatt Sie in separaten Ressourcen Paketen (oder Ressourcen Paketen) zu verwenden. In diesem Thema wird erläutert, wie das geht.

Wenn Sie einen [App Bundle (. appxbundle)](/windows/msix/package/packaging-uwp-apps)erstellen, werden standardmäßig nur die Standard Ressourcen für die sprach-, Skalierungs-und DirectX-Featureebene in das App-Paket integriert. Ihre übersetzten Ressourcen &mdash; und ihre Ressourcen, die auf nicht standardmäßige Skalen und/oder DirectX-featureebenen zugeschnitten &mdash; sind, sind in Ressourcen Pakete integriert und werden nur auf Geräte heruntergeladen, die Sie benötigen. Wenn ein Kunde Ihre APP aus dem Microsoft Store mit einem Gerät kauft, das als Spracheinstellung Spanisch eingestellt ist, wird nur Ihre APP und das spanische Ressourcenpaket heruntergeladen und installiert. Wenn derselbe Benutzer später die bevorzugte Sprache in den **Einstellungen**in Französisch ändert, wird das französische Ressourcenpaket der App heruntergeladen und installiert. Ähnliche Dinge treten bei ihren Ressourcen auf, die für die Skalierung und die DirectX-Funktionsebene qualifiziert sind. Bei den meisten apps stellt dieses Verhalten eine wertvolle Effizienz dar und ist genau das, was Sie und der Kunde tun *möchten* .

Wenn die APP jedoch dem Benutzer ermöglicht, die Sprache im Handumdrehen innerhalb der APP (anstelle von **Einstellungen**) zu ändern, ist dieses Standardverhalten nicht geeignet. Sie möchten, dass alle Ihre Sprachressourcen mit der APP einmal bedingungslos heruntergeladen und installiert werden und dann auf dem Gerät verbleiben. Sie möchten all diese Ressourcen in Ihrem App-Paket erstellen, anstatt separate Ressourcen Pakete zu verwenden.

**Hinweis** Das Einschließen von Ressourcen in ein App-Paket erhöht im Wesentlichen die Größe der app. Daher ist es nur sinnvoll, wenn die Art der APP es erfordert. Wenn dies nicht der Fall ist, müssen Sie keine Aktionen ausführen, außer wie üblich eine reguläre App Bundle erstellen.

Sie können Visual Studio so konfigurieren, dass Ressourcen in Ihrem App-Paket mit einer von zwei Methoden erstellt werden. Sie können dem Projekt entweder eine Konfigurationsdatei hinzufügen, oder Sie können die Projektdatei direkt bearbeiten. Verwenden Sie unabhängig von den Optionen, mit denen Sie am besten vertraut sind oder die am besten mit dem Buildsystem zusammenarbeiten.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>Option 1. Verwenden Sie priconfig.packaging.xml, um Ressourcen in Ihrem App-Paket zu erstellen.

1. Fügen Sie Ihrem Projekt in Visual Studio ein neues Element hinzu. Wählen Sie XML-Datei aus, und benennen Sie die Datei `priconfig.packaging.xml` .
2. Wählen Sie in Projektmappen-Explorer `priconfig.packaging.xml` die Eigenschaftenfenster aus, und überprüfen Sie Sie. Die Buildaktion der Datei sollte auf keine festgelegt werden, und in das Ausgabeverzeichnis kopieren sollte auf nicht kopieren festgelegt sein.
3. Ersetzen Sie den Inhalt der Datei durch diesen XML-Code.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. Jedes- `<autoResourcePackage>` Element weist Visual Studio an, die Ressourcen für den angegebenen Qualifizierernamen automatisch in separate Ressourcen Pakete aufzuteilen. Dies wird als *Automatische Aufteilung*bezeichnet. Mit dem Inhalt der Datei, die Sie bisher haben, haben Sie das Verhalten von Visual Studio nicht geändert. Anders ausgedrückt: Visual Studio hat sich *bereits so verhält, als ob* diese Datei mit diesen Inhalten vorhanden wäre, da es sich hierbei um die Standardwerte handelt. Wenn Sie nicht möchten, dass Visual Studio automatisch auf einen Qualifizierernamen aufgeteilt wird, löschen Sie dieses `<autoResourcePackage>` Element aus der Datei. Im folgenden wird erläutert, wie die Datei aussehen würde, wenn Sie möchten, dass alle Ihre Sprachressourcen in das App-Paket integriert werden, anstatt automatisch in separate Ressourcen Pakete aufgeteilt zu werden.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. Speichern und schließen Sie die Datei, und erstellen Sie das Projekt neu.

Um sicherzustellen, dass Ihre automatische Aufteilung ausgewählt wird, suchen Sie nach der Datei, `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml` und vergewissern Sie sich, dass Ihre Inhalte ihren Optionen entsprechen. Wenn dies der Fall ist, haben Sie Visual Studio erfolgreich konfiguriert, um die Ressourcen Ihrer Wahl im App-Paket zu erstellen.

Es gibt einen abschließenden Schritt, den Sie ausführen müssen. **Aber nur, wenn Sie den `Language` Namen des Qualifizierers gelöscht**haben. Sie müssen die Gesamtmenge der unterstützten Sprache Ihrer APP als Standardwert für die Sprache Ihrer APP angeben. Weitere Informationen finden [Sie unter Angeben der Standard Ressourcen, die von Ihrer APP verwendet](specify-default-resources-installed.md)werden. Dies `priconfig.default.xml` wäre, wenn Sie in Ihrem App-Paket Ressourcen für Englisch, Spanisch und Französisch einschließen.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>Wie funktioniert das?

Im Hintergrund öffnet Visual Studio ein Tool mit dem Namen, `MakePri.exe` um eine Datei zu generieren, die als Paket Ressourcen Index bezeichnet wird, in dem alle Ressourcen Ihrer APP beschrieben werden, einschließlich der Angabe, auf welche Ressourcen Qualifizierernamen automatisch aufgeteilt werden soll. Weitere Informationen zu diesem Tool finden Sie unter [manuelles Kompilieren von Ressourcen mit MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio übergibt eine Konfigurationsdatei an `MakePri.exe` . Der Inhalt der `priconfig.packaging.xml` Datei wird als `<packaging>` Element der Konfigurationsdatei verwendet. Dies ist der Teil, der die automatische Aufteilung bestimmt. Das Hinzufügen und bearbeiten wirkt sich daher auf `priconfig.packaging.xml` den Inhalt der Paket Ressourcen Index Datei aus, die Visual Studio für Ihre APP generiert, sowie auf die Inhalte der Pakete in Ihrer APP Bundle.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>Verwenden eines anderen Datei namens als `priconfig.packaging.xml`

Wenn Sie die Datei benennen `priconfig.packaging.xml` , wird Sie von Visual Studio erkannt und automatisch verwendet. Wenn Sie einen anderen Namen angeben, müssen Sie Visual Studio informieren. Fügen Sie in der Projektdatei zwischen dem öffnenden und dem schließenden Tag des ersten `<PropertyGroup>` Elements diesen XML-Code hinzu.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

Ersetzen `FILE-PATH-AND-NAME` Sie durch den Pfad und den Namen der Datei.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>Option 2. Verwenden Sie die Projektdatei, um Ressourcen in Ihrem App-Paket zu erstellen.

Dies ist eine Alternative zu Option 1. Wenn Sie die Funktionsweise von Option 1 verstanden haben, können Sie stattdessen Option 2 auswählen, wenn dies für ihren Entwicklungs-und/oder buildworkflow besser geeignet ist.

Fügen Sie in der Projektdatei zwischen dem öffnenden und dem schließenden Tag des ersten `<PropertyGroup>` Elements diesen XML-Code hinzu.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

So sieht das aus, nachdem Sie den ersten Qualifizierernamen gelöscht haben.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Speichern und schließen Sie das Projekt, und erstellen Sie es neu.

Es gibt einen abschließenden Schritt, den Sie ausführen müssen. **Aber nur, wenn Sie den `Language` Namen des Qualifizierers gelöscht**haben. Sie müssen die Gesamtmenge der unterstützten Sprache Ihrer APP als Standardwert für die Sprache Ihrer APP angeben. Weitere Informationen finden [Sie unter Angeben der Standard Ressourcen, die von Ihrer APP verwendet](specify-default-resources-installed.md)werden. Die Projektdatei enthält dann, wenn Sie in Ihrem App-Paket Ressourcen für Englisch, Spanisch und Französisch einschließen.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>Zugehörige Themen

* [Packen einer UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps)
* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md)
* [Angeben der von der App verwendeten Standardressourcen](specify-default-resources-installed.md)