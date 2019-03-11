---
title: EDS-Parameter
assetID: 9475b427-53bc-697b-6d24-1787320260b7
permalink: en-us/docs/xboxlive/rest/edsparameters.html
description: " EDS-Parameter"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 5412bcebc73dfd25d81b2073527e64e81de1bba4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655415"
---
# <a name="eds-parameters"></a>EDS-Parameter

<a id="ID4EO"></a>


## <a name="query-string-parameters"></a>Abfragezeichenfolgen-Parameter

Diese Abfrageparameter sind nicht unbedingt von allen akzeptiert [Unterhaltung Discovery Services (EDS) APIs](../uri/marketplace/atoc-reference-marketplace.md), aber alle von mehr als eine API akzeptiert werden.

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| combinedContentRating| string| Optional. Finden Sie unter [abrufen (/media/ {MarketplaceId} / ContentRating)](../uri/marketplace/uri-medialocalecontentratingget.md).|
| continuationToken| string| Optional. Das Fortsetzungstoken ist ein nicht transparentes Blob, das Informationen enthält, der Dienst für die Paginierung in bestimmten Szenarien muss. Wenn der Wert ausgelassen wird, wird die erste Seite mit Ergebnissen zurückgegeben (wobei "Seitengröße" durch den Parameter "MaxItems" bestimmt wird), sowie ein Fortsetzungstoken, die verwendet werden können, um der zweiten Seite der Ergebnisse zu erhalten. Die zweite Seite würde das Fortsetzungstoken für die dritte Seite mit Ergebnissen, usw. enthalten.|
| Gewünschter| string| Optional. Finden Sie in der kombinierten Feld Name-API.|
| desiredMediaItemTypes| Array von Zeichenfolgen| Optional. Dieser Parameter bestimmt die Typen der Elemente in der Antwort zurückgegeben werden.|
| Domäne| string| Optional. Der Parameter für die Domäne bestimmt den Spiele- und app-Marketplace-Kontext in der Client für aufruft. Standardmäßig ist die Domäne "Modern", der angibt, dass der Client nur für Xbox One Inhalt anfordern kann. Wenn der Client die Domäne zu Xbox 360 Inhalte zu wechseln möchte, müssen sie Domäne als der "Xbox360" angeben. Domänenübergreifende werden Ergebnisse derzeit nicht unterstützt. Mögliche Werte: <ul><li>Xbox360</li><li>Modern</li></ul> Der Wert "der Xbox360" wird nur für SingleMediaGroupSearch unterstützt. Durchsuchen und Details werden unterstützt. CrossMediaGroupSearch wird nicht unterstützt und gibt einen 400-Fehler zurück.|
| Felder| string| Optional. Finden Sie unter [abrufen (/media/ {MarketplaceId} / Felder)](../uri/marketplace/uri-medialocalefieldsget.md).|
| firstPartyOnly| Boolescher Wert| Optionalen Filter-Parameter. Bestimmt, ob nur Erstanbieter-Inhalt zurückgegeben wird oder angibt, ob beide erste – und Drittanbieter-Inhalt aus der Abfrage zurückgegeben werden. |
| freeOnly| Boolescher Wert| Optionalen Filter-Parameter. Schränkt die Ergebnisse, um nur die Inhalte freizugeben.|
| GroupBy| TK| Der GroupBy-Parameter wird verwendet, können Sie das Resultset in Gruppen, anstatt ein einzelnes Resultset zu kategorisieren. Die Angabe dieses Parameters, ändert das Resultset auf mehrere Listen von Elementen zurückzugeben, auf, in dem die Anzahl der Elemente in jedem Bucket wird durch den Parameter "MaxItems" bestimmt. <ul><li>MediaGroup – die Ergebnisse werden nach MediaGroup gruppiert.</li></ul> |
| hasTrailer| Boolescher Wert| Optionalen Filter-Parameter. Bestimmt, ob die zurückgegebenen Elemente einen Nachspann enthalten müssen, oder wenn mit einem Nachspann optional ist. Wenn der Wert auf "true" festgelegt ist, müssen alle Elemente Anhänger.|
| id| string| Optional. Wenn angegeben, schränkt die Ergebnisse, um nur untergeordnete Elemente des Elements mit der angegebenen ID sein Wenn dieser Parameter angegeben wird, muss auch MediaItemType angegeben werden. |
| IDs| Array von Zeichenfolgen| Erforderlich. Alle IDs (bis zu 10) für die Details zurückgegeben werden. Beachten Sie, dass alle, die ID enthält Zeichen, die nicht zulässig, fügen Sie in eine URL (die ProviderContentId-Typ-IDs, normalerweise vollständige URLs selbst werden und daher ungültigen Zeichen enthalten) muss die URL-codierte ordnungsgemäß an EDS gesendet werden.|
| idType| string| Optional. Der Typ der IDs der an den Ids-Parameter übergeben werden. Gültige Werte sind: <ul><li>Kanonische (Bing/Marketplace)</li><li>XboxHexTitle (App, die auf der Konsole)</li></ul>  Alle bereitgestellten IDs die gleiche ID-Typ freigeben müssen. Wenn dieser Wert ausgelassen wird, werden alle IDs als Canonical sein.|
| latestOnly| Boolescher Wert| Optionalen Filter-Parameter. Schränkt die Ergebnisse auf nur die Ereignisse mit dem neuesten Release-Datum.|
| "MaxItems"| 32-Bit-Ganzzahl mit Vorzeichen| Optional. Bestimmt die maximale Anzahl der Elemente, die von einem Aufruf zurückgegeben werden sollen. Gültige Werte sind Zahlen zwischen 1 und 25, inklusive. Der Parameter ist standardmäßig auf 25, wenn nicht angegeben.|
| mediaGroup| string| Optional. Die Media-Gruppe, der die IDs werden soll. Alle bereitgestellten IDs die gleiche Media Group freigeben müssen.|
| MediaItemType| string| Optional. Der Typ des Elements, dessen ID im Parameter "Id" angegeben wurde. Wenn der Id-Parameter angegeben wird, muss diese Parameter ebenfalls angegeben werden.|
| orderBy| string| Erforderlich. Die OrderBy-Parameter bestimmt, wie die zurückgegebenen Elemente sortiert werden soll. Die üblichen Werte für dieses Feld sind hier aufgeführt, aber einige APIs unterstützen möglicherweise weitere Werte.<ul><li>PlayCountDaily - von Zeiten Medien wiedergegeben, für den letzten Tag.</li><li>FreeAndPaidCountDaily – von der Anzahl der kostenlosen und kostenpflichtigen Käufe, für den letzten Tag.</li><li>PaidCountAllTime - durch die Anzahl der für die gesamte Laufzeit nur kostenpflichtige Käufe.</li><li>PaidCountDaily – von der Anzahl der für den letzten Tag nur kostenpflichtige Käufe.</li><li>DigitalReleaseDate - Datum, die zum Download zur Verfügung.</li><li>releaseDate – nach Datum, die im Speicher verfügbaren Fallback auf digitale Veröffentlichungsdatum (falls verfügbar).</li><li>UserRatings - durch Durchschnittliche Bewertungen.</li></ul> |
| preferredProvider| string| Optional. Wenn der Benutzer einen bevorzugte Inhaltsanbieter, z. B. Comcast Xfinity oder Verizon FIOS, kann dieses Anbieters-ID in übergeben werden. Während die tatsächliche Reihenfolge der Elemente, für jedes Element, nicht geändert werden den angegebenen Anbieter Informationen (sofern es sich bei der bevorzugte Inhaltsanbieter das Element verfügbar ist) am oberen Rand der Liste der Anbieter angezeigt.|
| q| string| Erforderlich. Fragen Sie ab, Ausdruck, der in der Suche verwendet.|
| queryRefiners| Array von Zeichenfolgen| Optional. Die Liste der [EDS Abfrage Raffinierer](edsqueryrefiners.md).|
| Beziehung| string| Optional. Filter, der den ID-Parameter als Basis zum Suchen nach verwendet andere Produkte, die mit den angegebenen Beziehungstyp übereinstimmen: <ul><li>BundledWith - Bundle-Produkte suchen, in denen der ID-Parameter als Teil von diesen Paketen werden.</li><li>BundledProducts - finden Sie die Produkte, die im durch den ID-Parameter angegebene Paket enthalten sind.</li></ul>  Mit diesem Parameter werden nur Produkte, die im Marketplace sichtbar sind (kann in einem Browse-Aufruf zurückgegeben werden) zurückgegeben. Wenn ein Paket ein ausgeblendetes Produkt verfügt, ist immer noch Teil des Pakets jedoch nicht in diesen Ergebnissen zurückgegeben.|
  | ScopeId | string | Dieser Parameter wird in Szenarien mit reverse-Lookup für video Medien verwendet. |
  | ScopeIdType | string | Dieser Parameter wird in Szenarien mit reverse-Lookup für video Medien verwendet. Mögliche Werte: Titel. |
  | skipItems | 32-Bit-Ganzzahl mit Vorzeichen | Optional. Für das Paging in nicht-Cross-Group-Szenarien, die SkipItems-Parameter wird verwendet, um zu bestimmen, wie viele Elemente bereits angezeigt wurden (und deshalb welches Element angezeigt werden soll zuerst im Ergebnis festgelegt). Der Wert ist 0-basierte, also SkipItems = 0 (oder einfach SkipItems keine Angabe) beginnt, abgerufen am Anfang der Liste. SkipItems = 3 würde lassen Sie die ersten drei Elemente in der Liste aus und beginnen Sie abrufen, mit dem vierten Element. |
  | subscriptionLevel | Array von Zeichenfolgen | Optionalen Filter-Parameter. Die Abonnementstufe-Parameter bestimmt, dass der Typ des Abonnements des Benutzers verfügt (z. B., ob ein Benutzer ein kostenpflichtiges Abonnement oder ein kostenloses Abonnement hat). Mögliche Werte sind wie folgt aus. <ul><li>gold: Der Benutzer verfügt über ein kostenpflichtiges Abonnement</li><li>Silber: Der Benutzer hat es sich um ein kostenloses Abonnement.</li></ul>  |
| TargetDevices| string| EDS bietet die Flexibilität zum Filtern für Zielgeräte bietet. Die Angebote (ProviderContent oder Verfügbarkeit) zurückgegeben, für das Element können auf einem Zielgerät eingeschränkt werden.|
| topRatedOnly| Boolescher Wert| Optionalen Filter-Parameter. Schränkt die Ergebnisse auf nur die besten Inhalte.|

<a id="ID4EGEAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EIEAC"></a>


### <a name="parent"></a>Parent

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)


<a id="ID4EUEAC"></a>


### <a name="further-information"></a>Weitere Informationen

[Marketplace-URIs](../uri/marketplace/atoc-reference-marketplace.md)
