---
description: In diesem Thema wird das Abhängigkeitseigenschaftensystem erläutert, das Ihnen beim Entwickeln einer Windows-Runtime-App mit C++, C# oder Visual Basic und XAML-Definitionen für die UI zur Verfügung steht.
title: Übersicht über Abhängigkeitseigenschaften
ms.assetid: AD649E66-F71C-4DAA-9994-617C886FDA7E
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 279f0d007be927e29632986ce8178c4e0b9778b3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259858"
---
# <a name="dependency-properties-overview"></a>Übersicht über Abhängigkeitseigenschaften

In diesem Thema wird das Abhängigkeitseigenschaftensystem erläutert, das Ihnen beim Entwickeln einer Windows-Runtime-App mit C++, C# oder Visual Basic und XAML-Definitionen für die UI zur Verfügung steht.

## <a name="what-is-a-dependency-property"></a>Was ist eine Abhängigkeitseigenschaft?

Eine Abhängigkeitseigenschaft ist eine spezielle Art von Eigenschaft. Es handelt sich um eine Eigenschaft, bei der der Eigenschaftswert von einem dedizierten Eigenschaftensystem nachverfolgt und beeinflusst wird, das Teil der Windows-Runtime ist.

Damit eine Abhängigkeitseigenschaft unterstützt wird, muss das die Eigenschaft definierende Objekt ein [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) sein, d. h. eine Klasse, die irgendwo in der Vererbung die **DependencyObject**-Basisklasse enthält. Viele der Typen, die Sie für die Benutzeroberflächen Definitionen für eine UWP-App mit XAML verwenden, sind eine **DependencyObject** -Unterklasse, die Abhängigkeits Eigenschaften unterstützt. Alle Typen, die aus einem Windows-Runtime-Namespace stammen, dessen Name nicht „XAML“ enthält, unterstützen jedoch keine Abhängigkeitseigenschaften. Eigenschaften dieses Typs sind gewöhnliche Eigenschaften, die nicht über das Abhängigkeitsverhalten des Eigenschaftensystems verfügen.

Der Zweck von Abhängigkeitseigenschaften besteht darin, es zu ermöglichen, den Wert einer Eigenschaft anhand anderer Eingaben systembedingt zu berechnen (andere Eigenschaften, Ereignisse und Zustände, die während der Ausführung der App eintreten). Zu diesen anderen Eingaben gehören:

- Externe Eingabe wie Benutzereinstellung
- Just-In-Time-Mechanismen zur Ermittlung von Eigenschaften, z. B. Datenbindung, Animationen und Storyboards
- Vorlagen zur mehrfachen Verwendung, z. B. Ressourcen und Stile
- Werte, die aufgrund von Beziehungen mit anderen Elementen in der Elementstruktur bekannt sind (übergeordnete und untergeordnete Elemente).

Eine Abhängigkeits Eigenschaft stellt eine bestimmte Funktion des Programmiermodells zum Definieren einer Windows-Runtime-App mit XAML für die Benutzeroberfläche C#und, Microsoft Visual Basic oder C++ Visual Component ExtensionsC++(/CX) für Code dar oder unterstützt diese. Verfügbare Features:

- Datenbindung
- Stile
- Storyboard-Animationen
- „PropertyChanged“-Verhalten: Eine Abhängigkeitseigenschaft kann implementiert werden, um Callbacks bereitzustellen, die Änderungen an andere Abhängigkeitseigenschaften weitergeben können.
- Verwenden eines Standardwerts, der aus Eigenschaftsmetadaten stammt
- Allgemeines Dienstprogramm für Eigenschaftssystem, wie etwa [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue) und die Metadatensuche

## <a name="dependency-properties-and-windows-runtime-properties"></a>Abhängigkeitseigenschaften und Windows-Runtime-Eigenschaften

Mit Abhängigkeitseigenschaften werden die grundlegenden Windows-Runtime-Eigenschaftsfunktionen erweitert, indem ein globaler, interner Eigenschaftenspeicher bereitgestellt wird, in dem alle Abhängigkeitseigenschaften einer App zur Laufzeit gesichert werden. Dies ist eine Alternative zur Standardvorgehensweise, bei der die Eigenschaft mit einem privaten Feld abgesichert wird, das in der Eigenschaftsdefinitionsklasse als privat festgelegt ist. Sie können sich diesen internen Eigenschaftenspeicher als einen Satz von Bezeichnern und Werten vorstellen, der für jedes einzelne Objekt vorhanden ist (sofern es sich um ein [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)-Element handelt). Die einzelnen Eigenschaften werden im Speicher nicht anhand des Namens identifiziert, sondern durch eine [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)-Instanz. Vom Eigenschaftensystem wird dieses Implementierungsdetail jedoch meist ausgeblendet: Sie können auf Abhängigkeitseigenschaften normalerweise über einen einfachen Namen (den programmgesteuerten Eigenschaftsnamen in der verwendeten Codesprache oder beim Schreiben von XAML-Code mithilfe eines Attributnamens) zugreifen.

Der Basistyp, der dem Abhängigkeitseigenschaftensystem zugrunde liegt, ist [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject). Mit **DependencyObject** werden Methoden definiert, die auf die Abhängigkeitseigenschaft zugreifen können. Instanzen einer abgeleiteten **DependencyObject** -Klasse stellen die interne Unterstützung des bereits erwähnten Eigenschaftenspeicherkonzepts bereit.

Im Folgenden finden Sie einen Überblick über die Terminologie, die für die Beschreibung der Abhängigkeitseigenschaften verwendet wird:

| Begriff | Beschreibung |
|------|-------------|
| Abhängigkeitseigenschaft | Eine Eigenschaft in einem [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)-Bezeichner (siehe unten). In der Regel ist dieser Bezeichner als statisches Element der definierenden abgeleiteten **DependencyObject**-Klasse verfügbar. |
| Abhängigkeitseigenschaftsbezeichner | Ein Konstantenwert zum Identifizieren der Eigenschaft, der in der Regel öffentlich und schreibgeschützt ist. |
| Eigenschaftenwrapper | Die aufrufbaren **get**- und **set**-Implementierungen für eine Windows-Runtime-Eigenschaft. Oder die sprachspezifische Projektion der ursprünglichen Definition. Eine **get**-Eigenschaftenwrapper-Implementierung ruft [**GetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.getvalue) auf, wobei der jeweilige Abhängigkeitseigenschaftsbezeichner übergeben wird. |

Eigenschaftenwrapper bieten nicht nur einen bequemen Weg zum Aufrufen, sondern sie machen auch die Abhängigkeitseigenschaft für jeden Prozess, jedes Tool oder jede Projektion verfügbar, die Windows-Runtime-Definitionen für Eigenschaften verwenden.

Im folgenden Beispiel wird eine benutzerdefinierte„IsSpinning“-Abhängigkeitseigenschaft für C# definiert. Außerdem veranschaulicht das Beispiel die Beziehung zwischen dem Abhängigkeitseigenschaftsbezeichner und dem Eigenschaftenwrapper.

```csharp
// IsSpinningProperty is the dependency property identifier
// no need for info in the last PropertyMetadata parameter, so we pass null
public static readonly DependencyProperty IsSpinningProperty =
    DependencyProperty.Register(
        "IsSpinning", typeof(Boolean),
        typeof(ExampleClass), null
    );
// The property wrapper, so that callers can use this property through a simple ExampleClassInstance.IsSpinning usage rather than requiring property system APIs
public bool IsSpinning
{
    get { return (bool)GetValue(IsSpinningProperty); }
    set { SetValue(IsSpinningProperty, value); }
}
```

> [!NOTE]
> Das vorherige Beispiel ist nicht als das komplette Beispiel für das Erstellen einer benutzerdefinierten Abhängigkeits Eigenschaft gedacht. Damit sollen die Konzepte von Abhängigkeitseigenschaften für alle Personen verdeutlicht werden, die es vorziehen, Konzepte über den Code zu erlernen. Ein vollständigeres Beispiel finden Sie unter [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md).

## <a name="dependency-property-value-precedence"></a>Rangfolge von Abhängigkeitseigenschaftswerten

Wenn Sie den Wert einer Abhängigkeitseigenschaft abrufen, erhalten Sie einen Wert, der für diese Eigenschaft mithilfe einer der Eingaben festgelegt wurde, die Teil des Windows-Runtime-Eigenschaftensystems sind. Die Rangfolge von Abhängigkeitseigenschaftswerten wird genutzt, damit vom Windows-Runtime-Eigenschaftensystem Werte auf vorhersehbare Weise berechnet werden können. Es ist auch wichtig, dass Sie mit der grundlegenden Reihenfolge vertraut sind, die für die Rangfolge gilt. Andernfalls kann es passieren, dass Sie eine Eigenschaft auf einer Rangfolgenebene festlegen möchten, während diese von einer anderen Komponente (System, Aufrufe von Dritten, Ihr eigener Code) auf einer anderen Ebene festgelegt wird. Sie sind dann frustriert, weil Sie herausfinden müssen, welcher Eigenschaftswert verwendet wird und woher dieser Wert stammt.

Zum Beispiel sind Stile und Vorlagen als gemeinsamer Ausgangspunkt zum Festlegen von Eigenschaftswerten und somit zum Festlegen der Darstellung eines Steuerelements vorgesehen. Nun kann es aber sein, dass Sie in einer bestimmten Steuerelementinstanz den Wert abweichend vom allgemeinen Vorlagenwert ändern möchten, beispielsweise wenn das Steuerelement eine andere Hintergrundfarbe oder einen anderen Text als Inhalt haben soll. Das Windows-Runtime-Eigenschaftssystem verwendet lokale Werte mit höherer Priorität als Werte, die von Stilen und Vorlagen bereitgestellt werden. Dadurch wird das Überschreiben der Vorlagen durch App-spezifische Werte ermöglicht, sodass die Steuerelemente nützlich für die eigene Verwendung in der App-UI sind.

### <a name="dependency-property-precedence-list"></a>Rangfolgeliste für Abhängigkeitseigenschaften

Im Folgenden wird die maßgebliche Rangfolge beschrieben, die beim Zuweisen des Laufzeitwerts für eine Abhängigkeitseigenschaft im Eigenschaftensystem verwendet wird. Die höchste Priorität befindet sich an erster Stelle der Liste. Eine ausführlichere Erklärung folgt nach dieser Liste.

1. **Animierte Werte:** Aktive Animationen, Animationen von Ansichtszuständen oder Animationen mit [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior)-Verhalten. Eine auf eine Eigenschaft angewendete Animation muss Vorrang vor dem Basiswert (nicht animiert) haben, auch wenn dieser Wert lokal festgelegt wurde, um die Wirksamkeit der Animation sicherzustellen.
1. **Lokaler Wert:** Ein lokaler Wert kann ganz bequem über den Eigenschaftenwrapper festgelegt werden, was dem Festlegen eines Werts als Attribut oder Eigenschaftselement in XAML gleichkommt. Eine weitere Möglichkeit ist das Aufrufen der [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue)-Methode über die Eigenschaft einer bestimmten Instanz. Wenn Sie einen lokalen Wert anhand einer Bindung oder einer statischen Ressource festlegen, werden die Bindung und die statische Ressource in der Rangfolge jeweils als festgelegter lokaler Wert behandelt. Somit werden Referenzen zu Bindungen oder Ressourcen gelöscht, wenn ein neuer lokaler Wert festgelegt wird.
1. **Auf Vorlagen basierende Eigenschaften:** Ein Element verfügt über diese Eigenschaften, wenn es als Teil einer Vorlage erstellt wurde (über [**ControlTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ControlTemplate) oder [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate)).
1. **Style Setters (Stilsetter):** Werte aus einem [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter)-Element innerhalb der Stile von Seiten- oder Anwendungsressourcen.
1. **Standardwert:** Eine Abhängigkeitseigenschaft kann in ihren Metadaten einen Standardwert enthalten.

### <a name="templated-properties"></a>Auf Vorlagen basierende Eigenschaften

Auf Vorlagen basierende Eigenschaften als Element in einer Rangfolgeliste werden nicht für jede Eigenschaft eines Elements angewendet, das Sie direkt in einem XAML-Seitenmarkup deklarieren. Das Konzept der auf Vorlagen basierenden Eigenschaft besteht nur für Objekte, die erstellt werden, wenn die Windows-Runtime eine XAML-Vorlage auf ein UI-Element anwendet und so dessen visuelle Effekte definiert.

Alle über eine Steuerelementvorlage festgelegte Eigenschaften besitzen Werte einer bestimmten Art. Diese Werte sind beinahe wie ein erweiterter Satz von Standardwerten für das Steuerelement. Sie sind häufig Werten zugeordnet, die Sie später durch direktes Festlegen der Eigenschaftswerte zurücksetzen können. Daher müssen die in der Vorlage festgelegten Werte von einem echten lokalen Wert unterschieden werden können, sodass ein neuer lokaler Wert ihn überschreiben kann.

> [!NOTE]
> In einigen Fällen können von der Vorlage sogar lokale Werte überschrieben werden, falls mit der Vorlage keine Verweise auf die [{TemplateBinding}-Markuperweiterung](templatebinding-markup-extension.md) für Eigenschaften verfügbar gemacht werden konnten, die für Instanzen einstellbar sein sollten. Dies wird normalerweise nur durchgeführt, wenn die Eigenschaft nicht für die Festlegung für Instanzen bestimmt ist, sondern beispielsweise nur für visuelle Effekte und das Vorlagenverhalten relevant ist und nicht für die beabsichtigte Funktion oder Laufzeitlogik des Steuerelements, von dem die Vorlage verwendet wird.

### <a name="bindings-and-precedence"></a>Bindungen und Rangfolge

Bindungsvorgänge verfügen jeweils über die passende Rangfolge für den Bereich, für den sie verwendet werden sollen. So fungiert zum Beispiel [{Binding}](binding-markup-extension.md) für einen lokalen Wert als lokaler Wert, während die [{TemplateBinding}-Markuperweiterung](templatebinding-markup-extension.md) für einen Eigenschaftssetter wie ein Stilsetter angewendet wird. Da Bindungen bis zur Laufzeit warten müssen, bis sie Werte aus Datenquellen abrufen können, wird der Vorgang zum Bestimmen der Eigenschaftswertrangfolge für jede Eigenschaft auch auf die Laufzeit ausgeweitet.

Bindungen haben nicht nur dieselbe Priorität wie lokale Werte, sondern sie sind auch lokale Werte. Dabei ist die Bindung ein Platzhalter für einen zurückgestellten Wert. Wenn für einen Eigenschaftswert eine Bindung eingerichtet ist und Sie dafür zur Laufzeit einen lokalen Wert festlegen, wird diese Bindung dadurch vollständig ersetzt. Wenn Sie [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding) zum Definieren einer Bindung aufrufen, die erst zur Laufzeit aktiv wird, ersetzen Sie ebenfalls alle lokalen Werte, die Sie im XAML-Code oder in vorher ausgeführtem Code ggf. angewendet haben.

### <a name="storyboarded-animations-and-base-value"></a>Storyboardanimationen und Basiswert

Storyboardanimationen basieren auf dem Konzept des *Basiswerts*. Der Basiswert ist der Wert, der vom Eigenschaftensystem anhand seiner Rangfolge ermittelt wird, wobei der letzte Schritt zum Suchen nach Animationen jedoch wegfällt. Beispielsweise kann ein Basiswert aus der Vorlage eines Steuerelements oder aus der Festlegung eines lokalen Werts für die Instanz eines Steuerelements stammen. In beiden Fällen führt das Anwenden einer Animation dazu, dass dieser Basiswert überschrieben wird. Der animierte Wert wird so lange angewendet, wie die Animation ausgeführt wird.

Bei einer animierten Eigenschaft kann sich der Basiswert trotzdem auf das Verhalten der Animation auswirken, wenn die Animation sowohl **Von** als auch **Bis** nicht explizit angibt oder wenn die Animation die Eigenschaft auf ihren Basiswert zurücksetzt, nachdem sie abgeschlossen wurde. In diesen Fällen wird der Rest der Rangfolge wiederverwendet, nachdem die Ausführung einer Animation beendet ist.

Eine Animation, die **Von** mit einem [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior)-Verhalten festlegt, hat jedoch so lange Vorrang vor einem lokalen Wert, bis die Animation entfernt wird, obwohl die Animation augenscheinlich gestoppt wurde. Vom Konzept her entspricht dies einer Animation, die unendlich lange ausgeführt wird, auch wenn in der UI keine visuelle Animation vorhanden ist.

Mehrere Animationen können auf eine einzelne Eigenschaft angewendet werden. Jede dieser Animationen kann ggf. definiert worden sein, um Basiswerte zu ersetzen, die von verschiedenen Punkten der Wertrangfolge stammen. Diese Animationen werden zur Laufzeit jedoch alle gleichzeitig ausgeführt. Häufig bedeutet dies, dass ihre Werte kombiniert werden müssen, da jede Animation den gleichen Einfluss auf den Wert hat. Dies hängt davon ab, wie die Animationen genau definiert wurden, sowie vom animierten Werttyp.

Weitere Informationen finden Sie unter [Storyboardanimationen](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations).

### <a name="default-values"></a>Standardwerte

Das Erstellen eines Standardwerts für eine Abhängigkeitseigenschaft mit einem [**PropertyMetadata**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.PropertyMetadata)-Wert wird im Thema [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md) ausführlicher erläutert.

Abhängigkeitseigenschaften verfügen auch über Standardwerte, wenn diese Standardwerte in den Metadaten der Eigenschaft nicht explizit definiert wurden. Sofern sie von den Metadaten nicht geändert wurden, können die Standardwerte für die Windows-Runtime-Abhängigkeitseigenschaften in der Regel wie folgt lauten:

- Eine Eigenschaft, die ein Laufzeitobjekt oder den grundlegenden **Object**-Typ (einen *Verweistyp*) verwendet, hat den Standardwert **null**. So ist beispielsweise [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext)**null**, bis dafür ein anderer Wert festgelegt oder geerbt wird.
- Eine Eigenschaft, die einen Basiswert verwendet, z. B. Zahlen oder einen booleschen Wert (einen *Werttyp*), nutzt für diesen Wert eine erwartete Standardangabe. Beispiel: 0 für ganze Zahlen und Gleitkommazahlen und **false** für einen booleschen Wert.
- Eine Eigenschaft, die eine Windows-Runtime-Struktur verwendet, verfügt über einen Standardwert, der per Aufruf des impliziten Standardkonstruktors der Struktur abgerufen wird. Dieser Konstruktor nutzt die Standardangaben für die Basiswertfelder der Struktur. Eine Standardangabe für einen [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point)-Wert wird beispielsweise mit den Werten **X** und **Y** als 0 initialisiert.
- Eine Eigenschaft, die eine Enumeration verwendet, verfügt über einen Standardwert des ersten Members dieser Enumeration. In der Referenz können Sie die Standardwerte der einzelnen Enumerationen einsehen.
- Eine Eigenschaft, die eine Zeichenfolge verwendet ([**System.String**](https://docs.microsoft.com/dotnet/api/system.string) für .NET, [**Platform::String**](https://docs.microsoft.com/cpp/cppcx/platform-string-class) für C++/CX), verfügt als Standardwert über eine leere Zeichenfolge ( **""** ).
- Sammlungseigenschaften werden in der Regel nicht als Abhängigkeitseigenschaften implementiert. Die Gründe hierfür werden weiter unten in diesem Thema erläutert. Wenn Sie jedoch eine benutzerdefinierte Sammlungseigenschaft implementieren und sie als Abhängigkeitseigenschaft verwenden möchten, vermeiden Sie unbedingt ein *unbeabsichtigtes Singleton*. Eine Beschreibung finden Sie am Ende von [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md).

## <a name="property-functionality-provided-by-a-dependency-property"></a>Von Abhängigkeitseigenschaften bereitgestellte Features

### <a name="data-binding"></a>Datenbindung

Für eine Abhängigkeitseigenschaft kann der Wert festgelegt werden, indem eine Datenbindung angewendet wird. Bei der Datenbindung wird im XAML-Code die Syntax der [{Binding}-](binding-markup-extension.md) oder [{x:Bind}-Markuperweiterung](x-bind-markup-extension.md) oder der Klasse [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) verwendet. Bei einer datengebundenen Eigenschaft wird die Ermittlung des endgültigen Eigenschaftswerts bis zur Laufzeit zurückgestellt. Der Wert wird dann aus einer Datenquelle abgerufen. Hierbei wird mithilfe des Abhängigkeitseigenschaftensystems ein Platzhalterverhalten für Vorgänge ermöglicht. Dies kann das Laden von XAML-Code sein, wenn der Wert noch nicht bekannt ist, und das anschließende Bereitstellen des Werts zur Laufzeit, indem eine Interaktion mit dem Datenbindungsmodul der Windows-Runtime erfolgt.

Im folgenden Beispiel wird der [**Text**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.textblock.text)-Wert für ein [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element per Bindung im XAML-Code festgelegt. Bei der Datenbindung werden ein übernommener Datenkontext und eine Objektdatenquelle verwendet. (Letztere sind in dem verkürzten Beispiel nicht sichtbar. Ein umfassenderes Beispiel mit Kontext und Quelle finden Sie unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).)

```xaml
<Canvas>
  <TextBlock Text="{Binding Team.TeamName}"/>
</Canvas>
```

Sie können Datenbindungen auch mit Code anstatt mit XAML erstellen. Informationen finden Sie unter [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding).

> [!NOTE]
> Bindungen wie diese werden als lokaler Wert für Zwecke der Rangfolge von Abhängigkeits Eigenschafts Werten behandelt. Wenn Sie für eine Eigenschaft, für die ursprünglich ein [**Binding**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding)-Wert festgelegt wurde, einen anderen lokalen Wert festlegen, überschreiben Sie die Bindung damit vollständig, und nicht ihren Laufzeitwert. {x:Bind}-Bindungen werden unter Verwendung von generiertem Code implementiert, der einen lokalen Wert für die Eigenschaft festlegt. Wenn Sie für eine Eigenschaft, die {x:Bind} verwendet, einen lokalen Wert festlegen, wird dieser beim nächsten Auswerten der Bindung ersetzt, z. B. wenn eine Eigenschaftsänderung an dessen Quellobjekt erkannt wird.

### <a name="binding-sources-binding-targets-the-role-of-frameworkelement"></a>Bindungsquellen, Bindungsziele und die Rolle von FrameworkElement

Eine Eigenschaft muss nicht unbedingt eine Abhängigkeitseigenschaft sein, um als Bindungsquelle zu fungieren. In der Regel kann jede Eigenschaft eine Bindungsquelle sein, aber dies hängt von Ihrer Programmiersprache ab, für die jeweils bestimmte Grenzfälle gelten. Als Ziel einer [{Binding}-Markuperweiterung](binding-markup-extension.md) oder einer [**Bindung**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.Binding) kommen jedoch ausschließlich Abhängigkeitseigenschaften in Frage. Für {x: Bind} gilt diese Anforderung nicht, da zum Übernehmen der Bindungswerte generierter Code verwendet wird.

Beachten Sie beim Erstellen einer Bindung im Code, dass die [**SetBinding**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.setbinding)-API nur für [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement) definiert wird. Sie können eine Bindungsdefinition stattdessen auch mit der Klasse [**BindingOperations**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.BindingOperations) erstellen und somit jede [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)-Eigenschaft referenzieren.

Beachten Sie sowohl bei der Verwendung von Code als auch bei XAML, dass [**DataContext**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.datacontext) eine [**FrameworkElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FrameworkElement)-Eigenschaft ist. Durch die Verwendung einer Art von über-/untergeordneter Eigenschaftsvererbung (meist im XAML-Markup) kann das Bindungssystem ein **DataContext**-Element auflösen, das zu einem übergeordneten Element gehört. Diese Vererbung kann auch ausgewertet werden, wenn das untergeordnete Objekt (das über die Zieleigenschaft verfügt) kein **FrameworkElement**-Element ist und daher keinen eigenen **DataContext**-Wert enthält. Das vererbte übergeordnete Element muss allerdings auf jeden Fall ein **FrameworkElement** sein, um die **DataContext**-Eigenschaft festlegen und annehmen zu können. Alternativ dazu müssen Sie die Bindung so definieren, dass sie auch mit einem **null**-Wert für **DataContext** funktioniert.

Das Verknüpfen der Bindung ist für die meisten Datenbindungsszenarien nicht die einzige Anforderung. Für eine effektive Bindung in eine oder zwei Richtungen muss die Quelleigenschaft Änderungsbenachrichtigungen unterstützen, die an das Bindungssystem und somit an das Ziel weitergegeben werden. Für benutzerdefinierte Bindungsquellen bedeutet dies, dass die Eigenschaft eine Abhängigkeitseigenschaft sein muss, oder dass das Objekt [**INotifyPropertyChanged**](https://docs.microsoft.com/dotnet/api/system.componentmodel.inotifypropertychanged) unterstützt. Sammlungen sollten die Schnittstelle [**INotifyCollectionChanged**](https://docs.microsoft.com/dotnet/api/system.collections.specialized.inotifycollectionchanged) unterstützen. Bestimmte Klassen unterstützen diese Schnittstellen in ihren Implementierungen, sodass sie sich als Basisklasse für Datenbindungen anbieten. Ein Beispiel für eine solche Klasse ist [**ObservableCollection&lt;T&gt;** ](https://docs.microsoft.com/dotnet/api/system.collections.objectmodel.observablecollection-1). Weitere Informationen zu Datenbindungen und zum Zusammenhang zwischen Datenbindungen und dem Eigenschaftensystem finden Sie unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

> [!NOTE]
> In den hier aufgeführten Typen werden Microsoft .NET Datenquellen unterstützt. C++/CX-Datenquellen nutzen andere Schnittstellen für Änderungsbenachrichtigungen oder feststellbares Verhalten. Weitere Informationen finden Sie unter [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth).

### <a name="styles-and-templates"></a>Stile und Vorlagen

Stile und Vorlagen sind zwei Szenarien für Eigenschaften, die als Abhängigkeitseigenschaften definiert sind. Stile sind für das Festlegen von Eigenschaften geeignet, mit denen die UI der App definiert wird. Stile werden als Ressourcen in XAML festgelegt, und zwar entweder als Eintrag in einer [**Resources**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.resources)-Sammlung oder in separaten XAML-Dateien, z. B. Designressourcenwörterbüchern. Stile interagieren mit dem Eigenschaftensystem, da sie sog. „Setter“ für Eigenschaften enthalten. Die wichtigste Eigenschaft hier ist die Eigenschaft [**Control.Template**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.template) einer Klasse [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control): Diese Eigenschaft definiert den größten Teil der visuellen Darstellung und des visuellen Zustands der Klasse **Control**. Weitere Informationen zu Stilen und XAML-Beispiele, in denen [**Style**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Style) definiert und Setter verwendet werden, finden Sie unter [Formatieren von Steuerelementen](https://docs.microsoft.com/windows/uwp/controls-and-patterns/styling-controls).

Werte, die aus Stilen oder Vorlagen stammen, sind zurückgestellte Werte (ähnlich wie bei Bindungen). So können Benutzer von Steuerelementen neue Vorlagen für Steuerelemente erstellen oder Stile neu definieren. Aus diesem Grund können Eigenschaftensetter in Stilen nur für Abhängigkeitseigenschaften und nicht für normale Eigenschaften verwendet werden.

### <a name="storyboarded-animations"></a>Storyboard-Animationen

Sie können den Wert einer Abhängigkeitseigenschaft mithilfe von Storyboardanimationen animieren. Storyboardanimationen in der Windows-Runtime dienen nicht nur reinen Dekorationszwecken. Stellen Sie sich Animationen eher als Zustandsautomatverfahren vor, mit dem die Werte einzelner Eigenschaften oder aller Eigenschaften und visuellen Elemente eines Steuerelements festgelegt und im Laufe der Zeit geändert werden können.

Die Zieleigenschaft einer Animation muss eine Abhängigkeitseigenschaft sein, um animiert werden zu können. Zudem muss der Wert einer Zieleigenschaft von einem der von der vorhandenen Klasse [**Timeline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.Timeline) abgeleiteten Animationstypen unterstützt werden. Werte von [**Color**](https://docs.microsoft.com/uwp/api/Windows.UI.Color), [**Double**](https://docs.microsoft.com/dotnet/api/system.double) und [**Point**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Point) können entweder mit Interpolations- oder Keyframeverfahren animiert werden. Die meisten anderen Werte können mithilfe von diskreten **Object**-Keyframes animiert werden.

Wenn eine Animation angewendet wurde und ausgeführt wird, verfügt der animierte Wert über eine höhere Priorität in der Rangfolge als alle anderen Werte (z. B. ein lokaler Wert), die der Eigenschaft sonst noch zugeordnet sind. Animationen können optional ein [**HoldEnd**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation.FillBehavior)-Verhalten aufweisen, das dazu führen kann, dass Animationen Eigenschaftswerte anwenden, obwohl die Animation augenscheinlich gestoppt wurde.

Das Prinzip des Zustandsautomaten liegt der Verwendung von Storyboardanimationen als Teil des [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager)-Zustandsmodells für Steuerelemente zugrunde. Weitere Informationen zu Storyboardanimationen finden Sie unter [Storyboardanimationen](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations). Weitere Informationen zu **VisualStateManager** und zur Definition von Ansichtszuständen für Steuerelemente finden Sie unter [Storyboardanimationen für visuelle Zustände](https://docs.microsoft.com/previous-versions/windows/apps/jj819808(v=win.10)) oder [Steuerelementvorlagen](../design/controls-and-patterns/control-templates.md).

### <a name="property-changed-behavior"></a>Verhalten bei „PropertyChanged“-Ereignis

Aufgrund des Verhaltens bei einem „PropertyChanged“-Ereignis wird eine Abhängigkeitseigenschaft überhaupt erst Abhängigkeitseigenschaft genannt. Die Erhaltung der Gültigkeit von Eigenschaftswerten in Konstellationen, in denen andere Eigenschaften den Wert der ersten Eigenschaft beeinflussen können, ist bei vielen Frameworks eine komplexe Herausforderung bei der Entwicklung. Im Windows-Runtime-Eigenschaftensystem kann jede Abhängigkeitseigenschaft ein Callback festlegen, das aufgerufen wird, sobald sich der Eigenschaftswert ändert. Mit diesem Callback können zugehörige Eigenschaften in der Regel synchron benachrichtigt oder deren Werte geändert werden. Viele bestehende Abhängigkeitseigenschaften haben ein Verhalten bei einem "PropertyChanged"-Ereignis. Sie können auch ein ähnliches Callback-Verhalten für Abhängigkeitseigenschaften festlegen und Ihre eigenen PropertyChanged-Callbacks implementieren. Unter [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md) finden Sie ein entsprechendes Beispiel.

Mit Windows 10 wird die Methode [**RegisterPropertyChangedCallback**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.registerpropertychangedcallback) eingeführt. Dadurch kann Anwendungscode für Benachrichtigungen registriert werden, wenn die angegebene Abhängigkeitseigenschaft für eine Instanz von [**DependencyObject**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject) geändert wird.

### <a name="default-value-and-clearvalue"></a>Standardwert und **ClearValue**

Für eine Abhängigkeitseigenschaft kann ein Standardwert als Teil der Metadaten der jeweiligen Eigenschaft definiert werden. Für eine Abhängigkeitseigenschaft wird ihr Standardwert nicht irrelevant, nachdem die Eigenschaft zum ersten Mal festgelegt wurde. Der Standardwert gilt zur Laufzeit möglicherweise jeweils erneut, wenn eine andere Determinante in der Wertrangfolge entfernt wird. (Der Vorrang von Abhängigkeitseigenschaftenwerten wird im nächsten Abschnitt erläutert). Zum Beispiel entfernen Sie unter Umständen zwar bewusst einen Formatwert oder eine Animation, der/die für eine Eigenschaft gilt, möchten aber, dass der Wert weiterhin ein angemessener Standardwert ist. Der Standardwert der Abhängigkeitseigenschaft kann diesen Wert bereitstellen, ohne dass die Werte der einzelnen Eigenschaften zusätzlich festgelegt werden müssen.

Sie können eine Eigenschaft auch dann absichtlich auf den Standardwert festlegen, wenn die Eigenschaft bereits einem lokalen Wert zugewiesen wurde. Rufen Sie die [**ClearValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.clearvalue)-Methode auf, um einen Wert auf den Standardwert zurückzusetzen und andere Teilnehmer in der Rangfolge zu aktivieren, die den Standardwert, nicht aber einen lokalen Wert außer Kraft setzen können (verweisen Sie auf die zu löschende Eigenschaft als Methodenparameter). Sie möchten nicht in allen Fällen, dass die Eigenschaft genau den Standardwert verwendet. Das Löschen des lokalen Werts und das Zurücksetzen auf den Standardwert kann jedoch zur Aktivierung eines anderen Elements in der Rangfolge führen, das sofort behandelt werden soll, z. B. bei der Verwendung des Werts aus einem Style Setter in einer Steuerelementvorlage.

## <a name="dependencyobject-and-threading"></a>**DependencyObject** und Threading

Alle [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)-Instanzen müssen in dem Benutzeroberflächenthread erstellt werden, der mit dem aktuellen [**Window**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Window) verknüpft ist, das von einer Windows-Runtime-App angezeigt wird. Obwohl jede **DependencyObject** im zentralen UI-Thread erstellt werden muss, kann durch den Zugriff auf die [**Dispatcher**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.dispatcher)-Eigenschaft mithilfe eines Verteilerverweises von anderen Threads aus auf die Objekte zugegriffen werden. Sie können anschließend Methoden wie [**RunAsync**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.runasync) für das [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher)-Objekt aufrufen und Ihren Code gemäß den Regeln der Threadeinschränkungen im UI-Thread ausführen.

Die Threadingmerkmale von [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject) sind relevant, da dies in der Regel bedeutet, dass nur im Benutzeroberflächenthread ausgeführter Code den Wert einer Abhängigkeitseigenschaft ändern oder auch nur lesen kann. Threadingprobleme können in typischem Benutzeroberflächencode in der Regel vermieden werden, wenn **async**-Muster und Hintergrundworkerthreads richtig verwendet werden. Threadingprobleme im Zusammenhang mit **DependencyObject** entstehen in der Regel nur, wenn Sie eigene **DependencyObject**-Typen definieren und versuchen, diese als Datenquellen oder für andere Szenarien zu verwenden, für die ein **DependencyObject** nicht notwendigerweise geeignet ist.

## <a name="related-topics"></a>Verwandte Themen

### <a name="conceptual-material"></a>Konzeptionelles Material

- [Benutzerdefinierte Abhängigkeitseigenschaften](custom-dependency-properties.md)
- [Übersicht über angefügte Eigenschaften](attached-properties-overview.md)
- [Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)
- [Storyboarding-Animationen](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations)
- [Erstellen von Windows-Runtime-Komponenten](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
- [XAML-Beispiel für Benutzer und benutzerdefinierte Steuerelemente](https://code.msdn.microsoft.com/windowsapps/XAML-user-and-custom-a8a9505e)

## <a name="apis-related-to-dependency-properties"></a>APIs im Zusammenhang mit Abhängigkeits Eigenschaften

- [**DependencyObject**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyObject)
- [**DependencyProperty**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DependencyProperty)

