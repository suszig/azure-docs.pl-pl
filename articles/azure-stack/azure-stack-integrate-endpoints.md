---
title: "Azure stosu integracji datacenter — publikować punkty końcowe"
description: "Dowiedz się, jak publikować punkty końcowe platformy Azure stosu w centrum danych."
services: azure-stack
author: jeffgilb
ms.service: azure-stack
ms.topic: article
ms.date: 01/31/2018
ms.author: jeffgilb
ms.reviewer: wamota
keywords: 
ms.openlocfilehash: e368109adc7db4c589ac37b28c4891cb3ec5346f
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
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
|ADFS|Adfs.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Portal (administrator)|Adminportal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12499<br>12646<br>12647<br>12648<br>12649<br>12650<br>13001<br>13003<br>13010<br>13011<br>13020<br>13021<br>13026<br>30015|
|Usługa Azure Resource Manager (administrator)|Adminmanagement.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Portal (użytkownika)|Portal.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>12495<br>12649<br>13001<br>13010<br>13011<br>13020<br>13021<br>30015<br>13003|
|Usługa Azure Resource Manager (użytkownika)|Management.*&lt;region>.&lt;fqdn>*|HTTPS|443<br>30024|
|Graph|Graph.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Listy odwołania certyfikatów|Crl.*&lt;region>.&lt;fqdn>*|HTTP|80|
|DNS|&#42;.*&lt;region>.&lt;fqdn>*|TCP I UDP|53|
|Magazyn kluczy (użytkownika)|&#42;.vault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Magazyn kluczy (administrator)|&#42;.adminvault.*&lt;region>.&lt;fqdn>*|HTTPS|443|
|Kolejka magazynu|&#42;.queue.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Tabela magazynu|&#42;.table.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Obiektu Blob magazynu|&#42;.blob.*&lt;region>.&lt;fqdn>*|HTTP<br>HTTPS|80<br>443|
|Dostawca zasobów SQL|sqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304|
|Dostawca zasobów MySQL|mysqladapter.dbadapter.*&lt;region>.&lt;fqdn>*|HTTPS|44300-44304

## <a name="ports-and-urls-outbound"></a>Porty i adresy URL (wychodzące)

Stos Azure obsługuje tylko serwery przezroczystego obiektu pośredniczącego. W przypadku wdrożenia w przypadku, gdy pasm przezroczystego obiektu pośredniczącego, aby serwer proxy tradycyjnych musisz zezwolić następujące porty i adresy URL, dla komunikacji wychodzącej:


|Przeznaczenie|Adres URL|Protokół|Porty|
|---------|---------|---------|---------|
|Tożsamość|login.windows.net<br>login.microsoftonline.com<br>graph.windows.net|HTTP<br>HTTPS|80<br>443|
|Syndykacja Marketplace|https://management.azure.com<br>https://&#42;.blob.core.windows.net<br>https://*.azureedge.net<br>https://&#42;.microsoftazurestack.com|HTTPS|443|
|Poprawek i aktualizacji|https://&#42;.azureedge.net|HTTPS|443|
|Rejestracja|https://management.azure.com|HTTPS|443|
|Sposób użycia|https://&#42;.microsoftazurestack.com<br>https://*.trafficmanager.com|HTTPS|443|


## <a name="next-steps"></a>Kolejne kroki
[Wymagania dotyczące usługi Azure stosu infrastruktury kluczy publicznych](azure-stack-pki-certs.md)