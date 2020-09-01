---
title: Schließfach für Anmeldeinformationen
description: In diesem Artikel wird beschrieben, wie Apps für die universelle Windows-Plattform (UWP) mit dem Schließfach für Anmeldeinformationen Benutzeranmeldeinformationen sicher speichern und abrufen können und sie zwischen den Geräten mit dem Microsoft-Konto des Benutzers per Roaming übertragen können.
ms.assetid: 7BCC443D-9E8A-417C-B275-3105F5DED863
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Sicherheit
ms.localizationpriority: medium
ms.openlocfilehash: 1a9f47a0f48d00c898bdb6df71f5cbd858b03bc4
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89167264"
---
# <a name="credential-locker"></a>Schließfach für Anmeldeinformationen




In diesem Artikel wird beschrieben, wie Apps für die universelle Windows-Plattform (UWP) mit dem Schließfach für Anmeldeinformationen Benutzeranmeldeinformationen sicher speichern und abrufen können und sie zwischen den Geräten mit dem Microsoft-Konto des Benutzers per Roaming übertragen können.

Angenommen, Sie haben eine App, die eine Verbindung mit einem Dienst herstellt, um auf geschützte Dateien wie z. B. Mediendateien, soziale Netzwerke usw. zuzugreifen. Ihr Dienst erfordert Anmeldeinformationen für jeden Benutzer. Sie haben die Benutzeroberfläche in Ihre App integriert, die den Benutzernamen und das Kennwort für den Benutzer abruft; diese Informationen werden dann für die Anmeldung des Benutzers am Dienst verwendet. Mit der API des Schließfachs für Anmeldeinformationen können Sie den Benutzernamen und das Kennwort für den Benutzer speichern und diese Informationen leicht abrufen und den Benutzer automatisch anmelden, wenn er Ihre App das nächste Mal startet, und zwar unabhängig vom verwendeten Gerät.

Benutzer Anmelde Informationen, die in der Datei "" erstellt werden, werden *nicht* ablaufen *, sind von* " [**ApplicationData. roamingstoragequota**](/uwp/api/windows.storage.applicationdata.roamingstoragequota)" nicht betroffen *und werden aufgrund* von Inaktivität wie herkömmlichen Roamingdaten nicht gelöscht. Allerdings können Sie nur bis zu 20 Anmelde Informationen pro app in der "andentizuweisung" speichern.

Die Funktionsweise des Schließfachs für Anmeldeinformationen sieht für Domänenkonten etwas anders aus. Wenn für Ihr Microsoft-Konto Anmeldeinformationen gespeichert sind und Sie dieses Konto mit einem Domänenkonto (z. B. das Konto, das Sie bei der Arbeit nutzen) verknüpfen, wandern Ihre Anmeldeinformationen zu diesem Domänenkonto. Neue Anmeldeinformationen, die während einer Anmeldung mit dem Domänenkonto hinzugefügt werden, werden jedoch nicht servergespeichert. So wird sichergestellt, dass private Anmeldeinformationen für die Domäne nicht außerhalb der Domäne verfügbar gemacht werden.

## <a name="storing-user-credentials"></a>Speichern von Benutzeranmeldeinformationen


1.  Rufen Sie mithilfe des [**PasswordVault**](/uwp/api/Windows.Security.Credentials.PasswordVault)-Objekts aus dem [**Windows.Security.Credentials**](/uwp/api/Windows.Security.Credentials)-Namespace einen Verweis auf das Schließfach für Anmeldeinformationen ab.
2.  Erstellen Sie ein [**PasswordCredential**](/uwp/api/Windows.Security.Credentials.PasswordCredential)-Objekt, das einen Bezeichner für Ihre App, den Benutzernamen und das Kennwort enthält, und übergeben Sie sie an die [**PasswordVault.Add**](/uwp/api/windows.security.credentials.passwordvault.add)-Methode, um die Anmeldeinformationen dem Schließfach hinzuzufügen.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Add(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="retrieving-user-credentials"></a>Abrufen von Benutzeranmeldeinformationen


Zum Abrufen von Benutzeranmeldeinformationen aus dem Schließfach für Anmeldeinformationen mithilfe eines Verweises auf das [**PasswordVault**](/uwp/api/Windows.Security.Credentials.PasswordVault)-Objekt stehen verschiedene Optionen zur Verfügung.

-   Mit der [**PasswordVault.RetrieveAll**](/uwp/api/windows.security.credentials.passwordvault.retrieveall)-Methode können Sie alle Anmeldeinformationen abrufen, die der Benutzer im Schließfach für Ihre App bereitgestellt hat.

-   Wenn Ihnen der Benutzername für die gespeicherten Anmeldeinformationen bekannt ist, können Sie mit der [**PasswordVault.FindAllByUserName**](/uwp/api/windows.security.credentials.passwordvault.findallbyusername)-Methode alle Anmeldeinformationen für diesen Benutzernamen abrufen.

-   Wenn Ihnen der Ressourcenname für die gespeicherten Anmeldeinformationen bekannt ist, können Sie mit der [**PasswordVault.FindAllByResource**](/uwp/api/windows.security.credentials.passwordvault.findallbyresource)-Methode alle Anmeldeinformationen für diesen Ressourcennamen abrufen.

-   Und wenn Ihnen sowohl der Benutzer- als auch der Ressourcenname für bestimmte Anmeldeinformationen bekannt ist, können Sie diese Anmeldeinformationen mithilfe der [**PasswordVault.Retrieve**](/uwp/api/windows.security.credentials.passwordvault.retrieve)-Methode abrufen.

Sehen wir uns ein Beispiel an, in dem wir den Ressourcennamen global in einer App gespeichert haben und den Benutzer automatisch anmelden, wenn wir entsprechende Anmeldeinformationen finden. Wenn mehrere Anmeldeinformationen für einen Benutzer gefunden werden, wird der Benutzer dazu aufgefordert, Standardanmeldeinformationen für die Anmeldung auszuwählen.

```cs
private string resourceName = "My App";
private string defaultUserName;

private void Login()
{
    var loginCredential = GetCredentialFromLocker();

    if (loginCredential != null)
    {
        // There is a credential stored in the locker.
        // Populate the Password property of the credential
        // for automatic login.
        loginCredential.RetrievePassword();
    }
    else
    {
        // There is no credential stored in the locker.
        // Display UI to get user credentials.
        loginCredential = GetLoginCredentialUI();
    }

    // Log the user in.
    ServerLogin(loginCredential.UserName, loginCredential.Password);
}


private Windows.Security.Credentials.PasswordCredential GetCredentialFromLocker()
{
    Windows.Security.Credentials.PasswordCredential credential = null;

    var vault = new Windows.Security.Credentials.PasswordVault();
    var credentialList = vault.FindAllByResource(resourceName);
    if (credentialList.Count > 0)
    {
        if (credentialList.Count == 1)
        {
            credential = credentialList[0];
        }
        else
        {
            // When there are multiple usernames,
            // retrieve the default username. If one doesn't
            // exist, then display UI to have the user select
            // a default username.

            defaultUserName = GetDefaultUserNameUI();

            credential = vault.Retrieve(resourceName, defaultUserName);
        }
    }

    return credential;
}
```

## <a name="deleting-user-credentials"></a>Löschen von Benutzeranmeldeinformationen


Das Löschen von Benutzeranmeldeinformationen im Schließfach für Anmeldeinformationen ist ebenfalls ein schneller Prozess mit zwei Schritten.

1.  Rufen Sie mithilfe des [**PasswordVault**](/uwp/api/Windows.Security.Credentials.PasswordVault)-Objekts aus dem [**Windows.Security.Credentials**](/uwp/api/Windows.Security.Credentials)-Namespace einen Verweis auf das Schließfach für Anmeldeinformationen ab.

2.  Übergeben Sie die zu löschenden Anmeldeinformationen an die [**PasswordVault.Remove**](/uwp/api/windows.security.credentials.passwordvault.remove)-Methode.

```cs
var vault = new Windows.Security.Credentials.PasswordVault();
vault.Remove(new Windows.Security.Credentials.PasswordCredential(
    "My App", username, password));
```

## <a name="best-practices"></a>Bewährte Methoden


Verwenden Sie das Schließfach für Anmeldeinformationen nur für Kennwörter und nicht für größere Daten-BLOBs.

Speichern Sie Kennwörter nur unter den folgenden Bedingungen im Schließfach für Anmeldeinformationen:

-   Der Benutzer hat sich erfolgreich angemeldet.
-   Der Benutzer hat dem Speichern von Kennwörtern zugestimmt.

Speichern Sie Anmeldeinformationen niemals als Nur-Text mit App-Daten oder Roamingeinstellungen.