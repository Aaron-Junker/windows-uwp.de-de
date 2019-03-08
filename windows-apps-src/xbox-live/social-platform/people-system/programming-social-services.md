---
title: Programmieren von Services soziale Netzwerke
description: Enthält ein Codebeispiel zur Verwendung der Xbox Live Social-Manager-API.
ms.assetid: 101d059a-e03f-472c-8300-800aa5730ee2
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, social-Manager, Beispiel
ms.localizationpriority: medium
ms.openlocfilehash: 5039d9ed205cadfee3b2e64527a1f58f1624accc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57592055"
---
# <a name="programming-social-services"></a>Programmieren von Services soziale Netzwerke

> [!NOTE]
> Dieser Artikel beschreibt erweiterte API-Nutzung.  Als Ausgangspunkt können Sie sehen Sie sich die [Einführung in die Social-Manager-API](../intro-to-social-manager.md) der erheblich vereinfacht die Entwicklung.  Informieren Sie Ihre DAM wissen, ob Sie ein nicht unterstütztes Szenario im US-Sozialversicherungsnummern-Manager finden.

Im folgenden Codebeispiel wird veranschaulicht, wie eine social-Beziehung mit der Xbox Live abgerufen wird. Generiert eine Liste aller Benutzer im System und ruft das erste Abonnement. Als Nächstes ruft alle sozialen Beziehungen des Benutzers ab. Schließlich werden die öffentlichen Eigenschaften der einzelnen Beziehungen.

```cpp
XboxLiveContext^ xboxLiveContext = NULL;

// An XboxLiveContext for a user should be created only once, or you may encounter unpredictable behavior.
void OneTimeInit()
{
  // Get the XboxLiveContext.  This should only be done once per user after signing in.
  xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));
}

void Example_SocialService_GetSocialRelationshipsAsync()
{
    // Generate a list of users on the system.
    // Create an XboxLiveContext from user 0 (the first one returned).
    // This user's authentication token will be used for service requests.
    XboxLiveContext^ xboxLiveContext = ref new Microsoft::Xbox::Services::XboxLiveContext(User::Users->GetAt(0));

    // Asynchronously retrieve all social relationships from that context.
    auto asyncOp = xboxLiveContext->SocialService->GetSocialRelationshipsAsync();

    create_task( asyncOp )
    .then([](XboxSocialRelationshipResult^ result)
    {
        // For each social relationship of the specified user...
        for( XboxSocialRelationship^ xboxSocialRelationship : result->Items )
        {
            // ...display the public properties of the relationship.
            LogComment( xboxSocialRelationship->XboxUserId );
            LogComment( xboxSocialRelationship->IsFavorite.ToString() );
        }

    })
    .wait();
}
```
