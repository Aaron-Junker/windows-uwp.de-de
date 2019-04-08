---
Description: Mit den Core-Text-APIs im Windows.UI.Text.Core-Namespace kann eine UWP-App (Universelle Windows-Plattform) Texteingaben von beliebigen Textdiensten empfangen, die auf Windows-Geräten unterstützt werden.
title: Übersicht über benutzerdefinierte Texteingabe
ms.assetid: 58F5F7AC-6A4B-45FC-8C2A-942730FD7B74
label: Custom text input
template: detail.hbs
keywords: Tastatur, Text, Core-Text, benutzerdefinierter Text, Textdienstframework, Eingabe, Benutzerinteraktionen
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: dfb2a5203d2a8e5c497fa427c6a2a7ed5fe2302d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57638705"
---
# <a name="custom-text-input"></a>Benutzerdefinierte Texteingabe



Mit den Core-Text-APIs im [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)-Namespace kann eine UWP-App (Universelle Windows-Plattform) Texteingaben von beliebigen Textdiensten empfangen, die auf Windows-Geräten unterstützt werden. Die APIs sind den [Textdienstframework](https://msdn.microsoft.com/library/windows/desktop/ms629032)-APIs dahingehend ähnlich, dass die App keine detaillierten Kenntnisse über die Textdienste benötigt. Auf diese Weise kann die App Text in einer beliebigen Sprache und von einem beliebigen Eingabegerät empfangen, wie Tastatur, Sprache oder Stift.

> **Wichtige APIs:** [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238), [ **CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)

## <a name="why-use-core-text-apis"></a>Gründe für die Verwendung von Core-Text-APIs


Für zahlreiche Apps sind die XAML- oder HTML-Textfeld-Steuerelemente für Texteingabe und Textbearbeitung ausreichend. Wenn Ihre App jedoch komplexe Textszenarien behandelt, beispielsweise eine Textverarbeitungs-App ist, benötigen Sie vielleicht die Flexibilität eines benutzerdefinierten Textbearbeitungssteuerelements. Sie könnten die [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)-Tastatur APIs verwenden, um das Textbearbeitungssteuerelement zu erstellen. Diese ermöglichen jedoch keinen Empfang von kompositionsbasierten Texteingaben, die für die Unterstützung ostasiatischer Sprachen erforderlich sind.

Verwenden Sie stattdessen die [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)-APIs, wenn Sie ein benutzerdefiniertes Textbearbeitungssteuerelement erstellen müssen. Diese APIs sind so konzipiert, dass sie Ihnen ein hohes Maß an Flexibilität bei der Verarbeitung von Texteingaben in allen Sprachen bieten. Sie können daher Textverarbeitung so gestalten, wie dies für Ihre App am besten ist. Texteingabe- und Textbearbeitungssteuerelemente, die mit den Core-Text-APIs erstellt wurde, können Texteingaben von allen vorhandenen Texteingabemethoden auf Windows-Geräten empfangen, von [Textdienstframework](https://msdn.microsoft.com/library/windows/desktop/ms629032)-basierten Eingabemethoden-Editoren (IMEs) und Handschrift auf PCs bis hin zu der WordFlow-Tastatur (die AutoKorrektur, Vorhersage und Diktat bereitstellt) auf mobilen Geräten.

## <a name="architecture"></a>Architecture


Im Folgenden finden Sie eine einfache Darstellung des Texteingabesystems.

-   „Anwendung“ ist eine UWP-App, die ein benutzerdefiniertes Bearbeitungssteuerelement mit den Core-Text-APIs hostet.
-   Die [**Windows.UI.Text.Core**](https://msdn.microsoft.com/library/windows/apps/dn958238)-APIs ermöglichen die Kommunikation mit Textdiensten über Windows. Die Kommunikation zwischen dem Textbearbeitungssteuerelement und den Textdiensten erfolgt in erster Linie über ein [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)-Objekt, das die Methoden und Ereignisse für die Kommunikation bereitstellt.

![Diagramm für die Kerntextarchitektur](images/coretext/architecture.png)

## <a name="text-ranges-and-selection"></a>Textbereiche und Auswahl


Bearbeitungssteuerelemente bieten Platz für die Eingabe von Text, und Benutzer erwarten, dass Text an einer beliebigen Stelle in diesem Bereich bearbeitet werden kann. Hier wird das von den Core-Text-APIs verwendete Textpositionierungssystem erläutert und wie Bereiche und Auswahlen in diesem System dargestellt werden.

### <a name="application-caret-position"></a>Textcursorposition der Anwendung

Textbereiche, die mit den Core-Text-APIs verwendet werden, werden mittels Textcursorpositionen ausgedrückt. Eine „Textcursorposition der Anwendung“ (Application Caret Position, ACP) ist eine nullbasierte Zahl, die die Anzahl der Zeichen vom Anfang des Textstreams bis unmittelbar vor dem Textcursor angibt, wie hier gezeigt.

![Beispieldiagramm für einen Textstream](images/coretext/stream-1.png)
### <a name="text-ranges-and-selection"></a>Textbereiche und Auswahl

Textbereiche und -auswahlen werden anhand der [**CoreTextRange**](https://msdn.microsoft.com/library/windows/apps/dn958201)-Struktur dargestellt, die zwei Felder enthält:

| Feld                  | Datentyp                                                                 | Beschreibung                                                                      |
|------------------------|---------------------------------------------------------------------------|----------------------------------------------------------------------------------|
| **StartCaretPosition** | **Anzahl** \[JavaScript\] | **System. Int32** \[.NET\] | **Int32** \[C++\] | Die Startposition eines Bereichs ist die Textcursorposition der Anwendung unmittelbar vor dem ersten Zeichen. |
| **EndCaretPosition**   | **Anzahl** \[JavaScript\] | **System. Int32** \[.NET\] | **Int32** \[C++\] | Die Endposition eines Bereichs ist Textcursorposition der Anwendung unmittelbar nach dem letzten Zeichen.     |

 

Z. B. in den Textbereich des Bereichs von zuvor gezeigte \[0, 5\] gibt an, das Wort "Hello". **StartCaretPosition** muss stets kleiner oder gleich der **EndCaretPosition** sein. Der Bereich \[5, 0\] ist ungültig.

### <a name="insertion-point"></a>Einfügemarke

Die aktuelle Textcursorposition, häufig als Einfügemarke bezeichnet, wird durch dargestellt, indem **StartCaretPosition** als gleich **EndCaretPosition** festgelegt wird.

### <a name="noncontiguous-selection"></a>Nicht zusammenhängende Auswahl

Einige Bearbeitungssteuerelemente unterstützen nicht zusammenhängende Auswahlen. Microsoft Office-Apps unterstützen z. B. eine beliebige Mehrfachauswahl, und viele Quellcode-Editoren unterstützen die Spaltenauswahl. Von Core-Text-APIs wird jedoch eine nicht zusammenhängende Auswahl nicht unterstützt. Bearbeitungssteuerelemente müssen nur eine einzige zusammenhängende Auswahl melden; meistens ist dies der aktive Unterbereich der nicht zusammenhängenden Auswahlen.

Betrachten Sie beispielsweise den folgenden Textstream:

![Beispieldiagramm für Text-Stream](images/coretext/stream-2.png) stehen zwei Optionen: \[0, 1\] und \[6, 11\]. Das Bearbeitungssteuerelement muss nur eine davon Bericht. entweder \[0, 1\] oder \[6, 11\].

## <a name="working-with-text"></a>Arbeiten mit Text


Die [**CoreTextEditContext**](https://msdn.microsoft.com/library/windows/apps/dn958158)-Klasse ermöglicht einen Textfluss zwischen Windows und Bearbeitungssteuerelementen über das [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignis, das [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis und die [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)-Methode.

Das Bearbeitungssteuerelement empfängt Text über die [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignisse, die generiert werden, wenn Benutzer mit Texteingabemethoden wie Tastaturen, Sprache oder IMEs interagieren.

Wenn Sie Text im Bearbeitungssteuerelement ändern, beispielsweise durch Einfügen von Text in das Steuerelement, müssen Sie Windows benachrichtigen, indem Sie [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172) aufrufen.

Wenn der Textdienst den neuen Text erfordert, wird ein [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis ausgelöst. Sie müssen den neuen Text in den **TextRequested**-Ereignishandler eingeben.

### <a name="accepting-text-updates"></a>Akzeptieren von Textupdates

Ihr Bearbeitungssteuerelement sollte in der Regel Textaktualisierungsanforderungen akzeptieren, da diese den Text darstellen, den der Benutzer eingeben möchte. Im [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignishandler werden die folgenden Aktionen vom Bearbeitungssteuerelement erwartet:

1.  Einfügen des Texts, der in [**CoreTextTextUpdatingEventArgs.Text**](https://msdn.microsoft.com/library/windows/apps/dn958236) an der Position angegeben wurde, die in [**CoreTextTextUpdatingEventArgs.Range**](https://msdn.microsoft.com/library/windows/apps/dn958234) angegeben wurde
2.  Platzieren der Auswahl an der in [**CoreTextTextUpdatingEventArgs.NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) angegebenen Position
3.  Benachrichtigen des Systems, dass das Update erfolgreich war, indem [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) auf [**CoreTextTextUpdatingResult.Succeeded**](https://msdn.microsoft.com/library/windows/apps/dn958237) festgelegt wird

Dies ist beispielsweise der Zustand eines Bearbeitungssteuerelements, bevor der Benutzer „d“ eingibt. Die Einfügemarke befindet sich am \[10, 10\].

![Beispieldiagramm für Text-Stream](images/coretext/stream-3.png) , wenn der Benutzer "d" gibt einen [ **TextUpdating** ](https://msdn.microsoft.com/library/windows/apps/dn958176) Ereignis wird ausgelöst, durch den folgenden [ **CoreTextTextUpdatingEventArgs** ](https://msdn.microsoft.com/library/windows/apps/dn958229) Daten:

-   [**Bereich**](https://msdn.microsoft.com/library/windows/apps/dn958234) = \[10, 10\]
-   [**Text** ](https://msdn.microsoft.com/library/windows/apps/dn958236) = "d"
-   [**NewSelection**](https://msdn.microsoft.com/library/windows/apps/dn958233) = \[11, 11\]

Wenden Sie in Ihrem Bearbeitungssteuerelement die angegebenen Änderungen an, und legen Sie [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) auf **Succeeded** fest. Hier sehen Sie den Zustand des Steuerelements, nachdem die Änderungen angewendet wurden.

![Beispieldiagramm für einen Textstream](images/coretext/stream-4.png)
### <a name="rejecting-text-updates"></a>Ablehnen von Textupdates

Manchmal können Textaktualisierungen nicht angewendet werden, da sich der angeforderte Bereich in einem Bereich des Bearbeitungssteuerelements befindet, der nicht geändert werden darf. In diesem Fall sollten Sie keine Änderungen anwenden. Benachrichtigen Sie stattdessen das System, dass die Aktualisierung fehlgeschlagen ist, indem Sie [**CoreTextTextUpdatingEventArgs.Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) auf [**CoreTextTextUpdatingResult.Failed**](https://msdn.microsoft.com/library/windows/apps/dn958237) festlegen.

Angenommen, Sie haben ein Bearbeitungssteuerelement, das nur eine E-Mail-Adresse akzeptiert. Leerzeichen sollten zurückgewiesen werden, da E-Mail-Adressen keine Leerzeichen enthalten dürfen. Wenn daher [**TextUpdating**](https://msdn.microsoft.com/library/windows/apps/dn958176)-Ereignisse für die Leertaste ausgelöst werden, können Sie [**Result**](https://msdn.microsoft.com/library/windows/apps/dn958235) im Bearbeitungssteuerelement einfach auf **Failed** festlegen.

### <a name="notifying-text-changes"></a>Benachrichtigen über Textänderungen

Manchmal nimmt das Bearbeitungssteuerelement Änderungen am Text vor, wenn beispielsweise Text eingefügt oder automatische korrigiert wird. In diesen Fällen müssen Sie die Textdienste über diese Änderungen benachrichtigen, indem Sie die [**NotifyTextChanged**](https://msdn.microsoft.com/library/windows/apps/dn958172)-Methode aufrufen.

Dies ist beispielsweise der Zustand eines Bearbeitungssteuerelements, bevor der Benutzer „World“ einfügt. Die Einfügemarke befindet sich am \[6, 6\].

![Beispieldiagramm für Text-Stream](images/coretext/stream-5.png) der Benutzer führt die Einfügeaktion und das Bearbeitungssteuerelement letztendlich mit dem folgenden Text:

![Beispieldiagramm für Text-Stream](images/coretext/stream-4.png) in diesem Fall sollten Sie aufrufen [ **NotifyTextChanged** ](https://msdn.microsoft.com/library/windows/apps/dn958172) mit diesen Argumenten:

-   *ModifiedRange* = \[6, 6\]
-   *newLength* = 5
-   *NewSelection* = \[11, 11\]

Es folgt mindestens ein [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis, das Sie zum Aktualisieren des Texts behandeln, mit dem die Textdienste arbeiten.

### <a name="overriding-text-updates"></a>Überschreiben von Textaktualisierungen

Vielleicht möchten Sie in Ihrem Bearbeitungssteuerelement eine Textaktualisierung überschreiben, um AutoKorrektur-Funktionen bereitzustellen.

Angenommen, Sie haben ein Bearbeitungssteuerelement, das eine Korrekturfunktion bereitstellt, das kontrahierte Schreibweisen formalisiert. Dies ist der Zustand des Bearbeitungssteuerelements, bevor der Benutzer die Leertaste drückt, um die Korrektur auszulösen. Die Einfügemarke befindet sich am \[3, 3\].

![Beispieldiagramm für Text-Stream](images/coretext/stream-6.png) der Benutzer drückt die LEERTASTE und einen entsprechenden [ **TextUpdating** ](https://msdn.microsoft.com/library/windows/apps/dn958176) Ereignis wird ausgelöst. Das Bearbeitungssteuerelement akzeptiert die Textaktualisierung. Dies ist der Zustand des Bearbeitungssteuerelements für einen kurzen Moment, bevor die Korrektur abgeschlossen ist. Die Einfügemarke befindet sich am \[4, 4\].

![Beispieldiagramm für Text-Stream](images/coretext/stream-7.png) außerhalb von den [ **TextUpdating** ](https://msdn.microsoft.com/library/windows/apps/dn958176) -Ereignishandler der Edit-Steuerelement stellt die folgende Korrektur. Dies ist der Zustand des Bearbeitungssteuerelements nach Abschluss der Korrektur. Die Einfügemarke befindet sich am \[5, 5\].

![Beispieldiagramm für Text-Stream](images/coretext/stream-8.png) in diesem Fall sollten Sie aufrufen [ **NotifyTextChanged** ](https://msdn.microsoft.com/library/windows/apps/dn958172) mit diesen Argumenten:

-   *ModifiedRange* = \[1, 2\]
-   *newLength* = 2
-   *NewSelection* = \[5, 5\]

Es folgt mindestens ein [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis, das Sie zum Aktualisieren des Texts behandeln, mit dem die Textdienste arbeiten.

### <a name="providing-requested-text"></a>Bereitstellen von angefordertem Text

Es ist wichtig, dass Textdienste über den richtigen Text verfügen, damit Funktionen wie AutoKorrektur oder Vorhersage bereitgestellt werden können, insbesondere bei Text, der im Bearbeitungssteuerelement bereits durch Laden eines Dokuments vorhanden war, oder bei Text, der vom Bearbeitungssteuerelement eingefügt wird, wie in den vorherigen Abschnitten erläutert. Wenn ein [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignis ausgelöst wird, müssen Sie daher stets den Text bereitstellen, der sich zurzeit im Bearbeitungssteuerelement für den angegebenen Bereich befindet.

Gelegentlich gibt [**Range**](https://msdn.microsoft.com/library/windows/apps/dn958227) in [**CoreTextTextRequest**](https://msdn.microsoft.com/library/windows/apps/dn958221) einen Bereich an, den das Bearbeitungssteuerelement nicht in der Form aufnehmen kann, in der dieser vorliegt. Beispielsweise ist **Range** zum Zeitpunkt des [**TextRequested**](https://msdn.microsoft.com/library/windows/apps/dn958175)-Ereignisses größer als das Bearbeitungssteuerelement, oder das Ende von **Range** liegt außerhalb des zulässigen Bereichs. In diesen Fällen sollten Sie zu einem Bereich zurückkehren, der sinnvoll ist. In der Regel ist dies eine Untergruppe des angeforderten Bereichs.

## <a name="related-articles"></a>Verwandte Artikel

**Beispiele**
* [Beispiel für die benutzerdefinierte Steuerelement bearbeiten](https://go.microsoft.com/fwlink/?linkid=831024)  
 **Beispiele archivieren**
* [Bearbeiten der XAML-Textbeispiel](https://go.microsoft.com/fwlink/p/?LinkID=251417)


