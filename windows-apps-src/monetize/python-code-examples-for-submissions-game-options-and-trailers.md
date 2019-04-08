---
description: Verwenden Sie die Python-Codebeispiele in diesem Abschnitt, um mehr über das Einreichen von Spieloptionen und Trailern über die Verwendung der Microsoft Store-Übermittlungs-API zu erfahren.
title: 'Python-Beispiel: App-Übermittlung mit Spieloptionen und Trailer'
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, Uwp, Microsoft Store-Übermittlungs-API, Codebeispiele, Spieloptionen, Trailer, erweiterte Angebote, Python
ms.localizationpriority: medium
ms.openlocfilehash: 59306e32fe1fcc68978c977b89934e64d85b8cc8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57629915"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Python-Beispiel: App-Übermittlung mit Spieloptionen und Trailer

Dieser Artikel enthält Python-Codebeispiele zeigt das Verwenden der [Microsoft Store-Übermittlungs-API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben:

* Abrufen eines Azure AD-Zugriffstokens zur Nutzung mit der Microsoft Store-Übermittlungs-API.
* Erstellen einer App-Übermittlung
* Konfigurieren von Store-Eintragsdateien für die App-Übermittlung, einschließlich der erweiterten Eintragsoptionen [Spiele](manage-app-submissions.md#gaming-options-object) und [Trailer](manage-app-submissions.md#trailer-object).
* Hochladen der ZIP-Datei mit den Paketen, Eintragsbildern und Trailerdateien für die App-Übermittlung.
* Ausführen des Commit für eine App-Übermittlung.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Dieser Code ruft andere Beispielklassen und Funktionen auf, um die Microsoft Store-Übermittlungs-API zum Erstellen und Ausführen eines Commits einer App-Übermittlung mit Optionen und einem Trailer verwendet. So passen Sie den Code für eigene Zwecke an:

* Weisen Sie die ```tenant```-Variable zur Mandanten-ID für Ihre App zu und weisen Sie die Variablen ```client``` und ```secret``` zur Client-ID und dem Schlüssel für die App zu. Weitere Informationen finden Sie unter [eine Azure AD-Anwendung mit Ihrem Partner Center-Konto zuordnen.](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Weisen Sie die ```application_id```-Variable zur [Store-ID](in-app-purchases-and-trials.md#store-ids) der App zu, für die eine Übermittlung erstellen möchten.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py#L1-L74)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Ein Azure AD-Zugriffstoken abrufen und Aufrufen der Übermittlungs-API

Das folgende Beispiel definiert die folgenden Klassen:

* Die ```DevCenterAccessTokenClient```-Klasse definiert eine Hilfsmethode, die Ihre ```tenantId```, ```clientId``` und ```clientSecret``` Werte zum Erstellen eines Azure AD-Zugriffstokens zur Verwendung mit Microsoft Store-Übermittlungs-API verwendet.
* Die ```DevCenterClient``` Klasse definiert Hilfsmethoden, die eine Vielzahl von Methoden in der Microsoft Store-Übermittlungs-API aufrufen und die ZIP-Datei mit den Paketen, Bildern und Trailerdateien für die App-Übermittlung hochladen.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py#L1-L126)]

<span id="token" />

## <a name="get-app-submission-listing-data"></a>Abrufen von Eintragsdaten

Das folgende Beispiel definiert Hilfsfunktionen, die JSON-formatierte Eintrag Daten für eine neue Übermittlung der Beispiel-App zurückzugeben.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py#L1-L170)]

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
