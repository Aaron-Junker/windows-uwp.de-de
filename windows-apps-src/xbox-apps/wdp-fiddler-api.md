---
title: Geräteportal – API-Referenz für Fiddler
description: Erfahren Sie, wie Sie die Fiddler-Netzwerk Ablauf Verfolgung in Ihrem devkit mithilfe der Xbox-Geräte Portal-Rest-API aktivieren und deaktivieren.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: f431adae41021432dfcfca6b4e79df5237fcb283
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89163994"
---
# <a name="fiddler-settings-api-reference"></a>Fiddler-Einstellungen – API-Referenz   
Sie können die Fiddler-Netzwerkablaufverfolgung für Ihr Dev Kit mittels dieser REST-API aktivieren und deaktivieren.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Bestimmen, ob die Fiddler-Ablauf Verfolgung aktiviert ist

**Anforderung**

Mithilfe der folgenden Anforderung können Sie überprüfen, ob die Fiddler-Ablauf Verfolgung auf dem Gerät aktiviert ist.

Methode      | Anforderungs-URI
:------     | :-----
GET | /ext/fiddler


**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- JSON-boolesche Eigenschaft isproxyaktivierte, die spezifigibt, ob der Proxy aktiviert ist.

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
200 | Erfolg
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="enable-fiddler-tracing"></a>Aktivieren der Fiddler-Ablaufverfolgung

**Anforderung**

Sie können die Fiddler-Ablaufverfolgung mittels der folgenden Anforderung für das Dev Kit verwenden.  Beachten Sie, dass das Gerät neu gestartet werden muss, bevor dies wirksam wird.

Methode      | Anforderungs-URI
:------     | :-----
POST | /ext/fiddler

**URI-Parameter**

Sie können im Anforderungs-URI die folgenden zusätzlichen Parameter angeben:

| URI-Parameter      | BESCHREIBUNG     | 
| ------------------ |-----------------|
| proxyAddress       | Die IP-Adresse oder der Hostnamen des Geräts, auf dem Fiddler ausgeführt wird. |
| proxyPort          | Der Port, den Fiddler für die Überwachung des Datenverkehrs verwendet. Der Standardport ist 8888. |
| updateCert (optional)| Ein boolescher Wert, der angibt, ob das Fiddler-Stammzertifikat angegeben wird. Dieser Wert muss true sein, wenn Fiddler nie zuvor für dieses Dev Kit oder für einen anderen Host konfiguriert wurde.  |


**Anforderungsheader**

- Keine

**Anforderungstext**

- Keiner, wenn updateCert false ist oder nicht angegeben wird. Andernfalls mehrteiliger konformer http-Text, der die Datei FiddlerRoot.cer enthält.

**Antwort**   

- Keine  

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
204 | Die Anforderung für die Aktivierung von Fiddler wurde akzeptiert. Fiddler wird aktiviert, wenn das Gerät das nächste Mal neu gestartet wird.
4XX | Fehlercodes
5XX | Fehlercodes

## <a name="disable-fiddler-tracing-on-the-devkit"></a>Deaktivieren Sie die Fiddler-Ablaufverfolgung für das Dev Kit.

**Anforderung**

Sie können die Fiddler-Ablaufverfolgung mithilfe der folgenden Anforderung für das Gerät deaktivieren. Beachten Sie, dass das Gerät neu gestartet werden muss, bevor dies wirksam wird.

Methode      | Anforderungs-URI
:------     | :-----
Delete | /ext/fiddler

**URI-Parameter**

- Keine

**Anforderungsheader**

- Keine

**Anforderungstext**   

- Keine

**Antwort**   

- Keine 

**Statuscode**

Diese API hat die folgenden erwarteten Statuscodes:

HTTP-Statuscode      | BESCHREIBUNG
:------     | :-----
204 | Die Anforderung zum Deaktivieren der Fiddler-Ablaufverfolgung war erfolgreich. Die Nachverfolgung wird deaktiviert, wenn das Gerät das nächste Mal neu gestartet wird.
4XX | Fehlercodes
5XX | Fehlercodes


**Verfügbare Gerätefamilien**

* Windows Xbox

## <a name="see-also"></a>Weitere Informationen
- [Konfigurieren von Fiddler für UWP auf Xbox](uwp-fiddler.md)

