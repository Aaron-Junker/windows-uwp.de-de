---
description: Verwenden Sie die Java-Codebeispiele in diesem Abschnitt, um mehr über das Einreichen von Spieloptionen und Trailern über die Verwendung der Microsoft Store-Übermittlungs-API zu erfahren.
title: 'Java-Beispiel: App-Übermittlung mit Spieloptionen und Trailer'
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, Uwp, Microsoft Store-Übermittlungs-API, Codebeispiele, Spieloptionen, Trailer, erweiterte Angebote, Java
ms.localizationpriority: medium
ms.openlocfilehash: 974bbc4c864edb9450f9ba677c60349b5e1f8ece
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641055"
---
# <a name="java-sample-app-submission-with-game-options-and-trailers"></a>Java-Beispiel: App-Übermittlung mit Spieloptionen und Trailer

Dieser Artikel enthält Java-Codebeispiele zeigt das Verwenden der [Microsoft Store-Übermittlungs-API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben:

* Abrufen eines Azure AD-Zugriffstokens zur Nutzung mit der Microsoft Store-Übermittlungs-API.
* Erstellen einer App-Übermittlung
* Konfigurieren von Store-Eintragsdateien für die App-Übermittlung, einschließlich der erweiterten Eintragsoptionen [Spiele](manage-app-submissions.md#gaming-options-object) und [Trailer](manage-app-submissions.md#trailer-object).
* Hochladen der ZIP-Datei mit den Paketen, Eintragsbildern und Trailerdateien für die App-Übermittlung.
* Ausführen des Commit für eine App-Übermittlung.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Die ```CreateAndSubmitSubmissionExample```-Klasse implementiert ein ```main```-Programm, das andere Beispielmethoden aufruft, um die Microsoft Store-Übermittlungs-API zum Erstellen und Ausführen eines Commits einer App-Übermittlung mit Optionen und einem Trailer verwendet. So passen Sie den Code für eigene Zwecke an:

* Weisen Sie die ```tenantId```-Variable zur Mandanten-ID für Ihre App zu und weisen Sie die Variablen ```clientId``` und ```clientSecret``` zur Client-ID und dem Schlüssel für die App zu. Weitere Informationen finden Sie unter [eine Azure AD-Anwendung mit Ihrem Partner Center-Konto zuordnen.](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Weisen Sie die ```applicationId```-Variable zur [Store-ID](in-app-purchases-and-trials.md#store-ids) der App zu, für die eine Übermittlung erstellen möchten.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/CreateAndSubmitSubmissionExample.java#L1-L313)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Abrufen eines Azure AD-Zugriffstokens

Die ```DevCenterAccessTokenClient```-Klasse definiert eine Hilfsmethode, die Ihre ```tenantId```, ```clientId``` und ```clientSecret``` Werte zum Erstellen eines Azure AD-Zugriffstokens zur Verwendung mit Microsoft Store-Übermittlungs-API verwendet.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterAccessTokenClient.java#L1-L69)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Hilfsmethoden zum Aufrufen von der Übermittlungs-API und zum Hochladen von Übermittlungsdateien

Die ```DevCenterClient``` Klasse definiert Hilfsmethoden, die eine Vielzahl von Methoden in der Microsoft Store-Übermittlungs-API aufrufen und die ZIP-Datei mit den Paketen, Bildern und Trailerdateien für die App-Übermittlung hochladen.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterClient.java#L1-L224)]

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
