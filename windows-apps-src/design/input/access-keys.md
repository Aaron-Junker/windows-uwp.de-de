---
description: Erfahren Sie, wie Sie die Benutzerfreundlichkeit und den Zugriff auf Ihre Windows-App verbessern können, indem Sie Benutzern eine intuitive Möglichkeit bieten, schnell zu navigieren und mit der sichtbaren Benutzeroberfläche einer APP über eine Tastatur anstelle eines Zeiger Geräts (z. b. berühren oder Maus) zu interagieren.
title: Zugriffstasten – Designrichtlinien
label: Access keys design guidelines
keywords: Tastatur, Zugriffstaste, KeyTip, schlüsseltipp, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktion
template: detail.hbs
ms.date: 09/24/2020
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 48f1d3bd69b10a8d500bb2d29b64c0eb91c9900b
ms.sourcegitcommit: 4f032d7bb11ea98783db937feed0fa2b6f9950ef
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/08/2020
ms.locfileid: "91829540"
---
# <a name="access-keys"></a>Zugriffsschlüssel

Zugriffstasten sind Tastenkombinationen, die die Benutzerfreundlichkeit und Barrierefreiheit Ihrer Windows-Anwendungen verbessern, indem Sie Benutzern eine intuitive Möglichkeit bieten, schnell zu navigieren und mit der sichtbaren Benutzeroberfläche einer APP über eine Tastatur anstelle eines Zeiger Geräts (z. b. berühren oder Maus) zu interagieren.

Ausführliche Informationen zum Aufrufen allgemeiner Aktionen in einer Windows-Anwendung mit Tastenkombinationen finden Sie im Thema Zugriffs [Tasten](keyboard-accelerators.md) . 

> [!NOTE]
> Eine Tastatur ist für Benutzer mit bestimmten Behinderungen unverzichtbar (siehe [Tastatur Barrierefreiheit](../accessibility/keyboard-accessibility.md)) und ist auch ein wichtiges Tool für Benutzer, die Sie als effizientere Methode für die Interaktion mit einer APP bevorzugen.

Die Windows-App bietet integrierte Unterstützung für Platt Form Steuerelemente sowohl für Tastatur basierte Zugriffsschlüssel als auch für das zugehörige Feedback der Benutzeroberfläche durch visuelle Hinweise, die als Schlüssel Tipps bezeichnet werden

## <a name="overview"></a>Übersicht

Eine Zugriffstaste ist eine Kombination aus der Alt-Taste und einem oder mehreren alphanumerischen Schlüsseln – manchmal auch als " *mnetmonik*" bezeichnet – in der Regel nicht gleichzeitig, sondern nacheinander gedrückt.

Wichtige Tipps sind neben Steuerelementen angezeigte Abzeichen, die Zugriffstasten unterstützen, wenn der Benutzer die Alt-Taste drückt. Jeder schlüsseltip enthält die alphanumerischen Schlüssel, die das zugeordnete Steuerelement aktivieren.

> [!NOTE]
> Tastenkombinationen werden automatisch für Zugriffstasten mit einem einzelnen alphanumerischen Zeichen unterstützt. Beispielsweise wird durch gleichzeitiges Drücken von Alt + F in Word das Menü Datei geöffnet, ohne dass wichtige Tipps angezeigt werden.

Durch Drücken der Alt-Taste werden Zugriffsschlüssel Funktionen initialisiert, und alle aktuell verfügbaren Tastenkombinationen werden in Schlüssel Tipps angezeigt. Nachfolgende Tastatureingaben werden vom Zugriffsschlüssel Framework behandelt, das ungültige Schlüssel ablehnt, bis entweder eine gültige Zugriffstaste gedrückt wird, oder die Tasten Eingabe, ESC, Tab oder Pfeil werden gedrückt, um Zugriffstasten zu deaktivieren und die Tastatureingaben an die APP zurückzugeben.

Microsoft Office-Apps bieten umfassende Unterstützung für Zugriffsschlüssel. Die folgende Abbildung zeigt die Registerkarte "Home" von Word mit aktivierten Zugriffs Schlüsseln (Beachten Sie die Unterstützung für beide Zahlen und mehrere Tastatureingaben).

![Key Tip-Kennzeichen für Zugriffsschlüssel in Microsoft Word](images/accesskeys/keytip-badges-word.png)

_KeyTip-Abzeichen für Zugriffsschlüssel in Microsoft Word_

Um einem Steuerelement einen Zugriffsschlüssel hinzuzufügen, verwenden Sie die **AccessKey-Eigenschaft**. Der Wert dieser Eigenschaft gibt die Zugriffsschlüssel Sequenz, die Verknüpfung (bei einem einzelnen alphanumerischen Wert) und den Schlüssel Tipp an.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>Verwendungszwecke von Zugriffs Schlüsseln

Es wird empfohlen, dass Sie überall auf der Benutzeroberfläche Zugriffsschlüssel angeben und Zugriffsschlüssel in allen benutzerdefinierten Steuerelementen unterstützen.

1.  Mithilfe von **Zugriffs Schlüsseln ist Ihre APP** für Benutzer mit motorischen Behinderungen zugänglicher, einschließlich der Benutzer, die jeweils nur einen Schlüssel drücken können oder Schwierigkeiten mit der Maus haben.

    Eine gut durchdachte Tastatur-UI ist ein wichtiger Aspekt für die Barrierefreiheit von Software. Sie ermöglicht es Benutzern mit einer Sehbeeinträchtigung oder mit bestimmten motorischen Einschränkungen, in einer App zu navigieren und mit deren Features zu interagieren. Diese Benutzer können u. U. keine Maus bedienen und sind auf verschiedene Hilfstechnologien wie etwa Tastaturerweiterungstools, Bildschirmtastaturen, Bildschirmlupen, Bildschirmleseprogramme oder die Möglichkeit der Spracheingabe angewiesen. Für diese Benutzer ist eine umfassende Befehls Abdeckung von entscheidender Bedeutung.

2.  Mithilfe von **Zugriffs Schlüsseln wird Ihre APP** für Hauptbenutzer verwendbar, die die Interaktion mit der Tastatur bevorzugen.

    Erfahrene Benutzer haben oft eine starke Vorliebe für die Verwendung der Tastatur, da Tastatur basierte Befehle schneller eingegeben werden können und Sie nicht verlangen, Ihre Hände von der Tastatur zu entfernen. Für diese Benutzer sind Effizienz und Konsistenz entscheidend. Die Vollständigkeit hingegen ist nur für am häufigsten verwendeten Befehle wichtig.

## <a name="set-access-key-scope"></a>Zugriffsschlüssel Bereich festlegen

Wenn auf dem Bildschirm viele Elemente vorhanden sind, die Zugriffstasten unterstützen, empfiehlt es sich, die Zugriffstasten zu beschränken, um die **Cognitive-Auslastung**zu verringern. Dadurch wird die Anzahl der Zugriffsschlüssel auf dem Bildschirm minimiert, sodass Sie leichter zu finden sind, und die Effizienz und Produktivität wird verbessert.

Beispielsweise bietet Microsoft Word zwei Zugriffsschlüssel Bereiche: einen primären Bereich für die Registerkarten des Menübands und einen sekundären Bereich für Befehle auf der ausgewählten Registerkarte.

Die folgenden Abbildungen veranschaulichen die zwei Zugriffsschlüssel Bereiche in Word. Der erste zeigt die primären Zugriffsschlüssel, mit denen ein Benutzer eine Registerkarte und andere Befehle auf oberster Ebene auswählen kann, während die zweite die sekundären Zugriffsschlüssel für die Registerkarte "Home" anzeigt.

![Primäre Zugriffsschlüssel in primären Microsoft Word- ](images/accesskeys/primary-access-keys-word.png)
 _Zugriffs Schlüsseln in Microsoft Word_

![Sekundäre Zugriffsschlüssel in sekundären Microsoft Word- ](images/accesskeys/secondary-access-keys-word.png)
 _Zugriffs Schlüsseln in Microsoft Word_

Zugriffsschlüssel können für Elemente in unterschiedlichen Bereichen dupliziert werden. Im vorherigen Beispiel ist "2" der Zugriffsschlüssel für Rückgängigmachen im primären Bereich und auch "Kursiv" im sekundären Bereich.

Hier wird gezeigt, wie Sie einen Zugriffsschlüssel Bereich definieren.

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![Primäre Zugriffsschlüssel für die Befehlsleiste](images/accesskeys/primary-access-keys-commandbar.png)

_Primärer CommandBar-Bereich und unterstützte Zugriffstasten_

![Sekundäre Zugriffsschlüssel für die Befehlsleiste](images/accesskeys/secondary-access-keys-commandbar.png)

_Sekundärer CommandBar-Bereich und unterstützte Zugriffstasten_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 Creators Update und älter

Vor dem Windows 10 Fall Creators Update konnten einige Steuerelemente, wie z. b. die CommandBar, keine integrierten Zugriffsschlüssel Bereiche unterstützen.

Im folgenden Beispiel wird gezeigt, wie Sie CommandBar secondarycommands mit Zugriffs Schlüsseln unterstützen, die nach dem Aufrufen eines übergeordneten Befehls verfügbar sind (ähnlich dem Menüband in Word).

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>Vermeiden von Zugriffsschlüssel Kollisionen

Zugriffsschlüssel Konflikte treten auf, wenn zwei oder mehr Elemente im gleichen Bereich über doppelte Zugriffsschlüssel verfügen oder mit denselben alphanumerischen Zeichen beginnen.

Das System löst doppelte Zugriffsschlüssel auf, indem der Zugriffsschlüssel des ersten Elements, das der visuellen Struktur hinzugefügt wird, verarbeitet und alle anderen ignoriert werden.

Wenn mehrere Zugriffsschlüssel mit demselben Zeichen (z. b. "A", "a1" und "ab") beginnen, verarbeitet das System den Schlüssel für den Einzelzeichen-Zugriff und ignoriert alle anderen.

Vermeiden Sie Konflikte, indem Sie eindeutige Zugriffsschlüssel oder Bereichs bezogene Befehle verwenden.

## <a name="choose-access-keys"></a>Zugriffsschlüssel auswählen

Beachten Sie beim Auswählen von Zugriffs Schlüsseln Folgendes:

-   Verwenden Sie ein einzelnes Zeichen, um Tastatureingaben zu minimieren, und unterstützen standardmäßig Tastenkombinationen (alt + AccessKey).
-   Vermeiden Sie die Verwendung von mehr als zwei Zeichen.
-   Vermeiden von Zugriffstasten Kollisionen
-   Vermeiden Sie Zeichen, die schwer von anderen Zeichen unterschieden werden können, z. b. den Buchstaben "I" und die Zahl "1" oder den Buchstaben "O" und die Zahl "0".
-   Verwenden Sie bekannte vorangestellt aus anderen gängigen apps, wie z. b. Word ("F" für "file", "H" für "Home" usw.)
-   Verwenden Sie das erste Zeichen des Befehlsnamens oder ein Zeichen mit einer Close-Zuordnung zum Befehl, der den Rückruf unterstützt.
    -   Wenn der erste Buchstabe bereits zugewiesen ist, verwenden Sie einen Buchstaben, der so nah wie möglich für den ersten Buchstaben des Befehlsnamens ist ("N" für INSERT).
    -   Verwenden Sie einen unverwechselbaren Konsonanten aus dem Befehlsnamen ("W" für die Ansicht).
    -   Verwenden Sie einen Vokal aus dem Befehlsnamen.

## <a name="localize-access-keys"></a>Lokalisieren von Zugriffs Schlüsseln

Wenn Ihre APP in mehreren Sprachen lokalisiert werden soll, sollten Sie auch **die Lokalisierung der Zugriffsschlüssel in Erwägung gezogen**. Beispiel: "H" für "Home" in "en-US" und "I" für "Incio" in es-es.

Verwenden Sie die x:UID-Erweiterung in Markup, um lokalisierte Ressourcen wie im folgenden dargestellt anzuwenden:

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
Die Ressourcen für die einzelnen Sprachen werden den entsprechenden Zeichen folgen Ordnern im Projekt hinzugefügt:

![Englische und spanische Ressourcen Zeichen folgen Ordner](images/accesskeys/resource-string-folders.png)

_Englische und spanische Ressourcen Zeichen folgen Ordner_

Lokalisierte Zugriffsschlüssel werden in der Datei "Resources. resw" des Projekts angegeben:

![Geben Sie die in der Datei "Resources. resw" angegebene AccessKey-Eigenschaft an.](images/accesskeys/resource-resw-file.png)

_Geben Sie die in der Datei "Resources. resw" angegebene AccessKey-Eigenschaft an._

Weitere Informationen finden Sie unter über [setzen von UI-Ressourcen](/previous-versions/windows/apps/hh965329(v=win.10)) .

## <a name="key-tip-positioning"></a>Positionieren von Schlüssel Tipps

Wichtige Tipps werden als Gleit Komma Zahlen in Relation zum entsprechenden Benutzeroberflächen Element angezeigt. dabei werden die Anwesenheit anderer Benutzeroberflächen Elemente, anderer Schlüssel Tipps und der Bildschirm Kante berücksichtigt.

Normalerweise ist der Standard Speicherort für den Schlüssel Tip ausreichend und bietet integrierte Unterstützung für die Adaptive Benutzeroberfläche.

![Beispiel für automatische schlüsseltip-Platzierung](images/accesskeys/auto-keytip-position.png)

_Beispiel für automatische schlüsseltip-Platzierung_

Wenn Sie jedoch mehr Kontrolle über die schlüsseltip Positionierung benötigen, empfehlen wir Folgendes:

1.  **Offensichtliches**Zuordnungs Prinzip: der Benutzer kann das Steuerelement problemlos mit dem Schlüssel Tipp verknüpfen.

    a.  Der Schlüssel Tipp sollte sich in der Nähe des Elements **befinden** , das über die Zugriffstaste verfügt (der Besitzer).  
    b.  Der Schlüssel Tipp sollte das **abdecken von aktivierten Elementen** mit Zugriffs Schlüsseln vermeiden.   
    c.  Wenn ein Schlüssel Tipp nicht in der Nähe des Besitzers platziert werden kann, sollte er sich überlappen. 

2.  **Auffindbarkeit**: der Benutzer kann das Steuerelement schnell mit dem Schlüssel Tipp ermitteln.

    a.  Der Schlüssel Tipp **überlappt** nie andere wichtige Tipps.  

3.  **Einfache** Überprüfung: Der Benutzer kann problemlos die Schlüssel Tipps erhalten.

    a.  Wichtige Tipps sollten miteinander und mit dem Benutzeroberflächen Element **ausgerichtet** werden.
    b.  Wichtige Tipps sollten so weit wie möglich **gruppiert** werden. 

### <a name="relative-position"></a>Relative Position

Verwenden Sie die **keytipplacementmode** -Eigenschaft, um die Platzierung des Schlüssel Tipps für ein pro-Element oder pro Gruppe anzupassen.

Die Platzierungs Modi sind: oben, unten, rechts, Links, ausgeblendet, zentriert und automatisch.

![Screenshot mit den relativen Positionen der schlüsseltip-Platzierungs Modi](images/accesskeys/keytip-postion-modes.png)

_Schlüsseltip-Platzierungs Modi_

Die zentrale Linie des Steuer Elements wird verwendet, um die vertikale und horizontale Ausrichtung des Schlüssel Tipps zu berechnen.

Im folgenden Beispiel wird gezeigt, wie die schlüsseltip Platzierung einer Gruppe von Steuerelementen mithilfe der keytipplacementmode-Eigenschaft eines StackPanel-Containers festgelegt wird.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Offsets

Verwenden Sie die keytiphorizontaloffset-Eigenschaft und die keytipverticaloffset-Eigenschaft eines Elements, um eine noch präzisetere Steuerung des schlüsseltip-Speicher Orts zu verwenden.

> [!NOTE]
> Offsets können nicht festgelegt werden, wenn keytipplacementmode auf Auto festgelegt ist.

Die keytiphorizontaloffset-Eigenschaft gibt an, wie weit der schlüsseltip nach links oder rechts verschoben werden soll.

![Screenshot der vertikalen und horizontalen KeyTip Offsets für eine Schaltfläche](images/accesskeys/keytip-offsets.png)

_Festlegen von vertikalen und horizontalen KeyTip Offsets für eine Schaltfläche_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Bildschirmrand Ausrichtung {#Screen-Edge-Alignment. Listparagraph}

Der Speicherort eines Schlüssel Tipps wird automatisch basierend auf dem Bildschirmrand angepasst, um sicherzustellen, dass der schlüsseltip vollständig sichtbar ist. In diesem Fall kann sich der Abstand zwischen dem Steuerelement und dem Tastatur Tipp-Ausrichtungs Punkt von den Werten unterscheiden, die für die horizontale und vertikale Offsets angegeben werden.

![Screenshot der Bildschirmrand Ausrichtung des Schlüssel Tipps](images/accesskeys/keytips-screen-edge.png)

_Schlüssel Tipps werden automatisch basierend auf dem Bildschirmrand positioniert_

## <a name="key-tip-style"></a>Schlüsseltip-Stil

Wir empfehlen die Verwendung der integrierten schlüsseltip Unterstützung für Platt Form Designs, einschließlich hoher Kontraste.

Wenn Sie Ihre eigenen Schlüssel Tipp Stile angeben müssen, verwenden Sie Anwendungs Ressourcen wie keytipfontsize (Font Size), keytipfontfamily (Font Family), keytipbackground (Background), keytipvorder Grund (Vordergrund), keytippadditions (Padding), keytipborderbrush (Rahmenfarbe) und keytipbordertemedicke (Rahmen Stärke).

![Screenshot der Optionen für die Anpassung von Schlüssel Tipps, einschließlich Schriftart, Reihenfolge und Farbe](images/accesskeys/keytip-customization.png)

_Optionen für die Anpassung von Schlüssel Tipps_

In diesem Beispiel wird veranschaulicht, wie diese Anwendungs Ressourcen geändert werden:

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>Zugriffstasten und Sprachausgabe

Das XAML-Framework macht Automatisierungs Eigenschaften verfügbar, die es Benutzeroberflächenautomatisierungs-Clients ermöglichen, Informationen zu Elementen in der Benutzeroberfläche zu ermitteln.

Wenn Sie die AccessKey-Eigenschaft für ein UIElement-oder Textelement-Steuerelement angeben, können Sie diesen Wert mit der [AutomationProperties. AccessKey](/dotnet/api/system.windows.automation.automationproperties.accesskey) -Eigenschaft erhalten. Barrierefreiheits Clients, z. b. die Sprachausgabe, lesen den Wert dieser Eigenschaft jedes Mal, wenn ein Element den Fokus erhält.

## <a name="related-articles"></a>Verwandte Artikel

* [Tastaturinteraktionen](keyboard-interactions.md)
* [Tastaturkürzel](keyboard-accelerators.md)

**Beispiele**
* [Katalog der XAML-Steuerelemente (auch als xamluibasics bezeichnet)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)
