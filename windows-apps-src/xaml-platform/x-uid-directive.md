---
description: Stellt einen eindeutigen Bezeichner für Markupelemente bereit. Bei UWP-XAML (Universelle Windows-Plattform) wird dieser eindeutige Bezeichner für XAML-Lokalisierungsprozesse und -tools verwendet, z. B. beim Verwenden von Ressourcen aus einer RESW-Ressourcendatei.
title: xUid-Direktive
ms.assetid: 9FD6B62E-D345-44C6-B739-17ED1A187D69
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 03cf647fdb243fd18212ca894f7682e913378907
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609635"
---
# <a name="xuid-directive"></a>x:Uid-Direktive


Stellt einen eindeutigen Bezeichner für Markupelemente bereit. Bei UWP-XAML (Universelle Windows-Plattform) wird dieser eindeutige Bezeichner für XAML-Lokalisierungsprozesse und -tools verwendet, z. B. beim Verwenden von Ressourcen aus einer RESW-Ressourcendatei.

## <a name="xaml-attribute-usage"></a>XAML-Attributsyntax

``` syntax
<object x:Uid="stringID".../>
```

## <a name="xaml-values"></a>XAML-Werte

| Begriff | Beschreibung |
|------|-------------|
| stringID | Eine Zeichenfolge, mit der ein XAML-Element in einer App eindeutig identifiziert wird und die Teil des Ressourcenpfads in einer Ressourcendatei wird. Siehe Anmerkungen.| 

## <a name="remarks"></a>Hinweise

Verwenden Sie **x:Uid** zum Identifizieren eines Objektelements im XAML-Code. Normalerweise ist dieses Objektelement eine Instanz einer Steuerelementklasse oder eines anderen Elements, das auf einer Benutzeroberfläche angezeigt wird. Die Beziehung zwischen der in **x:Uid** verwendeten Zeichenfolge und den in einer Ressourcendatei verwendeten Zeichenfolgen wird so angegeben, dass die Zeichenfolgen der Ressourcendatei als **x:Uid** gefolgt von einem Punkt (.) und dann vom Namen einer spezifischen Eigenschaft des Elements, das lokalisiert wird, dargestellt werden. Betrachten Sie das folgende Beispiel:

``` syntax
<Button x:Uid="GoButton" Content="Go"/>
```

Um Inhalte als Ersatz für den Anzeigetext **Go** festzulegen, müssen Sie eine neue Ressource aus einer Ressourcendatei angeben. Die Ressourcendatei sollte einen Eintrag für die Ressource mit dem Namen „GoButton.Content“ enthalten. [**Inhalt** ](/uwp/api/windows.ui.xaml.controls.contentcontrol.content) in diesem Fall wird eine bestimmte Eigenschaft, die von geerbt wird die [ **Schaltfläche** ](/uwp/api/windows.ui.xaml.controls.button) Klasse. Sie können auch lokalisierte Werte für die anderen Eigenschaften dieser Schaltfläche bereitstellen, z. B. einen auf einer Ressource basierenden Wert für „GoButton.FlowDirection“. Weitere Informationen zur gemeinsamen Verwendung von **x:Uid** und Ressourcendateien finden Sie unter [Lokalisieren der Zeichenfolgen in Ihrer Benutzeroberfläche und im App-Paketmanifest](../app-resources/localize-strings-ui-manifest.md).

Die Gültigkeit der Zeichenfolgen für einen **x:Uid**-Wert ist in der Praxis davon abhängig, welche Zeichenfolgen als Bezeichner in einer Ressourcendatei und in einem Ressourcenpfad zulässig sind.

**x:Uid** wird getrennt von **x:Name** behandelt – zum einen aufgrund des angegebenen XAML-Lokalisierungsszenarios, zum anderen, damit für die Lokalisierung verwendete Bezeichner nicht von den Programmiermodellaspekten des **x:Name**-Elements abhängig sind. Darüber hinaus unterliegt **x:Name** dem XAML-Namescope-Konzept, während die Eindeutigkeit für **x:Uid** über das Paketressourcenindex-System (PRI) kontrolliert wird. Weitere Informationen finden Sie unter [Ressourcenverwaltungssystem](../app-resources/resource-management-system.md).

Die von UWP-XAML verwendeten Regeln für die Eindeutigkeit von **x:Uid** unterscheiden sich von den Regeln, die bei früheren Technologien mit XAML zum Einsatz kamen. Bei UWP-XAML kann der gleiche ID-Wert vom Typ **x:Uid** als Direktive für mehrere XAML-Elemente vorhanden sein. In diesem Fall muss allerdings jedes Element dieser Art die gleiche Auflösungslogik verwenden, um die Ressourcen in einer Ressourcendatei aufzulösen. Darüber hinaus verwenden alle XAML-Dateien in einem Projekt den gleichen Ressourcenbereich für die **x:Uid**-Auflösung. Es gibt kein Konzept, bei dem **x:Uid**-Bereiche auf einzelne XAML-Dateien ausgerichtet werden.

In einigen Fällen verwenden Sie anstelle der integrierten Funktionen des Paketressourcenindex-Systems (PRI) einen Ressourcenpfad. Jede als **x:Uid**-Wert verwendete Zeichenfolge definiert einen Ressourcenpfad, der mit „ms-resource:///Resources/“ beginnt und die Zeichenfolge **x:Uid** enthält. Am Ende des Pfads stehen die Namen der Eigenschaften, die Sie in einer Ressourcendatei oder anderweitig angeben.

Fügen Sie **x:Uid** nicht in Eigenschaftselemente ein, da dies in Windows-Runtime-XAML nicht zulässig ist.

