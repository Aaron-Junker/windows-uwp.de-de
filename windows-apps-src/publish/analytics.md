---
Description: Erhalten Sie detaillierte Analysen für Ihre Windows-apps, im Partner Center oder mithilfe anderer Methoden.
title: Analysieren der App-Leistung
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, Uwp, Analysen, Berichte, Dashboards, apps, Daten, Metriken
ms.localizationpriority: medium
ms.openlocfilehash: 6f76b1f897c345fb71beec8e37e592165922b2ed
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57625445"
---
# <a name="analyze-app-performance"></a>Analysieren der App-Leistung

Sie finden detaillierte Analysen für Ihre apps in [Partner Center](https://partner.microsoft.com/dashboard). Statistiken und Diagramme geben Aufschluss über den Erfolg Ihrer Apps, z. B. wie viele Kunden Sie erreichen, wie die Kunden Ihre App einsetzen und was die Kunden über die App denken. Außerdem finden Sie dort Metriken zur App-Integrität, zur Anzeigennutzung und mehr.

Sehen Sie Analytics-Berichte direkt im Partner Center oder [Herunterladen der Berichte Sie müssen](download-analytic-reports.md) zum Analysieren von Daten offline. Wir bieten auch verschiedene Möglichkeiten für Sie [Zugriff auf Ihre Analytics-Daten außerhalb von Partner Center](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Zeigen Sie die wichtigsten Analysen für Ihre gesamten Apps an

Um die wichtigsten Analysen zu den am häufigsten heruntergeladenen Apps anzuzeigen, erweitern Sie **Analyse** und wählen Sie **Übersicht** aus. Auf der Seite „Übersicht” werden standardmäßig Informationen zu den fünf während ihrer Lebensdauer am häufigsten gekauften Apps angezeigt. Um andere veröffentlichte Apps auszuwählen und anzuzeigen, wählen Sie **Filter** aus.

## <a name="view-individual-reports-for-each-app"></a>Zeigen Sie einzelne Berichte für die einzelnen Apps an

In diesem Abschnitt erhalten Sie Details zu den Informationen, die in den folgenden Berichten enthalten sind:

-   [Bericht „Käufe“](acquisitions-report.md)
-   [Bericht zu Add-On-Käufen](add-on-acquisitions-report.md)
-   [Nutzungsbericht](usage-report.md)
-   [Bericht zur Integrität](health-report.md)
-   [Bewertungen-Bericht](ratings-report.md)
-   [Rezensionsbericht](reviews-report.md)
-   [Feedbackbericht](feedback-report.md)
-   [Xbox-Analysebericht](xbox-analytics-report.md)
-   [Insights-Bericht](insights-report.md)
-   [Bericht zur Anzeigenleistung](advertising-performance-report.md)
-   [Werbekampagnen-Bericht](promote-your-app-report.md)


> [!NOTE]
> Abhängig von den spezifischen Features und der Implementierung Ihrer App enthalten einige dieser Berichte möglicherweise keine Daten.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Analytics-Daten für den Zugriff außerhalb von Partner Center

Zusätzlich zum Anzeigen von Berichten im Partner Center können Sie app-Analyse auf andere Weise zugreifen.

### <a name="microsoft-store-analytics-api"></a>Microsoft Store-Analyse-API

Verwenden Sie die [Store-Analyse-API](../monetize/access-analytics-data-using-windows-store-services.md), um programmgesteuert Analysedaten für Ihre Apps abzurufen. Mit dieser REST-API können Sie Daten für App- und Add-On-Käufe, Fehler sowie App-Bewertungen und -Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Windows Dev Center-Inhaltspaket für Power BI

Verwenden der [Windows Dev Center-Inhaltspaket für Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) zu untersuchen und überwachen Sie Ihre Partner Center Analytics-Daten in Power BI. Power BI ist ein cloudbasierter Geschäftsanalysedienst, der Ihnen Ihre Geschäftsdaten in einer einzelnen Ansicht präsentiert.

Verwenden Sie die folgenden Ressourcen, um mit Power BI auf Ihre Analysedaten zuzugreifen.

* [Melden Sie sich für Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Erfahren Sie, wie Sie Power BI verwenden](https://powerbi.microsoft.com/guided-learning/)
* [Erfahren Sie, wie das Windows Dev Center-Inhaltspaket für Power BI zu verwenden, um Ihre Analytics-Daten herstellen](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Um mit dem Windows Dev Center-Inhaltspaket für Power BI verbinden, empfehlen wir, dass Sie Anmeldeinformationen von Azure AD-Verzeichnis angeben, die mit Ihrem Partner Center-Konto verknüpft ist. Wenn Sie die Anmeldeinformationen Ihres Microsoft-Kontos verwenden, werden Ihre Analysedaten in Power BI nicht automatisch aktualisiert. Für eine Aktualisierung müssen Sie sich dann in Power BI anmelden. Wenn in Ihrer Organisation bereits mit Office 365 oder anderen Unternehmensdiensten von Microsoft gearbeitet wird, verfügen Sie bereits über Azure AD. Andernfalls können Sie es [kostenlos abrufen](https://go.microsoft.com/fwlink/p/?LinkId=703757). Weitere Informationen zum Einrichten der Zuordnung finden Sie unter [ordnen Sie Azure Active Directory mit Ihrem Partner Center-Konto](associate-azure-ad-with-dev-center.md).
