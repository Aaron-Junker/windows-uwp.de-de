---
title: Fehlerbehandlung für C++-API
description: Erfahren Sie, wie Fehler behandelt werden, wenn Sie einen Aufruf des Xbox Live-Dienst mit der C++-APIs vornehmen.
ms.assetid: 10b47e68-8b1f-4023-96a4-404f3f6a9850
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, Fehlerbehandlung
ms.localizationpriority: medium
ms.openlocfilehash: 90fd816f8d44b27c1df0ded9bee6473f642478a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636525"
---
# <a name="c-api-error-handling"></a>Fehlerbehandlung für C++-API

In der C++-API, anstatt das Auslösen von Ausnahmen zurück die meisten Aufrufe Xbox_live_result < Payload_type > nach Bedarf.

## <a name="xboxliveresult-structure"></a>Xbox_live_result-Struktur
xbox_live_result has 3 items:
1. Der Fehler, die vom Vorgang zurückgegeben wird,
2. Spezifische Fehlermeldung, die für Debuggingzwecke verwendet und
3. Die Nutzlast des Ergebnisses (kann leer sein, wenn ein Fehler aufgetreten)"

Sie erhalten weitere Informationen zu Xbox_live_result als sowie welche Fehler die Codes in der Dokumentation Xbox Live sind.

Die Struktur sieht wie folgt aus:

```cpp
template<typename T>
class xbox_live_result
{
    const std::error_code& err();
    const std::string& err_message();
    T& payload();
};
```

**Err** -Fehler zurückgegeben.  Sie werden ein NULL-Verweis und kein Fehler.  Dies verhält sich wie der C++-STL Fehler ist, dass Sie das Grundwert durch Aufrufen der Value()-Methode abrufen können.  Aufruf message() erhalten Sie eine Zeichenfolgendarstellung.  Wenn der Fehlercode "Ungültiges Argument", dann bedeutet ```err().message()``` wäre, dass der Text "Ungültiges Argument".

**Err_message** -werden zu diesem Fehler.  Z. B. wenn **err** ist "Ungültiges Argument" **Err_message** Details würde auf welches Argument ungültig ist.

**Nutzlast** -relevanten Elements zurück.  Betrachten Sie beispielsweise ```xbox_live_result<achievement>``` die Sie aufrufende Get_achievement erhalten können.  In diesem Beispiel wird die Nutzlast das Erreichen der Zertifikation selbst vorliegen, (wenn keine Fehler vorhanden ist).

## <a name="example"></a>Beispiel

```cpp
// Function which returns an xbox_live_result
xbox_live_result<std::shared_ptr<title_presence_change_subscription>> presenceChangeSubscriptionResult =
xbox::services::presence::subscribe_to_title_presence_change(
    xboxUserId,
    titleId
    );

printf("Error value %d, string %s", achievementResult.err().value(), achievementResult.err().message());

// Would output:
// "0 Success" if successful
// "401 Unauthorized" if auth issue

if (!achievementResult.err())
{
  // Do things on success.  Payload will be populated if applicable.
  std::shared_ptr<title_presence_change_subscription> presenceChangeSubscription = presenceChangeSubscriptionResult->payload();

  // ...
}
else if (achievement.err() == xboxlive_error_code::http_status_403_forbidden)
{
  // Special handling for 403 errors
}
else if (achievementResult.err() == xbox_live_error_condition::auth)
{
  // Handle broad auth failures.  See below section for more info on xbox_live_error_condition
}

```

## <a name="using-xboxliveerrorcondition-to-test-against-broad-error-categories"></a>Verwenden Sie Xbox_live_error_condition für Breite Fehlerkategorien testen
Im obigen Beispiel testen wir den Fehlercode 403-Fehlern sowie so genannte ```xbox_live_error_condition::auth```.

 Wenn Xbox_live_result Fehler ()-Funktion zu verwenden, kann eine einzeln für Fehlercodes testen.  Für Klassen-400-Fehlern können Sie beispielsweise einzelne Tests und Steuerelement für flow aufweisen:

* xbox_live_error_code::http_status_400_bad_request
* xbox_live_error_code::http_status_401_unauthorized
* xbox_live_error_code::http_status_403_forbidden
* usw.

Aber dies ist normalerweise nicht was Sie möchten, und Sie vor einer Klasse von Fehlern als eine testen möchten.  Damit Sie testen können, vor einer Klasse von Fehlern verwenden die Enumerationen zur Verfügung, in der ```xbox_live_error_condition``` Klasse.  Wir implementieren eine Überladung für den Equality-Operator der Tests mit Anzahl Fehlercodes automatisieren.  Zusätzlich zu ```auth```, es gibt Kategorien wie ```rta``` und ```http```.  Die vollständige Liste finden Sie im *errors.h* oder im *xblsdk_cpp.chm*.

Für ein Video, das dies und einige andere Features der C++ Xbox-API behandelt, sehen Sie sich unsere XFest reden [Xfest 2015 Videos](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) unter *XSAPI: C++, die keine Ausnahmen.*
