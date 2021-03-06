<properties 
   pageTitle="Planung und Entwurf von VPN-Gateways | Microsoft Azure"
   description="Erfahren Sie mehr über Planung und Entwurf von VPN-Gateways für standortübergreifende, Hybrid- und VNet-zu-VNet-Verbindungen."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management,azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/16/2016"
   ms.author="cherylmc"/>

# Planung und Entwurf für VPN Gateway

Die Planung und der Entwurf Ihrer standortübergreifenden und VNet-zu-VNet-Verbindungen sind je nach Ihren Netzwerkanforderungen entweder sehr einfach oder recht kompliziert. Dieser Artikel führt Sie durch die grundlegenden Aspekte der Planung und des Entwurfs.

## Planung


### <a name="compare"></a>Optionen der standortübergreifenden Konnektivität

Wenn Sie Ihre lokalen Standorte sicher mit einem virtuellen Netzwerk verbinden möchten, stehen Ihnen drei verschiedene Möglichkeiten zur Verfügung: Standort-zu-Standort, Punkt-zu-Standort und ExpressRoute. Vergleichen Sie die verschiedenen verfügbaren standortübergreifenden Verbindungen. Die von Ihnen gewählte Option kann von einer Vielzahl von Faktoren abhängen:


- Welche Form von Durchsatz benötigt Ihre Lösung?
- Soll die Kommunikation über das öffentliche Internet über ein sicheres VPN oder über eine private Verbindung erfolgen?
- Steht Ihnen eine öffentliche IP-Adresse zur Verfügung?
- Planen Sie den Einsatz eines VPN-Geräts? Wenn ja, ist es kompatibel?
- Stellen Sie nur eine Verbindung zwischen ein paar Computern her, oder möchten Sie eine dauerhafte Verbindung für Ihren Standort?
- Welche Art von VPN-Gateway ist für die Lösung erforderlich, die Sie erstellen möchten?
- Welche Gateway-SKU sollten Sie verwenden?


Die folgende Tabelle kann Ihnen dabei helfen, die beste Verbindungsoption für Ihre Lösung zu finden.


[AZURE.INCLUDE [vpn-gateway-cross-premises](../../includes/vpn-gateway-cross-premises-include.md)]



### <a name="gwrequire"></a>Gatewayanforderungen nach VPN-Typ und SKU


Beim Erstellen eines VPN Gateways müssen Sie die gewünschte Gateway-SKU angeben. Es gibt drei VPN-Gateway-SKUs:

- Basic
- Standard
- Leistung

[AZURE.INCLUDE [vpn-gateway-table-requirements](../../includes/vpn-gateway-table-requirements-include.md)]



### <a name="aggthroughput"></a>Gatewaytypen und aggregierte Durchsatzschätzungen

In der folgenden Tabelle sind die Gatewaytypen und der geschätzte zusammengefasste Durchsatz angegeben. Der geschätzte zusammengefasste Durchsatz ist möglicherweise ein entscheidender Faktor für den Entwurf. Für die einzelnen Gateway-SKUs gelten unterschiedliche Preise. Informationen zu den Preisen finden Sie unter [VPN-Gateway – Preise](https://azure.microsoft.com/pricing/details/vpn-gateway/). Diese Tabelle betrifft sowohl das Resource Manager-Bereitstellungsmodell als auch das klassische Bereitstellungsmodell.

[AZURE.INCLUDE [vpn-gateway-table-gwtype-aggtput](../../includes/vpn-gateway-table-gwtype-aggtput-include.md)]



### <a name="wf"></a>Workflow

Die folgende Liste beschreibt den allgemeinen Workflow für die Cloud-Konnektivität:

1.	Entwerfen und planen Sie die Netzwerktopologie, und erstellen Sie eine Liste der Adressräume für alle Netzwerke, die Sie verbinden möchten.
2.	Erstellen Sie ein virtuelles Azure-Netzwerk. 
3.	Erstellen Sie ein VPN-Gateway für das virtuelle Netzwerk.
4.	Erstellen und konfigurieren Sie Verbindungen mit lokalen Netzwerken oder anderen virtuellen Netzwerken (bei Bedarf).
5.	Erstellen und konfigurieren Sie eine Punkt-zu-Standort-Verbindung für Ihr Azure-VPN-Gateway (bei Bedarf).
 

## Entwurf

### <a name="topologies"></a>Verbindungstopologien

Sehen Sie sich zunächst die Diagramme im Artikel [Verbindungstopologien](vpn-gateway-topology.md) an. Der Artikel enthält grundlegende Diagramme, die Bereitstellungsmodelle für jede Topologie (Resource Manager oder klassisch) und Informationen dazu, welche Bereitstellungstools Sie für Ihre Konfiguration verwenden können.

### <a name="designbasics"></a>Entwurfsgrundlagen

In den folgenden Abschnitten werden die Grundlagen des VPN-Gateways erläutert. Außerdem sollten Sie die [Einschränkungen der Netzwerkdienste](../articles/azure-subscription-service-limits.md#networking-limits) berücksichtigen.


#### <a name="subnets"></a>Informationen zu Subnetzen

Beim Planen und Entwerfen der für Ihre Umgebung am besten geeigneten Verbindung ist sehr wichtig, die IP-Adressbereiche und Subnetze zu berücksichtigen, die Ihnen zur Verfügung stehen.

Sie müssen ein Gatewaysubnetz für Ihr VNet erstellen, um ein VPN Gateway zu konfigurieren. Alle Gatewaysubnetze müssen den Namen „GatewaySubnet“ haben, damit sie einwandfrei funktionieren. Achten Sie darauf, Ihrem Gatewaysubnetz keinen anderen Namen zu geben, und stellen Sie dem Gatewaysubnetz keine virtuellen Computer o. ä. bereit. Weitere Informationen zu Gatewaysubnetzen finden Sie im Abschnitt [Gatewaysubnetz](vpn-gateway-about-vpngateways.md#gwsub) des Artikels „Informationen zu VPN-Gateways“.

Beim Erstellen von Verbindungen müssen Sie in vielen Fällen sicherstellen, dass sich die Subnetz-Adressbereiche zwischen Ihren Verbindungen nicht überschneiden. Bei überlappenden Subnetzen enthalten ein virtuelles Netzwerk und ein lokaler Standort denselben Adressraum. Dies bedeutet, dass die Netzwerktechniker für Ihre lokalen Netzwerke einen Bereich zur Verwendung für Ihre Azure-IP-Adressräume/Subnetze zuteilen müssen. Sie benötigen einen Adressraum, der nicht im lokalen Netzwerk verwendet wird.

Das Vermeiden von überlappenden Subnetzen ist auch bei der Arbeit mit VNet-zu-VNet-Verbindungen wichtig. Das Erstellen einer VNet-zu-VNet-Verbindung schlägt fehl, wenn die Subnetze überlappen und eine IP-Adresse sowohl im sendenden als auch im Ziel-VNet vorhanden ist. In diesem Fall kann Azure die Daten nicht an das andere VNet weiterleiten, da die Zieladresse Teil des sendenden VNet ist.



#### <a name="local"></a>Informationen zu Gateways des lokalen Netzwerks

Mit dem Gateway des lokalen Netzwerks ist normalerweise Ihr lokaler Standort gemeint. Beim klassischen Bereitstellungsmodell wurde das Gateway des lokalen Netzwerks als „Lokaler Standort“ bezeichnet. Sie geben dem Gateway des lokalen Netzwerks einen Namen, stellen die öffentliche IP-Adresse des lokalen VPN-Geräts bereit und geben die Adresspräfixe an, die sich am lokalen Standort befinden. Azure sucht unter den Zieladresspräfixen nach Netzwerkdatenverkehr, sieht in der Konfiguration nach, die Sie für das Gateway des lokalen Netzwerks angegeben haben, und leitet die Pakete entsprechend weiter. Sie können diese Adresspräfixe bei Bedarf ändern. Weitere Informationen zu Gateways des lokalen Netzwerks finden Sie im Abschnitt [Gateways des lokalen Netzwerks](vpn-gateway-about-vpngateways.md#lng) des Artikels „Informationen zu VPN-Gateways“.


#### <a name="gwtype"></a>Informationen zu Gatewaytypen

Das Auswählen des richtigen Gateways für Ihre Topologie ist wichtig. Ihr Gateway funktioniert nicht ordnungsgemäß, wenn Sie den falschen Typ auswählen. Mit dem Gatewaytyp wird angegeben, wie die Verbindung für das Gateway selbst hergestellt wird. Es ist eine erforderliche Konfigurationseinstellung für das Resource Manager-Bereitstellungsmodell.

VPN-Gatewaytypen:

- VPN
- ExpressRoute

#### <a name="connectiontype"></a>Informationen zu Verbindungstypen

Für jede Konfiguration ist ein bestimmter Verbindungstyp erforderlich. Verbindungstypen:

- IPsec
- Vnet2Vnet
- ExpressRoute
- VPNClient


#### <a name="vpntype"></a>Informationen zu VPN-Typen

Für jede Konfiguration ist ein bestimmter VPN-Typ erforderlich, damit sie funktioniert. Wenn Sie zwei Konfigurationen kombinieren, z.B. das Erstellen einer Site-to-Site-Verbindung und einer Point-to-Site-Verbindung mit demselben VNET, müssen Sie einen VPN-Typ verwenden, mit dem beide Verbindungsanforderungen erfüllt werden. Bei einer Koexistenz von Point-to-Site- und Site-to-Site-Verbindungen müssen Sie einen routenbasierten VPN-Typ nutzen, wenn Sie das Azure Resource Manager-Bereitstellungsmodell verwenden, bzw. ein dynamisches Gateway, wenn Sie das klassische Bereitstellungsmodell verwenden.

[AZURE.INCLUDE [vpn-gateway-vpntype](../../includes/vpn-gateway-vpntype-include.md)]

Die folgenden Tabellen zeigen VPN-Typen mit der jeweiligen Zuordnung zu einer Verbindungskonfiguration. Stellen Sie sicher, dass der VPN-Typ für Ihr Gateway mit der Konfiguration übereinstimmt, die Sie erstellen möchten.


[AZURE.INCLUDE [vpn-gateway-table-vpntype](../../includes/vpn-gateway-table-vpntype-include.md)]

### <a name="devices"></a>VPN-Geräte für Standort-zu-Standort-Verbindungen

Zum Konfigurieren einer Standort-zu-Standort-Verbindung benötigen Sie unabhängig vom Bereitstellungsmodell die folgenden Elemente:

- Ein mit Azure-VPN-Gateways kompatibles VPN-Gerät
- Eine öffentlich zugängliche IPv4-IP-Adresse, die sich nicht hinter einem NAT-Gerät befindet

Darüber hinaus müssen Sie im Konfigurieren von VPN-Geräten erfahren sein. Weitere Informationen zu VPN-Geräten finden Sie unter [Informationen zu VPN-Geräten](vpn-gateway-about-vpn-devices.md). Der Artikel zu VPN-Geräten enthält Informationen zu überprüften Geräten, Anforderungen an nicht überprüfte Geräte und Links zu den Dokumenten zur Gerätekonfiguration die einzelnen Geräte, sofern diese verfügbar sind.

### <a name="forcedtunnel"></a>Erwägen der Weiterleitung per Tunnelerzwingung

Für die meisten Konfigurationen können Sie die Tunnelerzwingung konfigurieren. Über die Tunnelerzwingung können Sie die Umleitung des gesamten Internetdatenverkehrs an Ihren lokalen Standort „erzwingen“. Sie verwenden dazu einen Standort-zu-Standort-VPN-Tunnel für die Kontrolle und Überwachung. Dies ist eine wichtige Sicherheitsvoraussetzung der IT-Richtlinien für die meisten Unternehmen.

Ohne die Tunnelerzwingung wird der Internetdatenverkehr Ihrer virtuellen Computer in Azure immer direkt von der Azure-Netzwerkinfrastruktur an das Internet geleitet, ohne dass Sie die Möglichkeit haben, diesen zu überprüfen oder zu überwachen. Nicht autorisierter Zugriff auf das Internet kann potenziell zur Offenlegung von Informationen oder anderen Arten von Sicherheitsverletzungen führen.

Weitere Informationen zum Konfigurieren der Tunnelerzwingung finden Sie unter [Konfigurieren der Tunnelerzwingung mit dem klassischen Bereitstellungsmodell](vpn-gateway-about-forced-tunneling.md) und [Konfigurieren der Tunnelerzwingung mit dem Azure Resource Manager-Bereitstellungsmodell](vpn-gateway-about-forced-tunneling.md).

**Diagramm zur Tunnelerzwingung**

![Verbindung durch Tunnelerzwingung](./media/vpn-gateway-plan-design/forced-tunnel.png "Tunnelerzwingung")

Eine Verbindung mit erzwungenem Tunneling kann in beiden Bereitstellungsmodellen und mit unterschiedlichen Tools konfiguriert werden. Weitere Informationen finden Sie in der folgenden Tabelle. Wir aktualisieren diese Tabelle, wenn neue Artikel, neue Bereitstellungsmodelle und weitere Tools für diese Konfiguration verfügbar werden. Wenn ein Artikel verfügbar ist, fügen wir in der Tabelle einen direkten Link dazu ein.

[AZURE.INCLUDE [vpn-gateway-table-forcedtunnel](../../includes/vpn-gateway-table-forcedtunnel-include.md)]



## Nächste Schritte

In den Artikeln [Häufig gestellte Fragen zu VPN-Gateways](vpn-gateway-vpn-faq.md) und [Informationen zu VPN-Gateways](vpn-gateway-about-vpngateways.md) finden Sie weitere Informationen zur Unterstützung bei Ihrem Entwurf.

Weitere Informationen zu Verbindungstopologien finden Sie unter [Verbindungstopologien](vpn-gateway-topology.md).

<!---HONumber=AcomDC_0518_2016-->