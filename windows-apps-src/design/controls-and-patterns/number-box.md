---
Description: Ein Zahlenfeld (NumberBox) ist ein Steuerelement, das zum Anzeigen und Bearbeiten von Zahlen verwendet werden kann.
title: Zahlenfeld
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 42f37a2753cced0f53fab54af393af74c21f3597
ms.sourcegitcommit: 39fb8c0dff1b98ededca2f12e8ea7977c2eddbce
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91749756"
---
# <a name="number-box"></a>Zahlenfeld

Stellt ein Steuerelement dar, das zum Anzeigen und Bearbeiten von Zahlen verwendet werden kann. Dies unterstützt Validierung, inkrementelle Schritte und das Berechnen von Inlineberechnungen einfacher Gleichungen, wie Multiplikation, Division, Addition und Subtraktion.

**Abrufen der Windows-UI-Bibliothek**

:::row:::
   :::column:::
      ![WinUI-Logo](images/winui-logo-64x64.png)
   :::column-end:::
   :::column span="3":::
      Das Steuerelement **NumberBox** erfordert die Windows-UI-Bibliothek. Diese Bibliothek ist ein NuGet-Paket, das neue Steuerelemente und Benutzeroberflächenfeatures für Windows-Apps enthält. Weitere Informationen, einschließlich Installationsanweisungen, finden Sie unter [Windows UI Library](/uwp/toolkits/winui/) (Windows-UI-Bibliothek).
   :::column-end:::
   :::column:::

   :::column-end:::
:::row-end:::

**Windows-UI-Bibliotheks-APIs:** [NumberBox-Klasse](/uwp/api/microsoft.ui.xaml.controls.NumberBox)

> [!TIP]
> In diesem Dokument stellt der Alias **muxc** in XAML die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben dem [Page](/uwp/api/windows.ui.xaml.controls.page)-Element Folgendes hinzugefügt: `xmlns:muxc="using:Microsoft.UI.Xaml.Controls"`
>
>Im CodeBehind stellt ebenfalls der Alias **muxc** in C# die APIs der Windows-UI-Bibliothek dar, die wir in unser Projekt aufgenommen haben. Wir haben am Anfang der Datei die folgende **using**-Anweisung hinzugefügt: `using muxc = Microsoft.UI.Xaml.Controls;`

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Sie können ein NumberBox-Steuerelement verwenden, um mathematische Eingaben zu erfassen und anzuzeigen. Wenn Sie ein editierbares Textfeld benötigen, das auch andere Werte als Zahlen akzeptiert, verwenden Sie das [TextBox](/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Steuerelement. Wenn Sie ein editierbares Textfeld benötigen, das Kennwörter oder andere vertrauliche Eingaben akzeptiert, verwenden Sie [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox). Wenn Sie ein Textfeld zur Eingabe von Suchbegriffen benötigen, verwenden Sie [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox). Informationen zum Eingeben oder Bearbeiten von formatiertem Text finden Sie unter [RichEditBox](/uwp/api/windows.ui.xaml.controls.richeditbox).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/TextBox">die App zu öffnen und NumberBox in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="create-a-simple-numberbox"></a>Erstellen einer einfachen NumberBox

Hier sehen Sie den XAML-Code für ein einfaches NumberBox-Steuerelement in der Standarddarstellung. Verwenden Sie [x:Bind](../../xaml-platform/x-bind-markup-extension.md#property-path), um sicherzustellen, dass die dem Benutzer angezeigten Daten stets mit den in Ihrer App gespeicherten Daten synchron sind.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![Ein Eingabefeld im Fokus, das 0 (null) anzeigt.](images/numberbox-basic.png)

### <a name="labeling-numberbox"></a>Bezeichnungen für NumberBox

Verwenden Sie `Header` oder `PlaceholderText`, wenn der Zweck des NumberBox-Steuerelements nicht selbsterklärend ist. `Header` ist immer sichtbar, unabhängig davon, ob die NumberBox einen Wert aufweist.

```xaml
<muxc:NumberBox Header="Enter a number:"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![Eine Kopfzeile "Ausdruck eingeben:" oberhalb eines NumberBox-Steuerelements.](images/numberbox-header.png)

`PlaceholderText` wird innerhalb des Zahlenfelds angezeigt und ist nur sichtbar, wenn `Value` auf NaN festgelegt ist oder die Eingabe vom Benutzer gelöscht wird.

```xaml
<muxc:NumberBox PlaceholderText="1+2^2"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

![Ein NumberBox-Steuerelement mit dem Platzhaltertext "A + B".](images/numberbox-placeholder-text.png)

### <a name="enable-calculation-support"></a>Aktivieren der Berechnungsunterstützung

Wenn Sie die `AcceptsExpression`-Eigenschaft auf TRUE festlegen, kann das NumberBox-Steuerelement einfache Inlineausdrücke wie Multiplikation, Division, Addition und Subtraktion in der Standardreihenfolge der Operationen berechnen. Die Auswertung wird durch den Verlust des Eingabefokus oder durch Drücken der EINGABETASTE durch den Benutzer ausgelöst. Nach der Auswertung eines Ausdrucks bleibt dessen ursprüngliche Form nicht erhalten.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    AcceptsExpression="True" />
```

### <a name="increment-and-decrement-stepping"></a>Inkrementelle und dekrementelle Schritte

Verwenden Sie die `SmallChange`-Eigenschaft, um zu konfigurieren, wie stark sich der Wert in einem NumberBox-Steuerelement ändert, wenn die NumberBox den Fokus hat und der Benutzer dies ausführt:

- Scrollen
- Drücken der NACH-OBEN-TASTE
- Drücken der NACH-UNTEN-TASTE

Verwenden Sie die `LargeChange`-Eigenschaft, um zu konfigurieren, wie stark sich der Wert in einer NumberBox ändert, wenn die NumberBox den Fokus hat und der Benutzer die NACH-OBEN- oder NACH-UNTEN-TASTE drückt.

Verwenden Sie die `SpinButtonPlacementMode`-Eigenschaft, um Schaltflächen zu aktivieren, die den Wert in der NumberBox durch Klicken um den in der `SmallChange`-Eigenschaft angegebenen Betrag ändern. Diese Schaltflächen werden deaktiviert, wenn mit dem nächsten Schritt ein Maximal- oder Minimalwert über- bzw. unterschritten würde.

Legen Sie `SpinButtonPlacementMode` auf `Inline` fest, um die Darstellung der Schaltflächen neben dem Steuerelement zu aktivieren.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Inline" />
```

![Ein NumberBox-Steuerelement mit seitlich angeordneten Schaltflächen nach oben und unten.](images/numberbox-spinbutton-inline.png)

Legen Sie `SpinButtonPlacementMode` auf `Compact` fest, um die Darstellung der Schaltflächen als Flyout zu aktivieren, wenn die NumberBox den Fokus hat.

```xaml
<muxc:NumberBox Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    SmallChange="10"
    LargeChange="100"
    SpinButtonPlacementMode="Compact" />
```

![Eine NumberBox mit einem kleinen Symbol darin, das einen aufwärts gerichteten Pfeil und einen abwärts gerichteten Pfeil darstellt.](images/numberbox-spinbutton-compact-non-visible.png)

![Eine NumberBox mit einem abwärts gerichteten Pfeil und einem aufwärts gerichteten Pfeil, das auf einer höheren Ebene seitlich unverankert ist.](images/numberbox-spinbutton-compact-visible.png)

### <a name="enabling-input-validation"></a>Aktivieren der Eingabevalidierung

Das Festlegen von `ValidationMode` auf `InvalidInputOverwritten` ermöglicht es NumberBox, ungültige Eingaben zu überschreiben, wenn eine Eingabe nicht numerisch ist oder syntaktisch mit dem letzten eingegebenen Wert keine Formel bildet, wenn die Auswertung durch den Verlust des Fokus oder Drücken der EINGABETASTE ausgelöst wird.

```xaml
<muxc:NumberBox Header="Quantity"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}"
    ValidationMode="InvalidInputOverwritten" />
```

Das Festlegen von `ValidationMode` auf `Disabled` ermöglicht die Konfiguration von benutzerdefinierter Eingabevalidierung.

Im Hinblick auf Dezimaltrennzeichen und Tausendertrennzeichen wird die von einem Benutzer verwendete Formatierung durch die für die NumberBox konfigurierte Formatierung ersetzt. Dafür wird kein Eingabevalidierungsfehler ausgelöst.

### <a name="formatting-input"></a>Formatieren der Eingabe

[Zahlenformate](/uwp/api/windows.globalization.numberformatting) können verwendet werden, um den Wert eines Zahlenfelds zu formatieren, indem eine Instanz einer Formatierungsklasse konfiguriert und der `NumberFormatter`-Eigenschaft zugewiesen wird. Dezimal, Währung, Prozent und signifikante Stellen sind nur einige der verfügbaren Klassen mit Zahlenformaten. Beachten Sie, dass durch die Eigenschaften zur Formatierung von Zahlen auch die Rundungseigenschaften festgelegt werden.

Hier sehen Sie ein Beispiel für die Verwendung von DecimalFormatter, um den Wert eines NumberBox-Steuerelements als eine ganzzahlige Stelle mit zwei Dezimalstellen und Aufrundung auf die nächsten 0,25 zu formatieren:

```xaml
<muxc:NumberBox  x:Name="FormattedNumberBox"
    Value="{x:Bind Path=ViewModel.NumberBoxValue, Mode=TwoWay}" />
```

```csharp
private void SetNumberBoxNumberFormatter()
{
    IncrementNumberRounder rounder = new IncrementNumberRounder();
    rounder.Increment = 0.25;
    rounder.RoundingAlgorithm = RoundingAlgorithm.RoundUp;

    DecimalFormatter formatter = new DecimalFormatter();
    formatter.IntegerDigits = 1;
    formatter.FractionDigits = 2;
    formatter.NumberRounder = rounder;
    FormattedNumberBox.NumberFormatter = formatter;
}
```

![Ein NumberBox-Steuerelement mit dem Wert 0,00.](images/numberbox-formatted.png)

Im Hinblick auf Dezimaltrennzeichen und Tausendertrennzeichen wird die von einem Benutzer verwendete Formatierung durch die für die NumberBox konfigurierte Formatierung ersetzt. Dafür wird kein Eingabevalidierungsfehler ausgelöst.

## <a name="remarks"></a>Hinweise

### <a name="input-scope"></a>Eingabebereich

`Number` wird für den [Eingabebereich](/uwp/api/Windows.UI.Xaml.Input.InputScopeNameValue) verwendet. Dieser Eingabebereich ist für die Funktion mit den Zahlen 0–9 vorgesehen. Diese Einstellung kann überschrieben werden, alternative InputScope-Typen werden aber nicht explizit unterstützt.

### <a name="not-a-number"></a>Not a Number (NaN, kein Zahlenwert)

Wenn in einer NumberBox alle Eingaben gelöscht werden, wird `Value` auf `NaN` festgelegt, um anzuzeigen, dass kein Zahlenwert vorhanden ist.

### <a name="expression-evaluation"></a>Ausdrucksauswertung

NumberBox verwendet die Infix-Notation zum Auswerten von Ausdrücken. Dies sind die zulässigen Operatoren in der Reihenfolge ihres Vorrangs:

* ^
* */
* +-

Beachten Sie, dass zum Außer-Kraft-Setzen der Vorrangsregeln Klammern verwendet werden können.

## <a name="recommendations"></a>Empfehlungen

* Mit `Text` und `Value` kann der Wert einer NumberBox leicht als Zeichenfolge oder als Double-Wert erfasst werden, ohne dass der Wert zwischen den Typen umgewandelt werden müsste. Wenn der Wert eines NumberBox-Steuerelements programmgesteuert geändert werden soll, empfiehlt sich dazu die `Value`-Eigenschaft. `Value` überschreibt `Text` in der ursprünglichen Einrichtung. Nach der ursprünglichen Einrichtung werden Änderungen am einen Typ an den anderen weitergegeben, die einheitliche Verwendung von `Value` für programmgesteuerte Änderungen hilft aber, konzeptionelle Missverständnisse, dass NumberBox nicht numerische Zeichen über `Text`. akzeptiert, zu vermeiden.
* Verwenden Sie `Header` oder `PlaceholderText`, um Benutzer darüber zu informieren, dass NumberBox nur Ziffern als Eingabe unterstützt. Die Wortdarstellung von Zahlen, wie etwa "Eins", wird nicht zu einem akzeptierten Wert aufgelöst.
