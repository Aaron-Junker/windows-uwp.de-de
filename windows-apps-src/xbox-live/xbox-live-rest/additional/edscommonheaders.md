---
title: Allgemeine EDS-Header
assetID: 91297c9e-709d-7886-1da0-8a896c4953f4
permalink: en-us/docs/xboxlive/rest/edscommonheaders.html
description: " Allgemeine EDS-Header"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 51a85383ee75fa51cb8376451ac955dcc4fa2edf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630705"
---
# <a name="eds-common-headers"></a>Allgemeine EDS-Header

<a id="ID4EO"></a>



## <a name="entertainment-discovery-services-eds-common-request-headers"></a>Unterhaltung Discovery Services (EDS) weit verbreiteten Anforderungsheadern

| Headername| Beschreibung| Erforderlich?| Anmerkungen|
| --- | --- | --- | --- |
| <b>x-xbl-contract-version</b>| EDS-Service-version| Ja| 3.2|
| <b>x-xbl-client-type</b>| Client-Type-header| Ja| Kommunizieren Sie mit Ihren eigenen Clienttyp Projektteam.|
| <b>x-xbl-client-version</b>| -Clientversion| Ja| Eine beliebige nicht leere Zeichenfolge.|
| <b>x-xbl-parent-ig</b>| Eindruck-guid| Ja| Zum Nachverfolgen der Anforderung in Protokollen und über andere Dienstaufrufe hinweg verwendet.|
| <b>x-xbl-device-type</b>| Gerätetyp| Ja| Gerät, das der Client, der angibt.|
| <b>Akzeptieren</b>| Typ akzeptieren| Ja| XML oder JSON.|
| <b>Autorisierung</b>| Autorisierungsheader| Ja|  |
| <b>x-xbl-autoSuggest-seed-text</b>| Automatischer Vorschlag Seed-text| Nein| Verwendet für BI und Relevanz|
| <b>x-xbl-search-term</b>| Suchbegriff| Nein|  |
| <b>x-xbl-input-method</b>| Die Eingabemethode verwendet durch Benutzer| Nein| Controller, Sprache, Kinect.|
| <b>x-xbl-kinect-enabled</b>| Kinect aktiviert| Nein| Ja/Nein.|
| <b>x-xbl-speech-session-id</b>| Spracherkennung Sitzungs-ID| Nein| Gibt an, ob die Sitzung initiiert wurde über Spracheingabe.|
| <b>x-xbl-client-id</b>| Anonymen Client-Id| Nein| Für BI-berichterstellung und Relevanz verwendet.|
| <b>x-xbl-device-id</b>| Geräte-ID| Nein| Für BI-berichterstellung und Relevanz verwendet.|
| <b>x-xbl-user-agent</b>| Client-Benutzer-agent| Nein| Für BI verwendet. "&lt;Name > /&lt;Version > (&lt;Betriebssystemversion >; &lt;Plattform >; &lt;Funktion >; &lt;Bauteile >; &lt;Modell >) ".|
| <b>x-xbl-parent-ig</b>| Vorherige Eindruck-Guid für "Chained"-Aufrufe| Nein (aber dringend empfohlen)| Wichtig für BI relevant ist. Beispielsweise ist ein Durchsuchen-Aufruf IG der übergeordnete IG für ein Detail-Aufruf.|
| <b>delid</b>| Delegieren der Identität| Nein| Verwendet durch interne Dienste im Auftrag eines Benutzers zu arbeiten.|

## <a name="common-response-headers"></a>Weit verbreiteten Antwortheadern

| Headername| Beschreibung| Erforderlich?| Anmerkungen|
| --- | --- | --- | --- | --- | --- | --- | --- |
| <b>Cache</b>| Cache-Header| Ja| Finden Sie unter <a href="https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9"> http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9 </a>.|
| <b>x-xbl-errors</b>| Fehler| Nein| Liste der Fehler von verschiedenen Datenanbietern.|
| <b>x-xbl-traceid</b>| Ablaufverfolgungs-Id von Protokollen| Ja| Verwendet, um die bestimmten Anforderungsprotokolle zurückzuverfolgen.|
| <b>x-server-name</b>| Verborgene Name des Servers, der die Anforderung verarbeitet.| Ja| Zum Debuggen von internen verwendet.|

<a id="ID4EECAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4EGCAC"></a>


##### <a name="parent"></a>Parent  

[Zusätzliche Referenz](atoc-xboxlivews-reference-additional.md)


<a id="ID4ESCAC"></a>


##### <a name="further-information"></a>Weitere Informationen

[Marketplace-URIs](../uri/marketplace/atoc-reference-marketplace.md)
