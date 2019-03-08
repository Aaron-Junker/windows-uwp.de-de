---
title: XboxServices.config
description: Beschreibt die XboxServices.config-Datei für das Zuordnen Ihrer UWP-Spiels mit einer Konfiguration für die Xbox Live.
ms.date: 03/29/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Dienstkonfiguration, xboxservices.config
ms.localizationpriority: medium
ms.openlocfilehash: 8ff538d691627bf4bb12b3ef6f8b1360e59ac701
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57606685"
---
# <a name="xboxservicesconfig-file-description"></a>DateiBeschreibung XboxServices.config

Bei der Entwicklung einer Xbox Live UWP-Spiel aktiviert ist, das Projekt muss eine XboxServices.config-Datei enthalten.  Diese Datei können das Xbox Live SDK, Ihr Spiel Ihre Partner Center-app und Ihre Xbox Live Services-Konfiguration zugeordnet werden soll. Diese Datei enthält ein JSON-Objekt diese Detailinformationen, z. B. die Dienst-Konfigurations-ID, Titel-ID usw. an.

Wenn Sie ein Spiel Xbox Live Creators-Programm zu entwerfen, mit der Xbox Live-Plug-in Unity verwenden, wird diese Datei automatisch für Sie durch den Xbox Live-Zuordnung-Assistenten erstellt.

## <a name="xboxservicesconfig-fields"></a>XboxServices.config-Felder

>[!NOTE]
> Die von der Xbox Live-Assistenten erstellte Datei kann zusätzliche Felder außer den hier unten enthalten, aber sie werden nicht vom Dienst verwendet.

Die folgenden Felder werden im JSON-Objekt in der Konfigurationsdatei definiert:

Feld | Beschreibung
--- | ---
PrimaryServiceConfigId  |  Der Xbox Live-Dienstkonfiguration ID (SCID). In [Partner Center](https://partner.microsoft.com/dashboard), Sie finden diesen Wert auf die **Xbox Live** Seite (für Creators-Programm) oder **Xbox Live-Setup** Seite (für vollständige Xbox Live Spiele), unter dem **Services** Abschnitt für Ihre app.
TitleId  |  Der Dezimalzahl Title-ID für Ihre app. In [Partner Center](https://partner.microsoft.com/dashboard), Sie finden diesen Wert auf die **Xbox Live** Seite (für Creators-Programm) oder **Xbox Live-Setup** Seite (für vollständige Xbox Live Spiele), unter dem **Services** Abschnitt für Ihre app.
XboxLiveCreatorsTitle  |  Bei "true" gibt an, dass die app eine app für die Xbox Live Creators-Programm. Andernfalls "False".
Bereich  |  **(Optional)**  Definiert den Umfang der Funktionalität, die von der app verwendet. Beschreibung finden Sie unten.

### <a name="scope-field"></a>Feld "Scope"

Die **Bereich** Feld ist ein optionales Feld, das Sie verwenden können, um die Funktion verwendet werden, indem Sie Ihr Spiel anzugeben.


Wenn die **Bereich** Feld nicht angegeben ist, und der Gültigkeitsbereich auf einen Standardwert an, von denen abhängig von den Wert der festgelegt wird die **XboxLiveCreatorsTitle** Feld, wie in der folgenden Tabelle beschrieben:

XboxLiveCreatorsTitle Wert | Standardwert für Bereich
--- | ---
„true“  |  "xbl.signin xbl.friends"
„false“  |  "xboxlive.signin"



Die folgende Liste beschreibt die gültigen Werte für die **Bereich** Feld.

Bereichswert | Beschreibung
--- | ---
xbl.signin  | Enthält die Zeichen an der Funktionalität für Creators-Programm-Spiele. Erforderlich für Creators-Programm-Spiele.
xbl.friends | Enthält die Freunde und soziale Bestenlisten-Funktionalität für Creators-Programm-Spiele.
xboxlive.signin | Enthält die Funktionalität für Spiele, die Zugriff auf die vollständigen Funktionen von Xbox Live. Erforderlich für nicht - Creators-Programm-Spiele.

Zurzeit der einzige Grund, geben Sie die **Bereich** Feld ist, wenn Sie eine Xbox Live Creators-Programm Spiel nutzen, und Ihr Spiel muss nicht den Zugriff auf Freundeslisten oder soziale Bestenlisten (Bestenlisten die Ihre Freunde zugeordnet sind). Wenn dies der Fall ist, können Sie die folgende Zeile Ihrer XboxServices.config-Datei hinzufügen:

```
  "Scope" : "xbl.signin"
```

Diese Zeile wird verhindert, dass die UWP-app Anforderung der Zugriffsberechtigung für die Friend-Listen zugreifen, wenn Sie die app zum ersten Mal starten.

## <a name="example-xboxservicesconfig-file"></a>XboxServices.config-Beispieldatei

```
{
  "PrimaryServiceConfigId": "00000000-0000-0000-0000-000064382e34",
  "TitleId": 9039138423,
  "XboxLiveCreatorsTitle": true,
  "Scope" : "xbl.signin xbl.friends"
}
```
