---
Description: Ihre App kann Bildressourcendateien mit Bildern laden, die speziell auf den Skalierungsfaktor für die Anzeige, das Design, den hohen Kontrast und andere Laufzeitkontexte angepasst wurden.
title: Laden von Bildern und Ressourcen mit Anpassung an Skalierung, Design, hohen Kontrast usw.
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 0a1d6639a901385c3a33fb0ed670b9b7e4cf683e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157604"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>Laden von Bildern und Ressourcen mit Anpassung an Skalierung, Design, hohen Kontrast usw.
Ihre APP kann Bild Ressourcen Dateien (oder andere Medienobjekt Dateien) laden, die für den [Bildschirm Skalierungsfaktor](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), das Design, den hohen Kontrast und andere Lauf Zeit Kontexte zugeschnitten sind. Auf diese Images kann aus imperativem Code oder XAML-Markup verwiesen werden, z. b. als **Quell** Eigenschaft eines **Bilds**. Sie können auch in der Quelldatei des App-Pakets (der `Package.appxmanifest` Datei) angezeigt &mdash; werden, z. b. als Wert für das App-Symbol auf der Registerkarte visuelle Objekte im Visual Studio &mdash; -Manifest-Designer oder auf ihren Kacheln und Toasts. Wenn Sie Qualifizierer in den Dateinamen Ihrer Bilder verwenden und diese optional mithilfe von [**resourcecontext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)laden, können Sie bewirken, dass die am besten geeignete Bilddatei geladen wird, die am besten mit den Lauf Zeit Einstellungen des Benutzers für Anzeige Skala, Design, hohem Kontrast, Sprache und anderen Kontexten übereinstimmt.

Eine Bildressource ist in einer Bild Ressourcen Datei enthalten. Sie können sich das Image auch als Medienobjekt vorstellen und die Datei, in der es enthalten ist, als Medienobjekt Datei. Diese Arten von Ressourcen Dateien finden Sie im Ordner "\assets" Ihres Projekts. Hintergrundinformationen zur Verwendung von Qualifizierern in den Namen der Bild Ressourcen Dateien finden [Sie unter Anpassen von Ressourcen für Sprache, Skalierung und andere Qualifizierer](tailor-resources-lang-scale-contrast.md).

Einige gängige Qualifizierer für Bilder sind [Skala](tailor-resources-lang-scale-contrast.md#scale) [, Design,](tailor-resources-lang-scale-contrast.md#theme) [Kontrast](tailor-resources-lang-scale-contrast.md#contrast)und [targetSize](tailor-resources-lang-scale-contrast.md#targetsize).

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>Qualifizieren einer Bildressource für Skalierung, Design und Kontrast
Der Standardwert für den `scale` Qualifizierer ist `scale-100` . Diese beiden Varianten sind also gleichwertig (beide bieten ein Image in der Skala 100 oder Skalierungsfaktor 1).

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
</pre>
</blockquote>


Sie können Qualifizierer in Ordnernamen anstelle von Dateinamen verwenden. Dies wäre eine bessere Strategie, wenn pro Qualifizierer mehrere Medienobjekt Dateien vorhanden sind. Zum Zweck der Veranschaulichung entsprechen diese beiden Varianten den beiden oben genannten Varianten.

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
</pre>
</blockquote>

Im folgenden finden Sie ein Beispiel dafür, wie Sie Varianten einer Bildressource mit dem &mdash; Namen `/Assets/Images/logo.png` &mdash; für verschiedene Einstellungen für die Anzeige Skala, das Design und den hohen Kontrast bereitstellen können. In diesem Beispiel wird die Ordner Benennung verwendet.

<blockquote>
<pre>
\Assets\Images\contrast-standard\theme-dark
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-standard\theme-light
    \scale-100\logo.png
    \scale-200\logo.png
\Assets\Images\contrast-high
    \scale-100\logo.png
    \scale-200\logo.png
</pre>
</blockquote>

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>Verweisen auf ein Bild oder ein anderes Objekt aus XAML-Markup und Code
Der Name &mdash; oder Bezeichner &mdash; einer Bildressource ist der Pfad und der Dateiname mit den entfernten und allen Qualifizierern. Wenn Sie Ordner und/oder Dateien wie in einem der Beispiele im vorherigen Abschnitt benennen, verfügen Sie über eine einzelne Bildressource und deren Namen (als absoluter Pfad) `/Assets/Images/logo.png` . Im folgenden wird erläutert, wie Sie diesen Namen in XAML-Markup verwenden.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

Beachten Sie, dass Sie das `ms-appx` URI-Schema verwenden, da Sie auf eine Datei verweisen, die aus dem Paket Ihrer APP stammt. Siehe [URI-Schemas](uri-schemes.md). Und im folgenden wird erläutert, wie Sie in imperativem Code auf dieselbe Image-Ressource verweisen.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

Sie können verwenden `ms-appx` , um beliebige Dateien aus dem App-Paket zu laden.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

Das `ms-appx-web` Schema greift auf dieselben Dateien wie zu `ms-appx` , aber im webdepot.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

Verwenden Sie für eines der in diesen Beispielen gezeigten Szenarien die [URI-Konstruktorüberladung](/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_) , die den [UriKind](/dotnet/api/system.urikind)leitet. Geben Sie einen gültigen absoluten URI an, einschließlich des Schemas und der Zertifizierungsstelle, oder lassen Sie die Zertifizierungsstelle wie im obigen Beispiel standardmäßig auf das Paket der APP festgelegt.

Beachten Sie, dass in diesen Beispiel-URIs dem Schema (" `ms-appx` " oder " `ms-appx-web` ") `://` ein "" gefolgt von einem absoluten Pfad folgt. In einem absoluten Pfad bewirkt die führende " `/` ", dass der Pfad vom Stamm des Pakets interpretiert wird.

> [!NOTE]
> Die `ms-resource` (für [Zeichen folgen Ressourcen](localize-strings-ui-manifest.md)) und `ms-appx(-web)` (für Bilder und andere Assets) URI-Schemas führen automatische qualifiziererübereinstimmung aus, um die Ressource zu ermitteln, die am besten für den aktuellen Kontext geeignet ist Das `ms-appdata` URI-Schema (das zum Laden von App-Daten verwendet wird) führt keine automatische Übereinstimmung aus, aber Sie können auf den Inhalt von [resourcecontext. qualifiervalues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) reagieren und die entsprechenden Assets explizit aus app-Daten laden, indem Sie den vollständigen physischen Dateinamen im URI verwenden. Informationen zu app-Daten finden Sie unter [Speichern und Abrufen von Einstellungen und anderen APP-Daten](../design/app-settings/store-and-retrieve-app-data.md). Web-URI-Schemas ( `http` z `https` . b., und `ftp` ) führen keine automatische Übereinstimmung aus. Informationen dazu, was in diesem Fall zu tun ist, finden Sie unter [Hosting und Laden von Images in der Cloud](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud).

Absolute Pfade sind eine gute Wahl, wenn die Bilddateien in der Projektstruktur verbleiben. Wenn Sie in der Lage sein möchten, eine Bilddatei zu verschieben, Sie aber darauf achten, dass Sie sich im selben Speicherort befindet wie die verweisende XAML-Markup Datei, sollten Sie anstelle eines absoluten Pfads einen Pfad verwenden, der relativ zur enthaltenden Markup Datei ist. Wenn Sie dies tun, müssen Sie kein URI-Schema verwenden. In diesem Fall profitieren Sie weiterhin von automatischem qualifizierungsabgleich, dies ist jedoch nur möglich, weil Sie den relativen Pfad in XAML-Markup verwenden.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

Siehe auch [Unterstützung für Kacheln und Toast für Sprache, Skalierung und hohen Kontrast](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

## <a name="qualify-an-image-resource-for-targetsize"></a>Abbild Ressource für targetSize qualifizieren
Sie können die `scale` -und- `targetsize` Qualifizierer für verschiedene Varianten derselben Bildressource verwenden. Sie können Sie jedoch nicht beides auf einer einzelnen Variante einer Ressource verwenden. Außerdem müssen Sie mindestens eine Variante ohne `TargetSize` Qualifizierer definieren. Diese Variante muss entweder einen Wert für definieren `scale` oder den Standardwert verwenden `scale-100` . Diese beiden Varianten der `/Assets/Square44x44Logo.png` Ressource sind also gültig.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

Diese beiden Varianten sind gültig. 

<blockquote>
<pre>
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

Diese Variante ist jedoch nicht gültig.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200_targetsize-24.png
</pre>
</blockquote>

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>Verweisen auf eine Bilddatei aus dem App-Paket Manifest
Wenn Sie Ordner und/oder Dateien als eines der beiden gültigen Beispiele im vorherigen Abschnitt benennen, verfügen Sie über eine einzelne APP-Symbolbild Ressource, deren Name (als relativer Pfad) lautet `Assets\Square44x44Logo.png` . Verweisen Sie in Ihrem App-Paket Manifest einfach mit dem Namen auf die Ressource. Es ist nicht erforderlich, ein URI-Schema zu verwenden.

![Ressource hinzufügen, Englisch](images/app-icon.png)

Das ist alles, was Sie tun müssen, und das Betriebssystem führt automatische qualifiziererübereinstimmung aus, um die Ressource zu ermitteln, die für den aktuellen Kontext am besten geeignet ist. Eine Liste aller Elemente im App-Paket Manifest, die Sie lokalisieren oder anderweitig qualifizieren können, finden Sie unter [lokalisierbare Manifest-Elemente](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="qualify-an-image-resource-for-layoutdirection"></a>Eine Bildressource für LayoutDirection qualifizieren
Siehe [Spiegelungs Bilder](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images).

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>Laden eines Bilds für eine bestimmte Sprache oder einen anderen Kontext
Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md).

Der standardmäßige [**resourcecontext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (abgerufen von [**resourcecontext. getforcurrentview**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) enthält einen qualifiziererwert für jeden Qualifizierernamen, der den standardmäßigen Lauf Zeit Kontext darstellt (d. h. die Einstellungen für den aktuellen Benutzer und den Computer). Bilddateien werden anhand der Qualifizierer &mdash; in ihren Namen anhand &mdash; der Qualifiziererwerte in diesem Lauf Zeit Kontext abgeglichen.

Es kann jedoch vorkommen, dass Ihre APP die Systemeinstellungen außer Kraft setzen und explizit über die Sprache, Skalierung oder einen anderen qualifiziererwert verfügen soll, der bei der Suche nach einem passenden Bild verwendet werden soll. Beispielsweise können Sie genau steuern, wann und welche Bilder mit hohem Kontrast geladen werden.

Dies können Sie erreichen, indem Sie einen neuen **resourcecontext** erstellen (anstatt den Standardwert zu verwenden), dessen Werte überschreiben und dann dieses Kontext Objekt in den bildsuchvorgängen verwenden.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView 
resourceContext.QualifierValues["Contrast"] = "high";
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
var resourceCandidate = namedResource.Resolve(resourceContext);
var imageFileStream = resourceCandidate.GetValueAsStreamAsync().GetResults();
var bitmapImage = new Windows.UI.Xaml.Media.Imaging.BitmapImage();
bitmapImage.SetSourceAsync(imageFileStream);
this.myXAMLImageElement.Source = bitmapImage;
```

Um denselben Effekt auf globaler Ebene zu erzielen, *können* Sie die Qualifiziererwerte in der Standardeinstellung **resourcecontext**überschreiben. Stattdessen wird jedoch empfohlen, [**resourcecontext. setglobalqualifiervalue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)aufzurufen. Sie legen die Werte einmal mit einem Aufruf von **setglobalqualifiervalue** fest. diese Werte sind dann bei jeder Verwendung für Suchvorgänge für den standardmäßigen **resourcecontext** wirksam. Standardmäßig verwendet die [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) -Klasse den Standardwert **resourcecontext**.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>Aktualisieren von Bildern als Reaktion auf Änderungs Ereignisse für qualifiziererwert
Ihre laufende App kann auf Änderungen in Systemeinstellungen reagieren, die die Qualifiziererwerte im Standard Ressourcen Kontext beeinflussen. Jede dieser Systemeinstellungen Ruft das [**mapchanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) -Ereignis auf [**resourcecontext. qualifiervalues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)auf.

Als Reaktion auf dieses Ereignis können Sie die Images mithilfe der standardmäßigen **resourcecontext**-Methode, die standardmäßig von [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) verwendet wird, erneut laden.

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myImageXAMLElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIImages();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIImages());
    }
}

private void RefreshUIImages()
{
    var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
    this.myImageXAMLElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
}
```

## <a name="important-apis"></a>Wichtige APIs
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)
* [Resourcecontext. setglobalqualifiervalue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Zugehörige Themen
* [Passen Sie Ihre Ressourcen für Sprache, Skalierung und andere Qualifizierer an.](tailor-resources-lang-scale-contrast.md)
* [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest](localize-strings-ui-manifest.md)
* [Speichern und Abrufen von Einstellungen und anderen App-Daten](../design/app-settings/store-and-retrieve-app-data.md)
* [Kachel-und Popup Unterstützung für Sprache, Skalierung und hohen Kontrast](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)
* [Lokalisierbare Manifestelemente](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Spiegelungs Bilder](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md)