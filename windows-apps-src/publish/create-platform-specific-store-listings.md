---
Description: Wenn die von Ihnen bereitgestellten Pakete auf unterschiedliche Betriebssysteme ausgerichtet sind, können Sie Teile Ihres Store-Eintrags für verschiedene Zielbetriebssysteme anpassen.
title: Erstellen plattformspezifischer Store-Einträge
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, anpassen, auflisten, Beschreibung, früher
ms.localizationpriority: medium
ms.openlocfilehash: 475526662737eaa5b95267e36016e342c6652a89
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945958"
---
# <a name="create-platform-specific-store-listings"></a>Erstellen plattformspezifischer Store-Einträge


Wenn Ihre zuvor veröffentlichte App Pakete enthält, die auf andere Betriebssysteme abzielen, haben Sie die Möglichkeit, Teile ihrer Store-Auflistung für Kunden unter früheren Betriebssystemversionen (Windows 8. x oder früher und/oder Windows Phone 8. x oder früher) anzupassen. 

Kunden unter Windows 10 (einschließlich Xbox) sehen immer die Standard [Speicher Auflistung](create-app-store-listings.md). Es wird nicht angezeigt, dass Sie plattformspezifische Store-Listen erstellen, es sei denn, Sie haben Ihre APP bereits mit Paketen veröffentlicht, die eine oder mehrere frühere Betriebssystemversionen unterstützen. 

> [!IMPORTANT]
> Sie können keine neuen XAP-Pakete mehr hochladen, die mit den Windows Phone 8. x SDK (s) erstellt wurden. Apps, die bereits mit XAP-Paketen im Speicher gespeichert sind, funktionieren weiterhin auf Windows 10 Mobile-Geräten. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Plattformspezifische Store-Auflistungen können nützlich sein, wenn Sie Features erwähnen möchten, die nur in einer Betriebssystemversion angezeigt werden, oder für ein bestimmtes Betriebssystem (unabhängig vom Gerätetyp) Screenshots bereitstellen möchten.

> [!NOTE]
> Wenn Sie eine plattformspezifische Speicher Auflistung in einer Sprache erstellen, wird keine plattformspezifische Speicher Auflistung in anderen Sprachen erstellt, die von ihrer App unterstützt werden. Sie müssen den plattformspezifischen Store-Eintrag getrennt für jede Sprache erstellen. Beachten Sie auch, dass Sie keine Speicher Listen Daten für plattformspezifische Auflistungen [importieren und exportieren](import-and-export-store-listings.md) können.


## <a name="creating-a-platform-specific-store-listing"></a>Erstellen eines plattformspezifischen Store-Eintrags

Wenn **Ihre zuvor** veröffentlichte App Pakete enthält, die frühere Betriebssystemversionen unterstützen ((Windows 8. x oder früher und/oder Windows Phone 8. x oder früher), können Sie die Option **plattformspezifische App Store-Auflistung erstellen**auswählen. Nachdem Sie diese Option ausgewählt haben, werden Sie aufgefordert, zwischen den von der Übermittlung unterstützten Betriebssystemversionen auszuwählen. Nachdem Sie für alle früheren Betriebssystemversionen Ihrer APP plattformspezifische Speicher Auflistungen erstellt haben, können Sie keine weitere Auswahl treffen.

Sie können Ihre standardmäßige (Windows 10) Store-Auflistung als Ausgangspunkt verwenden. Dadurch werden die entsprechenden Text und Bilder übernommen, die Sie für die Standard Speicher Liste eingegeben haben. Sie können dann alle gewünschten Änderungen vornehmen, bevor Sie speichern. Alternativ können Sie mit einem vollständig leeren Store-Eintrag starten.

Nachdem Sie auf " **weiter**" geklickt haben, enthält die Seite " **Store-Auflistung** " nun einen Abschnitt für die plattformspezifische Speicher Liste, die Sie soeben erstellt haben. Dieser Abschnitt enthält einen eigenen Satz von Feldern für **Beschreibung** (erforderlich), **Neuerungen in dieser Version**, **Screenshots**, Symbol für **App-Kachel**, APP- **Features**und **zusätzliche Systemanforderungen**. Achten Sie darauf, Informationen in jedes Feld einzugeben, für das Sie Informationen im benutzerdefinierten Store-Eintrag anzeigen möchten, auch wenn die Informationen mit dem standardmäßigen Store-Eintrag übereinstimmen. Wenn Sie eines dieser Felder leer lassen, werden keine Informationen für dieses Feld im benutzerdefinierten Store-Eintrag angezeigt.

> [!IMPORTANT]
> Die Felder im Abschnitt " [zusätzliche Informationen](create-app-store-listings.md#additional-information) " der Store-Auflistung können nicht für verschiedene Betriebssystemversionen angepasst werden.
> 
> Da einige Felder auf der standardmäßigen [Store-Auflistungs](create-app-store-listings.md) Seite nur für Kunden unter Windows 10 gelten, werden bei der Erstellung einer plattformspezifischen Store-Auflistung nicht dieselben Optionen angezeigt. Beispielsweise können Sie keine Anhänger zu einer plattformspezifischen Store-Auflistung hinzufügen, da Sie nur Kunden unter Windows 10, Version 1607 oder höher, angezeigt werden. 

Sie können nach Bedarf weiterhin plattformspezifische Auflistungen bearbeiten, um Änderungen für Kunden mit einer bestimmten Betriebssystemversion vorzunehmen.


## <a name="removing-a-platform-specific-store-listing"></a>Entfernen eines plattformspezifischen Store-Eintrags

Wenn Sie eine plattformspezifische Speicher Auflistung erstellen und später entscheiden, dass Sie die Standardliste der Filialen für Kunden unter diesem Betriebssystem anzeigen möchten, wählen Sie den Link **Löschen** neben dieser Auflistung aus.

Nachdem Sie bestätigt haben, dass Sie diese Kunden in Ihrer standardmäßigen Store-Liste anzeigen möchten, wählen Sie **OK**aus. Die plattformspezifische Store-Auflistung wird entfernt (für alle Sprachen, in denen Sie vorhanden war), und Kunden in dieser Betriebssystemversion sehen nun Ihre Standard Speicher Auflistung. Wenn Sie Ihre Meinung ändern, können Sie eine weitere plattformspezifische Store-Auflistung für dieses Betriebssystem erstellen, indem Sie die oben aufgeführten Schritte ausführen.
 

 




