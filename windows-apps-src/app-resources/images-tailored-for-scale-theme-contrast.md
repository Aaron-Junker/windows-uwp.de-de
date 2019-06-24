---
Description: Ihre App kann Bild-Ressourcendateien mit Bildern laden, die speziell auf den Skalierungsfaktor für die Anzeige, das Design, den hohen Kontrast und anderen Laufzeitkontexte angepasst wurden.
title: Laden von Bildern und Ressourcen mit Anpassung an Skalierung, Design, hohen Kontrast usw.
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: f069899da69e4689e21fa578ff7124fbaf453ea3
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317727"
---
# <a name="load-images-and-assets-tailored-for-scale-theme-high-contrast-and-others"></a>Laden von Bildern und Ressourcen mit Anpassung an Skalierung, Design, hohen Kontrast usw.
Ihre App kann Bild-Ressourcendateien mit Bildern (und andere Ressourcendateien) laden, die speziell auf den [Skalierungsfaktor für die Anzeige](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md), das Design, den hohen Kontrast und anderen Laufzeitkontexte angepasst wurden Auf diese Bilder kann im imperativen Code oder im XAML-Markup verwiesen werden, z. B. als **Quellen**-Eigenschaft eines **Bildes**. Sie können auch in Ihrer App-Paketmanifest-Quelldatei erscheinen (in der Datei `Package.appxmanifest`), z. B. als Wert für das App-Symbol auf der Registerkarte „Visuelle Anlagen” des Manifest-Designers von Visual Studio, oder auf Ihren Kacheln und Popups. Mithilfe von Qualifizierern in den Dateinamen Ihrer Bilder und optional durch das dynamische Laden mithilfe des [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live), können Sie die am besten geeignete Bilddatei aufrufen, die den Laufzeiteinstellungen des Benutzers am besten für die Anzeige, das Design, den hohen Kontrast, die Sprache und anderen Kontexten entsprechen.

Eine Bildressource ist in einer Bild-Ressourcendatei enthalten. Sie können Bilder auch als Ressource betrachten, und die Datei, die es enthält als Ressourcendatei. Diese Arten von Ressourcendateien finden Sie im Ordner des Projekts \Assets. Hintergrundinformationen zur Verwendung von Qualifizierern in den Namen der Bild-Ressourcendateien finden Sie unter [Passen Sie Ihrer Ressourcen der Sprache, Skalierung und anderen Qualifizierern an](tailor-resources-lang-scale-contrast.md).

Einige allgemeine Qualifizierer für Bilder sind [Skalierung](tailor-resources-lang-scale-contrast.md#scale), [Design](tailor-resources-lang-scale-contrast.md#theme), [Kontrast](tailor-resources-lang-scale-contrast.md#contrast) und [Zielgröße](tailor-resources-lang-scale-contrast.md#targetsize).

## <a name="qualify-an-image-resource-for-scale-theme-and-contrast"></a>Qualifizieren von Bildressourcen für Skalierung, Design und Kontrast
Der Standardwert für den `scale` Qualifizierer lautet `scale-100`. Daher sind diese beiden Varianten gleichwertig (sie bieten ein Bild mit einer Skalierung von 100 oder einem Skalierungsfaktor von 1).

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\logo.scale-100.png
</pre>
</blockquote>


Sie können Qualifizierer in Ordnernamen anstelle von Dateinamen verwenden. Wenn Sie mehrere Ressourcendateien pro Qualifizierer haben, ist dies eine bessere Strategie. Zu Demonstrationszwecken sind diese zwei Varianten mit den beiden obigen gleichwertig.

<blockquote>
<pre>
\Assets\Images\logo.png
\Assets\Images\scale-100\logo.png
</pre>
</blockquote>

Als Nächstes sehen Sie ein Beispiel zur Bereitstellung von Varianten in einer Bildressource&mdash;mit dem Namen `/Assets/Images/logo.png`&mdash;für unterschiedliche Einstellungen der Anzeige: Skalierung, Design, hoher Kontrast. Dieses Beispiel verwendet die Ordnerbenennung.

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

## <a name="reference-an-image-or-other-asset-from-xaml-markup-and-code"></a>Referenzieren eines Bilds oder eines anderen Elements in XAML-Markup und Code
Der Name&mdash;oder Bezeichner&mdash;einer Bildressource ist der Pfad und Dateiname, aus dem alle Qualifizierer entfernt wurden. Wenn Sie Ordner und/oder Dateien wie im obigen Beispiele des vorherigen Abschnitts benannt haben, erhalten Sie eine einzelne Bild-Ressource und der Anzeigename (als absoluter Pfad) lautet `/Assets/Images/logo.png`. So verwenden Sie diesen Namen in XAML-Markups.

```xaml
<Image x:Name="myXAMLImageElement" Source="ms-appx:///Assets/Images/logo.png"/>
```

Beachten Sie, dass Sie das `ms-appx`-URI-Schema verwenden, da Sie auf eine Datei verweisen, die von Ihrem App-Paket stammt. Weitere Informationen finden Sie unter [URI-Schemen](uri-schemes.md). Hier erfahren Sie, wie Sie auf die gleiche Bildressource in imperativem Code verweisen.

```csharp
this.myXAMLImageElement.Source = new BitmapImage(new Uri("ms-appx:///Assets/Images/logo.png"));
```

Sie können mit `ms-appx` jede beliebige Datei aus dem App-Paket laden.

```csharp
var uri = new System.Uri("ms-appx:///Assets/anyAsset.ext");
var storagefile = await Windows.Storage.StorageFile.GetFileFromApplicationUriAsync(uri);
```

Das `ms-appx-web`-Schema greift auf die gleichen Dateien wie `ms-appx` zu, jedoch im Webdepot.

```xaml
<WebView x:Name="myXAMLWebViewElement" Source="ms-appx-web:///Pages/default.html"/>
```

```csharp
this.myXAMLWebViewElement.Source = new Uri("ms-appx-web:///Pages/default.html");
```

Für die in diesen Beispielen gezeigten Szenarien verwenden Sie die [URI-Konstruktor](https://docs.microsoft.com/en-us/dotnet/api/system.uri.-ctor?view=netcore-2.0#System_Uri__ctor_System_String_)-Überladung, die [UriKind](https://docs.microsoft.com/dotnet/api/system.urikind?view=netframework-4.8) ableitet. Geben Sie einen gültigen absoluten URI einschließlich Schema und Autorität ein oder überlassen Sie die Autorität dem Standardwert des App-Pakets, wie im obigen Beispiel gezeigt.

Beachten Sie, wie auf die Beispielschemen-URIs ("`ms-appx` "oder" `ms-appx-web`") folgt, denen "`://`" folgt, das einem absoluten Pfad folgt. In einem absoluten Pfad, bewirkt das führende "`/`", das der Pfad aus dem Stammverzeichnis des Pakets interpretiert wird.

> [!NOTE]
> Die `ms-resource` (für [Zeichenfolgenressourcen](localize-strings-ui-manifest.md)) und `ms-appx(-web)` (für Bilder und andere Ressourcen) URI-Schemas führen Sie automatische Qualifizierer zugeordnet werden, um die Ressource zu finden, die für den aktuellen Kontext am besten geeignet ist. Das `ms-appdata`URI-Schema (das zum Laden von App-Daten verwendet wird) führt keinen automatischen Abgleich durch, Sie können allerdings auf den Inhalt des [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) reagieren und die entsprechenden Ressourcen explizit von App-Daten mit ihrem gesamten physischen Dateinamen in der URI verwenden. Weitere Informationen zu Anwendungsdaten finden Sie unter [Speichern und Abrufen von Einstellungen und anderen App-Daten](../design/app-settings/store-and-retrieve-app-data.md). Web-URI-Schemen (z. B. `http`, `https`  und `ftp`) führen keinen automatischen Abgleich durch. Informationen darüber, was Sie in diesem Fall tun sollten, finden Sie unter [Hosting and loading images in the cloud](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md#hosting-and-loading-images-in-the-cloud).

Absolute Pfade sind eine gute Wahl, wenn die Dateien dort bleiben, wo sie sich in der Projektstruktur befinden. Wenn Sie eine Bilddatei verschieben möchten, Sie allerdings sehr genau darauf achten möchten, dass sie in der gleichen Position relativ zu der verweisenden XAML-Markup-Datei bleibt, können Sie anstelle eines absoluten Pfads einen Pfad verwenden, der relativ zur enthaltenden Markup-Datei existiert. Wenn Sie dies tun, brauchen Sie kein URI-Schema. Sie profitieren in diesem Fall auch weiterhin vom automatischen Qualifiziererabgleich, jedoch nur deshalb, weil Sie den relativen Pfad im XAML-Markup verwenden.

```xaml
<Image Source="Assets/Images/logo.png"/>
```

Siehe ebenfalls [Kachel- und Popupunterstützung für Sprache, Skalierung und hohen Kontrast](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

## <a name="qualify-an-image-resource-for-targetsize"></a>Qualifizieren einer Bildressource als Zielgröße
Sie können die `scale` und `targetsize`-Qualifizierer auf verschiedenen Varianten der gleichen Bildressource verwenden. Sie können diese allerdings nicht gleichzeitig auf einer einzelnen Variante einer Ressource verwenden. Es muss ebenfalls mindestens eine Variante ohne `TargetSize`-Qualifizierer definiert sein. Diese Variante muss entweder einen Wert für `scale` definieren oder ihn standardmäßig auf `scale-100` lassen. Diese beiden Varianten der `/Assets/Square44x44Logo.png`-Ressource sind ebenfalls gültig.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200.png
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

Und diese zwei Varianten sind gültig. 

<blockquote>
<pre>
\Assets\Square44x44Logo.png // defaults to scale-100
\Assets\Square44x44Logo.targetsize-24.png
</pre>
</blockquote>

Diese Variante ist allerdings nicht gültig.

<blockquote>
<pre>
\Assets\Square44x44Logo.scale-200_targetsize-24.png
</pre>
</blockquote>

## <a name="refer-to-an-image-file-from-your-app-package-manifest"></a>Verweisen auf eine Bilddatei aus Ihrem App-Paketmanifest
Wenn Sie Ordner und/oder Dateien wie in einem der obigen Beispiele des vorherigen Abschnitts benannt haben, erhalten Sie eine einzelne Symbolbildressource und der Anzeigename (als relativer Pfad) lautet `Assets\Square44x44Logo.png`. Verweisen Sie in Ihrem App-Paketmanifest einfach auf die Ressource nach Namen. Es ist kein URI-Schema notwendig.

![Ressource hinzufügen, Englisch](images/app-icon.png)

Dies ist alles, was, die Sie tun müssen. Das Betriebssystem führt einen automatischen Qualifiziererabgleich aus, um die Ressource zu suchen, die für den aktuellen Kontext am besten geeignet ist. Sie finden unter [Localizable Manifest Elemente](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live) eine Liste aller Elemente im App-Paketmanifest, die Sie lokalisieren oder anderweitig auf diese Weise qualifizieren können.

## <a name="qualify-an-image-resource-for-layoutdirection"></a>Qualifizieren einer Bildressource als Layoutrichtung
Weitere Informationen finden Sie unter [Spiegeln von Bildern](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images).

## <a name="load-an-image-for-a-specific-language-or-other-context"></a>Laden von Bildern für eine bestimmte Sprache oder anderen Kontext
Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md).

Der standardmäßige [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (abgerufen über [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) enthält einen Qualifiziererwert für jeden Qualifizierernamen, der den standardmäßigen Laufzeitkontext darstellt (also die Einstellungen für den aktuellen Benutzer und Computer). Bilddateien werden abgeglichen&mdash;basierend auf den Qualifizierern in ihren Namen&mdash;und anhand der Qualifiziererwerte in diesem Laufzeitkontext.

Sie können die Systemeinstellungen Ihrer App außer Kraft setzen und explizit die Sprache, Skalierung oder andere Qualifiziererwerte festlegen, wenn ein übereinstimmendes Bild geladen werden soll. Sie können z. B. steuern, wann und welche Bilder mit hohem Kontrast geladen werden sollen.

Sie können hierzu einen neuen **ResourceContext** erstellen (anstelle des Standardmäßigen), die Werte überschreiben und das Kontextobjekt anschließend in Ihren Suchvorgängen verwenden.

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

Um den gleichen Effekt auf globaler Ebene zu erzielen, *können* Sie die Qualifiziererwerte im standardmäßigen **ResourceContext** außer Kraft setzen. Wir empfehlen, stattdessen [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_) aufzurufen. Sie legen die Werte nur einmal mit einem Aufruf von **SetGlobalQualifierValue** fest und diese Werte sind auf dem standardmäßigen **ResourceContext** jedes Mal gültig, wenn Sie ihn für Suchvorgänge verwenden. Standardmäßig verwendet die [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)-Klasse den standardmäßigen **ResourceContext**.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Contrast", "high");
var namedResource = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap[@"Files/Assets/Logo.png"];
this.myXAMLImageElement.Source = new Windows.UI.Xaml.Media.Imaging.BitmapImage(namedResource.Uri);
```

## <a name="updating-images-in-response-to-qualifier-value-change-events"></a>Aktualisieren von Bildern als Antwort auf Änderungsereignisse im Qualifiziererwert
Die aktive App kann auf Änderungen in den Systemeinstellungen reagieren, die die Qualifiziererwerte im standardmäßigen Ressourcekontext betreffen. Die folgenden Systemeinstellungen rufen ein [ **"MapChanged"** ](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)-Ereignis auf [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) auf.

Als Reaktion auf dieses Ereignis können Sie Ihre Bilder mithilfe des standardmäßigen **ResourceContext** aktualisieren, den der [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) standardmäßig verwendet.

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
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [Passen Sie Ihre Ressourcen für die Sprache, Skalierung und andere Kennzeichner](tailor-resources-lang-scale-contrast.md)
* [Lokalisieren von Zeichenfolgen auf der Benutzeroberfläche und im App-Paketmanifest](localize-strings-ui-manifest.md)
* [Speichern und Abrufen von Einstellungen und anderen App-Daten](../design/app-settings/store-and-retrieve-app-data.md)
* [Kachel- und popupbildgrößen unterstützen, für die Sprache, Skalierung und hoher Kontrast](tile-toast-language-scale-contrast.md)
* [Lokalisierbaren manifest Elementen](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [Datenbankspiegelung images](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md#mirroring-images)
* [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md)
