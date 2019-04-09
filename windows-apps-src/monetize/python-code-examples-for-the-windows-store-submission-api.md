---
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Verwenden Sie die Python-Codebeispiele in diesem Abschnitt, um mehr über die Verwendung der Microsoft Store-Übermittlungs-API zu erfahren.
title: 'Python-Beispiel: Übermittlungen für Apps, Add-Ons und Flights'
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store-Übermittlungs-API, Codebeispiele, Python
ms.localizationpriority: medium
ms.openlocfilehash: bc3959b4e26bd54542edc3f69666f6d97cddba26
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334828"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Python-Beispiel: Übermittlungen für Apps, Add-Ons und Flights

Dieser Artikel enthält Python-Codebeispiele zeigt das Verwenden der [Microsoft Store-Übermittlungs-API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben:

* [Abrufen eines Azure AD-Zugriffstokens](#token)
* [Ein Add-on erstellen](#create-add-on)
* [Erstellen Sie einen Paket Flug](#create-package-flight)
* [Erstellen Sie ein app-Übermittlung](#create-app-submission)
* [Erstellen Sie ein Add-on-Übermittlung](#create-add-on-submission)
* [Erstellen Sie eine Paket-Flight-Eingabe](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Abrufen eines Azure AD-Zugriffstokens

Im folgenden Beispiel wird gezeigt, wie Sie ein [Azure AD-Zugriffstoken abrufen](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), mit dem Sie Methoden in der Microsoft Store-Übermittlungs-API aufrufen können. Nach dem Abruf eines Tokens können Sie es für einen Zeitraum von 60 Minuten in Aufrufen der Microsoft Store-Übermittlungs-API verwenden, bevor es abläuft. Nach Ablauf des Tokens können Sie ein neues Token generieren.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Erstellen eines Add-Ons

Das folgende Beispiel zeigt, wie Sie EIN Flight-Paket [erstellen](create-an-add-on.md) und anschließend ein Add-On [löschen](delete-an-add-on.md).

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Erstellen eines Flight-Pakets

Das folgende Beispiel zeigt, wie Sie EIN Flight-Paket [erstellen](create-a-flight.md) und anschließend [löschen](delete-a-flight.md).

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Das folgende Beispiel zeigt, wie Sie verschiedene Methoden in der Microsoft Store-Übermittlungs-API verwenden, um eine App-Übermittlung zu erstellen. Zu diesem Zweck der Code erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlicht, und aktualisiert dann und führt einen Commit für die geklonte Übermittlung zum Partner Center. Insbesondere werden im Beispiel folgende Aufgaben gezeigt:

1. Zunächst [werden Daten für die angegebene App abgerufen](get-an-app.md).
2. Als Nächstes [löscht sie die ausstehende Übermittlung für die App](delete-an-app-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für die App erstellt](create-an-app-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es werden einige Details für die neue Übermittlung geändert und ein neues Paket für die Übermittlung zu Azure Blob Storage hochgeladen.
5. Als Nächstes wird es [Updates](update-an-app-submission.md) und dann [führt einen Commit für](commit-an-app-submission.md) die neue Übermittlung zum Partner Center.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-app-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Das folgende Beispiel zeigt, wie Sie verschiedene Methoden in der Microsoft Store-Übermittlungs-API verwenden, um eine Add-On-Übermittlung zu erstellen. Zu diesem Zweck der Code erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlicht, und aktualisiert dann und führt einen Commit für die geklonte Übermittlung zum Partner Center. Insbesondere werden im Beispiel folgende Aufgaben gezeigt:

1. Zunächst [werden Daten für das angegebene Add-On abgerufen](get-an-add-on.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Add-On gelöscht](delete-an-add-on-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Add-On erstellt](create-an-add-on-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein ZIP-Archiv hochgeladen, das Symbole für die Übermittlung an Azure Blob Storage enthält. Weitere Informationen finden Sie in den entsprechenden Anweisungen zum Hochladen von ZIP-Archiven zu Azure Blob Storage in [Erstellen einer Add-On-Übermittlung](manage-add-on-submissions.md#create-an-add-on-submission).
5. Als Nächstes wird es [Updates](update-an-add-on-submission.md) und dann [führt einen Commit für](commit-an-add-on-submission.md) die neue Übermittlung zum Partner Center.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-add-on-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Erstellen einer Flight-Paket-Übermittlung

Das folgende Beispiel zeigt, wie Sie verschiedene Methoden in der Microsoft Store-Übermittlungs-API verwenden, um eine Flight-Paket-Übermittlung zu erstellen. Zu diesem Zweck der Code erstellt eine neue Übermittlung als Klon der letzten Übermittlung veröffentlicht, und aktualisiert dann und führt einen Commit für die geklonte Übermittlung zum Partner Center. Insbesondere werden im Beispiel folgende Aufgaben gezeigt:

1. Zunächst [werden Daten für das angegebene Flight-Paket abgerufen](get-a-flight.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Flight-Paket gelöscht](delete-a-flight-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Flight-Paket erstellt](create-a-flight-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein neues Paket für die Übermittlung auf Azure Blob Storage hochgeladen. Weitere Informationen finden Sie in den entsprechenden Anweisungen zum Hochladen von ZIP-Archiven zu Azure Blob Storage in [Erstellen einer Flight-Paket-Übermittlung](manage-flight-submissions.md#create-a-package-flight-submission).
5. Als Nächstes wird es [Updates](update-a-flight-submission.md) und dann [führt einen Commit für](commit-a-flight-submission.md) die neue Übermittlung zum Partner Center.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-a-flight-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

[!code-python[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen, die mithilfe von Microsoft Store services](create-and-manage-submissions-using-windows-store-services.md)
