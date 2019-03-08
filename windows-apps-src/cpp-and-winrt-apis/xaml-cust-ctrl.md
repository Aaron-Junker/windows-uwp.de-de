---
description: Dieses Thema führt Sie durch die Schritte zum Erstellen eines einfachen benutzerdefinierten Steuerelements mit der Verwendung von C++ / WinRT. Sie können auf die Informationen zum Erstellen eigener funktionsreiche und anpassbare Benutzeroberflächen-Steuerelemente erstellen.
title: Benutzerdefinierte (vorlagenbasierte) XAML-Steuerelemente mit C++ / WinRT
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, Uwp, Standard, c++, Cpp, Winrt, Projektion, XAML, benutzerdefinierte, auf Vorlagen basierenden Steuerelements
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ce4f7eea074233c625a2cc92ef773f0b06c2be9f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635145"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Benutzerdefinierte (vorlagenbasierte) XAML-Steuerelemente mit C++ / WinRT

> [!IMPORTANT]
> Für die wesentlichen Konzepte und Begriffe, die Ihre Kenntnisse nutzen, und Erstellen von Common Language Runtime-Klassen mit unterstützen [C++ / WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt), finden Sie unter [Nutzen von APIs mit C++ / WinRT](consume-apis.md) und [Autor-APIs mit C++ / WinRT](author-apis.md).

Eines der leistungsstärksten Features von der universellen Windows-Plattform (UWP) wird die Flexibilität, die der Stapel der Benutzeroberfläche (UI) zum Erstellen von benutzerdefinierter Steuerelementen, die basierend auf der XAML bietet [ **Steuerelement** ](/uwp/api/windows.ui.xaml.controls.control) Typ. Das Framework für XAML-UI stellt Funktionen bereit, z. B. [benutzerdefinierten Abhängigkeitseigenschaften](/windows/uwp/xaml-platform/custom-dependency-properties) und [angefügte Eigenschaften](/windows/uwp/xaml-platform/custom-attached-properties), und [Steuerelementvorlagen](/windows/uwp/design/controls-and-patterns/control-templates), die erleichtern Ihnen die Erstellung funktionsreiche und anpassbaren Steuerelemente. Dieses Thema führt Sie durch die Schritte zum Erstellen einer benutzerdefinierten (Steuerelementvorlage) mit C++ / WinRT.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Erstellen Sie eine leere App (BgLabelControlApp)
Erstellen Sie zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie eine **Visual C++** > **Windows Universal** > **leere App (C++ / WinRT)** Projekt, und nennen Sie sie *BgLabelControlApp* . In einem späteren Abschnitt dieses Themas, werden Sie zum Erstellen Ihres Projekts weitergeleitet werden (nicht bis dahin erstellen).

Wir werden eine neue Klasse zum Darstellen eines benutzerdefinierten (Vorlagen) zu erstellen. Wir erstellen und nutzen die Klasse innerhalb derselben Kompilierungseinheit. Aber wir in der Lage, instanziiert diese Klasse aus dem XAML-Markup, und für, der Grund, dass es ein einer Runtime-Klasse sein. Und wir werden C++/WinRT verwenden, um diese zu schreiben und zu nutzen.

Der erste Schritt beim Erstellen einer neuen Laufzeitklasse besteht darin, dem Projekt ein neues **Midl-Datei (.idl)**-Element hinzuzufügen. Nennen Sie es `BgLabelControl.idl`. Löschen Sie den Standardinhalt von `BgLabelControl.idl` und fügen Sie diese Laufzeitklassendeklaration ein.

```idl
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Windows.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

Die oben stehenden Codebeispiel zeigt das Muster, das Sie ausführen, wenn Sie eine Abhängigkeitseigenschaft (DP) deklarieren. Es gibt zwei Teile auf jedes DP. Zuerst deklarieren Sie eine schreibgeschützte statische Eigenschaft vom Typ [ **DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty). Es hat den Namen Ihrer DP plus *Eigenschaft*. Sie verwenden diese statische Eigenschaft in Ihrer Implementierung. Andererseits können Sie eine Lese-/ Schreibzugriff-Eigenschaft mit dem Typ und Namen von Ihrem DP deklarieren. Wenn Sie, erstellen möchten eine *angefügte Eigenschaft* (anstatt einer DP), klicken Sie dann finden Sie in den Codebeispielen in [benutzerdefinierte angefügte Eigenschaften](/windows/uwp/xaml-platform/custom-attached-properties).

> [!NOTE]
> Wenn Sie einen Verteilungspunkt mit einem Gleitkommatyp möchten, legen Sie es `double` (`Double` in [MIDL 3.0](/uwp/midl-3/)). Deklarieren und Implementieren von einem DP des Typs `float` (`Single` in "MIDL"), und klicken Sie dann eine Einstellung für diese DP im XAML-Markup führt zu dem Fehler *Fehler beim Erstellen einer "Windows.Foundation.Single" aus dem Text "<NUMBER>"*.

Speichern Sie die Datei, und erstellen Sie das Projekt. Während des Buildprozesses wird das `midl.exe`-Tool ausgeführt, um eine Windows-Runtime-Metadaten-Datei (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) zu erstellen, die die Laufzeitklasse beschreibt. Dann wird das `cppwinrt.exe`-Tool ausgeführt, um Quellcodedateien zu erzeugen, die Sie bei der Erstellung und Nutzung Ihrer Laufzeitklasse unterstützen. Diese Dateien umfassen Stubs, um Ihnen den Einstieg implementieren die **BgLabelControl** -Runtime-Klasse, die Sie in der IDL-Datei deklariert. Diese Stubs sind `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` und `BgLabelControl.cpp`.

Kopieren Sie die Stub-Dateien `BgLabelControl.h` und `BgLabelControl.cpp` von `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` in den Projektordner `\BgLabelControlApp\BgLabelControlApp\`. Vergewissern Sie sich im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicken Sie mit der rechten Maustaste auf die kopierten Stub-Dateien und klicken Sie auf **In Projekt aufnehmen**.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementieren der **BgLabelControl** benutzerdefinierten Steuerelement-Klasse
Nun öffnen wir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` und `BgLabelControl.cpp` und implementieren unsere Laufzeitklasse. In `BgLabelControl.h`, ändern Sie den Konstruktor, um die Schlüssel, Stil-Standardimplementierung festzulegen **Bezeichnung** und **LabelProperty**, fügen Sie einen statische Ereignishandler mit dem Namen **OnLabelChanged** auf Verarbeiten von Änderungen des Werts der Abhängigkeitseigenschaft, und fügen Sie einen privaten Member zum Speichern der dahinter liegende Feld für **LabelProperty**.

Nachdem Sie hinzugefügt haben, Ihre `BgLabelControl.h` sieht wie folgt aus.

```cppwinrt
// BgLabelControl.h
...
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
    BgLabelControl() { DefaultStyleKey(winrt::box_value(L"BgLabelControlApp.BgLabelControl")); }

    winrt::hstring Label()
    {
        return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
    }

    void Label(winrt::hstring const& value)
    {
        SetValue(m_labelProperty, winrt::box_value(value));
    }

    static Windows::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

    static void OnLabelChanged(Windows::UI::Xaml::DependencyObject const&, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const&);

private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
};
...
```

In `BgLabelControl.cpp`, definieren Sie die statischen Member wie folgt.

```cppwinrt
// BgLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
);

void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // Call members of the projected type via theControl.

        BgLabelControlApp::implementation::BgLabelControl* ptr{ winrt::get_self<BgLabelControlApp::implementation::BgLabelControl>(theControl) };
        // Call members of the implementation type via ptr.
    }
}
...
```

In dieser exemplarischen Vorgehensweise, wir verwenden werden nicht **OnLabelChanged**. Es ist jedoch vorhanden, damit Sie zum Registrieren einer Abhängigkeitseigenschaft mit einem Rückruf durch geänderte Eigenschaften ausgelöste sehen können. Die Implementierung der **OnLabelChanged** zeigt außerdem, wie einen abgeleiteten projizierten Typ aus einer Basisklasse projizierten abgerufen (ist der Basistyp für die voraussichtliche **DependencyObject**, in diesem Fall). Und es zeigt, wie einen Zeiger auf den Typ klicken Sie dann zu erhalten, die der projizierten Typ implementiert. Der zweiter Vorgang auf natürliche Weise nur im Projekt möglich sein wird, implementiert der projizierten Typ (d. h. das Projekt, die die Common Language Runtime-Klasse implementiert).

> [!NOTE]
> Wenn die Windows-SDK-Version 10.0.17763.0 (Windows 10, Version 1809) nicht installiert haben, oder höher verwenden, Sie zum Aufrufen müssen [ **winrt::from_abi** ](/uwp/cpp-ref-for-winrt/from-abi) in der oben genannten-Abhängigkeit changed-Ereignishandler anstelle von [ **winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self).

## <a name="design-the-default-style-for-bglabelcontrol"></a>Entwerfen Sie den Standardstil für **BgLabelControl**

In seinem Konstruktor **BgLabelControl** legt einen Stil Standardschlüssel für sich selbst. Aber was *ist* einen Standardstil? Ein benutzerdefiniertes Steuerelement für die (auf Vorlagen basierende) muss einen Standardstil&mdash;, enthält eine Standardvorlage für das Steuerelement&mdash;verwendet können, um sich selbst mit zu rendern, für den Fall, dass der Consumer des Steuerelements keinen Stil bzw. Vorlage festgelegt. In diesem Abschnitt fügen wir eine Markupdatei auf das Projekt mit unserer Standardstil.

Klicken Sie unter den Projektknoten erstellen Sie einen neuen Ordner, und nennen sie "Designs". Klicken Sie unter `Themes`, fügen Sie ein neues Element vom Typ **Visual C++** > **XAML** > **XAML-Ansicht**, und nennen Sie es mit "Generic.xaml". Die Ordner und Dateinamen müssen wie folgt in der Reihenfolge für die XAML-Framework, um den Standardstil für ein benutzerdefiniertes Steuerelement zu finden sein. Löschen Sie den Standardinhalt der `Generic.xaml`, und fügen Sie in das unten stehende Markup.

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

In diesem Fall ist die einzige Eigenschaft, die der Standardstil festlegt, die Steuerelementvorlage. Die Vorlage besteht aus einem Quadrat (an, dessen Hintergrund gebunden ist die **Hintergrund** Eigenschaft, die alle Instanzen von der XAML [ **Steuerelement** ](/uwp/api/windows.ui.xaml.controls.control) Typ), und ein Textelement (, deren Text gebunden ist, um die **BgLabelControl::Label** Abhängigkeitseigenschaft).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Fügen Sie eine Instanz des **BgLabelControl** zur UI-Hauptseite

Öffnen Sie `MainPage.xaml` mit dem XAML-Markup für unsere UI-Hauptseite. Unmittelbar nach der **Schaltfläche** Element (innerhalb der **StackPanel**), fügen Sie das folgende Markup hinzu.

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Fügen Sie außerdem hinzu, die folgenden include-Anweisung, um `MainPage.h` , damit die **"MainPage"** Typ (eine Kombination der Kompilierung von XAML-Markup und imperativem Code) beachtet die **BgLabelControl** benutzerdefinierte Steuerelementtyp. Wenn Sie verwenden möchten **BgLabelControl** aus einer anderen XAML-Seite, fügen Sie dann diese gleiche #include-Anweisung der Headerdatei für diese Seite zu. Oder Sie können auch einfach eine einzelne include-Anweisung in der vorkompilierten Headerdatei.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Erstellen Sie nun das Projekt und führen Sie es aus. Sehen Sie, dass die standardmäßige Steuerelementvorlage der Hintergrundpinsel und die Bezeichnung der Bindung ist die **BgLabelControl** Instanz im Markup.

In dieser exemplarischen Vorgehensweise wurde ein einfaches Beispiel einer benutzerdefinierten (Steuerelementvorlage) in C++ / WinRT. Sie können eigene benutzerdefinierte Steuerelemente beliebig umfangreiche, voll funktionsfähige vornehmen. Beispielsweise kann ein benutzerdefiniertes Steuerelement Form von etwas so kompliziert, als ein bearbeitbares Datenraster, ein video Player oder ein Visualisierungstool 3D-Geometrie dauern.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementieren von *überschreibbare* Funktionen wie **MeasureOverride** und **OnApplyTemplate**

Sie leiten Sie ein benutzerdefiniertes Steuerelement aus der [ **Steuerelement** ](/uwp/api/windows.ui.xaml.controls.control) Common Language Runtime-Klasse, die selbst Weitere wird von Basis-Runtime-Klassen abgeleitet. Und es gibt überschreibbare Methoden **Steuerelement**, [ **"FrameworkElement"**](/uwp/api/windows.ui.xaml.frameworkelement), und [ **"UIElement"** ](/uwp/api/windows.ui.xaml.uielement) dass Sie in der abgeleiteten Klasse überschreiben können. Hier ist ein Codebeispiel, das veranschaulicht, wie dies aus.

```cppwinrt
struct BgLabelControl : BgLabelControlT<BgLabelControl>
{
...
    // Control overrides.
    void OnPointerPressed(Windows::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

    // FrameworkElement overrides.
    Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
    void OnApplyTemplate() const { ... };

    // UIElement overrides.
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
...
};
```

*Überschreibbare* Funktionen bieten sich anders als in anderen sprachprojektionen. In C#, z. B. überschreibbare-Funktionen in der Regel als geschützte virtuelle Funktionen angezeigt werden. In C++ / WinRT, sind weder virtuell noch geschützten, Sie können jedoch überschrieben werden und Ihre eigene Implementierung, bereitstellen, wie oben gezeigt.

## <a name="important-apis"></a>Wichtige APIs
* [Control-Klasse](/uwp/api/windows.ui.xaml.controls.control)
* [DependencyProperty-Klasse](/uwp/api/windows.ui.xaml.dependencyproperty)
* [FrameworkElement-Klasse](/uwp/api/windows.ui.xaml.frameworkelement)
* [UIElement-Klasse](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Verwandte Themen
* [Steuerelementvorlagen](/windows/uwp/design/controls-and-patterns/control-templates)
* [Benutzerdefinierte Abhängigkeitseigenschaften](/windows/uwp/xaml-platform/custom-dependency-properties)
