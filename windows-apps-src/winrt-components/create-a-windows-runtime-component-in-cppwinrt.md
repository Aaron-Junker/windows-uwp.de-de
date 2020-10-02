---
title: Windows-Runtime-Komponenten mit C++/WinRT
description: In diesem Thema wird gezeigt, wie [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) verwendet wird, um eine Windows-Runtime Komponente zu erstellen und zu nutzen, &mdash; die von einer universellen Windows-app aufgerufen werden kann, die mit einer beliebigen Windows-Runtime Sprache erstellt wurde.
ms.date: 07/06/2020
ms.topic: article
keywords: Windows 10, UWP, Windows, Runtime, Komponente, Komponenten, Windows-Runtime Komponente, WRC, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: 12fa7c499dd0203a489b5657f1e3e2634bdad8b1
ms.sourcegitcommit: a93a309a11cdc0931e2f3bf155c5fa54c23db7c3
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/02/2020
ms.locfileid: "91646293"
---
# <a name="windows-runtime-components-with-cwinrt"></a>Windows-Runtime-Komponenten mit C++/WinRT

In diesem Thema wird gezeigt, wie [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) verwendet wird, um eine Windows-Runtime Komponente zu erstellen und zu nutzen, &mdash; die von einer universellen Windows-app aufgerufen werden kann, die mit einer beliebigen Windows-Runtime Sprache erstellt wurde.

Es gibt verschiedene Gründe für das Entwickeln einer Windows-Runtime Komponente in C++/WinRT.
- Um die Leistungsvorteile von C++ in komplexen oder rechenintensiven Vorgängen zu nutzen.
- Zum wieder verwenden von Standard-C++-Code, der bereits geschrieben und getestet wurde.
- Zum verfügbar machen von Win32-Funktionen für eine in geschriebene universelle Windows-Plattform (UWP)-app, z. b. c#.

Wenn Sie die C++/WinRT-Komponente erstellen, können Sie im allgemeinen Typen aus der C++-Standardbibliothek und integrierte Typen verwenden, außer an der Grenze der Anwendungs Binärschnittstelle (ABI), an die Sie Daten an den Code in einem anderen `.winmd` Paket übergeben. Verwenden Sie in der ABI Windows-Runtime Typen. Verwenden Sie außerdem im C++/WinRT-Code Typen wie Delegat und Ereignis, um Ereignisse zu implementieren, die von der Komponente ausgelöst und in einer anderen Sprache behandelt werden können. Weitere Informationen zu C++/WinRT. finden Sie unter [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) .

Im restlichen Teil dieses Themas erfahren Sie, wie Sie eine Windows-Runtime Komponente in C++/WinRT erstellen und dann in einer Anwendung verwenden.

Die Windows-Runtime Komponente, die Sie in diesem Thema erstellen, enthält eine Lauf Zeit Klasse, die ein Thermometer darstellt. Außerdem wird in diesem Thema eine Core-App veranschaulicht, die die Thermometer-Lauf Zeit Klasse nutzt und eine Funktion aufruft, mit der die Temperatur angepasst wird.

> [!NOTE]
> Informationen zum Installieren und Verwenden der Visual Studio-Erweiterung (VSIX) [C++/WinRT](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio support for C++/WinRT, XAML, the VSIX extension, and the NuGet package](../cpp-and-winrt-apis/intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) (Visual Studio-Unterstützung für C++/WinRT, XAML, die VSIX-Erweiterung und das NuGet-Paket).

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe im Zusammenhang mit der Nutzung und Erstellung von Laufzeitklassen mit C++/WinRT findest du unter [Verwenden von APIs mit C++/WinRT](../cpp-and-winrt-apis/consume-apis.md) sowie unter [Erstellen von APIs mit C++/WinRT](../cpp-and-winrt-apis/author-apis.md).

## <a name="create-a-windows-runtime-component-thermometerwrc"></a>Erstellen einer Windows-Runtime Komponente (thermometerwrc)

Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Erstellen Sie ein **Windows-Runtime Component-Projekt (C++/WinRT)** , und nennen Sie es *thermometerwrc* (für "Thermometer Windows-Runtime Component"). Stellen Sie sicher, dass **Platzieren Sie die Projektmappe und das Projekt im selben Verzeichnis** deaktiviert ist. Die neueste allgemein verfügbare Version von Windows SDK (d. h. keine Vorschauversion). Wenn Sie das Projekt *thermometerwrc* benennen, erhalten Sie die einfachste Möglichkeit, die restlichen Schritte in diesem Thema auszuführen. 

Führen Sie noch keinen Buildvorgang für das Projekt aus.

Das neu erstellte Projekt enthält eine Datei namens `Class.idl`. Ändern Sie im Projektmappen-Explorer den Namen der Datei `Thermometer.idl`. (Durch die Umbenennung der Datei vom Typ `.idl` werden automatisch auch die abhängigen Dateien `.h` und `.cpp` umbenannt.) Ersetze den Inhalt von `Thermometer.idl` durch das folgende Listing:

```idl
// Thermometer.idl
namespace ThermometerWRC
{
    runtimeclass Thermometer
    {
        Thermometer();
        void AdjustTemperature(Single deltaFahrenheit);
    };
}
```

Speichern Sie die Datei. Das Projekt wird im Moment nicht bis zum Abschluss erstellt, aber das Erstellen von jetzt ist eine sinnvolle Sache, da es die Quell Code Dateien generiert, in denen Sie die **thermometerlaufzeitklasse** implementieren. Erstelle daher als Nächstes das Projekt. (Die in dieser Phase zu erwartenden Buildfehler sind darauf zurückzuführen, dass `Class.h` und `Class.g.h` nicht gefunden wurden.)

Während des Buildprozesses wird das Tool `midl.exe` ausgeführt, um die Windows-Runtime-Metadatendatei (`\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd`) deiner Komponente zu erstellen. Danach wird das Tool `cppwinrt.exe` (mit der Option `-component`) ausgeführt, um Quellcodedateien zu generieren, die dich bei der Erstellung deiner Komponente unterstützen. Diese Dateien enthalten stubvorgänge, um den Einstieg in die Implementierung der **Thermometer** -Lauf Zeit Klasse zu erleichtern, die Sie in ihrer IDL deklariert haben Diese Stubs sind `\ThermometerWRC\ThermometerWRC\Generated Files\sources\Thermometer.h` und `Thermometer.cpp`.

Klicke mit der rechten Maustaste auf den Projektknoten, und klicke auf **Ordner in Datei-Explorer öffnen**. Dadurch wird der Projektordner im Datei-Explorer geöffnet. Kopiere dort die Stub-Dateien `Thermometer.h` und `Thermometer.cpp` aus dem Ordner `\ThermometerWRC\ThermometerWRC\Generated Files\sources\` in den Ordner mit deinen Projektdateien (`\ThermometerWRC\ThermometerWRC\`), und ersetze die Dateien am Ziel. Als Nächstes öffnen wir `Thermometer.h` und `Thermometer.cpp` und implementieren unsere Laufzeitklasse. `Thermometer.h`Fügen Sie in der-Implementierung (*nicht* der Factory-Implementierung) des **Thermometers**einen neuen privaten Member hinzu.

```cppwinrt
// Thermometer.h
...
namespace winrt::ThermometerWRC::implementation
{
    struct Thermometer : ThermometerT<Thermometer>
    {
        ...

    private:
        float m_temperatureFahrenheit { 0.f };
    };
}
...
```

Implementieren Sie in `Thermometer.cpp` die Methode "- **Temperatur** ", wie in der folgenden Liste gezeigt.

```cppwinrt
// Thermometer.cpp
...
namespace winrt::ThermometerWRC::implementation
{
    void Thermometer::AdjustTemperature(float deltaFahrenheit)
    {
        m_temperatureFahrenheit += deltaFahrenheit;
    }
}
```

Sie müssen auch `static_assert` aus beiden Dateien löschen.

Sollte die Erstellung aufgrund von Warnungen nicht möglich sein, behebe entweder die Ursachen, oder lege die Projekteigenschaft **C/C++**  > **Allgemein** > **Warnungen als Fehler behandeln** auf **Nein (/WX-)** fest, und erstelle das Projekt erneut.

## <a name="create-a-core-app-thermometercoreapp-to-test-the-windows-runtime-component"></a>Erstellen Sie eine Core-app (thermometercoreapp), um die Windows-Runtime Komponente zu testen.

Erstellen Sie nun ein neues Projekt (entweder in Ihrer *thermometerwrc* -Projekt Mappe oder in einer neuen). Erstellen Sie ein Core-App-Projekt **(C++/WinRT)** , und nennen Sie es *thermometercoreapp*. Legen Sie *thermometercoreapp* als Startprojekt fest, wenn sich die beiden Projekte in derselben Projekt Mappe befinden.

> [!NOTE]
> Wie bereits erwähnt, wird die Windows-Runtime Metadatendatei für die Windows-Runtime Komponente (deren Projekt Sie " *thermometerwrc*" genannt haben) im Ordner erstellt `\ThermometerWRC\Debug\ThermometerWRC\` . Das erste Segment dieses Pfads ist der Name des Ordners, der die Projektmappendatei enthält, das nächste Segment ist dessen Unterverzeichnis namens `Debug`, das letzte Segment ist das Unterverzeichnis, das nach der Komponente für Windows-Runtime benannt ist. Wenn Sie Ihr Projekt nicht mit dem Namen *thermometerwrc*benennen, befindet sich Ihre Metadatendatei im Ordner `\<YourProjectName>\Debug\<YourProjectName>\` .

Fügen Sie nun in Ihrem Core-App-Projekt (*thermometercoreapp*) einen Verweis hinzu, und navigieren Sie zu `\ThermometerWRC\Debug\ThermometerWRC\ThermometerWRC.winmd` (oder fügen Sie einen Projekt-zu-Projekt-Verweis hinzu, wenn sich die beiden Projekte in derselben Projekt Mappe befinden). Klicke auf **Hinzufügen** und anschließend auf **OK**. Erstellen Sie nun *thermometercoreapp*. Im unwahrscheinlichen Fall, dass eine Fehlermeldung angezeigt wird, dass die Nutz Last Datei `readme.txt` nicht vorhanden ist, schließen Sie diese Datei aus dem Projekt Windows-Runtime Komponente aus, erstellen Sie Sie neu, und erstellen Sie dann *thermometercoreapp*neu

Während des Buildprozesses wird das Tool `cppwinrt.exe` ausgeführt, um die referenzierte Datei vom Typ `.winmd` zu Quellcodedateien mit projizierten Typen zu verarbeiten, die dich bei der Nutzung deiner Komponente unterstützen. Der Header für die projizierten Typen für die Laufzeitklassen deiner Komponente (`ThermometerWRC.h`) wird im Ordner `\ThermometerCoreApp\ThermometerCoreApp\Generated Files\winrt\` generiert.

Schließe diesen Header in `App.cpp` ein.

```cppwinrt
// App.cpp
...
#include <winrt/ThermometerWRC.h>
...
```

`App.cpp`Fügen Sie außerdem in den folgenden Code hinzu, um ein **thermometerobjekt** (mithilfe des Standardkonstruktors des projizierten Typs) zu instanziieren und eine Methode für das thermometerobjekt aufzurufen.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    ThermometerWRC::Thermometer m_thermometer;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_thermometer.AdjustTemperature(1.f);
        ...
    }
    ...
};
```

Jedes Mal, wenn Sie auf das Fenster klicken, erhöhen Sie die Temperatur des thermometerobjekts. Sie können Haltepunkte festlegen, wenn Sie den Code schrittweise durchlaufen möchten, um zu bestätigen, dass die Anwendung tatsächlich die Windows-Runtime Komponente aufruft.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Ihrer C++/WinRT Windows-Runtime-Komponente noch mehr Funktionalität oder neue Windows-Runtime Typen hinzufügen möchten, können Sie die oben gezeigten Muster befolgen. Verwenden Sie zuerst IDL, um die Funktionalität zu definieren, die Sie verfügbar machen möchten. Erstellen Sie dann das Projekt in Visual Studio, um eine Stub-Implementierung zu generieren. Und schließen Sie die Implementierung dann entsprechend ab. Alle Methoden, Eigenschaften und Ereignisse, die Sie in IDL definieren, sind für die Anwendung sichtbar, die Ihre Windows-Runtime Komponente verwendet. Weitere Informationen zu IDL finden Sie unter [Einführung in Microsoft Interface Definition Language 3,0](/uwp/midl-3/intro).

Ein Beispiel für das Hinzufügen eines Ereignisses zur Windows-Runtime Komponente finden Sie unter [Verfassen von Ereignissen in C++/WinRT](../cpp-and-winrt-apis/author-events.md).

## <a name="troubleshooting"></a>Problembehandlung

| Symptom | Problembehandlung |
|---------|--------|
|Wenn in einer C++/WinRT-App eine [C#-Komponente für Windows-Runtime](./creating-windows-runtime-components-in-csharp-and-visual-basic.md), die XAML verwendet, verarbeitet wird, erzeugt der Compiler einen Fehler der Form „ *'MyNamespace_XamlTypeInfo': is not a member of 'winrt::MyNamespace'* “, wobei *MyNamespace* der Name des Namespace der Windows Runtime-Komponente ist. | Fügen Sie in `pch.h` in der verarbeitenden C++/WinRT-App `#include <winrt/MyNamespace.MyNamespace_XamlTypeInfo.h>` hinzu, und ersetzen Sie *MyNamespace* entsprechend. |
