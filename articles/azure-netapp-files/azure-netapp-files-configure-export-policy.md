---
title: Konfigurieren der Exportrichtlinie für ein NFS-Volume – Azure NetApp Files
description: Beschreibt, wie Sie die Exportrichtlinie zum Steuern des Zugriffs auf ein NFS-Volume mit Azure NetApp Files konfigurieren.
services: azure-netapp-files
author: b-juche
ms.author: b-juche
ms.service: azure-netapp-files
ms.workload: storage
ms.topic: conceptual
ms.date: 10/18/2019
ms.openlocfilehash: b96fca3a5627a1c6c96c8db5c1c209a51c5e102a
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 12/31/2019
ms.locfileid: "75551557"
---
# <a name="configure-export-policy-for-an-nfs-volume"></a>Konfigurieren der Exportrichtlinie für ein NFS-Volume

Sie können optional die Exportrichtlinie zum Steuern des Zugriffs auf ein Azure NetApp Files-Volume konfigurieren. Die Azure NetApp Files-Exportrichtlinie unterstützt nur NFS-Volumes.  Sowohl NFSv3 als auch NFSv4 wird unterstützt. 

## <a name="steps"></a>Schritte 

1.  Klicken Sie im Navigationsbereich von Azure NetApp Files auf **Richtlinie exportieren**. 

2.  Geben Sie Informationen für die folgenden Felder an, um eine Exportrichtlinienregel zu erstellen:   
    *  **Index**   
        Geben Sie die Indexnummer für die Regel an.  
        Eine Exportrichtlinie kann bis zu fünf Regeln umfassen. Regeln werden gemäß ihrer Reihenfolge in der Indexnummernliste ausgewertet. Regeln mit einer niedrigeren Indexnummer werden zuerst ausgewertet. So wird beispielsweise die Regel mit der Indexnummer 1 vor der Regel mit der Indexnummer 2 ausgewertet. 

    * **Zulässige Clients**   
        Geben Sie den Wert in einem der folgenden Formate an:  
        * IPv4-Adresse (Beispiel: `10.1.12.24`) 
        * IPv4-Adresse mit einer Subnetzmaske, ausgedrückt als Anzahl von Bits (Beispiel: `10.1.12.10/4`)

    * **zugreifen**  
        Wählen Sie einen der folgenden Berechtigungstypen aus:  
        * Kein Zugriff 
        * Lesen/Schreiben
        * Nur Leseberechtigung

    ![Exportrichtlinie](../media/azure-netapp-files/azure-netapp-files-export-policy.png) 


## <a name="next-steps"></a>Nächste Schritte 
* [Verwalten von Volumes](azure-netapp-files-manage-volumes.md)
* [Bereitstellen oder Aufheben der Bereitstellung eines Volumes für virtuelle Computer](azure-netapp-files-mount-unmount-volumes-for-virtual-machines.md)
* [Verwalten von Momentaufnahmen](azure-netapp-files-manage-snapshots.md)
