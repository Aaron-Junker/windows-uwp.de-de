---
description: In diesem Thema wird das allgemeine Konzept von Qualifizierern, deren Verwendung und der Zweck der einzelnen Qualifizierernamen erläutert.
title: Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 59f0b636384ba133e985f0704e2033c1acc5f15e
ms.sourcegitcommit: efa5f793607481dcae24cd1b886886a549e8d6e5
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/03/2020
ms.locfileid: "89412014"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung, hohen Kontrast und anderen Qualifizierern

In diesem Thema wird das allgemeine Konzept der Ressourcenqualifizierer erläutert, wie sie verwendet werden und wozu die einzelnen Qualifizierernamen dienen. Unter [**resourcecontext. qualifiervalues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) finden Sie eine Verweis Tabelle mit allen möglichen Qualifiziererwerten.

Ihre APP kann Assets und Ressourcen laden, die auf Lauf Zeit Kontexte zugeschnitten sind, wie z. b. Anzeige Sprache, hoher Kontrast, [Anzeige Skalierungsfaktor](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)und viele andere. Die Vorgehensweise besteht darin, die Ordner oder Dateien Ihrer Ressourcen so zu benennen, dass Sie mit den Qualifizierernamen und Qualifiziererwerten übereinstimmen, die diesen Kontexten entsprechen. Beispielsweise möchten Sie, dass Ihre APP einen anderen Satz von Image-Assets im Modus für hohe Kontraste lädt.

Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md).

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>Qualifizierer, qualifiziererwert und Qualifizierer

Ein qualifizierername ist ein Schlüssel, der einem Satz von Qualifiziererwerten zugeordnet wird. Im Gegensatz dazu sind der qualifizierername und die Qualifiziererwerte.

| Kontext | Qualifizierername | Qualifiziererwerte |
| :--------------- | :--------------- | :--------------- |
| Die Einstellung für den hohen Kontrast | Kontrast | Standard, hoch, schwarz, weiß |

Zum Erstellen eines Qualifizierers kombinieren Sie einen Qualifizierernamen mit einem qualifiziererwert `<qualifier name>-<qualifier value>` das Format eines Qualifizierers. `contrast-standard` ein Beispiel für einen Qualifizierer.

Daher ist der Satz von Qualifizierern `contrast-standard` ,, `contrast-high` `contrast-black` und `contrast-white` . Bei Qualifizierernamen und Qualifikations Werten wird die Groß-/Kleinschreibung `contrast-standard`Und `Contrast-Standard` sind z. b. derselbe Qualifizierer.

## <a name="use-qualifiers-in-folder-names"></a>Verwenden von Qualifizierern in Ordnernamen

Hier ist ein Beispiel für die Verwendung von Qualifizierern zum Benennen von Ordnern, die Medienobjekt Dateien enthalten. Verwenden Sie Qualifizierer in Ordnernamen, wenn mehrere Ressourcen Dateien pro Qualifizierer vorhanden sind. Auf diese Weise legen Sie den Qualifizierer einmal auf Ordnerebene fest, und der Qualifizierer gilt für alles innerhalb des Ordners.

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

Wenn Sie die Ordner wie im obigen Beispiel benennen, verwendet Ihre APP die Einstellung für den hohen Kontrast zum Laden von Ressourcen Dateien aus dem Ordner, der für den entsprechenden Qualifizierer benannt ist. Wenn die Einstellung also hoher Kontrast schwarz ist, werden die Ressourcen Dateien im `\Assets\Images\contrast-black` Ordner geladen. Wenn die Einstellung auf keine festgelegt ist (d. h., der Computer befindet sich nicht im Modus für hohe Kontraste), werden die Ressourcen Dateien im `\Assets\Images\contrast-standard` Ordner geladen.

## <a name="use-qualifiers-in-file-names"></a>Verwenden von Qualifizierern in Dateinamen

Anstatt Ordner zu erstellen und zu benennen, können Sie einen Qualifizierer verwenden, um die Ressourcen Dateien selbst zu benennen. Dies empfiehlt sich, wenn Sie nur über eine Ressourcen Datei pro Qualifizierer verfügen. Hier sehen Sie ein Beispiel.

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

Die Datei, deren Name den Qualifizierer enthält, der für die Einstellung am besten geeignet ist, ist die Datei, die geladen wird Diese übereinstimmende Logik funktioniert für Dateinamen genauso wie für Ordnernamen.

## <a name="reference-a-string-or-image-resource-by-name"></a>Verweisen auf eine Zeichen folgen-oder Bildressource anhand ihres Namens

Weitere Informationen finden Sie unter [verweisen auf einen Zeichen folgen Ressourcen Bezeichner aus XAML-Markup](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml), [verweisen auf einen Zeichen folgen Ressourcen Bezeichner aus Code](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code)und verweisen auf [ein Bild oder ein anderes Objekt aus XAML-Markup und-Code](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

## <a name="actual-and-neutral-qualifier-matches"></a>Tatsächlicher und neutraler Qualifizierer
Sie müssen nicht für *jeden* qualifiziererwert eine Ressourcen Datei bereitstellen. Wenn Sie z. b. feststellen, dass Sie nur eine visuelle Ressource für einen hohen Kontrast und eine für den Standard Kontrast benötigen, können Sie diese Objekte wie folgt benennen.

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

Der erste Dateiname enthält den `contrast-high` Qualifizierer. Dieser Qualifizierer ist eine *tatsächliche* Entsprechung für jede hohe Kontrasteinstellung, wenn ein hoher Kontrast *auf on*festgelegt ist Anders ausgedrückt: Es handelt sich um eine schließende Entsprechung. Eine *tatsächliche* Entsprechung kann nur auftreten, wenn der Qualifizierer einen *tatsächlichen* Wert enthält, wie dies in diesem Fall der Fall ist. In diesem Fall `high` ist ein *tatsächlicher* Wert für `contrast` .

Die Datei mit dem Namen `logo.png` hat überhaupt keinen Kontrast Qualifizierer. Das Fehlen eines Qualifizierers ist ein *neutraler* Wert. Wenn keine bevorzugte Entsprechung gefunden werden kann, fungiert der neutrale Wert als Fall Back Übereinstimmung. In diesem Beispiel gibt es keine tatsächliche Entsprechung, wenn der hohe Kontrast *deaktiviert ist.* Die *neutrale* Übereinstimmung ist die beste Entsprechung, die gefunden werden kann, sodass das Asset `logo.png` geladen wird.

Wenn Sie den Namen von in ändern möchten `logo.png` `logo.contrast-standard.png` , würde der Dateiname einen tatsächlichen qualifiziererwert enthalten. Bei einem hohen Kontrast ist `logo.contrast-standard.png` die tatsächliche Übereinstimmung, und das ist die zu ladende Ressourcen Datei. Daher werden dieselben Dateien unter denselben Bedingungen geladen, aber aufgrund von unterschiedlichen Übereinstimmungen.

Wenn Sie nur einen Satz von Assets für einen hohen Kontrast und einen Satz für den Standard Kontrast benötigen, können Sie anstelle von Dateinamen Ordnernamen verwenden. In diesem Fall erhalten Sie durch das Weglassen des Ordner namens vollständige Übereinstimmung.

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

Weitere Informationen zur Funktionsweise der qualifiziererübereinstimmung finden Sie unter [Resource Management System](resource-management-system.md).

## <a name="multiple-qualifiers"></a>Mehrere Qualifizierer

Sie können Qualifizierer in Ordner-und Dateinamen kombinieren. Beispielsweise möchten Sie, dass Ihre APP Image Ressourcen lädt, wenn der Modus für hohe Kontraste eingeschaltet ist *und* der Anzeige Skalierungsfaktor 400 ist. Eine Möglichkeit hierfür ist die Verwendung von untergeordneten Ordnern.

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

Für `logo.png` und die anderen Dateien, die geladen werden sollen, müssen die Einstellungen *beiden* Qualifizierern entsprechen.

Eine andere Möglichkeit besteht darin, mehrere Qualifizierer in einem Ordnernamen zu kombinieren.

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

In einem Ordnernamen kombinieren Sie mehrere Qualifizierer, die durch einen Unterstrich getrennt sind. `<qualifier1>[_<qualifier2>...]` das Format.

Sie können mehrere Qualifizierer in einem Dateinamen im gleichen Format kombinieren.

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

Abhängig von den Tools und dem Workflow, die Sie für die Erstellung von Medienobjekten verwenden, oder von dem, was Sie am einfachsten zu lesen und/oder zu verwalten finden, können Sie entweder eine einzelne Benennungs Strategie für alle Qualifizierer auswählen oder Sie für verschiedene Qualifizierer kombinieren.

## <a name="alternateform"></a>Alternative Form

Der `alternateform` Qualifizierer wird verwendet, um eine alternative Form einer Ressource für einen speziellen Zweck bereitzustellen. Dies wird in der Regel nur von japanischen App-Entwicklern verwendet, um eine Furigana-Zeichenfolge bereitzustellen, für die der Wert `msft-phonetic` reserviert ist (Weitere Informationen finden Sie im Abschnitt "Unterstützung von Furigana für japanische Zeichen folgen, die sortiert werden können" in [Vorbereiten der Lokalisierung](/previous-versions/windows/apps/hh967762(v=win.10))).

Ihr Zielsystem oder Ihre APP muss einen Wert bereitstellen, mit dem `alternateform` Qualifizierer abgeglichen werden. Verwenden Sie das `msft-` Präfix nicht für Ihre eigenen benutzerdefinierten `alternateform` Qualifiziererwerte.

## <a name="configuration"></a>Konfiguration

Es ist unwahrscheinlich, dass Sie den `configuration` Qualifizierernamen benötigen. Sie kann verwendet werden, um Ressourcen anzugeben, die nur auf eine bestimmte Erstellungszeit Umgebung anwendbar sind, z. b. auf nur-Test-Ressourcen.

Der `configuration` Qualifizierer wird verwendet, um eine Ressource zu laden, die am besten mit dem Wert der `MS_CONFIGURATION_ATTRIBUTE_VALUE` Umgebungsvariablen übereinstimmt. Daher können Sie die Variable auf den Zeichen folgen Wert festlegen, der den relevanten Ressourcen zugewiesen wurde, z. b `designer` . oder `test` .

## <a name="contrast"></a>Vergleichen Sie

`contrast`Mit dem Qualifizierer werden Ressourcen bereitgestellt, die den Einstellungen für hohe Kontraste am besten entsprechen.

## <a name="custom"></a>Benutzerdefiniert

Ihre APP kann einen Wert für den `custom` Qualifizierer festlegen, und dann werden Ressourcen geladen, die dem Wert am besten entsprechen. Beispielsweise können Sie Ressourcen basierend auf der Lizenz ihrer App laden. Wenn Ihre APP gestartet wird, wird die Lizenz überprüft und als Wert für den `custom` Qualifizierer verwendet, indem [setglobalqualifiervalue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)aufgerufen wird, wie im Codebeispiel gezeigt.

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

In diesem Szenario würden Sie Ihre Ressourcennamen angeben, die die Qualifizierer `custom-premium` , `custom-standard` und enthalten `custom-trial` .

## <a name="devicefamily"></a>Devicefamily

Es ist unwahrscheinlich, dass Sie den `devicefamily` Qualifizierernamen benötigen. Sie können es vermeiden, wenn dies möglich ist, denn es gibt Techniken, die Sie stattdessen verwenden können, die viel bequemer und robuster sind. Diese Techniken werden unter [erkennen der Plattform, auf der Ihre APP ausgeführt wird](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) , und der [Version von adaptiver Code](../debug-test-perf/version-adaptive-code.md)beschrieben.

Aber als letztes Mittel ist es möglich, devicefamily-Qualifizierer zum Benennen von Ordnern zu verwenden, die Ihre XAML-Ansichten enthalten (eine XAML-Ansicht ist eine XAML-Datei, die das Layout und Steuerelemente der Benutzeroberfläche enthält).

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

Oder Sie können Dateien benennen.

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

In beiden Fällen hat jede Kopie von `MainPage.[<qualifier>].xaml` eine gemeinsame `MainPage.xaml.cs` , die im Projekt in Bezug auf Name, Speicherort und Inhalt unverändert bleibt.

Sie können auch einen devicefamily-Qualifizierer verwenden, um eine Ressourcen Datei ( `.resw` ) oder einen Ordner zu benennen. Wenn Ihre APP beispielsweise auf der mobilen Gerätefamilie ausgeführt wird, verwendet das UI `<TextBlock x:Uid="DeviceFriendlyName"/>` -Element die Text-und Vordergrund Ressourcen, die in der Datei definiert sind, `Resources.devicefamily-mobile.resw` Wenn Sie

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

Weitere Informationen zum Verwenden einer Ressourcen Datei finden [Sie unter Lokalisieren von UI](localize-strings-ui-manifest.md)-Zeichen folgen.

## <a name="dxfeaturelevel"></a>Dxfeaturelevel

Es ist unwahrscheinlich, dass Sie den `dxfeaturelevel` Qualifizierernamen benötigen. Es wurde für die Verwendung mit Direct3D-Spielobjekten entwickelt, damit downlevelressourcen geladen werden, um eine bestimmte downlevelhardware-Konfiguration der Zeit zu erfüllen. Die Verbreitung dieser Hardwarekonfiguration ist jetzt jedoch so niedrig, dass Sie diesen Qualifizierer nicht verwenden sollten.

## <a name="homeregion"></a>Homeregion

Der `homeregion` Qualifizierer entspricht der Einstellung des Benutzers für Land oder Region. Es stellt den Start Speicherort des Benutzers dar. Zu den Werten gehören alle gültigen [bcp-47-Regions Tags](https://tools.ietf.org/html/bcp47). Das heißt, alle **ISO 3166-1 Alpha-2** aus zwei Buchstaben bestehenden Regions Codes sowie der Satz von **ISO 3166-1 numerischen** dreistelligen geografischen Codes für zusammengesetzte Regionen (siehe [United Nations Statistic Division M49 Composition of Region Codes](https://unstats.un.org/unsd/methods/m49/m49regin.htm)). Die Codes für "ausgewählte wirtschaftliche und andere Gruppierungen" sind ungültig.

## <a name="language"></a>Sprache

Ein `language` Qualifizierer entspricht der Einstellung der Anzeige Sprache. Zu den Werten gehören alle gültigen [bcp-47-Sprachtags](https://tools.ietf.org/html/bcp47). Eine Liste der Sprachen finden Sie in der [IANA Language Tags-Registrierung](https://www.iana.org/assignments/language-subtag-registry).

Wenn Sie möchten, dass Ihre APP verschiedene Anzeige Sprachen unterstützt, und Sie Zeichen folgen Literale in Ihrem Code oder in Ihrem XAML-Markup haben, verschieben Sie diese Zeichen folgen aus dem Code/Markup und in eine Ressourcen Datei ( `.resw` ). Sie können dann eine übersetzte Kopie dieser Ressourcendatei für jede Sprache erstellen, die Ihre App unterstützt.

In der Regel verwenden Sie einen `language` Qualifizierer, um die Ordner zu benennen, die ihre Ressourcen Dateien ( `.resw` ) enthalten.

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

Sie können den `language-` Teil eines `language` Qualifizierers (d. h. den Qualifizierernamen) weglassen. Dies ist mit den anderen Arten von Qualifizierern nicht möglich. Dies ist nur in einem Ordnernamen möglich.

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

Anstatt Ordner zu benennen, können Sie `language` die Ressourcen Dateien mithilfe von Qualifizierern benennen.

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

Weitere Informationen zum Lokalisieren der App mithilfe von Zeichen folgen Ressourcen finden Sie unter [Lokalisieren von UI](localize-strings-ui-manifest.md) -Zeichen folgen und verweisen auf eine Zeichen folgen Ressource in Ihrer APP.

## <a name="layoutdirection"></a>LayoutDirection

Ein `layoutdirection` Qualifizierer entspricht der Layoutrichtung der Anzeige Spracheinstellung. Ein Bild kann z. b. für eine Sprache von rechts nach links (z. b. Arabisch oder Hebräisch) gespiegelt werden. Layoutpanels und Bilder in der Benutzeroberfläche reagieren entsprechend auf Layoutrichtung, wenn Sie Ihre [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) -Eigenschaft festlegen (siehe [Anpassen von Layout und Schriftarten und Unterstützung von RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)). Der `layoutdirection` Qualifizierer ist jedoch für Fälle geeignet, in denen das einfache kippen nicht ausreicht, und ermöglicht es Ihnen, auf allgemeinere Weise auf die Direktionalität bestimmter lesereihen und Textausrichtung zu reagieren.

## <a name="scale"></a>Skalieren

Windows wählt automatisch einen Skalierungsfaktor für jede Anzeige basierend auf dem dpi-Faktor (dpi-pro-Zoll) und der Anzeige Distanz des Geräts aus. Weitere Informationen finden Sie unter [effektive Pixel und Skalierungsfaktor](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor). Sie sollten Ihre Images mit verschiedenen empfohlenen Größen (mindestens 100, 200 und 400) erstellen, damit Windows entweder die perfekte Größe auswählen oder die nächstgelegene Größe verwenden und skalieren können. Damit Windows ermitteln kann, welche physische Datei die richtige Größe des Bilds für den Anzeige Skalierungsfaktor enthält, verwenden Sie einen `scale` Qualifizierer. Die Skalierung einer Ressource entspricht dem Wert von [Displayinformation. resolutionscale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale)oder der nächstgrößten skalierten Ressource.

Hier ist ein Beispiel für das Festlegen des Qualifizierers auf Ordnerebene.

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

Und in diesem Beispiel wird Sie auf Dateiebene festgelegt.

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

Informationen zum qualifizieren einer Ressource für `scale` und finden Sie `targetsize` unter [qualifizieren einer Image Ressource für targetSize](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="targetsize"></a>TargetSize

Der `targetsize` Qualifizierer wird primär zum Angeben von [Dateityp](/windows/desktop/shell/how-to-assign-a-custom-icon-to-a-file-type) -Zuordnungs Symbolen oder [Protokoll Symbolen](/windows/desktop/search/-search-3x-wds-ph-ui-extensions) verwendet, die im Datei-Explorer angezeigt werden. Der qualifiziererwert stellt die Seitenlänge eines quadratischen Bilds in unformatierten (physischen) Pixeln dar. Die Ressource, deren Wert mit der Ansichts Einstellung im Datei-Explorer übereinstimmt, wird geladen. oder die Ressource mit dem nächstgrößten Wert, wenn keine genaue Entsprechung vorhanden ist.

Sie können Assets definieren, die mehrere Größen des `targetsize` qualifiziererwerts für das App-Symbol ( `/Assets/Square44x44Logo.png` ) auf der Registerkarte visuelle Objekte des App-Paket Manifest-Designers darstellen.

Informationen zum qualifizieren einer Ressource für `scale` und finden Sie `targetsize` unter [qualifizieren einer Image Ressource für targetSize](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="theme"></a>Design

Der `theme` Qualifizierer wird verwendet, um Ressourcen bereitzustellen, die am besten mit der Standardeinstellung für den Anwendungsmodus oder mit [Application. requestedtheme](/uwp/api/windows.ui.xaml.application.requestedtheme)mit der APP überschrieben werden.


## <a name="shell-light-theme-and-unplated-resources"></a>Shell Light-Design und nicht geplatzte Ressourcen
Das *Windows 10-Update von Mai 2019* hat ein neues "Light"-Design für die Windows-Shell eingeführt. Folglich werden einige Anwendungs Ressourcen, die zuvor in einem dunklen Hintergrund angezeigt wurden, nun in einem hellen Hintergrund angezeigt. Für apps, die apps bereitstellen, die für die Taskleiste und die Fenster Switcher (Alt + Tab, Aufgaben Ansicht usw.) in altform Geschützte Assets bereitgestellt haben, sollten Sie überprüfen, ob Sie einen akzeptablen Kontrast in einem hellen Hintergrund haben.

### <a name="providing-light-theme-specific-assets"></a>Bereitstellen von Design spezifischen Ressourcen
Apps, die eine angepasste Ressource für das Shell Light-Design bereitstellen möchten, können einen neuen alternativen Formular Ressourcen Qualifizierer verwenden: `altform-lightunplated` . Dieser Qualifizierer spiegelt den vorhandenen, in altform ungebundenen Qualifizierer wider. 

### <a name="downlevel-considerations"></a>Überlegungen zu downlevels
Apps sollten nicht den `theme-light` Qualifizierer mit dem `altform-unplated` Qualifizierer verwenden. Dies führt zu unvorhersehbarem Verhalten bei RS5 und früheren Versionen von Windows aufgrund der Art und Weise, wie Ressourcen für die Taskleiste geladen werden. In früheren Versionen von Windows kann die Design-Light-Version falsch verwendet werden. Der `altform-lightunplated` Qualifizierer vermeidet dieses Problem. 

### <a name="compatibility-behavior"></a>Kompatibilitäts Verhalten
Aus Gründen der Abwärtskompatibilität enthält Windows Logik, um ein Mono-Symbol zu erkennen und zu überprüfen, ob es mit dem vorgesehenen Hintergrund in Kontrast steht. Wenn das Symbol die Kontrast Anforderungen nicht erfüllt, wird in Windows nach der Kontrast weißen Version des Assets gesucht. Wenn dies nicht verfügbar ist, greift Windows auf die geplatzte Version des Assets zurück.



## <a name="important-apis"></a>Wichtige APIs

* [Resourcecontext. qualifiervalues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [Setglobalqualifiervalue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue)

## <a name="related-topics"></a>Verwandte Themen

* [Effektive Pixel und Skalierungsfaktor](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [Ressourcenverwaltungssystem](resource-management-system.md)
* [Vorbereiten der Lokalisierung](/previous-versions/windows/apps/hh967762(v=win.10))
* [Erkennen der Plattform, auf der Ihre App ausgeführt wird](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [Programmieren mit Erweiterungs-sdert](/uwp/extension-sdks/device-families-overview)
* [Lokalisieren von UI-Zeichen folgen](localize-strings-ui-manifest.md)
* [BCP-47](https://tools.ietf.org/html/bcp47)
* [Statistik Division der Vereinten Nationen M49 Komposition von Regions Codes](https://unstats.un.org/unsd/methods/m49/m49regin.htm)
* [IANA-sprachsubtag-Registrierung](https://www.iana.org/assignments/language-subtag-registry)
* [Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)