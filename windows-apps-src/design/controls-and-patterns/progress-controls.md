---
Description: Ein Statussteuerelement gibt dem Benutzer eine Rückmeldung, dass ein Vorgang mit langer Laufzeit ausgeführt wird.
title: Richtlinien für Statussteuerelemente
ms.assetid: FD53B716-C43D-408D-8B07-522BC1F3DF9D
label: Progress controls
template: detail.hbs
ms.date: 11/29/2019
ms.topic: article
keywords: Windows 10, UWP
pm-contact: kisai
design-contact: jeffarn
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 3e5ff5d0e9172432110d60a007228e59e48785b9
ms.sourcegitcommit: 27cb7c4539bb6417d32883824ccea160bb948c15
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/05/2019
ms.locfileid: "74830808"
---
# <a name="progress-controls"></a>Statussteuerelemente

Ein Statussteuerelement gibt dem Benutzer eine Rückmeldung, dass ein Vorgang mit langer Laufzeit ausgeführt wird. Dies kann bedeuten, dass der Benutzer bei Anzeigen der Statusanzeige nicht mit der App interagieren kann. Je nach verwendetem Indikator wird auch die Länge der Wartezeit angegeben.

| **Plattform-APIs** | **Windows-UI-Bibliotheks-APIs** |
| - | - |
| [ProgressBar-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar), [IsIndeterminate Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressbar.isindeterminate), [ProgressRing-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing), [IsActive-Eigenschaft](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.progressring.isactive) | [ProgressBar-Klasse](https://docs.microsoft.com/uwp/api/Microsoft.UI.Xaml.Controls.ProgressBar), [IsIndeterminate-Eigenschaft](https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.progressbar.isindeterminate) |

| **Abrufen der Windows-UI-Bibliothek** |
| - |
| Dieses Steuerelement ist als Bestandteil der Windows-UI-Bibliothek enthalten. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für UWP-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](https://docs.microsoft.com/uwp/toolkits/winui/) (Windows-UI-Bibliothek). |

> [!NOTE]
> Es gibt zwei Versionen des ProgressBar-Steuerelements: eine in der Plattform, dargestellt durch den Windows.UI.XAML-Namespace; die andere in der Bibliothek der Windows-Benutzeroberfläche, im Microsoft.UI.XAML-Namespace. Zwar ist die API für ProgressBar identisch, die Darstellung des Steuerelements unterscheidet sich jedoch zwischen diesen beiden Versionen. In diesem Dokument werden Abbildungen der neueren Version aus der Windows-Benutzeroberflächenbibliothek gezeigt.
In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt:

```xaml
xmlns:muxc="using:Microsoft.UI.Xaml.Controls"
```

Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using**-Anweisung hinzugefügt:

```csharp
using muxc = Microsoft.UI.Xaml.Controls;
```

```vb
Imports muxc = Microsoft.UI.Xaml.Controls
```

## <a name="types-of-progress"></a>Typen von Statussteuerelementen

Es gibt zwei Steuerelemente, die dem Benutzer anzeigen, dass ein Vorgang ausgeführt wird: ein ProgressBar-Element oder ein ProgressRing-Element.

-   Der ProgressBar-Status *bestimmt* gibt den Prozentsatz an, zu dem eine Aufgabe abgeschlossen ist. Dieses Element sollte für Vorgänge verwendet werden, deren Dauer bekannt ist, deren Fortschritt jedoch nicht die Interaktion des Benutzers mit der App blockieren sollte.
-   Der ProgressBar-Status *unbestimmt* gibt an, dass ein Vorgang ausgeführt wird, der die Interaktion des Benutzers mit der App nicht blockiert und dessen Abschlusszeit nicht bekannt ist.
-   Für das ProgressRing-Element gibt es nur den Status *unbestimmt*. Es sollte verwendet werden, wenn vor dem Abschluss eines Vorgangs keine Benutzerinteraktion möglich ist.

Ein Statussteuerelement ist zudem schreibgeschützt und nicht interaktiv. Dies bedeutet, dass der Benutzer diese Steuerelemente nicht direkt aufrufen oder verwenden kann.

![ProgressBar-Status](images/progress-bar-two-states.png)

*Von oben nach unten – unbestimmtes ProgressBar-Element und bestimmtes ProgressBar-Element*

![ProgressRing-Status](images/ProgressRing_SingleState.png)

*Ein unbestimmtes ProgressRing-Element*

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
-   **Verfügt das Steuerelement über eine festgelegte Dauer oder ein vorhersehbares Ende?**

    Verwenden Sie in diesem Fall eine bestimmte Statusanzeige, und aktualisieren Sie deren Prozentsatz oder Wert entsprechend.

-   **Kann der Benutzer fortfahren, ohne den Status des Vorgang zu überwachen?**

    Bei Verwendung eines ProgressBar-Elements ist die Interaktion nicht modal. Das bedeutet in der Regel, dass der Benutzer durch den Abschluss des Vorgangs nicht blockiert wird und die aktive App bis zum Abschluss der Aktion weiterhin verwenden kann.

-   **Schlüsselwörter**

    Wenn der anzuzeigende Vorgang sich mit den folgenden Schlüsselwörtern in Verbindung bringen lässt oder wenn Sie während des Vorgangs Text anzeigen möchten, in dem diese Schlüsselwörter vorkommen, sollten Sie ein ProgressBar-Element verwenden:

    - *Wird geladen...*
    - *Wird abgerufen...*
    - *In Bearbeitung...*

### <a name="progressring"></a>ProgressRing

-   **Muss der Benutzer den Abschluss des Vorgangs abwarten, bevor er seine Aktivität fortsetzen kann?**

    Wenn ein Vorgang bis zu seinem Abschluss eine umfassende Interaktion mit der App erfordert, empfiehlt sich die Verwendung eines ProgressRing-Elements. Das ProgressRing-Steuerelement ist für modale Interaktionen vorgesehen, in denen die Aktivitäten des Benutzers blockiert werden, bis das ProgressRing-Element nicht mehr angezeigt wird.

-   **Wartet die App darauf, dass der Benutzer eine Aufgabe ausführt?**

    In diesem Fall verwenden Sie ein ProgressRing-Steuerelement, um den Benutzer auf eine unbestimmte Wartezeit hinzuweisen.

-   **Schlüsselwörter**

    Wenn der anzuzeigende Vorgang sich mit den folgenden Schlüsselwörtern in Verbindung bringen lässt oder wenn Sie während des Vorgangs Text anzeigen möchten, in dem diese Schlüsselwörter vorkommen, sollten Sie ein ProgressRing-Element verwenden:

    - *Wird aktualisiert*
    - *Anmelden...*
    - *Verbinden...*

### <a name="no-progress-indication-necessary"></a>Keine Fortschrittsanzeige erforderlich
-   **Müssen Benutzer wissen, dass Vorgänge ausgeführt werden?**

    Wenn die App z. B. im Hintergrund einen Download ausführt, der nicht vom Benutzer eingeleitet wurde, ist es auch nicht unbedingt erforderlich, den Benutzer darüber zu informieren.

-   **Wird der Vorgang im Hintergrund ausgeführt, ohne die Aktivitäten des Benutzers zu blockieren, und ist er für Benutzer von geringem Interesse, aber nicht völlig irrelevant?**

    Verwenden Sie Text, wenn die App Aufgaben ausführt, die zwar nicht immer sichtbar sein müssen, bei denen aber der Status angezeigt werden soll.

-   **Möchte der Benutzer nur über den Abschluss des Vorgangs informiert werden?**

    Manchmal ist es am besten, nur auf den Abschluss eines Vorgangs hinzuweisen oder den unmittelbaren Abschluss des Vorgangs durch ein visuelles Element anzukündigen, und den restlichen Vorgang im Hintergrund auszuführen.

## <a name="progress-controls-best-practices"></a>Bewährte Methoden für Statussteuerelemente

Manchmal ist eine visuelle Darstellung hilfreich, um zu ermitteln, zu welchem Zeitpunkt welches Statussteuerelement verwendet werden sollte:

**ProgressBar – bestimmt**

![Beispiel für ein bestimmtes ProgressBar-Element](images/progress-bar-determinate-example.png)

Beim ersten Beispiel handelt es sich um ein bestimmtes ProgressBar-Steuerelement. Wenn die Dauer des Vorgangs bekannt ist, etwa beim Installieren, Herunterladen oder Einrichten, eignet sich ein bestimmtes ProgressBar-Steuerelement.

**ProgressBar – unbestimmt**

![Beispiel für ein unbestimmtes ProgressBar-Element](images/progress-bar-indeterminate-example.png)

Wenn die Dauer des Vorgangs nicht bekannt ist, verwenden Sie ein unbestimmtes ProgressBar-Steuerelement. Unbestimmte ProgressBar-Elemente können auch beim Ausfüllen virtualisierter Listen verwendet werden oder um einen glatten visuellen Übergang von einem unbestimmten zu einem bestimmten ProgressBar-Element zu erstellen.

-   **Befindet sich der Vorgang in einer virtualisierten Sammlung?**

    In diesem Fall legen Sie nicht für jedes angezeigte Listenelement eine Statusanzeige fest. Positionieren Sie stattdessen ein ProgressBar-Element am Anfang der Auflistung der zu ladenden Elemente, um anzuzeigen, dass die Elemente geladen werden.

**ProgressRing – unbestimmt**

![Beispiel für ein unbestimmtes ProgressRing-Steuerelement](images/PR_IndeterminateExample.png)

Unbestimmte ProgressRing-Elemente werden verwendet, wenn jegliche Benutzerinteraktion mit der App ausgesetzt ist oder die App auf eine Benutzereingabe wartet, um den Vorgang fortzusetzen. Das „Anmelden...“- Beispiel oben ist ein optimales Szenario für das ProgressRing-Steuerelement, da der Benutzer die App erst weiterverwenden kann, nachdem der Anmeldevorgang abgeschlossen ist.

## <a name="customizing-a-progress-control"></a>Anpassen eines Statussteuerelements

Die beiden Statussteuerelemente sind recht einfach gehalten. Bei verschiedenen visuellen Features der Steuerelemente sind jedoch deren Anpassungsoptionen nicht offensichtlich.

**Ändern der Größe des ProgressRing-Elements**

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

**Farben der Statussteuerelemente**

Standardmäßig ist die Grundfarbe der Statussteuerelemente auf die Akzentfarbe des Systems festgelegt. Dies können Sie anpassen, indem Sie einfach die „Foreground“-Eigenschaft der Steuerelemente ändern.

```XAML
<ProgressRing IsActive="True" Height="100" Width="100" Foreground="Blue"/>
<muxc:ProgressBar Width="100" Foreground="Green"/>
```

Durch das Ändern die Vordergrundfarbe des ProgressRing-Elements ändern sich die Farben der Punkte. Die „Foreground“-Eigenschaft des ProgressBar-Elements ändert dann die Füllfarbe der Leiste. Um den ungefüllten Teil der Leiste zu ändern, überschreiben Sie einfach die „Background“-Eigenschaft.

**Anzeigen eines Wartecursors**

Manchmal ist es am besten, nur kurz einen Wartecursor anzuzeigen, wenn eine App oder ein Vorgang etwas Zeit erfordert und Sie dem Benutzer anzeigen müssen, dass mit der App oder dem Bereich, in dem sich der Wartecursor befindet, bis zu dessen Ausblenden nicht interagiert werden sollte.

```C#
Window.Current.CoreWindow.PointerCursor = new Windows.UI.Core.CoreCursor(Windows.UI.Core.CoreCursorType.Wait, 10);
```

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel für einen XAML-Steuerelementekatalog:](https://github.com/Microsoft/Xaml-Controls-Gallery) Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

- [ProgressBar-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressBar)
- [ProgressRing-Klasse](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ProgressRing)

**Für Entwickler (XAML)**
- [Hinzufügen von Statussteuerelementen](https://docs.microsoft.com/previous-versions/windows/apps/hh780651(v=win.10))
- [So wird's gemacht: Erstellen einer benutzerdefinierten unbestimmten Statusleiste für Windows Phone](https://msdn.microsoft.com/library/windows/apps/gg442303.aspx)
