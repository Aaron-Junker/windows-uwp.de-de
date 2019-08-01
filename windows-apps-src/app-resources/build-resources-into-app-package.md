---
Description: Einige Arten von Apps (mehrsprachige Wörterbücher, Übersetzungstools usw.) müssen das Standardverhalten von einem App Bundle überschreiben und Ressourcen im App-Paket und nicht in separaten Ressourcenpaketen integrieren. In diesem Thema wird erläutert, wie das geht.
title: Integrieren von Ressourcen im App-Paket und nicht in einem Ressourcenpaket
template: detail.hbs
ms.date: 11/14/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: f7acabc9858f5a2fa0b6b76d752d2a342959f41f
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682514"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>Integrieren von Ressourcen im App-Paket und nicht in einem Ressourcenpaket

Einige Arten von Apps (mehrsprachige Wörterbücher, Übersetzungstools usw.) müssen das Standardverhalten von einem App Bundle überschreiben und Ressourcen im App-Paket und nicht in separaten Ressourcenpaketen (oder Ressourcenpaketen) integrieren. In diesem Thema wird erläutert, wie das geht.

Wenn Sie standardmäßig ein [App Bundle (.appxbundle)](/windows/msix/package/packaging-uwp-apps) erstellen, werden nur Ihre Standardressourcen für Sprache, Skalierung und DirectX-Featureebene im App-Paket integriert. Die übersetzten Ressourcen – und Ihre Ressourcen speziell für nicht standardmäßige Skalierungen und/oder DirectX-Featureebenen – sind in Ressourcenpakete integriert und werden nur auf Geräte heruntergeladen, die sie benötigen. Wenn ein Kunde Ihre App vom Microsoft Store mit einem Gerät erwirbt, auf dem die bevorzugte Sprache auf Spanisch festgelegt ist, werden nur Ihre App sowie das spanische Ressourcenpaket heruntergeladen und installiert. Wenn derselbe Benutzer seine bevorzugte Sprache später unter **Einstellungen** in Französisch ändert, wird das französische Ressourcenpaket für Ihre App heruntergeladen und installiert. Ähnliches passiert mit den Ressourcen, die für die Skalierung und DirectX-Featureebene qualifiziert sind. Bei den meisten Apps bedeutet dieses Verhalten mehr Effizienz, und dies ist genau das, was Sie und der Kunde *wünschen*.

Wenn Ihre App aber dem Benutzer das Ändern der Sprache direkt über die App erlaubt (statt über **Einstellungen**), ist dieses Standardverhalten nicht korrekt. Sie möchten, dass alle Ihre Sprachressourcen zusammen mit der App bedingungslos einmal heruntergeladen und installiert werden und dann auf dem Gerät verbleiben. Sie möchten, dass diese Ressourcen in das App-Paket und nicht in separaten Ressourcenpaketen integriert werden.

**Hinweis:** Durch Integrieren der Ressourcen in einem App-Paket erhöht sich die Größe der App erheblich. Deshalb sollten Sie dies nur tun, wenn die Art der App dies erfordert. Falls dies nicht der Fall ist, müssen Sie nichts weiter tun, als wie gewohnt ein reguläres App Bundle zu erstellen.

Sie können Visual Studio so konfigurieren, dass Ressourcen in Ihrem App-Paket auf eine von zwei Weisen integriert werden. Sie können entweder eine Konfigurationsdatei zu Ihrem Projekt hinzufügen oder Ihre Projektdatei direkt bearbeiten. Verwenden Sie die Option, mit der Sie am besten vertraut sind oder die mit Ihrem Buildsystem am besten funktioniert.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>Option 1: Verwenden von „priconfig.packaging.xml“, um Ressourcen in Ihrem App-Paket zu erstellen

1. Fügen Sie in Visual Studio ein neues Element zu Ihrem Projekt hinzu. Wählen Sie „XML-Datei“, und nennen Sie die Datei `priconfig.packaging.xml`.
2. Wählen Sie im Projektmappen-Explorer `priconfig.packaging.xml` aus, und überprüfen Sie das Eigenschaftenfenster. Die Buildaktion der Datei sollte auf „Keine” und „In Ausgabeverzeichnis kopieren” sollte auf „Nicht kopieren” festgelegt sein.
3. Ersetzen Sie den Inhalt der Datei durch diese XML.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. Jedes `<autoResourcePackage>`-Element weist Visual Studio an, automatisch die Ressourcen für den angegebenen Qualifizierernamen in separate Ressourcenpakete aufzuteilen. Dies wird als *automatische Aufteilung* bezeichnet. Mit den bisherigen Dateiinhalten haben Sie das Visual Studio Verhalten noch nicht wirklich geändert. Anders ausgedrückt: Visual Studio *verhält sich bereits so, als ob* diese Datei mit diesem Inhalt vorhanden wäre, da dies der Standard ist. Wenn Sie nicht möchten, dass Visual Studio automatisch nach Qualifizierername aufteilt, löschen Sie dieses `<autoResourcePackage>`-Element aus der Datei. Hier sehen Sie, wie die Datei aussehen würde, wenn alle Ihre Sprachressourcen im App-Paket integriert werden sollen, anstatt sie automatisch in separate Ressourcenpakete aufzuteilen.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. Speichern und schließen Sie die Datei, und erstellen Sie das Projekt neu.

Um zu überprüfen, ob Ihre Auswahl für die automatische Aufteilung berücksichtigt wurde, suchen Sie nach der Datei `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml`, und stellen Sie sicher, dass der Inhalt Ihrer Auswahl entspricht. Wenn dies der Fall ist, haben Sie Visual Studio erfolgreich so konfiguriert, dass die Ressourcen Ihrer Wahl im App-Paket integriert werden.

Es gibt noch einen letzten Schritt, den Sie durchführen müssen. **Aber nur, wenn Sie den `Language`-Qualifizierernamen gelöscht haben.** Sie müssen die Gesamtheit aller Sprachen, die von Ihrer App unterstützt werden, als Standardsprachenwert für Ihre App angeben. Weitere Informationen finden Sie unter [Angeben der von der App verwendeten Standardressourcen](specify-default-resources-installed.md). Dies würde die Datei `priconfig.default.xml` enthalten, wenn Sie Ressourcen für Englisch, Spanisch und Französisch in Ihrem App-Paket integrieren würden.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>Wie funktioniert das?

Im Hintergrund wird von Visual Studio ein Tool namens `MakePri.exe` gestartet, um eine Datei (den Paketressourcenindex) zu generieren, die alle Ressourcen Ihrer App beschreibt und außerdem darauf hinweist, für welche Qualifizierernamen automatisch aufgeteilt werden soll. Weitere Informationen zu diesem Tool finden Sie unter [Manuelles Kompilieren von Ressourcen mit MakePri.exe](compile-resources-manually-with-makepri.md). Visual Studio übergibt eine Konfigurationsdatei an `MakePri.exe`. Der Inhalt der Datei `priconfig.packaging.xml` wird als `<packaging>`-Element dieser Konfigurationsdatei verwendet, welches die automatische Aufteilung festlegt. Durch Hinzufügen und Bearbeiten von `priconfig.packaging.xml` wird letztlich der Inhalt der Paketressourcenindex-Datei beeinflusst, die Visual Studio für Ihre App generiert, sowie der Inhalt der Pakete in Ihrem App Bundle.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>Verwenden eines anderen Dateinamens als `priconfig.packaging.xml`

Wenn Sie die Datei `priconfig.packaging.xml` nennen, wird sie von Visual Studio erkannt und automatisch verwendet. Wenn Sie ihr einen anderen Namen geben, müssen Sie dies Visual Studio mitteilen. Fügen Sie diese XML-Datei in Ihrer Projektdatei zwischen dem öffnenden und schließenden Tag des ersten `<PropertyGroup>`-Elements hinzu.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

Ersetzen Sie `FILE-PATH-AND-NAME` durch den Pfad zur und den Namen der Datei.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>Option 2: Verwenden der Projektdatei, um Ressourcen im App-Paket zu integrieren

Dies ist eine Alternative zu Option 1. Sobald Sie mit der Funktionsweise von Option 1 vertraut sind, können Sie stattdessen Option 2 wählen, sollte dies besser zu Ihrem Entwicklungs- und/oder Buildworkflow passen.

Fügen Sie diese XML-Datei in Ihrer Projektdatei zwischen dem öffnenden und schließenden Tag des ersten `<PropertyGroup>`-Elements hinzu.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Hier sehen Sie, wie das aussieht, nachdem Sie den ersten Qualifizierernamen gelöscht haben.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Speichern und schließen Sie, und erstellen Sie das Projekt dann neu.

Es gibt noch einen letzten Schritt, den Sie durchführen müssen. **Aber nur, wenn Sie den `Language`-Qualifizierernamen gelöscht haben.** Sie müssen die Gesamtheit aller Sprachen, die von Ihrer App unterstützt werden, als Standardsprachenwert für Ihre App angeben. Weitere Informationen finden Sie unter [Angeben der von der App verwendeten Standardressourcen](specify-default-resources-installed.md). Dies würde die Projektdatei enthalten, wenn Sie Ressourcen für Englisch, Spanisch und Französisch in Ihrem App-Paket integrieren würden.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>Verwandte Themen

* [Verpacken einer UWP-App mit Visual Studio](../packaging/packaging-uwp-apps.md)
* [Manuelles Kompilieren von Ressourcen mit „MakePri.exe“](compile-resources-manually-with-makepri.md)
* [Angeben der von der App verwendeten Standardressourcen](specify-default-resources-installed.md)