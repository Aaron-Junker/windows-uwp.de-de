---
description: In diesem Thema werden die verschiedenen Kategorien von Werten beschrieben, die in C++ vorhanden sind. Sie haben sicherlich schon von „lvalues“ und „rvalues“ gehört, aber es gibt auch andere Arten.
title: Wertekategorien und zugehörige Verweise
ms.date: 08/11/2018
ms.topic: article
keywords: Windows 10, UWP, Standard, C++, CPP, WinRT, Projizierung, Verschieben, Weiterleiten, Wertekategorien, Verschiebesemantik, perfekte Weiterleitung, lvalue, rvalue, glvalue, prvalue, xvalue
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: a11d7763c33df6733a8dbf78392d27417e7cf18d
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270210"
---
# <a name="value-categories-and-references-to-them"></a>Wertekategorien und zugehörige Verweise
In diesem Thema werden die verschiedenen Kategorien von Werten (und Verweisen auf Werte) beschrieben, die in C++ vorhanden sind. Sicherlich haben Sie bereits von *lvalues* und *rvalues* gehört, der in diesem Artikel erläuterte Zusammenhang ist Ihnen jedoch möglicherweise neu. Außerdem gibt es noch andere Arten von Werten.

Jeder Ausdruck in C++ liefert einen Wert, der einer der in diesem Thema erläuterten Kategorien angehört. Es gibt Aspekte der C++-Sprache, ihrer Merkmale und Regeln, die ein umfassendes Verständnis dieser Wertekategorien sowie der Verweise auf sie voraussetzen. Beispiele hierfür sind das Übernehmen der Adresse eines Werts, das Kopieren eines Werts, das Verschieben eines Werts sowie das Weiterleiten eines Werts an eine andere Funktion. In diesem Thema werden nicht alle diese Aspekte ausführlich erörtert, Sie erhalten jedoch grundlegende Informationen, die ausreichend für ein allgemeines Verständnis sind.

Die Informationen in diesem Thema beziehen sich auf die Stroustrup-Analyse von Wertekategorien anhand der zwei unabhängigen Eigenschaften der Identität und Verschiebbarkeit [Stroustrup, 2013].

## <a name="an-lvalue-has-identity"></a>Ein lvalue weist eine Identität auf.
Was bedeutet es für einen Wert, eine *Identität* zu besitzen? Wenn Sie über die Speicheradresse eines Werts verfügen (oder übernehmen können) und diese sicher verwenden, weist der Wert eine Identität auf. Auf diese Weise können Sie viel mehr tun, als lediglich den Inhalt von Werten vergleichen: Sie können sie anhand der Identität vergleichen und voneinander unterscheiden.

Ein *lvalue* weist eine Identität auf. Heute ist es nur ein „historischer“ Fakt, dass das „l“ in „lvalue“ als Abkürzung für „links“ steht (das heißt, die linke Seite einer Zuweisung). In C++ kann ein lvalue auf der linken *oder* rechten Seite einer Zuweisung stehen. Daher kann vom „l“ in „lvalues“ nicht wirklich abgeleitet werden, worum es sich bei diesen Werten handelt. Sie müssen lediglich wissen, dass ein lvalue ein Wert ist, der eine Identität besitzt.

Beispiele für Ausdrücke, die lvalues darstellen: eine benannte Variable bzw. Konstante und eine Funktion, die einen Verweis zurückgibt. Beispiele für Ausdrücke, die *keine* lvalues sind: ein temporärer Wert oder eine Funktion, deren Rückgabe nach Wert erfolgt.

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

lvalues weisen eine Identität auf, und dasselbe gilt für xvalues. Auf das Wesen eines *xvalue* wird später in diesem Artikel eingegangen. Vorerst soll nur kurz darauf hingewiesen werden, dass es eine Wertekategorie namens „glvalue“ gibt (dies steht für „generalisierter lvalue“). Die Obermenge von glvalues enthält sowohl lvalues (die auch als *klassische lvalues* bezeichnet werden) als auch xvalues. Die Aussage„ein lvalue weist eine Identität auf“ trifft zwar zu, aber die Gesamtheit der Elemente mit einer Identität ist die Menge der glvalues, wie in dieser Abbildung veranschaulicht.

![Ein lvalue weist eine Identität auf.](images/has-identity1.png)

## <a name="an-rvalue-is-movable-an-lvalue-is-not"></a>Ein rvalue ist verschiebbar, während dies für einen lvalue nicht zutrifft.
Es gibt jedoch auch Werte, die keine glvalues sind. Demzufolge gibt es Werte, für die Sie *nicht* die Speicheradresse abrufen können (bzw. bei denen Sie sich nicht darauf verlassen können, dass sie gültig ist). Einige dieser Werte haben wir im obigen Codebeispiel gesehen. Das klingt wie ein Nachteil. Tatsächlich ist ein Wert wie dieser von Vorteil, weil Sie daraus *verschieben* können (was generell kostengünstig ist), und nicht daraus kopieren müssen (was generell teuer ist). Verschieben aus einem Wert bedeutet, dass er sich nicht mehr an der ursprünglichen Stelle befindet. Der Versuch, auf den Wert an seiner ursprünglichen Stelle zuzugreifen, sollte daher vermieden werden. Eine Erörterung, unter welchen Umständen und *wie* ein Wert verschoben wird, sprengt den Rahmen dieses Artikels. Für diesen Artikel müssen wir lediglich wissen, dass ein verschiebbarer Wert als *rvalue* (oder *klassischer rvalue*) bezeichnet wird.

Das „r“ in „rvalue“ steht für „rechts“ (d. h. die rechte Seite einer Zuweisung). Sie können jedoch rvalues und Verweise auf rvalues außerhalb von Zuweisungen verwenden. Wir konzentrieren uns also nicht auf das „r“ in „rvalues“. Sie müssen lediglich wissen, dass ein rvalue ein Wert ist, der verschoben werden kann.

Ein lvalue hingegen kann nicht verschoben werden, wie in dieser Abbildung veranschaulicht. Ein verschobener lvalue würde der Definition eines *lvalue* widersprechen und ein unerwartetes Problem für Code bewirken, in dem sinnvollerweise erwartet wird, dass weiterhin auf den lvalue zugegriffen werden kann.

![Ein rvalue ist verschiebbar, während dies für einen lvalue nicht zutrifft.](images/is-movable.png)

Ein lvalue kann nicht verschoben werden. Es *gibt* jedoch eine Art von glvalue (alle Werte mit einer Identität), der verschoben werden kann &mdash; wenn Sie diesen Vorgang bewusst ausführen (und u. a. darauf achten, nach dem Verschieben nicht mehr darauf zuzugreifen) &mdash; hierbei handelt es sich um den xvalue. Wir kommen auf dieses Konzept im Folgenden noch einmal zurück, wenn wir uns einen Gesamtüberblick über Wertekategorien verschaffen.

## <a name="rvalue-references-and-reference-binding-rules"></a>rvalue-Verweise und Verweis-Bindungsregeln
Dieser Abschnitt enthält die Syntax für einen Verweis auf einen rvalue. Bisher liegt noch kein erschöpfendes Thema zum Verschieben und Weiterleiten vor; hier werden jedoch Probleme behandelt, die durch rvalue-Verweise behandelt werden. Bevor wir rvalue-Verweise betrachten, müssen wir uns eingehender `T&`&mdash;mit dem beschäftigen, was wir eingangs einfach als „Verweis“ bezeichnet haben. Tatsächlich handelt es sich um einen „lvalue (non-const)-Verweis“, der auf einen Wert verweist, in den der Benutzer des Verweises schreiben kann.

```cppwinrt
template<typename T> T& get_by_lvalue_ref() { ... } // Get by lvalue (non-const) reference.
template<typename T> void set_by_lvalue_ref(T&) { ... } // Set by lvalue (non-const) reference.
```

Ein lvalue-Verweis kann an einen lvalue, jedoch nicht an einen rvalue gebunden werden.

Außerdem gibt es lvalue-const-Verweise (`T const&`), die auf Objekte verweisen, in die der Benutzer des Verweises *nicht* schreiben kann (z. B. eine Konstante).

```cppwinrt
template<typename T> T const& get_by_lvalue_cref() { ... } // Get by lvalue const reference.
template<typename T> void set_by_lvalue_cref(T const&) { ... } // Set by lvalue const reference.
```

Ein lvalue-const-Verweis kann an einen lvalue oder an einen rvalue gebunden werden.

Die Syntax für einen Verweis auf einen rvalue vom Typ `T` lautet `T&&`. Ein rvalue-Verweis bezieht sich auf einen verschiebbaren Wert, d. h. einen Wert, dessen Inhalt nach seiner Verwendung nicht beibehalten werden muss (z. B. einen temporären Wert). Da es nur darum geht, aus dem an einen rvalue-Verweis gebundenen Wert zu verschieben (und diesen dadurch zu ändern), gelten die Qualifizierer `const` und `volatile` (die auch als cv-Qualifizierer bezeichnet werden) nicht für rvalue-Verweise.

```cppwinrt
template<typename T> T&& get_by_rvalue_ref() { ... } // Get by rvalue reference.
struct A { A(A&& other) { ... } }; // A move constructor takes an rvalue reference.
```

Ein rvalue-Verweis ist an einen rvalue-Wert gebunden. Tatsächlich wird ein rvalue im Rahmen einer Überladungsauflösung *eher* an einen rvalue-Verweis gebunden, als an einen lvalue-const-Verweis. Ein rvalue-Verweis kann jedoch nicht an einen lvalue gebunden werden, da ein rvalue-Verweis bekanntlich auf einen Wert verweist, für dessen Inhalt angenommen wird, dass er nicht beibehalten werden muss (z. B. der Parameter für einen Verschiebekonstruktor).

Sie können auch einen rvalue übergeben, wenn ein ByVal-Argument erwartet wird; dies erfolgt über einen Kopiervorgang zur Bearbeitung (oder einen Kopiervorgang zur Verschiebung, wenn der rvalue ein xvalue ist).

## <a name="a-glvalue-has-identity-a-prvalue-does-not"></a>Ein glvalue weist eine Identität auf; bei einem prvalue ist dies nicht der Fall
Nun ist uns bekannt, welche Werte eine Identität haben. Außerdem wissen wir, welche Werte verschoben und welche nicht verschoben werden können. Wir haben jedoch noch keinen Namen für die Menge der Werte, die *keine* Identität haben. Diese werden als *prvalue* oder *reiner rvalue* bezeichnet.

```cppwinrt
int& get_by_ref() { ... }
int get_by_val() { ... }

int main()
{
    int* addr3{ &(get_by_ref() + 1) }; // Error: get_by_ref() + 1 is a prvalue.
    int* addr4{ &get_by_val() }; // Error: get_by_val() is a prvalue.
}
```

![Ein lvalue weist eine Identität auf; bei einem prvalue ist dies nicht der Fall](images/has-identity2.png)

## <a name="the-complete-picture-of-value-categories"></a>Kompletter Überblick über die Wertekategorien
Nun müssen wir nur noch die obigen Informationen und Abbildungen zu einem einzigen großen Überblick kombinieren.

![Kompletter Überblick über die Wertekategorien](images/value-categories.png)

### <a name="glvalue-i"></a>glvalue (i)
Ein glvalue (generalisierter lvalue) weist eine Identität auf.

### <a name="lvalue-im"></a>lvalue (i\&\!m)
Ein lvalue (eine Art von glvalue) weist eine Identität auf, kann jedoch nicht verschoben werden. Hierbei handelt es sich in der Regel um Werte mit Lese-/Schreibzugriff, die per Verweis bzw. const-Verweis oder per Wert übergeben werden, wenn Kopiervorgänge kostengünstig sind. Ein lvalue kann nicht an einen rvalue-Verweis gebunden werden.

### <a name="xvalue-im"></a>xvalue (i\&m)
Ein xvalue (eine Art von glvalue, jedoch auch eine Art von rvalue) weist eine Identität auf, kann jedoch auch verschoben werden. Dies kann ein früherer lvalue sein, denn Sie nun verschieben, weil das Kopieren zu teuer ist; Sie müssen nun darauf achten, dass Sie später nicht wieder darauf zugreifen. Im Folgenden wird erläutert, wie Sie einen lvalue in einen xvalue umwandeln können.

```cppwinrt
struct A { ... };
A a; // a is an lvalue...
static_cast<A&&>(a); // ...but this expression is an xvalue.
```

Im obigen Codebeispiel wurde noch nicht verschoben. Wir haben lediglich einen xvalue erstellt, indem ein lvalue in einen unbenannten rvalue-Verweis umgewandelt wurde. Er kann weiterhin anhand seines lvalue-Namens erkannt werden; als xvalue ist es jedoch nun *möglich*, ihn zu verschieben. Die Gründe für diese Vorgehensweise und der tatsächliche Ablauf des Verschiebens werden in einem künftigen Artikel erläutert. Stellen Sie sich einfach vor, dass das „x“ in „xvalue“ für „ausschließlich für eXperten“ steht; dies mag fürs Erste genügen. Durch das Umwandeln eines lvalue in einen xvalue (eine Art von rvalue), kann der erhaltene Wert an einen rvalue-Verweis gebunden werden.

Hier sind zwei weitere Beispiele für xvalues: Aufrufen einer Funktion, die einen unbenannten rvalue-Verweis zurückgibt und Zugreifen auf einen Member eines xvalue.

```cppwinrt
struct A { int m; };
A&& f();
f(); // This expression is an xvalue...
f().m; // ...and so is this.
```

### <a name="prvalue-im"></a>prvalue (\!i\&m)
Ein prvalue (reiner rvalue; eine Art von rvalue) weist keine Identität auf, er kann jedoch verschoben werden. Hierbei handelt es sich in der Regel um temporäre Werte, das Ergebnis des Aufrufs einer Funktion, deren Rückgabe nach Wert erfolgt oder das Ergebnis der Auswertung eines anderen Ausdrucks, der kein glvalue ist,

### <a name="rvalue-m"></a>rvalue (m)
Ein rvalue kann verschoben werden. Ein rvalue-*Verweis* bezieht sich stets auf einen rvalue (einen Wert, für dessen Inhalt angenommen wird, dass er nicht beibehalten werden muss).

Ist aber ein rvalue-Verweis selbst ein rvalue? Ein *unbenannter* rvalue-Verweis (wie die in den obigen xvalue-Codebeispielen veranschaulichten) ist ein Wert. Es handelt sich also tatsächlich um einen rvalue. Er wird vorzugsweise an den Funktionsparameter eines rvalue-Verweises gebunden, z. B. an den eines Verschiebekonstruktors. Umgekehrt (und vielleicht weniger intuitiv) gilt Folgendes: Wenn ein rvalue-Verweis einen Namen aufweist, ist der Ausdruck des betreffenden Namens ein lvalue. Somit kann er *nicht* an den Parameter eines rvalue-Verweises gebunden werden. Dies ist jedoch problemlos möglich. Wandeln Sie ihn einfach wieder in einen unbenannten rvalue-Verweis (einen xvalue) um.

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
Die Art von Wert, die keine Identität aufweist und nicht verschoben werden kann, ist die einzige Kombination, die bisher noch nicht erörtert wurde. Wir können sie jedoch nicht ignorieren, da diese Kategorie ein nützliches Konzept in C++ darstellt.

## <a name="reference-collapsing-rules"></a>Reduzierungsregeln für Verweise
Mehrere gleichartige Verweise in einem Ausdruck (ein lvalue-Verweis auf einen lvalue-Verweis oder ein rvalue-Verweis auf einen rvalue-Verweis) heben einander auf.

- `A& &` wird reduziert zu `A&`.
- `A&& &&` wird reduziert zu `A&&`.

Mehrere unterschiedliche Verweise in einem Ausdruck werden zu einem lvalue-Verweis reduziert.

- `A& &&` wird reduziert zu `A&`.
- `A&& &` wird reduziert zu `A&`.

## <a name="forwarding-references"></a>Weiterleitungsverweise
In diesem letzten Abschnitt werden die bereits erörterten rvalue-Verweise dem abweichenden Konzept eines *Weiterleitungsverweises* gegenübergestellt.

```cppwinrt
void foo(A&& a) { ... }
```

- Wie wir festgestellt haben, ist `A&&` ein rvalue-Verweis. Die Typen const und volatile gelten nicht für rvalue-Verweise.
- `foo` akzeptiert nur rvalues vom Typ **A**.
- rvalue-Verweise (wie `A&&`) gibt es, damit Sie eine Überladung schreiben können, die für den Fall der Übergabe eines temporären Werts (oder sonstigen rvalue) optimiert ist.

```cppwinrt
template <typename _Ty> void bar(_Ty&& ty) { ... }
```

- `_Ty&&` ist ein *Weiterleitungsverweis*. Je nachdem, was Sie an `bar` übergeben, kann der Typ **_Ty** const/non-const sein, unabhängig von volatile/non-volatile.
- `bar` akzeptiert beliebige lvalues und rvalues vom Typ **_Ty**.
- Durch das Übergeben eines lvalue wird der Weiterleitungsverweis zu `_Ty& &&`, wodurch der lvalue-Verweis `_Ty&` reduziert wird.
- Durch das Übergeben eines rvalue wird der Weiterleitungsverweis zu `_Ty&& &&`, wodurch der rvalue-Verweis `_Ty&&` reduziert wird.
- Weiterleitungsverweise (wie `_Ty&&`) gibt es *nicht* aus Optimierungsgründen. Stattdessen sollen sie übergebene Werte annehmen und transparent und effizient weiterleiten. Einen Weiterleitungsverweis treffen Sie wahrscheinlich nur an, wenn Sie Bibliothekscode schreiben (oder eingehender untersuchen), beispielsweise eine Factory-Funktion, mit der die Weiterleitung für Konstruktorargumente erfolgt.

## <a name="sources"></a>Quellen
* \[Stroustrup, 2013\] B. Stroustrup: The C++ Programming Language, Fourth Edition. Addison-Wesley. 2013.
