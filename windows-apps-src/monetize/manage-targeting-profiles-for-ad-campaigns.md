---
ms.assetid: d305746a-d370-4404-8cde-c85765bf3578
description: Verwenden Sie diese Methode in der Microsoft Store promotionapi, um Ziel Profile für Werbekampagnen zu verwalten.
title: Verwalten von Zielgruppenprofilen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store promotionapi, Werbekampagnen
ms.localizationpriority: medium
ms.openlocfilehash: 10a10e15c736d77e132543a54dff11455f08cde3
ms.sourcegitcommit: 80ea62d6c0ee25d73750437fe1e37df5224d5797
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/26/2021
ms.locfileid: "105619516"
---
# <a name="manage-targeting-profiles"></a>Verwalten von Zielgruppenprofilen


Verwenden Sie diese Methoden in der Microsoft Store promotionapi, um die Benutzer, Regionen und Inventur Typen auszuwählen, die Sie für jede Übermittlungs Zeile in einer Werbe-Werbekampagne als Ziel verwenden möchten. Ziel Profile können über mehrere Übermittlungs Zeilen erstellt und wieder verwendet werden.

Weitere Informationen über die Beziehung Zwischenziel Profilen und Ad-Kampagnen, Übermittlungs Zeilen und schöpfenden finden [Sie unter Ausführen von Ad-Kampagnen mit Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md#call-the-windows-store-promotions-api).

## <a name="prerequisites"></a>Voraussetzungen

Um diese Methoden zu verwenden, müssen Sie zunächst die folgenden Schritte ausführen:

* Wenn Sie dies noch nicht getan haben, müssen Sie alle [Voraussetzungen](run-ad-campaigns-using-windows-store-services.md#prerequisites) für die Microsoft Store promotionapi erfüllen.
* Rufen Sie [ein Azure AD Zugriffs Token](run-ad-campaigns-using-windows-store-services.md#obtain-an-azure-ad-access-token) ab, das im-Anforderungs Header für diese Methoden verwendet werden soll. Nachdem Sie ein Zugriffstoken erhalten haben, haben Sie 60 Minuten Zeit, es zu verwenden, bevor es abläuft. Wenn das Token abgelaufen ist, können Sie ein neues abrufen.

## <a name="request"></a>Anforderung

Diese Methoden verfügen über die folgenden URIs.

| Methodentyp | Anforderungs-URI                                                      |  BESCHREIBUNG  |
|--------|------------------------------------------------------------------|---------------|
| POST   | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile``` |  Erstellt ein neues Ziel Profil.  |
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Bearbeitet das von *targetingprofileid* angegebene Ziel Profil.  |
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/{targetingProfileId}``` |  Ruft das von *targetingprofileid* angegebene Ziel Profil ab.  |


### <a name="header"></a>Header

| Header        | type   | BESCHREIBUNG         |
|---------------|--------|---------------------|
| Authorization | Zeichenfolge | Erforderlich. Das Azure AD-Zugriffstoken im Format **Bearer** &lt;*token*&gt;. |
| Nachverfolgungs-ID   | GUID   | Optional. Eine ID, die den aufrufflow nachverfolgt.                                  |


### <a name="request-body"></a>Anforderungstext

Die Post-und Put-Methoden erfordern einen JSON-Anforderungs Text mit den erforderlichen Feldern eines [Ziel Profil](#targeting-profile) Objekts und zusätzlicher Felder, die Sie festlegen oder ändern möchten.


### <a name="request-examples"></a>Beispiele für Anforderungen

Im folgenden Beispiel wird veranschaulicht, wie die Post-Methode aufgerufen wird, um ein Ziel Profil zu erstellen.

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

Im folgenden Beispiel wird veranschaulicht, wie Sie die Get-Methode aufrufen, um ein Ziel Profil abzurufen.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/targeting-profile/310023951  HTTP/1.1
Authorization: Bearer <your access token>
```

<span/>

## <a name="response"></a>Antwort

Diese Methoden geben einen JSON-Antworttext mit einem [Ziel Profil](#targeting-profile) Objekt zurück, das Informationen zum Profil Erstellungs Profil enthält, das erstellt, aktualisiert oder abgerufen wurde. Das folgende Beispiel zeigt einen Antworttext für diese Methoden.

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

## <a name="targeting-profile-object"></a>Profil Objekt für Ziel

Die Anforderungs-und Antwort Texte für diese Methoden enthalten die folgenden Felder. Diese Tabelle zeigt, welche Felder schreibgeschützt sind (was bedeutet, dass Sie in der Put-Methode nicht geändert werden können) und welche Felder im Anforderungs Text für die Post-Methode erforderlich sind.

| Feld        | type   |  BESCHREIBUNG      |  Nur Lesezugriff  | Standard  | Für Post erforderlich |  
|--------------|--------|---------------|------|-------------|------------|
|  id   |  integer   |  Die ID des Ziel Profils.     |   Ja    |       |   Nein      |       
|  name   |  Zeichenfolge   |   Der Name des Ziel Profils.    |    Nein   |      |  Ja     |       
|  targetingType   |  Zeichenfolge   |  Einer der folgenden Werte: <ul><li>**Auto**: Geben Sie diesen Wert an, damit Microsoft das Ziel Profil basierend auf den Einstellungen für Ihre APP im Partner Center auswählen kann.</li><li>**Manuell**: Geben Sie diesen Wert an, um Ihr eigenes Ziel Profil zu definieren.</li></ul>     |  Nein     |  Auto    |   Yes    |       
|  age   |  array   |   Eine oder mehrere ganze Zahlen, die die Alters Bereiche der Ziel Benutzer identifizieren. Eine vollständige Liste mit ganzen Zahlen finden Sie in diesem Artikel unter [Alters Werte](#age-values) .    |    Nein    |  NULL    |     Nein    |       
|  gender   |  array   |  Eine oder mehrere ganze Zahlen, die die genderer der Ziel Benutzer identifizieren. Eine umfassende Liste mit ganzen Zahlen finden Sie in diesem Artikel unter " [Geschlecht Values](#gender-values) ".       |  Nein    |  NULL    |     Nein    |       
|  country   |  array   |  Eine oder mehrere ganze Zahlen, die die Ländercodes der Ziel Benutzer identifizieren. Eine umfassende Liste mit ganzen Zahlen finden Sie unter [Ländercode Werte](#country-code-values) in diesem Artikel.    |  Nein    |  NULL   |      Nein   |       
|  osVersion   |  array   |   Eine oder mehrere ganze Zahlen, die die Betriebssystemversionen der Ziel Benutzer identifizieren. Eine vollständige Liste mit ganzen Zahlen finden Sie in diesem Artikel unter [Werte der Betriebssystemversion](#osversion-values) .     |   Nein    |  NULL   |     Nein    |       
|  deviceType   | array    |  Eine oder mehrere ganze Zahlen, die die Gerätetypen der Ziel Benutzer identifizieren. Eine umfassende Liste mit ganzen Zahlen finden Sie in diesem Artikel unter [Gerätetyp Werte](#devicetype-values) .       |   Nein    |  NULL    |    Nein     |       
|  supplytype   |  array   |  Mindestens ein Integer-Wert, der den Typ des Bestands identifiziert, in dem die Werbeeinblendungen angezeigt werden. Eine umfassende Liste mit ganzen Zahlen finden Sie in diesem Artikel unter [Angeben von Typwerten](#supplytype-values) .      |   Nein    |  NULL   |     Nein    |


<span id="age-values"/>

### <a name="age-values"></a>Alters Werte

Das Feld " *Age* " im Objekt " [targetingprofile](#targeting-profile) " enthält mindestens einen der folgenden Ganzzahlen, die die Alters Bereiche der Ziel Benutzer identifizieren.

|  Ganzzahliger Wert für *Alters* Feld  |  Entsprechender Altersbereich  |  
|---------------------------------|---------------------------|
|     651     |            13 bis 17             |
|     652     |           18 bis 24             |
|     653     |            25 bis 34             |
|     654     |            35 bis 49             |
|     655     |            50 und höher             |

Wenn Sie die unterstützten Werte für das Feld " *Age* " Programm gesteuert abrufen möchten, können Sie die folgende Get-Methode abrufen.  Übergeben Sie für den- ```Authorization``` Header das Azure AD Zugriffs Token im Formular  &lt; *bearertoken* &gt; .

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/age
Authorization: Bearer <your access token>
```

Das folgende Beispiel zeigt den Antworttext für diese Methode.

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

### <a name="gender-values"></a>Geschlechts Werte

Das Feld *Geschlecht* im [targetingprofile](#targeting-profile) -Objekt enthält mindestens eine der folgenden Ganzzahlen, die die genderer der Ziel Benutzer identifizieren.

|  Ganzzahliger Wert für das Feld *Geschlecht*  |  Entsprechendes Geschlecht  |  
|---------------------------------|---------------------------|
|     700     |            Male             |
|     701     |           Female             |

Wenn Sie die unterstützten Werte für das Feld *Geschlecht* Programm gesteuert abrufen möchten, können Sie die folgende Get-Methode abrufen.  Übergeben Sie für den- ```Authorization``` Header das Azure AD Zugriffs Token im Formular  &lt; *bearertoken* &gt; .

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/gender
Authorization: Bearer <your access token>
```

Das folgende Beispiel zeigt den Antworttext für diese Methode.

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

### <a name="os-version-values"></a>Betriebssystem-Versions Werte

Das *OSVersion* -Feld im [targetingprofile](#targeting-profile) -Objekt enthält mindestens eine der folgenden Ganzzahlen, die die Betriebssystemversionen der Ziel Benutzer identifizieren.

|  Ganzzahliger Wert für *OSVersion* -Feld  |  Zugehörige Betriebssystemversion  |  
|---------------------------------|---------------------------|
|     500     |            Windows Phone 7             |
|     501     |           Windows Phone 7.1             |
|     502     |           Windows Phone 7.5             |
|     503     |           Windows Phone 7,8             |
|     504     |           Windows Phone 8.0             |
|     505     |           Windows Phone 8.1             |
|     506     |           Windows 8.0             |
|     507     |           Windows 8.1             |
|     508     |           Windows 10             |
|     509     |           Windows 10 Mobile             |

Wenn Sie die unterstützten Werte für das Feld *OSVersion* Programm gesteuert abrufen möchten, können Sie die folgende Get-Methode abrufen.  Übergeben Sie für den- ```Authorization``` Header das Azure AD Zugriffs Token im Formular  &lt; *bearertoken* &gt; .

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/osversion
Authorization: Bearer <your access token>
```

Das folgende Beispiel zeigt den Antworttext für diese Methode.

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

### <a name="device-type-values"></a>Gerätetyp Werte

Das Feld " *DeviceType* " im Objekt " [targetingprofile](#targeting-profile) " enthält mindestens einen der folgenden Ganzzahlen, die die Gerätetypen der Ziel Benutzer identifizieren.

|  Ganzzahliger Wert für das *DeviceType* -Feld  |  Entsprechender Gerätetyp  |  BESCHREIBUNG  |
|---------------------------------|---------------------------|---------------------------|
|     710     |  Windows   |  Dabei handelt es sich um Geräte, auf denen eine Desktop Version von Windows 10 oder Windows 8. x ausgeführt wird.  |
|     711     |  Phone     |  Dabei handelt es sich um Geräte, auf denen Windows 10 Mobile, Windows Phone 8. x oder Windows Phone 7. x ausgeführt wird.

Wenn Sie die unterstützten Werte für das Feld *DeviceType* Programm gesteuert abrufen möchten, können Sie die folgende Get-Methode abrufen.  Übergeben Sie für den- ```Authorization``` Header das Azure AD Zugriffs Token im Formular  &lt; *bearertoken* &gt; .

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/devicetype
Authorization: Bearer <your access token>
```

Das folgende Beispiel zeigt den Antworttext für diese Methode.

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

### <a name="supply-type-values"></a>Typwerte angeben

Das Feld *supplytype* im [targetingprofile](#targeting-profile) -Objekt enthält mindestens eine der folgenden Ganzzahlen, die den Typ des Inventars identifizieren, in dem die Werbeeinblendungen angezeigt werden.

|  Ganzzahliger Wert für *supplytype* -Feld  |  Entsprechender Typ der Bereitstellung  |  BESCHREIBUNG  |
|---------------------------------|---------------------------|---------------------------|
|     11470     |  App        |  Dies bezieht sich auf anzeigen, die nur in-apps angezeigt werden.  |
|     11471     |  Universell        |  Dies bezieht sich auf Werbeeinblendungen, die in-apps, im Web und auf anderen Anzeige Oberflächen angezeigt werden.  |

Wenn Sie die unterstützten Werte für das Feld *supplytype* Programm gesteuert abrufen möchten, können Sie die folgende Get-Methode abrufen.  Übergeben Sie für den- ```Authorization``` Header das Azure AD Zugriffs Token im Formular  &lt; *bearertoken* &gt; .

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/supplytype
Authorization: Bearer <your access token>
```

Das folgende Beispiel zeigt den Antworttext für diese Methode.

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

### <a name="country-code-values"></a>Ländercode Werte

Das Feld *Country* im [targetingprofile](#targeting-profile) -Objekt enthält mindestens eine der folgenden Ganzzahlen, die die [ISO 3166-1 Alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2) -Ländercodes der Ziel Benutzer identifizieren.

|  Ganzzahliger Wert für *Country* -Feld  |  Entsprechender Ländercode  |  
|-------------------------------------|------------------------------|
|     1      |            US                  |
|     2      |            AU                  |
|     3      |            AT                  |
|     4      |            BE                  |
|     5      |            BR                  |
|     6      |            CA                  |
|     7      |            DK                  |
|     8      |            FI                  |
|     9      |            BV                  |
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
|     21      |            Nein                  |
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
|     41      |            BY                  |
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
|     59      |            HR                  |
|     60      |            HR                  |
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
|     108      |            Allgemein verfügbar                  |
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
|     146      |            NI                  |
|     147      |            MZ                  |
|     148      |            Nicht verfügbar                  |
|     150      |            NP                  |
|     151      |            NI                  |
|     153      |            NG                  |
|     154      |            OM                  |
|     155      |            PK                  |
|     157      |            PA                  |
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

Wenn Sie die unterstützten Werte für das Feld *Country* Programm gesteuert abrufen möchten, können Sie die folgende Get-Methode abrufen.  Übergeben Sie für den- ```Authorization``` Header das Azure AD Zugriffs Token im Formular  &lt; *bearertoken* &gt; .

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/promotion/reference/country
Authorization: Bearer <your access token>
```

Das folgende Beispiel zeigt den Antworttext für diese Methode.

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

## <a name="related-topics"></a>Zugehörige Themen

* [Ausführen von Ad-Kampagnen mithilfe von Microsoft Store Services](run-ad-campaigns-using-windows-store-services.md)
* [Verwalten von Anzeigenkampagnen](manage-ad-campaigns.md)
* [Verwalten von Übermittlungs Zeilen für Werbekampagnen](manage-delivery-lines-for-ad-campaigns.md)
* [Verwalten von kreativen für Werbekampagnen](manage-creatives-for-ad-campaigns.md)
* [Abrufen der Leistungsdaten einer Anzeigenkampagne](get-ad-campaign-performance-data.md)
