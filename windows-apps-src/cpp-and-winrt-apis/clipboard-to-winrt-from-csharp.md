---
title: Portieren des Beispiels „Zwischenablage“ (Clipboard) von C# zu C++/WinRT –eine Fallstudie
description: Dieses Thema enthält eine Fallstudie für das Portieren einer der [UWP-App-Beispiele (Universal Windows Platform)](https://github.com/microsoft/Windows-universal-samples) von [C#](/visualstudio/get-started/csharp) nach [C++/WinRT](./intro-to-using-cpp-with-winrt.md).
ms.date: 04/13/2020
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, C#, Beispiel, Zwischenablage, Fall, Studie
ms.localizationpriority: medium
ms.openlocfilehash: 5a7ec46b28a8ddf0b4accadb37b40e786ac8c47a
ms.sourcegitcommit: 4df27104a9e346d6b9fb43184812441fe5ea3437
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/26/2020
ms.locfileid: "89170414"
---
# <a name="porting-the-clipboard-sample-to-cwinrt-from-cmdasha-case-study"></a>Portieren des Beispiels „Zwischenablage“ (Clipboard) von C# zu C++/WinRT – eine Fallstudie

Dieses Thema enthält eine Fallstudie für das Portieren einer der [UWP-App-Beispiele (Universal Windows Platform)](https://github.com/microsoft/Windows-universal-samples) von [C#](/visualstudio/get-started/csharp) nach [C++/WinRT](./intro-to-using-cpp-with-winrt.md). Sie können Praxis und Erfahrung mit dem Portieren sammeln, indem Sie der exemplarischen Vorgehensweise folgen und in deren Verlauf das Beispiel für sich selbst portieren.

Einen umfassenden Katalog der technischen Details der Portierung nach C++/WinRT von C# findest du im begleitenden Thema [Umstellen von C# auf C++/WinRT](./move-to-winrt-from-csharp.md).

## <a name="a-brief-preface-about-c-and-c-source-code-files"></a>Eine kurze Übersicht über C#- und C++-Quellcodedateien

In einem C#-Projekt sind die Quellcodedateien in erster Linie `.cs`-Dateien. Wenn Sie zu C++ wechseln, werden Sie feststellen, dass es mehr Arten von Quellcodedateien gibt, mit denen Sie sich vertraut machen müssen. Der Grund dafür liegt in dem Unterschied zwischen Compilern, der Art und Weise, wie C++-Quellcode wiederverwendet wird, und den Konzepten *Deklaration* und *Definition* von Typen sowie deren Funktionen (Methoden).

Die *Deklaration* einer Funktion beschreibt nur die *Signatur* der Funktion (ihren Rückgabetyp und dessen Namen sowie ihre Parametertypen und deren Namen). Die *Definition* einer Funktion enthält den *Text* der Funktion (ihre Implementierung).

Bei Typen ist es etwas anders. Sie *definieren* einen Typ, indem Sie seinen Namen angeben und (mindestens) alle seine Memberfunktionen (und andere Member) *deklarieren*. Richtig: Sie können einen Typ *definieren*, auch ohne seine Memberfunktionen zu definieren.

- Gängige C++-Quellcodedateien sind `.h`- und `.cpp`-Dateien. Bei einer `.h`-Datei handelt es sich um eine *Header*-Datei, die einen oder mehrere Typen definiert. Obwohl Sie Memberfunktionen in einem Header definieren *können*, wird dazu in der Regel eine `cpp`-Datei verwendet. Bei einem hypothetischen C++-Typ **MyClass** würden Sie also **MyClass** in `MyClass.h` und die Memberfunktionen in `MyClass.cpp` definieren. Damit andere Entwickler Ihre Klassen wiederverwenden können, brauchen Sie nur die `.h`-Dateien und den Objektcode freizugeben. Sie sollten ihre `.cpp`-Dateien geheim halten, da die Implementierung Ihr geistiges Eigentum darstellt.
- Vorkompilierter Header (`pch.h`). In der Regel gibt es eine Reihe von Headerdateien, die Sie in Ihre Anwendung einschließen, und die nur selten geändert werden. Anstatt den Inhalt dieses Satzes von Headern also bei jeder Kompilierung zu verarbeiten, können Sie die Header zu einem aggregieren, diesen einmal kompilieren und die Ausgabe dieses Vorkompilierungsschritts dann bei jeder Erstellung verwenden. Dies erfolgt über eine *vorkompilierte Headerdatei* (üblicherweise mit dem Namen `pch.h`).
- `.idl`-Dateien. Diese Dateien enthalten Interface Definition Language (IDL). Stellen Sie sich IDL als Headerdateien für Windows-Runtime-Typen vor. Weitere Informationen zu IDL finden Sie im Abschnitt [IDL für den **MainPage**-Typ](#idl-for-the-mainpage-type).

## <a name="download-and-test-the-clipboard-sample"></a>Herunterladen und Testen des Beispiels „Zwischenablage“

Besuchen Sie die Webseite des [Beispiels „Zwischenablage“](/samples/microsoft/windows-universal-samples/clipboard/), und klicken Sie auf **ZIP herunterladen**. Entpacken Sie die heruntergeladene Datei, und werfen Sie einen Blick auf die Ordnerstruktur.

- Die C#-Version des Beispielquellcodes befindet sich im Ordner mit dem Namen `cs`.
- Die C++/WinRT-Version des Beispielquellcodes befindet sich im Ordner mit dem Namen `cppwinrt`.
- Andere Dateien (sowohl von der C#-Version als auch von der/WinRT-Version verwendete) befinden sich C++ in den Ordnern `shared` und `SharedContent`.

In der exemplarischen Vorgehensweise in diesem Thema wird gezeigt, wie Sie die C++/WinRT-Version des Beispiels „Clipboard“ neu erstellen können, indem Sie sie aus dem C#-Quellcode portieren. Auf diese Weise können Sie sehen, wie Sie Ihre eigenen C#-Projekte zu C++/WinRT portieren können.

Um ein Gefühl für die Funktionsweise des Beispiels zu erhalten, öffnen Sie die C#-Projektmappe (`\Clipboard_sample\cs\Clipboard.sln`), ändern Sie die Konfiguration nach Bedarf (vielleicht in *x64*), erstellen und führen Sie sie aus. Die eigene Benutzeroberfläche (User Interface, UI) des Beispiels führt Sie Schritt für Schritt durch seine verschiedenen Funktionen.

## <a name="create-a-blank-app-cwinrt-named-clipboard"></a>Erstellen einer leeren App (C++/WinRT) namens „Clipboard“

> [!NOTE]
> Informationen zum Installieren und Verwenden der C++/WinRT Visual Studio-Erweiterung (VSIX) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

Beginnen Sie mit dem Portierungsprozess, indem Sie ein neues C++/WinRT-Projekt in Microsoft Visual Studio erstellen. Erstellen Sie ein neues Projekt anhand der Projektvorlage **Leere App (C++/WinRT)** . Legen Sie seinen Namen auf *Clipboard* fest, und stellen Sie sicher, dass **Legen Sie die Projektmappe und das Projekt im selben Verzeichnis ab** deaktiviert ist (damit Ihre Ordnerstruktur mit der exemplarischen Vorgehensweise übereinstimmt).

Stellen Sie, nur um eine Baseline zu erhalten, sicher, dass sich dieses neue, leere Projekt erstellen und ausführen lässt.

## <a name="packageappxmanifest-and-asset-files"></a>„Package.appxmanifest“ und Objektdateien

Wenn die C#- und C++/WinRT-Versionen des Beispiels nicht parallel auf demselben Computer installiert werden müssen, können die Quelldateien für das App-Paketmanifest (`Package.appxmanifest`) der beiden Projekte identisch sein. In diesem Fall können Sie einfach `Package.appxmanifest` aus dem C#-Projekt in das C++/WinRT-Projekt kopieren, und fertig sind Sie.

Damit die beiden Versionen des Beispiels parallel vorhanden sein können, benötigen sie unterschiedliche Bezeichner. Öffnen Sie in diesem Fall im C++/WinRT-Projekt die Datei `Package.appxmanifest` in einem XML-Editor, und notieren Sie sich diese drei Werte.

- Notieren Sie den Wert des **Name**-Attributs innerhalb des **/Package/Identity**-Elements. Dies ist der *Paketname*. Bei einem neu erstellten Projekt vergibt das Projekt dafür als Anfangswert eine eindeutige GUID.
- Notieren Sie den Wert des **ID**-Attributs innerhalb des **/Package/Applications/Application**-Elements. Dies ist die *Anwendungs-ID*.
- Notieren Sie den Wert des **PhoneProductId**-Attributs innerhalb des **/Package/mp:PhoneIdentity**-Elements. Auch hier wird für ein neu erstelltes Projekt dies auf dieselbe GUID festgelegt, auf die auch der Paketname festgelegt ist.

Kopieren Sie dann `Package.appxmanifest` aus dem C#-Projekt in das C++/WinRT-Projekt. Schließlich können Sie die drei Werte wiederherstellen, die Sie notiert haben. Oder Sie bearbeiten die kopierten Werte so, dass sie für die Anwendung und für Ihre Organisation eindeutig und/oder angemessen sind (wie Sie es normalerweise für ein neues Projekt tun würden). In diesem Fall können wir beispielsweise, anstatt den Wert des Paketnamens wiederherzustellen, einfach den kopierten Wert von *Microsoft.SDKSamples.Clipboard.CS* in *Microsoft.SDKSamples.Clipboard.CppWinRT* ändern. Und die Anwendungs-ID können wir auf *App* festgelegt lassen. Solange der Paketname *oder* die Anwendungs-ID unterschiedlich ist, verfügen die beiden Anwendungen über unterschiedliche App-Benutzermodell-IDs (Application User Model IDs, AUMIDs).

Im Rahmen dieser exemplarischen Vorgehensweise ist es sinnvoll, ein paar weitere Änderungen in `Package.appxmanifest` vorzunehmen. Es gibt drei Vorkommen der Zeichenfolge *Clipboard C# Sample*. Ändern Sie diese in *Clipboard C++/WinRT Sample*.

Im C++/WinRT-Projekt sind die `Package.appxmanifest`-Datei und das Projekt in Bezug auf die Objektdateien, auf die sie verweisen, nicht mehr synchronisiert. Um dies zu beheben, entfernen Sie zuerst die Objekte aus dem C++/WinRT-Projekt, indem Sie alle Dateien im Ordner `Assets` (im Projektmappen-Explorer in Visual Studio) auswählen und sie entfernen (im Dialogfeld **Löschen** auswählen).

Das C#-Projekt verweist auf Objektdateien aus einem freigegebenen Ordner. Dasselbe können Sie im C++/WinRT-Projekt machen, oder Sie können die Dateien kopieren, wie wir es in dieser exemplarischen Vorgehensweise tun.

Navigieren Sie zum Ordner `\Clipboard_sample\SharedContent\media`. Wählen Sie die sieben Dateien aus, die das C#-Projekt enthält (`microsoft-sdk.png` bis `windows-sdk.png`), kopieren Sie sie, und fügen Sie sie in den Ordner `\Clipboard\Clipboard\Assets` im neuen Projekt ein.

Klicken Sie mit der rechten Maustaste auf den Ordner `Assets` (im Projektmappen-Explorer im C++/WinRT-Projekt) > **Hinzufügen** > **Vorhandenes Element...** , und navigieren Sie zu `\Clipboard\Clipboard\Assets`. Wählen Sie in der Dateiauswahl die sieben Dateien aus, und klicken Sie auf **Hinzufügen**.

`Package.appxmanifest` ist nun wieder synchron mit den Objektdateien des Projekts.

## <a name="mainpage-including-the-functionality-that-configures-the-sample"></a>**MainPage**, einschließlich der Funktionen zum Konfigurieren des Beispiels

Das Beispiel „Zwischenablage“, wie alle [UWP-App-Beispiele (Universelle Windows-Plattform)](https://github.com/microsoft/Windows-universal-samples), besteht aus einer Sammlung von Szenarien, die der Benutzer nacheinander schrittweise durchlaufen kann. Die Sammlung der Szenarien in einem bestimmten Beispiel wird im Quellcode des Beispiels konfiguriert. Jedes Szenario in der Sammlung ist ein Datenelement, das einen Titel speichert, sowie den Typ der Klasse im Projekt, das das Szenario implementiert.

In der C#-Version des Beispiels sehen Sie, wenn Sie in die `SampleConfiguration.cs`-Quellcodedatei sehen, zwei Klassen. Der größte Teil der Konfigurationslogik befindet sich in der **MainPage**-Klasse, bei der es sich um eine partielle Klasse handelt (sie bildet eine vollständige Klasse, wenn sie mit dem Markup in `MainPage.xaml` und dem imperativen Code in `MainPage.xaml.cs` kombiniert wird). Die andere Klasse in dieser Quellcodedatei ist **Scenario** mit ihren Eigenschaften **Title** und **ClassType**.

In den nächsten Unterabschnitten sehen wir uns an, wie **MainPage** und **Scenario** portiert werden.

### <a name="idl-for-the-mainpage-type"></a>IDL für den Typ **MainPage**

Beginnen wir diesen Abschnitt, indem wir kurz über Interface Definition Language (IDL) sprechen und erfahren, wie IDL uns beim Programmieren mit C++/WinRT hilft. IDL ist eine Art von Quellcode, der die aufrufbare Oberfläche eines Windows-Runtime Typs beschreibt. Die aufrufbare (oder öffentliche) Oberfläche eines Typs wird in die Welt *projiziert*, damit der Typ verwendet werden kann. Dieser *projizierte* Teil des Typs steht im Gegensatz zur eigentlichen internen Implementierung des Typs, die natürlich nicht aufrufbar und nicht öffentlich ist. Es handelt sich lediglich um den projizierten Teil, der in IDL definiert wird.

Wenn Sie IDL-Quellcode (in einer `.idl`-Datei) erstellt haben, können Sie die IDL in maschinell lesbare Metadatendateien (auch als Windows-Metadaten bezeichnet) kompilieren. Diese Metadatendateien weisen die Erweiterung `.winmd` auf. Im folgenden finden Sie einige ihrer Verwendungsmöglichkeiten.

- Eine `.winmd` kann die Windows-Runtime-Typen in einer Komponente beschreiben. Wenn Sie von einem Anwendungsprojekt aus auf eine Windows-Runtime Komponente (WRC) verweisen, liest das Anwendungsprojekt die Windows-Metadaten, die zum WRC gehören (die Metadaten können sich in einer separaten Datei befinden, oder sie können in dieselbe Datei wie das WRC selbst gepackt werden), damit Sie die WRC-Typen aus der Anwendung heraus verwenden können.
- Eine `.winmd` kann die Windows-Runtime-Typen in einem Teil Ihrer Anwendung beschreiben, damit sie von einem anderen Teil der gleichen Anwendung verwendet werden können. Beispiel: Ein Windows-Runtime Typ, der von einer XAML-Seite in der gleichen App verwendet wird.
- Um Ihnen die Verwendung von Windows-Runtime-Typen (integriert oder von Drittanbietern) zu erleichtern, verwendet das C++/WinRT-Buildsystem `.winmd`-Dateien zur Generierung von Wrappertypen, die die projizierten Teile dieser Windows-Runtime-Typen darstellen.
- Um Ihnen die Implementierung Ihrer eigenen Windows-Runtime-Typen zu erleichtern, wandelt das C++/WinRT-Buildsystem ihre IDL in eine `.winmd`-Datei um und verwendet diese dann zum Generieren von Wrappern für die Projektion sowie Stubs, auf denen Ihre Implementierung basieren soll. (Weitere Informationen zu diesen Stubs finden Sie weiter unten in diesem Thema).

Die spezifische IDL-Version, die mit C++/WinRT verwendet wird, ist [Microsoft Interface Definition Language 3.0](/uwp/midl-3/intro). Im restlichen Teil dieses Abschnitts wird der C#-Typ **MainPage** ausführlich behandelt. Wir legen fest, welche Teile davon in der *Projektion* des C++/WinRT-Typs **MainPage** (d. h. in der aufrufbaren oder öffentlichen Oberfläche) enthalten sein sollen, und die welche nur Teil der Implementierung sein können. Dieser Unterschied ist wichtig, denn wenn wir unsere IDL erstellen (was wir nachfolgenden Abschnitt tun werden) definieren wir darin nur die aufrufbaren Teile.

Die C#-Quellcodedateien, die zusammen den **MainPage**-Typ implementieren, sind: `MainPage.xaml` (wird bald durch Kopieren portiert), `MainPage.xaml.cs` und `SampleConfiguration.cs`.

In der C++/WinRT-Version beziehen wir unseren **MainPage**-Typ auf ähnliche Weise in die Quellcodedateien ein. Wir übernehmen die Logik aus `MainPage.xaml.cs` und übersetzen sie zum größten Teil in `MainPage.h` und `MainPage.cpp`. Und die Logik in `SampleConfiguration.cs` übersetzen wir in `SampleConfiguration.h` und `SampleConfiguration.cpp`.

Die Klassen in einer C#-UWP-Anwendung (Universelle Windows-Plattform) sind natürlich Windows-Runtime-Typen. Wenn Sie jedoch einen Typ in einer C++/WinRT-Anwendung erstellen, können Sie auswählen, ob dieser Typ ein Windows-Runtime-Typ oder eine reguläre C++-Klasse/Struktur/Enumeration sein soll.

Jede XAML-Seite in unserem Projekt muss ein Windows-Runtime Typ sein, daher muss auch **MainPage-** ein Windows-Runtime Typ sein. Im C++/WinRT-Projekt ist **MainPage** bereits ein Windows-Runtime-Typ, sodass wir diesen Aspekt nicht mehr ändern müssen. Es handelt sich insbesondere um eine *Laufzeitklasse*.

- Weitere Informationen dazu, ob Sie eine Laufzeitklasse für einen bestimmten Typ erstellen oder nicht erstellen sollten, finden Sie im Thema [Erstellen von APIs mit C++/WinRT](./author-apis.md).
- In C++/WinRT sind die interne Implementierung einer Laufzeitklasse und die projizierten (öffentlichen) Teile davon in Form von zwei verschiedenen Klassen vorhanden. Diese werden als *Implementierungstyp* und *projizierter Typ* bezeichnet. Weitere Informationen hierzu finden Sie im oben in der Aufzählung erwähnten Thema sowie unter [Verwenden von APIs mit C++/WinRT](./consume-apis.md).
- Weitere Informationen über die Verbindung zwischen Laufzeitklassen und IDL (`.idl`-Dateien) finden Sie im Thema [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](./binding-property.md), das Sie auch durcharbeiten können. Dieses Thema führt Sie durch den Vorgang der Erstellung einer neuen Laufzeitklasse, wobei der erste Schritt darin besteht, dem Projekt ein neues Element vom Typ **Midl-Datei (.idl)** hinzuzufügen.

Für **MainPage** verfügen wir bereits über die erforderliche `MainPage.idl`-Datei im C++/WinRT-Projekt. Das liegt daran, dass die Projektvorlage diese für uns erstellt hat. Später in dieser exemplarischen Vorgehensweise fügen wir dem Projekt jedoch weitere `.idl`-Dateien hinzu.

In Kürze werden Sie ein Listing exakt der IDL sehen, die wir der vorhandenen `MainPage.idl`-Datei hinzufügen müssen. Doch zuvor müssen wir noch besprechen, was in die IDL aufgenommen werden muss und was nicht.

Um zu ermitteln, welche Member von **MainPage** wir in `MainPage.idl` deklarieren müssen (damit Sie Teil der **MainPage**-Laufzeitklasse werden) und welche einfach Member des **MainPage**-Implementierungstyps sein können, erstellen wir eine Liste der Member der C#-Klasse **MainPage**. Diese Member finden wir durch Nachsehen in `MainPage.xaml.cs` und `SampleConfiguration.cs`.

Wir finden insgesamt zwölf `protected` und `private` Felder und Methoden. Außerdem finden wir die folgenden `public`-Member.

- Den Standardkonstruktor `MainPage()`.
- Die statischen Felder **Current** und **FEATURE_NAME**.
- Die Eigenschaften **IsClipboardContentChangedEnabled** und **Scenarios**.
- Die Methoden **BuildClipboardFormatsOutputString**, **DisplayToast**, **EnableClipboardContentChangedNotifications** und **NotifyUser**.

Es sind diese `public` Member, die Kandidaten für die Deklaration in `MainPage.idl` sind. Untersuchen wir also alle einzeln, um zu sehen, ob Sie Teil der **MainPage**-Laufzeitklasse sein müssen, oder ob Sie nur Teil ihrer Implementierung sein müssen.

- Den Standardkonstruktor `MainPage()`. Für eine XAML-**Seite** ist es normal, einen Standardkonstruktor in ihrer IDL zu deklarieren. Auf diese Weise kann das XAML-Benutzeroberflächen-Framework den Typ aktivieren.
- Das statische Feld **Current** wird aus den einzelnen XAML-Seiten des jeweiligen Szenarios verwendet, um auf die **MainPage**-Instanz der Anwendung zuzugreifen. Da **Current** nicht für die Interaktion mit dem XAML-Framework verwendet wird (und auch nicht über Kompilierungseinheiten hinweg), können wir es reservieren, damit es nur ein Member des Implementierungstyps ist. Bei Ihren eigenen Projekten könnten Sie in solchen Fällen so vorgehen. Da es sich aber bei dem Feld um eine Instanz des projizierten Typs handelt, erscheint es logisch, diesen in der IDL zu deklarieren. Genau das machen wir also hier (wodurch der Code auch noch etwas übersichtlicher wird).
- Bei dem statischen Feld **FEATURE_NAME**, auf das innerhalb des **MainPage**-Typs zugegriffen wird, sieht dies ähnlich aus. Wiederum gestaltet sich unser Code durch die Entscheidung für eine Deklaration in der IDL übersichtlicher.
- Die Eigenschaft **IsClipboardContentChangedEnabled** wird nur in der **OtherScenarios**-Klasse verwendet. Während der Portierung werden wir also die Dinge ein bisschen vereinfachen, und machen daraus ein privates Feld der **OtherScenarios**-Laufzeitklasse. Diese wird also nicht in die IDL aufgenommen.
- Die Eigenschaften **Scenarios** ist eine Sammlung von Objekten des Typs **Scenario** (ein Typ, den wir zuvor schon erwähnt haben). **Scenario**  werden wir im nächsten Unterabschnitt besprechen, weshalb wir uns auch erst dort um die Eigenschaft **Scenarios** kümmern werden.
- Die Methoden **BuildClipboardFormatsOutputString**, **DisplayToast** und **EnableClipboardContentChangedNotifications** sind Hilfsfunktionen, die mehr mit dem allgemeinen Zustand des Beispiels als mit der Hauptseite zu tun zu haben scheinen. Deshalb werden wir diese drei Methoden während der Portierung in einen neuen Hilfstyp namens **SampleState** neu einbeziehen (der kein Windows-Runtime-Typ sein muss). Aus diesem Grund werden diese drei Methoden nicht in die IDL aufgenommen.
- Die Methode **NotifyUser** wird aus den einzelnen XAML-Seiten des Szenarios für die Instanz von **MainPage** aufgerufen, die von dem statischen Feld *Current* zurückgegeben wird. Da (wie bereits erwähnt) **Current** eine Instanz des projizierten Typs ist, müssen wir **NotifyUser** in der IDL deklarieren. **NotifyUser** nimmt einen Parameter vom Typ **NotifyType**. Dies wird im nächsten Unterabschnitt näher erläutert.

Jedes Member, mit dem Sie eine Datenbindung herstellen möchten, muss auch in IDL deklariert werden (unabhängig davon, ob Sie `{x:Bind}` oder `{Binding}` verwenden). Weitere Informationen finden Sie unter [Datenbindung](../data-binding/index.md).

Wir machen Fortschritte: Wir entwickeln eine Liste der Member, die der Datei `MainPage.idl` hinzugefügt werden sollen, und der, die nicht hinzugefügt werden sollen. Aber wir müssen immer noch die Eigenschaft **Scenarios** und den Typ **NotifyType** besprechen. Das machen wir nun als Nächstes.

### <a name="idl-for-the-scenario-and-notifytype-types"></a>IDL für die Typen **Scenario** und **NotifyType**

Die **Scenario**-Klasse ist in `SampleConfiguration.cs` definiert. Wir müssen eine Entscheidung treffen, wie wir diese Klasse zu C++/WinRT portieren. Standardmäßig würden wir dies wahrscheinlich als normale C++ `struct` schreiben. Wenn aber **Scenario** in Binärdateien verwendet wird, oder um mit dem XAML-Framework zusammenzuarbeiten, muss die Deklaration in der IDL als Windows-Runtime-Typ erfolgen.

Wenn wir uns den C#-Quellcode näher ansehen, stellen wir fest, dass **Scenario** in diesem Kontext verwendet wird.

```xaml
<ListBox x:Name="ScenarioControl" ... >
```

```csharp
var itemCollection = new List<Scenario>();
int i = 1;
foreach (Scenario s in scenarios)
{
    itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
}
ScenarioControl.ItemsSource = itemCollection;
```

Eine Sammlung von **Scenario**-Objekten wird der [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)-Eigenschaft einer **ListBox** (wobei es sich um ein Items-Steuerelement handelt) zugewiesen. Da **Scenario** mit XAML interagieren *muss*, muss es ein Windows-Runtime-Typ sein. Deshalb muss es in IDL definiert werden. Die Definition des **Scenario**-Typs in IDL bewirkt, dass das C++/WinRT-Buildsystem eine Quellcodedefinition von **Scenario** für Sie in einer verdeckten Headerdatei generiert (deren Name und Speicherort sind für diese exemplarische Vorgehensweise nicht wichtig).

Beachten Sie außerdem, dass **MainPage.Scenarios** eine Sammlung von **Scenario**-Objekten ist, von denen wir gerade gesagt haben, dass sie in der IDL enthalten sein müssen. Aus diesem Grund muss **MainPage.Scenarios** auch in der IDL deklariert werden.

**NotifyType** ist eine in der `MainPage.xaml.cs` von C# deklarierte `enum`. Da wir **NotifyType** an eine Methode übergeben, die zur **MainPage**-Laufzeitklasse gehört, muss **NotifyType** ebenfalls ein Windows-Runtime-Typ sein, und es muss in `MainPage.idl` definiert sein.

Fügen wir nun die neuen Typen zur Datei `MainPage.idl` hinzu sowie den neuen Member von **MainPage-** , für dessen Deklaration in IDL wir uns entschieden haben. Gleichzeitig entfernen wir die Platzhaltermember von **MainPage** aus der IDL, die wir durch die Visual Studio-Projektvorlage erhalten haben.

Öffnen Sie also in Ihrem C++/WinRT-Projekt `MainPage.idl`, und bearbeiten Sie sie so, dass sie wie das folgende Listing aussieht. Beachten Sie, dass einer der Bearbeitungen darin besteht, den Namen des Namespace von **Clipboard** in **SDKTemplate** zu ändern. Be Bedarf kannst du auch den gesamten Inhalt von `MainPage.idl` durch den folgenden Code ersetzen. Eine weitere anzumerkende Änderung ist, dass wir den Namen von **Scenario::ClassType** in **Scenario::ClassName** ändern.

```idl
// MainPage.idl
namespace SDKTemplate
{
    struct Scenario
    {
        String Title;
        Windows.UI.Xaml.Interop.TypeName ClassName;
    };

    enum NotifyType
    {
        StatusMessage,
        ErrorMessage
    };

    [default_interface]
    runtimeclass MainPage : Windows.UI.Xaml.Controls.Page
    {
        MainPage();

        static MainPage Current{ get; };
        static String FEATURE_NAME{ get; };

        static Windows.Foundation.Collections.IVector<Scenario> scenarios{ get; };

        void NotifyUser(String strMessage, NotifyType type);
    };
}
```

> [!NOTE]
> Weitere Informationen zum Inhalt einer `.idl`-Datei in einem C++/WinRT-Projekt finden Sie unter [Microsoft Interface Definition Language 3.0](/uwp/midl-3/).

Bei Ihrem eigenen Portiervorgang möchten oder müssen Sie vielleicht den Namen des Namespace nicht ändern, wie wir es zuvor getan haben. Wir machen dies hier auch nur, weil der Standardnamespace des C#-Projekts, das wir portieren, **SDKTemplate** ist, während der Name des Projekts und der Assembly **Clipboard** lautet.

Während wir aber mit der Portierung in dieser exemplarischen Vorgehensweise fortschreiten, ändern wir jedes Vorkommen des Namespacenamens **Clipboard** im Quellcode in **SDKTemplate**. Es gibt auch eine Stelle in den C++/WinRT-Projekteigenschaften, wo der Namespacename **Clipboard** vorkommt, sodass wir die Gelegenheit nutzen, dies jetzt zu ändern.

Legen Sie in Visual Studio für das C++/WinRT-Projekt die Projekteigenschaft **Allgemeine Eigenschaften** \> **C++/WinRT** \> **Stammnamespace** auf den Wert *SDKTemplate*.

### <a name="save-the-idl-and-re-generate-stub-files"></a>Speichern der IDL und erneutes Generieren von Stubdateien

Im Thema [XAML-Steuerelemente; Binden an eine C++/WinRT-Eigenschaft](./binding-property.md) wird das Konzept der *Stubdateien* eingeführt und ihre Funktionsweise in einer exemplarischen Vorgehensweise beschrieben. Wir haben Stubs bereits zuvor in diesem Thema erwähnt, als wir erläutert haben, dass das C++/WinRT-Buildsystem den Inhalt Ihrer `.idl`-Dateien in Windows-Metadaten umwandelt und dass dann ein Tool mit dem Namen `cppwinrt.exe` Stubs generiert, auf denen Sie Ihre Implementierung aufbauen können.

Jedes Mal, wenn Sie etwas in ihrer IDL hinzufügen, entfernen oder ändern und erstellen, aktualisiert das Buildsystem die Stubimplementierungen in diesen Stubdateien. Wenn Sie also Ihre IDL und Ihr Build ändern, empfiehlt es sich, diese Stubdateien anzuzeigen, geänderte Signaturen zu kopieren und sie in das Projekt einzufügen. Wir werden gleich weitere Einzelheiten und Beispiele dafür nennen, wie genau das zu tun ist. Der Vorteil dieser Vorgehensweise besteht jedoch darin, dass Sie jederzeit und fehlerfrei wissen, wie die Form Ihres Implementierungstyps und die Signatur der Methoden aussehen sollten.

An dieser Stelle in der exemplarischen Vorgehensweise sind wir mit dem Bearbeiten der `MainPage.idl`-Datei fertig, weshalb Sie sie jetzt speichern sollten. Im aktuellen Zustand wird das Projekt zwar nicht vollständig erstellt, doch jetzt eine Erstellung durchzuführen ist hilfreich, da dadurch die Stubdateien für **MainPage** neu generiert werden.

Für dieses C++/WinRT-Projekt werden die Stubdateien im Ordner `\Clipboard\Clipboard\Generated Files\sources` generiert. Sie finden sie dort im Anschluss an die partielle Erstellung (Noch einmal: Die Erstellung wird erwartungsgemäß nicht vollständig erfolgen. Doch der Schritt, an dem wir interessiert sind, nämlich das Generieren von Stubs, *wird erfolgreich* verlaufen sein.). Die Dateien, die uns interessieren, sind `MainPage.h` und `MainPage.cpp`.

In diesen zwei Stubdateien sehen Sie neue Stubimplementierungen der Member von **MainPage**, die wir der IDL hinzugefügt haben (z. B. **Current** und **FEATURE_NAME**). Kopieren Sie diese Stubimplementierungen in die Dateien `MainPage.h` und `MainPage.cpp`, die sich bereits im Projekt befinden. Gleichzeitig entfernen wir, ebenso wie wir es mit der IDL gemacht haben, die Platzhaltermember von **MainPage**, die in der Visual Studio-Projektvorlage enthalten waren, aus diesen vorhandenen Dateien (die Dummyeigenschaft namens **MyProperty** sowie den Ereignishandler namens **ClickHandler**).

Tatsächlich ist der einzige Member der aktuellen Version von **MainPage**, den wir behalten möchten, der Konstruktor.

Nachdem Sie die neuen Member aus den Stubdateien kopiert, die unerwünschten Member gelöscht und den Namespace aktualisiert haben, sollten die Dateien `MainPage.h` und `MainPage.cpp` in Ihrem Projekt wie die unten stehenden Codelistings aussehen. Beachten Sie, dass es zwei **MainPage**-Typen gibt. Eine im Namespace **implementation** und eine zweite im Namespace **factory_implementation**. Die einzige Änderung, die wir am Namespace **factory_implementation** vorgenommen haben, besteht darin, dass **SDKTemplate** zum Namespace hinzugefügt wurde.

```cppwinrt
// MainPage.h
#pragma once
#include "MainPage.g.h"

namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
        MainPage();

        static SDKTemplate::MainPage Current();
        static hstring FEATURE_NAME();
        static Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> scenarios();
        void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
    };
}
namespace winrt::SDKTemplate::factory_implementation
{
    struct MainPage : MainPageT<MainPage, implementation::MainPage>
    {
    };
}
```

```cppwinrt
// MainPage.cpp
#include "pch.h"
#include "MainPage.h"
#include "MainPage.g.cpp"

namespace winrt::SDKTemplate::implementation
{
    MainPage::MainPage()
    {
        InitializeComponent();
    }
    SDKTemplate::MainPage MainPage::Current()
    {
        throw hresult_not_implemented();
    }
    hstring MainPage::FEATURE_NAME()
    {
        throw hresult_not_implemented();
    }
    Windows::Foundation::Collections::IVector<SDKTemplate::Scenario> MainPage::scenarios()
    {
        throw hresult_not_implemented();
    }
    void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
    {
        throw hresult_not_implemented();
    }
}
```

Für Zeichenfolgen verwendet C# **System.String**. Ein Beispiel finden Sie in der **MainPage.NotifyUser**-Methode. In unserer IDL deklarieren wir eine Zeichenfolge mit **String**, und wenn das Tool `cppwinrt.exe` den C++/WinRT-Code für uns generiert, verwendet es den Typ [**winrt::hstring**](/uw/cpp-ref-for-winrt/hstring). Immer, wenn wir eine Zeichenfolge im C#-Code finden, portieren wir diese zu **winrt::hstring**. Weitere Informationen finden Sie unter [Verarbeitung von Zeichenfolgen in C++/WinRT](./strings.md).

Eine Erläuterung der `const&`-Parameter in den Methodensignaturen finden Sie unter [Parameterübergabe](./concurrency.md#parameter-passing).

### <a name="update-all-remaining-namespace-declarationsreferences-and-build"></a>Aktualisieren aller verbleibenden Namespacedeklarationen/-verweise und Erstellen

Suchen Sie vor dem Erstellen des C++/WinRT-Projekts alle Deklarationen von (und Verweise auf) den Namespace **Clipboard**, und ändern Sie diese in **SDKTemplate**.

- `MainPage.xaml` und `App.xaml`. Der Namespace wird in den Werten der Attribute `x:Class` und `xmlns:local` angezeigt.
- `App.idl`.
- `App.h`.
- `App.cpp`. Es gibt zwei `using namespace`-Direktiven (suchen Sie nach der Teilzeichenfolge `using namespace Clipboard`) und zwei Qualifikationen des **MainPage**-Typs (suchen Sie nach `Clipboard::MainPage`). Diese müssen geändert werden.

Da wir den Ereignishandler von **MainPage** entfernt haben, wechseln Sie auch in die `MainPage.xaml`, und löschen Sie das **Button**-Element aus dem Markup.

Speichern Sie alle Dateien. Bereinigen Sie die Projektmappe (**Erstellen** > **Clipboard bereinigen**), und erstellen Sie sie dann. Die Erstellung verläuft erfolgreich, wenn die bisher von Ihnen vorgenommenen Änderungen gültig sind.

### <a name="implement-the-mainpage-members-that-we-declared-in-idl"></a>Implementieren Sie die **MainPage**-Member, die wir in IDL deklariert haben.

#### <a name="the-constructor-current-and-feature_name"></a>Den Konstruktor, **Current** und **FEATURE_NAME**.

Hier finden Sie den relevanten Code (aus dem C#-Projekt), den wir portieren müssen.

```xaml
<!-- MainPage.xaml -->
...
<TextBlock x:Name="SampleTitle" ... />
...
```

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
    public static MainPage Current;

    public MainPage()
    {
        InitializeComponent();
        Current = this;
        SampleTitle.Text = FEATURE_NAME;
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
    public const string FEATURE_NAME = "Clipboard C# sample";
...
}
...
```

Wir werden `MainPage.xaml` in Kürze vollständig wiederverwenden (durch Kopieren). Vorerst fügen wir vorübergehend ein **TextBlock**-Element mit dem entsprechenden Namen in die `MainPage.xaml` des C++/WinRT-Projekts ein.

**FEATURE_NAME** ist ein statisches Feld von **MainPage** (ein C#-Feld vom Typ `const` ist hinsichtlich seines Verhaltens im Wesentlichen statisch), das in `SampleConfiguration.cs` definiert ist. Für C++/WinRT machen wir anstelle eines (statischen) Felds daraus den C++/WinRT-Ausdruck einer (statischen) schreibgeschützten Eigenschaft. Die C++/WinRT-Methode zum Ausdrücken eines Eigenschaften-Getters ist eine Funktion, die den Eigenschaftswert zurückgibt und keinen Parameter (ein Accessor) annimmt. Somit wird das statische C#-Feld **FEATURE_NAME** zur statischen C++/WinRT-Accessorfunktion **FEATURE_NAME** (die in diesem Fall das Zeichenfolgenliteral zurückgibt).

Übrigens würden wir dasselbe tun, wenn wir eine schreibgeschützte C#-Eigenschaft portieren würden. Bei einer schreibbaren C#-Eigenschaft ist die C++/WinRT-Methode zum Ausdrücken eines Eigenschaften-Setters eine `void`-Funktion, die den Eigenschaftswert als Parameter (ein Mutator) nimmt. In beiden Fällen ist, wenn das C#-Feld oder die C#-Eigenschaft statisch ist, der C++/WinRT-Accessor und/oder -Mutator ebenfalls statisch.

**Current** ist ein statisches (kein konstantes) Feld von **MainPage-** . Wir machen wiederum eine schreibgeschützt Eigenschaft daraus (dem C++/WinRT-Ausdruck), die wir auch wieder statisch machen. Wenn **FEATURE_NAME** konstant ist, ist **Current** dies nicht. Daher benötigen wir in C++/WinRT ein Unterstützungsfeld, das dann von unserem Accessor zurückgegeben wird. Im C++/WinRT-Projekt deklarieren wir also in `MainPage.h` ein privates statisches Feld mit dem Namen **current**, wir definieren/initialisieren **current** in `MainPage.cpp` (weil es eine statische Speicherdauer hat), und wir greifen über eine öffentliche statische Accessorfunktion mit dem Namen **Current** darauf zu.

Der Konstruktor selbst führt einige Zuweisungen durch, die einfach zu portieren sind.

Fügen Sie im C++/WinRT-Projekt ein neues **Visual C++**  > **Code** > **C++-Datei** element (.cpp) mit dem Namen `SampleConfiguration.cpp` hinzu.

Bearbeiten Sie `MainPage.xaml`, `MainPage.h`, `MainPage.cpp` und `SampleConfiguration.cpp` so, dass sie den nachstehenden Listings entsprechen.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    <TextBlock x:Name="SampleTitle" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
namespace winrt::SDKTemplate::implementation
{
    struct MainPage : MainPageT<MainPage>
    {
...
        static SDKTemplate::MainPage Current() { return current; }
...
    private:
        static SDKTemplate::MainPage current;
...
    };
...
}

// MainPage.cpp
...
namespace winrt::SDKTemplate::implementation
{
    SDKTemplate::MainPage MainPage::current{ nullptr };
...
    MainPage::MainPage()
    {
        InitializeComponent();
        MainPage::current = *this;
        SampleTitle().Text(FEATURE_NAME());
    }
...
}

// SampleConfiguration.cpp
#include "pch.h"
#include "MainPage.h"

using namespace winrt;
using namespace SDKTemplate;

hstring implementation::MainPage::FEATURE_NAME()
{
    return L"Clipboard C++/WinRT Sample";
}
```

Stellen Sie außerdem sicher, dass Sie die vorhandenen Funktionskörper aus `MainPage.cpp` für **MainPage::Current()** und **MainPage::FEATURE_NAME()** löschen, da wir diese Methoden nun an anderer Stelle definieren.

Wie Sie sehen, wird **MainPage::current** als Typ **SDKTemplate::MainPage** deklariert, bei dem es sich um den projizierten Typ handelt. Es ist nicht vom Typ **SDKTemplate::implementation::MainPage**, wobei es sich um den Implementierungstyp handelt. Der projizierte Typ ist derjenige, der darauf ausgelegt ist, entweder innerhalb des Projekts für die XAML-Interaktion oder über Binärdateien hinweg verwendet zu werden. Mit dem Implementierungstyp implementieren Sie die Funktionen, die Sie in Ihrem projizierten Typ verfügbar gemacht haben. Da die Deklaration von **MainPage::current** (in `MainPage.h`) im Implementierungsnamespace (**winrt::SDKTemplate::implementation**) vorkommt, hätte eine nicht qualifizierte **MainPage** auf den Implementierungstyp verwiesen. Also qualifizieren wir mittels **SDKTemplate::** , um klar zu zeigen, dass **MainPage::current-** eine Instanz des projizierten Typs **winrt::SDKTemplate::MainPage** sein soll.

Im Konstruktor gibt es einige Punkte im Zusammenhang mit `MainPage::current = *this;`, die eine Erläuterung verdienen.
- Wenn Sie den `this`-Zeiger innerhalb eines Members des Implementierungstyps verwenden, ist der `this`-Zeiger natürlich *ein Zeiger auf den Implementierungstyp*.
- Um den `this`-Zeiger in den entsprechenden projizierten Typ zu konvertieren, dereferenzieren Sie ihn. Wenn Sie den Implementierungstyp aus IDL generieren (wie wir es hier gemacht haben), verfügt der Implementierungstyp über einen Konvertierungsoperator, der in seinen projizierten Typ konvertiert. Aus diesem Grund funktioniert die Zuweisung hier.

Weitere Informationen zu diesen Details finden Sie unter [Instanziierung und Rückgabe von Implementierungstypen und Schnittstellen](./author-apis.md#instantiating-and-returning-implementation-types-and-interfaces).

Der Konstruktor enthält außerdem `SampleTitle().Text(FEATURE_NAME());`. Der `SampleTitle()`-Teil ist der Aufruf einer einfache Accessorfunktion namens **SampleTitle**, die den **TextBlock** zurückgibt, den wir dem XAML hinzugefügt haben. Immer, wenn Sie ein XAML-Element mit `x:Name` versehen, generiert der XAML-Compiler einen Accessor für Sie, der für das-Element benannt ist. Der `.Text(...)`-Teil ruft die **Text**-Mutatorfunktion für das **TextBlock**-Objekt auf, das der **SampleTitle**-Accessor zurückgegeben hat. Und `FEATURE_NAME()` ruft unsere statische **MainPage::FEATURE_NAME**-Accessorfunktion auf, um das Zeichenfolgenliteral zurückzugeben. Insgesamt legt diese Codezeile die **Text**-Eigenschaft des **TextBlock-** namens *SampleTitle* fest.

Beachten Sie, dass wir, da Zeichenfolgen in der Windows-Runtime „wide“ sind, zum Portieren eines Zeichenfolgenliterals diesem das Codierungspräfix `L` vom Typ „wide-char“ voranstellen. So ändern wir beispielsweise „ein Zeichenfolgenliteral“ in L„ein Zeichenfolgenliteral“. Siehe auch [„Wide“ Zeichenfolgenliterale](/cpp/cpp/string-and-character-literals-cpp#wide-string-literals).

#### <a name="scenarios"></a>**Szenarios**

Hier finden Sie den relevanten C#-Code, den wir portieren müssen.

```csharp
// MainPage.xaml.cs
...
public sealed partial class MainPage : Page
{
...
    public List<Scenario> Scenarios
    {
        get { return this.scenarios; }
    }
...
}
...

// SampleConfiguration.cs
...
public partial class MainPage : Page
{
...
    List<Scenario> scenarios = new List<Scenario>
    {
        new Scenario() { Title = "Copy and paste text", ClassType = typeof(CopyText) },
        new Scenario() { Title = "Copy and paste an image", ClassType = typeof(CopyImage) },
        new Scenario() { Title = "Copy and paste files", ClassType = typeof(CopyFiles) },
        new Scenario() { Title = "Other Clipboard operations", ClassType = typeof(OtherScenarios) }
    };
...
}
...
```

Aus unserer vorherigen Untersuchung wissen wir, dass diese Sammlung von **Scenario**-Objekten in einer **ListBox-** angezeigt wird. In C++/WinRT gibt es Grenzwerte für die *Art von Sammlung*, die wir der **ItemsSource**-Eigenschaft eines Items-Steuerelements zuweisen können. Die Sammlung muss entweder ein Vektor oder ein beobachtbarer Vektor sein, und seine Elemente müssen von einem der folgenden Typen sein.

- Entweder Laufzeitklassen oder
- [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable).

Im Fall von **IInspectable** müssen diese Elemente, wenn sie nicht selbst Laufzeitklassen sind, von einer Art sein, die von [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) mittels Boxing und Unboxing behandelt werden kann. Und dies bedeutet, dass sie Windows-Runtime-Typen sein müssen (siehe [Boxing und Unboxing von skalaren Werten für IInspectable](./boxing.md)).

Für diese Fallstudie haben wir **Scenario** nicht zu einer Laufzeitklasse gemacht. Allerdings ist dies immer noch eine sinnvolle Option. Und es wird auch Fälle bei Ihrer eigenen Portierung geben, bei denen eine Laufzeitklasse definitiv das Richtige ist. Beispielsweise, wenn Sie den Elementtyp *beobachtbar* machen müssen (siehe [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](./binding-property.md)), oder wenn das Element aus anderen Gründen über Methoden verfügen muss, und es sich um mehr als nur einen Satz von Datenmembern handelt.

Da wir in dieser exemplarischen Vorgehensweise *keine* Laufzeitklasse für den **Scenario**-Typ verwenden, müssen wir uns mit Boxing beschäftigen. Wenn wir aus **Scenario** eine reguläre C++-`struct` gemacht hätten, wäre ein Boxing nicht möglich. Da wir aber **Scenario** als `struct` in IDL deklariert haben, *können* wir ein Boxing durchführen.

Wir haben die Auswahl, das Boxing von **Scenario** im Voraus auszuführen, oder zu warten, bis wir im Begriff stehen, die Zuweisung zu **ItemsSource** vorzunehmen, um dann auf Just-in-Time-Basis das Boxing durchzuführen. Im Folgenden finden Sie einige Überlegungen zu diesen beiden Optionen.

- Boxing im Voraus. Bei dieser Option ist unser Datenmember eine Sammlung von **IInspectable**, die für die Zuweisung zur Benutzeroberfläche bereit ist. Bei der Initialisierung erfolgt das Boxing der **Scenario**-Objekten in diesen Datenmember. Wir benötigen nur eine Kopie dieser Sammlung, aber wir müssen jedes Mal das Unboxing eines Elements vornehmen, wenn wir seine Felder lesen müssen.
- Just-In-Time-Boxing. Bei dieser Option ist unser Datenmember eine Sammlung von **Scenario**. Wenn der Zeitpunkt für die Zuweisung zur Benutzeroberfläche kommt, erfolgt das Boxing der **Scenario**-Objekte aus dem Datenmember in eine neue Sammlung von **IInspectable**. Wir können die Felder der Elemente ohne Unboxing in die Datenmember lesen, aber wir benötigen zwei Kopien der Sammlung.

Wie Sie sehen können, sind die Vor- und Nachteile bei einer kleinen Sammlung wie dieser recht ausgeglichen. Somit verwenden wir für diese Fallstudie die Just-in-Time-Option.

Der **Scenarios**-Member ist ein Feld von **MainPage**, das in `SampleConfiguration.cs` definiert und initialisiert wird. Außerdem ist **Scenarios** eine schreibgeschützte Eigenschaft von **MainPage**, die in `MainPage.xaml.cs` definiert ist (und implementiert wird, um lediglich das Feld **scenarios** zurückzugeben). Etwas ähnliches machen wir im C++/WinRT-Projekt, doch machen wir die beiden Elemente statisch (da wir nur eine Instanz in der gesamten Anwendung benötigen, und damit wir darauf ohne eine Klasseninstanz zugreifen können). Ferner nennen wir sie *scenariosInner* bzw. *scenarios*. Wir deklarieren *scenariosInner* in `MainPage.h`. Und da es über eine statische Speicherdauer verfügt, definieren/initialisieren wird es in einer `.cpp`-Datei (in diesem Fall `SampleConfiguration.cpp`).

Bearbeiten Sie `MainPage.h` und `SampleConfiguration.cpp` so, dass sie den nachstehenden Listings entsprechen.

```cppwinrt
// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    static Windows::Foundation::Collections::IVector<Scenario> scenarios() { return scenariosInner; }
...
private:
    static winrt::Windows::Foundation::Collections::IVector<Scenario> scenariosInner;
...
};

// SampleConfiguration.cpp
...
using namespace Windows::Foundation::Collections;
...
IVector<Scenario> implementation::MainPage::scenariosInner = winrt::single_threaded_observable_vector<Scenario>(
{
    Scenario{ L"Copy and paste text", xaml_typename<SDKTemplate::CopyText>() },
    Scenario{ L"Copy and paste an image", xaml_typename<SDKTemplate::CopyImage>() },
    Scenario{ L"Copy and paste files", xaml_typename<SDKTemplate::CopyFiles>() },
    Scenario{ L"History and roaming", xaml_typename<SDKTemplate::HistoryAndRoaming>() },
    Scenario{ L"Other Clipboard operations", xaml_typename<SDKTemplate::OtherScenarios>() },
});
```

Stellen Sie außerdem sicher, dass Sie den vorhandenen Funktionskörper aus `MainPage.cpp` für **MainPage::scenarios()** löschen, da wir diese Methoden nun in der Headerdatei definieren.

Wie Sie sehen können, initialisieren wir den statischen Datenmember *scenariosInner* in `SampleConfiguration.cpp` durch Aufrufen einer C++/WinRT-Hilfsfunktion namens [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector). Diese Funktion erstellt ein neues Windows-Runtime-Sammlungsobjekt für uns und gibt es als [**IObservableVector**](/uwp/api/windows.foundation.collections.iobservablevector_t_)-Schnittstelle zurück. Da in diesem Beispiel die Sammlung nicht *beobachtbar* ist (dies ist nicht erforderlich, da sie nach der Initialisierung Elemente weder hinzufügt noch entfernt), hätten wir uns stattdessen auch für [winrt::single_threaded_vector](/uwp/cpp-ref-for-winrt/single-threaded-vector) entscheiden können. Diese Funktion gibt die Sammlung als [**IVector**](/uwp/api/windows.foundation.collections.ivector_t_)-Schnittstelle zurück.

Weitere Informationen zu Sammlungen sowie zum Binden an diese finden Sie unter [XAML-Elementsteuerelemente: Binden an eine C++/WinRT-Sammlung](./binding-collection.md) und [Sammlungen mit C++/WinRT](./collections.md).

Der Initialisierungscode, den Sie gerade hinzugefügt haben, verweist auf Typen, die noch nicht im Projekt enthalten sind (z. B. **winrt::SDKTemplate::CopyText**. Um dies zu beheben, fügen wir dem Projekt fünf neue leere XAML-Seiten hinzu.

#### <a name="add-five-new-blank-xaml-pages"></a>Hinzufügen von fünf neuen leeren XAML-Seiten

Fügen Sie dem Projekt ein neues **XAML** > **Leere Seite (C++/WinRT)** -Element hinzu (stellen Sie sicher, dass es sich um die Elementvorlage **Leere Seite (C++/WinRT)** handelt, und nicht um die Elementvorlage **Leere Seite**). Nennen Sie es `CopyText`. Die neue XAML-Seite wird im **SDKTemplate**-Namespace definiert, was wir so auch wollen.

Wiederholen Sie den obigen Prozess viermal, und nennen Sie die XAML-Seiten `CopyImage`, `CopyFiles`, `HistoryAndRoaming` und `OtherScenarios`.

Wenn Sie möchten, können Sie nun eine erneute Erstellung durchführen.

#### <a name="notifyuser"></a>**NotifyUser**

Im C#-Projekt finden Sie die Implementierung der **MainPage.NotifyUser**-Methode in `MainPage.xaml.cs`. **MainPage.NotifyUser** weist eine Abhängigkeit von **MainPage.UpdateStatus** auf, und diese Methode besitzt wiederum Abhängigkeiten von XAML-Elementen, die wir noch nicht portiert haben. Deshalb wird vorerst nur eine **UpdateStatus**-Methode als Stub im C++/WinRT-Projekt erstellt, und wir portieren diese später.

Hier finden Sie den relevanten C#-Code, den wir portieren müssen.

```csharp
// MainPage.xaml.cs
...
public void NotifyUser(string strMessage, NotifyType type)
if (Dispatcher.HasThreadAccess)
{
    UpdateStatus(strMessage, type);
}
else
{
    var task = Dispatcher.RunAsync(CoreDispatcherPriority.Normal, () => UpdateStatus(strMessage, type));
}
private void UpdateStatus(string strMessage, NotifyType type) { ... }{
...
```

**NotifyUser** verwendet die [**Windows.UI.Core.CoreDispatcherPriority**](/uwp/api/windows.ui.core.coredispatcherpriority)-Enumeration. Immer wenn Sie in C++/WinRT einen Typ aus einem Windows-Namespace verwenden möchten, müssen Sie die entsprechende C++/WinRT-Windows-Namespace-Headerdatei einschließen (weitere Informationen hierzu finden Sie unter [Erste Schritte mit C++/WinRT](./get-started.md)). In diesem Fall, wie Sie im folgenden Codelisting sehen können, ist der Header `winrt/Windows.UI.Core.h`, und wir schließen ihn in `pch.h` ein.

**UpdateStatus** ist privat. Somit machen wir daraus eine private Methode in unserem **MainPage**-Implementierungstyp. **UpdateStatus** ist nicht dafür gedacht, in der Laufzeitklasse aufgerufen zu werden, weshalb wir es nicht in IDL deklarieren.

Nachdem **MainPage.NotifyUser** portiert und von **MainPage.UpdateStatus** ein Stub erstellt wurde, haben wir Folgendes im C++/WinRT-Projekt. Im Anschluss an dieses Codelisting werden wir einige der Details untersuchen.

```cppwinrt
// pch.h
...
#include <winrt/Windows.UI.Core.h>
...

// MainPage.h
...
struct MainPage : MainPageT<MainPage>
{
...
    void NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
private:
    void UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type);
...
};

// MainPage.cpp
...
void MainPage::NotifyUser(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    if (Dispatcher().HasThreadAccess())
    {
        UpdateStatus(strMessage, type);
    }
    else
    {
        Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [strMessage, type, this]()
            {
                UpdateStatus(strMessage, type);
            });
    }
}
void MainPage::UpdateStatus(hstring const& strMessage, SDKTemplate::NotifyType const& type)
{
    throw hresult_not_implemented();
}
...
```

In C# kannst du mithilfe der *Punktnotation („dot“)* mit geschachtelten Eigenschaften arbeiten. Somit kann der C#-Typ **MainPage** mit der Syntax `Dispatcher` auf seine eigene **Dispatcher**-Eigenschaft zugreifen. Außerdem kann C# diesen Wert mithilfe der *Punktnotation* und einer Syntax wie `Dispatcher.HasThreadAccess` noch erweitern. In C++/WinRT werden Eigenschaften als Accessorfunktionen implementiert, sodass sich die Syntax nur dadurch unterscheidet, dass Sie bei jedem Funktionsaufruf Klammern hinzufügen.

|C#|C++/WinRT|
|-|-|
|`Dispatcher.HasThreadAccess`|`Dispatcher().HasThreadAccess()`|

Wenn die C#-Version von **NotifyUser** [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) aufruft, implementiert sie den asynchronen Rückrufdelegaten als Lambda-Funktion. Die C++/WinRT-Version führt dasselbe aus, aber die Syntax unterscheidet sich ein wenig. In C++/WinRT *erfassen* wir die beiden Parameter, die wir verwenden werden, sowie den `this`-Zeiger (da wir eine Memberfunktion aufrufen werden). Weitere Informationen zum Implementieren von Delegaten als Lambdas sowie Codebeispiele finden Sie im Thema [Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT](./handle-events.md). Außerdem können wir den `var task =`-Teil in diesem speziellen Fall außer Acht lassen. Wir warten nicht auf das zurückgegebene asynchrone Objekt, weshalb wir es nicht speichern müssen. 

### <a name="implement-the-remaining-mainpage-members"></a>Implementieren der verbleibenden **MainPage**-Member

Lassen Sie uns eine vollständige Liste der Member von **MainPage** erstellen (implementiert über `MainPage.xaml.cs` und `SampleConfiguration.cs`), damit wir sehen können, welche wir bisher portiert haben und welche noch portiert werden müssen.

|Member|Access|Status|
|-|-|-|
|**MainPage**-Konstruktor|`public`|Portiert|
|**Current**-Eigenschaft|`public`|Portiert|
|**FEATURE_NAME**-Eigenschaft|`public`|Portiert|
|**IsClipboardContentChangedEnabled**-Eigenschaft|`public`|Nicht begonnen|
|**Scenarios**-Eigenschaft|`public`|Portiert|
|**BuildClipboardFormatsOutputString**-Methode|`public`|Nicht begonnen|
|**DisplayToast**-Methode|`public`|Nicht begonnen|
|**EnableClipboardContentChangedNotifications**-Methode|`public`|Nicht begonnen|
|**NotifyUser**-Methode|`public`|Portiert|
|**OnNavigatedTo**-Methode|`protected`|Nicht begonnen|
|**isApplicationWindowActive**-Feld|`private`|Nicht begonnen|
|**needToPrintClipboardFormat**-Feld|`private`|Nicht begonnen|
|**scenarios**-Feld|`private`|Portiert|
|**Button_Click**-Methode|`private`|Nicht begonnen|
|**DisplayChangedFormats**-Methode|`private`|Nicht begonnen|
|**Footer_Click**-Methode|`private`|Nicht begonnen|
|**HandleClipboardChanged**-Methode|`private`|Nicht begonnen|
|**OnClipboardChanged**-Methode|`private`|Nicht begonnen|
|**OnWindowActivated**-Methode|`private`|Nicht begonnen|
|**ScenarioControl_SelectionChanged**-Methode|`private`|Nicht begonnen|
|**UpdateStatus**-Methode|`private`|Stub erstellt|

In den folgenden Unterabschnitten werden die bisher nicht portierten Member besprochen.

> [!NOTE]
> Von Zeit zu Zeit werden uns Verweise im Quellcode auf Benutzeroberflächenelemente im XAML-Markup begegnen (in `MainPage.xaml`). Wenn wir zu diesen Verweisen kommen, werden wir sie vorübergehend umgehen, indem wir dem XAML einfache Platzhalterelemente hinzufügen. Auf diese Weise lässt sich das Projekt nach jedem Unterabschnitt noch erstellen. Die Alternative besteht darin, die Verweise aufzulösen, indem der *gesamte* Inhalt von `MainPage.xaml` jetzt aus dem C#-Projekt in das C++/WinRT-Projekt kopiert wird. Doch wenn wir dies tun, dauert es sehr lange, bis wir mal wieder einen Boxenstopp einlegen können, um erneut zu erstellen (wodurch möglicherweise vorhandene Tippfehler oder andere Fehler, die wir im Verlauf gemacht haben, verschleiert werden).
>
> Nachdem wir das Portieren des imperativen Codes für die **MainPage**-Klasse abgeschlossen haben, kopieren wir *dann* den Inhalt der XAML-Datei und sind uns sicher, dass das Projekt sich weiterhin erstellen lässt.

#### <a name="isclipboardcontentchangedenabled"></a>**IsClipboardContentChangedEnabled**

Dies ist eine Get-Set-Eigenschaft von C#, die standardmäßig `false` ist. Sie ist ein Member von **MainPage** und wird in `SampleConfiguration.cs` definiert.

Für C++/WinRT benötigen wir eine Accessorfunktion, eine Mutatorfunktion und einen Unterstützungsdatenmember als Feld. Da **IsClipboardContentChangedEnabled** den Zustand eines der Szenarien in dem Beispiel darstellt, anstatt den Zustand von **MainPage** selbst, erstellen wir die neuen Member mit einem neuen Hilfstyp mit dem Namen **SampleState**. Und wir implementieren dies in unserer `SampleConfiguration.cpp`-Quellcodedatei und machen die Member `static` (da wir nur eine Instanz in der gesamten Anwendung benötigen, und damit wir darauf ohne eine Klasseninstanz zugreifen können).

Um unsere `SampleConfiguration.cpp` im C++/WinRT-Projekt zu begleiten, fügen Sie ein neues **Visual C++**  > **Code** > **Headerdatei** element (.h) mit dem Namen `SampleConfiguration.h` hinzu. Bearbeiten Sie `SampleConfiguration.h` und `SampleConfiguration.cpp` so, dass sie den nachstehenden Listings entsprechen.

```cppwinrt
// SampleConfiguration.h
#pragma once 
#include "pch.h"

namespace winrt::SDKTemplate
{
    struct SampleState
    {
        static bool IsClipboardContentChangedEnabled();
        static void IsClipboardContentChangedEnabled(bool checked);
    private:
        static bool isClipboardContentChangedEnabled;
    };
}

// SampleConfiguration.cpp
...
#include "SampleConfiguration.h"
...
bool SampleState::isClipboardContentChangedEnabled = false;
...
bool SampleState::IsClipboardContentChangedEnabled()
{
    return isClipboardContentChangedEnabled;
}
void SampleState::IsClipboardContentChangedEnabled(bool checked)
{
    if (isClipboardContentChangedEnabled != checked)
    {
        isClipboardContentChangedEnabled = checked;
    }
}
```

Wiederum muss ein Feld mit `static` Speicher (z. B. **SampleState::isClipboardContentChangedEnabled**) einmalig in der Anwendung definiert werden, wobei eine `.cpp`-Datei ist ein guter Ort hierfür ist (in diesem Fall `SampleConfiguration.cpp`).

#### <a name="buildclipboardformatsoutputstring"></a>**BuildClipboardFormatsOutputString**

Diese Methode ist ein öffentlicher Member von **MainPage** und wird in `SampleConfiguration.cs` definiert.

```csharp
// SampleConfiguration.cs
...
public string BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent = Windows.ApplicationModel.DataTransfer.Clipboard.GetContent();
    StringBuilder output = new StringBuilder();

    if (clipboardContent != null && clipboardContent.AvailableFormats.Count > 0)
    {
        output.Append("Available formats in the clipboard:");
        foreach (var format in clipboardContent.AvailableFormats)
        {
            output.Append(Environment.NewLine + " * " + format);
        }
    }
    else
    {
        output.Append("The clipboard is empty");
    }
    return output.ToString();
}
...
```

In C++/WinRT machen wir **BuildClipboardFormatsOutputString** zu einer öffentlichen statischen Methode von **SampleState**. Wir können es `static` machen, da es auf keine Instanzmember zugreift.

Um die Typen **Clipboard** und **DataPackageView-** in C++/WinRT zu verwenden, müssen wir die C++/WinRT-Headerdatei `winrt/Windows.ApplicationModel.DataTransfer.h` des Windows-Namespace einschließen.

In C# ist die **DataPackageView.AvailableFormats**-Eigenschaft eine **IReadOnlyList**, sodass wir auf deren **Count**-Eigenschaft zugreifen können. In C++/WinRT gibt die **DataPackageView::AvailableFormats**-Accessorfunktion eine **IVectorView** zurück, die über eine **Size**-Accessorfunktion verfügt, die wir aufrufen können.

Zum Portieren der Verwendung des C#-Typs **System.Text.StringBuilder** verwenden wir den C++-Standardtyp [**std::wostringstream**](/cpp/standard-library/sstream-typedefs#wostringstream). Bei diesem Typ handelt es sich um einen Ausgabestream für „wide“ Zeichenfolgen (und um ihn zu verwenden, müssen wir die `sstream`-Headerdatei einschließen). Anstatt eine **Append**-Methode wie bei einem **StringBuilder** zu verwenden, verwenden Sie den [Einfügeoperator](/cpp/standard-library/using-insertion-operators-and-controlling-format) (`<<`) mit einem Ausgabestream wie **wostringstream**. Weitere Informationen finden Sie unter [iostream-Programmierung0](/cpp/standard-library/iostream-programming) und [Formatieren von C++/WinRT-Zeichenfolgen](./strings.md#formatting-strings).

Der C#-Code erstellt einen **StringBuilder-** mit dem `new`-Schlüsselwort. In C# sind Objekte standardmäßig Verweistypen, die im Heap mit `new` deklariert werden. In modernem Standards-C++ sind Objekte standardmäßig Werttypen, die im Stapel deklariert werden (ohne Verwendung von `new`). Somit portieren wir `StringBuilder output = new StringBuilder();` zu C++/WinRT einfach als `std::wostringstream output;`.

Mit dem C#-Schlüsselwort `var` wird der Compiler angewiesen, einen Typ abzuleiten. Du portierst `var` in `auto` in C++/WinRT. In C++/WinRT gibt es jedoch Fälle, in denen Sie (um Kopien zu vermeiden) einen *Verweis* auf einen abgeleiteten Typ benötigen und einen lvalue-Verweis auf den abgeleiteten Typ mit `auto&` formulieren. Es gibt auch Fälle, in denen du eine besondere Art von Verweis benötigst, der korrekt bindet, egal, ob er mit einem *lvalue*- oder *rvalue*-Ausdruck initialisiert wird. Dies formulierst du mit `auto&&`. Das ist die Form, die in der `for`-Schleife im folgenden portierten Code zu sehen ist. Weitere Informationen zu *lvalue*- und *rvalue*-Ausdrücken findest du unter [Wertekategorien und zugehörige Verweise](./cpp-value-categories.md).

Bearbeiten Sie `pch.h`, `SampleConfiguration.h` und `SampleConfiguration.cpp` so, dass sie den nachstehenden Listings entsprechen.

```cppwinrt
// pch.h
...
#include <sstream>
#include "winrt/Windows.ApplicationModel.DataTransfer.h"
...

// SampleConfiguration.h
...
static hstring BuildClipboardFormatsOutputString();
...

// SampleConfiguration.cpp
...
using namespace Windows::ApplicationModel::DataTransfer;
...
hstring SampleState::BuildClipboardFormatsOutputString()
{
    DataPackageView clipboardContent{ Clipboard::GetContent() };
    std::wostringstream output;

    if (clipboardContent && clipboardContent.AvailableFormats().Size() > 0)
    {
        output << L"Available formats in the clipboard:";
        for (auto&& format : clipboardContent.AvailableFormats())
        {
            output << std::endl << L" * " << std::wstring_view(format);
        }
    }
    else
    {
        output << L"The clipboard is empty";
    }

    return hstring{ output.str() };
}
```

> [!NOTE]
> In der Syntax in der Codezeile `DataPackageView clipboardContent{ Clipboard::GetContent() };` wird ein Feature des modernen Standard-C++ verwendet, das als *einheitliche Initialisierung* bezeichnet wird. Charakteristisch für diese Initialisierung ist die Verwendung von geschweiften Klammern anstelle eines `=`-Zeichens. Mit dieser Syntax wird verdeutlicht, dass keine Zuweisung, sondern eine Initialisierung erfolgt. Wenn du die Syntaxform bevorzugst, die wie eine Zuweisung *aussieht* (aber tatsächlich keine ist), kannst du die obige Syntax durch den gleichwertigen Ausdruck `DataPackageView clipboardContent = Clipboard::GetContent();` ersetzen. Es ist eine gute Idee, sich mit beiden Formulierungen für Initialisierungen vertraut zu machen, da du wahrscheinlich beide häufig in Code zu sehen bekommst, den du dir anschaust.

#### <a name="displaytoast"></a>**DisplayToast**

**DisplayToast** ist eine öffentliche statische Methode der C#-Klasse **MainPage**, und Sie ist in `SampleConfiguration.cs` definiert. In C++/WinRT machen wir daraus eine statische Methode von **SampleState**.

Wir haben die meisten Details und Verfahren bereits kennengelernt, die für das Portieren dieser Methode relevant sind. Ein bemerkenswertes neues Element ist, dass Sie ein ausführliches C#-Zeichenfolgenliteral (`@`) zu einem standardmäßigen [unformatierten Zeichenfolgenliteral](/cpp/cpp/string-and-character-literals-cpp#raw-string-literals-c11)von C# (`LR`) portieren.

Außerdem können Sie, wenn Sie auf die Typen [**ToastNotification**](/uwp/api/windows.ui.notifications.toastnotification) und [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) in C++/WinRT verweisen, diese entweder anhand des Namespacenamens qualifizieren, oder Sie können `SampleConfiguration.cpp` bearbeiten und `using namespace`-Direktiven wie im folgenden Beispiel hinzufügen.

```cppwinrt
using namespace Windows::UI::Notifications;
```

Sie haben dieselbe Wahlmöglichkeit, wenn Sie auf den Typ [**XmlDocument**](/uwp/api/windows.data.xml.dom.xmldocument) verweisen, und immer, wenn Sie auf einen beliebigen anderen Windows-Runtime-Typ verweisen.

Abgesehen von diesen Elementen befolgen Sie einfach dieselben Anweisungen wie zuvor, um die folgenden Schritte durchzuführen.

- Deklarieren Sie die Methode in `SampleConfiguration.h`, und definieren Sie sie in `SampleConfiguration.cpp`.
- Bearbeiten Sie `pch.h`, um alle erforderlichen C++/WinRT-Headerdateien des Windows-Namespace einzuschließen.
- Erstellen C++/WinRT-Objekte im Stapel, nicht im Heap.
- Ersetzen Sie Aufrufe von Get-Accessoren für Eigenschaften durch Funktionsaufrufsyntax (`()`).

Eine sehr gängige Ursache von Compiler-/Linkerfehlern ist das Vergessen, die C++/WinRT-Headerdateien des Windows-Namespace einzuschließen, die Sie benötigen. Weitere Informationen zu einem möglichen Fehler finden Sie unter [Warum erhalte ich den Fehler „LNK2019: Nicht aufgelöstes externes Symbol“ vom Linker?](./faq.md#why-is-the-linker-giving-me-a-lnk2019-unresolved-external-symbol-error).

Wenn Sie die exemplarische Vorgehensweise durcharbeiten und **DisplayToast** selbst portieren möchten, können Sie Ihre Ergebnisse mit dem Code in der C++/WinRT-Version in der von Ihnen heruntergeladenen ZIP-Datei des Quellcodes für das [Beispiel „Zwischenablage“](/samples/microsoft/windows-universal-samples/clipboard/) vergleichen.

#### <a name="enableclipboardcontentchangednotifications"></a>**EnableClipboardContentChangedNotifications**

**EnableClipboardContentChangedNotifications** ist eine öffentliche statische Methode der C#-Klasse **MainPage**, und Sie ist in `SampleConfiguration.cs` definiert.

```csharp
// SampleConfiguration.cs
...
public bool EnableClipboardContentChangedNotifications(bool enable)
{
    if (IsClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled = enable;
    if (enable)
    {
        Clipboard.ContentChanged += OnClipboardChanged;
        Window.Current.Activated += OnWindowActivated;
    }
    else
    {
        Clipboard.ContentChanged -= OnClipboardChanged;
        Window.Current.Activated -= OnWindowActivated;
    }
    return true;
}
...
private void OnClipboardChanged(object sender, object e) { ... }
private void OnWindowActivated(object sender, WindowActivatedEventArgs e) { ... }
...
```

In C++/WinRT machen wir daraus eine statische Methode von **SampleState**.

In C# verwenden Sie die `+=`- und `-=`-Operatorsyntax zum Registrieren und Wiederrufen von Ereignisbehandlungsdelegaten. In C++/WinRT haben Sie mehrere syntaktische Optionen, um einen Delegaten zu registrieren/widerrufen, wie in [Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT](./handle-events.md) beschrieben. Die allgemeine Form besteht jedoch darin, dass die Registrierung bzw. der Widerruf über Aufrufe eines Paars von Funktion erfolgen, die für das Ereignis benannt sind. Für die jeweilige Registrierung übergibst du deinen Delegaten an die registrierende Funktion, und im Gegenzug rufst du ein Widerrufstoken ab (eine [**winrt::event_token**](/uwp/cpp-ref-for-winrt/event-token)-Struktur). Für das Widerrufen übergibst du dieses Token an die Widerrufsfunktion (Sperrfunktion). In diesem Fall ist der Handler statisch, und (wie Sie im folgenden Codelisting sehen können) die Funktionsaufrufsyntax ist unkompliziert.

Ähnliche Token werden in C# *tatsächlich* im Hintergrund verwendet. Aber die Sprache macht dieses Detail implizit. C++/WinRT macht es explizit.

Der **object**-Typ kommt in den C#-Ereignishandlersignaturen vor. In der C#-Sprache ist **object** ein [Alias](/dotnet/csharp/language-reference/builtin-types/reference-types) für den .NET-Typ [**System.Object**](/dotnet/api/system.object). Die Entsprechung in C++/WinRT ist [**winrt::Windows::Foundation::IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable). Somit wird in den C++/WinRT-Ereignishandlern **IInspectable** angezeigt.

Bearbeiten Sie `SampleConfiguration.h` und `SampleConfiguration.cpp` so, dass sie den nachstehenden Listings entsprechen.

```cppwinrt
// SampleConfiguration.h
...
private:
    static event_token clipboardContentChangedToken;
    static event_token activatedToken;
    static void OnClipboardChanged(Windows::Foundation::IInspectable const& sender, Windows::Foundation::IInspectable const& e);
    static void OnWindowActivated(Windows::Foundation::IInspectable const& sender, Windows::UI::Core::WindowActivatedEventArgs const& e);
...

// SampleConfiguration.cpp
...
using namespace Windows::Foundation;
using namespace Windows::UI::Core;
using namespace Windows::UI::Xaml;
...
event_token SampleState::clipboardContentChangedToken;
event_token SampleState::activatedToken;
...
bool SampleState::EnableClipboardContentChangedNotifications(bool enable)
{
    if (isClipboardContentChangedEnabled == enable)
    {
        return false;
    }

    IsClipboardContentChangedEnabled(enable);
    if (enable)
    {
        clipboardContentChangedToken = Clipboard::ContentChanged(OnClipboardChanged);
        activatedToken = Window::Current().Activated(OnWindowActivated);
    }
    else
    {
        Clipboard::ContentChanged(clipboardContentChangedToken);
        Window::Current().Activated(activatedToken);
    }
    return true;
}
void SampleState::OnClipboardChanged(IInspectable const&, IInspectable const&){}
void SampleState::OnWindowActivated(IInspectable const&, WindowActivatedEventArgs const& e){}
```

Lassen Sie die Ereignisbehandlungsdelegaten selbst (**OnClipboardChanged** und **OnWindowActivated**) vorerst als Stubs. Sie befinden sich bereits auf unserer Liste zu portierender Member, weshalb wir sie in späteren Unterabschnitten behandeln werden.

#### <a name="onnavigatedto"></a>**OnNavigatedTo**

**OnNavigatedTo** ist eine geschützte Methode der C#-Klasse **MainPage** und ist in `MainPage.xaml.cs` definiert. Hier sehen Sie sie zusammen mit der XAML-**ListBox**, auf die sie verweist.

```xaml
<!-- MainPage.xaml -->
...
<ListBox x:Name="ScenarioControl" ... />
...
```

```csharp
// MainPage.xaml.cs
protected override void OnNavigatedTo(NavigationEventArgs e)
{
    // Populate the scenario list from the SampleConfiguration.cs file
    var itemCollection = new List<Scenario>();
    int i = 1;
    foreach (Scenario s in scenarios)
    {
        itemCollection.Add(new Scenario { Title = $"{i++}) {s.Title}", ClassType = s.ClassType });
    }
    ScenarioControl.ItemsSource = itemCollection;

    if (Window.Current.Bounds.Width < 640)
    {
        ScenarioControl.SelectedIndex = -1;
    }
    else
    {
        ScenarioControl.SelectedIndex = 0;
    }
}
```

Es handelt sich um eine wichtige und interessante Methode, da genau hier unsere Sammlung von **Scenario**-Objekten der Benutzeroberfläche zugewiesen wird. Der C#-Code erstellt eine [**System.Collections.Generic.List**](/dotnet/api/system.collections.generic.list-1) von **Scenario**-Objekten und weist diese der [**ItemsSource**](/uwp/api/windows.ui.xaml.controls.itemscontrol.itemssource)-Eigenschaft einer **ListBox** (wobei es sich um ein Items-Steuerelement handelt) zu. Außerdem verwenden wir in C# [Zeichenfolgeninterpolation](/dotnet/csharp/language-reference/tokens/interpolated), um den Titel für jedes **Scenario**-Objekt zu erstellen (beachten Sie die Verwendung des Sonderzeichens `$`).

In C++/WinRT machen wir aus **OnNavigatedTo** eine statische Methode von **MainPage**. Und wir fügen dem XAML ein **ListBox**-Stubelement hinzu, damit eine Erstellung erfolgreich verläuft. Im Anschluss an das Codelisting werden wir einige der Details untersuchen.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <ListBox x:Name="ScenarioControl" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
void OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e);
...

// MainPage.cpp
...
using namespace winrt::Windows::UI::Xaml;
using namespace winrt::Windows::UI::Xaml::Navigation;
...
void MainPage::OnNavigatedTo(NavigationEventArgs const& /* e */)
{
    auto itemCollection = winrt::single_threaded_observable_vector<IInspectable>();
    int i = 1;
    for (auto s : MainPage::scenarios())
    {
        s.Title = winrt::to_hstring(i++) + L") " + s.Title;
        itemCollection.Append(winrt::box_value(s));
    }
    ScenarioControl().ItemsSource(itemCollection);

    if (Window::Current().Bounds().Width < 640)
    {
        ScenarioControl().SelectedIndex(-1);
    }
    else
    {
        ScenarioControl().SelectedIndex(0);
    }
}
...
```

Auch hier rufen wir die [winrt::single_threaded_observable_vector](/uwp/cpp-ref-for-winrt/single-threaded-observable-vector)-Funktion auf, aber diesmal um eine Sammlung von [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) zu erstellen. Dies war Teil der Entscheidung, die wir getroffen haben, um das Boxing unserer **Scenario**-Objekte auf einer Just-in-Time-Basis durchzuführen.

Und anstelle von C#s Verwendung von [Zeichenfolgeninterpolation](/dotnet/csharp/language-reference/tokens/interpolated) wird hier eine Kombination der [**to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring)-Funktion und des [Verkettungsoperators](/uwp/cpp-ref-for-winrt/hstring#operator-concatenation-operator) von **winrt::hstring** verwendet.

#### <a name="isapplicationwindowactive"></a>**isApplicationWindowActive**

In C# ist **isApplicationWindowActive** ein einfaches privates `bool`-Feld, das zur **MainPage**-Klasse gehört und in `SampleConfiguration.cs` definiert ist. Es ist standardmäßig `false`. In C++/WinRT machen wir daraus ein öffentliches statisches Feld von **SampleState** (aus den bereits beschriebenen Gründen) in den Dateien `SampleConfiguration.h` und `SampleConfiguration.cpp` mit demselben Standardwert.

Wir haben bereits gesehen, wie ein statisches Feld deklariert, definiert und initialisiert wird. Sehen Sie sich zur Auffrischung noch einmal an, was wir mit dem Feld **isClipboardContentChangedEnabled** gemacht haben, und führen Sie dasselbe mit **isApplicationWindowActive** aus.

#### <a name="needtoprintclipboardformat"></a>**needToPrintClipboardFormat**

Dasselbe Muster wie bei **isApplicationWindowActive** (siehe die Überschrift unmittelbar vor dieser).

#### <a name="button_click"></a>**Button_Click**

**Button_Click** ist eine private Methode (für die Ereignisbehandlung) der C#-Klasse **MainPage**, die in `MainPage.xaml.cs` definiert ist. Hier sehen Sie das Element, zusammen mit dem **SplitView** _Element in XAML, auf das verwiesen wird, und dem **ToggleButton**-Element, das es registriert.

```xaml
<!-- MainPage.xaml -->
...
<SplitView x:Name="Splitter" ... />
...
<ToggleButton Click="Button_Click" .../>
...
```

```csharp
private void Button_Click(object sender, RoutedEventArgs e)
{
    Splitter.IsPaneOpen = !Splitter.IsPaneOpen;
}
```

Und dies ist die Entsprechung, portiert zu C++/WinRT. Beachten Sie, dass der Ereignishandler in der C++/WinRT-Version `public` ist (wie Sie sehen können, deklarieren Sie ihn *vor* den `private:`-Deklarationen). Das liegt daran, dass ein in XAML-Markup registrierter Ereignishandler, wie dieser hier, in C++/WinRT `public` sein muss, damit das XAML-Markup darauf zugreifen kann. Wenn Sie einen Ereignishandler in imperativem Code registrieren (wie vorhin in **MainPage::EnableClipboardContentChangedNotifications**), dann muss der Ereignishandler nicht `public` sein.

```xaml
<!-- MainPage.xaml -->
...
<StackPanel ...>
    ...
    <SplitView x:Name="Splitter" />
</StackPanel>
...
```

```cppwinrt
// MainPage.h
...
    void Button_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
void MainPage::Button_Click(Windows::Foundation::IInspectable const& /* sender */, Windows::UI::Xaml::RoutedEventArgs const& /* e */)
{
    Splitter().IsPaneOpen(!Splitter().IsPaneOpen());
}
```

#### <a name="displaychangedformats"></a>**DisplayChangedFormats**

In C# ist **DisplayChangedFormats** eine private Methode, die zur **MainPage**-Klasse gehört und in `SampleConfiguration.cs` definiert ist.

```csharp
private void DisplayChangedFormats()
{
    string output = "Clipboard content has changed!" + Environment.NewLine;
    output += BuildClipboardFormatsOutputString();
    NotifyUser(output, NotifyType.StatusMessage);
}
```

In C++/WinRT machen wir daraus ein privates statisches Feld von **SampleState** (es greift auf keine Instanzmember zu) in den Dateien `SampleConfiguration.h` und `SampleConfiguration.cpp`. Der C#-Code für diese Methode verwendet **System.Text.StringBuilder** nicht, aber er nimmt ausreichende Zeichenfolgenformatierungen vor, sodass dies für die C++/WinRT-Version eine weitere gute Stelle für die Verwendung von **std::wostringstream** ist.

Anstelle der statischen [**System.Environment.NewLine**](/dotnet/api/system.environment.newline)-Eigenschaft, die im C#-Code verwendet wird, fügen wir den C++-Standardcode `std::endl` (ein Zeilenumbruchzeichen) in den Ausgabestream ein.

```cppwinrt
// SampleConfiguration.h
...
private:
    static void DisplayChangedFormats();
...

// SampleConfiguration.cpp
void SampleState::DisplayChangedFormats()
{
    std::wostringstream output;
    output << L"Clipboard content has changed!" << std::endl;
    output << BuildClipboardFormatsOutputString().c_str();
    MainPage::Current().NotifyUser(output.str(), NotifyType::StatusMessage);
}
```

Der Entwurf der oben gezeigten C++/WinRT-Version ist geringfügig ineffizient. Zuerst erstellen wir eine **std::wostringstream**. Wir können aber auch die **BuildClipboardFormatsOutputString**-Methode aufrufen (die wir zuvor portiert haben). Diese Methode erstellt ihre eigene **std::wostringstream**. Außerdem wird ihr Stream in eine **winrt::hstring** umgewandelt und diese zurückgegeben. Wir rufen die [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function)-Funktion auf, um diese zurückgegebenen **hstring** wieder zurück in eine Zeichenfolge im C-Format umzuwandeln, und anschließend fügen wir diese dann in unseren Stream ein. Es wäre effizienter, nur einen **std::wostringstream** zu erstellen und diesen (bzw. einen Verweis darauf) zu übergeben, damit Methoden Zeichenfolgen direkt einfügen können.

So gehen wir in der C++/WinRT-Version des Quellcodes für das [Beispiel „Zwischenablage“](/samples/microsoft/windows-universal-samples/clipboard/) (in der von Ihnen heruntergeladenen ZIP-Datei) vor. In diesem Quellcode gibt es eine neue private statische Methode namens **samplestate::AddClipboardFormatsOutputString**, die einen Verweis auf einen Ausgabestream akzeptiert und bearbeitet. Und dann werden die Methoden **SampleState::DisplayChangedFormats** und **SampleState::BuildClipboardFormatsOutputString** neu einbezogen, um diese neue Methode aufzurufen. Dies entspricht funktional den Codelistings in diesem Thema, ist aber effizienter.

#### <a name="footer_click"></a>**Footer_Click**

**Footer_Click** ist ein asynchroner Ereignishandler, der zur C#-Klasse **MainPage** gehört und in `MainPage.xaml.cs` definiert ist. Das folgende Codebeispiel entspricht funktional der Methode, die in dem Quellcode enthalten ist, den Sie heruntergeladen haben. Aber hier haben wir sie von einer Zeile auf vier Zeilen verteilt, damit Sie leichter erkennen können, was Sie macht und somit auch, wie wir sie portieren müssen.

```csharp
async void Footer_Click(object sender, RoutedEventArgs e)
{
    var hyperlinkButton = (HyperlinkButton)sender;
    string tagUrl = hyperlinkButton.Tag.ToString();
    Uri uri = new Uri(tagUrl);
    await Windows.System.Launcher.LaunchUriAsync(uri);
}
```

Obwohl die Methode technisch gesehen asynchron ist, wird keine Aktion mehr nach dem `await` ausgeführt, sodass das `await` überflüssig ist (ebenfalls das Schlüsselwort `async`). Sie werden wahrscheinlich verwendet, um die IntelliSense-Meldung in Visual Studio zu vermeiden.

Die entsprechende C++/WinRT-Methode ist ebenfalls asynchron (da sie [**Launcher.LaunchUriAsync**](/uwp/api/windows.system.launcher.launchuriasync) aufruft). Aber sie muss weder `co_await` noch ein asynchrones Objekt zurückgeben. Informationen zu `co_await` und asynchronen Objekten finden Sie unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](./concurrency.md).

Sprechen wir nun über das, was die Methode macht. Da es sich hierbei um einen Ereignishandler für das **Click**-Ereignis eines **HyperlinkButton** handelt, ist das Objekt mit dem Namen *sender* tatsächlich ein **HyperlinkButton**. Die Typumwandlung ist also sicher (wir hätten diese Umwandlung sonst auch als `sender as HyperlinkButton` ausdrücken können). Als Nächstes rufen wir den Wert der **Tag**-Eigenschaft ab (wenn Sie sich das XAML-Markup im C#-Projekt ansehen, werden Sie feststellen, dass dies auf eine Zeichenfolge festgelegt ist, die eine Web-URL darstellt). Obwohl die **FrameworkElement.Tag**-Eigenschaft (**HyperlinkButton** ist ein **FrameworkElement**) vom Typ **object** ist , können wir dies in C# mit [**Object.ToString**](/dotnet/api/system.object.tostring) stringifizieren. Aus der resultierenden Zeichenfolge erstellen wir ein **Uri**-Objekt. Und schließlich (mithilfe der Shell) starten wir einen Browser und navigieren zu der URL.

Hier sehen Sie die zu C++/WinRT portierte Methode (wiederum zur besseren Übersichtlichkeit erweitert) und im Anschluss daran eine Beschreibung der Details.

```cppwinrt
// pch.h
...
#include "winrt/Windows.System.h"
...

// MainPage.h
...
    void Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const& e);
private:
...

// MainPage.cpp
...
using namespace winrt::Windows::Foundation;
using namespace winrt::Windows::UI::Xaml::Controls;
...
void MainPage::Footer_Click(Windows::Foundation::IInspectable const& sender, Windows::UI::Xaml::RoutedEventArgs const&)
{
    auto hyperlinkButton{ sender.as<HyperlinkButton>() };
    hstring tagUrl{ winrt::unbox_value<hstring>(hyperlinkButton.Tag()) };
    Uri uri{ tagUrl };
    Windows::System::Launcher::LaunchUriAsync(uri);
}
```

Wie immer machen wir den Ereignishandler `public`. Wir verwenden die [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)-Funktion mit dem *sender*-Objekt, um die Umwandlung in einen **HyperlinkButton-** vorzunehmen. In C++/WinRT ist die **Tag**-Eigenschaft ein [**IInspectable**](/windows/desktop/api/inspectable/nn-inspectable-iinspectable) (die Entsprechung eines [**Object**](/dotnet/api/system.object)). **IInspectable** verfügt jedoch über keinen **ToString**. Stattdessen müssen wir ein Unboxing von **IInspectable** in einen Skalarwert (in diesem Fall eine Zeichenfolge) vornehmen. Auch hier finden Sie weitere Informationen zum Boxing und Unboxing unter [Boxing und Unboxing von Skalarwerten für „IInspectable“](./boxing.md).

In den letzten beiden Zeilen werden Portierungsmuster wiederholt, die uns bereits begegnet sind, und sie spiegeln die C#-Version recht deutlich wider.

#### <a name="handleclipboardchanged"></a>**HandleClipboardChanged**

Am Portieren dieser Methode ist nichts Neues beteiligt. Sie können die C# -und C++/WinRT-Versionen in der von Ihnen heruntergeladenen ZIP-Datei des Quellcodes für das [Beispiel „Zwischenablage“](/samples/microsoft/windows-universal-samples/clipboard/) vergleichen.

#### <a name="onclipboardchanged-and-onwindowactivated"></a>**OnClipboardChanged** und **OnWindowActivated**

Bisher verfügen wir nur über leere Stubs für diese beiden Ereignishandler. Doch ihre Portierung ist unkompliziert, und es gibt keine neuen Aspekte zu diskutieren.

#### <a name="scenariocontrol_selectionchanged"></a>**ScenarioControl_SelectionChanged**

Dies ist ein weiterer privater Ereignishandler, der zur C#-Klasse **MainPage** gehört und in `MainPage.xaml.cs` definiert ist. In C++/WinRT machen wir ihn öffentlich und implementieren ihn in `MainPage.h` und `MainPage.cpp`.

Für diese Methode benötigen wir **MainPage::navigation**, dem es sich um ein privates boolesches Feld handelt, das auf `false` initialisiert ist. Außerdem benötigen Sie einen **Frame** namens *ScenarioFrame* in `MainPage.xaml`. Doch abgesehen von diesen Details enthüllt das Portieren dieser Methode keine neuen Methoden.

#### <a name="updatestatus"></a>**UpdateStatus**

Bisher haben wir nur einen Stub für **MainPage.UpdateStatus**. Mit dem Portieren seiner Implementierung befinden wir uns auch hier wieder auf größtenteils bekanntem Gelände. Ein neuer bemerkenswerter Punkt ist, dass, während wir in C# eine **Zeichenfolge** mit **String.Empty** vergleichen können, wir in C++/WinRT Sie stattdessen die [**winrt::hstring::empty**](/uwp/cpp-ref-for-winrt/hstring#hstringempty-function)-Funktion aufrufen. Ein weiterer Aspekt ist, dass `nullptr` die standardmäßige C++-Entsprechung von `null` in C# ist.

Den Rest der Portierung können Sie mit Methoden durchführen, die wir bereits behandelt haben. Im Folgenden finden Sie eine Liste der Dinge, die Sie erledigen müssen, bevor sich die portierte Version dieser Methode kompilieren lässt.

- Fügen Sie `MainPage.xaml` eine **Border** namens *StatusBorder* hinzu.
- Fügen Sie `MainPage.xaml` einen **TextBlock** namens *StatusBlock* hinzu.
- Fügen Sie `MainPage.xaml` ein **StackPanel** namens *StatusPanel* hinzu.
- Fügen Sie `pch.h` `#include "winrt/Windows.UI.Xaml.Media.h"` hinzu.
- Fügen Sie `pch.h` `#include "winrt/Windows.UI.Xaml.Automation.Peers.h"` hinzu.
- Fügen Sie `MainPage.cpp` `using namespace winrt::Windows::UI::Xaml::Media;` hinzu.
- Fügen Sie `MainPage.cpp` `using namespace winrt::Windows::UI::Xaml::Automation::Peers;` hinzu.

### <a name="copy-the-xaml-and-styles-necessary-to-finish-up-porting-mainpage"></a>Kopieren von XAML und Stilen, die notwendig sind, um das Portieren von **MainPage** abzuschließen

Bei XAML besteht der ideale Fall darin, dass Sie *dasselbe* XAML-Markup sowohl in einem C#- als auch in einem C++/WinRT-Projekt verwenden können. Und das „Zwischenablage“-Beispiel ist einer dieser Fälle.

Das Beispiel „Zwischenablage“ enthält in seiner Datei `Styles.xaml` ein XAML-**ResourceDictionary** mit Stilen, die auf die Schaltflächen, Menüs und anderen Benutzeroberflächenelemente in der Benutzeroberfläche der Anwendung angewendet werden. Die Seite `Styles.xaml` wird in `App.xaml` zusammengeführt. Und dann gibt es den Standardausgangspunkt `MainPage.xaml` für die Benutzeroberfläche, die wir bereits kurz gesehen haben. Wir können nun diese drei `.xaml`-Dateien unverändert in der C++/WinRT-Version des Projekts wiederverwenden.

Wie bei Objektdateien können Sie sich entschließen, aus mehreren Versionen Ihrer Anwendung auf dieselben freigegebenen XAML-Dateien zu verweisen. In dieser exemplarischen Vorgehensweise kopieren wir lediglich aus Gründen der Einfachheit Dateien in das C++/WinRT-Projekt und fügen sie auf diese Weise hinzu.

Navigieren Sie zum Ordner `\Clipboard_sample\SharedContent\xaml`, wählen Sie `App.xaml` und `MainPage.xaml`aus, kopieren Sie sie, und fügen Sie diese beiden Dateien in den Ordner `\Clipboard\Clipboard` Ihres C++/WinRT-Projekts ein, wobei Sie auswählen, dass Dateien ersetzt werden sollen, wenn dies abgefragt wird.

Fügen Sie dem C++/WinRT-Projekt direkt unter dem Projektknoten einen neuen Ordner hinzu, und nennen Sie diesen `Styles`. Navigieren Sie zum Ordner `\Clipboard_sample\SharedContent\xaml`, wählen Sie `Styles.xaml` aus, kopieren Sie sie, und fügen Sie sie in den Ordner `\Clipboard\Clipboard\Styles` in Ihrem C++/WinRT-Projekt ein. Klicken Sie mit der rechten Maustaste auf den Ordner `Styles` (im Projektmappen-Explorer im C++/WinRT-Projekt) > **Hinzufügen** > **Vorhandenes Element...** , und navigieren Sie zu `\Clipboard\Clipboard\Styles`. Wählen Sie in der Dateiauswahl `Styles` aus, und klicken Sie auf **Hinzufügen**.

Wir haben nun die Portierung von **MainPage** abgeschlossen, und wenn Sie entlang dieser Schritte gearbeitet haben, lässt sich Ihr C++/WinRT-Projekt nun erstellen und ausführen.

## <a name="consolidate-your-idl-files"></a>Konsolidieren deiner `.idl`-Dateien

Zusätzlich zum `MainPage.xaml`-Standardstartpunkt für die Benutzeroberfläche gibt es für das „Zwischenablage“-Beispiel fünf weitere szenariospezifische XAML-Seiten zusammen mit deren zugehörigen CodeBehind-Dateien. Das tatsächliche XAML-Markup aller dieser Seiten wird, unverändert, in der C++/WinRT-Version des Projekts verwendet. Und es wird in den nächsten Hauptabschnitten vorgestellt, wie die CodeBehind-Dateien portiert werden. Davor soll aber IDL besprochen werden.

Es ist sinnvoll, dass du deine Laufzeitklassen in einer einzigen IDL-Datei konsolidierst (weitere Informationen findest du unter [Einbeziehen von Laufzeitklassen in Midl-Dateien (.idl)](./author-apis.md#factoring-runtime-classes-into-midl-files-idl)). Im nächsten Schritt werden daher die Inhalte von `CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` und `OtherScenarios.idl` konsolidiert, indem sie in eine einzelne Datei namens `Project.idl` verschoben werden (und die ursprünglichen Dateien dann gelöscht werden).

Dabei sollen auch die automatisch generierte Dummyeigenschaft (`Int32 MyProperty;` und deren Implementierung) aus jedem dieser fünf XAML-Seitentypen entfernt werden.

Füge zunächst dem C++/WinRT-Projekt ein neues **Midl-Datei (.idl)** -Element hinzu. Nenne es `Project.idl`. Ersetze den gesamten Inhalt von `Project.idl` durch folgenden Code:

```idl
// Project.idl
namespace SDKTemplate
{
    [default_interface]
    runtimeclass CopyFiles : Windows.UI.Xaml.Controls.Page
    {
        CopyFiles();
    }

    [default_interface]
    runtimeclass CopyImage : Windows.UI.Xaml.Controls.Page
    {
        CopyImage();
    }

    [default_interface]
    runtimeclass CopyText : Windows.UI.Xaml.Controls.Page
    {
        CopyText();
    }

    [default_interface]
    runtimeclass HistoryAndRoaming : Windows.UI.Xaml.Controls.Page
    {
        HistoryAndRoaming();
    }

    [default_interface]
    runtimeclass OtherScenarios : Windows.UI.Xaml.Controls.Page
    {
        OtherScenarios();
    }
}
```

Wie du siehst, ist dies nur eine Kopie der Inhalte der einzelnen `.idl`-Dateien, die sich alle in einem Namespace befinden und für die `MyProperty` aus jeder Laufzeitklasse entfernt wurde.

Wähle im Projektmappen-Explorer in Visual Studio alle ursprünglichen IDL-Dateien (`CopyFiles.idl`, `CopyImage.idl`, `CopyText.idl`, `HistoryAndRoaming.idl` und `OtherScenarios.idl`) gleichzeitig aus, und entferne sie über **Bearbeiten** > **Entfernen** (wähle **Löschen** im Dialogfeld aus).

Lösche schließlich &mdash; um das Entfernen von `MyProperty` abzuschließen &mdash; in den `.h`- und `.cpp`-Dateien für jeden der fünf XAML-Seitentypen die Deklarationen und Definitionen der `int32_t MyProperty()`Accessor- und `void MyProperty(int32_t)`-Mutatorfunktionen.

Übrigens ist es immer ratsam, die Namen Ihrer XAML-Dateien mit dem Namen der Klassen, die sie darstellen, abzugleichen. Wenn Sie z. B. `x:Class="MyNamespace.MyPage"` in einer XAML-Markupdatei verwenden, sollten Sie diese Datei `MyPage.xaml` nennen. Dies ist zwar keine technische Voraussetzung, aber wenn Sie nicht mit verschiedenen Namen für ein und dasselbe Artefakt jonglieren müssen, wird Ihr Projekt verständlicher und wartbarer und die Arbeit damit einfacher.

## <a name="copyfiles"></a>**CopyFiles**

Im C#-Projekt ist der **CopyFiles**-XAML-Seitentyp in den Quellcodedateien `CopyFiles.xaml` und `CopyFiles.xaml.cs` implementiert. Schauen wir uns nacheinander jedes der Elemente von **CopyFiles** an.

### <a name="rootpage"></a>**rootPage**

Dies ist ein privates Feld.

```csharp
// CopyFiles.xaml.cs
...
public sealed partial class CopyFiles : Page
{
    MainPage rootPage = MainPage.Current;
    ...
}
...
```

In C++/WinRT kann es wie folgt definiert und initialisiert werden.

```cppwinrt
// CopyFiles.h
...
struct CopyFiles : CopyFilesT<CopyFiles>
{
    ...
private:
    SDKTemplate::MainPage rootPage{ MainPage::Current() };
};
...
```

Auch **CopyFiles::rootPage** ist (wie **MainPage::current**) mit dem Typ **SDKTemplate::MainPage** deklariert, der der projizierte Typ und nicht der Implementierungstyp ist.

### <a name="copyfiles-the-constructor"></a>**CopyFiles** (der Konstruktor)

Im C++/WinRT-Projekt hat der **CopyFiles**-Typ bereits einen Konstruktor, der den gewünschten Code enthält (in ihm wird nur **InitializeComponent** aufgerufen).

### <a name="copybutton_click"></a>**CopyButton_Click**

Die C#-Methode **CopyButton_Click** ist ein Ereignishandler, und am `async`-Schlüsselwort in deren Signatur ist zu erkennen, dass die Methode ihre Aufgaben asynchron erledigt. In C++/WinRT wird eine asynchrone Methode als *Coroutine* implementiert. Eine Einführung in Parallelität in C++/WinRT samt einer Beschreibung, was eine *Coroutine* ist, findest du unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](./concurrency.md).

Es ist üblich, für weitere Aufgaben nach Abschluss einer Coroutine zu planen. In solchen Fällen gibt die Coroutine einen asynchronen Objekttyp zurück, auf den gewartet werden kann und über den optional Fortschritt gemeldet werden kann. Diese Überlegungen gelten in der Regel jedoch nicht für einen Ereignishandler. Daher kannst du, wenn du einen Ereignishandler hast, der asynchrone Vorgänge ausführt, diesen Handler als Coroutine implementieren, die **winrt::fire_and_forget** zurückgibt. Weitere Informationen findest du unter [Fire and Forget (Auslösen und Vergessen)](./concurrency-2.md#fire-and-forget).

Obwohl die Idee einer „Fire-and-Forget“-Coroutine darin besteht, dass Sie sich nicht darum kümmern, wann sie abgeschlossen ist, wird die Arbeit im Hintergrund fortgesetzt (oder sie ist angehalten, und es wird auf ihre Wiederaufnahme gewartet). Anhand der C#-Implementierung kannst du erkennen, dass **CopyButton_Click** vom `this`-Zeiger abhängt (über ihn wird auf den Instanzdatenmember `rootPage` zugegriffen). Daher muss sichergestellt werden, dass der `this`-Zeiger (ein Zeiger auf ein **CopyFiles**-Objekt) die **CopyButton_Click**-Coroutine „überlebt“. In einer Situation, wie sie dieser Beispielanwendung entspricht, in der der Benutzer zwischen Benutzeroberflächenseiten navigiert, kann die Lebensdauer solcher Seiten nicht direkt gesteuert werden. Sollte die **CopyFiles**-Seite zerstört werden (indem von ihr weg navigiert wird), während **CopyButton_Click** noch in einem Hintergrundthread aktiv ist, wäre kein sicherer Zugriff auf `rootPage` möglich. Damit die Coroutine korrekt funktioniert, muss sie einen starken Verweis auf den `this`-Zeiger erhalten, und dieser Verweis muss für die Lebensdauer der Coroutine beibehalten werden. Weitere Informationen findest du unter [Starke und schwache Verweise in C++/WinRT](./weak-references.md).

Wenn du dir die C++/WinRT-Version des Beispiels anschaust, siehst du in **CopyFiles::CopyButton_Click**, dass dies mit einer einfachen Deklaration im Stapel umgesetzt wird.

```cppwinrt
fire_and_forget CopyFiles::CopyButton_Click(IInspectable const&, RoutedEventArgs const&)
{
    auto lifetime{ get_strong() };
    ...
}
```

Sehen wir uns die weiteren Aspekte des portierten Codes an, die bemerkenswert sind.

Im Code wird ein [**FileOpenPicker**](/uwp/api/windows.storage.pickers.fileopenpicker)-Objekt instanziiert, und zwei Zeilen später wird auf die [**FileTypeFilter**](/uwp/api/windows.storage.pickers.fileopenpicker.filetypefilter)-Eigenschaft dieses Objekts zugegriffen. Im Rückgabetyp dieser Eigenschaft ist ein **IVector**-Element aus Zeichenfolgen implementiert. Und für dieses **IVector**-Element wird die [IVector<T>.ReplaceAll(T[])](/uwp/api/windows.foundation.collections.ivector-1.replaceall)-Methode aufgerufen. Der interessierende Aspekt ist der Wert, der an diese Methode übergeben wird, wobei ein Array erwartet wird. So sieht die Codezeile aus.

```cppwinrt
filePicker.FileTypeFilter().ReplaceAll({ L"*" });
```

Der Wert, der übergeben wird (`{ L"*" }`), ist eine standardmäßige C++ -*Initialisierungsliste*. Die Liste enthält in diesem Fall ein einziges Objekt, aber eine Initialisierungsliste kann eine beliebige Anzahl von Objekten enthalten, die durch Kommas getrennt sind. Die Elemente von C++/WinRT, die die Möglichkeit bieten, eine Initialisierungsliste an eine Methode wie diese zu übergeben, sind in [Standard-Initialisierungslisten](./std-cpp-data-types.md#standard-initializer-lists) beschrieben.

Das C#-Schlüsselwort `await` wird in `co_await` in C++/WinRT portiert. Es folgt ein Beispiel aus dem Code.

```cppwinrt
auto storageItems{ co_await filePicker.PickMultipleFilesAsync() };
```

Sieh dir nun diese C#-Codezeile an.

```csharp
dataPackage.SetStorageItems(storageItems);
```

C# kann die **IReadOnlyList<StorageFile>** -Sammlung, die durch *storageItems* dargestellt wird, implizit in das **IEnumerable<IStorageItem>** -Objekt konvertieren, das von [**DataPackage.SetStorageItems**](/uwp/api/windows.applicationmodel.datatransfer.datapackage.setstorageitems) erwartet wird. In C++/WinRT ist es jedoch erforderlich, explizit aus **IVectorView<StorageFile>** in **IIterable<IStorageItem>** umzuwandeln. Daher gibt es ein weiteres Beispiel für die [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)-Funktion in Aktion.

```cppwinrt
dataPackage.SetStorageItems(storageItems.as<IVectorView<IStorageItem>>());
```

Wenn in C# das `null`-Schlüsselwort verwendet wird (z. B. `Clipboard.SetContentWithOptions(dataPackage, null)`), wird in C++/WinRT `nullptr` verwendet (z. B. `Clipboard::SetContentWithOptions(dataPackage, nullptr)`).

### <a name="pastebutton_click"></a>**PasteButton_Click**

Dies ist ein weiterer Ereignishandler in Form einer „Fire-and-Forget“-Coroutine. Sehen wir uns die Aspekte des portierten Codes an, die bemerkenswert sind.

In der C#-Version des Beispiels werden Ausnahmen mit `catch (Exception ex)` abgefangen. Im portierten C++/WinRT-Code ist der Ausdruck `catch (winrt::hresult_error const& ex)` zu sehen. Weitere Informationen über [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) und darüber, wie damit gearbeitet wird, findest du unter [Fehlerbehandlung bei C++/WinRT](./error-handling.md).

Ein Beispiel, wie getestet wird, ob ein C#-Objekt gleich `null` ist oder nicht, ist `if (storageItems != null)`. In C++/WinRT kann ein Konvertierungsoperator zu `bool` herangezogen werden, in dem der Test hinsichtlich `nullptr` intern ausgeführt wird.

Es folgt eine etwas vereinfachte Version eines Codefragments aus der portierten C++/WinRT-Version des Beispiels.

```cppwinrt
std::wostringstream output;
output << std::wstring_view(ApplicationData::Current().LocalFolder().Path());
```

Das Erstellen eines **std::wstring_view**-Objekts aus einer **winrt::hstring**-Zeichenfolge wie dieser veranschaulicht eine Alternative zum Aufrufen der [**hstring::c_str**](/uwp/cpp-ref-for-winrt/hstring#hstringc_str-function)-Funktion (um die **winrt::hstring**-Zeichenfolge in eine Zeichenfolge im C-Format umzuwandeln). Diese Alternative funktioniert, weil **hstring** einen [Konvertierungsoperator in **std::wstring_view**](/uwp/cpp-ref-for-winrt/hstring#hstringoperator-stdwstring_view) hat.

Sieh dir dieses C#-Fragment an.

```csharp
var file = storageItem as StorageFile;
if (file != null)
...
```

Um das C#-Schlüsselwort `as` in C++/WinRT zu portieren, wurde bereits mehrfach die [**as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function)-Funktion verwendet. Diese Funktion löst eine Ausnahme aus, wenn die Typumwandlung fehlschlägt. Soll für die Umwandlung aber `nullptr` zurückgegeben werden, wenn sie fehlschlägt (damit diese Bedingung im Code verarbeitet werden kann), wird stattdessen die [**try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function)-Funktion verwendet.

```cppwinrt
auto file{ storageItem.try_as<StorageFile>() };
if (file)
...
```

### <a name="copy-the-xaml-necessary-to-finish-up-porting-copyfiles"></a>Kopieren des XAML-Inhalts, der notwendig sind, um das Portieren von **CopyFiles** abzuschließen

Du kannst jetzt den gesamten Inhalt der `CopyFiles.xaml`-Datei aus dem C# Projekt auswählen und ihn in die `CopyFiles.xaml`-Datei im C++/WinRT-Projekt einfügen (wobei der vorhandene Inhalt dieser Datei im C++/WinRT-Projekt ersetzt wird).

Bearbeite schließlich `CopyFiles.h` und `.cpp`, und lösche die **ClickHandler**-Dummyfunktion, weil das entsprechende XAML-Markup soeben überschrieben wurde.

Das Portieren von **CopyFiles** ist nun abgeschlossen, und wenn du die zugehörigen Schritte ausgeführt hast, kann dein C++/WinRT-Projekt jetzt erstellt und ausgeführt werden, und das **CopyFiles**-Szenario ist funktionsfähig.

## <a name="copyimage"></a>**CopyImage**

Um den **CopyImage**-XAML-Seitentyp zu portieren, gehst du auf die gleiche Weise vor wie für **CopyFiles**. Beim Portieren von **CopyImage** wird die C#-Anweisung [*using*](/dotnet/csharp/language-reference/keywords/using-statement) verwendet, in der sichergestellt wird, dass Objekte, die die [**IDisposable**](/dotnet/api/system.idisposable)-Schnittstelle implementieren, ordnungsgemäß verworfen (gelöscht) werden.

```csharp
if (imageReceived != null)
{
    using (var imageStream = await imageReceived.OpenReadAsync())
    {
        ... // Pass imageStream to other APIs, and do other work.
    }
}
```

Die entsprechende Schnittstelle C++in/WinRT ist [**IClosable**](/uwp/api/windows.foundation.iclosable) mit ihrer einzigen **Close**-Methode. So sieht die C++/WinRT-Entsprechung des obigen C#-Codes aus.

```cppwinrt
if (imageReceived)
{
    auto imageStream{ co_await imageReceived.OpenReadAsync() };
    ... // Pass imageStream to other APIs, and do other work.
    imageStream.Close();
}
```

In C++/WinRT-Objekten wird **IClosable** in erster Linie zum Nutzen von Sprachen implementiert, die keine deterministische Finalisierung haben. C++/WinRT hat deterministische Finalisierung, sodass es häufig nicht erforderlich ist, **IClosable::Close** aufzurufen, wenn C++/WinRT-Code geschrieben wird. Es gibt jedoch Situationen, in denen es sinnvoll ist, die Methode aufzurufen, und dies ist eine dieser Situationen. Hier ist der *imageStream*-Bezeichner ein Wrapper mit Verweiszählung, der ein zugrunde liegendes Windows-Runtime-Objekt umschließt (in diesem Fall ein Objekt, in dem [**IRandomAccessStreamWithContentType**](/uwp/api/windows.storage.streams.irandomaccessstreamwithcontenttype) implementiert ist). Zwar lässt sich feststellen, dass der Finalizer von *imageStream* (dessen Destruktor) am Ende des einschließenden Bereichs (die geschweiften Klammern) ausgeführt wird, ist nicht sichergestellt, dass der Finalizer **Close** aufruft. Das liegt daran, dass *imageStream* an andere APIs weitergegeben wurde, und diese ändern möglicherweise weiterhin die Verweisanzahl des zugrunde liegenden Windows-Runtime-Objekts. Dies ist also ein Fall, in dem es sich empfiehlt, **Close** explizit aufzurufen. Weitere Informationen findest du unter [Muss ich IClosable::Close bei von mir verwendeten Laufzeitklassen aufrufen?](./faq.md#do-i-need-to-call-iclosableclose-on-runtime-classes-that-i-consume).

Sieh dir als nächstes den C#-Ausdruck `(uint)(imageDecoder.OrientedPixelWidth * 0.5)` an, den du im **OnDeferredImageRequestedHandler**-Ereignishandler findest. In diesem Ausdruck wird ein `uint`-Wert mit einem `double`-Wert multipliziert, was zu einem `double`-Wert führt. Dieser Wert wird dann in einen `uint`-Wert umgewandelt. In C++/WinRT *könnte* eine ähnlich aussehende Umwandlung im C-Stil verwendet werden (`(uint32_t)(imageDecoder.OrientedPixelWidth() * 0.5)`), aber es ist vorzuziehen, genau die Art von Umwandlung deutlich zu machen, die beabsichtigt ist. In diesem Fall würde dies mit `static_cast<uint32_t>(imageDecoder.OrientedPixelWidth() * 0.5)` vorgenommen.

Die C#-Version von **CopyImage.OnDeferredImageRequestedHandler** hat eine `finally`-Klausel, aber keine `catch`-Klausel. Nach einem Wechsel zu einer etwas späteren Stelle in der C++/WinRT-Version wurde eine `catch`-Klausel implementiert, sodass nun berichtet werden kann, ob das verzögerte Rendern erfolgreich war.

Ein Portieren des Rests dieser XAML-Seite bringt keine neuen Erkenntnisse, die zu erläutern sind. Vergessen Sie nicht, die **ClickHandler**-Dummyfunktion zu löschen. Und so wie bei **CopyFiles** besteht der letzte Schritt der Portierung darin, den gesamten Inhalt von `CopyImage.xaml` auszuwählen und in die gleiche Datei im C++/WinRT-Projekt einzufügen.

## <a name="copytext"></a>**CopyText**

Du kannst `CopyText.xaml` und `CopyText.xaml.cs` mit bereits behandelten Techniken portieren.

## <a name="historyandroaming"></a>**HistoryAndRoaming**

Es gibt einige interessante Punkte, die sich beim Portieren des **HistoryAndRoaming**-XAML-Seitentyps ergeben.

Sieh dir zunächst den C#-Quellcode an, und folge dem Ablauf der Steuerung von **OnNavigatedTo** über den **OnHistoryEnabledChanged**-Ereignishandler und schließlich zur asynchronen Funktion **CheckHistoryAndRoaming** (auf die nicht gewartet wird, sodass sie im Wesentlichen als „Fire and Forget“ (Auslösen und Vergessen) zu werten ist). Da **CheckHistoryAndRoaming** asynchron ist, muss in C++/WinRT unbedingt die Lebensdauer des `this`-Zeigers beachtet werden. Du kannst das Ergebnis sehen, wenn du dir die Implementierung in der Quellcodedatei `HistoryAndRoaming.cpp` ansiehst. Zunächst wird, wenn Delegaten zu **Clipboard::HistoryEnabledChanged** und **Clipboard::RoamingEnabledChanged** zugewiesen werden, nur ein schwacher Verweis auf das **HistoryAndRoaming**-Seitenobjekt übernommen. Dies erfolgt dadurch, dass der Delegat mit einer Abhängigkeit von dem Wert erstellt wird, der von [**winrt::get_weak**](/uwp/cpp-ref-for-winrt/implements#implementsget_weak-function)zurückgegeben wird, anstelle einer Abhängigkeit vom `this`-Zeiger. Dies bedeutet, dass der Delegat, der schließlich asynchronen Code aufruft, die **HistoryAndRoaming**-Seite nicht aktiv hält, sollte er beim Navigieren verlassen werden.

Und zweitens besteht, wenn schlussendlich die „Fire-and-Forget“-Coroutine **CheckHistoryAndRoaming** erreicht ist, der erste Schritt darin, einen starken Verweis auf `this` abzurufen, damit sichergestellt ist, dass die **HistoryAndRoaming**-Seite zumindest so lange aktiv ist, bis die Coroutine endgültig abgeschlossen ist. Weitere Informationen zu den beiden soeben beschriebenen Aspekten findest du unter [Starke und schwache Verweise in C++/WinRT](./weak-references.md).

Für das Portieren von **CheckHistoryAndRoaming** gibt es einen weiteren interessanten Punkt. Die Coroutine enthält Code zum Aktualisieren der Benutzeroberfläche. Daher muss sicher sein, dass dies im Hauptthread der Benutzeroberfläche erfolgt. Der Thread, der anfänglich einen Ereignishandler aufruft, ist der Hauptthread der Benutzeroberfläche. Aber üblicherweise kann eine asynchrone Methode in jedem beliebigen Thread ausgeführt und/oder fortgesetzt werden. In C# besteht die Lösung darin, [**CoreDispatcher.RunAsyncc**](/uwp/api/windows.ui.core.coredispatcher.runasync) aufzurufen und die Benutzeroberfläche in der Lambda-Funktion zu aktualisieren. In C++/WinRT kann die [**winrt::resume_foreground**](/uwp/cpp-ref-for-winrt/resume-foreground)-Funktion zusammen mit dem [**Verteiler**](/uwp/api/windows.ui.xaml.dependencyobject.dispatcher) (Dispatcher) des `this`-Zeigers verwendet werden, um die Coroutine anzuhalten und sofort im Hauptthread der Benutzeroberfläche fortzusetzen.

Der entsprechende Ausdruck ist `co_await winrt::resume_foreground(Dispatcher());`. Alternativ, jedoch weniger deutlich, könnte dies einfach als `co_await Dispatcher();` formuliert werden. Die kürzere Version lässt sich dank eines Konvertierungsoperators erreichen, der von C++/WinRT bereitgestellt wird.

Ein Portieren des Rests dieser XAML-Seite bringt keine neuen Erkenntnisse, die zu erläutern sind. Vergessen Sie nicht, die **ClickHandler**-Dummyfunktion zu löschen und das XAML-Markup zu kopieren.

## <a name="otherscenarios"></a>**OtherScenarios**

Du kannst `OtherScenarios.xaml` und `OtherScenarios.xaml.cs` mit bereits behandelten Techniken portieren.

## <a name="conclusion"></a>Schlussbemerkung

Hoffentlich hast du in dieser exemplarische Vorgehensweise genügend Portierungsinformationen und -techniken an die Hand bekommen, so dass du nun fortfahren und deine eigenen C#-Anwendungen nach C++/WinRT portieren kannst. Zur Auffrischung kannst du weiterhin auf die Vorher- (C#) und die Nachher-Version (C++/WinRT) des Quellcodes im Beispiel „Zwischenablage“ (Clipboard) zurückgreifen und diese nebeneinander vergleichen, um die Entsprechungen zu sehen.

## <a name="related-topics"></a>Zugehörige Themen
* [Umstellen von C# auf C++/WinRT](./move-to-winrt-from-csharp.md)