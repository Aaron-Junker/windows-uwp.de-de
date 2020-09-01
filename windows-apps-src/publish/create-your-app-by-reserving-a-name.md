---
Description: Der erste Schritt beim Erstellen einer neuen app in Partner Center ist das Reservieren eines App-namens. In diesem Thema wird erläutert, wie Sie App-Namen reservieren. Es enthält einige Vorschläge zur Wahl aussagekräftiger App-Namen.
title: Erstellen einer App durch Reservieren eines Namens
keywords: Windows 10, UWP, namens Reservierung, App-Name, APP-Namen, Namen, Produktname, Benennung, reservierter Name, Titel, Namen, Titel
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: e2b95e30faa64ce6507de5a57b350ec59ca683e1
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89164174"
---
# <a name="create-your-app-by-reserving-a-name"></a>Erstellen einer App durch Reservieren eines Namens

Der erste Schritt beim Erstellen einer neuen app in [Partner Center](https://partner.microsoft.com/dashboard) ist das Reservieren eines App-namens. Jeder reservierte Name (manchmal auch als *Titel*Ihrer APP bezeichnet) muss im gesamten Microsoft Store eindeutig sein.

Sie können einen Namen für Ihre APP reservieren, auch wenn Sie noch nicht damit begonnen haben, Ihre APP zu erstellen. Es wird empfohlen, so bald wie möglich zu verwenden, damit niemand andere den Namen verwenden kann. Beachten Sie, dass Sie die APP innerhalb von drei Monaten übermitteln müssen, um den Namen für ihre Verwendung reserviert zu halten.

Wenn Sie die [Pakete der APP hochladen](upload-app-packages.md), muss der Wert für " [**Package/Properties/Display Name**](/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) " mit dem Namen, den Sie für Ihre APP reserviert haben, identisch sein. Wenn Sie das App-Paket mit Microsoft Visual Studio erstellen, wird dieses Attribut für Sie ausgefüllt.

> [!IMPORTANT]
> Sie können zusätzliche Namen für eine APP reservieren, und Sie können entscheiden, ob Sie eine der apps in der veröffentlichten Version Ihrer APP verwenden möchten, anstatt Sie zu verwenden, wenn Sie Ihre APP zum ersten Mal im Partner Center erstellen. Beachten Sie jedoch, dass der erste Name, den Sie hier eingeben, in einigen der [Identitäts Details](view-app-identity-details.md)Ihrer APP verwendet wird, wie z. b. der **Paket Familienname (PFN)**. Diese Werte sind möglicherweise für einige Benutzer sichtbar und können nicht geändert werden. Stellen Sie daher sicher, dass der von Ihnen reservierte Name für diese Verwendung geeignet ist.


## <a name="create-your-app-by-reserving-a-new-name"></a>App-Erstellung durch Reservierung eines neuen Namens

Das Reservieren eines Namens ist der erste Schritt beim Erstellen einer APP im Partner Center. 

1.  Klicken Sie auf der Seite **Übersicht** auf **neue APP erstellen**.
2.  Geben Sie im Textfeld den Namen ein, den Sie verwenden möchten, und wählen Sie dann **Verfügbarkeit überprüfen**. Wenn der Name verfügbar ist, wird ein grünes Häkchen angezeigt. (Ist der von Ihnen eingegebene Name bereits reserviert oder wird von einem anderen Entwickler verwendet, wird eine Fehlermeldung angezeigt, dass der Name nicht verfügbar ist.)
3.  Klicken Sie auf **Produktnamen reservieren**.

Der Name ist nun für Sie reserviert, und Sie können mit der Arbeit an Ihrer [Übermittlung](app-submissions.md) beginnen, sobald Sie bereit sind. 

> [!NOTE]
> Möglicherweise stellen Sie fest, dass Sie keinen Namen reservieren können, auch wenn im Microsoft Store keine apps angezeigt werden, die mit diesem Namen aufgelistet sind. Das liegt normalerweise daran, dass ein anderer Entwickler den Namen für seine App reserviert, aber die entsprechende App noch nicht eingereicht hat. Wenn Sie nicht in der Lage sind, einen Namen zu reservieren, für den Sie die Marke oder ein anderes Recht Recht haben, oder wenn Sie eine andere app in der Microsoft Store mit diesem Namen sehen, wenden Sie sich [an Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html).

Nachdem Sie einen Namen reserviert haben, haben Sie drei Monate Zeit, um die APP zu übermitteln. Wenn Sie Sie nicht innerhalb von drei Monaten einreichen, läuft die namens Reservierung ab, und ein anderer Entwickler kann diesen Namen für eine APP verwenden. Wenn Sie versuchen, eine App unter einem abgelaufenen Namen einzureichen, tritt ein Fehler auf.


## <a name="choosing-your-apps-name"></a>Auswählen des App-Namens

Es ist sehr wichtig, für Ihre App den richtigen Namen auszuwählen. Wählen Sie einen Namen, der die Aufmerksamkeit von Kunden auf sich zieht und diese dazu animiert, sich ausführlicher über Ihre App zu informieren. Hier sind ein paar Tipps zum Auswählen eines geeigneten App-Namens.

-   **Halten Sie den Namen kurz.** Für die Anzeige des App-Namens ist meist nur wenig Platz – lassen Sie sich also einen möglichst kurzen Namen einfallen. Der Name der App kann bis zu 256 Zeichen haben, aber möglicherweise ist das Ende eines sehr langen Namens für Kunden nicht immer sichtbar.
    > [!NOTE]
    > Die tatsächliche Anzahl von Zeichen, die an verschiedenen Speicherorten angezeigt werden, kann variieren. Dies hängt von der zugewiesenen Länge und den Zeichen Typen ab, die im Namen Ihrer APP verwendet werden. So benötigen beispielsweise in der von Windows verwendeten Schriftart „Segoe UI“ etwa 30 I-Zeichen genauso viel Platz wie zehn W-Zeichen. Stellen Sie aufgrund dieser Variation sicher, dass Sie Ihre APP testen und überprüfen, wie Ihr Name auf ihren Kacheln angezeigt wird (wenn Sie den APP-Namen überlagern), in den Suchergebnissen und innerhalb der APP selbst. Berücksichtigen Sie auch die Sprachen, in denen Sie die App anbieten möchten. Denken Sie daran, dass ostasiatische Zeichen im Allgemeinen breiter sind als lateinische Zeichen, sodass weniger Zeichen angezeigt werden.
-   **Seien Sie unverwechselbar.** Suchen Sie einen eindeutigen App-Namen aus, damit die App nicht so leicht mit einer anderen App verwechselt werden kann.
-   **Verwenden Sie keine von anderen geschützten Namen.** Stellen Sie sicher, dass sie zum Verwenden des reservierten Namens berechtigt sind. Wurde der Name als Marke eingetragen, kann ein Verstoß gemeldet werden, und Sie dürfen den Namen nicht weiter verwenden. Geschieht dies nach der Veröffentlichung Ihrer App, wird sie aus dem Store entfernt. Sie müssen dann den Namen der App sowie alle Vorkommen des Namens innerhalb Ihrer App und der zugehörigen Inhalte ändern, bevor Sie [die App erneut zur Zertifizierung einreichen](app-submissions.md).
-   **Fügen Sie Informationen zur Unterscheidung nicht am Ende des Namens hinzu.** Wenn Infos zur Unterscheidung mehrerer Apps am Ende eines Namens angefügt werden, werden sie von den Kunden vor allem bei langen Namen vielleicht übersehen, und es scheint, als hätten alle Apps den gleichen Namen. Wenn dies unvermeidlich ist, verwenden Sie verschiedene Logos und App-Images, um die Unterscheidung einer APP von einer anderen zu vereinfachen.
-   **Fügen Sie keine Emojis in ihren Namen ein.** Sie sind nicht in der Lage, einen Namen zu reservieren, der Emojis oder andere nicht unterstützte Zeichen enthält.


## <a name="manage-additional-app-names"></a>Verwalten zusätzlicher App-Namen

Sie können zusätzliche Namen auf der Seite APP- **Namen verwalten** im Abschnitt APP- **Verwaltung** für jede Ihrer Apps im Partner Center hinzufügen und verwalten.

In einigen Fällen möchten Sie möglicherweise mehrere Namen für die gleiche APP reservieren, z. b. Wenn Sie Ihre APP in mehreren Sprachen anbieten möchten und für jede Sprache verschiedene Namen verwenden möchten. Sie müssen einen zusätzlichen Namen reservieren, wenn Sie den Namen einer App vollständig ändern möchten.

Auf dieser Seite können Sie auch reservierte Namen, die Sie nicht mehr verwenden, löschen.

Weitere Informationen finden Sie unter [Verwalten von App-Namen](manage-app-names.md).

 

 