---
ms.assetid: DC235C16-8DAF-4078-9365-6612A10F3EC3
title: Erstellen der App „Hello World“ in C++/CX (Windows 10)
description: In Microsoft Visual Studio 2019 können Sie mithilfe von C++/CX eine App entwickeln, die unter Windows 10 sowie auf Smartphones mit Windows 10 ausgeführt werden kann. Die Benutzeroberfläche dieser Apps ist in XAML (Extensible Application Markup Language) definiert.
ms.date: 06/11/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 79022d1be13b98be4b086cbe452e55767ca0cedc
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86492905"
---
# <a name="create-a-hello-world-app-in-ccx"></a>Erstellen einer „Hello, World“- -App in C++/CX

> [!IMPORTANT]
> In diesem Tutorial wird C++/CX verwendet. Microsoft hat C++/WinRT veröffentlicht: eine vollständig standardmäßige und moderne C++17-Sprachprojektion für Windows-Runtime-APIs (WinRT). Weitere Informationen zu dieser Sprache findest du unter [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/).

Mit Microsoft Visual Studio kannst du eine App in C++/CX entwickeln, die unter Windows 10 mit einer XAML-definierten (Extensible Application Markup Language) Benutzeroberfläche ausgeführt wird.

> [!NOTE]
> In diesem Tutorial wird Visual Studio Community 2019 verwendet. Wenn du eine andere Version von Visual Studio verwendest, sieht die Oberfläche unter Umständen etwas anders aus.

## <a name="before-you-start"></a>Bevor Sie beginnen

-   Für dieses Tutorial benötigst du Visual Studio Community oder eine der anderen Community-Versionen von Visual Studio sowie einen Computer mit Windows 10. Informationen zum Herunterladen finden Sie unter [Herunterladen der Tools](https://visualstudio.microsoft.com/downloads/).
-   Es wird davon ausgegangen, dass du über Grundkenntnisse in C++/CX und XAML verfügst und mit den Konzepten in der [XAML-Übersicht](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-overview) vertraut bist.
-   Wir gehen davon aus, dass Sie das Standardfensterlayout in Visual Studio verwenden. Um das Layout auf das Standardlayout zurückzusetzen, klicken Sie in der Menüleiste auf **Fenster** > **Fensterlayout zurücksetzen**.

## <a name="comparing-c-desktop-apps-to-windows-apps"></a>Vergleich zwischen C++-Desktop-Apps und Windows-Apps

Wenn du bereits Windows-Desktop-Apps mit C++ programmiert hast, werden dir einige Aspekte der Programmierung von UWP-Apps wahrscheinlich bekannt sein, andere aber nicht.

### <a name="whats-the-same"></a>Gemeinsamkeiten

-   Du kannst die STL-, CRT- (mit ein paar Ausnahmen) und jede andere C++-Bibliothek verwenden, solange der Code ausschließlich Windows-Funktionen aufruft, auf die über die Windows-Runtimeumgebung zugegriffen werden kann.

-   Wenn Sie es gewohnt sind, visuelle Designer zu verwenden, können Sie immer noch den in Microsoft Visual Studio integrierten Designer verwenden, oder Sie können das Tool Blend für Visual Studio nutzen, das einen umfassenderen Umfang an Features bietet. Wenn Sie es gewohnt sind, UI manuell zu codieren, können Sie Ihren XAML-Code manuell programmieren.

-   Sie erstellen wie gehabt Apps, die Windows-Betriebssystemtypen und Ihre eigenen benutzerdefinierten Typen verwenden.

-   Sie verwenden weiterhin Debugger, Profiler und andere Entwicklungstools von Visual Studio.

-   Sie erstellen weiterhin Apps, die mit dem Visual C++-Compiler in systemeigenem Computercode kompiliert werden. UWP-Apps in C++/CX können in einer verwalteten Runtimeumgebung nicht ausgeführt werden.

### <a name="whats-new"></a>Neuigkeiten

-   Die Designprinzipien für UWP- und universelle Windows-Apps unterscheiden sich erheblich von denen für Desktop-Apps. Der Schwerpunkt liegt nicht mehr auf Fensterrahmen, Bezeichnungen, Dialogfeldern usw. Der Inhalt steht im Vordergrund. Eindrucksvolle universelle Windows-Apps folgen diesen Prinzipien schon mit Beginn der Planungsphase.

-   Die gesamte UI wird in XAML definiert. Die Trennung zwischen UI und Kernprogrammlogik ist bei Universal Windows-Apps viel eindeutiger als bei einer MFC- oder Win32-App. Designer können in der XAML-Datei am Erscheinungsbild der UI feilen, während Sie sich mit dem Verhalten in der Codedatei beschäftigen.

-   Sie programmieren in erster Linie für die Windows-Runtime – eine neue, navigationsfreundliche, objektorientierte API. Auf Windows-Geräten ist für einige Funktionen aber auch weiterhin Win32 verfügbar.

-   Sie verwenden C++/CX zum Verwenden oder Erstellen von Windows-Runtime-Objekten. C++/CX ermöglicht die Behandlung von C++-Ausnahmen, die Verwendung von Delegaten und Ereignissen sowie die automatische Verweiszählung dynamisch erstellter Objekte. Bei Verwendung von C++/CX sind die Details der zugrunde liegende COM- und Windows-Architektur im Code der App nicht zugänglich. Weitere Informationen finden Sie in der [Programmiersprachenreferenz für C++/CX](https://docs.microsoft.com/cpp/cppcx/visual-c-language-reference-c-cx).

-   Ihre App wird zu einem Paket kompiliert, das auch Metadaten zu den in der App enthaltenen Typen, den verwendeten Ressourcen und den benötigten Funktionen (Datei-, Internet- und Kamerazugriff usw.) enthält.

-   Im Microsoft Store und dem Windows Phone Store wird die Sicherheit deiner App anhand eines Zertifizierungsprozesses geprüft, und die App kann von Millionen potenzieller Kunden entdeckt werden.

## <a name="hello-world-store-app-in-ccx"></a>Store-App „Hallo Welt“ in C++/CX

Unsere erste App ist „Hello World“. Sie veranschaulicht einige grundlegende Interaktivitätsfunktionen, Layouts und Stile. Wir erstellen eine App auf der Grundlage der Projektvorlage für universelle Windows-Apps. Wenn du bereits Apps für Windows 8.1 und Windows Phone 8.1 entwickelt hast, erinnerst du dich wahrscheinlich daran, dass du drei Projekte in Visual Studio verwendet hast: eins für die Windows-App, eins für die Phone-App und ein weiteres mit gemeinsam genutztem Code. Die universelle Windows-Plattform (UWP) von Windows 10 ermöglicht die Verwendung eines einzelnen Projekts, das auf allen Geräten (Desktop- und Laptop-PCs mit Windows 10, Tablets, Smartphones, VR-Geräte usw.) ausgeführt werden kann.

Wir beginnen mit den Grundlagen:

-   Erstellen eines universellen Windows-Projekts in Visual Studio

-   Kennenlernen der erstellten Projekte und Dateien

-   Kennenlernen der Erweiterungen in Visual C++-Komponentenerweiterungen (C++/CX) und ihrer Verwendungsmöglichkeiten

**Erstellen einer Lösung in Visual Studio**

1.  Wählen Sie in der Menüleiste von Visual Studio **Datei** > **Neu** > **Projekt** aus.

2.  Wählen Sie im Dialogfeld **Neues Projekt erstellen** die Option **Leere App (Universelles Windows: C++/CX)** aus.  Sollte diese Option nicht angezeigt werden, vergewissern Sie sich, dass die Entwicklungstools für universelle Windows-Apps installiert sind. Weitere Informationen finden Sie unter [Vorbereiten](get-set-up.md).

![C++/CX-Projektvorlagen im Dialogfeld „Neues Projekt erstellen“ ](images/vs2019-uwp-01.png)

3.  Klicken Sie auf **Weiter**, und geben Sie dann einen Namen für das Projekt ein. Wir nennen unser Projekt „HelloWorld“.

4.  Klicken Sie auf die Schaltfläche **Erstellen**.

> [!NOTE]
> Wenn du Visual Studio zum ersten Mal verwendest, wird möglicherweise das Dialogfeld „Einstellungen“ angezeigt, in dem du zur Aktivierung von **Entwicklermodus** aufgefordert wirst. Der Entwicklermodus ist eine spezielle Einstellung, die bestimmte Features unterstützt, z. B. die direkte Ausführung von Apps und nicht nur die Ausführung aus dem Store. Weitere Informationen findest du unter [Aktivieren deines Geräts für die Entwicklung](enable-your-device-for-development.md). Wähle **Entwicklermodus** aus, klicke auf **Ja**, und schließe das Dialogfeld, um mit dem Tutorial fortzufahren.

   Ihre Projektdateien werden erstellt.

Werfen wir einen Blick darauf, was sich in der Lösung befindet, bevor wir fortfahren.

![Projektmappe der universellen App mit reduzierten Knoten](images/vs2017-uwp-02.png)

### <a name="about-the-project-files"></a>Informationen zu Projektdateien

Jede XAML-Datei in einem Projektordner verfügt über eine zugehörige XAML.H- und eine XAML.CPP-Datei im selben Ordner und eine G- und eine G.HPP-Datei im Ordner „Generierte Dateien“, der auf dem Datenträger vorhanden ist, jedoch nicht zum Projekt gehört. Sie können die XAML-Dateien modifizieren, um Benutzeroberflächenelemente zu erstellen und sie mit Datenquellen zu verbinden (DataBinding). Sie können die „.h“- und „.cpp“-Dateien modifizieren, um benutzerdefinierte Logik für Ereignishandler hinzuzufügen. Die automatisch erstellten Dateien stellen die Umwandlung von XAML-Markup in C++/CX dar. Verändern Sie diese Dateien nicht, sehen Sie sich die Dateien jedoch genauer an, um den CodeBehind besser zu verstehen. Im Grunde genommen enthält die generierte Datei eine partielle Klassendefinition für ein XAML-Stammelement. Diese Klasse ist die gleiche Klasse, die du in den Dateien vom Typ „\*.xaml.h“ und CPP-Dateien bearbeitest. Die generierten Dateien deklarieren die untergeordneten XAML-UI-Elemente als Klassenmember, sodass Sie in Ihrem Code auf sie verweisen können. Beim Erstellen des Builds werden der generierte Code und Ihr Code zu einer vollständigen Klassendefinition zusammengeführt und anschließend kompiliert.

Befassen wir uns zuerst mit den Projektdateien.

-   **App.xaml, App.xaml.h, App.xaml.cpp:** Stellen das Application-Objekt dar, das als Einstiegspunkt einer App fungiert. „App.xaml“ enthält kein seitenspezifisches UI-Markup, Sie können jedoch UI-Formate und andere Elemente hinzufügen, die auf allen Seiten verfügbar sein sollen. Die CodeBehind-Dateien enthalten Handler für die Ereignisse **OnLaunched** und **OnSuspending**. In der Regel können Sie hier benutzerdefinierten Code hinzufügen, um Ihre App zu initialisieren, wenn sie gestartet wird, und eine Bereinigung durchzuführen, wenn sie unterbrochen oder beendet wird.
-   **„MainPage.xaml“, „MainPage.xaml.h“, „MainPage.xaml.cpp“:** Enthalten das XAML-Markup und den CodeBehind für die standardmäßige Startseite in einer App. Sie bietet keine Unterstützung für Navigation oder integrierte Steuerelemente.
-   **pch.h, pch.cpp:** Eine vorkompilierte Headerdatei und die Datei, die sie in dein Projekt einfügt In „pch.h“ können Sie alle Header einfügen, die sich nur selten ändern und sich in anderen Dateien in der Lösung befinden.
-   **Package.appxmanifest:** Eine XML-Datei, die die von deiner App benötigten Gerätefunktionen sowie die App-Versionsinformationen und andere Metadaten beschreibt Doppelklicken Sie auf die Datei, um sie im **Manifest-Designer** zu öffnen.
-   **HelloWorld\_TemporaryKey.pfx:** Ein Schlüssel von Visual Studio, der die Bereitstellung der App auf diesem Gerät ermöglicht.

## <a name="a-first-look-at-the-code"></a>Ein erster Blick auf den Code

Wenn Sie den Code in den Dateien „App.Xaml.h“ und „App.Xaml.cpp“ im freigegebenen Projekt untersuchen, werden Sie feststellen, dass es sich meistens um C++-Code handelt. Einige Syntaxelemente kennen Sie jedoch möglicherweise nicht, wenn Sie noch nicht mit Windows-Runtime-Apps vertraut sind oder wenn Sie mit C++/CLI gearbeitet haben. Die häufigsten nicht standardmäßigen Syntaxelemente, die in C++/CX auftauchen, sind die folgenden:

**Referenzklassen**

Nahezu alle Windows-Runtime-Klassen, zu denen alle Typen in der Windows-API zählen (XAML-Steuerelemente, die Seiten in Ihrer App, die App-Klasse selbst, alle Geräte- und Netzwerkobjekte sowie alle Containertypen), werden als **ref class** deklariert. (Einige Windows-Typen werden als **value class** oder **value struct** deklariert.) Eine Referenzklasse (ref class) kann von beliebigen Programmiersprachen verwendet werden. In C++/CX wird der Lebenszyklus dieser Typen von der automatischen Verweiszählung (nicht Garbage Collection) bestimmt, sodass du diese Objekte niemals explizit löscht. Sie können auch Ihre eigenen Referenzklassen erstellen.

```cpp
namespace HelloWorld
{
   /// <summary>
   /// An empty page that can be used on its own or navigated to within a Frame.
   /// </summary>
   public ref class MainPage sealed
   {
      public:
      MainPage();
   };
}
```    

Alle Windows-Runtime-Typen müssen innerhalb eines Namespace deklariert sein, und anders als bei ISO C++ haben die Typen selbst einen Zugriffsmodifikator. Der Modifikator **public** macht die Klasse für Komponenten für Windows-Runtime außerhalb des Namespace sichtbar. Das Schlüsselwort **sealed** bedeutet, dass die Klasse nicht als Basisklasse dienen kann. Nahezu alle Referenzklassen sind „sealed“. Klassenvererbung wird häufig nicht verwendet, da JavaScript sie nicht versteht.

**ref new** und **^ (Hütchen)**

Sie können eine Variable einer Referenzklasse deklarieren, indem Sie den Operator ^ (Hütchen) verwenden, und Sie können das Objekt mit dem Schlüsselwort „ref new“ instanziieren. Danach greifen Sie auf die Instanzmethoden des Objekts mit dem Operator -> zu, wie bei einem C++-Zeiger. Auf statische Methoden kann mit dem Operator :: zugegriffen werden, genau wie in ISO C++.

Im folgenden Code verwenden wir den vollständig qualifizierten Namen, um ein Objekt zu instanziieren, und verwenden den Operator -> zum Aufrufen einer Instanzmethode.

```cpp
Windows::UI::Xaml::Media::Imaging::BitmapImage^ bitmapImage =
     ref new Windows::UI::Xaml::Media::Imaging::BitmapImage();

bitmapImage->SetSource(fileStream);
```

In einer CPP-Datei würden wir in der Regel eine `using namespace  Windows::UI::Xaml::Media::Imaging`-Direktive und das automatische Schlüsselwort hinzufügen, sodass derselbe Code wie folgt aussieht:

```cpp
auto bitmapImage = ref new BitmapImage();
bitmapImage->SetSource(fileStream);
```

**Eigenschaften**

Eine Referenzklasse kann Eigenschaften haben, die, ebenso wie bei verwalteten Sprachen, spezielle Memberfunktionen darstellen, die für verwendenden Code als Felder erscheinen.

```cpp
public ref class SaveStateEventArgs sealed
{
   public:
   // Declare the property
   property Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ PageState
   {
      Windows::Foundation::Collections::IMap<Platform::String^, Platform::Object^>^ get();
   }
   ...
};

   ...
   // consume the property like a public field
   void PhotoPage::SaveState(Object^ sender, Common::SaveStateEventArgs^ e)
   {    
      if (mruToken != nullptr && !mruToken->IsEmpty())
   {
      e->PageState->Insert("mruToken", mruToken);
   }
}
```

**Delegaten**

Ebenso wie bei verwalteten Sprachen stellt ein Delegat einen Referenztyp dar, der eine Funktion mit einer bestimmten Signatur umschließt. Sie kommen meistens mit Ereignissen und Ereignishandlern zum Einsatz.

```cpp
// Delegate declaration (within namespace scope)
public delegate void LoadStateEventHandler(Platform::Object^ sender, LoadStateEventArgs^ e);

// Event declaration (class scope)
public ref class NavigationHelper sealed
{
   public:
   event LoadStateEventHandler^ LoadState;
};

// Create the event handler in consuming class
MainPage::MainPage()
{
   auto navigationHelper = ref new Common::NavigationHelper(this);
   navigationHelper->LoadState += ref new Common::LoadStateEventHandler(this, &MainPage::LoadState);
}
```

## <a name="adding-content-to-the-app"></a>Hinzufügen von Inhalt zur App

Lassen Sie uns der App einige Inhalte hinzufügen.

**Schritt 1: Anpassen der Startseite**

1.  Öffnen Sie im **Projektmappen-Explorer** die Datei „MainPage.xaml.cs“.
2.  Erstellen Sie Steuerelemente für die Benutzeroberfläche, indem Sie den folgenden XAML-Code direkt vor dem schließenden Tag zum [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)-Stammelement hinzufügen. Er enthält ein [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) mit einem [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock), in dem der Benutzer zur Eingabe seines Namens aufgefordert wird, ein [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Element, in das der Name eingegeben wird, sowie ein [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)- und ein weiteres **TextBlock**-Element.

    ```xaml
    <StackPanel x:Name="contentPanel" Margin="120,30,0,0">
        <TextBlock HorizontalAlignment="Left" Text="Hello World" FontSize="36"/>
        <TextBlock Text="What's your name?"/>
        <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
            <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
            <Button x:Name="inputButton" Content="Say &quot;Hello&quot;"/>
        </StackPanel>
        <TextBlock x:Name="greetingOutput"/>
    </StackPanel>
    ```

3.  Sie haben nun eine sehr einfache universelle Windows-App erstellt. Falls Sie wissen möchten, wie die UWP-App aussieht, drücken Sie F5, um die App zu erstellen, bereitzustellen und im Debugmodus auszuführen.

Zunächst erscheint der standardmäßige Begrüßungsbildschirm. Er setzt sich aus einem Bild (Assets\\SplashScreen.scale-100.png) und einer Hintergrundfarbe zusammen, die in der Manifestdatei der App angegeben sind. Weitere Informationen zur Anpassung des Begrüßungsbildschirms finden Sie unter [Hinzufügen eines Begrüßungsbildschirms](https://docs.microsoft.com/previous-versions/windows/apps/hh465332(v=win.10)).

Wenn der Begrüßungsbildschirm verschwindet, wird Ihre App angezeigt. Die Hauptseite der App wird angezeigt.

![UWP-App-Bildschirm mit Steuerelementen](images/xaml-hw-app2.png)

Viel zu bieten hat sie zwar noch nicht, aber trotzdem: Herzlichen Glückwunsch! Sie haben Ihre erste UWP-App erstellt!

Kehren Sie zu Visual Studio zurück, und drücken Sie Umschalt+F5, um den Debugmodus zu beenden und die App zu schließen.

Weitere Informationen finden Sie unter [Ausführen einer Store-App über Visual Studio](https://msdn.microsoft.com/library/windows/apps/xaml/Hh441477(v=VS.140).aspx).

In der App können Sie Text in das [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Element eingeben, das Klicken auf [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) hat jedoch keinerlei Auswirkung. In späteren Schritten erstellen wir daher einen Ereignishandler für das [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis der Schaltfläche, um eine personalisierte Begrüßung einzublenden.

## <a name="step-2-create-an-event-handler"></a>Schritt 2: Erstellen eines Ereignishandlers

1.  Wählen Sie in „MainPage.xaml“ entweder in der XAML- oder in der Entwurfsansicht das [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)-Element „Say Hello“ aus dem zuvor hinzugefügten [**StackPanel**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel)-Element aus.
2.  Öffne durch Drücken von F4 das **Eigenschaftenfenster**, und wähle anschließend die Ereignisschaltfläche (![Ereignisschaltfläche](images/eventsbutton.png)) aus.
3.  Suchen Sie das [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignis. Geben Sie im Textfeld den Namen der Funktion ein, die das **Click**-Ereignis behandelt. Gib für dieses Beispiel „Button\_Click“ ein.

    ![Eigenschaftenfenster, Ereignisansicht](images/xaml-hw-event.png)

4.  Drücken Sie die EINGABETASTE. Die Ereignishandlermethode wird in „MainPage.xaml.cpp“ erstellt und im Code-Editor geöffnet, sodass Sie den Code hinzufügen können, der beim Auftreten des Ereignisses ausgeführt werden soll.

   Gleichzeitig wird in „MainPage.xaml“ der XAML-Code für das [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button)-Element aktualisiert, um den [**Click**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click)-Ereignishandler wie folgt zu deklarieren:

```xaml
<Button Content="Say &quot;Hello&quot;" Click="Button_Click"/>
```

   Sie können dies auch einfach manuell zum XAML-Code hinzufügen, was vor allem dann hilfreich ist, wenn der Designer nicht geladen wird. Wenn Sie dies manuell eingeben, geben Sie „Click“ ein, und warten Sie, bis IntelliSense die Option zum Hinzufügen eines neuen Ereignishandlers anzeigt. Auf diese Weise erstellt Visual Studio die erforderliche Methodendeklaration und den erforderlichen Stub.

   Im Designer tritt ein Fehler beim Laden auf, wenn eine unbehandelte Ausnahme während des Renderns auftritt. Für das Rendern im Designer muss eine Entwurfszeitversion der Seite ausgeführt werden. Es ist möglicherweise hilfreich, den ausgeführten Benutzercode zu deaktivieren. Ändern Sie dazu die Einstellung im Dialogfeld **Tools, Optionen**. Deaktivieren Sie unter **XAML-Designer** die Option **Projektcode im XAML-Designer ausführen (falls unterstützt)** .

5.  Füge in „MainPage.xaml.cpp“ dem gerade erstellten **Button\_Click**-Ereignishandler den folgenden Code hinzu. Dieser Code ruft den Namen des Benutzers aus dem [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox)-Steuerelement `nameInput` ab und erstellt damit eine Begrüßung. Das [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element `greetingOutput` zeigt das Ergebnis an.

```cpp
void HelloWorld::MainPage::Button_Click(Platform::Object^ sender, Windows::UI::Xaml::RoutedEventArgs^ e)
{
    greetingOutput->Text = "Hello, " + nameInput->Text + "!";
}
```

6.  Legen Sie das Projekt als Startprojekt fest, und drücken Sie anschließend F5, um die App zu erstellen und auszuführen. Wenn Sie einen Namen in das Textfeld eingeben und anschließend auf die Schaltfläche klicken, zeigt die App eine personalisierte Begrüßung an.

![App-Bildschirm mit angezeigter Meldung](images/xaml-hw-app4.png)

## <a name="step-3-style-the-start-page"></a>Schritt 3: Gestalten der Startseite

### <a name="choosing-a-theme"></a>Auswählen eines Designs

Das Erscheinungsbild Ihrer App lässt sich ganz einfach anpassen. Standardmäßig verwendet die App Ressourcen mit hellem Design. Die Systemressourcen enthalten auch ein helles Design. Probieren wir doch einmal aus, wie das aussieht.

**So wechselst du zum dunklen Design**

1.  Öffnen Sie „App.xaml“.
2.  Bearbeiten Sie im öffnenden [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application)-Tag die [**RequestedTheme**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.requestedtheme)-Eigenschaft, und legen Sie ihren Wert auf **Dark** fest:

```xaml
RequestedTheme="Dark"
```

    Here's the full [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) tag with the dark theme :

```xaml
    <Application
    x:Class="HelloWorld.App"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:HelloWorld"
    RequestedTheme="Dark">
```

3.  Drücken Sie F5, um die App zu erstellen und auszuführen. Wie Sie sehen, wird nun das dunkle Design verwendet.

![App-Bildschirm mit dunklem Design](images/xaml-hw-app3.png)

Welches Design sollten Sie verwenden? Das bleibt ganz Ihnen überlassen. Bei Apps, die hauptsächlich Bilder oder Videos anzeigen, sollten Sie das dunkle Design verwenden, während sich bei Apps mit viel Text die Verwendung des hellen Designs empfiehlt. Falls Sie ein benutzerdefiniertes Farbschema verwenden, wählen Sie das Design, das am besten zum Erscheinungsbild Ihrer App passt. Im verbleibenden Teil dieses Lernprogramms verwenden wir in Screenshots das helle Design.

**Hinweis:**   Das Design wird beim Aktivieren der App angewendet und kann nicht geändert werden, während die App ausgeführt wird.

### <a name="using-system-styles"></a>Verwenden von Systemstilen

Momentan ist der Text in der Windows-App ziemlich klein und nur schwer lesbar. Lassen Sie uns dies ändern, indem wir einen Systemstil anwenden.

**So änderst du den Stil eines Elements**

1.  Öffnen Sie „MainPage.xaml“ im Windows-Projekt.
2.  Wählen Sie in der XAML- oder Entwurfsansicht das von Ihnen hinzugefügte [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element „What’s your name?“ aus.
3.  Wählen Sie im Dialogfeld **Eigenschaften** (**F4**) rechts oben die Schaltfläche „Eigenschaften“ (![Schaltfläche „Eigenschaften“](images/propertiesbutton.png)) aus.
4.  Erweitern Sie die Gruppe **Text** , und legen Sie den Schriftgrad auf „18 px“ fest.
5.  Erweitern Sie die Gruppe **Sonstiges**, und suchen Sie dort nach der Eigenschaft **Style**.
6.  Klicken Sie auf den Eigenschaftenmarker (das grüne Feld rechts neben der Eigenschaft **Style**), und wählen Sie anschließend **Systemressource** > **BaseTextBlockStyle** im Menü aus.

     **BaseTextBlockStyle** ist eine Ressource, die in [**ResourceDictionary**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.ResourceDictionary) in „<root>\\Program Files\\Windows Kits\\10\\Include\\winrt\\xaml\\design\\generic.xaml“ definiert ist.

    ![Eigenschaftenfenster, Eigenschaftenansicht](images/xaml-hw-style-cpp.png)

     Auf der XAML-Entwurfsoberfläche ändert sich die Textdarstellung. Im XAML-Editor wird der XAML-Code für [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) aktualisiert:

```xaml
<TextBlock Text="What's your name?" Style="{ThemeResource BaseTextBlockStyle}"/>
```

7.  Wiederholen Sie den Vorgang, um den Schriftgrad festzulegen und **BaseTextBlockStyle** dem [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element `greetingOutput` zuzuweisen.

    **Tipp:**   Obwohl sich in diesem [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock)-Element kein Text befindet, zeigt eine blaue Umrandung seine Position an, wenn du den Mauszeiger über die XAML-Entwurfsoberfläche bewegst, sodass du ihn auswählen kannst.  

    Ihr XAML-Code sieht nun so aus:

```xaml
<StackPanel x:Name="contentPanel" Margin="120,30,0,0">
    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" Text="What's your name?"/>
    <StackPanel x:Name="inputPanel" Orientation="Horizontal" Margin="0,20,0,20">
        <TextBox x:Name="nameInput" Width="300" HorizontalAlignment="Left"/>
        <Button x:Name="inputButton" Content="Say &quot;Hello&quot;" Click="Button_Click"/>
    </StackPanel>
    <TextBlock Style="{ThemeResource BaseTextBlockStyle}" FontSize="18" x:Name="greetingOutput"/>
</StackPanel>
```

8.  Drücken Sie F5, um die App zu erstellen und auszuführen. Sie sieht jetzt so aus:

![App-Bildschirm mit größerem Text](images/xaml-hw-app5.png)

### <a name="step-4-adapt-the-ui-to-different-window-sizes"></a>Schritt 4: Anpassen der UI an verschiedene Fenstergrößen

Als Nächstes sorgen wir dafür, dass sich die UI verschiedenen Bildschirmgrößen anpasst, damit sie auch auf mobilen Geräten gut aussieht. Dazu fügen Sie ein [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager)-Element hinzu und legen Eigenschaften fest, die für die unterschiedlichen Ansichtszustände angewendet werden.

**So passt du das UI-Layout an**

1.  Fügen Sie im XAML-Editor nach dem Starttag des [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid)-Stammelements den folgenden XAML-Block hinzu:

```xaml
<VisualStateManager.VisualStateGroups>
    <VisualStateGroup>
        <VisualState x:Name="wideState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="641" />
            </VisualState.StateTriggers>
        </VisualState>
        <VisualState x:Name="narrowState">
            <VisualState.StateTriggers>
                <AdaptiveTrigger MinWindowWidth="0" />
            </VisualState.StateTriggers>
            <VisualState.Setters>
                <Setter Target="contentPanel.Margin" Value="20,30,0,0"/>
                <Setter Target="inputPanel.Orientation" Value="Vertical"/>
                <Setter Target="inputButton.Margin" Value="0,4,0,0"/>
            </VisualState.Setters>
        </VisualState>
    </VisualStateGroup>
</VisualStateManager.VisualStateGroups>
```

2.  Debuggen Sie die App auf dem lokalen Computer. Sie sehen, dass die UI genauso aussieht wie vorher, es sei denn, die Fensterbreite beträgt weniger als 641 DIP (geräteunabhängige Pixel).
3.  Debuggen Sie die App auf dem Emulator für mobile Geräte. Beachten Sie, dass für die UI die Eigenschaften verwendet werden, die Sie unter `narrowState` definiert haben, und dass die Benutzeroberfläche auf dem kleinen Bildschirm korrekt angezeigt wird.

![Bildschirm der mobilen App mit Textdesign](images/hw10-screen2-mob.png)

Wenn Sie bereits in früheren Versionen von XAML ein [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager)-Element genutzt haben, fällt Ihnen vielleicht auf, dass für den XAML-Code hier eine vereinfachte Syntax verwendet wird.

Das [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)-Element mit dem Namen `wideState` verfügt über ein [**AdaptiveTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)-Element, für das die [**MinWindowWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth)-Eigenschaft auf den Wert 641 festgelegt ist. Das bedeutet, dass der Zustand nur angewendet wird, wenn die Fensterbreite nicht geringer als die Mindestgröße von 641 DIP ist. Da Sie keine [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter)-Objekte für diesen Zustand definieren, werden die Layouteigenschaften verwendet, die Sie im XAML-Code für den Seiteninhalt festgelegt haben.

Das zweite [**VisualState**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState)-Element `narrowState` verfügt über ein [**AdaptiveTrigger**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.AdaptiveTrigger)-Element, für das die [**MinWindowWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.adaptivetrigger.minwindowwidth)-Eigenschaft auf 0 festgelegt ist. Dieser Zustand wird angewendet, wenn die Fensterbreite größer 0, aber kleiner als 641 DIP ist. (Bei 641 DIPs wird `wideState` angewendet.) In diesem Zustand definierst du einige [**Setter**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Setter)-Objekte, um die Layouteigenschaften der Steuerelemente auf der UI zu ändern:

-   Verringern Sie den linken Rand des `contentPanel`-Elements von 120 auf 20.
-   Ändern Sie das [**Orientation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.stackpanel.orientation)-Element des `inputPanel`-Elements von **Horizontal** in **Vertical**.
-   Fügen Sie dem `inputButton`-Element einen oberen Rand mit einer Breite von 4 DIP hinzu.

### <a name="summary"></a>Zusammenfassung

Herzlichen Glückwunsch! Sie haben das erste Lernprogramm abgeschlossen. Darin haben Sie gelernt, wie Sie Inhalte zu universellen Windows-Apps hinzufügen, wie Sie sie interaktiv machen und wie Sie ihr Erscheinungsbild ändern.

## <a name="next-steps"></a>Nächste Schritte

Wenn du ein Projekt für universelle Windows-Apps für Windows 8.1 und/oder Windows Phone 8.1 besitzt, kannst du es zu Windows 10 portieren. Es gibt keinen automatischen Prozess dafür, du kannst das Projekt jedoch manuell portieren. Beginnen Sie mit einem neuen universellen Windows-Projekt, um die aktuelle Projektsystemstruktur und Manifestdateien abzurufen. Kopieren Sie dann die Codedateien in die Verzeichnisstruktur des Projekts, fügen Sie Ihrem Projekt Elemente hinzu, und schreiben Sie Ihren XAML-Code mithilfe von [**VisualStateManager**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) gemäß den Anweisungen in diesem Thema um. Weitere Informationen finden Sie unter [Portieren eines Windows-Runtime 8-Projekts zu einem UWP-Projekt (Universelle Windows-Plattform)](https://docs.microsoft.com/windows/uwp/porting/w8x-to-uwp-porting-to-a-uwp-project) sowie unter [Portieren zur Universellen Windows-Plattform (C++)](https://msdn.microsoft.com/library/mt186164.aspx).

Informationen dazu, wie du vorhandenen C++-Code in einer UWP-App verwenden kannst, um beispielsweise eine neue UWP-Benutzeroberfläche für eine vorhandene Anwendung zu erstellen, findest du unter [Vorgehensweise: Verwenden von vorhandenem C++-Code in einer UWP-App (Universelle Windows-Plattform)](https://msdn.microsoft.com/library/mt186162.aspx).
