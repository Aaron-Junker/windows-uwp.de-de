---
Description: Add-ons (oder in-App-Produkte) werden über Partner Center veröffentlicht.
title: Add-On-Übermittlungen
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, IAP, In-App-Kauf, In-App-Produkt, IAP-Übermittlung
ms.localizationpriority: medium
ms.openlocfilehash: 87e5298f677a078eb1d6f89b72af3ea15d44b75f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259059"
---
# <a name="add-on-submissions"></a>Add-On-Übermittlungen

Add-Ons (auch als In-App-Produkte bezeichnet) sind ergänzende Elemente für Ihre App, die von Kunden erworben werden können. Ein Add-on kann ein interessantes neues Feature, eine neue Spielebene oder etwas anderes sein, was Sie denken. Add-Ons sind nicht nur eine gute Möglichkeit, um Geld zu verdienen, sondern fördern zudem die Kundeninteraktion und -bindung.

Add-ons werden über [Partner Center](https://partner.microsoft.com/dashboard)veröffentlicht und erfordern ein aktives [Entwicklerkonto](https://developer.microsoft.com/store/register). Sie müssen die [Add-Ons außerdem im Code Ihrer App aktivieren](../monetize/in-app-purchases-and-trials.md).

Der erste Schritt im Add-on-Übermittlungs Prozess besteht darin, das Add-on in Partner Center zu erstellen, indem der [Produkttyp und die Produkt-ID definiert](set-your-add-on-product-id.md)werden. Anschließend erstellen Sie eine Übermittlung, damit das Add-on über das Microsoft Store erworben werden kann. Sie können ein Add-On gleichzeitig mit [Ihrer App einreichen](app-submissions.md) oder unabhängig vorgehen. Außerdem können Sie [Updates](#updating-an-add-on-after-publication) für Add-Ons ausführen, nachdem die App im Store eingetragen wurde, ohne dass die App erneut übermittelt werden muss.

> [!NOTE]
> In diesem Abschnitt der Dokumentation wird beschrieben, wie Sie Add-ons im Partner Center übermitteln. Alternativ dazu können Sie auch die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) verwenden, um Add-On-Übermittlungen zu automatisieren.


## <a name="checklist-for-submitting-an-add-on"></a>Prüfliste für die Übermittlung eines Add-Ons

Hier finden Sie eine Liste mit den Informationen, die Sie beim Erstellen Ihrer Add-On-Übermittlung angeben können. Die erforderlichen Elemente sind im Folgenden aufgeführt. Einige Elemente sind optional oder verfügen bereits über Standardwerte, die Sie nach Bedarf ändern können.


### <a name="create-a-new-add-on-page"></a>Erstellen einer neuen Add-On-Seite

| Name des Felds                    | Anmerkungen                            |
|-------------------------------|----------------------------------|
| [**Produkttyp**](set-your-add-on-product-id.md#product-type)      | Erforderlich |  
| [**Produkt-ID**](set-your-add-on-product-id.md#product-id)          | Erforderlich |        


### <a name="properties-page"></a>Seite „Eigenschaften“

| Name des Felds                    | Anmerkungen                              |   
|-------------------------------|------------------------------------|
| [**Produktlebensdauer**](enter-add-on-properties.md#product-lifetime)  | Erforderlich, wenn der Produkttyp **Gebrauchsgut** lautet. Gilt nicht für andere Produkttypen. |
| [**Mess**](enter-add-on-properties.md#quantity)  | Erforderlich, wenn der Produkttyp **Vom Store verwalteter Verbrauchsartikel** lautet. Gilt nicht für andere Produkttypen. |
| [**Abonnementzeitraum**](enter-add-on-properties.md#subscription-period)          | Erforderlich, wenn der Produkttyp **Dauerauftrag** lautet. Gilt nicht für andere Produkttypen.       |  
| [**Kostenlose Testversion**](enter-add-on-properties.md#free-trial)          | Erforderlich, wenn der Produkttyp **Dauerauftrag** lautet. Gilt nicht für andere Produkttypen.       |
| [**Inhaltstyp**](enter-add-on-properties.md#content-type)          | Erforderlich    |               
| [**Keywords**](enter-add-on-properties.md#keywords)                  | Optional (bis zu zehn Schlüsselwörter mit jeweils maximal 30 Zeichen) |
| [**Benutzerdefinierte Entwickler Daten**](enter-add-on-properties.md#custom-developer-data)   | Optional (maximal 3.000 Zeichen)            |


### <a name="pricing-and-availability-page"></a>Seite „Preise und Verfügbarkeit“

| Name des Felds                    | Anmerkungen                                       |
|-------------------------------|---------------------------------------------|
| [**Agrar**](set-add-on-pricing-and-availability.md#markets)  | Standard: alle möglichen Märkte |
| [**Transparenz**](set-add-on-pricing-and-availability.md#visibility)   | Standard: Zum Kauf erhältlich. Kann im App-Eintrag angezeigt werden |
| [**Vereinbaren**](set-add-on-pricing-and-availability.md#schedule)    | Standard: so schnell wie möglich veröffentlichen
| [**SP**](set-add-on-pricing-and-availability.md#pricing)                | Erforderlich                                    |
| [**Verkaufspreise**](put-apps-and-add-ons-on-sale.md)               | Optional                    |


### <a name="store-listings"></a>Store-Einträge

Ein Store-Eintrag ist erforderlich. Es wird empfohlen, für jede von der App unterstützte [Sprache](create-add-on-store-listings.md#store-listing-languages) Store-Einträge anzugeben.

| Name des Felds                    | Anmerkungen                                       |
|-------------------------------|---------------------------------------------|
| [**Tel**](create-add-on-store-listings.md#title)                    | Erforderlich (max. 100 Zeichen)           |
| [**Beschreibung**](create-add-on-store-listings.md#description)       | Optional (max. 200 Zeichen)            |
| [**Angezeigt**](create-add-on-store-listings.md#icon)                    | Optional (PNG, 300x300 Pixel)            |


Nachdem Sie diese Informationen eingegeben haben, klicken Sie auf **An Store einreichen**. In den meisten Fällen dauert der Zertifizierungsprozess etwa eine Stunde. Danach wird Ihr Add-On im Store veröffentlicht und steht für Kunden zum Kauf bereit.

> [!NOTE]
> Das Add-On muss außerdem im Code Ihrer App implementiert werden. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](../monetize/in-app-purchases-and-trials.md).


## <a name="updating-an-add-on-after-publication"></a>Aktualisieren eines Add-Ons nach der Veröffentlichung

Sie können ein veröffentlichtes Add-On jederzeit ändern. Add-on-Änderungen werden unabhängig von Ihrer APP gesendet und veröffentlicht, sodass Sie in der Regel nicht die gesamte APP aktualisieren müssen, um Änderungen an einem Add-on vorzunehmen, wie z. b. das Aktualisieren des Preises oder der Beschreibung.

Zum Übermitteln von Updates wechseln Sie zur Seite des Add-ons im Partner Center, und klicken Sie auf **Aktualisieren**. Dadurch wird eine neue Übermittlung für das Add-on erstellt, wobei die Informationen aus der vorherigen Übermittlung als Ausgangspunkt verwendet werden. Nehmen Sie die gewünschten Änderungen vor, und klicken Sie dann **auf an den Store senden**.

Wenn Sie ein zuvor angebotenes Add-On entfernen möchten, können Sie dies tun, indem Sie eine neue Übermittlung erstellen und die Option [Verteilung und Sichtbarkeit](set-add-on-pricing-and-availability.md) unter **Im Store ausgeblendet** in **Beenden des Erwerbs**. Achten Sie darauf, dass Sie den Code Ihrer APP nach Bedarf aktualisieren, um auch Verweise auf das Add-on zu entfernen (insbesondere, wenn Ihre zuvor veröffentlichte App bereits Windows 8.1 unterstützt; diese Sichtbarkeits Einstellung gilt nicht für diese Kunden).

> [!IMPORTANT]
> Wenn Ihre bereits veröffentlichte App für Kunden unter Windows 8. x verfügbar ist, müssen Sie eine neue APP-Übermittlung erstellen und veröffentlichen, um die Add-on-Updates für diese Kunden sichtbar zu machen. Auch wenn Sie neue Add-Ons einer App für Windows 8.x hinzufügen, nachdem die App veröffentlicht wurde, müssen Sie den App-Code aktualisieren, um auf diese Add-Ons zu verweisen, und die App dann erneut übermitteln. Andernfalls sind die neuen Add-Ons nicht für Kunden unter Windows 8.x sichtbar.
