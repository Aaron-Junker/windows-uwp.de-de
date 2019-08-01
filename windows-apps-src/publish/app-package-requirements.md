---
Description: Halten Sie die folgenden Richtlinien ein, wenn Sie die App-Pakete für die Übermittlung an den Microsoft Store vorbereiten.
title: App-Paketanforderungen
ms.assetid: 651B82BA-9D0C-45AC-8997-88CD93DC903C
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Paketanforderungen, Pakete, Paketformat, unterstützte Version, übermitteln
ms.localizationpriority: medium
ms.openlocfilehash: 144e2fefc5802d53187684b6e34cbb2af1d8da0a
ms.sourcegitcommit: 350d6e6ba36800df582f9715c8d21574a952aef1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/31/2019
ms.locfileid: "68682636"
---
# <a name="app-package-requirements"></a>App-Paketanforderungen

Halten Sie die folgenden Richtlinien ein, wenn Sie die App-Pakete für die Übermittlung an den Microsoft Store vorbereiten.

## <a name="before-you-build-your-apps-package-for-the-microsoft-store"></a>Vor dem Erstellen des App-Pakets für den Microsoft Store

Denken Sie daran, [Ihre App mit dem Zertifizierungskit für Windows-Apps zu testen](../debug-test-perf/windows-app-certification-kit.md). Außerdem wird empfohlen, Ihre App mit verschiedenen Hardwaretypen zu testen. Beachten Sie, dass Ihre App nur auf Computern mit Entwicklerlizenzen installiert und ausgeführt werden kann, bis wir die App zertifiziert und im Microsoft Store verfügbar gemacht haben.

## <a name="building-the-app-package-using-microsoft-visual-studio"></a>Erstellen des App-Pakets mit Microsoft Visual Studio

Wenn Sie Microsoft Visual Studio als Entwicklungsumgebung verwenden, verfügen Sie bereits über integrierte Tools zum schnellen und einfachen Erstellen eines App-Pakets. Weitere Informationen finden Sie unter [Verpacken von Apps](../packaging/index.md).

> [!NOTE]
> Achten Sie darauf, für alle Dateinamen ANSI zu verwenden. 

Um Ihr Paket in Visual Studio zu erstellen, müssen Sie sich mit demselben Konto anmelden, das Ihrem Entwicklerkonto zugeordnet ist. Einige Teile des Paketmanifests enthalten spezifische kontobezogene Details. Diese Informationen werden erkannt und automatisch hinzugefügt. Ohne die zusätzlichen Informationen, die dem Manifest hinzugefügt wurden, können beim Hochladen von Paketen Fehler auftreten. 

Wenn Sie die UWP-Pakete Ihrer APP erstellen, kann Visual Studio eine msix-oder AppX-Datei oder eine. msixupload-oder. appxupload-Datei erstellen. Für UWP-apps wird empfohlen, dass Sie immer die Datei ". msixupload" oder ". appxupload" auf der Seite " [Pakete](upload-app-packages.md) " hochladen. Weitere Informationen zum Verpacken von UWP-Apps für den Store finden Sie unter [Verpacken einer UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps).

App-Pakete müssen nicht mit einem Stammzertifikat einer vertrauenswürdigen Zertifizierungsstelle signiert werden.


### <a name="app-bundles"></a>App-Bündel

Für UWP-apps kann Visual Studio eine APP Bundle (. msixbundle oder. appxbundle) generieren, um die Größe der APP zu reduzieren, die von den Benutzern heruntergeladen wird. Dieser Schritt ist in der Regel sinnvoll, wenn Sie sprachspezifische Ressourcen, mehrere Ressourcen für die Bildgröße oder Ressourcen für bestimmte Versionen von Microsoft DirectX definiert haben.

> [!NOTE]
> Eine APP Bundle kann Ihre Pakete für alle Architekturen enthalten.

Bei einem App-Bündel lädt der Benutzer nicht alle vorhandenen Ressourcen, sondern nur relevante Dateien herunter. Weitere Informationen zu App-Bündeln finden Sie unter [Verpacken von Apps](../packaging/index.md) und [Verpacken von UWP-App mit Visual Studio](/windows/msix/package/packaging-uwp-apps).


## <a name="building-the-app-package-manually"></a>Manuelles Erstellen des App-Pakets

Wenn Sie Ihr Paket nicht mit Visual Studio erstellen, müssen Sie Ihr [Paketmanifest manuell erstellen](https://docs.microsoft.com/uwp/schemas/appxpackage/how-to-create-a-package-manifest-manually).

Ausführliche Informationen und die Anforderungen für Manifeste finden Sie in der Dokumentation zum [App-Paketmanifest](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest). Es werden nur Apps zertifiziert, deren Manifest dem Paketmanifestschema entspricht.

Ihr Manifest muss spezifische konto- und App-bezogene Informationen enthalten. Sie finden diese Informationen unter [Anzeigen von Details zur App-Identität](view-app-identity-details.md) im Abschnitt **App-Verwaltung** der App-Übersichtsseite im Dashboard.

> [!NOTE]
> Bei Werten im Manifest wird die Groß-/Kleinschreibung beachtet. Leerzeichen und Satzzeichen müssen ebenfalls übereinstimmen. Geben Sie die Werte richtig ein, und überprüfen Sie sie anschließend auf ihre Korrektheit.


App-Bündel (. msixbundle oder. appxbundle) verwenden ein anderes Manifest. Ausführliche Informationen und die Anforderungen für App-Bündel finden Sie in der Dokumentation zum [Bündelmanifest](https://docs.microsoft.com/uwp/schemas/bundlemanifestschema/bundle-manifest). Beachten Sie, dass das Manifest jedes enthaltenen Pakets in einer msixbundle-oder appxbundle-Datei dieselben Elemente und Attribute verwenden muss, mit Ausnahme des Attributs **ProcessorArchitecture** des [Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) -Elements.

> [!TIP]
> Stellen Sie sicher, dass Sie das [zertifizierungskit für Windows-apps](../debug-test-perf/windows-app-certification-kit.md) ausführen, bevor Sie Ihre Pakete So können Sie feststellen, ob es mit Ihrem Manifest Probleme gibt, die Zertifizierungs- oder Einreichungsfehler verursachen können.


## <a name="package-format-requirements"></a>Paketformatanforderungen

Ihre App-Pakete müssen die folgenden Anforderungen erfüllen:

| App-Paketeigenschaft | Anforderung                                                          |
|----------------------|----------------------------------------------------------------------|
| Paketgröße         | . msixbundle oder. appxbundle: maximal 25 GB pro Bundle <br>msix-oder AppX-Pakete für Windows 10: maximal 25 GB pro Paket<br>AppX-Pakete, die auf Windows 8.1 abzielen: maximal 8 GB pro Paket <br> AppX-Pakete für Windows 8: maximal 2 GB pro Paket <br> AppX-Pakete für Windows Phone 8,1: maximal 4 GB pro Paket <br> XAP-Pakete: maximal 1 GB pro Paket                                                                           |
| Hashes für Blockzuordnung     | SHA2-256-Algorithmus                                                   |

> [!IMPORTANT]
> Ab dem 31. Oktober 2018 können neu erstellte Produkte keine Pakete enthalten, die auf Windows 8. x/Windows Phone 8. x oder früher abzielen. Weitere Informationen finden Sie in diesem [Blogbeitrag](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="supported-versions"></a>Unterstützte Versionen

Für UWP-Apps müssen alle Pakete auf eine Version von Windows 10 ausgerichtet sein, die vom Store unterstützt wird. Die Versionen, die von Ihrem Paket unterstützt werden, müssen in den Attributen **MinVersion** und **MaxVersionTested** des [TargetDeviceFamily](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily)-Element im App-Manifest angegeben werden.

Derzeit wird der folgende Versionsbereich unterstützt: 
- Minimum: 10.0.10240.0
- Maximal: 10.0.17763.1


## <a name="storemanifest-xml-file"></a>Datei „StoreManifest.xml“

„StoreManifest.xml“ ist eine optionale Konfigurationsdatei, die in App-Pakete aufgenommen werden kann. Sie dient zum Aktivieren von Features, die vom Paketmanifest nicht abgedeckt werden – beispielsweise Features zum Deklarieren Ihrer App als Microsoft Store-Geräte-App oder zum Deklarieren von Anforderungen, die für ein Paket erfüllt werden müssen, damit es auf ein Gerät angewendet werden kann. Wenn Sie verwendet wird, wird "storemanifest. xml" mit dem App-Paket übermittelt und muss sich im Stamm Ordner des Hauptprojekts Ihrer APP befinden. Weitere Informationen finden Sie unter [StoreManifest-Schema](https://docs.microsoft.com/uwp/schemas/storemanifest/store-manifest-schema-portal).

 

 




