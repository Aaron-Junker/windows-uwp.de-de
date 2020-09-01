---
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: Erfahren Sie, wie Sie In-App-Käufe und Testversionen in UWP-Apps aktivieren, die für Versionen vor Windows 10, Version 1607 bestimmt sind.
title: In-App-Käufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace
ms.date: 08/25/2017
ms.topic: article
keywords: UWP, in-App-Käufe, IAPS, Add-ons, Tests, Windows. applicationmodel. Store
ms.localizationpriority: medium
ms.openlocfilehash: 1ee18b6ea4cc8a2e77a970a273bc4a8a51516247
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162354"
---
# <a name="in-app-purchases-and-trials-using-the-windowsapplicationmodelstore-namespace"></a>In-App-Käufe und Testversionen mit dem Windows.ApplicationModel.Store-Namespace

Sie können Mitglieder des [Windows.ApplicationModel.Store](/uwp/api/windows.applicationmodel.store) verwenden, um Ihrer Universal Windows Platform (UWP)-App In-App-Käufe und Testfunktionen hinzuzufügen und die Monetarisierung Ihrer App zu unterstützen. Diese APIs ermöglichen auch den Zugriff auf die Lizenzinformationen für Ihre App.

Die Artikel in diesem Abschnitt enthalten ausführliche Anleitungen und Codebeispiele für die Verwendung der Mitgliedern des **Windows.ApplicationModel.Store**-Namespace für verschiedene häufige Szenarien. Eine Übersicht über die Basiskonzepte im Zusammenhang mit In-App-Käufen in UWP-Apps finden Sie unter [In-App-Käufe und Testversionen](in-app-purchases-and-trials.md). Ein vollständiges Beispiel, das zeigt, wie Sie Testversionen und In-App-Käufe mithilfe des **Windows.ApplicationModel.Store**-Namespace implementieren, finden Sie im [Store-Beispiel](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store).

> [!IMPORTANT]
> Der **Windows. applicationmodel. Store** -Namespace wird nicht mehr mit neuen Features aktualisiert. Wenn Ihr Projekt auf **Windows 10 Anniversary Edition abzielt (10,0; Build 14393)** oder eine neuere Version in Visual Studio (d. h. Sie verwenden Windows 10, Version 1607 oder höher), empfiehlt es sich, stattdessen den [Windows. Services. Store](/uwp/api/windows.services.store) -Namespace zu verwenden. Weitere Informationen finden Sie unter [In-App-Käufe und Testversionen](./in-app-purchases-and-trials.md). Der **Windows. applicationmodel. Store** -Namespace wird nicht in Windows-Desktop Anwendungen unterstützt, die die [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) verwenden, oder in Apps oder spielen, die eine Entwicklungs Sandbox im Partner Center verwenden (z. b. bei jedem Spiel, das in Xbox Live integriert ist). Diese Produkte müssen den **Windows. Services. Store** -Namespace verwenden, um in-App-Käufe und-Testversionen zu implementieren.

## <a name="get-started-with-the-currentapp-and-currentappsimulator-classes"></a>Erste Schritte mit den Klassen CurrentApp und CurrentAppSimulator

Den Haupteinstiegspunkt in den **Windows.ApplicationModel.Store**-Namespace stellt die Klasse [CurrentApp](/uwp/api/windows.applicationmodel.store.currentapp) dar. Diese Klasse stellt statische Eigenschaften und Methoden bereit, die Sie verwenden können, um Informationen für die aktuelle APP und die verfügbaren Add-ons zu erhalten, Lizenzinformationen für die aktuelle APP oder deren Add-ons zu erhalten, eine APP oder ein Add-on für den aktuellen Benutzer zu erwerben und andere Aufgaben auszuführen.

Die [currentapp](/uwp/api/windows.applicationmodel.store.currentapp) -Klasse ruft Ihre Daten aus der Microsoft Store ab, sodass Sie über ein Entwicklerkonto verfügen müssen und die APP im Store veröffentlicht werden muss, bevor Sie diese Klasse in Ihrer APP erfolgreich verwenden können. Vor dem Übermitteln Ihrer App an den Store können Sie den Code mit einer simulierten Version dieser Klasse mit dem Namen [CurrentAppSimulator](/uwp/api/windows.applicationmodel.store.currentappsimulator) testen. Nachdem Sie Ihre APP getestet haben und bevor Sie Sie an den Microsoft Store senden, müssen Sie die Instanzen von **currentappsimulator** durch **currentapp**ersetzen. Ihre App wird nicht zertifiziert, wenn sie **CurrentAppSimulator** verwendet.

Wenn **CurrentAppSimulator** verwendet wird, wird der ursprünglichen Status der Lizenzierung Ihrer App und In-App-Produkte in einer lokalen Datei mit dem Namen WindowsStoreProxy.xml auf Ihrem Entwicklungscomputer beschrieben. Weitere Informationen zu dieser Datei finden Sie unter [Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator](#proxy).

Weitere Informationen zu den allgemeinen Aufgaben, die Sie mit **CurrentApp** und **CurrentAppSimulator** ausführen können, finden Sie in den folgenden Artikeln.

| Thema       | BESCHREIBUNG                 |
|----------------------------|-----------------------------|
| [Ausschließen oder Einschränken von Features in einer Testversion](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | Durch einen kostenlose, zeitlich begrenzte Testversion Ihrer App mit eingeschränkten Features können Sie Ihre Kunden motivieren, auf die Vollversion Ihrer App zu aktualisieren. |
| [Unterstützen des Kaufs von In-App-Produkten](enable-in-app-product-purchases.md)      |  Sie können unabhängig davon, ob Ihre App kostenlos oder kostenpflichtig ist, Inhalte, andere Apps oder neue App-Funktionen (wie das Freischalten des nächsten Levels eines Spiels) direkt in der App verkaufen. Hier zeigen wir Ihnen, wie Sie diese Produkte in Ihrer App aktivieren können.  |
| [Käufe von konsumierbaren In-App-Produkten aktivieren](enable-consumable-in-app-product-purchases.md)      | Sie können In-App-Käufe von Endverbraucherprodukten – Artikel, die gekauft, verwendet und wieder gekauft werden können – über die Store-Handelsplattform anbieten, um Kunden beim Kauf Stabilität und Zuverlässigkeit zu bieten. Dies ist besonders nützlich für Dinge wie spielinterne Währungen (Gold, Münzen usw.), die gekauft und dann zum Erwerben bestimmter Power-Ups verwendet werden können. |
| [Verwalten eines großen Katalogs von In-App-Produkten](manage-a-large-catalog-of-in-app-products.md)      |   Wenn Ihre App einen großen In-App-Produktkatalog enthält, können Sie optional das in diesem Thema beschriebene Verfahren zum Verwalten des Katalogs ausführen.    |
| [Überprüfen von Produktkäufen anhand von Belegen](use-receipts-to-verify-product-purchases.md)      |   Jede Microsoft Store Transaktion, die zu einem erfolgreichen Produkt Einkauf führt, kann optional eine Transaktionsbestätigung zurückgeben, die Informationen zu dem aufgelisteten Produkt und den monetären Kosten für den Kunden liefert. Der Zugriff auf diese Informationen unterstützt Szenarien, in denen Ihre APP überprüfen muss, ob ein Benutzer Ihre APP gekauft hat, oder dass in-App-Produkt Einkäufe aus dem Microsoft Store getätigt wurden. |

<span id="proxy" />

## <a name="using-the-windowsstoreproxyxml-file-with-currentappsimulator"></a>Verwenden der Datei „WindowsStoreProxy.xml“ mit CurrentAppSimulator

Wenn **CurrentAppSimulator** verwendet wird, wird der ursprünglichen Status der Lizenzierung Ihrer App und In-App-Produkte in einer lokalen Datei mit dem Namen WindowsStoreProxy.xml auf Ihrem Entwicklungscomputer beschrieben. Durch **CurrentAppSimulator**-Methoden, die den Status der App verändern, beispielsweise durch den Kauf einer Lizenz oder die Verarbeitung eines In-App-Kaufs, wird lediglich der Status des **CurrentAppSimulator**-Objekts im Speicher aktualisiert. Der Inhalt von WindowsStoreProxy.xml wird nicht geändert. Wenn die App erneut gestartet wird, wird der Lizenzstatus auf den Status zurückgesetzt, der in WindowsStoreProxy.xml beschrieben ist.

Eine WindowsStoreProxy.xml Datei wird standardmäßig an folgendem Speicherort erstellt:%USERPROFILE%\appdata\local\packages \\ &lt; App Package Folder &gt; \localstate\microsoft\windows store\apidata. Sie können diese Datei bearbeiten, um das Szenario zu definieren, das Sie in den **CurrentAppSimulator**-Eigenschaften simulieren möchten.

Auch wenn Sie die Werte in dieser Datei ändern können, wird empfohlen, dass Sie eine eigene WindowsStoreProxy.xml-Datei (in einem Datenordner des Visual Studio-Projekts) zur Verwendung durch **CurrentAppSimulator** erstellen. Rufen Sie [ReloadSimulatorAsync](/uwp/api/windows.applicationmodel.store.currentappsimulator.reloadsimulatorasync) auf, um Ihre Datei zu laden, wenn Sie die Transaktion simulieren. Wenn Sie **ReloadSimulatorAsync** nicht aufrufen, um Ihre eigene WindowsStoreProxy.xml-Datei zu laden, erstellt/lädt **CurrentAppSimulator** die standardmäßige WindowsStoreProxy.xml-Datei (überschreibt sie jedoch nicht).

> [!NOTE]
> Beachten Sie, dass **currentappsimulator** nicht vollständig initialisiert ist, bis **reloadsimulatorasync** abgeschlossen ist. Und da **ReloadSimulatorAsync** eine asynchrone Methode ist, müssen Sie darauf achten, eine Racebedingung zu vermeiden, in der **CurrentAppSimulator** in einem Thread abgefragt und gleichzeitig in einem anderen Thread initialisiert wird. Eine Möglichkeit besteht darin, ein Flag verwenden, um anzugeben, dass die Initialisierung abgeschlossen ist. Eine APP, die aus dem Microsoft Store installiert wird, muss **currentapp** anstelle von **currentappsimulator**verwenden, und in diesem Fall wird **reloadsimulatorasync** nicht aufgerufen. Daher gilt die eben erwähnte Racebedingung nicht. Entwerfen Sie aus diesem Grund den Code so, dass er in beiden Fällen sowohl asynchron als auch synchron funktioniert.


<span id="proxy-examples" />

### <a name="examples"></a>Beispiele

In diesem Beispiel wird eine WindowsStoreProxy.xml-Datei (UTF-16-codiert) gezeigt, die eine App mit einem Testmodus beschreibt, der am 19. Januar 2015 um 05:00 Uhr (UTC) abläuft.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="UTF-16"?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>2B14D306-D8F8-4066-A45B-0FB3464C67F2</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/2B14D306-D8F8-4066-A45B-0FB3464C67F2</LinkUri>
      <CurrentMarket>en-US</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with a trial license</Name>
        <Description>Sample app for demonstrating trial license management</Description>
        <Price>4.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>true</IsTrial>
      <ExpirationDate>2015-01-19T05:00:00.00Z</ExpirationDate>
    </App>
  </LicenseInformation>
  <Simulation SimulationMode="Automatic">
    <DefaultResponse MethodName="LoadListingInformationAsync_GetResult" HResult="E_FAIL"/>
  </Simulation>
</CurrentApp>
```

Im nächsten Beispiel wird eine WindowsStoreProxy.xml-Datei (UTF-16-codiert) gezeigt, die eine App beschreibt, die gekauft wurde, ein Feature besitzt, das am 19. Januar 2015 um 05:00 (UTC) abläuft und über einen konsumierbaren In-App-Kauf verfügt.

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-16" ?>
<CurrentApp>
  <ListingInformation>
    <App>
      <AppId>988b90e4-5d4d-4dea-99d0-e423e414ffbc</AppId>
      <LinkUri>http://apps.windows.microsoft.com/app/988b90e4-5d4d-4dea-99d0-e423e414ffbc</LinkUri>
      <CurrentMarket>en-us</CurrentMarket>
      <AgeRating>3</AgeRating>
      <MarketData xml:lang="en-us">
        <Name>App with several in-app products</Name>
        <Description>Sample app for demonstrating an expiring in-app product and a consumable in-app product</Description>
        <Price>5.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </App>
    <Product ProductId="feature1" LicenseDuration="10" ProductType="Durable">
      <MarketData xml:lang="en-us">
        <Name>Expiring Item</Name>
        <Price>1.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
    <Product ProductId="consumable1" LicenseDuration="0" ProductType="Consumable">
      <MarketData xml:lang="en-us">
        <Name>Consumable Item</Name>
        <Price>2.99</Price>
        <CurrencySymbol>$</CurrencySymbol>
      </MarketData>
    </Product>
  </ListingInformation>
  <LicenseInformation>
    <App>
      <IsActive>true</IsActive>
      <IsTrial>false</IsTrial>
    </App>
    <Product ProductId="feature1">
      <IsActive>true</IsActive>
      <ExpirationDate>2015-01-19T00:00:00.00Z</ExpirationDate>
    </Product>
  </LicenseInformation>
  <ConsumableInformation>
    <Product ProductId="consumable1" TransactionId="00000001-0000-0000-0000-000000000000" Status="Active"/>
  </ConsumableInformation>
</CurrentApp>
```


<span id="proxy-schema" />

### <a name="schema"></a>Schema

In diesem Abschnitt wird die XSD-Datei aufgelistet, die die Struktur der WindowsStoreProxy.xml-Datei definiert. Um dieses Schema beim Arbeiten mit Ihrer WindowsStoreProxy.xml-Datei auf den XML-Editor in Visual Studio anzuwenden, führen Sie folgende Schritte aus:

1. Öffnen Sie die WindowsStoreProxy.xml-Datei in Microsoft Visual Studio.
2. Klicken Sie im Menü **XML** auf **Schema erstellen**. Hierdurch wird eine temporäre WindowsStoreProxy.xsd-Datei auf Grundlage des Inhalts der XML-Datei erstellt.
3. Ersetzen Sie den Inhalt dieser XSD-Datei durch das folgende Schema.
4. Speichern Sie die Datei an einem Ort, an dem Sie sie auf mehrere App-Projekte anwenden können.
5. Wechseln Sie zur WindowsStoreProxy.xml-Datei in Visual Studio.
6. Klicken Sie im Menü **XML** auf **Schemas**, und suchen Sie die Zeile in der Liste für die WindowsStoreProxy.xsd-Datei. Wenn der Speicherort der Datei nicht der gewünschte ist (wenn beispielsweise die temporäre Datei weiterhin angezeigt wird), klicken Sie auf **Hinzufügen**. Navigieren Sie zur richtigen Datei, und klicken Sie dann auf **OK**. Nun sollte Ihnen diese Datei in der Liste angezeigt werden. Stellen Sie sicher, dass in der Spalte **Verwenden** für dieses Schema ein Häkchen angezeigt wird.

Anschließend unterliegen Ihre Bearbeitungen der WindowsStoreProxy.xml-Datei dem Schema. Weitere Informationen finden Sie unter [So wählen Sie die zu verwendenden XML-Schemas aus](/visualstudio/xml-tools/how-to-select-the-xml-schemas-to-use).

> [!div class="tabbedCodeSnippets"]
```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:import namespace="http://www.w3.org/XML/1998/namespace"/>
  <xs:element name="CurrentApp" type="CurrentAppDefinition"></xs:element>
  <xs:complexType name="CurrentAppDefinition">
    <xs:sequence>
      <xs:element name="ListingInformation" type="ListingDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LicenseInformation" type="LicenseDefinition" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ConsumableInformation" type="ConsumableDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Simulation" type="SimulationDefinition" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:simpleType name="ResponseCodes">
    <xs:restriction base="xs:string">
      <xs:enumeration value="S_OK">
        <xs:annotation>
          <xs:documentation>0x00000000</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_INVALIDARG">
        <xs:annotation>
          <xs:documentation>0x80070057</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_CANCELLED">
        <xs:annotation>
          <xs:documentation>0x800704C7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_FAIL">
        <xs:annotation>
          <xs:documentation>0x80004005</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="E_OUTOFMEMORY">
        <xs:annotation>
          <xs:documentation>0x8007000E</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
      <xs:enumeration value="ERROR_ALREADY_EXISTS">
        <xs:annotation>
          <xs:documentation>0x800700B7</xs:documentation>
        </xs:annotation>
      </xs:enumeration>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="ConsumableStatus">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Active"/>
      <xs:enumeration value="PurchaseReverted"/>
      <xs:enumeration value="PurchasePending"/>
      <xs:enumeration value="ServerError"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="StoreMethodName">
    <xs:restriction base="xs:string">
      <xs:enumeration value="RequestAppPurchaseAsync_GetResult" id="RPPA"/>
      <xs:enumeration value="RequestProductPurchaseAsync_GetResult" id="RFPA"/>
      <xs:enumeration value="LoadListingInformationAsync_GetResult" id="LLIA"/>
      <xs:enumeration value="ReportConsumableFulfillmentAsync_GetResult" id="RPFA"/>
      <xs:enumeration value="LoadListingInformationByKeywordsAsync_GetResult" id="LLIKA"/>
      <xs:enumeration value="LoadListingInformationByProductIdAsync_GetResult" id="LLIPA"/>
      <xs:enumeration value="GetUnfulfilledConsumablesAsync_GetResult" id="GUC"/>
      <xs:enumeration value="GetAppReceiptAsync_GetResult" id="GARA"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:simpleType name="SimulationMode">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Interactive"/>
      <xs:enumeration value="Automatic"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ListingDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppListingDefinition"/>
      <xs:element name="Product" type="ProductListingDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ConsumableDefinition">
    <xs:sequence>
      <xs:element name="Product" type="ConsumableProductDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppListingDefinition">
    <xs:sequence>
      <xs:element name="AppId" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="LinkUri" type="xs:anyURI" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrentMarket" type="xs:language" minOccurs="1" maxOccurs="1"/>
      <xs:element name="AgeRating" type="xs:unsignedInt" minOccurs="1" maxOccurs="1"/>
      <xs:element name="MarketData" type="MarketSpecificAppData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="MarketSpecificAppData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="MarketSpecificProductData">
    <xs:sequence>
      <xs:element name="Name" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="Price" type="xs:float" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencySymbol" type="xs:string" minOccurs="1" maxOccurs="1"/>
      <xs:element name="CurrencyCode" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Description" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Tag" type="xs:string" minOccurs="0" maxOccurs="1"/>
      <xs:element name="Keywords" type="KeywordDefinition" minOccurs="0" maxOccurs="1"/>
      <xs:element name="ImageUri" type="xs:anyURI" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute ref="xml:lang" use="required"/>
  </xs:complexType>
  <xs:complexType name="ProductListingDefinition">
    <xs:sequence>
      <xs:element name="MarketData" type="MarketSpecificProductData" minOccurs="1" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="LicenseDuration" type="xs:integer" use="optional"/>
    <xs:attribute name="ProductType" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:simpleType name="guid">
    <xs:restriction base="xs:string">
      <xs:pattern value="[\da-fA-F]{8}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{4}-[\da-fA-F]{12}"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:complexType name="ConsumableProductDefinition">
    <xs:attribute name="ProductId" use="required">
      <xs:simpleType>
        <xs:restriction base="xs:string">
          <xs:maxLength value="100"/>
          <xs:pattern value="[^,]*"/>
        </xs:restriction>
      </xs:simpleType>
    </xs:attribute>
    <xs:attribute name="TransactionId" type="guid" use="required"/>
    <xs:attribute name="Status" type="ConsumableStatus" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="LicenseDefinition">
    <xs:sequence>
      <xs:element name="App" type="AppLicenseDefinition"/>
      <xs:element name="Product" type="ProductLicenseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="AppLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="IsTrial" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
  </xs:complexType>
  <xs:complexType name="ProductLicenseDefinition">
    <xs:sequence>
      <xs:element name="IsActive" type="xs:boolean" minOccurs="1" maxOccurs="1"/>
      <xs:element name="ExpirationDate" type="xs:dateTime" minOccurs="0" maxOccurs="1"/>
    </xs:sequence>
    <xs:attribute name="ProductId" type="xs:string" use="required"/>
    <xs:attribute name="OfferId" type="xs:string" use="optional"/>
  </xs:complexType>
  <xs:complexType name="SimulationDefinition" >
    <xs:sequence>
      <xs:element name="DefaultResponse" type="DefaultResponseDefinition" minOccurs="0" maxOccurs="unbounded"/>
    </xs:sequence>
    <xs:attribute name="SimulationMode" type="SimulationMode" use="optional"/>
  </xs:complexType>
  <xs:complexType name="DefaultResponseDefinition">
    <xs:attribute name="MethodName" type="StoreMethodName" use="required"/>
    <xs:attribute name="HResult" type="ResponseCodes" use="required"/>
  </xs:complexType>
  <xs:complexType name="KeywordDefinition">
    <xs:sequence>
      <xs:element name="Keyword" type="xs:string" minOccurs="0" maxOccurs="10"/>
    </xs:sequence>
  </xs:complexType>
</xs:schema>
```


<span id="proxy-descriptions" />

### <a name="element-and-attribute-descriptions"></a>Element- und Attributbeschreibungen

In diesem Abschnitt werden die Elemente und Attribute in der WindowsStoreProxy.xml-Datei beschrieben.

Das Stammelement dieser Datei ist das **CurrentApp**-Element, das die aktuelle App darstellt. Dieses Element enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Quantity (Menge)  |  BESCHREIBUNG   |
|-------------|------------|--------|--------|
|  [ListingInformation](#listinginformation)  |    Ja        |  1  |  Enthält Daten aus dem App Eintrag.            |
|  [LicenseInformation](#licenseinformation)  |     Ja       |   1    |   Beschreibt die Lizenzen, die für diese App verfügbar sind, und ihre dauerhaften Add-Ons.     |
|  [ConsumableInformation](#consumableinformation)  |      Nein      |   0 oder 1   |   Beschreibt die konsumierbaren Add-Ons, die für diese App verfügbar sind.      |
|  [Simulation](#simulation)  |     Nein       |      0 oder 1      |   Beschreibt, wie Aufrufe verschiedener [CurrentAppSimulator](/uwp/api/windows.applicationmodel.store.currentappsimulator)-Methoden während der Testphase in der App funktionieren.    |

<span id="listinginformation" />

#### <a name="listinginformation-element"></a>ListingInformation-Element

Dieses Element enthält Daten aus dem App Eintrag. **ListingInformation** ist ein erforderliches untergeordnetes Element des **CurrentApp**-Elements.

**ListingInformation** enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Quantity (Menge)  |  BESCHREIBUNG   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-listinginformation)  |    Ja   |  1   |    Stellt Daten zur App bereit.         |
|  [Produkt](#product-child-of-listinginformation)  |    Nein  |  0 oder mehr   |      Beschreibt ein Add-On für die App.     |     |

<span id="app-child-of-listinginformation"/>

#### <a name="app-element-child-of-listinginformation"></a>App-Element (untergeordnetes Element von ListingInformation)

Dieses Element beschreibt die App-Lizenz. **App** ist ein erforderliches untergeordnetes Element des [ListingInformation](#listinginformation)-Elements.

**App** enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Quantity (Menge)  | BESCHREIBUNG   |
|-------------|------------|--------|--------|
|  **AppId**  |    Ja   |  1   |   Die GUID, die die App im Store identifiziert. Dies kann eine beliebige GUID für Tests sein.        |
|  **LinkUri**  |    Ja  |  1   |    Der URI der Eintragsseite im Store. Dies kann ein beliebiger URI für Tests sein.         |
|  **CurrentMarket**  |    Ja  |  1   |    Das Land/die Region des Kunden.         |
|  **AgeRating**  |    Ja  |  1   |     Eine ganze Zahl, die die Mindestaltersfreigabe der App darstellt. Dies ist derselbe Wert, den Sie in Partner Center angeben würden, wenn Sie die APP übermitteln. Die vom Store verwendeten Werte sind: 3, 7, 12 und 16. Weitere Informationen zu diesen Bewertungen finden Sie unter [Altersfreigaben](../publish/age-ratings.md).        |
|  [MarketData](#marketdata-child-of-app)  |    Ja  |  Mindestens 1      |    Enthält Informationen über die App für ein bestimmtes Land oder eine bestimmte Region. Für jedes Land/jede Region, in denen die App eingetragen ist, müssen Sie ein **MarketData**-Element einschließen.       |    |

<span id="marketdata-child-of-app"/>

#### <a name="marketdata-element-child-of-app"></a>MarketData-Element (untergeordnetes Element von App)

Dieses Element stellt Informationen zur App für ein bestimmtes Land oder eine bestimmte Region bereit. Für jedes Land/jede Region, in denen die App eingetragen ist, müssen Sie ein **MarketData**-Element einschließen. **MarketData** ist ein erforderliches untergeordnetes Element des [App](#app-child-of-listinginformation)-Elements.

**MarketData** enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Quantity (Menge)  | BESCHREIBUNG   |
|-------------|------------|--------|--------|
|  **Name**  |    Ja   |  1   |   Der Name der App in diesem Land/dieser Region.        |
|  **Beschreibung**  |    Ja  |  1   |      Die Beschreibung der App für dieses Land/diese Region.       |
|  **Preis**  |    Ja  |  1   |     Der Preis der App in diesem Land/dieser Region.        |
|  **CurrencySymbol**  |    Ja  |  1   |     Das Währungssymbol, das in diesem Land/dieser Region verwendet wird.        |
|  **Vorkommnisse**  |    Nein  |  0 oder 1      |      Der Währungscode, der in diesem Land/dieser Region verwendet wird.         |  |

**MarketData** hat die folgenden Attribute.

|  attribute  |  Erforderlich  |  BESCHREIBUNG   |
|-------------|------------|----------------|
|  **xml:lang**  |    Ja        |     Gibt das Land/die Region an, für das/die die Marktdateninformationen gelten.          |  |

<span id="product-child-of-listinginformation"/>

#### <a name="product-element-child-of-listinginformation"></a>Produktelement (untergeordnetes Element von ListingInformation.)

Dieses Element beschreibt ein Add-On für die App. **Product** ist ein optionales untergeordnetes Element des [ListingInformation](#listinginformation) -Elements und enthält mindestens ein [MarketData](#marketdata-child-of-product)-Element.

**Product** hat die folgenden Attribute.

|  attribute  |  Erforderlich  |  BESCHREIBUNG   |
|-------------|------------|----------------|
|  **ProductId**  |    Ja        |    Enthält die Zeichenfolge, die von der App zur Identifizierung des Add-Ons verwendet wird.           |
|  **LicenseDuration**  |    Nein        |    Gibt die Anzahl der Tage an, für die die Lizenz gültig sein wird, nachdem der Artikel gekauft wurde. Das Ablaufdatum der neuen, durch einen Produktkauf erstellten Lizenz ist das Kaufdatum plus Lizenzdauer. Dieses Attribut wird nur verwendet, wenn das **ProductType**-Attribut **Durable** ist. Dieses Attribut wird für konsumierbare Add-Ons ignoriert.           |
|  **ProductType**  |    Nein        |    Enthält einen Wert für die Identifizierung der Persistenz des In-App-Produkts. Die unterstützten Werte sind **Durable** (Standard) und **Consumable**. Zusätzliche Informationen zu Typen dauerhafter Add-Ons werden durch ein [Product](#product-child-of-licenseinformation)-Element unter [LicenseInformation](#licenseinformation) beschrieben. Zusätzliche Informationen zu Typen konsumierbarer Add-Ons werden durch ein [Product](#product-child-of-consumableinformation)-Element unter [ConsumableInformation](#consumableinformation) beschrieben.           |  |

<span id="marketdata-child-of-product"/>

#### <a name="marketdata-element-child-of-product"></a>MarketData-Element (untergeordnetes Element von Product)

Dieses Element stellt Informationen zum Add-On für ein bestimmtes Land oder eine bestimmte Region bereit. Für jedes Land/jede Region, in denen das Add-On eingetragen ist, müssen Sie ein **MarketData**-Element einschließen. **MarketData** ist ein erforderliches untergeordnetes Element des [Product](#product-child-of-listinginformation)-Elements.

**MarketData** enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Quantity (Menge)  | BESCHREIBUNG   |
|-------------|------------|--------|--------|
|  **Name**  |    Ja   |  1   |   Der Name des Add-Ons in diesem Land/dieser Region.        |
|  **Preis**  |    Ja  |  1   |     Der Preis des Add-Ons in diesem Land/dieser Region.        |
|  **CurrencySymbol**  |    Ja  |  1   |     Das Währungssymbol, das in diesem Land/dieser Region verwendet wird.        |
|  **Vorkommnisse**  |    Nein  |  0 oder 1      |      Der Währungscode, der in diesem Land/dieser Region verwendet wird.         |  
|  **Beschreibung**  |    Nein  |   0 oder 1   |      Die Beschreibung des Add-Ons für dieses Land/diese Region.       |
|  **Tag**  |    Nein  |   0 oder 1   |      Die [benutzerdefinierten Daten](../publish/enter-add-on-properties.md#custom-developer-data) (auch als „Tag“ bezeichnet) für das Add-On.       |
|  **Schlüsselwörter**  |    Nein  |   0 oder 1   |      Enthält bis zu 10 **Keyword**-Elemente, die die [Schlüsselwörter](../publish/enter-add-on-properties.md#keywords) für das Add-On enthalten.       |
|  **Image-URI**  |    Nein  |   0 oder 1   |      Der [URI für das Bild](../publish/create-add-on-store-listings.md#icon) im Add-On-Eintrag.           |  |

**MarketData** hat die folgenden Attribute.

|  attribute  |  Erforderlich  |  BESCHREIBUNG   |
|-------------|------------|----------------|
|  **xml:lang**  |    Ja        |     Gibt das Land/die Region an, für das/die die Marktdateninformationen gelten.          |  |

<span id="licenseinformation"/>

#### <a name="licenseinformation-element"></a>LicenseInformation-Element

Dieses Element beschreibt die Lizenzen, die für diese App und deren dauerhafte In-App-Produkte verfügbar sind. **LicenseInformation** ist ein erforderliches untergeordnetes Element des **CurrentApp**-Elements.

**LicenseInformation** enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Quantity (Menge)  | BESCHREIBUNG   |
|-------------|------------|--------|--------|
|  [App](#app-child-of-licenseinformation)  |    Ja   |  1   |    Beschreibt die App-Lizenz.         |
|  [Produkt](#product-child-of-licenseinformation)  |    Nein  |  0 oder mehr   |      Beschreibt den Lizenzstatus eines dauerhaften Add-Ons in der App.         |   |

Die folgende Tabelle zeigt, wie Sie einige häufige Bedingungen simulieren, indem Sie Werte unter den Elementen **App** und **Product** kombinieren.

|  Zu simulierende Bedingung  |  IsActive  |  IsTrial  | ExpirationDate   |
|-------------|------------|--------|--------|
|  Vollständig lizenziert  |    true   |  false  |    Nicht vorhanden. Es kann vorhanden sein und ein späteres Datum angeben. Es wird jedoch empfohlen, das Element nicht in die XML-Datei einzuschließen. Wenn es vorhanden ist und ein Datum in der Vergangenheit angibt, wird **IsActive** ignoriert und als „false“ bewertet.          |
|  Im Testzeitraum  |    true  |  true   |      &lt;ein DateTime-Wert in der Zukunft &gt; dieses Element muss vorhanden sein, da " **istrial** " "true" ist. Sie können eine Website aufrufen, die die aktuelle koordinierte Weltzeit (UTC) anzeigt, um festzustellen, auf wie weit in der Zukunft dies festgelegt werden muss, um den gewünschten verbleibenden Testzeitraum zu erhalten.         |
|  Abgelaufene Testversion  |    false  |  true   |      &lt;ein DateTime-Wert in der Vergangenheit, in dem &gt; dieses Element vorhanden sein muss, da " **istrial** " true ist Sie können eine Website aufrufen, die die aktuelle koordinierte Weltzeit (UTC) anzeigt, um festzustellen, was in UTC Vergangenheit ist.         |
|  Ungültig  |    false  | false       |     &lt;Jeder Wert oder ausgelassen&gt;          |  |

<span id="app-child-of-licenseinformation"/>

#### <a name="app-element-child-of-licenseinformation"></a>App-Element (untergeordnetes Element von LicenseInformation)

Dieses Element beschreibt die App-Lizenz. **App** ist ein erforderliches untergeordnetes Element des [LicenseInformation](#licenseinformation)-Elements.

**App** enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Quantity (Menge)  | BESCHREIBUNG   |
|-------------|------------|--------|--------|
|  **IsActive**  |    Ja   |  1   |    Beschreibt den aktuellen Lizenzstatus der App. Der Wert **true** gibt an, dass die Lizenz gültig ist. Der Wert **false** gibt an, dass die Lizenz ungültig ist. Normalerweise lautet dieser Wert **true**, unabhängig davon, ob die App einen Testmodus hat oder nicht.  Legen Sie diesen Wert auf **false** fest, um zu testen, wie sich Ihre App verhält, wenn die Lizenz ungültig ist.           |
|  **IsTrial**  |    Ja  |  1   |      Beschreibt den aktuellen Testversionsstatus der App. Der Wert **true** gibt an, dass die App während des Testzeitraums verwendet wird. Der Wert **false** gibt an, dass die App keine Testversion ist, entweder weil die App gekauft wurde oder weil der Testzeitraum abgelaufen ist.         |
|  **ExpirationDate**  |    Nein  |  0 oder 1       |     Das Datum, an dem der Testzeitraum für diese App abläuft, angegeben in der koordinierten Weltzeit (UTC). Das Datum muss ausgedrückt werden als: yyyy-mm-ddThh:mm:ss.ssZ. Beispielsweise würde 05:00 Uhr am 19. Januar 2015 als 2015-01-19T05:00:00.00Z angegeben. Dieses Element ist erforderlich, wenn **IsTrial****true** ist. Andernfalls ist es nicht erforderlich.          |  |

<span id="product-child-of-licenseinformation"/>

#### <a name="product-element-child-of-licenseinformation"></a>Product-Element (untergeordnetes Element von LicenseInformation.)

Dieses Element beschreibt den Lizenzstatus eines dauerhaften Add-Ons in der App. **Product** ist ein optionales untergeordnetes Element des [LicenseInformation](#licenseinformation)-Elements.

**Product** enthält die folgenden untergeordneten Elemente.

|  Element  |  Erforderlich  |  Quantity (Menge)  | BESCHREIBUNG   |
|-------------|------------|--------|--------|
|  **IsActive**  |    Ja   |  1     |    Beschreibt den aktuellen Lizenzstatus des Add-Ons. Der Wert **true** gibt an, dass das Add-On verwendet werden kann. Der Wert **false** gibt an, dass das Add-On nicht verwendet werden kann oder nicht gekauft wurde.           |
|  **ExpirationDate**  |    Nein   |  0 oder 1     |     Das Datum, an dem das Add-On abläuft, angegeben in der koordinierten Weltzeit (UTC). Das Datum muss ausgedrückt werden als: yyyy-mm-ddThh:mm:ss.ssZ. Beispielsweise würde 05:00 Uhr am 19. Januar 2015 als 2015-01-19T05:00:00.00Z angegeben. Wenn dieses Element vorhanden ist, hat das Add-On ein Ablaufdatum. Wenn es nicht vorhanden ist, läuft das Add-On nicht ab.  |  

**Product** hat die folgenden Attribute.

|  attribute  |  Erforderlich  |  BESCHREIBUNG   |
|-------------|------------|----------------|
|  **ProductId**  |    Ja        |   Enthält die Zeichenfolge, die von der App zur Identifizierung des Add-Ons verwendet wird.            |
|  **OfferId**  |     Nein       |   Enthält die Zeichenfolge, die von der App zur Identifizierung der Kategorie verwendet wird, zu der das Add-On gehört. Dies stellt Unterstützung für große Artikelkataloge bereit, wie unter [Verwalten eines großen Katalogs mit In-App-Produkten](manage-a-large-catalog-of-in-app-products.md) beschrieben.           |

<span id="simulation"/>

#### <a name="simulation-element"></a>Simulation-Element

Dieses Element beschreibt, wie Aufrufe verschiedener [CurrentAppSimulator](/uwp/api/windows.applicationmodel.store.currentappsimulator)-Methoden während der Testphase in der App funktionieren. **Simulation** ist ein optionales untergeordnetes Element des **CurrentApp**-Elements und enthält keine oder mehr [DefaultResponse](#defaultresponse)-Elemente.

**Simulation** hat die folgenden Attribute.

|  attribute  |  Erforderlich  |  BESCHREIBUNG   |
|-------------|------------|----------------|
|  **SimulationMode**  |    Nein        |      Werte können **Interactive** oder **Automatic** sein. Wenn dieses Attribut auf **Automatic** festgelegt ist, geben die Methoden automatisch die angegebenen HRESULT-Fehlercodes zurück. Dies kann während der Ausführung automatisierter Testfälle verwendet werden.       |

<span id="defaultresponse"/>

#### <a name="defaultresponse-element"></a>DefaultResponse-Element

Dieses Element beschreibt den Standardfehlercode, der von einer **CurrentAppSimulator**-Methode zurückgegeben wird. **DefaultResponse** ist ein optionales untergeordnetes Element des [Simulation](#simulation)-Elements.

**DefaultResponse** hat die folgenden Attribute.

|  attribute  |  Erforderlich  |  BESCHREIBUNG   |
|-------------|------------|----------------|
|  **MethodName**  |    Ja        |   Weisen Sie dieses Attribut einem der Enumerationswerte für den Typ **StoreMethodName** im [Schema](#schema) zu. Jede dieser Enumerationswerte stellt eine **CurrentAppSimulator**-Methode dar, für die Sie während der Testphase in Ihrer App einen Fehlercode-Rückgabewert simulieren möchten. Beispielsweise gibt der Wert **RequestAppPurchaseAsync_GetResult** an, dass Sie den Fehlercode-Rückgabewert der [RequestAppPurchaseAsync](/uwp/api/windows.applicationmodel.store.currentappsimulator.requestapppurchaseasync)-Methode simulieren möchten.            |
|  **HRESULT**  |     Ja       |   Weisen Sie dieses Attribut einem der Enumerationswerte für den Typ **ResponseCodes** im [Schema](#schema) zu. Jeder dieser Enumerationswerte stellt den Fehlercode dar, den Sie für die Methode zurückgeben möchten, die Sie dem **MethodName**-Attribut für dieses **DefaultResponse**-Element zugewiesen haben.           |

<span id="consumableinformation"/>

#### <a name="consumableinformation-element"></a>ConsumableInformation-Element

Dieses Element beschreibt die konsumierbaren Add-Ons, die für diese App verfügbar sind. **ConsumableInformation** ist ein optionales untergeordnetes Element des **CurrentApp**-Elements und kann keine oder mehr [Product](#product-child-of-consumableinformation)-Elemente enthalten.

<span id="product-child-of-consumableinformation"/>

#### <a name="product-element-child-of-consumableinformation"></a>Product-Element (untergeordnetes Element von ConsumableInformation)

Dieses Element beschreibt ein konsumierbares Add-On. **Product** ist ein optionales untergeordnetes Element des [ConsumableInformation](#consumableinformation)-Elements.

**Product** hat die folgenden Attribute.

|  attribute  |  Erforderlich  |  BESCHREIBUNG   |
|-------------|------------|----------------|
|  **ProductId**  |    Ja        |   Enthält die Zeichenfolge, die von der App zur Identifizierung des konsumierbaren Add-Ons verwendet wird.            |
|  **TransactionId**  |     Ja       |   Enthält eine GUID (als Zeichenfolge), die von der App verwendet wird, um die Kauftransaktion für ein konsumierbares Add-On während der Ausführung nachzuverfolgen. Weitere Informationen finden Sie unter [Käufe von konsumierbaren In-App-Produkten aktivieren](enable-consumable-in-app-product-purchases.md).            |
|  **Status**  |      Ja      |  Enthält die Zeichenfolge, die von der App verwendet wird, um den Ausführungsstatus eines konsumierbaren Add-Ons anzugeben. Werte können **Active**, **PurchaseReverted**, **PurchasePending** oder **ServerError** sein.             |
|  **OfferId**  |     Nein       |    Enthält die Zeichenfolge, die von der App zur Identifizierung der Kategorie verwendet wird, zu der das konsumierbare Add-On gehört. Dies stellt Unterstützung für große Artikelkataloge bereit, wie unter [Verwalten eines großen Katalogs mit In-App-Produkten](manage-a-large-catalog-of-in-app-products.md) beschrieben.           |