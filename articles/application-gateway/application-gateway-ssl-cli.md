---
title: "Konfigurowanie protokołu SSL odciążania - Azure Application Gateway - Azure CLI 2.0 | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera instrukcje, aby utworzyć bramę aplikacji przy użyciu protokołu SSL Odciążanie przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0"
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/26/2017
ms.author: davidmu
ms.openlocfilehash: a2c4062db821e39e1af4fa1d54da0121d3993db4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-cli-20"></a>Skonfiguruj bramę aplikacji dla odciążania protokołu SSL przy użyciu interfejsu wiersza polecenia platformy Azure w wersji 2.0

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](application-gateway-ssl-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-ssl-arm.md)
> * [Program Azure PowerShell klasycznego](application-gateway-ssl.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](application-gateway-ssl-cli.md)

Usługę Azure Application Gateway można skonfigurować tak, aby przerywała sesję protokołu SSL (Secure Sockets Layer) na poziomie bramy, co pozwoli na uniknięcie wykonywania kosztownych zadań szyfrowania protokołu SSL w kolektywie serwerów sieci Web. Odciążanie protokołu SSL także upraszcza zarządzanie certyfikatami na serwerze frontonu.

## <a name="prerequisite-install-the-azure-cli-20"></a>Wymagania wstępne: Instalacja Azure CLI 2.0

Aby wykonać kroki opisane w tym artykule, należy [zainstalować interfejs wiersza polecenia platformy Azure dla komputerów Mac, Linux i Windows (Azure CLI)](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="required-components"></a>Wymagane składniki

* **Pula serwerów zaplecza**: Lista adresów IP serwerów zaplecza. Na liście adresów IP powinna należeć do podsieci sieci wirtualnej lub powinien być publiczny adres IP lub wirtualnego adresu IP (VIP).
* **Ustawienia puli serwera zaplecza**: co Pula ma ustawienia, takie jak port protokołu i koligacji na podstawie plików cookie. Te ustawienia są powiązane z pulą i są stosowane do wszystkich serwerów w tej puli.
* **Port frontonu**: ten port jest port publiczny, który jest otwarty na bramie aplikacji. Ruch trafia do tego portu, a następnie jest przekierowywany do jednego z serwerów zaplecza.
* **Odbiornik**: odbiornika ma port frontonu, protokół (Http lub Https; tych ustawień jest rozróżniana wielkość liter) oraz nazwę certyfikatu SSL (jeśli odciążania Konfigurowanie protokołu SSL).
* **Reguła**: reguła wiąże odbiornika i puli serwerów zaplecza i określa, które puli serwerów zaplecza umożliwiających kierowanie ruchem do gdy liczba trafień określonego odbiornika. Obecnie jest obsługiwana tylko reguła *podstawowa*. Reguła *podstawowa* to dystrybucja obciążenia z działaniem okrężnym.

**Uwagi dotyczące konfiguracji dodatkowych**

W przypadku konfiguracji certyfikatów SSL protokół w polu **HttpListener** należy zmienić na *Https* (z uwzględnieniem wielkości liter). Dodaj **SslCertificate** elementu **HttpListener** z wartością zmiennej skonfigurowane dla certyfikatu SSL. Port frontonu powinny zostać uaktualnione do **443**.

**Aby włączyć koligacji na podstawie plików cookie**: można skonfigurować bramę aplikacji, aby upewnić się, że żądanie z sesji klienta jest zawsze kierowane do tej samej maszyny Wirtualnej w kolektywie serwerów sieci web. W tym celu należy wstawić umożliwia bramy do kierowania ruchem danych odpowiednio plik cookie sesji. Aby włączyć koligację opartą na plikach cookie, ustaw element **CookieBasedAffinity** na wartość *Enabled* w elemencie **BackendHttpSettings**.

## <a name="configure-ssl-offload-on-an-existing-application-gateway"></a>Skonfiguruj odciążanie protokołu SSL na istniejącą bramę aplikacji

Wprowadź następujące polecenia, aby skonfigurować odciążanie protokołu SSL do istniejącej bramy aplikacji:

```azurecli-interactive
#!/bin/bash

# Create a new front end port to be used for SSL
az network application-gateway frontend-port create \
  --name sslport \
  --port 443 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Upload the .pfx certificate for SSL offload
az network application-gateway ssl-cert create \
  --name "newcert" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new listener referencing the port and certificate created earlier
az network application-gateway http-listener create \
  --frontend-ip "appGatewayFrontendIP" \
  --frontend-port sslport  \
  --name sslListener \
  --ssl-cert newcert \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new back-end pool to be used
az network application-gateway address-pool create \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG" \
  --name "appGatewayBackendPool2" \
  --servers 10.0.0.7 10.0.0.8

# Create a new back-end HTTP settings using the new probe
az network application-gateway http-settings create \
  --name "settings2" \
  --port 80 \
  --cookie-based-affinity Enabled \
  --protocol "Http" \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

# Create a new rule linking the listener to the back-end pool
az network application-gateway rule create \
  --name "rule2" \
  --rule-type Basic \
  --http-settings settings2 \
  --http-listener ssllistener \
  --address-pool temp1 \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"

```

## <a name="create-an-application-gateway-with-ssl-offload"></a>Utwórz bramę aplikacji z odciążania protokołu SSL

Poniższy przykład tworzy bramę aplikacji z odciążania protokołu SSL. Certyfikat i hasło muszą zostać zaktualizowane do prawidłowego klucza prywatnego.

```azurecli-interactive
#!/bin/bash

# Creates an application gateway with SSL offload
az network application-gateway create \
  --name "AdatumAppGateway3" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG2" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --cert-file /home/azureuser/self-signed/AdatumAppGatewayCert.pfx \
  --cert-password P@ssw0rd \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet" \
  --subnet-address-prefix "10.0.0.0/28" \
  --frontend-port 443 \
  --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 \
  --sku "Standard_Small" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip" \
  --public-ip-address-allocation "dynamic"
```

## <a name="get-an-application-gateway-dns-name"></a>Pobierz nazwę DNS bramy aplikacji

Po utworzeniu bramy, następnym krokiem jest skonfigurowanie frontonu dla komunikacji.  Brama aplikacji wymaga przypisywany dynamicznie nazwy DNS, korzystając z publicznego adresu IP, który nie jest przyjazną. Aby zapewnić, że użytkownicy końcowi mogą trafień bramy aplikacji, umożliwia rekord CNAME wskaż publiczny punkt końcowy bramy aplikacji. Aby uzyskać więcej informacji, zobacz [Konfigurowanie niestandardowej nazwy domeny dla platformy Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Aby skonfigurować alias, pobrać szczegółów bramy aplikacji i jego skojarzony IP DNS nazwy przy użyciu **publicznego adresu IP** element dołączony na bramie aplikacji. Użyj nazwy DNS bramy aplikacji, aby utworzyć rekord CNAME, wskazujący aplikacji dwie sieci web do tej nazwy DNS. Nie zaleca się użycie rekordów A, ponieważ adres VIP, można zmienić na ponownego uruchomienia bramy aplikacji.


```azurecli-interactive
az network public-ip show --name "pip" --resource-group "AdatumAppGatewayRG"
```

```
{
  "dnsSettings": {
    "domainNameLabel": null,
    "fqdn": "8c786058-96d4-4f3e-bb41-660860ceae4c.cloudapp.net",
    "reverseFqdn": null
  },
  "etag": "W/\"3b0ac031-01f0-4860-b572-e3c25e0c57ad\"",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/publicIPAddresses/pip2",
  "idleTimeoutInMinutes": 4,
  "ipAddress": "40.121.167.250",
  "ipConfiguration": {
    "etag": null,
    "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/AdatumAppGatewayRG/providers/Microsoft.Network/applicationGateways/AdatumAppGateway2/frontendIPConfigurations/appGatewayFrontendIP",
    "name": null,
    "privateIpAddress": null,
    "privateIpAllocationMethod": null,
    "provisioningState": null,
    "publicIpAddress": null,
    "resourceGroup": "AdatumAppGatewayRG",
    "subnet": null
  },
  "location": "eastus",
  "name": "pip2",
  "provisioningState": "Succeeded",
  "publicIpAddressVersion": "IPv4",
  "publicIpAllocationMethod": "Dynamic",
  "resourceGroup": "AdatumAppGatewayRG",
  "resourceGuid": "3c30d310-c543-4e9d-9c72-bbacd7fe9b05",
  "tags": {
    "cli[2] owner[administrator]": ""
  },
  "type": "Microsoft.Network/publicIPAddresses"
}
```

## <a name="next-steps"></a>Następne kroki

Jeśli chcesz skonfigurować bramę aplikacji za pomocą wewnętrznego modułu równoważenia obciążenia, zobacz [Utwórz bramę aplikacji z wewnętrznego modułu równoważenia obciążenia](application-gateway-ilb.md).

Aby uzyskać więcej informacji o opcjach były ogólnie równoważenia obciążenia Zobacz:

* [Azure Load Balancer](https://azure.microsoft.com/documentation/services/load-balancer/)
* [Azure Traffic Manager](https://azure.microsoft.com/documentation/services/traffic-manager/)
