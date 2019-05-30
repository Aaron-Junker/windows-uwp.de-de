---
Description: In diesem Thema wird das allgemeine Konzept der Qualifizierer erläutert, wie sie verwendet werden und wofür die einzelnen Qualifizierernamen dienen.
title: Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 0e5ba7ddb6ef37b4aa54584602fc890bbabc8998
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359353"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern

In diesem Thema wird das allgemeine Konzept der Ressourcenqualifizierer erläutert, wie sie verwendet werden und wofür die einzelnen Qualifizierernamen dienen. Eine Referenztabelle für die verschiedenen Qualifiziererwerte finden Sie unter [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

Ihre App kann Assets und Ressourcen laden, die für Runtime-Kontexte wie Anzeigesprache, hoher Kontrast [Skalierungsfaktor des Displays](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor) und mehr zugeschnitten sind. Das erreichen Sie unter anderem durch das Benennen des Ressourcen-Ordners oder der Dateien mit dem Namen des Qualifizierers und den Qualifiziererwerten, die diesen Kontexten entsprechen. Beispielsweise können Sie für Ihre App einen anderen Satz von Bildressourcen laden, wenn sie in einem Modus mit hohem Kontrast ausgeführt wird.

Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md).

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>Qualifizierernamen Qualifiziererwerte und Qualifizierer

Ein Qualifizierername ist ein Schlüssel, der einer Reihe von Qualifiziererwerten zugeordnet ist. Hier sind Qualifizierernamen und Qualifiziererwerte für den Kontrast aufgeführt.

| Kontext | Qualifizierername | Qualifiziererwerte |
| :--------------- | :--------------- | :--------------- |
| Einstellung für hohen Kontrast | Kontrast | Standard, hoch, schwarz, weiß |

Kombinieren Sie einen Qualifizierernamen mit einem Qualifiziererwert, um einen Qualifizierer zu bilden. `<qualifier name>-<qualifier value>` ist das Format der Qualifizierer an. `contrast-standard` ist ein Beispiel für einen Qualifizierer an.

Für hohen Kontrast ist der Satz von Qualifizierern `contrast-standard`, `contrast-high`, `contrast-black` und `contrast-white`. Bei Qualifizierernamen und Qualifiziererwerten wird die Groß-/Kleinschreibung nicht beachtet. Beispielsweise: `contrast-standard` und `Contrast-Standard` sind die gleiche Qualifizierer.

## <a name="use-qualifiers-in-folder-names"></a>Qualifizierer in Ordnernamen verwenden

Hier ist ein Beispiel für die Verwendung von Qualifizierer zum Benennen von Ordnern, die Ressourcendateien enthalten. Verwenden Sie Qualifizierer in Ordnernamen, wenn Sie mehrere Ressourcendateien pro Qualifizierer haben. Auf diese Weise legen Sie den Qualifizierer einmal auf Ordnerebene fest und der Qualifizierer gilt für alle Elemente im Ordner.

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

Wenn Sie Ihre Ordner wie im obigen Beispiel benennen, verwendet Ihre App den hohen Kontrast zum Laden von Dateien aus dem Ordner mit dem Namen für den entsprechenden Qualifizierer. Wenn die Einstellung hoher Kontrast (Schwarz) ist, werden die Ressourcendateien in den Ordern `\Assets\Images\contrast-black` geladen. Wenn die Einstellung "Keine" ist (d. h. der Computer ist nicht im Modus mit hohem Kontrast), werden die Ressourcendateien in den Ordner `\Assets\Images\contrast-standard` geladen.

## <a name="use-qualifiers-in-file-names"></a>Qualifizierer in Dateinamen verwenden

Anstatt Ordner zu erstellen und zu benennen können Sie einen Qualifizierer verwenden, um die Ressourcendateien selbst zu benennen. Möglicherweise möchten dies tun, wenn Sie nur eine Ressourcendatei pro Qualifizierer haben. Hier ist ein Beispiel.

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

Die Datei, deren Name den geeignetsten Qualifizierer für die Einstellung enthält, wird geladen. Diese übereinstimmende Logik funktioniert genauso für Dateinamen als auch für Ordnernamen.

## <a name="reference-a-string-or-image-resource-by-name"></a>Verweisen auf eine Zeichenfolgen- oder Bildressource nach Namen

Weitere Informationen finden Sie unter [Referenzieren eines Bezeichners für Zeichenfolgenressourcen im XAML-Markup](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml-markup), [Referenzieren eines Bezeichners für Zeichenfolgenressourcen im Code](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code) und [Referenzieren eines Bilds oder eines anderen Elements in XAML-Markup und Code](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

## <a name="actual-and-neutral-qualifier-matches"></a>Aktuelle und neutrale Qualifizierertreffer
Sie müssen nicht für *jeden* Qualifiziererwert eine Ressourcendatei erstellen. Wenn Sie z. B. feststellen, dass Sie nur eine visuelle Ressource für hohen Kontrast und eine für den standardmäßigen Kontrast benötigen, benennen Sie diese Ressourcen wie folgt.

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

Der erste Dateiname enthält den `contrast-high` Qualifizierer. Dieser Qualifizierer ist ein *tatsächlicher* Treffer für jede Einstellung mit hohem Kontrast, wenn der hohe Kontrast *aktiviert* ist. Anders ausgedrückt, ist es ähnlich und wird daher bevorzugt. Eine *tatsächliche* Übereinstimmung kann nur auftreten, wenn der Qualifizierer einen *tatsächlichen* Wert enthält, wie hier gezeigt. In diesem Fall ist `high` ein *tatsächlicher* Wert für `contrast`.

Die Datei mit dem Namen `logo.png` hat keinen Kontrastqualifizierer. Das Fehlen eines Qualifizierers bedeutet, es ist ein *neutraler* Wert. Wenn keine bevorzugten Übereinstimmung gefunden werden kann, dient der neutrale Wert als Fallback-Übereinstimmung. In diesem Beispiel: Wenn der hohe Kontrast *deaktiviert* ist, ist keine tatsächliche Übereinstimmung vorhanden. Die *neutrale* Übereinstimmung ist die beste Übereinstimmung, die gefunden werden kann, und das Element `logo.png` wird geladen.

Wenn Sie den Namen von `logo.png` auf `logo.contrast-standard.png` ändern würden, würde der Dateiname einen tatsächlichen Qualifiziererwert enthalten. Wenn der hohe Kontrast deaktiviert ist, gäbe es eine tatsächliche Übereinstimmung mit `logo.contrast-standard.png` und diese Element-Datei würde geladen. Es würden daher dieselben Dateien geladen, unter den gleichen Bedingungen, aber aufgrund unterschiedlicher Übereinstimmungen.

Wenn Sie nur einen Satz von Ressourcen für hohen Kontrast brauchen und einen Satz für Standard-Kontrast festgelegt ist, können Sie Ordnernamen anstelle von Dateinamen verwenden. In diesem Fall führt ein Weglassen des Ordnernamens zu einer neutralen Übereinstimmung.

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

Weitere Informationen zur Funktionsweise der Übereinstimmung für Qualifizierer finden Sie unter [Ressourcenverwaltungssystem](resource-management-system.md).

## <a name="multiple-qualifiers"></a>Mehrere Qualifizierer

Sie können Qualifizierer in Ordner- und Dateinamen kombinieren. Wenn z. B. Ihre App Bildressourcen bei aktiviertem Modus für hohen Kontrast lädt *und* der Skalierungsfaktor für die Anzeige ist 400. Eine Möglichkeit hierfür sind geschachtelte Ordner.

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

Damit `logo.png` und anderen Dateien geladen werden, müssen die Einstellungen mit *beiden* Qualifizierern übereinstimmen.

Eine weitere Möglichkeit ist, mehrere Qualifizierer in einem Ordnernamen zu kombinieren.

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

Kombinieren Sie in einem Ordnernamen mehrere Qualifizierer und trennen Sie diese mit einem Unterstrich. `<qualifier1>[_<qualifier2>...]` ist das Format.

Sie können mehrere Qualifizierer in einem Dateinamen im selben Format kombinieren.

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

Je nach verwendeten Tools und Workflows bei der Asset-Erstellung und je nachdem, was am einfachsten zu Lesen und Verwalten ist, können Sie entweder eine einzelne Benennungsstrategie für alle Qualifizierer auswählen oder diese für andere Qualifizierer kombinieren.

## <a name="alternateform"></a>Alternatives Format

Mit dem `alternateform` Qualifizierer können Sie ein alternatives Format einer Ressource für einen speziellen Zweck angeben. Dies wird in der Regel nur von japanischen App-Entwicklern verwendet, um eine Furigana-Zeichenfolge bereitzustellen, für die der Wert `msft-phonetic` reserviert ist (Informationen hierzu finden Sie im Abschnitt "Unterstützung von Furigana für japanische Zeichenfolgen, die sortiert werden können" unter [ Vorbereiten für die Lokalisierung](https://docs.microsoft.com/previous-versions/windows/apps/hh967762(v=win.10))).

Entweder Ihr Zielsystem oder Ihre App müssen einen Wert bereitstellen, für die `alternateform`-Qualifizierer übereinstimmen müssen. Verwenden Sie das Präfix `msft-` nicht für eigene benutzerdefinierte `alternateform` Qualifiziererwerte.

## <a name="configuration"></a>Konfiguration

Es ist unwahrscheinlich, dass Sie den `configuration` Qualifizierernamen benötigen. Dieser dient zum Angeben von Ressourcen, die nur für eine bestimmte Umgebung zur Erstellungszeit gültig sind (beispielsweise Ressourcen zu Testzwecken).

Der Qualifizierer `configuration` wird verwendet, um eine Ressource zu laden, die am besten dem Wert der `MS_CONFIGURATION_ATTRIBUTE_VALUE`-Umgebungsvariablen entspricht. Die Variable kann auf den Zeichenfolgenwert festgelegt werden, der den relevanten Ressourcen zugewiesen wurde, beispielsweise `designer` oder `test`.

## <a name="contrast"></a>Vergleichen Sie

Der `contrast`-Qualifizierer wird verwendet, um Ressourcen anzubieten, die den Einstellungen für hohen Kontrast am besten entsprechen.

## <a name="custom"></a>Benutzerdefiniert

Ihre App kann einen Wert für den `custom`-Qualifizierer festlegen, wobei Ressourcen geladen werden, die am besten diesem Wert entsprechen. Möglicherweise möchten Sie Ressourcen basierend auf der App-Lizenz laden. Wenn die App startet, wird die Lizenz überprüft und als Wert für den `custom`-Qualifizierer durch Aufrufen von [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue) verwendet, wie im Codebeispiel dargestellt.

```csharp
public void SetLicenseLevel(BrandID brand)
{
    if (brand == BrandID.Premium)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Premium", ResourceQualifierPersistence.LocalMachine);
    }
    else if (brand == BrandID.Standard)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", " Standard", ResourceQualifierPersistence.LocalMachine);
    }
    else
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Trial", ResourceQualifierPersistence.LocalMachine);
    }
}
```

In diesem Fall würden Sie Ihren Ressourcen Namen geben, die den Qualifizierer `custom-premium`, `custom-standard` und `custom-trial` enthalten.

## <a name="devicefamily"></a>Gerätefamilie

Es ist unwahrscheinlich, dass Sie den `devicefamily` Qualifizierernamen benötigen. Sie können und sollten die Verwendung wann immer möglich vermeiden, da es viele praktische und zuverlässige Verfahren gibt, die Sie stattdessen verwenden können. Diese Techniken werden unter [Erkennen der Plattform, auf der Ihre App ausgeführt wird](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) und [Versionsadaptiver Code](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code) beschrieben.

Als letztes Mittel ist es jedoch möglich, Gerätefamilien-Qualifizierer zum Benennen von Ordnern zu verwenden, die Ihre XAML-Ansichten enthalten (eine XAML-Ansicht ist eine XAML-Datei, die UI-Layout und Steuerelemente enthält).

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

Oder Sie können Dateien benennen.

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

In jedem Fall teilt jede Kopie von `MainPage.[<qualifier>].xaml` eine gemeinsame `MainPage.xaml.cs`, die in Ihrem Projekt hinsichtlich des Namens, Speicherorts und Inhalts unverändert bleibt.

Sie können ebenfalls einen Gerätefamilien-Qualifizierer verwenden, um einen Ressourcen-Dateinamen (`.resw`) oder einen Ordner zu benennen. Wenn Ihre App beispielsweise auf der Familie der Mobilgeräte ausgeführt wird, verwendet das UI-Element `<TextBlock x:Uid="DeviceFriendlyName"/>` die Text- und Vordergrund-Ressourcen, die in Ihrer `Resources.devicefamily-mobile.resw`-Datei enthalten sind.

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

Weitere Informationen zur Verwendung einer Ressourcendatei finden Sie unter [Lokalisieren Ihrer UI-Zeichenfolgen](localize-strings-ui-manifest.md).

## <a name="dxfeaturelevel"></a>DXFeatureLevel

Es ist unwahrscheinlich, dass Sie den `dxfeaturelevel` Qualifizierernamen benötigen. Es wurde entwickelt, um mit Direct3D-Spielressourcen verwendet zu werden, um Vorgängerversionen der Ressourcen zu laden, die einer bestimmten Vorgängerversion der Hardwarekonfiguration dieser Zeit entsprechen. Aber die Verbreitung dieser Hardwarekonfiguration ist jetzt so gering, dass es empfohlen wird, dass Sie diese Qualifizierer nicht verwenden.

## <a name="homeregion"></a>HomeRegion

Der `homeregion`-Qualifizierer entspricht auf der Benutzereinstellung für Land/Region. Es stellt den Wohnort des Benutzers dar. Werte umfassen beliebig gültige [BCP-47-Regionstags](https://go.microsoft.com/fwlink/p/?linkid=227302). D. h. jeder **ISO 3166-1-Alpha-2** Regionscode mit zwei Buchstaben sowie ein Satz **ISO 3166-1 numerischen** dreistelliger geografischer Codes für zusammengesetzte Regionen (siehe [Zusammenstellung von Regionscodes der Statistikabteilung der Vereinten Nationen (M49)](https://go.microsoft.com/fwlink/p/?linkid=247929)). Codes für bestimmte wirtschaftliche und andere Gruppierungen sind ungültig.

## <a name="language"></a>Sprache

Ein `language`-Qualifizierer entspricht der Einstellung für die Anzeigesprache. Werte umfassen beliebig gültige [BCP-47-Sprachtags](https://go.microsoft.com/fwlink/p/?linkid=227302). Eine Liste der Sprachen finden Sie unter [IANA Language Subtag Registry](https://go.microsoft.com/fwlink/p/?linkid=227303).

Wenn Ihre App unterschiedliche Sprachen unterstützten soll und Sie Zeichenfolgenliterale im Code oder im XAML-Markup haben, verschieben Sie diese Zeichenfolgen aus dem Code/Markup und in eine Ressourcendatei (`.resw`). Sie können dann eine übersetzte Kopie dieser Ressourcendatei für jede Sprache erstellen, die Ihre App unterstützt.

Verwenden Sie in der Regel einen `language`-Qualifizierer, um die Ordner zu benennen, die die Ressourcen-Dateien enthalten (`.resw`).

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

Lassen Sie den `language-`-Teil des `language`-Qualifizierers aus (d. h. den Qualifizierernamen). Sie können dies nicht mit anderen Arten von Qualifizierern durchführen. Sie können dies nur in einem Ordnernamen tun.

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

Anstatt Ordner zu benennen können Sie einen `language` -Qualifizierer verwenden, um die Ressourcendateien selbst zu benennen.

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

Unter [Lokalisieren Ihrer UI-Zeichenfolgen](localize-strings-ui-manifest.md) finden Sie weitere Informationen zum Lokalisieren Ihrer App mit Zeichenfolgenressourcen und wie Sie Ihrer App eine Zeichenfolgenressource zuweisen.

## <a name="layoutdirection"></a>LayoutDirection

Ein `layoutdirection`-Qualifizierer entspricht der Layoutrichtung der Einstellung für die Anzeigesprache. Beispielsweise muss ein Bild für eine Rechts-nach-links-Sprache wie Arabisch oder Hebräisch unter Umständen gespiegelt werden. Layoutpanels und Bilder in Ihrer Benutzeroberfläche reagieren der Layoutrichtung entsprechend, wenn Sie deren [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)-Eigenschaft festlegen (Informationen hierzu finden Sie unter [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)). Allerdings ist der `layoutdirection`-Qualifizierer nur für Fälle, in denen ein einfaches Kippen nicht ausreicht. Sie können auf die Ausrichtung bestimmter Leserichtungen und Textausrichtungen auf allgemeine Weise reagieren.

## <a name="scale"></a>Skalierung

Windows wählt automatisch einen Skalierungsfaktor für jede Anzeige aus, basierend auf dem DPI-Wert (Punkte pro Zoll) und dem Betrachtungsabstand des Geräts. Weitere Informationen finden Sie unter [Effektive Pixel und Skalierungsfaktor](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor). Sie sollten Ihre Bilder in mehreren empfohlenen Größen (mindestens 100, 200 und 400) erstellen, damit Windows entweder die passende Größe auswählen oder die nächstgelegene Größe verwenden und dann skalieren kann. Damit Windows erkennen kann, welche physische Datei die richtige Bildgröße für den Anzeigeskalierungsfaktor enthält, verwenden Sie einen `scale`-Qualifizierer. Die Skalierung einer Ressource entspricht dem Wert von [DisplayInformation.ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale) oder der Ressource mit der nächstgrößeren Skalierung.

Hier ist ein Beispiel, wie Sie den Qualifizierer auf Ordnerebene festlegen.

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

Und in diesem Beispiel wird er auf Dateiebene festgelegt.

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

Weitere Informationen zum Qualifizieren einer Ressource für `scale` und `targetsize` finden Sie unter [Qualifizieren einer Bildressource als Zielgröße](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="targetsize"></a>TargetSize

Der `targetsize`-Qualifizierer dient in erster Linie zum Angeben von [Dateityp-Zuordnungssymbolen](https://docs.microsoft.com/windows/desktop/shell/how-to-assign-a-custom-icon-to-a-file-type) oder [Protokollsymbolen](https://docs.microsoft.com/windows/desktop/search/-search-3x-wds-ph-ui-extensions), die im Datei-Explorer angezeigt werden. Der Qualifiziererwert stellt die Seitenlänge eines quadratischen Bilds in unformatierten (physischen) Pixeln dar. Die Ressource, deren Wert den Einstellungseigenschaften im Datei-Explorer entspricht, wird geladen. Andernfalls wird die Ressource mit dem nächsten größten Wert geladen, wenn keine genaue Übereinstimmung vorhanden ist.

Definieren Sie Ressourcen, die verschiedene Größen des `targetsize`-Qualifiziererwerts für das App-Symbol darstellen (`/Assets/Square44x44Logo.png`) auf der Registerkarte für visuelle Ressourcen für die App-Paket-Manifest-Designer.

Weitere Informationen zum Qualifizieren einer Ressource für `scale` und `targetsize` finden Sie unter [Qualifizieren einer Bildressource als Zielgröße](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="theme"></a>Design

Der Qualifizierer `theme` wird verwendet, um Ressourcen bereitzustellen, die am besten mit der Standardeinstellung für den App-Modus übereinstimmen oder mit der Überschreibung durch [Application.RequestedTheme](/uwp/api/windows.ui.xaml.application.requestedtheme).


## <a name="shell-light-theme-and-unplated-resources"></a>Shell Design "hell" und unplated Ressourcen
Die *Windows 10 aktualisieren ggf. 2019* eingeführt, ein neue "helle" Design für die Windows-Shell. Daher werden eine Anwendungsressourcen, die zuvor auf einen dunklen Hintergrund angezeigt wurden jetzt auf einen hellen Hintergrund angezeigt. Für apps, apps, die bereitgestellt Altform unplated Ressourcen für die Taskleiste und Fenster Switcher (Alt + Tab, Aufgabenansicht usw.), sollten Sie überprüfen, ob sie über einen hellen Hintergrund akzeptable Kontrast verfügen.

### <a name="providing-light-theme-specific-assets"></a>Bereitstellen von spezifischen Datenobjekte aus Design "hell"
Apps, die eine angepasste Ressource bereitstellen, Shell Design "hell" kann einen neue alternative ressourcenqualifizierer verwenden möchten: `altform-lightunplated`. Dieser Qualifizierer spiegelt den vorhandenen Altform unplated Qualifizierer. 

### <a name="downlevel-considerations"></a>Kompatible Überlegungen
Apps sollten nicht verwendet werden. die `theme-light` Qualifizierer mit dem `altform-unplated` Qualifizierer. Dadurch wird zu einem unvorhersehbaren Verhalten auf RS5 und frühere Versionen von Windows aufgrund der Möglichkeit Ressourcen werden geladen, für die Taskleiste. In früheren Versionen von Windows kann die Design-Light-Version nicht ordnungsgemäß verwendet werden. Die `altform-lightunplated` Qualifizierer wird dieses Problem vermieden. 

### <a name="compatibility-behavior"></a>Kompatibilität-Verhalten
Für Abwärtskompatibilität Kompatibilität mit der Windows Logik zum Erkennen von eine monochrome Symbole, und überprüfen Sie enthält, ob sie mit dem gewünschten Hintergrund steht im Gegensatz zu. Wenn das Symbol nicht kontrastanforderungen zu erfüllen, wird Windows einen Kontrast-weiß-Version der Ressource gesucht. Wenn diese nicht verfügbar ist, wird Windows ausweichen auf den mit vergoldeten Kontakten Version des Medienobjekts.



## <a name="important-apis"></a>Wichtige APIs

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)

## <a name="related-topics"></a>Verwandte Themen

* [Effektive Pixel und Skalierungsfaktor](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [Ressourcenverwaltungssystem](resource-management-system.md)
* [Vorbereiten für die Lokalisierung](https://docs.microsoft.com/previous-versions/windows/apps/hh967762(v=win.10))
* [Erkennen von der Plattform wird Ihrer app auf ausgeführt](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [Familien Geräteübersicht](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
* [Lokalisieren Sie Ihre UI-Zeichenfolgen](localize-strings-ui-manifest.md)
* [BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [United Nations Statistik Division M49 Zusammensetzung Landes-/Regionscodes](https://go.microsoft.com/fwlink/p/?linkid=247929)
* [IANA Language-Sprachtags Registrierung](https://go.microsoft.com/fwlink/p/?linkid=227303)
* [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)
