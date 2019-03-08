---
title: Kunden-Datenbank app-Struktur
description: Überprüfen Sie die Struktur des datenbanklernprogramms Kunden, und warum er erstellt wurde, wie es ist.
keywords: Enterprise "," Tutorial "," Kunde "," Daten "," crud
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b1f8f8c8a2fd1522d8c304a45514d5257543f222
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656615"
---
# <a name="customer-database-app-structure"></a>Kunden-Datenbank app-Struktur

Komplexe Line-of-Business-apps haben häufig viele Seiten und Features und sehr viele Codezeilen. Aus diesem Grund ist es sehr wichtig, dass Sie Ihre app um eine vorhersagbare Struktur entwerfen. Es gibt verschiedene Entwurfsmuster für Anwendungen geeignet für Unternehmens-apps sind, aber sie basieren alle auf dem Ziel, der leichter zu verstehen und Arbeiten mit einer umfangreichen Anwendung.

Während der [Kunden datenbanktutorial](customer-database-tutorial.md) eine Einzelseiten app zeigt aus Gründen der Einfachheit implementiert das Entwurfsmuster von Model-View-ViewModel (MVVM) app Showcase diese Ideen in Aktion. Wie der Name schon sagt, trennt das MVVM-Entwurfsmuster die Kernlogik für die app in drei Kategorien:

* Modelle sind die Klassen, die die Daten der Anwendung enthalten.
* Ansichten sind die Benutzeroberfläche der betreffenden Seite.
* Geben Sie die Anwendungslogik ViewModels. Dies kann beinhalten, behandeln die Benutzeraktionen aus der Sicht bzw. Verwalten von Interaktionen mit den Modellen.

Während dieser app ein Beispiel für perfekt und Architypical MVVM nicht ist, zeigt es die wichtigsten Prinzipien der Trennung von Zuständigkeiten in Aktion. [Sehen Sie sich hier die app.](https://github.com/Microsoft/windows-tutorials-customer-database)

## <a name="application-structure"></a>Anwendungsstruktur

Nachdem Sie die app geöffnet haben, beginnen Sie mit der Untersuchung der **Projektmappen-Explorer.** Alle Sie, sehen es sollte vertraut sein, wenn Sie mit einer UWP-app, bevor Sie gearbeitet haben, aber Sie sehen auch eine Auflistung von Ordnern zu speichern, die der app-Komponenten.

![App Ausgangspunkt im Projektmappen-Explorer](images/customer-database-tutorial/solution-explorer.png)

### <a name="views"></a>Ansichten

Alle app Benutzeroberfläche wird im Ordner "Views" definiert. Da unser Tutorial ein Single-Page-app ist, dies bedeutet, dass es nur eine Ansicht - **CustomerListPage**. Es sind XAML-UI-Markup und CodeBehind von "XAML.cs" ausgedrückt: Diese beiden Dateien besteht aus einer Ansicht. Sie UI-Elemente für hinzufügen **CustomerListPage.xaml**.

> [!NOTE]
> Sie werden feststellen, dass diese app eine "MainPage" nicht. Dies liegt daran geben wir in **"App.Xaml.cs"** , die die app sollte gestartet werden **CustomerListPage** beim Starten.

### <a name="viewmodels"></a>ViewModels

Wenn diese app nur eine Ansicht aufweist, ist verfügt über zwei ViewModels. Warum ist das?

**CustomerListPageViewModel.cs** ist ein standard "ViewModel" in das MVVM-Muster. Es ist, in denen die grundlegende Logik des app Seite befindet, und die Seite Sie arbeiten mit den meisten in diesem Tutorial. Jedes UI-Aktion, die durch den User gewählt wird über die Sicht an "ViewModel" für die Verarbeitung übergeben.

**CustomerViewModel.cs**, jedoch nicht mit einer bestimmten Ansicht zugeordnet. Stattdessen ordnet sie einer programmgesteuerten Konzept (welche Eigenschaften bearbeitet wurde) mit den Daten im Modell für einen einzelnen Kunden enthalten.

### <a name="models"></a>Modelle

Diese app enthält drei Modelle, die die Daten der app zu speichern, und stellen Ihnen Schnittstellen zur Interaktion mit dem Repository. Während dieser kritischen Teilen der app befinden, ist dies jedoch nicht etwas, das Sie in diesem Tutorial direkt bearbeitet werden.

Am wichtigsten ist **Customer.cs**, die beschreibt, die Kunden-Datenstruktur, die Sie in diesem Tutorial verwenden möchten.

> [!NOTE]
> Das Tutorial ignoriert die *-e-Mail* und *Phone* Eigenschaften des Customer-Objekts. Sollten Sie hinausgehen ist wie hier angezeigt wird, diese beiden Eigenschaften hinzufügen, in der Benutzeroberfläche der app ein guter erster Schritt.

### <a name="repository"></a>Repository

Repository-Ordner enthält Klassen, die erstellt und mit der lokalen SQLite-Datenbank zu interagieren. Für das Lernprogramm die SQLite-Datenbank als dargestellt wird-wird. Während Sie im Code, um hinzufügen **CustomerListPageViewModel.cs** um von diesen Klassen definierten Methoden aufrufen, brauchen Sie keine Änderungen vornehmen, um deren Einrichtung.

Weitere Informationen zu SQLite in UWP [finden Sie im Artikel](../data-access/sqlite-databases.md).

Wenn Sie im Abschnitt "Weiterführende Themen" des Tutorials versuchen, ist dies die, in dem Sie eine Klasse für die Verbindung mit der REST-Remotedatenbank erstellen. Es implementiert auch die **ICustomerRepository** -Schnittstelle, die im Abschnitt "Modelle" definiert, aber sie unterscheiden sich sehr als Gegenstück SQLite.

### <a name="other-elements"></a>Andere Elemente

Wie gewöhnlich für UWP-apps ist, wird das Verhalten der Anwendung starten in definiert die **"App.Xaml.cs"** Klasse. Großteil des Codes hier ist der Standard-Code für eine UWP-app. Aber wir haben bereits einige geringfügige Änderungen vorgenommen:

* Wir haben angegeben, dass die app anzeigen soll **CustomerListPage** starten.
* Wir haben ein Repository-Objekt, erstellt, die die Datenquelle enthält, die wir verwenden.
* Wir haben hinzugefügt, ein **SQLiteDatabase** Methode, die die lokale Datenbank initialisiert und als das angegebene Repository festgelegt.

Wenn Sie im Abschnitt "Weiterführende Themen" versuchen, fügen Sie eine ähnliche Methode um eine REST-Repository-Objekt zu initialisieren. Da wir unsere Aspekte getrennt haben und die gleiche definierte Schnittstelle für sowohl SQLite und REST-Vorgänge verwenden, wird dies die einzige vorhandene Code sein, die, den Sie zum Verwenden von REST anstelle von SQLite in Ihrer app ändern müssen.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie bereits das Tutorial abgeschlossen haben, sehen Sie sich die [vollständige beispielanwendung](https://github.com/Microsoft/Windows-appsample-customers-orders-database) angezeigt, wie diese Funktionen in einem größeren Rahmen implementiert werden.

Jetzt wissen Sie, warum das alles ist, wo es ist andernfalls sollte [zurück, mit dem Tutorial](customer-database-tutorial.md) und das Arbeiten mit der Struktur, die wir gerade beschrieben haben.