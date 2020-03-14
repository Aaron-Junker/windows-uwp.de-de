---
Description: Auf der Seite App-Eigenschaften des App-Übermittlungsprozesses können Sie die Kategorie Ihrer App festlegen sowie Hardwareeinstellungen und weitere Deklarationen angeben.
title: Eingeben von App-Eigenschaften
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Spieleinstellungen, Anzeigemodus, Systemanforderungen, Hardwareanforderungen, Mindestanforderungen an die Hardware, empfohlene Hardware, Datenschutzrichtlinie, Kontaktinformationen für den Support, App-Website, Supportinformationen
ms.localizationpriority: medium
ms.openlocfilehash: 5bc1283560334d2eac97fd358feabfca0f411014
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210756"
---
# <a name="enter-app-properties"></a>Eingeben von App-Eigenschaften

Auf der Seite **Eigenschaften** des [App-Übermittlungsprozesses](app-submissions.md) können Sie die Kategorie Ihrer App festlegen sowie andere Informationen und weitere Deklarationen angeben. Achten Sie darauf, dass Sie auf dieser Seite vollständige und genaue Details zu Ihrer App angeben.


## <a name="category-and-subcategory"></a>Kategorie und Unterkategorie

Geben Sie hier die Kategorie (und ggf. eine Unterkategorie/Genre) an, die im Store zur Kategorisierung der App verwendet werden soll. Die Angabe einer Kategorie ist für die Einreichung Ihrer App erforderlich.

Weitere Informationen finden Sie unter [Kategorie- und Unterkategorietabelle](category-and-subcategory-table.md).


## <a name="support-info"></a>Supportinformationen

In diesem Abschnitt erhalten Sie Informationen, wie Sie Ihren Kunden weitere Informationen zu Ihrer App übermitteln und wie Sie Support erhalten.

### <a name="privacy-policy-url"></a>URL zu den Datenschutzrichtlinien

Sie müssen sicherstellen, dass die geltenden Datenschutzgesetze eingehalten werden und müssen bei Bedarf hier eine gültige Datenschutzrichtlinien-URL bereitstellen.

In diesem Abschnitt müssen Sie angeben, ob die App auf [persönliche Informationen](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information) zugreift, diese erfasst oder überträgt . Wenn Sie diese Frage mit **Ja** beantworten, ist eine URL zur Datenschutzrichtlinie erforderlich. Andernfalls ist diese optional, (wenn wir feststellen, dass Ihre App eine Datenschutzrichtlinie benötigt und Sie diese noch nicht bereitgestellt haben, kann die Zertifizierung Ihre Übermittlung fehlschlagen).

> [!NOTE]
> Wenn wir feststellen, dass Ihre Pakete [Funktionen](../packaging/app-capability-declarations.md) deklarieren, die den Zugriff, die Übertragung oder die Sammlung persönlicher Informationen ermöglicht, markieren wir diese Frage mit **Ja**, und Sie müssen eine URL zur Datenschutzrichtlinie eingeben.

Um festzustellen, ob Ihre App eine Datenschutzrichtlinie benötigt, lesen Sie die [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) und die [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information). 

> [!NOTE]
> Microsoft stellt keine standardmäßigen Datenschutzrichtlinien für Ihre App bereit. Außerdem ist Ihre App nicht durch Datenschutzrichtlinien von Microsoft abgedeckt. 


### <a name="website"></a>Website

Geben Sie die URL der Webseite für Ihre App ein. Diese URL muss auf eine Seite Ihrer eigenen Website verweisen, nicht auf den Webeintrag Ihrer App im Store. Dieses Feld ist optional. Ihre Verwendung wird jedoch empfohlen.

### <a name="support-contact-info"></a>Support – Kontaktinfos

Geben Sie die URL der Webseite ein, auf der Ihre Kunden Support für die App erhalten, oder eine E-Mail-Adresse, unter der Kunden den Support erreichen können. Wir empfehlen diese Informationen für alle Übermittlungen anzugeben, damit Ihre Kunden wissen, wie Sie Unterstützung erhalten, wenn sie diese benötigen. Microsoft bietet keinen App-Support für Ihre Kunden.

> [!IMPORTANT]
> Das Feld **Supportkontaktinformationen** ist erforderlich, wenn die App bzw. das Spiel auf Xbox verfügbar ist. Andernfalls ist dies optional (wird jedoch empfohlen).


## <a name="game-settings"></a>Einstellungen für Spiele

Dieser Abschnitt wird nur angezeigt, wenn Sie als Kategorie des Produkts **Spiele** ausgewählt haben. Hier können Sie angeben, welche Features von Ihrem Spiel unterstützt werden. Die Informationen, die Sie in diesem Abschnitt angeben, werden in der Store-Liste des Produkts angezeigt.

Wenn Ihr Spiel eine Multiplayer-Option unterstützt, müssen Sie die minimale und maximale Anzahl der Spieler für eine Sitzung angeben. Sie können nicht mehr als 1.000 minimale oder maximale Spieler eingeben.

Die **Plattformübergreifende Multiplayer-Option** bedeutet, dass das Spiel Multiplayer-Sitzungen zwischen Spielern auf Windows 10-PCs und Xbox unterstützt.


## <a name="display-mode"></a>Anzeigemodus

In diesem Abschnitt können Sie angeben, ob Ihr Produkt in einer immersiven Ansicht (nicht nur 2D) für [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality) auf PC bzw. HoloLens-Geräten geeignet ist. Wenn Sie dies bestätigen, müssen Sie ebenfalls folgendes angeben:
- Wählen Sie entweder **Mindesthardwareanforderungen** oder **Empfohlene Hardware** für **immersives Windows Mixed Reality-Headset** unter [Systemanforderungen](#system-requirements) auf dem unten angezeigten Abschnitt der Seite **Eigenschaften** aus.
- Geben Sie **Boundary setup** an (wenn ein PC ausgewählt wurde), damit Benutzer wissen, ob es nur einer sitzenden oder stehenden Position verwendet werden soll, oder ob der Benutzer sich bei der Verwendung bewegen kann. 

Wenn Sie **Spiele** als Kategorie des Produkts ausgewählt haben, werden zusätzliche Optionen in der Auswahl des **Anzeigemodus** angezeigt, mit denen Sie angeben können, ob Ihr Produkt eine 4K-Auflösung der Videoausgabe, High Dynamic Range (HDR) oder die variable Aktualisierungsratenanzeige unterstützt.

Wenn Ihr Produkt keine dieser Anzeigemodioptionen unterstützt, lassen Sie alle Kontrollkästchen leer.


## <a name="product-declarations"></a>Produktdeklarationen

Über die Kontrollkästchen in diesem Abschnitt geben Sie an, ob Deklarationen auf Ihre App zutreffen. Dies kann sich darauf auswirken, wie Ihre App angezeigt wird, ob sie bestimmten Kunden angeboten wird und wie sie von Kunden genutzt werden kann.

Weitere Informationen finden Sie unter [Produktdeklarationen](app-declarations.md).

## <a name="system-requirements"></a>Systemanforderungen

In diesem Abschnitt können Sie angeben, ob bestimmte Hardwarefeatures erforderlich sind oder empfohlen werden, um Ihre App ordnungsgemäß auszuführen und zu verwenden. Sie können das Kontrollkästchen für jedes Hardwareelement aktivieren (oder die entsprechende Option angeben), für das Sie **Mindesthardwareanforderungen** und/oder **Empfohlene Hardware** festlegen möchten.

Wenn Sie eine Auswahl für **Empfohlene Hardware** festlegen, werden diese Elemente Kunden unter Windows 10, Version 1607 oder höher, in Ihrem Store-Eintrag für das Produkt als empfohlene Hardware angezeigt. Kunden mit älteren Betriebssystemversionen werden diese Informationen nicht angezeigt.

Wenn Sie eine Auswahl für **Mindesthardwareanforderungen** treffen, werden diese Elemente Kunden unter Windows 10, Version 1607 oder höher, in Ihrem Store-Eintrag für das Produkt als erforderliche Hardware angezeigt. Kunden mit älteren Betriebssystemversionen werden diese Informationen nicht angezeigt. Im Store wird mit dem Eintrag Ihrer App möglicherweise auch eine Warnung für Kunden angezeigt, deren Gerät die Hardwareanforderungen nicht erfüllt. Dies hindert Kunden nicht daran, Ihre App auf Geräte ohne geeignete Hardware herunterzuladen. Allerdings können sie Ihre App auf diesen Geräten nicht bewerten oder eine Rezension für diese verfassen. 

Das Verhalten für Kunden variiert abhängig von den spezifischen Anforderungen und der Windows-Version des Kunden:

- **Für Kunden unter Windows 10, Version 1607 oder höher:**
     - Alle Mindest- und empfohlenen Anforderungen werden im Store-Eintrag angezeigt.
     - Der Store überprüft alle Mindestanforderungen und zeigt eine Warnung für Kunden an, deren Gerät die Anforderungen nicht erfüllt.
- **Für Kunden mit früheren Versionen von Windows 10:**
     - Für die meisten Kunden werden alle Mindest- und empfohlenen Hardwareanforderungen im Store-Eintrag angezeigt (Kunden mit älteren Versionen des Store-Clients werden jedoch nur die Mindesthardwareanforderungen angezeigt).
     - Der Store versucht, Elemente zu überprüfen, die Sie als **Mindesthardwareanforderungen** kennzeichnen, mit Ausnahme von **Speicher**, **DirectX**, **Videospeicher**, **Grafiken** und **Prozessor**. Diese Optionen werden nicht überprüft, und Kunden mit Geräten, die diese Anforderungen nicht erfüllen, wird keine Warnung angezeigt. 
- **Für Kunden unter Windows 8. x und früher oder Windows Phone 8. x und früher:**
     - Wenn Sie das Feld **Mindesthardwareanforderungen** für **Touchscreen** aktivieren, wird diese Anforderung im Store-Eintrag Ihrer App angezeigt und Kunden auf Geräten ohne Touchscreen wird eine Warnung angezeigt, wenn sie versuchen, die App herunterladen. Es werden keine weiteren Anforderungen überprüft oder in Ihrem Store-Eintrag angezeigt.

Zusätzlich wird empfohlen, der App Laufzeitprüfungen für die angegebene Hardware hinzuzufügen, da vom Store nicht immer erkannt werden kann, ob ein Kundengerät über das ausgewählte Feature verfügt, sodass Kunden die App trotz der Warnung herunterladen können. Wenn Sie verhindern möchten, dass Ihre UWP-App auf ein Gerät heruntergeladen wird, das die Mindestanforderungen für die Arbeitsspeicherkapazität oder DirectX-Ebene nicht erfüllt, können Sie die Mindestanforderungen in der [Datei StoreManifest XML](https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root).

> [!TIP]
> Wenn für Ihr Produkt zusätzliche Elemente erforderlich sind, die nicht in diesem Abschnitt aufgeführt sind, damit es ordnungsgemäß ausgeführt werden kann wie z. B. 3D-Drucker oder USB-Geräte, können Sie ebenfalls [Weitere Systemanforderungen](create-app-store-listings.md#additional-system-requirements) bei der Erstellung Ihrer Store-Eintrags angeben.





