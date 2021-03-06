---
title: Authentifizierungsoptionen für die Registrierung
description: Hier erfahren Sie mehr über Authentifizierungsoptionen für eine private Azure-Containerregistrierung wie z. B. das Anmelden mit einer Azure Active Directory-Identität, mithilfe von Dienstprinzipalen sowie mittels optionalen Administratoranmeldeinformationen.
ms.topic: article
ms.date: 01/30/2020
ms.openlocfilehash: 5459ac29c1264b18404cb2863b9d4209907ac029
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2020
ms.locfileid: "77152942"
---
# <a name="authenticate-with-an-azure-container-registry"></a>Authentifizieren mit einer Azure-Containerregistrierung

Es gibt mehrere Möglichkeiten, sich mit einer Azure-Containerregistrierung zu authentifizieren, von denen jede für eine oder mehrere Registrierungsverwendungsszenarien gilt.

Empfohlene Möglichkeiten umfassen die Authentifizierung bei einer Registrierung durch [individuelle Anmeldung](#individual-login-with-azure-ad), oder Ihre Anwendungen und Containerorchestratoren können eine unbeaufsichtigte oder „monitorlose“ Authentifizierung mithilfe eines Azure Active Directory-[Dienstprinzipals](#service-principal) (Azure AD) durchführen.

## <a name="authentication-options"></a>Authentifizierungsoptionen

In der folgenden Tabelle werden die verfügbaren Authentifizierungsmethoden und empfohlenen Szenarios aufgeführt. Weitere Informationen finden Sie in den verlinkten Inhalten.

| Methode                               | Authentifizierung                                           | Szenarien                                                            | RBAC                             | Einschränkungen                                |
|---------------------------------------|-------------------------------------------------------|---------------------------------------------------------------------|----------------------------------|--------------------------------------------|
| [Individuelle AD-Identität](#individual-login-with-azure-ad)                | `az acr login`  in der Azure CLI                             | Interaktiver Push/Pull von Entwicklern oder Testern                                    | Ja                              | Das AD-Token muss alle 3 Stunden erneuert werden.     |
| [AD-Dienstprinzipal](#service-principal)                  | `docker login`<br/><br/>`az acr login` in der Azure CLI<br/><br/> Registrierungsanmeldungseinstellungen in APIs oder Tools<br/><br/> [PullSecret in Kubernetes](container-registry-auth-kubernetes.md)                                           | Unbeaufsichtigter Push aus der CI/CD-Pipeline<br/><br/> Unbeaufsichtigter Pull in Azure oder externe Dienste  | Ja                              | Standardablaufzeit für Dienstprinzipalkennwörter beträgt 1 Jahr       |                                                           
| [Integration mit Azure Kubernetes Service](../aks/cluster-container-registry-integration.md?toc=/azure/container-registry/toc.json&bc=/azure/container-registry/breadcrumb/toc.json)                    | Registrierung anfügen, wenn AKS-Cluster erstellt oder aktualisiert werden  | Unbeaufsichtigter Pull in AKS-Cluster                                                  | Nein, Zugriff ist nur per Pull möglich             | Nur mit AKS-Cluster verfügbar            |
| [Verwaltete Identität für Azure-Ressourcen](container-registry-authentication-managed-identity.md)  | `docker login`<br/><br/> `az acr login`  in der Azure CLI                                       | Unbeaufsichtigter Push aus der Azure CI/CD-Pipeline<br/><br/> Unbeaufsichtigter Pull in Azure-Dienste<br/><br/>   | Ja                              | Kann nur über Azure-Dienste verwendet werden, die [verwaltete Identitäten für Azure-Ressourcen unterstützen](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md#azure-services-that-support-managed-identities-for-azure-resources)              |
| [Administratorbenutzer](#admin-account)                            | `docker login`                                          | Interaktiver Push/Pull durch einen einzelnen Entwickler oder Tester                           | Nein, der Zugriff erfolgt immer per Pull/Push.  | Nur ein Konto pro Registrierung, wird nicht für mehrere Benutzer empfohlen         |
| [Repositorybezogene Zugriffstoken](container-registry-repository-scoped-permissions.md)               | `docker login`<br/><br/>`az acr login` in der Azure CLI   | Interaktiver Push/Pull zum Repository durch einen einzelnen Entwickler oder Tester<br/><br/> Unbeaufsichtigter Push/Pull zum Repository durch ein einzelnes System oder ein externes Gerät                  | Ja                              | Derzeit nicht mit AD-Identität integriert  |

## <a name="individual-login-with-azure-ad"></a>Individuelle Anmeldung bei Azure AD

Authentifizieren Sie sich bei der direkten Arbeit mit der Registrierung wie der Pullübertragung von Images auf Ihre und der Pushübertragung von Images von Ihrer Entwicklungsarbeitsstation mithilfe des [az acr login](/cli/azure/acr?view=azure-cli-latest#az-acr-login) -Befehls in der [Azure CLI](/cli/azure/install-azure-cli):

```azurecli
az acr login --name <acrName>
```

Bei der Anmeldung mit `az acr login` verwendet die CLI das Token, das erstellt wurde, als Sie [az login](/cli/azure/reference-index#az-login) ausgeführt haben, zur nahtlosen Authentifizierung Ihrer Sitzung mit Ihrer Registrierung. Docker muss in Ihrer Umgebung installiert sein und ausgeführt werden, damit der Authentifizierungsablauf abgeschlossen werden kann. `az acr login` verwendet den Docker-Client, um ein Azure Active Directory-Token in der Datei `docker.config` festzulegen. Sobald Sie sich auf diese Weise angemeldet haben, werden Ihre Anmeldeinformationen zwischengespeichert, und nachfolgende `docker`-Befehle in Ihrer Sitzung benötigen weder einen Benutzernamen noch das Kennwort.

> [!TIP]
> Verwenden Sie außerdem `az acr login` zum Authentifizieren einer einzelnen Identität, wenn Sie Artefakte an Ihre Registrierung pushen/pullen möchten, die keine Docker-Images sind (z. B. [OCI-Artefakte](container-registry-oci-artifacts.md)).  


Für den Zugriff auf die Registrierung ist das von `az acr login` verwendete Token **3 Stunden** lang gültig. Daher empfehlen wir Ihnen, sich immer bei der Registrierung anzumelden, bevor Sie einen `docker`-Befehl ausführen. Wenn das Token abgelaufen ist, können Sie es aktualisieren, indem Sie den `az acr login`-Befehl erneut zur Authentifizierung verwenden. 

Die Verwendung von `az acr login` mit Azure-Identitäten ermöglicht [rollenbasierten Zugriff](../role-based-access-control/role-assignments-portal.md). In einigen Szenarien können Sie sich bei einer Registry mit Ihrer eigenen individuellen Identität in Azure AD anmelden. Bei dienstübergreifenden Szenarios oder zum Erfüllen der Anforderungen einer Arbeitsgruppe oder eines Entwicklungsworkflows, für die Sie den Zugriff nicht individuell verwalten möchten, können Sie sich auch mit einer [verwalteten Identität für Azure-Ressourcen anmelden](container-registry-authentication-managed-identity.md).

## <a name="service-principal"></a>Dienstprinzipal

Wenn Sie Ihrer Registrierung einen [Dienstprinzipal](../active-directory/develop/app-objects-and-service-principals.md) zuweisen, kann Ihre Anwendung oder Ihr Dienst diesen für die monitorlose Authentifizierung verwenden. Dienstprinzipale ermöglichen [rollenbasierten Zugriff](../role-based-access-control/role-assignments-portal.md) auf eine Registrierung, und Sie können einer Registrierung mehrere Dienstprinzipale zuweisen. Mit mehreren Dienstprinzipalen können Sie unterschiedliche Zugriffsberechtigungen für verschiedene Anwendungen definieren.

Die folgenden Rollen für eine Containerregistrierung sind verfügbar:

* **AcrPull**: Pull

* **AcrPush**: Pull und Push

* **Besitzer**: Pull, Push und Zuweisen von Rollen an andere Benutzer

Eine vollständige Liste der Rollen finden Sie unter [Azure Container Registry – Rollen und Berechtigungen](container-registry-roles.md).

Informationen über CLI-Skripts zum Erstellen eines Dienstprinzipals für die Authentifizierung mit einer Azure-Containerregistrierung und weitere Anweisungen finden Sie unter [Azure Container Registry-Authentifizierung mit Dienstprinzipalen](container-registry-auth-service-principal.md).

## <a name="admin-account"></a>Administratorkonto

Jede Containerregistrierung enthält ein Administratorbenutzerkonto, das standardmäßig deaktiviert ist. Sie können den Administratorbenutzer aktivieren und seine Anmeldeinformationen im Azure-Portal oder mithilfe der Azure CLI oder anderer Azure-Tools verwalten.

> [!IMPORTANT]
> Das Administratorkonto ist dafür ausgelegt, dass ein einzelner Benutzer auf die Registrierung zugreift (hauptsächlich für Testzwecke). Sie sollten die Administratorkonto-Anmeldeinformationen nicht für mehrere Benutzer freigeben. Alle Benutzer, die sich mit dem Administratorkonto authentifizieren, werden als ein einzelner Benutzer mit Push- und Pullzugriff auf die Registrierung angezeigt. Wenn dieses Konto geändert oder deaktiviert wird, wird der Zugriff auf die Registrierung für alle Benutzer deaktiviert, die dessen Anmeldeinformationen verwenden. Für Benutzer und Dienstprinzipale wird für monitorlose Szenarien einzelne Identität empfohlen.
>

Das Administratorkonto erhält zwei Kennwörter, die beide erneut generiert werden können. Die beiden Kennwörter ermöglichen Ihnen, Verbindungen mit der Registrierung aufrechtzuerhalten, indem Sie ein Kennwort verwenden, während Sie das andere Kennwort neu generieren. Wenn das Administratorkonto aktiviert ist, können Sie den Benutzernamen und eines der Kennwörter bei Aufforderung an den Befehl `docker login` übergeben, um die Standardauthentifizierung für die Registrierung zu erhalten. Beispiel:

```
docker login myregistry.azurecr.io 
```

Best Practices zur Verwaltung von Anmeldeinformationen finden Sie in der Befehlsreferenz [Docker-Anmeldung](https://docs.docker.com/engine/reference/commandline/login/).

Um den Administratorbenutzer für eine vorhandene Registrierung zu aktivieren, können Sie den `--admin-enabled`-Parameter des [az acr update](/cli/azure/acr?view=azure-cli-latest#az-acr-update)-Befehls in der Azure CLI verwenden:

```azurecli
az acr update -n <acrName> --admin-enabled true
```

Sie können den Administratorbenutzer im Azure-Portal aktivieren, indem Sie zu Ihrer Registrierung navigieren, **Zugriffsschlüssel** unter **EINSTELLUNGEN** und dann **Aktivieren** unter **Administratorbenutzer** auswählen.

![Aktivieren der Administratorbenutzeroberfläche im Azure-Portal][auth-portal-01]

## <a name="next-steps"></a>Nächste Schritte

* [Freigeben Ihres ersten Image mit der Azure CLI](container-registry-get-started-azure-cli.md)

<!-- IMAGES -->
[auth-portal-01]: ./media/container-registry-authentication/auth-portal-01.png
