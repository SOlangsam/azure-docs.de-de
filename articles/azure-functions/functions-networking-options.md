---
title: Netzwerkoptionen von Azure Functions
description: Enthält eine Übersicht über alle Netzwerkoptionen, die in Azure Functions verfügbar sind.
author: alexkarcher-msft
ms.topic: conceptual
ms.date: 4/11/2019
ms.author: alkarche
ms.openlocfilehash: 7b47e7b0672716141f62e3f7df4b0d3ed95c663d
ms.sourcegitcommit: d12880206cf9926af6aaf3bfafda1bc5b0ec7151
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114298"
---
# <a name="azure-functions-networking-options"></a>Netzwerkoptionen von Azure Functions

In diesem Artikel werden die Netzwerkfunktionen beschrieben, die für die Hostingoptionen von Azure Functions zur Verfügung stehen. Alle folgenden Netzwerkoptionen geben Ihnen die Möglichkeit zum Zugreifen auf Ressourcen ohne Verwendung von Adressen, die über das Internet geroutet werden können, oder die Einschränkung des Internetzugriffs auf eine Funktions-App.

Die Hostingmodelle weisen verschiedene verfügbare Ebenen der Netzwerkisolation auf. Durch Auswahl der jeweils richtigen Ebene lassen sich Ihre Anforderungen an die Netzwerkisolation erfüllen.

Sie können Funktions-Apps auf verschiedene Arten hosten:

* Es gibt eine Gruppe von Tarifoptionen, die in einer mehrinstanzenfähigen Infrastruktur ausgeführt werden und über unterschiedliche Ebenen von Konnektivitäts- und Skalierungsoptionen für virtuelle Netzwerke verfügen:
    * Der [Verbrauchstarif](functions-scale.md#consumption-plan), der als Reaktion auf Last dynamisch skaliert wird und nur über Optionen für eine minimale Netzwerkisolation verfügt.
    * Der [Premium-Tarif](functions-scale.md#premium-plan), bei dem ebenfalls dynamisch skaliert wird, der aber eine umfassendere Netzwerkisolation ermöglicht.
    * Der [Azure App Service-Plan](functions-scale.md#app-service-plan), bei dem eine feste Skalierung verwendet wird und der eine ähnliche Netzwerkisolation wie der Premium-Tarif aufweist.
* Sie können Funktionen in einer [App Service-Umgebung](../app-service/environment/intro.md) ausführen. Mit dieser Methode werden die Funktionen in Ihrem virtuellen Netzwerk bereitgestellt und eine umfassende Netzwerksteuerung und -isolation ermöglicht.

## <a name="matrix-of-networking-features"></a>Matrix der Netzwerkfunktionen

|                |[Verbrauchstarif](functions-scale.md#consumption-plan)|[Premium-Plan](functions-scale.md#premium-plan)|[App Service-Plan](functions-scale.md#app-service-plan)|[App Service-Umgebung](../app-service/environment/intro.md)|
|----------------|-----------|----------------|---------|-----------------------|  
|[IP-Einschränkungen für eingehenden Datenverkehr und Zugriff auf private Sites](#inbound-ip-restrictions)|✅Ja|✅Ja|✅Ja|✅Ja|
|[Integration in ein virtuelles Netzwerk](#virtual-network-integration)|❌Nein|✅Ja (regional)|✅Ja (regional und Gateway)|✅Ja|
|[Trigger für virtuelle Netzwerke (nicht HTTP)](#virtual-network-triggers-non-http)|❌Nein| ✅Ja |✅Ja|✅Ja|
|[Hybridverbindungen](#hybrid-connections) (nur Windows)|❌Nein|✅Ja|✅Ja|✅Ja|
|[IP-Einschränkungen für ausgehenden Datenverkehr](#outbound-ip-restrictions)|❌Nein| ❌Nein|❌Nein|✅Ja|

## <a name="inbound-ip-restrictions"></a>IP-Einschränkungen für eingehenden Datenverkehr

Mit IP-Einschränkungen können Sie eine nach Priorität sortierte Liste mit IP-Adressen definieren, über die der Zugriff auf Ihre App zugelassen bzw. abgelehnt wird. Die Liste kann IPv4- und IPv6-Adressen enthalten. Wenn mindestens ein Eintrag vorhanden ist, enthält die Liste am Ende einen impliziten Eintrag vom Typ „Alle ablehnen“. IP-Einschränkungen können für alle Hostingoptionen für Funktionen verwendet werden.

> [!NOTE]
> Mit den geltenden Netzwerkbeschränkungen können Sie den Portal-Editor nur in Ihrem virtuellen Netzwerk verwenden, oder wenn Sie die IP-Adresse des Computers, mit dem Sie auf das Azure-Portal zugreifen, der Liste der sicheren Empfänger hinzugefügt haben. Sie können aber weiterhin von jedem Computer aus auf alle Funktionen der Registerkarte **Plattformfeatures** zugreifen.

Weitere Informationen finden Sie unter [Azure App Service – statische Zugriffseinschränkungen](../app-service/app-service-ip-restrictions.md).

## <a name="private-site-access"></a>Privater Websitezugriff

Privater Websitezugriff bezieht sich darauf, den Zugriff auf Ihre App nur über ein privates Netzwerk zuzulassen, z.B. über ein virtuelles Azure-Netzwerk.

* Zugriff auf private Sites ist im [Premium-Tarif](./functions-premium-plan.md), [Verbrauchstarif](functions-scale.md#consumption-plan) und [App Service-Plan](functions-scale.md#app-service-plan) verfügbar, wenn Dienstendpunkte konfiguriert sind.
    * Dienstendpunkte können unter **Plattformfeatures** > **Netzwerk** > **Zugriffseinschränkungen konfigurieren** > **Regel hinzufügen** pro App konfiguriert werden. Virtuelle Netzwerke können nun als Regeltyp ausgewählt werden.
    * Weitere Informationen finden Sie unter [Dienstendpunkte im virtuellen Netzwerk](../virtual-network/virtual-network-service-endpoints-overview.md).
    * Bedenken Sie, dass Ihre Funktion auch mit Dienstendpunkten immer noch vollständigen ausgehenden Zugriff auf das Internet besitzt, selbst wenn die virtuelle Netzwerkintegration konfiguriert ist.
* Der private Websitezugriff ist auch verfügbar, wenn eine App Service-Umgebung mit internen Lastenausgleich (ILB) konfiguriert ist. Weitere Informationen finden Sie unter [Erstellen und Verwenden eines internen Lastenausgleichs mit einer App Service-Umgebung](../app-service/environment/create-ilb-ase.md).

## <a name="virtual-network-integration"></a>Integration in ein virtuelles Netzwerk

Durch die Integration des virtuellen Netzwerks kann die Funktions-App auf Ressourcen eines virtuellen Netzwerks zugreifen. Diese Funktion ist sowohl für den Premium-Tarif als auch für den App Service-Plan verfügbar. In einer App Service-Umgebung befindet sich Ihre App bereits in einem virtuellen Netzwerk, sodass die Integration des virtuellen Netzwerks nicht erforderlich ist, um Ressourcen im selben virtuellen Netzwerk zu erreichen.

Mithilfe der Integration des virtuellen Netzwerks kann der Zugriff von Apps auf Datenbanken und Webdienste im virtuellen Netzwerk ermöglicht werden. Mit der Integration des virtuellen Netzwerks müssen Sie keinen öffentlichen Endpunkt für Anwendungen auf Ihrem virtuellen Computer verfügbar machen. Stattdessen können Sie private Adressen verwenden, die nicht über das Internet geroutet werden.

Es gibt zwei Formen der virtuellen Netzwerkintegration:

+ **Regionale Integration des virtuellen Netzwerks (Vorschau)** : Ermöglicht die Integration in virtuelle Netzwerke in der gleichen Region. Dieser Typ der Integration erfordert ein Subnetz in einem virtuellen Netzwerk in der gleichen Region. Diese Funktion befindet sich noch in der Vorschau, wird aber für Funktions-Apps unter Windows mit den Einschränkungen unterstützt, die in der folgenden Problem-/Lösungstabelle beschrieben werden.
+ **Integration des virtuellen Netzwerks mit erforderlichem Gateway**: Ermöglicht die Integration in virtuelle Netzwerke in Remoteregionen oder in klassische virtuelle Netzwerke. Dieser Typ der Integration erfordert die Bereitstellung eines virtuellen Netzwerkgateways in Ihrem VNET. Dies ist eine Funktion, die auf einer Point-to-Site-Verbindung basiert und nur für Funktions-Apps unter Windows unterstützt wird.

Eine App kann jeweils nur einen Typ der Integrationsfunktion für virtuelle Netzwerke verwenden. Beide Typen sind in vielen Szenarien nützlich. Die folgende Tabelle gibt an, unter welchen Umständen der jeweilige Typ verwendet werden sollte:

| Problem  | Lösung |
|----------|----------|
| Sie möchten eine RFC 1918-Adresse (10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16) in der gleichen Region erreichen. | Regionale Integration des virtuellen Netzwerks |
| Sie möchten Ressourcen in einem klassischen virtuellen Netzwerk oder einem virtuellen Netzwerk in einer anderen Region erreichen. | Integration des virtuellen Netzwerks mit erforderlichem Gateway |
| Sie möchten RFC 1918-Endpunkte über Azure ExpressRoute erreichen. | Regionale Integration des virtuellen Netzwerks |
| Sie möchten Ressourcen über Dienstendpunkte erreichen. | Regionale Integration des virtuellen Netzwerks |

Mit keiner dieser Funktionen erreichen Sie RFC 1918-fremde Adressen über ExpressRoute. Zu diesem Zweck müssen Sie zurzeit eine App Service-Umgebung verwenden.

Durch die Integration von regionalen virtuellen Netzwerken wird Ihr virtuelles Netzwerk nicht mit lokalen Endpunkten verbunden, und es werden keine Dienstendpunkte konfiguriert. Dafür ist eine separate Netzwerkkonfiguration erforderlich. Die regionale Integration virtueller Netzwerke ermöglicht es Ihrer App lediglich, Aufrufe über diese Verbindungstypen auszuführen.

Unabhängig von der verwendeten Version ermöglicht die Integration des virtuellen Netzwerks Ihrer Funktions-App den Zugriff auf Ressourcen in Ihrem virtuellen Netzwerk, gewährt aber keinen privaten Websitezugriff auf Ihre Funktions-App aus dem virtuellen Netzwerk. Privater Websitezugriff bedeutet, den Zugriff auf Ihre App nur über ein privates Netzwerk zuzulassen, z.B. über ein virtuelles Azure-Netzwerk. Die Integration des virtuellen Netzwerks wird nur für ausgehende Aufrufe verwendet, die von Ihrer App an Ihr virtuelles Netzwerk gerichtet werden.

Die Funktion Integration eines virtuellen Netzwerks:

* Erfordert einen Standard-, Premium- oder PremiumV2-App Service-Plan.
* Unterstützt TCP und UDP.
* Funktioniert mit App Service-Apps und Funktions-Apps.

Einige Dinge werden von der Integration eines virtuellen Netzwerks nicht unterstützt, z.B.:

* Bereitstellung eines Laufwerks
* Active Directory-Integration
* NetBIOS

Bei der Integration virtueller Netzwerke in Azure Functions wird die gemeinsame Infrastruktur mit App Service-Web-Apps verwendet. Weitere Informationen zu den beiden Arten der Integration virtueller Netzwerke finden Sie unter:

* [Regionale Integration des virtuellen Netzwerks](../app-service/web-sites-integrate-with-vnet.md#regional-vnet-integration)
* [Integration des virtuellen Netzwerks mit erforderlichem Gateway](../app-service/web-sites-integrate-with-vnet.md#gateway-required-vnet-integration)

Weitere Informationen zur Verwendung der Integration virtueller Netzwerke finden Sie unter [Integrieren einer Funktions-App in ein virtuelles Azure-Netzwerk](functions-create-vnet.md).

## <a name="connecting-to-service-endpoint-secured-resources"></a>Herstellen einer Verbindung mit durch den Dienstendpunkt gesicherten Ressourcen

> [!NOTE]
> Derzeit kann es bis zu 12 Stunden dauern, bis neue Dienstendpunkte für Ihre Funktions-App verfügbar sind, nachdem Sie die Zugriffsbeschränkungen für die Downstreamressource konfiguriert haben. Während dieses Zeitraums ist die Ressource für Ihre App vollständig nicht verfügbar.

Um eine höhere Sicherheitsstufe zu gewährleisten, können Sie eine Reihe von Azure-Diensten auf ein virtuelles Netzwerk beschränken, indem Sie Dienstendpunkte verwenden. Sie müssen Ihre Funktions-App dann in dieses virtuelle Netzwerk integrieren, um auf die Ressource zugreifen zu können. Diese Konfiguration wird für alle Pläne unterstützt, die die Integration in ein virtuelles Netzwerk unterstützen.

[Erfahren Sie mehr über Dienstendpunkte virtueller Netzwerke.](../virtual-network/virtual-network-service-endpoints-overview.md)

### <a name="restricting-your-storage-account-to-a-virtual-network"></a>Einschränken Ihres Speicherkontos auf ein virtuelles Netzwerk

Beim Erstellen einer Funktions-App müssen Sie ein allgemeines Azure Storage-Konto erstellen oder verknüpfen, das Blob-, Queue- und Table Storage unterstützt. Für dieses Konto können derzeit keine Einschränkungen für virtuelle Netzwerke verwendet werden. Wenn Sie einen Dienstendpunkt des virtuellen Netzwerks für das Speicherkonto konfigurieren, das Sie für Ihre Funktions-App verwenden, funktioniert die App nicht mehr. Diese Funktionalität ist derzeit bei Verwendung des Premium-Plans und einer Integration in ein virtuelles Netzwerk verfügbar.

[Weitere Informationen zu Speicherkontoanforderungen.](./functions-create-function-app-portal.md#storage-account-requirements)

### <a name="using-key-vault-references"></a>Verwenden von Key Vault-Verweisen 

Key Vault-Verweise ermöglichen Ihnen die Verwendung von Geheimnissen aus Ihrer Azure Key Vault-Instanz in Ihrer Azure Functions-Anwendung, ohne dass Codeänderungen erforderlich sind. Azure Key Vault ist ein Dienst, der eine zentralisierte Verwaltung von Geheimnissen mit voller Kontrolle über Zugriffsrichtlinien und Überprüfungsverlauf ermöglicht.

Derzeit funktionieren [Key Vault-Verweise](../app-service/app-service-key-vault-references.md) nicht, wenn Ihr Key Vault-Instanz mit Dienstendpunkten gesichert ist. Zum Herstellen einer Verbindung mit einer Key Vault-Instanz mithilfe der Integration von virtuellen Netzwerken müssen Sie Key Vault in Ihrem Anwendungscode aufrufen.

## <a name="virtual-network-triggers-non-http"></a>Trigger für virtuelle Netzwerke (nicht HTTP)

HTTP-fremde Triggerfunktionen können aktuell auf zwei Arten von einem virtuellen Netzwerk aus verwendet werden: 
+ Sie können Ihre Funktions-App in einem Premium-Plan ausführen und die Triggerunterstützung für virtuelle Netzwerke aktivieren.
+ Sie können Ihre Funktions-App in einem App Service-Plan oder in einer App Service-Umgebung ausführen.

### <a name="premium-plan-with-virtual-network-triggers"></a>Premium-Plan mit Triggern für virtuelle Netzwerke

Bei Verwendung eines Premium-Plans können Sie HTTP-fremde Triggerfunktionen mit Diensten verbinden, die innerhalb eines virtuellen Netzwerks ausgeführt werden. Hierzu müssen Sie für Ihre Funktions-App die Triggerunterstützung für virtuelle Netzwerke aktivieren. Die Einstellung **Triggerunterstützung für virtuelle Netzwerke** finden Sie im [Azure-Portal](https://portal.azure.com) unter **Funktions-App-Einstellungen**.

![VNETToggle](media/functions-networking-options/virtual-network-trigger-toggle.png)

Trigger für virtuelle Netzwerke können auch mithilfe des folgenden Azure CLI-Befehls aktiviert werden:

```azurecli-interactive
az resource update -g <resource_group> -n <function_app_name>/config/web --set properties.functionsRuntimeScaleMonitoringEnabled=1 --resource-type Microsoft.Web/sites
```

Trigger für virtuelle Netzwerke werden ab Version 2.x der Functions-Runtime unterstützt. Folgende HTTP-fremde Triggertypen werden unterstützt:

| Durchwahl | Mindestversion |
|-----------|---------| 
|[Microsoft.Azure.WebJobs.Extensions.Storage](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.Storage/) | 3.0.10 oder höher |
|[Microsoft.Azure.WebJobs.Extensions.EventHubs](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.EventHubs)| 4.1.0 oder höher|
|[Microsoft.Azure.WebJobs.Extensions.ServiceBus](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.ServiceBus)| 3.2.0 oder höher|
|[Microsoft.Azure.WebJobs.Extensions.CosmosDB](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.CosmosDB)| 3.0.5 oder höher|
|[Microsoft.Azure.WebJobs.Extensions.DurableTask](https://www.nuget.org/packages/Microsoft.Azure.WebJobs.Extensions.DurableTask)| 2.0.0 oder höher|

> [!IMPORTANT]
> Wenn Sie die Triggerunterstützung für virtuelle Netzwerke aktivieren, werden nur die oben aufgeführten Triggertypen dynamisch mit Ihrer Anwendung skaliert. Sie können zwar weiterhin Trigger verwenden, die oben nicht aufgeführt sind, diese werden jedoch nicht über die Anzahl vorab aufgewärmter Instanzen hinaus skaliert. Die vollständige Triggerliste finden Sie unter [Unterstützte Bindungen](./functions-triggers-bindings.md#supported-bindings).

### <a name="app-service-plan-and-app-service-environment-with-virtual-network-triggers"></a>App Service-Plan und App Service-Umgebung mit Triggern für virtuelle Netzwerke

Wenn Ihre Funktions-App entweder in einem App Service-Plan oder in einer App Service-Umgebung ausgeführt wird, können Sie HTTP-fremde Triggerfunktionen verwenden. Damit Ihre Funktionen ordnungsgemäß ausgelöst werden, muss eine Verbindung mit einem virtuellen Netzwerk bestehen. Außerdem muss auf die in der Triggerverbindung definierte Ressource zugegriffen werden können. 

Angenommen, Sie möchten Azure Cosmos DB so konfigurieren, dass nur Datenverkehr aus einem virtuellen Netzwerk akzeptiert wird. In diesem Fall müssen Sie Ihre Funktions-App in einem App Service-Plan bereitstellen, der die VNET-Integration mit diesem virtuellen Netzwerk ermöglicht. Dadurch kann eine Funktion durch diese Azure Cosmos DB-Ressource ausgelöst werden. 

## <a name="hybrid-connections"></a>Hybridverbindungen

[Hybrid Connections](../service-bus-relay/relay-hybrid-connections-protocol.md) ist ein Feature von Azure Relay, das Sie zum Zugreifen auf Anwendungsressourcen in anderen Netzwerken verwenden können. Es ermöglicht den Zugriff von Ihrer App auf einen Anwendungsendpunkt. Sie können es nicht verwenden, um auf Ihre Anwendung zuzugreifen. Hybrid Connections steht für Funktionen unter Windows in allen Tarifen (mit Ausnahme des Verbrauchstarifs) zur Verfügung.

Bei der Verwendung in Azure Functions entspricht jede Hybridverbindung einer Kombination aus einem einzelnen TCP-Host und einem Port. Dies bedeutet, dass sich der Hybridverbindungsendpunkt in einem beliebigen Betriebssystem und einer beliebigen Anwendung befinden kann, solange der Zugriff über einen TCP-Überwachungsport erfolgt. Das Feature „Hybrid Connections“ verfügt nicht über Informationen zum Anwendungsprotokoll oder zum abzurufenden Inhalt und benötigt diese Informationen auch nicht. Es ermöglicht lediglich den Netzwerkzugriff.

Weitere Informationen finden Sie in der [App Service-Dokumentation zu Hybrid Connections](../app-service/app-service-hybrid-connections.md). Diese Konfigurationsschritte unterstützen auch Azure Functions.

>[!IMPORTANT]
> Hybrid Connections wird nur in Windows-Tarifen unterstützt. Linux wird nicht unterstützt.

## <a name="outbound-ip-restrictions"></a>IP-Einschränkungen für ausgehenden Datenverkehr

IP-Einschränkungen für ausgehenden Datenverkehr sind nur für Funktionen verfügbar, die in einer App Service-Umgebung bereitgestellt werden. Sie können ausgehende Einschränkungen für das virtuelle Netzwerk konfigurieren, wo Ihre App Service-Umgebung bereitgestellt wird.

Wenn Sie eine Funktions-App in einen Premium-Tarif oder einen App Service-Plan mit einem virtuellen Netzwerk integrieren, kann die App weiterhin ausgehende Aufrufe ins Internet vornehmen.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zum Netzwerk und zu Azure Functions:

* [Tutorial zum Einstieg in die Integration des virtuellen Netzwerks](./functions-create-vnet.md)
* [Häufig gestellte Fragen zu Netzwerken in Functions](./functions-networking-faq.md)
* [Weitere Informationen zur Integration des virtuellen Netzwerks mit App Service und Functions](../app-service/web-sites-integrate-with-vnet.md)
* [Weitere Informationen zu virtuellen Netzwerken in Azure](../virtual-network/virtual-networks-overview.md)
* [Aktivieren weiterer Netzwerkfunktionen und Steuerung mit App Service-Umgebungen](../app-service/environment/intro.md)
* [Verbinden mit einzelnen lokalen Ressourcen ohne Änderungen an der Firewall mithilfe von Hybrid Connections](../app-service/app-service-hybrid-connections.md)
