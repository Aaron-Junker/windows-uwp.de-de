---
Description: Festlegen von Rollen oder benutzerdefinierten Berechtigungen für Kontobenutzer.
title: Legen Sie Rollen oder benutzerdefinierte Berechtigungen für Kontenbenutzer fest
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Benutzerrollen, Benutzerberechtigung, benutzerdefinierte Rollen, Zugriff für Benutzer, Berechtigungen anpassen, Standardrollen
ms.localizationpriority: medium
ms.openlocfilehash: d14187509c274d23e09ac26b876a0e6ff2a03bb4
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259913"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>Legen Sie Rollen oder benutzerdefinierte Berechtigungen für Kontenbenutzer fest

Wenn Sie [Ihrem Partner Center-Kontobenutzer hinzufügen](add-users-groups-and-azure-ad-applications.md), müssen Sie den Zugriff auf das Konto angeben. Sie können ihnen [Standardrollen](#roles) zuweisen oder [die Berechtigungen so anpassen](#custom), dass die Benutzer über die entsprechende Zugriffsebene verfügen. Einige dieser benutzerdefinierten Berechtigungen gelten für das gesamte Konto, während andere für alle Produkte erteilt (oder auf bestimmte Produkte beschränkt) werden können.

> [!NOTE] 
> Unabhängig davon, ob Sie einen Benutzer, eine Gruppe oder eine Azure AD-Anwendung hinzufügen, können die gleichen Rollen und Berechtigungen übernommen werden.

Beim Ermitteln der Rollen oder Berechtigungen sollten Sie folgendes bedenken: 
-   Benutzer (einschließlich Gruppen und Azure AD Anwendungen) können mit den Berechtigungen, die ihren zugewiesenen Rollen zugeordnet sind, auf das gesamte Partner Center-Konto zugreifen, sofern Sie keine [Berechtigungen anpassen](#custom) und [Berechtigungen auf Produktebene](#product-level-permissions) zuweisen, sodass Sie nur mit bestimmten apps und/oder Add-ons funktionieren können.
-   Sie können einem Benutzer, einer Gruppe oder einer Azure AD-Anwendung den Zugriff auf die Funktionen mehrerer Rollen gewähren, indem Sie mehrere Rollen auswählen oder indem Sie mithilfe benutzerdefinierter Berechtigungen den Zugriff gewähren, den Sie ihnen geben möchten.
-   Ein Benutzer mit einer bestimmten Rolle (oder einer Reihe benutzerdefinierter Berechtigungen) kann auch Teil einer Gruppe mit einer anderen Rolle (oder einem anderen Satz von Berechtigungen) sein. In diesem Fall hat der Benutzer Zugriff auf alle Funktionen, die mit der Gruppe und dem individuellen Konto verbunden sind.

> [!TIP]
> Dieses Thema ist spezifisch für das Entwicklerprogramm von Windows-apps in [Partner Center](https://partner.microsoft.com/dashboard). Weitere Informationen zu Benutzerrollen im Hardware-Entwicklerprogramm finden Sie unter [Verwalten von Benutzerrolleng](https://docs.microsoft.com/windows-hardware/drivers/dashboard/managing-user-roles). Weitere Informationen zu Benutzerrollen im Windows-Desktopanwendungsprogramm finden Sie unter [Windows Desktopanwendungsprogramm](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users).


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>Kontobenutzern Rollen zuweisen

Standardmäßig wird eine Reihe von Standardrollen angezeigt, die Sie auswählen können, wenn Sie Ihrem Partner Center-Konto eine Benutzer-, Gruppen-oder Azure AD Anwendung hinzufügen. Jede Rolle verfügt über spezifische Berechtigungen, mit denen bestimmte Funktionen innerhalb des Kontos ausgeführt werden können. 

Sofern Sie keine [benutzerdefinierten Berechtigungen](#custom) durch die Auswahl **benutzerdefinierten Berechtigungen** verwenden, müssen alle Benutzer, Gruppen oder Azure AD-Anwendungen, die Sie einem Konto hinzufügen, mindestens einer der folgenden Standardrollen zugewiesen sein. 

> [!NOTE]
> Der **Besitzer** des Kontos ist die Person, die es als erste mit einem Microsoft-Konto erstellt hat (und keiner der Benutzer, die über Azure AD hinzugefügt wurden). Dieser Kontobesitzer ist die einzige Person mit Vollzugriff auf das Konto. Hierzu zählt die Möglichkeit, Apps zu löschen, zu erstellen und zu bearbeiten, alle Kontobenutzer zu bearbeiten sowie sämtliche finanziellen Einstellungen und Kontoeinstellungen zu ändern. 


| Role-Eigenschaft                 | Beschreibung              |
|----------------------|--------------------------|
| Manager              | Verfügt über vollständigen Zugriff auf das Konto, kann jedoch keine Steuer- und Auszahlungseinstellungen ändern. Dies umfasst die Verwaltung von Benutzern in Partner Center, aber beachten Sie, dass die Möglichkeit, Benutzer im Azure AD Mandanten zu erstellen und zu löschen, von der Berechtigung des Kontos in Azure AD abhängig ist. Das heißt, wenn einem Benutzer die Rolle "Manager" zugewiesen ist, aber keine globalen Administrator Berechtigungen im Azure Ad der Organisation vorhanden sind, können Sie keine neuen Benutzer erstellen oder Benutzer aus dem Verzeichnis löschen (obwohl Sie die Partner Center-Rolle eines Benutzers ändern können). <p> Beachten Sie Folgendes: Wenn das Partner Center-Konto mehr als einem Azure AD Mandanten zugeordnet ist, kann ein Manager keine kompletten Details für einen Benutzer anzeigen (einschließlich Vorname, Nachname, e-Mail-Adresse der Kenn Wort Wiederherstellung und ob es sich um einen Azure AD globalen Administrator handelt), es sei denn, Sie sind Anmeldung beim gleichen Mandanten wie dieser Benutzer mit einem Konto, das über globale Administrator Berechtigungen für diesen Mandanten verfügt. Allerdings können Sie Benutzer in jedem Mandanten hinzufügen und entfernen, der dem Partner Center-Konto zugeordnet ist. |
| Entwickler            | Kann Pakete hochladen und Apps und Add-Ons einreichen sowie den [Nutzungsbericht](usage-report.md) für Telemetriedetails einsehen. Kann auf [Geräte übergreifende](https://developer.microsoft.com/windows/project-rome) Funktionen zugreifen. Kann keine finanziellen Informationen oder Kontoeinstellungen anzeigen.   |
| Mitwirkender im Geschäftsbereich | Kann [Integritäts](health-report.md)- und [Nutzungs](usage-report.md)-Berichte anzeigen. Kann keine Produkte erstellen oder übermitteln, Kontoeinstellungen ändern oder finanzielle Informationen anzeigen.   |
| Mitwirkender im Finanzbereich  | Kann [Auszahlungsberichte](payout-summary.md), finanzielle Informationen und Erwerbsberichte anzeigen. Kann keine Änderungen an Apps, Add-Ons oder Kontoeinstellungen vornehmen.    |
| Händler             | Kann auf [Kundenbewertungen reagieren](respond-to-customer-reviews.md) und nicht finanzbezogene [Analyseberichte](analytics.md) einsehen. Kann keine Änderungen an Apps, Add-Ons oder Kontoeinstellungen vornehmen.      |

In der nachfolgenden Tabelle sind einige der spezifischen Features aufgeführt, die für diese Rollen (und für den Kontobesitzer) verfügbar sind.

|                                                       |    Kontobesitzer                 |    Manager                       |    Entwickler                     |    Mitwirkender im Geschäftsbereich    |    Mitwirkender im Finanzbereich    |    Händler                      |
|-------------------------------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    Erwerbs Bericht (einschließlich Near Real Time Data) |    Kann anzeigen                      |    Kann anzeigen                      |    Kein Zugriff                     |    Kein Zugriff               |    Kann anzeigen               |    Kein Zugriff                     |
|    Feedbackbericht/Antworten                          |    Kann Feedback anzeigen und senden    |    Kann Feedback anzeigen und senden    |    Kann Feedback anzeigen und senden    |    Kein Zugriff               |    Kein Zugriff              |    Kann Feedback anzeigen und senden    |
|    Integritäts Bericht (einschließlich Near Real Time Data)      |    Kann anzeigen                      |    Kann anzeigen                      |    Kann anzeigen                      |    Kann anzeigen                |    Kein Zugriff              |    Kein Zugriff                     |
|    Nutzungsbericht                                       |    Kann anzeigen                      |    Kann anzeigen                      |    Kann anzeigen                      |    Kann anzeigen                |    Kein Zugriff              |    Kein Zugriff                     |
|    Auszahlungskonto                                     |    Kann aktualisieren                    |    Kein Zugriff                     |    Kein Zugriff                     |    Kein Zugriff               |    Kann aktualisieren             |    Kein Zugriff                     |
|    Steuerprofil                                        |    Kann aktualisieren                    |    Kein Zugriff                     |    Kein Zugriff                     |    Kein Zugriff               |    Kann aktualisieren             |    Kein Zugriff                     |
|    Auszahlungsübersicht                                     |    Kann anzeigen                      |    Kein Zugriff                     |    Kein Zugriff                     |    Kein Zugriff               |    Kann anzeigen               |    Kein Zugriff                     |

Wenn keine der standardmäßigen Rollen geeignet ist oder wenn Sie den Zugriff auf bestimmte Apps und/oder Add-Ons einschränken möchten, können Sie benutzerdefinierte Berechtigungen für den Benutzer gewähren, indem Sie wie oben beschrieben **Berechtigungen anpassen** auswählen.


<span id="custom" />

## <a name="assign-custom-permissions-to-account-users"></a>Legen Sie benutzerdefinierte Berechtigungen für Kontenbenutzer fest

Um anstelle von Standardrollen benutzerdefinierte Berechtigungen zuzuordnen, klicken Sie beim Hinzufügen oder Bearbeiten des Benutzerkontos im Abschnitt **Rollen** auf **Berechtigungen anpassen**. 

Um eine Berechtigung für den Benutzer zu aktivieren, aktivieren Sie das Kontrollkästchen für die entsprechende Einstellung. 

![Anleitung für die Zugriffseinstellungen](images/permission_key.png)

- **Kein Zugriff**: Der Benutzer verfügt nicht über die angegebene Berechtigung.
- **Schreibgeschützt**: Der Benutzer kann Features im Zusammenhang mit dem angegebenen Bereich anzeigen, sie jedoch nicht ändern. 
- **Lese-/Schreibzugriff**: Der Benutzer kann für den Bereich Änderungen vornehmen sowie diesen anzeigen.
- **Gemischt**: Diese Option kann nicht direkt ausgewählt werden. **Gemischt** ist jedoch verfügbar, wenn Sie für die Berechtigung eine Zugriffskombination zugelassen haben. Wenn Sie z. B. **Schreibgeschützt** bei **Preise und Verfügbarkeit** für **Alle Produkte** festlegen, anschließend jedoch **Lese-/Schreibzugriff** auf **Preise und Verfügbarkeit** für ein bestimmtes Produkt gewähren, wird für **Preise und Verfügbarkeit** unter **Alle Produkte** „Gemischt“ angezeigt. Dasselbe gilt, wenn für einige Produkte als Berechtigung **Kein Zugriff** festgelegt ist, für andere jedoch **Lese-/Schreibzugriff** und/oder **Schreibgeschützt**.

Für einige Berechtigungen (z. B. im Zusammenhang mit dem Anzeigen von Analysedaten) kann nur Lesezugriff (**Schreibgeschützt**) gewährt werden. Beachten Sie, dass in der aktuellen Implementierung bei einigen Berechtigungen keine Unterscheidung zwischen **Schreibgeschützt** und **Lese-/Schreibzugriff** besteht. Ein besseres Verständnis der jeweiligen Funktionen, die mit **Schreibgeschützt** und/oder **Lese-/Schreibzugriff** gewährt werden, erhalten Sie anhand der Details zu den einzelnen Berechtigungen.

Die Details der einzelnen Berechtigungen werden in den folgenden Tabellen beschrieben.

## <a name="account-level-permissions"></a>Berechtigungen auf Kontoebene

Die Berechtigungen in diesem Abschnitt können nicht auf bestimmte Produkte beschränkt werden. Wird dem Benutzer Zugriff auf eine dieser Berechtigungen gewährt, gilt dies für das gesamte Konto.

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Name der Berechtigung</th>
    <th align="left">Schreibgeschützt</th>
    <th align="left">Lese-/Schreibzugriff</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    <b>Kontoeinstellungen</b>                    </td><td align="left">  Kann alle Seiten im Abschnitt <b>Kontoeinstellungen</b> anzeigen, einschließlich der <a href="managing-your-profile.md">Kontaktinformationen</a>.       </td><td align="left">  Kann alle Seiten im Abschnitt <b>Kontoeinstellungen</b> anzeigen. Kann Änderungen an <a href="managing-your-profile.md">Kontaktinformationen</a> und anderen Seiten, nicht jedoch am Auszahlungskonto oder Steuerprofil vornehmen (es sei denn, diese Berechtigung wird separat erteilt).            </td></tr>
<tr><td align="left">    <b>Kontobenutzer</b>                       </td><td align="left">  Kann Benutzer anzeigen, die dem Konto im Abschnitt <b>Benutzer</b> hinzugefügt wurden.          </td><td align="left">  Kann dem Konto Benutzer hinzufügen und im Abschnitt <b>Benutzer</b> Änderungen an vorhandenen Benutzern vornehmen.             </td></tr>
<tr><td align="left">      des <b>Ad-Leistungs Berichts auf Kontoebene</b></td><td align="left">  Kann den <a href="advertising-performance-report.md">Bericht zur Anzeigenleistung auf Kontoebene</a> anzeigen.      </td><td align="left">  N/V   </td></tr>
<tr><td align="left">    <b>Ad-Kampagnen</b>                        </td><td align="left">  Kann im Konto erstellte <a href="create-an-ad-campaign-for-your-app.md">Anzeigenkampagnen</a> anzeigen.      </td><td align="left">  Kann im Konto erstellte <a href="create-an-ad-campaign-for-your-app.md">Anzeigenkampagnen</a> erstellen, verwalten und anzeigen.          </td></tr>
<tr><td align="left">    <b>AD-Vermittlungs</b>                        </td><td align="left">  Kann AD-Vermittlungs Konfigurationen für alle Produkte im Konto anzeigen.    </td><td align="left">  Können die Konfigurationen der AD-Vermittlung für alle Produkte im Konto anzeigen und ändern.        </td></tr>
<tr><td align="left">    <b>AD-Vermittlungs Berichte</b>                </td><td align="left">  Kann den <a href="ad-mediation-report.md">Bericht zur Anzeigenvermittlung</a> für alle Produkte des Kontos anzeigen.    </td><td align="left">  N/V    </td></tr>
<tr><td align="left">                   der <b>AD-Leistungsberichte</b></td><td align="left">  Kann <a href="advertising-performance-report.md">Berichte zur Anzeigenleistung</a> für alle Produkte des Kontos anzeigen.       </td><td align="left">  N/V         </td></tr>
<tr><td align="left">    <b>Ad-Einheiten</b>                            </td><td align="left">  Kann die für das Konto erstellten <a href="in-app-ads.md">Anzeigeneinheiten</a> anzeigen.    </td><td align="left">  Kann <a href="in-app-ads.md">Anzeigeneinheiten</a> für das Konto erstellen, verwalten und anzeigen.             </td></tr>
<tr><td align="left">    <b>Partner Werbung</b>                       </td><td align="left">  Kann die Nutzung von <a href="about-affiliate-ads.md">Partneranzeigen</a> für alle Produkte des Kontos anzeigen.    </td><td align="left">  Kann die Nutzung von <a href="about-affiliate-ads.md">Partneranzeigen</a> für alle Produkte des Kontos verwalten und anzeigen.                </td></tr>
<tr><td align="left">    <b>Leistungsberichte</b> für Unternehmen      </td><td align="left">  Kann den <a href="affiliates-performance-report.md">Bericht zur Partneranzeigenleistung</a> für alle Produkte des Kontos anzeigen.   </td><td align="left">  N/V   </td></tr>
<tr><td align="left">    <b>App install ADS Reports</b>             </td><td align="left">  Können den <a href="promote-your-app-report.md">Bericht „Anzeigenkampagne“</a> anzeigen.           </td><td align="left">  N/V   </td></tr>
<tr><td align="left">    <b>Community ADS</b>                       </td><td align="left">  Kann die Nutzung kostenloser <a href="about-community-ads.md">Community-Anzeigen</a> für alle Produkte des Kontos anzeigen.          </td><td align="left">  Kann die Nutzung kostenloser <a href="about-community-ads.md">Community-Anzeigen</a> für alle Produkte des Kontos erstellen, verwalten und anzeigen.               </td></tr>
<tr><td align="left">    <b>Kontaktinformationen</b>                        </td><td align="left">  Kann <a href="managing-your-profile.md">Kontaktinformationen</a> im Abschnitt mit den Kontoeinstellungen anzeigen.        </td><td align="left">  Kann <a href="managing-your-profile.md">Kontaktinformationen</a> im Abschnitt mit den Kontoeinstellungen anzeigen und bearbeiten.            </td></tr>
<tr><td align="left">    <b>Coppa</b> -Kompatibilitäts                    </td><td align="left">  Kann für alle Produkte des Kontos die Einstellungen für die <a href="in-app-ads.md#coppa-compliance">COPPA-Compliance</a> anzeigen (die angeben, ob sich Produkte an Kinder unter 13 Jahren richten).                                            </td><td align="left">  Kann für alle Produkte des Kontos die Einstellungen für die <a href="in-app-ads.md#coppa-compliance">COPPA-Compliance</a> anzeigen und bearbeiten (die angeben, ob sich Produkte an Kinder unter 13 Jahren richten).         </td></tr>
<tr><td align="left">    <b>Kundengruppen</b>                     </td><td align="left">  Kann <a href="create-customer-groups.md">Kundengruppen</a> (Segmente und bekannte Benutzergruppen) anzeigen.      </td><td align="left">  Können <a href="create-customer-groups.md">Kundengruppen</a> (Segmente und bekannte Benutzergruppen) erstellen, bearbeiten und anzeigen.       </td></tr>
<tr><td align="left">    <b>Verwalten von Produktgruppen</b>&nbsp;*                            </td><td align="left">  Kann die Seite zum Erstellen neuer Produktgruppen anzeigen, jedoch keine neuen Produktgruppen im Konto erstellen.    </td><td align="left">  Kann Produktgruppen erstellen und bearbeiten.     </td></tr>
<tr><td align="left">    <b>Neue apps</b>                            </td><td align="left">  Kann die Seite zum Erstellen neuer Apps anzeigen, jedoch keine neuen Apps im Konto erstellen.    </td><td align="left">  Kann im Konto <a href="create-your-app-by-reserving-a-name.md">neue Apps erstellen</a>, indem neue App-Namen reserviert werden. Zudem können Übermittlungen erstellt und Apps an den Store übermittelt werden.     </td></tr>
<tr><td align="left">    <b>Neue Bündel</b>&nbsp;*                       </td><td align="left">  Kann die Seite zum Erstellen neuer Bündel anzeigen, jedoch keine neuen Bündel im Konto erstellen.     </td><td align="left">  Kann neue Produktbündel erstellen.          </td></tr>
<tr><td align="left">    <b>Partnerdienste</b>&nbsp;*                  </td><td align="left">  Kann Zertifikate für das Installieren in Diensten zum Abrufen von XTokens anzeigen.     </td><td align="left">  Kann Zertifikate für das Installieren in Diensten zum Abrufen von XTokens verwalten und anzeigen.       </td></tr>
<tr><td align="left">    <b>Auszahlungs Konto</b>                      </td><td align="left">  Kann <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">Auszahlungskontodaten</a> unter <b>Kontoeinstellungen</b> anzeigen.     </td><td align="left">  Kann <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">Auszahlungskontodaten</a> unter <b>Kontoeinstellungen</b> bearbeiten und anzeigen.       </td></tr>
<tr><td align="left">    <b>Auszahlungs Zusammenfassungs</b>                      </td><td align="left">  Kann die <a href="payout-summary.md">Auszahlungsübersicht</a> anzeigen, um auf Auszahlungsberichtsdaten zuzugreifen und diese herunterzuladen.       </td><td align="left">  Kann die <a href="payout-summary.md">Auszahlungsübersicht</a> anzeigen, um auf Auszahlungsberichtsdaten zuzugreifen und diese herunterzuladen.   </td></tr>
<tr><td align="left">    <b>Vertrauende Seiten</b>&nbsp;*                   </td><td align="left">  Kann vertrauende Seiten anzeigen, um XTokens abzurufen.    </td><td align="left">  Kann vertrauende Seiten verwalten und anzeigen, um XTokens abzurufen.     </td></tr>
<tr><td align="left">    <b>Sandboxes</b>&nbsp;*                         </td><td align="left">  Kann auf die Seite <b>Sandboxes</b> zugreifen und für das Konto die Sandboxes sowie alle gültigen Konfigurationen anzeigen. Kann nicht die Produkte und Übermittlungen für die jeweilige Sandbox anzeigen, sofern keine entsprechenden Berechtigungen auf Produktebene erteilt wurden. </td><td align="left">  Kann auf die Seite <b>Sandboxes</b> zugreifen und die Sandboxes im Konto anzeigen und verwalten, z. B. um Sandboxes zu erstellen und zu löschen oder deren Konfiguration zu verwalten. Kann nicht die Produkte und Übermittlungen für die jeweilige Sandbox anzeigen, sofern keine entsprechenden Berechtigungen auf Produktebene erteilt wurden.    </td></tr>
<tr><td align="left">    <b>Store-Verkaufsereignisse</b>&nbsp;*                            </td><td align="left">  N/V    </td><td align="left">  Kann die Option konfigurieren, automatisch Produkte im Store in Verkaufsereignisse aufzunehmen.     </td></tr>
<tr><td align="left">    <b>Steuer Profil</b>                         </td><td align="left">  Kann <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">Steuerprofildaten und -formulare</a> in den <b>Kontoeinstellungen</b> anzeigen.     </td><td align="left">  Kann Steuerformulare ausfüllen und <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">Steuerprofildaten</a> in den <b>Kontoeinstellungen</b> aktualisieren.     </td></tr>
<tr><td align="left">    <b>Testkonten</b>&nbsp;*                     </td><td align="left">  Kann Konten zum Testen der Xbox Live-Konfiguration anzeigen.      </td><td align="left">  Kann Konten zum Testen der Xbox Live-Konfiguration erstellen, verwalten und anzeigen.      </td></tr>
<tr><td align="left">    <b>Xbox-Geräte</b>                        </td><td align="left">  Kann im Abschnitt <b>Kontoeinstellungen</b> die für das Konto aktivierten Xbox-Entwicklungskonsolen anzeigen.       </td><td align="left">  Kann die für das Konto aktivierten Xbox-Entwicklungskonsolen im Abschnitt <b>Kontoeinstellungen</b> hinzufügen, entfernen und anzeigen.     </td></tr>
    </tbody>
    </table>

\* mit einem Sternchen (*) markierten Berechtigungen gewähren Zugriff auf Funktionen, die für alle Konten nicht verfügbar sind. Wenn Ihr Konto nicht für diese Features aktiviert wurde, ist Ihre Auswahl für diese Berechtigungen nicht wirksam.   


## <a name="product-level-permissions"></a>Berechtigungen auf Produktebene

Die Berechtigungen in diesem Abschnitt können für alle Produkte im Konto erteilt werden. Zudem können sie so angepasst werden, dass sie nur für ein oder mehrere bestimmte Produkte erteilt werden. 

Diese Berechtigungen auf Produktebene sind in vier Kategorien unterteilt: **Analysen**, **Monetarisierung**, **Veröffentlichung** und **Xbox Live**. Sie können die einzelnen Kategorien erweitern, um die jeweiligen Berechtigungen anzuzeigen. Sie haben auch die Option **Alle Berechtigungen** für einen oder mehrere bestimmte Produkte zu aktivieren.

Um eine Berechtigung für jedes Produkt des Kontos zu erteilen, treffen Sie in der Zeile **Alle Produkte** Ihre Auswahl für diese Berechtigung (indem Sie für das Feld **Schreibgeschützt** oder **Lese-/Schreibzugriff** aktivieren). 
 
> [!TIP]
> Die für **Alle Produkte** getroffene Auswahl gilt für sämtliche derzeit im Konto vorhandenen Produkte sowie für alle zukünftig für das Konto erstellten Produkte. Um zu verhindern, dass die Berechtigungen aus der Anwendung auf zukünftige Produkte zutreffen, wählen Sie sämtliche Produkte einzeln aus, anstatt **Alle Produkte** zu wählen.

Unterhalb der Zeile **Alle Produkte** sind die einzelnen Produkte des Kontos in jeweils eigenen Zeilen aufgeführt. Um nur für ein bestimmtes Produkt eine Berechtigung zu erteilen, treffen Sie Ihre Berechtigungsauswahl in der Zeile für das Produkt.

Jedes Add-On wird in einer separaten Zeile unterhalb des übergeordneten Produkts aufgeführt. Zudem gibt es die Zeile **Alle Add-Ons**. Die unter **Alle Add-Ons** getroffene Auswahl gilt für alle aktuellen Add-Ons des Produkts sowie für alle zukünftig für das Produkte erstellten Add-Ons.

Beachten Sie, dass einige Berechtigungen nicht für Add-Ons festgelegt werden können. Dies liegt entweder daran, dass sie nicht für Add-Ons gelten (z. B. die Berechtigung **Kundenfeedback**), oder dass die auf der Ebene des übergeordneten Produkts erteilte Berechtigung für alle Add-Ons des Produkts gilt (z. B. **Werbecodes**). Beachten Sie jedoch, dass alle für Add-Ons verfügbaren Berechtigungen separat festgelegt werden müssen. Add-Ons erben nicht die für das übergeordnete Produkt getroffene Auswahl. Wenn Sie z. B. einem Benutzer gestatten möchten, Preis- und Verfügbarkeitsoptionen für ein Add-On vorzunehmen, müssen Sie die Berechtigung **Preise und Verfügbarkeit** für das Add-On (oder für **Alle Add-Ons**) unabhängig davon aktivieren, ob Sie die Berechtigung **Preise und Verfügbarkeit** für das übergeordnete Produkt erteilt haben. 


### <a name="analytics"></a>Analysen

<table>
    <thead>
    <tr class="header">
    <th align="left">Name der Berechtigung</th>
    <th align="left">Schreibgeschützt</th>
    <th align="left">Lese-/Schreibzugriff</th>
    <th align="left">Schreibgeschützt&nbsp;(Add-On) </th>
    <th align="left">Lese- und Schreibzugriff&nbsp;(Add-On)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Akquisitionen</b> (einschließlich Near Real Time Data) </td><td>    Kann die Berichte <a href="acquisitions-report.md">Käufe</a> und <a href="add-on-acquisitions-report.md">Add-On-Käufe</a> für das Produkt anzeigen.        </td><td>    N/V    </td><td>    N/v (Einstellungen für das übergeordnete Produkt enthalten den Bericht **zu den Add-on-Akquisitionen** )        </td><td>    N/V                         </td></tr>
    <tr><td align="left">    <b>Verwendungs</b> </td><td>    Kann den <a href="usage-report.md">Bericht „Nutzung“</a> für das Produkt anzeigen.     </td><td>    N/V       </td><td>    N/V     </td><td>    N/V         </td></tr>
    <tr><td align="left">    <b>Health</b> (einschließlich Near Real Time Data) </td><td>    Kann den <a href="health-report.md">Bericht „Integrität“</a> für das Produkt anzeigen.    </td><td>    N/V     </td><td>    N/V     </td><td>    N/V         </td></tr>
    <tr><td align="left">    <b>Kundenfeedback</b>    </td><td>    Kann die Berichte <a href="reviews-report.md">Rezensionen</a> und <a href="feedback-report.md">Feedback</a> für das Produkt anzeigen.       </td><td>    Nicht verfügbar (Um auf Feedback oder Rezensionen reagieren zu können, muss die Berechtigung <b>Kunden kontaktieren</b> erteilt werden)   </td><td>    N/V     </td><td>    N/V         </td></tr>
    <tr><td align="left">    <b>Xbox Analytics</b> - </td><td>    Kann den <a href="xbox-analytics-report.md">Xbox Analytics-Bericht</a> für das Produktanzeigen.    </td><td>    N/V   </td><td>    N/V       </td><td>    N/V          </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>Monetisierung

<table>
    <thead>
    <tr class="header">
    <th align="left">Name der Berechtigung</th>
    <th align="left">Schreibgeschützt</th>
    <th align="left">Lese-/Schreibzugriff</th>
    <th align="left">Schreibgeschützt&nbsp;(Add-On) </th>
    <th align="left">Lese- und Schreibzugriff&nbsp;(Add-On)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    <b>Aktionscodes</b>     </td><td>    Kann Aufträge und Nutzungsdaten für <a href="generate-promotional-codes.md">Werbecodes</a> für das Produkt und dessen Add-Ons anzeigen.         </td><td>    Kann Aufträge und Nutzungsdaten für <a href="generate-promotional-codes.md">Werbecodes</a> für das Produkt und dessen Add-Ons anzeigen, verwalten und erstellen.          </td><td>    Nicht verfügbar (Einstellungen für das übergeordnete Produkt gelten für alle Add-Ons)     </td><td>    Nicht verfügbar (Einstellungen für das übergeordnete Produkt gelten für alle Add-Ons)     </td></tr>
    <tr><td align="left">    <b>Gezielte Angebote</b>     </td><td>    Kann die <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">zielgerichteten Angebote</a> für das Produkt sehen.         </td><td>    Kann <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">zielgerichtete Angebote</a> für das Produkt erstellen, verwalten und anzeigen.          </td><td>    N/V     </td><td>    N/V      </td></tr>
    <tr><td align="left">    <b>Kunden   kontaktieren</b></td><td>    Kann <a href="respond-to-customer-feedback.md">Reaktionen auf Kundenfeedback</a> und <a href="respond-to-customer-reviews.md">Antworten auf Kundenrezensionen</a> anzeigen, sofern zudem die Berechtigung <b>Kundenfeedback</b> erteilt wurde. Kann zudem für das Produkt erstellte <a href="send-push-notifications-to-your-apps-customers.md">benutzerorientierte Benachrichtigungen</a> anzeigen.    </td><td>    Kann <a href="respond-to-customer-feedback.md">auf Kundenfeedback reagieren</a> und <a href="respond-to-customer-reviews.md">auf Kunden Reviews reagieren</a>, solange die <b>Kundenfeedback</b> Berechtigung ebenfalls erteilt wurde. Kann zudem für das Produkt <a href="send-push-notifications-to-your-apps-customers.md">benutzerorientierte Benachrichtigungen erstellen und senden</a>.                   </td><td>    N/V         </td><td>    N/V                          </td></tr>
    <tr><td align="left">    <b>Eller</b></td><td>    Kann <a href="../monetize/run-app-experiments-with-a-b-testing.md">Experimente (A/B-Tests)</a> sowie Experimentdaten für das Produkt anzeigen.   </td><td>    Kann <a href="../monetize/run-app-experiments-with-a-b-testing.md">Experimente (A/B-Tests)</a> für das Produkt erstellen, verwalten und anzeigen sowie Experimentdaten anzeigen.     </td><td>    N/V  </td><td>    N/V                 </td></tr>
    <tr><td align="left">    <b>Store-Verkaufsereignisse</b>&nbsp;*</td><td>    Kann den Verkaufsereignisstatus für das Produkt anzeigen.   </td><td>    Kann das Produkt zu Verkaufsereignissen hinzufügen und Rabatte konfigurieren.      </td><td>    Kann den Verkaufsereignisstatus für das Produkt anzeigen.   </td><td>    Kann das Produkt zu Verkaufsereignissen hinzufügen und Rabatte konfigurieren.      </td></tr>
    </tbody>
    </table>

### <a name="publishing"></a>Publishing 

<table>
    <thead>
    <tr class="header">
    <th align="left">Name der Berechtigung</th>
    <th align="left">Schreibgeschützt</th>
    <th align="left">Lese-/Schreibzugriff</th>
    <th align="left">Schreibgeschützt&nbsp;(Add-On) </th>
    <th align="left">Lese- und Schreibzugriff&nbsp;(Add-On)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">       für das <b>Produkt Setup</b></td><td>    Kann die Produkt Einrichtungs Seite der Produkte anzeigen.     </td><td>    Kann die Produkt Einrichtungs Seite von Produkten anzeigen und bearbeiten. </td><td>    Die Seite "Produkt Setup" von Add-ons kann angezeigt werden.   </td><td>    Kann die Add-ons für die Produktinstallation anzeigen und bearbeiten.          </td></tr>
    <tr><td align="left">    <b>Preise und Verfügbarkeit</b>  </td><td>    Kann die Seite " <a href="set-app-pricing-and-availability.md">Preise und Verfügbarkeit</a> " der Produkte anzeigen.     </td><td>    Kann die Seite " <a href="set-app-pricing-and-availability.md">Preise und Verfügbarkeit</a> " von Produkten anzeigen und bearbeiten. </td><td>    Kann die Seite " <a href="set-add-on-pricing-and-availability.md">Preise und Verfügbarkeit</a> " von Add-ons anzeigen.   </td><td>    Kann die Seite " <a href="set-add-on-pricing-and-availability.md">Preise und Verfügbarkeit</a> " von Add-ons anzeigen und bearbeiten.          </td></tr>
    <tr><td align="left">    <b>Eigenschaften</b>   </td><td>    Kann die <a href="enter-app-properties.md">Eigenschaften</a> Seite von Produkten anzeigen.      </td><td>    Kann die <a href="enter-app-properties.md">Eigenschaften</a> Seite von Produkten anzeigen und bearbeiten.       </td><td>    Die <a href="enter-add-on-properties.md">Eigenschaften</a> Seite von Add-ons kann angezeigt werden.     </td><td>    Kann die <a href="enter-add-on-properties.md">Eigenschaften</a> Seite von Add-ons anzeigen und bearbeiten.               </td></tr>
    <tr><td align="left">    <b>Alters Bewertungs</b>    </td><td>    Kann die Seite <a href="age-ratings.md">Alters Bewertungen</a> der Produkte anzeigen.       </td><td>    Kann die <a href="age-ratings.md">Alters Bewertungs</a> Seite der Produkte anzeigen und bearbeiten.    </td><td>    Kann die Seite mit den Alters Bewertungen von Add-ons anzeigen.          </td><td>     Kann die Seite mit den Alters Bewertungen von Add-ons anzeigen und bearbeiten.       </td></tr>
    <tr><td align="left">    <b>Pakete</b>        </td><td>    Kann die Seite <a href="upload-app-packages.md">Pakete</a> der Produkte anzeigen.  </td><td>    Kann die Seite " <a href="upload-app-packages.md">Pakete</a> " von Produkten anzeigen und bearbeiten, einschließlich des Uploads von Paketen.     </td><td>   Kann die Seite " <a href="upload-app-packages.md">Pakete</a> " von Addons anzeigen (falls zutreffend).   </td><td>     Kann die Seite " <a href="upload-app-packages.md">Pakete</a> " von Addons anzeigen und bearbeiten (falls zutreffend).             </td></tr>
    <tr><td align="left">    <b>Store-Listen</b>  </td><td>    Kann die <a href="create-app-store-listings.md">Seite (n)</a> von Produkten für die Speicher Auflistung anzeigen.  </td><td>    Kann die <a href="create-app-store-listings.md">Seite (n)</a> von Produkten für die Store-Auflistung anzeigen und bearbeiten und neue Store-Listen für verschiedene Sprachen hinzufügen.     </td><td>    Kann die <a href="create-add-on-store-listings.md">Seite (n)</a> von Add-ons für die Speicher Auflistung anzeigen.            </td><td>    Kann die <a href="create-add-on-store-listings.md">Seite (n)</a> von Add-ons für die Store-Auflistung anzeigen und bearbeiten und Speicher Listen für verschiedene Sprachen hinzufügen.                 </td></tr>
    <tr><td align="left">    <b>Speicher Übermittlung</b>     </td><td>    Es wird kein Zugriff gewährt, wenn diese Berechtigung als schreibgeschützt festgelegt ist.           </td><td>    Kann das Produkt an den Store übermitteln und Zertifizierungsberichte anzeigen. Dies gilt sowohl für neue als auch für aktualisierte Übermittlungen. </td><td>Es wird kein Zugriff gewährt, wenn diese Berechtigung als schreibgeschützt festgelegt ist.     </td><td>    Kann das Add-On an den Store übermitteln und Zertifizierungsberichte anzeigen. Dies gilt sowohl für neue als auch für aktualisierte Übermittlungen.</td></tr>
    <tr><td align="left">    <b>Neue</b> Übermittlungs Erstellung       </td><td>    Es wird kein Zugriff gewährt, wenn diese Berechtigung als schreibgeschützt festgelegt ist.        </td><td>    Kann neue <a href="app-submissions.md">Übermittlungen</a> für das Produkt erstellen.  </td><td>    Es wird kein Zugriff gewährt, wenn diese Berechtigung als schreibgeschützt festgelegt ist.   </td><td>    Kann neue <a href="add-on-submissions.md">Übermittlungen</a> für das Add-On erstellen.        </td></tr>
    <tr><td align="left">    <b>Neue Add-ons</b>    </td><td>    Es wird kein Zugriff gewährt, wenn diese Berechtigung als schreibgeschützt festgelegt ist. </td><td>    Kann neue <a href="set-your-add-on-product-id.md">Add-Ons</a> für das Produkt erstellen. </td><td>    N/V    </td><td>    N/V        </td></tr>
    <tr><td align="left">    <b>Namens Reservierungen</b>   </td><td>    Kann die Seite <a href="manage-app-names.md">App-Namen verwalten</a> für das Produkt anzeigen.</td><td>    Kann die Seite <a href="manage-app-names.md">App-Namen verwalten</a> für das Produkt anzeigen und bearbeiten sowie zusätzliche Namen reservieren und reservierte Namen löschen. </td><td>   Kann reservierte Namen für das Add-On anzeigen.    </td><td>   Kann reservierte Namen für das Add-On anzeigen und bearbeiten.          </td></tr>
    <tr><td align="left">        für die <b>Festplatten Anforderung</b></td><td>    Die Anforderungsseite kann von der Festplatte angezeigt werden. </td><td>    Kann Festplatten Anforderungen erstellen. </td><td>   N/V    </td><td>   N/V          </td></tr>
    <tr><td align="left">    <b>Festplatten-Lizenz</b>   </td><td>    Kann die Seite "Lizenzen" der Seite "Lizenzen" anzeigen</td><td>    Kann Festplatten-Lizenzen erstellen. </td><td>   N/V    </td><td>   N/V          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live-\*

<table>
    <thead>
    <tr class="header">
    <th align="left">Name der Berechtigung</th>
    <th align="left">Schreibgeschützt</th>
    <th align="left">Lese-/Schreibzugriff</th>
    <th align="left">Schreibgeschützt&nbsp;(Add-On) </th>
    <th align="left">Lese- und Schreibzugriff&nbsp;(Add-On)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    * der vertrauenden <b>Seiten</b>&nbsp;</td><td>    Kann die Seite der vertrauenden Seiten eines Kontos anzeigen.   </td><td>    Kann die Seite der vertrauenden Seiten eines Kontos anzeigen und bearbeiten.    </td><td>    N/V    </td><td>    N/V                      </td></tr>
    <tr><td align="left">    <b>Partner Dienste</b>&nbsp;*</td><td>    Kann die Seite "Webdienste" eines Kontos anzeigen.  </td><td>    Kann die Seite "Webdienste" eines Kontos anzeigen und bearbeiten.      </td><td>    N/V    </td><td>    N/V                      </td></tr>
    <tr><td align="left">    <b>Xbox-Test Konten</b>&nbsp;*</td><td>    Kann die Seite "Xbox-Test Konten" eines Kontos anzeigen.  </td><td>    Kann die Seite "Xbox-Test Konten" eines Kontos anzeigen und bearbeiten.    </td><td>    N/V    </td><td>    N/V                      </td></tr>
    <tr><td align="left">    <b>Xbox-Test Konten pro Sandbox</b>&nbsp;*</td><td>    Die Seite Xbox-Test Konten kann nur für die angegebenen Sand Fächer eines Kontos angezeigt werden.  </td><td>    Kann den Xbox-Test anzeigen und bearbeiten.   <tr><td align="left">    <b>Konten Seite nur für die angegebenen Sandkästen eines Kontos    </td><td>    N/V    </td><td>    N/V                      </td></tr>
    <tr><td align="left"><b>Xbox-Geräte</b>&nbsp;*</td><td>    Kann die Seite Xbox One-Entwicklungs Konsolen eines Kontos anzeigen.  </td><td>    Kann die Seite Xbox One-Entwicklungs Konsolen eines Kontos anzeigen und bearbeiten.    </td><td>    N/V    </td><td>    N/V                      </td></tr>
    <tr><td align="left">    <b>Xbox-Geräte pro Sandbox</b>&nbsp;*</td><td>    Kann die Seite Xbox One-Entwicklungs Konsolen nur für die angegebenen Sand Fächer eines Kontos anzeigen.  </td><td>    Kann die Seite Xbox One-Entwicklungs Konsolen nur für die angegebenen Sand Fächer eines Kontos anzeigen und bearbeiten.    </td><td>    N/V    </td><td>    N/V                      </td></tr>
    <tr><td align="left">    <b>App-Kanäle</b>&nbsp;*</td><td>    N/V  </td><td>    Kann Werbevideokanäle auf der Xbox-Konsole für die Anzeige über OneGuide veröffentlichen.    </td><td>    N/V    </td><td>    N/V                      </td></tr>
    <tr><td align="left">    <b>Dienst Konfigurations</b>&nbsp;*</td><td>    Kann die Seite für die Xbox Live-Dienst Konfiguration eines Produkts anzeigen.  </td><td>    Kann die Seite für die Xbox Live-Dienst Konfiguration eines Produkts anzeigen und bearbeiten.    </td><td>    N/V    </td><td>    N/V                      </td></tr>
    <tr><td align="left">    <b>Tools Access</b>&nbsp;*</td><td>    Kann Xbox Live-Tools für ein Produkt ausführen, um nur Daten anzuzeigen.  </td><td>    Kann Xbox Live-Tools auf einem Produkt ausführen, um Daten anzuzeigen und zu bearbeiten.    </td><td>    N/V    </td><td>    N/V                      </td></tr>
</tbody>
</table>

\* mit einem Sternchen (*) markierten Berechtigungen gewähren Zugriff auf Funktionen, die für alle Konten nicht verfügbar sind. Wenn Ihr Konto nicht für diese Features aktiviert wurde, ist Ihre Auswahl für diese Berechtigungen nicht wirksam.  
