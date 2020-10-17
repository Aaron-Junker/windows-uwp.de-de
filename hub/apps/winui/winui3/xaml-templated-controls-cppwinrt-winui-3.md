---
description: In diesem Artikel werden Sie schrittweise durch den Vorgang zum Erstellen eines XAML-Steuerelements mit Vorlagen für WinUI 3 mit C++/WinRT geführt.
title: XAML-Steuerelemente in Vorlagen für WinUI 3-Apps mit C++/WinRT
ms.date: 07/09/2020
ms.topic: article
keywords: Windows 10, UWP, benutzerdefiniertes Steuerelement, Steuerelement mit Vorlagen, WinUI, C++/WinRT
ms.author: drewbat
author: drewbatgit
ms.localizationpriority: high
ms.custom: 19H1
ms.openlocfilehash: 9790db23160538e90e0386c34003831e5efb20ac
ms.sourcegitcommit: aabd6f40df6cc82bb8ce3a43275e4abd568c236f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/14/2020
ms.locfileid: "92061708"
---
# <a name="templated-xaml-controls-for-winui-3-apps-with-cwinrt"></a>XAML-Steuerelemente in Vorlagen für WinUI 3-Apps mit C++/WinRT

In diesem Artikel werden Sie schrittweise durch das Erstellen eines XAML-Steuerelements mit Vorlagen für WinUI 3 mit C++/WinRT geführt. Steuerelemente mit Vorlagen erben von der **Microsoft.UI.Xaml.Controls.Control**-Klasse und besitzen visuelle Struktur sowie visuelles Verhalten, die mithilfe von XAML-Steuerelementvorlagen angepasst werden können. Dieser Artikel beschreibt dasselbe Szenario wie der Artikel [Benutzerdefinierte XAML-Steuerelemente (mit Vorlagen) mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/xaml-cust-ctrl), wurde hinsichtlich der Verwendung von WinUI 3 angepasst.

Bevor Sie die Schritte in diesem Artikel ausführen, sollten Sie sicherstellen, dass Ihre Entwicklungsumgebung für die Erstellung von WinUI 3-Apps konfiguriert ist. Informationen zum Setup finden Sie unter [Erste Schritte mit WinUI 3 für Desktop-Apps](./get-started-winui3-for-desktop.md). Sie müssen zudem die aktuelle Version der [C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) vom [Visual Studio Marketplace](https://marketplace.visualstudio.com) herunterladen und installieren.

## <a name="create-a-blank-app-bglabelcontrolapp"></a>Erstellen einer leeren App (BgLabelControlApp)

Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Wählen Sie im Dialogfeld `Create a new project` die Projektvorlage **Leere App (WinUI in UWP)** aus, und vergewissern Sie sich, dass die Sprachversion C++ ausgewählt ist. Legen Sie den Projektnamen auf „BgLabelControlApp“ fest, damit die Dateinamen denjenigen im Code in den folgenden Beispielen entsprechen. Legen Sie die **Zielversion** auf Windows 10, Version 1903 (Build 18362), und die **Mindestversion** auf Windows 10, Version 1803 (Build 17134), fest. Diese exemplarische Vorgehensweise funktioniert auch für Desktop-Apps, die mit der Projektvorlage **Leere App, gepackt (WinUI in Desktop)** erstellt wurden. Stellen Sie lediglich sicher, dass Sie alle Schritte im Projekt **BgLabelControlApp (Desktop)** ausführen.

![Projektvorlage „Leere App“](images/WinUI-cpp-newproject-UWP.png)

## <a name="add-a-templated-control-to-your-app"></a>Hinzufügen eines vordefinierten Steuerelements zu Ihrer App

Um ein vordefiniertes Steuerelement hinzuzufügen, klicken Sie auf das Menü **Projekt** in der Symbolleiste, oder klicken Sie mit der rechten Maustaste auf Ihr Projekt im **Projektmappen-Explorer**, und wählen Sie **Neues Element hinzufügen** aus. Wählen Sie unter **Visual C++->WinUI** die Vorlage **Benutzerdefiniertes Steuerelement (WinUI)** aus. Benennen Sie das neue Steuerelement „BgLabelControl“, und klicken Sie auf *Hinzufügen*. Dadurch werden dem Projekt drei neue Dateien hinzugefügt. `BgLabelControl.h` ist der Header, der die Steuerelementdeklarationen enthält, und `BgLabelControl.cpp` enthält die C++/WinRT-Implementierung des Steuerelements. `BgLabelControl.idl` ist die Schnittstellendefinitionsdatei, mit der das Steuerelement als Laufzeitklasse instanziiert werden kann.

## <a name="implement-the-bglabelcontrol-custom-control-class"></a>Implementieren der benutzerdefinierten Steuerelementklasse „BgLabelControl“

In den folgenden Schritten aktualisieren Sie den Code in den Dateien `BgLabelControl.idl`, `BgLabelControl.h` und `BgLabelControl.cpp` im Projektverzeichnis, um die Laufzeitklasse zu implementieren. 


Die Steuerelementklasse mit Vorlagen wird per XAML-Markup instanziiert, und wird dadurch eine Laufzeitklasse. Wenn Sie das fertige Projekt erstellen, wird der MIDL-Compiler (midl.exe) die `BgLabelControl.idl`-Datei verwenden, um die Windows-Runtime-Metadatendatei (.winmd) für Ihr Steuerelement zu generieren, auf die die Consumer Ihrer Komponente verweisen. Weitere Informationen zum Erstellen von Laufzeitklassen finden Sie unter [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

Das Steuerelement mit Vorlagen, das wir erstellen, macht eine einzelne Eigenschaft verfügbar, nämlich eine Zeichenfolge, die als Beschriftung für das Steuerelement verwendet wird. Ersetzen Sie den Inhalt von `BgLabelControl.idl` durch den folgenden Code.

```cppwinrt
// BgLabelControl.idl
namespace BgLabelControlApp
{
    runtimeclass BgLabelControl : Microsoft.UI.Xaml.Controls.Control
    {
        BgLabelControl();
        static Microsoft.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}
```

Das obige Listing veranschaulicht das Muster zum Deklarieren einer Abhängigkeitseigenschaft (Dependency Property, DP). Eine Abhängigkeitseigenschaft besteht aus zwei Teilen: Zunächst deklarieren Sie eine schreibgeschützte statische Eigenschaft vom Typ „DependencyProperty“. Diese trägt den Namen Ihrer Abhängigkeitseigenschaft plus „Property“ (Eigenschaft). Diese statische Eigenschaft wird in deiner Implementierung verwendet. Danach wird eine Instanzeigenschaft mit Lese-/Schreibzugriff sowie mit dem Typ und dem Namen deiner Abhängigkeitseigenschaft deklariert. Wenn Sie anstelle einer Abhängigkeitseigenschaft eine angefügte Eigenschaft erstellen möchten, sehen Sie sich die Codebeispiele unter [Benutzerdefinierte angefügte Eigenschaften](/windows/uwp/xaml-platform/custom-attached-properties) an.

Beachten Sie, dass die XAML-Klassen, auf die im obigen Code verwiesen wird, in „Microsoft.UI.Xaml“-Namespaces enthalten sind. Dies unterscheidet Sie als WinUI-Steuerelemente von UWP-XAML-Steuerelementen, die in „Windows.UI.XAML“-Namespaces definiert sind.

Ersetzen Sie den Inhalt von „BgLabelControl.h“ durch folgenden Code.

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

        static Microsoft::UI::Xaml::DependencyProperty LabelProperty() { return m_labelProperty; }

        static void OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const&, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const&);

    private:
        static Microsoft::UI::Xaml::DependencyProperty m_labelProperty;
    };
}
namespace winrt::BgLabelControlApp::factory_implementation
{
    struct BgLabelControl : BgLabelControlT<BgLabelControl, implementation::BgLabelControl>
    {
    };
}
```

Der oben gezeigte Code implementiert die Eigenschaften **Label** und **LabelProperty**, fügt einen statischen Ereignishandler namens **OnLabelChanged** hinzu, um Änderungen am Wert der Abhängigkeitseigenschaft zu verarbeiten, und fügt einen privaten Member zum Speichern des dahinter liegenden Felds für **LabelProperty** hinzu. Beachten Sie wiederum, dass die XAML-Klassen, auf die in der Headerdatei verwiesen wird, in den „Microsoft.UI.Xaml“-Namespaces enthalten sind, die zum WinUI 3-Framework gehören, und nicht in den „Windows.UI.Xaml“-Namespaces, die vom UWP-Benutzeroberflächen-Framework verwendet werden.


Ersetzen Sie als Nächstes den Inhalt von „BgLabelControl.cpp“ durch folgenden Code.

```cppwinrt
// BgLabelControl.cpp
#include "pch.h"
#include "BgLabelControl.h"
#if __has_include("BgLabelControl.g.cpp")
#include "BgLabelControl.g.cpp"

namespace winrt::BgLabelControlApp::implementation
{
    Microsoft::UI::Xaml::DependencyProperty BgLabelControl::m_labelProperty =
        Microsoft::UI::Xaml::DependencyProperty::Register(
            L"Label",
            winrt::xaml_typename<winrt::hstring>(),
            winrt::xaml_typename<BgLabelControlApp::BgLabelControl>(),
            Microsoft::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Microsoft::UI::Xaml::PropertyChangedCallback{ &BgLabelControl::OnLabelChanged } }
    );

    void BgLabelControl::OnLabelChanged(Microsoft::UI::Xaml::DependencyObject const& d, Microsoft::UI::Xaml::DependencyPropertyChangedEventArgs const& /* e */)
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
In dieser exemplarischen Vorgehensweise wird der Rückruf **OnLabelChanged** nicht verwendet, er wird aber zur Verfügung gestellt, um zu zeigen, wie Sie eine Abhängigkeitseigenschaft mit einem „PropertyChanged“-Rückruf registrieren. Die Implementierung von **OnLabelChanged** zeigt auch, wie Sie einen abgeleiteten projizierten Typ eines projizierten Basistyps (in diesem Fall „DependencyObject“) erhalten. Darüber hinaus wird gezeigt, wie du anschließend einen Zeiger auf den Typ erhältst, der den projizierten Typ implementiert. Dieser zweite Vorgang ist natürlich nur in dem Projekt möglich, das den projizierten Typ implementiert (also in dem Projekt, das die Laufzeitklasse implementiert).

Die [xaml_typename](/uwp/cpp-ref-for-winrt/xaml-typename)-Funktion wird vom „Windows.UI.Xaml.Interop“-Namespace bereitgestellt, der nicht standardmäßig in der WinUI 3-Projektvorlage enthalten ist. Fügen Sie der vorkompilierten Headerdatei für Ihr Projekt `pch.h` eine Zeile hinzu, um die diesem Namespace zugeordnete Headerdatei einzuschließen.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Xaml.Interop.h>
...
```



## <a name="define-the-default-style-for-bglabelcontrol"></a>Definieren des Standardstils für „BgLabelControl“

**BgLabelControl** legt in seinem Konstruktor einen Standardstilschlüssel für sich selbst fest. Ein Steuerelement mit Vorlagen muss über einen Standardstil (mit einer Standardvorlage für das Steuerelement) verfügen, mit dem es sich selbst rendern kann, falls der Consumer des Steuerelements keinen Stil und/oder keine Vorlage festlegt. In diesem Abschnitt fügen wir dem Projekt eine Markupdatei mit unserem Standardstil hinzu.

Vergewissere dich, dass **Alle Dateien anzeigen** weiterhin aktiviert ist (im **Projektmappen-Explorer**). Erstelle unter deinem Projektknoten einen neuen Ordner (keinen Filter, sondern einen Ordner) mit dem Namen „Themes”. Fügen Sie unter `Themes` ein neues Element vom Typ **Visual C++ > WinUI > Ressourcenwörterbuch (WinUI)** hinzu, und nennen Sie es „Generic.xaml“. Ordner- und Dateiname müssen wie angegeben festgelegt werden, damit das XAML-Framework den Standardstil für ein Steuerelement mit Vorlagen findet. Löschen Sie den Standardinhalt von „Generic.xaml“, und fügen Sie das folgende Markup ein.

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

In diesem Fall legt der Standardstil als einzige Eigenschaft die Steuerelementvorlage fest. Die Vorlage besteht aus einem Quadrat (dessen Hintergrund an die Eigenschaft **Background** gebunden ist, über die alle Instanzen des XAML-Typs **Control** verfügen) sowie aus einem Textelement (dessen Text an die Abhängigkeitseigenschaft **BgLabelControl::Label** gebunden ist).

## <a name="add-an-instance-of-bglabelcontrol-to-the-main-ui-page"></a>Hinzufügen einer Instanz von „BgLabelControl“ zur Hauptseite der Benutzeroberfläche

Öffne `MainPage.xaml`. Darin befindet sich das XAML-Markup für unsere UI-Hauptseite. Füge direkt nach dem Element **Button** (innerhalb von **StackPanel**) das folgende Markup hinzu:

```xaml
<local:BgLabelControl Background="Red" Label="Hello, World!"/>
```

Fügen Sie außerdem die folgende include-Anweisung zu `MainPage.h` hinzu, um den Typ **MainPage** (eine Kombination aus XAML-Kompilierungsmarkup und imperativem Code) auf den Steuerelementtyp mit Vorlagen **BgLabelControl** aufmerksam zu machen. Wenn du **BgLabelControl** von einer anderen XAML-Seite aus verwenden möchtest, musst du der Headerdatei für diese Seite die gleiche include-Anweisung hinzufügen. Alternativ kannst du auch einfach deiner vorkompilierten Headerdatei eine einzelne include-Anweisung hinzufügen.

```cppwinrt
//MainPage.h
...
#include "BgLabelControl.h"
...
```

Erstelle nun das Projekt, und führe es aus. Wie du siehst, wird die Standardsteuerelementvorlage an den Hintergrundpinsel (und an die Bezeichnung) der Instanz **BgLabelControl** im Markup gebunden.

![Steuerelement mit Vorlagen, Ergebnis](images/winui-templated-control-result.png)

## <a name="implementing-overridable-functions-such-as-measureoverride-and-onapplytemplate"></a>Implementieren überschreibbarer Funktionen wie **MeasureOverride** und **OnApplyTemplate**

Ein Steuerelement mit Vorlagen wird von der Laufzeitklasse **Control** abgeleitet, die wiederum selbst von Basislaufzeitklassen abgeleitet wird. Ferner stehen überschreibbare Methoden von **Control**, **FrameworkElement** und **UIElement** zur Verfügung, die Sie in Ihrer abgeleiteten Klasse überschreiben können. Das folgende Codebeispiel veranschaulicht, wie das geht:

```cppwinrt
// Control overrides.
void OnPointerPressed(Microsoft::UI::Xaml::Input::PointerRoutedEventArgs const& /* e */) const { ... };

// FrameworkElement overrides.
Windows::Foundation::Size MeasureOverride(Windows::Foundation::Size const& /* availableSize */) const { ... };
void OnApplyTemplate() const { ... };

// UIElement overrides.
Microsoft::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer() const { ... };
```

*Überschreibbare* Funktionen werden in verschiedenen Sprachprojektionen unterschiedlich dargestellt. In C# werden überschreibbare Funktionen beispielsweise in der Regel als geschützte virtuelle Funktionen dargestellt. In C++/WinRT sind sie zwar weder virtuell noch geschützt, du kannst sie aber trotzdem überschreiben und eine eigene Implementierung verwenden, wie weiter oben gezeigt.



## <a name="generating-the-control-source-files-without-using-a-template"></a>Generieren der Steuerelement-Quelldateien ohne Verwendung einer Vorlage.

In diesem Abschnitt wird gezeigt, wie Sie die erforderlichen Quelldateien zum Erstellen Ihres benutzerdefinierten Steuerelements ohne Verwendung der Elementvorlage **Benutzerdefiniertes Steuerelement** generieren können. 

Fügen Sie dem Projekt zunächst ein neues Midl-Datei (.idl)-Element hinzu. Wählen Sie im Menü **Projekt** die Option **Neues Element hinzufügen...** aus, und geben Sie in das Suchfeld „MIDL“ ein, um das IDL-Dateielement zu suchen. Nennen Sie die neue Datei `BgLabelControl.idl`, damit der Name mit den Schritten in diesem Artikel konsistent ist. Löschen Sie den Standardinhalt von `BgLabelControl.idl`, und füge die in den oben stehenden Schritten gezeigte Laufzeitklassendeklaration ein.


Nach dem Speichern der neuen IDL-Datei besteht der nächste Schritt im Generieren der Windows-Runtime-Metadatendatei (.winmd) und der Stubdateien für die .cpp- und .h-Implementierungsdateien, die Sie verwenden, um das Steuerelement mit Vorlagen zu implementieren. Generieren Sie diese Dateien, indem Sie die Projektmappe erstellen, wodurch der MIDL-Compiler (midl.exe) die von Ihnen erstellte IDL-Datei kompiliert. Beachten Sie, dass die Projektmappe nicht erfolgreich erstellt wird und dass Visual Studio Buildfehler im Ausgabefenster anzeigt, dass aber die erforderlichen Dateien generiert werden.

Kopieren Sie die Stubdateien „BgLabelControl.h“ und „BgLabelControl.cpp“ aus „\BgLabelControlApp\BgLabelControlApp\Generated“ in den Projektordner. Vergewissern Sie sich im **Projektmappen-Explorer**, dass „Alle Dateien anzeigen“ aktiviert ist. Klicke mit der rechten Maustaste auf die kopierten Stub-Dateien, und klicke auf **Zu Projekt hinzufügen**.

Der Compiler platziert eine „static_assert“-Zeile am Anfang von „BgLabelControl.h“ und von „BgLabelControl.cpp“, um zu verhindern, dass die generierten Dateien kompiliert werden. Wenn Sie Ihr Steuerelement implementieren, sollten Sie diese Zeilen aus den Dateien entfernen, die Sie in Ihrem Projektverzeichnis abgelegt haben. In dieser exemplarischen Vorgehensweise können Sie einfach den gesamten Inhalt der Dateien mit dem oben angegebenen Code überschreiben.
