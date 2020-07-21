---
title: Windows-Runtime Komponenten mit C++/WinRT
description: In diesem Thema wird gezeigt, wie [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) verwendet wird, um eine Windows-Runtime Komponente zu erstellen und zu nutzen, &mdash; die von einer universellen Windows-app aufgerufen werden kann, die mit einer beliebigen Windows-Runtime Sprache erstellt wurde.
ms.date: 07/06/2020
ms.topic: article
keywords: Windows 10, UWP, Windows, Runtime, Komponente, Komponenten, Windows-Runtime Komponente, WRC, C++/WinRT
ms.localizationpriority: medium
ms.openlocfilehash: e47175579fcfc5544587ff36baaaa653003c4c63
ms.sourcegitcommit: c1226b6b9ec5ed008a75a3d92abb0e50471bb988
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/20/2020
ms.locfileid: "86494149"
---
# <a name="windows-runtime-components-with-cwinrt"></a>Windows-Runtime Komponenten mit C++/WinRT

In diesem Thema wird gezeigt, wie [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) verwendet wird, um eine Windows-Runtime Komponente zu erstellen und zu nutzen, &mdash; die von einer universellen Windows-app aufgerufen werden kann, die mit einer beliebigen Windows-Runtime Sprache erstellt wurde.

Es gibt verschiedene Gründe für das Entwickeln einer Windows-Runtime Komponente in C++/WinRT.
- Um die Leistungsvorteile von C++ in komplexen oder rechenintensiven Vorgängen zu nutzen.
- Zum wieder verwenden von Standard-C++-Code, der bereits geschrieben und getestet wurde.
- Zum verfügbar machen von Win32-Funktionen für eine in geschriebene universelle Windows-Plattform (UWP)-app, z. b. c#.

Wenn Sie die C++/WinRT-Komponente erstellen, können Sie im allgemeinen Typen aus der C++-Standardbibliothek und integrierte Typen verwenden, außer an der Grenze der Anwendungs Binärschnittstelle (ABI), an die Sie Daten an den Code in einem anderen `.winmd` Paket übergeben. Verwenden Sie in der ABI Windows-Runtime Typen. Verwenden Sie außerdem im C++/WinRT-Code Typen wie Delegat und Ereignis, um Ereignisse zu implementieren, die von der Komponente ausgelöst und in einer anderen Sprache behandelt werden können. Weitere Informationen zu C++/WinRT. finden Sie unter [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) .

Im restlichen Teil dieses Themas erfahren Sie, wie Sie eine Windows-Runtime Komponente in C++/WinRT erstellen und dann in einer Anwendung verwenden.

Die Windows-Runtime Komponente, die Sie in diesem Thema erstellen, enthält eine Lauf Zeit Klasse, die ein Bankkonto darstellt. Das Thema veranschaulicht auch eine Core-APP, die die Bankkonto-Lauf Zeit Klasse nutzt und eine Funktion aufruft, um das Guthaben anzupassen.

> [!NOTE]
> Informationen zum Installieren und Verwenden der Visual Studio-Erweiterung (VSIX) [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) und des NuGet-Pakets (die zusammen die Projektvorlage und Buildunterstützung bereitstellen) findest du unter [Visual Studio support for C++/WinRT, XAML, the VSIX extension, and the NuGet package](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) (Visual Studio-Unterstützung für C++/WinRT, XAML, die VSIX-Erweiterung und das NuGet-Paket).

> [!IMPORTANT]
> Wichtige Konzepte und Begriffe im Zusammenhang mit der Nutzung und Erstellung von Laufzeitklassen mit C++/WinRT findest du unter [Verwenden von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/consume-apis) sowie unter [Erstellen von APIs mit C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-apis).

## <a name="create-a-windows-runtime-component-bankaccountwrc"></a>Erstellen einer Komponente für Windows-Runtime (BankAccountWRC)

Erstelle zunächst ein neues Projekt in Microsoft Visual Studio. Erstelle ein Projekt vom Typ **Leere App (C++/WinRT)** , und nenne es *BankAccountWRC* (Bankkonto-Komponente für Windows-Runtime). Stellen Sie sicher, dass **Projekt Mappe und Projekt in demselben Verzeichnis platzieren** nicht aktiviert ist. Die neueste allgemein verfügbare Version von Windows SDK (d. h. keine Vorschauversion). Wenn du das Projekt *BankAccountWRC* benennst, erleichtert dies die Ausführung der restlichen Schritte in diesem Thema. 

Führen Sie noch keinen Buildvorgang für das Projekt aus.

Das neu erstellte Projekt enthält eine Datei namens `Class.idl`. Ändern Sie im Projektmappen-Explorer den Namen der Datei `BankAccount.idl`. (Durch die Umbenennung der Datei vom Typ `.idl` werden automatisch auch die abhängigen Dateien `.h` und `.cpp` umbenannt.) Ersetze den Inhalt von `BankAccount.idl` durch das folgende Listing:

> [!NOTE]
> Natürlich wäre es nicht so, dass Sie auf diese Weise Finanzsoftware für die Produktion implementieren. Wir verwenden `Single` in diesem Beispiel nur zur einfacheren Verwendung.

```idl
// BankAccountWRC.idl
namespace BankAccountWRC
{
    runtimeclass BankAccount
    {
        BankAccount();
        void AdjustBalance(Single value);
    };
}
```

Speichern Sie die Datei. Im aktuellen Zustand wird das Projekt zwar nicht vollständig erstellt, die Erstellung ist jedoch hilfreich, da dadurch die Quellcodedateien generiert werden, in denen die Laufzeitklasse **BankAccount** implementiert wird. Erstelle daher als Nächstes das Projekt. (Die in dieser Phase zu erwartenden Buildfehler sind darauf zurückzuführen, dass `Class.h` und `Class.g.h` nicht gefunden wurden.)

Während des Buildprozesses wird das Tool `midl.exe` ausgeführt, um die Windows-Runtime-Metadatendatei (`\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd`) deiner Komponente zu erstellen. Danach wird das Tool `cppwinrt.exe` (mit der Option `-component`) ausgeführt, um Quellcodedateien zu generieren, die dich bei der Erstellung deiner Komponente unterstützen. Diese Dateien enthalten Stubs zur Implementierung der Laufzeitklasse **BankAccount**, die du in deiner IDL deklariert hast. Diese Stubs sind `\BankAccountWRC\BankAccountWRC\Generated Files\sources\BankAccount.h` und `BankAccount.cpp`.

Klicke mit der rechten Maustaste auf den Projektknoten, und klicke auf **Ordner in Datei-Explorer öffnen**. Dadurch wird der Projektordner im Datei-Explorer geöffnet. Kopiere dort die Stub-Dateien `BankAccount.h` und `BankAccount.cpp` aus dem Ordner `\BankAccountWRC\BankAccountWRC\Generated Files\sources\` in den Ordner mit deinen Projektdateien (`\BankAccountWRC\BankAccountWRC\`), und ersetze die Dateien am Ziel. Als Nächstes öffnen wir `BankAccount.h` und `BankAccount.cpp` und implementieren unsere Laufzeitklasse. `BankAccount.h`Fügen Sie in der-Implementierung (*nicht* der Factory-Implementierung) von **BankAccount**ein neues privates Mitglied hinzu.

```cppwinrt
// BankAccount.h
...
namespace winrt::BankAccountWRC::implementation
{
    struct BankAccount : BankAccountT<BankAccount>
    {
        ...

    private:
        float m_balance{ 0.f };
    };
}
...
```

Implementieren Sie in `BankAccount.cpp` die Methode " **accessbalance** ", wie in der folgenden Liste gezeigt.

```cppwinrt
// BankAccount.cpp
...
namespace winrt::BankAccountWRC::implementation
{
    void BankAccount::AdjustBalance(float value)
    {
        m_balance += value;
    }
}
```

Sie müssen auch `static_assert` aus beiden Dateien löschen.

Sollte die Erstellung aufgrund von Warnungen nicht möglich sein, behebe entweder die Ursachen, oder lege die Projekteigenschaft **C/C++**  > **Allgemein** > **Warnungen als Fehler behandeln** auf **Nein (/WX-)** fest, und erstelle das Projekt erneut.

## <a name="create-a-core-app-bankaccountcoreapp-to-test-the-windows-runtime-component"></a>Erstellen einer Core-App (BankAccountCoreApp) zum Testen der Komponente für Windows-Runtime

Erstelle ein neues Projekt (entweder in der Projektmappe *BankAccountWRC* oder in einer neuen Projektmappe). Erstelle ein Projekt vom Typ **Core-App (C++/WinRT)** , und nenne es *BankAccountCoreApp*. Legen Sie *BankAccountCoreApp* als Startprojekt fest, wenn sich beide Projekte in derselben Projektmappe befinden.

> [!NOTE]
> Wie bereits erwähnt, wird die Metadatendatei der Windows-Runtime für die Komponente für Windows-Runtime (deren Projekt *BankAccountWRC* benannt wurde) im Ordner `\BankAccountWRC\Debug\BankAccountWRC\` erstellt. Das erste Segment dieses Pfads ist der Name des Ordners, der die Projektmappendatei enthält, das nächste Segment ist dessen Unterverzeichnis namens `Debug`, das letzte Segment ist das Unterverzeichnis, das nach der Komponente für Windows-Runtime benannt ist. Wenn das Projekt nicht *BankAccountWRC-* genannt wurde, befindet sich die Metadatendatei im Ordner `\<YourProjectName>\Debug\<YourProjectName>\`.

Füge jetzt in deinem Core App-Projekt (*BankAccountCoreApp*) einen Verweis hinzu, und navigiere zu `\BankAccountWRC\Debug\BankAccountWRC\BankAccountWRC.winmd` (oder füge einen Verweis von Projekt zu Projekt hinzu, falls sich die beiden Projekte in der gleichen Projektmappe befinden). Klicke auf **Hinzufügen** und anschließend auf **OK**. Führe als Nächstes einen Buildvorgang für *BankAccountCoreApp* aus. Sollte ein Fehler mit dem Hinweis angezeigt werden, dass die Nutzlastdatei `readme.txt` nicht vorhanden ist, schließe diese Datei aus dem Projekt „Komponente für Windows-Runtime“ aus, erstelle es neu, und erstelle anschließend *BankAccountCoreApp* neu. (Dieser Fehler ist allerdings nicht sehr wahrscheinlich.)

Während des Buildprozesses wird das Tool `cppwinrt.exe` ausgeführt, um die referenzierte Datei vom Typ `.winmd` zu Quellcodedateien mit projizierten Typen zu verarbeiten, die dich bei der Nutzung deiner Komponente unterstützen. Der Header für die projizierten Typen für die Laufzeitklassen deiner Komponente (`BankAccountWRC.h`) wird im Ordner `\BankAccountCoreApp\BankAccountCoreApp\Generated Files\winrt\` generiert.

Schließe diesen Header in `App.cpp` ein.

```cppwinrt
// App.cpp
...
#include <winrt/BankAccountWRC.h>
...
```

`App.cpp`Fügen Sie außerdem in den folgenden Code hinzu, um ein **BankAccount** -Objekt (mithilfe des Standardkonstruktors des projizierten Typs) zu instanziieren und eine Methode für das Bankkonto "Bank Account" aufzurufen.

```cppwinrt
struct App : implements<App, IFrameworkViewSource, IFrameworkView>
{
    BankAccountWRC::BankAccount m_bankAccount;
    ...
    
    void OnPointerPressed(IInspectable const &, PointerEventArgs const & args)
    {
        m_bankAccount.AdjustBalance(1.f);
        ...
    }
    ...
};
```

Jedes Mal, wenn Sie auf das Fenster klicken, erhöhen Sie den Saldo des Bank Konto Objekts. Sie können Haltepunkte festlegen, wenn Sie den Code schrittweise durchlaufen möchten, um zu bestätigen, dass die Anwendung tatsächlich die Windows-Runtime Komponente aufruft.

## <a name="next-steps"></a>Nächste Schritte

Wenn Sie Ihrer C++/WinRT Windows-Runtime-Komponente noch mehr Funktionalität oder neue Windows-Runtime Typen hinzufügen möchten, können Sie die oben gezeigten Muster befolgen. Verwenden Sie zuerst IDL, um die Funktionalität zu definieren, die Sie verfügbar machen möchten. Erstellen Sie dann das Projekt in Visual Studio, um eine Stub-Implementierung zu generieren. Und schließen Sie die Implementierung dann entsprechend ab. Alle Methoden, Eigenschaften und Ereignisse, die Sie in IDL definieren, sind für die Anwendung sichtbar, die Ihre Windows-Runtime Komponente verwendet. Weitere Informationen zu IDL finden Sie unter [Einführung in Microsoft Interface Definition Language 3,0](/uwp/midl-3/intro).

Ein Beispiel für das Hinzufügen eines Ereignisses zur Windows-Runtime Komponente finden Sie unter [Verfassen von Ereignissen in C++/WinRT](/windows/uwp/cpp-and-winrt-apis/author-events).
