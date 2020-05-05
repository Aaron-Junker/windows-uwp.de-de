---
description: In diesem Thema werden die Schritte zum Erstellen eines einfachen benutzerdefinierten Steuerelements mit C++/WinRT beschrieben. Sie können diese Informationen nutzen, um Ihre eigenen Benutzeroberflächen-Steuerelemente mit vielen Funktionen und Anpassungsmöglichkeiten zu erstellen.
title: Benutzerdefinierte (vorlagenbasierte) XAML-Steuerelemente mit C++/WinRT
ms.date: 04/24/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projektion, XAML, benutzerdefiniert, vorlagenbasiert, Steuerelement
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a6cde5a62367dccd83ca8dc6a46c203587850422
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "80760522"
---
# <a name="xaml-custom-templated-controls-with-cwinrt"></a>Benutzerdefinierte (vorlagenbasierte) XAML-Steuerelemente mit C++/WinRT

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe im Zusammenhang mit der Nutzung und Erstellung von Laufzeitklassen mit [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) findest du unter [Verwenden von APIs mit C++/WinRT](consume-apis.md) sowie unter [Erstellen von APIs mit C++/WinRT](author-apis.md).

Eines der leistungsstärksten Features der universellen Windows-Plattform (UWP) ist die Flexibilität des Benutzeroberflächenstapels hinsichtlich der Erstellung benutzerdefinierter Steuerelemente auf der Grundlage des XAML-Typs [**Control**](/uwp/api/windows.ui.xaml.controls.control). Das XAML-Benutzeroberflächenframework bietet Features wie [benutzerdefinierte Abhängigkeitseigenschaften](/windows/uwp/xaml-platform/custom-dependency-properties) und [angefügte Eigenschaften](/windows/uwp/xaml-platform/custom-attached-properties) sowie [Steuerelementvorlagen](/windows/uwp/design/controls-and-patterns/control-templates) zur mühelosen Erstellung vielseitiger und anpassbarer Steuerelemente. In diesem Thema werden die Schritte zum Erstellen eines benutzerdefinierten (vorlagenbasierten) Steuerelements mit C++/WinRT beschrieben.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Erstellen einer leeren App (BgLabelControlApp)
Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Erstelle ein **Leere App (C++/WinRT)** -Projekt, lege dessen Name auf *BgLabelControlApp* fest, und stelle sicher, dass **Legen Sie die Projektmappe und das Projekt im selben Verzeichnis ab** deaktiviert ist (damit deine Ordnerstruktur mit der exemplarischen Vorgehensweise übereinstimmt).

Führe den Buildvorgang für dein Projekt erst aus, wenn du weiter unten in diesem Thema dazu aufgefordert wirst.

> [!NOTE]
> Informationen zum Einrichten von Visual Studio für die C++/WinRT-Entwicklung&mdash;einschließlich Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen)&mdash; finden Sie unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Wir erstellen eine neue Klasse, um ein benutzerdefiniertes (vorlagenbasiertes) Steuerelement darzustellen. Die Klasse wird innerhalb der gleichen Kompilierungseinheit erstellt und genutzt. Da diese Klasse jedoch per XAML-Markup instanziierbar sein soll, verwenden wir eine Laufzeitklasse. Und wir verwenden C++/WinRT, um sie zu schreiben und zu nutzen.

Um eine neue Laufzeitklasse zu erstellen, müssen wir dem Projekt zunächst ein neues Element vom Typ **Midl-Datei (.idl)** hinzufügen. Nenne es `BgLabelControl.idl`. Lösche den Standardinhalt von `BgLabelControl.idl`, und füge die folgende Laufzeitklassendeklaration ein:

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

Das obige Listing veranschaulicht das Muster zum Deklarieren einer Abhängigkeitseigenschaft (Dependency Property, DP). Eine Abhängigkeitseigenschaft besteht aus zwei Teilen: Zuerst wird eine schreibgeschützte statische Eigenschaft vom Typ [**DependencyProperty**](/uwp/api/windows.ui.xaml.dependencyproperty) deklariert. Sie hat den Namen deiner Abhängigkeitseigenschaft plus *Property*. Diese statische Eigenschaft wird in deiner Implementierung verwendet. Danach wird eine Instanzeigenschaft mit Lese-/Schreibzugriff sowie mit dem Typ und dem Namen deiner Abhängigkeitseigenschaft deklariert. Wenn du anstelle einer Abhängigkeitseigenschaft eine *angefügte Eigenschaft* erstellen möchtest, sieh dir die Codebeispiele unter [Benutzerdefinierte angefügte Eigenschaften](/windows/uwp/xaml-platform/custom-attached-properties) an.

> [!NOTE]
> Wenn du eine Abhängigkeitseigenschaft mit einem Gleitkommatyp erstellen möchtest, lege sie auf `double` (`Double` in [MIDL 3.0](/uwp/midl-3/)) fest. Wenn du eine Abhängigkeitseigenschaft vom Typ `float` (`Single` in MIDL) deklarierst und implementierst und anschließend im XAML-Markup einen Wert für diese Abhängigkeitseigenschaft festlegst, tritt der folgende Fehler auf: *Fehler beim Erstellen von "Windows.Foundation.Single" aus dem Text "<NUMBER>".* .

Speichere die Datei, und erstelle das Projekt. Während des Buildprozesses wird das Tool `midl.exe` ausgeführt, um eine Windows-Runtime-Metadatendatei (`\BgLabelControlApp\Debug\BgLabelControlApp\Unmerged\BgLabelControl.winmd`) zu erstellen, die die Laufzeitklasse beschreibt. Danach wird das Tool `cppwinrt.exe` ausgeführt, um Quellcodedateien zu generieren, die dich bei der Erstellung und Nutzung deiner Laufzeitklasse unterstützen. Diese Dateien enthalten Stubs zur Implementierung der Laufzeitklasse **BgLabelControl**, die du in deiner IDL deklariert hast. Diese Stubs sind `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\BgLabelControl.h` und `BgLabelControl.cpp`.

Kopiere die Stub-Dateien `BgLabelControl.h` und `BgLabelControl.cpp` aus `\BgLabelControlApp\BgLabelControlApp\Generated Files\sources\` in den Projektordner `\BgLabelControlApp\BgLabelControlApp\`. Vergewissern Sie sich im **Projektmappen-Explorer**, dass **Alle Dateien anzeigen** aktiviert ist. Klicke mit der rechten Maustaste auf die kopierten Stub-Dateien, und klicke auf **Zu Projekt hinzufügen**.

Du siehst eine `static_assert`-Deklaration am Anfang von `BgLabelControl.h` und `BgLabelControl.cpp`, die du entfernen musst, bevor das Projekt erstellt wird.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementieren der benutzerdefinierten Steuerelementklasse **BgLabelControl**
Als Nächstes öffnen wir `\BgLabelControlApp\BgLabelControlApp\BgLabelControl.h` und `BgLabelControl.cpp` und implementieren unsere Laufzeitklasse. Ändere in `BgLabelControl.h` den Konstruktor, um den Standarddesignschlüssel festzulegen, implementiere **Label** und **LabelProperty**, füge einen statischen Ereignishandler namens **OnLabelChanged** für die Verarbeitung von Wertänderungen der Abhängigkeitseigenschaft hinzu, und füge einen privaten Member zum Speichern des Unterstützungsfelds für **LabelProperty** hinzu.

Danach sieht `BgLabelControl.h` wie folgt aus: Du kannst diese Codezeile kopieren und einfügen, um den Inhalt von `BgLabelControl.h` zu ersetzen.

```cppwinrt
// BgLabelControl.h
#pragma once
#include "BgLabelControl.g.h"

namespace winrt::BgLabelControlApp::implementation
{
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
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

Definiere in `BgLabelControl.cpp` die statischen Member: Du kannst diese Codezeile kopieren und einfügen, um den Inhalt von `BgLabelControl.cpp` zu ersetzen.

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#include "BgLabelControl.g.cpp"

namespace winrt::BgLabelControlApp::implementation
{
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
}
```

In dieser exemplarischen Vorgehensweise wird **OnLabelChanged** nicht verwendet. Es ist jedoch vorhanden, um zu zeigen, wie du eine Abhängigkeitseigenschaft mit einem PropertyChanged-Rückruf registrierst. Die Implementierung von **OnLabelChanged** zeigt auch, wie du einen abgeleiteten projizierten Typ eines projizierten Basistyps (in diesem Fall: **DependencyObject**) erhältst. Darüber hinaus wird gezeigt, wie du anschließend einen Zeiger auf den Typ erhältst, der den projizierten Typ implementiert. Dieser zweite Vorgang ist natürlich nur in dem Projekt möglich, das den projizierten Typ implementiert (also in dem Projekt, das die Laufzeitklasse implementiert).

> [!NOTE]
> Falls nicht mindestens die Windows SDK-Version 10.0.17763.0 (Windows 10, Version 1809) installiert ist, muss im obigen Ereignishandler für die Änderung der Abhängigkeitseigenschaft [**winrt::from_abi**](/uwp/cpp-ref-for-winrt/from-abi) (anstelle von [**winrt::get_self**](/uwp/cpp-ref-for-winrt/get-self)) aufgerufen werden.

## <a name="design-the-default-style-for-bglabelcontrol"></a>Entwerfen des Standardstils für **BgLabelControl**

**BgLabelControl** legt in seinem Konstruktor einen Standardstilschlüssel für sich selbst fest. *Aber was ist ein Standardstil?* Ein benutzerdefiniertes (vorlagenbasiertes) Steuerelement muss über einen Standardstil (mit einer Standardvorlage für das Steuerelement) verfügen, mit dem es sich selbst rendern kann, falls der Consumer des Steuerelements keinen Stil und/oder keine Vorlage festlegt. In diesem Abschnitt fügen wir dem Projekt eine Markupdatei mit unserem Standardstil hinzu.

Vergewissere dich, dass **Alle Dateien anzeigen** weiterhin aktiviert ist (im **Projektmappen-Explorer**). Erstelle unter deinem Projektknoten einen neuen Ordner (keinen Filter, sondern einen Ordner) mit dem Namen „Themes”. Füge unter `Themes` ein neues Element vom Typ **Visual C++**  > **XAML** > **XAML-Ansicht** hinzu, und nenne es „Generic.xaml“. Ordner- und Dateiname müssen wie angegeben festgelegt werden, damit das XAML-Framework den Standardstil für ein benutzerdefiniertes Steuerelement findet. Lösche den Standardinhalt von `Generic.xaml`, und füge das folgende Markup ein:

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

In diesem Fall legt der Standardstil als einzige Eigenschaft die Steuerelementvorlage fest. Die Vorlage besteht aus einem Quadrat (dessen Hintergrund an die Eigenschaft **Background** gebunden ist, über die alle Instanzen des XAML-Typs [**Control**](/uwp/api/windows.ui.xaml.controls.control) verfügen) sowie aus einem Textelement (dessen Text an die Abhängigkeitseigenschaft **BgLabelControl::Label** gebunden ist).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Hinzufügen einer Instanz von **BgLabelControl** zur UI-Hauptseite

Öffne `MainPage.xaml`. Darin befindet sich das XAML-Markup für unsere UI-Hauptseite. Füge direkt nach dem Element **Button** (innerhalb von **StackPanel**) das folgende Markup hinzu:

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Füge außerdem die folgende include-Anweisung zu `MainPage.h` hinzu, um den Typ **MainPage** (eine Kombination aus XAML-Kompilierungsmarkup und imperativem Code) auf den benutzerdefinierten Steuerelementtyp **BgLabelControl** aufmerksam zu machen. Wenn du **BgLabelControl** von einer anderen XAML-Seite aus verwenden möchtest, musst du der Headerdatei für diese Seite die gleiche include-Anweisung hinzufügen. Alternativ kannst du auch einfach deiner vorkompilierten Headerdatei eine einzelne include-Anweisung hinzufügen.

```cppwinrt
// MainPage.h
...
#include "BgLabelControl.h"
...
```

Erstelle nun das Projekt, und führe es aus. Wie du siehst, wird die Standardsteuerelementvorlage an den Hintergrundpinsel (und an die Bezeichnung) der Instanz **BgLabelControl** im Markup gebunden.

In dieser exemplarischen Vorgehensweise wurde ein einfaches Beispiel für ein benutzerdefiniertes (vorlagenbasiertes) Steuerelement in C++/WinRT gezeigt. Du kannst deine eigenen Steuerelemente nach Belieben mit verschiedensten Features ausstatten. Ein benutzerdefiniertes Steuerelement kann beispielsweise ein komplexes bearbeitbares Datenraster, ein Videoplayer oder eine 3D-Geometrievisualisierung sein.

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementieren *überschreibbarer* Funktionen wie **MeasureOverride** und **OnApplyTemplate**

Ein benutzerdefiniertes Steuerelement wird von der Laufzeitklasse [**Control**](/uwp/api/windows.ui.xaml.controls.control) abgeleitet, die wiederum selbst von Basislaufzeitklassen abgeleitet wird. Für **Control**, [**FrameworkElement**](/uwp/api/windows.ui.xaml.frameworkelement) und [**UIElement**](/uwp/api/windows.ui.xaml.uielement) stehen überschreibbare Methoden zur Verfügung, mit denen du deine abgeleitete Klasse überschreiben kannst. Das folgende Codebeispiel veranschaulicht, wie das geht:

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

*Überschreibbare* Funktionen werden in verschiedenen Sprachprojektionen unterschiedlich dargestellt. In C# werden überschreibbare Funktionen beispielsweise in der Regel als geschützte virtuelle Funktionen dargestellt. In C++/WinRT sind sie zwar weder virtuell noch geschützt, du kannst sie aber trotzdem überschreiben und eine eigene Implementierung verwenden, wie weiter oben gezeigt.

## <a name="important-apis"></a>Wichtige APIs
* [Klasse „Control“](/uwp/api/windows.ui.xaml.controls.control)
* [Klasse „DependencyProperty“](/uwp/api/windows.ui.xaml.dependencyproperty)
* [Klasse „FrameworkElement“](/uwp/api/windows.ui.xaml.frameworkelement)
* [Klasse „UIElement“](/uwp/api/windows.ui.xaml.uielement)

## <a name="related-topics"></a>Verwandte Themen
* [Steuerelementvorlagen](/windows/uwp/design/controls-and-patterns/control-templates)
* [Benutzerdefinierte Abhängigkeitseigenschaften](/windows/uwp/xaml-platform/custom-dependency-properties)
