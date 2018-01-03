---
title: "Utwórz bramę aplikacji - Azure CLI 2.0 | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć bramę aplikacji przy użyciu 2.0 interfejsu wiersza polecenia Azure Resource Manager."
services: application-gateway
documentationcenter: na
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c2f6516e-3805-49ac-826e-776b909a9104
ms.service: application-gateway
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/31/2017
ms.author: davidmu
ms.openlocfilehash: beb2dab177d021fee1dbbe630f8b6854a7d94f68
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="create-an-application-gateway-by-using-the-azure-cli-20"></a>Tworzenie bramy aplikacji przy użyciu 2.0 interfejsu wiersza polecenia platformy Azure

> [!div class="op_single_selector"]
> * [Azure portal](application-gateway-create-gateway-portal.md)
> * [Azure Resource Manager — program PowerShell](application-gateway-create-gateway-arm.md)
> * [Program Azure PowerShell klasycznego](application-gateway-create-gateway.md)
> * [Szablon usługi Azure Resource Manager](application-gateway-create-gateway-arm-template.md)
> * [Interfejs wiersza polecenia platformy Azure 1.0](application-gateway-create-gateway-cli.md)
> * [Interfejs wiersza polecenia platformy Azure 2.0](application-gateway-create-gateway-cli.md)

Azure bramy aplikacji jest wirtualne dedykowanych udostępnia kontroler dostarczania aplikacji (ADC) jako usługa, oferty różnych warstwy 7 Równoważenie obciążenia sieciowego funkcji aplikacji.

## <a name="cli-versions"></a>Wersje interfejsu wiersza polecenia

Można utworzyć bramy aplikacji przy użyciu jednej z następujących wersji interfejsu wiersza polecenia (CLI):

* [Azure CLI 1.0](application-gateway-create-gateway-cli-nodejs.md): wiersza polecenia platformy Azure dla klasycznego i modeli wdrażania usługi Azure Resource Manager
* [Azure CLI 2.0](application-gateway-create-gateway-cli.md): Next generation interfejsu wiersza polecenia dla modelu wdrażania usługi Resource Manager

## <a name="prerequisite-install-the-azure-cli-20"></a>Wymagania wstępne: Instalacja Azure CLI 2.0

Aby wykonać kroki opisane w tym artykule, należy [instalowanie interfejsu wiersza polecenia Azure macOS, Linux i Windows](https://docs.microsoft.com/cli/azure/install-az-cli2).

> [!NOTE]
> Potrzebujesz konta platformy Azure, aby utworzyć bramę aplikacji. Jeśli nie masz, zaloguj się do [bezpłatnej wersji próbnej](../active-directory/sign-up-organization.md).

## <a name="scenario"></a>Scenariusz

W tym scenariuszu należy Dowiedz się, jak utworzyć bramę aplikacji przy użyciu portalu Azure.

W tym scenariuszu obejmują:

* Utwórz bramę aplikacji średnia z dwóch wystąpień.
* Tworzenie sieci wirtualnej o nazwie AdatumAppGatewayVNET z zarezerwowanym blokiem CIDR 10.0.0.0/16.
* Tworzenie podsieci o nazwie Appgatewaysubnet używającej bloku 10.0.0.0/28 jako bloku CIDR.

> [!NOTE]
> Dalsza konfiguracja bramy aplikacji, w tym sondy kondycji niestandardowych, adresy w puli zaplecza i dodatkowe reguły odbywa się po utworzeniu bramy aplikacji i nie podczas pierwszego wdrożenia.

## <a name="before-you-begin"></a>Przed rozpoczęciem

Brama aplikacji wymaga jego własnej podsieci. Podczas tworzenia sieci wirtualnej, upewnij się, czy pozostanie wystarczająco dużo przestrzeni adresowej używanej przez wiele podsieci. Po wdrożeniu bramę aplikacji do podsieci, można dodać tylko bramy dodatkowych aplikacji do tej podsieci.

## <a name="sign-in-to-azure"></a>Logowanie do platformy Azure

Otwórz **wiersza polecenia usługi Microsoft Azure** i zaloguj się na:

```azurecli-interactive
az login -u "username"
```

> [!NOTE]
> Można również użyć `az login` bez przełącznika dla nazwy logowania urządzenia, która wymaga wprowadzenie kodu na aka.ms/devicelogin.

Po wprowadzeniu poprzedniego polecenia, otrzymasz kod. Przejdź do https://aka.ms/devicelogin w przeglądarce, aby kontynuować proces logowania.

![cmd przedstawiający urządzenia logowania][1]

W przeglądarce wprowadź otrzymany kod. To przekieruje Cię do strony logowania.

![przeglądarki, aby wprowadzić kod][2]

Wprowadź kod, aby zarejestrować, a następnie zamknij przeglądarkę, aby kontynuować.

![pomyślnie zalogował się][3]

## <a name="create-the-resource-group"></a>Tworzenie grupy zasobów

Przed utworzeniem bramy aplikacji, Utwórz grupę zasobów, które zawierałoby proces. Użyj następującego polecenia:

```azurecli-interactive
az group create --name myresourcegroup --location "eastus"
```

## <a name="create-the-application-gateway"></a>Tworzenie bramy aplikacji

Użyj adresów IP zaplecza na adresy IP serwera zaplecza. Te wartości można prywatnych adresów IP w sieci wirtualnej, publiczne adresy IP lub nazwy FQDN dla serwerów zaplecza. Poniższy przykład tworzy bramę aplikacji z dodatkowe konfiguracje dla ustawień protokołu HTTP, portów i reguł:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers 10.0.0.4 10.0.0.5 \
--capacity 2 \
--sku Standard_Small \
--http-settings-cookie-based-affinity Enabled \
--http-settings-protocol Http \
--frontend-port 80 \
--routing-rule-type Basic \
--http-settings-port 80 \
--public-ip-address "pip2" \
--public-ip-address-allocation "dynamic" \

```

W poprzednim przykładzie pokazano kilka właściwości, które nie są wymagane podczas tworzenia bramy aplikacji. Poniższy przykład kodu tworzy bramę aplikacji z informacjami wymaganymi:

```azurecli-interactive
az network application-gateway create \
--name "AdatumAppGateway" \
--location "eastus" \
--resource-group "myresourcegroup" \
--vnet-name "AdatumAppGatewayVNET" \
--vnet-address-prefix "10.0.0.0/16" \
--subnet "Appgatewaysubnet" \
--subnet-address-prefix "10.0.0.0/28" \
--servers "10.0.0.5"  \
--public-ip-address pip
```
 
> [!NOTE]
> Aby uzyskać listę parametrów do użycia podczas tworzenia, uruchom następujące polecenie: `az network application-gateway create --help`.

W tym przykładzie tworzy bramę aplikacji w warstwie podstawowa z domyślnych ustawień odbiornika, puli zaplecza, ustawienia HTTP zaplecza i zasady. Można zmodyfikować te ustawienia do wdrożenia po udostępnianie zakończy się pomyślnie.

Jeśli aplikacja sieci web został zdefiniowany z puli zaplecza w poprzednich krokach, równoważenie obciążenia sieciowego rozpocznie się teraz.

## <a name="get-the-application-gateway-dns-name"></a>Pobierz nazwę DNS bramy aplikacji
Po utworzeniu bramy, następnie należy skonfigurować frontonu dla komunikacji. Gdy używasz publicznego adresu IP bramy aplikacji wymaga przypisywany dynamicznie nazwy DNS, który nie jest przyjazną. Zapewnienie, że użytkownicy mogą trafień bramy aplikacji, wskaż publiczny punkt końcowy bramy aplikacji przy użyciu rekordu CNAME. Aby uzyskać więcej informacji, zobacz [użycia usługi Azure DNS, aby określić ustawienia domeny niestandardowej dla usługi Azure](../dns/dns-custom-domain.md).

Aby skonfigurować alias, należy pobrać szczegółów bramy aplikacji i skojarzonej z nią nazwy IP DNS przy użyciu elementu publicznego adresu IP dołączony na bramie aplikacji. Użyj nazwy DNS bramy aplikacji, aby utworzyć rekord CNAME, wskazujący aplikacji dwie sieci web do tej nazwy DNS. Nie korzystać z rekordów, ponieważ adres VIP mogą ulec zmianie sprawie ponownego uruchomienia bramy aplikacji.


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

## <a name="delete-all-resources"></a>Usuwanie wszystkich zasobów

Aby usunąć wszystkie zasoby utworzone w tym artykule, uruchom następujące polecenie:

```azurecli-interactive
az group delete --name AdatumAppGatewayRG
```
 
## <a name="next-steps"></a>Kolejne kroki

Aby dowiedzieć się, jak utworzyć sondy kondycji niestandardowych, przejdź do [Tworzenie niestandardowych sondowania bramy aplikacji przy użyciu portalu](application-gateway-create-probe-portal.md).

Aby dowiedzieć się, jak skonfigurować odciążanie protokołu SSL i podejmij kosztowne odszyfrowywania SSL poza serwerów sieci web, zobacz [skonfigurować bramę aplikacji dla odciążania protokołu SSL przy użyciu usługi Azure Resource Manager](application-gateway-ssl-arm.md).

<!--Image references-->

[scenario]: ./media/application-gateway-create-gateway-cli/scenario.png
[1]: ./media/application-gateway-create-gateway-cli/figure1.png
[2]: ./media/application-gateway-create-gateway-cli/figure2.png
[3]: ./media/application-gateway-create-gateway-cli/figure3.png
