---
Description: Verwalten und Anzeigen von Details für jede Ihrer apps im Partner Center an und Konfigurieren von Diensten wie z. B. ein / B-Tests und zugeordnet.
title: App-Verwaltung und -Dienste
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ba28fb49accc213f570e470142e259c08122b704
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334578"
---
# <a name="app-management-and-services"></a>App-Verwaltung und -Dienste

Können Sie verwalten und Anzeigen von Details im Zusammenhang mit der jede Ihrer apps im [Partner Center](https://partner.microsoft.com/dashboard/), und Konfigurieren von Diensten wie z. B. Benachrichtigungen, die ein / B-Tests und zugeordnet.

Bei der Arbeit mit einer app in Partner Center sehen Sie im linken Navigationsmenü für Abschnitte **Services** und **App-Verwaltung**. Sie können diese Abschnitte erweitern, um auf die unten beschriebenen Funktionen zuzugreifen.

## <a name="services"></a>Dienste

Im Abschnitt **Dienste** können Sie verschiedene Dienste für Ihre Apps verwalten.

## <a name="xbox-live"></a>Xbox Live

Wenn Sie ein Spiel veröffentlichen, können Sie aktivieren die [Xbox Live Creators-Programm](https://xbox.com/developers/creators-program) auf dieser Seite. Dadurch können Sie konfigurieren und Testen Xbox Live-Features zu starten, und schließlich veröffentlichen Sie Ihr Spiel Xbox Live Creators-Programm.

Weitere Informationen finden Sie unter [erste Schritte mit dem Xbox Live Creators-Programm](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) und [einen neuen Titel für die Xbox Live Creators-Programm zu erstellen und veröffentlichen Sie in der testumgebung](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title).

## <a name="experimentation"></a>Experimentation

Auf der Seite **Experimentation** können Sie Experimente mit A/B-Tests für Ihre Apps für die universelle Windows-Plattform (UWP) erstellen und ausführen. Bei einem A/B-Test wird die Effektivität von Featureänderungen (oder Abweichungen) in Ihrer App für einige Kunden ermittelt, bevor die Änderungen für die Allgemeinheit aktiviert werden.

Weitere Informationen finden Sie unter [Ausführen von App-Experimenten mit A/B-Tests](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Karten

Um Kartendienste in Apps zu verwenden, die auf Windows 10 or Windows 8.x ausgerichtet sind, besuchen Sie das [Bing Karten Dev Center](https://go.microsoft.com/fwlink/p/?LinkId=614880). Informationen dazu, wie Sie einen Authentifizierungsschlüssel für die Zuordnungen aus dem Bing Maps-Entwicklercenter anzufordern und zu Ihrer app hinzufügen, finden Sie unter [fordern Sie einen Maps-Authentifizierungsschlüssel](../maps-and-location/authentication-key.md) für Weitere Informationen. 

Verwenden der **Maps** Seite nur für zuvor veröffentlichte apps für Windows Phone 8.1 und frühere Versionen. Um die Map-Dienste in diesen apps zu verwenden, müssen Sie zum Anfordern einer Map-Service-Anwendungs-ID und ein Token in den Code Ihrer app eingeschlossen werden sollen. Beim Klicken auf **Tokenabruf**, generieren wir einen Map-Diensts Anwendungs-ID (**ApplicationID**), und ordnen Sie die Service-Authentifizierungstoken (**AuthenticationToken**) für Ihre app. Achten Sie darauf, um Ihren Code vor dem Verpacken Sie diese Werte hinzu, und senden Sie Ihre app. Weitere Informationen finden Sie unter [Hinzufügen eines Kartensteuerelements zu einer Seite (Windows Phone 8.1)](https://go.microsoft.com/fwlink/p/?LinkId=614882).

## <a name="product-collections-and-purchases"></a>Produktsammlungen und Einkäufe

Um den Microsoft Store modelldatensammlungs-API und der Microsoft Store-Einkauf-API den Zugriff auf den von Besitzinformationen für apps und -Add-Ons zu verwenden, müssen Sie die zugeordnete eingeben hier Azure AD-Client-IDs. Beachten Sie, dass es bis zu 16 Stunden dauern kann, bis diese Änderungen wirksam werden.

Weitere Informationen finden Sie unter [Verwalten von Produktansprüchen aus einem Dienst](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Zustimmung des Administrators

Wenn Ihr Produkt Azure AD integriert und Aufrufe von APIs, die entweder anfordern [Anwendungsberechtigungen oder delegierte Berechtigungen](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) , die Zustimmung des Administrators erfordern, geben Sie Ihre Azure AD-Client-ID hier. Dadurch können Administratoren, die die app für die Zustimmung erteilen Organisation für Ihr Produkt im Namen aller Benutzer im Mandanten abrufen.

Weitere Informationen finden Sie unter [Anfordern der Zustimmung für einen gesamten Mandanten](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>App-Verwaltung

Im Abschnitt **App-Verwaltung** können Sie Identitäts- und Paketdetails anzeigen sowie die Namen Ihrer App verwalten.

## <a name="app-identity"></a>App-Identität

Diese Seite enthält die Details zur eindeutigen App-Identität im Store, einschließlich den URL(s) zur Verknüpfung der App mit dem App-Eintrag.

Weitere Informationen finden Sie unter [Anzeigen von Details zur App-Identität](view-app-identity-details.md).

## <a name="manage-app-names"></a>Verwalten von App-Namen

Auf dieser Seite können Sie alle für Ihre App reservierten Namen anzeigen. Sie können hier auch zusätzliche Namen reservieren oder nicht mehr verwendete Namen löschen.

Weitere Informationen finden Sie unter [Verwalten von App-Namen](manage-app-names.md).

## <a name="current-packages"></a>Aktuelle Pakete

Auf dieser Seite können Sie Details zu allen veröffentlichten Paketen anzeigen.

> [!NOTE]
> Hier werden erst Informationen angezeigt, wenn Ihre App veröffentlicht wurde.

Der Name, die Version und die Architektur der einzelnen Pakete werden angezeigt. Klicken Sie auf **Details**, um zusätzliche Informationen wie z. B. unterstützte Sprache, App-Funktionen und Dateigrößen anzuzeigen. Welche Informationen jeweils für ein Paket angezeigt werden, variiert abhängig vom Zielbetriebssystem und anderen Faktoren. 

Entwickler mit OEM-Berechtigungen können auf der Seite **Aktuelle Pakete** außerdem [Vorinstallationspakete generieren](generate-preinstall-packages-for-oems.md).

## <a name="wnsmpns"></a>WNS/MPNS

Die **WNS/MPNS** Abschnitt bietet Optionen zum Erstellen und Senden von Benachrichtigungen an Ihre app Kunden. 

> [!TIP]
> Für UWP-apps, es wird empfohlen die **Benachrichtigungen** Funktion im Partner Center. Dieses Feature ermöglicht Ihnen das Senden von Benachrichtigungen an alle Kunden mit Ihrer app oder auf einen entsprechenden Teil Ihrer Windows 10-Kunden, die die Kriterien erfüllen, die Sie in definiert haben eine [Kundensegment](create-customer-segments.md). Weitere Informationen finden Sie unter [Senden von Benachrichtigung an die Kunden Ihrer App](send-push-notifications-to-your-apps-customers.md).

Abhängig von Ihrer app-Paket-Typ und die spezifischen Anforderungen können Sie auch eine der folgenden Optionen verwenden: 

-   **Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)** ermöglichen es, Popup-, Kachel- und Badgeupdates sowie unformatierte Updates von ihren eigenen Clouddiensten aus zu senden. Weitere Informationen finden Sie unter [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Microsoft Azure Mobile Apps** ermöglichen das Senden von Pushbenachrichtigungen, die Authentifizierung und Verwaltung von App-Benutzern und das Speichern von App-Daten in der Cloud. Weitere Informationen finden Sie in der [Mobile Apps-Dokumentation](https://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Microsoft Push Notifications Service (MPNS)** kann mit zuvor veröffentlichte XAP-Paketen für Windows Phone verwendet werden. Sie können eine begrenzte Anzahl nicht authentifizierter Benachrichtigungen senden, ohne hier eine Konfiguration vorzunehmen. Zur Vermeidung von Drosselungslimits wird jedoch empfohlen, authentifizierte Benachrichtigungen zu verwenden. Wenn Sie MPNS verwenden, müssen Sie ein Zertifikat in das Feld auf Hochladen der **WNS/MPNS** Seite. Weitere Informationen finden Sie unter [Einrichten eines authentifizierten Webdiensts zum Senden von Pushbenachrichtigungen für Windows Phone 8](https://go.microsoft.com/fwlink/p/?LinkId=528736).
 

 
