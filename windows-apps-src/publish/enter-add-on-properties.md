---
Description: Beim Übermitteln eines Add-Ons sind die Optionen auf der Seite „Eigenschaften“ hilfreich, um das Verhalten Ihres Add-Ons festzulegen, wenn es Kunden angeboten wird.
title: Eingeben von Add-On-Eigenschaften
ms.assetid: 26D2139F-66FD-479E-940B-7491238ADCAE
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Add-on, Eigenschaften, Abonnementzeitraum, Produktlebensdauer, Inhaltstyp, IAP, in-App-Käufe, in-App-Produkt
ms.localizationpriority: medium
ms.openlocfilehash: 9d092f443ab643b74cdd0221c96540fed0c7d474
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89157994"
---
# <a name="enter-add-on-properties"></a>Eingeben von Add-On-Eigenschaften

Wenn Sie ein Add-on senden, können Sie mithilfe der Optionen auf der Seite **Eigenschaften** das Verhalten des Add-on ermitteln, wenn es Kunden angeboten wird.

## <a name="product-type"></a>Produkttyp

Beim ersten [Erstellen des Add-Ons](set-your-add-on-product-id.md) wird der Produkttyp ausgewählt. Der ausgewählte Produkttyp wird hier angezeigt, Sie können ihn jedoch nicht ändern.

> [!TIP]
> Wenn Sie das Add-on noch nicht veröffentlicht haben, können Sie die Übermittlung löschen und erneut starten, wenn Sie einen anderen Produkttyp auswählen möchten.

Die Felder, die auf dieser Seite angezeigt werden, variieren je nach Produkttyp des Add-on.


## <a name="product-lifetime"></a>Produktlebensdauer

Wenn Sie **Durable** für Ihren Produkttyp ausgewählt haben, wird hier die **Produktlebensdauer** angezeigt. Die standardmäßige **Produktlebenszeit** dauerhafter Add-Ons ist **Unbegrenzt**. Das Add-On läuft also niemals ab. Wenn Sie möchten, können Sie die **Produktlebensdauer** so ändern, dass das Add-on nach einer festgelegten Dauer (mit Optionen von 1-365 Tagen) abläuft.


## <a name="quantity"></a>Quantity (Menge)

Wenn Sie für Ihren Produkttyp **Store-verwaltete verwendbar** ausgewählt haben, wird hier die **Menge** angezeigt. Sie müssen eine Zahl zwischen 1 und 1000000 eingeben. Diese Menge wird Kunden gewährt, wenn sie Ihr Add-On erwerben, und vom Store wird der Betrag nachverfolgt, wenn die App die Nutzung des Add-Ons durch Kunden meldet.


## <a name="subscription-period"></a>Abonnementzeitraum

Wenn Sie ein **Abonnement** für Ihren Produkttyp ausgewählt haben, wird hier der **Abonnementzeitraum** angezeigt. Wählen Sie eine Option aus, um anzugeben, wie häufig ein Kunde für das Abonnement in Rechnung gestellt wird. Die Standardoption ist **monatlich**, Sie können jedoch auch **drei Monate**, **6 Monate**, **jährlich**oder **24 Monate**auswählen.

> [!IMPORTANT]
> Nachdem das Add-on veröffentlicht wurde, können Sie die Auswahl Ihres **Abonnementzeitraums** nicht mehr ändern.


## <a name="free-trial"></a>Kostenlose Testversion

Wenn Sie ein **Abonnement** für Ihren Produkttyp ausgewählt haben, wird auch eine **Kostenlose Testversion** angezeigt. Die Standardoption ist **keine kostenlose Testversion.** Wenn Sie dies bevorzugen, können Sie es Kunden ermöglichen, das Add-on für einen festgelegten Zeitraum (entweder eine **Woche** oder **einen Monat**) kostenlos zu verwenden. 

> [!IMPORTANT]
> Nachdem das Add-on veröffentlicht wurde, können Sie die Auswahl der **kostenlosen Testversion** nicht mehr ändern.


## <a name="content-type"></a>Inhaltstyp

Unabhängig vom Produkttyp Ihres Add-ons müssen Sie den Typ des Inhalts angeben, den Sie anbieten. Für die meisten Add-Ons sollte der Inhaltstyp **Download elektronischer Software** lauten. Wenn eine andere Option in der Liste das Add-on besser beschreibt (z. b. Wenn Sie einen Musikdownload oder ein e-book anbieten), wählen Sie stattdessen diese Option aus.

Mögliche Optionen für den Inhaltstyp eines Add-Ons:

-   Download elektronischer Software
-   Elektronische Bücher
-   Einzelausgabe einer elektronischen Zeitschrift
-   Einzelausgabe einer elektronischen Zeitung
-   Musikdownload
-   Musikstreaming
-   Onlinedatenspeicher/-dienste
-   Software-as-a-Service
-   Videodownload
-   Videostreaming


## <a name="additional-properties"></a>Zusätzliche Eigenschaften

Diese Felder sind für alle Typen von Add-ons optional.

<span id="keywords" />

### <a name="keywords"></a>Keywords

Sie können für jedes eingereichte Add-On bis zu zehn Schlüsselwörter von jeweils bis zu 30 Zeichen bereitstellen. Danach kann Ihre App nach Add-Ons suchen, die diesen Schlüsselwörtern entsprechen. Durch dieses Feature können Sie Bildschirme in Ihrer App erstellen, die Add-Ons laden können, ohne dass Sie die Produkt-ID im App-Code direkt angeben müssen. Sie können die Schlüsselwörter des Add-Ons später jederzeit ändern, ohne Codeänderungen an der App vorzunehmen oder die App erneut einzureichen.

Um dieses Feld abzufragen, verwenden Sie die [storeproduct. Keywords](/uwp/api/windows.services.store.storeproduct.Keywords) -Eigenschaft im [Windows. Services. Store-Namespace](/uwp/api/Windows.Services.Store). (Oder verwenden Sie, wenn Sie den [Windows. applicationmodel. Store-Namespace](/uwp/api/Windows.ApplicationModel.Store)verwenden, die [productlisting. Keywords](/uwp/api/windows.applicationmodel.store.productlisting.Keywords) -Eigenschaft.)

> [!NOTE]
> Schlüsselwörter sind nicht für die Verwendung in Paketen verfügbar, die auf Windows 8 und Windows 8.1 abzielen.

<span id="custom-developer-data" />

### <a name="custom-developer-data"></a>Benutzerdefinierte Entwicklerdaten

Sie können bis zu 3000 Zeichen in das **benutzerdefinierte Datenfeld für Entwickler** (früher als **Tag**bezeichnet) eingeben, um zusätzlichen Kontext für Ihr in-App-Produkt bereitzustellen. In den meisten Fällen ist dies in Form einer XML-Zeichenfolge, Sie können aber auch beliebige Elemente in diesem Feld eingeben. Ihre APP kann dann dieses Feld Abfragen, um ihren Inhalt zu lesen (obwohl die APP die Daten nicht bearbeiten kann und die Änderungen nicht zurückgegeben werden kann).

Angenommen, Sie haben ein Spiel, und Sie verkaufen ein Add-on, das dem Kunden den Zugriff auf zusätzliche Ebenen ermöglicht. Mit dem **benutzerdefinierten Entwickler Datenfeld** kann die APP Abfragen, welche Ebenen verfügbar sind, wenn ein Kunde dieses Add-on besitzt. Sie können den Wert jederzeit anpassen (in diesem Fall die Ebenen, die eingeschlossen werden), ohne Codeänderungen in der APP vornehmen oder die APP erneut einreichen zu müssen, indem Sie die Informationen im **benutzerdefinierten Entwickler Datenfeld** des Add-ons aktualisieren und anschließend eine aktualisierte Übermittlung für das Add-on veröffentlichen.

Um dieses Feld abzufragen, verwenden Sie die [storesku. customdeveloperdata](/uwp/api/windows.services.store.storesku.customdeveloperdata#Windows_Services_Store_StoreSku_CustomDeveloperData) -Eigenschaft im [Windows. Services. Store-Namespace](/uwp/api/Windows.Services.Store). (Oder verwenden Sie, wenn Sie den [Windows. applicationmodel. Store-Namespace](/uwp/api/Windows.ApplicationModel.Store)verwenden, die [productlisting. Tag](/uwp/api/windows.applicationmodel.store.productlisting.tag#Windows_ApplicationModel_Store_ProductListing_Tag) -Eigenschaft.)

> [!NOTE]
> Das **benutzerdefinierte Entwickler Datenfeld** ist nicht für die Verwendung in Paketen verfügbar, die auf Windows 8 und Windows 8.1 abzielen.

 

 

 