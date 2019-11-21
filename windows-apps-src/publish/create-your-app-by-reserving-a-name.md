---
Description: The first step in creating a new app in Partner Center is reserving an app name. In diesem Thema wird erläutert, wie Sie App-Namen reservieren. Es enthält einige Vorschläge zur Wahl aussagekräftiger App-Namen.
title: Erstellen einer App durch Reservieren eines Namens
keywords: Windows 10, UWP, Namen reservieren, App-Name, App-Namen, Namen, Produktname, benennen, reservierter Name, Titel, Namen, Titel
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 79831e1e14ddf8c525f1697074e6a435513f5ea1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259028"
---
# <a name="create-your-app-by-reserving-a-name"></a>Erstellen einer App durch Reservieren eines Namens

The first step in creating a new app in [Partner Center](https://partner.microsoft.com/dashboard) is reserving an app name. Jeder reservierte Name (wird auch als App *Titel* bezeichnet) muss im Microsoft Store eindeutig sein.

Die Reservierung eines Namens für Ihre App ist möglich, auch wenn Sie noch nicht mit der App-Erstellung begonnen haben. We recommend doing so as soon as possible, so that nobody else can use the name. Beachten Sie, dass Sie die App innerhalb von drei Monaten übermitteln müssen, damit der Namen für Ihre Verwendung reserviert bleibt.

Wenn Sie Ihre [App-Pakete hochladen](upload-app-packages.md), muss der [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) -Wert dem Namen entsprechen, den Sie für Ihre App reserviert haben. Wenn Sie das App-Paket mit Microsoft Visual Studio erstellen, wird dieses Attribut für Sie ausgefüllt.

> [!IMPORTANT]
> You can reserve additional names for an app, and you may choose to use one of those in the published version of your app instead of the one you reserve when you first create your app in Partner Center. However, be aware that the first name you enter here will be used in some of your app's [identity details](view-app-identity-details.md), such as the **Package Family Name (PFN)** . These values may be visible to some users, and cannot be changed, so make sure that the name you reserve is appropriate for this use.


## <a name="create-your-app-by-reserving-a-new-name"></a>App-Erstellung durch Reservierung eines neuen Namens

Reserving a name is the first step in creating an app in Partner Center. 

1.  Klicken Sie auf der Seite **Übersicht** auf **Neue App erstellen**.
2.  Geben Sie im Textfeld den Namen ein, den Sie verwenden möchten, und wählen Sie dann **Verfügbarkeit überprüfen**. Wenn der Name verfügbar ist, wird ein grünes Häkchen angezeigt. (Ist der von Ihnen eingegebene Name bereits reserviert oder wird von einem anderen Entwickler verwendet, wird eine Fehlermeldung angezeigt, dass der Name nicht verfügbar ist.)
3.  Klicken Sie auf **Produktnamen reservieren**.

Der Name ist jetzt für Sie reserviert, und Sie können mit der [Einreichung](app-submissions.md) beginnen, sobald Sie dazu bereit sind. 

> [!NOTE]
> Vielleicht können Sie einen Namen nicht reservieren, obwohl keine Apps mit diesem Namen im Microsoft Store gelistet sind. Das liegt normalerweise daran, dass ein anderer Entwickler den Namen für seine App reserviert, aber die entsprechende App noch nicht eingereicht hat. Wenn Sie einen Namen, den Sie markenrechtlich oder anderweitig geschützt haben, nicht reservieren können oder im Microsoft Store eine andere App mit diesem Namen entdecken, [wenden Sie sich an Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html).

Nach dem Reservieren eines Namens haben Sie drei Monate Zeit, um die App einzureichen. Wenn Sie sie nicht innerhalb von drei Monaten einreichen, läuft die Reservierung des Namens ab, und ein anderer Entwickler kann eventuell den Namen für seine App verwenden. Wenn Sie versuchen, eine App unter einem abgelaufenen Namen einzureichen, tritt ein Fehler auf.


## <a name="choosing-your-apps-name"></a>Auswählen des App-Namens

Es ist sehr wichtig, für Ihre App den richtigen Namen auszuwählen. Wählen Sie einen Namen, der die Aufmerksamkeit von Kunden auf sich zieht und diese dazu animiert, sich ausführlicher über Ihre App zu informieren. Hier sind ein paar Tipps zum Auswählen eines geeigneten App-Namens.

-   **Keep it short.** Für die Anzeige des App-Namens ist meist nur wenig Platz – lassen Sie sich also einen möglichst kurzen Namen einfallen. Der Name der App kann bis zu 256 Zeichen haben, aber möglicherweise ist das Ende eines sehr langen Namens für Kunden nicht immer sichtbar.
    > [!NOTE]
    > Die tatsächliche Anzahl von an unterschiedlichen Stellen angezeigten Zeichen kann je nach zugewiesener Länge und im App-Namen verwendetem Zeichentyp variieren. So benötigen beispielsweise in der von Windows verwendeten Schriftart „Segoe UI“ etwa 30 I-Zeichen genauso viel Platz wie zehn W-Zeichen. Because of this variation, be sure to test your app and verify how its name appears on its tiles (if you choose to overlay the app name), in search results, and within the app itself. Berücksichtigen Sie auch die Sprachen, in denen Sie die App anbieten möchten. Denken Sie daran, dass ostasiatische Zeichen im Allgemeinen breiter sind als lateinische Zeichen, sodass weniger Zeichen angezeigt werden.
-   **Be original.** Suchen Sie einen eindeutigen App-Namen aus, damit die App nicht so leicht mit einer anderen App verwechselt werden kann.
-   **Don't use names trademarked by others.** Stellen Sie sicher, dass sie zum Verwenden des reservierten Namens berechtigt sind. Wurde der Name als Marke eingetragen, kann ein Verstoß gemeldet werden, und Sie dürfen den Namen nicht weiter verwenden. Geschieht dies nach der Veröffentlichung Ihrer App, wird sie aus dem Store entfernt. Sie müssen dann den Namen der App sowie alle Vorkommen des Namens innerhalb Ihrer App und der zugehörigen Inhalte ändern, bevor Sie [die App erneut zur Zertifizierung einreichen](app-submissions.md).
-   **Avoid adding differentiating info at the end of the name.** Wenn Infos zur Unterscheidung mehrerer Apps am Ende eines Namens angefügt werden, werden sie von den Kunden vor allem bei langen Namen vielleicht übersehen, und es scheint, als hätten alle Apps den gleichen Namen. If this is unavoidable, use different logos and app images to make it easier to differentiate one app from another.
-   **Don't include emojis in your name.** Sie können keinen Namen reservieren, der Emoticons oder andere nicht unterstützte Zeichen enthält.


## <a name="manage-additional-app-names"></a>Verwalten zusätzlicher App-Namen

You can add and manage additional names on the **Manage app names** page in the **App management** section for each of your apps in Partner Center.

In einigen Fällen möchten Sie u. U. mehrere Namen reservieren, die für dieselbe App verwendet werden sollen; beispielsweise wenn Sie Ihre App unter verschiedenen Namen für jede Sprache anbieten möchten. Sie müssen einen zusätzlichen Namen reservieren, wenn Sie den Namen einer App vollständig ändern möchten.

Auf dieser Seite können Sie auch reservierte Namen, die Sie nicht mehr verwenden, löschen.

Weitere Informationen finden Sie unter [Verwalten von App-Namen](manage-app-names.md).

 

 




