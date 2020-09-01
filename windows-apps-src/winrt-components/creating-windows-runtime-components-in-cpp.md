---
title: Komponenten für Windows-Runtime mit C++/CX
description: In diesem Artikel wird beschrieben, wie Sie eine Komponente für Windows-Runtime mit C++/CX erstellen&mdash;eine Komponente, die aus einer universellen Windows-App aufgerufen werden kann, die in einer der Windows-Runtime-Sprachen erstellt wurde.
ms.assetid: F7E06AA2-DCEC-427E-BD5D-9CA2A0ED2612
ms.date: 05/14/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2965eb3196f2a19f7d5351ee422013c6c22ba88a
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174304"
---
# <a name="windows-runtime-components-with-ccx"></a>Komponenten für Windows-Runtime mit C++/CX

> [!NOTE]
> Dieses Thema enthält Informationen, die Sie beim Verwalten Ihrer C++/CX-Anwendung unterstützen. Es wird jedoch empfohlen, [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) für neue Anwendungen zu verwenden. C++/WinRT ist eine vollständig standardisierte, moderne C++17-Programmiersprache für Windows-Runtime-APIs (WinRT), die als headerdateibasierte Bibliothek implementiert ist und Ihnen einen erstklassigen Zugriff auf die moderne Windows-API bietet. Informationen zum Erstellen einer Windows-Runtime-Komponente mithilfe von C++/WinRT finden Sie unter [Windows-Runtime-Komponenten mit C++/WinRT](./create-a-windows-runtime-component-in-cppwinrt.md).

In diesem Thema wird gezeigt, wie C++/CX verwendet wird, um eine Windows-Runtime Komponente &mdash; eine Komponente zu erstellen, die von einer universellen Windows-app aufgerufen werden kann, die mit einer beliebigen Windows-Runtime Sprache (c#, Visual Basic, C++ oder JavaScript) erstellt wurde.

Es gibt mehrere Gründe, eine Windows-Runtime Komponente in C++ zu entwickeln.
- Um die Leistungsvorteile von C++ für komplexe oder rechenintensive Vorgänge zu übernehmen.
- Um Code wiederzuverwenden, der bereits geschrieben und getestet wurde.

Wenn Sie eine Projektmappe erstellen, die ein JavaScript- oder .NET-Projekt und ein Komponentenprojekt für Windows-Runtime enthält, werden die JavaScript-Projektdateien und die kompilierte DLL in ein Paket zusammengeführt, das Sie lokal im Simulator oder remote auf einem verbundenen Gerät debuggen können. Sie können auch nur das Komponentenprojekt als Erweiterungs-SDK verteilen. Weitere Informationen finden Sie unter [Erstellen eines Software Development Kits](/visualstudio/extensibility/creating-a-software-development-kit?view=vs-2015).

Wenn Sie die C++/CX-Komponente codieren, verwenden Sie im Allgemeinen die reguläre C++-Bibliothek und integrierte Typen, außer an der Grenze der abstrakten binären Schnittstelle (ABI), in der Sie Daten an den Code in einem anderen winmd-Paket übergeben. Verwenden Sie dort Windows-Runtime Typen und die spezielle Syntax, die C++/CX zum Erstellen und Bearbeiten dieser Typen unterstützt. Verwenden Sie außerdem im C++/CX-Code Typen wie Delegat und Ereignis, um Ereignisse zu implementieren, die von der Komponente ausgelöst und in JavaScript, Visual Basic, C++ oder c# verarbeitet werden können. Weitere Informationen zur C++/CX-Syntax finden Sie unter [Visual C++-Sprachreferenz (C++/CX)](/cpp/cppcx/visual-c-language-reference-c-cx).

## <a name="casing-and-naming-rules"></a>Regeln für die Groß-/Kleinschreibung und die Benennung

### <a name="javascript"></a>JavaScript
In JavaScript muss die Groß-/Kleinschreibung beachtet werden. Daher müssen Sie die folgenden Konventionen zur Groß-/Kleinschreibung einhalten:

-   Verwenden Sie die C++-Schreibweise, wenn Sie auf C++-Namespaces und -Klassen verweisen.
-   Verwenden Sie beim Aufrufen von Methoden die Kamelschreibweise, auch wenn der Methodenname in C++ großgeschrieben wird. Beispielsweise muss die C++-Methode „GetDate()” aus JavaScript mit „getDate()” aufgerufen werden.
-   Ein aktivierbarer Klassenname und Namespacename darf keine UNICODE-Zeichen enthalten.

### <a name="net"></a>.NET
Die .NET-Sprachen folgen ihren üblichen Regeln für die Groß-und Kleinschreibung.

## <a name="instantiating-the-object"></a>Instanziieren des Objekts
Nur Windows-Runtime-Typen können über die ABI-Grenze hinweg übergeben werden. Der Compiler löst einen Fehler aus, wenn die Komponente über einen Typ wie „std::wstring” als Rückgabetyp oder Parameter in einer öffentlichen Methode verfügt. Die integrierten Typen der Visual C++ Component Extensions (C++/CX) enthalten die üblichen skalare, wie z. b. int und Double, und auch Ihre typedef-Entsprechungen Int32, float64 usw. Weitere Informationen finden Sie unter [Typsystem (C++/CX)](/cpp/cppcx/type-system-c-cx).

```cpp
// ref class definition in C++
public ref class SampleRefClass sealed
{
    // Class members...

    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }

};
```

```javascript
//Instantiation in JavaScript (requires "Add reference > Project reference")
var nativeObject = new CppComponent.SampleRefClass();
```

```csharp
//Call a method and display result in a XAML TextBlock
var num = nativeObject.LogCalc(21.5);
ResultText.Text = num.ToString();
```

## <a name="ccx-built-in-types-library-types-and-windows-runtime-types"></a>C++/CX integrierte Typen, Bibliothekstypen und Windows-Runtime Typen
Eine aktivierbare Klassen (auch als Verweisklasse bezeichnet) kann in einer anderen Sprache, wie z. B. JavaScript, C# oder Visual Basic, instanziiert werden. Um von einer anderen Sprache verwendet werden zu können, muss eine Komponente mindestens eine aktivierbare Klasse enthalten.

Eine Komponente für Windows-Runtime kann mehrere öffentliche aktivierbare Klassen sowie zusätzliche Klassen enthalten, die der Komponente nur intern bekannt sind. Wenden Sie das [webhosthidden](/uwp/api/windows.foundation.metadata.webhosthiddenattribute) -Attribut auf C++/CX Types an, die nicht für JavaScript sichtbar sind.

Alle öffentliche Klassen müssen sich im selben Stammnamespace befinden, der denselben Namen wie die Metadatendatei der Komponente hat. Zum Beispiel kann eine Klasse namens A.B.C.MyClass nur instanziiert werden, wenn sie in einer Metadatendatei definiert ist, die A.winmd oder A.B.winmd oder A.B.C.winmd heißt. Der Name der DLL muss nicht mit dem Namen der WINMD-Datei übereinstimmen.

Clientcode erstellt eine Instanz der Komponente mit dem Schlüsselwort **new** (**New** in Visual Basic) so wie für jede andere Klasse auch.

Eine aktivierbare Klasse muss als **public ref class sealed** deklariert werden. Das Schlüsselwort **ref class** teilt dem Compiler mit, die Klasse als einen mit der Windows-Runtime kompatiblen Typ zu erstellen, und das Schlüsselwort „sealed” gibt an, dass die Klasse nicht vererbt werden kann. Die Windows-Runtime unterstützt derzeit kein generalisiertes Vererbungsmodell; ein beschränktes Vererbungsmodell unterstützt die Erstellung von benutzerdefinierten XAML-Steuerelementen. Weitere Informationen finden Sie unter [Verweisklassen und Strukturen (C++/CX)](/cpp/cppcx/ref-classes-and-structs-c-cx).

Bei C++/CX werden alle numerischen primitive im Standard Namespace definiert. Der [Platform](/cpp/cppcx/platform-namespace-c-cx) -Namespace enthält C++/CX-Klassen, die für das Windows-Runtime Type-systemspezifisch sind. Dazu gehören die Klassen [Platform:: String](/cpp/cppcx/platform-string-class) und [Platform::Object](/cpp/cppcx/platform-object-class). Die konkreten Sammlungstypen, wie die Klassen [Platform::Collections::Map](/cpp/cppcx/platform-collections-map-class) und [Platform::Collections::Vector](/cpp/cppcx/platform-collections-vector-class), sind im Namespace [Platform::Collections](/cpp/cppcx/platform-collections-namespace) definiert. Die öffentlichen Schnittstellen, die diese Typen implementieren, sind im [Windows::Foundation::Collections-Namespace (C++/CX)](/cpp/cppcx/windows-foundation-collections-namespace-c-cx) definiert. Diese Schnittstellentypen werden von JavaScript, C# und Visual Basic verwendet. Weitere Informationen finden Sie unter [Typsystem (C++/CX)](/cpp/cppcx/type-system-c-cx).

## <a name="method-that-returns-a-value-of-built-in-type"></a>Methode, die einen Wert mit einem integrierten Typ zurückgibt
```cpp
    // #include <valarray>
public:
    double LogCalc(double input)
    {
        // Use C++ standard library as usual.
        return std::log(input);
    }
```

```javascript
//Call a method
var nativeObject = new CppComponent.SampleRefClass;
var num = nativeObject.logCalc(21.5);
document.getElementById('P2').innerHTML = num;
```

## <a name="method-that-returns-a-custom-value-struct"></a>Methode, die eine benutzerdefinierte Wertstruktur zurückgibt
```cpp
namespace CppComponent
{
    // Custom struct
    public value struct PlayerData
    {
        Platform::String^ Name;
        int Number;
        double ScoringAverage;
    };

    public ref class Player sealed
    {
    private:
        PlayerData m_player;
    public:
        property PlayerData PlayerStats
        {
            PlayerData get(){ return m_player; }
            void set(PlayerData data) {m_player = data;}
        }
    };
}
```

Um benutzerdefinierte Wert Strukturen über die ABI zu übergeben, definieren Sie ein JavaScript-Objekt, das dieselben Member wie die Wert Struktur hat, die in C++ definiert ist/CX. Sie können dieses Objekt dann als Argument an eine C++/CX-Methode übergeben, sodass das Objekt implizit in den C++/CX-Typ konvertiert wird.

```javascript
// Get and set the value struct
function GetAndSetPlayerData() {
    // Create an object to pass to C++
    var myData =
        { name: "Bob Homer", number: 12, scoringAverage: .357 };
    var nativeObject = new CppComponent.Player();
    nativeObject.playerStats = myData;

    // Retrieve C++ value struct into new JavaScript object
    var myData2 = nativeObject.playerStats;
    document.getElementById('P3').innerHTML = myData.name + " , " + myData.number + " , " + myData.scoringAverage.toPrecision(3);
}
```

Ein anderer Ansatz besteht darin, eine Klasse zu definieren, die „IPropertySet” implementiert (nicht dargestellt).

In den .NET-Sprachen erstellen Sie einfach eine Variable des-Typs, der in der C++/CX-Komponente definiert ist.

```csharp
private void GetAndSetPlayerData()
{
    // Create a ref class
    var player = new CppComponent.Player();

    // Create a variable of a value struct
    // type that is defined in C++
    CppComponent.PlayerData myPlayer;
    myPlayer.Name = "Babe Ruth";
    myPlayer.Number = 12;
    myPlayer.ScoringAverage = .398;

    // Set the property
    player.PlayerStats = myPlayer;

    // Get the property and store it in a new variable
    CppComponent.PlayerData myPlayer2 = player.PlayerStats;
    ResultText.Text += myPlayer.Name + " , " + myPlayer.Number.ToString() +
        " , " + myPlayer.ScoringAverage.ToString();
}
```

## <a name="overloaded-methods"></a>Überladene Methoden
Eine C++/CX Public-Verweis Klasse kann überladene Methoden enthalten, aber JavaScript bietet eingeschränkte Möglichkeiten, überladene Methoden zu unterscheiden. Beispielsweise kann JavaScript den Unterschied zwischen diesen Signaturen erkennen:

```cpp
public ref class NumberClass sealed
{
public:
    int GetNumber(int i);
    int GetNumber(int i, Platform::String^ str);
    double GetNumber(int i, MyData^ d);
};
```

Aber den Unterschied zwischen diesen nicht:

```cpp
int GetNumber(int i);
double GetNumber(double d);
```

In mehrdeutigen Fällen können Sie sicherstellen, dass JavaScript immer eine bestimmte Überladung aufruft, indem Sie der Methodensignatur in der Headerdatei das [Windows::Foundation::Metadata::DefaultOverload](/uwp/api/windows.foundation.metadata.defaultoverloadattribute)-Attribut hinzufügen.

Dieser JavaScript-Code ruft immer die attributierte Überladung auf:

```javascript
var nativeObject = new CppComponent.NumberClass();
var num = nativeObject.getNumber(9);
document.getElementById('P4').innerHTML = num;
```

## <a name="net"></a>.NET
Die .NET-Sprachen erkennen über Ladungen in einer C++/CX ref-Klasse genau wie in jeder .NET-Klasse.

## <a name="datetime"></a>Datetime
In der Windows-Runtime ist ein [Windows::Foundation::DateTime](/uwp/api/windows.foundation.datetime)-Objekt einfach eine 64-Bit-Ganzzahl mit Vorzeichen, die die Anzahl der 100-Nanosekunden-Intervalle vor oder nach dem 1. Januar 1601 darstellt. Es gibt keine Methoden für ein Windows:Foundation::DateTime-Objekt. Stattdessen projiziert jede Sprache den DateTime-Wert in der nativen Weise in diese Sprache: das Date-Objekt in JavaScript und die System. DateTime-und System. DateTimeOffset-Typen in .net.

```cpp
public  ref class MyDateClass sealed
{
public:
    property Windows::Foundation::DateTime TimeStamp;
    void SetTime(Windows::Foundation::DateTime dt)
    {
        auto cal = ref new Windows::Globalization::Calendar();
        cal->SetDateTime(dt);
        TimeStamp = cal->GetDateTime(); // or TimeStamp = dt;
    }
};
```

Wenn Sie einen DateTime-Wert von C++/CX an JavaScript übergeben, akzeptiert JavaScript ihn als Datums Objekt und zeigt ihn standardmäßig als lange Datums Zeichenfolge an.

```javascript
function SetAndGetDate() {
    var nativeObject = new CppComponent.MyDateClass();

    var myDate = new Date(1956, 4, 21);
    nativeObject.setTime(myDate);

    var myDate2 = nativeObject.timeStamp;

    //prints long form date string
    document.getElementById('P5').innerHTML = myDate2;

}
```

Wenn eine .NET-Sprache einen System. DateTime-Wert an eine C++/CX-Komponente übergibt, akzeptiert die Methode Sie als Windows:: Foundation::D atetime. Wenn die Komponente eine Windows:: Foundation::D atetime an eine .NET-Methode übergibt, akzeptiert die Framework-Methode Sie als DateTimeOffset.

```csharp
private void DateTimeExample()
{
    // Pass a System.DateTime to a C++ method
    // that takes a Windows::Foundation::DateTime
    DateTime dt = DateTime.Now;
    var nativeObject = new CppComponent.MyDateClass();
    nativeObject.SetTime(dt);

    // Retrieve a Windows::Foundation::DateTime as a
    // System.DateTimeOffset
    DateTimeOffset myDate = nativeObject.TimeStamp;

    // Print the long-form date string
    ResultText.Text += myDate.ToString();
}
```

## <a name="collections-and-arrays"></a>Sammlungen und Arrays
Sammlungen werden immer über die ABI-Grenze hinweg als Handles an Windows-Runtime-Typen wie z. B. Windows::Foundation::Collections::IVector^ und Windows::Foundation::Collections::IMap^ übergeben. Wenn Sie beispielsweise ein Handle für Platform::Collections::Map zurückzugeben, wird er implizit in Windows::Foundation::Collections::IMap^ konvertiert. Die Sammlungs Schnittstellen werden in einem Namespace definiert, der von den C++/CX-Klassen getrennt ist, die die konkreten Implementierungen bereitstellen. JavaScript und .NET-Sprachen nutzen die Schnittstellen. Weitere Informationen finden Sie unter [Sammlungen (C++/CX)](/cpp/cppcx/collections-c-cx) und [Array und WriteOnlyArray (C++/CX)](/cpp/cppcx/array-and-writeonlyarray-c-cx).

## <a name="passing-ivector"></a>Übergeben von „IVector“
```cpp
// Windows::Foundation::Collections::IVector across the ABI.
//#include <algorithm>
//#include <collection.h>
Windows::Foundation::Collections::IVector<int>^ SortVector(Windows::Foundation::Collections::IVector<int>^ vec)
{
    std::sort(begin(vec), end(vec));
    return vec;
}
```

```javascript
var nativeObject = new CppComponent.CollectionExample();
// Call the method to sort an integer array
var inVector = [14, 12, 45, 89, 23];
var outVector = nativeObject.sortVector(inVector);
var result = "Sorted vector to array:";
for (var i = 0; i < outVector.length; i++)
{
    outVector[i];
    result += outVector[i].toString() + ",";
}
document.getElementById('P6').innerHTML = result;
```

In den .NET-Sprachen wird „IVector&lt;T&gt; ” als „IList&lt;T&gt;” betrachtet.

```csharp
private void SortListItems()
{
    IList<int> myList = new List<int>();
    myList.Add(5);
    myList.Add(9);
    myList.Add(17);
    myList.Add(2);

    var nativeObject = new CppComponent.CollectionExample();
    IList<int> mySortedList = nativeObject.SortVector(myList);

    foreach (var item in mySortedList)
    {
        ResultText.Text += " " + item.ToString();
    }
}
```

## <a name="passing-imap"></a>Übergeben von „IMap“
```cpp
// #include <map>
//#include <collection.h>
Windows::Foundation::Collections::IMap<int, Platform::String^> ^GetMap(void)
{    
    Windows::Foundation::Collections::IMap<int, Platform::String^> ^ret =
        ref new Platform::Collections::Map<int, Platform::String^>;
    ret->Insert(1, "One ");
    ret->Insert(2, "Two ");
    ret->Insert(3, "Three ");
    ret->Insert(4, "Four ");
    ret->Insert(5, "Five ");
    return ret;
}
```

```javascript
// Call the method to get the map
var outputMap = nativeObject.getMap();
var mStr = "Map result:" + outputMap.lookup(1) + outputMap.lookup(2)
    + outputMap.lookup(3) + outputMap.lookup(4) + outputMap.lookup(5);
document.getElementById('P7').innerHTML = mStr;
```

In den .NET-Sprachen wird „IMap” als „IDictionary&lt;K, V&gt;” betrachtet.

```csharp
private void GetDictionary()
{
    var nativeObject = new CppComponent.CollectionExample();
    IDictionary<int, string> d = nativeObject.GetMap();
    ResultText.Text += d[2].ToString();
}
```

## <a name="properties"></a>Eigenschaften
Eine öffentliche Verweis Klasse in C++/CX-Komponenten Erweiterungen machen öffentliche Datenmember mithilfe des Property-Schlüssel Worts als Eigenschaften verfügbar. Das Konzept ist mit .net-Eigenschaften identisch. Eine triviale Eigenschaft ähnelt einem Datenmember, da ihre Funktionalität implizit ist. Eine nicht triviale Eigenschaft verfügt über explizite Get- und Set-Accessoren und über eine benannte private Variable, die der „Sicherungsspeicher" für den Wert ist. In diesem Beispiel ist die private Member-Variable \_ propertyavalue der Sicherungs Speicher für PropertyA. Eine Eigenschaft kann ein Ereignis auslösen, wenn sich ihr Wert ändert, und eine Client-App kann sich für den Empfang dieses Ereignisses registrieren.

```cpp
//Properties
public delegate void PropertyChangedHandler(Platform::Object^ sender, int arg);
public ref class PropertyExample  sealed
{
public:
    PropertyExample(){}

    // Event that is fired when PropertyA changes
    event PropertyChangedHandler^ PropertyChangedEvent;

    // Property that has custom setter/getter
    property int PropertyA
    {
        int get() { return m_propertyAValue; }
        void set(int propertyAValue)
        {
            if (propertyAValue != m_propertyAValue)
            {
                m_propertyAValue = propertyAValue;
                // Fire event. (See event example below.)
                PropertyChangedEvent(this, propertyAValue);
            }
        }
    }

    // Trivial get/set property that has a compiler-generated backing store.
    property Platform::String^ PropertyB;

private:
    // Backing store for propertyA.
    int m_propertyAValue;
};
```

```javascript
var nativeObject = new CppComponent.PropertyExample();
var propValue = nativeObject.propertyA;
document.getElementById('P8').innerHTML = propValue;

//Set the string property
nativeObject.propertyB = "What is the meaning of the universe?";
document.getElementById('P9').innerHTML += nativeObject.propertyB;
```

Die .NET-Sprachen greifen auf Eigenschaften eines systemeigenen C++/CX-Objekts genau wie auf einem .NET-Objekt zu.

```csharp
private void GetAProperty()
{
    // Get the value of the integer property
    // Instantiate the C++ object
    var obj = new CppComponent.PropertyExample();

    // Get an integer property
    var propValue = obj.PropertyA;
    ResultText.Text += propValue.ToString();

    // Set a string property
    obj.PropertyB = " What is the meaning of the universe?";
    ResultText.Text += obj.PropertyB;

}
```

## <a name="delegates-and-events"></a>Delegaten und Ereignisse
Ein Delegat ist ein Windows-Runtime-Typ, der ein Funktionsobjekt darstellt. Sie können mit Delegaten in Verbindung mit Ereignissen, Rückrufen und asynchronen Methodenaufrufen eine Aktion festlegen, die zu einem späteren Zeitpunkt ausgeführt werden soll. Wie ein Funktionsobjekt bietet der Delegat Typsicherheit, indem dem Compiler ermöglicht wird, den Rückgabetyp und die Parametertypen der Funktion zu überprüfen. Die Deklaration eines Delegaten ähnelt einer Funktionssignatur, die Implementierung ähnelt einer Klassendefinition und der Aufruf ähnelt einen Funktionsaufruf.

## <a name="adding-an-event-listener"></a>Hinzufügen eines Ereignislisteners
Mit dem Schlüsselwort „event” können Sie einen öffentlichen Member eines bestimmten Delegattyps deklarieren. Clientcode abonniert das Ereignis anhand der Standardmechanismen, die die jeweilige Sprache bereitstellt.

```cpp
public:
    event SomeHandler^ someEvent;
```

In diesem Beispiel wird der gleiche C++-Code verwendet, wie im vorherigen Abschnitt „Eigenschaften".

```javascript
function Button_Click() {
    var nativeObj = new CppComponent.PropertyExample();
    // Define an event handler method
    var singlecasthandler = function (ev) {
        document.getElementById('P10').innerHTML = "The button was clicked and the value is " + ev;
    };

    // Subscribe to the event
    nativeObj.onpropertychangedevent = singlecasthandler;

    // Set the value of the property and fire the event
    var propValue = 21;
    nativeObj.propertyA = 2 * propValue;

}
```

In den .NET-Sprachen ist das Abonnieren eines Ereignisses in einer C++-Komponente das gleiche wie das Abonnieren eines Ereignisses in einer .NET-Klasse:

```csharp
//Subscribe to event and call method that causes it to be fired.
private void TestMethod()
{
    var objWithEvent = new CppComponent.PropertyExample();
    objWithEvent.PropertyChangedEvent += objWithEvent_PropertyChangedEvent;

    objWithEvent.PropertyA = 42;
}

//Event handler method
private void objWithEvent_PropertyChangedEvent(object __param0, int __param1)
{
    ResultText.Text = "the event was fired and the result is " +
         __param1.ToString();
}
```

## <a name="adding-multiple-event-listeners-for-one-event"></a>Hinzufügen mehrere Ereignislistener für ein Ereignis
JavaScript verfügt über eine addEventListener-Methode, mit der mehrere Handler ein einzelnes Ereignis abonnieren können.

```cpp
public delegate void SomeHandler(Platform::String^ str);

public ref class LangSample sealed
{
public:
    event SomeHandler^ someEvent;
    property Platform::String^ PropertyA;

    // Method that fires an event
    void FireEvent(Platform::String^ str)
    {
        someEvent(Platform::String::Concat(str, PropertyA->ToString()));
    }
    //...
};
```

```javascript
// Add two event handlers
var multicast1 = function (ev) {
    document.getElementById('P11').innerHTML = "Handler 1: " + ev.target;
};
var multicast2 = function (ev) {
    document.getElementById('P12').innerHTML = "Handler 2: " + ev.target;
};

var nativeObject = new CppComponent.LangSample();
//Subscribe to the same event
nativeObject.addEventListener("someevent", multicast1);
nativeObject.addEventListener("someevent", multicast2);

nativeObject.propertyA = "42";

// This method should fire an event
nativeObject.fireEvent("The answer is ");
```

In C# kann eine beliebige Anzahl von Ereignishandlern mit dem Operator += das Ereignis abonnieren, wie im vorherigen Beispiel gezeigt.

## <a name="enums"></a>Enumerationen
Eine Windows-Runtime Enumeration in C++/CX wird mithilfe der öffentlichen Klassen Enumeration deklariert. Sie ähnelt einer Bereichs bezogenen Aufzählung in Standard-C++.

```cpp
public enum class Direction {North, South, East, West};

public ref class EnumExampleClass sealed
{
public:
    property Direction CurrentDirection
    {
        Direction  get(){return m_direction; }
    }

private:
    Direction m_direction;
};
```

Enumerationswerte werden zwischen C++/CX und JavaScript als ganze Zahlen übermittelt. Sie können optional ein JavaScript-Objekt deklarieren, das die gleichen benannten Werte wie die C++/CX-Enumeration enthält, und es wie folgt verwenden.

```javascript
var Direction = { 0: "North", 1: "South", 2: "East", 3: "West" };
//. . .

var nativeObject = new CppComponent.EnumExampleClass();
var curDirection = nativeObject.currentDirection;
document.getElementById('P13').innerHTML =
Direction[curDirection];
```

C# und Visual Basic bieten Sprachunterstützung für Enumerationen. Diese Sprachen sehen eine öffentliche Enumerationsklasse von C++, genauso wie eine .net-Enumeration.

## <a name="asynchronous-methods"></a>Asynchrone Methoden
Verwenden Sie die [task-Klasse (Concurrency Runtime)](/cpp/parallel/concrt/reference/task-class), um asynchrone Methoden zu nutzen, die von anderen Windows-Runtime-Objekten verfügbar gemacht werden. Weitere Informationen finden Sie unter [Aufgabenparallelität (Concurrency Runtime)](/cpp/parallel/concrt/task-parallelism-concurrency-runtime).

Verwenden Sie zum Implementieren von asynchronen Methoden in C++/CX die [Create \_ Async](/cpp/parallel/concrt/reference/concurrency-namespace-functions?view=vs-2017) -Funktion, die in "ppltasks. h" definiert ist. Weitere Informationen finden Sie unter [Erstellen von asynchronen Vorgängen in C++/CX für UWP-apps](/cpp/parallel/concrt/creating-asynchronous-operations-in-cpp-for-windows-store-apps). Ein Beispiel finden Sie unter Exemplarische Vorgehensweise: [Erstellen einer C++ Windows-Runtime/CX-Komponente und Aufrufen dieser Komponente über JavaScript oder c#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md). Die .NET-Sprachen verwenden asynchrone C++/CX-Methoden genauso wie jede beliebige asynchrone Methode, die in .net definiert ist.

## <a name="exceptions"></a>Ausnahmen
Sie können jeden Ausnahmetyp auslösen, der von der Windows-Runtime definiert ist. Sie können keine benutzerdefinierten Typen von einem Windows-Runtime-Ausnahmetyp ableiten. Allerdings können Sie eine COMException auslösen und ein benutzerdefiniertes HRESULT bereitstellen, auf das der Code, der die Ausnahme abfängt, zugreifen kann. Es gibt keine Möglichkeit, eine benutzerdefinierte Meldung in einer COMException anzugeben.

## <a name="debugging-tips"></a>Tipps zum Debuggen
Beim Debuggen einer JavaScript-Lösung mit einer Komponenten-DLL können Sie den Debugger so einstellen, dass er das schrittweise Ausführen von Skripts oder das schrittweise Ausführen von systemeigenem Code in der Komponente, aber nicht beides gleichzeitig ermöglicht. Um die Einstellung zu ändern, wählen Sie im Projektmappen-Explorer den JavaScript-Projektknoten aus und dann „Eigenschaften”, „Debuggen”, „Debuggertyp”.

Achten Sie darauf, entsprechende Funktionen im Paket-Designer auszuwählen. Wenn Sie zum Beispiel eine Bilddatei in der Bildbibliothek des Benutzers mit den Windows-Runtime-APIs öffnen möchten, müssen Sie das Kontrollkästchen „Bildbibliothek” im Bereich „Funktionen” des Manifest-Designers aktivieren.

Wenn der JavaScript-Code die öffentlichen Eigenschaften oder Methoden in der Komponente nicht zu erkennen scheint, stellen Sie sicher, dass Sie in JavaScript die Kamelschreibweise verwenden. Beispielsweise muss die logcalc C++/CX-Methode als logcalc in JavaScript referenziert werden.

Wenn Sie ein C++ Windows-Runtime/CX-Komponenten Projekt aus einer Projekt Mappe entfernen, müssen Sie auch den Projekt Verweis manuell aus dem JavaScript-Projekt entfernen. Andernfalls werden nachfolgende Debug- oder Buildvorgänge verhindert. Bei Bedarf können Sie dann einen Assemblyverweis zur DLL-Datei hinzufügen.

## <a name="related-topics"></a>Zugehörige Themen
* [Exemplarische Vorgehensweise zum Erstellen einer Komponente für Windows-Runtime in C++/CX und Aufrufen der Komponente über JavaScript oder C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md)