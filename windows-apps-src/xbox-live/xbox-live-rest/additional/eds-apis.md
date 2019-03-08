---
title: Auxiliary EDS-APIs
assetID: 5729ab80-e88d-0190-fb61-bd0cc4f134f6
permalink: en-us/docs/xboxlive/rest/eds-apis.html
description: " Auxiliary EDS-APIs"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 3f2f729359f52b09879e7227ede71e238fe63801
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603235"
---
# <a name="auxiliary-eds-apis"></a>Auxiliary EDS-APIs

Es gibt mehrere Unterhaltung Discovery Services (EDS) APIs, die nicht direkt enthalten Informationen zum Inhalt, aber geben Sie allgemeine Informationen zum Verwenden des Diensts oder allgemeine Benutzeroberfläche Laufwerksmodellen helfen.

<a id="ID4EQ"></a>


## <a name="auxiliary-apis"></a>Zusätzliche APIs

| API| URI| Beschreibung|
| --- | --- | --- |
| API-Parameterwerte| /{locale}/metadata| Enumeration der möglichen Werte von Parametern, die in Aufrufe an den Dienst verwendet werden kann|
| Kombiniert die Einstufung des Inhalts-Generators| /{locale}/contentRating| Erstellt einen Wert, der verwendet werden kann in anderen APIs zum Filtern von potenziell unerwünscht eingestuft oder explizite Inhalt an. Weitere Informationen finden Sie unten.|
| Kombinierte Feld über den Namensgenerator| /{locale}/fields| Erstellt einen Wert, der verwendet werden kann in die Details APIs, um zu steuern, welche Felder zurückgegeben werden. Weitere Informationen finden Sie unten.|

<a id="ID4EBC"></a>


### <a name="api-parameter-values"></a>API-Parameterwerte

Diese API beschreibt die Parameter, die mit dem Dienst verwendet werden können. Die zurückgegebene Informationen ist von der Benutzeroberfläche des Clients verwendet werden, da lokalisierter Text jedes Parameters begleitet.

Keines der folgenden APIs akzeptieren Parameter.

| API| URI| Beschreibung|
| --- | --- | --- | --- | --- | --- |
| Typen| / {Gebietsschema} / Metadata/MediaGroups| Die vollständige Liste der Mediengruppen|
| Medien-Elementtypen pro Media.| /{locale}/metadata/mediaGroups/{mediaItemTypeGroup}/mediaItemTypes| Die Liste von Elementtypen ab, der in der Mediengruppe angegebenen enthalten ist.|
| Alle Medien-Elementtypen| /{locale}/metadata/mediaItemTypes| Die vollständige Liste von Medientypen-Element|
| Felder pro Medientyp-Element| /{locale}/metadata/mediaItemTypes/{mediaItemType}/fields| Die Liste der Felder in ein einziger Medientyp des Elements|
| Abfrage Raffinierer| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners| Die Liste der Abfrage Raffinierer unterstützt für den angegebenen Medientyp des Elements|
| Alle Abfragen Farbänderungsfeld Werte| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}| Die Werte für die angegebene Abfrage Farbänderungsfeld für das angegebene Medium Elementtyp.|
| Fragen Sie alle untergeordneten Farbänderungsfeld-Werte| /{locale}/metadata/mediaItemTypes/{mediaItemType}/queryRefiners/{queryRefiner}/subQueryRefinerValues| Die Liste der untergeordneten Werte für eine bestimmte Abfrage Farbänderungsfeld-Wert (z. B. "Subgenres in einem bestimmten Genre"). Der Wert für das Farbänderungsfeld wird in übergeben, als ein Abfragezeichenfolgen-Parameter mit dem Namen "QueryRefinerValue", die durchgeführt wird, um die Abfrage Farbänderungsfeld Werte mit Zeichen, die im URI-Stämme ist unzulässig, übergeben werden können.|
| Sortierungen| /{locale}/metadata/mediaItemTypes/{mediaItemType}/sortOrders| Die Liste der Sortierreihenfolgen für den angegebenen Medientyp des Elements|

<a id="ID4EEF"></a>


### <a name="combined-content-rating-generator"></a>Kombiniert die Einstufung des Inhalts-Generators

Durchsetzung von zugangssteuerungen über die Inhalte, die untergeordneten Elemente zulässig sind, finden Sie unter ist eine komplizierte Aufgabe. Nicht nur besitzt jedes Element Medientyp eigene Bewertungssystem, sondern diese Bewertungssystemen können von Land zu Land unterscheiden. Dies bedeutet, dass es mehrere verschiedene Teile der Daten, die müssen angegeben werden, um alle Elemente ordnungsgemäß zu filtern.

Anstatt alle Parameter in allen API-aufrufen, generiert dieser API einen Wert in CombinedContentRating-Parameter in andere APIs übergeben und die gleiche Informationen trotzdem kommunizieren. Dies soll erleichtern die APIs verwenden und verwalten, da mehrere Parameter übergeben, die in diese API für die anderen APIs in einen einzelnen, wieder verwendbaren Wert reduziert werden.

Obwohl die genauen Werte, die von dieser API zurückgegebenen schließlich geändert werden können, sollten sie nur sehr selten ändern (z. B. zwischen Versionen von EDS) und somit für längere Zeit zwischengespeichert werden kann. Jede API, die Annahme, dass CombinedContentRating Parameter bieten einer sinnvolle Fehlermeldung, wenn der übergebene Wert ungültig ist, wird die ist eine Angabe über das den Aufrufer nur muss zum Aufrufen dieser API erneut aus, um eine aktualisierte Wert zu erhalten. Wenn eine API einen CombinedContentRating-Parameter akzeptiert, aber ist nicht angegeben, keine Filterung des Inhalts erfolgt basierend auf Jugendschutz

> [!NOTE]
> Dies bedeutet nicht, dass nur "sicher" Inhalt zurückgegeben werden – dies bedeutet, dass der gesamte Inhalt einschließlich potenziell anstößiger Inhalt zurückgegeben wird,).



<a id="ID4EWF"></a>


### <a name="combined-field-name"></a>Kombinierte Feldname

Den EDS-APIs, die standardmäßig einen sehr kleinen minimalen Satz von Feldern für jedes Element zurück:

   * Medientyp-Element
   * Medien-Gruppe
   * Id
   * Name

Weitere Informationen zu erhalten, akzeptieren die APIs einen "Felder"-Parameter, der angibt, welche zusätzlichen Datenelemente zurückgegeben werden sollen. Da viele mögliche Felder vorhanden sind, würde die Anforderung ihren Namen angeben, vollen Zugriff auf jeden API-Aufruf erheblich aufzublähen. Stattdessen können die Namen in diese API übergeben werden, die einen viel niedrigeren Wert generiert wird, der in die andere APIs übergeben werden kann.

Für jede API, die dieser Parameter akzeptiert, muss der angegebene Wert die Obermenge aller Felder in der alle angegebenen Medien-Elementtypen. Es ist nicht möglich, verschiedene Sätze von Feldern für verschiedene Medientypen für Element angeben. Allerdings, wenn ein Feld für eine Medien-Elementtyp jedoch nicht eine andere und es gilt wird nur angezeigt, auf dem Medium work Item Types, in dem Daten vorhanden sind (z. B. wenn im Aufruf der API kombiniert Feld Name "AvatarBodyType" enthalten ist, enthält nur AvatarItems das Feld).

Die von dieser API zurückgegebenen Werte werden in der Tat hohem Maße zwischenspeicherbar –, sie sollten nicht mit Ausnahme von ändern, zwischen den Bereitstellungen von EDS. Es wird empfohlen, dass die Zwischenspeicherung gewünscht ist, der Cache nicht mehr als die Sitzung des Benutzers zuletzt.

Zusätzlich zum Akzeptieren von der tatsächlichen Feldnamen, akzeptiert die API "all" als gültiger Wert. Dadurch wird einen Wert generiert, der jedes Feld, ist es möglich enthält, anzugeben. Der Wert "all" ist wahrscheinlich nur für Entwicklung, Debuggen und Testzwecke verwendet werden.

<a id="ID4ERG"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ETG"></a>


##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)


<a id="ID4E6G"></a>


##### <a name="further-information"></a>Weitere Informationen

[Marketplace-URIs](../uri/marketplace/atoc-reference-marketplace.md)
