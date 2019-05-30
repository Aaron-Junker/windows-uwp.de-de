---
Description: Erfahren Sie, wie Sie Benutzerfreundlichkeit und Barrierefreiheit Ihrer UWP-App verbessern, indem Sie Benutzern eine intuitive Möglichkeit bereitstellen, schnell in der sichtbaren UI einer App über eine Tastatur statt per Eingabe mit dem Finger oder der Maus zu navigieren und zu interagieren.
title: Zugriffstasten – Designrichtlinien
label: Access keys design guidelines
keywords: Tastatur, Zugriffstaste, Zugriffstasteninfo, Info zu Zugriffstasten, Barrierefreiheit, Navigation, Fokus, Text, Eingabe, Benutzerinteraktion
template: detail.hbs
ms.date: 06/08/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 6c13ac8f1421fe785ebce70789c8ea6d0bf6c068
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66364016"
---
# <a name="access-keys"></a>Zugriffstasten

Zugriffstasten sind Verknüpfungen, die die Benutzerfreundlichkeit und Barrierefreiheit Ihrer Windows-Anwendungen verbessern können, indem sie Benutzern eine intuitive Möglichkeit bereitstellen, schnell in der sichtbaren UI einer App über eine Tastatur statt per Eingabe mit dem Finger oder der Maus zu navigieren und zu interagieren.

Weitere Details zum Aufrufen gängiger Aktionen in einer Windows-Anwendung mit Tastenkombinationen finden Sie im Thema [Zugriffstasten](keyboard-accelerators.md). 

> [!NOTE]
> Eine Tastatur ist unentbehrlich für Benutzer mit bestimmten körperlichen Einschränkungen (siehe [Barrierefreiheit der Tastaturnavigation](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)) und auch ein wichtiges Tool für Benutzer, die effizienter mit einer App interagieren möchten.

Die Universelle Windows-Plattform (UWP) bietet integrierte Unterstützung für Plattformsteuerelemente für tastaturbasierte Zugriffstasten und zugehöriges UI-Feedback über optische Hinweise namens Zugriffstasteninfos.

## <a name="overview"></a>Übersicht

Eine Zugriffstaste ist eine Kombination aus der Alt-Taste und einer oder mehreren alphanumerischen Tasten (manchmal als *mnemonisches Zeichen* bezeichnet). Die Tasten werden in der Regel nacheinander und nicht gleichzeitig gedrückt.

Zugriffstasteninfos sind Signale, die neben den Steuerelementen angezeigt werden, die Zugriffstasten unterstützen, wenn der Benutzer die Alt-Taste drückt. Jede Zugriffstasteninfo enthält die alphanumerischen Tasten, die das zugeordnete Steuerelement aktivieren.

> [!NOTE]
> Tastenkombinationen werden für Zugriffstasten mit einem einzelnen alphanumerischen Zeichen automatisch unterstützt. Durch gleichzeitiges Drücken von Alt+F in Word wird beispielsweise das Menü „Datei“ ohne Zugriffstasteninfos angezeigt.

Durch Drücken der Alt-Taste wird die Zugrifftastenfunktionalität initialisiert. Zudem werden alle derzeit verfügbaren Tastenkombinationen in Zugriffstasteninfos angezeigt. Nachfolgende Tastaturanschläge werden durch das Zugriffstasten-Framework verarbeitet, das ungültige Tasten ablehnt, bis eine gültige Zugriffstaste oder die Eingabetaste, Esc-Taste, Tabulatortaste oder Pfeiltaste gedrückt wird, um Zugriffstasten zu deaktivieren und die Behandlung von Tastaturanschlägen an die App zurückzugeben.

Microsoft Office-Apps bieten umfassende Unterstützung für Zugriffstasten. Die folgende Abbildung zeigt die Registerkarte „Start“ von Word mit aktivierten Zugriffstasten. (Beachten Sie die Unterstützung für Zahlen und mehrere Tastaturanschläge.)

![Zugriffstasteninfo-Signale für Zugriffstasten in Microsoft Word](images/accesskeys/keytip-badges-word.png)

_KeyTip-Badges für Zugriffstasten in Microsoft Word_

Um eine Zugriffstaste zu einem Steuerelement hinzuzufügen, verwenden Sie die **AccessKey-Eigenschaft**. Der Wert dieser Eigenschaft gibt die Zugriffstastensequenz, die Verknüpfung (bei einem einzelnen alphanumerischen Zeichen) und die Zugriffstasteninfo an.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>Verwenden von Zugriffstasten

Es wird empfohlen, dass Sie Zugriffstasten überall in Ihrer UI angegeben, wo dies sinnvoll ist, und Zugriffstasten für alle benutzerdefinierten Steuerelemente unterstützen.

1.  **Zugriffstasten erleichtern den Zugriff auf Ihre App** für Benutzer mit motorischen Einschränkungen, einschließlich der Benutzer, die jeweils nur eine Taste drücken können oder Probleme bei der Verwendung einer Maus haben.

    Eine gut durchdachte Tastatur-UI ist ein wichtiger Aspekt für die Barrierefreiheit von Software. Sie ermöglicht es Benutzern mit einer Sehbeeinträchtigung oder mit bestimmten motorischen Einschränkungen, in einer App zu navigieren und mit deren Features zu interagieren. Diese Benutzer können u. U. keine Maus bedienen und sind auf verschiedene Hilfstechnologien wie etwa Tastaturerweiterungstools, Bildschirmtastaturen, Bildschirmlupen, Bildschirmleseprogramme oder die Möglichkeit der Spracheingabe angewiesen. Für diese Benutzer ist eine vollständige Befehlsabdeckung entscheidend.

2.  **Zugriffstasten machen Ihre App benutzerfreundlicher** für erfahrene Benutzer, die über die Tastatur interagieren möchten.

    Erfahrene Benutzer haben oftmals eine starke Vorliebe für die Verwendung der Tastatur, da tastaturbasierte Befehle viel schneller eingegeben werden können. Zudem ist es dafür nicht erforderlich, die Hände von der Tastatur wegzubewegen. Für diese Benutzer sind Effizienz und Konsistenz entscheidend. Die Vollständigkeit hingegen ist nur für am häufigsten verwendeten Befehle wichtig.

## <a name="set-access-key-scope"></a>Festlegen des Zugriffstastenbereichs

Wenn viele Elemente auf dem Bildschirm vorhanden sind, die Zugriffstasten unterstützen, empfehlen wir die Bereichsdefinition für Zugriffstasten, um den Benutzer **kognitiv zu entlasten**. Dies reduziert die Anzahl der Zugriffstasten auf dem Bildschirm, wodurch sie leichter zu finden sind, und verbessert die Effizienz und Produktivität.

Microsoft Word bietet beispielsweise zwei Zugriffstastenbereiche: einen primären Bereich für die Menüband-Registerkarten und einen sekundären Bereich für Befehle auf der ausgewählten Registerkarte.

Die folgenden Abbildungen zeigen die zwei Zugriffstastenbereiche in Word. Das erste Beispiel zeigt die primären Zugriffstasten, mit denen ein Benutzer eine Registerkarte und andere Befehle auf oberster Ebene auswählen kann. Das zweite zeigt die sekundären Zugriffstasten für die Registerkarte „Start“.

![Primären Zugriffsschlüssel in Microsoft Word](images/accesskeys/primary-access-keys-word.png)
_primären Zugriffsschlüssel in Microsoft Word_

![Sekundären Zugriffsschlüssel in Microsoft Word](images/accesskeys/secondary-access-keys-word.png)
_sekundären Zugriffsschlüssel in Microsoft Word_

Zugriffstasten können für Elemente in verschiedenen Bereichen dupliziert werden. Im obigen Beispiel ist „2“ die Zugriffstaste für „Rückgängig“ im primären Bereich und „Kursiv“ im sekundären Bereich.

Hier erfahren Sie, wie Sie einen Zugriffstastenbereich definieren.

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

![Primäre Zugriffstasten für Befehlsleiste](images/accesskeys/primary-access-keys-commandbar.png)

_Primäre Bereich CommandBar und unterstützten Zugriffsschlüssel_

![Sekundäre Zugriffstasten für Befehlsleiste](images/accesskeys/secondary-access-keys-commandbar.png)

_Sekundäre Bereich CommandBar und unterstützten Zugriffsschlüssel_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 Creators Update oder älter

Vor dem Windows 10 Fall Creators Update boten einige Steuerelemente wie die Befehlsleiste keine Unterstützung für integrierte Zugriffstastenbereiche.

Im folgenden Beispiel ist gezeigt, wie Sie die sekundären Befehle der Befehlsleiste mit Zugriffstasten unterstützen, die verfügbar sind, sobald ein übergeordneter Befehl aufgerufen wird (vergleichbar mit dem Menüband in Word).

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

## <a name="avoid-access-key-collisions"></a>Vermeiden von Zugriffstastenkonflikten

Zugriffstastenkonflikte treten auf, wenn zwei oder mehr Elemente im selben Bereich duplizierte Zugriffstasten aufweisen oder mit denselben alphanumerischen Zeichen beginnen.

Das System löst duplizierte Zugriffstasten auf, indem die Zugriffstaste des ersten zur visuellen Struktur hinzugefügten Elements verarbeitet wird und alle anderen ignoriert werden.

Wenn mehrere Zugriffstasten mit demselben Zeichen (z. B. „A“, „A1“ und „AB“) beginnen, verarbeitet das System die Zugriffstaste mit einem einzelnen Zeichen und ignoriert alle anderen.

Vermeiden Sie Konflikte durch die Verwendung eindeutiger Zugriffstasten oder die Bereichsdefinition für Befehle.

## <a name="choose-access-keys"></a>Wählen von Zugriffstasten

Berücksichtigen Sie Folgendes, wenn Sie Zugriffstasten wählen:

-   Verwenden Sie ein einzelnes Zeichen zur Minimierung von Tastaturanschlägen und unterstützen Sie standardmäßig Tastenkombinationen (Alt + Zugriffstaste).
-   Vermeiden Sie die Verwendung von mehr als zwei Zeichen.
-   Vermeiden Sie Zugriffstastenkonflikte.
-   Vermeiden Sie Zeichen, die nur schwer von anderen Zeichen zu unterscheiden sind, z. B. den Buchstaben „I“ und die Zahl „1“ oder den Buchstaben „O“ und die Zahl „0“.
-   Verwenden Sie bekannte Vorgänger anderer beliebter Apps wie Word („D“ für „Datei“, „S“ für „Startseite“ usw.).
-   Verwenden Sie das erste Zeichen des Befehlsnamens oder ein Zeichen mit einer engen Assoziation zum Befehl, das man sich gut merken kann.
    -   Wenn der erste Buchstaben bereits zugewiesen ist, verwenden Sie einen Buchstaben, der dem ersten Buchstaben des Befehlsnamens so nah wie möglich ist („I“ für Einfügen).
    -   Verwenden Sie einen markanten Konsonant aus dem Befehlsnamen („X“ für Extras).
    -   Verwenden Sie einen Vokal aus dem Befehlsnamen.

## <a name="localize-access-keys"></a>Lokalisieren von Zugriffstasten

Wenn Ihre App in mehrere Sprachen lokalisiert wird, sollten Sie auch **die Lokalisierung der Zugriffstasten in Betracht ziehen**. Beispielsweise „H“ für „Home“ in en-US und „I“ für „Inicio“ in es-ES.

Verwenden Sie die x:Uid-Erweiterung im Markup, um lokalisierte Ressourcen wie hier gezeigt anzuwenden:

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
Ressourcen für die einzelnen Sprachen werden in entsprechenden Zeichenfolgenordnern im Projekt hinzugefügt:

![Ressourcen-Zeichenfolgenordner für Englisch und Spanisch](images/accesskeys/resource-string-folders.png)

_Englisch und Spanisch Zeichenfolge Ressourcenordner_

Lokalisierte Zugriffstasten werden in der Datei projects resources.resw angegeben:

![Geben Sie die AccessKey-Eigenschaft an, die in der Datei resources.resw angegeben ist.](images/accesskeys/resource-resw-file.png)

_Geben Sie die AccessKey-Eigenschaft, die in der Datei "Resources.resw" angegeben_

Weitere Informationen finden Sie unter [Übersetzen von UI-Ressourcen ](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965329(v=win.10).aspx).

## <a name="key-tip-positioning"></a>Positionieren der Zugriffstasteninfos

Zugriffstasteninfos werden als Gleitkommawertsignale relativ zu ihrem entsprechenden UI-Element angezeigt. Dabei wird das Vorhandensein anderer UI-Elemente, anderer Zugriffstasteninfos und des Bildschirmrands berücksichtigt.

In der Regel ist die Standardposition der Zugriffstasteninfo ausreichend und bietet integrierte Unterstützung für die adaptive UI.

![Beispiel für die automatische Platzierung der Zugriffstasteninfo](images/accesskeys/auto-keytip-position.png)

_Beispiel für die automatische Zugriffstasteninfo-Platzierung_

Falls Sie jedoch mehr Kontrolle über die Positionierung der Zugriffstasteninfo benötigen, empfehlen wir Folgendes:

1.  **Prinzip der offensichtliche Zuordnung**: Der Benutzer kann das Steuerelement Zugriffstasteninfo leicht zuordnen.

    a.  Die Zugriffstasteninfo sollte sich **in der Nähe** des Elements mit der Zugriffstaste befinden (Besitzer).  
    b.  Die Zugriffstasteninfo sollte **aktivierte Elemente nicht verdecken**, die Zugriffstasten aufweisen.   
    c.  Wenn eine Zugriffstasteninfo nicht in der Nähe des Besitzers platziert werden kann, sollte sie mit dem Besitzer überlappen. 

2.  **Erkennbarkeit**: Der Benutzer kann das Steuerelement mit Zugriffstasteninfo schnell ermitteln.

    a.  Die Zugriffstasteninfo sollte andere Zugriffstasteninfos nie **überlappen**.  

3.  **Einfache Überprüfung:** Der Benutzer kann die Zugriffstasteninfos einfach zu erfassen.

    a.  Zugriffstasteninfos sollten aneinander und am UI-Element **ausgerichtet** sein.
    b.  Zugriffstasteninfos sollten so weitgehend wie möglich **gruppiert** sein. 

### <a name="relative-position"></a>Relative Position

Verwenden Sie die Eigenschaft **KeyTipPlacementMode**, um die Platzierung der Zugriffstasteninfo auf Element- oder Gruppenbasis anzupassen.

Die Platzierungsmodi sind: Oben, unten, rechts, links, ausgeblendet, Center und automatisch.

![Platzierungsmodi für Zugriffstasteninfo](images/accesskeys/keytip-postion-modes.png)

_Modi der Zugriffstasteninfos-Platzierung_

Die Mittellinie des Steuerelements wird verwendet, um die vertikale und horizontale Ausrichtung der Zugriffstasteninfo zu berechnen.

Das folgende Beispiel zeigt, wie Sie die Platzierung der Zugriffstasteninfo einer Gruppe von Steuerelementen mit der KeyTipPlacementMode-Eigenschaft eines StackPanel-Containers festlegen.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Offsets

Verwenden Sie die Eigenschaften KeyTipHorizontalOffset und KeyTipVerticalOffset eines Elements für eine noch genauere Kontrolle der Position der Zugriffstasteninfo.

> [!NOTE]
> Offsets können nicht festgelegt werden, wenn KeyTipPlacementMode auf Auto festgelegt ist.

Die KeyTipHorizontalOffset-Eigenschaft gibt an, wie weit die Zugriffstasteninfo nach links oder rechts verschoben werden soll. Das Beispiel veranschaulicht, wie die Offsets der Zugriffstasteninfo für eine Schaltfläche festgelegt werden.

![Platzierungsmodi für Zugriffstasteninfo](images/accesskeys/keytip-offsets.png)

_Festlegen der vertikalen und horizontalen Offsets für eine Zugriffstasteninfo_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Ausrichtung des Bildschirmrands {#screen-edge-alignment .ListParagraph}

Die Position einer Zugriffstasteninfo wird automatisch am Bildschirmrand ausgerichtet, um sicherzustellen, dass die Zugriffstasteninfo vollständig sichtbar ist. In diesem Fall kann der Abstand zwischen dem Steuerelement und dem Zugriffstasten-Ausrichtungspunkt von den Werten für die horizontalen und vertikalen Offsets abweichen.

![Platzierungsmodi für Zugriffstasteninfo](images/accesskeys/keytips-screen-edge.png)

_Am Bildschirmrand führt dazu, dass es sich bei Zugriffstasteninfo zu selbst automatisch neu_

## <a name="key-tip-style"></a>Zugriffstasteninfoformat

Wir empfehlen die Verwendung der integrierten Unterstützung für Zugriffstasteninfos für Plattformdesigns, einschließlich hoher Kontrast.

Wenn Sie Ihre eigenen Formate für Zugriffstasteninfos angeben müssen, verwenden Sie Anwendungsressourcen wie KeyTipFontSize (Schriftgröße), KeyTipFontFamily (Schriftfamilie), KeyTipBackground (Hintergrund), KeyTipForeground (Vordergrund), KeyTipPadding (Abstand), KeyTipBorderBrush (Rahmenfarbe) und KeyTipBorderThemeThickness (Rahmendicke).

![Platzierungsmodi für Zugriffstasteninfo](images/accesskeys/keytip-customization.png)

_Optionen für die Anpassung von Zugriffstasteninfos_

In diesem Beispiel wird veranschaulicht, wie Sie diese Anwendungsressourcen ändern:

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

Das XAML-Framework stellt Automatisierungseigenschaften bereit, mit denen UI-Automatisierungsclients Informationen zu Elementen in der Benutzeroberfläche ermitteln können.

Wenn Sie die AccessKey-Eigenschaft für ein UIElement- oder TextElement-Steuerelement angeben, können Sie mithilfe der [AutomationProperties.AccessKey](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.accesskey?view=netframework-4.8)-Eigenschaft diesen Wert abrufen. Barrierefreiheitclients, z. B. die Sprachausgabe, lesen den Wert dieser Eigenschaft jedes Mal, wenn ein Element den Fokus erhält.

## <a name="related-articles"></a>Verwandte Artikel

* [Tastenkombinationen](keyboard-interactions.md)
* [Zugriffstasten](keyboard-accelerators.md)

**Beispiele**
* [XAML-Steuerelementsammlungen (auch bekannt als XamlUiBasics)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


