---
description: Hier wird erläutert, wie Sie benutzerdefinierte Abhängigkeitseigenschaften für eine Windows-Runtime-App mit C++, C# oder Visual Basic definieren und implementieren können.
title: Benutzerdefinierte Abhängigkeitseigenschaften
ms.assetid: 5ADF7935-F2CF-4BB6-B1A5-F535C2ED8EF8
ms.date: 07/12/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 796ee7ed1454515817f5fc994ccb9242d2a2918c
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259871"
---
# <a name="custom-dependency-properties"></a>Benutzerdefinierte Abhängigkeitseigenschaften

Hier wird erläutert, wie Sie eigene Abhängigkeitseigenschaften für eine Windows-Runtime-App mit C++, C# oder Visual Basic verwenden können. Wir zählen mögliche Gründen für die Erstellung benutzerdefinierter Abhängigkeitseigenschaften durch App-Entwickler und Komponentenautoren auf. Des Weiteren beschreiben wir die Implementierungsschritte für eine benutzerdefinierte Abhängigkeitseigenschaft sowie einige bewährte Methoden, die die Leistung, Benutzerfreundlichkeit und Vielseitigkeit der Abhängigkeitseigenschaft verbessern.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen davon aus, dass Sie die [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md) bereits gelesen haben und dass Sie Abhängigkeitseigenschaften aus der Perspektive eines Konsumenten von bestehenden Abhängigkeitseigenschaften verstehen. Für ein besseres Verständnis der in diesem Thema aufgeführten Beispiele sollten Sie XAML verstehen und wissen, wie eine einfache Windows-Runtime-App mit C++, C# oder Visual Basic geschrieben wird.

## <a name="what-is-a-dependency-property"></a>Was ist eine Abhängigkeitseigenschaft?

Um Formate, Datenbindung, Animationen und Standardwerte für eine Eigenschaft zu unterstützen, sollte sie als Abhängigkeitseigenschaft implementiert werden. Werte der Abhängigkeitseigenschaft werden nicht als Felder in der Klasse gespeichert, sondern sie werden vom XAML-Framework gespeichert und mit einem Schlüssel verwiesen, der abgerufen wird, wenn die Eigenschaft durch Aufruf der [**DependencyProperty.Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)-Methode im Windows-Runtime-Eigenschaftensystem registriert wird.   Abhängigkeitseigenschaften können nur von Typen verwendet werden, die von [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) abgeleitet sind. Das **DependencyObject** befindet sich jedoch ziemlich weit oben in der Klassenhierarchie, sodass die Mehrzahl der Klassen, die für die UI- und Darstellungsunterstützung bestimmt sind, Abhängigkeitseigenschaften unterstützen können. Weitere Informationen zu Abhängigkeitseigenschaften und für die in dieser Dokumentation verwendeten Begriffe und Konventionen finden Sie unter [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md).

Beispiele für Abhängigkeitseigenschaften in der Windows-Runtime sind: [**Control.Background**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.background), [**FrameworkElement.Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width), [**TextBox.Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textbox.text) und viele weitere.

Gemäß der Konvention besitzt jede durch eine Klasse verfügbar gemachte Abhängigkeitseigenschaft eine entsprechende **public static readonly**-Eigenschaft des Typs [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty), die für die gleiche Klasse verfügbar gemacht wird, die den Bezeichner für die Abhängigkeitseigenschaft bereitstellt. Für die Benennung des Bezeichners wird folgende Konvention verwendet: der Name der Abhängigkeitseigenschaft mit der am Namensende angehängten Zeichenfolge „Property“. Beispielsweise ist der **DependencyProperty**-Bezeichner für die Eigenschaft **Control.Background**[**Control.BackgroundProperty**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.backgroundproperty). Der Bezeichner speichert die Informationen zur Abhängigkeitseigenschaft, die bei der Registrierung gelten, und kann anschließend für andere Vorgänge verwendet werden, welche die Abhängigkeitseigenschaft betreffen, wie z. B. der Aufruf von [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue).

## <a name="property-wrappers"></a>Eigenschaftenwrapper

Abhängigkeitseigenschaften haben für gewöhnlich eine Wrapperimplementierung. Ohne den Wrapper können die Eigenschaften nur durch die Verwendung der Methoden der Abhängigkeitseigenschafts-Hilfsprogramme [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue) und [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) sowie durch die Nutzung des Bezeichners als Parameter abgerufen oder festgelegt werden. Dies handelt es sich um eine ziemlich ungewöhnliche Verwendung für eine offensichtliche Eigenschaft. Mit dem Wrapper können Ihr Code und jegliche anderen Codes, die auf die Abhängigkeitseigenschaft verweisen, eine unkomplizierte, für die von Ihnen verwendete Sprache natürliche Syntax für Objekteigenschaften verwenden.

Wenn Sie eine benutzerdefinierte Abhängigkeitseigenschaft selbst implementieren und diese veröffentlichen und leicht auffindbar machen möchten, definieren Sie auch die Eigenschaftenwrapper. Eigenschaftenwrapper sind des Weiteren nützlich für die Berichterstattung über grundlegende Informationen der Abhängigkeitseigenschaft für Betrachtungen und statische Analysen. Der Wrapper befindet sich insbesondere dort, wo Sie Attribute platzieren, beispielsweise [**ContentPropertyAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.ContentPropertyAttribute).

## <a name="when-to-implement-a-property-as-a-dependency-property"></a>Wann sollte eine Eigenschaft als Abhängigkeitseigenschaft implementiert werden?

Wenn Sie eine öffentliche Lese-/Schreibeigenschaft in eine Klasse implementieren und die Klasse von [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) abgeleitet ist, können Sie Ihre Eigenschaft als Abhängigkeitseigenschaft nutzen. In einigen Fällen ist die typische Methode, Ihre Eigenschaft mit einem privaten Feld abzusichern, angemessen. Das Definieren Ihrer benutzerdefinierten Eigenschaft als Abhängigkeitseigenschaft ist nicht immer notwendig oder angemessen. Die Entscheidung hängt von den Szenarien ab, die Ihre Eigenschaft unterstützen soll.

Sie können Ihre Eigenschaft als Abhängigkeitseigenschaft implementieren, wenn diese mindestens eine der folgenden Eigenschaften der Windows-Runtime oder von Windows-Runtime-Apps unterstützen soll:

- Festlegen der Eigenschaft über einen [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style)
- Funktion als gültige Zieleigenschaft für Datenbindung mit [ **{Binding}** ](binding-markup-extension.md)
- Unterstützung animierter Werte durch ein [**Storyboard**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Storyboard)
- Melden von Änderungen des Eigenschaftswerts durch:
  - Vom Eigenschaftensystem selbst durchgeführte Aktionen
  - Die Umgebung
  - Benutzeraktionen
  - Lese- und Schreibstile

## <a name="checklist-for-defining-a-dependency-property"></a>Prüfliste für die Definition einer Abhängigkeitseigenschaft

Das Definieren einer Abhängigkeitseigenschaft umfasst mehrere Konzepte. Bei diesen Begriffen handelt es sich nicht unbedingt um einzelne Schritte, da einige Schritte bei der Implementierung im Code in einzelnen Codezeilen zusammengefasst werden: Diese Liste verschafft Ihnen einen schnellen Überblick. Wir werden später noch genauer auf jeden Begriff eingehen und Ihnen Beispielcode in verschiedenen Sprachen zeigen.

- Registrieren Sie den Eigenschaftennamen im Eigenschaftensystem (durch Aufruf von [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)), indem Sie einen Besitzertyp und den Typ des Eigenschaftswerts angeben.
  - Es gibt auch einen erforderlichen Parameter für [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register), der Eigenschaftenmetadaten benötigt. Geben Sie **null** dafür an, oder wenn Sie PropertyChanged-Verhalten oder einen metadatenbasierten Standardwert haben möchten, der durch den Aufruf von [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) wiederhergestellt werden kann, geben Sie eine Instanz von [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.propertymetadata) an.
- Definieren Sie einen [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)-Bezeichner als ein **public static readonly**-Eigenschaftenmember des Besitzertyps.
- Definieren Sie eine Wrappereigenschaft nach dem Accessor-Modell für Eigenschaften, das in der von Ihnen implementierten Sprache verwendet wird. Der Name der Wrappereigenschaft muss mit der Zeichenfolge *string* übereinstimmen, die Sie in [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register) verwendet haben. Implementieren Sie die **get**- und **set**-Accessoren, um den Wrapper durch den Aufruf von [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue) und [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) und die Übergabe des Bezeichners Ihrer Eigenschaft als Parameter mit der von diesem umschlossenen Abhängigkeitseigenschaft zu verbinden.
- (Optional) Platzieren Sie Attribute wie [**ContentPropertyAttribute**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Markup.ContentPropertyAttribute) auf dem Wrapper.

> [!NOTE]
> Wenn Sie eine benutzerdefinierte angefügte Eigenschaft definieren, lassen Sie den Wrapper in der Regel aus. Stattdessen verwenden Sie einen anderen Accessor-Stil, den ein XAML-Prozessor verwenden kann. Siehe [Benutzerdefinierte angefügte Eigenschaften](custom-attached-properties.md). 

## <a name="registering-the-property"></a>Registrieren der Eigenschaft

Damit Ihre Eigenschaft zu einer Abhängigkeitseigenschaft wird, müssen Sie die Eigenschaft in einem Eigenschaftenspeicher registrieren, der vom Windows-Runtime-Eigenschaftensystem verwaltet wird.  Rufen Sie für die Registrierung der Eigenschaft die [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)-Methode auf.

Im Fall von Microsoft .NET-Sprachen (C# und Microsoft Visual Basic) rufen Sie [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register) im Text Ihrer Klasse auf (innerhalb der Klasse, jedoch außerhalb der Memberdefinitionen). Der Bezeichner wird vom [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)-Methodenaufruf als Rückgabewert bereitgestellt. Der [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)-Aufruf erfolgt in der Regel als statischer Konstruktor oder als Teil der Initialisierung einer **public static readonly**-Eigenschaft des Typs [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) als Teil der Klasse. Diese Eigenschaft macht den Bezeichner für Ihre Abhängigkeitseigenschaft verfügbar. Im Anschluss finden Sie Beispiele für den [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)-Aufruf.

> [!NOTE]
> Die Registrierung der Abhängigkeits Eigenschaft als Teil der Definition der Bezeichnereigenschaft ist die typische Implementierung, aber Sie können auch eine Abhängigkeits Eigenschaft im statischen Konstruktor der Klasse registrieren. Dieser Ansatz eignet sich ggf., wenn Sie mehr als eine Codezeile benötigen, um die Abhängigkeitseigenschaft zu initialisieren.

Für C++/CX haben Sie Optionen, wie Sie die Implementierung zwischen dem Header und der Codedatei aufteilen. In der Regel wird der Bezeichner selbst als **public static**-Eigenschaft in der Kopfzeile mit einer **get**-Implementierung, jedoch ohne **set**-Implementierung deklariert. Die **get**-Implementierung bezieht sich auf ein privates Feld, das eine nicht initialisierte [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)-Instanz ist. Sie können darüber hinaus die Wrapper und die **get**- und **set**-Implementierungen des Wrappers deklarieren. In diesem Fall enthält der Wrapper eine minimale Implementierung. Wenn der Wrapper eine Zuordnung zur Windows-Runtime benötigt, führen Sie die Zuordnung auch in der Kopfzeile durch. Platzieren Sie den [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)-Aufruf in die Codedatei innerhalb einer Hilfsfunktion, die nur bei der ersten Initialisierung der App ausgeführt wird. Verwenden Sie den Rückgabewert von **Register**, um die statischen, jedoch nicht initialisierten Bezeichner zu füllen, die Sie in der Kopfzeile deklariert haben und zunächst im Stammbereich der Implementierungsdatei auf **nullptr** festgelegt haben.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null)
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty = 
    DependencyProperty.Register("Label", 
      GetType(String), 
      GetType(ImageWithLabelControl), 
      New PropertyMetadata(Nothing))
```

```cppwinrt
// ImageWithLabelControl.idl
namespace ImageWithLabelControlApp
{
    runtimeclass ImageWithLabelControl : Windows.UI.Xaml.Controls.Control
    {
        ImageWithLabelControl();
        static Windows.UI.Xaml.DependencyProperty LabelProperty{ get; };
        String Label;
    }
}

// ImageWithLabelControl.h
...
private:
    static Windows::UI::Xaml::DependencyProperty m_labelProperty;
...

// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr }
);
...
```

```cpp
//.h file
//using namespace Windows::UI::Xaml::Controls;
//using namespace Windows::UI::Xaml::Interop;
//using namespace Windows::UI::Xaml;
//using namespace Platform;

public ref class ImageWithLabelControl sealed : public Control
{
private:
    static DependencyProperty^ _LabelProperty;
...
public:
    static void RegisterDependencyProperties();
    static property DependencyProperty^ LabelProperty
    {
        DependencyProperty^ get() {return _LabelProperty;}
    }
...
};

//.cpp file
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml.Interop;

DependencyProperty^ ImageWithLabelControl::_LabelProperty = nullptr;

// This function is called from the App constructor in App.xaml.cpp
// to register the properties
void ImageWithLabelControl::RegisterDependencyProperties()
{ 
    if (_LabelProperty == nullptr)
    { 
        _LabelProperty = DependencyProperty::Register(
          "Label", Platform::String::typeid, ImageWithLabelControl::typeid, nullptr);
    } 
}
```

> [!NOTE]
> Der Grund C++für den/CX-Code ist der Grund, warum Sie ein privates Feld und eine öffentliche schreibgeschützte Eigenschaft haben, die die [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) -Eigenschaft verwendet, sodass andere Aufrufer, die ihre Abhängigkeits Eigenschaft verwenden, auch APIs für das Hilfsprogramm für den Dienst verwenden können, für die der Bezeichner öffentlich sein muss Wenn Sie den Bezeichner nicht offenlegen, können Benutzer diese Hilfsprogramm-APIs nicht verwenden. Beispiele für eine API und Szenarien dieser Art sind [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue), [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue), [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue), [**GetAnimationBaseValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getanimationbasevalue), [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) und [**Setter.Property**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.setter.property). Ein öffentliches Feld kann dazu nicht verwendet werden, da Windows-Runtime-Metadatenregeln keine öffentlichen Felder zulassen.

## <a name="dependency-property-name-conventions"></a>Namenskonventionen für Abhängigkeitseigenschaften

Für Abhängigkeitseigenschaften gelten Namenskonventionen, die Sie bis auf bestimmte Ausnahmefälle befolgen müssen. Die Abhängigkeitseigenschaft selbst hat einen Basisnamen („Label“ im vorherigen Beispiel), der als erster Parameter von [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register) angegeben wird. Dieser Name muss innerhalb jedes Registrierungstyps eindeutig sein. Diese Eindeutigkeit muss auch von vererbten Membern eingehalten werden. Abhängigkeitseigenschaften, die über Basistypen geerbt werden, werden als Teil des Registrierungstyps betrachtet. Die Namen geerbter Eigenschaften können nicht erneut registriert werden.

> [!WARNING]
> Obwohl es sich bei dem hier angegebenen Namen um einen beliebigen Zeichen folgen Bezeichner handeln kann, der in der Programmiersprache Ihrer Wahl gültig ist, möchten Sie in der Regel auch die Abhängigkeits Eigenschaft in XAML festlegen können. Für die Verwendung in XAML muss der von Ihnen gewählte Eigenschaftenname ein gültiger XAML-Name sein. Weitere Informationen finden Sie in der [XAML-Übersicht](xaml-overview.md).

Kombinieren Sie beim Erstellen der Bezeichnereigenschaft den von Ihnen registrierten Eigenschaftennamen mit dem Suffix „Property“ (beispielsweise „LabelProperty“). Diese Eigenschaft ist Ihr Bezeichner für die Abhängigkeitseigenschaft und wird als Eingabe für die [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue)- und [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue)-Aufrufe verwendet, die Sie in Ihren eigenen Eigenschaftenwrappern ausführen. Sie wird auch vom Eigenschaftensystem und anderen XAML-Prozessoren, wie z. B. [ **{x:Bind}** ](x-bind-markup-extension.md), verwendet.

## <a name="implementing-the-wrapper"></a>Implementieren des Wrappers

Ihr Eigenschaftenwrapper sollte [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue) in der **get**-Implementierung und [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) in der **set**-Implementierung aufrufen.

> [!WARNING]
> In allen außer Ausnahmefällen sollten Ihre Wrapper Implementierungen nur die Vorgänge " [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue) " und " [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) " ausführen. Andernfalls erhalten Sie ein anderes Verhalten, wenn Ihre Eigenschaft über XAML anstelle über Code festgelegt wird. Aus Effizienzgründen umgeht der XAML-Parser Wrapper beim Festlegen von Abhängigkeitseigenschaften und kommuniziert mit dem Sicherungsspeicher über **SetValue**.

```csharp
public String Label
{
    get { return (String)GetValue(LabelProperty); }
    set { SetValue(LabelProperty, value); }
}
```

```vb
Public Property Label() As String
    Get
        Return DirectCast(GetValue(LabelProperty), String) 
    End Get 
    Set(ByVal value As String)
        SetValue(LabelProperty, value)
    End Set
End Property
```

```cppwinrt
// ImageWithLabelControl.h
...
winrt::hstring Label()
{
    return winrt::unbox_value<winrt::hstring>(GetValue(m_labelProperty));
}

void Label(winrt::hstring const& value)
{
    SetValue(m_labelProperty, winrt::box_value(value));
}
...
```

```cpp
//using namespace Platform;
public:
...
  property String^ Label
  {
    String^ get() {
      return (String^)GetValue(LabelProperty);
    }
    void set(String^ value) {
      SetValue(LabelProperty, value);
    }
  }
```

## <a name="property-metadata-for-a-custom-dependency-property"></a>Eigenschaftenmetadaten für eine benutzerdefinierte Abhängigkeitseigenschaft

Wenn Eigenschaftenmetadaten einer Abhängigkeitseigenschaft zugewiesen werden, gelten die gleichen Metadaten für diese Eigenschaft für jede Instanz des Eigenschaftenbesitzertyps oder dessen Unterklassen. Sie können zwei Verhalten in Eigenschaftenmetadaten festlegen:

- Einen Standardwert, den das Eigenschaftensystem allen Anfragen der Eigenschaft zuweist.
- Eine statische Rückrufmethode, die automatisch innerhalb des Eigenschaftensystems aufgerufen wird, sobald eine Eigenschaftswertänderung erkannt wird.

### <a name="calling-register-with-property-metadata"></a>Aufrufen von Register mit Eigenschaftenmetadaten

In den bisherigen Beispielen für den Aufruf von [**DependencyProperty.Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register) wurde für den Parameter *propertyMetadata* ein Nullwert übergeben. Damit eine Abhängigkeitseigenschaft einen Standardwert bereitstellt oder einen Rückruf mit geänderter Eigenschaft verwendet, müssen Sie eine [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata)-Instanz definieren, die mindestens eine dieser beiden Funktionen bereitstellt.

In der Regel geben Sie einen [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata) als inline erstellte Instanz innerhalb der Parameter für [**DependencyProperty.Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register) an.

> [!NOTE]
> Wenn Sie eine [**createdefaultvaluecallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.createdefaultvaluecallback) -Implementierung definieren, müssen Sie die-hilfsprogrammmethode [**PropertyMetadata. Create**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.propertymetadata.create) verwenden, anstatt einen [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata) -Konstruktor zum Definieren der **PropertyMetadata** -Instanz aufzurufenden.

Im nächsten Beispiel werden die zuvor gezeigten Beispiele für [**DependencyProperty.Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register) modifiziert, indem auf eine [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata)-Instanz mit einem [**PropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.propertychangedcallback)-Wert verwiesen wird. Die Implementierung des OnLabelChanged-Rückrufs wird später in diesem Abschnitt gezeigt.

```csharp
public static readonly DependencyProperty LabelProperty = DependencyProperty.Register(
  "Label",
  typeof(String),
  typeof(ImageWithLabelControl),
  new PropertyMetadata(null,new PropertyChangedCallback(OnLabelChanged))
);
```

```vb
Public Shared ReadOnly LabelProperty As DependencyProperty =
    DependencyProperty.Register("Label",
      GetType(String),
      GetType(ImageWithLabelControl),
      New PropertyMetadata(
        Nothing, new PropertyChangedCallback(AddressOf OnLabelChanged)))
```

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ nullptr, Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

```cpp
DependencyProperty^ ImageWithLabelControl::_LabelProperty =
    DependencyProperty::Register("Label",
    Platform::String::typeid,
    ImageWithLabelControl::typeid,
    ref new PropertyMetadata(nullptr,
      ref new PropertyChangedCallback(&ImageWithLabelControl::OnLabelChanged))
    );
```

### <a name="default-value"></a>Standardwert

Sie können einen Standardwert für eine Abhängigkeitseigenschaft festlegen, damit diese immer einen bestimmten Standardwert zurückgibt, wenn ihre Festlegung aufgehoben wird. Dieser Wert kann vom vererbten Standardwert für den Typ dieser Eigenschaft abweichen.

Wenn kein Standardwert festgelegt ist, ist der Standardwert einer Abhängigkeitseigenschaft für einen Verweistyp null oder entspricht der Standardeinstellung des Werttyps oder des Sprachengrundtyps (zum Beispiel 0 für einen Integer oder eine leere Zeichenfolge für eine Zeichenfolge). Ein Standardwert wird vor allem erstellt, damit dessen Wert wiederhergestellt wird, wenn [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) in der Eigenschaft aufgerufen wird. Das Erstellen eines Standardwerts pro Eigenschaft kann vorteilhafter als das Erstellen von Standardwerten in Konstruktoren sein, insbesondere für Werttypen. Achten Sie jedoch bei Verweistypen darauf, dass beim Erstellen eines Standardwerts kein unbeabsichtigtes Singleton-Muster entsteht. Weitere Informationen finden Sie weiter unten in diesem Thema unter [Bewährte Methoden](#best-practices).

```cppwinrt
// ImageWithLabelControl.cpp
...
Windows::UI::Xaml::DependencyProperty ImageWithLabelControl::m_labelProperty =
    Windows::UI::Xaml::DependencyProperty::Register(
        L"Label",
        winrt::xaml_typename<winrt::hstring>(),
        winrt::xaml_typename<ImageWithLabelControlApp::ImageWithLabelControl>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(L"default label"), Windows::UI::Xaml::PropertyChangedCallback{ &ImageWithLabelControl::OnLabelChanged } }
);
...
```

> [!NOTE]
> Registrieren Sie sich nicht mit dem Standardwert [**UnsetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.unsetvalue). Diese Handlung verwirrt Eigenschaftennutzer und führt zu unbeabsichtigten Folgen innerhalb des Eigenschaftensystems.

### <a name="createdefaultvaluecallback"></a>CreateDefaultValueCallback

In einigen Szenarien werden Abhängigkeitseigenschaften für Objekte definiert, die in mehreren UI-Threads verwendet werden. Dies ist beispielsweise der Fall, wenn ein Datenobjekt oder ein Steuerelement für mehrere Apps definiert wird. Sie können den Austausch des Objekts zwischen verschiedenen Benutzeroberflächenthreads ermöglichen, indem Sie anstelle einer Standardwertinstanz eine [**CreateDefaultValueCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.createdefaultvaluecallback)-Implementierung bereitstellen, die an den Thread gebunden ist, der die Eigenschaft registriert hat. Grundsätzlich definiert ein [**CreateDefaultValueCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.createdefaultvaluecallback) eine Zuordnungsinstanz für Standardwerte. Der Wert, der von **CreateDefaultValueCallback** zurückgegeben wird, ist stets mit dem aktuellen Benutzeroberflächenthread von **CreateDefaultValueCallback** verbunden, von dem das Objekt verwendet wird.

Um Metadaten zu definieren, die einen [**CreateDefaultValueCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.createdefaultvaluecallback) angeben, müssen Sie [**PropertyMetadata.Create**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.propertymetadata.create) aufrufen, um eine Metadateninstanz zurückzugeben. Die [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata)-Konstruktoren besitzen keine Signatur, die einen **CreateDefaultValueCallback**-Parameter enthält.

Das typische Implementierungsmuster für einen [**CreateDefaultValueCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.createdefaultvaluecallback) besteht im Erstellen einer neuen [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)-Klasse, dem Festlegen der spezifischen Eigenschaftswerte der einzelnen Eigenschaften von **DependencyObject** auf die beabsichtigen Standardwerte und der anschließenden Rückgabe der neuen Klasse als **Object**-Verweis über den Rückgabewert der Methode **CreateDefaultValueCallback**.

### <a name="property-changed-callback-method"></a>Rückrufmethode mit geänderter Eigenschaft

Sie können eine PropertyChanged-Rückrufmethode verwenden, um die Interaktionen Ihrer Eigenschaft mit anderen Abhängigkeitseigenschaften zu definieren oder eine interne Eigenschaft bzw. einen Status Ihres Objekts zu aktualisieren, sobald die Eigenschaft geändert wird. Wenn Ihr Rückruf aufgerufen wurde, hat das Eigenschaftensystem festgestellt, dass es eine effektive Änderung des Eigenschaftswerts gibt. Da die Rückrufmethode statisch ist, ist der *d*-Parameter des Rückrufs wichtig, da dieser anzeigt, welche Instanz der Klasse eine Änderung gemeldet hat. In einer typischen Implementierung wird die [**NewValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.newvalue)-Eigenschaft des Ereignisdatums verwendet und dieser Wert auf bestimmte Weise verarbeitet, in der Regel durch Ausführung einer anderen Änderung für das Objekt, das als *d* übergeben wurde. Weitere Reaktionen auf eine Eigenschaftenänderung sind die Zurückweisung des von **NewValue** gemeldeten Werts, die Wiederherstellung von [**OldValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.oldvalue) oder die Festlegung des Werts auf eine programmgesteuerte Einschränkung für den **NewValue**.

Das nächste Beispiel zeigt eine [**PropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.propertychangedcallback)-Implementierung. Hier wird die Methode implementiert, auf die in den vorigen [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)-Beispielen als Teil des Konstruktionsarguments für die [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata) Bezug genommen wurde. In diesem Rückrufszenario besitzt die Klasse auch eine berechnete schreibgeschützte Eigenschaft „HasLabelValue“ (Implementierung nicht gezeigt). Diese Rückrufmethode wird jedes Mal aufgerufen, wenn die Label-Eigenschaft neu ausgewertet wird. Der Rückruf ermöglicht die kontinuierliche Synchronisierung des abhängigen berechneten Werts mit den Änderungen der Abhängigkeitseigenschaft.

```csharp
private static void OnLabelChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    ImageWithLabelControl iwlc = d as ImageWithLabelControl; //null checks omitted
    String s = e.NewValue as String; //null checks omitted
    if (s == String.Empty)
    {
        iwlc.HasLabelValue = false;
    } else {
        iwlc.HasLabelValue = true;
    }
}
```

```vb
    Private Shared Sub OnLabelChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
        Dim iwlc As ImageWithLabelControl = CType(d, ImageWithLabelControl) ' null checks omitted
        Dim s As String = CType(e.NewValue,String) ' null checks omitted
        If s Is String.Empty Then
            iwlc.HasLabelValue = False
        Else
            iwlc.HasLabelValue = True
        End If
    End Sub
```

```cppwinrt
void ImageWithLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto iwlc{ d.as<ImageWithLabelControlApp::ImageWithLabelControl>() };
    auto s{ winrt::unbox_value<winrt::hstring>(e.NewValue()) };
    iwlc.HasLabelValue(s.size() != 0);
}
```

```cpp
static void OnLabelChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    ImageWithLabelControl^ iwlc = (ImageWithLabelControl^)d;
    Platform::String^ s = (Platform::String^)(e->NewValue);
    if (s->IsEmpty()) {
        iwlc->HasLabelValue=false;
    }
}
```

### <a name="property-changed-behavior-for-structures-and-enumerations"></a>Verhalten bei geänderter Eigenschaft für Strukturen und Enumerationen

Wenn der Typ einer [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty) eine Enumeration oder Struktur ist, kann der Rückruf auch aufgerufen werden, wenn die internen Werte der Struktur oder der Enumerationswert nicht geändert wurden. Dieses Verhalten weicht gegenüber einem Systemgrundtyp wie einer Zeichenfolge ab. Hier erfolgt der Aufruf nur bei einem geänderten Wert. Dies ist der Nebeneffekt eines internen Boxing- und Unboxing-Vorgangs für diese Werte. Wenn Sie über eine [**PropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.propertychangedcallback)-Methode für eine Eigenschaft verfügen, deren Wert eine Enumeration oder Struktur ist, müssen Sie [**OldValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.oldvalue) und [**NewValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencypropertychangedeventargs.newvalue) vergleichen. Dazu wandeln Sie die Werte selbst um und verwenden die überladenen Vergleichsoperatoren, die für die nun umgewandelten Werte verfügbar sind. Wenn kein entsprechender Operator verfügbar ist (beispielsweise weil es sich um eine benutzerdefinierte Struktur handelt), müssen Sie möglicherweise die einzelnen Werte vergleichen. Wenn Sie zu dem Ergebnis kommen, dass sich die Werte nicht geändert haben, ist normalerweise keine Aktion erforderlich.

```csharp
private static void OnVisibilityValueChanged(DependencyObject d, DependencyPropertyChangedEventArgs e) {
    if ((Visibility)e.NewValue != (Visibility)e.OldValue)
    {
        //value really changed, invoke your changed logic here
    } // else this was invoked because of boxing, do nothing
}
```

```vb
Private Shared Sub OnVisibilityValueChanged(d As DependencyObject, e As DependencyPropertyChangedEventArgs)
    If CType(e.NewValue,Visibility) != CType(e.OldValue,Visibility) Then
        '  value really changed, invoke your changed logic here
    End If
    '  else this was invoked because of boxing, do nothing
End Sub
```

```cppwinrt
static void OnVisibilityValueChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    auto oldVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.OldValue()) };
    auto newVisibility{ winrt::unbox_value<Windows::UI::Xaml::Visibility>(e.NewValue()) };

    if (newVisibility != oldVisibility)
    {
        // The value really changed; invoke your property-changed logic here.
    }
    // Otherwise, OnVisibilityValueChanged was invoked because of boxing; do nothing.
}
```

```cpp
static void OnVisibilityValueChanged(DependencyObject^ d, DependencyPropertyChangedEventArgs^ e)
{
    if ((Visibility)e->NewValue != (Visibility)e->OldValue)
    {
        //value really changed, invoke your changed logic here
    } 
    // else this was invoked because of boxing, do nothing
    }
}
```

## <a name="best-practices"></a>Empfohlene Methoden

Berücksichtigen Sie die folgenden Überlegungen als bewährte Methoden, wenn Sie Ihre benutzerdefinierte Abhängigkeitseigenschaft festlegen.

### <a name="dependencyobject-and-threading"></a>DependencyObject und Threading

Alle [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)-Instanzen müssen in dem Benutzeroberflächenthread erstellt werden, der mit dem aktuellen [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) verknüpft ist, das von einer Windows-Runtime-App angezeigt wird. Obwohl jedes **DependencyObject** im zentralen Benutzeroberflächenthread erstellt werden muss, kann durch Aufrufen von [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) mithilfe eines Verteilerverweises von anderen Threads aus auf die Objekte zugegriffen werden.

Die Threadingmerkmale von [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) sind relevant, da dies in der Regel bedeutet, dass nur im Benutzeroberflächenthread ausgeführter Code den Wert einer Abhängigkeitseigenschaft ändern oder auch nur lesen kann. Threadingprobleme können in typischem Benutzeroberflächencode in der Regel vermieden werden, wenn **async**-Muster und Hintergrundworkerthreads richtig verwendet werden. Threadingprobleme im Zusammenhang mit **DependencyObject** entstehen in der Regel nur, wenn Sie eigene **DependencyObject**-Typen definieren und versuchen, diese als Datenquellen oder für andere Szenarien zu verwenden, für die ein **DependencyObject** nicht notwendigerweise geeignet ist.

### <a name="avoiding-unintentional-singletons"></a>Vermeiden unbeabsichtigter Singletons

Ein unbeabsichtigter Singleton kann auftreten, wenn eine Abhängigkeitseigenschaft deklariert wird, die einen Verweistyp annimmt, und Sie einen Konstruktor für diesen Verweistyp als Teil des Codes aufrufen, der [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata) erstellt. In diesem Fall teilen sich alle Anwendungen der Abhängigkeitseigenschaft eine einzige Instanz von **PropertyMetadata** und versuchen daher, den einzelnen von Ihnen erstellten Verweistyp gemeinsam zu nutzen. Alle untergeordneten Eigenschaften dieses Werttyps, die Sie durch Ihre Abhängigkeitseigenschaft festlegen, werden anschließend auf andere Objekte übertragen, und zwar auf eine Weise, die Sie möglicherweise nicht beabsichtigt haben.

Sie können Klassenkonstruktoren verwenden, um erste Werte für eine Abhängigkeitseigenschaft vom Typ "Verweis" festzulegen, wenn Sie einen Nicht-null-Wert haben möchten. Beachten Sie jedoch, dass dies als lokaler Wert für die [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md) betrachtet werden würde. Es ist unter Umständen angemessener, eine Vorlage – falls diese von Ihrer Klasse unterstützt wird – für diesen Zweck zu verwenden. Eine andere Möglichkeit, um Singleton-Muster zu vermeiden, aber dennoch einen nützlichen Standardwert bereitzustellen, besteht darin, eine statische Eigenschaft für den Verweistyp, der einen passenden Standardwert für die Werte dieser Klasse liefert, verfügbar zu machen.

### <a name="collection-type-dependency-properties"></a>Abhängigkeitseigenschaften des Sammlungstyps

Bei Abhängigkeitseigenschaften vom Typ "Sammlung" sind einige zusätzliche Implementierungsaspekte zu beachten.

Abhängigkeitseigenschaften des Sammlungstyps sind in der Windows-Runtime-API vergleichsweise selten. In den meisten Fällen können Sie Sammlungen verwenden, deren Elemente eine [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)-Unterklasse sind. Die Sammlungseigenschaft selbst wird jedoch als herkömmliche CLR- oder eine C++-Eigenschaft implementiert. Das ist darauf zurückzuführen, dass sich Sammlungen nicht unbedingt für herkömmliche Szenarien eignen, bei denen Abhängigkeitseigenschaften involviert sind. Zum Beispiel:

- Sie animieren für gewöhnlich keine Sammlung.
- Sie füllen die Elemente einer Sammlung für gewöhnlich nicht vorher mit Stilen oder einer Vorlage aus.
- Obwohl das Binden an Sammlungen ein wichtiges Szenario ist, muss die Sammlung keine Abhängigkeitseigenschaft sein, um eine Bindungsquelle darzustellen. Im Fall von Bindungszielen werden in der Regel Unterklassen von [**ItemsControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ItemsControl) oder [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) verwendet, um Sammlungselemente zu unterstützen oder Modellanzeigemuster zu verwenden. Weitere Informationen zur Bindung zu und von Sammlungen finden Sie unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).
- Benachrichtigungen zu Sammlungsänderungen sollten besser durch Schnittstellen wie **INotifyPropertyChanged** oder **INotifyCollectionChanged** oder durch Ableiten des Sammlungstyps von [**ObservableCollection&lt;T&gt;** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1) behandelt werden.

Dennoch gibt es Szenarien für Abhängigkeitseigenschaften des Sammlungstyps. In den nächsten drei Abschnitten finden Sie Informationen zur Implementierung einer Abhängigkeitseigenschaft vom Typ "Sammlung".

### <a name="initializing-the-collection"></a>Initialisieren der Sammlung

Wenn Sie eine Abhängigkeitseigenschaft erstellen, geben Sie den Standardwert über die Metadaten für die Abhängigkeitseigenschaft an. Achten Sie darauf, keine einzelne statische Sammlung als Standardwert zu verwenden. Stattdessen müssen Sie sicherstellen, dass Sie den Sammlungswert bewusst auf eine eindeutige (Instanz-)Sammlung im Rahmen der Klassenkonstruktorlogik für die Besitzerklasse der Sammlungseigenschaft festlegen.

### <a name="change-notifications"></a>Änderungsbenachrichtigungen

Durch die Definition der Sammlung als Abhängigkeitseigenschaft werden nicht automatisch Änderungsbenachrichtigungen für die Elemente in der Sammlung bereitgestellt, indem das Eigenschaftensytem die Rückrufmethode „PropertyChanged“ aufruft. Wenn Sie Benachrichtigungen für Sammlungen oder Sammlungselemente einrichten möchten, beispielsweise für ein Datenbindungsszenario, implementieren Sie die **INotifyPropertyChanged**- oder **INotifyCollectionChanged**-Schnittstelle. Weitere Informationen finden Sie unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

### <a name="dependency-property-security-considerations"></a>Sicherheitsüberlegungen in Bezug auf Abhängigkeitseigenschaften

Deklarieren Sie Abhängigkeitseigenschaften als öffentliche Eigenschaften. Bezeichner von Abhängigkeitseigenschaften sollten als **public static readonly**-Member deklariert werden. Auch wenn Sie versuchen, andere durch eine Sprache zugelassene Zugriffsebenen (beispielsweise **protected**) zu deklarieren, können Sie stets auf eine Abhängigkeitseigenschaft zugreifen, indem Sie den Bezeichner in Verbindung mit den APIs des Eigenschaftensystems verwenden. Das Deklarieren des Bezeichners der Abhängigkeitseigenschaft als intern oder privat wird nicht funktionieren, da das Eigenschaftensystem in diesem Fall nicht ordnungsgemäß arbeitet.

Wrappereigenschaften sind nur als Vereinfachung gedacht. Die Sicherheitsmechanismen, die für die Wrapper angewendet werden, können durch das Aufrufen von [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue) oder [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) umgangen werden. Wrappereigenschaften sollten daher öffentlich bleiben. Andernfalls machen Sie die Nutzung Ihrer Eigenschaft für legitime Aufrufer schwieriger, ohne einen tatsächlichen Vorteil in Bezug auf die Sicherheit zu erhalten.

Die Windows-Runtime bietet keine Möglichkeit, eine benutzerdefinierte Abhängigkeitseigenschaft als schreibgeschützt zu registrieren.

### <a name="dependency-properties-and-class-constructors"></a>Abhängigkeitseigenschaften und Klassenkonstruktoren

Hier gilt der allgemeine Grundsatz, dass Klassenkonstruktoren keine virtuellen Methoden aufrufen sollten. Der Grund hierfür ist, dass Konstruktoren als Basisinitialisierung eines abgeleiteten Klassenkonstruktors aufgerufen werden können, und das Eingeben der virtuellen Methode über den Konstruktor könnte zu einem Zeitpunkt erfolgen, zu dem das erstellte Objekt einen unvollständigen Initialisierungszustand aufweist. Wenn Sie eine Ableitung von einer Klasse durchführen, die bereits von [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) abgeleitet ist, sollten Sie daran denken, dass das Eigenschaftensystem selbst als Teil seiner Dienste virtuelle Methoden intern aufruft und offenlegt. Um mögliche Probleme mit der Laufzeitinitialisierung zu vermeiden, sollten Sie in Konstruktoren von Klassen keine Werte von Abhängigkeitseigenschaften festlegen.

### <a name="registering-the-dependency-properties-for-ccx-apps"></a>Registrieren der Abhängigkeitseigenschaften für C++/CX-Apps

Die Implementierung für das Registrieren einer Eigenschaft in C++/CX ist schwieriger als für C#, zum einen aufgrund der Aufteilung in Kopfzeile und Implementierungsdatei und zum anderen, weil die Initialisierung im Stammbereich der Implementierungsdatei nicht empfohlen wird. (Visual C++ Component Extensions (C++/CX) fügt statischen initialisierercode aus dem Stamm Bereich direkt in **DllMain**ein C# , wohingegen Compiler die statischen Initialisierer den Klassen zuweisen und somit **DllMain** -Probleme bei der Lade Sperre vermeiden.). Die bewährte Methode besteht hier im Deklarieren einer Hilfsfunktion, die die gesamte Registrierung von Abhängigkeitseigenschaften für eine Klasse durchführt, d. h. eine Funktion pro Klasse. Für jede benutzerdefinierte Klasse, die Ihre App nutzt, müssen Sie dann auf die Hilfsregistrierungsfunktion verweisen, die von den einzelnen benutzerdefinierten Klassen jeweils verfügbar gemacht wird, die Sie verwenden möchten. Rufen Sie jede Hilfsregistrierungsfunktion einmalig als Teil des [**Application constructor**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.-ctor) (`App::App()`) vor `InitializeComponent` auf. Dieser Konstruktor wird nur ausgeführt, wenn wirklich zum ersten Mal auf die App verwiesen wir. Er wird nicht erneut ausgeführt, wenn beispielsweise eine angehaltene App fortgesetzt wird. Wie im vorherigen C++-Registrierungsbeispiel gezeigt, ist auch die **nullptr**-Überprüfung jedes [**Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)-Aufrufs wichtig, da hierdurch sichergestellt wird, dass kein Aufrufer der Funktion die Eigenschaft zweimal registrieren kann. Bei einem zweiten Registrierungsaufruf würde Ihre App ohne eine solche Überprüfung wahrscheinlich abstürzen, da der Eigenschaftsname doppelt vorhanden wäre. Dieses Implementierungsmuster können Sie im [XAML-Beispiel für Benutzer und benutzerdefinierte Steuerelemente](https://code.msdn.microsoft.com/windowsapps/XAML-user-and-custom-a8a9505e) im Code für die C++/CX-Version des Beispiels sehen.

## <a name="related-topics"></a>Verwandte Themen

- [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)
- [**DependencyProperty. Register**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyproperty.register)
- [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md)
- [XAML-Beispiel für Benutzer und benutzerdefinierte Steuerelemente](https://code.msdn.microsoft.com/windowsapps/XAML-user-and-custom-a8a9505e)
 