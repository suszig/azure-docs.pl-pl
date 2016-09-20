<properties
   pageTitle="Tworzenie sieci wirtualnej za pomocą połączenia sieci VPN typu lokacja-lokacja przy użyciu usługi Azure Resource Manager i portalu Azure | Microsoft Azure"
   description="Tworzenie sieci wirtualnej przy użyciu modelu wdrażania usługi Azure Resource Manager i nawiązywanie połączenia z siecią lokalną za pomocą połączenia typu lokacja-lokacja bramy sieci VPN."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/31/2016"
   ms.author="cherylmc"/>

# Tworzenie sieci wirtualnej za pomocą połączenia typu lokacja-lokacja przy użyciu witryny Azure Portal

> [AZURE.SELECTOR]
- [Azure Portal](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Klasyczny portal Azure](vpn-gateway-site-to-site-create.md)
- [Program PowerShell — model usługi Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)


W tym artykule opisano proces tworzenia sieci wirtualnej i połączenia sieci VPN typu lokacja-lokacja z siecią lokalną z wykorzystaniem **modelu wdrażania usługi Azure Resource Manager** i witryny Azure Portal. Z połączeń typu lokacja-lokacja można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji.

![Diagram](./media/vpn-gateway-howto-site-to-site-resource-manager-portal/s2srmportal.png)



### Modele wdrażania i narzędzia używane w kontekście połączeń typu lokacja-lokacja

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

[AZURE.INCLUDE [vpn-gateway-table-site-to-site-table](../../includes/vpn-gateway-table-site-to-site-include.md)] 

Jeśli chcesz połączyć ze sobą sieci wirtualne, nie tworzysz jednak połączenia z lokalizacją lokalną, zapoznaj się z artykułem [Configure a VNet-to-VNet connection](vpn-gateway-vnet-vnet-rm-ps.md) (Konfigurowanie połączenia między sieciami wirtualnymi).

## Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami.

- Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Zobacz artykuł [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN — informacje). Jeśli nie dysponujesz wiedzą niezbędną do skonfigurowania urządzenia sieci VPN lub nie znasz zakresu adresów IP ujętego w konfiguracji sieci lokalnej, skontaktuj się z osobą, która może podać Ci te dane.

- Dostępny zewnętrznie publiczny adres IP urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.
    
- Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](http://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](http://azure.microsoft.com/pricing/free-trial/).

### <a name="values"></a>Przykładowe wartości konfiguracji dla tego ćwiczenia


Korzystając z tych kroków w charakterze ćwiczenia, można użyć przykładowych wartości konfiguracji:

- Nazwa sieci wirtualnej: TestVNet1
- Przestrzeń adresowa: 10.11.0.0/16 i 10.12.0.0/16
- Podsieci: 
    - FrontEnd: 10.11.0.0/24
    - BackEnd: 10.12.0.0/24
    - GatewaySubnet: 10.12.255.0/27
- Grupa zasobów: TestRG1
- Lokalizacja: Wschodnie stany USA
- Serwer DNS: 8.8.8.8
- Gateway Name: VNet1GW
- Publiczny adres IP: VNet1GWIP
- Typ sieci VPN: oparta na trasach
- Typ połączenia: lokacja-lokacja (IPsec)
- Typ bramy: VPN
- Nazwa bramy sieci lokalnej: Site2
- Nazwa połączenia: VNet1toSite2



## 1. Tworzenie sieci wirtualnej 

Jeśli masz już sieć wirtualną, sprawdź, czy ustawienia są zgodne z projektem bramy sieci VPN. Zwróć szczególną uwagę na wszelkie podsieci, które mogą pokrywać się z innymi sieciami. Obecność nakładających się podsieci spowoduje, że połączenie nie będzie działać prawidłowo. Jeśli sieć wirtualną skonfigurowano poprawnie, można rozpocząć wykonywanie kroków z sekcji [Określanie serwera DNS](#dns).

### Aby utworzyć sieć wirtualną

[AZURE.INCLUDE [vpn-gateway-basic-vnet-rm-portal](../../includes/vpn-gateway-basic-vnet-rm-portal-include.md)]  

## 2. Dodawanie dodatkowej przestrzeni adresowej i podsieci

Po utworzeniu sieci wirtualnej można dodać do niej dodatkową przestrzeń adresową oraz podsieci.

[AZURE.INCLUDE [vpn-gateway-additional-address-space](../../includes/vpn-gateway-additional-address-space-include.md)] 

## <a name="dns"></a>3. Określanie serwera DNS

Jeśli tworzysz tę konfigurację w ramach ćwiczenia, użyj tych [wartości](#values) podczas określania serwera DNS.

### Określenie serwera DNS

[AZURE.INCLUDE [vpn-gateway-add-dns-rm-portal](../../includes/vpn-gateway-add-dns-rm-portal-include.md)]

## 4. Tworzenie podsieci bramy

Przed połączeniem sieci wirtualnej z bramą należy najpierw utworzyć podsieć bramy sieci wirtualnej, z którą ma zostać nawiązane połączenie. Utworzona podsieć bramy musi mieć nazwę *GatewaySubnet* — w przeciwnym razie nie będzie działać prawidłowo. 

Prefiks podsieci bramy wymaga w przypadku niektórych konfiguracji podsieci /28 lub większej w celu dostosowania do liczby adresów IP niezbędnych w puli. Oznacza to, że prefiks podsieci bramy musi mieć długość /28, /27, /26 itp. Można utworzyć większą podsieć w celu uwzględnienia elementów, które mogą zostać dodane do konfiguracji w przyszłości.

Jeśli tworzysz tę konfigurację w ramach ćwiczenia, użyj tych [wartości](#values) podczas tworzenia podsieci bramy.

### Aby utworzyć podsieć bramy

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

[AZURE.INCLUDE [vpn-gateway-add-gwsubnet-rm-portal](../../includes/vpn-gateway-add-gwsubnet-rm-portal-include.md)]

## 5. Tworzenie bramy sieci wirtualnej

Jeśli tworzysz tę konfigurację w ramach ćwiczenia, użyj tych [wartości](#values) podczas tworzenia bramy.

### Aby utworzyć bramę sieci wirtualnej

[AZURE.INCLUDE [vpn-gateway-add-gw-rm-portal](../../includes/vpn-gateway-add-gw-rm-portal-include.md)]

## 6. Tworzenie bramy sieci lokalnej

*Brama sieci lokalnej* zazwyczaj odwołuje się do lokalizacji lokalnej. Bramie sieci lokalnej należy nadać nazwę, za pomocą której platforma Azure będzie mogła się do niej odwoływać. 

Jeśli tworzysz tę konfigurację w ramach ćwiczenia, użyj tych [wartości](#values) podczas dodawania lokalizacji lokalnej.

### Aby utworzyć bramę sieci lokalnej

[AZURE.INCLUDE [vpn-gateway-add-lng-rm-portal](../../includes/vpn-gateway-add-lng-rm-portal-include.md)]

## 7. Konfiguracja urządzenia sieci VPN

[AZURE.INCLUDE [vpn-gateway-configure-vpn-device-rm](../../includes/vpn-gateway-configure-vpn-device-rm-include.md)]

## 8. Tworzenie połączenia sieci VPN typu lokacja-lokacja

Następnie należy utworzyć połączenie sieci VPN typu lokacja-lokacja między bramą sieci wirtualnej i urządzeniem sieci VPN. Przedstawione wartości należy zastąpić własnymi. Klucz wspólny musi odpowiadać wartości użytej podczas konfiguracji urządzenia sieci VPN. 

Przed rozpoczęciem wykonywania czynności opisanych w tej sekcji należy sprawdzić, czy zakończono tworzenie bramy sieci wirtualnej i bram sieci lokalnej. Jeśli tworzysz tę konfigurację w ramach ćwiczenia, użyj tych [wartości](#values) podczas tworzenia połączenia.

### Aby utworzyć połączenie sieci VPN


[AZURE.INCLUDE [vpn-gateway-add-site-to-site-connection-rm-portal](../../includes/vpn-gateway-add-site-to-site-connection-rm-portal-include.md)]

## 9. Sprawdzenie połączenia sieci VPN

Połączenie sieci VPN można sprawdzić, korzystając z portalu lub z programu PowerShell.

[AZURE.INCLUDE [vpn-gateway-verify-connection-rm](../../includes/vpn-gateway-verify-connection-rm-include.md)]

## Następne kroki

- Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Więcej informacji zawiera [ścieżka szkoleniowa](https://azure.microsoft.com/documentation/learning-paths/virtual-machines) dotycząca maszyn wirtualnych.

- Informacje na temat protokołu BGP można znaleźć w artykułach [BGP Overview](vpn-gateway-bgp-overview.md) (Omówienie protokołu BGP) i [How to configure BGP](vpn-gateway-bgp-resource-manager-ps.md) (Konfigurowanie protokołu BGP).



<!--HONumber=sep16_HO1-->


