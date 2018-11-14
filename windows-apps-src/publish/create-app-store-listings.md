---
author: jnHs
Description: The Store listings section of the app submission process is where you provide the text and images that customers will see when viewing your app's listing in the Microsoft Store.
title: Erstellen von Store-Einträgen für Apps
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows10, UWP, Eintrag, Beschreibung, Store-Seite, Versionshinweise, Titel
ms.localizationpriority: medium
ms.openlocfilehash: ec1867e747f3458e3a9cffabe9a45535d4c27489
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/09/2018
ms.locfileid: "6187854"
---
# <a name="create-app-store-listings"></a>Erstellen von Store-Einträgen für Apps


Im Abschnitt **Store-Einträge** des [App-Übermittlungsprozesses](app-submissions.md) stellen Sie den Text und die [Bilder](app-screenshots-and-images.md) bereit, die den Kunden im Microsoft Store angezeigt werden, wenn diese Ihre App ansehen.

Viele der Felder in einem **Store-Eintrag** sind optional. Es wird jedoch empfohlen, mehrere Bilder und so viele Infos wie möglich bereitzustellen, damit Ihr Eintrag auffällt. Im Schritt **Store-Einträge** sollten Sie mindestens eine Textbeschreibung und mindestens einen [Screenshot](app-screenshots-and-images.md#screenshots) angeben.

> [!TIP]
> Sie können optional [Store-Einträge importieren und Exportieren](import-and-export-store-listings.md) , wenn Sie lieber Ihre Eintragsinformationen offline in eine CSV-Datei, anstatt bereitstellenden Informationen und Hochladen von Dateien direkt im Partner Center eingeben. Das Verwenden der Option zum Importieren und Exportieren ist besonders dann hilfreich, wenn Sie Einträge in mehreren Sprachen besitzen, da Sie mehrere Updates auf einmal vornehmen können. 

Wenn Ihre app zuvor veröffentlichten Windows unterstützt 8.x und/oder Windows Phone 8.x oder früher, können Sie [plattformspezifische Store-Einträge erstellen](create-platform-specific-store-listings.md) , um für diese Kunden anzuzeigen. 

## <a name="store-listing-languages"></a>Sprachen für Store-Einträge

Sie müssen die Seite **Store-Eintrag** für mindestens eine Sprache ausfüllen. Es wird empfohlen, einen Store-Eintrag in jeder Sprache bereitzustellen, die Ihre Pakete unterstützen. Sie haben jedoch die Möglichkeit, Sprachen zu entfernen, für die Sie keinen Store-Eintrag bereitstellen möchten. Sie können auch Store-Einträge in zusätzlichen Sprachen bereitstellen, die von Ihren Paketen nicht unterstützt werden.

> [!NOTE]
> Wenn Ihre Übermittlung bereits Pakete enthält, werden die in Ihren Paketen unterstützten [Sprachen](supported-languages.md) in der Übermittlungsübersicht angezeigt (es sei denn, Sie diese entfernen welche).

Klicken Sie zum Hinzufügen oder Entfernen von Sprachen für Ihre Store-Einträge in der Übermittlungsübersicht auf **Add/remove languages**. Wenn Sie bereits Pakete hochgeladen haben, sind die Sprachen dafür im Abschnitt **Languages supported by your packages** aufgeführt. Klicken Sie zum Entfernen einer oder mehrerer dieser Sprachen auf **Entfernen**. Wenn Sie später eine Sprache angeben möchten, die Sie zuvor aus diesem Abschnittentfernt haben, können Sie auf **Hinzufügen** klicken.

Im Abschnitt **Additional Store listing languages** können Sie auf **Manage additional languages** klicken, um Sprachen hinzuzufügen oder zu entfernen, die  *nicht* in Ihren Paketen enthalten sind. Aktivieren Sie die Kontrollkästchen für die Sprachen, die Sie hinzufügen möchten, und klicken Sie dann auf **Aktualisieren**. Die ausgewählten Sprachen werden im Abschnitt **Additional Store listing languages** angezeigt. Klicken Sie zum Entfernen einer oder mehrerer dieser Sprachen auf **Entfernen** (oder klicken Sie auf **Manage additional languages**, und deaktivieren Sie die Kontrollkästchen für die Sprachen, die Sie entfernen möchten).

Wenn Sie Ihre Auswahl getroffen haben, klicken Sie auf **Speichern**, um zur Übermittlungsübersicht zurückzukehren. 

## <a name="add-and-edit-store-listing-info"></a>Hinzufügen und Bearbeiten von Store-Eintrag Informationen

Wählen Sie den Namen der Sprache zum Bearbeiten eines Store-Eintrags in der übermittlungsübersicht. Sie müssen jede Sprache separat bearbeiten, es sei denn, Sie festlegen, dass Ihre Store-Einträge exportieren offline arbeiten, und importieren alle Eintrag Daten auf einmal. Weitere Informationen zur Funktionsweise der, die, finden Sie unter [Importieren und Exportieren von Store-Einträge](import-and-export-store-listings.md).

Die verfügbaren Felder sind unten beschrieben.

## <a name="product-name"></a>Produktname

Diese Dropdown-Feld können Sie angeben, welche Namen im Store-Eintrag verwendet werden soll (Wenn Sie mehr als einen Namen für die app reserviert haben).

Wenn Sie hochgeladen haben Pakete in der gleichen Sprache wie die Store-Eintrag arbeiten an, die Namen in diese Pakete ausgewählt werden. Wenn Sie zum [Benennen Sie der app](manage-app-names.md#rename-an-app-that-has-already-been-published) benötigen, nachdem es bereits veröffentlicht wurde, können Sie hier einen anderen reservierten Namen auswählen, bei der Erstellung einer neuen Übermittlungs, nachdem die Pakete erfolgreich hochgeladen haben, die den neuen Namen verwenden.

Für die Sprache Pakete erfolgreich hochgeladen wurden Sie arbeiten, und Sie mehr als einen Namen reserviert haben, müssen Sie eines Ihrer reservierten app-Namen auswählen, da es kein verknüpftes Paket in dieser Sprache aus dem der Name abgerufen werden könnte.

> [!NOTE]
> Der **Produktname** nur gewählte gilt für den Store-Eintrag in der Sprache in dem Sie arbeiten. Es hat keinen Einfluss auf den Namen angezeigt, wenn ein Kunde die app installiert werden; Dieser Name stammt aus dem Manifest des Pakets, das installiert wird. Um Missverständnisse zu vermeiden, empfehlen wir, dass jede Sprache Pakete und Store-Eintrag den gleichen Namen verwenden.

## <a name="description"></a>Beschreibung

Über das Beschreibungsfeld können Sie Ihren Kunden den Zweck der App mitteilen. Dieses Feld ist erforderlich und kann bis zu 10.000 Zeichen reinen Text enthalten.

Tipps zum Erstellen einer aussagekräftigen Beschreibung finden Sie unter [Erstellen einer interessanten App-Beschreibung](write-a-great-app-description.md).

<span id="release-notes" />

## <a name="whats-new-in-this-version"></a>Neuigkeiten in dieser Version

Wenn Sie Ihre App erstmalig einrichten, lassen Sie dieses Feld leer. Für eine Aktualisierung einer vorhandenen App ist dies an, teilen Sie dem Kunden, was in der aktuellen Version geändert hat. Dieses Feld ist auf 1.500 Zeichen beschränkt. (Dieses Feld hieß zuvor **Anmerkungen zu dieser Version**).

## <a name="product-features"></a>Produktfunktionen

Hierbei handelt es sich um kurze Zusammenfassungen der wichtigsten App-Features. Diese werden dem Kunden neben der **Beschreibung** als Aufzählung im Abschnitt der **Funktionen** im Store-Eintrag Ihrer App angezeigt. Die Beschreibung sollte pro Feature nur wenige Wörter (und nicht mehr als 200Zeichen) enthalten. Sie können bis zu 20Features hinzufügen.

> [!NOTE]
> Diese Features erscheint in Store-Eintrag mit Aufzählungszeichen, daher keine eigenen Aufzählungszeichen hinzu.

## <a name="screenshots"></a>Screenshots

Ein Bildschirmfoto ist erforderlich, um Ihre App zu übermitteln. Wir empfehlen, mindestens 4 Screenshots für jeden Gerätetyp hinzuzufügen, die von Ihrer App unterstützt werden, damit Benutzer sehen, wie die App auf ihrem Gerätetyp aussieht.

Weitere Informationen finden Sie unter [App-Screenshots und -Bilder](app-screenshots-and-images.md#screenshots).


## <a name="store-logos"></a>Store-Logos 

Store-Logos sind optionale Bilder, die Sie hochladen können, um die Art und Weise zu verbessern, auf die Ihre App für Kunden angezeigt wird. Sie können optional auch angeben, dass nur hier hochgeladene Bilder im Store-Eintrag der App für Kunden auf Windows 10 (einschließlich Xbox) verwendet werden sollen, statt dem Store die Verwendung von Logobildern aus den Paketen Ihrer App zu erlauben.

> [!IMPORTANT]
> Wenn Ihre App Xbox unterstützt oder wenn Windows Phone8.1 oder frühere Versionen unterstützt werden, müssen Sie hier bestimmte Bilder bereitstellen, damit der Eintrag im Store korrekt angezeigt wird. 

Weitere Informationen finden Sie unter [Store-Logos](app-screenshots-and-images.md#store-logos).


## <a name="trailers-and-additional-assets"></a>Trailer und weitere Ressourcen

Sie können zusätzliche Ressourcen für das Produkt, einschließlich der video-Trailer und Werbebilder übermitteln. Diese sind optional, aber es empfiehlt sich, so viele wie möglich hochzuladen. Diese Bilder können Kunden eine bessere Vorstellung davon bieten, worum es sich bei Ihrem Produkt handelt, und den Eintrag ansprechender gestalten.

Weitere Informationen finden Sie unter [Trailer und weitere Ressourcen](app-screenshots-and-images.md#trailers-and-additional-assets).

<a id="supplemental-information" />

## <a name="supplemental-fields"></a>Ergänzende Felder

Die Felder in diesem Abschnitt sind alle optional. Überprüfen Sie die Informationen unten, um zu bestimmen, ob die Bereitstellung dieser Informationen für Ihre Übermittlung sinnvoll ist. Insbesondere die **Kurzbeschreibung** wird für die meisten Übermittlungen empfohlen. Die anderen Felder unterstützen eine optimale Erfahrung für Ihr Produkt in den verschiedenen Szenarien.

### <a name="short-title"></a>Kurzer Titel

Eine kürzere Version des Produktnamens. Wenn vorhanden, kann dieser kürzere Namen anstelle des Produkttitels an verschiedenen Stellen auf Xbox One angezeigt werden (während der Installation, in Ihren Erfolgen usw.).

Dieses Feld ist auf 50 Zeichen beschränkt.


### <a name="sort-title"></a>Sortierungstitel

Wenn Ihr Produkt alphabetisch sortiert oder auf unterschiedliche Weise geschrieben werden kann, können Sie hier eine andere Version eingeben. Dadurch können Kunden das Produkt schnell finden, wenn sie diese Version bei der Suche eingeben. 

Dieses Feld ist auf 255 Zeichen beschränkt.


### <a name="voice-title"></a>Sprachtitel

Ein alternativer Name für das Produkt, das ggf. als Audio-Erlebnis auf Xbox One verwendet werden kann, wenn Kinect oder ein Kopfhörer genutzt wird.

Dieses Feld ist auf 255 Zeichen beschränkt.


### <a name="short-description"></a>Kurze Beschreibung

Eine kürzere, ansprechende Beschreibung, die am oberen Rand des Store-Eintrags Ihres Produkts verwendet werden kann. Falls nicht anders angegeben, wird stattdessen der erste Absatz (bis zu 500 Zeichen) Ihrer längeren [Beschreibung](#description) verwendet. Da die Beschreibung auch unter diesem Text angezeigt wird, empfiehlt es sich, eine kurze Beschreibung mit einem anderen Text bereitzustellen, sodass Ihr Store-Eintrag keine Wiederholungen enthält.

Bei Spielen erscheint die Kurzbeschreibung auch im Abschnitt „Informationen“ im Spiele-Hub auf Xbox One.

Beachten Sie für optimale Ergebnisse zu erzielen Ihre kurze Beschreibung unter 270 Zeichen. Das Feld ist auf 500 Zeichen beschränkt, aber in einigen Ansichten, die nur die ersten 270 Zeichen angezeigt werden, (mit einem Link, der für den Rest des die Kurzbeschreibung anzuzeigen verfügbar ist).


### <a name="additional-system-requirements"></a>Weitere Systemanforderungen

Bei Bedarf können Sie die Hardwarekonfigurationen beschreiben, die für die ordnungsgemäße Funktionsweise der App erforderlich sind, über die Informationen hinaus, die Sie im Abschnitt **Systemanforderungen** unter [App-Eigenschaften](enter-app-properties.md#system-requirements) angegeben haben. Dies ist besonders wichtig, wenn Ihre App Hardware benötigt, die u.U. nicht auf jedem Computer vorhanden ist. Wenn Ihre App beispielsweise nur mit externer USB-Hardware wie z.B. einem 3D-Drucker oder einem Mikrocontroller nur funktionsfähig ist, empfehlen wir, diese hier einzugeben. Die Informationen, die Sie hier eingeben, werden Kunden im Store-Eintrag Ihrer App unter Windows10, Version 1607 oder höher (einschließlich Xbox), zusammen mit den auf der Eigenschaftenseite des Produkts angegebenen Anforderungen angezeigt. 

Sie können bis zu 11Elemente sowohl für **Mindesthardwareanforderungen** als auch für **Empfohlene Hardware** eingeben. Diese werden dem Kunden im Store-Eintrag als Aufzählung angezeigt. Die Beschreibung sollte pro Element nur wenige Wörter (und nicht mehr als 200Zeichen) enthalten.

> [!NOTE]
> Die zusätzlichen Systemanforderungen werden im Store-Eintrag mit Aufzählungszeichen versehen. Fügen Sie daher keine eigenen Aufzählungszeichen hinzu.


<span id="shared-fields" />

## <a name="additional-information"></a>Weitere Informationen

Mit den unten beschriebenen Elementen können Kunden Ihr Produkt finden und verstehen. (Dieser Abschnitt hieß früher **Freigegebene Felder**).

### <a name="search-terms"></a>Suchbegriffe

Suchbegriffe (früher als Schlüsselwörter bezeichnet) sind einzelne Wörter oder kurze Ausdrücke, die für Kunden nicht angezeigt werden, jedoch dazu beitragen, dass Ihre App im Store sichtbar ist, wenn Kunden nach dem Begriff suchen. Sie können bis zu 7 Suchbegriffe mit jeweils maximal 30 Zeichen aufnehmen und nicht mehr als 21 separate Wörter für alle Suchbegriffe verwenden.

Beim Hinzufügen von Suchbegriffen sollten Sie überlegen, mit welchen Begriffen Kunden nach Ihrer App suchen würden, insbesondere, wenn sie nicht im App-Namen vorkommen. Verwenden Sie nur Suchbegriffe, die wirklich für Ihre App relevant sind.

### <a name="copyright-and-trademark-info"></a>Urheberrecht- und Markeninformationen

Zusätzliche Urheberrecht- und Markeninformationen können Sie bei Bedarf hier eingeben. Dieses Feld ist auf 200 Zeichen beschränkt.


### <a name="additional-license-terms"></a>Zusätzliche Lizenzbedingungen

Lassen Sie dieses Feld leer, wenn Sie möchten, dass Ihre App für Kunden gemäß den **Standardbedingungen für anwendungsbezogene Lizenzen** (am Ende der [Vereinbarung für App-Entwickler](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement)) lizenziert wird.

Wenn sich Ihre Lizenzbedingungen von den **Standardbedingungen für anwendungsbezogene Lizenzen** unterscheiden, geben Sie sie hier ein.

Wenn Sie einen einzelnen URL in dieses Feld eingeben, wird dieser für Kunden als Link angezeigt, auf den sie klicken können, um Ihre zusätzlichen Lizenzbedingungen zu lesen. Dies ist hilfreich, wenn Ihre zusätzlichen Lizenzbedingungen sehr lang sind, oder wenn Sie Links oder Formatierungen in die zusätzlichen Lizenzbedingungen einschließen möchten, auf die geklickt werden kann.

Sie können auch bis zu 10.000 Zeichen Texts in dieses Feld eingeben. Wenn Sie dies tun, werden Kunden die zusätzlichen Lizenzbedingungen als reiner Text angezeigt.


### <a name="developed-by"></a>Entwickelt von

Geben Sie hier Text ein, wenn Sie das Feld **Entwickelt von** im Store-Eintrag Ihrer App anzeigen möchten. (Das Feld **Veröffentlicht von** zeigt den mit Ihrem Konto verknüpften Anzeigename des Herausgebers an, wenn Sie einen Wert für das Feld **Veröffentlicht von** angeben.)

Dieses Feld ist auf 255 Zeichen beschränkt.
 

<span id="privacy-policy" />

> [!NOTE]
> Die Felder **Datenschutzrichtlinie**, **Website**, und **Supportkontaktinfos** befinden sich jetzt auf der Seite [Eigenschaften](enter-app-properties.md).

