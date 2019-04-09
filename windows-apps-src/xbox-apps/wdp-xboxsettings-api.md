---
title: Device Portal - Referenz zur API für Xbox-Entwicklereinstellungen
description: Erfahren Sie, wie Sie auf Xbox-Entwicklereinstellungen zugreifen.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 54a15be26adf0da97105f15f3a44f26ee7bfc96d
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240038"
---
# <a name="developer-settings-api-reference"></a>Referenz zur API für Entwicklereinstellungen

Mit dieser API können Sie auf Xbox One-Einstellungen zugreifen, die für die Entwicklung nützlich sind.

## <a name="get-all-developer-settings-at-once"></a>Gleichzeitiges Abrufen aller Entwicklereinstellungen

**Anforderung**

Mithilfe der folgenden Anforderung können Sie alle Entwicklereinstellungen in einer einzigen Anforderung abrufen.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/settings

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**   
Die Antwort ist ein JSON-Einstellungsarray mit allen Einstellungen. Jedes Einstellungsobjekt enthält die folgenden Felder:

* Name (Zeichenfolge): Der Name der Einstellung.
* Value (Zeichenfolge): Der Wert der Einstellung.
* RequiresReboot („Yes“|„No“): Dieses Feld gibt an, ob ein Neustart erforderlich ist, damit die Einstellung wirksam wird.
* Disabled – („Yes“ | „No“): Dieses Feld gibt an, ob die Einstellung deaktiviert ist und nicht bearbeitet werden kann.
* Category (String): Die Kategorie der Einstellung.
* Type („Text“ | „Number“ | „Bool“ | „Select“): Dieses Feld gibt den Einstellungstyp an – Texteingabe, ein boolescher Wert („true“ oder „false“), eine Zahl mit einem Mindestwert oder Maximalwert oder Auswahl aus einer bestimmten Liste mit Werten.

Wenn die Einstellung auf eine Zahl ist:

* Min - (Anzahl) dieses Feld gibt den minimalen numerischen Wert der Einstellung an.
* Max: (Anzahl) dieses Feld gibt den maximalen numerischen Wert der Einstellung an.

Wenn die Einstellung zu wählen:

* OptionsVariable - ("Yes" | "No") dieses Feld gibt an, ob die Festlegen von Optionen Variablen sind, wenn die gültigen Optionen, ohne einen Neustart des Computers ändern können.
* Options: JSON-Array mit den gültigen Auswahloptionen als Zeichenfolgen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="get-settings-one-at-a-time"></a>Abrufen einzelner Einstellungen

Einstellungen können auch einzeln abgerufen werden.

**Anforderung**

Mit der folgenden Anforderung können Sie Informationen zu einer einzelnen Einstellung abrufen.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/Settings/\<Einstellungsname\>

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Antwort**   
Die Antwort ist ein JSON-Objekt mit folgenden Feldern:

* Name (Zeichenfolge): Der Name der Einstellung.
* Value (Zeichenfolge): Der Wert der Einstellung.
* RequiresReboot („Yes“|„No“): Dieses Feld gibt an, ob ein Neustart erforderlich ist, damit die Einstellung wirksam wird.
* Disabled – („Yes“ | „No“): Dieses Feld gibt an, ob die Einstellung deaktiviert ist und nicht bearbeitet werden kann.
* Category (String): Die Kategorie der Einstellung.
* Type („Text“ | „Number“ | „Bool“ | „Select“): Dieses Feld gibt den Einstellungstyp an – Texteingabe, ein boolescher Wert („true“ oder „false“), eine Zahl mit einem Mindestwert oder Maximalwert oder Auswahl aus einer bestimmten Liste mit Werten.

Wenn die Einstellung auf eine Zahl ist:

* Min - (Anzahl) dieses Feld gibt den minimalen numerischen Wert der Einstellung an.
* Max: (Anzahl) dieses Feld gibt den maximalen numerischen Wert der Einstellung an.

Wenn die Einstellung zu wählen:

* OptionsVariable - ("Yes" | "No") dieses Feld gibt an, ob die Festlegen von Optionen Variablen sind, wenn die gültigen Optionen, ohne einen Neustart des Computers ändern können.
* Options: JSON-Array mit den gültigen Auswahloptionen als Zeichenfolgen.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="set-the-value-of-a-setting"></a>Festlegen des Werts einer Einstellung

Sie können den Wert einer Einstellung festlegen.

**Anforderung**

Mit der folgenden Anforderung können Sie den Wert für eine Einstellung festlegen.

Methode      | Anforderungs-URI
:------     | :-----
PUT | /ext/Settings/\<Einstellungsname\>

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   
Der Anforderungstext ist ein JSON-Objekt mit dem folgenden Feld:   
Value (Zeichenfolge): Der neue Wert für die Einstellung.

**Antwort**   

- Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | Beschreibung
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

**Verfügbare Gerätefamilien**

* Windows Xbox