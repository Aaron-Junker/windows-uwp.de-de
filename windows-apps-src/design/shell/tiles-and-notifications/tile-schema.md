---
Description: Im folgenden Artikel werden alle Eigenschaften und Elemente für die Kachelinhalte beschrieben.
title: Kachelinhaltsschema
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.date: 07/28/2017
ms.topic: article
keywords: Windows 10, Uwp, Kachel, Kachelbenachrichtigung, Kachelinhalt, Schema, Kachelnutzlast
ms.localizationpriority: medium
ms.openlocfilehash: eaf4583be8fdc5f0a70dddb7261b9b2d6d6afc09
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684194"
---
# <a name="tile-content-schema"></a>Kachelinhaltsschema

 

Im Folgenden werden alle Eigenschaften und Elemente für die Kachelinhalte beschrieben.

Wenn Sie anstelle der [Benachrichtigungsbibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) lieber unformatiertes XML verwenden, finden Sie im [XML-Schema](../tiles-and-notifications/adaptive-tiles-schema.md) weitere Informationen.

[Tilecontent](#tilecontent)
* [Tilevisual](#tilevisual)
  * [Tilebinding](#tilebinding)
    * [Tilebindingcontentadaptive](#tilebindingcontentadaptive)
    * [Tilebindingcontenticonic](#tilebindingcontenticonic)
    * [Tilebindingcontentcontact](#tilebindingcontentcontact)
    * [Tilebindingcontentpeople](#tilebindingcontentpeople)
    * [Tilebindingcontentphotos](#tilebindingcontentphotos)


## <a name="tilecontent"></a>TileContent
TileContent ist das Objekt der obersten Ebene, das die Inhalte einer Kachelbenachrichtigung beschreibt, einschließlich der visuellen Elemente.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich | Beschreibung |
|---|---|---|---|
| **Grafisches Element** | [-Visualisierung](#tilevisual) | wahr | Beschreibt den visuellen Teil der Kachelbenachrichtigung. |


## <a name="tilevisual"></a>TileVisual
Der Visual-Teil der Kacheln enthält die visuellen Spezifikationen für alle Kachelgrößen und weitere Eigenschaften zur Visualisierung.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich | Beschreibung |
|---|---|---|---|
| **Tilesmall** | [Tilebinding](#tilebinding) | Falsch | Geben Sie eine optionale Small-Bindung an, um die Inhalte für die Small-Kachelgröße anzugeben. |
| **Tilemedium** | [Tilebinding](#tilebinding) | Falsch | Geben Sie eine optionale Medium-Bindung an, um die Inhalte für die Medium-Kachelgröße anzugeben. |
| **Tilewide** | [Tilebinding](#tilebinding) | Falsch | Geben Sie eine optionale Wide-Bindung an, um die Inhalte für die Wide-Kachelgröße anzugeben. |
| **Tilelarge** | [Tilebinding](#tilebinding) | Falsch | Geben Sie eine optionale Large-Bindung an, um die Inhalte für die Large-Kachelgröße anzugeben. |
| **Branding** | TileBranding | Falsch | Die Form, die von der Kachel zum Anzeigen des Brands der App verwendet werden sollte. Erbt standardmäßig das Branding der Standardkachel. |
| **DisplayName** | String | Falsch | Eine optionale Zeichenfolge zum Überschreiben des Anzeigenamens der Kachel beim Anzeigen dieser Benachrichtigung. |
| **Argumente** | String | Falsch | Neues im Anniversary Update: Von der App definierte Daten, die über die Eigenschaft TileActivatedInfo von LaunchActivatedEventArgs an die App zurückgegeben werden wenn der Benutzer die App über die Live-Kachel startet. Dadurch wissen Sie, welche Kachelbenachrichtigung der Benutzer beim Tippen auf die Live-Kachel gesehen hat. Bei Geräten ohne Anniversary Update wird dies einfach ignoriert. |
| **LockDetailedStatus1** | String | Falsch | Wenn Sie dies angeben, müssen Sie auch eine TileWide-Bindung bereitstellen. Dies ist die erste Zeile des Texts, der auf dem Sperrbildschirm angezeigt wird, wenn der Benutzer die Kachel der App als detaillierte Status-App ausgewählt hat. |
| **LockDetailedStatus2** | String | Falsch | Wenn Sie dies angeben, müssen Sie auch eine TileWide-Bindung bereitstellen. Dies ist die zweite Zeile des Texts, der auf dem Sperrbildschirm angezeigt wird, wenn der Benutzer die Kachel der App als detaillierten Status-App ausgewählt hat. |
| **LockDetailedStatus3** | String | Falsch | Wenn Sie dies angeben, müssen Sie auch eine TileWide-Bindung bereitstellen. Dies ist die dritte Zeile des Texts, der auf dem Sperrbildschirm angezeigt wird, wenn der Benutzer die Kachel der App als detaillierten Status-App ausgewählt hat. |
| **BaseUri** | Uri | Falsch | Eine grundlegende Standard-URL, die mit relativen URLs in Bildquellattributen kombiniert wird. |
| **Addimagequery** | bool? | Falsch | Legen Sie den Wert auf „Wahr“ fest, um an die in der Popupbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |
| **Sprache**| String | Falsch | Das Zielgebietsschema der visuellen Nutzlast bei Verwendung lokalisierter Ressourcen, angegeben als BCP-47-Sprachtags wie z. B. „en-US“ oder „fr-FR“. Dieses Gebietsschema wird von jedem in der Bindung oder dem Text angegebenen Gebietsschema überschrieben. Falls nicht angegeben, wird stattdessen das Gebietsschema des Systems verwendet. |


## <a name="tilebinding"></a>TileBinding
Das Binding-Objekt enthält die visuellen Inhalte für eine bestimmte Kachelgröße.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich | Beschreibung |
|---|---|---|---|
| **Inhalt** | [Itilebindingcontent](#itilebindingcontent) | Falsch | Die visuellen Inhalte, die auf der Kachel angezeigt werden. [TileBindingContentAdaptive](#tilebindingcontentadaptive), [TileBindingContentIconic](#tilebindingcontenticonic), [TileBindingContentContact](#tilebindingcontentcontact), [TileBindingContentPeople](#tilebindingcontentpeople) oder [TileBindingContentPhotos](#tilebindingcontentphotos). |
| **Branding** | TileBranding | Falsch | Die Form, die von der Kachel zum Anzeigen des Brands der App verwendet werden sollte. Erbt standardmäßig das Branding der Standardkachel. |
| **DisplayName** | String | Falsch | Eine optionale Zeichenfolge zum Überschreiben des Anzeigenamens der Kachel für diese Kachelgröße. |
| **Argumente** | String | Falsch | Neues im Anniversary Update: Von der App definierte Daten, die über die Eigenschaft TileActivatedInfo von LaunchActivatedEventArgs an die App zurückgegeben werden wenn der Benutzer die App über die Live-Kachel startet. Dadurch wissen Sie, welche Kachelbenachrichtigung der Benutzer beim Tippen auf die Live-Kachel gesehen hat. Bei Geräten ohne Anniversary Update wird dies einfach ignoriert. |
| **BaseUri** | Uri | Falsch | Eine grundlegende Standard-URL, die mit relativen URLs in Bildquellattributen kombiniert wird. |
| **Addimagequery** | bool? | Falsch | Legen Sie den Wert auf „Wahr“ fest, um an die in der Popupbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |
| **Sprache**| String | Falsch | Das Zielgebietsschema der visuellen Nutzlast bei Verwendung lokalisierter Ressourcen, angegeben als BCP-47-Sprachtags wie z. B. „en-US“ oder „fr-FR“. Dieses Gebietsschema wird von jedem in der Bindung oder dem Text angegebenen Gebietsschema überschrieben. Falls nicht angegeben, wird stattdessen das Gebietsschema des Systems verwendet. |


## <a name="itilebindingcontent"></a>ITileBindingContent
Markierungsschnittstelle für die Kachel-Bindungsinhalte. Dient zum Festlegen der visuellen Objekte der Kachel in adaptiven Vorlagen oder einer der speziellen Vorlagen.

| Implementierungen |
| --- |
| [Tilebindingcontentadaptive](#tilebindingcontentadaptive) |
| [Tilebindingcontenticonic](#tilebindingcontenticonic) |
| [Tilebindingcontentcontact](#tilebindingcontentcontact) |
| [Tilebindingcontentpeople](#tilebindingcontentpeople) |
| [Tilebindingcontentphotos](#tilebindingcontentphotos) |


## <a name="tilebindingcontentadaptive"></a>TileBindingContentAdaptive
Für alle Größe unterstützt. Dies ist die empfohlene Methode zum Festlegen der Kachelinhalte. Vorlagen für adaptive Kacheln sind in Windows 10 neu. Sie können eine Vielzahl von benutzerdefinierten Kacheln über Vorlagen für adaptive Kacheln erstellen.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich | Beschreibung |
|---|---|---|---|
| **Children** | IList-<ITileBindingContentAdaptiveChild> | Falsch | Die visuellen Inline-Elemente. Es können [AdaptiveText](#adaptivetext)-, [AdaptiveImage](#adaptiveimage)- und [AdaptiveGroup](#adaptivegroup)-Objekte hinzugefügt werden. Die untergeordneten Elemente werden als vertikales StackPanel angezeigt. |
| **Background Image** | [Tilebackgroundimage](#tilebackgroundimage) | Falsch | Ein optionales Hintergrundbild, das randlos hinter den Kachelinhalten angezeigt wird. |
| **Peer Image** | [Tilepeer Image](#tilepeekimage) | Falsch | Ein optionales Vorschaubild, das von oben in die Kachel hineingleitet. |
| **Textstapel** | [Tiletextstacking](#tiletextstacking) | Falsch | Steuert den gestapelten Text (vertikale Ausrichtung) der untergeordneten Inhalte als Ganzes. |


## <a name="adaptivetext"></a>AdaptiveText
Ein adaptives Textelement.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Text** | String | Falsch | Der Text, der angezeigt werden soll. |
| **Hintstyle** | [Adaptivetextstyle](#adaptivetextstyle) | Falsch | Der Stil steuert den Schriftgrad, die Schriftbreite und die Deckkraft des Texts. |
| **Hintwrap** | bool? | Falsch | Legen Sie diese Option auf „Wahr“ fest, um Textumbruch zu aktivieren. Der Standardwert ist „Falsch“. |
| **Hintmaxlines** | int? | Falsch | Die maximale Anzahl der Zeilen, die das Textelement anzeigen darf. |
| **Hintminlines** | int? | Falsch | Die Mindestanzahl der Zeilen, die das Textelement anzeigen muss. |
| **Hintalign** | [Adaptivetextalign](#adaptivetextalign) | Falsch | Die horizontale Ausrichtung des Texts. |
| **Sprache** | String | Falsch | Das Zielgebietsschema der XML-Nutzlast, angegeben als BCP-47-Sprachtags wie z. B. „ en-US“ oder „fr-FR“. Das hier angegebene Gebietsschema überschreibt jedes andere – etwa in der Bindung oder im visuellen Element – angegebene Gebietsschema. Wenn dieser Wert eine Literalzeichenfolge ist, wird für dieses Attribut standardmäßig die Sprache der Benutzeroberfläche des Benutzers verwendet. Wenn dieser Wert ein Zeichenfolgenverweis ist, wird für dieses Attribut standardmäßig das Gebietsschema verwendet, das beim Auflösen der Zeichenfolge von Windows-Runtime ausgewählt wurde. |


### <a name="adaptivetextstyle"></a>AdaptiveTextStyle
Textstil steuert Schriftgrad, Schriftbreite und Deckkraft. Dezente Deckkraft ist 60 % undurchsichtig.

| Value | Bedeutung |
|---|---|
| **Default** | Standardwert. Stil wird durch den Renderer bestimmt. |
| **Beschriftung** | Kleiner als Schriftgrad des Absatzes. |
| **Captionfeine** | Identisch mit „Untertitel für Hörgeschädigte“, aber mit dezenter Deckkraft. |
| **Textkörper** | Schriftgrad des Absatzes. |
| **Bodyfein** | Identisch mit „Textkörper“, aber mit dezenter Deckkraft. |
| **Sock** | Schriftgrad des Absatzes, fette Schriftbreite. Im Wesentlichen die fettgedruckte Version des Textkörpers. |
| **Basesubtle** | Identisch mit „Basis“, aber mit dezenter Deckkraft. |
| **Titel** | H4-Schriftgrad. |
| **Subtitlefeine** | Identisch mit „Untertitel“, aber mit dezenter Deckkraft. |
| **Title** | H3-Schriftgrad. |
| **Titlefeine** | Identisch mit „Titel“, aber mit dezenter Deckkraft. |
| **Titleneneral** | Identisch mit „Titel“, aber ohne Abstand nach oben/unten. |
| **Subheader** | H2-Schriftgrad. |
| **Subheaderfein** | Identisch mit „Unterüberschrift“, aber mit dezenter Deckkraft. |
| **Subheadernumeral** | Identisch mit „Unterüberschrift“, aber ohne Abstand nach oben/unten. |
| **Header** | H1-Schriftgrad. |
| **Headerfein** | Identisch mit „Kopfzeile“, aber mit dezenter Deckkraft. |
| **Headernumeral** | Identisch mit „Kopfzeile“, aber ohne Abstand nach oben/unten. |


### <a name="adaptivetextalign"></a>AdaptiveTextAlign
Steuert die horizontale Ausrichtung des Texts.

| Value | Bedeutung |
|---|---|
| **Default** | Standardwert. Ausrichtung wird automatisch vom Renderer bestimmt. |
| **Auto** | Ausrichtung durch die aktuelle Sprache und Kultur bestimmt. |
| **Links** | Den Text horizontal linksbündig ausrichten. |
| **Tagesstätte** | Den Text horizontal zentrieren. |
| **Rechts** | Den Text horizontal rechtsbündig ausrichten. |


## <a name="adaptiveimage"></a>AdaptiveImage
Ein Inlinebild.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Quelle** | String | wahr | Die URL zum Bild. ms-appx, ms-appdata und http werden unterstützt. Im Fall Creators Update kann die Größe der Webbilder 3 MB für normale Verbindungen und 1 MB für getaktete Verbindungen betragen. Auf Geräten, die noch nicht das Fall Creators Update haben, dürfen Webbilder nicht größer als 200 KB sein. |
| **Hintcrop** | [Adaptiveimagecrop](#adaptiveimagecrop) | Falsch | Steuert den gewünschten Zuschnitt des Bilds. |
| **Hintremovemargin** | bool? | Falsch | Bilder in Gruppen/Untergruppen verfügen standardmäßig über einen Rand von 8 Pixel. Sie können diesen Rand entfernen, indem Sie diese Eigenschaft auf „Wahr“ festlegen. |
| **Hintalign** | [Adaptiveimagealign](#adaptiveimagealign) | Falsch | Die horizontale Ausrichtung des Bilds. |
| **AlternateText** | String | Falsch | Alternativtext, der das Bild beschreibt; wird für Bedienungshilfen verwendet. |
| **Addimagequery** | bool? | Falsch | Legen Sie den Wert auf „Wahr“ fest, um an die in der Kachelbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |


### <a name="adaptiveimagecrop"></a>AdaptiveImageCrop
Gibt den gewünschten Zuschnitt des Bilds an.

| Value | Bedeutung |
|---|---|
| **Default** | Standardwert. Zuschneideverhalten wird vom Renderer bestimmt. |
| **Keine** | Bild wird nicht zugeschnitten. |
| **Kreisen** | Bild wird kreisförmig zugeschnitten. |


### <a name="adaptiveimagealign"></a>AdaptiveImageAlign
Gibt die horizontale Ausrichtung für ein Bild an.

| Value | Bedeutung |
|---|---|
| **Default** | Standardwert. Ausrichtungsverhalten vom Renderer bestimmt. |
| **Tre** | Bild wird gestreckt, um die verfügbare Breite (und möglicherweise auch die verfügbare Höhe, je nachdem, wo das Bild platziert wird) auszufüllen. |
| **Links** | Das Bild links ausrichten, wobei das Bild mit der systemeigenen Auflösung angezeigt wird. |
| **Tagesstätte** | Das Bild zentrieren, wobei das Bild mit der systemeigenen Auflösung angezeigt wird. |
| **Rechts** | Das Bild rechts ausrichten, wobei das Bild mit der systemeigenen Auflösung angezeigt wird. |


## <a name="adaptivegroup"></a>AdaptiveGroup
Gruppen geben semantisch an, dass der Inhalt in der Gruppe entweder als Ganzes oder nicht angezeigt werden soll, wenn nicht genügend Platz vorhanden ist. Gruppen können auch mehrere Spalten erstellen.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Children** | IList<[AdaptiveSubgroup](#adaptivesubgroup)> | Falsch | Untergruppen werden als vertikale Spalten angezeigt. Sie müssen Untergruppen verwenden, um Inhalt innerhalb einer AdaptiveGroup bereitzustellen. |


## <a name="adaptivesubgroup"></a>AdaptiveSubgroup
Untergruppen sind vertikale Spalten, die Text und Bilder enthalten können.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Children** | IList<[IAdaptiveSubgroupChild](#iadaptivesubgroupchild)> | Falsch | [AdaptiveText](#adaptivetext) und [AdaptiveImage](#adaptiveimage) gültige untergeordnete Elemente von Untergruppen. |
| **Hintweight** | int? | Falsch | Steuern Sie die Breite dieser Untergruppenspalte, indem Sie die Breite im Verhältnis zu den anderen Untergruppen angeben. |
| **Hinttextstapel** | [Adaptivesubgrouptextstacking](#adaptivesubgrouptextstacking) | Falsch | Steuern Sie die vertikale Ausrichtung des Inhalts dieser Untergruppe. |


### <a name="iadaptivesubgroupchild"></a>IAdaptiveSubgroupChild
Markierungsschnittstelle für untergeordnete Untergruppenelemente.

| Implementierungen |
| --- |
| [Adaptivetext](#adaptivetext) |
| [Adaptiveimage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>AdaptiveSubgroupTextStacking
TextStacking gibt die vertikale Ausrichtung des Inhalts an.

| Value | Bedeutung |
|---|---|
| **Default** | Standardwert. Renderer wählt automatisch die standardmäßige vertikale Ausrichtung aus. |
| **Top** (Oben) | Vertikal oben ausrichten. |
| **Tagesstätte** | Vertikal zentrieren. |
| **Bottom** (Unten) | Vertikal unten ausrichten. |


## <a name="tilebackgroundimage"></a>TileBackgroundImage
Ein Hintergrundbild, das randlos auf der Kachel angezeigt wird.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Quelle** | String | wahr | Die URL zum Bild. ms-appx, ms-appdata und http(s) werden unterstützt. HTTP-Bilder müssen kleiner als 200 KB sein. |
| **Hinum** | int? | Falsch | Ein schwarzes Overlay auf dem Hintergrundbild. Dieser Wert steuert die Deckkraft des schwarzen Overlays, wobei 0 kein Overlay und 100 vollständig schwarz ist. Die Standardeinstellung ist 20. |
| **Hintcrop** | [Tilebackgroundimagecrop](#tilebackgroundimagecrop) | Falsch | Neu in 1511: Geben Sie an, wie das Bild zugeschnitten werden soll. In Versionen vor 1511 wird dies ignoriert und das Hintergrundbild ohne Zuschneiden angezeigt. |
| **AlternateText** | String | Falsch | Alternativtext, der das Bild beschreibt; wird für Bedienungshilfen verwendet. |
| **Addimagequery** | bool? | Falsch | Legen Sie den Wert auf „Wahr“ fest, um an die in der Kachelbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |


### <a name="tilebackgroundimagecrop"></a>TileBackgroundImageCrop
Steuert das Zuschneiden des Hintergrundbilds.

| Value | Bedeutung |
|---|---|
| **Default** | Beim Zuschneiden wird das Standardverhalten des Renderers verwendet. |
| **Keine** | Bild wird nicht zugeschnitten, wird quadratisch angezeigt. |
| **Kreisen** | Bild wird kreisförmig zugeschnitten. |


## <a name="tilepeekimage"></a>TilePeekImage
Ein Vorschaubild, das von oben in die Kachel hineingleitet.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Quelle** | String | wahr | Die URL zum Bild. ms-appx, ms-appdata und http(s) werden unterstützt. HTTP-Bilder müssen kleiner als 200 KB sein. |
| **Hinum** | int? | Falsch | Neu ab 1511: Ein schwarzes Overlay auf dem Vorschaubild. Dieser Wert steuert die Deckkraft des schwarzen Overlays, wobei 0 kein Overlay und 100 vollständig schwarz ist. Die Standardeinstellung ist 20. In früheren Versionen wird dieser Wert ignoriert und das Peek-Bild wird mit 0 Overlay angezeigt. |
| **Hintcrop** | [Tileetekimagecrop](#tilepeekimagecrop) | Falsch | Neu in 1511: Geben Sie an, wie das Bild zugeschnitten werden soll. In Versionen vor 1511 wird dies ignoriert und das Vorschaubild ohne Zuschneiden angezeigt. |
| **AlternateText** | String | Falsch | Alternativtext, der das Bild beschreibt; wird für Bedienungshilfen verwendet. |
| **Addimagequery** | bool? | Falsch | Legen Sie den Wert auf „Wahr“ fest, um an die in der Kachelbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |


### <a name="tilepeekimagecrop"></a>TilePeekImageCrop
Steuert das Zuschneiden des Vorschaubilds.

| Value | Bedeutung |
|---|---|
| **Default** | Beim Zuschneiden wird das Standardverhalten des Renderers verwendet. |
| **Keine** | Bild wird nicht zugeschnitten, wird quadratisch angezeigt. |
| **Kreisen** | Bild wird kreisförmig zugeschnitten. |


### <a name="tiletextstacking"></a>TileTextStacking
Das Text-Stacking gibt die vertikale Ausrichtung des Inhalts an.

| Value | Bedeutung |
|---|---|
| **Default** | Standardwert. Renderer wählt automatisch die standardmäßige vertikale Ausrichtung aus. |
| **Top** (Oben) | Vertikal oben ausrichten. |
| **Tagesstätte** | Vertikal zentrieren. |
| **Bottom** (Unten) | Vertikal unten ausrichten. |


## <a name="tilebindingcontenticonic"></a>TileBindingContentIconic
Für Small und Medium unterstützt. Ermöglicht eine Symbol-Kachelvorlage, bei der Sie ein Symbol und Badge nebeneinander auf der Kachel anzeigen lassen können, im klassischen Stil von Windows Phone. Die Zahl neben dem Symbol wird durch eine separate Badge-Benachrichtigung erreicht.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Icon** | [Tilebasicimage](#tilebasicimage) | wahr | Um sowohl Desktop- als auch Mobile-Small und -Medium-Kacheln zu unterstützen, stellen Sie mindestens ein Bild mit quadratischem Seitenverhältnis und einer Auflösung von 200x200 im PNG-Format mit Transparenz und keiner anderen Farbe als Weiß zur bereit. Weitere Informationen finden Sie unter [Spezielle Kachelvorlagen](../tiles-and-notifications/special-tile-templates-catalog.md). |


## <a name="tilebindingcontentcontact"></a>TileBindingContentContact
Nur Mobile. Für Small, Medium und Wide unterstützt.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Image** | [Tilebasicimage](#tilebasicimage) | wahr | Das anzuzeigende Bild. |
| **Text** | [Tilebasictext](#tilebasictext) | Falsch | Eine Textzeile, die angezeigt wird. Nicht auf kleiner Kachel angezeigt. |


## <a name="tilebindingcontentpeople"></a>TileBindingContentPeople
In 1511 neu: Unterstützt von Medium, Wide und Large (Desktop und Mobile). Zuvor war dies nur für Mobile und nur Medium und Wide gültig.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Bilder** | IList<[TileBasicImage](#tilebasicimage)> | wahr | Bilder, die sich im Kreis drehen. |


## <a name="tilebindingcontentphotos"></a>TileBindingContentPhotos
Animiert durch eine Diashow mit Fotos. Für alle Größe unterstützt.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Bilder** | IList<[TileBasicImage](#tilebasicimage)> | wahr | Es können bis zu 12 Bilder zur Verfügung gestellt werden (Mobile zeigt nur bis zu 9 Bilder an), die für die Diashow verwendet werden. Das Hinzufügen von mehr als 12 löst eine Ausnahme aus. |


### <a name="tilebasicimage"></a>TileBasicImage
Ein Bild, das für verschiedenen spezielle Vorlagen verwendet wird.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Quelle** | String | wahr | Die URL zum Bild. ms-appx, ms-appdata und http(s) werden unterstützt. HTTP-Bilder müssen kleiner als 200 KB sein. |
| **AlternateText** | String | Falsch | Alternativtext, der das Bild beschreibt; wird für Bedienungshilfen verwendet. |
| **Addimagequery** | bool? | Falsch | Legen Sie den Wert auf „Wahr“ fest, um an die in der Kachelbenachrichtigung angegebene Bild-URL eine Abfragezeichenfolge anzufügen. Verwenden Sie dieses Attribut, wenn Ihr Server Bilder hostet und Abfragezeichenfolgen verarbeiten kann, indem er entweder basierend auf den Abfragezeichenfolgen eine Bildvariante abruft oder die Abfragezeichenfolge ignoriert und das Bild wie angegeben ohne die Abfragezeichenfolge zurückgibt. Diese Abfragezeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. So wird beispielsweise ein in der Benachrichtigung angegebener Wert „www.website.com/images/hello.png“ zu „www.website.com/images/hello.png?ms-scale=100&ms-contrast=standard&ms-lang=en-us“ |


### <a name="tilebasictext"></a>TileBasicText
Ein einfaches Text-Element, das für verschiedene spezielle Vorlagen verwendet wird.

| Eigenschaft | Geben Sie in das Suchfeld auf der Taskleiste | Erforderlich |Beschreibung |
|---|---|---|---|
| **Text** | String | Falsch | Der Text, der angezeigt werden soll. |
| **Sprache** | String | Falsch | Das Zielgebietsschema der XML-Nutzlast, angegeben als BCP-47-Sprachtags wie z. B. „ en-US“ oder „fr-FR“. Das hier angegebene Gebietsschema überschreibt jedes andere – etwa in der Bindung oder im visuellen Element – angegebene Gebietsschema. Wenn dieser Wert eine Literalzeichenfolge ist, wird für dieses Attribut standardmäßig die Sprache der Benutzeroberfläche des Benutzers verwendet. Wenn dieser Wert ein Zeichenfolgenverweis ist, wird für dieses Attribut standardmäßig das Gebietsschema verwendet, das beim Auflösen der Zeichenfolge von Windows-Runtime ausgewählt wurde. |


## <a name="related-topics"></a>Zugehörige Themen

* [Schnellstart: Senden einer Benachrichtigung über eine lokale Kachel](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Benachrichtigungs Bibliothek auf GitHub](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)