---
Description: Ein Statussteuerelement gibt dem Benutzer eine Rückmeldung, dass ein Vorgang mit langer Laufzeit ausgeführt wird.
title: Richtlinien für Statussteuerelemente
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: a409c4b940ad0e194428981f536823d880e56302
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364488"
---
# <a name="progress-controls"></a>Statussteuerelemente

 

Ein Statussteuerelement gibt dem Benutzer eine Rückmeldung, dass ein Vorgang mit langer Laufzeit ausgeführt wird. Dies kann bedeuten, dass der Benutzer bei Anzeigen der Statusanzeige nicht mit der App interagieren kann. Je nach verwendetem Indikator wird auch die Länge der Wartezeit angegeben.

> **Wichtige APIs:** [Klasse "ProgressBar"](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar), [IsIndeterminate Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.isindeterminate), [ProgressRing Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing), [IsActive-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.isactive)

## <a name="types-of-progress"></a>Typen von Statussteuerelementen

Es gibt zwei Steuerelemente, die dem Benutzer anzeigen, dass ein Vorgang ausgeführt wird: ein ProgressBar-Element oder ein ProgressRing-Element.

-   Der ProgressBar-Status *bestimmt* gibt den Prozentsatz an, zu dem eine Aufgabe abgeschlossen ist. Dieses Element für Vorgänge verwendet werden, deren Dauer bekannt ist, deren Fortschritt jedoch nicht die Interaktion des Benutzers mit der App blockieren sollte.
-   Der ProgressBar-Status *unbestimmt* gibt an, dass ein Vorgang ausgeführt wird, der die Interaktion des Benutzers mit der App nicht blockiert und dessen Abschlusszeit nicht bekannt ist.
-   Für das ProgressRing-Element gibt es nur den Status *unbestimmt*. Es sollte verwendet werden, wenn vor dem Abschluss eines Vorgangs keine Benutzerinteraktion möglich ist.

Ein Statussteuerelement ist zudem schreibgeschützt und nicht interaktiv. Dies bedeutet, dass der Benutzer diese Steuerelemente nicht direkt aufrufen oder verwenden kann.

![ProgressBar-Status](images/ProgressBar_TwoStates.png)

*Von oben nach unten - unbestimmte Fortschrittsleiste und eine bestimmte "ProgressBar"*

![ProgressRing-Status](images/ProgressRing_SingleState.png)

*Eine unbestimmte ProgressRing*

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ProgressBar">ProgressBar</a> oder <a href="xamlcontrolsgallery:/item/ProgressRing">ProgressRing</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="when-to-use-each-control"></a>Verwendung der einzelnen Steuerelemente

Es ist nicht immer klar erkennbar, welches Steuerelement oder welcher Status (bestimmt oder unbestimmt) zu verwenden ist, um einen Vorgang anzuzeigen. Manchmal ist eine Aufgabe so deutlich zu erkennen, dass kein Statussteuerelement erforderlich ist. Manchmal ist jedoch auch bei Verwendung eines Statussteuerelements eine Textzeile erforderlich, die den Benutzer darüber informiert, welcher Vorgang gerade ausgeführt wird.

### <a name="progressbar"></a>ProgressBar
-   **Besitzt das Steuerelement ein vorhersagbares End "oder" definierten Zeitraum?**

    Verwenden Sie in diesem Fall eine bestimmte Statusanzeige, und aktualisieren Sie deren Prozentsatz oder Wert entsprechend.

-   **Werden der Benutzer kann weiterhin, ohne dass der Status des Vorgangs überwachen?**

    Bei Verwendung eines ProgressBar-Elements ist die Interaktion nicht modal. Das bedeutet in der Regel, dass der Benutzer durch den Abschluss des Vorgangs nicht blockiert wird und die aktive App bis zum Abschluss der Aktion weiterhin verwenden kann.

-   **Keywords**

    Wenn der anzuzeigende Vorgang sich mit den folgenden Schlüsselwörtern in Verbindung bringen lässt oder wenn Sie während des Vorgangs Text anzeigen möchten, in dem diese Schlüsselwörter vorkommen, sollten Sie ein ProgressBar-Element verwenden:

    - *Wird geladen...*
    - *Abrufen von*
    - *Arbeiten...*

### <a name="progressring"></a>ProgressRing

-   **Bewirkt der Vorgang, den Benutzer warten, um den Vorgang fortzusetzen?**

    Wenn ein Vorgang bis zu seinem Abschluss eine umfassende Interaktion mit der App erfordert, empfiehlt sich die Verwendung eines ProgressRing-Elements. Das ProgressRing-Steuerelement ist für modale Interaktionen vorgesehen, in denen die Aktivitäten des Benutzers blockiert werden, bis das ProgressRing-Element nicht mehr angezeigt wird.

-   **Werden die app wird für den Benutzer zum Abschluss einer Aufgabe gewartet?**

    In diesem Fall verwenden Sie ein ProgressRing-Steuerelement, um den Benutzer auf eine unbestimmte Wartezeit hinzuweisen.

-   **Keywords**

    Wenn der anzuzeigende Vorgang sich mit den folgenden Schlüsselwörtern in Verbindung bringen lässt oder wenn Sie während des Vorgangs Text anzeigen möchten, in dem diese Schlüsselwörter vorkommen, sollten Sie ein ProgressRing-Element verwenden:

    - *Aktualisieren*
    - *Anmeldung wird durchgeführt...*
    - *Verbindung wird hergestellt...*

### <a name="no-progress-indication-necessary"></a>Keine Fortschrittsanzeige erforderlich
-   **Muss der Benutzer wissen, dass etwas passiert ist?**

    Wenn die App z. B. im Hintergrund einen Download ausführt, der nicht vom Benutzer eingeleitet wurde, ist es auch nicht unbedingt erforderlich, den Benutzer darüber zu informieren.

-   **Ist der Vorgang eine Hintergrundaktivitäten, die blockiert nicht die Benutzeraktivität und der minimalen (aber immer noch einige) für den Benutzer interessant ist?**

    Verwenden Sie Text, wenn die App Aufgaben ausführt, die zwar nicht immer sichtbar sein müssen, bei denen aber der Status angezeigt werden soll.

-   **Ist der Benutzer nur über den Abschluss des Vorgangs wichtig?**

    Manchmal ist es am besten, nur auf den Abschluss eines Vorgangs hinzuweisen oder den unmittelbaren Abschluss des Vorgangs durch ein visuelles Element anzukündigen, und den restlichen Vorgang im Hintergrund auszuführen.

## <a name="progress-controls-best-practices"></a>Bewährte Methoden für Statussteuerelemente

Manchmal ist eine visuelle Darstellung hilfreich, um zu ermitteln, zu welchem Zeitpunkt welches Statussteuerelement verwendet werden sollte:

**ProgressBar - bestimmte**

![Beispiel für ein bestimmtes ProgressBar-Element](images/PB_DeterminateExample.png)

Beim ersten Beispiel handelt es sich um ein bestimmtes ProgressBar-Steuerelement. Wenn die Dauer des Vorgangs bekannt ist, etwa beim Installieren, Herunterladen oder Einrichten, eignet sich ein bestimmtes ProgressBar-Steuerelement.

**ProgressBar - unbestimmt**

![Beispiel für ein unbestimmtes ProgressBar-Element](images/PB_IndeterminateExample.png)

Wenn die Dauer des Vorgangs nicht bekannt ist, verwenden Sie ein unbestimmtes ProgressBar-Steuerelement. Unbestimmte ProgressBar-Elemente können auch beim Ausfüllen virtualisierter Listen verwendet werden oder um einen glatten visuellen Übergang von einem unbestimmten zu einem bestimmten ProgressBar-Element zu erstellen.

-   **Ist der Vorgang in einer virtualisierten Collection an?**

    In diesem Fall legen Sie nicht für jedes angezeigte Listenelement eine Statusanzeige fest. Positionieren Sie stattdessen ein ProgressBar-Element am Anfang der Auflistung der zu ladenden Elemente, um anzuzeigen, dass die Elemente geladen werden.

**ProgressRing - unbestimmt**

![Beispiel für ein unbestimmtes ProgressRing-Steuerelement](images/PR_IndeterminateExample.png)

Unbestimmte ProgressRing-Elemente werden verwendet, wenn jegliche Benutzerinteraktion mit der App ausgesetzt ist oder die App auf eine Benutzereingabe wartet, um den Vorgang fortzusetzen. Das „Anmelden...“- Beispiel oben ist ein optimales Szenario für das ProgressRing-Steuerelement, da der Benutzer die App erst weiterverwenden kann, nachdem der Anmeldevorgang abgeschlossen ist.

## <a name="customizing-a-progress-control"></a>Anpassen eines Statussteuerelements

Die beiden Statussteuerelemente sind recht einfach gehalten. Bei verschiedenen visuellen Features der Steuerelemente sind jedoch deren Anpassungsoptionen nicht offensichtlich.

**Ändern der Größe der ProgressRing**

Sie können das ProgressRing-Element so groß gestalten, wie Sie möchten. Die Mindestgröße beträgt jedoch 20 x 20 Epx. Um die Größe eines ProgressRing-Elements zu ändern, müssen Sie dessen Höhe und Breite festlegen. Wenn nur die Höhe oder nur die Breite festgelegt wird, erhält das Steuerelement die Mindestgröße (20 x 20 Epx). Wenn die Höhe und die Breite auf zwei verschiedene Größen festgelegt sind, wird die kleinere Größen verwendet.
Um sicherzustellen, dass Ihr ProgressRing-Steuerelement seinen Zweck erfüllt, legen Sie für die Höhe und die Breite denselben Wert fest:

```XAML
<ProgressRing Height="100" Width="100"/>
```

Wenn das ProgressRing-Element sichtbar und animiert sein soll, legen Sie die IsActive-Eigenschaft auf „true“ fest:

```XAML
<ProgressRing IsActive="True" Height="100" Width="100"/>
```

```C#
progressRing.IsActive = true;
```

**Einfärben der Statussteuerelemente**

Standardmäßig ist die Grundfarbe der Statussteuerelemente auf die Akzentfarbe des Systems festgelegt. Dies können Sie anpassen, indem Sie einfach die „Foreground“-Eigenschaft der Steuerelemente ändern.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<ProgressBar Width="100" Foreground="Green"/>
```

Durch das Ändern die Vordergrundfarbe des ProgressRing-Elements ändern sich die Farben der Punkte. Die „Foreground“-Eigenschaft des ProgressBar-Elements ändert dann die Füllfarbe der Leiste. Um den ungefüllten Teil der Leiste zu ändern, überschreiben Sie einfach die „Background“-Eigenschaft.

**Einen Wartecursor angezeigt**

Manchmal ist es am besten, nur kurz einen Wartecursor anzuzeigen, wenn eine App oder ein Vorgang etwas Zeit erfordert und Sie dem Benutzer anzeigen müssen, dass mit der App oder dem Bereich, in dem sich der Wartecursor befindet, bis zu dessen Ausblenden nicht interagiert werden sollte.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [Klasse "ProgressBar"](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)
- [ProgressRing-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)

**Für Entwickler (XAML)**
- [Hinzufügen von Statussteuerelemente](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10))
- [So erstellen Sie eine benutzerdefinierte unbestimmte Fortschrittsleiste für Windows Phone](https://go.microsoft.com/fwlink/p/?LinkID=392426)
