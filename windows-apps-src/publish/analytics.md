---
Description: Hier erhalten Sie ausführliche Analysen für Ihre Windows-apps im Partner Center oder über andere Methoden.
title: Analysieren der App-Leistung
ms.assetid: 3A3C6F10-0DB1-416D-B632-CD388EA66759
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Analysen, Berichte, Dashboards, apps, Daten, Metriken
ms.localizationpriority: medium
ms.openlocfilehash: 9b14ee776b31e21fbb8a0ee1d66b933648549b69
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162084"
---
# <a name="analyze-app-performance"></a>Analysieren der App-Leistung

Sie können ausführliche Analysen für Ihre apps in [Partner Center](https://partner.microsoft.com/dashboard)anzeigen. Statistiken und Diagramme geben Aufschluss über den Erfolg Ihrer Apps, z. B. wie viele Kunden Sie erreichen, wie die Kunden Ihre App einsetzen und was die Kunden über die App denken. Sie können auch Metriken zu app-Integrität, AD-Nutzung und mehr finden.

Sie können analytische Berichte direkt in Partner Center anzeigen oder [die Berichte herunterladen, die Sie benötigen](download-analytic-reports.md) , um Ihre Daten offline zu analysieren. Außerdem bieten wir Ihnen verschiedene Möglichkeiten, auf [ihre Analysedaten außerhalb von Partner Center zuzugreifen](#outside).

## <a name="view-key-analytics-for-all-your-apps"></a>Wichtige Analysen für alle Ihre apps anzeigen

Wenn Sie wichtige Analysen zu ihren am häufigsten heruntergeladenen apps anzeigen möchten, erweitern Sie **analysieren** , und wählen Sie **Übersicht**. Standardmäßig werden auf der Übersichtsseite Informationen zu den fünf apps angezeigt, die über die meisten Lebensdauer Käufe verfügen. Wählen Sie **Filter**aus, um unterschiedliche veröffentlichte Apps für die Anzeige auszuwählen.

## <a name="view-individual-reports-for-each-app"></a>Anzeigen einzelner Berichte für jede APP

In diesem Abschnitt erhalten Sie Details zu den Informationen, die in den folgenden Berichten enthalten sind:

-   [Bericht „Käufe“](acquisitions-report.md)
-   [Bericht zu Add-On-Käufen](add-on-acquisitions-report.md)
-   [Nutzungsbericht](usage-report.md)
-   [Integritätsbericht](health-report.md)
-   [Bericht „Bewertungen“](ratings-report.md)
-   [Bericht „Rezensionen“](reviews-report.md)
-   [Feedbackbericht](feedback-report.md)
-   [Xbox-Analysebericht](xbox-analytics-report.md)
-   [Datenbericht](insights-report.md)
-   [Bericht zur Anzeigenleistung](advertising-performance-report.md)
-   [Werbekampagnenbericht](/windows/uwp/publish/ad-campaign-report)


> [!NOTE]
> Abhängig von den spezifischen Features und der Implementierung Ihrer App enthalten einige dieser Berichte möglicherweise keine Daten.

<span id="outside"/>

## <a name="access-analytics-data-outside-of-partner-center"></a>Zugreifen auf Analytics-Daten außerhalb von Partner Center

Zusätzlich zum Anzeigen von Berichten im Partner Center können Sie auf andere Weise auf App Analytics zugreifen.

### <a name="microsoft-store-analytics-api"></a>Microsoft Store Analytics-API

Verwenden Sie die [Store Analytics-API](../monetize/access-analytics-data-using-windows-store-services.md) , um Analysedaten für ihre Apps Programm gesteuert abzurufen. Mit dieser REST-API können Sie Daten für App- und Add-On-Käufe, Fehler sowie App-Bewertungen und -Rezensionen abrufen. Diese API verwendet Azure Active Directory (Azure AD), um die Aufrufe von Ihrer App oder Ihrem Dienst zu authentifizieren.

### <a name="windows-dev-center-content-pack-for-power-bi"></a>Windows Dev Center-Inhaltspaket für Power BI

Verwenden Sie das [Windows dev Center-Inhalts Paket für Power BI](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/) , um Ihre Partner Center Analytics-Daten in Power BI zu untersuchen und zu überwachen. Power BI ist ein cloudbasierter Geschäftsanalysedienst, der Ihnen Ihre Geschäftsdaten in einer einzelnen Ansicht präsentiert.

Verwenden Sie die folgenden Ressourcen, um mit Power BI auf Ihre Analysedaten zuzugreifen.

* [Registrieren bei Power BI](https://powerbi.microsoft.com/documentation/powerbi-service-self-service-signup-for-power-bi/)
* [Informationen zur Verwendung von Power BI](https://powerbi.microsoft.com/guided-learning/)
* [Informationen zum Einsetzen des Windows Dev Center-Inhaltspakets für Power BI, um eine Verbindung zu Ihren Analysedaten herzustellen](https://powerbi.microsoft.com/documentation/powerbi-content-pack-windows-dev-center/)

> [!NOTE]
> Zum Herstellen einer Verbindung mit dem Windows dev Center-Inhalts Paket für Power BI empfiehlt es sich, Anmelde Informationen aus einem Azure AD Verzeichnis anzugeben, das Ihrem Partner Center-Konto zugeordnet ist. Wenn Sie die Anmeldeinformationen Ihres Microsoft-Kontos verwenden, werden Ihre Analysedaten in Power BI nicht automatisch aktualisiert. Für eine Aktualisierung müssen Sie sich dann in Power BI anmelden. Wenn Ihre Organisation bereits Microsoft 365 oder andere Unternehmens Dienste von Microsoft verwendet, verfügen Sie bereits über Azure AD. Andernfalls können Sie es [kostenlos abrufen](https://account.azure.com/organization). Weitere Informationen zum Einrichten der Zuordnung finden [Sie unterzuordnen von Azure Active Directory zu Ihrem Partner Center-Konto](./associate-azure-ad-with-partner-center.md).