---
description: Dient zum eindeutigen Identifizieren von Objektelementen für den Zugriff auf das instanziierte Objekt aus CodeBehind- oder allgemeinem Code.
title: xName-Attribut
ms.assetid: 4FF1F3ED-903A-4305-B2BD-DCD29E0C9E6D
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 187a937c82c83f4653e58e7699a21051d03b09e7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372304"
---
# <a name="xname-attribute"></a>x:Name-Attribut


Dient zum eindeutigen Identifizieren von Objektelementen für den Zugriff auf das instanziierte Objekt aus CodeBehind- oder allgemeinem Code. Nach Anwendung auf ein zugrunde liegendes Programmiermodell kann **x:Name** als Äquivalent der Variablen mit einem Objektverweis betrachtet werden (gemäß Rückgabe von einem Konstruktor).

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object x:Name="XAMLNameValue".../>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | Beschreibung |
|------|-------------|
| XAMLNameValue | Eine Zeichenfolge, die den Einschränkungen der XamlName-Grammatik entspricht. |

##  <a name="xamlname-grammar"></a>XamlName-Grammatik

Im Anschluss finden Sie die maßgebende Grammatik für eine Zeichenfolge, die in dieser XAML-Implementierung als Schlüssel verwendet wird:

``` syntax
XamlName ::= NameStartChar (NameChar)*
NameStartChar ::= LetterCharacter | '_'
NameChar ::= NameStartChar | DecimalDigit
LetterCharacter ::= ('a'-'z') | ('A'-'Z')
DecimalDigit ::= '0'-'9'
CombiningCharacter::= none
```

-   Zeichen sind eingeschränkt, auf den niedrigeren ASCII-Bereich und genauer gesagt mit lateinischen Alphabet in Großbuchstaben und Kleinbuchstaben, Ziffern und Unterstriche (\_) Zeichen.
-   Der Unicode-Zeichenbereich wird nicht unterstützt.
-   Ein Name darf nicht mit einer Ziffer beginnen. Bei einigen Implementierungen Tool einen Unterstrich voran (\_) in eine Zeichenfolge, wenn der Benutzer eine Ziffer als das erste Zeichen oder das Tool automatisch bereitstellt **X: Name** Werte basierend auf andere Werte, die Ziffern enthalten.

## <a name="remarks"></a>Hinweise

Das angegebene **x:Name**-Attribut wird zum Namen eines Felds, das bei der XAML-Verarbeitung im zugrunde liegenden Code erstellt wird. Dieses Feld enthält einen Verweis auf das Objekt. Die Erstellung dieses Felds erfolgt im Rahmen der MSBuild-Zielschritte. Im Rahmen dieser Schritte erfolgt auch der Beitritt zu den partiellen Klassen für eine XAML-Datei und das zugehörige CodeBehind. Dieses Verhalten ist nicht zwingend XAML-bedingt. Ausschlaggebend ist vielmehr die jeweilige Implementierung, die von der UWP-Programmierung (Universelle Windows-Plattform) für XAML angewendet wird, um **x:Name** in den entsprechenden Programmier- und Anwendungsmodellen zu verwenden.

Jedes definierte **x:Name**-Attribut muss innerhalb eines XAML-Namescopes eindeutig sein. Im Allgemeinen wird ein XAML-Namescope auf der Stammelementebene einer geladenen Seite definiert und enthält alle Elemente unter diesem Element auf einer einzelnen XAML-Seite. Zusätzliche XAML-Namescopes werden von beliebigen, ebenfalls auf dieser Seite definierten Steuerelement- oder Datenvorlagen definiert. Zur Laufzeit wird ein weiterer XAML-Namescope für den Stamm der aus einer angewendeten Steuerelementvorlage erstellten Objektstruktur und auch von Objektstrukturen, die aus einem Aufruf von [**XamlReader.Load**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.markup.xamlreader.load) erstellt werden, definiert. Weitere Informationen finden Sie unter [XAML-Namescopes](xaml-namescopes.md).

Entwurfstools erzeugen oft automatisch **x:Name**-Werte für Elemente, wenn diese auf der Entwurfsoberfläche eingeführt werden. Welches Schema für die automatische Generierung verwendet wird, hängt zwar davon ab, welchen Designer Sie verwenden, ein häufig verwendetes Schema ist jedoch die Generierung einer Zeichenfolge, die mit dem zugrunde liegenden Klassennamen des Elements beginnt und im Anschluss daran mit einer laufenden ganzen Zahl versehen wird. Wenn Sie also beispielsweise das erste [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)-Element im Designer einführen, besitzt dieses Element in XAML unter Umständen für das **x:Name**-Attribut den Wert „Button1“.

**x:Name** kann nicht in der XAML-Eigenschaftselementsyntax oder in Code mit [**SetValue**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.dependencyobject.setvalue) verwendet werden. **x:Name** kann nur über die XAML-Attributsyntax für Elemente festgelegt werden.

**Beachten Sie**  speziell für C++ / CX-apps, ein dahinter liegendes Feld für eine **X: Name** Verweis wird nicht für das Stammelement einer XAML-Datei oder eine Seite erstellt. Wenn Sie in C++-CodeBehind auf das Stammobjekt verweisen müssen, verwenden Sie andere APIs oder eine Strukturtraversierung. Sie können beispielsweise erst [**FindName**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.findname) für ein bekanntes benanntes Unterelement und anschließend [**Parent**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.parent) aufrufen.

### <a name="xname-and-other-name-properties"></a>x:Name und andere Name-Eigenschaften

Einige in UWP-XAML verwendete Typen verfügen auch über die Eigenschaft **Name**. Beispiele: [**FrameworkElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.name) und [**TextElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.documents.textelement.name).

Falls **Name** als einstellbare Eigenschaft für ein Element verfügbar ist, können **Name** und **x:Name** in XAML synonym verwendet werden. Werden allerdings beide Attribute für das gleiche Element angegeben, tritt ein Fehler auf. In einigen Fällen ist zwar eine **Name**-Eigenschaft vorhanden, diese ist jedoch schreibgeschützt (z. B. [**VisualState.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstate.name)). In diesem Fällen verwenden Sie immer **Name** zum Benennen dieses Elements im XAML, und die schreibgeschützte **Name** ist für seltener verwendete Codeszenarien vorhanden.

**Hinweis**  [**FrameworkElement.Name**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.name) sollte im Allgemeinen nicht zum Ändern von Werten verwendet werden, die ursprünglich von **x:Name** festgelegt wurde. Einige Szenarien bilden jedoch Ausnahmen zu dieser Regel. In typischen Szenarien handelt es sich bei der Erstellung und Definition von XAML-Namescopes um Vorgänge des XAML-Prozessors. Das Ändern von **FrameworkElement.Name** zur Laufzeit kann einen inkonsistenten XAML-Namescope bzw. eine inkonsistente Benennungsausrichtung privater Felder zur Folge haben, was im CodeBehind nur schwer nachvollziehbar ist.

### <a name="xname-and-xkey"></a>x:Name und x:Key

**x:Name** kann als Attribut auf Elemente innerhalb von [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) angewendet werden, um als Ersatz für das [x:Key-Attribut](x-key-attribute.md) zu dienen. (Es ist eine Regel, die alle Elemente in einem **ResourceDictionary** muss ein X: Key "oder" X: Name-Attribut aufweisen.) Dies tritt häufig bei [niedergeschrieben Animationen](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations). Weitere Informationen finden Sie im entsprechenden Abschnitt unter [ResourceDictionary- und XAML-Ressourcenverweise](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references).

