---
description: Im Abschnitt Store-Auflistungen des App-Übermittlungs Prozesses geben Sie den Text und die Bilder an, die Kunden angezeigt werden, wenn Sie die Liste Ihrer Apps im Microsoft Store anzeigen.
title: Erstellen von Store-Einträgen für Apps
ms.assetid: 50D67219-B6C6-4EF0-B76A-926A5F24997D
ms.date: 03/13/2019
ms.topic: article
keywords: Windows 10, UWP, Auflistung, Beschreibung, Store-Seite, Anmerkungen zu dieser Version, Titel
ms.localizationpriority: medium
ms.openlocfilehash: cb895dcabcc72361575f2d160b10fade03905d9a
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033293"
---
# <a name="create-app-store-listings"></a>Erstellen von Store-Einträgen für Apps

Im Abschnitt **Store-Auflistungen** des App-Übermittlungs [Prozesses](app-submissions.md) geben Sie den Text und die [Bilder](app-screenshots-and-images.md) an, die Kunden angezeigt werden, wenn Sie die Liste Ihrer Apps im Microsoft Store anzeigen.

Viele der Felder in einer **Speicher Auflistung** sind optional, aber es wird empfohlen, mehrere Abbilder und so viele Informationen wie möglich bereitzustellen, damit Ihre Auflistung heraus steht. Der als vollständig zu berücksichtigende **Schritt muss** mindestens eine Textbeschreibung und mindestens einen [Screenshot](app-screenshots-and-images.md#screenshots)aufweisen.

> [!TIP]
> Sie können [Speicher Listen optional importieren und exportieren](import-and-export-store-listings.md) , wenn Sie Ihre Auflistungs Informationen in einer CSV-Datei Offline eingeben möchten, anstatt Informationen bereitzustellen und Dateien direkt in Partner Center hochzuladen. Die Verwendung der Import-und Exportoption kann besonders praktisch sein, wenn Sie überlisten in vielen Sprachen verfügen, da Sie mehrere Updates gleichzeitig durchführen können.

Wenn Ihre zuvor veröffentlichte APP Windows 8. x und/oder Windows Phone 8. x oder früher unterstützt, können Sie [plattformspezifische Store-Listen erstellen](create-platform-specific-store-listings.md) , die diesen Kunden angezeigt werden.

## <a name="store-listing-languages"></a>Sprachen für Store-Einträge

Sie müssen die Seite **Store-Eintrag** für mindestens eine Sprache ausfüllen. Es wird empfohlen, einen Store-Eintrag in jeder Sprache bereitzustellen, die Ihre Pakete unterstützen. Sie haben jedoch die Möglichkeit, Sprachen zu entfernen, für die Sie keinen Store-Eintrag bereitstellen möchten. Sie können auch Store-Einträge in zusätzlichen Sprachen bereitstellen, die von Ihren Paketen nicht unterstützt werden.

> [!NOTE]
> Wenn Ihre Übermittlung bereits Pakete enthält, werden die in ihren Paketen unterstützten [Sprachen](supported-languages.md) auf der Übersichtsseite für die Übermittlung angezeigt (es sei denn, Sie entfernen Sie).

Klicken Sie zum Hinzufügen oder Entfernen von Sprachen für Ihre Store-Listen auf der Seite Übersicht über die Übermittlung auf **Sprachen hinzufügen/entfernen** . Wenn Sie bereits Pakete hochgeladen haben, sind die Sprachen dafür im Abschnitt **Languages supported by your packages** aufgeführt. Klicken Sie zum Entfernen einer oder mehrerer dieser Sprachen auf **Entfernen** . Wenn Sie später eine Sprache angeben möchten, die Sie zuvor aus diesem Abschnitt entfernt haben, können Sie auf **Hinzufügen** klicken.

Im Abschnitt **Additional Store listing languages** können Sie auf **Manage additional languages** klicken, um Sprachen hinzuzufügen oder zu entfernen, die  *nicht* in Ihren Paketen enthalten sind. Aktivieren Sie die Kontrollkästchen für die Sprachen, die Sie hinzufügen möchten, und klicken Sie dann auf **Aktualisieren** . Die ausgewählten Sprachen werden im Abschnitt **Additional Store listing languages** angezeigt. Klicken Sie zum Entfernen einer oder mehrerer dieser Sprachen auf **Entfernen** (oder klicken Sie auf **Manage additional languages** , und deaktivieren Sie die Kontrollkästchen für die Sprachen, die Sie entfernen möchten).

Wenn Sie Ihre Auswahl getroffen haben, klicken Sie auf **Speichern** , um zur Übermittlungsübersicht zurückzukehren.

## <a name="add-and-edit-store-listing-info"></a>Hinzufügen und Bearbeiten von Speicher Listen Informationen

Um eine Store-Auflistung zu bearbeiten, wählen Sie den Namen der Sprache auf der Seite Übersicht über die Übermittlung aus. Sie müssen jede Sprache separat bearbeiten, es sei denn, Sie exportieren ihre Store-Listen und arbeiten offline und importieren dann alle Auflistungs Daten gleichzeitig. Weitere Informationen zur Funktionsweise finden Sie unter [importieren und Exportieren von Speicher Listen](import-and-export-store-listings.md).

Die verfügbaren Felder werden unten beschrieben.

## <a name="product-name"></a>Produktname

In diesem Dropdown Feld können Sie angeben, welcher Name in der Store-Auflistung verwendet werden soll (wenn Sie mehr als einen Namen für die APP reserviert haben).

Wenn Sie Pakete in derselben Sprache hochgeladen haben wie die Speicher Auflistung, an der Sie arbeiten, wird der in diesen Paketen verwendete Name ausgewählt. Wenn Sie [die APP umbenennen](manage-app-names.md#rename-an-app-that-has-already-been-published) müssen, nachdem Sie bereits veröffentlicht wurde, können Sie hier einen anderen reservierten Namen auswählen, wenn Sie eine neue Übermittlung erstellen, nachdem Sie die Pakete hochgeladen haben, die den neuen Namen verwenden.

Wenn Sie keine Pakete für die Sprache hochgeladen haben, an der Sie arbeiten, und Sie mehr als einen Namen reserviert haben, müssen Sie einen ihrer reservierten APP-Namen auswählen, da in dieser Sprache kein zugeordnetes Paket vorhanden ist, aus dem Sie den Namen abrufen können.

> [!NOTE]
> Der von Ihnen gewählte **Produktname** gilt nur für die Store-Auflistung in der Sprache, in der Sie arbeiten. Er wirkt sich nicht auf den Namen aus, der angezeigt wird, wenn ein Kunde die APP installiert. Dieser Name stammt aus dem Manifest des Pakets, das installiert wird. Um Verwechslungen zu vermeiden, wird empfohlen, dass die Paket-(e) und Speicher Listen der einzelnen Sprachen denselben Namen verwenden.

## <a name="description"></a>BESCHREIBUNG

Über das Beschreibungsfeld können Sie Ihren Kunden den Zweck der App mitteilen. Dieses Feld ist erforderlich und kann bis zu 10.000 Zeichen reinen Text enthalten.

Tipps zum Erstellen einer aussagekräftigen Beschreibung finden Sie unter [Erstellen einer interessanten App-Beschreibung](write-a-great-app-description.md).

<a name="release-notes"></a>

## <a name="whats-new-in-this-version"></a>Neuerungen in dieser Version

Wenn Sie Ihre APP zum ersten Mal einreichen, lassen Sie dieses Feld leer. Wenn Sie ein Update für eine vorhandene app ausführen möchten, können Sie den Kunden mitteilen, was sich in der neuesten Version geändert hat. Dieses Feld ist auf 1.500 Zeichen beschränkt. (Zuvor hieß dieses Feld Versions **Hinweise** ).

## <a name="product-features"></a>Produkt Features

Hierbei handelt es sich um kurze Zusammenfassungen der wichtigsten App-Features. Sie werden dem Kunden neben der **Beschreibung** im Abschnitt **Features** der Store-Liste Ihrer APP als Auflistungs Liste angezeigt. Die Beschreibung sollte pro Feature nur wenige Wörter (und nicht mehr als 200 Zeichen) enthalten. Sie können bis zu 20 Features hinzufügen.

> [!NOTE]
> Diese Features werden in ihrer Store-Auflistung angezeigt. Fügen Sie also keine eigenen Aufzählungen hinzu.

## <a name="screenshots"></a>Screenshots

Ein Screenshot ist erforderlich, um Ihre APP zu übermitteln. Es wird empfohlen, für jeden Gerätetyp, der von ihrer App unterstützt wird, mindestens vier Screenshots bereitzustellen, damit Benutzer sehen können, wie die APP Ihren Gerätetyp untersucht.

Weitere Informationen finden Sie unter [App-Screenshots und -Bilder](app-screenshots-and-images.md#screenshots).

## <a name="store-logos"></a>Store-Logos

Store-Logos sind optionale Images, die Sie hochladen können, um die Art und Weise zu verbessern, wie Ihre APP Kunden angezeigt wird. Sie können optional auch angeben, dass nur Bilder, die Sie hier hochladen, in der Store-Liste Ihrer APP für Kunden auf Windows 10 (einschließlich Xbox) verwendet werden sollen, anstatt dem Store zu gestatten, Logo Bilder aus den Paketen Ihrer APP zu verwenden.

> [!IMPORTANT]
> Wenn Ihre APP Xbox unterstützt oder Windows Phone 8,1 oder früher unterstützt, müssen Sie hier bestimmte Bilder bereitstellen, damit die Auflistung ordnungsgemäß im Store angezeigt wird.

Weitere Informationen finden Sie unter [Store-Logos](app-screenshots-and-images.md#store-logos).

## <a name="trailers-and-additional-assets"></a>Nachspann und zusätzliche Assets

Sie können zusätzliche Assets für Ihr Produkt übermitteln, einschließlich Video Nachspann und Werbebilder. Diese sind alle optional, aber es wird empfohlen, dass Sie so viele wie möglich hochladen. Diese Images können Kunden dabei helfen, eine bessere Vorstellung davon zu erhalten, was Ihr Produkt ist, und eine verlockende Auflistung zu erstellen.

Weitere Informationen finden Sie unter Nachspann [und zusätzliche Ressourcen](app-screenshots-and-images.md#trailers-and-additional-assets).

<a name="supplemental-information"></a>

## <a name="supplemental-fields"></a>Ergänzende Felder

Die Felder in diesem Abschnitt sind optional. Überprüfen Sie die folgenden Informationen, um zu bestimmen, ob diese Informationen für ihre Übermittlung sinnvoll sind. Insbesondere wird die **kurze Beschreibung** für die meisten Einsendungen empfohlen. Die anderen Felder können Ihnen helfen, eine optimale Leistung für Ihr Produkt in verschiedenen Szenarien zu bieten.

### <a name="short-title"></a>Kurztitel

Eine kürzere Version des Produkt namens. Falls angegeben, wird dieser kürzere Name möglicherweise an verschiedenen Stellen auf Xbox One (während der Installation, in Errungenschaften usw.) anstelle des vollständigen Titels Ihres Produkts angezeigt.

Dieses Feld hat eine Beschränkung von 50 Zeichen.

### <a name="sort-title"></a>Sortier Titel

Wenn Ihr Produkt alphabetisch oder auf unterschiedliche Weise geschrieben werden kann, können Sie hier eine andere Version eingeben. Dadurch können Kunden Ihr Produkt schneller finden, wenn Sie diese Version in beim Suchen eingeben.

Dieses Feld hat eine Beschränkung von 255 Zeichen.

### <a name="voice-title"></a>Sprach Titel

Ein alternativer Name für Ihr Produkt, der ggf. in der Audiodarstellung auf Xbox One bei Verwendung von kinect oder einem Headset verwendet werden kann.

Dieses Feld hat eine Beschränkung von 255 Zeichen.

### <a name="short-description"></a>Kurze Beschreibung

Eine kürzere, einprägsame Beschreibung, die im oberen Bereich der Store-Auflistung Ihres Produkts verwendet werden kann. Wenn keine Angabe erfolgt, wird stattdessen der erste Absatz (bis zu 500 Zeichen) ihrer längeren [Beschreibung](#description) verwendet. Da Ihre Beschreibung auch unterhalb dieses Texts angezeigt wird, empfiehlt es sich, eine kurze Beschreibung mit einem anderen Text bereitzustellen, damit sich Ihre Store-Auflistung nicht wiederholt.

Bei spielen kann die kurze Beschreibung auch im Abschnitt "Informationen" des spielhubs auf Xbox One angezeigt werden.

Um optimale Ergebnisse zu erzielen, behalten Sie die kurze Beschreibung unter 270 Zeichen. Das Feld hat ein Limit von 1000 Zeichen, aber in einigen Ansichten werden nur die ersten 270 Zeichen angezeigt (mit einem Link, der zum Anzeigen der restlichen kurzen Beschreibung verfügbar ist).

### <a name="additional-system-requirements"></a>Weitere Systemanforderungen

Bei Bedarf können Sie die Hardwarekonfigurationen beschreiben, die für die ordnungsgemäße Funktionsweise der App erforderlich sind, über die Informationen hinaus, die Sie im Abschnitt **Systemanforderungen** unter [App-Eigenschaften](enter-app-properties.md#system-requirements) angegeben haben. Dies ist besonders wichtig, wenn Ihre App Hardware benötigt, die u. U. nicht auf jedem Computer vorhanden ist. Wenn Ihre APP z. b. nur ordnungsgemäß mit externer USB-Hardware wie z. b. einem 3D-Drucker oder einem Mikrocontroller funktioniert, empfiehlt es sich, diese hier einzugeben. Die von Ihnen eingegebenen Informationen werden Kunden angezeigt, die die Store-Auflistung Ihrer APP unter Windows 10, Version 1607 oder höher (einschließlich Xbox), zusammen mit den Anforderungen anzeigen, die Sie auf der Eigenschaften Seite des Produkts angegeben haben.

Sie können bis zu 11 Elemente sowohl für **Mindesthardwareanforderungen** als auch für **Empfohlene Hardware** eingeben. Diese werden dem Kunden als aufsortierte Liste in ihrer Store-Auflistung angezeigt. Die Beschreibung sollte pro Element nur wenige Wörter (und nicht mehr als 200 Zeichen) enthalten.

> [!NOTE]
> Ihre zusätzlichen Systemanforderungen werden in ihrer Store-Auflistung angezeigt. Fügen Sie also keine eigenen Aufzählungen hinzu.

<a name="shared-fields"></a>

## <a name="additional-information"></a>Zusätzliche Informationen

Die nachfolgend beschriebenen Elemente helfen Kunden dabei, Ihr Produkt zu ermitteln und zu verstehen. (Dieser Abschnitt wurde früher als frei **gegebene Felder** bezeichnet.)

### <a name="search-terms"></a>Suchbegriffe

Suchbegriffe (früher als Schlüsselwörter bezeichnet) sind einfache Wörter oder kurze Ausdrücke, die Kunden nicht angezeigt werden, aber Sie können dazu beitragen, dass Ihre APP im Store sichtbar ist, wenn die Kunden diese Begriffe durchsuchen. Sie können bis zu 7 Suchbegriffe mit maximal 30 Zeichen einschließen und dürfen nicht mehr als 21 separate Wörter für alle Suchbegriffe verwenden.

Berücksichtigen Sie beim Hinzufügen von Suchbegriffen die Wörter, die Kunden bei der Suche nach apps wie Ihrem verwenden können, insbesondere, wenn Sie nicht Teil des App-namens sind. Achten Sie darauf, keine Suchbegriffe zu verwenden, die für Ihre APP nicht relevant sind.

### <a name="copyright-and-trademark-info"></a>Urheberrecht- und Markeninformationen

Zusätzliche Urheberrecht- und Markeninformationen können Sie bei Bedarf hier eingeben. Dieses Feld ist auf 200 Zeichen beschränkt.

### <a name="additional-license-terms"></a>Zusätzliche Lizenzbedingungen

Lassen Sie dieses Feld leer, wenn Sie möchten, dass Ihre App für Kunden gemäß den **Standardbedingungen für anwendungsbezogene Lizenzen** (am Ende der [Vereinbarung für App-Entwickler](/legal/windows/agreements/app-developer-agreement)) lizenziert wird.

Wenn sich Ihre Lizenzbedingungen von den **Standardbedingungen für anwendungsbezogene Lizenzen** unterscheiden, geben Sie sie hier ein.

Wenn Sie einen einzelnen URL in dieses Feld eingeben, wird dieser für Kunden als Link angezeigt, auf den sie klicken können, um Ihre zusätzlichen Lizenzbedingungen zu lesen. Dies ist hilfreich, wenn Ihre zusätzlichen Lizenzbedingungen sehr lang sind, oder wenn Sie Links oder Formatierungen in die zusätzlichen Lizenzbedingungen einschließen möchten, auf die geklickt werden kann.

Sie können in diesem Feld bis zu 10.000 Zeichen Text eingeben. Wenn Sie dies tun, werden Kunden die zusätzlichen Lizenzbedingungen als reiner Text angezeigt.

### <a name="developed-by"></a>Entwickelt von

Geben Sie hier Text ein, wenn Sie ein Feld " **entwickelt von** " in die Store-Auflistung Ihrer APP einschließen möchten. (Im Feld **veröffentlicht von** wird der Anzeige Name des Herausgebers aufgelistet, der Ihrem Konto zugeordnet ist, und zwar unabhängig davon, ob Sie einen Wert für das Feld **entwickelt von** bereitstellen.)

Dieses Feld hat eine Beschränkung von 255 Zeichen.
 
<a name="privacy-policy"></a>

> [!NOTE]
> Die Felder " **Datenschutzrichtlinie** ", " **Website** " und "Support" für **Kontaktinformationen** befinden sich jetzt auf der [Eigenschaften](enter-app-properties.md) Seite.
