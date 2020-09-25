---
title: Anfordern eines Kartenauthentifizierungsschlüssels
description: Ihre universelle Windows-App muss authentifiziert werden, bevor sie die MapControl-Klasse und Kartendienste im Windows.Services.Maps-Namespace verwenden kann.
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Karten Authentifizierungsschlüssel, Karten Steuerelement
ms.localizationpriority: medium
ms.openlocfilehash: da98ea9f09ef5d7804b07b7066847afbab7a2681
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91220523"
---
# <a name="request-a-maps-authentication-key"></a>Anfordern eines Kartenauthentifizierungsschlüssels

> [!WARNING]
> Online Maps-Dienste sind unter älteren Versionen von Windows 10 möglicherweise nicht verfügbar. In den folgenden Versionen zeigt mapcontrol möglicherweise keine Zuordnungen mehr und APIs im Windows. Services. Maps-Namespace keine Ergebnisse zurück:
> - Windows 10, Version 1607 und frühere Versionen: Kartendienste sind ab Oktober 2020 weltweit nicht verfügbar.
> - Windows 10, Version 1703 und frühere Versionen: Kartendienste sind auf [einigen in China verkauften Geräten](/windows-hardware/customize/desktop/unattend/microsoft-windows-mapcontrol-desktop-chinavariantwin10) nicht verfügbar.

Ihre [universelle Windows-App](../get-started/universal-application-platform-guide.md) muss authentifiziert werden, bevor Sie den [**mapcontrol**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl) -und den Map-Dienst im [**Windows. Services. Maps**](/uwp/api/Windows.Services.Maps) -Namespace verwenden kann. Zum Authentifizieren Ihrer App müssen Sie einen Kartenauthentifizierungsschlüssel angeben. In diesem Thema wird beschrieben, wie Sie einen Kartenauthentifizierungsschlüssel vom [Bing Maps Developer Center](https://www.bingmapsportal.com/) anfordern und Ihrer App hinzufügen.

**Tipp** Um mehr über das Verwenden von Karten in Ihrer App zu erfahren, laden Sie das folgende Beispiel aus den [API-Beispielen für die universelle Windows-Plattform](https://github.com/Microsoft/Windows-universal-samples) auf GitHub herunter:

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)

## <a name="get-a-key"></a>Abrufen eines Schlüssels


Erstellen und verwalten Sie Kartenauthentifizierungsschlüssel für Ihre universellen Windows-Apps im [Bing Maps Developer Center](https://www.bingmapsportal.com/).

So erstellen Sie einen neuen Schlüssel

1.  Navigieren Sie in Ihrem Browser zum Entwickler Center von "Info Maps" ( [https://www.bingmapsportal.com](https://www.bingmapsportal.com/) ).

2.  Wenn Sie zum Anmelden aufgefordert werden, geben Sie Ihre Microsoft-Kontoinformationen ein, und klicken Sie auf **Anmelden**.

3.  Wählen Sie das Konto, das mit Ihrem Bing Karten-Konto verknüpft werden soll. Wenn Sie Ihr Microsoft-Konto verwenden möchten, klicken Sie auf **Ja**. Andernfalls klicken Sie auf die Option zum **Anmelden mit einem anderen Konto**.

4.  Wenn Sie noch kein Bing Karten-Konto besitzen, erstellen Sie ein neues Bing Karten-Konto. Geben Sie **Kontoname**, **Kontaktname**, **Firmenname**, **E-Mail-Adresse** und **Telefonnummer** ein. Akzeptieren Sie die Nutzungsbedingungen, und klicken Sie auf **Erstellen**.

5.  Klicken Sie im Menü **mein Konto** auf **meine Schlüssel**.

6.  Wenn Sie zuvor einen Schlüssel erstellt haben, klicken Sie auf den Link, um einen neuen Schlüssel zu erstellen. Andernfalls fahren Sie mit dem Formular Create Key fort.

7.  Füllen Sie das Formular **Schlüssel erstellen** aus, und klicken Sie dann auf **Erstellen**.

    -   **Anwendungsname:** Der Name Ihrer Anwendung.
    -   **Anwendungs-URL (optional):** Die URL Ihrer Anwendung.
    -   **Schlüsseltyp:** Wählen Sie **Basic** oder **Enterprise** aus.
    -   **Anwendungstyp:** Wählen Sie **Windows-Anwendung** für die Verwendung in ihrer universellen Windows-App aus.

    So sieht das Formular aus.

    ![Beispiel des Formulars „Schlüssel erstellen“.](images/createkeydialog.png)

8.  Nachdem Sie auf **Erstellen** geklickt haben, wird der neue Schlüssel unterhalb des Formulars **Schlüssel erstellen** angezeigt. Kopieren Sie ihn an einen sicheren Ort, oder fügen Sie ihn sofort wie im nächsten Schritt beschrieben Ihrer App hinzu.

## <a name="add-the-key-to-your-app"></a>Hinzufügen des Schlüssels zur App


Der Kartenauthentifizierungsschlüssel ist erforderlich, um die [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Klasse und Kartendienste ([**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps)) in Ihrer universellen Windows-App zu verwenden. Fügen Sie ihn ggf. dem Kartensteuerelement und Kartendienstobjekten hinzu.

### <a name="to-add-the-key-to-a-map-control"></a>So fügen Sie den Schlüssel einem Kartensteuerelement hinzu

Setzen Sie zum Authentifizieren der [**MapControl**](/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Klasse die [**MapServiceToken**](/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)-Eigenschaft auf den Wert des Authentifizierungsschlüssels. Sie können diese Eigenschaft je nach Ihren Einstellungen im Code oder im XAML-Markup festlegen. Weitere Informationen zur Verwendung der **MapControl**-Klasse finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md).

-   In diesem Beispiel wird die **MapServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im Code gesetzt.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   In diesem Beispiel wird die **MapServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im XAML-Markup gesetzt.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>So fügen Sie den Schlüssel Kartendiensten hinzu

Um Dienste im [**Windows.Services.Maps**](/uwp/api/Windows.Services.Maps)-Namespace zu verwenden, setzen Sie die [**ServiceToken**](/uwp/api/windows.services.maps.mapservice.servicetoken)-Eigenschaft auf den Wert des Authentifizierungsschlüssels. Weitere Informationen zur Verwendung von Kartendiensten finden Sie unter [Anzeigen von Routen und Wegbeschreibungen auf einer Karte](routes-and-directions.md) und [Durchführen der Geocodierung und umgekehrten Geocodierung](geocoding.md).

-   In diesem Beispiel wird die **ServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im Code gesetzt.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>Zugehörige Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/MapControl)
* [Entwurfsrichtlinien für Karten](./display-maps.md)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)
