---
title: Device Portal - Referenz zur API für Xbox-Entwicklereinstellungen
description: Erfahren Sie, wie Sie mit der Xbox-Geräte Portal-Rest-API auf Xbox One-Einstellungen zugreifen, die für die Entwicklung nützlich sind.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: 6ab12b99-2944-49c9-92d9-f995efc4f6ce
ms.localizationpriority: medium
ms.openlocfilehash: 0aceb7afdce9cc76eab3ee330f0018fdc7ccd1bb
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168984"
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

**Auf**   
Die Antwort ist ein JSON-Einstellungsarray mit allen Einstellungen. Jedes Einstellungsobjekt enthält die folgenden Felder:

* Name (Zeichenfolge): Der Name der Einstellung.
* Value (Zeichenfolge): Der Wert der Einstellung.
* RequiresReboot („Yes“|„No“): Dieses Feld gibt an, ob ein Neustart erforderlich ist, damit die Einstellung wirksam wird.
* Deaktiviert-("Ja" | "No") dieses Feld gibt an, ob die Einstellung deaktiviert ist und nicht bearbeitet werden kann.
* Category-(Zeichenfolge) die Kategorie der Einstellung.
* Type-("Text" | "Number" | "Bool" | "Select") dieses Feld gibt an, welcher Typ eine Einstellung ist: Texteingabe, ein boolescher Wert ("true" oder "false"), eine Zahl mit einem minimal-und Maximalwert, oder wählen Sie eine bestimmte Werteliste aus.

Wenn die Einstellung eine Zahl ist:

* Min-(Zahl) dieses Feld gibt den minimalen numerischen Wert der Einstellung an.
* Max-(Zahl) dieses Feld gibt den maximalen numerischen Wert der Einstellung an.

Wenn die Einstellung ist, wählen Sie Folgendes aus:

* Optionsvariable-("Ja" | "No") dieses Feld gibt an, ob die Einstellungsoptionen variabel sind, wenn sich die gültigen Optionen ohne einen Neustart ändern können.
* Optionen: JSON-Array, das die gültigen SELECT-Optionen als Zeichen folgen enthält.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
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
GET | /ext/settings/\<setting name\>

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**

- Keine

**Auf**   
Die Antwort ist ein JSON-Objekt mit folgenden Feldern:

* Name (Zeichenfolge): Der Name der Einstellung.
* Value (Zeichenfolge): Der Wert der Einstellung.
* RequiresReboot („Yes“|„No“): Dieses Feld gibt an, ob ein Neustart erforderlich ist, damit die Einstellung wirksam wird.
* Deaktiviert-("Ja" | "No") dieses Feld gibt an, ob die Einstellung deaktiviert ist und nicht bearbeitet werden kann.
* Category-(Zeichenfolge) die Kategorie der Einstellung.
* Type-("Text" | "Number" | "Bool" | "Select") dieses Feld gibt an, welcher Typ eine Einstellung ist: Texteingabe, ein boolescher Wert ("true" oder "false"), eine Zahl mit einem minimal-und Maximalwert, oder wählen Sie eine bestimmte Werteliste aus.

Wenn die Einstellung eine Zahl ist:

* Min-(Zahl) dieses Feld gibt den minimalen numerischen Wert der Einstellung an.
* Max-(Zahl) dieses Feld gibt den maximalen numerischen Wert der Einstellung an.

Wenn die Einstellung ist, wählen Sie Folgendes aus:

* Optionsvariable-("Ja" | "No") dieses Feld gibt an, ob die Einstellungsoptionen variabel sind, wenn sich die gültigen Optionen ohne einen Neustart ändern können.
* Optionen: JSON-Array, das die gültigen SELECT-Optionen als Zeichen folgen enthält.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
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
PUT | /ext/settings/\<setting name\>

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungs Text**   
Der Anforderungstext ist ein JSON-Objekt mit dem folgenden Feld:   
Value (Zeichenfolge): Der neue Wert für die Einstellung.

**Antwort**   

- Keine

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
200 | Die Anforderung war erfolgreich.
4XX | Fehlercodes
5XX | Fehlercodes

**Verfügbare Gerätefamilien**

* Windows Xbox