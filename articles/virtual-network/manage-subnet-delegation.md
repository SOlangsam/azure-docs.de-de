---
title: Hinzufügen oder Entfernen einer Subnetzdelegierung in einem virtuellen Azure-Netzwerk
titlesuffix: Azure Virtual Network
description: Erfahren Sie, wie Sie ein delegiertes Subnetz für einen Dienst in Azure hinzufügen oder entfernen.
services: virtual-network
documentationcenter: na
author: KumudD
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/06/2019
ms.author: kumud
ms.openlocfilehash: 6f767abdf8673e3adffc6c4e3748733054ba723d
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201865"
---
# <a name="add-or-remove-a-subnet-delegation"></a>Hinzufügen oder Entfernen einer Subnetzdelegierung

Bei der Subnetzdelegierung erhält der Dienst explizite Berechtigungen, um dienstspezifische Ressourcen im Subnetz zu erstellen, indem beim Bereitstellen des Diensts ein eindeutiger Bezeichner verwendet wird. Dieser Artikel beschreibt das Hinzufügen oder Entfernen eines delegierten Subnetzes für einen Azure-Dienst.

## <a name="portal"></a>Portal

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Melden Sie sich unter https://portal.azure.com beim Azure-Portal an.

### <a name="create-the-virtual-network"></a>Erstellen des virtuellen Netzwerks

In diesem Abschnitt erstellen Sie ein virtuelles Netzwerk und das Subnetz, das Sie später an einen Azure-Dienst delegieren.

1. Wählen Sie oben links auf dem Bildschirm **Ressource erstellen** > **Netzwerk** > **Virtuelles Netzwerk** aus.
1. Geben Sie in **Virtuelles Netzwerk erstellen** diese Informationen ein, oder wählen Sie sie aus:

    | Einstellung | value |
    | ------- | ----- |
    | Name | Geben Sie *MyVirtualNetwork* ein. |
    | Adressraum | Geben Sie *10.0.0.0/16* ein. |
    | Subscription | Wählen Sie Ihr Abonnement aus.|
    | Resource group | Wählen Sie **Neue erstellen** aus, geben Sie *myResourceGroup* ein, und wählen Sie **OK** aus. |
    | Location | Wählen Sie **EastUS** aus.|
    | Subnetzname | Geben Sie *mySubnet* ein. |
    | Subnetzadressbereich | Geben Sie *10.0.0.0/24* ein. |
    |||
1. Übernehmen Sie die übrigen Standardeinstellungen, und wählen Sie dann **Erstellen** aus.

### <a name="permissions"></a>Berechtigungen

Wenn Sie das Subnetz, das Sie an den Azure-Dienst delegieren möchten, nicht persönlich erstellt haben, benötigen Sie die folgende Berechtigung: `Microsoft.Network/virtualNetworks/subnets/write`.

Die integrierte Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) enthält ebenfalls die erforderlichen Berechtigungen.

### <a name="delegate-a-subnet-to-an-azure-service"></a>Delegieren eines Subnetzes an einen Azure-Dienst

In diesem Abschnitt delegieren Sie das Subnetz, das Sie im vorhergehenden Abschnitt erstellt haben, an einen Azure-Dienst.

1. Geben Sie in der Suchleiste des Portals *myVirtualNetwork* ein. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie in den Suchergebnissen *myVirtualNetwork* aus.
3. Wählen Sie unter **EINSTELLUNGEN** **Subnetze** und dann **mySubnet** aus.
4. Treffen Sie auf der Seite *mySubnet* für die LIste **Subnetzdelegierung** eine Auswahl unter den Diensten, die unter **Subnetz an einen Dienst delegieren** aufgelistet sind (beispielsweise **Microsoft.DBforPostgreSQL/serversv2**).  

### <a name="remove-subnet-delegation-from-an-azure-service"></a>Entfernen einer Subnetzdelegierung aus einem Azure-Dienst

1. Geben Sie in der Suchleiste des Portals *myVirtualNetwork* ein. Wenn **myVirtualNetwork** in den Suchergebnissen angezeigt wird, können Sie den Begriff auswählen.
2. Wählen Sie in den Suchergebnissen *myVirtualNetwork* aus.
3. Wählen Sie unter **EINSTELLUNGEN** **Subnetze** und dann **mySubnet** aus.
4. Wählen Sie auf der Seite *mySubnet* für die Liste **Subnetzdelegierung** unter den Diensten, die unter **Subnetz an einen Dienst delegieren** **Keins** aus. 

## <a name="azure-cli"></a>Azure-Befehlszeilenschnittstelle

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Wenn Sie sich stattdessen entscheiden, die Azure CLI lokal zu installieren und zu verwenden, müssen Sie für diesen Artikel zumindest die Azure CLI-Version 2.0.28 verwenden. Führen Sie `az --version` aus, um die installierte Version zu ermitteln. Informationen zum Ausführen einer Installation oder eines Upgrades finden Sie unter [Installieren der Azure CLI](/cli/azure/install-azure-cli).

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit [az group create](https://docs.microsoft.com/cli/azure/group) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen **myResourceGroup** am Standort **eastus** erstellt:

```azurecli-interactive

  az group create \
    --name myResourceGroup \
    --location eastus

```

### <a name="create-a-virtual-network"></a>Erstellen eines virtuellen Netzwerks
Erstellen Sie ein virtuelles Netzwerk mit dem Namen **myVnet** und dem Subnetz **mySubnet** in **myResourceGroup**, indem Sie den Befehl [az network vnet create](https://docs.microsoft.com/cli/azure/network/vnet) verwenden.

```azurecli-interactive
  az network vnet create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myVnet \
    --address-prefix 10.0.0.0/16 \
    --subnet-name mySubnet \
    --subnet-prefix 10.0.0.0/24
```
### <a name="permissions"></a>Berechtigungen

Wenn Sie das Subnetz, das Sie an den Azure-Dienst delegieren möchten, nicht persönlich erstellt haben, benötigen Sie die folgende Berechtigung: `Microsoft.Network/virtualNetworks/subnets/write`.

Die integrierte Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) enthält ebenfalls die erforderlichen Berechtigungen.

### <a name="delegate-a-subnet-to-an-azure-service"></a>Delegieren eines Subnetzes an einen Azure-Dienst

In diesem Abschnitt delegieren Sie das Subnetz, das Sie im vorhergehenden Abschnitt erstellt haben, an einen Azure-Dienst. 

Verwenden Sie [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update), um das Subnetz namens **mySubnet** mit Delegierung an einen Azure-Dienst zu aktualisieren.  In diesem Beispiel wird **Microsoft.DBforPostgreSQL/serversv2** für die exemplarische Delegierung verwendet:

```azurecli-interactive
  az network vnet subnet update \
  --resource-group myResourceGroup \
  --name mySubnet \
  --vnet-name myVnet \
  --delegations Microsoft.DBforPostgreSQL/serversv2
```

Um zu überprüfen, ob die Delegierung angewendet wurde, verwenden Sie [az network vnet subnet show](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-show). Überprüfen Sie unter der Eigenschaft **serviceName**, ob der Dienst an das Subnetz delegiert wurde:

```azurecli-interactive
  az network vnet subnet show \
  --resource-group myResourceGroup \
  --name mySubnet \
  --vnet-name myVnet \
  --query delegations
```

```json
[
  {
    "actions": [
      "Microsoft.Network/virtualNetworks/subnets/join/action"
    ],
    "etag": "W/\"8a8bf16a-38cf-409f-9434-fe3b5ab9ae54\"",
    "id": "/subscriptions/3bf09329-ca61-4fee-88cb-7e30b9ee305b/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet/delegations/0",
    "name": "0",
    "provisioningState": "Succeeded",
    "resourceGroup": "myResourceGroup",
    "serviceName": "Microsoft.DBforPostgreSQL/serversv2",
    "type": "Microsoft.Network/virtualNetworks/subnets/delegations"
  }
]
```

### <a name="remove-subnet-delegation-from-an-azure-service"></a>Entfernen einer Subnetzdelegierung aus einem Azure-Dienst

Verwenden Sie [az network vnet subnet update](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-update), um die Delegierung aus dem Subnetz **mySubnet** zu entfernen:

```azurecli-interactive
  az network vnet subnet update \
  --resource-group myResourceGroup \
  --name mySubnet \
  --vnet-name myVnet \
  --remove delegations
```
Um zu überprüfen, ob die Delegierung entfernt wurde, verwenden Sie [az network vnet subnet show](https://docs.microsoft.com/cli/azure/network/vnet/subnet?view=azure-cli-latest#az-network-vnet-subnet-show). Überprüfen Sie unter der Eigenschaft **serviceName**, ob der Dienst aus dem Subnetz entfernt wurde:

```azurecli-interactive
  az network vnet subnet show \
  --resource-group myResourceGroup \
  --name mySubnet \
  --vnet-name myVnet \
  --query delegations
```
Die Ausgabe des Befehls ist ein leeres eckiges Klammerpaar:
```json
[]
```

## <a name="azure-powershell"></a>Azure PowerShell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

```azurepowershell-interactive
  Connect-AzAccount
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe
Erstellen Sie mit [New-AzResourceGroup](https://docs.microsoft.com/cli/azure/group) eine Ressourcengruppe. Eine Azure-Ressourcengruppe ist ein logischer Container, in dem Azure-Ressourcen bereitgestellt und verwaltet werden.

Im folgenden Beispiel wird eine Ressourcengruppe mit dem Namen *myResourceGroup* am Standort *eastus* erstellt:

```azurepowershell-interactive
  New-AzResourceGroup -Name myResourceGroup -Location eastus
```
### <a name="create-virtual-network"></a>Virtuelles Netzwerk erstellen

Erstellen Sie ein virtuelles Netzwerk namens **myVnet** mit einem Subnetz namens **mySubnet** mit [New-AzVirtualNetworkSubnetConfig](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetworksubnetconfig?view=latest) in **myResourceGroup**, indem Sie [New-AzVirtualNetwork](https://docs.microsoft.com/powershell/module/az.network/new-azvirtualnetwork?view=latest) verwenden. Der IP-Adressraum für das virtuelle Netzwerk ist **10.0.0.0/16**. Das Subnetz im virtuellen Netzwerk ist **10.0.0.0/24**.  

```azurepowershell-interactive
  $subnet = New-AzVirtualNetworkSubnetConfig -Name mySubnet -AddressPrefix "10.0.0.0/24"

  New-AzVirtualNetwork -Name myVnet -ResourceGroupName myResourceGroup -Location eastus -AddressPrefix "10.0.0.0/16" -Subnet $subnet
```
### <a name="permissions"></a>Berechtigungen

Wenn Sie das Subnetz, das Sie an den Azure-Dienst delegieren möchten, nicht persönlich erstellt haben, benötigen Sie die folgende Berechtigung: `Microsoft.Network/virtualNetworks/subnets/write`.

Die integrierte Rolle [Netzwerkmitwirkender](../role-based-access-control/built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor) enthält ebenfalls die erforderlichen Berechtigungen.

### <a name="delegate-a-subnet-to-an-azure-service"></a>Delegieren eines Subnetzes an einen Azure-Dienst

In diesem Abschnitt delegieren Sie das Subnetz, das Sie im vorhergehenden Abschnitt erstellt haben, an einen Azure-Dienst. 

Verwenden Sie [Add-AzDelegation](https://docs.microsoft.com/powershell/module/az.network/add-azdelegation?view=latest), um das Subnetz namens **mySubnet** mit einer Delegierung namens **myDelegation** an einen Azure-Dienst zu aktualisieren.  In diesem Beispiel wird **Microsoft.DBforPostgreSQL/serversv2** für die exemplarische Delegierung verwendet:

```azurepowershell-interactive
  $vnet = Get-AzVirtualNetwork -Name "myVNet" -ResourceGroupName "myResourceGroup"
  $subnet = Get-AzVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $vnet
  $subnet = Add-AzDelegation -Name "myDelegation" -ServiceName "Microsoft.DBforPostgreSQL/serversv2" -Subnet $subnet
  Set-AzVirtualNetwork -VirtualNetwork $vnet
```
Verwenden Sie [Get-AzDelegation](https://docs.microsoft.com/powershell/module/az.network/get-azdelegation?view=latest), um die Delegierung zu überprüfen:

```azurepowershell-interactive
  $subnet = Get-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup" | Get-AzVirtualNetworkSubnetConfig -Name "mySubnet"
  Get-AzDelegation -Name "myDelegation" -Subnet $subnet

  ProvisioningState : Succeeded
  ServiceName       : Microsoft.DBforPostgreSQL/serversv2
  Actions           : {Microsoft.Network/virtualNetworks/subnets/join/action}
  Name              : myDelegation
  Etag              : W/"9cba4b0e-2ceb-444b-b553-454f8da07d8a"
  Id                : /subscriptions/3bf09329-ca61-4fee-88cb-7e30b9ee305b/resourceGroups/myResourceGroup/providers/Microsoft.Network/virtualNetworks/myVnet/subnets/mySubnet/delegations/myDelegation

```
### <a name="remove-subnet-delegation-from-an-azure-service"></a>Entfernen einer Subnetzdelegierung aus einem Azure-Dienst

Verwenden Sie [Remove-AzDelegation](https://docs.microsoft.com/powershell/module/az.network/remove-azdelegation?view=latest), um die Delegierung aus dem Subnetz **mySubnet** zu entfernen:

```azurepowershell-interactive
  $vnet = Get-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
  $subnet = Get-AzVirtualNetworkSubnetConfig -Name "mySubnet" -VirtualNetwork $vnet
  $subnet = Remove-AzDelegation -Name "myDelegation" -Subnet $subnet
  Set-AzVirtualNetwork -VirtualNetwork $vnet
```
Verwenden Sie [Get-AzDelegation](https://docs.microsoft.com/powershell/module/az.network/get-azdelegation?view=latest), um zu überprüfen, ob die Delegierung entfernt wurde:

```azurepowershell-interactive
  $subnet = Get-AzVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup" | Get-AzVirtualNetworkSubnetConfig -Name "mySubnet"
  Get-AzDelegation -Name "myDelegation" -Subnet $subnet

  Get-AzDelegation: Sequence contains no matching element

```

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum [Verwalten von Subnetzen in Azure](virtual-network-manage-subnet.md).
