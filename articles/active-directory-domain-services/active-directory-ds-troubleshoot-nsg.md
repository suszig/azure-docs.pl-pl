---
title: "Usługi Azure Active Directory Domain Services: Konfiguracja Rozwiązywanie problemów z sieciowej grupy zabezpieczeń | Dokumentacja firmy Microsoft"
description: "Rozwiązywania problemów z konfiguracją NSG dla usług domenowych Azure AD"
services: active-directory-ds
documentationcenter: 
author: eringreenlee
manager: 
editor: 
ms.assetid: 95f970a7-5867-4108-a87e-471fa0910b8c
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2018
ms.author: ergreenl
ms.openlocfilehash: 447f9119ea379e278be77d8699c7dcb751ea3085
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-network-security-group-configuration"></a>Usługi domenowe AD Azure - konfiguracji Rozwiązywanie problemów z grupy zabezpieczeń sieci



## <a name="aadds104-network-error"></a>AADDS104: Błąd sieci

**Komunikat alertu:** *Microsoft nie będzie mógł łączność z kontrolerami domeny dla tej domeny zarządzanej. Może się tak zdarzyć, jeśli grupa zabezpieczeń sieci (NSG) skonfigurowany na Twojej sieci wirtualnej blokuje dostęp do domeny zarządzanej. Inny możliwych przyczyn jest Brak trasy zdefiniowane przez użytkownika tego blokuje ruch przychodzący z Internetu.*

Najczęstszą przyczyną błędów sieciowych dla usług domenowych Azure AD mogą zostać przypisane do niepoprawne konfiguracje NSG. Aby zapewnić Microsoft możliwość usługi i obsługa zarządzane domeny, należy użyć grupy zabezpieczeń sieci (NSG) zezwala na dostęp [określone porty](active-directory-ds-networking.md#ports-required-for-azure-ad-domain-services) wewnątrz podsieci. Jeśli te porty są zablokowane, Microsoft nie może uzyskać dostępu zasoby, które są niezbędne, które mogą być szkodliwe dla usługi. Podczas tworzenia Twojej grupy NSG, nie zamykaj te porty zapewnienie nie przerwy w działaniu usługi.

## <a name="sample-nsg"></a>Przykład grupy NSG
Poniższa tabela przedstawia przykład grupy NSG, który będzie chronić domeny zarządzanej zezwalając Microsoft w celu monitorowania, zarządzania i aktualizowania informacji.

![przykład grupy NSG](.\media\active-directory-domain-services-alerts\default-nsg.png)

>[!NOTE]
> Usługi domenowe Azure AD wymaga nieograniczony dostęp ruchu wychodzącego. Zaleca się nie utworzyć wszelkie dodatkowe outboud reguły NSG.

## <a name="adding-a-rule-to-a-network-security-group-using-the-azure-portal"></a>Dodanie reguły do grupy zabezpieczeń sieci przy użyciu portalu Azure
Jeśli nie chcesz użyć programu PowerShell, można ręcznie dodać jednej reguły do grupy NSG przy użyciu portalu Azure. Wykonaj następujące kroki, aby utworzyć reguły w swojej grupy zabezpieczeń sieci.

1. Przejdź do [sieciowej grupy zabezpieczeń](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.Network%2FNetworkSecurityGroups) strony w portalu Azure
2. Wybierz grupy NSG skojarzony z domeną z tabeli.
3. W obszarze Ustawienia w obszarze nawigacji po lewej stronie kliknij **reguły zabezpieczeń dla ruchu przychodzącego** lub **reguł zabezpieczeń dla ruchu wychodzącego**.
4. Utworzyć regułę, klikając **Dodaj** i wypełnianie informacji. Kliknij przycisk **OK**.
5. Sprawdź, czy reguła została utworzona przez lokalizowanie jej w tabeli reguł.


## <a name="create-a-default-nsg-using-powershell"></a>Tworzy domyślny NSG przy użyciu programu PowerShell

Poprzedzających są kroki, które służy do tworzenia nowej grupy NSG przy użyciu programu PowerShell, który przechowuje wszystkie porty wymagane do uruchomienia domenowych Azure AD usługi otworzyć odrzuca nieautoryzowanemu dostępowi.


To rozwiązanie wymaga instalowania i uruchamiania [programu Azure AD Powershell](https://docs.microsoft.com/en-us/powershell/azure/active-directory/install-adv2?toc=%2Fazure%2Factive-directory-domain-services%2Ftoc.json&view=azureadps-2.0).

1. Nawiązać połączenia z katalogiem Azure AD.

  ```PowerShell
  # Connect to your Azure AD directory.
  Connect-AzureAD
  ```
2. Zaloguj się do subskrypcji platformy Azure.

  ```PowerShell
  # Log in to your Azure subscription.
  Login-AzureRmAccount
  ```

3. Utwórz grupy NSG z trzech reguł. Poniższy skrypt definiuje trzy reguły NSG, który umożliwia dostępu do portów wymaganych do uruchomienia usług domenowych Azure AD. Następnie skrypt tworzy Nowa grupa NSG, który zawiera te reguły. Jesteś Zapraszamy dodać dodatkowe zasady, zgodnie z własnymi potrzebami, w tym samym formacie.

  ```PowerShell
  # Create the rules needed
  $rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 443
  $rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
  -SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 3389
  $rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
  -Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
  -SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
  -DestinationPortRange 5986
  # Create the NSG with the 3 rules above
  $nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
  -Name "AADDS-NSG" -SecurityRules $rule1,$rule2,$rule3
  ```

4. Ponadto skrypt kojarzy grupy NSG z sieci wirtualnej i podsieci wyboru.

  ```PowerShell
  # Find vnet and subnet
  $vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
  $subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName
  # Set the nsg to the subnet and save the changes
  $subnet.NetworkSecurityGroup = $nsg
  Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
  ```

### <a name="full-script"></a>Pełny skrypt

```PowerShell
#Change the following values to match your deployment
$resourceGroup = "ResourceGroupName"
$vnetName = "exampleVnet"
$subnetName = "exampleSubnet"

$serviceIPs = "52.180.183.8, 23.101.0.70, 52.225.184.198, 52.179.126.223, 13.74.249.156, 52.187.117.83, 52.161.13.95, 104.40.156.18, 104.40.87.209, 52.180.179.108, 52.175.18.134, 52.138.68.41, 104.41.159.212, 52.169.218.0, 52.187.120.237, 52.161.110.169, 52.174.189.149, 13.64.151.161"

# Create the rules needed
$rule1 = New-AzureRmNetworkSecurityRuleConfig -Name https-rule -Description "Allow HTTP" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 101 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 443

$rule2 = New-AzureRmNetworkSecurityRuleConfig -Name manage-3389 -Description "Manage domain through port 3389" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 102 `
-SourceAddressPrefix * -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389

$rule3 = New-AzureRmNetworkSecurityRuleConfig -Name manage-5986 -Description "Manage domain through port 5986" `
-Access Allow -Protocol Tcp -Direction Inbound -Priority 103 `
-SourceAddressPrefix $serviceIPs -SourcePortRange * -DestinationAddressPrefix * `
-DestinationPortRange 3389


# Connect to your Azure AD directory.
Connect-AzureAD

# Log in to your Azure subscription.
Login-AzureRmAccount

# Create the NSG with the 3 rules above
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $resourceGroup -Location westus `
-Name "NSG-Default" -SecurityRules $rule1,$rule2,$rule3

# Find vnet and subnet
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $resourceGroup -Name $vnetName
$subnet = Get-AzureRmVirtualNetworkSubnetConfig -VirtualNetwork $vnet -Name $subnetName

# Set the nsg to the subnet and save the changes
$subnet.NetworkSecurityGroup = $nsg
Set-AzureRmVirtualNetwork -VirtualNetwork $vnet
```

> [!NOTE]
>To ustawienie domyślne grupy NSG nie zablokować dostęp do port używany do bezpiecznego LDAP. Aby dowiedzieć się, jak utworzyć regułę dla tego portu, odwiedź stronę [w tym artykule](active-directory-ds-troubleshoot-ldaps.md).
>

## <a name="contact-us"></a>Skontaktuj się z nami
Skontaktuj się z zespołem produktu usługi Azure Active Directory Domain Services, aby [udostępnić opinię lub pomocy technicznej](active-directory-ds-contact-us.md).
