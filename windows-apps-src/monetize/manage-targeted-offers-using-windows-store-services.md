---
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Verwenden Sie die API für das Microsoft Store gezielte Angebote, um gezielte Angebote zu erhalten, die für den aktuellen Benutzer Ihrer app verfügbar sind.
title: Verwalten von gezielten angeboten mithilfe von Store Services
ms.date: 10/10/2017
ms.topic: article
keywords: Windows 10, UWP, Store Services, Microsoft Store gezielte Angebote API, gezielte Angebote
ms.localizationpriority: medium
ms.openlocfilehash: 836ef99f827eba52699663d4a24ea58598fe3400
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363843"
---
# <a name="manage-targeted-offers-using-store-services"></a>Verwalten von gezielten angeboten mithilfe von Store Services

Wenn Sie ein *Ziel Angebot* auf der Seite **> gezielte Angebote einbinden** für Ihre APP in Partner Center erstellen, verwenden Sie die *Microsoft Store zielangebote-API* im Code Ihrer APP zum Abrufen von Informationen, die Ihnen bei der Implementierung der in-App-Funktion für das Ziel Angebot helfen. Weitere Informationen zu gezielten angeboten und deren Erstellung im Dashboard finden Sie [unter Verwenden von zielgerichteten angeboten zum Maximieren von Engagement und Konvertierungen](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

Die zielangebote-API ist eine einfache Rest-API, mit der Sie die für den aktuellen Benutzer verfügbaren Ziel Angebote erhalten können, je nachdem, ob der Benutzer Teil des Kunden Segments für das Ziel Angebot ist. Um diese API im Code Ihrer APP zu verwenden, führen Sie die folgenden Schritte aus:

1.  Fordern [Sie ein Microsoft-Konto Token](#obtain-a-microsoft-account-token) für den aktuell angemeldeten Benutzer Ihrer APP an.
2.  [Holen Sie sich die Ziel Angebote für den aktuellen Benutzer](#get-targeted-offers).
3.  Implementieren Sie die in-App-Käufe für das Add-on, das einem der Ziel Angebote zugeordnet ist. Weitere Informationen zum Implementieren von in-App-Käufen finden Sie in [diesem Artikel](enable-in-app-purchases-of-apps-and-add-ons.md).

Ein vollständiges Codebeispiel, das alle diese Schritte veranschaulicht, finden Sie im [Codebeispiel](#code-example) am Ende dieses Artikels. In den folgenden Abschnitten finden Sie weitere Informationen zu den einzelnen Schritten.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Ein Microsoft-Konto Token für den aktuellen Benutzer erhalten

Fordern Sie im Code Ihrer APP ein Microsoft-Konto (MSA)-Token für den aktuell angemeldeten Benutzer an. Sie müssen dieses Token im ```Authorization``` Anforderungs Header für die Microsoft Store zielgerichteten Angebote-API übergeben. Dieses Token wird vom Speicher verwendet, um die Ziel Angebote abzurufen, die für den aktuellen Benutzer verfügbar sind.

Um das MSA-Token zu erhalten, verwenden Sie die [webauthenticationcoremanager](/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager) -Klasse, um ein Token mit dem Bereich anzufordern ```devcenter_implicit.basic,wl.basic``` . Im folgenden Beispiel wird die dafür erforderliche Vorgehensweise veranschaulicht. Dieses Beispiel ist ein Auszug aus dem [gesamten Beispiel](#code-example)und erfordert die **Verwendung** von-Anweisungen, die im gesamten Beispiel bereitgestellt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetMSAToken":::

Weitere Informationen zum erhalten von MSA-Token finden Sie unter [Webkonto-Manager](../security/web-account-manager.md).

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>Die Ziel Angebote für den aktuellen Benutzer erhalten

Nachdem Sie über ein MSA-Token für den aktuellen Benutzer verfügen, müssen Sie die Get-Methode des URIs abrufen, ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user``` um die verfügbaren Ziel Angebote für den aktuellen Benutzer abzurufen. Weitere Informationen zu dieser Rest-Methode finden [Sie unter Get-Ziel Angebote](get-targeted-offers.md).

Diese Methode gibt die Produkt-IDs der Add-ons zurück, die den Ziel angeboten zugeordnet sind, die für den aktuellen Benutzer verfügbar sind. Mit diesen Informationen können Sie einem Benutzer ein oder mehrere der Ziel Angebote als in-App-Käufe anbieten.

Im folgenden Beispiel wird veranschaulicht, wie die Ziel Angebote für den aktuellen Benutzer erhalten werden. Dieses Beispiel ist ein Auszug aus dem [gesamten Beispiel](#code-example). Hierfür ist die [JSON.net](https://www.newtonsoft.com/json) -Bibliothek von newtonsoft und zusätzlichen Klassen und die **Verwendung** von-Anweisungen erforderlich, die im kompletten Beispiel bereitgestellt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetTargetedOffers":::

<span id="code-example" />

## <a name="complete-code-example"></a>Vollständiges Codebeispiel

Im folgenden Codebeispiel werden die folgenden Aufgaben veranschaulicht:

* Ein MSA-Token für den aktuellen Benutzer erhalten.
* Alle Ziel Angebote für den aktuellen Benutzer mithilfe der Get-Methode " [gezielte Angebote](get-targeted-offers.md) " erhalten.
* Erwerben Sie das Add-on, das mit einem Ziel Angebot verknüpft ist.

Dieses Beispiel erfordert die [JSON.net](https://www.newtonsoft.com/json) -Bibliothek von newtonsoft. Im Beispiel wird diese Bibliothek verwendet, um JSON-formatierte Daten zu serialisieren und zu deserialisieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs" id="GetTargetedOffersSample":::

## <a name="related-topics"></a>Verwandte Themen

* [Verwenden gezielter Angebote zum Maximieren von Engagement und Konvertierungen](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Abrufen von gezielten Angeboten](get-targeted-offers.md)
