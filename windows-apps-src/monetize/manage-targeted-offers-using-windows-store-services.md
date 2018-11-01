---
author: Xansky
ms.assetid: 9F0A59A1-FAD7-4AD5-B78B-C1280F215D23
description: Verwenden Sie die Microsoft Store-API für gezielte Angebote, um die gezielten Angebote abzurufen, die für den aktuellen Benutzer Ihrer App verfügbar sind.
title: Verwalten von gezielten Angeboten mithilfe von Store-Diensten
ms.author: mhopkins
ms.date: 10/10/2017
ms.topic: article
keywords: Windows10, UWP, Store-Dienste, Microsoft Store-API für gezielte Angebote, gezielte Angebote
ms.localizationpriority: medium
ms.openlocfilehash: 706f48e64fb8e7534686b8fd7e9666b98dffd9b7
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/01/2018
ms.locfileid: "5942022"
---
# <a name="manage-targeted-offers-using-store-services"></a>Verwalten von gezielten Angeboten mithilfe von Store-Diensten

Wenn Sie auf der Seite **Interaktion > Zielgerichtete Angebote** zu Ihrer App im Windows Dev Center-Dashboard ein *gezieltes Angebot* erstellen, verwenden Sie die *Microsoft Store-API für gezielte Angebote* in Ihrem App-Code, um Informationen zu erhalten, die Ihnen bei der Implementierung der In-App-Umgebung für das gezielte Angebot helfen. Weitere Informationen zu gezielten Angeboten und Anleitungen zu deren Erstellung im Dashboard finden Sie unter [Verwenden Sie gezielte Angebote, um Interaktionen und Abschlüsse zu maximieren.](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md).

Die Gezielte-Angebote-API ist eine einfache REST-API, mit der Sie gezielte Angebote abrufen können, die für den aktuellen Benutzer verfügbar sind– basierend darauf, ob der Benutzer zum Kundensegment für das gezielte Angebot gehört. Gehen Sie folgendermaßen vor, um diese API in Ihrem App-Code zu verwenden:

1.  [Rufen Sie ein Microsoft-Kontotoken](#obtain-a-microsoft-account-token) für den aktuell angemeldeten Benutzer Ihrer App ab.
2.  [Rufen Sie die gezielten Angebote für den aktuellen Benutzer ab](#get-targeted-offers).
3.  Implementieren Sie die In-App-Kaufumgebung für das Add-On, das einem der gezielten Angebote zugeordnet ist. Weitere Informationen zur Implementierung von In-App-Käufen finden Sie in [diesem Artikel](enable-in-app-purchases-of-apps-and-add-ons.md).

Ein vollständiges Codebeispiel, das alle diese Schritteveranschaulicht, finden Sie unter der [Codebeispiel](#code-example) am Ende dieses Artikels. Die folgenden Abschnitte enthalten weitere Details zu den einzelnen Schritten.

<span id="obtain-a-microsoft-account-token" />

## <a name="get-a-microsoft-account-token-for-the-current-user"></a>Abrufen eines Microsoft-Kontotokens für den aktuellen Benutzer

Rufen Sie in Ihrem App-Code ein Microsoft-Konto (MSA)-Token für den aktuell angemeldeten Benutzer ab. Sie müssen dieses Token im ```Authorization```-Anforderungsheader für jede Methode in der Microsoft Store-API für gezielte Angebote übergeben. Dieses Token wird vom Store verwendet, um die gezielten Angebote abzurufen, die für den aktuellen Benutzer verfügbar sind.

Um das MSA-Token abzurufen, verwenden Sie die [WebAuthenticationCoreManager](https://docs.microsoft.com/uwp/api/windows.security.authentication.web.core.webauthenticationcoremanager)-Klasse, um ein Token unter Angabe des Bereichs anzufordern ```devcenter_implicit.basic,wl.basic```. Das folgende Beispiel veranschaulicht die Vorgehensweise. In diesem Beispiel wird ein Auszug aus dem [vollständige Beispiel](#code-example) verwendet. Es erfordert **using**-Anweisungen, die im vollständigen Beispiel bereitgestellt werden.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetMSAToken)]

Weitere Informationen zum Abrufen von MSA-Token finden Sie unter [Web Account Manager](../security/web-account-manager.md).

<span id="get-targeted-offers" />

## <a name="get-the-targeted-offers-for-the-current-user"></a>Abrufen der gezielten Angebote für den aktuellen Benutzer

Rufen Sie nach Erhalt des MSA-Tokens für den aktuellen Benutzer die GET-Methode zum ```https://manage.devcenter.microsoft.com/v2.0/my/storeoffers/user```-URI auf, um die für den aktuellen Benutzer verfügbaren Angebote abzurufen. Weitere Informationen zu dieser REST-Methode finden Sie unter [Abrufen gezielter Angebote](get-targeted-offers.md).

Bei dieser Methode wird ein Array mit Produkt-IDs für die Add-Ons zurückgegeben, die den gezielten Angeboten zugeordnet sind, die für den aktuellen Benutzer verfügbar sind. Mit diesen Informationen können Sie ein oder mehrere gezielte Angebote als In-App-Einkauf für den Benutzer erstellen.

Das folgende Beispiel zeigt, wie Sie die gezielten Angebote für den aktuellen Benutzer abrufen. Dieses Beispiel ist ein Auszug aus dem [vollständige Beispiel](#code-example). Erfordert die [Json.NET](http://www.newtonsoft.com/json)-Bibliothek von Newtonsoft und zusätzliche Klassen und **using**-Anweisungen, die im vollständigen Beispiel bereitgestellt werden.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffers)]

<span id="code-example" />

## <a name="complete-code-example"></a>Vollständiger Beispielcode

Das folgende Codebeispiel veranschaulicht die folgenden Aufgaben:

* Abrufen eines MSA-Tokens für den aktuellen Benutzer
* Abrufen aller gezielten Angebote für den aktuellen Benutzer mithilfe der Methode zum [Abrufen gezielter Angebote](get-targeted-offers.md)
* Kaufen Sie das Add-On, das mit einem gezielten Angebot verknüpft ist.

Dieses Beispiel erfordert die [Json.NET](http://www.newtonsoft.com/json)-Bibliothek von Newtonsoft. Im Beispiel wird diese Bibliothek verwendet, um Daten im JSON-Format zu serialisieren bzw. deserialisieren.

[!code-cs[TargetedOffers](./code/StoreServicesExamples_TargetedOffers/cs/TargetedOffers.cs#GetTargetedOffersSample)]

## <a name="related-topics"></a>Verwandte Themen

* [Verwenden Sie zielgerichtete Angebote, um Interaktionen und Abschlüsse zu maximieren.](../publish/use-targeted-offers-to-maximize-engagement-and-conversions.md)
* [Abrufen gezielter Angebote](get-targeted-offers.md)
