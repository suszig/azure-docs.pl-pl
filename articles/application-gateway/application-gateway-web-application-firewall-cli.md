---
title: "Konfigurowanie zapory aplikacji sieci web: Brama aplikacji w usłudze Azure | Dokumentacja firmy Microsoft"
description: "Ten artykuł zawiera wskazówki dotyczące sposobu uruchamiania za pomocą zapory aplikacji sieci web w istniejącej lub nowej bramy aplikacji."
documentationcenter: na
services: application-gateway
author: davidmu1
manager: timlt
editor: tysonn
ms.assetid: 670b9732-874b-43e6-843b-d2585c160982
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/20/2017
ms.author: davidmu
ms.openlocfilehash: c9c740a3a1a28a1a9a4f2abf579fe2adb54e4f47
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="configure-a-web-application-firewall-on-a-new-or-existing-application-gateway-with-azure-cli"></a>Konfigurowanie zapory aplikacji sieci web w nowej lub istniejącej aplikacji bramy przy użyciu wiersza polecenia platformy Azure

> [!div class="op_single_selector"]
> * [Witryna Azure Portal](application-gateway-web-application-firewall-portal.md)
> * [PowerShell](application-gateway-web-application-firewall-powershell.md)
> * [Interfejs wiersza polecenia platformy Azure](application-gateway-web-application-firewall-cli.md)

Dowiedz się, jak utworzyć zapory aplikacji sieci web (WAF)-włączone bramy aplikacji. Również informacje o sposobie dodawanie zapory aplikacji sieci Web do istniejącej bramy aplikacji.

Zapory aplikacji sieci Web w brama aplikacji w usłudze Azure chroni aplikacje sieci web przed wspólnej ataków opartych na sieci web takich jak iniekcja kodu SQL, ataki skryptów między witrynami i hijacks sesji.

 Brama aplikacji jest modułem równoważenia obciążenia warstwy 7. Zapewnia on trybu failover, wydajności routingu żądań HTTP między różnymi serwerami, czy są one w chmurze lub lokalnie. Brama aplikacji w oferują wiele funkcji kontrolera (ADC) dostarczania aplikacji:

 * Równoważenie obciążenia HTTP 
 * Koligacji na podstawie plików cookie sesji 
 * Secure Sockets Layer (SSL) odciążania 
 * Sondy kondycji niestandardowych 
 * Obsługa funkcji wielooddziałowości
 
 Aby uzyskać pełną listę obsługiwanych funkcji, zobacz [brama Omówienie aplikacji](application-gateway-introduction.md).

W tym artykule przedstawiono sposób [Dodawanie zapory aplikacji sieci web do istniejącej bramy aplikacji](#add-web-application-firewall-to-an-existing-application-gateway). Zawiera także jak [Utwórz bramę aplikacji używającej zapory aplikacji sieci web](#create-an-application-gateway-with-web-application-firewall).

![Scenariusz obrazu][scenario]

## <a name="prerequisite-install-the-azure-cli-20"></a>Wymagania wstępne: Instalacja Azure CLI 2.0

Aby wykonać kroki opisane w tym artykule, należy [zainstalować interfejs wiersza polecenia platformy Azure (Azure CLI) dla komputerów Mac, Linux i Windows](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

## <a name="waf-configuration-differences"></a>Różnice w konfiguracji zapory aplikacji sieci Web

Jeśli zostały przeczytane [Utwórz bramę aplikacji z wiersza polecenia platformy Azure](application-gateway-create-gateway-cli.md), zrozumieć ustawienia jednostki SKU można skonfigurować podczas tworzenia bramy aplikacji. Zapory aplikacji sieci Web udostępnia dodatkowe ustawienia, aby zdefiniować podczas konfigurowania jednostka SKU bramy aplikacji. Brak dodatkowych zmian wprowadzonych na bramy aplikacji.

| **Ustawienie** | **Szczegóły**
|---|---|
|**SKU** |Bramy normalne aplikacji bez zapory aplikacji sieci Web obsługuje **standardowe\_małych**, **standardowe\_średni**, i **standardowe\_duży**rozmiary. Wraz z wprowadzeniem zapory aplikacji sieci Web, istnieją dwie dodatkowe jednostki SKU, **WAF\_średni** i **zapory aplikacji sieci Web\_duży**. Zapory aplikacji sieci Web nie jest obsługiwana w bramach małych aplikacji.|
|**Tryb** | To ustawienie jest tryb zapory aplikacji sieci Web. dozwolone wartości to **wykrywania** i **zapobiegania**. Gdy zapory aplikacji sieci Web jest ustawiany w **wykrywania** trybie wszystkie zagrożenia są przechowywane w pliku dziennika. W **zapobiegania** tryb, zdarzenia są nadal rejestrowane, ale osoba atakująca odbiera 403 nieautoryzowany odpowiedzi z bramy aplikacji.|

## <a name="add-a-web-application-firewall-to-an-existing-application-gateway"></a>Dodawanie zapory aplikacji sieci web do istniejącej bramy aplikacji

Polecenie zmiany istniejącą bramę standardowej aplikacji do bramy aplikacji z obsługą zapory aplikacji sieci Web:

```azurecli-interactive
#!/bin/bash

az network application-gateway waf-config set \
  --enabled true \
  --firewall-mode Prevention \
  --gateway-name "AdatumAppGateway" \
  --resource-group "AdatumAppGatewayRG"
```

To polecenie aktualizuje bramy aplikacji zapory aplikacji sieci Web. Aby poznać sposób wyświetlania dzienniki bramy aplikacji, zobacz [diagnostyki bramy aplikacji](application-gateway-diagnostics.md). Z powodu zabezpieczeń rodzaj zapory aplikacji sieci Web Przejrzyj dzienniki regularnie, aby poznać strukturę aplikacji sieci web.

## <a name="create-an-application-gateway-with-a-web-application-firewall"></a>Utwórz bramę aplikacji za pomocą zapory aplikacji sieci web

Poniższe polecenie tworzy bramę aplikacji z zapory aplikacji sieci Web:

```azurecli-interactive
#!/bin/bash

az network application-gateway create \
  --name "AdatumAppGateway2" \
  --location "eastus" \
  --resource-group "AdatumAppGatewayRG" \
  --vnet-name "AdatumAppGatewayVNET2" \
  --vnet-address-prefix "10.0.0.0/16" \
  --subnet "Appgatewaysubnet2" \
  --subnet-address-prefix "10.0.0.0/28" \
 --servers "10.0.0.5 10.0.0.4" \
  --capacity 2 
  --sku "WAF_Medium" \
  --http-settings-cookie-based-affinity "Enabled" \
  --http-settings-protocol "Http" \
  --frontend-port "80" \
  --routing-rule-type "Basic" \
  --http-settings-port "80" \
  --public-ip-address "pip2" \
  --public-ip-address-allocation "dynamic" \
  --tags "cli[2] owner[administrator]"
```

> [!NOTE]
> Bramy aplikacji utworzonych za pomocą podstawową konfigurację zapory aplikacji sieci Web są skonfigurowane z CRS 3.0 do ochrony.

## <a name="get-an-application-gateway-dns-name"></a>Pobierz nazwę DNS bramy aplikacji

Po utworzeniu bramy, następnym krokiem jest skonfigurowanie frontonu dla komunikacji. Gdy używasz publicznego adresu IP bramy aplikacji wymaga przypisywany dynamicznie nazwy DNS, który nie jest przyjazną. Aby upewnić się, że użytkownicy mogą trafień bramy aplikacji, wskaż publiczny punkt końcowy bramy aplikacji przy użyciu rekordu CNAME. Aby uzyskać więcej informacji, zobacz [Konfigurowanie niestandardowej nazwy domeny dla usługi w chmurze Azure](../cloud-services/cloud-services-custom-domain-name-portal.md). 

Aby skonfigurować rekord CNAME, należy pobrać szczegółów bramy aplikacji i skojarzonej z nią nazwy IP DNS przy użyciu elementu publicznego adresu IP dołączony na bramie aplikacji. Użyj nazwy DNS bramy aplikacji, aby utworzyć rekord CNAME, wskazujący aplikacji dwie sieci web do tej nazwy DNS. Nie zaleca się przy użyciu rekordów, ponieważ adres VIP może ulec zmianie po ponownym uruchomieniu bramy aplikacji.

```azurecli-interactive
#!/bin/bash

az network public-ip show \
  --name pip2 \
  --resource-group "AdatumAppGatewayRG"
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

Aby dowiedzieć się, jak dostosować reguły zapory aplikacji sieci Web, zobacz [dostosować reguły zapory aplikacji sieci web za pośrednictwem 2.0 interfejsu wiersza polecenia Azure](application-gateway-customize-waf-rules-cli.md).

[scenario]: ./media/application-gateway-web-application-firewall-cli/scenario.png
