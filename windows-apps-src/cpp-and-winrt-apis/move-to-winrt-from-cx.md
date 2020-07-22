---
description: In diesem Thema werden die technischen Details der Portierung von Quellcode in einem [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)-Projekt zum entsprechenden Äquivalent in [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) beschrieben.
title: Umstellen von C++/CX auf C++/WinRT
ms.date: 01/17/2019
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, portieren, migrieren, C++/CX
ms.localizationpriority: medium
ms.openlocfilehash: 7282f85d5dcc093cbdabb1e03471ed533136fa7f
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86493575"
---
# <a name="move-to-cwinrt-from-ccx"></a>Umstellen von C++/CX auf C++/WinRT

In diesem Thema werden die technischen Details der Portierung von Quellcode in einem [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)-Projekt zum entsprechenden Äquivalent in [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) beschrieben.

## <a name="porting-strategies"></a>Strategien für das Portieren

Es ist möglich, dass du deinen C++/CX-Code nach und nach in C++/WinRT-Code portierst. C++/CX- und C++/WinRT-Code kann in demselben Projekt gleichzeitig vorhanden sein, mit Ausnahme von XAML-Compilerunterstützung und der Windows-Runtime-Komponenten. Für diese beiden Ausnahmen musst du dich in einem Projekt entweder für C++/CX oder C++/WinRT entscheiden. Dies bedeutet, dass alle deine XAML-Seitentypen entweder vollständig C++/CX oder vollständig C++/WinRT sein müssen. Die kannst weiterhin C++/CX und C++/WinRT außerhalb von XAML-Seitentypen innerhalb desselben Projekts mischen.

> [!IMPORTANT]
> Wenn in deinem Projekt eine XAML-Anwendung erstellt wird, besteht ein von uns empfohlener Workflow darin, zuerst mit einer der C++/WinRT-Projektvorlagen (siehe [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)) ein neues Projekt in Visual Studio zu erstellen. Beginne dann damit, den Quellcode und das Markup aus dem C++/CX-Projekt zu kopieren. Du kannst **Projekt** \> **Neues Element hinzufügen** verwenden, um neue XAML-Seiten hinzuzufügen. \> **Visual C++**  > **Leere Seite (C++/WinRT)** .
>
> Alternativ kannst du eine Windows-Runtime-Komponente nutzen, um beim Portieren Code aus dem XAML-C++/CX-Projekt auszuklammern. Verschiebe entweder so viel C++/CX-Code wie möglich in eine Komponente, und ändere anschließend das XAML-Projekt in C++/WinRT. Oder behalte für das XAML-Projekt C++/CX bei, erstelle eine neue C++/WinRT-Komponente, und beginne damit, C++/CX-Code aus dem XAML-Projekt in die Komponente zu portieren. Außerdem kannst du auch ein C++/CX-Komponentenprojekt neben einem C++/WinRT-Komponentenprojekt innerhalb derselben Lösung verwenden, auf beide aus deinem Anwendungsprojekt verweisen und nach und nach das Portieren von einem zum anderen durchführen. Weitere Informationen zur Verwendung der beiden Sprachprojektionen in demselben Projekt findest du unter [Interoperabilität zwischen C++/WinRT und C++/CX](interop-winrt-cx.md).

> [!NOTE]
> Sowohl [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx) als auch das Windows SDK deklarieren Typen im Stammnamespace **Windows**. Ein Windows-Typ, der in C++/WinRT projiziert wird, verfügt über den gleichen vollqualifizierten Namen wie der Windows-Typ, befindet sich aber im C++-**winrt**-Namespace. Diese unterschiedlichen Namespaces ermöglichen die Portierung von C++/CX zu C++/WinRT nach deinem eigenen Tempo.

## <a name="first-steps-in-porting-a-ccx-project-to-cwinrt"></a>Erste Schritte bei der Portierung eines C++/CX-Projekts zu C++/WinRT

Unter Berücksichtigung der oben erwähnten Ausnahmen ist der erste Schritt beim Portieren eines C++/CX-Projekts zu C++/WinRT das manuelle Hinzufügen von C++/WinRT-Unterstützung (siehe [Visual Studio-Unterstützung für C++/WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Installiere zu diesem Zweck das [Microsoft.Windows.CppWinRT NuGet-Paket](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) in deinem Projekt. Öffne das Projekt in Visual Studio, klicke auf **Projekt** \> **NuGet-Pakete verwalten**. \> **Durchsuchen**, gib oder füge **Microsoft.Windows.CppWinRT** in das Suchfeld ein, wähle das Element in den Suchergebnissen aus, und klicke dann auf **Installieren**, um das Paket für das Projekt zu installieren. Eine Auswirkung dieser Änderung ist, dass die Unterstützung für C++/CX im Projekt deaktiviert wird.

Wenn du in einer Aktion portieren kannst, empfiehlt es sich, die Unterstützung deaktiviert zu lassen, damit beim Ermitteln (und Portieren) aller deiner Abhängigkeiten aus C++/CX Buildmeldungen als Hilfe angezeigt werden.

Oder wenn Sie die Portierung schrittweise vollziehen müssen, aktivieren Sie die Unterstützung wieder (in den Projekteigenschaften **C/C++** \> **Allgemein** \> **Windows-Runtime-Erweiterung verwenden** \> **Ja (/ZW)** ). Alternativ (oder bei einem XAML-Projekt zusätzlich) füge deiner `.vcxproj`-Datei die folgende Eigenschaft hinzu, indem du die Eigenschaftenseite des C++/WinRT-Projekts in Visual Studio verwendest (in den Projekteigenschaften **Allgemeine Eigenschaften** \> **C++/WinRT** \> **Projektsprache** \> **C++/CX**). Eine Liste mit ähnlichen Anpassungsoptionen (um das Verhalten des Tools `cppwinrt.exe` zu optimieren) findest du in der [Infodatei](https://github.com/microsoft/cppwinrt/blob/master/nuget/readme.md#customizing) zum Microsoft.Windows.CppWinRT-NuGet-Paket. Beachte, dass du diesen Eigenschaftswert jedes Mal wieder in **C++/WinRT** zurückändern musst, wenn du den Inhalt einer **MIDL-Datei (.idl)** in Stubdateien verarbeiten musst.

```xml
<syntaxhighlight lang="xml">
  <PropertyGroup Label="Globals">
    <CppWinRTProjectLanguage>C++/CX</CppWinRTProjectLanguage>
  </PropertyGroup>
</syntaxhighlight>
```

Stelle sicher, dass die Projekteigenschaft **Allgemein** \> **Version der Zielplattform** auf 10.0.17134.0 (Windows 10, Version 1803) oder höher festgelegt ist.

Fügen Sie Ihrer vorkompilierten Headerdatei (in der Regel `pch.h`) `winrt/base.h` hinzu.

```cppwinrt
#include <winrt/base.h>
```

Wenn du Windows-API-Header mit C++/WinRT-Projektion hinzufügst (z. B. `winrt/Windows.Foundation.h`), musst du `winrt/base.h` nicht so explizit wie hier einfügen, da dies automatisch erfolgt.

Wenn für den Projekt zudem Typen der [C++-Vorlagenbibliothek für Windows-Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) verwendet werden, helfen dir die Informationen unter [Wechsel zu C++/WinRT von WRL](move-to-winrt-from-wrl.md) weiter.

## <a name="file-naming-conventions"></a>Konventionen für die Dateibenennung

### <a name="xaml-markup-files"></a>XAML-Markupdateien

| Dateiursprung | C++/CX | C++/WinRT |
| - | - | - |
| **XAML-Entwicklerdateien** | MyPage.xaml<br/>MyPage.xaml.h<br/>MyPage.xaml.cpp | MyPage.xaml<br/>MyPage.h<br/>MyPage.cpp<br/>MyPage.idl (siehe unten) |
| **Generierte XAML-Dateien** | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp | MyPage.xaml.g.h<br/>MyPage.xaml.g.hpp<br/>MyPage.g.h |

Beachte, dass C++/WinRT die Zeichenfolge `.xaml` aus den Dateinamen `*.h` und `*.cpp` entfernt.

C++/WinRT fügt eine zusätzliche Entwicklerdatei hinzu: die **Midl-Datei (.idl)** . C++/CX generiert diese Datei automatisch intern und fügt sie zu jedem öffentlichen und geschützten Member hinzu. In C++/WinRT erstellst du die Datei selbst und fügst sie hinzu. Weitere Informationen, Codebeispiele und eine exemplarische Vorgehensweise zum Schreiben in IDL findest du unter [XAML-Steuerelemente: Binden an eine C++/WinRT-Eigenschaft](/windows/uwp/cpp-and-winrt-apis/binding-property).

Siehe auch [Einbeziehen von Laufzeitklassen in Midl-Dateien (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

### <a name="runtime-classes"></a>Laufzeitklassen

In C++/CX gibt es keine Einschränkungen in Bezug auf die Namen von Headerdateien; es ist gängige Praxis, mehrere Laufzeitklassendefinitionen in eine einzelne Headerdatei zu schreiben, insbesondere bei kleinen Klassen. C++/WinRT erfordert jedoch für jede Laufzeitklasse eine eigene Headerdatei, die nach dem Klassennamen benannt ist. 

| C++/CX | C++/WinRT |
| - | - |
| **Common.h**<br>`ref class A { ... }`<br>`ref class B { ... }` | **Common.idl**<br>`runtimeclass A { ... }`<br>`runtimeclass B { ... }` |
|  | **A.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct A { ... };`<br>`}` |
|  | **B.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct B { ... };`<br>`}` |

Weniger gängig, aber dennoch erlaubt ist in C++/CX die Verwendung von Headerdateien mit unterschiedlichen Namen für benutzerdefinierte XAML-Steuerelemente. Diese Headerdateien müssen umbenannt werden, damit sie dem Klassennamen entsprechen.

| C++/CX | C++/WinRT |
| - | - |
| **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` | **A.xaml**<br>`<Page x:Class="LongNameForA" ...>` |
| **A.h**<br>`partial ref class LongNameForA { ... }` | **LongNameForA.h**<br>`namespace implements {`<br>&nbsp;&nbsp;`struct LongNameForA { ... };`<br>`}` |

## <a name="header-file-requirements"></a>Anforderungen an Headerdateien

In C++/CX musst du keine speziellen Headerdateien einschließen, weil Headerdateien automatisch aus `.winmd`-Dateien generiert werden. In C++/CX ist es gängige Praxis, `using`-Direktiven für Namespaces zu verwenden, die anhand des Namens genutzt werden.

```cppcx
using namespace Windows::Media::Playback;

String^ NameOfFirstVideoTrack(MediaPlaybackItem^ item)
{
    return item->VideoTracks->GetAt(0)->Name;
}
```

Die `using namespace Windows::Media::Playback`-Direktive ermöglicht es, `MediaPlaybackItem` ohne Namespacepräfix zu schreiben. Der `Windows.Media.Core`-Namespace ist ebenfalls beteiligt, weil `item->VideoTracks->GetAt(0)` ein **Windows.Media.Core.VideoTrack**-Element zurückgibt. Da der Name **VideoTrack** nirgendwo eingegeben werden muss, wird keine `using Windows.Media.Core`-Direktive benötigt.

In C++/WinRT musst du jedoch eine Headerdatei für jeden genutzten Namespace einschließen, auch wenn der Name nicht genannt wird.

```cppwinrt
#include <winrt/Windows.Media.Playback.h>
#include <winrt/Windows.Media.Core.h> // !!This is important!!

using namespace winrt;
using namespace Windows::Media::Playback;

winrt::hstring NameOfFirstVideoTrack(MediaPlaybackItem const& item)
{
    return item.VideoTracks().GetAt(0).Name();
}
```

Andererseits gilt auch: Obwohl das **MediaPlaybackItem.AudioTracksChanged**-Ereignis den Typ **TypedEventHandler\<MediaPlaybackItem, Windows.Foundation.Collections.IVectorChangedEventArgs\>** aufweist, müssen wir `winrt/Windows.Foundation.Collections.h` nicht einschließen, weil wir dieses Ereignis nicht verwendet haben.

In C++/WinRT musst du auch Headerdateien für Namespaces einschließen, die von XAML-Markup genutzt werden.

```xaml
<!-- MainPage.xaml -->
<Rectangle Height="400"/>
```

Bei Verwendung der **Rectangle**-Klasse muss dieses Include hinzugefügt werden.

```cppwinrt
// MainPage.h
#include <winrt/Windows.UI.Xaml.Shapes.h>
```

Wenn du eine Headerdatei vergisst, funktioniert die Kompilierung ordnungsgemäß, aber du erhältst Linkerfehler, weil die `consume_`-Klassen fehlen.

## <a name="parameter-passing"></a>Parameterübergabe

Beim Schreiben von C++/CX-Quellcode übergibst du C++/CX-Typen als Funktionsparameter in Form von Hütchenverweisen (\^).

```cppcx
void LogPresenceRecord(PresenceRecord^ record);
```

In C++/WinRT solltest du für synchrone Funktionen standardmäßig `const&`-Parameter verwenden. Hierdurch werden Kopiervorgänge und unnötiger Zusatzaufwand vermieden. Für deine Coroutinen sollte aber Pass-by-Value verwendet werden, um sicherzustellen, dass sie nach Wert erfassen, und um Probleme mit der Lebensdauer zu vermeiden (weitere Informationen unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](concurrency.md)).

```cppwinrt
void LogPresenceRecord(PresenceRecord const& record);
IASyncAction LogPresenceRecordAsync(PresenceRecord const record);
```

Ein C++/WinRT-Objekt ist im Grunde genommen ein Wert, der einen Schnittstellenzeiger zum zugrunde liegenden Windows-Runtime-Objekt enthält. Beim Kopieren eines C++/WinRT-Objekts kopiert der Compiler den gekapselten Schnittstellenzeiger, wodurch sich sein Verweiszähler erhöht. Wenn die Kopie zerstört wird, wird der Verweiszähler verringert. Der mit einem Kopiervorgang verbundene Aufwand sollte also nur in Kauf genommen werden, wenn dies wirklich erforderlich ist.

## <a name="variable-and-field-references"></a>Variablen und Feldverweise

Beim Schreiben von C++/CX-Quellcode verwendest du Hütchenvariablen (\^) zum Verweisen auf Windows-Runtime-Objekte sowie den Pfeiloperator (-&gt;), um den Verweis einer Hütchenvariablen aufzuheben.

```cppcx
IVectorView<User^>^ userList = User::Users;

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList->Size; ++iUser)
    ...
```

Beim Portieren in den entsprechenden C++/WinRT-Code ist es sehr effektiv, die Hütchen zu entfernen und den Pfeiloperator (-&gt;) in den Punktoperator (.) zu ändern. Bei Typen mit C++/WinRT-Projektion handelt es sich um Werte, nicht um Zeiger.

```cppwinrt
IVectorView<User> userList = User::Users();

if (userList != nullptr)
{
    for (UINT32 iUser = 0; iUser < userList.Size(); ++iUser)
    ...
```

Der Standardkonstruktor für einen C++/CX-Handledeklaratorverweis (^) führt zu einer Initialisierung mit NULL. Hier ist ein C++/CX-Codebeispiel angegeben, in dem wir eine Variable bzw. ein Feld des richtigen Typs erstellen, die bzw. das aber nicht initialisiert ist. Es ist also anfänglich kein Verweis auf ein **TextBlock**-Element vorhanden. Wir weisen später einen Verweis zu.

```cppcx
TextBlock^ textBlock;

class MyClass
{
    TextBlock^ textBlock;
};
```

Informationen zur Entsprechung in C++/WinRT findest du unter [Verzögerte Initialisierung](consume-apis.md#delayed-initialization).

## <a name="properties"></a>Eigenschaften

Die C++/CX-Spracherweiterungen beinhalten das Konzept der „Eigenschaften“. Beim Schreiben von C++/CX-Quellcode kannst du so auf eine Eigenschaft zugreifen, als ob sie ein Feld wäre. Da der C++-Standardcode nicht über das Konzept einer Eigenschaft verfügt, rufst du in C++/WinRT Get- und Set-Funktionen auf.

In den folgenden Beispielen sind **XboxUserId**, **UserState**, **PresenceDeviceRecords** und **Size** jeweils Eigenschaften.

### <a name="retrieving-a-value-from-a-property"></a>Abrufen eines Werts aus einer Eigenschaft

Hier ist beschrieben, wie du einen Eigenschaftswert in C++/CX erhältst.

```cppcx
void Sample::LogPresenceRecord(PresenceRecord^ record)
{
    auto id = record->XboxUserId;
    auto state = record->UserState;
    auto size = record->PresenceDeviceRecords->Size;
}
```

Der entsprechende C++/WinRT-Quellcode ruft eine Funktion mit dem gleichen Namen wie die Eigenschaft auf, aber ohne Parameter.

```cppwinrt
void Sample::LogPresenceRecord(PresenceRecord const& record)
{
    auto id = record.XboxUserId();
    auto state = record.UserState();
    auto size = record.PresenceDeviceRecords().Size();
}
```

Beachte hierbei, dass die Funktion **PresenceDeviceRecords** ein Windows-Runtime-Objekt zurückgibt, das über eine **Size**-Funktion verfügt. Da es sich beim zurückgegebenen Objekt ebenfalls um einen Typ mit C++/WinRT-Projektion handelt, führen wir das Dereferenzieren durch, indem wir den Punktoperator zum Aufrufen von **Size** verwenden.

### <a name="setting-a-property-to-a-new-value"></a>Festlegen einer Eigenschaft auf einen neuen Wert

Beim Festlegen einer Eigenschaft auf einen neuen Wert wird ein ähnliches Muster angewandt. Zuerst in C++/CX.

```cppcx
record->UserState = newValue;
```

Um dasselbe in C++/WinRT durchzuführen, rufst du eine Funktion mit dem gleichen Namen wie die Eigenschaft auf und übergibst ein Argument.

```cppwinrt
record.UserState(newValue);
```

## <a name="creating-an-instance-of-a-class"></a>Erstellen einer Instanz einer Klasse

Hierfür verwendest du ein C++/ CX-Objekt über einen Handle (Hütchenverweis \^). Du erstellst ein neues Objekt über das Schlüsselwort `ref new`, das wiederum [**RoActivateInstance**](https://docs.microsoft.com/windows/desktop/api/roapi/nf-roapi-roactivateinstance) aufruft, um eine neue Instanz der Laufzeitklasse zu aktivieren.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
private:
    Buffer^ m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
};
```

Ein C++/WinRT-Objekt ist ein Wert. Folglich kannst du es im Stapel oder als Feld eines Objekts zuweisen. Du verwendest *niemals*`ref new` (oder `new`), um ein C++/WinRT-Objekt zuzuordnen. Im Hintergrund wird dennoch **RoActivateInstance** aufgerufen.

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
private:
    Buffer m_gamerPicBuffer{ MAX_IMAGE_SIZE };
};
```

Falls die Initialisierung einer Ressource aufwendig ist, wird dieser Vorgang meist verzögert, bis sie tatsächlich benötigt wird. Wie bereits erwähnt, führt der Standardkonstruktor für einen C++/CX-Handledeklaratorverweis (^) zu einer Initialisierung mit NULL.

```cppcx
using namespace Windows::Storage::Streams;

class Sample
{
public:
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = ref new Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer^ m_gamerPicBuffer;
};
```

Hier ist der gleiche Code nach dem Portieren zu C++/WinRT angegeben. Beachten Sie die Verwendung des **std::nullptr_t**-Konstruktors. Weitere Informationen zu diesem Konstruktor findest du unter [Verzögerte Initialisierung](consume-apis.md#delayed-initialization).

```cppwinrt
using namespace winrt::Windows::Storage::Streams;

struct Sample
{
    void DelayedInit()
    {
        // Allocate the actual buffer.
        m_gamerPicBuffer = Buffer(MAX_IMAGE_SIZE);
    }

private:
    Buffer m_gamerPicBuffer{ nullptr };
};
```

## <a name="how-the-default-constructor-affects-collections"></a>Auswirkungen des Standardkonstruktors auf Sammlungen

C++Sammlungstypen verwenden den Standardkonstruktor, was zu einer unbeabsichtigten Objektkonstruktion führen kann.

| Szenario | C++/CX | C++/WinRT (falsch) | C++/WinRT (richtig) |
| - | - | - | - |
| Lokale Variable, anfänglich leer | `TextBox^ textBox;` | `TextBox textBox; // Creates a TextBox!` | `TextBox textBox{ nullptr };` |
| Membervariable, anfänglich leer | `class C {`<br/>&nbsp;&nbsp;`TextBox^ textBox;`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textBox; // Creates a TextBox!`<br/>`};` | `class C {`<br/>&nbsp;&nbsp;`TextBox textbox{ nullptr };`<br/>`};` |
| Globale Variable, anfänglich leer | `TextBox^ g_textBox;` | `TextBox g_textBox; // Creates a TextBox!` | `TextBox g_textBox{ nullptr };` |
| Vektor von leeren Verweisen | `std::vector<TextBox^> boxes(10);` | `// Creates 10 TextBox objects!`<br/>`std::vector<TextBox> boxes(10);` | `std::vector<TextBox> boxes(10, nullptr);` |
| Festlegen eines Werts in einer Zuordnung | `std::map<int, TextBox^> boxes;`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`// Creates a TextBox at 2,`<br/>`// then overwrites it!`<br/>`boxes[2] = value;` | `std::map<int, TextBox> boxes;`<br/>`boxes.insert_or_assign(2, value);` |
| Array aus leeren Verweisen | `TextBox^ boxes[2];` | `// Creates 2 TextBox objects!`<br/>`TextBox boxes[2];` | `TextBox boxes[2] = { nullptr, nullptr };` |
| Koppeln | `std::pair<TextBox^, String^> p;` | `// Creates a TextBox!`<br/>`std::pair<TextBox, String> p;` | `std::pair<TextBox, String> p{ nullptr, nullptr };` |

### <a name="more-about-collections-of-empty-references"></a>Weitere Informationen zu Sammlungen leerer Verweise

Wenn du über ein **Platform::Array\^** -Element in C++ verfügst (siehe [Port **Platform:: Array\^** ](#port-platformarray)), kannst du es in ein **std::vector**-Element in C++/WinRT portieren (tatsächlich in jeden beliebigen zusammenhängenden Container), anstatt es als Array zu belassen. Die Auswahl von **std::vector** bietet Vorteile.

Während es beispielsweise eine Kurzform für die Erstellung eines Vektors von leeren Verweisen mit fester Größe gibt (siehe Tabelle oben), gibt es keine solche Kurzform für die Erstellung eines *Arrays* von leeren Verweisen. Du musst `nullptr` für jedes Element im Array wiederholen. Wenn zu wenige „nullptr“ vorhanden sind, werden die zusätzlichen Elemente standardmäßig konstruiert.

Einen Vektor kannst du bei der Initialisierung mit leeren Verweisen füllen (wie in der obigen Tabelle), oder du kannst ihn nach der Initialisierung mit einem Code wie diesem mit leeren Verweisen füllen.

```cppwinrt
std::vector<TextBox> boxes(10); // 10 default-constructed TextBoxes.
boxes.resize(10, nullptr); // 10 empty references.
```

### <a name="more-about-the-stdmap-example"></a>Weitere Informationen zum **std::map**-Beispiel

Der subscript-Operator `[]` für **std::map** weist folgendes Verhalten auf.

- Wenn der Schlüssel in der Zuordnung gefunden wird, wird ein Verweis auf den vorhandenen Wert zurückgegeben (den du überschreiben kannst).
- Wenn der Schlüssel in der Zuordnung nicht gefunden wird, wird ein neuer Eintrag in der Zuordnung erstellt, der den Schlüssel (verschoben, wenn verschiebbar) und *einen standardmäßig konstruierten Wert* enthält. Dann wird ein Verweis auf den Wert zurückgegeben (den du dann überschreiben kannst).

Anders gesagt: Der `[]`-Operator erstellt immer einen Eintrag in der Zuordnung. Dieses Verhalten unterscheidet sich von C#, Java und JavaScript.

## <a name="converting-from-a-base-runtime-class-to-a-derived-one"></a>Konvertieren von einer Basis-Laufzeitklasse zu einer abgeleiteten Klasse

Üblicherweise wird ein Verweis auf die Basis verwendet, für die bekannt ist, dass auf ein Objekt mit einem abgeleiteten Typ verwiesen wird. In C++/CX verwendest du `dynamic_cast`, um „Verweis auf Basis“ in „Verweis auf abgeleitet“ *umzuwandeln*. `dynamic_cast` ist eigentlich nur ein versteckter Aufruf von [**QueryInterface**](https://docs.microsoft.com/windows/desktop/api/unknwn/nf-unknwn-iunknown-queryinterface(q_)). Hier ist ein typisches Beispiel angegeben: Du verarbeitest ein Ereignis für eine geänderte Abhängigkeitseigenschaft und möchtest die Umwandlung von **DependencyObject** zurück in den eigentlichen Typ durchführen, der der Besitzer der Abhängigkeitseigenschaft ist.

```cppcx
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject^ d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs^ e)
{
    BgLabelControl^ theControl{ dynamic_cast<BgLabelControl^>(d) };

    if (theControl != nullptr)
    {
        // succeeded ...
    }
}
```

Der entsprechende C++/WinRT-Code ersetzt `dynamic_cast` durch einen Aufruf der Funktion [**IUnknown::try_as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknowntry_as-function), in die **QueryInterface** eingekapselt ist. Darüber hinaus hast du die Option, stattdessen [**IUnknown::as**](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#iunknownas-function) aufzurufen. Hierbei wird eine Ausnahme ausgelöst, wenn bei der Abfrage der erforderlichen Schnittstelle (Standardschnittstelle des angeforderten Typs) keine Rückgabe erfolgt. Hier ist ein C++/WinRT-Codebeispiel angegeben.

```cppwinrt
void BgLabelControl::OnLabelChanged(Windows::UI::Xaml::DependencyObject const& d, Windows::UI::Xaml::DependencyPropertyChangedEventArgs const& e)
{
    if (BgLabelControlApp::BgLabelControl theControl{ d.try_as<BgLabelControlApp::BgLabelControl>() })
    {
        // succeeded ...
    }

    try
    {
        BgLabelControlApp::BgLabelControl theControl{ d.as<BgLabelControlApp::BgLabelControl>() };
        // succeeded ...
    }
    catch (winrt::hresult_no_interface const&)
    {
        // failed ...
    }
}
```

## <a name="derived-classes"></a>Abgeleitete Klassen

Um aus einer Laufzeitklasse abzuleiten, muss die Basisklasse *zusammensetzbar* sein. In C++/CX sind keine besonderen Schritte erforderlich, um Klassen zusammensetzbar zu machen, in C++/WinRT schon. Du verwendest das [unsealed-Schlüsselwort](/uwp/midl-3/intro#base-classes), um anzugeben, dass die Klasse als Basisklasse verwendet werden kann.

```idl
unsealed runtimeclass BasePage : Windows.UI.Xaml.Controls.Page
{
    ...
}
runtimeclass DerivedPage : BasePage
{
    ...
}
```

In der Implementierungsheaderklasse musst du die Basisklassen-Headerdatei einschließen, bevor du den automatisch generierten Header für die abgeleitete Klasse einschließt. Andernfalls erhältst du Fehler wie „Ungültige Verwendung dieses Typs als Ausdruck“.

```cppwinrt
// DerivedPage.h
#include "BasePage.h"       // This comes first.
#include "DerivedPage.g.h"  // Otherwise this header file will produce an error.

namespace winrt::MyNamespace::implementation
{
    struct DerivedPage : DerivedPageT<DerivedPage>
    {
        ...
    }
}
```

## <a name="event-handling-with-a-delegate"></a>Ereignisbehandlung mit einem Delegaten

Hier ist ein typisches Beispiel für die Behandlung eines Ereignisses in C++/CX mit einer Lambda-Funktion als Delegat angegeben.

```cppcx
auto token = myButton->Click += ref new RoutedEventHandler([=](Platform::Object^ sender, RoutedEventArgs^ args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Dies ist die Entsprechung in C++/WinRT.

```cppwinrt
auto token = myButton().Click([=](IInspectable const& sender, RoutedEventArgs const& args)
{
    // Handle the event.
    // Note: locals are captured by value, not reference, since this handler is delayed.
});
```

Anstelle einer Lambda-Funktion kannst du den Delegaten als freie Funktion oder als Pointer-to-Member-Funktion (Zeiger auf Member) implementieren. Weitere Informationen findest du unter [Verarbeiten von Ereignissen über Delegaten in C++/WinRT](handle-events.md).

Wenn du von einer C++/CX-Codebasis portierst, für die Ereignisse und Delegate intern verwendet werden (nicht über Binärdateien), hilft dir [**winrt::delegate**](/uwp/cpp-ref-for-winrt/delegate) beim Replizieren dieses Musters in C++/WinRT weiter. Siehe auch [Parametrisierte Delegaten, einfache Signale und Rückrufe in einem Projekt](author-events.md#parameterized-delegates-simple-signals-and-callbacks-within-a-project).

## <a name="revoking-a-delegate"></a>Widerrufen eines Delegaten

In C++/CX verwendest du den Operator `-=`, um eine frühere Ereignisregistrierung zu widerrufen.

```cppcx
myButton->Click -= token;
```

Dies ist die Entsprechung in C++/WinRT.

```cppwinrt
myButton().Click(token);
```

Weitere Informationen und Optionen findest du unter [Einen registrierten Delegaten widerrufen](handle-events.md#revoke-a-registered-delegate).

## <a name="boxing-and-unboxing"></a>Boxing und Unboxing

C++/CX führt automatisch ein Boxing von Skalaren zu Objekten durch. In C++/WinRT musst du die Funktion [**winrt::box_value**](/uwp/cpp-ref-for-winrt/box-value) explizit aufrufen. In beiden Sprachen musst du ein Unboxing explizit angeben. Siehe [Boxing und Unboxing mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/boxing).

In der unten stehenden Tabelle werden folgende Definitionen verwendet.

| C++/CX | C++/WinRT|
|-|-|
| `int i;` | `int i;` |
| `String^ s;` | `winrt::hstring s;` |
| `Object^ o;` | `IInspectable o;`|

| Vorgang | C++/CX | C++/WinRT|
|-|-|-|-|
| Boxing | `o = 1;`<br>`o = "string";` | `o = box_value(1);`<br>`o = box_value(L"string");` |
| Unboxing | `i = (int)o;`<br>`s = (String^)o;` | `i = unbox_value<int>(o);`<br>`s = unbox_value<winrt::hstring>(o);` |

C++/CX und C# lösen Ausnahmen aus, wenn du versuchst, ein Unboxing eines NULL-Zeigers auf einen Werttyp auszuführen. C++/WinRT betrachtet dies als Programmierfehler und stürzt ab. Verwende in C++/WinRT die Funktion [**winrt::unbox_value_or**](/uwp/cpp-ref-for-winrt/unbox-value-or), wenn ein Objekt nicht den von dir angenommenen Typ aufweist.

| Szenario | C++/CX | C++/WinRT|
|-|-|-|-|
| Unboxing eines bekannten integer-Elements | `i = (int)o;` | `i = unbox_value<int>(o);` |
| Wenn „o“ NULL ist | `Platform::NullReferenceException` | Absturz |
| Wenn „o“ kein geboxter int-Wert ist | `Platform::InvalidCastException` | Absturz |
| Unboxing für „int“, Fallback bei NULL; Absturz in allen anderen Fällen | `i = o ? (int)o : fallback;` | `i = o ? unbox_value<int>(o) : fallback;` |
| Unboxing für „int“ (falls möglich); Fallback in allen anderen Fällen | `auto box = dynamic_cast<IBox<int>^>(o);`<br>`i = box ? box->Value : fallback;` | `i = unbox_value_or<int>(o, fallback);` |

### <a name="boxing-and-unboxing-a-string"></a>Boxing und Unboxing von string-Elementen

Ein string-Element ist in einigen Fällen ein Werttyp, in anderen Fällen ein Verweistyp. C++/CX und C++/WinRT behandeln string-Elemente unterschiedlich.

Der ABI-Typ [**HSTRING**](/windows/win32/winrt/hstring) ist ein Zeiger auf ein als Verweis gezähltes string-Element. Er wird jedoch nicht von [**IInspectable**](/windows/win32/api/inspectable/nn-inspectable-iinspectable) abgeleitet, ist also technisch gesehen kein *Objekt*. Darüber hinaus stellt ein **HSTRING** mit Wert NULL ein leeres string-Element dar. Das Boxing von Elementen, die nicht von **IInspectable** abgeleitet werden, erfolgt über den Einschluss in [**IReference\<T\>** ](/uwp/api/windows.foundation.ireference_t_), und die Windows-Runtime stellt eine Standardimplementierung in Form des [**PropertyValue**](/uwp/api/windows.foundation.propertyvalue)-Objekts bereit (benutzerdefinierte Typen werden als [**PropertyType::OtherType**](/uwp/api/windows.foundation.propertytype) gemeldet).

C++/CX stellt ein string-Element der Windows-Runtime als Referenztyp dar, C++/WinRT dagegen als Werttyp. Das bedeutet, dass eine geboxte NULL-Zeichenfolge unterschiedliche Darstellungen aufweisen kann, je nachdem, wie du dorthin gelangt bist.

Darüber hinaus erlaubt C++/CX das Dereferenzieren eines **String^** -Elements mit NULL-Wert, woraufhin dieses Element sich wie die Zeichenfolge `""` verhält.

| Verhalten | C++/CX | C++/WinRT|
|-|-|-|
| Deklarationen | `Object^ o;`<br>`String^ s;` | `IInspectable o;`<br>`hstring s;` |
| Kategorie des string-Typs | Verweistyp | Werttyp |
| **HSTRING** mit NULL-Wert wird dargestellt als | `(String^)nullptr` | `hstring{}` |
| Sind NULL und `""` identisch? | Ja | Ja |
| Gültigkeit von NULL | `s = nullptr;`<br>`s->Length == 0` (gültig) | `s = hstring{};`<br>`s.size() == 0` (gültig) |
| Wenn Sie einem Objekt eine NULL-Zeichenfolge zuweisen | `o = (String^)nullptr;`<br>`o == nullptr` | `o = box_value(hstring{});`<br>`o != nullptr` |
| Wenn Sie einem Objekt `""` zuweisen | `o = "";`<br>`o == nullptr` | `o = box_value(hstring{L""});`<br>`o != nullptr` |

Grundlegendes Boxing und Unboxing

| Vorgang | C++/CX | C++/WinRT|
|-|-|-|
| Boxing eines string-Elements | `o = s;`<br>Eine leere Zeichenfolge wird zu einem nullptr. | `o = box_value(s);`<br>Eine leere Zeichenfolge wird zu einem Nicht-NULL-Objekt. |
| Unboxing eines bekannten string-Elements | `s = (String^)o;`<br>Ein NULL-Objekt wird zu einer leeren Zeichenfolge.<br>InvalidCastException, falls keine Zeichenfolge. | `s = unbox_value<hstring>(o);`<br>Absturz eines NULL-Objekts.<br>Absturz, falls keine Zeichenfolge. |
| Unboxing einer möglichen Zeichenfolge | `s = dynamic_cast<String^>(o);`<br>Ein NULL-Objekt oder eine Nicht-Zeichenfolge wird zu einer leeren Zeichenfolge. | `s = unbox_value_or<hstring>(o, fallback);`<br>NULL oder eine Nicht-Zeichenfolge wird zu einem Fallback.<br>Leere Zeichenfolge beibehalten. |

## <a name="concurrency-and-asynchronous-operations"></a>Parallelität und asynchrone Vorgänge

Die Parallel Patterns Library (PPL) (beispielsweise [**concurrency::task**](/cpp/parallel/concrt/reference/task-class)) wurde aktualisiert und unterstützt jetzt C++/CX-Handledeklaratorverweise (^).

In C++/WinRT solltest du stattdessen Coroutinen und `co_await` verwenden. Weitere Informationen und Codebeispiele findest du unter [Parallelität und asynchrone Vorgänge mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

## <a name="consuming-objects-from-xaml-markup"></a>Verwenden von Objekten aus XAML-Markup

In einem C++/CX-Projekt kannst du private Member und benannte Elemente aus XAML-Markup nutzen. In C++/WinRT dagegen müssen alle Entitäten, die über die XAML-[ **{x:Bind}-Markuperweiterung**](/windows/uwp/xaml-platform/x-bind-markup-extension) genutzt werden, in IDL öffentlich verfügbar gemacht werden.

Durch Binden an einen booleschen Typ wird in C++/CX `true` oder `false` angezeigt, in C++/WinRT dagegen **Windows.Foundation.IReference`1\<Boolean\>** .

Weitere Informationen sowie Codebeispiele findest du unter [Verwenden von Objekten aus Markup](/windows/uwp/cpp-and-winrt-apis/binding-property#consuming-objects-from-xaml-markup).

## <a name="mapping-ccx-platform-types-to-cwinrt-types"></a>Zuordnen von C++/CX-**Platform**-Typen zu C++/WinRT-Typen

Unter C++/CX werden im **Platform**-Namespace verschiedene Datentypen bereitgestellt. Diese Typen gehören nicht zum C++-Standard. Daher kannst du sie nur verwenden, wenn du die Windows-Runtime-Spracherweiterungen aktivierst (Visual Studio-Projekteigenschaft **C/C++**  > **Allgemein** > **Windows-Runtime-Erweiterung verwenden** > **Ja (/ZW)** ). In der Tabelle unten findest du Informationen zum Portieren von **Platform**-Typen in ihre Entsprechungen in C++/WinRT. Anschließend kannst du die Option `/ZW` deaktivieren, da C++/WinRT zum C++-Standard gehört.

| C++/CX | C++/WinRT |
| ---- | ---- |
| **Platform::Agile\^** | [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref) |
| **Platform::Array\^** | Siehe [Portieren von **Platform::Array\^** ](#port-platformarray) |
| **Platform::Exception\^** | [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) |
| **Platform::InvalidArgumentException\^** | [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) |
| **Platform::Object\^** | **winrt::Windows::Foundation::IInspectable** |
| **Platform::String\^** | [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) |

### <a name="port-platformagile-to-winrtagile_ref"></a>Portieren von **Platform::Agile\^** zu **winrt::agile_ref**

Der Typ **Platform::Agile\^** in C++/CX stellt eine Windows-Runtime-Klasse dar, auf die über einen beliebigen Thread zugegriffen werden kann. Die C++/WinRT-Entsprechung lautet [**winrt::agile_ref**](/uwp/cpp-ref-for-winrt/agile-ref).

In C++/CX:

```cppcx
Platform::Agile<Windows::UI::Core::CoreWindow> m_window;
```

In C++/WinRT:

```cppwinrt
winrt::agile_ref<Windows::UI::Core::CoreWindow> m_window;
```

### <a name="port-platformarray"></a>Portieren von **Platform::Array\^**

In Fällen, in denen C++/CX die Verwendung eines Arrays erfordert, erlaubt C++/WinRT die Verwendung eines beliebigen zusammenhängenden Containers. Unter [Auswirkungen des Standardkonstruktors auf Sammlungen](#how-the-default-constructor-affects-collections) wird erläutert, warum **std::vector** eine gute Wahl ist.

Wenn du also ein **Platform::Array\^** -Element in C++/CX verwendest, gehören die Verwendung einer Initialisiererliste, eines **std::array**- oder **std::vector**-Elements zu deinen Portierungsoptionen. Weitere Informationen und Codebeispiele findest du unter [Standard-Initialisierungslisten](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-initializer-lists) und [Standard-Arrays und -Vektoren](/windows/uwp/cpp-and-winrt-apis/std-cpp-data-types#standard-arrays-and-vectors).

### <a name="port-platformexception-to-winrthresult_error"></a>Portieren von **Platform::Exception\^** zu **winrt::hresult_error**

Der Typ **Platform::Exception\^** wird in C++/CX erzeugt, wenn eine Windows-Runtime-API kein S\_OK HRESULT zurückgibt. Für C++/WinRT ist die Entsprechung [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error).

Zum Portieren zu C++/WinRT musst du den gesamten Code ändern, für den **Platform::Exception\^** verwendet wird, um stattdessen **winrt::hresult_error** zu nutzen.

In C++/CX:

```cppcx
catch (Platform::Exception^ ex)
```

In C++/WinRT:

```cppwinrt
catch (winrt::hresult_error const& ex)
```

C++/WinRT stellt die hier angegebenen Ausnahmeklassen bereit.

| Ausnahmetyp | Basisklasse | HRESULT |
| ---- | ---- | ---- |
| [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) | | Aufruf von [**hresult_error::to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function) |
| [**winrt::hresult_access_denied**](/uwp/cpp-ref-for-winrt/error-handling/hresult-access-denied) | **winrt::hresult_error** | E_ACCESSDENIED |
| [**winrt::hresult_canceled**](/uwp/cpp-ref-for-winrt/error-handling/hresult-canceled) | **winrt::hresult_error** | ERROR_CANCELLED |
| [**winrt::hresult_changed_state**](/uwp/cpp-ref-for-winrt/error-handling/hresult-changed-state) | **winrt::hresult_error** | E_CHANGED_STATE |
| [**winrt::hresult_class_not_available**](/uwp/cpp-ref-for-winrt/error-handling/hresult-class-not-available) | **winrt::hresult_error** | CLASS_E_CLASSNOTAVAILABLE |
| [**winrt::hresult_illegal_delegate_assignment**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-delegate-assignment) | **winrt::hresult_error** | E_ILLEGAL_DELEGATE_ASSIGNMENT |
| [**winrt::hresult_illegal_method_call**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-method-call) | **winrt::hresult_error** | E_ILLEGAL_METHOD_CALL |
| [**winrt::hresult_illegal_state_change**](/uwp/cpp-ref-for-winrt/error-handling/hresult-illegal-state-change) | **winrt::hresult_error** | E_ILLEGAL_STATE_CHANGE |
| [**winrt::hresult_invalid_argument**](/uwp/cpp-ref-for-winrt/error-handling/hresult-invalid-argument) | **winrt::hresult_error** | E_INVALIDARG |
| [**winrt::hresult_no_interface**](/uwp/cpp-ref-for-winrt/error-handling/hresult-no-interface) | **winrt::hresult_error** | E_NOINTERFACE |
| [**winrt::hresult_not_implemented**](/uwp/cpp-ref-for-winrt/error-handling/hresult-not-implemented) | **winrt::hresult_error** | E_NOTIMPL |
| [**winrt::hresult_out_of_bounds**](/uwp/cpp-ref-for-winrt/error-handling/hresult-out-of-bounds) | **winrt::hresult_error** | E_BOUNDS |
| [**winrt::hresult_wrong_thread**](/uwp/cpp-ref-for-winrt/error-handling/hresult-wrong-thread) | **winrt::hresult_error** | RPC_E_WRONG_THREAD |

Beachte, dass jede Klasse (über die **hresult_error**-Basisklasse) eine [**to_abi**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errorto_abi-function)-Funktion bereitstellt, die das HRESULT für den Fehler zurückgibt, sowie eine [**message**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error#hresult_errormessage-function)-Funktion, die die Darstellung der Zeichenfolge dieses HRESULT zurückgibt.

Hier ist ein Beispiel für das Auslösen einer Ausnahme in C++/CX angegeben:

```cppcx
throw ref new Platform::InvalidArgumentException(L"A valid User is required");
```

Dies ist die Entsprechung in C++/WinRT:

```cppwinrt
throw winrt::hresult_invalid_argument{ L"A valid User is required" };
```

### <a name="port-platformobject-to-winrtwindowsfoundationiinspectable"></a>Portieren von **Platform::Object\^** zu **winrt::Windows::Foundation::IInspectable**

Wie alle C++/WinRT-Typen ist **winrt::Windows::Foundation::IInspectable** ein Werttyp. Hier erfährst du, wie du eine Variable dieses Typs auf null initialisierst.

```cppwinrt
winrt::Windows::Foundation::IInspectable var{ nullptr };
```

### <a name="port-platformstring-to-winrthstring"></a>Portieren von **Platform::String\^** zu **winrt::hstring**

**Platform::String\^** ist das Äquivalent zum HSTRING-ABI-Typ der Windows-Runtime. Für C++/WinRT ist die Entsprechung [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). Bei C++/WinRT kannst du aber Windows-Runtime-APIs mit Wide-String-Typen der C++-Standardbibliothek, z. B. **std::wstring**, bzw. Wide-String-Literale aufrufen. Weitere Informationen und Codebeispiele findest du unter [String-Verarbeitung in C++/WinRT](strings.md).

Mit C++/CX kannst du auf die Eigenschaft [**Platform::String::Data**](https://docs.microsoft.com/cpp/cppcx/platform-string-class?view=vs-2019#data) zugreifen, um die Zeichenfolge als **const wchar_t\*** -Array im C-Stil abzurufen (z. B. zur Übergabe an **std::wcout**).

```cppcx
auto var{ titleRecord->TitleName->Data() };
```

Mit C++/WinRT erreichst du dies, indem du die Funktion [**hstring::c_str**](/uwp/api/windows.foundation.uri.-ctor#Windows_Foundation_Uri__ctor_System_String_) verwendest, um eine auf null beendete Zeichenfolgenversion im C-Stil abzurufen (wie über **std::wstring**).

```cppwinrt
auto var{ titleRecord.TitleName().c_str() };
```

Bei der Implementierung von APIs, die Zeichenfolgen übernehmen oder zurückgeben, änderst du in der Regel jeglichen C++/CX-Code, der **Platform::String\^** verwendet, um stattdessen **winrt::hstring** zu nutzen.

Hier ist ein Beispiel für eine C++/CX-API angegeben, für die eine Zeichenfolge verwendet wird:

```cppcx
void LogWrapLine(Platform::String^ str);
```

Für C++/WinRT kannst du diese API in [MIDL 3.0](/uwp/midl-3) beispielsweise wie folgt deklarieren:

```idl
// LogType.idl
void LogWrapLine(String str);
```

Die C++/WinRT-Toolkette generiert dann Quellcode für dich, der wie folgt aussieht.

```cppwinrt
void LogWrapLine(winrt::hstring const& str);
```

#### <a name="tostring"></a>ToString()

C++/CX-Typen stellen die [Object::ToString](/cpp/cppcx/platform-object-class#tostring)-Methode bereit.

```cppcx
int i{ 2 };
auto s{ i.ToString() }; // s is a Platform::String^ with value L"2".
```

In C++/WinRT ist dies nicht direkt verfügbar, aber du kannst Alternativen nutzen.

```cppwinrt
int i{ 2 };
auto s{ std::to_wstring(i) }; // s is a std::wstring with value L"2".
```

C++/WinRT unterstützt auch [**winrt::to_hstring**](/uwp/cpp-ref-for-winrt/to-hstring) für eine begrenzte Anzahl von Typen. Du musst Überladungen für alle zusätzlichen Typen hinzufügen, für die du eine Stringification durchführen möchtest.

| Language | Stringification von „int“ | Stringification von „enum“ |
| - | - | - |
| C++/CX | `String^ result = "hello, " + intValue.ToString();` | `String^ result = "status: " + status.ToString();` |
| C++/WinRT | `hstring result = L"hello, " + to_hstring(intValue);` | `// must define overload (see below)`<br>`hstring result = L"status: " + to_hstring(status);` |

Bei der Stringification eines enum-Typs musst du die Implementierung von **winrt::to_hstring** bereitstellen.

```cppwinrt
namespace winrt
{
    hstring to_hstring(StatusEnum status)
    {
        switch (status)
        {
        case StatusEnum::Success: return L"Success";
        case StatusEnum::AccessDenied: return L"AccessDenied";
        case StatusEnum::DisabledByPolicy: return L"DisabledByPolicy";
        default: return to_hstring(static_cast<int>(status));
        }
    }
}
```

Diese Stringifications werden häufig implizit von der Datenbindung verwendet.

```xaml
<TextBlock>
You have <Run Text="{Binding FlowerCount}"/> flowers.
</TextBlock>
<TextBlock>
Most recent status is <Run Text="{x:Bind LatestOperation.Status}"/>.
</TextBlock>
```

Diese Bindungen führen **winrt::to_hstring** der gebundenen Eigenschaft durch. Im zweiten Beispiel (**StatusEnum**) musst du eine eigene Überladung von **winrt::to_hstring** bereitstellen, andernfalls erhältst du einen Compilerfehler.

#### <a name="string-building"></a>Erstellung von string-Elementen

C++/CX und C++/WinRT nutzen ein standardmäßiges **std::wstringstream**-Element für die Erstellung von string-Elementen.

| Vorgang | C++/CX | C++/WinRT |
|-|-|-|
| Anfügen eines string-Elements, NULL-Werte beibehalten | `stream.print(s->Data(), s->Length);` | `stream << std::wstring_view{ s };` |
| Anfügen eines string-Elements, bei erstem NULL-Wert beenden | `stream << s->Data();` | `stream << s.c_str();` |
| Ergebnisse extrahieren | `ws = stream.str();` | `ws = stream.str();` |

#### <a name="more-examples"></a>Weitere Beispiele anzeigen

In den unten stehenden Beispielen ist *ws* eine Variable des Typs **std::wstring**. C++/CX kann ein **Platform::String**-Element aus einer 8-Bit-Zeichenfolge erstellen, dies ist in C++/WinRT nicht möglich.

| Vorgang | C++/CX | C++/WinRT |
| - | - | - |
| string-Element aus Literal erstellen | `String^ s = "hello";`<br>`String^ s = L"hello";` | `// winrt::hstring s{ "hello" }; // Doesn't compile`<br>`winrt::hstring s{ L"hello" };` |
| Aus **std::wstring** konvertieren, NULL-Werte beibehalten | `String^ s = ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size());` | `winrt::hstring s{ ws };`<br>`s = winrt::hstring(ws);`<br>`// s = ws; // Doesn't compile` |
| Aus **std::wstring** konvertieren, bei erstem NULL-Wert beenden | `String^ s = ref new String(ws.c_str());` | `winrt::hstring s{ ws.c_str() };`<br>`s = winrt::hstring(ws.c_str());`<br>`// s = ws.c_str(); // Doesn't compile` |
| Zu **std::wstring** konvertieren, NULL-Werte beibehalten | `std::wstring ws{ s->Data(), s->Length };`<br>`ws = std::wstring(s>Data(), s->Length);` | `std::wstring ws{ s };`<br>`ws = s;` |
| Zu **std::wstring** konvertieren, bei erstem NULL-Wert beenden | `std::wstring ws{ s->Data() };`<br>`ws = s->Data();` | `std::wstring ws{ s.c_str() };`<br>`ws = s.c_str();` |
| Literal an Methode übergeben | `Method("hello");`<br>`Method(L"hello");` | `// Method("hello"); // Doesn't compile`<br>`Method(L"hello");` |
| **std::wstring** an Methode übergeben | `Method(ref new String(ws.c_str(),`<br>&nbsp;&nbsp;`(uint32_t)ws.size()); // Stops on first null` | `Method(ws);`<br>`// param::winrt::hstring accepts std::wstring_view` |

## <a name="important-apis"></a>Wichtige APIs
* [Strukturvorlage „winrt::delegate“](/uwp/cpp-ref-for-winrt/delegate)
* [Struktur „winrt::hresult_error“](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::hstring-Struktur](/uwp/cpp-ref-for-winrt/hstring)
* [winrt-Namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Zugehörige Themen
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Erstellen von Ereignissen in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-events)
* [Parallelität und asynchrone Vorgänge mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency)
* [Verwenden von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-apis)
* [Behandeln von Ereignissen mithilfe von Delegaten in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/handle-events)
* [Interoperabilität zwischen C++/WinRT und C++/CX](/windows/uwp/cpp-and-winrt-apis/interop-winrt-cx)
* [Microsoft Interface Definition Language 3.0 – Referenz](/uwp/midl-3)
* [Umstellen von WRL auf C++/WinRT](/windows/uwp/cpp-and-winrt-apis/move-to-winrt-from-wrl)
* [Verarbeitung von Zeichenfolgen in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/strings)
