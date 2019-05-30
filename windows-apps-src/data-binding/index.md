---
ms.assetid: 83b4be37-6613-4d00-a48a-0451a24a30fb
title: Datenbindung
description: Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 96bb0ca90ee3a55f43837fa92bad750b95b248eb
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66362441"
---
# <a name="data-binding"></a>Datenbindung

Die Datenbindung ist eine Methode, mit der die Benutzeroberfläche Ihrer App Daten anzeigen und diese Daten optional synchronisieren kann. Mit der Datenbindung können Sie Datenaspekte von Benutzeroberflächenaspekten trennen, was zu einem einfacheren konzeptionellen Modell und besserer Lesbarkeit, Testbarkeit und Wartung Ihrer App führt. Im Markup können Sie entweder die [{X:Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) oder die [{Binding}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) verwenden. Sie können sogar eine Mischung aus den beiden in derselben App verwenden – sogar im gleichen Benutzeroberflächenelement. {x:Bind} ist neu in Windows 10 und bietet eine bessere Leistung.

| Thema | Beschreibung |
|-------|-------------|
| [Übersicht über Datenbindung](data-binding-quickstart.md) | In diesem Thema erfahren Sie, wie Sie in einer UWP-App (Universelle Windows-Plattform) ein Steuerelement (oder ein anderes Benutzeroberflächenelement) an ein einzelnes Element oder ein Elementsteuerelement an eine Sammlung von Elementen binden. Darüber hinaus wird erläutert, wie Sie die Anzeige von Elementen steuern, eine Detailansicht auf Grundlage einer Auswahl implementieren und Daten für die Anzeige umwandeln. Ausführliche Informationen finden Sie unter [Datenbindung im Detail](data-binding-in-depth.md). | 
| [Datenbindung im Detail](data-binding-in-depth.md) | In diesem Thema werden die Datenbindungsfeatures ausführlich beschrieben. |
| [Beispieldaten für die Entwurfsoberfläche und Prototyperstellung](displaying-data-in-the-designer.md) | Es gibt mehrere Möglichkeiten, Entwurfszeit-Beispieldaten zu verwenden, damit Steuerelemente im Visual Studio-Designer mit Daten aufgefüllt werden (sodass Sie das Layout, die Vorlagen und andere visuelle Eigenschaften der App bearbeiten können). Beispieldaten können auch hilfreich sein und Zeit sparen, wenn Sie eine App als Skizze (oder Prototyp) erstellen. Sie können zur Laufzeit Beispieldaten in der Skizze oder im Prototyp verwenden, um Ihre Ideen zu veranschaulichen, ohne echte Livedaten nutzen zu müssen. |
| [Binden von hierarchischen Daten und Erstellen einer Master/Details-Ansicht](how-to-bind-to-hierarchical-data-and-create-a-master-details-view.md) | Sie können eine Master/Details-Ansicht mit mehreren Ebenen (auch bekannt als Listen-Details-Ansicht) von hierarchischen Daten erstellen, indem Sie Elementsteuerelemente an [<strong>CollectionViewSource</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource)-Instanzen binden, die in einer Kette verbunden sind. |
| [Datenbindungen und MVVM](data-binding-and-mvvm.md) | In diesem Thema wird das UI-Architekturentwurfsmuster „Model-View-ViewModel“ (MVVM) beschrieben. Die Grundlage von MVVM ist die Datenbindung, die eine lose Kopplung zwischen UI-Code und Nicht-UI-Code ermöglicht. |
