---
description: In diesem Artikel werden Sie schrittweise durch den Vorgang zum Erstellen eines XAML-Steuerelements mit Vorlagen für WinUI 3 mit C# geführt.
title: XAML-Steuerelemente in Vorlagen für WinUI 3-Apps mit C#
ms.date: 03/05/2021
ms.topic: article
keywords: Windows 10, UWP, benutzerdefiniertes Steuerelement, Steuerelement mit Vorlagen, WinUI
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: b0616219b1950d476a1b5dd281281399196aad3e
ms.sourcegitcommit: 4b4ace01ab130f0f9784fa0be0dcce28bdfc422f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/09/2021
ms.locfileid: "107265705"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-c"></a>XAML-Steuerelemente in Vorlagen für WinUI 3-Apps mit C#

In diesem Artikel werden Sie schrittweise durch das Erstellen eines XAML-Steuerelements mit Vorlagen für WinUI 3 mit C# geführt. Steuerelemente mit Vorlagen erben von der **Microsoft.UI.Xaml.Controls.Control**-Klasse und besitzen visuelle Struktur sowie visuelles Verhalten, die mithilfe von XAML-Steuerelementvorlagen angepasst werden können.

Bevor Sie die Schritte in diesem Artikel ausführen, sollten Sie sicherstellen, dass Ihre Entwicklungsumgebung für die Erstellung von WinUI 3-Apps konfiguriert ist. Informationen zum Setup finden Sie unter [Erste Schritte mit WinUI 3 für Desktop-Apps](./get-started-winui3-for-desktop.md).

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Erstellen einer leeren App (BgLabelControlApp)

Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Wählen Sie im Dialogfeld **Neues Projekt erstellen** die Projektvorlage **Leere App (WinUI in UWP)** aus, und vergewissern Sie sich, dass die Sprachversion C# ausgewählt ist. Legen Sie den Projektnamen auf „BgLabelControlApp“ fest, damit die Dateinamen denjenigen im Code in den folgenden Beispielen entsprechen. Legen Sie die **Zielversion** auf Windows 10, Version 1903 (Build 18362), und die **Mindestversion** auf Windows 10, Version 1803 (Build 17134), fest. Diese exemplarische Vorgehensweise funktioniert auch für Desktop-Apps, die mit der Projektvorlage **Leere App, gepackt (WinUI in Desktop)** erstellt wurden. Stellen Sie lediglich sicher, dass Sie alle Schritte im Projekt **BgLabelControlApp (Desktop)** ausführen.

![Projektvorlage „Leere App“](images/winui-csharp-new-project-uwp.png)

## <a name="add-a-templated-control-to-your-app"></a>Hinzufügen eines vordefinierten Steuerelements zu Ihrer App

Um ein vordefiniertes Steuerelement hinzuzufügen, klicken Sie auf das Menü **Projekt** in der Symbolleiste, oder klicken Sie mit der rechten Maustaste auf Ihr Projekt im **Projektmappen-Explorer**, und wählen Sie **Neues Element hinzufügen** aus. Wählen Sie unter **Visual C#->WinUI** die Vorlage **Benutzerdefiniertes Steuerelement (WinUI)** aus. Benennen Sie das neue Steuerelement „BgLabelControl“, und klicken Sie auf *Hinzufügen*. 

## <a name="update-the-custom-control-c-file"></a>Aktualisieren der C#-Datei des benutzerdefinierten Steuerelements

Beachten Sie, dass der Konstruktor in der C#-Datei „BgLabelControl.cs“ die Eigenschaft **DefaultStyleKey** für unser Steuerelement definiert. Dieser Schlüssel identifiziert die Standardvorlage, die verwendet wird, wenn der Consumer des Steuerelements nicht ausdrücklich eine Vorlage angibt. Der Schlüsselwert ist der *Typ* des Steuerelements. Dieser Schlüssel wird später bei der Implementierung unserer generischen Vorlagendatei verwendet.

```csharp
public BgLabelControl()
{
    this.DefaultStyleKey = typeof(BgLabelControl);
}
```

Unser Steuerelement mit Vorlagen wird über eine Beschriftung verfügen, die programmseitig im Code, in XAML oder über Datenbindung festgelegt werden kann. Damit das System den Text der Beschriftung unseres Steuerelements auf dem neuesten Stand hält, muss sie als [DependencyPropety](/uwp/api/Windows.UI.Xaml.DependencyProperty) implementiert werden. Dazu deklarieren wir zunächst eine Zeichenfolgeneigenschaft und nennen diese **Label**. Anstatt eine dahinter liegende Variable zu verwenden, wird der Wert der Abhängigkeitseigenschaft durch Aufrufen von [GetValue](/uwp/api/windows.ui.xaml.dependencyobject.getvalue) und [SetValue](/uwp/api/windows.ui.xaml.dependencyobject.setvalue) abgerufen bzw. festgelegt. Diese Methoden werden vom [DependencyObject-](/uwp/api/windows.ui.xaml.dependencyobject) bereitgestellt, das von **Microsoft.UI.Xaml.Controls.Control** erbt.

```csharp
public string Label
{
    get => (string)GetValue(LabelProperty);
    set => SetValue(LabelProperty, value);
}
```
Als nächstes deklarieren Sie die Abhängigkeitseigenschaft und registrieren sie im System durch Aufrufen von [DependencyProperty.Register](/uwp/api/windows.ui.xaml.dependencyproperty.register). Diese Methode legt den Namen und Typ unserer **Label**-Eigenschaft, den Typ des Besitzers der Eigenschaft, unsere **BgLabelControl**-Klasse und den Standardwert für die Eigenschaft fest.

```csharp
DependencyProperty LabelProperty = DependencyProperty.Register(
    nameof(Label), 
    typeof(string),
    typeof(BgLabelControl), 
    new PropertyMetadata(default(string), new PropertyChangedCallback(OnLabelChanged)));
```

Diese beiden Schritte sind alles, was erforderlich ist, um eine Abhängigkeitseigenschaft zu implementieren, aber im Rahmen dieses Beispiels fügen wir einen optionalen Handler für das Ereignis **OnLabelChanged** hinzu. Dieses Ereignis wird vom System immer dann ausgelöst, wenn der Eigenschaftswert aktualisiert wird. In diesem Fall überprüfen wir, ob die neue Beschriftung eine leere Zeichenfolge ist, und aktualisieren eine Klassenvariable entsprechend.

```csharp
public bool HasLabelValue { get; set; }

private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e)
{
    BgLabelControl labelControl = d as BgLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        labelControl.HasLabelValue = false;
    }
    else
    {
        labelControl.HasLabelValue = true;
    }
}
```
Weitere Informationen zur Funktionsweise von Abhängigkeitseigenschaften finden Sie unter [Übersicht über Abhängigkeitseigenschaften](/windows/uwp/xaml-platform/dependency-properties-overview).

## <a name="define-the-default-style-for-bglabelcontrol"></a>Definieren des Standardstils für „BgLabelControl“
Ein Steuerelement mit Vorlagen muss eine Standardstilvorlage bereitstellen, die verwendet wird, wenn der Benutzer des Steuerelements nicht explizit einen Stil festlegt. In diesem Schritt wird die generische Vorlagendatei für das Steuerelement geändert.

Die generische Vorlagendatei wird generiert, wenn Sie Ihrer APP das **benutzerdefinierte Steuerelement (WinUI)** hinzufügen. Die Datei heißt „Generic.xaml“ und wird im Projektmappen-Explorer im Ordner **Designs** generiert. Der Ordner- und der Dateiname werden benötigt, damit das XAML-Framework den Standardstil für ein Steuerelement mit Vorlagen findet. Löschen Sie den Standardinhalt von „Generic.xaml“, und fügen Sie das folgende Markup ein.



```xaml
<!-- \Themes\Generic.xaml -->
<ResourceDictionary
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:BgLabelControlApp">

    <Style TargetType="local:BgLabelControl" >
        <Setter Property="Template">
            <Setter.Value>
                <ControlTemplate TargetType="local:BgLabelControl">
                    <Grid Width="100" Height="100" Background="{TemplateBinding Background}">
                        <TextBlock HorizontalAlignment="Center" VerticalAlignment="Center" Text="{TemplateBinding Label}"/>
                    </Grid>
                </ControlTemplate>
            </Setter.Value>
        </Setter>
    </Style>
</ResourceDictionary>
```

In diesem Beispiel sehen Sie, dass das **TargetType**-Attribut des **Style**-Elements auf unseren **BgLabelControl**-Typ innerhalb des **BgLabelControlApp**-Namespace festgelegt ist. Dieser Typ ist derselbe Wert, den wir oben für die **DefaultStyleKey**-Eigenschaft im Konstruktor des Steuerelements angegeben haben, der dies als Standardstil für das Steuerelement identifiziert.

Die **Text**-Eigenschaft des **TextBlock**-Objekts in der Steuerelementvorlage wird an die **Label**-Eigenschaft des Steuerelements gebunden. Die Eigenschaft wird mit der Markuperweiterung [TemplateBinding](/windows/uwp/xaml-platform/templatebinding-markup-extension) gebunden. Dieses Beispiel bindet auch den **Grid**-Hintergrund an die **Background**-Abhängigkeitseigenschaft, die von der Klasse **Control** geerbt wird.

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Hinzufügen einer Instanz von „BgLabelControl“ zur Hauptseite der Benutzeroberfläche

Öffne `MainPage.xaml`. Darin befindet sich das XAML-Markup für unsere UI-Hauptseite. Füge direkt nach dem Element **Button** (innerhalb von **StackPanel**) das folgende Markup hinzu:

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Wenn Sie die Anwendung erstellen und ausführen, wird das Steuerelement mit Vorlagen mit der Hintergrundfarbe und Beschriftung angezeigt, die Sie festgelegt haben.

![Steuerelement mit Vorlagen, Ergebnis](images/winui-templated-control-result.png)


