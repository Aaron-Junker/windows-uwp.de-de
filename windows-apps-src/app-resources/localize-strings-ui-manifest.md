---
Description: Wenn Sie möchten, dass Ihre App verschiedene Anzeigesprachen unterstützt und Ihr Code oder XAML-Markup- oder App-Paketmanifest Zeichenfolgenliterale enthält, verschieben Sie diese Zeichenfolgen in eine Ressourcendatei (.resw). Sie können dann eine übersetzte Kopie dieser Ressourcendatei für jede Sprache erstellen, die Ihre App unterstützt.
title: Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.date: 11/01/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 6412416fe2d73c4fc313a32fdc2929fff9dc51c4
ms.sourcegitcommit: d37a543cfd7b449116320ccfee46a95ece4c1887
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/16/2019
ms.locfileid: "68270089"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest

Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md).

Wenn Sie möchten, dass Ihre App verschiedene Anzeigesprachen unterstützt und Ihr Code oder XAML-Markup- oder App-Paketmanifest Zeichenfolgenliterale enthält, verschieben Sie diese Zeichenfolgen in eine Ressourcendatei (.resw). Sie können dann eine übersetzte Kopie dieser Ressourcendatei für jede Sprache erstellen, die Ihre App unterstützt.

Hartcodierte Zeichenfolgenliterale können in imperativem Code oder in XAML-Markup stehen, z. B. die **Text**-Eigenschaft für einen **TextBlock**. Sie können auch in der Quelldatei des App-Paketmanifests (Datei `Package.appxmanifest`) auftreten, z. B. als Wert für den Anzeigenamen auf der Registerkarte „Anwendung” im Manifest-Designer von Visual Studio. Verschieben Sie diese Zeichenfolgen in eine Ressourcendatei (.resw), und ersetzen Sie die hartcodierten Zeichenfolgenliterale in Ihrer App und in Ihrem Manifest durch Verweise auf Ressourcenbezeichner.

Während eine Bildressourcendatei immer nur eine Bildressource enthält, sind in einer Zeichenfolgen-Ressourcendatei *mehrere* Zeichenfolgenressourcen enthalten. Eine Zeichenfolgen-Ressourcendatei ist die Art von Ressourcendatei (.resw), die Sie üblicherweise in einem \Strings-Ordner Ihres Projekts erstellen. Hintergrundinformationen zur Verwendung von Qualifizierern in den Namen von Ressourcendateien (.resw) finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung und andere Eigenschaften](tailor-resources-lang-scale-contrast.md).

## <a name="store-strings-in-a-resources-file"></a>Store-Zeichenfolgen in einer Ressourcendatei

1. Legen Sie die Standardsprache Ihrer App fest.
    1. Öffnen Sie `Package.appxmanifest`, während Ihre Projektmappe in Visual Studio geöffnet ist.
    2. Vergewissern Sie sich auf der Registerkarte „Anwendung”, dass die Standardsprache passend festgelegt ist (z. B. auf „En” oder „en-US”). Für die verbleibenden Schritte wird davon ausgegangen, dass Sie die Standardsprache auf "en-US" festgelegt haben.
    <br>**Beachten Sie** zumindest müssen Sie die Zeichenfolgenressourcen für diesen Standardsprache lokalisiert angeben. Diese Ressourcen werden geladen, wenn für die bevorzugte Sprache des Benutzers oder die eingestellte Anzeigesprache keine bessere Übereinstimmung gefunden wird.
2. Erstellen Sie eine Ressourcendatei (.resw) für die Standardsprache.
    1. Erstellen Sie unter Ihrem Projektknoten einen neuen Ordner mit dem Namen „Strings”.
    2. Erstellen Sie unter `Strings` einen neuen Unterordner mit dem Namen „en-US”.
    3. Erstellen Sie unter `en-US` eine neue Ressourcendatei (.resw). Vergewissern Sie sich, dass sie „Resources.resw” heißt.
    <br>**Beachten Sie** .NET Ressourcendateien (.resx), die Sie portieren möchten, finden Sie unter [Portieren von XAML und Benutzeroberfläche](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization).
3. Öffnen Sie `Resources.resw`, und fügen Sie diese Zeichenfolgenressourcen hinzu.

    `Strings/en-US/Resources.resw`

    ![Ressource hinzufügen, Englisch](images/addresource-en-us.png)

    In diesem Beispiel ist „Greeting” der Bezeichner einer Zeichenfolgenressource, auf den Sie, wie noch gezeigt wird, im Markup verweisen können. Für den Bezeichner „Greeting” wird eine Zeichenfolge für eine Text-Eigenschaft bereitgestellt, und es wird eine Zeichenfolge für eine Width-Eigenschaft bereitgestellt. „Greeting.Text” ist ein Beispiel für einen Eigenschaftsbezeichner, da er der Eigenschaft eines Benutzeroberflächenelements entspricht. Es wäre auch möglich, „Greeting.Foreground” in der Spalte „Name” hinzuzufügen, beispielsweise mit dem Wert „Red” in der Spalte „Value”. Der Bezeichner „Farewell” ist ein einfacher Bezeichner für eine Zeichenfolgenressource. Er verfügt über keine untergeordneten Eigenschaften und kann, wie noch gezeigt wird, aus imperativem Code geladen werden. Die Spalte „Kommentar” ist gut geeignet, um spezielle Anweisungen für Übersetzer bereitzustellen.

    Da wir in diesem Beispiel mit „Farewell” einen einfachen Bezeichner für Zeichenfolgenressourcen eingetragen haben, können wir *zusätzlich* keine Eigenschaftsbezeichner verwenden, die auf demselben Bezeichner basieren. Das Hinzufügen von „Farewell.Text” würde deshalb dazu führen, dass beim Erstellen von `Resources.resw` der Fehler „Doppelter Eintrag” auftritt.

    Bei Ressourcenbezeichnern wird zwischen Groß-/Kleinschreibung unterschieden, und die Bezeichner müssen pro Ressourcendatei eindeutig sein. Wählen Sie aussagekräftige Ressourcenbezeichner, um für die Übersetzung einen weiteren Kontext bereitzustellen. Ändern Sie die Ressourcenbezeichner nicht, nachdem die Zeichenfolgenressourcen zur Übersetzung weitergegeben wurden. Lokalisierungsteams verfolgen Ergänzungen, Löschungen und Updates an den Ressourcen anhand der Ressourcenbezeichner nach. Bei Änderungen an Ressourcenbezeichnern – auch „Resource Identifiers Shift“ genannt – müssen die Zeichenfolgen neu übersetzt werden, da der Eindruck entsteht, dass Zeichenfolgen gelöscht oder hinzugefügt wurden.

## <a name="refer-to-a-string-resource-identifier-from-xaml"></a>Ein Zeichenfolgenbezeichner für die Ressource von XAML finden Sie unter

Verwenden Sie eine [x:Uid-Direktive](../xaml-platform/x-uid-directive.md), um ein Steuerelement oder ein anderes Element in Ihrem Markup mit dem Bezeichner einer Zeichenfolgenressource zu verknüpfen.

```xaml
<TextBlock x:Uid="Greeting"/>
```

Zur Laufzeit wird `\Strings\en-US\Resources.resw` geladen (da diese momentan die einzige Ressourcendatei im Projekt ist). Die Direktive **x:Uid** im **TextBlock**-Element bewirkt in `Resources.resw` eine Suche nach Eigenschaftsbezeichnern, die den Zeichenfolgenressourcen-Bezeichner „Greeting” enthalten. Die Eigenschaftsbezeichner „Greeting.Text” und „Greeting.Width” werden gefunden, und ihre Werte werden auf den **TextBlock** angewendet, wobei alle Werte, die lokal im Markup festgelegt sind, überschrieben werden. Der Wert „Greeting.Foreground” würde ebenfalls angewendet, wenn Sie ihn hinzugefügt hätten. Es hätte aber keine Auswirkung, wenn Sie in diesem TextBlock-Element **a:Uid** auf „Farewell” festlegen würden, da nur Eigenschaftsbezeichner verwendet werden, um Eigenschaften für XAML-Markupelemente festzulegen. `Resources.resw` *ist* den Ressourcenbezeichner für die Zeichenfolge "Abschied" enthalten, enthält aber keine Eigenschaftsbezeichner für sie.

Sollten Sie einem XAML-Element einen Zeichenfolgeressourcen-Bezeichner zuordnen, müssen Sie sicherstellen, dass *alle* Eigenschaftsbezeichner für diesen Bezeichner für das XAML-Element geeignet sind. Wenn Sie beispielsweise `x:Uid="Greeting"` in einem **TextBlock**-Element verwenden, wird „Greeting.Text” korrekt zugeordnet, da der Typ **TextBlock** über eine Text-Eigenschaft verfügt. Wenn Sie `x:Uid="Greeting"` aber in einem **Button**-Element verwenden, wird „Greeting.Text” einen Laufzeitfehler verursachen, da der Typ **Button** nicht über eine Texteigenschaft verfügt. Eine Lösung für diesen Fall ist, einen Eigenschaftsbezeichner mit dem Namen "ButtonGreeting.Content" zu erstellen und `x:Uid="ButtonGreeting"` im **Button**-Element zu verwenden.

Statt **Width** mithilfe einer Ressourcendatei festzulegen, können Sie veranlassen, dass sich Steuerelemente in ihrer Größe dynamisch an Inhalte anpassen.

**Beachten Sie** für [angefügte Eigenschaften](../xaml-platform/attached-properties-overview.md), benötigen Sie eine spezielle Syntax in der Spalte "Name" in eine resw-Datei. Um beispielsweise einen Wert für die angefügte Eigenschaft [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) des Bezeichners „Greeting” festzulegen, müssen Sie Folgendes in der Spalte „Name” eingeben:

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>Referenzieren eines Bezeichners für Zeichenfolgenressourcen im Code

Sie können eine Zeichenfolgenressource explizit durch Angabe eines einfachen Zeichenfolgenressourcen-Bezeichners laden:

> [!NOTE]
> Wenn Sie einen Aufruf zu einer beliebigen **GetForCurrentView**-Methode haben, die *möglicherweise* für einen Hintergrund-/Arbeitsthread ausgeführt wird, schützen Sie diesen Aufruf mit einem `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)`-Test. Der Aufruf von **GetForCurrentView** aus einem Hintergrund-/Arbeitsthread resultiert in einer Ausnahme mit etwa folgendem Wortlaut: „ *&lt;Typname&gt; kann nicht für Threads erstellt werden, die kein CoreWindow haben*“

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView() };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"Farewell"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView();
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("Farewell");
```

Sie können diesen Code in einer Klassenbibliothek (Universal Windows) oder in einem [Windows Runtime Library (Universal Windows)](../winrt-components/index.md)-Projekt verwenden. Zur Laufzeit werden die Ressourcen der App geladen, die die Bibliothek hostet. Wir empfehlen, dass eine Bibliothek Ressourcen aus der App lädt, die sie hostet, da die App wahrscheinlich einen höheren Lokalisierungsgrad aufweist. Wenn eine Bibliothek Ressourcen bereitstellen muss, sollte sie es der App, die sie hostet, ermöglichen, diese Ressourcen als Eingabe zu ersetzen.

Wenn der Name einer segmentiert werden (es enthält "." Zeichen), dann wird mit ersetzen Sie Punkte mit Schrägstrich ("/") für Zeichen in den Namen der Ressource. -Eigenschaftenbezeichnern enthalten z. B. Punkte; Daher müssten Sie diese Substition zu tun, um eine solche aus Code zu laden.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

Wenn sich nicht sicher sind, können Sie [MakePri.exe](makepri-exe-command-options.md) um PRI-Datei Ihrer app zu speichern. Jeder Ressource `uri` in die ausgegebene Datei angezeigt wird.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>Verweisen auf einen Ressourcenbezeichner aus dem App-Paketmanifest

1. Öffnen Sie Ihre app-Paket-manifest Source-Datei (die `Package.appxmanifest` Datei) in der standardmäßig Ihrer app `Display name` wird als Zeichenfolgenliteral angegeben.

   ![Ressource hinzufügen, Englisch](images/display-name-before.png)

2. Um eine lokalisierbare Version dieser Zeichenfolge zu erstellen, öffnen Sie `Resources.resw` und fügen eine neue Zeichenfolgenressource mit dem Namen „AppDisplayName” und dem Wert „Adventure Works Cycles” hinzu.

3. Ersetzen Sie das Zeichenfolgenliteral für den Anzeigennamen durch einen Verweis auf den Zeichenfolgenressourcen-Bezeichner, den Sie gerade erstellt haben („AppDisplayName”). Verwenden Sie dazu das URI (Uniform Resource Identifier)-Schema `ms-resource`.

   ![Ressource hinzufügen, Englisch](images/display-name-after.png)

4. Wiederholen Sie den Vorgang für alle Zeichenfolgen im Manifest, die Sie lokalisieren möchten. Beispielsweise für den Kurznamen Ihrer App (den Sie so konfigurieren können, dass er im Startmenü auf der App-Kachel angezeigt wird). Eine Liste aller Elemente im App-Paketmanifest, die Sie lokalisieren können, finden Sie unter [Lokalisierbare Manifestelemente](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="localize-the-string-resources"></a>Lokalisieren der Zeichenfolgenressourcen

1. Erstellen Sie eine Kopie der Ressourcendatei (.resw) für eine andere Sprache.
    1. Erstellen Sie unter „Strings” einen neuen Unterordner, und nennen Sie ihn „de-DE” für Deutsch (Deutschland).
   <br>**Beachten Sie** für den Ordnernamen, können Sie alle verwenden [BCP-47-Sprachtag](https://go.microsoft.com/fwlink/p/?linkid=227302). Details zum Sprachqualifizierer und eine Liste häufiger Sprachtags finden Sie unter [Anpassen von Ressourcen mit Qualifizierern für Sprache, Skalierung und anderen](tailor-resources-lang-scale-contrast.md).
   2. Erstellen Sie im Ordner `Strings/de-DE` eine Kopie von `Strings/en-US/Resources.resw`.
2. Übersetzen Sie die Zeichenfolgen.
    1. Öffnen Sie `Strings/de-DE/Resources.resw`, und übersetzen Sie die Werte in der Spalte „Value”. Sie müssen die Kommentare nicht übersetzen.

    `Strings/de-DE/Resources.resw`

    ![Ressource hinzufügen, Deutsch](images/addresource-de-de.png)

Bei Bedarf wiederholen Sie die Schritte 1 und 2 für eine weitere Sprache.

`Strings/fr-FR/Resources.resw`

![Ressource hinzufügen, Französisch](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>Testen der App

Testen Sie die App hinsichtlich der Standardanzeigesprache. Sie können dann die Anzeigesprache unter **Einstellungen** > **Zeit & Sprache** > **Region & Sprache** > **Sprachen** ändern und die App erneut testen. Beachten Sie Zeichenfolgen in der Benutzeroberfläche und auch in der Shell (z. B. den Anzeigenamen in der Titelleiste und den Kurznamen für Ihre Kacheln).

**Hinweis:** Wenn ein Ordnername gefunden wird, der mit der Einstellung für die Anzeigesprache übereinstimmt, wird die Ressourcendatei in diesem Ordner geladen. Andernfalls erfolgt ein Fallback bis auf die Ressourcen für die standardmäßige Sprache Ihrer App.

## <a name="factoring-strings-into-multiple-resources-files"></a>Verteilen von Zeichenfolgen in mehrere Ressourcendateien

Sie können alle Zeichenfolgen in einer einzelnen Ressourcendatei (resw) zusammenfassen oder sie auf mehrere Ressourcendateien verteilen. Beispielsweise können in einer Ressourcendatei Fehlermeldungen ablegen, die Zeichenfolgen für das App-Paketmanifest in einer anderen, und die Zeichenfolgen für die Benutzeroberfläche in einer dritten Ressourcendatei. In diesem Fall würde die Ordnerstruktur wie folgt aussehen.

![Ressource hinzufügen, Englisch](images/manifest-resources.png)

Um den Verweis auf den Bezeichner einer Zeichenfolgenressource auf eine bestimmte Datei zu beziehen, setzen Sie einfach `/<resources-file-name>/` vor den Bezeichner. Im folgenden Markupbeispiel wird davon ausgegangen, dass `ErrorMessages.resw` eine Ressource mit dem Namen „PasswordTooWeak.Text” enthält, deren Wert einen Fehler beschreibt.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

Sie müssen nur hinzufügen `/<resources-file-name>/` vor der Zeichenfolge-Ressourcenbezeichner für Ressourcendateien *außer* `Resources.resw`. Der Grund ist, dass „Resources.resw” der Standarddateiname ist, der verwendet wird, wenn Sie keinen Dateinamen angeben (wie in früheren Beispielen in diesem Thema).

Im folgenden Codebeispiel wird davon ausgegangen, dass `ErrorMessages.resw` eine Ressource mit dem Namen „MismatchedPasswords” enthält, deren Wert einen Fehler beschreibt.

> [!NOTE]
> Wenn Sie einen Aufruf zu einer beliebigen **GetForCurrentView**-Methode haben, die *möglicherweise* für einen Hintergrund-/Arbeitsthread ausgeführt wird, schützen Sie diesen Aufruf mit einem `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)`-Test. Der Aufruf von **GetForCurrentView** aus einem Hintergrund-/Arbeitsthread resultiert in einer Ausnahme mit etwa folgendem Wortlaut: „ *&lt;Typname&gt; kann nicht für Threads erstellt werden, die kein CoreWindow haben*“

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ErrorMessages");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("MismatchedPasswords");
```

```cppwinrt
auto resourceLoader{ Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView(L"ErrorMessages") };
myXAMLTextBlockElement().Text(resourceLoader.GetString(L"MismatchedPasswords"));
```

```cpp
auto resourceLoader = Windows::ApplicationModel::Resources::ResourceLoader::GetForCurrentView("ErrorMessages");
this->myXAMLTextBlockElement->Text = resourceLoader->GetString("MismatchedPasswords");
```

Würden Sie die Ressource „AppDisplayName” aus `Resources.resw` in `ManifestResources.resw` verschieben, müssten Sie in Ihrem App-Paketmanifest `ms-resource:AppDisplayName` in `ms-resource:/ManifestResources/AppDisplayName` ändern.

Wenn Sie ein Dateinamen für die Ressource segmentiert werden (es enthält "." Zeichen), lassen Sie die Punkte im Namen, wenn Sie darauf verweisen. **Keine** ersetzen Sie Punkte mit Schrägstrich ("/")-Zeichen, wie Sie für einen Ressourcennamen.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

Wenn sich nicht sicher sind, können Sie [MakePri.exe](makepri-exe-command-options.md) um PRI-Datei Ihrer app zu speichern. Jeder Ressource `uri` in die ausgegebene Datei angezeigt wird.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>Laden einer Zeichenfolge für eine bestimmte Sprache oder einen anderen Kontext

Der standardmäßige [**ResourceContext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (abgerufen über [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) enthält einen Qualifiziererwert für jeden Qualifizierernamen, der den standardmäßigen Laufzeitkontext darstellt (also die Einstellungen für den aktuellen Benutzer und Computer). Ressourcendateien (.resw) werden anhand der Qualifizierer in ihren Namen mit den Qualifiziererwerten in diesem Laufzeitkontext abgeglichen.

In einigen Fällen wünschen Sie vielleicht, dass Ihre App die Systemeinstellungen überschreiben und Sprache, Skalierung oder andere Qualifiziererwerte explizit angeben kann, wenn sie nach einer passenden Ressourcendatei sucht, um sie zu laden. So könnten Sie es Ihren Benutzern beispielsweise ermöglichen, eine alternative Sprache für QuickInfos oder Fehlermeldungen auszuwählen.

Das ist möglich, indem Sie einen neuen **ResourceContext** erstellen (statt den standardmäßigen Kontext zu verwenden), dessen Werte überschreiben und dann dieses Kontextobjekt bei der Suche nach Zeichenfolgen verwenden.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

Wie **QualifierValues** im obigen Codebeispiel kann jeder Qualifizierer verwendet werden. Für den Sonderfall Language können Sie alternativ wie folgt verfahren.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

Um den gleichen Effekt auf globaler Ebene zu erzielen, *können* Sie die Qualifiziererwerte im standardmäßigen **ResourceContext** außer Kraft setzen. Wir empfehlen, stattdessen [**ResourceContext.SetGlobalQualifierValue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_) aufzurufen. Sie legen die Werte nur einmal mit einem Aufruf von **SetGlobalQualifierValue** fest und diese Werte sind auf dem standardmäßigen **ResourceContext** jedes Mal gültig, wenn Sie ihn für Suchvorgänge verwenden.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

Einige Qualifizierer besitzen einen Systemdatenanbieter. Statt **SetGlobalQualifierValue** aufzurufen, können Sie dann den Anbieter über seine eigene API anpassen. Der folgende Code zeigt an einem Beispiel, wie [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) festzulegen ist.

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>Aktualisieren von Zeichenfolgen als Reaktion auf Änderungen von Qualifiziererwerten

Eine aktive App kann auf Änderungen der Systemeinstellungen reagieren, wenn sich diese Änderungen auf Qualifiziererwerte im standardmäßigen **ResourceContext** auswirken. Die folgenden Systemeinstellungen rufen ein [ **"MapChanged"** ](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)-Ereignis auf [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) auf.

Als Reaktion auf dieses Ereignis können Sie die Zeichenfolgen aus dem standardmäßigen **ResourceContext** laden.

```csharp
public MainPage()
{
    this.InitializeComponent();

    ...

    // Subscribe to the event that's raised when a qualifier value changes.
    var qualifierValues = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues;
    qualifierValues.MapChanged += new Windows.Foundation.Collections.MapChangedEventHandler<string, string>(QualifierValues_MapChanged);
}

private async void QualifierValues_MapChanged(IObservableMap<string, string> sender, IMapChangedEventArgs<string> @event)
{
    var dispatcher = this.myXAMLTextBlockElement.Dispatcher;
    if (dispatcher.HasThreadAccess)
    {
        this.RefreshUIText();
    }
    else
    {
        await dispatcher.RunAsync(Windows.UI.Core.CoreDispatcherPriority.Normal, () => this.RefreshUIText());
    }
}

private void RefreshUIText()
{
    var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
    this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
}
```

## <a name="load-strings-from-a-class-library-or-a-windows-runtime-library"></a>Laden von Zeichenfolgen aus einer Klassenbibliothek oder eine Windows-Runtime-Bibliothek

Die Zeichenfolgenressourcen einer referenzierten Klassenbibliothek (Universal Windows) oder [Windows-Runtime Library (Universal Windows)](../winrt-components/index.md) stehen in der Regel in einem Unterordner des Pakets. Dort werden sie während des Buildprozesses eingefügt. Der Ressourcenbezeichner solche Zeichenfolgen besitzen in der Regel die Form *Bibliotheksname/Ressourcendateiname/Ressourcenbezeichner*.

Eine Bibliothek kann einen ResourceLoader für ihre eigenen Ressourcen abrufen. Der folgende Code veranschaulicht beispielsweise wie entweder eine Bibliothek oder eine app, die darauf verweist ResourceLoader für die Bibliothek Zeichenfolgenressourcen abrufen kann.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

Für eine Windows Runtime-Bibliothek (Universal Windows), wenn der Standardnamespace segmentiert werden (es enthält "." Zeichen), verwenden Sie Punkte in den Namen der Ressource.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

Sie müssen nicht dies für eine Klassenbibliothek (Universal Windows). Wenn Sie unsicher sind, die Sie angeben können [MakePri.exe-Befehlszeilenoptionen](makepri-exe-command-options.md) um der Komponente oder der Bibliothek PRI-Datei zu speichern. Jeder Ressource `uri` in die ausgegebene Datei angezeigt wird.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>Laden von Zeichenfolgen aus anderen Paketen

Die Ressourcen für ein app-Paket verwaltet und erfolgt über der Pakets besitzen auf oberster Ebene [**ResourceMap** ](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) zugegriffen werden kann aus dem aktuellen [**ResourceManager-Dienst** ](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live). Verschiedene Komponenten können in jedes Pakets eigene ResourceMap Unterstrukturen ein, das Sie, über zugreifen können verwenden [ **ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live).

Ein Frameworkpaket kann auf seine eigenen Ressourcen über einen absolute Ressourcenbezeichner-URI zugreifen. Weitere Informationen finden Sie unter [URI-Schemas](uri-schemes.md).

## <a name="loading-strings-in-non-packaged-applications"></a>Laden von Zeichenfolgen in nicht gepackte Anwendungen

Nicht gepackte Anwendungen können auch das Ressourcenverwaltungssystem nutzen, ab der Windows-Version 1903 (Mai 2019-Update).

Erstellen Sie einfach Ihren UWP Benutzer Steuerelemente/Bibliotheken und [alle Zeichenfolgen in einer Ressourcendatei speichern](#store-strings-in-a-resources-file). Anschließend können Sie [XAML finden Sie in ein Ressourcenbezeichner Zeichenfolge](#refer-to-a-string-resource-identifier-from-xaml), [Code finden Sie in ein Ressourcenbezeichner Zeichenfolge](#refer-to-a-string-resource-identifier-from-code), oder [Laden von Zeichenfolgen aus einer Klassenbibliothek oder eine Windows-Runtime-Bibliothek](#load-strings-from-a-class-library-or-a-windows-runtime-library).

Um Ressourcen in nicht gepackte Anwendungen zu verwenden, sollten Sie Folgendes tun:

1. Verwendung [GetForViewIndependentUse](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.resourceloader.getforviewindependentuse) anstelle von [GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) beim Auflösen von Ressourcen aus Code, wie es ist keine *aktuelle Ansicht* in Szenarien nicht gepackt. Die folgende Ausnahme auftritt, wenn Sie aufrufen [GetForCurrentView](https://docs.microsoft.com/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) in Szenarien nicht enthalten: *-Ressourcenkontexten verwendet möglicherweise nicht für Threads erstellt werden, die nicht über ein "corewindow" verfügen.*
1. Verwendung [MakePri.exe](https://docs.microsoft.com/windows/uwp/app-resources/compile-resources-manually-with-makepri) resources.pri-Datei Ihrer app manuell zu erstellen.
    - Ausführen von `makepri new /pr <PROJECTROOT> /cf <PRICONFIG> /of resources.pri`
    - Die &lt;PRICONFIG&gt; dürfen die "&lt;paketerstellung&gt;" im Abschnitt, damit alle Ressourcen in einer einzelnen resources.pri-Datei gebündelt werden. Wenn die Standardeinstellung verwenden [MakePri.exe-Konfigurationsdatei](https://docs.microsoft.com/windows/uwp/app-resources/makepri-exe-configuration) erstellt [Createconfig](https://docs.microsoft.com/windows/uwp/app-resources/makepri-exe-command-options#createconfig-command), müssen gelöscht werden der "&lt;paketerstellung&gt;" im Abschnitt manuell, nachdem es erstellt wurde.
    - Die &lt;PRICONFIG&gt; muss alle relevanten Indexer erforderlich, um alle Ressourcen in Ihrem Projekt in eine Datei für die einzelnen resources.pri merge enthalten. Der Standardwert [MakePri.exe-Konfigurationsdatei](https://docs.microsoft.com/windows/uwp/app-resources/makepri-exe-configuration) erstellt [Createconfig](https://docs.microsoft.com/windows/uwp/app-resources/makepri-exe-command-options#createconfig-command) alle Indexer enthält.
    - Wenn Sie die Standardkonfiguration nicht verwenden, stellen Sie sicher, dass der Indexer PRI aktiviert ist (Überprüfen Sie die Standardkonfiguration für hierzu) PRIs gefunden von UWP-Projekt-verweisen, NuGet-Verweise und So weiter, zusammenführen, die sich in das Stammverzeichnis des Projekts befinden.
        > [!NOTE]
        > Durch Auslassen `/IndexName`, und das Projekt, ohne dass ein app-Manifest, der IndexName/Root-Namespace, der die PRI-Datei automatisch auf festgelegt ist *Anwendung*, die der Laufzeit für nicht gepackte apps kompatibel ist (Dies entfernt die Vorherige harte Abhängigkeit für Paket-ID). Beim Angeben der Ressourcen-URIs, ms-Resource: / / / Verweise, die den Stamm-Namespace auslassen ableiten *Anwendung* als den Stammnamespace für nicht gepackte apps (oder Sie können angeben, *Anwendung* explizit als in ms-Resource://Application/).
1. Kopieren Sie die PRI-Datei in das Buildausgabeverzeichnis der .exe
1. Führen Sie die .exe 
    > [!NOTE]
    > Das Ressourcenverwaltungssystem verwendet die Anzeigesprache System, anstatt die bevorzugten Sprache Benutzerliste beim Auflösen von Ressourcen auf der Sprache nicht gepackte apps basieren. Die bevorzugte Sprache Benutzerliste wird nur für UWP-apps verwendet werden.

> [!Important]
> Sie müssen manuell PRI-Dateien neu erstellen, wenn Ressourcen geändert werden. Es wird empfohlen, ein Post-Build-Skript, das verarbeitet die [MakePri.exe](https://docs.microsoft.com/windows/uwp/app-resources/compile-resources-manually-with-makepri) -Befehl aus, und die resources.pri-Ausgabe in die .exe-Verzeichnis kopiert.

## <a name="important-apis"></a>Wichtige APIs
* [ApplicationModel.Resources.ResourceLoader](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Resources.ResourceLoader)
* [ResourceContext.SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [Portieren von XAML und Benutzeroberfläche](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [X: Uid-Anweisung](../xaml-platform/x-uid-directive.md)
* [Angefügte Eigenschaften](../xaml-platform/attached-properties-overview.md)
* [Lokalisierbaren manifest Elementen](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [BCP-47-Sprachtag.](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [Passen Sie Ihre Ressourcen für die Sprache, Skalierung und andere Kennzeichner](tailor-resources-lang-scale-contrast.md)
* [Gewusst wie: Laden von Zeichenfolgenressourcen](https://docs.microsoft.com/previous-versions/windows/apps/hh965323(v=win.10))
