---
title: Komponenten für Windows-Runtime
description: Komponenten für Windows-Runtime sind unabhängige Objekte, die Sie instanziieren und von jeder Sprache aus verwenden können, einschließlich C#, Visual Basic, JavaScript und C++.
ms.assetid: 55887622-828b-4318-87f2-25592268f7c1
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 76026c4f499a068bab689eadf050e0ec6277bbe8
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393684"
---
# <a name="windows-runtime-components"></a>Komponenten für Windows-Runtime
Komponenten für Windows-Runtime sind unabhängige Objekte, die Sie instanziieren und von jeder Sprache aus verwenden können, einschließlich C#, Visual Basic, JavaScript und C++.

Sie können Visual Studio und C#, Visual Basic oder C++ verwenden, um Komponenten für Windows-Runtime zu erstellen, die in UWP-Apps (Universelle Windows-Plattform) verwendet werden können.

| Thema | Beschreibung |
|-------|-------------|
| [Komponenten für Windows-Runtime mit C++/CX](creating-windows-runtime-components-in-cpp.md) | In diesem Artikel wird beschrieben, wie Sie eine Komponente für Windows-Runtime mit C++/CX erstellen&mdash;eine Komponente, die aus einer universellen Windows-App aufgerufen werden kann, die in einer der Windows-Runtime-Sprachen erstellt wurde. |
| [Exemplarische Vorgehensweise zum Erstellen einer Komponente für Windows-Runtime in C++/CX und Aufrufen der Komponente über JavaScript oder C#](walkthrough-creating-a-basic-windows-runtime-component-in-cpp-and-calling-it-from-javascript-or-csharp.md) | In dieser exemplarischen Vorgehensweise wird veranschaulicht, wie Sie eine einfache Komponenten-DLL-Datei für Windows-Runtime erstellen, die von JavaScript, C# oder Visual Basic aufgerufen werden kann. Bevor Sie mit dieser exemplarischen Vorgehensweise beginnen, stellen Sie sicher, dass Sie Konzepte wie die abstrakte binäre Schnittstelle (ABI), Verweisklassen und Visual C++-Komponentenerweiterungen kennen, die das Verwenden von Verweisklassen vereinfachen. Weitere Informationen finden Sie unter [Erstellen von Komponenten für Windows-Runtime in C++](creating-windows-runtime-components-in-cpp.md) und [Visual C++-Programmiersprachenreferenz (C++/CX)](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx). |
| [Komponenten für Windows-Runtime in C# und Visual Basic](creating-windows-runtime-components-in-csharp-and-visual-basic.md) | Sie können mit verwaltetem Code eigene Windows-Runtime-Typen erstellen, die in einer Komponente für Windows-Runtime gepackt sind. Diese Komponente können Sie in UWP-Apps (Universelle Windows-Plattform) mit C++, JavaScript, Visual Basic oder C# verwenden. In diesem Thema werden die Regeln zum Erstellen einer Komponente und einige Aspekte der .NET-Unterstützung für die Windows-Runtime erläutert. Im Allgemeinen ist diese Unterstützung allen .NET-Programmierern klar. Wenn Sie aber eine Komponente erstellen, die mit JavaScript oder C++ verwendet werden soll, müssen Sie auf die Unterschiede bei der Unterstützung der Windows-Runtime durch diese Sprachen achten. |
| [Exemplarische Vorgehensweise zum Erstellen einer Komponente für Windows-Runtime in C# oder Visual Basic und Aufrufen der Komponente über JavaScript](walkthrough-creating-a-simple-windows-runtime-component-and-calling-it-from-javascript.md) | In dieser exemplarischen Vorgehensweise wird gezeigt, wie Sie .NET mit Visual Basic oder C# verwenden, um eigene Windows-Runtime-Typen zu erstellen, die in einer Komponente für Windows-Runtime verpackt sind, und wie Sie die Komponente in Ihrer universellen Windows-App aufrufen, die mit JavaScript für Windows erstellt wurde. |
| [Auslösen von Ereignissen in Komponenten für Windows-Runtime](raising-events-in-windows-runtime-components.md) | Wenn die Komponente für Windows-Runtime ein Ereignis eines benutzerdefinierten Delegattyps in einem Hintergrundthread (Arbeitsthread) auslöst und JavaScript in der Lage sein soll, das Ereignis zu empfangen, können Sie es auf eine der folgenden Weisen implementieren und/oder auslösen: | 
| [Vermittelte Komponenten für Windows-Runtime für quergeladene UWP-Apps](brokered-windows-runtime-components-for-side-loaded-windows-store-apps.md) | In diesem Thema wird ein für Unternehmen bestimmtes Feature beschrieben, das von Windows 10 Update und höher unterstützt wird und .NET-Apps, die Toucheingaben unterstützen, die Verwendung vorhandenen Codes ermöglichen, der für wichtige unternehmenskritische Vorgänge verantwortlich ist. |
