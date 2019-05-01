---
Description: Der erste Schritt beim Erstellen einer neuen app im Partner Center ist einen app-Namen reservieren. In diesem Thema wird erläutert, wie Sie App-Namen reservieren. Es enthält einige Vorschläge zur Wahl aussagekräftiger App-Namen.
title: Erstellen einer App durch Reservieren eines Namens
keywords: Windows 10, UWP, Namen reservieren, App-Name, App-Namen, Namen, Produktname, benennen, reservierter Name, Titel, Namen, Titel
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e1cb52bdb579f2ce4cda28440c0b820329173a3e
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63807374"
---
# <a name="create-your-app-by-reserving-a-name"></a>Erstellen einer App durch Reservieren eines Namens

Der erste Schritt beim Erstellen einer neuen app im [Partner Center](https://partner.microsoft.com/dashboard) einen app-Namen reserviert. Jeder reservierte Name (wird auch als App *Titel* bezeichnet) muss im Microsoft Store eindeutig sein.

Die Reservierung eines Namens für Ihre App ist möglich, auch wenn Sie noch nicht mit der App-Erstellung begonnen haben. Es wird empfohlen auf diese Weise so bald wie möglich, sodass niemand sonst auf den Namen verwenden kann. Beachten Sie, dass Sie die App innerhalb von drei Monaten übermitteln müssen, damit der Namen für Ihre Verwendung reserviert bleibt.

Wenn Sie Ihre [App-Pakete hochladen](upload-app-packages.md), muss der [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) -Wert dem Namen entsprechen, den Sie für Ihre App reserviert haben. Wenn Sie das App-Paket mit Microsoft Visual Studio erstellen, wird dieses Attribut für Sie ausgefüllt.

> [!IMPORTANT]
> Können weitere Namen für eine app reserviert, und Sie können auch einen der Werte in der veröffentlichten Version Ihrer App anstelle des verwenden, die Sie reservieren, wenn Sie die app zunächst im Partner Center erstellt. Bedenken Sie jedoch, dass der Vorname, die Sie hier einige Ihrer app verwendet wird [Details zur Authentifizierungsidentität](view-app-identity-details.md), z. B. die **Package Family Name (PFN)**. Diese Werte möglicherweise für einige Benutzer sichtbar und nicht geändert, also stellen Sie sicher, dass der Name, Sie reservieren, für diesen Zweck geeignet ist.


## <a name="create-your-app-by-reserving-a-new-name"></a>App-Erstellung durch Reservierung eines neuen Namens

Reservieren einen Namen, ist der erste Schritt beim Erstellen einer app in Partner Center. 

1.  Klicken Sie auf der Seite **Übersicht** auf **Neue App erstellen**.
2.  Geben Sie im Textfeld den Namen ein, den Sie verwenden möchten, und wählen Sie dann **Verfügbarkeit überprüfen**. Wenn der Name verfügbar ist, wird ein grünes Häkchen angezeigt. (Ist der von Ihnen eingegebene Name bereits reserviert oder wird von einem anderen Entwickler verwendet, wird eine Fehlermeldung angezeigt, dass der Name nicht verfügbar ist.)
3.  Klicken Sie auf **Produktnamen reservieren**.

Der Name ist jetzt für Sie reserviert, und Sie können mit der [Einreichung](app-submissions.md) beginnen, sobald Sie dazu bereit sind. 

> [!NOTE]
> Vielleicht können Sie einen Namen nicht reservieren, obwohl keine Apps mit diesem Namen im Microsoft Store gelistet sind. Das liegt normalerweise daran, dass ein anderer Entwickler den Namen für seine App reserviert, aber die entsprechende App noch nicht eingereicht hat. Wenn Sie einen Namen, den Sie markenrechtlich oder anderweitig geschützt haben, nicht reservieren können oder im Microsoft Store eine andere App mit diesem Namen entdecken, [wenden Sie sich an Microsoft](https://go.microsoft.com/fwlink/p/?LinkId=233777).

Nach dem Reservieren eines Namens haben Sie drei Monate Zeit, um die App einzureichen. Wenn Sie sie nicht innerhalb von drei Monaten einreichen, läuft die Reservierung des Namens ab, und ein anderer Entwickler kann eventuell den Namen für seine App verwenden. Wenn Sie versuchen, eine App unter einem abgelaufenen Namen einzureichen, tritt ein Fehler auf.


## <a name="choosing-your-apps-name"></a>Auswählen des App-Namens

Es ist sehr wichtig, für Ihre App den richtigen Namen auszuwählen. Wählen Sie einen Namen, der die Aufmerksamkeit von Kunden auf sich zieht und diese dazu animiert, sich ausführlicher über Ihre App zu informieren. Hier sind ein paar Tipps zum Auswählen eines geeigneten App-Namens.

-   **Halten Sie es kurz.** Für die Anzeige des App-Namens ist meist nur wenig Platz – lassen Sie sich also einen möglichst kurzen Namen einfallen. Der Name der App kann bis zu 256 Zeichen haben, aber möglicherweise ist das Ende eines sehr langen Namens für Kunden nicht immer sichtbar.
    > [!NOTE]
    > Die tatsächliche Anzahl von an unterschiedlichen Stellen angezeigten Zeichen kann je nach zugewiesener Länge und im App-Namen verwendetem Zeichentyp variieren. So benötigen beispielsweise in der von Windows verwendeten Schriftart „Segoe UI“ etwa 30 I-Zeichen genauso viel Platz wie zehn W-Zeichen. Aufgrund dieser Vielfalt werden Sie sicher, dass Ihre app testen und überprüfen, wie der Namen auf seine Kacheln angezeigt wird (Wenn Sie auswählen, für die Überlagerung von der app-Name), in den Suchergebnissen und innerhalb der app selbst. Berücksichtigen Sie auch die Sprachen, in denen Sie die App anbieten möchten. Denken Sie daran, dass ostasiatische Zeichen im Allgemeinen breiter sind als lateinische Zeichen, sodass weniger Zeichen angezeigt werden.
-   **Werden Sie ursprünglichen.** Suchen Sie einen eindeutigen App-Namen aus, damit die App nicht so leicht mit einer anderen App verwechselt werden kann.
-   **Verwenden Sie keine Namen, die von anderen Markennamen.** Stellen Sie sicher, dass sie zum Verwenden des reservierten Namens berechtigt sind. Wurde der Name als Marke eingetragen, kann ein Verstoß gemeldet werden, und Sie dürfen den Namen nicht weiter verwenden. Geschieht dies nach der Veröffentlichung Ihrer App, wird sie aus dem Store entfernt. Sie müssen dann den Namen der App sowie alle Vorkommen des Namens innerhalb Ihrer App und der zugehörigen Inhalte ändern, bevor Sie [die App erneut zur Zertifizierung einreichen](app-submissions.md).
-   **Vermeiden Sie unterscheidenden Informationen am Ende des Namens hinzufügen.** Wenn Infos zur Unterscheidung mehrerer Apps am Ende eines Namens angefügt werden, werden sie von den Kunden vor allem bei langen Namen vielleicht übersehen, und es scheint, als hätten alle Apps den gleichen Namen. Ist dies nicht zu vermeiden, verwenden Sie verschiedene Logos und app-Images um zu erleichtern, eine app von einem anderen zu unterscheiden.
-   **Schließen Sie nicht Emojis in Ihren Namen.** Sie können keinen Namen reservieren, der Emoticons oder andere nicht unterstützte Zeichen enthält.


## <a name="manage-additional-app-names"></a>Verwalten zusätzlicher App-Namen

Sie können das Hinzufügen und Verwalten von weiteren Namen auf die **Verwalten von app-Namen** auf der Seite die **App-Verwaltung** Abschnitt für jede Ihrer apps im Partner Center.

In einigen Fällen möchten Sie u. U. mehrere Namen reservieren, die für dieselbe App verwendet werden sollen; beispielsweise wenn Sie Ihre App unter verschiedenen Namen für jede Sprache anbieten möchten. Sie müssen einen zusätzlichen Namen reservieren, wenn Sie den Namen einer App vollständig ändern möchten.

Auf dieser Seite können Sie auch reservierte Namen, die Sie nicht mehr verwenden, löschen.

Weitere Informationen finden Sie unter [Verwalten von App-Namen](manage-app-names.md).

 

 




