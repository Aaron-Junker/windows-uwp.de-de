---
title: Anfordern eines Kartenauthentifizierungsschlüssels
description: Ihre universelle Windows-App muss authentifiziert werden, bevor sie die MapControl-Klasse und Kartendienste im Windows.Services.Maps-Namespace verwenden kann.
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Kartenauthentifizierungsschlüssel, Kartensteuerelement
ms.localizationpriority: medium
ms.openlocfilehash: bb6cf66a6725e8231fac693a81a39bc2f0b15fd2
ms.sourcegitcommit: 68051d2adcf3facc036c9f4c76d82bb4d342a75f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/02/2019
ms.locfileid: "67498902"
---
# <a name="request-a-maps-authentication-key"></a>Anfordern eines Kartenauthentifizierungsschlüssels




Ihre [universelle Windows-App](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) muss authentifiziert werden, bevor sie die [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Klasse und Kartendienste im [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)-Namespace verwenden kann. Zum Authentifizieren Ihrer App müssen Sie einen Kartenauthentifizierungsschlüssel angeben. In diesem Thema wird beschrieben, wie Sie einen Kartenauthentifizierungsschlüssel vom [Bing Maps Developer Center](https://www.bingmapsportal.com/) anfordern und Ihrer App hinzufügen.

**Tipp** Um mehr über das Verwenden von Karten in Ihrer App zu erfahren, laden Sie das folgende Beispiel aus den [API-Beispielen für die universelle Windows-Plattform](https://go.microsoft.com/fwlink/p/?LinkId=619979) auf GitHub herunter:

-   [Kartenbeispiel für die Universelle Windows-Plattform (UWP)](https://go.microsoft.com/fwlink/p/?LinkId=619977)

## <a name="get-a-key"></a>Abrufen eines Schlüssels


Erstellen und verwalten Sie Kartenauthentifizierungsschlüssel für Ihre universellen Windows-Apps im [Bing Maps Developer Center](https://www.bingmapsportal.com/).

So erstellen Sie einen neuen Schlüssel

1.  Wechseln Sie in Ihrem Browser zu Bing Maps Developer Center ([https://www.bingmapsportal.com](https://www.bingmapsportal.com/)).

2.  Wenn Sie zum Anmelden aufgefordert werden, geben Sie Ihre Microsoft-Kontoinformationen ein, und klicken Sie auf **Anmelden**.

3.  Wählen Sie das Konto, das mit Ihrem Bing Karten-Konto verknüpft werden soll. Wenn Sie Ihr Microsoft-Konto verwenden möchten, klicken Sie auf **Ja**. Andernfalls klicken Sie auf die Option zum **Anmelden mit einem anderen Konto**.

4.  Wenn Sie noch kein Bing Karten-Konto besitzen, erstellen Sie ein neues Bing Karten-Konto. Geben Sie **Kontoname**, **Kontaktname**, **Firmenname**, **E-Mail-Adresse** und **Telefonnummer** ein. Akzeptieren Sie die Nutzungsbedingungen, und klicken Sie auf **Erstellen**.

5.  Klicken Sie im Menü **Mein Konto** auf **Eigene Schlüssel**.

6.  Wenn Sie bereits einen Schlüssel erstellt haben, klicken Sie auf den Link, um einen neuen Schlüssel zu erstellen. “Andernfalls fahren Sie mit dem Formular „Schlüssel erstellen” fort.

7.  Füllen Sie das Formular **Schlüssel erstellen** aus, und klicken Sie dann auf **Erstellen**.

    -   **Anwendungsname:** Der Name der Anwendung.
    -   **Anwendungs-URL (optional):** Die URL Ihrer Anwendung.
    -   **Typ des Schlüssels:** Wählen Sie **grundlegende** oder **Enterprise**.
    -   **Anwendungstyp:** Wählen Sie **Windows-Anwendung** für die Verwendung in Ihrer universellen Windows-app.

    So sieht das Formular aus.

    ![Beispiel des Formulars „Schlüssel erstellen“.](images/createkeydialog.png)

8.  Nachdem Sie auf **Erstellen** geklickt haben, wird der neue Schlüssel unterhalb des Formulars **Schlüssel erstellen** angezeigt. Kopieren Sie ihn an einen sicheren Ort, oder fügen Sie ihn sofort wie im nächsten Schritt beschrieben Ihrer App hinzu.

## <a name="add-the-key-to-your-app"></a>Hinzufügen des Schlüssels zur App


Der Kartenauthentifizierungsschlüssel ist erforderlich, um die [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Klasse und Kartendienste ([**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)) in Ihrer universellen Windows-App zu verwenden. Fügen Sie ihn ggf. dem Kartensteuerelement und Kartendienstobjekten hinzu.

### <a name="to-add-the-key-to-a-map-control"></a>So fügen Sie den Schlüssel einem Kartensteuerelement hinzu

Setzen Sie zum Authentifizieren der [**MapControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Maps.MapControl)-Klasse die [**MapServiceToken**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.maps.mapcontrol.mapservicetoken)-Eigenschaft auf den Wert des Authentifizierungsschlüssels. Sie können diese Eigenschaft je nach Ihren Einstellungen im Code oder im XAML-Markup festlegen. Weitere Informationen zur Verwendung der **MapControl**-Klasse finden Sie unter [Anzeigen von Karten mit 2D-, 3D- und Streetside-Ansichten](display-maps.md).

-   In diesem Beispiel wird die **MapServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im Code gesetzt.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   In diesem Beispiel wird die **MapServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im XAML-Markup gesetzt.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### <a name="to-add-the-key-to-map-services"></a>So fügen Sie den Schlüssel Kartendiensten hinzu

Um Dienste im [**Windows.Services.Maps**](https://docs.microsoft.com/uwp/api/Windows.Services.Maps)-Namespace zu verwenden, setzen Sie die [**ServiceToken**](https://docs.microsoft.com/uwp/api/windows.services.maps.mapservice.servicetoken)-Eigenschaft auf den Wert des Authentifizierungsschlüssels. Weitere Informationen zur Verwendung von Kartendiensten finden Sie unter [Anzeigen von Routen und Wegbeschreibungen auf einer Karte](routes-and-directions.md) und [Durchführen der Geocodierung und umgekehrten Geocodierung](geocoding.md).

-   In diesem Beispiel wird die **ServiceToken**-Eingeschaft auf den Wert des Authentifizierungsschlüssels im Code gesetzt.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## <a name="related-topics"></a>Verwandte Themen

* [Bing Karten Developer Center](https://www.bingmapsportal.com/)
* [Beispiel für UWP-Karte](https://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Entwurfsrichtlinien für Karten](https://docs.microsoft.com/windows/uwp/maps-and-location/controls-map)
* [Build 2015-Video: Nutzen von Karten und Ortung über Telefon, Tablet und PC in Ihren Windows-Apps](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Beispiel für eine UWP-App mit Verkehrsinformationen](https://go.microsoft.com/fwlink/p/?LinkId=619982)
