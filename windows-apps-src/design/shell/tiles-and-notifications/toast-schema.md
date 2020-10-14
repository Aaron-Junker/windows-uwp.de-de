---
Description: Im folgenden Artikel werden alle Eigenschaften und Elemente innerhalb des Popup Inhalts beschrieben.
title: Popupinhaltsschema
ms.assetid: 7CBC3BD5-D9C3-4781-8BD0-1F28039E1FA8
label: Toast content schema
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 6399cb3aa6c22e188ed84941c3209632511d90e4
ms.sourcegitcommit: 8b01b9ab7293dad1259da32d1459fdd454796e12
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92020170"
---
# <a name="toast-content-schema"></a>Popupinhaltsschema

 

Im folgenden werden alle Eigenschaften und Elemente innerhalb des Popup Inhalts beschrieben.

Wenn Sie anstelle der [Benachrichtigungs Bibliothek](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/)unformatierte XML-Daten verwenden möchten, lesen Sie [das XML-Schema](https://docs.microsoft.com/uwp/schemas/tiles/toastschema/schema-root).

[Inhalts Inhalt](#toastcontent)
* [-Visualisierung](#toastvisual)
  * ["Deastbindinggeneric"](#toastbindinggeneric)
    * [Ideastbindinggenericchild](#itoastbindinggenericchild)
    * ["Ein"-Logo](#toastgenericapplogo)
    * [Verzeichnis Image](#toastgenericheroimage)
    * ["" "" "" "".](#toastgenericattributiontext)
* [Ideastactions](#itoastactions)
* [Ein-und ausgeschaltet](#toastaudio)
* [Verzeichnis Leser](#toastheader)


## <a name="toastcontent"></a>Inhalts Inhalt
"Inhalts Inhalt" ist das Objekt der obersten Ebene, das den Inhalt einer Benachrichtigung beschreibt, einschließlich visueller Elemente, Aktionen und Audiodaten.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Starten**| Zeichenfolge | false | Eine Zeichenfolge, die an die Anwendung übermittelt wird, wenn Sie durch den Toast aktiviert wird. Das Format und der Inhalt dieser Zeichenfolge werden von der App für eigene Zwecke definiert. Wenn der Benutzer auf den Toast tippt oder klickt, um die zugehörige APP zu starten, stellt die Start Zeichenfolge den Kontext für die APP bereit, der es dem Benutzer ermöglicht, dem Benutzer eine Ansicht anzuzeigen, die für den Popup Inhalt relevant ist, anstatt ihn auf seine Standard Art zu starten |
| **Grafisches Element** | [-Visualisierung](#toastvisual) | true | Beschreibt den visuellen Teil der Popup Benachrichtigung. |
| **Aktionen** | [Ideastactions](#itoastactions) | false | Erstellen Sie optional benutzerdefinierte Aktionen mit Schaltflächen und Eingaben. |
| **Audio** | [Ein-und ausgeschaltet](#toastaudio) | false | Beschreibt den Audioteil der Popup Benachrichtigung. |
| **ActivationType** | ["Deastactivationtype"](#toastactivationtype) | false | Gibt an, welcher Aktivierungstyp verwendet wird, wenn der Benutzer auf den Hauptteil dieses Popups klickt. |
| **Activationoptions** | ["Deastactivationoptions"](#toastactivationoptions) | false | Neu in Creators Update: zusätzliche Optionen im Zusammenhang mit der Aktivierung der Popup Benachrichtigung. |
| **Szenario** | [Ein-/ausgeschaltet](#toastscenario) | false | Deklariert das Szenario, für das der Toast verwendet wird, wie z. b. ein Alarm oder eine Erinnerung. |
| **Display Timestamp** | DateTimeOffset? | false | Neu in Creators Update: Überschreiben Sie den Standardzeit Stempel mit einem benutzerdefinierten Zeitstempel, der angibt, wann Ihr Benachrichtigungs Inhalt tatsächlich übermittelt wurde, und nicht die Zeit, zu der die Benachrichtigung von der Windows-Plattform empfangen wurde. |
| **Header** | [Verzeichnis Leser](#toastheader) | false | Neu in Creators Update: Fügen Sie Ihrer Benachrichtigung einen benutzerdefinierten Header hinzu, um mehrere Benachrichtigungen innerhalb des Aktions Centers zu gruppieren. |


### <a name="toastscenario"></a>Ein-/ausgeschaltet
Gibt an, welches Szenario der Toast darstellt.

| Wert | Bedeutung |
|---|---|
| **Standard** | Das normale Popup Verhalten. |
| **Reminder** | Eine Erinnerungs Benachrichtigung. Diese wird vorab erweitert angezeigt und bleibt auf dem Bildschirm des Benutzers, bis Sie verworfen wird. |
| **Alarm** | Eine Warn Benachrichtigung. Diese wird vorab erweitert angezeigt und bleibt auf dem Bildschirm des Benutzers, bis Sie verworfen wird. Audiodaten werden standardmäßig Schleifen und verwenden die Alarm Audiodatei. |
| **IncomingCall** | Eine eingehende Benachrichtigung. Dies wird in einem speziellen Callcenter als vorerweitert angezeigt und bleibt auf dem Bildschirm des Benutzers, bis er verworfen wird. Audiodaten werden standardmäßig Schleifen und verwenden Rington-Audiodaten. |


## <a name="toastvisual"></a>-Visualisierung
Der visuelle Teil von Popups enthält die Bindungen, die Text, Bilder, adaptiver Inhalt und mehr enthalten.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Bindinggeneric** | ["Deastbindinggeneric"](#toastbindinggeneric) | true | Die generische Popup Bindung, die auf allen Geräten gerendert werden kann. Diese Bindung ist erforderlich und darf nicht NULL sein. |
| **BaseUri** | Uri | false | Eine Standard-Basis-URL, die mit relativen URLs in Image Quellen Attributen kombiniert wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Popup Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |
| **Sprache**| Zeichenfolge | false | Das Ziel Gebiets Schema der visuellen Nutzlast, wenn lokalisierte Ressourcen verwendet werden, die als bcp-47-sprach Tags, z. b. "en-US" oder "fr-FR", angegeben werden. Dieses Gebiets Schema wird von einem beliebigen Gebiets Schema überschrieben, das in Bindung oder Text angegeben ist. Wenn keine Angabe erfolgt, wird stattdessen das Gebiets Schema des Systems verwendet. |


## <a name="toastbindinggeneric"></a>"Deastbindinggeneric"
Die generische Bindung ist die Standard Bindung für Toasts und ist der Ort, an dem Sie den Text, die Bilder, den adaptiven Inhalt usw. angeben.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Children** | IList<[ideastbindinggenericchild](#itoastbindinggenericchild)> | false | Der Inhalt des Popup Texts, der Text, Bilder und Gruppen enthalten kann (in Anniversary Update hinzugefügt). Text Elemente müssen vor allen anderen Elementen stehen, und es werden nur drei Textelemente unterstützt. Wenn ein Textelement nach einem anderen Element platziert wird, wird es entweder nach oben oder nach dem Löschvorgang abgelegt. Und schließlich werden bestimmte Texteigenschaften wie hintstyle nicht für die Textelemente der Stamm-untergeordneten Elemente unterstützt und können nur in einer adaptivesubgroup verwendet werden. Wenn Sie adaptivegroup auf Geräten ohne das Anniversary Update verwenden, wird der Gruppen Inhalt einfach gelöscht. |
| **Applogooverride** | ["Ein"-Logo](#toastgenericapplogo) | false | Ein optionales Logo, um das App-Logo zu überschreiben. |
| **Heroimage** | [Verzeichnis Image](#toastgenericheroimage) | false | Ein optionales "Hero"-Image, das auf dem Toast und innerhalb des Aktions Centers angezeigt wird. |
| **Nung** | ["" "" "" "".](#toastgenericattributiontext) | false | Optionaler Zuweisungs Text, der am unteren Rand der Popup Benachrichtigung angezeigt wird. |
| **BaseUri** | Uri | false | Eine Standard-Basis-URL, die mit relativen URLs in Image Quellen Attributen kombiniert wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Popup Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |
| **Sprache**| Zeichenfolge | false | Das Ziel Gebiets Schema der visuellen Nutzlast, wenn lokalisierte Ressourcen verwendet werden, die als bcp-47-sprach Tags, z. b. "en-US" oder "fr-FR", angegeben werden. Dieses Gebiets Schema wird von einem beliebigen Gebiets Schema überschrieben, das in Bindung oder Text angegeben ist. Wenn keine Angabe erfolgt, wird stattdessen das Gebiets Schema des Systems verwendet. |


## <a name="itoastbindinggenericchild"></a>Ideastbindinggenericchild
Markerschnittstelle für untergeordnete Popup Elemente, die Text, Bilder, Gruppen und mehr enthalten.

| Implementierungen |
| --- |
| [Adaptivetext](#adaptivetext) |
| [Adaptiveimage](#adaptiveimage) |
| [Adaptivegroup](#adaptivegroup) |
| [Adaptiveprogressbar](#adaptiveprogressbar) |


## <a name="adaptivetext"></a>Adaptivetext
Ein adaptives Textelement. Wenn Sie auf der obersten Ebene "" "" "" "" "" "" "" "" "" "". Wenn dies jedoch als untergeordnetes Element einer Gruppe/Untergruppe platziert wird, wird die voll Textformatierung unterstützt.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Text** | String oder [bindablestring](#bindablestring) | false | Der anzuzeigende Text. Die Unterstützung der Datenbindung wurde in Creators Update hinzugefügt, funktioniert aber nur für Textelemente der obersten Ebene. |
| **Hintstyle** | [Adaptivetextstyle](#adaptivetextstyle) | false | Der Stil steuert die Schriftgröße, die Gewichtung und die Deckkraft des Texts. Funktioniert nur für Textelemente innerhalb einer Gruppe/Untergruppe. |
| **Hintwrap** | bool? | false | Legen Sie diese Einstellung auf "true" fest, um die Text um Textelemente der obersten Ebene ignorieren diese Eigenschaft und werden immer umschlossen (Sie können "hintmaxlines = 1" verwenden, um das umschließen für Textelemente der obersten Ebene zu deaktivieren). Text Elemente in Gruppen/Untergruppen werden beim umwickeln standardmäßig auf false eingestellt. |
| **Hintmaxlines** | int? | false | Die maximale Anzahl von Zeilen, die das Textelement anzeigen darf. |
| **Hintminlines** | int? | false | Die Mindestanzahl von Zeilen, die das Textelement anzeigen muss. Funktioniert nur für Textelemente innerhalb einer Gruppe/Untergruppe. |
| **Hintalign** | [Adaptivetextalign](#adaptivetextalign) | false | Die horizontale Ausrichtung des Texts. Funktioniert nur für Textelemente innerhalb einer Gruppe/Untergruppe. |
| **Sprache** | Zeichenfolge | false | Das Ziel Gebiets Schema der XML-Nutzlast, das als bcp-47-sprach Tags, z. b. "en-US" oder "fr-FR", angegeben ist. Das hier angegebene Gebiets Schema überschreibt ein anderes angegebenes Gebiets Schema, z. b. das in der Bindung oder in Visual. Wenn dieser Wert eine Literalzeichenfolge ist, wird dieses Attribut standardmäßig auf die Benutzeroberflächen Sprache des Benutzers eingestellt. Wenn dieser Wert ein Zeichen folgen Verweis ist, wird dieses Attribut standardmäßig auf das von Windows-Runtime beim Auflösen der Zeichenfolge gewählte Gebiets Schema eingestellt. |


### <a name="bindablestring"></a>Bindablestring
Ein Bindungs Wert für Zeichen folgen.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **BindingName** | Zeichenfolge | true | Ruft den Namen ab, der dem Bindungs Datenwert zugeordnet ist, oder legt diesen fest. |


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
| **Header** | H1 Schrift Grad. |
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
| **Hintcrop** | [Adaptiveimagecrop](#adaptiveimagecrop) | false | Neu in Anniversary Update: Steuern Sie das gewünschte Zuschneiden des Bilds. |
| **Hintremovemargin** | bool? | false | Standardmäßig haben Bilder innerhalb von Gruppen/Untergruppen einen 8px-Rand um diese. Sie können diesen Rand entfernen, indem Sie diese Eigenschaft auf "true" festlegen. |
| **Hintalign** | [Adaptiveimagealign](#adaptiveimagealign) | false | Die horizontale Ausrichtung des Bilds. Funktioniert nur für Bilder innerhalb einer Gruppe/Untergruppe. |
| **AlternateText** | Zeichenfolge | false | Alternativer Text, der das Bild beschreibt, das zu Barrierefreiheits Zwecken verwendet wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Popup Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |


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
Neu in Anniversary Update: Gruppen semantisch identifizieren, dass der Inhalt in der Gruppe als Ganzes angezeigt werden muss, oder nicht angezeigt werden, wenn er nicht passt. Gruppen ermöglichen auch das Erstellen mehrerer Spalten.

| Eigenschaft | type | Erforderlich |Beschreibung |
|---|---|---|---|
| **Children** | IList<[adaptivesubgroup](#adaptivesubgroup)> | false | Untergruppen werden als vertikale Spalten angezeigt. Sie müssen Untergruppen verwenden, um Inhalte innerhalb einer adaptivegroup bereitzustellen. |


## <a name="adaptivesubgroup"></a>Adaptivesubgroup
Neu in Anniversary Update: Untergruppen sind vertikale Spalten, die Text und Bilder enthalten können.

| Eigenschaft | type | Erforderlich |Beschreibung |
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


## <a name="adaptiveprogressbar"></a>Adaptiveprogressbar
Neu in Creators Update: eine Statusanzeige. Wird nur für-Auffassungen auf dem Desktop, Build 15063 oder neuer unterstützt.

| Eigenschaft | type | Erforderlich | BESCHREIBUNG |
|---|---|---|---|
| **Titel** | String oder [bindablestring](#bindablestring) | false | Ruft eine optionale Zeichenfolge ab oder legt Sie fest. Unterstützt die Datenbindung. |
| **Wert** | Double oder [adaptiveprogressbarvalue](#adaptiveprogressbarvalue) oder [bindableprogressbarvalue](#bindableprogressbarvalue) | false | Ruft den Wert der Statusanzeige ab oder legt ihn fest. Unterstützt die Datenbindung. Der Standardwert ist 0. |
| **Valuestringoverride** | String oder [bindablestring](#bindablestring) | false | Ruft eine optionale Zeichenfolge ab, die anstelle der standardmäßigen Prozent Zeichenfolge angezeigt wird, oder legt diese fest Wenn dies nicht angegeben wird, wird etwa "70%" angezeigt. |
| **Status** | String oder [bindablestring](#bindablestring) | true | Ruft eine Status Zeichenfolge (erforderlich) ab, die unterhalb der Statusleiste auf der linken Seite angezeigt wird, oder legt Sie fest. Diese Zeichenfolge sollte den Status des Vorgangs widerspiegeln, wie z. b. "herunterladen..." oder "wird installiert..." |


### <a name="adaptiveprogressbarvalue"></a>Adaptiveprogressbarvalue
Eine-Klasse, die den Wert der Statusanzeige darstellt.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Wert** | double | false | Ruft den Wert (0,0-1,0) ab, der den Prozentsatz darstellt, oder legt diesen fest. |
| **IsIndeterminate** | bool | false | Ruft einen Wert ab, der angibt, ob die Statusanzeige unbestimmt ist, oder legt diesen fest Wenn dieser Wert true ist, wird der **Wert** ignoriert. |


### <a name="bindableprogressbarvalue"></a>Bindableprogressbarvalue
Ein bindbarer Status leisten Wert.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **BindingName** | Zeichenfolge | true | Ruft den Namen ab, der dem Bindungs Datenwert zugeordnet ist, oder legt diesen fest. |


## <a name="toastgenericapplogo"></a>"Ein"-Logo
Ein Logo, das anstelle des App-Logos angezeigt werden soll.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Quelle** | Zeichenfolge | true | Die URL des Bilds. MS-AppX, MS-APPDATA und http werden unterstützt. Http-Bilder müssen mindestens 200 KB groß sein. |
| **Hintcrop** | ["Anastgenericapplogocrop"](#toastgenericapplogocrop) | false | Geben Sie an, wie das Bild abgeschnitten werden soll. |
| **AlternateText** | Zeichenfolge | false | Alternativer Text, der das Bild beschreibt, das zu Barrierefreiheits Zwecken verwendet wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Popup Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |


### <a name="toastgenericapplogocrop"></a>"Anastgenericapplogocrop"
Steuert das Zuschneiden des App-Logo Bilds.

| Wert | Bedeutung |
|---|---|
| **Standard** | Beim Zuschneiden wird das Standardverhalten des Renderers verwendet. |
| **None** | Das Bild wird nicht abgeschnitten, das Quadrat wird angezeigt. |
| **Circle** | Das Bild wird auf einen Kreis zugeschnitten. |


## <a name="toastgenericheroimage"></a>Verzeichnis Image
Ein vorschauetes "Hero"-Bild, das im Toast und innerhalb des Aktions Centers angezeigt wird.

| Eigenschaft | type | Erforderlich |BESCHREIBUNG |
|---|---|---|---|
| **Quelle** | Zeichenfolge | true | Die URL des Bilds. MS-AppX, MS-APPDATA und http werden unterstützt. Http-Bilder müssen mindestens 200 KB groß sein. |
| **AlternateText** | Zeichenfolge | false | Alternativer Text, der das Bild beschreibt, das zu Barrierefreiheits Zwecken verwendet wird. |
| **Addimagequery** | bool? | false | Legen Sie diese Einstellung auf "true" fest, damit Windows eine Abfrage Zeichenfolge an die in der Popup Benachrichtigung angegebene Bild-URL anfügen kann. Verwenden Sie dieses Attribut, wenn der Server Bilder hostet und Abfrage Zeichenfolgen verarbeiten kann. dazu wird entweder eine Bild Variante basierend auf den Abfrage Zeichenfolgen abgerufen oder die Abfrage Zeichenfolge ignoriert und das Image zurückgegeben, wie es ohne die Abfrage Zeichenfolge angegeben ist Diese Abfrage Zeichenfolge gibt Skalierung, Kontrasteinstellung und Sprache an. Beispielsweise wird der Wert "www.Website.com/images/hello.png", der in der Benachrichtigung angegeben ist, zu "www.Website.com/images/hello.png? MS-Scale = 100&MS-Contrast = Standard&ms-lang = en-US". |


## <a name="toastgenericattributiontext"></a>"" "" "" "".
Der im unteren Teil der Popup Benachrichtigung angezeigte Zuweisungs Text.

| Eigenschaft | type | Erforderlich | BESCHREIBUNG |
|---|---|---|---|
| **Text** | Zeichenfolge | true | Der anzuzeigende Text. |
| **Sprache** | Zeichenfolge | false | Das Ziel Gebiets Schema der visuellen Nutzlast, wenn lokalisierte Ressourcen verwendet werden, die als bcp-47-sprach Tags, z. b. "en-US" oder "fr-FR", angegeben werden. Wenn keine Angabe erfolgt, wird stattdessen das Gebiets Schema des Systems verwendet. |


## <a name="itoastactions"></a>Ideastactions
Markerschnittstelle für Popup Aktionen/Eingaben.

| Implementierungen |
| --- |
| [Ein-und ausgeschaltet](#toastactionscustom) |
| ["", "", "", ""](#toastactionssnoozeanddismiss) |


## <a name="toastactionscustom"></a>Ein-und ausgeschaltet
*Implementiert [ideastactions](#itoastactions)*

Erstellen Sie eigene benutzerdefinierte Aktionen und Eingaben mithilfe von Steuerelementen wie Schaltflächen, Textfeldern und Auswahl Eingaben.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Eingaben** | IList<[ium-Eingabe](#itoastinput)> | false | Eingaben wie Textfelder und Auswahl Eingaben. Es sind nur bis zu 5 Eingaben zulässig. |
| **Schaltflächen** | IList<[ionastbutton](#itoastbutton)> | false | Schaltflächen werden nach allen Eingaben angezeigt (oder neben einer Eingabe, wenn die Schaltfläche als Quick Reply-Schaltfläche verwendet wird). Nur bis zu 5 Schaltflächen sind zulässig (oder weniger, wenn Sie auch über Kontextmenü Elemente verfügen). |
| **Contextmenuitems** | IList<"$ [astcontextmenuitem](#toastcontextmenuitem) "> | false | Neu in Anniversary Update: benutzerdefinierte Kontextmenü Elemente, die zusätzliche Aktionen angeben, wenn der Benutzer mit der rechten Maustaste auf die Benachrichtigung klickt. Es können nur bis zu 5 Schaltflächen und Kontextmenü Elemente *kombiniert*werden. |


## <a name="itoastinput"></a>Ibetastinput
Markerschnittstelle für Popup Eingaben.

| Implementierungen |
| --- |
| [-Textfeld](#toasttextbox) |
| ["-Selectionbox"](#toastselectionbox) |


## <a name="toasttextbox"></a>-Textfeld
*Implementiert [ionastinput](#itoastinput)*

Ein Textfeld-Steuerelement, in das der Benutzer Text eingeben kann.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Id** | Zeichenfolge | true | Die ID ist erforderlich und wird verwendet, um den vom Benutzer eingegebenen Text einem Schlüssel-Wert-Paar von ID/Wert zuzuordnen, das Ihre APP später verwendet. |
| **Titel** | Zeichenfolge | false | Der Titeltext, der über dem Textfeld angezeigt werden soll. |
| **Placeholdercontent** | Zeichenfolge | false | Platzhalter Text, der im Textfeld angezeigt werden soll, wenn der Benutzer noch keinen Text eingegeben hat. |
| **Defaultinput** | Zeichenfolge | false | Der Anfangstext, der in das Textfeld platziert werden soll. Belassen Sie für ein leeres Textfeld NULL. |


## <a name="toastselectionbox"></a>"-Selectionbox"
*Implementiert [ionastinput](#itoastinput)*

Ein Auswahlfeld-Steuerelement, mit dem Benutzer aus einer Dropdown Liste von Optionen auswählen können.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Id** | Zeichenfolge | true | Die ID ist erforderlich. Wenn der Benutzer dieses Element ausgewählt hat, wird diese ID an den Code der APP zurückgegeben und repräsentiert, welche Auswahl Sie ausgewählt haben. |
| **Inhalt** | Zeichenfolge | true | Der Inhalt ist erforderlich, und ist eine Zeichenfolge, die auf dem Auswahl Element angezeigt wird. |


### <a name="toastselectionboxitem"></a>"Element"
Ein Auswahlfeld Element (ein Element, das der Benutzer aus der Dropdown Liste auswählen kann).

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Id** | Zeichenfolge | true | Die ID ist erforderlich und wird verwendet, um den vom Benutzer eingegebenen Text einem Schlüssel-Wert-Paar von ID/Wert zuzuordnen, das Ihre APP später verwendet. |
| **Titel** | Zeichenfolge | false | Der Titeltext, der über dem Auswahlfeld angezeigt werden soll. |
| **Defaultselectionboxitemid** | Zeichenfolge | false | Dadurch wird gesteuert, welches Element standardmäßig ausgewählt ist, und verweist auf die ID-Eigenschaft von [toastselectionboxitem](#toastselectionboxitem). Wenn Sie diese Option nicht angeben, ist die Standardauswahl leer (Benutzer sieht nichts). |
| **Elemente** | IList<"$ [astselectionboxitem](#toastselectionboxitem) "> | false | Die Auswahlelemente, die der Benutzer in diesem selectionbox-Element auswählen kann. Es können nur fünf Elemente hinzugefügt werden. |


## <a name="itoastbutton"></a>Iper astbutton
Markerschnittstelle für Popup Schaltflächen.

| Implementierungen |
| --- |
| [Umschalten](#toastbutton) |
| ["Onastbuttonsnooze"](#toastbuttonsnooze) |
| [Ein-und ausblenden](#toastbuttondismiss) |


## <a name="toastbutton"></a>Umschalten
*Implementiert [Iper astbutton](#itoastbutton) .*

Eine Schaltfläche, auf die der Benutzer klicken kann.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Inhalt** | Zeichenfolge | true | Erforderlich. Der Text, der auf der Schaltfläche angezeigt werden soll. |
| **Argumente** | Zeichenfolge | true | Erforderlich. App-definierte Argument Zeichenfolge, die von der APP später empfangen wird, wenn der Benutzer auf diese Schaltfläche klickt. |
| **ActivationType** | ["Deastactivationtype"](#toastactivationtype) | false | Steuert, welche Art von Aktivierung diese Schaltfläche verwendet, wenn darauf geklickt wird. Der Standardwert ist Vordergrund. |
| **Activationoptions** | ["Deastactivationoptions"](#toastactivationoptions) | false | Neu in Creators Update: Ruft zusätzliche Optionen im Zusammenhang mit der Aktivierung der Popup Schaltfläche ab oder legt Sie fest. |


### <a name="toastactivationtype"></a>"Deastactivationtype"
Bestimmt den Aktivierungstyp, der verwendet wird, wenn der Benutzer mit einer bestimmten Aktion interagiert.

| Wert | Bedeutung |
|---|---|
| **Vordergrund** | Standardwert. Ihre Vordergrund-APP wird gestartet. |
| **Hintergrund** | Die entsprechende Hintergrundaufgabe (vorausgesetzt, Sie legen alles fest) wird ausgelöst, und Sie können Code im Hintergrund ausführen (z. b. das Senden der Quick Reply-Nachricht des Benutzers), ohne den Benutzer zu unterbrechen. |
| **Protokoll** | Starten Sie mithilfe der Protokoll Aktivierung eine andere app. |


### <a name="toastactivationoptions"></a>"Deastactivationoptions"
Neu in Creators Update: zusätzliche Optionen in Bezug auf die Aktivierung.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Afteractivationbehavior** | ["Deastafteractivationbehavior"](#toastafteractivationbehavior) | false | Neu bei Fall Creators Update: Ruft das Verhalten ab, das der Toast verwenden soll, wenn der Benutzer diese Aktion aufruft, oder legt dieses fest. Dies funktioniert nur auf dem Desktop, [für "](#toastbutton) ", "", "", " [" und "](#toastcontextmenuitem)" "". |
| **Protocolactivationtargetapplicationpfn** | Zeichenfolge | false | Wenn Sie *toastactivationtype. Protocol*verwenden, können Sie optional den Ziel-PFN angeben, sodass unabhängig davon, ob mehrere Apps für die Behandlung desselben Protokoll-URI registriert sind, die gewünschte App immer gestartet wird. |


### <a name="toastafteractivationbehavior"></a>"Deastafteractivationbehavior"
Gibt das Verhalten an, das der Toast verwenden soll, wenn der Benutzeraktionen für den Toast durchführt.

| Wert | Bedeutung |
|---|---|
| **Standard** | Standardverhalten. Der Toast wird verworfen, wenn der Benutzeraktionen für den Toast durchführt. |
| **"Pendingupdate"** | Nachdem der Benutzer auf eine Schaltfläche im Toast geklickt hat, bleibt die Benachrichtigung im visuellen Zustand "ausstehender Update" vorhanden. Sie sollten den Popup Vorgang direkt von einer Hintergrundaufgabe aus aktualisieren, damit der visuelle Zustand "ausstehender Update" für den Benutzer nicht zu lange angezeigt wird. |


## <a name="toastbuttonsnooze"></a>"Onastbuttonsnooze"
*Implementiert [Iper astbutton](#itoastbutton) .*

Eine vom System behandelte Schaltfläche zum Zurücksetzen der Benachrichtigung, die automatisch das Zurücksetzen der Benachrichtigung behandelt.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Customcontent** | Zeichenfolge | false | Optionaler benutzerdefinierter Text, der auf der Schaltfläche angezeigt wird, der den standardmäßig lokalisierten Text "zurückstellen" überschreibt |


## <a name="toastbuttondismiss"></a>Ein-und ausblenden
*Implementiert [Iper astbutton](#itoastbutton) .*

Eine vom System behandelte Schaltfläche zum verwerfen, die die Benachrichtigung beim Klicken schließt.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Customcontent** | Zeichenfolge | false | Optionaler benutzerdefinierter Text, der auf der Schaltfläche angezeigt wird und den standardmäßigen "verwerfen"-Text überschreibt. |


## <a name="toastactionssnoozeanddismiss"></a>"", "", "", ""
* Implementieren von [ideastactions](#itoastactions)

Erstellt automatisch ein Auswahlfeld für Intervalle von wiederholtem Intervall und zum Zurückstellen/Verwerfen von Schaltflächen, die alle automatisch lokalisiert werden, und das Zurücksetzen der Logik wird automatisch vom System behandelt.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Contextmenuitems** | IList<"$ [astcontextmenuitem](#toastcontextmenuitem) "> | false | Neu in Anniversary Update: benutzerdefinierte Kontextmenü Elemente, die zusätzliche Aktionen angeben, wenn der Benutzer mit der rechten Maustaste auf die Benachrichtigung klickt. Es können nur bis zu 5 Elemente vorhanden sein. |


## <a name="toastcontextmenuitem"></a>"Objekt"
Ein Kontextmenü Element Eintrag.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Inhalt** | Zeichenfolge | true | Erforderlich. Der anzuzeigende Text. |
| **Argumente** | Zeichenfolge | true | Erforderlich. Eine APP-definierte Argument Zeichenfolge, die die APP später abrufen kann, sobald Sie aktiviert wird, wenn der Benutzer auf das Menü Element klickt. |
| **ActivationType** | ["Deastactivationtype"](#toastactivationtype) | false | Steuert, welche Art von Aktivierung dieses Menü Element verwendet, wenn darauf geklickt wird. Der Standardwert ist Vordergrund. |
| **Activationoptions** | ["Deastactivationoptions"](#toastactivationoptions) | false | Neu in Creators Update: zusätzliche Optionen im Zusammenhang mit der Aktivierung des Popup Kontextmenü Elements. |


## <a name="toastaudio"></a>Ein-und ausgeschaltet
Geben Sie das Audioformat an, das beim Empfang der Popup Benachrichtigung abgespielt werden soll.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Src** | uri | false | Die Mediendatei, die anstelle des Standard Sounds abgespielt werden soll. Nur MS-AppX und MS-APPDATA werden unterstützt. |
| **ESE** | boolean | false | Auf "true" festgelegt, wenn der Sound wiederholt werden soll, solange der Popup angezeigt wird. false, um nur einmal zu spielen (Standard). |
| **Me** | boolean | false | True zum stumm schalten des Sounds. false, um das Abspielen des Popup Benachrichtigungs Sounds zuzulassen (Standard). |


## <a name="toastheader"></a>Verzeichnis Leser
Neu in Creators Update: eine benutzerdefinierte Kopfzeile, die mehrere Benachrichtigungen innerhalb des Aktions Centers gruppiert.

| Eigenschaft | type | Erforderlich | Beschreibung |
|---|---|---|---|
| **Id** | Zeichenfolge | true | Ein vom Entwickler erstellter Bezeichner, der diesen Header eindeutig identifiziert. Wenn zwei Benachrichtigungen dieselbe Header-ID aufweisen, werden Sie im Aktions Center unterhalb desselben Headers angezeigt. |
| **Titel** | Zeichenfolge | true | Ein Titel für den Header. |
| **Argumente**| Zeichenfolge | true | Ruft eine von Entwicklern definierte Argument Zeichenfolge ab oder legt diese fest, die an die APP zurückgegeben wird, wenn der Benutzer auf diesen Header klickt. Darf nicht NULL sein. |
| **ActivationType** | ["Deastactivationtype"](#toastactivationtype) | false | Ruft den Aktivierungstyp ab, den dieser Header beim Klicken verwendet, oder legt diesen fest. Der Standardwert ist Vordergrund. Beachten Sie, dass nur Vordergrund und Protokoll unterstützt werden. |
| **Activationoptions** | ["Deastactivationoptions"](#toastactivationoptions) | false | Ruft zusätzliche Optionen im Zusammenhang mit der Aktivierung des Popup Headers ab oder legt diese fest. |


## <a name="related-topics"></a>Verwandte Themen

* [Schnellstart: Senden einer lokalen Popupbenachrichtigung und Behandeln der Aktivierung](/archive/blogs/tiles_and_toasts/quickstart-sending-a-local-toast-notification-and-handling-activations-from-it-windows-10)
* [Benachrichtigungsbibliothek auf GitHub](https://github.com/windows-toolkit/WindowsCommunityToolkit/tree/dev/Notifications)
