---
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Verwenden Sie die python-Codebeispiele in diesem Abschnitt, um weitere Informationen zur Verwendung der Microsoft Store Übermittlungs-API zu erhalten.
title: Python-Code zum Übermitteln von apps, Add-ons und Flügen
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, UWP, Microsoft Store Übermittlungs-API, Codebeispiele, python
ms.localizationpriority: medium
ms.openlocfilehash: 3fd29164dd91199b1557496985577ea956893250
ms.sourcegitcommit: 7e8dfd83b181fe720b4074cb42adc908e1ba5e44
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811263"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Python-Beispiel: Übermittlungen für Apps, Add-Ons und Flights

Dieser Artikel enthält python-Codebeispiele, die veranschaulichen, wie die Microsoft Store Übermittlungs- [API](create-and-manage-submissions-using-windows-store-services.md) für diese Aufgaben verwendet wird:

* [Abrufen eines Azure AD-Zugriffstokens](#token)
* [Erstellen eines Add-Ons](#create-add-on)
* [Erstellen eines Flight-Pakets](#create-package-flight)
* [Erstellen einer App-Übermittlung](#create-app-submission)
* [Erstellen einer Add-On-Übermittlung](#create-add-on-submission)
* [Erstellen einer Flight-Paket-Übermittlung](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Abrufen eines Azure AD-Zugriffstokens

Im folgenden Beispiel wird veranschaulicht, wie Sie [ein Azure AD Zugriffs Token](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) abrufen, das Sie zum Aufrufen von Methoden in der Microsoft Store Übermittlungs-API verwenden können. Nachdem Sie ein Token erhalten haben, haben Sie 60 Minuten Zeit, dieses Token in Aufrufen der Microsoft Store Übermittlungs-API zu verwenden, bevor das Token abläuft. Nach Ablauf des Tokens können Sie ein neues Token generieren.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="1-20":::

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Erstellen eines Add-Ons

Im folgenden Beispiel wird veranschaulicht, wie ein Add-on [erstellt](create-an-add-on.md) und anschließend [gelöscht](delete-an-add-on.md) wird.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="26-52":::

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Erstellen eines Flight-Pakets

Das folgende Beispiel zeigt, wie Sie EIN Flight-Paket [erstellen](create-a-flight.md) und anschließend [löschen](delete-a-flight.md).

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="58-87":::

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Erstellen einer App-Übermittlung

Im folgenden Beispiel wird gezeigt, wie Sie in der Microsoft Store Übermittlungs-API mehrere Methoden zum Erstellen einer APP-Übermittlung verwenden. Zu diesem Zweck erstellt der Code eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Insbesondere werden im Beispiel folgende Aufgaben gezeigt:

1. Zunächst [werden Daten für die angegebene App abgerufen](get-an-app.md).
2. Als Nächstes [löscht sie die ausstehende Übermittlung für die App](delete-an-app-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für die App erstellt](create-an-app-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es werden einige Details für die neue Übermittlung geändert und ein neues Paket für die Übermittlung an Azure BLOB Storage hochgeladen.
5. [Im nächsten](commit-an-app-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-an-app-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-app-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="93-166":::

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Erstellen einer Add-On-Übermittlung

Im folgenden Beispiel wird gezeigt, wie mehrere Methoden in der Microsoft Store Übermittlungs-API verwendet werden, um eine Add-on-Übermittlung zu erstellen. Zu diesem Zweck erstellt der Code eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Insbesondere werden im Beispiel folgende Aufgaben gezeigt:

1. Zunächst [werden Daten für das angegebene Add-On abgerufen](get-an-add-on.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Add-On gelöscht](delete-an-add-on-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Add-On erstellt](create-an-add-on-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es lädt ein ZIP-Archiv hoch, das Symbole für die Übermittlung an Azure BLOB Storage enthält. Weitere Informationen finden Sie in den entsprechenden Anweisungen zum Hochladen eines ZIP-Archivs in Azure BLOB Storage in [Erstellen einer Add-on-Übermittlung](manage-add-on-submissions.md#create-an-add-on-submission).
5. [Im nächsten](commit-an-add-on-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-an-add-on-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-an-add-on-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="172-245":::

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Erstellen einer Flight-Paket-Übermittlung

Im folgenden Beispiel wird gezeigt, wie mehrere Methoden in der Microsoft Store Übermittlungs-API verwendet werden, um eine paketflight-Übermittlung zu erstellen. Zu diesem Zweck erstellt der Code eine neue Übermittlung als Klon der letzten veröffentlichten Übermittlung und aktualisiert dann die geklonte Übermittlung an Partner Center und führt Sie aus. Insbesondere werden im Beispiel folgende Aufgaben gezeigt:

1. Zunächst [werden Daten für das angegebene Flight-Paket abgerufen](get-a-flight.md).
2. Als Nächstes [wird die ausstehende Übermittlung für das Flight-Paket gelöscht](delete-a-flight-submission.md), wenn vorhanden.
3. Anschließend [wird eine neue Übermittlung für das Flight-Paket erstellt](create-a-flight-submission.md). (Die neue Übermittlung ist eine Kopie der letzten veröffentlichten Übermittlung.)
4. Es wird ein neues Paket für die Übermittlung an Azure BLOB Storage hochgeladen. Weitere Informationen finden Sie in den entsprechenden Anweisungen zum Hochladen eines ZIP-Archivs in Azure BLOB Storage unter [Erstellen einer paketflight-Übermittlung](manage-flight-submissions.md#create-a-package-flight-submission).
5. [Im nächsten](commit-a-flight-submission.md) Schritt wird die neue Übermittlung an Partner Center [aktualisiert](update-a-flight-submission.md) und anschließend ein Commit ausgeführt.
6. Schließlich [wird der Status der neuen Übermittlung regelmäßig überprüft](get-status-for-a-flight-submission.md), bis die Übermittlung erfolgreich gesendet wurde.

:::code language="python" source="~/../snippets-windows/windows-uwp/monetize/StoreServicesExamples_Submission/python/Examples.py" range="251-325":::

## <a name="related-topics"></a>Verwandte Themen

* [Erstellen und Verwalten von Übermittlungen mithilfe von Microsoft Store Services](create-and-manage-submissions-using-windows-store-services.md)
