---
title: "Azure stosu integracji datacenter — publikować punkty końcowe"
description: "Dowiedz się, jak publikować punkty końcowe platformy Azure stosu w centrum danych."
services: azure-stack
author: troettinger
ms.service: azure-stack
ms.topic: article
ms.date: 10/18/2017
ms.author: victorh
keywords: 
ms.openlocfilehash: 0d15252079b62f6a74a1279309fb9b1b3ed5711e
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/18/2017
---
# <a name="azure-stack-datacenter-integration---publish-endpoints"></a>Azure stosu integracji datacenter — publikować punkty końcowe

*Dotyczy: Azure stosu zintegrowane systemy*

Stos Azure konfiguruje różnych punktów końcowych (adresów VIP - wirtualnych adresów IP) dla jego role infrastruktury. Te adresy VIP są przydzielone z puli publicznych adresów IP. Każdy adres VIP jest zabezpieczony z listy kontroli dostępu (ACL) w warstwie sieci zdefiniowanych przez oprogramowanie. Listy ACL są również używane przez przełączniki fizyczne (torach i BMC) dodatkowo zabezpieczyć rozwiązanie. Wpis DNS jest tworzony dla każdego punktu końcowego w strefie DNS zewnętrznego, który został określony w czasie wdrażania.


Na poniższym diagramie architektury przedstawiono warstwy inną sieć i listy kontroli dostępu:

![Diagram architektury](media/azure-stack-integrate-endpoints/Integrate-Endpoints-01.png)

## <a name="ports-and-protocols-inbound"></a>Porty i protokoły (ruch przychodzący)

W poniższej tabeli wymieniono infrastruktury wirtualne adresy IP, które są wymagane do publikowania stosu Azure punktów końcowych sieci zewnętrznych. Lista zawiera każdego punktu końcowego, wymagany port i protokół. Punkty końcowe wymagane dla dostawców dodatkowych zasobów, takich jak dostawca zasobów SQL i innych osób, znajdują się w dokumentacji wdrażania dostawcy określonego zasobu.

Wewnętrzna infrastruktura adresy VIP nie są wyświetlane, ponieważ nie są one wymagane do publikowania stosu Azure.

> [!NOTE]
> Adresy VIP użytkownika są dynamiczne, zdefiniowany przez użytkowników się za pomocą formantu nie przez podmiot stosu Azure.


|Punkt końcowy (VIP)|Rekord hosta DNS|Protokół|Porty|
|---------|---------|---------|---------|
|USŁUGI AD FS|`Adfs.[Region].[External FQDN]`|HTTPS|443|
|Portal (administrator)|`Adminportal.[Region].[External FQDN]`|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Usługa Azure Resource Manager (administrator)|`Adminmanagement.[Region].[External FQDN]`|HTTPS|443<br>30024|
|Portal (użytkownika)|`Portal. [Region].[External FQDN]`|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|Usługa Azure Resource Manager (użytkownika)|`Management.[Region].[External FQDN]`|HTTPS|443<br>30024|
|Graph|`Graph.[Region].[External FQDN]`|HTTPS|443|
|Listy odwołania certyfikatów|`Crl.[Region].[External FQDN]`|HTTP|80|
|DNS|`*.[Region].[External FQDN]`|TCP I UDP|53|
|Magazyn kluczy (użytkownika)|`*.vault.[Region].[External FQDN]`|TCP|443|
|Magazyn kluczy (administrator)|`*.adminvault.[Region].[External FQDN]`|TCP|443|
|Kolejki magazynu|`*.queue.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|
|Tabela magazynu|`*.table.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|
|Obiektu Blob magazynu|`*.blob.[Region].[External FQDN]`|HTTP<br>HTTPS|80<br>443|

## <a name="ports-and-urls-outbound"></a>Porty i adresy URL (wychodzące)

Stos Azure obsługuje tylko serwery przezroczystego obiektu pośredniczącego. W przypadku wdrożenia w przypadku, gdy pasm przezroczystego obiektu pośredniczącego, aby serwer proxy tradycyjnych musisz zezwolić następujące porty i adresy URL, dla komunikacji wychodzącej:


|Przeznaczenie|ADRES URL|Protokół|Porty|
|---------|---------|---------|---------|
|Tożsamość|`login.windows.net`<br>`login.microsoftonline.com`<br>`graph.windows.net`|HTTP<br>HTTPS|80<br>443|
|Syndykacja Marketplace|`https://management.azure.com`<br>`https://*.blob.core.windows.net`<br>`https://*.azureedge.net`<br>`https://*.microsoftazurestack.com`|HTTPS|443|
|Poprawek i aktualizacji|`https://*.azureedge.net`|HTTPS|443|
|Rejestracja|`https://management.azure.com`|HTTPS|443|
|Sposób użycia|`https://*.microsoftazurestack.com`<br>`https://*.trafficmanager.com`|HTTPS|443|

## <a name="firewall-publishing"></a>Publikowanie zapory

Portach wymienionych w poprzedniej sekcji dotyczą przychodzącej komunikacji podczas publikowania usług Azure stosu przez zaporę istniejących.

Zalecane jest użycie urządzenia zapory ułatwiające bezpieczne stosu Azure. Jednak nie jest ścisłym wymogiem. Mimo że zapory można ułatwić rozproszonej typu "odmowa usługi" (DDOS) ataków i inspekcji zawartości, może również zostać wąskie gardło przepływności dla usług magazynu Azure, takich jak obiekty BLOB, tabel i kolejek.

Oparte na modelu tożsamości (Azure AD lub AD FS), może lub nie mogą wymagać opublikować punktu końcowego usług AD FS. Jeśli używany jest tryb odłączonego wdrożenia, należy opublikować punktu końcowego usług AD FS. (Aby uzyskać więcej informacji, zobacz temat identity integration centrum danych).

Usługi Azure Resource Manager (administrator), portalu administratora i punktów końcowych usługi Key Vault (administrator) nie wymaga zewnętrznego publikowania. To zależy od scenariusza. Na przykład jako dostawcę usługi można ograniczyć obszar narażony na ataki i tylko administrowania stosu Azure z wewnątrz sieci, a nie z Internetu.

W przedsiębiorstwie zewnętrznej sieci może być istniejącej sieci firmowej. W takiej sytuacji należy opublikować te punkty końcowe działanie stosu Azure z sieci firmowej.

## <a name="edge-firewall-scenario"></a>Scenariusz zapory krawędzi

We wdrożeniu krawędzi stosu Azure jest wdrażany bezpośrednio za router brzegowy (udostępniony przez usługodawcę internetowego) lub bez zapory przed nim.

![Diagram architektury wdrożenia krawędzi stosu Azure](media/azure-stack-integrate-endpoints/Integrate-Endpoints-02.png)

Zazwyczaj publiczne rutowalne adresy IP są określone dla publicznych pulę adresów VIP w czasie wdrażania we wdrożeniu krawędzi. Ten scenariusz umożliwia użytkownikowi wystąpić środowisko pełnej chmury własnym kontrolowanego, tak jak w publicznej w chmurze, takich jak Azure.

### <a name="using-nat"></a>Przy użyciu translatora adresów Sieciowych

Chociaż nie jest zalecane z powodu obciążenie, można użyć translatora adresów sieciowych (NAT) dla publikowania punktów końcowych. Publikowanie punktu końcowego, który pełni jest kontrolowany przez użytkowników, wymaga reguły NAT dla poszczególnych użytkowników VIP, który zawiera wszystkie porty, które użytkownik może użyć.

Kolejnym zagadnieniem są Azure nie obsługuje konfigurowania tunelu VPN punkt końcowy w scenariuszu chmury hybrydowej z platformy Azure przy użyciu translatora adresów Sieciowych.

## <a name="enterpriseintranetperimeter-network-firewall-scenario"></a>Scenariusz zapory sieci obwodowej Enterprise/intranet

We wdrożeniu obwodowej enterprise/intranet stosu Azure jest wdrażany poza drugi zapory, która zwykle jest częścią sieci obwodowej (znanej także jako strefa DMZ).

![Azure scenariusz zapory stosu](media/azure-stack-integrate-endpoints/Integrate-Endpoints-03.png)

Publiczne adresy IP routingu zostały określone dla publicznych pulę adresów VIP stosu Azure, te adresy logicznie należą do sieci obwodowej i wymagają reguł publikowania na zaporze podstawowej.

### <a name="using-nat"></a>Przy użyciu translatora adresów Sieciowych

Użycie niepublicznych rutowalne adresy IP dla puli adresów VIP publicznego stosu Azure NAT służy na zaporze dodatkowej do publikowania punktów końcowych stosu Azure. W tym scenariuszu należy skonfigurować reguły publikowania na zaporze głównej poza krawędź i dodatkowej zapory. Jeśli chcesz użyć NAT, należy wziąć pod uwagę następujące kwestie:

- Translator adresów Sieciowych dodaje nakłady związane z zarządzaniem reguły zapory, ponieważ użytkownicy kontrolować własnych punktów końcowych i własne reguły publikowania w sieci stosu zdefiniowanych przez oprogramowanie (SDN). Użytkownicy, musisz skontaktować się z operatora stosu Azure można pobrać ich adresy VIP opublikowane oraz do aktualizowania listy portów.
- Podczas użycia NAT ogranicza środowisko użytkownika, zapewnia pełnej kontroli dla operatora za pośrednictwem publikowania żądania.
- W scenariuszach chmur hybrydowych przy użyciu platformy Azure należy wziąć pod uwagę Azure nie obsługuje konfigurowania tunel sieci VPN do punktu końcowego przy użyciu translatora adresów sieciowych.


## <a name="next-steps"></a>Następne kroki

[Integracja Azure datacenter stosu - zabezpieczeń](azure-stack-integrate-security.md)