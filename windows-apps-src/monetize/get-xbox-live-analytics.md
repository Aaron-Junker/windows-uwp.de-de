---
description: Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um Xbox Live Analytics-Daten zu erhalten.
title: Abrufen von Xbox Live-Analysedaten
ms.date: 06/04/2018
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store Analytics-API, Xbox Live Analytics
ms.localizationpriority: medium
ms.openlocfilehash: 5649a81e1c6e869e9e010841cb31432350bb33d6
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89172884"
---
# <a name="get-xbox-live-analytics-data"></a>Abrufen von Xbox Live-Analysedaten

Verwenden Sie diese Methode in der Microsoft Store Analytics-API, um die letzten 30 Tage allgemeiner Analysedaten für Kunden zu erhalten, die Ihr [Xbox Live-fähiges Spiel](/gaming/xbox-live/index.md)spielen, einschließlich Geräte Zubehör, Internet Verbindungstyp, Gamerscore-Verteilung, Spielstatistik und Freunde und Follower-Daten. Diese Informationen sind auch im Bericht " [Xbox Analytics](../publish/xbox-analytics-report.md) " im Partner Center verfügbar.

> [!IMPORTANT]
> Diese Methode unterstützt nur Spiele für Xbox oder Spiele, die Xbox Live-Dienste verwenden. Diese Spiele müssen den [Genehmigungsprozess des Konzepts](../gaming/concept-approval.md)durchlaufen, der Spiele umfasst, die von [Microsoft-Partnern](/gaming/xbox-live/developer-program-overview.md#microsoft-partners) und über das [ ID@Xbox Programm](/gaming/xbox-live/developer-program-overview.md#id)gesendeten spielen veröffentlicht wurden. Diese Methode unterstützt zurzeit keine Spiele, die über das [Xbox Live Creators-Programm](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md)veröffentlicht wurden.

Zusätzliche Analysedaten für Spiele, die Xbox Live-fähig sind, sind über die folgenden Methoden verfügbar:
* [Abrufen von Xbox Live-Erfolgsdaten](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Integritätsdaten](get-xbox-live-health-data.md)
* [Abrufen von Xbox Live Spielehubdaten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)
* [Abrufen von Xbox Live-Daten zur gleichzeitigen Nutzung](get-xbox-live-concurrent-usage-data.md)

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methode sind folgende Schritte erforderlich:

* Wenn Sie dies nicht bereits getan haben, müssen Sie alle [Voraussetzungen](access-analytics-data-using-windows-store-services.md#prerequisites) für die Microsoft Store Analytics-API erfüllen.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), das im Anforderungsheader für diese Methode verwendet wird. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung


### <a name="request-syntax"></a>Anforderungssyntax

| Methode | Anforderungs-URI       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics``` |


### <a name="request-header"></a>Anforderungsheader

| Header        | type   | BESCHREIBUNG                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |


### <a name="request-parameters"></a>Anforderungsparameter

| Parameter        | Typ   |  BESCHREIBUNG      |  Erforderlich  
|---------------|--------|---------------|------|
| applicationId | Zeichenfolge | Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie allgemeine Xbox Live-Analysedaten abrufen möchten.  |  Ja  |
| metrictype | Zeichenfolge | Eine Zeichenfolge, die den Typ der abzurufenden Xbox Live Analytics-Daten angibt. Geben Sie für diese Methode den Wert **ProductValues**an.  |  Ja  |


### <a name="request-example"></a>Anforderungsbeispiel

Das folgende Beispiel zeigt eine Anforderung zum erhalten allgemeiner Analysedaten für Kunden, die Ihr Xbox Live-fähiges Spiel spielen. Ersetzen Sie den Wert *ApplicationId* durch die Store-ID für Ihr Spiel.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/gameanalytics?applicationId=9NBLGGGZ5QDR&metrictype=productvalues HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Antwort

Diese Methode gibt ein *Wert* Array zurück, das die folgenden Objekte enthält.

| Object      | BESCHREIBUNG                  |
|-------------|---------------------------------------------------|
| Productdata   |   Enthält ein [deviceproperties](#deviceproperties) -Objekt und ein [UserProperties](#userproperties) -Objekt, das die letzten 30 Tage der Geräte-und Benutzer Analysedaten für das Spiel enthält.    |  
| Xboxwidedata   |  Enthält ein [deviceproperties](#deviceproperties) -Objekt und ein [UserProperties](#userproperties) -Objekt, das die letzten 30 Tage der durchschnittlichen Geräte-und Benutzer Analysedaten für alle Xbox Live-Kunden als Prozentsätze enthält. Diese Daten sind für Vergleichszwecke mit den Daten für Ihr Spiel enthalten.   |                                           


### <a name="deviceproperties"></a>Deviceproperties

Diese Ressource enthält Geräte Verwendungs Daten für Ihr Spiel oder die durchschnittlichen Geräte Verwendungs Daten für alle Xbox Live-Kunden während der letzten 30 Tage.

| Wert           | Typ    | BESCHREIBUNG        |
|-----------------|---------|------|
|  applicationId               |    Zeichenfolge     |  Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie Analysedaten abgerufen haben.   |
|  connectiontypedistribution               |    array     |   Enthält Objekte, die angeben, wie viele Kunden eine kabelgebundene Internetverbindung oder eine drahtlos Internetverbindung auf Xbox verwenden. Jedes-Objekt verfügt über zwei Zeichen folgen Felder: <ul><li>**Contype**: gibt den Verbindungstyp an.</li><li>**devicecount**: im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden des Spiels an, die den Verbindungstyp verwenden. Im **xboxwidedata** -Objekt gibt dieses Feld den Prozentsatz aller Xbox Live-Kunden an, die den Verbindungstyp verwenden.</li></ul>   |     
|  deviceCount               |   Zeichenfolge      |  Im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden Geräte an, auf denen das Spiel während der letzten 30 Tage wiedergegeben wurde. Im **xboxwidedata** -Objekt ist dieses Feld immer 1, was einen Anfangs Prozentsatz von 100% für Daten für alle Xbox Live-Kunden angibt.   |     
|  elitecontrollerpresentdevicecount               |   Zeichenfolge      |  Im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden Ihres Spiels an, die den drahtlosen Xbox Elite-Controller verwenden. Im **xboxwidedata** -Objekt gibt dieses Feld den Prozentsatz aller Xbox Live-Kunden an, die den drahtlosen Xbox Elite-Controller verwenden.  |     
|  externaldrivepresentdevicecount               |   Zeichenfolge      |  Im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden Ihres Spiels an, die eine externe Festplatte auf Xbox verwenden. Im **xboxwidedata** -Objekt gibt dieses Feld den Prozentsatz aller Xbox Live-Kunden an, die eine externe Festplatte auf der Xbox verwenden.  |


### <a name="userproperties"></a>UserProperties

Diese Ressource enthält Benutzerdaten für das Spiel oder die durchschnittlichen Benutzerdaten für alle Xbox Live-Kunden während der letzten 30 Tage.

| Wert           | Typ    | BESCHREIBUNG        |
|-----------------|---------|------|
|  applicationId               |    Zeichenfolge     |   Die [Speicher-ID](in-app-purchases-and-trials.md#store-ids) des Spiels, für das Sie Analysedaten abgerufen haben.  |
|  userCount               |    Zeichenfolge     |   Im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden an, die Ihr Spiel innerhalb der letzten 30 Tage wiedergegeben haben. Im **xboxwidedata** -Objekt ist dieses Feld immer 1, was einen Anfangs Prozentsatz von 100% für Daten für alle Xbox Live-Kunden angibt.   |     
|  dvrusagecounts               |   array      |  Enthält Objekte, die angeben, wie viele Kunden Game DVR verwendet haben, um das-Spiel aufzuzeichnen und anzuzeigen. Jedes-Objekt verfügt über zwei Zeichen folgen Felder: <ul><li>**dvrname**: gibt das verwendete Spiel-DVR-Feature an. Mögliche Werte sind " **gameclipuploads**", " **gameclipviews**", " **screenshotuploads**" und " **screenshotviews**".</li><li>**User count**: im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden Ihres Spiels an, die das angegebene Spiel-DVR-Feature verwendet haben. Im **xboxwidedata** -Objekt gibt dieses Feld den Prozentsatz aller Xbox Live-Kunden an, die das angegebene Spiel-DVR-Feature verwendet haben.</li></ul>   |     
|  Follow-count-perzentiles               |   array      |  Enthält Objekte, die Details zur Anzahl der Follower für Kunden bereitstellen. Jedes-Objekt verfügt über zwei Zeichen folgen Felder: <ul><li>**Prozentsatz**: derzeit ist dieser Wert immer 50, was darauf hinweist, dass die Follower-Daten als Medianwert bereitgestellt werden.</li><li>**Wert**: im **productdata** -Objekt gibt dieses Feld die durchschnittliche Anzahl von Followern für die Kunden Ihres Spiels an. Im **xboxwidedata** -Objekt gibt dieses Feld die durchschnittliche Anzahl von Followern für alle Xbox Live-Kunden an.</li></ul>  |   
|  friendzählperzentile               |   array      |  Enthält Objekte, die Details zur Anzahl der Freunde für Kunden bereitstellen. Jedes-Objekt verfügt über zwei Zeichen folgen Felder: <ul><li>**Prozentsatz**: derzeit ist dieser Wert immer 50, was darauf hinweist, dass die Friends-Daten als Medianwert bereitgestellt werden.</li><li>**Wert**: im **productdata** -Objekt gibt dieses Feld die durchschnittliche Anzahl der Freunde für die Kunden Ihres Spiels an. Im **xboxwidedata** -Objekt gibt dieses Feld die durchschnittliche Anzahl der Freunde für alle Xbox Live-Kunden an.</li></ul>  |     
|  gamerscorerangedistribution               |   array      |  Enthält Objekte, die Details zur Gamerscore-Verteilung für Kunden bereitstellen. Jedes-Objekt verfügt über zwei Zeichen folgen Felder: <ul><li>**scorerange**: der Gamerscore-Bereich, für den das folgende Feld Verwendungs Daten bereitstellt. Beispielsweise **10K-25K**.</li><li>**User count**: im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden des Spiels an, die ein Gamerscore im angegebenen Bereich für alle Spiele haben, die Sie gespielt haben. Im **xboxwidedata** -Objekt gibt dieses Feld den Prozentsatz aller Xbox Live-Kunden an, die über ein Gamerscore im angegebenen Bereich für alle Spiele verfügen, die Sie gespielt haben.</li></ul>  |
|  titlegamerscorerangedistribution               |   array      |  Enthält-Objekte, die Details zur Gamerscore-Verteilung für das spielbereit stellen. Jedes-Objekt verfügt über zwei Zeichen folgen Felder: <ul><li>**scorerange**: der Gamerscore-Bereich, für den das folgende Feld Verwendungs Daten bereitstellt. Beispiel: **100-200**.</li><li>**User count**: im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden des Spiels an, die über ein Gamerscore im angegebenen Bereich für Ihr Spiel verfügen. Im **xboxwidedata** -Objekt gibt dieses Feld den Prozentsatz aller Xbox Live-Kunden an, die über ein Gamerscore im angegebenen Bereich für das Spiel verfügen.</li></ul>   |
|  socialusagecounts               |   array      |  Enthält Objekte, die Details zur sozialen Nutzung für Kunden bereitstellen. Jedes-Objekt verfügt über zwei Zeichen folgen Felder: <ul><li>**scname**: der Typ der sozialen Nutzung. Beispielsweise **Gaman Vites** und **TextMessages**.</li><li>**User count**: im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden des Spiels an, die am angegebenen Typ der sozialen Nutzung beteiligt waren. Im **xboxwidedata** -Objekt gibt dieses Feld den Prozentsatz aller Xbox Live-Kunden an, die am angegebenen Typ der sozialen Nutzung beteiligt sind.</li></ul>   |
|  streamingusagecounts               |   array      |  Enthält Objekte, die Details zur streamingverwendung für Kunden bereitstellen. Jedes-Objekt verfügt über zwei Zeichen folgen Felder: <ul><li>**stname**: der Typ der streamingplattform. Beispielsweise " **youtubeusage**", " **twitchusage**" und " **mixerusage**".</li><li>**User count**: im **productdata** -Objekt gibt dieses Feld die Anzahl der Kunden Ihres Spiels an, die die angegebene streamingplattform verwendet haben. Im **xboxwidedata** -Objekt gibt dieses Feld den Prozentsatz aller Xbox Live-Kunden an, die die angegebene streamingplattform verwendet haben.</li></ul>  |


### <a name="response-example"></a>Antwortbeispiel

Das folgende Beispiel zeigt ein Beispiel für einen JSON-Antworttext für diese Anforderung.

```json
{
  "Value": [
    {
      "ProductData": {
        "DeviceProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "43806"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "104035"
              }
            ],
            "deviceCount": "148063",
            "eliteControllerPresentDeviceCount": "10615",
            "externalDrivePresentDeviceCount": "46388"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "9NBLGGGZ5QDR",
            "userCount": "142345",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "31264"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "52236"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "27051"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "45640"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "11"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "30015"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "20495"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "32438"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "10608"
              },
              {
                "scoreRange": "<3K",
                "userCount": "45726"
              },
              {
                "scoreRange": ">100K",
                "userCount": "3063"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "400-600",
                "userCount": "133875"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "45960"
              },
              {
                "scoreRange": "<100",
                "userCount": "269137"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "11634"
              },
              {
                "scoreRange": "100-200",
                "userCount": "334471"
              },
              {
                "scoreRange": "600-800",
                "userCount": "123044"
              },
              {
                "scoreRange": "200-400",
                "userCount": "396725"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "82390"
              },
              {
                "scName": "textMessages",
                "userCount": "91880"
              },
              {
                "scName": "partySessionCount",
                "userCount": "68129"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "74092"
              },
              {
                "stName": "twitchUsage",
                "userCount": "13401"
              }
              {
                "stName": "mixerUsage",
                "userCount": "22907"
              }
            ]
          }
        ]
      },
      "XboxwideData": {
        "DeviceProperties": [
          {
            "applicationId": "XBOXWIDE",
            "connectionTypeDistribution": [
              {
                "conType": "WIRED",
                "deviceCount": "0.213677732584786"
              },
              {
                "conType": "WIRELESS",
                "deviceCount": "0.786322267415214"
              }
            ],
            "deviceCount": "1",
            "eliteControllerPresentDeviceCount": "0.0476609278128012",
            "externalDrivePresentDeviceCount": "0.173747147416134"
          }
        ],
        "UserProperties": [
          {
            "applicationId": "XBOXWIDE",
            "userCount": "1",
            "dvrUsageCounts": [
              {
                "dvrName": "gameClipUploads",
                "userCount": "0.173210623993245"
              },
              {
                "dvrName": "gameClipViews",
                "userCount": "0.202104713778096"
              },
              {
                "dvrName": "screenshotUploads",
                "userCount": "0.136682414274251"
              },
              {
                "dvrName": "screenshotViews",
                "userCount": "0.158057895120314"
              }
            ],
            "followerCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "friendCountPercentiles": [
              {
                "percentage": "50",
                "value": "5"
              }
            ],
            "gamerScoreRangeDistribution": [
              {
                "scoreRange": "10K-25K",
                "userCount": "0.134709282586519"
              },
              {
                "scoreRange": "25K-50K",
                "userCount": "0.0549468789343825"
              },
              {
                "scoreRange": "50K-100K",
                "userCount": "0.017301313342277"
              },
              {
                "scoreRange": "3K-10K",
                "userCount": "0.216512780268453"
              },
              {
                "scoreRange": "<3K",
                "userCount": "0.573515440094644"
              },
              {
                "scoreRange": ">100K",
                "userCount": "0.00301430477372488"
              }
            ],
            "titleGamerScoreRangeDistribution": [
              {
                "scoreRange": "100-200",
                "userCount": "0.178055695637076"
              },
              {
                "scoreRange": "200-400",
                "userCount": "0.173283639825241"
              },
              {
                "scoreRange": "400-600",
                "userCount": "0.0986865193958259"
              },
              {
                "scoreRange": "600-800",
                "userCount": "0.0506375775462092"
              },
              {
                "scoreRange": "800-1000",
                "userCount": "0.0232398822856435"
              },
              {
                "scoreRange": "<100",
                "userCount": "0.456443551582991"
              },
              {
                "scoreRange": "≥1K",
                "userCount": "0.0196531337270126"
              }
            ],
            "socialUsageCounts": [
              {
                "scName": "gameInvites",
                "userCount": "0.460375855738335"
              },
              {
                "scName": "textMessages",
                "userCount": "0.429256324070832"
              },
              {
                "scName": "partySessionCount",
                "userCount": "0.378446577751268"
              },
              {
                "scName": "gamehubViews",
                "userCount": "0.000197115778147329"
              }
            ],
            "streamingUsageCounts": [
              {
                "stName": "youtubeUsage",
                "userCount": "0.330320919178683"
              },
              {
                "stName": "twitchUsage",
                "userCount": "0.040666241835399"
              }
              {
                "stName": "mixerUsage",
                "userCount": "0.140193816053558"
              }
            ]
          }
        ]
      }
    }
  ],
  "@nextLink": null,
  "TotalCount": 4
}
```

## <a name="related-topics"></a>Zugehörige Themen

* [Zugreifen auf Analytics-Daten mithilfe von Microsoft Store Services](access-analytics-data-using-windows-store-services.md)
* [Abrufen von Xbox Live-Erfolgsdaten](get-xbox-live-achievements-data.md)
* [Abrufen von Xbox Live-Integritätsdaten](get-xbox-live-health-data.md)
* [Get Xbox Live Game Hub-Daten](get-xbox-live-game-hub-data.md)
* [Abrufen von Xbox Live-Clubdaten](get-xbox-live-club-data.md)
* [Abrufen von Xbox Live Multiplayerdaten](get-xbox-live-multiplayer-data.md)
* [Abrufen von Xbox Live-Daten zur gleichzeitigen Nutzung](get-xbox-live-concurrent-usage-data.md)