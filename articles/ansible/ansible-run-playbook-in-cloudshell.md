---
title: 'Schnellstart: Ausführen von Ansible-Playbooks per Bash in Azure Cloud Shell'
description: In dieser Schnellstartanleitung wird beschrieben, wie Sie verschiedene Ansible-Aufgaben mit Bash in Azure Cloud Shell durchführen.
keywords: Ansible, Azure, DevOps, Bash, CloudShell, Playbook, Bash
ms.topic: quickstart
ms.date: 04/30/2019
ms.openlocfilehash: 2a938179cf2e07a61749042db32ef9e1c9d843ba
ms.sourcegitcommit: e4c33439642cf05682af7f28db1dbdb5cf273cc6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/03/2020
ms.locfileid: "78247894"
---
# <a name="quickstart-run-ansible-playbooks-via-bash-in-azure-cloud-shell"></a>Schnellstart: Ausführen von Ansible-Playbooks per Bash in Azure Cloud Shell

Azure Cloud Shell ist eine interaktive, über den Browser zugängliche Shell für die Verwaltung von Azure-Ressourcen. Mit Cloud Shell können Sie entweder eine Bash- oder PowerShell-Befehlszeile verwenden. In diesem Artikel nutzen Sie Bash in Azure Cloud Shell, um ein Ansible-Playbook auszuführen.

## <a name="prerequisites"></a>Voraussetzungen

[!INCLUDE [open-source-devops-prereqs-azure-subscription.md](../../includes/open-source-devops-prereqs-azure-subscription.md)]
- **Konfigurieren von Azure Cloud Shell**: Wenn Sie mit Azure Cloud Shell noch nicht vertraut sind, helfen Ihnen die Informationen unter [Schnellstart für Bash in Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart) weiter.
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Automatische Konfiguration von Anmeldeinformationen

Wenn Sie bei Cloud Shell angemeldet sind, führt Ansible eine Authentifizierung bei Azure durch, um die Infrastruktur ohne zusätzliche Konfiguration verwalten zu können. 

Geben Sie bei Verwendung von mehreren Abonnements das von Ansible genutzte Abonnement an, indem Sie die Umgebungsvariable `AZURE_SUBSCRIPTION_ID` exportieren. 

Zum Auflisten all Ihrer Azure-Abonnements führen Sie den folgenden Befehl aus:

```azurecli-interactive
az account list
```

Legen Sie die `AZURE_SUBSCRIPTION_ID` wie folgt fest, indem Sie Ihre Azure-Abonnement-ID verwenden:

```console
export AZURE_SUBSCRIPTION_ID=<your-subscription-id>
```

## <a name="verify-the-configuration"></a>Überprüfen der Konfiguration
Erstellen Sie mit Ansible eine Azure-Ressourcengruppe, um zu überprüfen, ob die Konfiguration erfolgreich war.

[!INCLUDE [create-resource-group-with-ansible.md](../../includes/ansible-snippet-create-resource-group.md)]

## <a name="next-steps"></a>Nächste Schritte

> [!div class="nextstepaction"] 
> [Schnellstart: Konfigurieren eines virtuellen Computers in Azure mit Ansible](./ansible-create-vm.md)