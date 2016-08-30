<properties
   pageTitle="Tworzenie sieci wirtualnej z wykorzystaniem połączenia bramy sieci VPN typu lokacja-lokacja przy użyciu klasycznego portalu Azure | Microsoft Azure"
   description="Tworzenie sieci wirtualnej z połączeniem bramy sieci VPN S2S dla konfiguracji hybrydowych i obejmujących wiele lokalizacji przy użyciu klasycznego modelu wdrażania."
   services="vpn-gateway"
   documentationCenter=""
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-service-management"/>

<tags
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/13/2016"
   ms.author="cherylmc"/>

# Tworzenie sieci wirtualnej z wykorzystaniem połączenia sieci VPN typu lokacja-lokacja przy użyciu klasycznego portalu Azure

> [AZURE.SELECTOR]
- [Portal Azure](vpn-gateway-howto-site-to-site-resource-manager-portal.md)
- [Klasyczny portal Azure](vpn-gateway-site-to-site-create.md)
- [Program PowerShell — model usługi Resource Manager](vpn-gateway-create-site-to-site-rm-powershell.md)


W tym artykule opisano proces tworzenia sieci wirtualnej i połączenia sieci VPN typu lokacja-lokacja z siecią lokalną. Z połączeń typu lokacja-lokacja można korzystać w ramach konfiguracji hybrydowych i obejmujących wiele lokalizacji. Ten artykuł dotyczy klasycznego modelu wdrażania i opisuje użycie klasycznego portalu Azure. 


**Modele wdrażania Azure — informacje**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## Diagram połączenia
 
![Diagram połączenia typu lokacja-lokacja](./media/vpn-gateway-site-to-site-create/site2site.png "site-to-site")

**Modele wdrażania i narzędzia używane w kontekście połączeń typu lokacja-lokacja**

[AZURE.INCLUDE [vpn-gateway-table-site-to-site](../../includes/vpn-gateway-table-site-to-site-include.md)]

Jeśli chcesz połączyć ze sobą sieci wirtualne, nie tworzysz jednak połączenia z lokalizacją lokalną, zapoznaj się z artykuł [Configure a VNet-to-VNet connection for the classic deployment model](virtual-networks-configure-vnet-to-vnet-connection.md) (Konfiguracja połączenia między sieciami wirtualnymi dla klasycznego modelu wdrażania). Jeśli potrzebujesz innego typu konfiguracji połączenia, zobacz artykuł [VPN Gateway connection topologies](vpn-gateway-topology.md) (Topologie połączenia bramy sieci VPN).

 
## Przed rozpoczęciem

Przed rozpoczęciem konfiguracji sprawdź, czy dysponujesz następującymi elementami.

- Zgodne urządzenie sieci VPN i osoba, która umie je skonfigurować. Zobacz artykuł [About VPN Devices](vpn-gateway-about-vpn-devices.md) (Urządzenia sieci VPN — informacje). Jeśli nie dysponujesz wiedzą niezbędną do skonfigurowania urządzenia sieci VPN lub nie znasz zakresu adresów IP ujętego w konfiguracji sieci lokalnej, skontaktuj się z osobą, która dysponuje tymi danymi.

-  Zewnętrzny publiczny adres IP urządzenia sieci VPN. Ten adres IP nie może się znajdować za translatorem adresów sieciowych.

- Subskrypcja platformy Azure. Jeśli nie masz jeszcze subskrypcji platformy Azure, możesz aktywować [korzyści dla subskrybentów MSDN](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) lub utworzyć [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).


## Tworzenie sieci wirtualnej

1. Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/).

2. W lewym dolnym rogu ekranu kliknij opcję **Nowe**. W okienku nawigacji kliknij opcję **Usługi sieciowe**, a następnie opcję **Sieć wirtualna**. Kliknij przycisk **Tworzenie niestandardowe**, aby rozpocząć korzystanie z kreatora konfiguracji.

3. Wprowadź informacje na następujących stronach w celu utworzenia sieci wirtualnej.

## Strona szczegółów sieci wirtualnej

Wprowadź następujące informacje.

- **Nazwa**: nadaj nazwę sieci wirtualnej. Na przykład: *EastUSVNet*. Ta nazwa sieci wirtualnej będzie używana podczas wdrażania wystąpień maszyn wirtualnych i usług PaaS, dlatego nie powinna być zbyt skomplikowana.
- **Lokalizacja**: omawiana lokalizacja jest bezpośrednio związana z lokalizacją fizyczną (regionem), w której mają się znajdować zasoby (maszyny wirtualne). Jeśli na przykład maszyny wirtualne wdrażane w ramach tej sieci wirtualnej mają znajdować się we wschodnim regionie USA, należy wybrać tę właśnie lokalizację (*Wschodnie stany USA*). Po utworzeniu sieci wirtualnej nie można zmienić powiązanego z nią regionu.

## Strona serwerów DNS i łączności VPN

Wprowadź następujące informacje, a następnie kliknij strzałkę widoczną w prawym dolnym rogu, aby przejść dalej.

- **Serwery DNS**: wprowadź nazwę serwera DNS i jego adres IP lub wybierz zarejestrowany wcześniej serwer DNS z menu skrótów. To ustawienie nie powoduje utworzenia serwera DNS, a jedynie pozwala określić, które serwery mają być używane do rozpoznawania nazw w danej sieci wirtualnej.
- **Konfiguracja sieci VPN typu lokacja-lokacja**: należy zaznaczyć pole wyboru **Konfiguracja sieci VPN typu lokacja-lokacja**.
- **Sieć lokalna**: wartość w tym polu określa fizyczną lokalizację sieci lokalnej. Możesz wybrać wcześniej utworzoną sieć lokalną lub utworzyć nową. W przypadku zaznaczenia opcji użycia utworzonej wcześniej sieci lokalnej należy przejść na stronę konfiguracji **Sieci lokalne** i upewnić się, że adres IP urządzenia sieci VPN (publiczny adres IPv4) używanego urządzenia sieci VPN dla tego połączenia jest prawidłowo wprowadzony.

## Strona łączności lokacja-lokacja

W toku procedury tworzenia nowej sieci lokalnej zostanie wyświetlona strona **Łączność typu „lokacja do lokacji”** . W przypadku wyboru opcji użycia wcześniej utworzonej sieci lokalnej ta strona nie zostanie wyświetlona w ramach kreatora i będzie można przejść bezpośrednio do następnej sekcji.

Wprowadź następujące informacje, a następnie kliknij strzałkę, aby przejść dalej.

-   **Nazwa**: nazwa, jaką chcesz nadać lokalizacji w sieci lokalnej.
-   **Adres IP urządzenia sieci VPN**: jest to publiczny adres IPv4 urządzenia lokalnej sieci VPN, które zostanie użyte do połączenia z platformą Azure. Urządzenie sieci VPN nie może znajdować się za translatorem adresów sieciowych.
-   **Przestrzeń adresowa**: należy uwzględnić początkowy adres IP oraz wartość CIDR (liczbę adresów). W tym obszarze należy określić zakresy adresów, które mają zostać wysłane za pośrednictwem bramy sieci wirtualnej do lokalizacji w ramach sieci lokalnej. Jeśli docelowy adres IP mieści się w zakresach określonych w tym obszarze, zostanie przesłany za pośrednictwem bramy sieci wirtualnej.
-   **Dodaj przestrzeń adresową**: chcąc wysyłać za pośrednictwem bramy sieci wirtualnej wiele zakresów adresów, należy określić w tym obszarze wszystkie dodatkowe zakresy. Zakresy można dodawać i usuwać w późniejszym czasie na stronie **Sieć lokalna**.

## Strona przestrzeni adresowych sieci wirtualnej

Określ zakres adresów, który ma zostać użyty dla sieci wirtualnej. Są to dynamiczne adresy IP (adresy DIP), które zostaną przypisane maszynom wirtualnym i innym wystąpieniom roli wdrażanym w ramach tej sieci wirtualnej.

Szczególnie istotne jest, aby wybrać zakres, który nie nakłada się na żaden z zakresów używanych w sieci lokalnej. Skontaktuj się z administratorem sieci. Może się okazać niezbędne wydzielenie przez administratora sieci z przestrzeni adresowej sieci lokalnej zakresu adresów IP do użycia dla sieci wirtualnej.

Wprowadź następujące informacje, a następnie kliknij znacznik wyboru widoczny w prawym dolnym rogu, aby skonfigurować sieć.

- **Przestrzeń adresowa**: należy uwzględnić początkowy adres IP oraz liczbę adresów. Sprawdź, czy określone przestrzenie adresowe nie nakładają się na żadne inne przestrzenie adresowe w obrębie sieci lokalnej.
- **Dodaj podsieć**: należy uwzględnić początkowy adres IP oraz liczbę adresów. Dodatkowe podsieci nie są wymagane, można jednak utworzyć osobne podsieci dla maszyn wirtualnych, które będą miały statyczne adresy DIP. Maszyny wirtualne mogą także tworzyć podsieci niezależne od innych wystąpień roli.
- **Dodaj podsieć bramy**: kliknij, aby dodać podsieć bramy. Podsieć bramy jest używana tylko dla bramy sieci wirtualnej i jest wymagana dla tej konfiguracji.

Kliknij znacznik wyboru na dole strony. Spowoduje to rozpoczęcie procesu tworzenia sieci wirtualnej. Po jego zakończeniu w obszarze **Stan** na stronie **Sieci** w klasycznym portalu Azure pojawi się wartość **Utworzona**. Po utworzeniu sieci wirtualnej można skonfigurować jej bramę.

[AZURE.INCLUDE [vpn-gateway-no-nsg](../../includes/vpn-gateway-no-nsg-include.md)] 

## Konfigurowanie bramy sieci wirtualnej

Następnie konieczne będzie skonfigurowanie bramy sieci wirtualnej w celu utworzenia bezpiecznego połączenia typu lokacja-lokacja. Zobacz artykuł [Configure a virtual network gateway in the Azure classic portal](vpn-gateway-configure-vpn-gateway-mp.md) (Konfigurowanie bramy sieci wirtualnej w klasycznym portalu Azure).

## Następne kroki

Po zakończeniu procesu nawiązywania połączenia można dodać do sieci wirtualnych maszyny wirtualne. Zobacz dokumentację [maszyn wirtualnych](https://azure.microsoft.com/documentation/services/virtual-machines/), aby uzyskać więcej informacji.



<!--HONumber=jun16_HO2-->


