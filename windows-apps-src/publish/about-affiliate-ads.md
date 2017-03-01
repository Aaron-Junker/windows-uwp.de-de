---
author: jnHs
Description: "Wenn Ihre App ein AdMediatorControl- oder ein AdControl-Element zum Anzeigen von Werbebannern verwendet, können Sie Ihre Anzeigenfüllrate und Ihren Umsatz steigern, indem Sie in Ihrer App Microsoft-Partneranzeigen anzeigen."
title: Informationen zu Partneranzeigen
ms.assetid: 7B5478FB-7E68-4956-82EF-B43C2873E3EF
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c114b7fa5c0e6a613a2c5485903d856592f0863e
ms.lasthandoff: 02/07/2017

---

# <a name="about-affiliate-ads"></a>Informationen zu Partneranzeigen

Wenn Ihre App ein **AdMediatorControl**- oder **AdControl**-Element zum Anzeigen von Werbebannern verwendet, können Sie Ihre Anzeigenfüllrate und Ihren Umsatz steigern, indem Sie in Ihrer App Microsoft-Partneranzeigen für Store-Produkte anzeigen. Wenn Benutzer auf die Partneranzeigen klicken und innerhalb eines bestimmten Attributionsfensters Produkte kaufen, generieren Sie dadurch Umsätze für genehmigte Käufe.

Das funktioniert wie folgt:

* Nachdem Sie sich im Dev Center für die [Teilnahme am Microsoft-Partneranzeigenprogramm](#opt-in) entschieden haben, wählt Microsoft Anzeigen für Top-Produkte aus dem Store aus, die dann in Ihrer App angezeigt werden. Bei den Produkten kann es sich um Apps, Spiele, Musik, Filme, Hardware oder Software handeln.

 > **Hinweis**  Microsoft stellt nur Partneranzeigen in der folgenden Größe zur Verfügung: 300 x 50, 480 x 80, 160 x 600, 300 x 250 oder 728 x 90.

* Microsoft stellt Ihrer App nur dann Partneranzeigen zur Verfügung, wenn keine Anzeigen anderer Anzeigennetzwerke verfügbar sind. Dadurch können Sie Ihre nicht erfüllten Anzeigenaufrufe monetisieren und Ihre Anzeigenfüllrate maximieren.
* Wenn ein Benutzer auf eine Partneranzeige klickt und innerhalb eines bestimmten Attributionsfensters ein Produkt im Store kauft, erhalten Sie entweder eine Umsatzbeteiligung oder eine feste Kommission für den Kauf (bis zu 10 Prozent).

  Damit eine Partneranzeige kommissionsberechtig ist, muss sich aus der Partneranzeige ein berechtigter Verkauf im Store auf Windows 10-Geräten oder im webbasierten Store ergeben. Verkäufe im Store auf Windows 8.x-Geräte sind nicht kommissionsberechtigt. Partneranzeigen für digitale Store Produkte (einschließlich Apps, Spiele, Musik und Filme) werden nur auf Windows 10-Geräten bereitgestellt. Partneranzeigen für Produkte im webbasierten Store (einschließlich Geräte und Software) werden auf Windows 10- und Windows 8.x-Geräten bereitgestellt.

    > **Hinweis**  Sie können für *jedes* Produkt bezahlt werden, das der Benutzer innerhalb des Attributionsfensters kauft (nicht nur für das in Ihrer App beworbene Produkt). Bei kostenlosen Apps, die in Ihrer App beworben werden, können Sie am Umsatz der In-App-Einkäufe beteiligt werden, die der Benutzer innerhalb des Attributionsfensters tätigt.

* Alle Einnahmen, die Sie in Verbindung mit dem Microsoft-Partneranzeigenprogramm erzielen, werden zusammen mit Ihren Microsoft Advertising-Einnahmen dem [im Dev Center eingerichteten Auszahlungskonto](setting-up-your-payout-account-and-tax-forms.md) gutgeschrieben.
* Die Performance der Partneranzeigen in Ihrer App können Sie im [Bericht zur Partnerleistung](affiliates-performance-report.md) ermitteln. Sie können die Einkäufe eines Tages, die über Partneranzeigen in Ihrer App getätigt wurden, sowie die erhaltene Umsatzbeteiligung nachverfolgen.  

  > **Hinweis**  Nachdem ein Benutzer ein Produkt im Store gekauft hat, gibt es eine Wartezeit von 45 Tagen, ehe der Kauf für das Partneranzeigenprogramm genehmigt werden kann. Aufgrund dieser Frist können die Daten für **Geschätzten Umsatz**, **Käufe (genehmigt)** und **Käufe (Genehmigung ausstehend)** im [Bericht zur Partnerleistung](affiliates-performance-report.md) für einen bestimmten Tag sich ändern, nachdem Einkäufe genehmigt oder abgelehnt werden.

<span id="opt-in" />
## <a name="how-to-opt-in-to-the-affiliate-ads-program"></a>So melden Sie sich für das Partneranzeigenprogramm an

So melden Sie sich für das Microsoft-Partneranzeigenprogramm an:

1. Navigieren Sie im Windows Dev Center-Dashboard zu **Monetarisierung** &gt; **Gewinnbringende Nutzung mit Anzeigen**.
2. Aktivieren Sie im Abschnitt **Microsoft affiliate ads** das Kontrollkästchen **Show Microsoft affiliate ads in my app**.

Nach dem Aktivieren oder Deaktivieren dieses Kontrollkästchens müssen Sie Ihre App nicht neu veröffentlichen, damit die Änderungen wirksam werden.


## <a name="related-topics"></a>Verwandte Themen


* [Monetisierung durch Werbeanzeigen](monetize-with-ads.md)
* [Bericht zur Partnerleistung](affiliates-performance-report.md)

