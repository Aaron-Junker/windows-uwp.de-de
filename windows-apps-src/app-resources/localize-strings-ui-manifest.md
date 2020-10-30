---
description: Wenn Sie möchten, dass Ihre App verschiedene Anzeigesprachen unterstützt und Ihr Code oder XAML-Markup- oder App-Paketmanifest Zeichenfolgenliterale enthält, verschieben Sie diese Zeichenfolgen in eine Ressourcendatei (.resw). Sie können dann eine übersetzte Kopie dieser Ressourcendatei für jede Sprache erstellen, die Ihre App unterstützt.
title: Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest
ms.assetid: E420B9BB-C0F6-4EC0-BA3A-BA2875B69722
label: Localize strings in your UI and app package manifest
template: detail.hbs
ms.date: 11/01/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 63e88322c7a85bba1ca1a4ff8ff5f2186c4da9f8
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031833"
---
# <a name="localize-strings-in-your-ui-and-app-package-manifest"></a>Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest

Weitere Informationen zu einer Werterhöhung Ihrer App durch Lokalisierung finden Sie unter [Globalisierung und Lokalisierung](../design/globalizing/globalizing-portal.md).

Wenn Sie möchten, dass Ihre App verschiedene Anzeigesprachen unterstützt und Ihr Code oder XAML-Markup- oder App-Paketmanifest Zeichenfolgenliterale enthält, verschieben Sie diese Zeichenfolgen in eine Ressourcendatei (.resw). Sie können dann eine übersetzte Kopie dieser Ressourcendatei für jede Sprache erstellen, die Ihre App unterstützt.

Hart codierte Zeichen folgen Literale können in imperativem Code oder in XAML-Markup vorkommen, z. b. als **Text** -Eigenschaft eines **TextBlock** . Sie können auch in der Quelldatei des App-Pakets (der `Package.appxmanifest` Datei) angezeigt werden, z. b. als Wert für Anzeige Name auf der Registerkarte Anwendung des Manifest-Designers von Visual Studio. Verschieben Sie diese Zeichen folgen in eine Ressourcen Datei (. resw), und ersetzen Sie die hart codierten Zeichen folgen Literale in der APP und im Manifest durch Verweise auf Ressourcen Bezeichner.

Im Gegensatz zu Bild Ressourcen, bei denen nur eine Bildressource in einer Bild Ressourcen Datei enthalten ist, sind *mehrere* Zeichen folgen Ressourcen in einer Zeichen folgen Ressourcen Datei enthalten. Eine Zeichen folgen Ressourcen Datei ist eine Ressourcen Datei (. resw), und Sie erstellen diese Art von Ressourcen Datei in der Regel in einem Ordner "\strings" in Ihrem Projekt. Hintergrundinformationen zur Verwendung von Qualifizierern in den Namen Ihrer Ressourcen Dateien (. resw) finden [Sie unter Anpassen von Ressourcen für Sprache, Skalierung und andere Qualifizierer](tailor-resources-lang-scale-contrast.md).

## <a name="store-strings-in-a-resources-file"></a>Speichern von Zeichen folgen in einer Ressourcen Datei

1. Legen Sie die Standardsprache Ihrer APP fest.
    1. Öffnen Sie die Projekt Mappe in Visual Studio, und öffnen Sie `Package.appxmanifest` .
    2. Vergewissern Sie sich auf der Registerkarte Anwendung, dass die Standardsprache entsprechend festgelegt ist (z. b. "en" oder "en-US"). Bei den restlichen Schritten wird davon ausgegangen, dass Sie die Standardsprache auf "en-US" festgelegt haben.
    <br>**Hinweis** Sie müssen mindestens Zeichen folgen Ressourcen bereitstellen, die für diese Standardsprache lokalisiert werden. Dabei handelt es sich um die Ressourcen, die geladen werden, wenn für die bevorzugte Sprache des Benutzers oder für die Anzeige Sprache keine bessere Übereinstimmung gefunden werden kann.
2. Erstellen Sie eine Ressourcen Datei (. resw) für die Standardsprache.
    1. Erstellen Sie unter dem Projekt Knoten einen neuen Ordner, und nennen Sie ihn "Strings".
    2. `Strings`Erstellen Sie unter einen neuen Unterordner, und nennen Sie ihn "en-US".
    3. `en-US`Erstellen Sie unter eine neue Ressourcen Datei (. resw), und vergewissern Sie sich, dass Sie den Namen "Resources. resw" hat.
    <br>**Hinweis** Wenn Sie über .NET-Ressourcen Dateien (RESX-Dateien) verfügen, die Sie portieren möchten, finden Sie weitere Informationen unter [Portieren von XAML und UI](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization).
3. Öffnen `Resources.resw` Sie, und fügen Sie diese Zeichen folgen Ressourcen hinzu.

    `Strings/en-US/Resources.resw`

    ![Screenshot der Add-Ressourcen Tabelle der Zeichen folgen > E N U S > Resources. resw-Datei.](images/addresource-en-us.png)

    In diesem Beispiel ist "Gruß" ein Zeichen folgen Ressourcen Bezeichner, auf den Sie aus Ihrem Markup verweisen können, wie wir zeigen. Für den Bezeichner "Gruß" wird eine Zeichenfolge für eine Text Eigenschaft bereitgestellt, und für eine Width-Eigenschaft wird eine Zeichenfolge bereitgestellt. "Gruß. Text" ist ein Beispiel für einen Eigenschafts Bezeichner, da es einer Eigenschaft eines UI-Elements entspricht. Sie können z. b. "Gruß. Vordergrund" in der Spalte "Name" hinzufügen und den Wert auf "rot" festlegen. Der "Farewell"-Bezeichner ist ein einfacher Zeichen folgen Ressourcen Bezeichner. Sie verfügt über keine untergeordneten Eigenschaften und kann aus imperativem Code geladen werden, wie wir zeigen werden. Die comment-Spalte ist ein guter Ort, um den Konvertierungsprogramm spezielle Anweisungen bereitzustellen.

    Da in diesem Beispiel ein einfacher Zeichen folgen Ressourcen-bezeichnereintrag mit dem Namen "Farewell" vorhanden ist, können wir *auch* keine Eigenschafts-IDs auf Grundlage desselben Bezeichners haben. Das Hinzufügen von "Farewell. Text" würde also beim Aufbau einen doppelten Eingabefehler verursachen `Resources.resw` .

    Bei Ressourcen bezeichnerfällen wird die Groß-/Kleinschreibung nicht beachtet, und die Ressourcen müssen eindeutig sein. Stellen Sie sicher, dass Sie aussagekräftige Ressourcen Bezeichner verwenden, um zusätzlichen Kontext für Konvertierer bereitzustellen. Und ändern Sie die Ressourcen Bezeichner nicht, nachdem die Zeichen folgen Ressourcen für die Übersetzung gesendet wurden. Lokalisierungsteams verfolgen Ergänzungen, Löschungen und Updates an den Ressourcen anhand der Ressourcenbezeichner nach. Änderungen in Ressourcen Bezeichner, &mdash; die auch als "Ressourcen Bezeichner verschieben" bezeichnet &mdash; werden, erfordern das erneute übersetzen von Zeichen folgen, da Sie so angezeigt werden, als ob Zeichen folgen gelöscht und andere hinzugefügt wurden.

## <a name="refer-to-a-string-resource-identifier-from-xaml"></a>Verweisen auf einen Zeichen folgen Ressourcen Bezeichner aus XAML

Sie verwenden eine [x:UID-Direktive](../xaml-platform/x-uid-directive.md) , um ein Steuerelement oder ein anderes Element im Markup einem Zeichen folgen Ressourcen Bezeichner zuzuordnen.

```xaml
<TextBlock x:Uid="Greeting"/>
```

Zur Laufzeit `\Strings\en-US\Resources.resw` wird geladen (da momentan die einzige Ressourcen Datei im Projekt ist). Die **x:UID-Direktive** im **TextBlock** bewirkt, dass eine Suche durchgeführt wird, um Eigenschafts Bezeichner in zu suchen, die `Resources.resw` den Zeichen folgen Ressourcen Bezeichner "Gruß" enthalten. Die Eigenschaften Bezeichner "Gruß. Text" und "Gruß. Width" werden gefunden, und ihre Werte werden auf den **TextBlock** angewendet, wobei alle Werte überschrieben werden, die lokal im Markup festgelegt sind. Der Wert "Gruß. Vordergrund" würde auch angewendet werden, wenn Sie diesen hinzugefügt haben. Aber nur Eigenschaften Bezeichner werden verwendet, um Eigenschaften für XAML-Markup Elemente festzulegen, sodass das Festlegen von **x:UID** auf "Farewell" für diesen TextBlock keine Auswirkung hat. `Resources.resw`*enthält den* Zeichen folgen Ressourcen Bezeichner "Farewell", enthält aber keine Eigenschaften Bezeichner.

Wenn Sie einem XAML-Element einen Zeichen folgen Ressourcen Bezeichner zuweisen, stellen Sie sicher, dass *alle* Eigenschaften Bezeichner für diesen Bezeichner für das XAML-Element geeignet sind. Wenn Sie z. b. für `x:Uid="Greeting"` einen **TextBlock** festlegen, wird "Gruß. Text" aufgelöst, da der **TextBlock** -Typ eine Text-Eigenschaft aufweist. Wenn Sie jedoch `x:Uid="Greeting"` für eine **Schaltfläche** festlegen, führt "Gruß. Text" zu einem Laufzeitfehler, da der **Schalt** Flächentyp nicht über eine Text-Eigenschaft verfügt. Eine Lösung für diesen Fall besteht darin, einen Eigenschafts Bezeichner mit dem Namen "buttongruß. Content" zu erstellen und `x:Uid="ButtonGreeting"` auf der **Schaltfläche** festzulegen.

Anstatt die **Breite** aus einer Ressourcen Datei festzulegen, sollten Sie den Steuerelementen die dynamische Größe von Inhalten erlauben.

**Hinweis** Für [angefügte Eigenschaften](../xaml-platform/attached-properties-overview.md)benötigen Sie eine spezielle Syntax in der Spalte "Name" einer resw-Datei. Wenn Sie z. b. einen Wert für die angefügte Eigenschaft " [**AutomationProperties.Name**](/uwp/api/windows.ui.xaml.automation.automationproperties.NameProperty) " für den Bezeichner "Gruß" festlegen möchten, geben Sie diesen Wert in die Spalte "Name" ein.

```xml
Greeting.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name
```

## <a name="refer-to-a-string-resource-identifier-from-code"></a>Verweisen Sie im Code auf einen Zeichen folgen Ressourcen Bezeichner.

Sie können eine Zeichen folgen Ressource explizit auf Grundlage eines einfachen Zeichen folgen Ressourcen Bezeichners laden.

> [!NOTE]
> Wenn Sie einen Rückruf für eine **getforcurrentview** -Methode durchgeführt haben, die *möglicherweise* für einen Hintergrund-/Workerthread ausgeführt wird, sollten Sie diesen Befehl mit einem `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` Test schützen. Der Aufruf von **getforcurrentview** von einem Hintergrund-/Arbeit-Thread führt zu der Ausnahme " *&lt; tygtame &gt; kann nicht für Threads erstellt werden, die kein corewindow-Fenster aufweisen".*

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

Sie können denselben Code in einer Klassenbibliothek (Universal Windows) oder in einem Windows-Runtime- [Bibliotheksprojekt (Universal Windows)](../winrt-components/index.md) verwenden. Zur Laufzeit werden die Ressourcen der App geladen, die die Bibliothek gehostet. Es wird empfohlen, dass eine Bibliothek Ressourcen von der APP lädt, die Sie hostet, da die APP wahrscheinlich ein höheres Maß an Lokalisierung hat. Wenn eine Bibliothek Ressourcen bereitstellen muss, sollte Sie Ihrer hostingapp die Möglichkeit geben, diese Ressourcen als Eingabe zu ersetzen.

Wenn ein Ressourcen Name segmentiert ist (er enthält die Zeichen "."), ersetzen Sie die Punkte durch Schrägstriche ("/") im Ressourcennamen. Eigenschafts Bezeichner z. b. Punkte enthalten. Daher müssen Sie diese untergeordnete Komponente durchführen, um eine dieser Komponenten aus dem Code zu laden.

```csharp
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Fare/Well"); // <data name="Fare.Well" ...> ...
```

Im Zweifelsfall können Sie [MakePri.exe](makepri-exe-command-options.md) verwenden, um die PRI-Datei Ihrer APP zu sichern. Die einzelnen Ressourcen werden `uri` in der abgekippten Datei angezeigt.

```xml
<ResourceMapSubtree name="Fare"><NamedResource name="Well" uri="ms-resource://<GUID>/Resources/Fare/Well">...
```

## <a name="refer-to-a-string-resource-identifier-from-your-app-package-manifest"></a>Verweisen auf einen Zeichen folgen Ressourcen Bezeichner aus dem App-Paket Manifest

1. Öffnen Sie die Quelldatei für das App-Paket Manifest (die `Package.appxmanifest` Datei), in der die App standardmäßig `Display name` als Zeichenfolgenliterale ausgedrückt wird.

   ![Screenshot der Datei "Package. appxmanifest", die die Registerkarte "Anwendung" mit dem anzeigen Amen "Adventure Works Cycles" anzeigt.](images/display-name-before.png)

2. Um eine lokalisierbare Version dieser Zeichenfolge zu erstellen, öffnen `Resources.resw` Sie, und fügen Sie eine neue Zeichen folgen Ressource mit dem Namen "appdisplayname" und dem Wert "Adventure Works Cycles" hinzu.

3. Ersetzen Sie die Zeichenfolge des Anzeige namens Zeichenfolgenliterals durch einen Verweis auf den soeben erstellten Zeichen folgen Ressourcen Bezeichner ("appdisplayname"). Hierfür verwenden Sie das `ms-resource` Schema URI (Uniform Resource Identifier).

   ![Screenshot der Datei "Package. appxmanifest", die die Registerkarte "Anwendung" mit dem anzeigen Amen "M S Resource App-Anzeige Name" anzeigt.](images/display-name-after.png)

4. Wiederholen Sie diesen Vorgang für jede Zeichenfolge im Manifest, die Sie lokalisieren möchten. Beispielsweise der Kurzname Ihrer APP (die Sie so konfigurieren können, dass Sie auf der Kachel der APP beim Start angezeigt wird). Eine Liste aller Elemente im App-Paket Manifest, die lokalisiert werden können, finden Sie unter [lokalisierbare Manifest-Elemente](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live).

## <a name="localize-the-string-resources"></a>Lokalisieren der Zeichen folgen Ressourcen

1. Erstellen Sie eine Kopie Ihrer Ressourcen Datei (. resw) für eine andere Sprache.
    1. Erstellen Sie unter "Zeichen folgen" einen neuen Unterordner, und nennen Sie ihn "de-de" für Deutsch (Deutschland).
   <br>**Hinweis** Für den Ordnernamen können Sie ein beliebiges [bcp-47-sprach Kennzeichen](https://tools.ietf.org/html/bcp47)verwenden. Ausführliche Informationen zum sprach Qualifizierer und eine Liste der allgemeinen Sprachen Tags finden [Sie unter Anpassen von Ressourcen für Sprache, Skalierung und andere Qualifizierer](tailor-resources-lang-scale-contrast.md) .
   2. Erstellen Sie eine Kopie von `Strings/en-US/Resources.resw` im `Strings/de-DE` Ordner.
2. Übersetzt die Zeichen folgen.
    1. Öffnen `Strings/de-DE/Resources.resw` und übersetzen Sie die Werte in der Spalte Wert. Sie müssen die Kommentare nicht übersetzen.

    `Strings/de-DE/Resources.resw`

    ![Ressource hinzufügen, Deutsch](images/addresource-de-de.png)

Wenn Sie möchten, können Sie die Schritte 1 und 2 für eine weitere Sprache wiederholen.

`Strings/fr-FR/Resources.resw`

![Ressource hinzufügen, Französisch](images/addresource-fr-fr.png)

## <a name="test-your-app"></a>Testen Ihrer App

Testen Sie die App hinsichtlich der Standardanzeigesprache. Anschließend können Sie die Anzeige Sprache in den **Einstellungen**  >  **Zeit & sprach**  >  **Region & Sprachen**  >  **Sprachen** ändern und die APP erneut testen. Sehen Sie sich die Zeichen folgen in der Benutzeroberfläche und auch in der Shell an (z. b. die Titelleiste, &mdash; die ihren anzeigen Amen &mdash; und den Kurznamen auf ihren Kacheln anzeigt).

**Hinweis** Wenn ein Ordnername gefunden werden kann, der mit der Einstellung der Anzeige Sprache übereinstimmt, wird die Ressourcen Datei in diesem Ordner geladen. Andernfalls erfolgt ein Fall Back, das mit den Ressourcen für die Standardsprache Ihrer APP endet.

## <a name="factoring-strings-into-multiple-resources-files"></a>Factoring von Zeichen folgen in mehrere Ressourcen Dateien

Sie können alle Zeichen folgen in einer einzelnen Ressourcen Datei (resw) aufbewahren, oder Sie können Sie über mehrere Ressourcen Dateien hinweg berücksichtigen. Beispielsweise können Sie die Fehlermeldungen in einer Ressourcen Datei, in Ihrem App-Paket Manifest-Zeichen folgen in einem anderen und in den Benutzeroberflächen Zeichenfolgen in einem dritten speichern. In diesem Fall sieht die Ordnerstruktur wie folgt aus.

![Screenshot des Lösungs Panels mit dem Ordner "Adventure Works Cycles > Strings" mit den Ordnern "Deutsch", "Deutsch" und "Französisch" im Gebiets Schema.](images/manifest-resources.png)

Zum Festlegen des Bereichs eines Verweis zeichenbezeichnerverweises auf eine bestimmte Datei fügen Sie einfach `/<resources-file-name>/` vor dem Bezeichner hinzu. Im folgenden Markup Beispiel wird davon ausgegangen, dass `ErrorMessages.resw` eine Ressource mit dem Namen "passwordumoweak. Text" enthält und deren Wert den Fehler beschreibt.

```xaml
<TextBlock x:Uid="/ErrorMessages/PasswordTooWeak"/>
```

Sie müssen nur `/<resources-file-name>/` vor dem Zeichen folgen Ressourcen Bezeichner für andere Ressourcen Dateien *als* hinzufügen `Resources.resw` . Das liegt daran, dass "Resources. resw" der Standard Dateiname ist. Dies wird angenommen, wenn Sie einen Dateinamen weglassen (wie in den vorherigen Beispielen in diesem Thema beschrieben).

Im folgenden Codebeispiel wird davon ausgegangen, dass `ErrorMessages.resw` eine Ressource enthält, deren Name "mismatchedworts" lautet und deren Wert den Fehler beschreibt.

> [!NOTE]
> Wenn Sie einen Rückruf für eine **getforcurrentview** -Methode durchgeführt haben, die *möglicherweise* für einen Hintergrund-/Workerthread ausgeführt wird, sollten Sie diesen Befehl mit einem `if (Windows.UI.Core.CoreWindow.GetForCurrentThread() != null)` Test schützen. Der Aufruf von **getforcurrentview** von einem Hintergrund-/Arbeit-Thread führt zu der Ausnahme " *&lt; tygtame &gt; kann nicht für Threads erstellt werden, die kein corewindow-Fenster aufweisen".*

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

Wenn Sie die Ressource "appdisplayname" aus `Resources.resw` und in verschieben `ManifestResources.resw` , ändern Sie in Ihrem App-Paket Manifest in `ms-resource:AppDisplayName` `ms-resource:/ManifestResources/AppDisplayName` .

Wenn ein Ressourcen Dateiname segmentiert ist (er enthält "."-Zeichen), lassen Sie die Punkte im Namen, wenn Sie darauf verweisen. Ersetzen Sie Punkte **nicht** durch Schrägstriche ("/"), wie für einen Ressourcennamen.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Err.Msgs");
```

Im Zweifelsfall können Sie [MakePri.exe](makepri-exe-command-options.md) verwenden, um die PRI-Datei Ihrer APP zu sichern. Die einzelnen Ressourcen werden `uri` in der abgekippten Datei angezeigt.

```xml
<ResourceMapSubtree name="Err.Msgs"><NamedResource name="MismatchedPasswords" uri="ms-resource://<GUID>/Err.Msgs/MismatchedPasswords">...
```

## <a name="load-a-string-for-a-specific-language-or-other-context"></a>Eine Zeichenfolge für eine bestimmte Sprache oder einen anderen Kontext laden

Der standardmäßige [**resourcecontext**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live) (abgerufen von [**resourcecontext. getforcurrentview**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView)) enthält einen qualifiziererwert für jeden Qualifizierernamen, der den standardmäßigen Lauf Zeit Kontext darstellt (d. h. die Einstellungen für den aktuellen Benutzer und den Computer). Ressourcen Dateien (. resw) werden anhand der Qualifizierer &mdash; in ihren Namen anhand &mdash; der Qualifiziererwerte in diesem Lauf Zeit Kontext abgeglichen.

Es kann jedoch vorkommen, dass Ihre APP die Systemeinstellungen außer Kraft setzen und explizit über die Sprache, Skalierung oder einen anderen qualifiziererwert verfügen soll, der bei der Suche nach einer passenden Ressourcen Datei verwendet werden soll. Beispielsweise können Sie möchten, dass Ihre Benutzer eine alternative Sprache für Quick Infos oder Fehlermeldungen auswählen können.

Dies können Sie erreichen, indem Sie einen neuen **resourcecontext** erstellen (statt den Standardwert zu verwenden), dessen Werte überschreiben und dann dieses Kontext Objekt in den Zeichen folgen suchen.

```csharp
var resourceContext = new Windows.ApplicationModel.Resources.Core.ResourceContext(); // not using ResourceContext.GetForCurrentView
resourceContext.QualifierValues["Language"] = "de-DE";
var resourceMap = Windows.ApplicationModel.Resources.Core.ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
this.myXAMLTextBlockElement.Text = resourceMap.GetValue("Farewell", resourceContext).ValueAsString;
```

Die Verwendung von **qualifiervalues** wie im obigen Codebeispiel funktioniert für jeden Qualifizierer. Für den Sonderfall der Sprache können Sie stattdessen auch dies tun.

```csharp
resourceContext.Languages = new string[] { "de-DE" };
```

Um denselben Effekt auf globaler Ebene zu erzielen, *können* Sie die Qualifiziererwerte in der Standardeinstellung **resourcecontext** überschreiben. Stattdessen wird jedoch empfohlen, [**resourcecontext. setglobalqualifiervalue**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)aufzurufen. Sie legen die Werte einmal mit einem Aufruf von **setglobalqualifiervalue** fest. diese Werte sind dann bei jeder Verwendung für Suchvorgänge für den standardmäßigen **resourcecontext** wirksam.

```csharp
Windows.ApplicationModel.Resources.Core.ResourceContext.SetGlobalQualifierValue("Language", "de-DE");
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView();
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("Farewell");
```

Einige Qualifizierer verfügen über einen Systemdaten Anbieter. Anstatt **setglobalqualifiervalue** aufrufen zu können, können Sie den Anbieter stattdessen über seine eigene API anpassen. Der folgende Code zeigt z. b., wie [**primarylanguageoverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)festgelegt wird.

```csharp
Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride = "de-DE";
```

## <a name="updating-strings-in-response-to-qualifier-value-change-events"></a>Aktualisieren von Zeichen folgen als Reaktion auf Änderungs Ereignisse für qualifiziererwert

Ihre laufende App kann auf Änderungen in Systemeinstellungen reagieren, die sich auf die Qualifiziererwerte in der Standardeinstellung **resourcecontext** auswirken. Jede dieser Systemeinstellungen Ruft das [**mapchanged**](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live) -Ereignis auf [**resourcecontext. qualifiervalues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)auf.

Als Reaktion auf dieses Ereignis können Sie die Zeichen folgen aus dem standardmäßigen **resourcecontext erneut** laden.

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

## <a name="load-strings-from-a-class-library-or-a-windows-runtime-library"></a>Laden von Zeichen folgen aus einer Klassenbibliothek oder einer Windows-Runtime Bibliothek

Die Zeichen folgen Ressourcen einer Klassenbibliothek (Universal Windows), auf die verwiesen wird, oder [Windows-Runtime Bibliothek (Universal Windows)](../winrt-components/index.md) werden in der Regel in einem Unterordner des Pakets hinzugefügt, in dem Sie während des Buildprozesses enthalten sind. Der Ressourcen Bezeichner einer solchen Zeichenfolge hat normalerweise die Form *Libraryname/resourcesfilename/resourceidentifier* .

Eine Bibliothek kann einen resourceloader für Ihre eigenen Ressourcen erhalten. Der folgende Code veranschaulicht z. b., wie eine Bibliothek oder eine APP, die darauf verweist, einen resourceloader für die Zeichen folgen Ressourcen der Bibliothek erhalten kann.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("ContosoControl/Resources");
this.myXAMLTextBlockElement.Text = resourceLoader.GetString("exampleResourceName");
```

Verwenden Sie für eine Windows-Runtime Bibliothek (universelle Windows), wenn der Standard Namespace segmentiert ist ("."-Zeichen enthält), die Punkte im Ressourcen Zuordnungs Namen.

```csharp
var resourceLoader = Windows.ApplicationModel.Resources.ResourceLoader.GetForCurrentView("Contoso.Control/Resources");
```

Dies ist für eine Klassenbibliothek (universelle Windows-Plattform) nicht erforderlich. Im Zweifelsfall können Sie [MakePri.exe Befehlszeilenoptionen](makepri-exe-command-options.md) angeben, um die PRI-Datei Ihrer Komponente oder Bibliothek zu sichern. Die einzelnen Ressourcen werden `uri` in der abgekippten Datei angezeigt.

```xml
<NamedResource name="exampleResourceName" uri="ms-resource://Contoso.Control/Contoso.Control/ReswFileName/exampleResourceName">...
```

## <a name="loading-strings-from-other-packages"></a>Laden von Zeichen folgen aus anderen Paketen

Die Ressourcen für ein App-Paket werden verwaltet, und der Zugriff erfolgt über die eigene [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live) des Pakets, auf die vom aktuellen [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)-Server aus zugegriffen werden kann. Innerhalb jedes Pakets können verschiedene Komponenten über eigene ResourceMap-Unterstrukturen verfügen, auf die Sie über [**ResourceMap. getsubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)zugreifen können.

Ein frameworkpaket kann mit einem absoluten ressourcenbezeichneruri auf seine eigenen Ressourcen zugreifen. Siehe auch [URI-Schemas](uri-schemes.md).

## <a name="loading-strings-in-non-packaged-applications"></a>Laden von Zeichen folgen in nicht gepackten Anwendungen

Ab Windows-Version 1903 (Update vom Mai 2019) können nicht gepackte Anwendungen auch das Ressourcen Verwaltungs System nutzen.

Erstellen Sie einfach Ihre UWP-Benutzer Steuerelemente/-Bibliotheken, und speichern Sie alle Zeichen folgen [in einer Ressourcen Datei](#store-strings-in-a-resources-file). Sie können dann [auf einen Zeichen folgen Ressourcen Bezeichner aus XAML verweisen](#refer-to-a-string-resource-identifier-from-xaml), [einen Zeichen folgen Ressourcen Bezeichner aus dem Code oder Zeichen](#refer-to-a-string-resource-identifier-from-code)folgen [aus einer Klassenbibliothek oder einer Windows-Runtime Bibliothek laden](#load-strings-from-a-class-library-or-a-windows-runtime-library).

Um Ressourcen in nicht verpackten Anwendungen zu verwenden, sollten Sie einige Dinge tun:

1. Verwenden Sie beim Auflösen von Ressourcen aus dem Code [getforviewindependentuse](/uwp/api/windows.applicationmodel.resources.resourceloader.getforviewindependentuse) anstelle von [getforcurrentview](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) , da keine *aktuelle Sicht* in Szenarios ohne Paket vorhanden ist. Die folgende Ausnahme tritt auf, wenn Sie [getforcurrentview](/uwp/api/windows.applicationmodel.resources.resourceloader.getforcurrentview) in Szenarios ohne Paket aufrufen: *Ressourcen Kontexte können nicht für Threads erstellt werden, die nicht über ein corewindow verfügen.*
1. Verwenden Sie [MakePri.exe](./compile-resources-manually-with-makepri.md) , um die Datei "Resources. pri" Ihrer APP manuell zu generieren.
    - Ausführen von `makepri new /pr <PROJECTROOT> /cf <PRICONFIG> /of resources.pri`
    - Die &lt; priconfig &gt; muss den &lt; Abschnitt "Verpacken" weglassen &gt; , damit alle Ressourcen in einer einzigen Datei "Resources. pri" gebündelt werden. Wenn Sie die standardmäßige [MakePri.exe Konfigurationsdatei](./makepri-exe-configuration.md) verwenden, die von " [kreateconfig](./makepri-exe-command-options.md#createconfig-command)" erstellt wurde, müssen Sie den &lt; Abschnitt "Verpacken" manuell löschen, &gt; nachdem er erstellt wurde.
    - Die &lt; priconfig &gt; muss alle relevanten Indexer enthalten, die erforderlich sind, um alle Ressourcen in Ihrem Projekt in einer einzelnen "Resources. pri"-Datei zusammenzuführen. Die standardmäßige [MakePri.exe Konfigurationsdatei](./makepri-exe-configuration.md) , die von " [kreateconfig](./makepri-exe-command-options.md#createconfig-command) " erstellt wurde, enthält alle Indexer.
    - Wenn Sie die Standardkonfiguration nicht verwenden, stellen Sie sicher, dass der PRI-Indexer aktiviert ist (überprüfen Sie die Standardkonfiguration für die Vorgehensweise), um Pris zusammenzuführen, die über UWP-Projekt Verweise, nuget-Verweise usw. gefunden werden, die sich innerhalb des Projekt Stamms befinden.
        > [!NOTE]
        > Wenn `/IndexName` Sie nicht über ein App-Manifest verfügen und das Projekt nicht über ein App-Manifest verfügt, wird der Indexname/Stamm Namespace der PRI-Datei automatisch auf die *Anwendung* festgelegt, die die Laufzeit für nicht gepackte apps versteht (Dadurch wird die vorherige harte Abhängigkeit von der Paket-ID entfernt). Bei der Angabe von Ressourcen-URIs werden MS-Resource:///Verweise, die die Stamm Namespace-ableiten- *Anwendung* als Stamm Namespace für nicht gepackte apps weglassen (oder Sie können die *Anwendung* explizit wie in MS-Resource://Application/angeben).
1. Kopieren Sie die PRI-Datei in das Buildausgabeverzeichnis der exe-Datei.
1. Ausführen der exe- 
    > [!NOTE]
    > Das Ressourcen Verwaltungs System verwendet die System Anzeige Sprache anstelle der Liste der vom Benutzer bevorzugten Sprachen, wenn Ressourcen basierend auf der Sprache in nicht gepackten apps aufgelöst werden. Die Liste Benutzer bevorzugte Sprache wird nur für UWP-Apps verwendet.

> [!Important]
> Sie müssen PRI-Dateien immer dann manuell neu erstellen, wenn Ressourcen geändert werden. Wir empfehlen die Verwendung eines postbuildskripts, das den [MakePri.exe](./compile-resources-manually-with-makepri.md) Befehl behandelt und die Ausgabe "Resources. pri" in das Verzeichnis ". exe" kopiert.

## <a name="important-apis"></a>Wichtige APIs
* [ApplicationModel.Resources.ResourceLoader](/uwp/api/Windows.ApplicationModel.Resources.ResourceLoader)
* [Resourcecontext. setglobalqualifiervalue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.setglobalqualifiervalue?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)
* [MapChanged](/uwp/api/windows.foundation.collections.iobservablemap-2.mapchanged?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [Portieren von XAML und UI](../porting/wpsl-to-uwp-porting-xaml-and-ui.md#localization-and-globalization)
* [x:Uid-Direktive](../xaml-platform/x-uid-directive.md)
* [angefügte Eigenschaften](../xaml-platform/attached-properties-overview.md)
* [Lokalisierbare Manifestelemente](/uwp/schemas/appxpackage/uapmanifestschema/localizable-manifest-items-win10?branch=live)
* [BCP-47-Sprachtag](https://tools.ietf.org/html/bcp47)
* [Passen Sie Ihre Ressourcen für Sprache, Skalierung und andere Qualifizierer an.](tailor-resources-lang-scale-contrast.md)
* [Laden von Zeichenfolgenressourcen](/previous-versions/windows/apps/hh965323(v=win.10))
