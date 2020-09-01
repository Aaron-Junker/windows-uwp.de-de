---
Description: Wenn Sie ein neues Add-on in Partner Center erstellen, müssen Sie einen Produkttyp angeben und ihm eine Produkt-ID zuweisen.
title: Festlegen von Produkt-ID und Produkttyp für das Add-On
ms.assetid: 59497B0F-82F0-4CEE-B628-040EF9ED8D3D
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Add-ons, IAP, durable, consudbar, Abonnement, Produkttyp, Produkt-ID, in-App-Käufe, in-App-Produkt
ms.localizationpriority: medium
ms.openlocfilehash: f62ac8c7ad366505d6730493212e7f8ba588012f
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89170994"
---
# <a name="set-your-add-on-product-type-and-product-id"></a>Festlegen von Produkt-ID und Produkttyp für das Add-On

Ein Add-on muss einer APP zugeordnet werden, die Sie im [Partner Center](https://partner.microsoft.com/dashboard) erstellt haben (auch wenn Sie es noch nicht übermittelt haben). Sie finden die Schaltfläche, um **ein neues Add-on** auf der **Übersichts** Seite Ihrer APP oder auf der Seite " **Add-ons** " zu erstellen.

Nachdem Sie **Neues Add-on erstellen**ausgewählt haben, werden Sie aufgefordert, einen Produkttyp anzugeben und eine Produkt-ID für das Add-on zuzuweisen.

## <a name="product-type"></a>Produkttyp

Zunächst müssen Sie angeben, welche Art von Add-On Sie anbieten. Diese Auswahloption bezieht sich darauf, wie das Add-On vom Kunden genutzt werden kann.

> [!NOTE]
> Sie können den Produkttyp nicht ändern, nachdem Sie diese Seite gespeichert haben, um das Add-on zu erstellen. Wenn Sie den falschen Produkttyp auswählen, können Sie jederzeit ihre laufende Add-on-Übermittlung löschen und beginnen, indem Sie ein neues Add-on erstellen.

<span id="durable" />

### <a name="durable"></a>Durable

Wählen Sie **Durable** als Produkttyp aus, wenn das Add-on in der Regel nur einmal gekauft wird. Diese Add-ons werden häufig verwendet, um zusätzliche Funktionen in einer APP freizugeben.

Die standardmäßige **Produktlebenszeit** dauerhafter Add-Ons ist **Unbegrenzt**. Das Add-On läuft also niemals ab. Sie haben die Möglichkeit, die **Produktlebensdauer** im Schritt " [Eigenschaften](enter-add-on-properties.md) " des Add-on-Übermittlungs Vorgangs auf eine andere Dauer festzulegen. Wenn Sie dies tun, läuft das Add-on nach der von Ihnen angegebenen Dauer ab (mit Optionen von 1-365 Tagen). in diesem Fall kann der Kunde ihn nach Ablauf wieder kaufen.

### <a name="consumable"></a>Nutzbar

Wenn das Add-on gekauft, verwendet (genutzt) und dann erneut gekauft werden kann, sollten Sie **einen der verwendbaren Produkttypen** auswählen. Konsumierbare Add-Ons bzw. Endverbraucher-Add-Ons werden häufig für Dinge wie spielinterne Währungen (Gold, Münzen usw.) verwendet, die in bestimmten Mengen erworben und vom Kunden aufgebraucht werden. Weitere Informationen finden Sie unter Aktivieren von nutzbaren [Add-on-Käufen](../monetize/enable-consumable-add-on-purchases.md).

Es gibt zwei Arten von nutzbaren Add-ons:
- Vom **Entwickler verwaltetes**nutzbares Konto: Balance und Erfüllung müssen innerhalb Ihrer APP verwaltet werden. Wird unter allen Betriebssystemversionen unterstützt.
- **Vom Store verwaltetes Endverbraucher-Add-On:** Der Saldo wird von Microsoft für alle Geräte des Kunden verfolgt, auf denen Windows 10 (Version 1607 oder höher) ausgeführt wird; nicht unterstützt unter früheren Betriebssystemversionen. Um diese Option zu verwenden, muss das übergeordnete Produkt mit Windows 10 SDK Version 14393 oder höher kompiliert werden. Beachten Sie außerdem, dass Sie ein vom Store verwaltetes, nutzbares Add-on nicht an den Store übermitteln können, bis das übergeordnete Produkt veröffentlicht wurde (obwohl Sie die Übermittlung im Partner Center erstellen und jederzeit daran arbeiten können). Sie müssen die Menge für das von Filialen verwaltete, nutzbare Add-on im **Eigenschaften** Schritt ihrer Übermittlung eingeben.

### <a name="subscription"></a>Subscription

Wenn Sie Kunden regelmäßig für Ihr Add-on berechnen möchten, wählen Sie **Abonnement**aus.

Nachdem ein Abonnement zuerst von einem Kunden abgerufen wurde, wird es weiterhin zu wiederkehrenden Intervallen abgerechnet, damit das Add-on weiterhin verwendet werden kann. Der Kunde kann das Abonnement jederzeit kündigen, um weitere Kosten zu vermeiden. Sie müssen den Abonnementzeitraum angeben und angeben, ob eine kostenlose Testversion im **Eigenschaften** Schritt ihrer Übermittlung angeboten werden soll.

Abonnement-Add-ons werden nur für Kunden unterstützt, auf denen Windows 10, Version 1607 oder höher, ausgeführt wird. Die übergeordnete app muss mit dem Windows 10 SDK, Version 14393 oder höher, kompiliert werden, und Sie muss die in-App-Kauf-API im **Windows. Services. Store** -Namespace anstelle des **Windows. applicationmodel. Store** -Namespace verwenden. Weitere Informationen finden Sie unter [Aktivieren von Add-ons für Abonnements für Ihre APP](../monetize/enable-subscription-add-ons-for-your-app.md).

Sie müssen das übergeordnete Produkt übermitteln, bevor Sie Abonnement-Add-ons im Store veröffentlichen können (Sie können die Übermittlung jedoch jederzeit im Partner Center erstellen und jederzeit daran arbeiten).

## <a name="product-id"></a>Product ID

Unabhängig vom gewählten Produkttyp müssen Sie eine eindeutige Produkt-ID für das Add-on eingeben. Dieser Name wird verwendet, um das Add-on im Partner Center zu identifizieren, und Sie können diesen Bezeichner verwenden, um [auf das Add-on in Ihrem Code zu verweisen](../monetize/in-app-purchases-and-trials.md#how-to-use-product-ids-for-add-ons-in-your-code).

Folgende Dinge sollten bei der Wahl einer Produkt-ID beachtet werden:

-   Eine Produkt-ID muss innerhalb des übergeordneten Produkts eindeutig sein.
-   Sie können die Produkt-ID eines Add-Ons nach der Veröffentlichung nicht ändern oder löschen.
-   Eine Produkt-ID darf maximal 100 Zeichen umfassen.
-   Eine Produkt-ID darf keines der folgenden Zeichen enthalten: ** &lt; &gt; \* % &: \\ ? +,**
-   Die Produkt-ID wird den Kunden nicht angezeigt. (Sie können später [einen Titel und eine Beschreibung](./create-app-store-listings.md) eingeben, die für Kunden sichtbar sind.)
-   Wenn Ihre zuvor veröffentlichte APP Windows Phone 8,1 oder früher unterstützt, dürfen Sie nur alphanumerische Zeichen, Punkte und/oder Unterstriche in der Produkt-ID verwenden. Bei Verwendung anderer Zeichen kann das Add-On von Kunden mit Windows Phone 8.1 oder früheren Versionen nicht erworben werden.

 