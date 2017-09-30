---
title: "Konfigurowanie punktów końcowych usługi sieci wirtualnej platformy Azure | Microsoft Docs"
description: "Dowiedz się, jak włączać i wyłączać punkty końcowe usługi z poziomu sieci wirtualnej"
services: virtual-network
documentationcenter: na
author: anithaa
manager: narayan
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/15/2017
ms.author: anithaa
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 7dceb7bb38b1dac778151e197db3b5be49dd568a
ms.openlocfilehash: 9c953e697d5d1c6e53df8eaf72c4e4fbd83ca286
ms.contentlocale: pl-pl
ms.lasthandoff: 09/25/2017

---
# <a name="configure-virtual-network-service-endpoints"></a>Konfigurowanie punktów końcowych usługi sieci wirtualnej

Punkty końcowe usługi sieci wirtualnej umożliwiają zabezpieczanie zasobów usługi platformy Azure w sieci wirtualnej Azure Virtual Network przez całkowite usunięcie dostępu do Internetu dla tych zasobów. Punkty końcowe usługi zapewniają bezpośrednie połączenie z sieci wirtualnej do usługi platformy Azure, co pozwala na wykorzystanie prywatnej przestrzeni adresowej sieci wirtualnej do uzyskiwania dostępu do usług platformy Azure. Ruch kierowany do usług platformy Azure za pośrednictwem punktów końcowych usługi zawsze pozostaje w sieci szkieletowej platformy Microsoft Azure. Dowiedz się więcej na temat [punktów końcowych usługi sieci wirtualnej](virtual-network-service-endpoints-overview.md)

W tym artykule przedstawiono kroki włączania i wyłączania punktów końcowych usługi. Po włączeniu punktów końcowych w podsieci do usługi platformy Azure można zabezpieczyć określone zasoby usługi w sieci wirtualnej.

Punkty końcowe usługi można konfigurować za pomocą [witryny Azure Portal](#azure-portal), [programu Azure PowerShell](#azure-powershell), [interfejsu wiersza polecenia platformy Azure](#azure-cli) lub [szablonu](#resource-manager-template) usługi Azure Resource Manager.

>[!NOTE]
W okresie obowiązywania wersji zapoznawczej funkcja punktów końcowych usługi sieci wirtualnej jest obsługiwana w określonych regionach. Aby uzyskać listę obsługiwanych regionów, zapoznaj się ze stroną [aktualizacji usługi Azure Virtual Network](https://azure.microsoft.com/updates/?product=virtual-network).

## <a name="service-endpoint-configuration-overview"></a>Omówienie konfiguracji punktu końcowego usługi

- Punkty końcowe usługi można konfigurować tylko w przypadku sieci wirtualnych wdrażanych za pośrednictwem modelu wdrażania usługi Azure Resource Manager.

- Punkty końcowe usługi są ustawiane w każdej podsieci sieci wirtualnej.

- W przypadku podsieci można skonfigurować tylko jeden punkt końcowy usługi dla usługi. Można skonfigurować wiele punktów końcowych usługi dla różnych usług (np. Azure Storage, Azure SQL).

- Punkty końcowe można włączyć w nowej lub istniejącej podsieci.

- Lokalizacja punktu końcowego jest konfigurowana automatycznie. Domyślnie punkty końcowe usługi są konfigurowane w regionie sieci wirtualnej. Aby obsługiwać regionalne scenariusze pracy w trybie failover w usłudze Azure Storage, punkty końcowe są automatycznie konfigurowane do [sparowanych regionów platformy Azure](https://docs.microsoft.com/azure/best-practices-availability-paired-regions#what-are-paired-regions).

  >[!NOTE]
  W zależności od rozmiaru sieci wirtualnej/podsieci włączanie punktu końcowego usługi może potrwać pewien czas. Upewnij się, że żadne krytyczne zadania nie są wykonywane podczas włączania punktów końcowych usługi. Punkty końcowe usługi przełączają trasy na każdej karcie sieciowej w podsieci — może to spowodować zakończenie dowolnych otwartych połączeń TCP. 

- Wywołanie punktu końcowego usługi zwraca stan „powodzenie” po przełączeniu przepływów ruchu do usługi na wszystkich kartach sieciowych w podsieci na prywatne adresy IP sieci wirtualnej.

- Skuteczne trasy weryfikowania konfiguracji punktu końcowego:

   Aby zweryfikować, czy punkt końcowy usługi został prawidłowo skonfigurowany, pozycja „skuteczne trasy” na dowolnej karcie sieciowej w podsieci przedstawia nową trasę „domyślną" nextHopType: VirtualNetworkServiceEndpoint dla każdej usługi w każdym regionie. Dowiedz się więcej na temat [rozwiązywania problemów ze skutecznymi trasami](https://docs.microsoft.com/azure/virtual-network/virtual-network-routes-troubleshoot-portal#using-effective-routes-to-troubleshoot-vm-traffic-flow)

   >[!NOTE]
   Skuteczne trasy można przeglądać tylko, jeśli co najmniej jedna karta sieciowa zostanie skonfigurowana i skojarzona z działającą maszyną wirtualną w podsieci.

## <a name="azure-portal"></a>Azure Portal

### <a name="setting-up-service-endpoint-on-a-subnet-during-vnet-create"></a>Konfigurowanie punktu końcowego usługi w podsieci podczas tworzenia sieci wirtualnej

1. Otwórz witrynę [Azure Portal](https://portal.azure.com/).
Zaloguj się do platformy Azure przy użyciu konta Azure. Jeśli nie masz konta platformy Azure, możesz skorzystać z bezpłatnej wersji próbnej. Konto musi mieć niezbędne [uprawnienia](#provisioning) do tworzenia sieci wirtualnej i punktu końcowego usługi.
2. Kliknij kolejno pozycje +Nowy > Sieć > Sieć wirtualna > +Dodaj.
3. W oknie „Tworzenie bloku sieci wirtualnej” wprowadź poniższe wartości, a następnie kliknij pozycję Utwórz:

Ustawienie | Wartość
------- | -----
Nazwa    | myVnet
Przestrzeń adresowa | 10.0.0.0/16
Nazwa podsieci|mySubnet
Zakres adresów podsieci|10.0.0.0/24
Grupa zasobów|Pozostaw pole Utwórz nowy zaznaczone, a następnie wprowadź nazwę.
Lokalizacja|Dowolny obsługiwany region, np. Australia Wschodnia
Subskrypcja|Wybierz subskrypcję.
__ServiceEndpoints__|Enabled (Włączony)
__Usługi__ | Wybierz jedną lub wszystkie dostępne usługi. Usługi obsługiwane w okresie obowiązywania wersji zapoznawczej: __„Microsoft.Storage”, „Microsoft.Sql”__.

Wybierz usługi dla punktów końcowych: ![Wybieranie usług punktu końcowego usługi](media/virtual-network-service-endpoints-portal/vnet-create-flow-services.png)

4. Sprawdź, czy wszystkie ustawienia są poprawne, i kliknij pozycję „Utwórz”.

![Ustawianie punktu końcowego usługi](media/virtual-network-service-endpoints-portal/create-vnet-flow.png)

5. Aby zakończyć zabezpieczanie zasobów usługi platformy Azure w sieci wirtualnej, kliknij dokumentację usługi w części [Następne kroki](#Next%20Steps).


### <a name="validating-service-endpoint-configuration"></a>Weryfikowanie konfiguracji punktu końcowego usługi

Potwierdź, że punkty końcowe zostały skonfigurowane, wykonując poniższe czynności:

- W obszarze zasobów kliknij pozycję „Sieci wirtualne”. Wyszukaj sieć wirtualną.
- Kliknij nazwę sieci wirtualnej i przejdź do obszaru „Punkty końcowe usługi”
- Skonfigurowane punkty końcowe są wyświetlane ze stanem „Powodzenie”. Widoczne są również lokalizacje skonfigurowane automatycznie

![Potwierdzanie konfiguracji punktu końcowego usługi](media/virtual-network-service-endpoints-portal/vnet-validate-settings.png
)

### <a name="effective-routes-to-validate-endpoint-configuration"></a>Skuteczne trasy na potrzeby weryfikowania konfiguracji punktu końcowego

Aby wyświetlić skuteczną trasę dla interfejsu sieciowego w podsieci, kliknij dowolną kartę sieciową w tej podsieci. W obszarze „Pomoc techniczna i rozwiązywanie problemów” kliknij pozycję „Skuteczne trasy”. Jeśli punkt końcowy został skonfigurowany, zobaczysz nową trasę „domyślną” z prefiksami adresów usługi jako miejscem docelowym i elementem nextHopType ustawionym na „VirtualNetworkServiceEndpoint”.

![Skuteczne trasy dla punktów końcowych usługi](media/virtual-network-service-endpoints-portal/effective-routes.png)

### <a name="setting-up-service-endpoints-for-existing-subnets-in-a-vnet"></a>Konfigurowanie punktów końcowych usługi dla podsieci istniejących w sieci wirtualnej

1. W obszarze zasobów kliknij pozycję „Sieci wirtualne” i wyszukaj wszystkie istniejące sieci wirtualne
2. Kliknij nazwę sieci wirtualnej i przejdź do obszaru „Punkty końcowe usługi”
3. Kliknij pozycję „Dodaj”. Wybierz pozycję „Usługa”. W danym momencie możesz utworzyć punkt końcowy tylko jednej usługi. 
4. Wybierz wszystkie podsieci, w których chcesz zastosować punkt końcowy. Kliknij pozycję „Dodaj”

![Konfiguracja punktu końcowego usługi podsieci](media/virtual-network-service-endpoints-portal/existing-subnet.png)

### <a name="deleting-service-endpoints"></a>Usuwanie punktów końcowych usługi
1. W obszarze zasobów kliknij pozycję „Sieci wirtualne”. Wyszukaj istniejącą sieć wirtualną za pomocą filtrowania według nazwy sieci wirtualnej.
2. Kliknij nazwę sieci wirtualnej i przejdź do obszaru „Punkty końcowe usługi”
3. Kliknij nazwę usługi i kliknij prawym przyciskiem myszy wpis punktu końcowego usługi
4. Wybierz pozycję „Usuń”

![Usuwanie punktu końcowego usługi](media/virtual-network-service-endpoints-portal/delete-endpoint.png)

## <a name="azure-powershell"></a>Azure PowerShell

Wymagania wstępne konfiguracji:

- Zainstaluj najnowszą wersję modułu [AzureRm](https://www.powershellgallery.com/packages/AzureRM/) programu PowerShell. Jeśli jesteś nowym użytkownikiem programu Azure PowerShell, zobacz temat [Azure PowerShell overview (Omówienie programu Azure PowerShell)](/powershell/azure/overview?toc=%2fazure%2fvirtual-network%2ftoc.json).
- Aby rozpocząć sesję programu PowerShell, przejdź do pozycji **Start**, wprowadź ciąg **powershell**, a następnie kliknij pozycję **PowerShell**.
- W programie PowerShell zaloguj się do platformy Azure, wprowadzając polecenie `login-azurermaccount`. Konto musi mieć niezbędne [uprawnienia](#provisioning) do tworzenia sieci wirtualnej i punktu końcowego usługi.

### <a name="get-available-service-endpoints-for-azure-region"></a>Pobieranie dostępnych punktów końcowych usługi dla regionu platformy Azure

Użyj poniższego polecenia, aby pobrać listę usług obsługiwanych dla punktów końcowych w wybranym regionie platformy Azure.
 ```powershell
Get-AzureRmVirtualNetworkAvailableEndpointService -location eastus
```

Dane wyjściowe: 
Nazwa | ID | Typ
-----|----|-------
Microsoft.Storage|/subscriptions/xxxx-xxx-xxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Storage|Microsoft.Network/virtualNetworkEndpointServices
Microsoft.Sql|/subscriptions/xxxx-xxx-xxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Sql|Microsoft.Network/virtualNetworkEndpointServices

### <a name="add-azure-storage-service-endpoint-to-a-subnet-mysubnet-while-creating-the-virtual-network-myvnet"></a>Dodawanie punktu końcowego usługi Azure Storage do podsieci *mySubnet* podczas tworzenia sieci wirtualnej *myVNet*

```powershell
$subnet = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet" -AddressPrefix "10.0.1.0/24" -ServiceEndpoint “Microsoft.Storage”

New-AzureRmVirtualNetwork -Name "myVNet" -AddressPrefix "10.0.0.0/16" -Subnet $subnet -ResourceGroupName "myRG" -Location "WestUS"
```
Aby włączyć wiele usług, można skorzystać z rozdzielanej przecinkami listy nazw usług.
Przykład: „Microsoft.Storage”, „Microsoft.Sql”

Oczekiwane dane wyjściowe:
```
Subnets : [
            {
            "Name": "mySubnet",
             ...
            "ServiceEndpoints": [
              {
                   "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Storage",
                    "Locations": [
                        "westus",
                        "eastus"
                                 ]
               }
                                ],
            "ProvisioningState": "Succeeded"
            }
          ]
```

Aby zakończyć zabezpieczanie zasobów usługi platformy Azure w sieci wirtualnej, kliknij dokumentację usługi w części [Następne kroki](#Next%20Steps).

### <a name="add-multiple-service-endpoints-to-an-existing-subnet"></a>Dodawanie wielu punktów końcowych usługi do istniejącej podsieci

```powershell
Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"  -AddressPrefix "10.0.1.0/24" -ServiceEndpoint "Microsoft.Storage", "Microsoft.Sql" | Set-AzureRmVirtualNetwork
```

Oczekiwane dane wyjściowe: 
```
Subnets : [
            {
                "Name": "mySubnet",
                 ...
                "ServiceEndpoints": [
                {
                    "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Storage",
                    "Locations": [
                        "eastus",
                        "westus"
                                 ]
                },
                {
                    "ProvisioningState": "Succeeded",
                    "Service": "Microsoft.Sql",
                    "Locations": [
                        "eastus"
                                 ]
                }
                ],
               "ProvisioningState": "Succeeded"
            }
         ]
```

### <a name="view-service-endpoints-configured-on-a-subnet"></a>Wyświetlanie punktów końcowych usługi skonfigurowanych w podsieci

```powershell
$subnet=Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Get-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"
$subnet.ServiceEndpoints
```
Dane wyjściowe:
```
ProvisioningState Service           Locations
----------------- -------           ---------
Succeeded         Microsoft.Storage {eastus, westus}
Succeeded         Microsoft.Sql     {eastus}
```

### <a name="delete-service-endpoints-on-a-subnet"></a>Usuwanie punktów końcowych usługi w podsieci
```powershell
Get-AzureRmVirtualNetwork -ResourceGroupName "myRG" -Name "myVNet" | Set-AzureRmVirtualNetworkSubnetConfig -Name "mySubnet"  -AddressPrefix "10.0.1.0/24" -ServiceEndpoint $null | Set-AzureRmVirtualNetwork
```

## <a name="azure-cli"></a>Interfejs wiersza polecenia platformy Azure

Wymagania wstępne konfiguracji:
- Zaloguj się do subskrypcji platformy Azure za pomocą polecenia [az login](/cli/azure/#login) i postępuj zgodnie z instrukcjami wyświetlanymi na ekranie. Aby uzyskać więcej informacji na temat logowania się, zobacz artykuł [Rozpoczynanie pracy z interfejsem wiersza polecenia platformy Azure 2.0](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
 - Konto musi mieć niezbędne [uprawnienia](#provisioning) do tworzenia sieci wirtualnej i punktu końcowego usługi.

 Aby uzyskać pełną listę poleceń dla sieci wirtualnych, zobacz [Azure CLI Virtual Network commands (Polecenia interfejsu wiersza polecenia platformy Azure dla usługi Virtual Network)](https://docs.microsoft.com/cli/azure/network/vnet?view=azure-cli-latest)

### <a name="get-available-service-endpoints-for-azure-region"></a>Pobieranie dostępnych punktów końcowych usługi dla regionu platformy Azure

Użyj poniższego polecenia, aby pobrać listę usług obsługiwanych dla punktów końcowych w wybranym regionie platformy Azure, np. „EastUS”.
```azure-cli
az network vnet list-endpoint-services -l eastus
```
Dane wyjściowe:
```
    {
    "id": "/subscriptions/xxxx-xxxx-xxxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Storage",
    "name": "Microsoft.Storage",
    "type": "Microsoft.Network/virtualNetworkEndpointServices"
     },
     {
     "id": "/subscriptions/xxxx-xxxx-xxxx/providers/Microsoft.Network/virtualNetworkEndpointServices/Microsoft.Sql",
     "name": "Microsoft.Sql",
     "type":   "Microsoft.Network/virtualNetworkEndpointServices"
     }
```

### <a name="add-azure-storage-service-endpoint-to-a-subnet-mysubnet-while-creating-the-virtual-network-myvnet"></a>Dodawanie punktu końcowego usługi Azure Storage do podsieci *mySubnet* podczas tworzenia sieci wirtualnej *myVNet*

```azure-cli
az network vnet create -g myRG -n myVNet --address-prefixes 10.0.0.0/16 -l eastUS

az network vnet subnet create -g myRG -n mySubnet --vnet-name myVNet --address-prefix 10.0.1.0/24 --service-endpoints Microsoft.Storage
```

Aby dodać wiele punktów końcowych: --service-endpoints Microsoft.Storage Microsoft.Sql

Dane wyjściowe:
```
{
  "addressPrefix": "10.0.1.0/24",
  ...
  "name": "mySubnet",
  "networkSecurityGroup": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myRG",
  "resourceNavigationLinks": null,
  "routeTable": null,
  "serviceEndpoints": [
    {
      "locations": [
        "eastus",
        "westus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Storage"
    }
  ]
}
```

Aby zakończyć zabezpieczanie zasobów usługi platformy Azure w sieci wirtualnej, kliknij dokumentację usługi w części [Następne kroki](#Next%20Steps).

### <a name="add-multiple-service-endpoints-to-an-existing-subnet"></a>Dodawanie wielu punktów końcowych usługi do istniejącej podsieci

```azure-cli
az network vnet subnet update -g myRG -n mySubnet2 --vnet-name myVNet --service-endpoints Microsoft.Storage Microsoft.Sql
```

Oczekiwane dane wyjściowe:
```
{
  "addressPrefix": "10.0.2.0/24",
  ...
  "name": "mySubnet2",
  ...
  "serviceEndpoints": [
    {
      "locations": [
        "eastus",
        "westus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Storage"
    },
    {
      "locations": [
        "eastus"
      ],
      "provisioningState": "Succeeded",
      "service": "Microsoft.Sql"
    }
  ]
}
```

### <a name="view-service-endpoints-configured-on-a-subnet"></a>Wyświetlanie punktów końcowych usługi skonfigurowanych w podsieci

```azure-cli
az network vnet subnet show -g myRG -n mySubnet --vnet-name myVNet
```

### <a name="delete-service-endpoints-on-a-subnet"></a>Usuwanie punktów końcowych usługi w podsieci
```azure-cli
az network vnet subnet update -g myRG -n mySubnet --vnet-name myVNet --service-endpoints ""
```

Dane wyjściowe: 
```
{
  "addressPrefix": "10.0.1.0/24",
  ...
  "name": "mySubnet",
  "networkSecurityGroup": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "myRG",
  "resourceNavigationLinks": null,
  "routeTable": null,
  "serviceEndpoints": null
}
```

## <a name="resource-manager-template"></a>Szablon usługi Resource Manager

### <a name="securing-azure-service-resources-to-vnets"></a>Zabezpieczanie zasobów usługi platformy Azure w sieciach wirtualnych

Określone zasoby platformy Azure można zabezpieczyć w sieci wirtualnej za pomocą punktów końcowych usługi.

Pobierz przykład [szablonu usługi Resource Manager](https://azure.microsoft.com/resources/templates/201-vnet-2subnets-service-endpoints-storage-integration) w celu zabezpieczenia konta magazynu do podsieci w sieci wirtualnej.

Szablon umożliwia tworzenie sieci wirtualnej z 2 podsieciami i maszyną wirtualną z jedną kartą sieciową w każdej podsieci. Włącza on punkt końcowy w jednej podsieci i zabezpiecza konto magazynu w tej podsieci.

Możesz pobrać szablon i zmodyfikować jego części, aby pasował do danego scenariusza.

Szablon jest dostarczany wraz z instrukcjami wdrażania przy użyciu witryny Azure Portal, programu PowerShell lub wiersza polecenia platformy Azure. Upewnij się, masz wymagane [uprawnienia](#provisioning) do konfigurowania punktu końcowego i zabezpieczania konta.

Aby zabezpieczyć zasoby platformy Azure w podsieci:

- należy skonfigurować punkt końcowy usługi w tej podsieci.
- należy zabezpieczyć zasób w sieci wirtualnej przez dodanie reguły sieci wirtualnej w zasobie.

### <a name="deleting-service-endpoints-with-resources-secured-to-the-subnet"></a>Usuwanie punktów końcowych usługi za pomocą zasobów zabezpieczonych w podsieci
Jeśli zasoby usługi Azure zostaną zabezpieczone w podsieci, a punkt końcowy usługi usunięty, nie będziesz już mieć dostępu do zasobu z podsieci.
Ponowne włączenie tylko punktu końcowego nie spowoduje przywrócenia dostępu do zasobów zabezpieczonych wcześniej w podsieci.

Aby ponownie zabezpieczyć zasób usługi w tej podsieci, należy:

- ponownie włączyć punkt końcowy
- usunąć starą regułę sieci wirtualnej dotyczącą zasobu
- dodać nową regułę zabezpieczającą zasoby do podsieci

## <a name="provisioning"></a>Inicjowanie obsługi

Punkty końcowe usługi można niezależnie konfigurować w sieciach wirtualnych — czynność tę może wykonać użytkownik z uprawnieniami do zapisu w sieci wirtualnej.

Aby zabezpieczyć zasoby usługi platformy Azure w sieci wirtualnej, użytkownik musi mieć uprawnienia do elementu „Microsoft.Network/JoinServicetoaSubnet” dla dodawanych podsieci. To uprawnienie jest domyślnie uwzględniane we wbudowanych rolach administratora usługi, domyślnie i może być modyfikowane przez tworzenie ról niestandardowych.

Dowiedz się więcej na temat [wbudowanych ról](https://docs.microsoft.com/azure/active-directory/role-based-access-built-in-roles) i przypisywaniu określonych uprawnień do [ról niestandardowych](https://docs.microsoft.com/en-us/azure/active-directory/role-based-access-control-custom-roles).

Sieci wirtualne i zasoby usług platformy Azure mogą należeć do tej samej lub różnych subskrypcji. Jeśli zasoby należą do różnych subskrypcji, w okresie obowiązywania tej wersji zapoznawczej powinny one znajdować się w tej samej dzierżawie usługi Active Directory (AD).

## <a name="next-steps"></a>Następne kroki

Aby uzyskać dodatkowe instrukcje dotyczące zabezpieczania zasobów usługi w sieciach wirtualnych, skorzystaj z poniższych linków:

[Securing Azure Storage accounts to Virtual Networks (Zabezpieczanie kont usługi Azure Storage w sieciach wirtualnych)](https://docs.microsoft.com/azure/storage/common/storage-network-security)

[Securing Azure SQL to Virtual networks (Zabezpieczanie usługi Azure SQL w sieciach wirtualnych)](https://docs.microsoft.com/azure/sql-database/sql-database-vnet-service-endpoint-rule-overview)

