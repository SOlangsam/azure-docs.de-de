---
title: Initiieren eines Speicherkontofailovers (Vorschau) – Azure Storage
description: Erfahren Sie, wie Sie ein Kontofailover für den Fall initiieren, dass der primäre Endpunkt für das Speicherkonto nicht mehr verfügbar ist. Bei dem Failover wird die sekundäre Region so aktualisiert, dass sie zur primären Region für das Speicherkonto wird.
services: storage
author: tamram
ms.service: storage
ms.topic: article
ms.date: 02/11/2019
ms.author: tamram
ms.reviewer: cbrooks
ms.subservice: common
ms.openlocfilehash: 76e34736238273f2af3fccae0ac2b5ed0ff491f0
ms.sourcegitcommit: f97d3d1faf56fb80e5f901cd82c02189f95b3486
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/11/2020
ms.locfileid: "79128333"
---
# <a name="initiate-a-storage-account-failover-preview"></a>Initiieren eines Speicherkontofailovers (Vorschau)

Wenn der primäre Endpunkt für das georedundante Speicherkonto aus einem bestimmten Grund nicht mehr verfügbar ist, können Sie ein Kontofailover initiieren (Vorschauversion). Bei einem Kontofailover wird der sekundäre Endpunkt so aktualisiert, dass er zum primären Endpunkt für das Speicherkonto wird. Nach Abschluss des Failovers können Clients in die neue primäre Region schreiben. Durch ein erzwungenes Failover können Sie die Hochverfügbarkeit für Ihre Anwendungen aufrechterhalten.

In diesem Artikel wird beschrieben, wie Sie über das Azure-Portal, PowerShell oder die Azure-Befehlszeilenschnittstelle ein Kontofailover für Ihr Speicherkonto initiieren. Weitere Informationen zum Kontofailover finden Sie unter [Notfallwiederherstellung und Kontofailover (Vorschau) in Azure Storage](storage-disaster-recovery-guidance.md).

> [!WARNING]
> Ein Kontofailover geht in der Regel mit einem gewissen Datenverlust einher. Informationen zu den Auswirkungen eines Kontofailovers und zur Vorbereitung auf Datenverluste finden Sie unter [Grundlegendes zum Vorgang des Kontofailovers](storage-disaster-recovery-guidance.md#understand-the-account-failover-process).

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Voraussetzungen

Damit Sie ein Kontofailover für Ihr Speicherkonto ausführen können, müssen Sie den folgenden Schritt ausführen:

- Stellen Sie sicher, dass Ihr Speicherkonto zur Verwendung als georedundanter Speicher (GRS) oder georedundanter Speicher mit Lesezugriff (RA-GRS) konfiguriert ist. Weitere Informationen zum georedundanten Speicher finden Sie unter [Azure Storage-Redundanz](storage-redundancy.md).

## <a name="important-implications-of-account-failover"></a>Wichtige Auswirkungen eines Kontofailovers

Wenn Sie ein Kontofailover für Ihr Speicherkonto initiieren, werden die DNS-Einträge für den sekundären Endpunkt so aktualisiert, dass der sekundäre Endpunkt zum primären Endpunkt wird. Vor dem Initiieren eines Failovers sollten Sie daher die möglichen Auswirkungen auf Ihr Speicherkonto verstehen.

Um den Umfang des wahrscheinlichen Datenverlusts schon vor dem Initiieren eines Failovers abschätzen zu können, sollten Sie mithilfe des PowerShell-Cmdlets `Get-AzStorageAccount` die Eigenschaft **Letzte Synchronisierungszeit** überprüfen und den Parameter `-IncludeGeoReplicationStats` einfügen. Überprüfen Sie dann die `GeoReplicationStats`-Eigenschaft für Ihr Konto. \

Nach dem Failover wird der Speicherkontotyp automatisch in einen lokal redundanten Speicher (LRS) in der neuen primären Region konvertiert. Sie können den georedundanten Speicher (GRS) oder den georedundanten Speicher mit Lesezugriff (RA-GRS) wieder aktivieren. Beachten Sie, dass für die Konvertierung von LRS in GRS oder RA-GRS zusätzliche Kosten anfallen. Weitere Informationen finden Sie unter [Preisübersicht Bandbreite](https://azure.microsoft.com/pricing/details/bandwidth/).

Nachdem Sie wieder GRS für Ihr Speicherkonto aktiviert haben, beginnt Microsoft, die Daten in Ihrem Konto in die neue sekundäre Region zu replizieren. Die Dauer der Replikation hängt von der Menge der zu replizierenden Daten ab.  

## <a name="portal"></a>[Portal](#tab/azure-portal)

Führen Sie die folgenden Schritte aus, um ein Kontofailover im Azure-Portal zu initiieren:

1. Navigieren Sie zum Speicherkonto.
2. Wählen Sie unter **Einstellungen** die Option **Georeplikation** aus. In der folgenden Abbildung sind die Georeplikation und der Failoverstatus eines Speicherkontos dargestellt.

    ![Screenshot: Georeplikation und Failoverstatus](media/storage-initiate-account-failover/portal-failover-prepare.png)

3. Überprüfen Sie, ob Ihr Speicherkonto als georedundanter Speicher (GRS) oder georedundanter Speicher mit Lesezugriff (RA-GRS) konfiguriert ist. Wenn dies nicht der Fall ist, wählen Sie unter **Einstellungen** die Option **Konfiguration** aus, um das Konto so zu ändern, dass es georedundant ist. 
4. Die Eigenschaft **Letzte Synchronisierungszeit** gibt an, wie weit der sekundäre Endpunkt hinter dem primären Endpunkt zurückliegt. Über **Letzte Synchronisierungszeit** lässt sich der Umfang des Datenverlusts nach Abschluss des Failovers abschätzen.
5. Wählen Sie **Auf Failover vorbereiten (Vorschau)** aus. 
6. Lesen Sie die Informationen im Bestätigungsdialogfeld. Geben Sie anschließend **Ja** ein, um das Failover zu bestätigen und zu initiieren.

    ![Screenshot: Bestätigungsdialogfeld für ein Kontofailover](media/storage-initiate-account-failover/portal-failover-confirm.png)

## <a name="powershell"></a>[PowerShell](#tab/azure-powershell)

Zur Verwendung von PowerShell zum Initiieren eines Kontofailovers müssen Sie zunächst das 6.0.1-Vorschaumodul installieren. Führen Sie dazu folgende Schritte aus:

1. Deinstallieren Sie alle älteren Installationen von Azure PowerShell:

    - Entfernen Sie alle früheren Installationen von Azure PowerShell mit der Einstellung **Apps & Features** (unter **Einstellungen**) aus Windows.
    - Entfernen Sie alle **Azure**-Module aus `%Program Files%\WindowsPowerShell\Modules`.

1. Vergewissern Sie sich, dass die aktuelle Version von PowerShellGet installiert ist. Öffnen Sie ein Windows PowerShell-Fenster, und führen Sie den folgenden Befehl aus, um die neueste Version zu installieren:

    ```powershell
    Install-Module PowerShellGet –Repository PSGallery –Force
    ```

1. Schließen Sie nach dem Installieren von PowerShellGet das PowerShell-Fenster, und öffnen Sie es dann erneut. 

1. Installieren Sie die neueste Version von Azure PowerShell:

    ```powershell
    Install-Module Az –Repository PSGallery –AllowClobber
    ```

1. Installieren ein Azure Storage-Vorschaumodul, das Kontofailover unterstützt:

    ```powershell
    Install-Module Az.Storage –Repository PSGallery -RequiredVersion 1.1.1-preview –AllowPrerelease –AllowClobber –Force 
    ```

1. Schließen Sie das PowerShell-Fenster, und öffnen Sie es dann erneut.
 
Führen Sie den folgenden Befehl aus, um ein Kontofailover über PowerShell zu initiieren:

```powershell
Invoke-AzStorageAccountFailover -ResourceGroupName <resource-group-name> -Name <account-name> 
```

## <a name="azure-cli"></a>[Azure-Befehlszeilenschnittstelle](#tab/azure-cli)

Führen Sie die folgenden Befehle aus, um ein Kontofailover über die Azure-Befehlszeilenschnittstelle zu initiieren:

```cli
az storage account show \ --name accountName \ --expand geoReplicationStats
az storage account failover \ --name accountName
```

---

## <a name="next-steps"></a>Nächste Schritte

- [Notfallwiederherstellung und Kontofailover (Vorschau) in Azure Storage](storage-disaster-recovery-guidance.md)
- [Entwerfen hochverfügbarer Anwendungen mithilfe von RA-GRS](storage-designing-ha-apps-with-ragrs.md)
- [Tutorial: Erstellen einer hochverfügbaren Anwendung mit Blob Storage](../blobs/storage-create-geo-redundant-storage.md) 
