---
title: Achievement (JSON)
assetID: d3b52f66-ddc7-e676-b419-82209caf71d6
permalink: en-us/docs/xboxlive/rest/json-achievementv2.html
description: " Achievement (JSON)"
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox Live, Xbox, Spiele, UWP, Windows 10, Xbox One
ms.localizationpriority: medium
ms.openlocfilehash: b0e20f46a0d97cba496df5c6fb9cda14fbeccccd
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628475"
---
# <a name="achievement-json"></a>Achievement (JSON)
Ein Objekt der Auszeichnung (Version 2).
<a id="ID4EN"></a>


## <a name="achievement"></a>Auszeichnung

Auszeichnung Objekt verfügt über den folgenden Spezifikationen. Alle Elemente sind erforderlich.

| Mitglied| Typ| Beschreibung|
| --- | --- | --- |
| id| string| Ressourcenbezeichner.|
| serviceConfigId| string| SCID für diese Ressource. Identifiziert die Berufsbezeichnung aus, denen um diese Auszeichnung verknüpft ist. |
| name| string| Der lokalisierte Name der Auszeichnung.|
| titleAssociations| Array von [TitleAssociation](json-titleassociation.md)| Ein Array von TitleAssociation.|
| progressState| **ProgressState** Enumeration| Der Status der Fortschritt: <ul><li>Ungültige (0): Der Fortschritt der Auszeichnung befindet sich in einem unbekannten Zustand.</li><li>erreicht (1): Das Erreichen der Zertifikation wurde entsperrt.</li><li>in Bearbeitung (2): Das Erreichen der Zertifikation ist gesperrt, aber der Benutzer hat die Fortschritte bei der es entsperrt.</li><li>notStarted (3): Das Erreichen der Zertifikation gesperrt ist, und der Benutzer hat keine Fortschritt in Bezug auf das es entsperrt.</li></ul> | 
| Fortschritt| [Fortschritt](json-progression.md)| Der Benutzer den Fortschritt innerhalb der Auszeichnung.|
| mediaAssets| Array von [MediaAsset](json-mediaasset.md)| Die Medienobjekte, die die Leistung wird z. B. Image-IDs zugeordnet. |
| Plattform| string| Die Plattform erreichen der Zertifikation wurde auf erworben.|
| isSecret| Boolescher Wert| Unabhängig davon, ob das Erreichen der Zertifikation geheimen ist.|
| description| string| Die Beschreibung des Erfolgs Wenn entsperrt.|
| lockedDescription| string| Die Beschreibung des Erfolgs, bevor es entsperrt wird.|
| productId| string| Das Erreichen der Zertifikation "ProductID" wurde mit veröffentlicht.|
| achievementType| **AchievementType** Enumeration| Der Typ der Auszeichnung (nicht die identisch mit dem vorherigen Typ auf ältere Erfolge): <ul><li>Ungültige (0): Eine Auszeichnung von unbekannten und nicht unterstützten Typ.</li><li>persistente (1): Eine Auszeichnung, die kein Enddatum und zu einem beliebigen Zeitpunkt entsperrt werden kann.</li><li>Die Herausforderung (2): Eine Auszeichnung, die ein bestimmtes Zeitfenster aufweist, in dem es nicht gesperrt sein kann.</li></ul> |
| participationType| **ParticipationType** Enumeration| Der Typ der Teilnahme für das Erreichen der Zertifikation. Gültige Werte sind einzeln oder Gruppe.|
| timeWindow| TimeWindow| Das Zeitfenster, in dem das Erreichen der Zertifikation entsperrt werden kann. Nur unterstützt für Herausforderungen.|
| Boni| Array von [Reward](json-reward.md)| Die Auflistung von Boni erworben werden, nachdem die Sperre aufgehoben werden soll.|
| estimatedTime| TimeSpan| Wie lange dauert das Erreichen der Zertifikation mit.|
| Deep-Link| string| Ein Deep-Link in den Titel.|
| isRevoked| Boolescher Wert| Unabhängig davon, ob das Erreichen der Zertifikation Erzwingung gesperrt ist.|

<a id="ID4EIAAC"></a>


## <a name="sample-json-syntax"></a>Beispiel-JSON-syntax


```json
{
        "id":"3",
        "serviceConfigId":"b5dd9daf-0000-0000-0000-000000000000",
        "name":"Default NameString for Microsoft Achievements Sample Achievement 3",
        "titleAssociations":
        [{
                "name":"Microsoft Achievements Sample",
                "id":3051199919,
                "version":"abc"
        }],
        "progressState":"Achieved",
        "progression":
        {
          "requirements":
          [{
            "id":"12345678-1234-1234-1234-123456789012",
            "current":null,
            "target":"100"
          }],
          "timeUnlocked":"2013-01-17T03:19:00.3087016Z",
        },
        "mediaAssets":
        [{
                "name":"Icon Name",
                "type":"Icon",
                "url":"https://www.xbox.com"
        }],
        "platform":"D",
        "isSecret":true,
        "description":"Default DescriptionString for Microsoft Achievements Sample Achievement 3",
        "lockedDescription":"Default UnachievedString for Microsoft Achievements Sample Achievement 3",
        "productId":"12345678-1234-1234-1234-123456789012",
        "achievementType":"Challenge",
        "participationType":"Individual",
        "timeWindow":
        {
                "startDate":"2013-02-01T00:00:00Z",
                "endDate":"2100-07-01T00:00:00Z"
        },
        "rewards":
        [{
                "name":null,
                "description":null,
                "value":"10",
                "type":"Gamerscore",
                "valueType":"Int"
        },
        {
                "name":"Default Name for InAppReward for Microsoft Achievements Sample Achievement 3",
                "description":"Default Description for InAppReward for Microsoft Achievements Sample Achievement 3",
                "value":"1",
                "type":"InApp",
                "valueType":"String"
        }],
        "estimatedTime":"06:12:14",
        "deeplink":"aWFtYWRlZXBsaW5r",
        "isRevoked":false
    }

```


<a id="ID4ERAAC"></a>


## <a name="see-also"></a>Siehe auch

<a id="ID4ETAAC"></a>


##### <a name="parent"></a>Parent

[Objektverweis für JavaScript Object Notation (JSON)](atoc-xboxlivews-reference-json.md)
