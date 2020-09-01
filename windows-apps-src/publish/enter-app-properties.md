---
Description: Auf der Seite App-Eigenschaften des App-Übermittlungsprozesses können Sie die Kategorie Ihrer App festlegen sowie Hardwareeinstellungen und weitere Deklarationen angeben.
title: Eingeben von App-Eigenschaften
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Spieleinstellungen, Anzeigemodus, Systemanforderungen, Hardwareanforderungen, minimale Hardware, Empfohlene Hardware, Datenschutzrichtlinie, Support Kontaktinformationen, App-Website, Support Informationen
ms.localizationpriority: medium
ms.openlocfilehash: f945b9908a86d660bde9713ca353f1f3a438bf90
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167404"
---
# <a name="enter-app-properties"></a>Eingeben von App-Eigenschaften

Auf der Seite **Eigenschaften** des [App](app-submissions.md) -Übermittlungs Prozesses definieren Sie die Kategorie Ihrer APP und geben andere Informationen und Deklarationen ein. Stellen Sie sicher, dass Sie auf dieser Seite umfassende und genaue Details zu Ihrer APP bereitstellen.


## <a name="category-and-subcategory"></a>Kategorie und Unterkategorie

Sie müssen die Kategorie (und ggf. SubCategory/Genre) angeben, die der Speicher zum Kategorisieren Ihrer APP verwenden soll. Die Angabe einer Kategorie ist für die Einreichung Ihrer App erforderlich.

Weitere Informationen finden Sie unter [Kategorie- und Unterkategorietabelle](category-and-subcategory-table.md).


## <a name="support-info"></a>Support Informationen

In diesem Abschnitt können Sie Informationen bereitstellen, die Kunden dabei helfen, mehr über Ihre APP zu erfahren und Support zu erhalten.

### <a name="privacy-policy-url"></a>URL zur Datenschutzrichtlinie

Sie sind dafür verantwortlich, sicherzustellen, dass Ihre APP den Datenschutzbestimmungen und Bestimmungen entspricht, und dass Sie bei Bedarf eine gültige URL für die Datenschutzrichtlinie angeben.

In diesem Abschnitt müssen Sie angeben, ob Ihre APP auf [persönliche Informationen](/legal/windows/agreements/store-policies#105-personal-information)zugreift, Sie sammelt oder überträgt. Wenn Sie auf " **Ja**" Antworten, ist eine Datenschutzrichtlinien-URL erforderlich. Andernfalls ist Sie optional (wenn wir jedoch feststellen, dass Ihre APP eine Datenschutzrichtlinie erfordert, und Sie keine bereitgestellt haben, kann die Übermittlung der Übermittlung fehlschlagen).

> [!NOTE]
> Wenn Sie feststellen, dass Ihre Pakete [Funktionen](../packaging/app-capability-declarations.md) deklarieren, die es ermöglichen, auf personenbezogene Informationen zuzugreifen, zu übertragen oder zu erfassen, wird diese Frage als **Ja**gekennzeichnet, und Sie müssen eine Datenschutzrichtlinien-URL eingeben.

Überprüfen Sie die [App-Entwickler Vereinbarung](/legal/windows/agreements/app-developer-agreement) und die [Microsoft Store Richtlinien](/legal/windows/agreements/store-policies#105-personal-information), damit Sie bestimmen können, ob Ihre APP eine Datenschutzrichtlinie erfordert. 

> [!NOTE]
> Microsoft stellt keine Standard-Datenschutzrichtlinie für Ihre APP bereit. Außerdem ist Ihre App nicht durch Datenschutzrichtlinien von Microsoft abgedeckt. 


### <a name="website"></a>Website

Geben Sie die URL der Webseite für Ihre App ein. Diese URL muss auf eine Seite Ihrer eigenen Website verweisen, nicht auf den Webeintrag Ihrer App im Store. Dieses Feld ist optional, wird jedoch empfohlen.

### <a name="support-contact-info"></a>Support – Kontaktinfos

Geben Sie die URL der Webseite ein, auf der Ihre Kunden Ihre APP unterstützen können, oder eine e-Mail-Adresse, die Kunden für den Support kontaktieren können. Es wird empfohlen, diese Informationen für alle Übermittlungen aufzunehmen, damit Ihre Kunden wissen, wie Sie Support erhalten, wenn Sie Sie benötigen. Beachten Sie, dass Microsoft ihren Kunden nicht die Unterstützung für Ihre APP bereitstellt.

> [!IMPORTANT]
> Wenn Ihre APP oder Ihr Spiel auf der Xbox verfügbar ist, ist das Feld **Support Contact Info** erforderlich. Andernfalls ist Sie optional (wird jedoch empfohlen).


## <a name="game-settings"></a>Spiele Einstellungen

Dieser Abschnitt wird nur angezeigt, wenn Sie **Games** als Kategorie Ihres Produkts ausgewählt haben. Hier können Sie angeben, welche Funktionen Ihr Spiel unterstützt. Die Informationen, die Sie in diesem Abschnitt angeben, werden in der Store-Liste des Produkts angezeigt.

Wenn Ihr Spiel eine der Multiplayeroptionen unterstützt, stellen Sie sicher, dass Sie die minimale und maximale Anzahl von Playern für eine Sitzung angeben. Sie können nicht mehr als 1.000 minimale oder maximale Anzahl von Playern eingeben.

**Plattformübergreifender Multiplayer** bedeutet, dass das Spiel multiplayersitzungen zwischen Playern auf Windows 10-PCs und Xbox unterstützt.


## <a name="display-mode"></a>Anzeigemodus

In diesem Abschnitt können Sie angeben, ob Ihr Produkt für die Entwicklung in einer immersiven (nicht 2D) Ansicht für [Windows Mixed Reality](https://developer.microsoft.com/mixed-reality) auf PCs-und/oder hololens-Geräten entworfen wurde. Wenn Sie angeben, dass dies der Fall ist, müssen Sie auch Folgendes ausführen:
- Wählen Sie im Abschnitt " [System Anforderungen](#system-requirements) " im Abschnitt "System Anforderungen", der unten auf der Seite " **Eigenschaften** " angezeigt wird, **mindestens Hardware** oder **Empfohlene Hardware** für **Windows Mixed Reality** -
- Geben Sie das Setup für die **Grenze** an (wenn der PC ausgewählt ist), damit die Benutzer wissen, ob Sie nur an einer festgelegten Position oder an einer Position verwendet werden sollen, oder ob Sie den Benutzer während der Verwendung unterstützt (oder erfordert). 

Wenn Sie **Games** als Kategorie Ihres Produkts ausgewählt haben, sehen Sie zusätzliche Optionen in der **Anzeigemodus** -Auswahl, mit denen Sie angeben können, ob Ihr Produkt eine Videoausgabe mit 4K-Auflösung, eine Videoausgabe mit hohem dynamischen Bereich (HDR) oder eine Variable Aktualisierungsrate unterstützt.

Wenn das Produkt diese Optionen für den Anzeigemodus nicht unterstützt, lassen Sie alle Kontrollkästchen deaktiviert.


## <a name="product-declarations"></a>Produktdeklarationen

Über die Kontrollkästchen in diesem Abschnitt geben Sie an, ob Deklarationen auf Ihre App zutreffen. Dies kann sich darauf auswirken, wie Ihre App angezeigt wird, ob sie bestimmten Kunden angeboten wird und wie sie von Kunden genutzt werden kann.

Weitere Informationen finden Sie unter [Produkt Deklarationen](./product-declarations.md).

## <a name="system-requirements"></a>Systemanforderungen

In diesem Abschnitt können Sie angeben, ob bestimmte Hardwarefeatures erforderlich sind oder empfohlen werden, um Ihre App ordnungsgemäß auszuführen und zu verwenden. Sie können das Kontrollkästchen für jedes Hardwareelement aktivieren (oder die entsprechende Option angeben), für das Sie **Mindesthardwareanforderungen** und/oder **Empfohlene Hardware** festlegen möchten.

Wenn Sie eine Auswahl für **Empfohlene Hardware** festlegen, werden diese Elemente Kunden unter Windows 10, Version 1607 oder höher, in Ihrem Store-Eintrag für das Produkt als empfohlene Hardware angezeigt. Kunden mit älteren Betriebssystemversionen werden diese Informationen nicht angezeigt.

Wenn Sie eine Auswahl für **Mindesthardwareanforderungen** treffen, werden diese Elemente Kunden unter Windows 10, Version 1607 oder höher, in Ihrem Store-Eintrag für das Produkt als erforderliche Hardware angezeigt. Kunden mit älteren Betriebssystemversionen werden diese Informationen nicht angezeigt. Im Store wird mit dem Eintrag Ihrer App möglicherweise auch eine Warnung für Kunden angezeigt, deren Gerät die Hardwareanforderungen nicht erfüllt. Dies hindert Kunden nicht daran, Ihre App auf Geräte ohne geeignete Hardware herunterzuladen. Allerdings können sie Ihre App auf diesen Geräten nicht bewerten oder eine Rezension für diese verfassen. 

Das Verhalten für Kunden variiert abhängig von den spezifischen Anforderungen und der Windows-Version des Kunden:

- **Für Kunden mit Windows 10, Version 1607 oder höher:**
     - Alle Mindest- und empfohlenen Anforderungen werden im Store-Eintrag angezeigt.
     - Der Store überprüft alle Mindestanforderungen und zeigt eine Warnung für Kunden an, deren Gerät die Anforderungen nicht erfüllt.
- **Für Kunden mit früheren Versionen von Windows 10:**
     - Für die meisten Kunden werden alle Mindest- und empfohlenen Hardwareanforderungen im Store-Eintrag angezeigt (Kunden mit älteren Versionen des Store-Clients werden jedoch nur die Mindesthardwareanforderungen angezeigt).
     - Der Store versucht, Elemente zu überprüfen, die Sie als **Mindesthardwareanforderungen** kennzeichnen, mit Ausnahme von **Speicher**, **DirectX**, **Videospeicher**, **Grafiken** und **Prozessor**. Diese Optionen werden nicht überprüft, und Kunden mit Geräten, die diese Anforderungen nicht erfüllen, wird keine Warnung angezeigt. 
- **Für Kunden mit Windows 8.x bzw. Windows Phone 8.x und früheren Versionen:**
     - Wenn Sie das Feld **Mindesthardwareanforderungen** für **Touchscreen** aktivieren, wird diese Anforderung im Store-Eintrag Ihrer App angezeigt und Kunden auf Geräten ohne Touchscreen wird eine Warnung angezeigt, wenn sie versuchen, die App herunterladen. Es werden keine weiteren Anforderungen überprüft oder in Ihrem Store-Eintrag angezeigt.

Zusätzlich wird empfohlen, der App Laufzeitprüfungen für die angegebene Hardware hinzuzufügen, da vom Store nicht immer erkannt werden kann, ob ein Kundengerät über das ausgewählte Feature verfügt, sodass Kunden die App trotz der Warnung herunterladen können. Wenn Sie vollständig verhindern möchten, dass Ihre UWP-App auf einem Gerät heruntergeladen wird, das die Mindestanforderungen für den Arbeitsspeicher oder die DirectX-Ebene nicht erfüllt, können Sie die Mindestanforderungen in einer [storemanifest-XML-Datei](/uwp/schemas/storemanifest/storemanifestschema2015/schema-root)festlegen.

> [!TIP]
> Wenn für Ihr Produkt zusätzliche Elemente erforderlich sind, die in diesem Abschnitt nicht zur ordnungsgemäßen Ausführung aufgeführt sind (z. b. 3D-Drucker oder USB-Geräte), können Sie beim Erstellen der Store-Liste auch [zusätzliche Systemanforderungen](create-app-store-listings.md#additional-system-requirements) eingeben.