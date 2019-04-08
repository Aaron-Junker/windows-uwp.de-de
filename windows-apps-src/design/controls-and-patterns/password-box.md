---
Description: Bei einem Kennwortfeld handelt es sich um ein Texteingabefeld, das die darin eingegebenen Zeichen zu Datenschutzzwecken verdeckt.
title: Richtlinien für Kennwortfelder
ms.assetid: 332B04D6-4FFE-42A4-8B3D-ABE8266C7C18
dev.assetid: 4BFDECC6-9BC5-4FF5-8C63-BB36F6DDF2EF
label: Password box
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
pm-contact: miguelrb
design-contact: ksulliv
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 43639ac22c07501da89e29419a226a8a3ce8af48
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57613065"
---
# <a name="password-box"></a>Kennwortfeld

 

Bei einem Kennwortfeld handelt es sich um ein Texteingabefeld, das die darin eingegebenen Zeichen zu Datenschutzzwecken verdeckt. Ein Kennwortfeld sieht wie eine Textfeld aus. Die einzige Ausnahme besteht darin, dass anstelle des eingegebenen Texts Platzhalterzeichen angezeigt werden. Sie können das Platzhalterzeichen konfigurieren.

> **Wichtige APIs:** [PasswordBox-Klasse](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.aspx), [Kennworteigenschaft](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.password.aspx), [PasswordChar-Eigenschaft](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchar.aspx), [PasswordRevealMode Eigenschaft](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordrevealmode.aspx), [PasswordChanged-Ereignis](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchanged.aspx)

Standardmäßig ermöglicht es das Kennwortfeld dem Benutzer, durch Gedrückthalten einer Schaltfläche zum Anzeigen des Kennworts sein Kennwort anzuzeigen. Sie können die Schaltfläche zum Anzeigen des Kennworts deaktivieren oder einen alternativen Mechanismus zum Anzeigen des Kennworts, z. B. ein Kontrollkästchen, bereitstellen.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Verwenden Sie ein **PasswordBox**-Steuerelement, um Kennwörter oder andere private Daten wie Sozialversicherungsnummern zu erfassen.

Weitere Informationen zur Auswahl des passenden Textsteuerelements finden Sie im Artikel [Textsteuerelemente](text-controls.md).

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um <a href="xamlcontrolsgallery:/item/PasswordBox">die App zu öffnen und PasswordBox in Aktion zu sehen</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Das Kennwortfeld verfügt über mehrere Zustände, wichtig sind insbesondere die folgenden.

Bei Inaktivität kann zu einem Kennwortfeld Hinweistext angezeigt werden, um dem Benutzer seinen Zweck mitzuteilen:

![Kennwortfeld im Ruhezustand mit Hinweistext](images/passwordbox-rest-hinttext.png)

Wenn der Benutzer Zeichen in ein Kennwortfeld eingibt, werden standardmäßig Aufzählungszeichen angezeigt, die den eingegebenen Text verbergen:

![Kennwortfeld im Fokuszustand bei Texteingabe](images/passwordbox-focus-typing.png)

Durch Drücken der Schaltfläche zur Offenlegung des Kennworts auf der rechten Seite wird das eingegebene Kennwort angezeigt:

![Kennwortfeld mit angezeigtem Text](images/passwordbox-text-reveal.png)

## <a name="create-a-password-box"></a>Erstellen eines Kennwortfelds

Verwenden Sie die [Password](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.password.aspx)-Eigenschaft, um den Inhalt des Kennwortfelds abzurufen oder festzulegen. Dies kann im Handler für das [PasswordChanged](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchanged.aspx)-Ereignis erfolgen, um die Überprüfung durchzuführen, während der Benutzer das Kennwort eingibt. Oder Sie können ein weiteres Ereignis, z. B. ein [Click](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.buttonbase.click.aspx)-Ereignis der Schaltfläche, verwenden, um die Überprüfung durchzuführen, nachdem der Benutzer die Texteingabe abgeschlossen hat.

Dieser XAML-Code für ein Kennwortfeld-Steuerelement zeigt das Standardaussehen des Kennwortfelds. Wenn der Benutzer ein Kennwort eingibt, wird überprüft, ob es dem Literalwert „Password“ entspricht. In diesem Fall wird dem Benutzer eine Meldung angezeigt.

```xaml
<StackPanel>  
  <PasswordBox x:Name="passwordBox" Width="200" MaxLength="16"
             PasswordChanged="passwordBox_PasswordChanged"/>

  <TextBlock x:Name="statusText" Margin="10" HorizontalAlignment="Center" />
</StackPanel>   
```

```csharp
private void passwordBox_PasswordChanged(object sender, RoutedEventArgs e)
{
    if (passwordBox.Password == "Password")
    {
        statusText.Text = "'Password' is not allowed as a password.";
    }
    else
    {
        statusText.Text = string.Empty;
    }
}
```
Dies ist das Ergebnis, wenn der Code ausgeführt und als Kennwort „Password“ eingegeben wird.

![Kennwortfeld mit Überprüfungshinweis](images/passwordbox-revealed-validation.png)

### <a name="password-character"></a>Kennwortzeichen

Durch Festlegen der [PasswordChar](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordchar.aspx)-Eigenschaft können Sie das zum Maskieren des Kennworts verwendete Zeichen ändern. Hier wird das standardmäßige Aufzählungszeichen durch ein Sternchen ersetzt.

```xaml
<PasswordBox x:Name="passwordBox" Width="200" PasswordChar="*"/>
```

Das Ergebnis sieht wie folgt aus.

![Kennwortfeld mit einem benutzerdefinierten Maskierungszeichen](images/passwordbox-custom-char.png)

### <a name="headers-and-placeholder-text"></a>Kopf- und Platzhaltertext

Mit den Eigenschaften [Header](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.header.aspx) und [PlaceholderText](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.placeholdertext.aspx) können Sie den Kontext für das Kennwortfeld angeben. Dies ist besonders hilfreich, wenn mehrere Felder vorhanden sind, z. B. in einem Formular zum Ändern des Kennworts.

```xaml
<PasswordBox x:Name="passwordBox" Width="200" Header="Password" PlaceholderText="Enter your password"/>
```

![Kennwortfeld im Ruhezustand mit Hinweistext](images/passwordbox-rest-hinttext.png)

### <a name="maximum-length"></a>Maximale Länge

Geben Sie die maximale Anzahl von Zeichen an, die der Benutzer eingeben darf, indem Sie die [MaxLength](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.maxlength.aspx)-Eigenschaft festlegen. Es gibt keine Eigenschaft zum Festlegen einer Mindestlänge. Sie können jedoch im App-Code die Kennwortlänge überprüfen und weitere Überprüfungen durchführen.

## <a name="password-reveal-mode"></a>Kennwortanzeigemodus

Das Kennwortfeld verfügt über eine integrierte Schaltfläche, mit der Benutzer das eingegebene Kennwort anzeigen können. Hier ist das Ergebnis der Benutzeraktion. Lässt der Benutzer die Schaltfläche los, wird das Kennwort automatisch wieder ausgeblendet.

![Kennwortfeld mit angezeigtem Text](images/passwordbox-text-reveal.png)

### <a name="peek-mode"></a>Vorschaumodus

Standardmäßig wird die Schaltfläche zum Anzeigen des Kennworts (oder die Vorschau-Schaltfläche) angezeigt. Der Benutzer muss die Schaltfläche gedrückt halten, um das Kennwort anzuzeigen, sodass ein hohes Maß an Sicherheit gewährleistet ist.

Der Wert der [PasswordRevealMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordbox.passwordrevealmode.aspx)-Eigenschaft ist nicht der einzige Faktor, der bestimmt, ob eine Schaltfläche zum Anzeigen des Kennworts für den Benutzer sichtbar ist. Weitere Faktoren sind, ob das Steuerelement mit einer größeren Breite als der Mindestbreite angezeigt wird, ob das Kennwortfeld den Fokus hat und ob das Texteingabefeld mindestens ein Zeichen enthält. Die Schaltfläche zum Anzeigen des Kennworts wird nur angezeigt, wenn das Kennwortfeld zum ersten Mal den Fokus erhält und ein Zeichen eingegeben wird. Wenn PasswordBox den Fokus verliert und ihn anschließend wieder erhält, wird die Schaltfläche zum Anzeigen des Kennworts nicht angezeigt, es sei denn, das Kennwort wird gelöscht und die Zeichen werden erneut eingegeben.

> **Achtung**&nbsp;&nbsp;Vor Windows 10 wurde die Schaltfläche zum Anzeigen des Kennworts nicht standardmäßig angezeigt. Wenn die Sicherheit Ihrer App erfordert, dass das Kennwort immer verdeckt ist, muss „PasswordRevealMode“ auf „Hidden“ festgelegt werden.

### <a name="hidden-and-visible-modes"></a>Ausblendungs- und Anzeigemodus

Mit den weiteren [PasswordRevealMode](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.passwordrevealmode.aspx)-Aufzählungswerten **Hidden** und **Visible** blenden Sie die Schaltfläche zum Anzeigen des Kennworts aus. Sie können programmgesteuert festlegen, ob das Kennwort verdeckt wird.

Um das Kennwort immer zu verdecken, legen Sie „PasswordRevealMode“ auf „Hide“ fest. Wenn das Kennwort nicht immer verdeckt sein muss, können Sie eine benutzerdefinierte Benutzeroberfläche bereitstellen, die dem Benutzer das Umschalten von „PasswordRevealMode“ zwischen „Hidden“ and „Visible“ ermöglicht.

In früheren Versionen von Windows Phone wurde für das Kennwortfeld eine Kontrollkästchen verwendet, um zwischen Anzeigen und Verdecken des Kennworts umzuschalten. Sie können eine ähnliche Benutzeroberfläche für Ihre App erstellen, wie im folgenden Beispiel gezeigt. Sie können auch andere Steuerelemente wie [ToggleButton](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.primitives.togglebutton.aspx) verwenden, damit der Benutzer zwischen den Modi wechseln kann.

In diesem Beispiel wird gezeigt, wie Sie mit einem [CheckBox](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.checkbox.aspx)-Steuerelement dem Benutzer das Wechseln des Anzeigemodus eines Kennwortfelds ermöglichen.

```xaml
<StackPanel Width="200">
    <PasswordBox Name="passwordBox1"
                 PasswordRevealMode="Hidden"/>
    <CheckBox Name="revealModeCheckBox" Content="Show password"
              IsChecked="False"
              Checked="CheckBox_Changed" Unchecked="CheckBox_Changed"/>
</StackPanel>
```

```csharp
private void CheckBox_Changed(object sender, RoutedEventArgs e)
{
    if (revealModeCheckBox.IsChecked == true)
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Visible;
    }
    else
    {
        passwordBox1.PasswordRevealMode = PasswordRevealMode.Hidden;
    }
}
```

Dieses Kennwortfeld sieht in etwa so aus.

![Kennwortfeld mit einer benutzerdefinierten Schaltfläche zum Anzeigen des Kennworts](images/passwordbox-custom-reveal.png)

## <a name="choose-the-right-keyboard-for-your-text-control"></a>Auswählen der richtigen Tastatur für Ihr Textsteuerelement

Um Benutzern die Eingabe von Daten mit der Bildschirmtastatur oder dem Soft Input Panel (SIP) zu erleichtern, können Sie den Eingabeumfang des Textsteuerelements an die Art der Daten anpassen, die der Benutzer vermutlich eingeben wird. Das Kennwortfeld unterstützt nur die Eingabeumfangwerte **Password** und **NumericPin**. Andere Werte werden ignoriert.

Weitere Informationen zur Verwendung von Eingabeumfängen finden Sie unter [Verwenden des Eingabeumfangs zum Ändern der Bildschirmtastatur](https://msdn.microsoft.com/library/windows/apps/mt280229).

## <a name="recommendations"></a>Empfehlungen

-   Verwenden Sie eine Beschriftung oder Platzhaltertext, wenn der Zweck des Kennwortfelds nicht eindeutig ist. Eine Beschriftung ist sichtbar, unabhängig davon, ob das Texteingabefeld über einen Wert verfügt. Platzhaltertext wird im Texteingabefeld angezeigt und wird erst ausgeblendet, wenn der Benutzer einen Wert eingibt.
-   Weisen Sie dem Kennwortfeld eine entsprechende Breite für den Bereich der Werte zu, die eingegeben werden können. Die Wortlänge variiert zwischen den einzelnen Sprachen. Sie sollten also die Lokalisierung berücksichtigen, wenn Ihre App global verwendet werden soll.
-   Platzieren Sie kein anderes Steuerelement neben einem Eingabefeld für Kennwörter. Das Kennwortfeld weist eine Schaltfläche zum Anzeigen des Kennworts auf, damit Benutzer die eingegebenen Kennwörter überprüfen können. Wenn ein anderes Steuerelement direkt daneben vorhanden ist, zeigen Benutzer möglicherweise versehentlich ihre Kennwörter an, wenn sie versuchen, mit dem anderen Steuerelement zu interagieren. Um dies zu verhindern, achten Sie auf einen gewissen Abstand zwischen dem Eingabefeld für Kennwörter und dem anderen Steuerelement. Sie können das andere Steuerelement auch in der nächsten Zeile platzieren.
-   Ziehen Sie für die Kontenerstellung die Anzeige von zwei Kennwortfeldern in Erwägung: eins für das neue Kennwort und ein zweites zum Bestätigen des neuen Kennworts.
-   Zeigen Sie für Benutzernamen nur ein einzelnes Kennwortfeld an.
-   Wenn ein Kennwortfeld zur PIN-Eingabe verwendet wird, sollten Sie möglicherweise statt einer Bestätigungsschaltfläche eine sofortige Reaktion nach der Eingabe der letzten Zahl vorsehen.

## <a name="get-the-sample-code"></a>Beispielcode herunterladen

- [Beispiel eines XAML-Steuerelementekatalogs](https://github.com/Microsoft/Xaml-Controls-Gallery) – Hier werden alle XAML-Steuerelemente in einem interaktiven Format dargestellt.

## <a name="related-articles"></a>Verwandte Artikel

[Textsteuerelemente](text-controls.md)

- [Richtlinien für die Rechtschreibprüfung](text-controls.md)
- [Hinzufügen von Suchfunktionen](https://msdn.microsoft.com/library/windows/apps/hh465231)
- [Richtlinien für die Texteingabe](text-controls.md)
- [TextBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br209683)
- [Windows.UI.Xaml.Controls PasswordBox-Klasse](https://msdn.microsoft.com/library/windows/apps/br227519)
- [String.Length-Eigenschaft](https://msdn.microsoft.com/library/system.string.length(v=vs.110).aspx)
