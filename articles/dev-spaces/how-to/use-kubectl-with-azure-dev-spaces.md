---
title: Verwenden von kubectl mit Azure Dev Spaces
services: azure-dev-spaces
ms.date: 05/11/2018
ms.topic: conceptual
description: Erfahren Sie, wie Sie in einem Entwicklungsbereich kubectl-Befehle in einem Azure Kubernetes Service-Cluster verwenden, wenn Azure Dev Spaces aktiviert ist.
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, Container, Helm, Service Mesh, Service Mesh-Routing, kubectl, k8s '
ms.openlocfilehash: 7530cde68b2ce8d06fb7b16a9a147f0f0bc8645c
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/25/2019
ms.locfileid: "75438361"
---
# <a name="use-kubectl-with-an-azure-dev-space"></a>Verwenden von kubectl mit einem Azure Dev Space

Sie können auf den Kubernetes-Cluster in einem Azure Dev Space zugreifen und vorhandene Kubernetes-Tools wie `kubectl` verwenden.

Beim Ausführen des Befehls `az aks use-dev-spaces` wird automatisch ein `kubectl`-Konfigurationskontext hinzugefügt, sodass kubectl bereits mit Ihrem Azure Dev Spaces-Kubernetes-Cluster verbunden sein sollte. Beispiele:
- Bestätigen des aktuellen Kontexts: `kubectl config current-context`
- Auflisten aller verfügbaren Kontexte: `kubectl config get-contexts` 
- Ändern des Kontexts: `kubectl config use-context <context-name>`
- Anzeigen des Kubernetes-Dashboards: Führen Sie `kubectl proxy` aus, und öffnen Sie dann in Ihrem Browser die Adresse, die dieser Befehl ausgibt (fügen Sie `/ui` an die URL an, um zum Kubernetes-Dashboard zu navigieren).
- Auflisten der ausgeführten Dienste im Azure Dev Spaces-Standardbereich namens *default*: `kubectl get services --namespace=default`

