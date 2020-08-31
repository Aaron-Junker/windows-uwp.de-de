---
description: Erläutert, wie eine angefügte XAML-Eigenschaft als Abhängigkeitseigenschaft implementiert und die Accessorkonvention definiert wird, die erforderlich ist, damit die angefügte Eigenschaft in XAML verwendet werden kann.
title: Benutzerdefinierte angefügte Eigenschaften
ms.assetid: E9C0C57E-6098-4875-AA3E-9D7B36E160E0
ms.date: 07/18/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 0512c4c599180144cc16148044e8722597411edd
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155124"
---
# <a name="custom-attached-properties"></a>Benutzerdefinierte angefügte Eigenschaften

Eine *angefügte Eigenschaft* ist ein XAML-Konzept. Angefügte Eigenschaften werden in der Regel als eine spezialisierte Form der Abhängigkeitseigenschaft definiert. In diesem Thema wird erläutert, wie eine angefügte Eigenschaft als Abhängigkeitseigenschaft implementiert und die Accessorkonvention definiert wird, die erforderlich ist, damit die angefügte Eigenschaft in XAML verwendet werden kann.

## <a name="prerequisites"></a>Voraussetzungen

Wir gehen davon aus, dass Sie die Abhängigkeitseigenschaften aus der Perspektive eines Consumers vorhandener Abhängigkeitseigenschaften sehen und dass Sie die [Übersicht über Abhängigkeitseigenschaften](dependency-properties-overview.md) gelesen haben. Sie sollten auch die [Übersicht über angefügte Eigenschaften](attached-properties-overview.md) gelesen haben. Für ein besseres Verständnis der in diesem Thema aufgeführten Beispiele sollten Sie XAML verstehen und wissen, wie eine einfache Windows-Runtime-App mit C++, C# oder Visual Basic geschrieben wird.

## <a name="scenarios-for-attached-properties"></a>Szenarien für angefügte Eigenschaften

Möglicherweise möchten Sie eine angefügte Eigenschaft erstellen, wenn es einen Grund für das Vorhandensein eines Mechanismus zum Festlegen von Eigenschaften für Klassen außer der definierenden Klasse gibt. Die häufigsten Szenarien dafür sind Layout- und Dienstunterstützung. Beispiele für vorhandene Layouteigenschaften sind [**Canvas.ZIndex**](/previous-versions/windows/silverlight/dotnet-windows-silverlight/cc190397(v=vs.95)) und [**Canvas.Top**](/dotnet/api/system.windows.controls.canvas.top). In einem Layoutszenario können Elemente, die als untergeordnete Elemente für layoutsteuernde Elemente fungieren, einzeln Layoutanforderungen gegenüber ihren übergeordneten Elementen ausdrücken. Dabei legt jedes Element einen Eigenschaftswert fest, den das übergeordnete Element als eine angefügte Eigenschaft definiert. Ein Beispiel für das Dienstunterstützungsszenario in der Windows-Runtime-API ist das Festlegen der angefügten Eigenschaften von [**ScrollViewer**](/uwp/api/Windows.UI.Xaml.Controls.ScrollViewer), z. B. [**ScrollViewer.IsZoomChainingEnabled**](/uwp/api/windows.ui.xaml.controls.scrollviewer.iszoomchainingenabled).

> [!WARNING]
> Eine vorhandene Einschränkung der Windows-Runtime XAML-Implementierung besteht darin, dass Sie die benutzerdefinierte angefügte Eigenschaft nicht animieren können.

## <a name="registering-a-custom-attached-property"></a>Registrieren einer benutzerdefinierten angefügten Eigenschaft

Wenn Sie die angefügte Eigenschaft immer für die Verwendung mit anderen Typen definieren, muss die Klasse, in der die Eigenschaft registriert ist, nicht aus [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) abgeleitet werden. Die Zielparameter für Accessoren müssen jedoch **DependencyObject** verwenden, wenn Sie das typische Modell befolgen, bei dem Ihre angefügte Eigenschaft auch eine Abhängigkeitseigenschaft sein kann, sodass Sie den Sicherungseigenschaftsspeicher verwenden können.

Sie definieren Ihre angefügte Eigenschaft als Abhängigkeitseigenschaft, indem Sie eine **public** **static** **readonly**-Eigenschaft des Typs [**DependencyProperty**](/uwp/api/Windows.UI.Xaml.DependencyProperty) deklarieren. Sie definieren diese Eigenschaft mithilfe des Rückgabewerts der [**RegisterAttached**](/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)-Methode. Der Eigenschaftenname muss mit dem Namen der angefügten Eigenschaft übereinstimmen, die Sie als **RegisterAttached** *name*-Parameter angeben. Am Ende muss die Zeichenfolge „Property“ hinzugefügt sein. Dies ist die bestehende Konvention zum Benennen der Bezeichner von Abhängigkeitseigenschaften im Verhältnis mit den Eigenschaften, die sie darstellen.

Der Hauptunterschied beim Definieren einer benutzerdefinierten Eigenschaft und beim Definieren einer benutzerdefinierten Abhängigkeitseigenschaft besteht darin, wie Sie die Accessoren oder Wrapper definieren. Anstatt die unter [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md) beschriebene Wrappertechnik zu verwenden, müssen Sie auch statische **Get**_PropertyName_- und **Set**_PropertyName_-Methoden als Accessoren für die angefügte Eigenschaft bereitstellen. Die Accessoren werden hauptsächlich vom XAML-Parser verwendet, obwohl sie auch von anderen aufrufenden Elementen verwendet werden können, um Werte in Nicht-XAML-Szenarien festzulegen.

> [!IMPORTANT]
> Wenn Sie die Accessoren nicht ordnungsgemäß definieren, kann der XAML-Prozessor nicht auf die angefügte Eigenschaft zugreifen, und jeder, der versucht, ihn zu verwenden, erhält wahrscheinlich einen XAML-Parserfehler. Entwurfs-und Codierungs Tools basieren außerdem häufig auf den " \* Property"-Konventionen für Benennungs Bezeichner, wenn Sie eine benutzerdefinierte Abhängigkeits Eigenschaft in einer Assembly finden, auf die verwiesen wird.

## <a name="accessors"></a>Accessoren

Die Signatur für den **Get**_PropertyName_-Accessor lautet folgendermaßen.

`public static` _valueType_ **Get**_PropertyName_ `(DependencyObject target)`

Bei Microsoft Visual Basic lautet sie folgendermaßen.

`Public Shared Function Get`_PropertyName_ `(ByVal target As DependencyObject) As ` _ValueType_`)`

Das *target*-Objekt kann in Ihrer Implementierung einen spezielleren Typ aufweisen, muss jedoch von [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) abgeleitet sein. Der *valueType*-Rückgabewert kann in Ihrer Implementierung auch einen spezielleren Typ aufweisen. Der grundlegende **Object**-Typ wird akzeptiert. Häufig soll jedoch Ihre angefügte Eigenschaft Typsicherheit erzwingen. Die Verwendung der Typisierung in den Getter- und Setter-Signaturen ist eine empfohlene Typsicherheitstechnik.

Die Signatur für den **Set**_PropertyName_-Accessor lautet folgendermaßen.

`public static void Set`_PropertyName_ ` (DependencyObject target , ` _ValueType_` value)`

Bei Microsoft Visual Basic lautet sie folgendermaßen.

`Public Shared Sub Set`_PropertyName_ ` (ByVal target As DependencyObject, ByVal value As ` _ValueType_`)`

Das *target*-Objekt kann in Ihrer Implementierung einen spezielleren Typ aufweisen, muss jedoch von [**DependencyObject**](/uwp/api/Windows.UI.Xaml.DependencyObject) abgeleitet sein. Das *value*-Objekt und dessen *valueType* können in Ihrer Implementierung einen spezielleren Typ aufweisen. Beachten Sie, dass der Wert für diese Methode die Eingabe ist, die vom XAML-Prozessor stammt, wenn er Ihre angefügte Eigenschaft im Markup feststellt. Die Typkonvertierung oder vorhandene Markuperweiterung muss für den von Ihnen verwendeten Typ unterstützt werden, damit der entsprechende Typ auf der Grundlage eines Attributwerts (der letztlich nur eine Zeichenfolge ist) erstellt werden kann. Der grundlegende **Object**-Typ ist akzeptabel. Häufig möchten Sie jedoch zusätzliche Typsicherheit erreichen. Versehen Sie hierzu die Accessoren mit einer Typerzwingung.

> [!NOTE]
> Es ist auch möglich, eine angefügte Eigenschaft zu definieren, bei der die beabsichtigte Verwendung über die Syntax von Eigenschafts Elementen erfolgt. In diesem Fall benötigen Sie keine Typkonvertierung für die Werte. Sie müssen allerdings sicherstellen, dass die beabsichtigten Werte in XAML konstruierbar sind. [**VisualStateManager.VisualStateGroups**](/dotnet/api/system.windows.visualstatemanager) ist ein Beispiel für eine vorhandene angefügte Eigenschaft, die nur die Eigenschaftselementverwendung unterstützt.

## <a name="code-example"></a>Codebeispiel

In diesem Beispiel werden die Abhängigkeitseigenschaftsregistrierung (mithilfe der [**RegisterAttached**](/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)-Methode) sowie die Accessoren **Get** und **Set** für eine benutzerdefinierte angefügte Eigenschaft dargestellt. Im Beispiel ist der Name der angefügten Eigenschaft `IsMovable`. Deshalb müssen die Accessoren `GetIsMovable` und `SetIsMovable` genannt werden. Der Besitzer der angefügten Eigenschaft ist eine Dienstklasse mit dem Namen `GameService`, die nicht über eine eigene Benutzeroberfläche verfügt. Sie dient lediglich zur Bereitstellung der Dienste der angefügten Eigenschaft, wenn die angefügte Eigenschaft **GameService.IsMovable** verwendet wird.

Das Definieren der angefügten-Eigenschaft in C++/CX ist etwas komplexer. Sie müssen entscheiden, wie die Faktorverteilung zwischen der Header- und Codedatei lauten soll. Außerdem sollten Sie den Bezeichner als Eigenschaft mit nur einem **get**-Accessor verfügbar machen. Die Gründe hierfür werden unter [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md) erläutert. In C++/CX müssen Sie diese Eigenschafts Feld Beziehung explizit definieren, anstatt sich auf die Schreib **geschützte Schlüssel Formulierung** von .net und die implizite Unterstützung von einfachen Eigenschaften zu verlassen. Sie müssen auch die Registrierung der angefügten Eigenschaft innerhalb einer Hilfsfunktion durchführen, die nur einmal ausgeführt wird; dies erfolgt beim ersten Start der App, jedoch bevor XAML-Seiten geladen werden, die die angefügte Eigenschaft benötigen. Der typische Ort, um die Hilfsobjekte für die Eigenschaften Registrierung für alle Abhängigkeiten oder angefügten Eigenschaften aufzurufen, liegt innerhalb des **App**-  /  [**anwendungskonstruktors**](/uwp/api/windows.ui.xaml.application.-ctor) im Code für die Datei "App. XAML".

```csharp
public class GameService : DependencyObject
{
    public static readonly DependencyProperty IsMovableProperty = 
    DependencyProperty.RegisterAttached(
      "IsMovable",
      typeof(Boolean),
      typeof(GameService),
      new PropertyMetadata(false)
    );
    public static void SetIsMovable(UIElement element, Boolean value)
    {
        element.SetValue(IsMovableProperty, value);
    }
    public static Boolean GetIsMovable(UIElement element)
    {
        return (Boolean)element.GetValue(IsMovableProperty);
    }
}
```

```vb
Public Class GameService
    Inherits DependencyObject

    Public Shared ReadOnly IsMovableProperty As DependencyProperty = 
        DependencyProperty.RegisterAttached("IsMovable",  
        GetType(Boolean), 
        GetType(GameService), 
        New PropertyMetadata(False))

    Public Shared Sub SetIsMovable(ByRef element As UIElement, value As Boolean)
        element.SetValue(IsMovableProperty, value)
    End Sub

    Public Shared Function GetIsMovable(ByRef element As UIElement) As Boolean
        GetIsMovable = CBool(element.GetValue(IsMovableProperty))
    End Function
End Class
```

```cppwinrt
// GameService.idl
namespace UserAndCustomControls
{
    [default_interface]
    runtimeclass GameService : Windows.UI.Xaml.DependencyObject
    {
        GameService();
        static Windows.UI.Xaml.DependencyProperty IsMovableProperty{ get; };
        static Boolean GetIsMovable(Windows.UI.Xaml.DependencyObject target);
        static void SetIsMovable(Windows.UI.Xaml.DependencyObject target, Boolean value);
    }
}

// GameService.h
...
    static Windows::UI::Xaml::DependencyProperty IsMovableProperty() { return m_IsMovableProperty; }
    static bool GetIsMovable(Windows::UI::Xaml::DependencyObject const& target) { return winrt::unbox_value<bool>(target.GetValue(m_IsMovableProperty)); }
    static void SetIsMovable(Windows::UI::Xaml::DependencyObject const& target, bool value) { target.SetValue(m_IsMovableProperty, winrt::box_value(value)); }

private:
    static Windows::UI::Xaml::DependencyProperty m_IsMovableProperty;
...

// GameService.cpp
...
Windows::UI::Xaml::DependencyProperty GameService::m_IsMovableProperty =
    Windows::UI::Xaml::DependencyProperty::RegisterAttached(
        L"IsMovable",
        winrt::xaml_typename<bool>(),
        winrt::xaml_typename<UserAndCustomControls::GameService>(),
        Windows::UI::Xaml::PropertyMetadata{ winrt::box_value(false) }
);
...
```

```cpp
// GameService.h
#pragma once

#include "pch.h"
//namespace WUX = Windows::UI::Xaml;

namespace UserAndCustomControls {
    public ref class GameService sealed : public WUX::DependencyObject {
    private:
        static WUX::DependencyProperty^ _IsMovableProperty;
    public:
        GameService::GameService();
        void GameService::RegisterDependencyProperties();
        static property WUX::DependencyProperty^ IsMovableProperty
        {
            WUX::DependencyProperty^ get() {
                return _IsMovableProperty;
            }
        };
        static bool GameService::GetIsMovable(WUX::UIElement^ element) {
            return (bool)element->GetValue(_IsMovableProperty);
        };
        static void GameService::SetIsMovable(WUX::UIElement^ element, bool value) {
            element->SetValue(_IsMovableProperty,value);
        }
    };
}

// GameService.cpp
#include "pch.h"
#include "GameService.h"

using namespace UserAndCustomControls;

using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::Foundation::Collections;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Data;
using namespace Windows::UI::Xaml::Documents;
using namespace Windows::UI::Xaml::Input;
using namespace Windows::UI::Xaml::Interop;
using namespace Windows::UI::Xaml::Media;

GameService::GameService() {};

GameService::RegisterDependencyProperties() {
    DependencyProperty^ GameService::_IsMovableProperty = DependencyProperty::RegisterAttached(
         "IsMovable", Platform::Boolean::typeid, GameService::typeid, ref new PropertyMetadata(false));
}
```

## <a name="setting-your-custom-attached-property-from-xaml-markup"></a>Festlegen der benutzerdefinierten angefügten Eigenschaft aus dem XAML-Markup

> [!NOTE]
> Wenn Sie C++/WinRT verwenden, fahren Sie mit dem folgenden Abschnitt fort ([Festlegen der benutzerdefinierten angefügten Eigenschaft Imperativ mit C++/WinRT](#setting-your-custom-attached-property-imperatively-with-cwinrt)).

Nachdem Sie die angefügte Eigenschaft definiert und ihre unterstützenden Elemente als Teil des benutzerdefinierten Typs eingefügt haben, müssen Sie die Definitionen anschließend für die Verwendung von XAML verfügbar machen. Dazu müssen Sie einen XAML-Namespace zuordnen, der auf den Codenamespace mit der relevanten Klasse verweist. In Fällen, in denen Sie die angefügte Eigenschaft als Teil einer Bibliothek definiert haben, müssen Sie diese Bibliothek als Teil des App-Pakets für die App einfügen.

Eine XML-Namespacezuordnung wird in der Regel im Stammelement einer XAML-Seite platziert. Für die Klasse mit dem Namen `GameService` im Namespace `UserAndCustomControls`, der die in den vorherigen Ausschnitten dargestellten Definitionen der angefügten Eigenschaften enthält, sieht die Zuordnung z. B. folgendermaßen aus.

```xaml
<UserControl
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:uc="using:UserAndCustomControls"
  ... >
```

Mithilfe der Zuordnung können Sie die angefügte `GameService.IsMovable` -Eigenschaft für beliebige Elemente festlegen, die Ihrer Zieldefinition entsprechen. Dies schließt auch einen vorhandenen von Windows-Runtime definierten Typ mit ein.

```xaml
<Image uc:GameService.IsMovable="True" .../>
```

Wenn Sie die Eigenschaft für ein Element festlegen, das sich auch im selben zugeordneten XML-Namespace befindet, müssen Sie das Präfix dennoch in den Namen der angefügten Eigenschaft einfügen. Das liegt daran, dass das Präfix den Besitzertyp qualifiziert. Es kann nicht angenommen werden, dass sich das Attribut der angefügten Eigenschaft innerhalb desselben XML-Namespace wie das Element befindet, in dem das Attribut enthalten ist, auch wenn Attribute gemäß normalen XML-Regeln Namespaces von Elementen erben können. Wenn Sie beispielsweise `GameService.IsMovable` für einen benutzerdefinierten Typ von `ImageWithLabelControl` (Definition nicht dargestellt) festlegen, sieht das XAML-Element nach wie vor folgendermaßen aus, auch wenn beide im selben Codenamespace definiert und demselben Präfix zugeordnet sind.

```xaml
<uc:ImageWithLabelControl uc:GameService.IsMovable="True" .../>
```

> [!NOTE]
> Wenn Sie eine XAML-Benutzeroberfläche mit C++/CX schreiben, müssen Sie den-Header für den benutzerdefinierten Typ einschließen, der die angefügte-Eigenschaft definiert, wenn eine XAML-Seite diesen Typ verwendet. Jede XAML-Seite verfügt über einen zugeordneten Code-Behind-Header (. XAML. h). An dieser Stelle sollten Sie den Header für die Definition des Besitzer Typs der angefügten Eigenschaft einschließen (mithilfe von ** \# include**).

## <a name="setting-your-custom-attached-property-imperatively-with-cwinrt"></a>Imperativ Festlegen der benutzerdefinierten angefügten Eigenschaft mit C++/WinRT

Wenn Sie C++/WinRT verwenden, können Sie auf eine benutzerdefinierte angefügte Eigenschaft aus imperativem Code, jedoch nicht aus XAML-Markup zugreifen. Der folgende Code zeigt die Vorgehensweise.

```xaml
<Image x:Name="gameServiceImage"/>
```

```cppwinrt
// MainPage.h
...
#include "GameService.h"
...

// MainPage.cpp
...
MainPage::MainPage()
{
    InitializeComponent();

    GameService::SetIsMovable(gameServiceImage(), true);
}
...
```

## <a name="value-type-of-a-custom-attached-property"></a>Werttyp einer benutzerdefinierten angefügten Eigenschaft

Der Typ, der als Werttyp einer benutzerdefinierten angefügten Eigenschaft verwendet wird, wirkt sich auf die Nutzung, die Definition oder auf beides aus. Der Werttyp der angefügten Eigenschaft wird an verschiedenen Stellen deklariert: in den Signaturen der **Get**- und **Set**-Accessormethoden und im *propertyType*-Parameter des [**RegisterAttached**](/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)-Aufrufs.

Der häufigste Werttyp für angefügte Eigenschaften (benutzerdefinierte oder andere) ist eine einfache Zeichenfolge. Das liegt daran, dass angefügte Eigenschaften in der Regel für die Nutzung mit XAML-Attributen vorgesehen sind, und durch die Verwendung einer Zeichenfolge als Werttyp bleiben die Eigenschaften einfach. Andere Grundtypen, die über eine systemeigene Konvertierung in Zeichenfolgenmethoden verfügen (z. B. eine ganze Zahl, ein Double oder ein Enumerationswert), werden ebenfalls häufig als Werttypen für angefügte Eigenschaften verwendet. Sie können andere Werttypen – die die systemeigene Zeichenfolgekonvertierung nicht unterstützen – als Wert der angefügten Eigenschaft verwenden. Dies hat jedoch zur Folge, dass eine Auswahl in Bezug auf die Nutzung oder Implementierung getroffen werden muss:

- Sie können die angefügte Eigenschaft so belassen, wie sie ist. Dann kann die angefügte Eigenschaft die Nutzung nur unterstützen, wenn die angefügte Eigenschaft ein Eigenschaftselement darstellt und der Wert als Objektelement deklariert wurde. In diesem Fall muss der Eigenschaftstyp die XAML-Nutzung als Objektelement unterstützen. Überprüfen Sie bei vorhandenen Windows-Runtime-Referenzklassen die XAML-Syntax, um sicherzustellen, dass der Typ die XAML-Objektelementnutzung unterstützt.
- Sie können die angefügte Eigenschaft so belassen, wie sie ist. Dann können Sie diese jedoch nur in einer Attributnutzung über eine XAML-Referenztechnik wie **Binding** oder **StaticResource** verwenden, die als Zeichenfolge ausgedrückt werden kann.

## <a name="more-about-the-canvasleft-example"></a>Weitere Informationen zum **Canvas.Left**-Beispiel

In vorherigen Beispielen für die Nutzung angefügter Eigenschaften wurden verschiedene Methoden zum Festlegen der angefügten Eigenschaft [**Canvas.Left**](/dotnet/api/system.windows.controls.canvas.left) gezeigt. Doch wie wirkt sich dies auf die Interaktionen einer [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) mit Ihrem Objekt aus, und wann tritt dies auf? Wir schauen uns dieses bestimmte Beispiel genauer an. Es ist beim Implementieren einer angefügten Eigenschaft interessant zu wissen, wozu eine typische Besitzerklasse der angefügten Eigenschaft die Werte der angefügten Eigenschaft außerdem verwendet, wenn sie diese in anderen Objekten findet.

Eine [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) dient hauptsächlich dazu, einen Layoutcontainer mit absoluter Position auf der UI darzustellen. Die untergeordneten Elemente einer **Canvas** werden in der definierten Basisklasseneigenschaft [**Children**](/uwp/api/windows.ui.xaml.controls.panel.children) gespeichert. **Canvas** verwendet als einziges Panel eine absolute Positionierung. Das Objektmodell des allgemeinen [**UIElement**](/uwp/api/Windows.UI.Xaml.UIElement)-Typs würde übermäßig aufgebläht werden, wenn Eigenschaften hinzugefügt werden, die nur für **Canvas** und die speziellen **UIElement**-Fälle wichtig wären, in denen diese untergeordnete Elemente eines **UIElement** sind. Wenn Sie die Eigenschaften von Layoutsteuerelementen einer **Canvas** als angefügte Eigenschaften definieren, die von allen **UIElement**-Elementen verwendet werden können, ist das Objektmodell übersichtlicher.

Um als Panel praktikabel zu sein, besitzt [**Canvas**](/uwp/api/Windows.UI.Xaml.Controls.Canvas) Verhaltensweisen, die die Frameworkmethoden [**Measure**](/uwp/api/windows.ui.xaml.uielement.measure) und [**Arrange**](/uwp/api/windows.ui.xaml.uielement.arrange) überschreiben. Hier wird von **Canvas** nach Werten der angefügten Eigenschaftswerte für ihre untergeordneten Elemente gesucht. Ein Teil der Muster **Measure** und **Arrange** ist eine Schleife, die alle Inhalte durchläuft. Ein Panel besitzt die Eigenschaft [**Children**](/uwp/api/windows.ui.xaml.controls.panel.children), die explizit angibt, was als untergeordnetes Element eines Panels gelten soll. Das **Canvas**-Layoutverhalten durchläuft diese untergeordneten Elemente und führt statische [**Canvas.GetLeft**](/uwp/api/windows.ui.xaml.controls.canvas.getleft)- und [**Canvas.GetTop**](/uwp/api/windows.ui.xaml.controls.canvas.gettop)-Aufrufe für alle untergeordneten Elemente aus, um zu ermitteln, ob diese angefügten Eigenschaften einen nicht standardmäßigen Wert enthalten (der standardmäßige Wert ist 0). Mithilfe dieser Werte werden die untergeordneten Elemente auf der verfügbaren **Canvas**-Layoutfläche gemäß den von den einzelnen untergeordneten Elementen bereitgestellten spezifischen Werten absolut positioniert. Anschließend wird mithilfe von **Arrange** ein Commit für sie ausgeführt.

Der Code sieht in etwa wie dieser Pseudocode aus.

```syntax
protected override Size ArrangeOverride(Size finalSize)
{
    foreach (UIElement child in Children)
    {
        double x = (double) Canvas.GetLeft(child);
        double y = (double) Canvas.GetTop(child);
        child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
    }
    return base.ArrangeOverride(finalSize); 
    // real Canvas has more sophisticated sizing
}
```

> [!NOTE]
> Weitere Informationen zur Funktionsweise von Panels finden Sie unter [Übersicht über XAML Custom Panels](../design/layout/custom-panels-overview.md).

## <a name="related-topics"></a>Zugehörige Themen

* [**RegisterAttached**](/uwp/api/windows.ui.xaml.dependencyproperty.registerattached)
* [Übersicht über angefügte Eigenschaften](attached-properties-overview.md)
* [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md)
* [Übersicht über XAML](xaml-overview.md)