---
description: Im folgenden Artikel werden alle Eigenschaften und Elemente innerhalb des Kachel Inhalts beschrieben.
title: Kachelinhaltsschema
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Tile content schema
template: detail.hbs
ms.date: 07/28/2017
ms.topic: article
keywords: Windows 10, UWP, Kachel, Kachel Benachrichtigung, Kachel Inhalt, Schema, Kachel Nutzlast
ms.localizationpriority: medium
ms.openlocfilehash: 4d1953e6d745a41c3bdd85d5f4dd3c6c9df8b900
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93034413"
---
# <a name="tile-content-schema"></a>Kachelinhaltsschema

 

Im folgenden werden alle Eigenschaften und Elemente innerhalb des Kachel Inhalts beschrieben.

Wenn Sie anstelle der [Benachrichtigungs Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)unformatierte XML-Daten verwenden möchten, lesen Sie [das XML-Schema](../tiles-and-notifications/adaptive-tiles-schema.md).

[Tilecontent](#tilecontent)
* [Tilevisual](#tilevisual)
  * [Tilebinding](#tilebinding)
    * [Tilebindingcontentadaptive](#tilebindingcontentadaptive)
    * [Tilebindingcontenticonic](#tilebindingcontenticonic)
    * [Tilebindingcontentcontact](#tilebindingcontentcontact)
    * [Tilebindingcontentpeople](#tilebindingcontentpeople)
    * [Tilebindingcontentphotos](#tilebindingcontentphotos)


## <a name="tilecontent"></a>Tilecontent
Tilecontent ist das Objekt der obersten Ebene, das den Inhalt einer Kachel Benachrichtigung beschreibt, einschließlich visueller Elemente.

| Eigenschaft | type | Erforderlich | BESCHREIBUNG |
|---|---|---|---|
| **Visuelles Element** | [-Visualisierung](#tilevisual) | true | Beschreibt den visuellen Teil der Kachel Benachrichtigung. |


## <a name="tilevisual"></a>Tilevisual
Der visuelle Teil der Kacheln enthält die visuellen Spezifikationen für alle Kachel Größen und weitere visuelle Eigenschaften.

| Eigenschaft | type | Erforderlich | BESCHREIBUNG |
|---|---|---|---|
| **TileSmall** | [Tilebinding](#tilebinding) | false | Geben Sie eine optionale kleine Bindung an, um Inhalte für die kleine Kachel Größe anzugeben. |
| **TileMedium** | [Tilebinding](#tilebinding) | false | Geben Sie eine optionale mittlere Bindung an, um den Inhalt für die mittlere Kachel Größe anzugeben. |
| **TileWide** | [Tilebinding](#tilebinding) | false | Stellen Sie eine optionale Breite Bindung bereit, um den Inhalt für die Breite Kachel Größe anzugeben. |
| **Tilelarge** | [Tilebinding](#tilebinding) | false | Stellen Sie eine optionale, große Bindung bereit, um Inhalte für die große Kachel Größe anzugeben. |
| **Branding** | Tilebranding | false | Das Formular, das die Kachel zum Anzeigen der Marke der App verwenden soll. Standardmäßig erbt Branding von der Standard Kachel. |
| **DisplayName** | Zeichenfolge | false | Eine optionale Zeichenfolge, die den anzeigen amen der Kachel beim Anzeigen dieser Benachrichtigung außer Kraft setzt. |
| **Argumente** | Zeichenfolge | false | Neu in Anniversary Update: App-definierte Daten, die über die tileactivatedinfo-Eigenschaft von launchactivatedeventargs an die APP zurückgegeben werden, wenn der Benutzer Ihre APP über die Live-Kachel startet. Auf diese Weise können Sie wissen, welche Kachel Benachrichtigungen der Benutzer beim Tippen auf Ihre Live-Kachel gesehen hat. Auf Geräten ohne das Anniversary Update wird dies einfach ignoriert. |
| **LockDetailedStatus1** | Zeichenfolge | false | Wenn Sie diese Angabe angeben, müssen Sie auch eine tilewide-Bindung bereitstellen. Dies ist die erste Textzeile, die auf dem Sperrbildschirm angezeigt wird, wenn der Benutzer ihre Kachel als detaillierte Status-App ausgewählt hat. |
| **LockDetailedStatus2** | Zeichenfolge | false | Wenn Sie diese Angabe angeben, müssen Sie auch eine tilewide-Bindung bereitstellen. Dies ist die zweite Textzeile, die auf dem Sperrbildschirm angezeigt wird, wenn der Benutzer ihre Kachel als detaillierte Status-App ausgewählt hat. |
| **LockDetailedStatus3** | Zeichenfolge | false | Wenn Sie diese Angabe angeben, müssen Sie auch eine tilewide-Bindung bereitstellen. Dies ist die dritte Textzeile, die auf dem Sperrbildschirm angezeigt wird, wenn der Benutzer ihre Kachel als detaillierte Status-App ausgewählt hat. |
| **BaseUri** | Uri | false | Eine Standard-Basis-URL, die mit relativen URLs in Image Quellen Attributen kombiniert wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Popup Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |
| **Sprache**| Zeichenfolge | false | Das Ziel Gebiets Schema der visuellen Nutzlast, wenn lokalisierte Ressourcen verwendet werden, die als bcp-47-sprach Tags, z. b. "en-US" oder "fr-FR", angegeben werden. Dieses Gebiets Schema wird von einem beliebigen Gebiets Schema überschrieben, das in Bindung oder Text angegeben ist. Wenn keine Angabe erfolgt, wird stattdessen das Gebiets Schema des Systems verwendet. |


## <a name="tilebinding"></a>Tilebinding
Das Bindungs Objekt enthält den visuellen Inhalt für eine bestimmte Kachel Größe.

| Eigenschaft | type | Erforderlich | BESCHREIBUNG |
|---|---|---|---|
| **Inhalt** | [Itilebindingcontent](#itilebindingcontent) | false | Der visuelle Inhalt, der auf der Kachel angezeigt werden soll. Eine von [tilebindingcontentadaptive](#tilebindingcontentadaptive), [tilebindingcontenticonic](#tilebindingcontenticonic), [tilebindingcontentcontact](#tilebindingcontentcontact), [tilebindingcontentpeople](#tilebindingcontentpeople)oder [tilebindingcontentphotos](#tilebindingcontentphotos). |
| **Branding** | Tilebranding | false | Das Formular, das die Kachel zum Anzeigen der Marke der App verwenden soll. Standardmäßig erbt Branding von der Standard Kachel. |
| **DisplayName** | Zeichenfolge | false | Eine optionale Zeichenfolge, um den anzeigen amen der Kachel für diese Kachel Größe zu überschreiben. |
| **Argumente** | Zeichenfolge | false | Neu in Anniversary Update: App-definierte Daten, die über die tileactivatedinfo-Eigenschaft von launchactivatedeventargs an die APP zurückgegeben werden, wenn der Benutzer Ihre APP über die Live-Kachel startet. Auf diese Weise können Sie wissen, welche Kachel Benachrichtigungen der Benutzer beim Tippen auf Ihre Live-Kachel gesehen hat. Auf Geräten ohne das Anniversary Update wird dies einfach ignoriert. |
| **BaseUri** | Uri | false | Eine Standard-Basis-URL, die mit relativen URLs in Image Quellen Attributen kombiniert wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Popup Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |
| **Sprache**| Zeichenfolge | false | Das Ziel Gebiets Schema der visuellen Nutzlast, wenn lokalisierte Ressourcen verwendet werden, die als bcp-47-sprach Tags, z. b. "en-US" oder "fr-FR", angegeben werden. Dieses Gebiets Schema wird von einem beliebigen Gebiets Schema überschrieben, das in Bindung oder Text angegeben ist. Wenn keine Angabe erfolgt, wird stattdessen das Gebiets Schema des Systems verwendet. |


## <a name="itilebindingcontent"></a>Itilebindingcontent
Markerschnittstelle für den Bindungs Inhalt der Kachel. Mit diesen Optionen können Sie auswählen, wie Sie Ihre Kachel Visualisierungen in adaptiver Form angeben möchten, oder eine der speziellen Vorlagen.

| Implementierungen |
| --- |
| [Tilebindingcontentadaptive](#tilebindingcontentadaptive) |
| [Tilebindingcontenticonic](#tilebindingcontenticonic) |
| [Tilebindingcontentcontact](#tilebindingcontentcontact) |
| [Tilebindingcontentpeople](#tilebindingcontentpeople) |
| [Tilebindingcontentphotos](#tilebindingcontentphotos) |


## <a name="tilebindingcontentadaptive"></a>Tilebindingcontentadaptive
Unterstützt für alle Größen. Dies ist die empfohlene Methode, um den Kachel Inhalt anzugeben. Adaptive Kachel Vorlagen neu in Windows 10, und Sie können eine Vielzahl von benutzerdefinierten Kacheln durch adaptive erstellen.

| Eigenschaft | type | Erforderlich | BESCHREIBUNG |
|---|---|---|---|
| **Children** | IList<ITileBindingContentAdaptiveChild> | false | Die visuellen Inline Elemente. [Adaptivetext](#adaptivetext)-, [adaptiveimage](#adaptiveimage)-und [adaptivegroup](#adaptivegroup) -Objekte können hinzugefügt werden. Die untergeordneten Elemente werden in einem vertikalen StackPanel-Stil angezeigt. |
| **Background Image** | [Tilebackgroundimage](#tilebackgroundimage) | false | Ein optionales Hintergrundbild, das hinter dem gesamten Kachel Inhalt angezeigt wird, vollständig bluten. |
| **Peer Image** | [Tilepeer Image](#tilepeekimage) | false | Ein optionales Peek-Bild, das vom oberen Rand der Kachel aus animiert. |
| **Textstapel** | [Tiletextstacking](#tiletextstacking) | false | Steuert den Text Stapel (vertikale Ausrichtung) des untergeordneten Inhalts als Ganzes. |


## <a name="adaptivetext"></a>Adaptivetext
Ein adaptives Textelement.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Text** | Zeichenfolge | false | Der anzuzeigende Text. |
| **Hintstyle** | [Adaptivetextstyle](#adaptivetextstyle) | false | Der Stil steuert die Schriftgröße, die Gewichtung und die Deckkraft des Texts. |
| **Hintwrap** | bool? | false | Legen Sie diese Einstellung auf "true" fest, um die Text um Der Standardwert ist false. |
| **Hintmaxlines** | int? | false | Die maximale Anzahl von Zeilen, die das Textelement anzeigen darf. |
| **Hintminlines** | int? | false | Die Mindestanzahl von Zeilen, die das Textelement anzeigen muss. |
| **Hintalign** | [Adaptivetextalign](#adaptivetextalign) | false | Die horizontale Ausrichtung des Texts. |
| **Sprache** | Zeichenfolge | false | Das Ziel Gebiets Schema der XML-Nutzlast, das als bcp-47-sprach Tags, z. b. "en-US" oder "fr-FR", angegeben ist. Das hier angegebene Gebiets Schema überschreibt ein anderes angegebenes Gebiets Schema, z. b. das in der Bindung oder in Visual. Wenn dieser Wert eine Literalzeichenfolge ist, wird dieses Attribut standardmäßig auf die Benutzeroberflächen Sprache des Benutzers eingestellt. Wenn dieser Wert ein Zeichen folgen Verweis ist, wird dieses Attribut standardmäßig auf das von Windows-Runtime beim Auflösen der Zeichenfolge gewählte Gebiets Schema eingestellt. |


### <a name="adaptivetextstyle"></a>Adaptivetextstyle
Textstil steuert die Schriftgröße, die Gewichtung und die Deckkraft. Die feine Deckkraft ist 60% nicht transparent.

| Wert | Bedeutung |
|---|---|
| **Standard** | Standardwert. Der Stil wird vom Renderer bestimmt. |
| **Caption** | Kleiner als Absatz Schriftgrad. |
| **Captionfeine** | Identisch mit Beschriftung, aber mit feiner Deckkraft. |
| **Text** | Schriftart Größe des Absatzes. |
| **Bodyfein** | Identisch mit Body, aber mit feiner Deckkraft. |
| **Sock** | Schriftart Größe des Absatzes, fett formatiert. Im Wesentlichen die Fett formatierte Version von Body. |
| **Basesubtle** | Identisch mit Basis, aber mit feiner Deckkraft. |
| **Untertitel** | H4-Schrift Grad. |
| **Subtitlefeine** | Identisch mit Untertiteln, aber mit feiner Deckkraft. |
| **Titel** | H3 Schrift Grad. |
| **Titlefeine** | Identisch mit Title, aber mit feiner Deckkraft. |
| **Titleneneral** | Identisch mit Title, aber mit dem oberen/unteren Abstand entfernt. |
| **Subheader** | H2-Schrift Grad. |
| **Subheaderfein** | Identisch mit der unter Kopfzeile, aber mit einer subtilen Deckkraft. |
| **Subheadernumeral** | Identisch mit der unter Kopfzeile, aber mit dem oberen/unteren Abstand entfernt. |
| **Kopfzeile** | H1 Schrift Grad. |
| **Headerfein** | Identisch mit Header, aber mit feiner Deckkraft. |
| **Headernumeral** | Identisch mit dem Header, aber mit dem oberen/unteren Abstand entfernt. |


### <a name="adaptivetextalign"></a>Adaptivetextalign
Steuert die horizontalen alignmen von Text.

| Wert | Bedeutung |
|---|---|
| **Standard** | Standardwert. Die Ausrichtung wird automatisch vom Renderer bestimmt. |
| **Automatisch** | Die Ausrichtung, die von der aktuellen Sprache und Kultur bestimmt wird. |
| **Left** | Richten Sie den Text horizontal linksbündig aus. |
| **Tagesstätte** | Richten Sie den Text in der Mitte horizontal aus. |
| **Right** | Richten Sie den Text horizontal nach rechts aus. |


## <a name="adaptiveimage"></a>Adaptiveimage
Ein Inline Bild.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Quelle** | Zeichenfolge | true | Die URL des Bilds. MS-AppX, MS-APPDATA und http werden unterstützt. Im Fall von Creators Update können webimages bei normalen Verbindungen bis zu 3 MB und bei getakteten Verbindungen 1 MB betragen. Auf Geräten, auf denen das Fall Creators Update noch nicht ausgeführt wird, dürfen webimages nicht größer als 200 KB sein. |
| **Hintcrop** | [Adaptiveimagecrop](#adaptiveimagecrop) | false | Steuern Sie das gewünschte Zuschneiden des Bilds. |
| **Hintremovemargin** | bool? | false | Standardmäßig haben Bilder innerhalb von Gruppen/Untergruppen einen 8px-Rand um diese. Sie können diesen Rand entfernen, indem Sie diese Eigenschaft auf "true" festlegen. |
| **Hintalign** | [Adaptiveimagealign](#adaptiveimagealign) | false | Die horizontale Ausrichtung des Bilds. |
| **AlternateText** | Zeichenfolge | false | Alternativer Text, der das Bild beschreibt, das zu Barrierefreiheits Zwecken verwendet wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Kachel Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |


### <a name="adaptiveimagecrop"></a>Adaptiveimagecrop
Gibt das gewünschte Zuschneiden des Bilds an.

| Wert | Bedeutung |
|---|---|
| **Standard** | Standardwert. Das durch Renderer festgelegte zuschneideverhalten. |
| **None** | Das Bild wird nicht abgeschnitten. |
| **Circle** | Das Bild wird auf eine Kreisform zugeschnitten. |


### <a name="adaptiveimagealign"></a>Adaptiveimagealign
Gibt die horizontale Ausrichtung eines Bilds an.

| Wert | Bedeutung |
|---|---|
| **Standard** | Standardwert. Ausrichtungs Verhalten, das vom Renderer bestimmt wird. |
| **Stretch** | Bild reicht aus, um die verfügbare Breite (und potenziell verfügbare Höhe) auszufüllen, je nachdem, wo das Bild platziert wird. |
| **Left** | Richten Sie das Bild linksbündig aus, und zeigen Sie das Bild bei seiner systemeigenen Auflösung an. |
| **Tagesstätte** | Richten Sie das Bild horizontal in der Mitte aus, und betrachten Sie das Bild bei seiner nativen Auflösung. |
| **Right** | Richten Sie das Bild rechtsbündig aus, und zeigen Sie das Bild bei seiner systemeigenen Auflösung an. |


## <a name="adaptivegroup"></a>Adaptivegroup
Gruppen stellen semantisch fest, dass der Inhalt in der Gruppe als Ganzes angezeigt oder nicht angezeigt werden muss, wenn er nicht passt. Gruppen ermöglichen auch das Erstellen mehrerer Spalten.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Children** | IList<[adaptivesubgroup](#adaptivesubgroup)> | false | Untergruppen werden als vertikale Spalten angezeigt. Sie müssen Untergruppen verwenden, um Inhalte innerhalb einer adaptivegroup bereitzustellen. |


## <a name="adaptivesubgroup"></a>Adaptivesubgroup
Untergruppen sind vertikale Spalten, die Text und Bilder enthalten können.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Children** | IList<[iadaptivesubgroupchild](#iadaptivesubgroupchild)> | false | [Adaptivetext](#adaptivetext) und [adaptiveimage](#adaptiveimage) sind gültige untergeordnete Elemente von Untergruppen. |
| **Hintweight** | int? | false | Steuern Sie die Breite dieser Untergruppen Spalte, indem Sie die Gewichtung angeben, relativ zu den anderen Untergruppen. |
| **Hinttextstapel** | [Adaptivesubgrouptextstacking](#adaptivesubgrouptextstacking) | false | Steuert die vertikale Ausrichtung des Inhalts der Untergruppe. |


### <a name="iadaptivesubgroupchild"></a>Iadaptivesubgroupchild
Markerschnittstelle für untergeordnete Untergruppen.

| Implementierungen |
| --- |
| [Adaptivetext](#adaptivetext) |
| [Adaptiveimage](#adaptiveimage) |


### <a name="adaptivesubgrouptextstacking"></a>Adaptivesubgrouptextstacking
Textstacking gibt die vertikale Ausrichtung des Inhalts an.

| Wert | Bedeutung |
|---|---|
| **Standard** | Standardwert. Der Renderer wählt automatisch die vertikale Standardausrichtung aus. |
| **Top** | Vertikale Ausrichtung am oberen Rand. |
| **Tagesstätte** | Vertikal ausgerichtet an der Mitte. |
| **bottom** | Vertikal ausgerichtet am unteren Rand. |


## <a name="tilebackgroundimage"></a>Tilebackgroundimage
Ein Hintergrundbild, das vollständig auf der Kachel angezeigt wird.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Quelle** | Zeichenfolge | true | Die URL des Bilds. MS-AppX, MS-APPDATA und http (s) werden unterstützt. Http-Bilder müssen mindestens 200 KB groß sein. |
| **Hinum** | int? | false | Eine schwarze Überlagerung für das Hintergrundbild. Dieser Wert steuert die Deckkraft des schwarzen Overlay, wobei 0 kein Overlay und 100 vollständig schwarz ist. Der Standardwert ist 20. |
| **Hintcrop** | [Tilebackgroundimagecrop](#tilebackgroundimagecrop) | false | Neu in 1511: Geben Sie an, wie das Bild abgeschnitten werden soll. In Versionen vor 1511 wird dies ignoriert, und das Hintergrundbild wird ohne Zuschneiden angezeigt. |
| **AlternateText** | Zeichenfolge | false | Alternativer Text, der das Bild beschreibt, das zu Barrierefreiheits Zwecken verwendet wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Kachel Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |


### <a name="tilebackgroundimagecrop"></a>Tilebackgroundimagecrop
Steuert das Abschneiden des Hintergrund Bilds.

| Wert | Bedeutung |
|---|---|
| **Standard** | Beim Zuschneiden wird das Standardverhalten des Renderers verwendet. |
| **None** | Das Bild wird nicht abgeschnitten, das Quadrat wird angezeigt. |
| **Circle** | Das Bild wird auf einen Kreis zugeschnitten. |


## <a name="tilepeekimage"></a>Tilepeer Image
Ein Peek-Bild, das vom oberen Rand der Kachel aus animiert.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Quelle** | Zeichenfolge | true | Die URL des Bilds. MS-AppX, MS-APPDATA und http (s) werden unterstützt. Http-Bilder müssen mindestens 200 KB groß sein. |
| **Hinum** | int? | false | Neu in 1511: ein schwarzes Overlay für das Peek-Bild. Dieser Wert steuert die Deckkraft des schwarzen Overlay, wobei 0 kein Overlay und 100 vollständig schwarz ist. Der Standardwert ist 20. In früheren Versionen wird dieser Wert ignoriert, und das Peek-Bild wird mit 0 Overlay angezeigt. |
| **Hintcrop** | [Tileetekimagecrop](#tilepeekimagecrop) | false | Neu in 1511: Geben Sie an, wie das Bild abgeschnitten werden soll. In Versionen vor 1511 wird dies ignoriert, und das Peek-Bild wird ohne Zuschneiden angezeigt. |
| **AlternateText** | Zeichenfolge | false | Alternativer Text, der das Bild beschreibt, das zu Barrierefreiheits Zwecken verwendet wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Kachel Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |


### <a name="tilepeekimagecrop"></a>Tileetekimagecrop
Steuert das Zuschneiden des Peek-Bilds.

| Wert | Bedeutung |
|---|---|
| **Standard** | Beim Zuschneiden wird das Standardverhalten des Renderers verwendet. |
| **None** | Das Bild wird nicht abgeschnitten, das Quadrat wird angezeigt. |
| **Circle** | Das Bild wird auf einen Kreis zugeschnitten. |


### <a name="tiletextstacking"></a>Tiletextstacking
Text Stapel gibt die vertikale Ausrichtung des Inhalts an.

| Wert | Bedeutung |
|---|---|
| **Standard** | Standardwert. Der Renderer wählt automatisch die vertikale Standardausrichtung aus. |
| **Top** | Vertikale Ausrichtung am oberen Rand. |
| **Tagesstätte** | Vertikal ausgerichtet an der Mitte. |
| **bottom** | Vertikal ausgerichtet am unteren Rand. |


## <a name="tilebindingcontenticonic"></a>Tilebindingcontenticonic
Unterstützt auf Small und Medium. Aktiviert eine Icon-Kachel Vorlage, in der Sie ein Symbol und eine Badge-Anzeige neben einander auf der Kachel mit dem klassischen Windows Phone Stil anzeigen können. Die Zahl neben dem Symbol wird durch eine separate Badge-Benachrichtigung erreicht.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Symbol:** | [Tilebasicimage](#tilebasicimage) | true | Um sowohl Desktop-als auch Mobile, kleine und mittlere Kacheln zu unterstützen, sollten Sie ein Quadrat-Seitenverhältnis Bild mit einer Auflösung von 200X200, PNG-Format und Transparenz und ohne Farbe als weiß bereitstellen. Weitere Informationen finden Sie unter: [besondere Kachel Vorlagen](../tiles-and-notifications/special-tile-templates-catalog.md). |


## <a name="tilebindingcontentcontact"></a>Tilebindingcontentcontact
Nur Mobilgerät. Unterstützt auf klein, Mittel und breit.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Image** | [Tilebasicimage](#tilebasicimage) | true | Das anzuzeigende Bild. |
| **Text** | [Tilebasictext](#tilebasictext) | false | Eine Textzeile, die angezeigt wird. Wird auf kleiner Kachel nicht angezeigt. |


## <a name="tilebindingcontentpeople"></a>Tilebindingcontentpeople
Neu in 1511: wird unterstützt (Desktop und mobil). Zuvor war dies nur Mobilgeräte und nur Mittel und breit.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Images** | IList<[tilebasicimage](#tilebasicimage)> | true | Bilder, die als Kreise umbrochen werden. |


## <a name="tilebindingcontentphotos"></a>Tilebindingcontentphotos
Führt eine Animation mit Fotos aus. Unterstützt für alle Größen.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Images** | IList<[tilebasicimage](#tilebasicimage)> | true | Es können bis zu 12 Abbilder bereitgestellt werden (Mobile wird nur bis zu 9 angezeigt), die für die Bildschirmpräsentation verwendet werden. Durch das Hinzufügen von mehr als 12 wird eine Ausnahme ausgelöst. |


### <a name="tilebasicimage"></a>Tilebasicimage
Ein Bild, das für verschiedene spezielle Vorlagen verwendet wird.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Quelle** | Zeichenfolge | true | Die URL des Bilds. MS-AppX, MS-APPDATA und http (s) werden unterstützt. Http-Bilder müssen mindestens 200 KB groß sein. |
| **AlternateText** | Zeichenfolge | false | Alternativer Text, der das Bild beschreibt, das zu Barrierefreiheits Zwecken verwendet wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Kachel Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |


### <a name="tilebasictext"></a>Tilebasictext
Ein grundlegendes Textelement, das für verschiedene spezielle Vorlagen verwendet wird.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Text** | Zeichenfolge | false | Der anzuzeigende Text. |
| **Sprache** | Zeichenfolge | false | Das Ziel Gebiets Schema der XML-Nutzlast, das als bcp-47-sprach Tags, z. b. "en-US" oder "fr-FR", angegeben ist. Das hier angegebene Gebiets Schema überschreibt ein anderes angegebenes Gebiets Schema, z. b. das in der Bindung oder in Visual. Wenn dieser Wert eine Literalzeichenfolge ist, wird dieses Attribut standardmäßig auf die Benutzeroberflächen Sprache des Benutzers eingestellt. Wenn dieser Wert ein Zeichen folgen Verweis ist, wird dieses Attribut standardmäßig auf das von Windows-Runtime beim Auflösen der Zeichenfolge gewählte Gebiets Schema eingestellt. |


## <a name="related-topics"></a>Verwandte Themen

* [Schnellstart: Senden einer lokalen Kachelbenachrichtigung](../tiles-and-notifications/sending-a-local-tile-notification.md)
* [Benachrichtigungsbibliothek auf GitHub](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)
