---
ms.assetid: d305746a-d370-4404-8cde-c85765bf3578
description: Verwenden Sie diese Methode in der Microsoft Store-Werbungs-API, um Zielgruppenprofile für Werbeanzeigenkampagnen zu verwalten.
title: Verwalten von Zielgruppenprofilen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Werbungs-API, Anzeigenkampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 0d84c6eb678bf884709e13ecefd81e64097ee738
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630205"
---
# <a name="manage-targeting-profiles"></a>Verwalten von Zielgruppenprofilen


Verwenden Sie diese Methoden in der Microsoft Store-Werbungs-API, um Benutzer, Regionen und Bestandstypen als Ziele für jede Lieferpositionen einer Werbeanzeigenkampagne auszuwählen. Zielgruppenprofile können erstellt und über mehrere Lieferpositionen der Übermittlung hinweg wiederverwendet werden.

Weitere Informationen zu der Beziehung zwischen Zielgruppenprofilen und Anzeigenkampagnen, Lieferpositionen und Werbemitteln finden Sie unter [Anzeigenkampagnen mit Microsoft Store-Diensten ausführen](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Voraussetzungen

Zur Verwendung dieser Methoden sind folgende Schritte erforderlich:

* Falls noch nicht geschehen, erfüllen Sie alle [Voraussetzungen](run-ad-campaigns-using-windows-store-services.md#prerequisites) für die Microsoft Store-Werbungs-API.
* [Rufen Sie ein Azure AD-Zugriffstoken ab](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token), das in der Anforderungskopfzeile für diese Methoden verwendet wird. Nach Erhalt eines Zugriffstokens können Sie es 60 Minuten lang verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anfordern

Diese Methoden haben die folgenden URIs.

| Methodentyp | Anforderungs-URI                                                      |  Beschreibung  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile``` |  Erstellt ein neues Zielgruppenprofil.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Ändert das durch *TargetingProfileId* angegebene Zielgruppenprofil.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Ruft das durch *TargetingProfileId* angegebene Zielgruppenprofil ab.  |


### <a name="header"></a>Header

| Header        | Typ   | Beschreibung         |
|---------------|--------|---------------------|
| Autorisierung | string | Erforderlich. Die Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;. |
| Tracking-ID   | GUID   | Optional. Eine ID, die den Abfrageablauf verfolgt.                                  |


### <a name="request-body"></a>Anforderungstext

Die POST- und PUT-Methoden erfordern einen JSON-Anforderungstext mit den erforderlichen Feldern für ein [Zielgruppenprofil](#targeting-profile)-Objekt und alle zusätzlichen Felder, die Sie festlegen oder ändern möchten.


### <a name="request-examples"></a>Anforderungsbeispiele

Im folgenden Beispiel wird veranschaulicht, wie die POST-Methode zum Erstellen eines Profils für aufgerufen wird.

```json
POST https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile HTTP/1.1
Authorization: Bearer <your access token>

{
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652],
    "gender": [
      700
    ],
    "country": [
      11,
      12
    ],
    "osVersion": [
      504
    ],
    "deviceType": [
      710
    ],
    "supplyType": [
      11470
    ]
}
```

Im folgenden Beispiel wird veranschaulicht, wie die GET-Methode zum Abrufen eines Profils für aufgerufen wird.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/310023951  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>

## <a name="response"></a>Antwort

Diese Methoden geben einen JSON-Antworttext mit einem [Zielgruppenprofil](#targeting-profile)-Objekt zurück, das Informationen über das erstellte, aktualisierte bzw. abgerufene Zielgruppenprofil enthält. Das folgende Beispiel zeigt einen Antworttext für diese Methoden.

```json
{
  "Data": {
    "id": 310021746,
    "name": "Contoso App Campaign - Targeting Profile 1",
    "targetingType": "Manual",
    "age": [
      651,
      652
    ],
    "gender": [
      700
    ],
    "country": [
      6,
      13,
      29
    ],
    "osVersion": [
      504,
      505,
      506,
      507,
      508
    ],
    "deviceType": [
      710,
      711
    ],
    "supplyType": [
      11470
    ]
  }
}
```

<span id="targeting-profile"/>

## <a name="targeting-profile-object"></a>Zielgruppenprofil-Objekt

Die Anforderungs- und Antworttexte für diese Methoden enthalten die folgenden Felder. Die folgende Tabelle zeigt, welche Felder schreibgeschützt sind (d. h. sie können in der PUT-Methode nicht geändert werden) und welche Felder in dem Anforderungstext für die POST-Methode erforderlich sind.

| Feld        | Typ   |  Beschreibung      |  Schreibgeschützt  | Standard  | Erforderlich für POST |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  Ganzzahl   |  Die ID des Zielgruppenprofils.     |   Ja    |       |   Nein      |       
|  name   |  string   |   Der Name des Zielgruppenprofils.    |    Nein   |      |  Ja     |       
|  targetingType   |  string   |  Einer der folgenden Werte: <ul><li>**Auto**: Geben Sie diesen Wert, um die Microsoft für die Zielgruppenadressierung basierte auf den Einstellungen für Ihre app im Partner Center Profil auswählen zu können.</li><li>**Manuell**: Geben Sie diesen Wert auf ein eigenes Profil für definieren.</li></ul>     |  Nein     |  Auto    |   Ja    |       
|  Alter   |  array   |   Eine oder mehrere ganze Zahlen, die den Altersbereich der Benutzer in der Zielgruppe angeben. Eine vollständige Liste von ganzen Zahlen finden Sie unter [Alterswerte](#age-values) in diesem Artikel.    |    Nein    |  Null    |     Nein    |       
|  gender   |  array   |  Eine oder mehrere Ganzzahlen, die das Geschlecht der Benutzer in der Zielgruppe angeben. Eine vollständige Liste von ganzen Zahlen finden Sie unter [Geschlechtswerte](#gender-values) in diesem Artikel.       |  Nein    |  Null    |     Nein    |       
|  Land   |  array   |  Eine oder mehrere ganze Zahlen, die die Ländercodes der Benutzer in der Zielgruppe angeben. Eine vollständige Liste von ganzen Zahlen finden Sie unter [Ländercodewerte](#country-code-values) in diesem Artikel.    |  Nein    |  Null   |      Nein   |       
|  osVersion   |  array   |   Eine oder mehrere ganze Zahlen, die die Betriebssystemversionen der Benutzer in der Zielgruppe angeben. Eine vollständige Liste von ganzen Zahlen finden Sie unter [Betriebssystemversionswerte](#osversion-values) in diesem Artikel.     |   Nein    |  Null   |     Nein    |       
|  deviceType   | array    |  Eine oder mehrere ganze Zahlen, die die Gerätetypen der Benutzer in der Zielgruppe angeben. Eine vollständige Liste von ganzen Zahlen finden Sie unter [Gerätetypenwerte](#devicetype-values) in diesem Artikel.       |   Nein    |  Null    |    Nein     |       
|  supplyType   |  array   |  Eine oder mehrere ganze Zahlen, die den Bestandstyp angeben, in dem die Anzeigen der Kampagne angezeigt werden sollen. Eine vollständige Liste von ganzen Zahlen finden Sie unter [Bestandstypenwerte](#supplytype-values) in diesem Artikel.      |   Nein    |  Null   |     Nein    |   |  


<span id="age-values"/>

### <a name="age-values"></a>Alterswerte

Das Feld *Alter* im Objekt [TargetingProfile](#targeting-profile) enthält eine oder mehrere der folgenden Ganzzahlen, die den Altersbereich der Benutzer in der Zielgruppe angeben.

|  Ganzzahliger Wert für Feld *Alter*  |  Entsprechender Altersbereich  |  
|---------------------------------|---------------------------|
|     651     |            13 bis 17             |
|     652     |           18 bis 24             |
|     653     |            25 bis 34             |
|     654     |            35 bis 49             |
|     655     |            50 und älter             |

Die unterstützten Werte für das Feld *age* können Sie programmgesteuert erhalten, indem Sie die folgende GET-Methode aufrufen.  Für die ```Authorization``` -Header, übergeben Sie Ihr Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/age
Authorization: Bearer <your access token>
```

Mit dem folgenden Codebeispiel wird der Antworttext für diese Methode veranschaulicht.

```json
{
  "Data": {
    "Age": {
      "651": "Age13To17",
      "652": "Age18To24",
      "653": "Age25To34",
      "654": "Age35To49",
      "655": "Age50AndAbove"
    }
  }
}
```

<span id="gender-values"/>

### <a name="gender-values"></a>Geschlechtswerte

Das Feld *Geschlecht* im Objekt [TargetingProfile](#targeting-profile) enthält eine oder mehrere der folgenden Ganzzahlen, die das Geschlecht der Benutzer in der Zielgruppe angeben.

|  Ganzzahliger Wert für das Feld *gender*  |  Entsprechendes Geschlecht  |  
|---------------------------------|---------------------------|
|     700     |            Männlich             |
|     701     |           Weiblich             |

Die unterstützten Werte für das Feld *gender* können Sie programmgesteuert erhalten, indem Sie die folgende GET-Methode aufrufen.  Für die ```Authorization``` -Header, übergeben Sie Ihr Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/gender
Authorization: Bearer <your access token>
```

Mit dem folgenden Codebeispiel wird der Antworttext für diese Methode veranschaulicht.

```json
{
  "Data": {
    "Gender": {
      "700": "Male",
      "701": "Female"
    }
  }
}
```


<span id="osversion-values"/>

### <a name="os-version-values"></a>Werte für Betriebssystemversion

Das Feld *Betriebssystemversion* im Objekt [TargetingProfile](#targeting-profile) enthält eine oder mehrere der folgenden Ganzzahlen, die die Betriebssystemversion der Benutzer in der Zielgruppe angeben.

|  Ganzzahliger Wert für das Feld *osVersion*  |  Entsprechende Betriebssystemversion  |  
|---------------------------------|---------------------------|
|     500     |            Windows Phone 7             |
|     501     |           Windows Phone 7.1             |
|     502     |           Windows Phone 7.5             |
|     503     |           Windows Phone 7.8             |
|     504     |           Windows Phone 8.0             |
|     505     |           Windows Phone 8.1             |
|     506     |           Windows 8.0             |
|     507     |           Windows 8.1             |
|     508     |           Windows 10             |
|     509     |           Windows 10 Mobile             |

Die unterstützten Werte für das Feld *osVersion* können Sie programmgesteuert erhalten, indem Sie die folgende GET-Methode aufrufen.  Für die ```Authorization``` -Header, übergeben Sie Ihr Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/osversion
Authorization: Bearer <your access token>
```

Mit dem folgenden Codebeispiel wird der Antworttext für diese Methode veranschaulicht.

```json
{
  "Data": {
    "OsVersion": {
      "500": "WindowsPhone70",
      "501": "WindowsPhone71",
      "502": "WindowsPhone75",
      "503": "WindowsPhone78",
      "504": "WindowsPhone80",
      "505": "WindowsPhone81",
      "506": "Windows80",
      "507": "Windows81",
      "508": "Windows10",
      "509": "WindowsPhone10"
    }
  }
}
```


<span id="devicetype-values"/>

### <a name="device-type-values"></a>Werte für Gerätetyp

Das Feld *deviceType* im Objekt [TargetingProfile](#targeting-profile) enthält eine oder mehrere der folgenden Ganzzahlen, die die Gerätetypen der Benutzer in der Zielgruppe angeben.

|  Ganzzahliger Wert für das Feld *deviceType*  |  Entsprechender Gerätetyp  |  Beschreibung  |
|---------------------------------|---------------------------|---------------------------|
|     710     |  Windows   |  Hierbei handelt es sich um Geräte mit einer Desktop-Version von Windows 10 oder Windows 8.x.  |
|     711     |  Phone     |  Hierbei handelt es sich um Geräte unter Windows 10 Mobile, Windows Phone 8.x oder Windows Phone 7.x.

Die unterstützten Werte für das Feld *deviceType* können Sie programmgesteuert erhalten, indem Sie die folgende GET-Methode aufrufen.  Für die ```Authorization``` -Header, übergeben Sie Ihr Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/devicetype
Authorization: Bearer <your access token>
```

Mit dem folgenden Codebeispiel wird der Antworttext für diese Methode veranschaulicht.

```json
{
  "Data": {
    "DeviceType": {
      "710": "Windows",
      "711": "Phone"
    }
  }
}
```


<span id="supplytype-values"/>

### <a name="supply-type-values"></a>Werte für Bestandstypen

Das Feld *supplyType* im Objekt [TargetingProfile](#targeting-profile) enthält eine oder mehrere der folgenden Ganzzahlen, die den Bestandstyp angeben, in dem die Kampagnenanzeigen angezeigt werden.

|  Ganzzahliger Wert für das Feld *SupplyType*  |  Entsprechender Bestandstyp  |  Beschreibung  |
|---------------------------------|---------------------------|---------------------------|
|     11470     |  App        |  Dies bezieht sich auf Anzeigen, die ausschließlich in Apps angezeigt werden.  |
|     11471     |  Universelle        |  Dies bezieht sich auf Anzeigen, die in Apps, im Internet und auf anderen Anzeigeflächen angezeigt werden.  |

Die unterstützten Werte für das Feld *supplyType* können Sie programmgesteuert erhalten, indem Sie die folgende GET-Methode aufrufen.  Für die ```Authorization``` -Header, übergeben Sie Ihr Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/supplytype
Authorization: Bearer <your access token>
```

Mit dem folgenden Codebeispiel wird der Antworttext für diese Methode veranschaulicht.

```json
{
  "Data": {
    "SupplyType": {
      "11470": "App",
      "11471": "Universal"
    }
  }
}
```

<span id="country-code-values"/>

### <a name="country-code-values"></a>Werte für Ländercode

Das Feld *country* in dem Objekt [TargetingProfile](#targeting-profile) enthält eine oder mehrere der folgenden Ganzzahlen, die die [ISO 3166-1-Alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) Ländercodes der Zielgruppenbenutzer identifizieren.

|  Ganzzahliger Wert für das Feld *country*  |  Entsprechender Ländercode  |  
|-------------------------------------|------------------------------|
|     1      |            USA                  |
|     2      |            AU                  |
|     3      |            AT                  |
|     4      |            BE                  |
|     5      |            BR                  |
|     6      |            KA                  |
|     7      |            DK                  |
|     8      |            FI                  |
|     9      |            FR                  |
|     10      |            DE                  |
|     11      |            GR                  |
|     12      |            HK                  |
|     13      |            IN                  |
|     14      |            IE                  |
|     15      |            IT                  |
|     16      |            JP                  |
|     17      |            LU                  |
|     18      |            MX                  |
|     19      |            NL                  |
|     20      |            NZ                  |
|     21      |            NEIN                  |
|     22      |            PL                  |
|     23      |            PT                  |
|     24      |            SG                  |
|     25      |            ES                  |
|     26      |            SE                  |
|     27      |            CH                  |
|     28      |            TW                  |
|     29      |            GB                  |
|     30      |            RU                  |
|     31      |            CL                  |
|     32      |            CO                  |
|     33      |            CZ                  |
|     34      |            HU                  |
|     35      |            ZA                  |
|     36      |            KR                  |
|     37      |            CN                  |
|     38      |            RO                  |
|     39      |            TR                  |
|     40      |            SK                  |
|     41      |            IL                  |
|     42      |            ID                  |
|     43      |            AR                  |
|     44      |            MY                  |
|     45      |            PH                  |
|     46      |            PE                  |
|     47      |            UA                  |
|     48      |            AE                  |
|     49      |            TH                  |
|     50      |            IQ                  |
|     51      |            VN                  |
|     52      |            CR                  |
|     53      |            VE                  |
|     54      |            QA                  |
|     55      |            SI                  |
|     56      |            BG                  |
|     57      |            LT                  |
|     58      |            RS                  |
|     59      |            Personalabteilung                  |
|     60      |            Personalabteilung                  |
|     61      |            LV                  |
|     62      |            EE                  |
|     63      |            IS                  |
|     64      |            KZ                  |
|     65      |            SA                  |
|     67      |            AL                  |
|     68      |            DZ                  |
|     70      |            AO                  |
|     72      |            AM                  |
|     73      |            AZ                  |
|     74      |            BS                  |
|     75      |            BD                  |
|     76      |            BB                  |
|     77      |            BY                  |
|     81      |            BO                  |
|     82      |            BA                  |
|     83      |            BW                  |
|     87      |            KH                  |
|     88      |            CM                  |
|     94      |            CD                  |
|     95      |            CI                  |
|     96      |            CY                  |
|     99      |            DO                  |
|     100      |            EC                  |
|     101      |            EG                  |
|     102      |            SV                  |
|     107      |            FJ                  |
|     108      |            GA                  |
|     110      |            GE                  |
|     111      |            GH                  |
|     114      |            GT                  |
|     118      |            HT                  |
|     119      |            HN                  |
|     120      |            JM                  |
|     121      |            JO                  |
|     122      |            KE                  |
|     124      |            KW                  |
|     125      |            KG                  |
|     126      |            LA                  |
|     127      |            LB                  |
|     133      |            MK                  |
|     135      |            MW                  |
|     138      |            MT                  |
|     141      |            MU                  |
|     145      |            ME                  |
|     146      |            MA                  |
|     147      |            MZ                  |
|     148      |            Nicht verfügbar                  |
|     150      |            NP                  |
|     151      |            NI                  |
|     153      |            NG                  |
|     154      |            OM                  |
|     155      |            PK                  |
|     157      |            AA                  |
|     159      |            PY                  |
|     167      |            SN                  |
|     172      |            LK                  |
|     176      |            TZ                  |
|     180      |            TT                  |
|     181      |            TN                  |
|     184      |            UG                  |
|     185      |            UY                  |
|     186      |            UZ                  |
|     189      |            ZM                  |
|     190      |            ZW                  |
|     219      |            MD                  |
|     224      |            PS                  |
|     225      |            RE                  |
|     246      |            PR                  |

Die unterstützten Werte für das Feld *country* können Sie programmgesteuert erhalten, indem Sie die folgende GET-Methode aufrufen.  Für die ```Authorization``` -Header, übergeben Sie Ihr Azure AD-Zugriffstoken in der Form **Bearer** &lt; *token*&gt;.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/country
Authorization: Bearer <your access token>
```

Mit dem folgenden Codebeispiel wird der Antworttext für diese Methode veranschaulicht.

```json
{
  "Data": {
    "Country": {
      "1": "US",
      "2": "AU",
      "3": "AT",
      "4": "BE",
      "5": "BR",
      "6": "CA",
      "7": "DK",
      "8": "FI",
      "9": "FR",
      "10": "DE",
      "11": "GR",
      "12": "HK",
      "13": "IN",
      "14": "IE",
      "15": "IT",
      "16": "JP",
      "17": "LU",
      "18": "MX",
      "19": "NL",
      "20": "NZ",
      "21": "NO",
      "22": "PL",
      "23": "PT",
      "24": "SG",
      "25": "ES",
      "26": "SE",
      "27": "CH",
      "28": "TW",
      "29": "GB",
      "30": "RU",
      "31": "CL",
      "32": "CO",
      "33": "CZ",
      "34": "HU",
      "35": "ZA",
      "36": "KR",
      "37": "CN",
      "38": "RO",
      "39": "TR",
      "40": "SK",
      "41": "IL",
      "42": "ID",
      "43": "AR",
      "44": "MY",
      "45": "PH",
      "46": "PE",
      "47": "UA",
      "48": "AE",
      "49": "TH",
      "50": "IQ",
      "51": "VN",
      "52": "CR",
      "53": "VE",
      "54": "QA",
      "55": "SI",
      "56": "BG",
      "57": "LT",
      "58": "RS",
      "59": "HR",
      "60": "BH",
      "61": "LV",
      "62": "EE",
      "63": "IS",
      "64": "KZ",
      "65": "SA",
      "67": "AL",
      "68": "DZ",
      "70": "AO",
      "72": "AM",
      "73": "AZ",
      "74": "BS",
      "75": "BD",
      "76": "BB",
      "77": "BY",
      "81": "BO",
      "82": "BA",
      "83": "BW",
      "87": "KH",
      "88": "CM",
      "94": "CD",
      "95": "CI",
      "96": "CY",
      "99": "DO",
      "100": "EC",
      "101": "EG",
      "102": "SV",
      "107": "FJ",
      "108": "GA",
      "110": "GE",
      "111": "GH",
      "114": "GT",
      "118": "HT",
      "119": "HN",
      "120": "JM",
      "121": "JO",
      "122": "KE",
      "124": "KW",
      "125": "KG",
      "126": "LA",
      "127": "LB",
      "133": "MK",
      "135": "MW",
      "138": "MT",
      "141": "MU",
      "145": "ME",
      "146": "MA",
      "147": "MZ",
      "148": "NA",
      "150": "NP",
      "151": "NI",
      "153": "NG",
      "154": "OM",
      "155": "PK",
      "157": "PA",
      "159": "PY",
      "167": "SN",
      "172": "LK",
      "176": "TZ",
      "180": "TT",
      "181": "TN",
      "184": "UG",
      "185": "UY",
      "186": "UZ",
      "189": "ZM",
      "190": "ZW",
      "219": "MD",
      "224": "PS",
      "225": "RE",
      "246": "PR"
    }
  }
}
```

## <a name="related-topics"></a>Verwandte Themen

* [Ausführen von Ad-Kampagnen, die mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Ad-Kampagnen verwalten](manage-ad-campaigns.md)
* [Übermittlung Zeilen für Ad-Kampagnen verwalten](manage-delivery-lines-for-ad-campaigns.md)
* [Verwalten von Werbemitteln für Ad-Kampagnen](manage-creatives-for-ad-campaigns.md)
* [Ad-Kampagne-Leistungsdaten abrufen](get-ad-campaign-performance-data.md)
