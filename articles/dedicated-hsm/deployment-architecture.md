---
title: Bereitstellungsarchitektur – Azure-Dienst für dedizierte HSMs | Microsoft-Dokumentation
description: Grundlegende Entwurfsüberlegungen zur Verwendung des Azure-Dienst für dedizierte HSMs als Teil einer Anwendungsarchitektur
services: dedicated-hsm
author: msmbaldwin
manager: rkarlin
ms.custom: mvc, seodec18
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/05/2020
ms.author: mbaldwin
ms.openlocfilehash: 89e3bf95a6b048e5e97cfb151ef9302b70eac1c9
ms.sourcegitcommit: 57669c5ae1abdb6bac3b1e816ea822e3dbf5b3e1
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/06/2020
ms.locfileid: "77048565"
---
# <a name="azure-dedicated-hsm-deployment-architecture"></a>Azure Dedicated HSM-Bereitstellungsarchitektur

Azure Dedicated HSM bietet Speicherung von kryptografischen Schlüsseln in Azure. Azure Dedicated HSM erfüllt strenge Sicherheitsanforderungen. Kunden profitieren von der Verwendung von Azure Dedicated HSM, wenn sie:

* Die FIPS 140-2 Level 3-Zertifizierung erfüllen müssen.
* Exklusiven Zugriff auf das HSM benötigen.
* Vollständige Kontrolle über ihre Geräte haben müssen.

Die HSMs sind auf die Datencenter von Microsoft verteilt und können problemlos als Gerätepaar als Basis für eine hochverfügbare Lösung bereitgestellt werden. Sie können auch regionsübergreifend bereitgestellt werden, um eine notfallresistente Lösung zu bieten. Die Regionen, in denen Dedicated HSM zurzeit verfügbar ist, sind:

* East US
* USA (Ost) 2
* USA (Westen)
* USA Süd Mitte
* Asien, Südosten
* Asien, Osten
* Indien, Mitte
* Indien, Süden
* Japan, Osten
* Japan, Westen
* Nordeuropa
* Europa, Westen
* UK, Süden
* UK, Westen
* Kanada, Mitte
* Kanada, Osten
* Australien (Osten)
* Australien, Südosten

Jede dieser Regionen verfügt über HSM-Racks, die entweder in zwei unabhängigen Datencentern oder in mindestens zwei unabhängigen Verfügbarkeitszonen bereitgestellt werden. „Asien, Südosten“ verfügt über drei Verfügbarkeitszonen und „USA, Osten 2“ über zwei. Es gibt insgesamt acht Regionen in Europa, Asien und den USA, die den Dedicated HSM-Dienst zur Verfügung stellen. Weitere Informationen zu Azure-Regionen finden Sie in den offiziellen [Informationen zu Azure-Regionen](https://azure.microsoft.com/global-infrastructure/regions/).
Einige Entwurfsfaktoren für jede Dedicated HSM-basierte Lösung sind Standort/Latenz, Hochverfügbarkeit und Unterstützung für andere verteilte Anwendungen.

## <a name="device-location"></a>Gerätestandort

Der optimale Standort der HSM-Geräte befindet sich in unmittelbarer Nähe zu den Anwendungen, die kryptographische Vorgänge ausführen. Die Latenzzeit innerhalb der Region sollte im einstelligen Millisekundenbereich liegen. Regionsübergreifende Latenz kann 5 bis 10 Mal höher als dieser Wert sein.

## <a name="high-availability"></a>Hochverfügbarkeit

Um Hochverfügbarkeit zu erreichen, muss ein Kunde zwei HSM-Geräte in einer Region verwenden, die mit der Gemalto-Software als Hochverfügbarkeitspaar konfiguriert werden. Diese Art der Bereitstellung stellt die Verfügbarkeit von Schlüsseln sicher, wenn ein einzelnes Gerät auf ein Problem stößt, das es daran hindert, wichtige Vorgänge zu verarbeiten. Sie verringert auch das Risiko bei der Durchführung von Wartungsarbeiten (z.B. dem Austausch der Stromversorgung) erheblich. Es ist wichtig, dass ein Entwurf jede Art von Versagen auf regionaler Ebene berücksichtigt. Ausfälle auf regionaler Ebene können bei Naturkatastrophen wie Orkanen, Überschwemmungen oder Erdbeben auftreten. Diese Art von Ereignissen sollte durch die Bereitstellung von HSM-Geräten in einer anderen Region abgemildert werden. Geräte, die in einer anderen Region bereitgestellt werden, können über die Gemalto-Softwarekonfiguration gekoppelt werden. Dies bedeutet, dass die Mindestbereitstellung für eine hochverfügbare und notfallresistente Lösung aus vier HSM-Geräten in zwei Regionen besteht. Lokale Redundanz und Redundanz über Regionen hinweg können als Grundlage für weitere HSM-Gerätebereitstellungen verwendet werden, um Latenzzeiten und Kapazitäten zu unterstützen oder andere anwendungsspezifische Anforderungen zu erfüllen.

## <a name="distributed-application-support"></a>Unterstützung verteilter Anwendungen

Dedicated HSM-Geräte werden typischerweise zur Unterstützung von Anwendungen bereitgestellt, die Schlüsselspeicher- und Schlüsselabrufvorgänge durchführen müssen. Dedicated HSM-Geräte verfügen über 10 Partitionen für die Unterstützung unabhängiger Anwendungen. Der Gerätestandort sollte auf einer ganzheitlichen Sichtweise aller Anwendungen basieren, die den Dienst nutzen müssen.

## <a name="next-steps"></a>Nächste Schritte

Sobald die Bereitstellungsarchitektur festgelegt wurde, werden die meisten Konfigurationsaktivitäten zur Implementierung dieser Architektur von Gemalto bereitgestellt. Dazu gehören sowohl die Gerätekonfiguration als auch Anwendungsintegrationsszenarien. Weitere Informationen finden Sie im [Gemalto-Kundensupportportal ](https://supportportal.gemalto.com/csm/). Sie können außerdem die Administrations- und Konfigurationsleitfäden herunterladen. Die Microsoft-Partnerwebsite verfügt über eine Vielzahl von Integrationsleitfäden.
Es wird empfohlen, sich mit allen Schlüsselkonzepten des Diensts (z.B. Hochverfügbarkeit und Sicherheit) vor der Gerätebereitstellung oder dem Entwurf und der Bereitstellung von Anwendungen vertraut zu machen.
Weitere Themen auf Konzeptebene:

* [Hochverfügbarkeit](high-availability.md)
* [Physische Sicherheit](physical-security.md)
* [Netzwerk](networking.md)
* [Unterstützungsmöglichkeiten](supportability.md)
* [Überwachung](monitoring.md)
