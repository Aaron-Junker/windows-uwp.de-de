---
title: Biometrischer Fingerabdruck
description: In diesem Artikel wird beschrieben, wie Sie die Funktion des biometrischen Fingerabdrucks Ihrer UWP-App (Universelle Windows-Plattform) hinzufügen.
ms.assetid: 55483729-5F8A-401A-8072-3CD611DDFED2
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: bbb40dc9fa65515a2b01d7a2c92145b27e04f075
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372565"
---
# <a name="fingerprint-biometrics"></a>Biometrischer Fingerabdruck




In diesem Artikel wird beschrieben, wie Sie die Funktion des biometrischen Fingerabdrucks Ihrer UWP-App (Universelle Windows-Plattform) hinzufügen. Sie können die Sicherheit Ihrer App erhöhen, indem Sie die Anforderung einer Authentifizierung per Fingerabdruck integrieren, wenn die Zustimmung des Benutzers für eine bestimmte Aktion erforderlich ist. So können Sie beispielsweise die Authentifizierung per Fingerabdruck vor der Autorisierung eines In-App-Kaufs oder vor dem Zugriff auf eingeschränkte Ressourcen anfordern. Die Verwaltung der Authentifizierung per Fingerabdruck erfolgt mithilfe der [**UserConsentVerifier**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.UI.UserConsentVerifier)-Klasse im [**Windows.Security.Credentials.UI**](https://docs.microsoft.com/uwp/api/Windows.Security.Credentials.UI)-Namespace.

## <a name="check-the-device-for-a-fingerprint-reader"></a>Suchen nach einem Fingerabdruckleser


Durch Aufrufen der [**UserConsentVerifier.CheckAvailabilityAsync**](https://docs.microsoft.com/uwp/api/windows.security.credentials.ui.userconsentverifier.checkavailabilityasync)-Methode kann ermittelt werden, ob das Gerät über einen Fingerabdruckleser verfügt. Auch wenn ein Gerät die Authentifizierung per Fingerabdruck unterstützt, sollte die App in den Einstellungen eine Option enthalten, mit der die Authentifizierung per Fingerabdruck aktiviert oder deaktiviert werden kann.

```cs
public async System.Threading.Tasks.Task<string> CheckFingerprintAvailability()
{
    string returnMessage = "";

    try
    {
        // Check the availability of fingerprint authentication.
        var ucvAvailability = await Windows.Security.Credentials.UI.UserConsentVerifier.CheckAvailabilityAsync();

        switch (ucvAvailability)
        {
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.Available:
                returnMessage = "Fingerprint verification is available.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerifierAvailability.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            default:
                returnMessage = "Fingerprints verification is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication availability check failed: " + ex.ToString();
    }

    return returnMessage;
}
```

## <a name="request-consent-and-return-results"></a>Anfordern der Zustimmung und Zurückgeben von Ergebnissen


Wenn Sie die Benutzerzustimmung mithilfe eines Fingerabdruckscans anfordern möchten, rufen Sie die [**UserConsentVerifier.RequestVerificationAsync**](https://docs.microsoft.com/uwp/api/windows.security.credentials.ui.userconsentverifier.requestverificationasync)-Methode auf. Damit die Authentifizierung per Fingerabdruck funktioniert, muss der Benutzer vorher einen Fingerabdruck in der Fingerabdruckdatenbank hinterlegt haben.

Beim Aufrufen von [**UserConsentVerifier.RequestVerificationAsync**](https://docs.microsoft.com/uwp/api/windows.security.credentials.ui.userconsentverifier.requestverificationasync) wird dem Benutzer ein modales Dialogfeld angezeigt, in dem ein Fingerabdruckscan angefordert wird. Sie können in der Methode **UserConsentVerifier.RequestVerificationAsync** eine Meldung bereitstellen, die dem Benutzer im modalen Dialogfeld angezeigt wird (siehe folgende Abbildung).

```cs
private async System.Threading.Tasks.Task<string> RequestConsent(string userMessage)
{
    string returnMessage;

    if (String.IsNullOrEmpty(userMessage))
    {
        userMessage = "Please provide fingerprint verification.";
    }

    try
    {
        // Request the logged on user's consent via fingerprint swipe.
        var consentResult = await Windows.Security.Credentials.UI.UserConsentVerifier.RequestVerificationAsync(userMessage);

        switch (consentResult)
        {
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Verified:
                returnMessage = "Fingerprint verified.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceBusy:
                returnMessage = "Biometric device is busy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DeviceNotPresent:
                returnMessage = "No biometric device found.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.DisabledByPolicy:
                returnMessage = "Biometric verification is disabled by policy.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.NotConfiguredForUser:
                returnMessage = "The user has no fingerprints registered. Please add a fingerprint to the " +
                                "fingerprint database and try again.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.RetriesExhausted:
                returnMessage = "There have been too many failed attempts. Fingerprint authentication canceled.";
                break;
            case Windows.Security.Credentials.UI.UserConsentVerificationResult.Canceled:
                returnMessage = "Fingerprint authentication canceled.";
                break;
            default:
                returnMessage = "Fingerprint authentication is currently unavailable.";
                break;
        }
    }
    catch (Exception ex)
    {
        returnMessage = "Fingerprint authentication failed: " + ex.ToString();
    }

    return returnMessage;
}
```