---
description: Führt die XAML-Sprache und XAML-Konzepte für die Windows-Runtime-App-Entwickler Audience ein und beschreibt die verschiedenen Methoden zum Deklarieren von Objekten und zum Festlegen von Attributen in XAML, da diese zum Erstellen einer Windows-Runtime-App verwendet werden.
title: Übersicht über XAML
ms.assetid: 48041B37-F1A8-44A4-BB8E-1D4DE30E7823
ms.date: 07/18/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: 6d45e70afa5b0dc6e903dbd253c09042bb2046c2
ms.sourcegitcommit: f7ef7e894d7b7fc24483b4485605686abf8f2e93
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/30/2019
ms.locfileid: "71679210"
---
# <a name="xaml-overview"></a>Übersicht über XAML

In diesem Artikel werden die XAML-Sprache und XAML-Konzepte der Windows-Runtime-App-Entwicklergruppe vorgestellt. Außerdem werden die verschiedenen Möglichkeiten beschrieben, Objekte zu deklarieren und Attribute in XAML festzulegen, da Sie zum Erstellen einer Windows-Runtime-App verwendet werden.

## <a name="what-is-xaml"></a>Was ist XAML?

XAML (Extensible Application Markup Language) ist eine deklarative Programmiersprache. Insbesondere kann XAML Objekte initialisieren und Eigenschaften von Objekten mithilfe einer Sprachstruktur festlegen, die hierarchische Beziehungen zwischen mehreren Objekten und einer Unterstützungs-typkonvention anzeigt, die die Erweiterung von Typen unterstützt. Sie können sichtbare UI-Elemente im deklarativen XAML-Markup erstellen. Anschließend können Sie eine separate CodeBehind-Datei für jede XAML-Datei zuordnen, die auf Ereignisse reagieren und die ursprünglich in XAML deklarierten Objekte verändern kann.

Die XAML-Sprache unterstützt den Austausch von Quellen zwischen verschiedenen Tools und Rollen im Entwicklungsprozess, z. b. das Austauschen von XAML-Quellen zwischen Entwurfs Tools und einer interaktiven Entwicklungsumgebung (IDE) oder zwischen primären Entwicklern und Lokalisierung. Anbietern. Durch die Verwendung von XAML als Austauschformat können Designerrollen und Entwicklerrollen separat gehalten oder zusammengeführt und von Designern und Entwicklern während der Produktion einer App durchlaufen werden.

Im Rahmen Ihrer Windows-Runtime-App-Projekte handelt es sich bei den XAML-Dateien um XML-Dateien mit der Dateinamenerweiterung XAML.

## <a name="basic-xaml-syntax"></a>Grundlegende XAML-Syntax

XAML verfügt über eine grundlegende Syntax, die auf dem XML-Format basiert. Der Definition nach muss es sich bei gültigen XAML-Daten auch um gültige XML-Daten handeln. XAML verfügt jedoch auch über Syntax Konzepte, denen eine andere und umfassendere Bedeutung zugewiesen wird, während Sie in XML gemäß der XML 1,0-Spezifikation gültig sind. Beispielsweise unterstützt XAML die *Syntax von Eigenschaftselementen*, in der Eigenschaftswerte innerhalb von Elementen festgelegt werden können, anstatt über Zeichenfolgenwerte in Attributen oder Inhalten. Für das normale XML-Format ist ein XAML-Eigenschaftselement ein Element mit einem Punkt im Namen, damit es für einfaches XML gültig ist, jedoch nicht die gleiche Bedeutung hat.

## <a name="xaml-and-visual-studio"></a>XAML und Visual Studio

Microsoft Visual Studio erleichtert das Erstellen gültiger XAML-Syntax, sowohl im XAML-Text-Editor als auch auf der eher grafikorientierten XAML-Designoberfläche. Wenn Sie XAML für Ihre APP mit Visual Studio schreiben, machen Sie sich keine Gedanken über die Syntax mit den einzelnen Tastatureingaben. Die IDE fördert eine gültige XAML-Syntax durch Bereitstellung von Hinweisen zur automatischen Vervollständigung, die Vorschläge in Microsoft IntelliSense-Listen und Dropdown Listen, die Anzeige von UI-Element Bibliotheken im Fenster **Toolbox** oder andere Techniken. Wenn Sie das erste Mal mit XAML vertraut sind, kann es hilfreich sein, die Syntax Regeln zu kennen und insbesondere die Terminologie, die manchmal verwendet wird, um die Einschränkungen oder Optionen beim Beschreiben der XAML-Syntax in Bezug auf andere Themen zu beschreiben. Die Feinheiten der XAML-Syntax werden in einem separaten Thema, dem [XAML-Syntax Handbuch](xaml-syntax-guide.md), behandelt.

## <a name="xaml-namespaces"></a>XAML-Namespaces

Bei der allgemeinen Programmierung handelt es sich bei einem Namespace um ein Organisationskonzept, mit dem festgelegt wird, wie Bezeichner für Programmierentitäten interpretiert werden. Wenn Sie Namespaces verwenden, kann ein Programmierframework benutzerdeklarierte von frameworkdeklarierten Bezeichnern unterscheiden, Mehrdeutigkeiten bei Bezeichnern durch Namespacequalifizierung auflösen und Regeln für die Bereichsdefinition von Namen durchsetzen usw. XAML verfügt in Bezug auf die XAML-Sprache über ein entsprechendes eigenes XAML-Namespace-Konzept. Im Folgenden wird erklärt, wie XAML die Konzepte für den XML-Sprachnamespace anwendet und erweitert:

- Für XAML wird das reservierte XML-Attribut **xmlns** für Namespacedeklarationen verwendet. Der Wert des Attributs ist normalerweise ein Uniform Resource Identifier (URI). Dies ist eine von XML geerbte Konvention.
- XAML nutzt Präfixdeklarationen, um nicht standardmäßige Namespaces zu deklarieren, und außerdem Präfixverwendungen in Elementen und Attributen, um auf den Namespace zu verweisen.
- Für XAML wird ein Standardnamespace verwendet, der als Namespace eingesetzt wird, wenn bei einer Nutzung oder Deklaration kein Präfix vorhanden ist. Der Standardnamespace kann für jedes XAML-Programmierframework unterschiedlich definiert werden.
- Namespacedefinitionen erben in einer XAML-Datei oder einem -Konstrukt vom übergeordneten zum untergeordneten Element. Wenn Sie z. b. einen Namespace im Stamm Element einer XAML-Datei definieren, erben alle Elemente in dieser Datei diese Namespace Definition. Wenn ein Element weiter unten auf der Seite den Namespace neu definiert, übernehmen die Nachfolgerelemente dieses Elements die neue Definition.
- Attribute eines Elements erben dessen Namespaces. Es ist unüblich, XAML-Attribute mit Präfixen zu verwenden.

Eine XAML-Datei deklariert beinahe immer einen Standard-XAML-Namespace im Stammelement. Der Standard-XAML-Namespace definiert, welche Elemente deklariert werden können, ohne sie durch ein Präfix zu qualifizieren. Bei typischen Windows-Runtime-App-Projekten enthält dieser Standardnamespace das gesamte integrierte XAML-Vokabular für die Windows-Runtime zur Definition der UI: Standardsteuerelemente, Textelemente, XAML-Grafiken und -Animationen, Datenbindungs- und Formatierungsunterstützungstypen usw. Beim Schreiben von XAML-Code für Windows-Runtime-Apps können Sie die Verwendung von XAML-Namespaces und -Präfixen also weitestgehend vermeiden, wenn Sie auf häufig genutzte UI-Elemente verweisen.

Der folgende Code Ausschnitt zeigt eine Vorlage, die <xref:Windows.UI.Xaml.Controls.Page>-Stamm der ersten Seite für eine App erstellt wurde (nur das öffnende Tag und vereinfacht). Darin werden der Standardnamespace und der **x** -Namespace deklariert (wird als Nächstes erklärt).

```xml
<Page
    x:Class="Application1.BlankPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
>
```

## <a name="the-xaml-language-xaml-namespace"></a>XAML-Namespace der XAML-Sprache

Ein spezieller XAML-Namespace, der in fast jeder Windows-Runtime-XAML-Datei deklariert ist, ist der XAML-Sprachnamespace. Dieser Namespace enthält Elemente und Konzepte, die durch die XAML-Sprachspezifikation definiert werden. Üblicherweise wird der XAML-Namespace für die XAML-Sprache dem Präfix „x“ zugeordnet. Die standardmäßigen Projekt- und Dateivorlagen für Projekte für Windows-Runtime-Apps definieren immer sowohl den Standard-XAML-Namespace (kein Präfix, nur `xmlns=`) als auch den XAML-Sprachnamespace (Präfix „x“) als Teil des Stammelements.

Der XAML-Sprachnamespace mit dem Präfix „x“ enthält mehrere Programmierkonstrukte, die in XAML häufig verwendet werden. Die häufigsten lauten:

| Begriff | Beschreibung |
|------|-------------|
| [x:Key](x-key-attribute.md) | Legt einen eindeutigen benutzerdefinierten Schlüssel für jede Ressource in einem XAML-<xref:Windows.UI.Xaml.ResourceDictionary> fest. Die Schlüsseltoken-Zeichenfolge ist das Argument für die **StaticResource**-Markuperweiterung. Sie verwenden diesen Schlüssel später zum Abrufen der XAML-Ressourcen aus einer anderen XAML-Verwendung in der XAML Ihrer App. |
| [x:Class](x-class-attribute.md) | Gibt den Code-Namespace und Codeklassennamen für die Klasse an, mit der CodeBehind-Daten für eine XAML-Seite bereitgestellt werden. Damit wird die Klasse benannt, die beim Erstellen Ihrer App erstellt oder zugeordnet wird. Diese Buildvorgänge unterstützen die XAML-Markupkompilierung und kombinieren das Markup und CodeBehind, wenn die App kompiliert wird. Eine solche Klasse ist für die Unterstützung von CodeBehind für eine XAML-Seite erforderlich. <xref:Windows.UI.Xaml.Window.Content%2A?displayProperty=nameWithType> im Standard Windows-Runtime Aktivierungs Modell. |
| [x:Name](x-name-attribute.md) | Gibt einen Laufzeitobjektnamen für die Instanz in Laufzeitcode an, nachdem ein in XAML definiertes Objektelement verarbeitet wird. Das Festlegen von **x:Name** in XAML ist mit dem Deklarieren einer benannten Variable in Code vergleichbar. Wie Sie später erfahren werden, geschieht genau das, wenn XAML als Komponente einer Windows-Runtime-App geladen wird. <br/><div class="alert">**Hinweis** <xref:Windows.UI.Xaml.FrameworkElement.Name%2A> ist eine ähnliche Eigenschaft im Framework, aber nicht alle Elemente unterstützen diese Eigenschaft. Verwenden Sie **x:Name** zur Identifizierung von Elementen, wenn **FrameworkElement.Name** für diesen Elementtyp nicht unterstützt wird. |
| [x:UID-Direktive](x-uid-directive.md) | Bezeichnet Elemente, die für einige ihrer Eigenschaftswerte lokalisierte Ressourcen verwenden sollen. Weitere Informationen zur Verwendung von **x:UID**finden Sie unter [quick Start: Übersetzen von UI-Ressourcen @ no__t-0. |
| [Systeminterne XAML-Datentypen](xaml-intrinsic-data-types.md) | Diese Typen können Werte für einfache Werttypen angeben, wenn dies für ein Attribut oder eine Ressource erforderlich ist. Diese systeminternen Typen entsprechen den einfachen Werttypen, die normalerweise als Teil der systeminternen Definitionen der jeweiligen Programmiersprache definiert sind. Beispielsweise benötigen Sie möglicherweise ein-Objekt, das einen **echten** booleschen Wert darstellt, der in einem <xref:Windows.UI.Xaml.Media.Animation.ObjectAnimationUsingKeyFrames>-Storyboarding-visuellen Zustand verwendet werden soll. Für diesen Wert in XAML verwenden Sie den systeminternen **x:Boolean** -Typ als Object-Element, wie folgt: <code>&lt;x:Boolean&gt;True&lt;/x:Boolean&gt;</code> |

Es werden auch andere Programmierkonstrukte im XAML-Sprachnamespace verwendet. Diese sind jedoch weniger gebräuchlich.

## <a name="mapping-custom-types-to-xaml-namespaces"></a>Zuordnen von benutzerdefinierten Typen zu XAML-Namespaces

Einer der nützlichsten Aspekte von XAML als Sprache besteht darin, dass die Erweiterung des XAML-Vokabulars für Windows-Runtime-Apps einfach ist. Definieren Sie dazu in der Programmiersprache der App Ihre eigenen benutzerdefinierten Typen, und verweisen Sie im XAML-Markup dann auf Ihre benutzerdefinierten Typen. Die Unterstützung für die Erweiterung mithilfe von benutzerdefinierten Typen ist integraler Bestandteil der Funktionsweise der XAML-Sprache. Frameworks oder App-Entwickler sind für das Erstellen der Sicherungsobjekte zuständig, auf die XAML verweist. Weder Frameworks noch der App-Entwickler werden durch Spezifikationen gebunden, die die Objekte in ihren vokabarys darstellen oder über die grundlegenden XAML-Syntax Regeln hinausgehen. (Es gibt einige Erwartungen, welche Möglichkeiten die XAML-Namespace Typen der XAML-Sprache haben sollten, aber die Windows-Runtime bietet die erforderliche Unterstützung.)

Wenn Sie XAML für Typen verwenden, die aus anderen Bibliotheken als den Windows Runtime-Kernbibliotheken und -Metadaten stammen, müssen Sie einen XAML-Namespace deklarieren und diesem ein Präfix zuordnen. Verwenden Sie dieses Präfix in Elementnutzungen, um auf die in Ihrer Bibliothek definierten Typen zu verweisen. Sie deklarieren Präfixzuordnungen als **xmlns**-Attribute, und zwar meist in einem Stammelement zusammen mit den anderen XAML-Namespacedefinitionen.

Zum Erstellen Ihrer eigenen Namespacedefinition, mit der auf benutzerdefinierte Typen verwiesen wird, geben Sie zuerst das Schlüsselwort **xmlns:** und dann das gewünschte Präfix an. Der Wert des Attributs muss das Schlüsselwort **using:** als ersten Teil des Werts enthalten. Der restliche Wert ist ein Zeichenfolgentoken, mit dem der Namespace für die Codesicherung, der Ihre benutzerdefinierten Typen enthält, anhand des Namens referenziert wird.

Das Präfix definiert das Markuptoken, das zur Referenzierung dieses XAML-Namespaces im Rest des Markups der XAML-Datei verwendet wird. Ein Doppelpunkt (":") wird zwischen das Präfix und die im XAML-Namespace zu referenzierende Entität gesetzt.

Die Attributsyntax zum Zuordnen des Präfix `myTypes` zum Namespace `myCompany.myTypes` lautet z. B. wie folgt: `    xmlns:myTypes="using:myCompany.myTypes"`. Ein Beispiel für eine Elementdarstellung lautet: `<myTypes:CustomButton/>`.

Weitere Informationen zur Zuordnung von XAML-Namespaces für benutzerdefinierte Typen einschließlich einiger besonderer Punkte zu Visual C++-Komponentenerweiterungen (C++/CX) finden Sie unter [XAML-Namespaces und -Namespacezuordnung](xaml-namespaces-and-namespace-mapping.md).

## <a name="other-xaml-namespaces"></a>Andere XAML-Namespaces

Häufig sehen Sie XAML-Dateien, die die Präfixe „d“ (für Designernamespace) und „mc“ (zur Markupkompatibilität) definieren. Diese sind im Allgemeinen für die Infrastruktur Unterstützung oder für das Aktivieren von Szenarien in einem Entwurfszeit Tool vorgesehen. Weitere Informationen finden Sie im [Abschnitt "Andere XAML-Namespaces" des Themas zu XAML-Namespaces](xaml-namespaces-and-namespace-mapping.md#other-XAML-namespaces).

## <a name="markup-extensions"></a>Markuperweiterungen

Markuperweiterungen sind ein XAML-Sprachkonzept, das häufig bei der Windows-Runtime-XAML-Implementierung verwendet wird. Häufig stellen Markuperweiterungen eine Art "Verknüpfung" dar, mit deren Hilfe eine XAML-Datei auf einen Wert oder ein Verhalten zugreifen kann, bei dem Elemente nicht einfach nur anhand von Sicherungstypen deklariert werden. Einige Markuperweiterungen können Eigenschaften mit einfachen Zeichenfolgen oder zusätzlichen geschachtelten Elementen festlegen; dies dient der Optimierung der Syntax bzw. der Faktoren zwischen verschiedenen XAML-Dateien.

In der XAML-Attributsyntax zeigen die geschweiften Klammern „{“ und „}“ die Verwendung von XAML-Markuperweiterungen an. Die Verwendung weist die XAML-Verarbeitung an, die allgemeine Behandlung von Attributwerten entweder als Literalzeichenfolge oder direkten zeichenfolgenkonvertiblen Wert zu vermeiden. Stattdessen ruft ein XAML-Parser Code auf, der das Verhalten für die jeweilige Markuperweiterung bereitstellt, und dieser Code stellt ein alternatives Ergebnis für das Objekt oder Verhalten bereit, dass der XAML-Parser benötigt. Markuperweiterungen können über Argumente verfügen, die dem Namen der Markuperweiterung folgen und auch innerhalb der geschweiften Klammern enthalten sind. Normalerweise stellt eine ausgewertete Markuperweiterung einen Objektrückgabewert bereit. Bei der Analyse wird dieser Rückgabewert an der Stelle in der Objektstruktur eingefügt, an der sich die Verwendung der Markuperweiterung in der XAML-Quelle befand.

Windows-Runtime-XAML unterstützt diese Markuperweiterungen, die unter dem Standard-XAML-Namespace definiert und vom Windows-Runtime-XAML-Parser verstanden werden:

- [{x:Bind}](x-bind-markup-extension.md): unterstützt die Datenbindung, die die Eigenschafts Auswertung bis zur Laufzeit abruft, indem spezieller Code ausgeführt wird, der zum Zeitpunkt der Kompilierung generiert wird. Diese Markuperweiterung unterstützt einen weiten Bereich von Argumenten.
- [{Binding}](binding-markup-extension.md): unterstützt die Datenbindung. Dadurch wird die Eigenschaftenauswertung durch Ausführung einer allgemeinen Laufzeitobjektüberprüfung bis zur Laufzeit zurückgestellt. Diese Markuperweiterung unterstützt einen weiten Bereich von Argumenten.
- [{Statikresource}](staticresource-markup-extension.md): unterstützt verweisende Ressourcen Werte, die in einem <xref:Windows.UI.Xaml.ResourceDictionary> definiert sind. Diese Ressourcen können sich in einer anderen XAML-Datei befinden, müssen vom XAML-Parser zur Ladezeit jedoch auffindbar sein. Das-Argument einer `{StaticResource}`-Verwendung identifiziert den Schlüssel (den Namen) für eine Schlüssel gebundene Ressource in einem <xref:Windows.UI.Xaml.ResourceDictionary>.
- [{ThemeResource}](themeresource-markup-extension.md): ähnlich wie die [{StaticResource}](staticresource-markup-extension.md)-Markuperweiterung, kann jedoch auf Designänderungen während der Laufzeit reagieren. {ThemeResource} tritt bei den Windows-Runtime-XAML-Standardvorlagen relativ häufig auf, da die meisten dieser Vorlagen darauf ausgerichtet sind, dass der Benutzer das Design wechseln kann, während die App ausgeführt wird.
- [{TemplateBinding}](templatebinding-markup-extension.md): Ein Sonderfall von [{Binding}](binding-markup-extension.md) , bei dem Steuerelementvorlagen in XAML und deren Nutzung zur Laufzeit unterstützt werden.
- [{RelativeSource}](relativesource-markup-extension.md): Ermöglicht eine bestimmte Form der Vorlagenbindung, bei der die Werte aus dem vorlagenbasierten übergeordneten Element stammen.
- [{CustomResource}](customresource-markup-extension.md): Für erweiterte Ressourcensuchszenarien.

Die Windows-Runtime unterstützt außerdem die [{x:Null}-Markuperweiterung](x-null-markup-extension.md). Diese wird verwendet, um [**Nullable**](/dotnet/api/system.nullable-1)-Werte in XAML auf **null** festzulegen. Beispielsweise können Sie dies in einer Steuerelement Vorlage für eine <xref:Windows.UI.Xaml.Controls.CheckBox> verwenden, die **null** als unbestimmten Status der Überprüfung interpretiert (wodurch der "unbestimmte" visuelle Zustand ausgelöst wird).

Eine Markup Erweiterung gibt im Allgemeinen eine vorhandene Instanz aus einem anderen Teil des Objekt Diagramms für die APP zurück oder entfernt einen Wert zur Laufzeit. Da Sie eine Markuperweiterung als Attributwert verwenden können und dies auch die übliche Art der Nutzung ist, ist häufig zu beobachten, dass Markuperweiterungen Werte für Eigenschaften vom Typ "Verweis" bereitstellen, für die andernfalls eine Eigenschaftselementsyntax erforderlich gewesen wäre.

Hier ist beispielsweise die Syntax für das verweisen auf ein wiederverwendbares <xref:Windows.UI.Xaml.Style> von einem <xref:Windows.UI.Xaml.ResourceDictionary>: `<Button Style="{StaticResource SearchButtonStyle}"/>`. Bei einem <xref:Windows.UI.Xaml.Style> handelt es sich um einen Verweistyp, nicht um einen einfachen Wert, sodass Sie ohne die Verwendung von `{StaticResource}` ein Eigenschaften Element `<Button.Style>` und eine `<Style>`-Definition benötigen, um die <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType>-Eigenschaft festzulegen.

Bei Einsatz von Markuperweiterungen kann jede Eigenschaft, die unter XAML festgelegt werden kann, praktisch auch in Attributsyntax festgelegt werden. Mithilfe der Attributsyntax können Sie auch dann Verweiswerte für eine Eigenschaft bereitstellen, wenn diese ansonsten keine Attributsyntax für die direkte Objektinstanziierung unterstützt. Oder Sie können ein spezifisches Verhalten aktivieren, das die allgemeine Anforderung zurückstellt, dass XAML-Eigenschaften durch Wertetypen oder neu erstellte Verweistypen gefüllt werden.

Um dies zu veranschaulichen, legt das nächste XAML-Beispiel den Wert der <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType>-Eigenschaft einer <xref:Windows.UI.Xaml.Controls.Border> mithilfe der Attribut Syntax fest. Die <xref:Windows.UI.Xaml.FrameworkElement.Style%2A?displayProperty=nameWithType>-Eigenschaft nimmt eine Instanz der <xref:Windows.UI.Xaml.Style?displayProperty=nameWithType>-Klasse an, ein Verweistyp, der standardmäßig nicht mithilfe einer Attribut Syntax Zeichenfolge erstellt werden konnte. In diesem Fall verweist jedoch das Attribut auf eine bestimmte Markuperweiterung, [StaticResource](staticresource-markup-extension.md). Wenn diese Markuperweiterung verarbeitet wird, gibt sie einen Verweis auf ein **Style**-Element zurück, das zuvor als Ressource mit Schlüssel in einem Ressourcenverzeichnis definiert wurde.

```xml
<Canvas.Resources>
  <Style TargetType="Border" x:Key="PageBackground">
    <Setter Property="BorderBrush" Value="Blue"/>
    <Setter Property="BorderThickness" Value="5"/>
  </Style>
</Canvas.Resources>
...
<Border Style="{StaticResource PageBackground}">
  ...
</Border>
```

Markuperweiterungen können geschachtelt werden. Die innerste Markuperweiterung wird zuerst ausgewertet.

Aufgrund der Markuperweiterungen benötigen Sie für einen Literalwert „{“ in einem Attribut eine spezielle Syntax. Weitere Informationen finden Sie unter [Anleitung zur XAML-Syntax](xaml-syntax-guide.md).

## <a name="events"></a>Ereignisse

XAML ist eine deklarative Programmiersprache für Objekte und ihre Eigenschaften, sie enthält jedoch auch eine Syntax zum Anfügen von Ereignishandlern an Objekte im Markup. Die XAML-Ereignissyntax kann dann die XAML-deklarierten Ereignisse über das Windows-Runtime-Programmiermodell integrieren. Sie geben den Namen des Ereignisses als Attributnamen in dem Objekt an, in dem das Ereignis behandelt wird. Für den Attributwert geben Sie den Namen einer Ereignishandler-Funktion an, die Sie im Code definieren. Der XAML-Prozessor verwendet diesen Namen zum Erstellen einer Delegatdarstellung in der geladenen Objektstruktur und fügt den angegebenen Handler einer internen Handlerliste hinzu. Fast alle Windows-Runtime-Apps werden sowohl durch Markup- als auch durch CodeBehind-Quellen definiert.

Dies ist ein einfaches Beispiel. Die <xref:Windows.UI.Xaml.Controls.Button>-Klasse unterstützt ein Ereignis mit dem Namen <xref:Windows.UI.Xaml.Controls.Primitives.ButtonBase.Click>. Sie können einen Handler für **Click** schreiben, mit dem Code ausgeführt wird, der aufgerufen wird, nachdem Benutzer auf das **Button**-Element geklickt haben. Im XAML-Code geben Sie **Click** als Attribut des **Button**-Elements an. Stellen Sie für den Attributwert eine Zeichenfolge bereit, bei der es sich um den Methodennamen des Handlers handelt.

```xml
<Button Click="showUpdatesButton_Click">Show updates</Button>
```

Bei der Kompilierung wird vom Compiler dann erwartet, dass eine Methode mit dem Namen `showUpdatesButton_Click` in der CodeBehind-Datei und im Namespace definiert ist, der im [x:Class](x-class-attribute.md)-Wert der XAML-Seite deklariert wurde. Außerdem muss diese Methode den delegatvertrag für das <xref:Windows.UI.Xaml.Controls.Primitives.ButtonBase.Click>-Ereignis erfüllen. Zum Beispiel:

```csharp
namespace App1
{
    public sealed partial class MainPage: Page {
        ...
        private void showUpdatesButton_Click (object sender, RoutedEventArgs e) {
            //your code
        }
    }
}
```

```vb
' Namespace included at project level
Public NotInheritable Class MainPage
    Inherits Page
        ...
        Private Sub showUpdatesButton_Click (sender As Object, e As RoutedEventArgs e)
            ' your code
        End Sub
    ...
End Class
```

```cppwinrt
namespace winrt::App1::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        ...
        void showUpdatesButton_Click(Windows::Foundation::IInspectable const&, Windows::UI::Xaml::RoutedEventArgs const&);
    };
}
```

```cpp
// .h
namespace App1
{
    public ref class MainPage sealed {
        ...
    private:
        void showUpdatesButton_Click(Object^ sender, RoutedEventArgs^ e);
    };
}
```

Innerhalb eines Projekts wird XAML als XAML-Datei geschrieben, und Sie können mit Ihrer bevorzugten Programmiersprache (C#, Visual Basic, C++/CX) eine CodeBehind-Datei schreiben. Wenn für eine XAML-Datei als Teil einer Buildaktion für das Projekt Markup kompiliert wird, wird die Position der XAML-CodeBehind-Datei für jede XAML-Seite durch Angabe eines Namespaces und einer Klasse als [x:Class](x-class-attribute.md)-Attribut des Stammelements der XAML-Seite bestimmt. Weitere Informationen zur Funktionsweise dieser Mechanismen in XAML und ihrer Beziehung zu den Programmierungs- und Anwendungsmodellen finden Sie unter [Übersicht über Ereignisse und Routingereignisse](events-and-routed-events-overview.md).

> [!NOTE]
> Für C++/CX gibt es zwei Code-Behind-Dateien: eine ist ein Header (. XAML. h), und die andere ist die Implementierung (. XAML. cpp). Die Implementierung verweist auf den Header, und aus technischer Sicht ist es der Header, der den Einstiegspunkt für die CodeBehind-Verbindung darstellt.

## <a name="resource-dictionaries"></a>Ressourcenverzeichnis

Das Erstellen eines <xref:Windows.UI.Xaml.ResourceDictionary> ist eine gängige Aufgabe, die in der Regel durch Erstellen eines Ressourcen Wörterbuchs als Bereich einer XAML-Seite oder einer separaten XAML-Datei erreicht wird. Ressourcenwörterbücher und ihre Verwendung sind ein umfangreicher Bereich, dessen Erläuterung den Rahmen dieses Themas sprengen würde. Weitere Informationen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md).

## <a name="xaml-and-xml"></a>XAML und XML

Die XAML-Sprache basiert im Großen und Ganzen auf der XML-Sprache. Durch XAML wird XML jedoch bedeutend erweitert. Im Speziellen das Schemakonzept wird wegen der Beziehung zum Unterstützungstyp anders behandelt, und es werden Sprachelemente wie angefügte Member und Markuperweiterungen hinzugefügt. **xml:lang** ist in XAML gültig, beeinflusst jedoch anstelle des Analyseverhaltens das Laufzeitverhalten, und der Alias ist normalerweise einer Eigenschaft auf Frameworkebene zugeordnet. Weitere Informationen finden Sie unter <xref:Windows.UI.Xaml.FrameworkElement.Language%2A?displayProperty=nameWithType>. **xml:base** ist im Markup gültig, wird aber von Parsern ignoriert. **xml:space** ist gültig, jedoch nur für im Thema [XAML und Leerzeichen](xaml-and-whitespace.md) beschriebene Szenarien relevant. Das **encoding**-Attribut ist in XAML gültig. Nur UTF-8- und UTF-16-Codierungen werden unterstützt. UTF-32-Codierung wird nicht unterstützt.

###  <a name="case-sensitivity-in-xaml"></a>Unterscheidung nach Groß-/Kleinschreibung in XAML

In XAML wird die Groß-/Kleinschreibung berücksichtigt. Dies ist eine weitere Konsequenz daraus, dass XAML auf XML basiert, in der die Groß-/Kleinschreibung berücksichtigt wird. In Namen von XAML-Elementen und -Attributen wird die Groß-/Kleinschreibung berücksichtigt. Im Wert eines Attributs wird die Groß-/Kleinschreibung unter Umständen berücksichtigt; dies hängt davon ab, wie der Attributwert für bestimmte Eigenschaften behandelt wird. Wenn der Attributwert beispielsweise einen Membernamen einer Enumeration deklariert, wird für das integrierte Verhalten, das eine Membernamen-Zeichenfolge typkonvertiert, um den Enumerationsmemberwert zurückzugeben, die Groß-/Kleinschreibung nicht berücksichtigt. Im Gegensatz dazu behandeln der Wert der **Name**-Eigenschaft und Hilfsmethoden zum Arbeiten mit Objekten basierend auf dem Namen, den die **Name**-Eigenschaft deklariert, die Namenszeichenfolge unter Berücksichtigung der Groß-/Kleinschreibung.

## <a name="xaml-namescopes"></a>XAML-Namescopes

Die XAML-Sprache definiert ein Konzept eines XAML-NameScopes. Das XAML-NameScope-Konzept beeinflusst, wie XAML-Prozessoren den Wert von **x:Name** oder **Name**, angewendet auf XAML-Elemente, behandeln, insbesondere die Bereiche, in denen Namen anwendbare eindeutige IDs sein sollten. XAML-NameScopes werden ausführlicher im separaten Thema [XAML-NameScopes](xaml-namescopes.md) behandelt.

## <a name="the-role-of-xaml-in-the-development-process"></a>Die Rolle von XAML im Entwicklungsprozess

XAML spielt in vielerlei Hinsicht eine wichtige Rolle beim App-Entwicklungsprozess.

- XAML stellt das Hauptformat für die Deklarierung der Benutzeroberfläche und deren Elementen in einer App dar, sofern deren Programmierung mit C#, Visual Basic oder C++/CX erfolgt. In der Regel stellt mindestens eine XAML-Datei in Ihrem Projekt eine Seitenmetapher in Ihrer App für die zu Anfang angezeigte Benutzeroberfläche dar. Zusätzliche XAML-Dateien können weitere Seiten für die Navigationsoberfläche deklarieren. Andere XAML-Dateien können Ressourcen deklarieren, etwa Vorlagen und Stile.
- Das XAML-Format wird zur Deklaration von Stilen und Vorlagen genutzt, die in einer App auf Steuerelemente und die Benutzeroberfläche angewendet werden.
- Dabei können Stile und Vorlagen entweder für bestehende Steuerelemente in Vorlagen überführt oder ein Steuerelement definiert werden, das Standardvorlagen als Teil eines Steuerelementepakets bereitstellt. Wenn Sie diese zum Definieren von Stilen und Vorlagen verwenden, wird das relevante XAML häufig als diskrete XAML-Datei mit einem <xref:Windows.UI.Xaml.ResourceDictionary>-Stamm deklariert.
- XAML ist das gemeinsame Format für Designerunterstützung, mit der Benutzeroberflächen für Apps erstellt und das Design zwischen verschiedenen Designer-Apps ausgetauscht werden kann. Am wichtigsten dürfte hier die Möglichkeit sein, den XAML-Code für die App zwischen unterschiedlichen XAML-Designtools (oder Designfenstern in Tools) austauschen zu können.
- Auch viele andere Technologien definieren die grundlegende Benutzeroberfläche in XAML. In Bezug auf das Windows Presentation Foundation (WPF)- und Microsoft Silverlight-XAML-Format nutzt das XAML-Format für Windows-Runtime denselben URI für seinen gemeinsam verwendeten Standard-XAML-Namespace. Das XAML-Vokabular für Windows-Runtime überschneidet sich stark mit dem XAML-Vokabular für Benutzeroberflächen, das ebenfalls von Silverlight und zu einem etwas geringeren Grad von WPF genutzt wird. Damit unterstützt XAML eine effiziente Migrationsmöglichkeit für Benutzeroberflächen, die ursprünglich für Vorgängertechnologien definiert wurden und ebenfalls auf XAML gesetzt haben.
- XAML definiert die visuelle Darstellung einer Benutzeroberfläche, und eine zugehörige CodeBehind-Datei definiert die Logik. Das Oberflächendesign kann geändert werden, ohne Änderungen an der Logik im CodeBehind vornehmen zu müssen. XAML vereinfacht den Workflow zwischen Designern und Entwicklern.
- Aufgrund der umfangreichen Unterstützung des visuellen Designers und der Designoberflächen für die XAML-Sprache, unterstützt XAML Rapid-UI-Prototyping in frühen Entwicklungsstadien.

Unter Umständen haben Sie selbst, abhängig von Ihrer Rolle im Entwicklungsprozess, nicht viel mit XAML zu tun. Der Grad, zu dem Sie mit XAML-Dateien interagieren, hängt auch davon ab, welche Entwicklungsumgebung Sie verwenden, ob Sie auf interaktive Designumgebungsfunktionen wie Toolboxen und Eigenschaften-Editoren zurückgreifen, wie groß der Umfang Ihrer Windows-Runtime-App ist und welchem Zweck sie dient. Trotzdem ist es wahrscheinlich, dass Sie während der Entwicklung Ihrer App eine XAML-Datei auf Elementebene mithilfe eines Text- oder XML-Editors bearbeiten werden. Mit diesen Informationen können Sie XAML-Dateien in einer Text- oder XML-Darstellung sicher bearbeiten und die Gültigkeit der Deklarationen und den Zweck der Dateien bewahren, wenn sie von Tools, Markupkompilierungsvorgängen oder in der Laufzeitphase Ihrer Windows-Runtime-App verarbeitet werden.

## <a name="optimize-your-xaml-for-load-performance"></a>Optimieren des XAML-Codes für hohe Auslastung

Im Folgenden sind einige Tipps zum Definieren von UI-Elementen in XAML-Code mithilfe der bewährten Methoden zur Verbesserung der Leistung aufgeführt. Viele dieser Tipps beziehen sich auf die Verwendung von XAML-Ressourcen. Sie wurden zur besseren Verständlichkeit jedoch auch hier in der allgemeinen XAML-Übersicht angegeben. Weitere Informationen zu XAML-Ressourcen finden Sie unter [ResourceDictionary- und XAML-Ressourcenverweise](../design/controls-and-patterns/resourcedictionary-and-xaml-resource-references.md). Weitere Tipps zur Leistung, z. B. XAML-Beispielcode, in dem absichtlich einige Praktiken veranschaulicht werden, die zu schlechter Leistung führen und vermieden werden sollten, finden Sie unter [Optimieren des XAML-Markups](../debug-test-perf/optimize-xaml-loading.md).

- Wenn Sie den gleichen Farbpinsel häufig in Ihrem XAML verwenden, definieren Sie ein <xref:Windows.UI.Xaml.Media.SolidColorBrush> als Ressource, anstatt jedes Mal eine benannte Farbe als Attribut Wert zu verwenden.
- Wenn Sie dieselbe Ressource auf mehr als einer UI-Seite verwenden, sollten Sie Sie ggf. in <xref:Windows.UI.Xaml.Application.Resources%2A> und nicht auf jeder Seite definieren. Falls eine Ressource nur von einer Seite genutzt wird, ist die Definition unter **Application.Resources** nicht ratsam. Definieren Sie die Ressource stattdessen nur für die jeweilige Seite. Dies ist sowohl für die XAML-Zerlegung beim Entwerfen der App als auch zur Steigerung der Leistung während der XAML-Analyse hilfreich.
- Führen Sie für Ressourcen, die von der App verpackt werden, eine Überprüfung auf ungenutzte Ressourcen durch (Ressourcen mit Schlüssel, für die in der App jedoch kein entsprechender [StaticResource](staticresource-markup-extension.md)-Verweis enthalten ist). Entfernen Sie diese vor der Freigabe der App aus dem XAML-Code.
- Wenn Sie separate XAML-Dateien verwenden, die Entwurfs Ressourcen (<xref:Windows.UI.Xaml.ResourceDictionary.MergedDictionaries%2A>) bereitstellen, sollten Sie in Erwägung ziehen, nicht verwendete Ressourcen aus diesen Dateien zu kommentieren oder zu entfernen. Auch wenn Sie über einen gemeinsamen XAML-Startpunkt verfügen, den Sie in mehr als einer App verwenden oder über den häufig verwendete Ressourcen für alle Apps bereitgestellt werden, werden die XAML-Ressourcen doch immer von Ihrer App verpackt und ggf. geladen.
- Definieren Sie keine UI-Elemente, die Sie für die Komposition nicht benötigen, und verwenden Sie nach Möglichkeit immer die Standardsteuerelementvorlagen (diese Vorlagen wurden bereits auf hohe Leistungsfähigkeit getestet und bestätigt).
- Verwenden Sie Container wie <xref:Windows.UI.Xaml.Controls.Border> anstelle von absichtlichen über schreibungen von UI-Elementen. Einfach ausgedrückt: Zeichnen Sie dasselbe Pixel nicht mehrere Male. Weitere Informationen zu über schreibungen und deren Prüfung finden Sie unter <xref:Windows.UI.Xaml.DebugSettings.IsOverdrawHeatMapEnabled?displayProperty=nameWithType>.
- Verwenden Sie die Standardelement Vorlagen für <xref:Windows.UI.Xaml.Controls.ListView> oder <xref:Windows.UI.Xaml.Controls.GridView>; Diese verfügen über eine spezielle **präsentatorlogik** , mit der Leistungsprobleme beim Aufbau der visuellen Struktur für eine große Anzahl von Listenelementen gelöst werden.

## <a name="debug-xaml"></a>XAML Debuggen

Da es sich bei XAML um eine Markupsprache handelt, sind einige der typischen Strategien für das Debuggen innerhalb von Microsoft Visual Studio nicht verfügbar. Beispielsweise besteht keine Möglichkeit zum Festlegen eines Haltepunkts innerhalb einer XAML-Datei. Jedoch bestehen andere Techniken, mit denen Sie Probleme mit UI-Definitionen oder XAML-Markups während der Appentwicklung beheben können.

Treten Probleme mit einer XAML-Datei auf, wird sehr wahrscheinlich von einem System oder Ihrer App eine XAML-Analyseausnahme ausgelöst. Tritt eine XAML-Analyseausnahme auf, konnte von der vom XAML-Parser geladenen XAML keine gültige Objektstruktur erstellt werden. In einigen Fällen, z. B. wenn die XAML die erste Seite der Anwendung darstellt, die als die visuelle Stammstruktur geladen wird, ist die XAML-Analyseausnahme nicht behebbar.

XAML wird häufig innerhalb einer IDE, z. B. Visual Studio, und einer der XAML-Designoberflächen bearbeitet. Visual Studio kann in vielen Fällen Überprüfung zu Entwurfszeit und Fehlerüberprüfung einer XAML-Quelle während der Bearbeitung bereitstellen. Beispielsweise werden ungültige Attributwerte im XAML-Texteditor bei der Eingabe unter Umständen durch Wellenlinien gekennzeichnet, und Sie sehen noch vor der XAML-Kompilierung, dass die Benutzeroberflächendefinition fehlerhaft ist.

Wird die App ausgeführt und wurden XAML-Analysefehler zur Entwurfszeit nicht erfasst, werden werden sie von der Common Language Runtime (CLR) als [**XamlParseException**](https://docs.microsoft.com/dotnet/api/Windows.UI.Xaml.markup.xamlparseexception?view=dotnet-uwp-10.0) gemeldet. Weitere Infos dazu, was im Falle einer Laufzeit-**XamlParseException** unternommen werden kann, finden Sie unter [Ausnahmebehandlung für Windows-Runtime-Apps in C# oder Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/dn532194(v=win.10)).

> [!NOTE]
> Apps, die C++/CX für Code verwenden, erhalten nicht die spezifische [xamlparameseexception](https://docs.microsoft.com/dotnet/api/Windows.UI.Xaml.markup.xamlparseexception?view=dotnet-uwp-10.0). Die Meldung in der Ausnahme stellt die Fehlerursache als XAML-bezogen dar und enthält Kontextinfos, z. B. Zeilennummern in einer XAML-Datei, so wie **XamlParseException**.

Weitere Informationen zum Debuggen einer Windows-Runtime-App finden Sie unter [Starten einer Debuggingsitzung](/visualstudio/debugger/start-a-debugging-session-for-a-store-app-in-visual-studio-vb-csharp-cpp-and-xaml?view=vs-2015).
