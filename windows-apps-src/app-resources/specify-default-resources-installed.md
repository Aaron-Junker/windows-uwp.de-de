---
Description: Wenn Ihre App über keine Ressourcen verfügt, die den speziellen Einstellungen eines Kundengeräts entsprechen, werden die Standardressourcen der App verwendet. In diesem Thema wird erläutert, wie Sie diese Standardressourcen festlegen.
title: Angeben der von der App verwendeten Standardressourcen
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: b11f6dfec2941ae4eaa277f37de66965bd1ea4aa
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593985"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>Angeben der von der App verwendeten Standardressourcen

Wenn Ihre App über keine Ressourcen verfügt, die den speziellen Einstellungen eines Kundengeräts entsprechen, werden die Standardressourcen der App verwendet. In diesem Thema wird erläutert, wie Sie diese Standardressourcen festlegen.

Wenn ein Kunde Ihre App aus dem Microsoft Store installiert, werden die Einstellungen auf dem Gerät des Kunden mit den verfügbaren Ressourcen der App verglichen. Damit wird sichergestellt, dass nur die richtigen Ressourcen heruntergeladen und für diesen Benutzer installiert werden müssen. Beispielsweise werden die am besten geeigneten Zeichenfolgen und Bilder für die Spracheinstellungen des Benutzers sowie die optimale Geräteauflösung und DPI-Einstellung verwendet. Zum Beispiel ist `200` der Standardwert für `scale`. Sie können die Standardeinstellungen bei Bedarf aber überschreiben.

Selbst für Ressourcen, die nicht in ihre eigenen Ressourcenpakete wechseln (z. B. Bilder, die für hohe Kontrasteinstellungen angepasst sind), können Sie angeben, welche Standardressourcen die App zur Laufzeit verwenden soll, wenn keine Ressource, die mit den Einstellungen des Benutzers übereinstimmt, gefunden werden kann. Zum Beispiel ist `standard` der Standardwert für `contrast`. Sie können die Standardeinstellungen bei Bedarf aber überschreiben.

Diese Standardwerte werden in Form von Standardressourcenqualifiziererwerten angegeben. Eine Erklärung zu Ressourcenqualifizierern, deren Verwendung und Zweck finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md).

Diese Standardwerte können Sie auf zweierlei Weise konfigurieren. Sie können entweder eine Konfigurationsdatei zu Ihrem Projekt hinzufügen oder Ihre Projektdatei direkt bearbeiten. Verwenden Sie die Option, mit der Sie am besten vertraut sind oder die mit Ihrem Buildsystem am besten funktioniert.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>Option 1: Verwenden von „priconfig.default.xml“ zur Angabe von Standardqualifiziererwerten

1. Fügen Sie in Visual Studio ein neues Element zu Ihrem Projekt hinzu. Wählen Sie „XML-Datei“, und nennen Sie die Datei `priconfig.default.xml`.
2. Wählen Sie im Projektmappen-Explorer `priconfig.default.xml` aus, und überprüfen Sie das Eigenschaftenfenster. Die Buildaktion der Datei sollte auf „Keine” und „In Ausgabeverzeichnis kopieren” sollte auf „Nicht kopieren” festgelegt sein.
3. Ersetzen Sie den Inhalt der Datei durch diese XML.
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
   
   **Hinweis** Der Wert `LANGUAGE-TAG(S)` muss mit der Standardsprache Ihrer App synchronisiert werden. Wenn es sich um ein einzelnes [BCP-47-Sprachtag](https://go.microsoft.com/fwlink/p/?linkid=227302) handelt, muss die Standardsprache Ihrer App das gleiche Tag sein. Wenn es sich um eine durch Trennzeichen getrennte Liste von Sprachtags handelt, muss die Standardsprache Ihrer App das erste Tag in der Liste sein. Sie definieren die Standardsprache der App im Feld **Standardsprache** auf der Registerkarte **Anwendung** in der Quelldatei des App-Paketmanifests (`Package.appxmanifest`).

4. Jedes `<qualifier>`-Element weist Visual Studio an, welcher Wert als Standard für jeden Qualifizierernamen verwendet werden soll. Mit den bisherigen Dateiinhalten haben Sie das Visual Studio Verhalten noch nicht wirklich geändert. Anders ausgedrückt: Visual Studio *verhält sich bereits so, als ob* diese Datei mit diesem Inhalt vorhanden wäre, da dies der Standard ist. Um also einen Standardwert mit Ihrem eigenen Standardwert zu überschreiben, müssen Sie einen Wert in der Datei ändern. Es folgt ein Beispiel dafür, wie die Datei aussehen könnte, wenn Sie die ersten drei Werte bearbeitet haben.
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

Um zu überprüfen, ob Ihre überschriebenen Werte berücksichtigt werden, suchen Sie nach der Datei `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml`, und stellen Sie sicher, dass der Inhalt Ihren Überschreibungen entspricht. Wenn dies der Fall ist, haben Sie die Qualifiziererwerte der Ressourcen erfolgreich konfiguriert, die Ihre App standardmäßig verwenden wird. Wenn keine Übereinstimmung für die Einstellungen des Benutzers gefunden werden, werden Ressourcen verwendet, deren Ordner- oder Dateinamen die Standardqualifiziererwerte enthalten, die Sie hier festgelegt haben.

### <a name="how-does-this-work"></a>Wie funktioniert das?

Im Hintergrund wird von Visual Studio ein Tool namens `MakePri.exe` gestartet, um eine Datei (den Paketressourcenindex, PRI) zu generieren, die alle Ressourcen der App beschreibt und darauf hinweist, welches die Standardressourcen sind. Weitere Informationen zu diesem Tool finden Sie unter [Manuelles Kompilieren von Ressourcen mit MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio übergibt eine Konfigurationsdatei an `MakePri.exe`. Der Inhalt Ihrer `priconfig.default.xml`-Datei wird als `<default>`-Element dieser Konfigurationsdatei verwendet. Dies ist der Teil, der die Qualifiziererwerte angibt, die als Standardwerte gelten. Durch Hinzufügen und Bearbeiten von `priconfig.default.xml` wird also der Inhalt der Paketressourcenindex-Datei beeinflusst, die Visual Studio für Ihre App generiert und in das App-Paket aufnimmt.

**Hinweis** Jedes Mal, wenn Sie den Wert des `<qualifier name="Language" ... />`-Elements ändern, müssen Sie diese Änderung mit der Standardsprache Ihrer App synchronisieren. Die Sprachressourcen, die in der PRI-Datei der App indiziert wurden, stimmen mit der Manifest-Standardsprache der App überein. Der Wert im `<qualifier name="Language" ... />`-Element überschreibt den Wert im Manifest in Bezug auf den Inhalt von `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml`, diese Datei und das Manifest Ihrer App sollten jedoch übereinstimmen.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>Verwenden eines anderen Dateinamens als `priconfig.default.xml`

Wenn Sie die Datei `priconfig.default.xml` nennen, wird sie von Visual Studio erkannt und automatisch verwendet. Wenn Sie ihr einen anderen Namen geben, müssen Sie dies Visual Studio mitteilen. Fügen Sie diese XML-Datei in Ihrer Projektdatei zwischen dem öffnenden und schließenden Tag des ersten `<PropertyGroup>`-Elements hinzu.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

Ersetzen Sie `FILE-PATH-AND-NAME` durch den Pfad zur und den Namen der Datei.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>Option 2: Verwenden der Projektdatei zur Angabe von Standardqualifiziererwerten

Dies ist eine Alternative zu Option 1. Sobald Sie mit der Funktionsweise von Option 1 vertraut sind, können Sie stattdessen Option 2 wählen, sollte dies besser zu Ihrem Entwicklungs- und/oder Buildworkflow passen.

Fügen Sie diese XML-Datei in Ihrer Projektdatei zwischen dem öffnenden und schließenden Tag des ersten `<PropertyGroup>`-Elements hinzu.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Hier ein Beispiel dafür, wie die Datei aussehen könnte, nachdem Sie die ersten drei Werte bearbeitet haben.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Speichern und schließen Sie, und erstellen Sie das Projekt dann neu.

**Hinweis** Jedes Mal, wenn Sie den `Language=`-Wert ändern, müssen Sie diese Änderung mit der Standardsprache Ihrer App im Manifest-Designer (durch Öffnen von `Package.appxmanifest`) synchronisieren.

## <a name="related-topics"></a>Verwandte Themen

* [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern](tailor-resources-lang-scale-contrast.md)
* [BCP-47-Sprachtag.](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md)
