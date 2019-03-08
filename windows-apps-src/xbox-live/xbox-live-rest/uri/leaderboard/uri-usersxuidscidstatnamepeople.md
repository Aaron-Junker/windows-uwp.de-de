---
title: /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
assetID: 0983dad0-59b7-45b7-505d-603e341fe0cc
permalink: en-us/docs/xboxlive/rest/uri-usersxuidscidstatnamepeople.html
description: " /users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: 85a6470a64ceef3b154384d1ca859fb28733aad3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632575"
---
# <a name="usersxuidxuidscidsscidstatsstatnamepeopleallfavorite"></a>/users/xuid({xuid})/scids/{scid}/stats/{statname)/people/{all|favorite}
Greift auf die Bestenliste sozialer Netzwerke (Rangfolge).
Die Domäne für diese URIs ist `leaderboards.xboxlive.com`.

  * [URI-Parameter](#ID4EV)

<a id="ID4EV"></a>


## <a name="uri-parameters"></a>URI-Parameter

| Parameter| Typ| Beschreibung|
| --- | --- | --- |
| xuid| string| Der Bezeichner des Benutzers.|
| scid| string| Der Bezeichner der Dienstkonfiguration aus, die die gewünschte Ressource enthält.|
| statname| string| Eindeutiger Bezeichner der Stat benutzerressource aus, auf die zugegriffen wird.|
| alle\|bevorzugten| Enumeration| Angibt, ob die Stat rank-Werte (Bewertungen) für alle bekannten Kontakte des aktuellen Benutzers oder nur die Kontakte, die als bevorzugte Personen von diesem Benutzer festgelegt.|

<a id="ID4EOC"></a>


## <a name="valid-methods"></a>Gültigen Methoden

[GET (/ Benutzer/Xuid ({Xuid}) /scids/ {scid} /stats/ {Statname) /people/ {alle\|bevorzugten})](uri-usersxuidscidstatnamepeopleget.md)

&nbsp;&nbsp;Gibt eine soziale Bestenliste Rangfolge, die die Stat (Bewertungen) für entweder alle bekannten Kontakte des aktuellen Benutzers oder nur die Kontakte, die als bevorzugte Personen festgelegt wird, von diesem Benutzer Werte zurück.

<a id="ID4EYC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4E1C"></a>


##### <a name="parent"></a>Parent

[Bestenlisten-URIs](atoc-reference-leaderboard.md)
