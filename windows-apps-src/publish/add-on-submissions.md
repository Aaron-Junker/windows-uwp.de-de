---
Description: Add-Ons (oder in-app-Produkte) werden über das Partner Center veröffentlicht.
title: Add-On-Übermittlungen
ms.assetid: E175AF9E-A1D4-45DF-B353-5E24E573AE67
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, IAP, In-App-Kauf, In-App-Produkt, IAP-Übermittlung
ms.localizationpriority: medium
ms.openlocfilehash: 28383ed82c418ff15806c325d6eab5a05f9987bf
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620365"
---
# <a name="add-on-submissions"></a>Add-On-Übermittlungen

Add-Ons (auch als In-App-Produkte bezeichnet) sind ergänzende Elemente für Ihre App, die von Kunden erworben werden können. Ein Add-on kann es sich um ein unterhaltsames sein, die neue Funktion, ein neues Spiel auf oder gibt es eine andere Meinung Benutzer interessiert bleiben. Add-Ons sind nicht nur eine gute Möglichkeit, um Geld zu verdienen, sondern fördern zudem die Kundeninteraktion und -bindung.

Add-Ons über veröffentlicht werden [Partner Center](https://partner.microsoft.com/dashboard), müssen Sie ein aktives [Entwicklerkonto](https://go.microsoft.com/fwlink/p/?LinkId=615100). Sie müssen die [Add-Ons außerdem im Code Ihrer App aktivieren](../monetize/in-app-purchases-and-trials.md).

Der erste Schritt in der Add-on-Übermittlungsprozess ist die Erstellung des Add-Ons im Partner Center von [die Produkttyp und die Produkt-ID definieren](set-your-add-on-product-id.md). Anschließend erstellen Sie eine Eingabe, damit das Add-on über den Microsoft Store erworben werden kann. Sie können ein Add-On gleichzeitig mit [Ihrer App einreichen](app-submissions.md) oder unabhängig vorgehen. Außerdem können Sie [Updates](#updating-an-add-on-after-publication) für Add-Ons ausführen, nachdem die App im Store eingetragen wurde, ohne dass die App erneut übermittelt werden muss.

> [!NOTE]
> In diesem Abschnitt der Dokumentation wird beschrieben, wie im Partner Center-Add-Ons zu übermitteln. Alternativ dazu können Sie auch die [Microsoft Store-Übermittlungs-API](../monetize/create-and-manage-submissions-using-windows-store-services.md) verwenden, um Add-On-Übermittlungen zu automatisieren.


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
| [**Produktlebenszyklus**](enter-add-on-properties.md#product-lifetime)  | Erforderlich, wenn der Produkttyp **Gebrauchsgut** lautet. Gilt nicht für andere Produkttypen. |
| [**Menge**](enter-add-on-properties.md#quantity)  | Erforderlich, wenn der Produkttyp **Vom Store verwalteter Verbrauchsartikel** lautet. Gilt nicht für andere Produkttypen. |
| [**Abonnementzeitraum**](enter-add-on-properties.md#subscription-period)          | Erforderlich, wenn der Produkttyp **Dauerauftrag** lautet. Gilt nicht für andere Produkttypen.       |  
| [**Kostenlose Testversion**](enter-add-on-properties.md#free-trial)          | Erforderlich, wenn der Produkttyp **Dauerauftrag** lautet. Gilt nicht für andere Produkttypen.       |
| [**Inhaltstyp**](enter-add-on-properties.md#content-type)          | Erforderlich    |               
| [**Keywords**](enter-add-on-properties.md#keywords)                  | Optional (bis zu zehn Schlüsselwörter mit jeweils maximal 30 Zeichen) |
| [**Benutzerdefinierte Entwickler-Daten**](enter-add-on-properties.md#custom-developer-data)   | Optional (maximal 3.000 Zeichen)            |


### <a name="pricing-and-availability-page"></a>Seite „Preise und Verfügbarkeit“

| Name des Felds                    | Anmerkungen                                       |
|-------------------------------|---------------------------------------------|
| [**Märkte**](set-add-on-pricing-and-availability.md#markets)  | Standardwert: Alle möglichen Märkte |
| [**Sichtbarkeit**](set-add-on-pricing-and-availability.md#visibility)   | Standardwert: Erhältlich. Kann im App-Eintrag angezeigt werden |
| [**Zeitplan**](set-add-on-pricing-and-availability.md#schedule)    | Standardwert: Version so bald wie möglich
| [**– Preise**](set-add-on-pricing-and-availability.md#pricing)                | Erforderlich                                    |
| [**Verkaufs-Preise**](put-apps-and-add-ons-on-sale.md)               | Optional                    |


### <a name="store-listings"></a>Store-Einträge

Ein Store-Eintrag ist erforderlich. Es wird empfohlen, für jede von der App unterstützte [Sprache](create-add-on-store-listings.md#store-listing-languages) Store-Einträge anzugeben.

| Name des Felds                    | Anmerkungen                                       |
|-------------------------------|---------------------------------------------|
| [**Titel**](create-add-on-store-listings.md#title)                    | Erforderlich (max. 100 Zeichen)           |
| [**Beschreibung**](create-add-on-store-listings.md#description)       | Optional (max. 200 Zeichen)            |
| [**Icon**](create-add-on-store-listings.md#icon)                    | Optional (PNG, 300x300 Pixel)            |


Nachdem Sie diese Informationen eingegeben haben, klicken Sie auf **An Store einreichen**. In den meisten Fällen dauert der Zertifizierungsprozess etwa eine Stunde. Danach wird Ihr Add-On im Store veröffentlicht und steht für Kunden zum Kauf bereit.

> [!NOTE]
> Das Add-On muss außerdem im Code Ihrer App implementiert werden. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](../monetize/in-app-purchases-and-trials.md).


## <a name="updating-an-add-on-after-publication"></a>Aktualisieren eines Add-Ons nach der Veröffentlichung

Sie können ein veröffentlichtes Add-On jederzeit ändern. Add-On-Änderungen übermittelt und unabhängig von Ihrer app veröffentlicht wird, damit Sie nicht in der Regel die gesamte app zu aktualisieren, um ein Add-on wie das Aktualisieren der Preis oder die Beschreibung ändern müssen.

Um Updates zu senden, fahren Sie mit der hinzufügen-auf der Seite im Partner Center, und klicken Sie auf **Update**. Hiermit wird eine neue Eingabe für das Add-on, mit den Informationen aus Ihrer vorherigen Übermittlung als Ausgangspunkt. Nehmen Sie die Änderungen, die Sie möchten, und klicken Sie dann auf **an den Store übermitteln**.

Wenn Sie ein zuvor angebotenes Add-On entfernen möchten, können Sie dies tun, indem Sie eine neue Übermittlung erstellen und die Option [Verteilung und Sichtbarkeit](set-add-on-pricing-and-availability.md) unter **Im Store ausgeblendet** in **Beenden des Erwerbs**. Achten Sie darauf, um Code Ihrer app zu aktualisieren, je nach Bedarf auch Verweise auf das Add-on entfernen (insbesondere dann, wenn Ihre app zuvor veröffentlichte Windows 8.1 unterstützt, die früher – diese sichtbarkeitseinstellung wird nicht gelten für Kunden).

> [!IMPORTANT]
> Wenn Ihre app zuvor veröffentlichte für Kunden mit Windows verfügbar ist 8.x, Sie benötigen zum Erstellen und veröffentlichen eine neue app senden, um die Add-on-Updates für die Kunden sichtbar zu machen. Auch wenn Sie neue Add-Ons einer App für Windows 8.x hinzufügen, nachdem die App veröffentlicht wurde, müssen Sie den App-Code aktualisieren, um auf diese Add-Ons zu verweisen, und die App dann erneut übermitteln. Andernfalls sind die neuen Add-Ons nicht für Kunden unter Windows 8.x sichtbar.
