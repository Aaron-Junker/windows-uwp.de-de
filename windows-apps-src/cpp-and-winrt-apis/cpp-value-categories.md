---
description: Dieses Thema beschreibt die verschiedenen Kategorien von Werten, die in C++ vorhanden sein. Sie werden ohne Zweifel auch schon von Lvalues und Rvalues, aber auch andere Arten, stehen.
title: Wert Kategorien und Verweise auf diese
ms.date: 08/11/2018
ms.topic: article
keywords: Windows 10, Uwp, Standard, c++, Cpp, Winrt, Projektion, verschieben, Weiterleitung, Wert Kategorien, Move-Semantik, perfekte Weiterleitung, l-Wert, Rvalue, Glvalue, Prvalue, Xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1860f562233ceefa6d9ebb3741378b3265b4c3a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593015"
---
# <a name="value-categories-and-references-to-them"></a>Wert Kategorien und Verweise auf diese
Dieses Thema beschreibt die verschiedenen Kategorien von Werte (und Verweise auf Werte), die in C++ vorhanden sind. Sie werden ohne Zweifel auch schon von *Lvalues* und *Rvalues*, aber Sie können nicht von ihnen vorstellen, in die Bedingungen, die in diesem Thema werden. Und andere Arten von Werten zu.

Jeder Ausdruck in C++ führt einen Wert, der eine der Kategorien in diesem Thema erläuterten gehört. Es gibt Aspekte der Programmiersprache C++, die Facilies und Regeln, die das richtige Verständnis dieser Wert Kategorien und Verweise darauf erfordern. Z. B. das Übernehmen der Adresse eines Werts, der einen Wert zu kopieren, verschieben einen Wert und einen Wert an eine andere Funktion weiterleiten. In diesem Thema nicht alle diese Aspekte ausführlich wechsle, aber es bietet grundlegende Informationen zu versorgen, die von ihnen.

Die Informationen in diesem Thema wird im Hinblick auf Stroustrups Analysis Wert Kategorien zwei unabhängige Eigenschaften der Identitäts- und Movability [Stroustrup 2013] eingeschlossen.

## <a name="an-lvalue-has-identity"></a>Ein Lvalue hat Identität.
Was bedeutet es für einen Wert haben *Identität*? Wenn Sie die Speicheradresse eines Werts (oder können) und sicher sind, und klicken Sie dann den Wert Identität hat. Auf diese Weise möglich mehr als Vergleich den Inhalt der Werte: Sie vergleichen oder von Identität unterscheiden können.

Ein *Lvalue* Identität hat. Es ist jetzt eine Frage der nur historische Interesse, dass "l" in "Lvalue" kurz für "Left" (z. B. die linke Seite einer Zuweisung) ist. In C++ kann ein l-Wert auf der linken Seite angezeigt werden *oder* auf der rechten Seite einer Zuweisung. "l" in "Lvalues", helfen nicht anschließend tatsächlich Ihnen beim verstehen und zu definieren, was sind. Sie müssen nur wissen, dass einen Lvalue nennen wir, der Identität wurde ein Wert ist.

Beispiele für Ausdrücke, die Lvalues: eine benannte Variable oder Konstante; oder eine Funktion, die einen Verweis zurückgibt. Beispiele für Ausdrücke, die *nicht* l-Werte enthalten: einen temporären; oder eine Funktion, die durch einen Wert zurückgibt.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    std::vector<byte> vec{ 99, 98, 97 };
    std::vector<byte>* addr1{ &vec }; // ok: vec is an lvalue.
    int* addr2{ &get_by_ref() }; // ok: get_by_ref() is an lvalue.

    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is not an lvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is not an lvalue.
}
```

Nun, eine Anweisung "true" ist, zwar Lvalues Identität haben dies auch für "XValues". Gehen wir mehr in die stattfindenden ein *Xvalue* wird weiter unten in diesem Thema. Jetzt müssen Sie nur Bedenken Sie, dass es eine Wertkategorie Glvalue ist, für die "generalisiert l-Wert" aufgerufen. Die Übermenge von Glvalues enthält beide Lvalues (auch bekannt als *klassischen Lvalues*) und "XValues". Daher zwar "Identität besitzt ein l-Wert" true ist, der vollständige Satz von Aufgaben, die Identität der Satz von Glvalues, wie in dieser Abbildung dargestellt.

![Ein Lvalue hat Identität.](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Ein Rvalue ist verschiebbar; Es ist kein l-Wert
Aber es Werte, die nicht Glvalues sind sind. Daher sind Werte, die Sie *kann nicht* eine Speicheradresse für abrufen (oder nicht verlässlich es gültig ist). Wir haben gesehen, einige dieser Werte im obigen Codebeispiel. Das hört sich wie ein Nachteil. Aber in der Tat der Vorteil eines Werts, können Sie *verschieben* aus (die im Allgemeinen günstig ist), als Kopie aus (die in der Regel aufwendig ist). Verschieben von einem Wert bedeutet, dass es nicht mehr an der Stelle, die sie verwendet werden. Ist es an der Stelle zugreifen, die zuvor möchten etwas vermieden werden. Eine Beschreibung dafür, wann und *wie* verschieben Sie ein Wert außerhalb des gültigen Bereichs für dieses Thema ist. In diesem Thema, es muss lediglich wissen, dass ein Wert, der verschiebbar ist, als bekannt ist ein *Rvalue* (oder *klassischen Rvalue*).

Der Wert für "R" in "Rvalue" ist die Abkürzung von "Right" (z. B. die rechte Seite einer Zuweisung). Sie können jedoch verwenden Rvalues sowie Verweise auf Rvalues, außerhalb von Zuweisungen. Die "R" in "Rvalues", ist, nicht das, was zu konzentrieren. Sie müssen nur wissen, dass ein Rvalue-Wert nennen wir ein Wert ist, der verschoben wird.

Ein Lvalue ist, ist dagegen verschoben, nicht, wie in dieser Abbildung dargestellt. Ein Lvalue, die verschoben würde die Definition der trotzen *Lvalue*, und es wäre ein unerwartetes Problem für Code, der sehr vernünftigerweise erwartet wird, um weiterhin auf Lvalue zugreifen zu können.

![Ein Rvalue ist verschiebbar; Es ist kein l-Wert](images/is-movable.png)

Einen Lvalue kann nicht verschoben werden. Aber es *ist* eine Art von Glvalue (die Gruppe von Aufgaben mit Identität), die Sie verschieben können&mdash;, wenn Sie wissen, was Sie tun (einschließlich Achten Sie darauf nicht für den Zugriff darauf nach dem Verschieben)&mdash;und das ist die Xvalue. Wir werden diese Idee noch einmal unten noch einmal beim Betrachten der vollständigen Bilds von Wert-Kategorien.

## <a name="rvalue-references-and-reference-binding-rules"></a>Rvalue-Referenzen und Referenz-Bindungsregeln
Dieser Abschnitt enthält die Syntax für einen Verweis auf ein Rvalue-Wert. Wir müssen auf einem anderen Thema in eine erhebliche Erläuterung von verschieben und die Weiterleitung zu warten, aber dies sind Probleme, die behoben werden, indem Sie Rvalue-Referenzen. Bevor wir die Rvalue-Referenzen betrachten, jedoch müssen wir zuerst über klarer sein `T&` &mdash;das, was wir haben zuvor wurden aufrufen einfach "eine Referenz". Es ist wirklich "ein l-Wert (nicht konstant) Reference", bezieht sich auf einen Wert, der der Benutzer des Verweises schreiben kann.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Ein Lvalue-Verweis kann an einen l-Wert, aber nicht auf ein Rvalue-Wert gebunden werden.

Const Lvalue-Verweise sind (`T const&`), beziehen sich auf Objekte, für die der Benutzer des Verweises *kann nicht* schreiben (z. B. eine Konstante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Ein const Lvalue-Verweis kann zu einem Lvalue und Rvalue-Wert binden.

Die Syntax für einen Verweis auf einen Rvalue vom Typ `T` lautet `T&&`. Ein Rvalue-Verweis bezieht sich auf eine verschiebbare Wert&mdash;ein Wert, dessen Inhalt müssen wir nicht beibehalten, nachdem wir es (beispielsweise eine temporäre) verwendet haben. Seit der ganze Sinn von verschieben (wodurch ändern) der Wert gebunden ist auf einen Rvalue-Verweis `const` und `volatile` Qualifizierer (auch bekannt als cv-Qualifizierer) an Rvalue-Verweise gelten nicht.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Ein Rvalue-Verweis bindet an ein Rvalue-Wert. In der Tat in Bezug auf die Auflösung von funktionsüberladungen, Rvalue *bevorzugt* an einen Rvalue-Verweis als auf ein const Lvalue-Verweis gebunden werden muss. Aber ein Rvalue-Verweis kann nicht an einen l-Wert gebunden, da, wie bereits erwähnt habe, ein Rvalue-Verweis auf einen Wert, dessen Inhalt bezieht sich davon, dass wir nicht brauchen ausgegangen wird, um (z. B. der Parameter für ein bewegungskonstruktor) zu erhalten.

Sie können auch übergeben ein Rvalue-Wert, an der ein Argument als Wert erwartet über Kopierkonstruktion (oder über verschiebungskonstruktion, wenn Rvalue ein Xvalue ist).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Einen Glvalue hat Identität; ein Prvalue nicht
In dieser Phase wissen wir, was Identität hat. Und wir wissen, was verschoben ist und was nicht. Aber wir noch nicht noch mit dem Namen des Satz von Werten, die *nicht* Identität aufweisen. Set bekannt ist, als die *Prvalue*, oder *reine Rvalue*.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Ein Lvalue hat Identität; ein Prvalue nicht](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>Das vollständige Bild der Wert-Kategorien
Es bleibt nur um die Informationen und Abbildungen weiter oben in einer einzelnen Überblick zu kombinieren.

![Das vollständige Bild der Wert-Kategorien](images/value-categories.png)

### <a name="glvalue-i"></a>Glvalue (i)
Einen Glvalue (generalisierte Lvalue) verfügt über die Identität.

### <a name="lvalue-im"></a>l-Wert (ich\&\!m)
L-Wert (eine Art von Glvalue) Identität besitzt, aber nicht verschoben. Hierbei handelt es sich um in der Regel Lese-/ schreibwerten, die Sie rund um als Verweis oder als const-Verweis oder als Wert übergeben, wenn das Kopieren von günstig ist. Ein Lvalue kann nicht in einen Rvalue-Verweis gebunden werden.

### <a name="xvalue-im"></a>xValue (ich\&m)
Ein Xvalue (eine Art von Glvalue, sondern auch eine Art von r-Wert) hat Identität und ist ebenfalls verschoben. Dies ist möglicherweise ein Pipelinekarton Lvalue, den Sie sich entschieden haben, verschieben, da das Kopieren teuer ist, und Sie darauf achten, nicht den Zugriff später. Hier ist, wie Sie einen l-Wert in eine Xvalue verwandeln können.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

Im obenstehenden Codebeispiel wird dies nicht getan haben wir etwas noch verschoben. Wir haben soeben ein Xvalue erstellt, durch einen Lvalue in einen unbenannten Rvalue-Verweis umwandeln. Sie können weiterhin anhand des l-Wert namens bezeichnet werden. als ein Xvalue, ist nun aber *kann* der verschoben werden. Die Gründe dafür haben, und welche verschieben tatsächlich aussieht muss auf einem anderen Thema zu warten. Jedoch können Sie das "X" in "Xvalue" Bedeutung "Experten nur" ob es hilft vorstellen. Durch einen l-Wert in eine Xvalue (eine Art von r-Wert) umzuwandeln, wird der Wert kann an einen Rvalue-Verweis gebunden wird.

Hier sind zwei weitere Beispiele für "XValues"&mdash;Aufrufen einer Funktion, die einen unbenannten Rvalue-Verweis zurückgibt und Zugreifen auf einen Member einer Xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>Prvalue (\!ich\&m)
Ein Prvalue (pure Rvalue, eine Art von r-Wert) besitzt keine Identität, aber es kann verschoben werden. Diese temporären Dateien, die in der Regel sind haben, wird das Ergebnis des Aufrufs einer Funktion, die nach Wert oder das Ergebnis der Auswertung beliebiger anderer Ausdrucks, der nicht auf einen Glvalue ist

### <a name="rvalue-m"></a>r-Wert (m)
Ein rvalue-Wert kann verschoben werden. Ein Rvalue *Verweis* verweist immer auf einen r-Wert (ein Wert, dessen Inhalt wird davon ausgegangen, die wir nicht beibehalten müssen).

Aber ein Rvalue-Verweis selbst ein Rvalue ist? Ein *unbenannte* Rvalue-Verweis (z. B. ersetzt die oben aufgeführten Codebeispiele Xvalue) ist ein Xvalue, Ja, es ist also ein Rvalue-Wert. Sie bevorzugt dabei ein Rvalue-Verweis-Funktionsparameter wie z. B. mit der ein bewegungskonstruktor gebunden sein. Im Gegensatz dazu (und vielleicht counter-intuitively), wenn ein Rvalue-Verweis verfügt über einen Namen, der Ausdruck, der mit diesem Namen ist ein Lvalue. Sodass es *kann nicht* an einen Rvalue-Verweis-Parameter gebunden werden. Aber es ist einfach, es dazu&mdash;einfach wieder in einen unbenannten Rvalue-Verweis (eine Xvalue) umzuwandeln.

```cppwinrt
void foo(A&) { ... }
void foo(A&&) { ... }
void bar(A&& a) // a is a named rvalue reference; it's an lvalue.
{
    foo(a); // Calls foo(A&).
    foo(static_cast<A&&>(a)); // Calls foo(A&&).
}
A&& get_by_rvalue_ref() { ... } // This unnamed rvalue reference is an xvalue.
```

### <a name="im"></a>\!i\&\!m
Die Art des Werts, der nicht die Identität haben und nicht verschiebbar ist, die eine Kombination aus, der wir noch nicht diskutiert. Aber wir können, da dieser Kategorie nützlich, sich in der C++-Sprache nicht ignorieren.

## <a name="reference-collapsing-rules"></a>Referenz-collapsing Regeln
Mehrere wie Verweise in einem Ausdruck (ein Lvalue-Verweis auf einen Lvalue-Verweis oder einen Rvalue-Verweis in einen Rvalue-Verweis) beenden eine eine andere Out.

- `A& &` reduziert zu `A&`.
- `A&& &&` reduziert zu `A&&`.

Mehrere im Gegensatz zu verweisen in einem Ausdruck in einen Lvalue-Verweis reduzieren.

- `A& &&` reduziert zu `A&`.
- `A&& &` reduziert zu `A&`.

## <a name="forwarding-references"></a>Weiterleiten von verweisen
In diesem letzten Abschnitt steht im Gegensatz zu Rvalue-Verweise, die wir bereits, mit dem anderen Konzept der besprochen haben ein *Weiterleitung Verweis*.

```cppwinrt
void foo(A&& a) { ... }
```

- `A&&` Rvalue-Verweis ist, wie wir gesehen haben. Const- und volatile gelten nicht für Rvalue-Referenzen.
- `foo` akzeptiert nur Rvalues des Typs **ein**.
- Der Grund Rvalue-Referenzen (wie z. B. `A&&`) vorhanden ist, damit Sie eine Überladung erstellen können, die für den Fall einer temporären (oder anderen r-Wert) übergeben wird, optimiert ist.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` ist eine *Weiterleitung Verweis*. Je nachdem, was Sie übergeben `bar`, Typ **_Ty** möglicherweise Const/nicht-Const unabhängig von Volatile/nicht flüchtigen.
- `bar` akzeptiert alle Lvalue- oder eines Rvalue vom Typ **_Ty**.
- Übergeben einen l-Wert bewirkt, dass die Weiterleitung-Referenz zu `_Ty& &&`, die reduziert auf den "Lvalue"-Verweis `_Ty&`.
- Übergabe ein Rvalue-Wert bewirkt, dass den Verweis Weiterleitung werden `_Ty&& &&`, die in der Rvalue-Verweis reduziert `_Ty&&`.
- Der Grund, die Weiterleitung von verweisen (z. B. `_Ty&&`) vorhanden ist *nicht* für die Optimierung, jedoch werden, was Sie an Sie übergeben und auf transparent und effizient weiterleiten. Sie sind wahrscheinlich einen Verweis für die Weiterleitung konfrontiert werden, nur dann, wenn Sie Bibliothekscode zu schreiben (oder genauer untersuchen)&mdash;z. B. einer Factory-Funktion, die auf Konstruktorargumente weiterleitet.

## <a name="sources"></a>Quellen
* \[Stroustrup 2013\] b Stroustrup: The C++ Programming Language, Fourth Edition. Addison-Wesley. 2013.
