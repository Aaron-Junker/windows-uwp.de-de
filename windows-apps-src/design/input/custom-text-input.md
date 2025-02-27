---
description: Mit den Kerntext-APIs im Windows. UI. Text. Core-Namespace kann eine Windows-App Texteingaben von einem beliebigen Text Dienst empfangen, der auf Windows-Geräten unterstützt wird.
title: Übersicht über benutzerdefinierte Texteingabe
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: Tastatur, Text, Core-Text, benutzerdefinierter Text, Textdienstframework, Eingabe, Benutzerinteraktionen
ms.date: 09/24/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 95dbd6de78cb6670ea7e904252bbc1f9f14edb77
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829635"
---
# <a name="custom-text-input"></a>Benutzerdefinierte Texteingabe



Mit den Kerntext-APIs im [**Windows. UI. Text. Core**](/uwp/api/Windows.UI.Text.Core) -Namespace kann eine Windows-App Texteingaben von einem beliebigen Text Dienst empfangen, der auf Windows-Geräten unterstützt wird. Die APIs sind den [Textdienstframework](/windows/desktop/TSF/text-services-framework)-APIs dahingehend ähnlich, dass die App keine detaillierten Kenntnisse über die Textdienste benötigt. Auf diese Weise kann die App Text in einer beliebigen Sprache und von einem beliebigen Eingabegerät empfangen, wie Tastatur, Sprache oder Stift.

> **Wichtige APIs**: [**Windows. UI. Text. Core**](/uwp/api/Windows.UI.Text.Core), [**coretexteditcontext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext)

## <a name="why-use-core-text-apis"></a>Gründe für die Verwendung von Core-Text-APIs


Für zahlreiche Apps sind die XAML- oder HTML-Textfeld-Steuerelemente für Texteingabe und Textbearbeitung ausreichend. Wenn Ihre App jedoch komplexe Textszenarien behandelt, beispielsweise eine Textverarbeitungs-App ist, benötigen Sie vielleicht die Flexibilität eines benutzerdefinierten Textbearbeitungssteuerelements. Sie könnten die [**CoreWindow**](/uwp/api/Windows.UI.Core.CoreWindow)-Tastatur APIs verwenden, um das Textbearbeitungssteuerelement zu erstellen. Diese ermöglichen jedoch keinen Empfang von kompositionsbasierten Texteingaben, die für die Unterstützung ostasiatischer Sprachen erforderlich sind.

Verwenden Sie stattdessen die [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core)-APIs, wenn Sie ein benutzerdefiniertes Textbearbeitungssteuerelement erstellen müssen. Diese APIs sind so konzipiert, dass sie Ihnen ein hohes Maß an Flexibilität bei der Verarbeitung von Texteingaben in allen Sprachen bieten. Sie können daher Textverarbeitung so gestalten, wie dies für Ihre App am besten ist. Texteingabe- und Textbearbeitungssteuerelemente, die mit den Core-Text-APIs erstellt wurde, können Texteingaben von allen vorhandenen Texteingabemethoden auf Windows-Geräten empfangen, von [Textdienstframework](/windows/desktop/TSF/text-services-framework)-basierten Eingabemethoden-Editoren (IMEs) und Handschrift auf PCs bis hin zu der WordFlow-Tastatur (die AutoKorrektur, Vorhersage und Diktat bereitstellt) auf mobilen Geräten.

## <a name="architecture"></a>Aufbau


Im Folgenden finden Sie eine einfache Darstellung des Texteingabesystems.

-   "Application" stellt eine Windows-App dar, die ein benutzerdefiniertes Bearbeitungs Steuerelement mit den Kerntext-APIs erstellt.
-   Die [**Windows.UI.Text.Core**](/uwp/api/Windows.UI.Text.Core)-APIs ermöglichen die Kommunikation mit Textdiensten über Windows. Die Kommunikation zwischen dem Textbearbeitungssteuerelement und den Textdiensten erfolgt in erster Linie über ein [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext)-Objekt, das die Methoden und Ereignisse für die Kommunikation bereitstellt.

![CoreText-Architektur Diagramm](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>Textbereiche und Auswahl


Bearbeitungssteuerelemente bieten Platz für die Eingabe von Text, und Benutzer erwarten, dass Text an einer beliebigen Stelle in diesem Bereich bearbeitet werden kann. Hier wird das von den Core-Text-APIs verwendete Textpositionierungssystem erläutert und wie Bereiche und Auswahlen in diesem System dargestellt werden.

### <a name="application-caret-position"></a>Textcursorposition der Anwendung

Textbereiche, die mit den Core-Text-APIs verwendet werden, werden mittels Textcursorpositionen ausgedrückt. Eine „Textcursorposition der Anwendung“ (Application Caret Position, ACP) ist eine nullbasierte Zahl, die die Anzahl der Zeichen vom Anfang des Textstreams bis unmittelbar vor dem Textcursor angibt, wie hier gezeigt.

![Screenshot mit der Anzahl der Zeichen für die Position der Anwendung (ACP)](images/coretext/stream-1.png)

### <a name="text-ranges-and-selection"></a>Textbereiche und Auswahl

Textbereiche und -auswahlen werden anhand der [**CoreTextRange**](/uwp/api/Windows.UI.Text.Core.CoreTextRange)-Struktur dargestellt, die zwei Felder enthält:

| Feld                  | Datentyp                                                                 | Beschreibung                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Zahl** \[ Ja\] | **System. Int32** \[ .net\] | **Int32** \[ C++\] | Die Startposition eines Bereichs ist die Textcursorposition der Anwendung unmittelbar vor dem ersten Zeichen. |
| **EndCaretPosition**   | **Zahl** \[ Ja\] | **System. Int32** \[ .net\] | **Int32** \[ C++\] | Die Endposition eines Bereichs ist Textcursorposition der Anwendung unmittelbar nach dem letzten Zeichen.     |

 

Im oben gezeigten Textbereich gibt der Bereich \[ 0, 5 z \] . b. das Wort "Hello" an. **StartCaretPosition** muss stets kleiner oder gleich der **EndCaretPosition** sein. Der Bereich \[ 5, 0 \] ist ungültig.

### <a name="insertion-point"></a>Einfügemarke

Die aktuelle Position der Einfügemarke, die häufig als Einfügemarke bezeichnet wird, wird dargestellt, indem die **startcaretposition** auf die **endcaretposition**festgelegt wird.

### <a name="noncontiguous-selection"></a>Nicht zusammenhängende Auswahl

Einige Bearbeitungssteuerelemente unterstützen nicht zusammenhängende Auswahlen. Microsoft Office-Apps unterstützen z. B. eine beliebige Mehrfachauswahl, und viele Quellcode-Editoren unterstützen die Spaltenauswahl. Die Kerntext-APIs unterstützen jedoch nicht zusammenhängende Auswahlmöglichkeiten. Bearbeitungssteuerelemente müssen nur eine einzige zusammenhängende Auswahl melden; meistens ist dies der aktive Unterbereich der nicht zusammenhängenden Auswahlen.

Die folgende Abbildung zeigt beispielsweise einen Textstream mit zwei nicht zusammenhängenden Auswahlen: \[ 0, 1 \] und \[ 6, 11 \] für die das Bearbeitungs Steuerelement nur eine Meldung (entweder \[ 0, 1 \] oder \[ 6, 11) melden muss \] .

![Screenshot, der eine nicht zusammenhängende Textauswahl zeigt, bei der das erste und das letzte fünf Zeichen ausgewählt sind.](images/coretext/stream-2.png)

## <a name="working-with-text"></a>Arbeiten mit Text


Die [**CoreTextEditContext**](/uwp/api/Windows.UI.Text.Core.CoreTextEditContext)-Klasse ermöglicht einen Textfluss zwischen Windows und Bearbeitungssteuerelementen über das [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating)-Ereignis, das [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested)-Ereignis und die [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged)-Methode.

Das Bearbeitungssteuerelement empfängt Text über die [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating)-Ereignisse, die generiert werden, wenn Benutzer mit Texteingabemethoden wie Tastaturen, Sprache oder IMEs interagieren.

Wenn Sie den Text im Bearbeitungs Steuerelement ändern, indem Sie z. b. Text in das Steuerelement einfügen, müssen Sie Windows Benachrichtigen, indem Sie [**notifytextchanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged)aufrufen.

Wenn der Textdienst den neuen Text erfordert, wird ein [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested)-Ereignis ausgelöst. Sie müssen den neuen Text in den **TextRequested**-Ereignishandler eingeben.

### <a name="accepting-text-updates"></a>Akzeptieren von Textupdates

Ihr Bearbeitungssteuerelement sollte in der Regel Textaktualisierungsanforderungen akzeptieren, da diese den Text darstellen, den der Benutzer eingeben möchte. Im [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating)-Ereignishandler werden die folgenden Aktionen vom Bearbeitungssteuerelement erwartet:

1.  Einfügen des Texts, der in [**CoreTextTextUpdatingEventArgs.Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) an der Position angegeben wurde, die in [**CoreTextTextUpdatingEventArgs.Range**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range) angegeben wurde
2.  Platzieren Sie die Auswahl an der in [**coretexttextupdatingeventargs. newselection**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection)angegebenen Position.
3.  Benachrichtigen des Systems, dass das Update erfolgreich war, indem [**CoreTextTextUpdatingEventArgs.Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) auf [**CoreTextTextUpdatingResult.Succeeded**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult) festgelegt wird

Dies ist beispielsweise der Zustand eines Bearbeitungssteuerelements, bevor der Benutzer „d“ eingibt. Die Einfügemarke liegt bei \[ 10, 10 \] .

![Screenshot eines textstreamdiagramms mit der Einfügemarke bei \[ 10, 10 \] , vor einer Einfügung](images/coretext/stream-3.png)

Wenn der Benutzer „d“ eingibt, wird ein [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating)-Ereignis mit den folgenden [**CoreTextTextUpdatingEventArgs**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingEventArgs)-Daten ausgelöst:

-   [**Range**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.range)  =  Bereich \[ 10, 10\]
-   [**Text**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.text) = "d"
-   [**Neuauswahl**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.newselection)  =  \[ 11, 11\]

Wenden Sie in Ihrem Bearbeitungssteuerelement die angegebenen Änderungen an, und legen Sie [**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) auf **Succeeded** fest. Hier sehen Sie den Zustand des Steuerelements, nachdem die Änderungen angewendet wurden.

:::image type="content" source="images/coretext/stream-4.png" alt-text="Screenshot eines textstreamdiagramms, das die Einfügemarke bei \[ 11, 11 \] nach einer Einfügung anzeigt":::

### <a name="rejecting-text-updates"></a>Ablehnen von Textupdates

Manchmal können Textaktualisierungen nicht angewendet werden, da sich der angeforderte Bereich in einem Bereich des Bearbeitungssteuerelements befindet, der nicht geändert werden darf. In diesem Fall sollten Sie keine Änderungen anwenden. Benachrichtigen Sie stattdessen das System, dass die Aktualisierung fehlgeschlagen ist, indem Sie [**CoreTextTextUpdatingEventArgs.Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) auf [**CoreTextTextUpdatingResult.Failed**](/uwp/api/Windows.UI.Text.Core.CoreTextTextUpdatingResult) festlegen.

Angenommen, Sie haben ein Bearbeitungssteuerelement, das nur eine E-Mail-Adresse akzeptiert. Leerzeichen sollten zurückgewiesen werden, da E-Mail-Adressen keine Leerzeichen enthalten dürfen. Wenn daher [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating)-Ereignisse für die Leertaste ausgelöst werden, können Sie [**Result**](/uwp/api/windows.ui.text.core.coretexttextupdatingeventargs.result) im Bearbeitungssteuerelement einfach auf **Failed** festlegen.

### <a name="notifying-text-changes"></a>Benachrichtigen über Textänderungen

Manchmal nimmt das Bearbeitungssteuerelement Änderungen am Text vor, wenn beispielsweise Text eingefügt oder automatische korrigiert wird. In diesen Fällen müssen Sie die Textdienste über diese Änderungen benachrichtigen, indem Sie die [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged)-Methode aufrufen.

Dies ist beispielsweise der Zustand eines Bearbeitungssteuerelements, bevor der Benutzer „World“ einfügt. Die Einfügemarke ist \[ 6, 6 \] .

![Screenshot eines textstreamdiagramms mit der Einfügemarke bei \[ 6, 6 \] , vor einer Einfügung](images/coretext/stream-5.png)

Der Benutzer führt die Einfüge Aktion und das Bearbeitungs Steuerelement aus, nachdem die Änderungen angewendet wurden:

:::image type="content" source="images/coretext/stream-4.png" alt-text="Screenshot eines textstreamdiagramms, das die Einfügemarke bei \[ 11, 11 \] nach einer Einfügung anzeigt":::

In diesem Fall rufen Sie [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) mit den folgenden Argumenten auf:

-   *modifiedrange*  =  \[ 6, 6\]
-   *newLength* = 5
-   *Neuauswahl*  =  \[ 11, 11\]

Es folgt mindestens ein [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested)-Ereignis, das Sie zum Aktualisieren des Texts behandeln, mit dem die Textdienste arbeiten.

### <a name="overriding-text-updates"></a>Überschreiben von Textaktualisierungen

Vielleicht möchten Sie in Ihrem Bearbeitungssteuerelement eine Textaktualisierung überschreiben, um AutoKorrektur-Funktionen bereitzustellen.

Angenommen, Sie haben ein Bearbeitungssteuerelement, das eine Korrekturfunktion bereitstellt, das kontrahierte Schreibweisen formalisiert. Dies ist der Zustand des Bearbeitungssteuerelements, bevor der Benutzer die Leertaste drückt, um die Korrektur auszulösen. Die Einfügemarke ist \[ 3, 3 \] .

![Screenshot eines textstreamdiagramms mit der Einfügemarke bei \[ 3, 3 \] , vor einer Einfügung](images/coretext/stream-6.png)

Der Benutzer drückt die Leertaste, und es wird ein entsprechendes [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating)-Ereignis ausgelöst. Das Bearbeitungssteuerelement akzeptiert die Textaktualisierung. Dies ist der Zustand des Bearbeitungssteuerelements für einen kurzen Moment, bevor die Korrektur abgeschlossen ist. Die Einfügemarke ist \[ 4, 4 \] .

![Screenshot eines textstreamdiagramms, das die Einfügemarke bei \[ 4, 4 \] nach einer Einfügung anzeigt](images/coretext/stream-7.png)

Außerhalb des [**TextUpdating**](/uwp/api/windows.ui.text.core.coretexteditcontext.textupdating)-Ereignishandlers nimmt das Bearbeitungssteuerelement die folgende Korrektur vor. Dies ist der Zustand des Bearbeitungssteuerelements nach Abschluss der Korrektur. Die Einfügemarke ist \[ 5, 5 \] .

![Screenshot eines textstreamdiagramms mit der Einfügemarke bei \[ 5, 5\]](images/coretext/stream-8.png)

In diesem Fall rufen Sie [**NotifyTextChanged**](/uwp/api/windows.ui.text.core.coretexteditcontext.notifytextchanged) mit den folgenden Argumenten auf:

-   *modifiedrange*  =  \[ 1, 2\]
-   *newLength* = 2
-   *Neuauswahl*  =  \[ 5, 5\]

Es folgt mindestens ein [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested)-Ereignis, das Sie zum Aktualisieren des Texts behandeln, mit dem die Textdienste arbeiten.

### <a name="providing-requested-text"></a>Bereitstellen von angefordertem Text

Es ist wichtig, dass Textdienste über den richtigen Text verfügen, damit Funktionen wie AutoKorrektur oder Vorhersage bereitgestellt werden können, insbesondere bei Text, der im Bearbeitungssteuerelement bereits durch Laden eines Dokuments vorhanden war, oder bei Text, der vom Bearbeitungssteuerelement eingefügt wird, wie in den vorherigen Abschnitten erläutert. Wenn ein [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested)-Ereignis ausgelöst wird, müssen Sie daher stets den Text bereitstellen, der sich zurzeit im Bearbeitungssteuerelement für den angegebenen Bereich befindet.

Gelegentlich gibt [**Range**](/uwp/api/windows.ui.text.core.coretexttextrequest.range) in [**CoreTextTextRequest**](/uwp/api/Windows.UI.Text.Core.CoreTextTextRequest) einen Bereich an, den das Bearbeitungssteuerelement nicht in der Form aufnehmen kann, in der dieser vorliegt. Beispielsweise ist **Range** zum Zeitpunkt des [**TextRequested**](/uwp/api/windows.ui.text.core.coretexteditcontext.textrequested)-Ereignisses größer als das Bearbeitungssteuerelement, oder das Ende von **Range** liegt außerhalb des zulässigen Bereichs. In diesen Fällen sollten Sie zu einem Bereich zurückkehren, der sinnvoll ist. In der Regel ist dies eine Untergruppe des angeforderten Bereichs.

## <a name="related-articles"></a>Verwandte Artikel

### <a name="samples"></a>Beispiele

- [Beispiel für ein benutzerdefiniertes Bearbeitungssteuerelement](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/CustomEditControl)

### <a name="archive-samples"></a>Archivbeispiele

- [Beispiel für die XAML-Textbearbeitung](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BVB%5D-Windows%208%20app%20samples/VB/Windows%208%20app%20samples/XAML%20text%20editing%20sample%20(Windows%208))
