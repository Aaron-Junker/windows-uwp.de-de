---
description: Verwenden Sie die python-Codebeispiele in diesem Abschnitt, um mehr über das Senden von Spieloptionen und-Nachspann mithilfe der Microsoft Store Übermittlungs-API zu erfahren
title: Python-Beispiel-App-Übermittlung mit Spieloptionen und-Nachspann
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Codebeispiele, Spieloptionen, Nachspann, erweiterte Auflistungen, python
ms.localizationpriority: medium
ms.openlocfilehash: 26a2062ff1d8e0f03ff8507cab89bed942012143
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364073"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Python-Beispiel: App-Übermittlung mit Spieloptionen und Trailern

Dieser Artikel enthält python-Codebeispiele, die veranschaulichen, wie die Microsoft Store Übermittlungs- [API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben verwendet wird:

* Rufen Sie ein Azure AD Zugriffs Token für die Verwendung mit der Microsoft Store Übermittlungs-API ab.
* Erstellen einer App-Übermittlung
* Konfigurieren Sie Speicher Listen Daten für die [App-Übermittlung](manage-app-submissions.md#trailer-object) , einschließlich der erweiterten Auflistungs Optionen für [Spiele](manage-app-submissions.md#gaming-options-object) und nach Spann.
* Laden Sie die ZIP-Datei mit den Paketen, Auflistungs Bildern und Nachspann Dateien für die APP-Übermittlung hoch.
* Commit der APP-Übermittlung.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Dieser Code ruft andere Beispiel Klassen und-Funktionen auf, um die Microsoft Store Übermittlungs-API zum Erstellen und Übermitteln einer APP-Übermittlung mit Spieloptionen und einem Nachspann zu verwenden. So passen Sie diesen Code für Ihre eigene Verwendung an:

* Weisen Sie die `tenant` Variable der Mandanten-ID für Ihre APP zu, und weisen Sie die `client` Variablen und der Client- `secret` ID und dem Schlüssel für Ihre APP zu. Weitere Informationen finden Sie unter [Zuordnen einer Azure AD Anwendung zu Ihrem Partner Center-Konto](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account) .
* Weisen `application_id` Sie die Variable der [Speicher-ID](in-app-purchases-and-trials.md#store-ids) der APP zu, für die Sie eine Übermittlung erstellen möchten.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py" range="1-74":::

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Abrufen eines Azure AD Zugriffs Tokens und Aufrufen der Übermittlungs-API

Im folgenden Beispiel werden die folgenden Klassen definiert:

* Die `DevCenterAccessTokenClient` -Klasse definiert eine Hilfsmethode, die die `tenantId` Werte your, und verwendet, `clientId` `clientSecret` um ein Azure AD Zugriffs Token zur Verwendung mit der Microsoft Store Übermittlungs-API zu erstellen.
* Die `DevCenterClient` -Klasse definiert Hilfsmethoden, die eine Vielzahl von Methoden in der Microsoft Store Übermittlungs-API aufrufen und die ZIP-Datei mit den Paketen, Listen Bildern und Nachspann Dateien für die APP-Übermittlung hochladen.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py" range="1-126":::

<span id="token" />

## <a name="get-app-submission-listing-data"></a>App-Übermittlungs Listen Daten erhalten

Im folgenden Beispiel werden Hilfsfunktionen definiert, die JSON-formatierte Auflistungs Daten für eine neue Beispiel-App-Übermittlung zurückgeben.

> [!div class="tabbedCodeSnippets"]
:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py" range="1-170":::

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
