---
Description: Dialogfelder und Flyouts zeigen vorübergehende UI-Elemente an, die angezeigt werden, wenn der Benutzer sie anfordert oder eine Aktion erfolgt, die eine Benachrichtigung oder Genehmigung erfordert.
title: Dialogfeld-Steuerelemente
label: Dialogs
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: bee954cba446ac7dc7eb41622d9275b3b73af6ee
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621835"
---
# <a name="dialog-controls"></a>Dialogfeld-Steuerelemente

Dialogfeld-Steuerelemente sind modale Benutzeroberfläche überlagert, die kontextbezogene app-Informationen bereitstellen. Sie blockieren Interaktionen mit dem app-Fenster, bis Sie explizit verworfen wird. Sie verlangen häufig eine Aktion vom Benutzer.

![Beispiel für ein Dialogfeld](../images/dialogs/dialog_RS2_delete_file.png)


> **Wichtige APIs**: [ContentDialog-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie Dialogfelder und Flyouts, um Benutzern wichtige Informationen mitzuteilen oder deren Bestätigung bzw. zusätzliche Informationen anzufordern, bevor eine Aktion abgeschlossen werden kann.

Finden Sie Empfehlungen dazu, wann verwenden Sie ein Dialogfeld oder ein Flyout anzeigt (ein ähnliches Steuerelement), verwenden Sie [Dialogfelder und Flyouts](index.md). 

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> oder <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Abrufen der XAML-Steuerelemente Katalog-app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="general-guidelines"></a>Allgemeine Richtlinien

-   Sie sollten das Problem oder das Ziel des Benutzers in der ersten Zeile des Dialogfeldtexts deutlich benennen.
-   Der Dialogfeldtitel ist die Hauptanweisung und optional.
    -   Verwenden Sie einen kurzen Titel, um dem Benutzer die erforderlichen Aktionen im Dialogfeld zu erklären.
    -   Wenn Sie mit dem Dialogfeld eine einfache Meldung, einen Fehler oder eine Frage anzeigen möchten, können Sie den Titel optional auslassen. Vermitteln Sie dann im Inhalt die wichtigsten Informationen.
    -   Stellen Sie sicher, dass sich der Titel direkt auf die Auswahlmöglichkeiten der Schaltflächen bezieht.
-   Der Inhalt des Dialogfelds enthält den beschreibenden Text und ist erforderlich.
    -   Stellen Sie die Meldung, den Fehler oder die blockierende Frage so einfach wie möglich dar.
    -   Stellen Sie bei Verwendung eines Dialogfeldtitels mithilfe des Inhaltsbereichs weitere Details bereit, oder definieren Sie Terminologie. Wiederholen Sie nicht den Titel mit anderen Worten.
-   Mindestens eine Dialogfeldschaltfläche muss angezeigt werden.
    -   Stellen Sie sicher, dass Ihr Dialogfeld mindestens eine Schaltfläche aufweist, die eine sichere, nicht-destruktive Aktion wie „Alles klar!“, „Schließen“ oder „Abbrechen“ auslöst. Verwenden Sie die CloseButton-API, um diese Schaltfläche hinzuzufügen.
    -   Verwenden Sie für den Schaltflächentext konkrete Antworten auf die Hauptanweisung oder den Inhalt. Beispiel: "Möchten Sie AppName den Zugriff auf Ihren Standort erlauben?", gefolgt von den Schaltflächen "Zulassen" und "Blockieren". Klare Antworten erleichtern das Verständnis und damit die schnelle Entscheidungsfindung.
    - Stellen Sie sicher, dass der Text der Aktionsschaltflächen kurz ist. Kurze Anweisungen ermöglichen dem Benutzer, schnell und zuverlässig eine Entscheidung zu treffen.
    - Zusätzlich zur sicheren, nicht-destruktiven Aktion können Sie dem Benutzer optional ein oder zwei Aktionsschaltflächen anzeigen, die im Zusammenhang mit der Hauptanweisung stehen. Diese „bestätigenden“ Aktionsschaltflächen unterstreichen den Hauptgrund des Dialogfelds. Verwenden Sie die PrimaryButton- und SecondaryButton-APIs, um diese „bestätigenden“ Aktionen hinzufügen.
    - Die „bestätigenden“ Aktionsschaltflächen sollten als die am weitesten links stehenden Schaltflächen angezeigt werden. Die sichere, nicht-destruktive Aktion sollte als die am weitesten rechts stehende Schaltfläche angezeigt werden.
    - Sie können optional eine der drei Schaltflächen als Standardschaltfläche des Dialogfelds festlegen. Verwenden Sie die DefaultButton-API, um eine der Schaltflächen abzugrenzen.  
-   Verwenden Sie Dialogfelder nicht für kontextbezogene Fehler, die sich auf eine bestimmte Stelle auf der Seite beziehen, beispielsweise Validierungsfehler (wie in Kennwortfeldern). Verwenden Sie die Canvas der App selbst zum Anzeigen von Inlinefehlern.
- Verwenden Sie die [ContentDialog-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), um Ihre Dialogfeldumgebung zu erstellen. Verwenden Sie nicht die veraltete MessageDialog-API.

## <a name="how-to-create-a-dialog"></a>Vorgehensweise: Erstellen Sie ein Dialogfeld
Um ein Dialogfeld zu erstellen, verwenden Sie die [ContentDialog-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog). Sie können ein Dialogfeld im Code oder Markup erstellen. Obwohl es in der Regel leichter ist, UI-Elemente in XAML zu definieren, ist es bei einem einfachen Dialogfeld unkomplizierter, Code zu verwenden. In diesem Beispiel wird ein Dialogfeld erstellt, um dem Benutzer mitzuteilen, dass keine WLAN-Verbindung vorhanden ist. Für die Anzeige wird die Methode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) verwendet.

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Wenn der Benutzer auf eine Dialogfeldschaltfläche klickt, gibt die Methode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) ein [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) zurück, damit Sie wissen, auf welche Schaltfläche der Benutzer klickt.

Das Dialogfeld in diesem Beispiel stellt eine Frage und verwendet das zurückgegebene [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult), um die Antwort des Benutzers zu ermitteln.

```csharp
private async void DisplayDeleteFileDialog()
{
    ContentDialog deleteFileDialog = new ContentDialog
    {
        Title = "Delete file permanently?",
        Content = "If you delete this file, you won't be able to recover it. Do you want to delete it?",
        PrimaryButtonText = "Delete",
        CloseButtonText = "Cancel"
    };

    ContentDialogResult result = await deleteFileDialog.ShowAsync();

    // Delete the file if the user clicked the primary button.
    /// Otherwise, do nothing.
    if (result == ContentDialogResult.Primary)
    {
        // Delete the file.
    }
    else
    {
        // The user clicked the CLoseButton, pressed ESC, Gamepad B, or the system back button.
        // Do nothing.
    }
}
```

## <a name="provide-a-safe-action"></a>Geben Sie ein sicheres vorgehen.
Da Dialogfelder Benutzerinteraktion blockieren, und Schaltflächen für die Benutzer das primäre Mittel sind, ein Dialogfeld zu schließen, sollten Sie sicherstellen, dass Ihr Dialogfeld mindestens eine „sichere“, nicht-destruktive Schaltfläche wie z. B. „Schließen“ oder „Alles klar!“ enthält. **Alle Dialoge müssen mindestens ein sicheres vorgehen-Schaltfläche, um das Dialogfeld zu schließen enthalten.** Dadurch wird sichergestellt, dass der Benutzer das Dialogfeld zuverlässig schließen kann, ohne eine Aktion auszuführen.<br>![Ein Dialogfeld eine Schaltfläche](../images/dialogs/dialog_RS2_one_button.png)

```csharp
private async void DisplayNoWifiDialog()
{
    ContentDialog noWifiDialog = new ContentDialog
    {
        Title = "No wifi connection",
        Content = "Check your connection and try again.",
        CloseButtonText = "Ok"
    };

    ContentDialogResult result = await noWifiDialog.ShowAsync();
}
```

Wenn Dialogfelder dazu verwendet werden, eine blockierende Frage anzuzeigen, sollte Ihr Dialogfeld dem Benutzer Aktionsschaltflächen bieten, die im Zusammenhang mit der Frage stehen. Die „sichere“, nicht-destruktive Schaltfläche kann durch ein oder zwei „bestätigende“ Aktionsschaltflächen ergänzt werden. Wenn dem Benutzer mehrere Optionen angezeigt werden, stellen Sie sicher, dass die Schaltflächen für die „bestätigende“ und die sichere/„ablehnende“ Aktion den Bezug zur Frage eindeutig darstellen.

![Dialogfeld mit zwei Schaltflächen](../images/dialogs/dialog_RS2_two_button.png)

```csharp
private async void DisplayLocationPromptDialog()
{
    ContentDialog locationPromptDialog = new ContentDialog
    {
        Title = "Allow AppName to access your location?",
        Content = "AppName uses this information to help you find places, connect with friends, and more.",
        CloseButtonText = "Block",
        PrimaryButtonText = "Allow"
    };

    ContentDialogResult result = await locationPromptDialog.ShowAsync();
}
```

Dialogfelder mit drei Schaltflächen werden verwendet, wenn Sie dem Benutzer zwei „bestätigende“ und eine „ablehnende“ Aktion präsentierten möchten. Dialogfelder mit drei Schaltflächen sollten sparsam verwendet werden und eine klare Unterscheidung zwischen der Sekundäraktion und der sicheren/ablehnenden Aktion enthalten.

![Dialogfeld mit drei Schaltflächen](../images/dialogs/dialog_RS2_three_button.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

## <a name="the-three-dialog-buttons"></a>Die drei Schaltflächen des Dialogfelds
Der ContentDialog umfasst drei unterschiedliche Arten von Schaltflächen, die Sie zur Erstellung einer Dialogfeldumgebung verwenden können.

- **CloseButton** – erforderlich – Stellt die sichere, nicht-destruktive Aktion dar, die es dem Benutzer ermöglicht, das Dialogfeld zu schließen. Wird als die am weitesten rechts stehende Schaltfläche angezeigt.
- **PrimaryButton** – optional – Stellt die erste „bestätigende“ Aktion dar. Wird als die am weitesten links stehende Schaltfläche angezeigt.
- **SecondaryButton** – optional – Stellt die zweite „bestätigende“ Aktion dar. Wird als mittlere Schaltfläche angezeigt.

Mithilfe der integrierten Schaltflächen kann das Dialogfeld optisch an andere Dialogfelder angepasst werden sowie die Position der Schaltflächen passend ausgerichtet werden. Stellen Sie sicher, dass die Schaltflächen entsprechend auf Tastaturbefehle reagieren und der Befehlsbereich sichtbar bleibt, auch wenn die Bildschirmtastatur aufgerufen ist.

### <a name="closebutton"></a>CloseButton
Jedes Dialogfeld sollte eine sichere, nicht-destruktive Aktionsschaltfläche aufweisen, die dem Benutzer das zuverlässige Beenden des Dialogfelds ermöglicht.

Verwenden Sie die ContentDialog.CloseButton-API, um diese Schaltfläche zu erstellen. Dadurch schaffen Sie die jeweils richtige Benutzerumgebung für alle Eingabemöglichkeiten wie z. B. Maus, Tastatur, Fingereingabe und Gamepad. Dies ist nötig, wenn:
<ol>
    <li>Der Benutzer auf CloseButton klickt oder tippt </li>
    <li>Der Benutzer die Zurück-Taste des Systems betätigt </li>
    <li>Der Benutzer auf der Tastatur die ESC-Taste betätigt </li>
    <li>Der Benutzer auf dem Gamepad die B-Taste betätigt </li>
</ol>

Wenn der Benutzer auf eine Dialogfeldschaltfläche klickt, gibt die Methode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) ein [ContentDialogResult](/uwp/api/Windows.UI.Xaml.Controls.ContentDialogResult) zurück, damit Sie wissen, auf welche Schaltfläche der Benutzer klickt. Klicken auf CloseButton resultiert in ContentDialogResult.None.

### <a name="primarybutton-and-secondarybutton"></a>PrimaryButton und SecondaryButton
Zusätzlich zu CloseButton können Sie dem Benutzer optional ein oder zwei Aktionsschaltflächen anbieten, die im Zusammenhang mit der Hauptanweisung stehen.
Nutzen Sie PrimaryButton für die erste „bestätigende“ Aktion und SecondaryButton für die zweite „bestätigende“ Aktion. Bei Dialogfeldern mit drei Schaltflächen stellt PrimaryButton in der Regel eine eindeutig „bestätigende“ Aktion dar, während SecondaryButton in der Regel eine neutrale oder sekundäre, „bestätigende“ Aktion darstellt.
Beispielsweise kann eine App den Benutzer dazu auffordern, einen Dienst zu abonnieren. In diesem Fall ist PrimaryButton die eindeutig „bestätigende“ Aktion und enthält den Text „Abonnieren“, während SecondaryButton als neutrale, „bestätigende“ Aktion den Text „Testen“ aufweist. CloseButton ermöglicht dem Benutzer in diesem Fall, die Aktion ohne Interaktion abzubrechen.

Klickt der Benutzer auf PrimaryButton, wird von der Methode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) ContentDialogResult.Primary zurückgegeben.
Klickt der Benutzer auf die SecondaryButton, wird von der Methode [ShowAsync](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog.ShowAsync) ContentDialogResult.Primary zurückgegeben.

![Dialogfeld mit drei Schaltflächen](../images/dialogs/dialog_RS2_three_button.png)

### <a name="defaultbutton"></a>DefaultButton
Sie können optional eine der drei Schaltflächen als Standardschaltfläche festlegen. Das Festlegen einer Standardschaltfläche hat zur Folge, dass:
- die Schaltfläche optisch als Akzentschaltfläche behandelt wird
- die Schaltfläche automatisch auf die EINGABETASTE reagiert
    - Drückt der Benutzer auf der Tastatur die EINGABETASTE, wird der mit der Standardschaltfläche verknüpfte Click-Handler ausgelöst und das ContentDialogResult gibt den mit der Standardschaltfläche verknüpften Wert zurück
    - Wenn der Benutzer den Tastaturfokus auf ein Steuerelement gelegt hat, das die EINGABETASTE handhabt, reagiert die Standardschaltfläche nicht auf Drücken der EINGABETASTE
- Die Schaltfläche erhält den Fokus automatisch, wenn das Dialogfeld geöffnet wird, es sei denn, der Inhalt des Dialogfelds enthält fokussierbare UI

Verwenden Sie die ContentDialog.DefaultButton-Eigenschaft, um die Standardschaltfläche anzugeben. Standardmäßig wird keine Standardschaltfläche festgelegt.

![Ein Dialogfeld mit drei Schaltflächen mit einer Standardschaltfläche](../images/dialogs/dialog_RS2_three_button_default.png)

```csharp
private async void DisplaySubscribeDialog()
{
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it",
        DefaultButton = ContentDialogButton.Primary
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

## <a name="confirmation-dialogs-okcancel"></a>Bestätigungsdialogfelder (OK/Abbrechen)
In einem Bestätigungsdialogfeld können Benutzer bestätigen, dass sie eine Aktion ausführen möchten. Sie können die Aktion bestätigen oder den Vorgang abbrechen.  
Ein typisches Bestätigungsdialogfeld verfügt über zwei Schaltflächen: eine Schaltfläche zur Bestätigung („OK“) und eine Schaltfläche zum Abbrechen.  

<ul>
    <li>
        <p>Die Bestätigungsschaltfläche sollte sich im Allgemeinen auf der linken Seite (die primäre Schaltfläche) und die Abbruchschaltfläche (die sekundäre Schaltfläche) auf der rechten Seite befinden.</p>
        <img alt="An OK/cancel dialog" src="../images/dialogs/dialog_RS2_delete_file.png" />
    </li>
    <li>Wie in den allgemeinen Empfehlungen erwähnt, sollten Sie Schaltflächen mit Text verwenden, der konkrete Antworten auf die Hauptanweisung bzw. den Hauptinhalt bietet.
    </li>
</ul>

> Auf einigen Plattformen befindet sich die Bestätigungsschaltfläche auf der rechten anstatt auf der linken Seite. Warum empfehlen wir die Platzierung auf der linken Seite?  Wenn Sie davon ausgehen, dass die meisten Benutzer Rechtshänder sind und ihr Telefon in dieser Hand halten, ist es bequemer, eine Bestätigungsschaltfläche zu drücken, die sich auf der linken Seite befindet, weil sie für den Benutzer wahrscheinlich einfacher mit dem Daumen erreichbar ist. Bei Schaltflächen auf der rechten Bildschirmseite muss der Benutzer seinen Daumen in eine weniger bequeme Position nach innen bewegen.





## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel
- [QuickInfos](../tooltips.md)
- [Menüs und Kontextmenüs](../menus.md)
- [Flyout-Klasse](/uwp/api/Windows.UI.Xaml.Controls.Flyout)
- [ContentDialog-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog)
