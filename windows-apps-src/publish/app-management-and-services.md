---
Description: Verwalten und Anzeigen von Details im Zusammenhang mit den einzelnen apps in Partner Center und Konfigurieren von Diensten wie A/B-Tests und-Zuordnungen.
title: App-Verwaltung und -Dienste
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: e34a4d87ae3df23f8641a09d1558b8c8606667fa
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164284"
---
# <a name="app-management-and-services"></a>App-Verwaltung und -Dienste

Sie können Details im Zusammenhang mit den einzelnen apps in [Partner Center](https://partner.microsoft.com/dashboard)verwalten und anzeigen und Dienste wie Benachrichtigungen, A/B-Tests und Zuordnungen konfigurieren.

Wenn Sie mit einer APP in Partner Center arbeiten, sehen Sie die Abschnitte im linken Navigationsmenü für die **Dienste** und die **App-Verwaltung**. Sie können diese Abschnitte erweitern, um auf die unten beschriebenen Funktionen zuzugreifen.

## <a name="services"></a>Dienste

Im Abschnitt **Dienste** können Sie verschiedene Dienste für Ihre Apps verwalten.

## <a name="xbox-live"></a>Xbox Live

Wenn Sie ein Spiel veröffentlichen, können Sie das [Xbox Live Creators-Programm](https://www.xbox.com/developers/creators-program) auf dieser Seite aktivieren. Auf diese Weise können Sie die Xbox Live-Features konfigurieren und testen und schließlich Ihr Xbox Live Creators-Programm Spiel veröffentlichen.

Weitere Informationen finden Sie unter [Einstieg in das Xbox Live Creators-Programm](/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) und [Erstellen eines neuen Xbox Live Creators-Programm Titels und veröffentlichen in der Testumgebung](/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title).

## <a name="experimentation"></a>Experimentieren

Auf der Seite **Experimentation** können Sie Experimente mit A/B-Tests für Ihre Apps für die universelle Windows-Plattform (UWP) erstellen und ausführen. Bei einem A/B-Test wird die Effektivität von Featureänderungen (oder Abweichungen) in Ihrer App für einige Kunden ermittelt, bevor die Änderungen für die Allgemeinheit aktiviert werden.

Weitere Informationen finden Sie unter [Ausführen von App-Experimenten mit A/B-Tests](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Karten

Um Kartendienste in apps zu verwenden, die auf Windows 10 oder Windows 8. x abzielen, besuchen Sie das Windows 10-Entwickler [Center](https://www.bingmapsportal.com/). Informationen dazu, wie Sie einen Zuordnungs-Authentifizierungsschlüssel aus dem Entwickler Center für den Kartendienst von Maps anfordern und zu Ihrer APP hinzufügen, finden Sie unter [Anfordern eines Zuordnungs Authentifizierungs Schlüssels](../maps-and-location/authentication-key.md) . 

Verwenden Sie die Seite **Maps** nur für zuvor veröffentlichte Apps für Windows Phone 8,1 und früher. Um Kartendienste in diesen apps zu verwenden, müssen Sie eine Zuordnungs Dienst-Anwendungs-ID und ein Token anfordern, die in den Code Ihrer APP eingeschlossen werden sollen. Wenn Sie auf **Token erhalten**klicken, generieren wir eine Zuordnungs Dienst-Anwendungs-ID (**ApplicationId**) und ein Kartendienst-Authentifizierungs Token (**authenticationtoken**) für Ihre APP. Stellen Sie sicher, dass Sie diese Werte dem Code hinzufügen, bevor Sie Ihre APP packen und übermitteln. Weitere Informationen finden Sie unter [Hinzufügen eines Kartensteuerelements zu einer Seite (Windows Phone 8.1)](/previous-versions/windows/apps/jj207033(v=vs.105)).

## <a name="product-collections-and-purchases"></a>Produktsammlungen und Einkäufe

Wenn Sie die Microsoft Store Sammlungs-API und die Microsoft Store Purchase-API für den Zugriff auf Besitz Informationen für apps und Add-ons verwenden möchten, müssen Sie hier die zugehörigen Azure AD Client-IDs eingeben. Beachten Sie, dass es bis zu 16 Stunden dauern kann, bis diese Änderungen wirksam werden.

Weitere Informationen finden Sie unter [Verwalten von Produkt Berechtigungen von einem Dienst](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Zustimmung des Administrators

Wenn Ihr Produkt in Azure AD integriert ist und APIs aufruft, die entweder [Anwendungs Berechtigungen oder Delegierte Berechtigungen](/graph/permissions-reference) anfordern, die Zustimmung des Administrators erfordern, geben Sie hier Ihre Azure AD Client-ID ein. Dadurch können Administratoren, die die APP für Ihre Organisation erwerben, Ihre Zustimmung für Ihr Produkt erteilen, um im Namen aller Benutzer im Mandanten zu agieren.

Weitere Informationen finden Sie im Abschnitt zum [Anfordern der Zustimmung für einen gesamten Mandanten](/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>App-Verwaltung

Im Abschnitt **App-Verwaltung** können Sie Identitäts-und Paket Details anzeigen und Namen für Ihre APP verwalten.

## <a name="app-identity"></a>App-Identität

Diese Seite enthält die Details zur eindeutigen App-Identität im Store, einschließlich den URL(s) zur Verknüpfung der App mit dem App-Eintrag.

Weitere Informationen finden Sie unter [Anzeigen von Details zur App-Identität](view-app-identity-details.md).

## <a name="manage-app-names"></a>Verwalten von App-Namen

Auf dieser Seite können Sie alle für Ihre App reservierten Namen anzeigen. Sie können hier auch zusätzliche Namen reservieren oder nicht mehr verwendete Namen löschen.

Weitere Informationen finden Sie unter [Verwalten von App-Namen](manage-app-names.md).

## <a name="current-packages"></a>Aktuelle Pakete

Auf dieser Seite können Sie Details zu allen veröffentlichten Paketen anzeigen.

> [!NOTE]
> Hier werden keine Informationen angezeigt, bis Ihre App veröffentlicht wurde.

Der Name, die Version und die Architektur der einzelnen Pakete werden angezeigt. Klicken Sie auf **Details**, um zusätzliche Informationen wie z. B. unterstützte Sprache, App-Funktionen und Dateigrößen anzuzeigen. Welche Informationen jeweils für ein Paket angezeigt werden, variiert abhängig vom Zielbetriebssystem und anderen Faktoren. 

Entwickler mit OEM-Berechtigungen können auf der Seite **Aktuelle Pakete** außerdem [Vorinstallationspakete generieren](generate-preinstall-packages-for-oems.md).

## <a name="wnsmpns"></a>WNS/mpns

Der Abschnitt **WNS/mpns** bietet Optionen zum Erstellen und Senden von Benachrichtigungen an die Kunden Ihrer APP. 

> [!TIP]
> Für UWP-apps empfiehlt es sich, das **Benachrichtigungs** Feature in Partner Center zu verwenden. Diese Funktion ermöglicht Ihnen das Senden von Benachrichtigungen an alle Kunden der APP oder an eine bestimmte Teilmenge ihrer Windows 10-Kunden, die die Kriterien erfüllen, die Sie in einem [Kundensegment](create-customer-segments.md)definiert haben. Weitere Informationen finden Sie unter [Senden von Benachrichtigungen an die Kunden Ihrer APP](send-push-notifications-to-your-apps-customers.md).

Abhängig vom Pakettyp Ihrer APP und den jeweiligen Anforderungen können Sie auch eine der folgenden Optionen verwenden: 

-   **Windows-Pushbenachrichtigungsdienste (Windows Push Notification Services, WNS)** ermöglichen es, Popup-, Kachel- und Badgeupdates sowie unformatierte Updates von ihren eigenen Clouddiensten aus zu senden. Weitere Informationen finden Sie unter [Übersicht über den Windows-Pushbenachrichtigungsdienst (Windows Push Notification Service, WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Microsoft Azure Mobile Apps** ermöglichen das Senden von Pushbenachrichtigungen, die Authentifizierung und Verwaltung von App-Benutzern und das Speichern von App-Daten in der Cloud. Weitere Informationen finden Sie in der [Mobile Apps-Dokumentation](/azure/app-service-mobile/).

-   Der **Microsoft-pushbenachrichtigungsdienst (mpns)** kann mit zuvor veröffentlichten XAP-Paketen für Windows Phone verwendet werden. Sie können eine begrenzte Anzahl nicht authentifizierter Benachrichtigungen senden, ohne hier eine Konfiguration vorzunehmen. Zur Vermeidung von Drosselungslimits wird jedoch empfohlen, authentifizierte Benachrichtigungen zu verwenden. Wenn Sie mpns verwenden, müssen Sie ein Zertifikat in das auf der Seite **WNS/mpns** angegebene Feld hochladen. Weitere Informationen finden Sie unter [Einrichten eines authentifizierten Webdiensts zum Senden von Pushbenachrichtigungen für Windows Phone 8](/previous-versions/windows/apps/ff941099(v=vs.105)).
 

 