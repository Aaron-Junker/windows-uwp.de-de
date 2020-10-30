---
description: Wenn Ihre App nicht über Ressourcen verfügt, die den speziellen Einstellungen eines Kundengeräts entsprechen, werden die Standardressourcen der App verwendet. In diesem Thema wird erläutert, wie Sie diese Standardressourcen festlegen.
title: Angeben der von der App verwendeten Standardressourcen
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 4db2fbce788bac38a0f3a54a108f91c8293de6ba
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031552"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>Angeben der von der App verwendeten Standardressourcen

Wenn Ihre App nicht über Ressourcen verfügt, die den speziellen Einstellungen eines Kundengeräts entsprechen, werden die Standardressourcen der App verwendet. In diesem Thema wird erläutert, wie Sie diese Standardressourcen festlegen.

Wenn ein Kunde Ihre APP aus dem Microsoft Store installiert, werden die Einstellungen auf dem Gerät des Kunden mit den verfügbaren Ressourcen der APP abgeglichen. Dieser Abgleich wird durchgeführt, sodass nur die entsprechenden Ressourcen für diesen Benutzer heruntergeladen und installiert werden müssen. Beispielsweise werden die am besten geeigneten Zeichen folgen und Bilder für die Spracheinstellungen des Benutzers und die Auflösung und die dpi-Einstellungen des Geräts verwendet. Beispielsweise `200` ist der Standardwert für `scale` , Sie können diesen Standardwert jedoch überschreiben, wenn Sie möchten.

Auch für Ressourcen, die nicht in Ihre eigenen Ressourcen Pakete (z. b. Images, die auf Einstellungen mit hohem Kontrast zugeschnitten sind) wechseln, können Sie angeben, welche Standard Ressourcen von der App zur Laufzeit verwendet werden sollen, wenn eine Ressource, die mit den Benutzereinstellungen übereinstimmt, nicht gefunden werden kann. Beispielsweise `standard` ist der Standardwert für `contrast` , Sie können diesen Standardwert jedoch überschreiben, wenn Sie möchten.

Diese Standardwerte werden in Form von Standardwerten für Ressourcen Qualifizierer angegeben. Eine Erläuterung der Ressourcen Qualifizierer, deren Verwendung und Zweck finden [Sie unter Anpassen Ihrer Ressourcen für Sprache, Skalierung, hohen Kontrast und andere Qualifizierer](tailor-resources-lang-scale-contrast.md).

Es gibt zwei Möglichkeiten, um zu konfigurieren, wie diese Standardwerte verwendet werden können. Sie können dem Projekt entweder eine Konfigurationsdatei hinzufügen, oder Sie können die Projektdatei direkt bearbeiten. Verwenden Sie unabhängig von den Optionen, mit denen Sie am besten vertraut sind oder die am besten mit dem Buildsystem zusammenarbeiten.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>Option 1. Verwenden von priconfig.default.xml zum Angeben von Standard Qualifiziererwerten

1. Fügen Sie Ihrem Projekt in Visual Studio ein neues Element hinzu. Wählen Sie XML-Datei aus, und benennen Sie die Datei `priconfig.default.xml` .
2. Wählen Sie in Projektmappen-Explorer `priconfig.default.xml` die Eigenschaftenfenster aus, und überprüfen Sie Sie. Die Buildaktion der Datei sollte auf keine festgelegt werden, und in das Ausgabeverzeichnis kopieren sollte auf nicht kopieren festgelegt sein.
3. Ersetzen Sie den Inhalt der Datei durch diesen XML-Code.
   ```xml
   <default>
      <qualifier name="Language" value="LANGUAGE-TAG(S)" />
      <qualifier name="Contrast" value="standard" />
      <qualifier name="Scale" value="200" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
   
   **Hinweis** Der Wert `LANGUAGE-TAG(S)` muss mit der Standardsprache Ihrer APP synchronisiert werden. Wenn es sich um ein einzelnes [bcp-47-sprach Kennzeichen](https://tools.ietf.org/html/bcp47)handelt, muss die Standardsprache Ihrer APP dasselbe Tag aufweisen. Wenn es sich um eine durch Trennzeichen getrennte Liste von sprach Tags handelt, muss die Standardsprache Ihrer APP das erste Tag in der Liste sein. Sie legen die Standardsprache Ihrer APP im Feld **Standardsprache** auf der Registerkarte **Anwendung** in der Quelldatei für das App-Paket Manifest ( `Package.appxmanifest` ) fest.

4. Jedes- `<qualifier>` Element teilt Visual Studio mit, welcher Wert als Standardwert für die einzelnen Qualifizierernamen verwendet werden soll. Mit dem Inhalt der Datei, die Sie bisher haben, haben Sie das Verhalten von Visual Studio nicht geändert. Anders ausgedrückt: Visual Studio hat sich *bereits so verhält, als ob* diese Datei mit diesen Inhalten vorhanden wäre, da es sich hierbei um die Standard Standardwerte handelt. Wenn Sie also einen Standardwert mit Ihrem eigenen Standardwert überschreiben möchten, müssen Sie einen Wert in der Datei ändern. Im folgenden finden Sie ein Beispiel dafür, wie die Datei aussehen kann, wenn Sie die ersten drei Werte bearbeitet haben.
   ```xml
   <default>
      <qualifier name="Language" value="de-DE" />
      <qualifier name="Contrast" value="black" />
      <qualifier name="Scale" value="400" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
5. Speichern und schließen Sie die Datei, und erstellen Sie das Projekt neu.

Um sicherzustellen, dass die außer Kraft gesetzten Standardwerte berücksichtigt werden, suchen Sie nach der Datei, `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` und vergewissern Sie sich, dass Ihr Inhalt ihren außer Kraft setzungen entspricht. Wenn dies der Fall ist, haben Sie die Qualifiziererwerte der Ressourcen, die von ihrer App standardmäßig verwendet werden, erfolgreich konfiguriert. Wenn keine Entsprechung für die Benutzereinstellungen gefunden wird, werden Ressourcen verwendet, deren Ordner-oder Dateinamen die Standardwert Werte enthalten, die Sie hier festgelegt haben.

### <a name="how-does-this-work"></a>Wie funktioniert das?

Im Hintergrund öffnet Visual Studio ein Tool mit dem Namen `MakePri.exe` , um eine Datei zu generieren, die als Paket Ressourcen Index (PRI) bezeichnet wird, in dem alle Ressourcen Ihrer APP beschrieben werden, einschließlich der Angabe, welche die Standard Ressourcen sind. Weitere Informationen zu diesem Tool finden Sie unter [manuelles Kompilieren von Ressourcen mit MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio übergibt eine Konfigurationsdatei an `MakePri.exe` . Der Inhalt der `priconfig.default.xml` Datei wird als `<default>` Element der Konfigurationsdatei verwendet. Dies ist der Teil, der den Satz von Qualifiziererwerten angibt, die als Standard betrachtet werden. Das Hinzufügen und bearbeiten wirkt sich daher auf `priconfig.default.xml` den Inhalt der Paket Ressourcen-Index Datei aus, die Visual Studio für Ihre APP generiert und in das zugehörige App-Paket einschließt.

**Hinweis** Wenn Sie den Wert des-Elements ändern `<qualifier name="Language" ... />` , müssen Sie diese Änderung mit der Standardsprache Ihrer APP synchronisieren. Dies ist der Fall, wenn die in der PRI-Datei Ihrer APP indizierten Sprachressourcen mit der Manifest-Standardsprache Ihrer APP identisch sind. Der Wert im- `<qualifier name="Language" ... />` Element überschreibt den Wert im Manifest in Bezug auf den Inhalt von `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` , aber diese Datei und das Manifest Ihrer APP sollten Stimmen.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>Verwenden eines anderen Datei namens als `priconfig.default.xml`

Wenn Sie die Datei benennen `priconfig.default.xml` , wird Sie von Visual Studio erkannt und automatisch verwendet. Wenn Sie einen anderen Namen angeben, müssen Sie Visual Studio informieren. Fügen Sie in der Projektdatei zwischen dem öffnenden und dem schließenden Tag des ersten `<PropertyGroup>` Elements diesen XML-Code hinzu.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

Ersetzen `FILE-PATH-AND-NAME` Sie durch den Pfad und den Namen der Datei.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>Option 2. Verwenden Sie die Projektdatei, um Standard Qualifiziererwerte anzugeben.

Dies ist eine Alternative zu Option 1. Wenn Sie die Funktionsweise von Option 1 verstanden haben, können Sie stattdessen Option 2 auswählen, wenn dies für ihren Entwicklungs-und/oder buildworkflow besser geeignet ist.

Fügen Sie in der Projektdatei zwischen dem öffnenden und dem schließenden Tag des ersten `<PropertyGroup>` Elements diesen XML-Code hinzu.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Im folgenden finden Sie ein Beispiel dafür, wie das aussehen könnte, nachdem Sie die ersten drei Werte bearbeitet haben.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Speichern und schließen Sie das Projekt, und erstellen Sie es neu.

**Hinweis** Wenn Sie den Wert ändern `Language=` , müssen Sie diese Änderung mit der Standardsprache Ihrer APP im Manifest-Designer (durch Öffnen von) synchronisieren `Package.appxmanifest` .

## <a name="related-topics"></a>Verwandte Themen

* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md)
* [BCP-47-Sprachtag](https://tools.ietf.org/html/bcp47)
* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md)
