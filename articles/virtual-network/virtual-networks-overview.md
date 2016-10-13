<properties
   pageTitle="Omówienie usługi Azure Virtual Network (VNet)"
   description="Dowiedz się więcej na temat sieci wirtualnych platformy Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="jimdial"
   manager="carmonm"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/15/2016"
   ms.author="jdial" />


# Omówienie usługi Virtual Network

Sieć wirtualna Azure odzwierciedla w chmurze Twoją sieć.  Jest logiczną izolacją chmury Azure przeznaczoną dla Twojej subskrypcji. W ramach tej sieci można w pełni kontrolować bloki adresów IP, ustawienia DNS, zasady zabezpieczeń i tabele tras. Można również podzielić sieć na jeszcze mniejsze segmenty i uruchomić maszyny wirtualne IaaS i/lub [usługi w chmurze Azure (wystąpienia roli PaaS)](../cloud-services/cloud-services-choose-me.md). Dodatkowo można połączyć sieć wirtualną z siecią lokalną przy użyciu jednej z [opcji łączności](../vpn-gateway/vpn-gateway-about-vpngateways.md#site-to-site-and-multi-site) dostępnej na platformie Azure. W zasadzie można rozbudować swoją sieć do sieci Azure, zachowując pełną kontrolę nad blokami adresów IP i wykorzystując zapewnianą przez platformę Azure skalowalność klasy korporacyjnej.

Aby lepiej zrozumieć sieci wirtualne, należy spojrzeć na poniższy rysunek, przedstawiający uproszczoną sieć lokalną.

![Sieć lokalna](./media/virtual-networks-overview/figure01.png)

Na ilustracji powyżej przedstawiono sieć lokalną połączoną z publiczną siecią Internet za pośrednictwem routera. Widać tam również zapory między routerem i strefą DMZ hostującą serwer DNS i farmę serwerów sieci Web. Obciążenie farmy serwerów sieci Web jest równoważone za pomocą sprzętowego modułu równoważenia obciążenia, który ma połączenie z Internetem i wykorzystuje zasoby podsieci wewnętrznej. Podsieć wewnętrzna, oddzielona od strefy DMZ przez inną zaporę, hostuje serwery kontrolerów domeny usługi Active Directory, serwery baz danych i serwery aplikacji.

Ta sama sieć może być hostowana na platformie Azure, jak pokazano na poniższej ilustracji.

![Sieć wirtualna Azure](./media/virtual-networks-overview/figure02.png)

Zwróć uwagę, jak infrastruktura platformy Azure przejmuje rolę routera, umożliwiając dostęp z Twojej sieci wirtualnej do publicznej sieci Internet bez konieczności konfigurowania. Zapory sieciowe można zastąpić grupami zabezpieczeń sieci (Network Security Group, NSG) zastosowanymi do poszczególnych podsieci. Fizyczne urządzenia równoważące obciążenie są zastępowane przez usługi równoważenia obciążenia działające pomiędzy Internetem i wewnętrzną siecią Azure.

>[AZURE.NOTE] Dostępne są dwa tryby wdrażania platformy Azure: klasyczny (znany także jako Service Management) i Azure Resource Manager (ARM). Klasyczne sieci wirtualne mogą być dodawane do grupy koligacji lub tworzone jako regionalne sieci wirtualne. Jeśli sieć wirtualna należy do grupy koligacji, zaleca się [przeprowadzić jej migrację do regionalnej sieci wirtualnej](virtual-networks-migrate-to-regional-vnet.md).

## Korzyści ze stosowania usługi Virtual Network

- **Izolacja**. Sieci wirtualne są całkowicie odizolowane od siebie. Pozwala to tworzyć rozłączne sieci dla środowisk deweloperskich, testowych i produkcyjnych z użyciem tych samych bloków adresów CIDR.

- **Dostęp do publicznego Internetu**. Wszystkie maszyny wirtualne IaaS i wystąpienia roli PaaS w sieci wirtualnej mają domyślnie dostęp do publicznej sieci Internet. Dostęp jest kontrolowany przy użyciu grup zabezpieczeń sieci (NSG).

- **Dostęp do maszyn wirtualnych w ramach sieci wirtualnej**. Wystąpienia roli PaaS i maszyny wirtualne IaaS można uruchomić w tej samej sieci wirtualnej i połączyć ze sobą przy użyciu prywatnych adresów IP (nawet jeśli znajdują się w różnych podsieciach) bez konieczności konfigurowania bramy ani używania publicznych adresów IP.

- **Rozpoznawanie nazw**. Platforma Azure udostępnia usługę rozpoznawania nazw wewnętrznych dla maszyn wirtualnych IaaS i wystąpień roli PaaS wdrożonych w sieci wirtualnej użytkownika. Można także wdrożyć własne serwery DNS i skonfigurować sieci wirtualne do korzystania z nich.

- **Bezpieczeństwo** Ruch wchodzący do maszyn wirtualnych oraz wystąpień roli PaaS w sieci wirtualnej i wychodzący z nich można kontrolować przy użyciu grup zabezpieczeń sieci.

- **Łączność**. Sieci wirtualne można łączyć ze sobą, a nawet z lokalnym centrum danych, korzystając z połączenia sieci VPN typu lokacja-lokacja lub połączenia ExpressRoute. Aby dowiedzieć się więcej na temat bram sieci VPN, odwiedź stronę [About VPN gateways](../vpn-gateway/vpn-gateway-about-vpngateways.md) (Informacje o bramach sieci VPN). Aby dowiedzieć się więcej na temat połączenia ExpressRoute, odwiedź stronę [ExpressRoute technical overview](../expressroute/expressroute-introduction.md) (Opis techniczny ExpressRoute).

    >[AZURE.NOTE] Upewnij się, że sieć wirtualną utworzono przed wdrożeniem maszyn wirtualnych IaaS lub wystąpień roli PaaS w środowisku platformy Azure. Maszyny wirtualne oparte o usługę ARM wymagają sieci wirtualnej, a jeśli nie podano istniejącej sieci wirtualnej, system Azure utworzy domyślną sieć wirtualną, która może zawierać blok adresów CIDR powodujący konflikty z siecią lokalną. Uniemożliwia to połączenie Twojej sieci wirtualnej z siecią lokalną.

## Podsieci

Podsieć jest zakresem adresów IP w sieci wirtualnej; sieć wirtualną można podzielić na wiele podsieci w celu jej uporządkowania i zapewnienia bezpieczeństwa. Maszyny wirtualne i wystąpienia ról PaaS wdrożone w podsieciach (tych samych lub różnych) w ramach sieci wirtualnej mogą komunikować się ze sobą bez dodatkowego konfigurowania. Można także skonfigurować tabele tras i grupy zabezpieczeń sieci dla podsieci.

## Adresy IP


Istnieją dwa typy adresów IP przypisanych do zasobów platformy Azure: *publiczne* i *prywatne*. Publiczne adresy IP umożliwiają zasobom platformy Azure komunikację z Internetem i innymi publicznymi usługami Azure, takimi jak [Azure Redis Cache](https://azure.microsoft.com/services/cache/) i [Azure Event Hubs](https://azure.microsoft.com/documentation/services/event-hubs/). Prywatne adresy IP umożliwiają komunikację między zasobami w sieci wirtualnej (również z tymi połączonymi za pośrednictwem sieci VPN) bez korzystania z adresów IP routingu internetowego.

Aby dowiedzieć się więcej o adresach IP w systemie Azure, odwiedź stronę [IP addresses in virtual network](virtual-network-ip-addresses-overview-arm.md) (Adresy IP w sieci wirtualnej)

## Moduły równoważenia obciążenia Azure

Maszyny wirtualne i usługi w chmurze znajdujące się w sieci wirtualnej mogą łączyć się z Internetem za pośrednictwem usługi równoważenia obciążenia Azure. Aplikacje biznesowe, które są ukierunkowane wyłącznie do wewnątrz, mogą być obsługiwane przez wewnętrzne moduły równoważenia obciążenia.

- **Zewnętrzne równoważenie obciążenia**. Aby maszynom wirtualnym IaaS i wystąpieniom roli PaaS zapewnić wysoką dostępność z publicznej sieci Internet, można użyć zewnętrznego modułu równoważenia obciążenia.

- **Wewnętrzne równoważenie obciążenia**. Aby maszynom wirtualnym IaaS i wystąpieniom roli PaaS zapewnić wysoką dostępność z innych usług sieci wirtualnej, można użyć wewnętrznego modułu równoważenia obciążenia.

Aby dowiedzieć się więcej na temat równoważenia obciążenia w Azure, odwiedź stronę [Load balancer overview](../load-balancer/load-balancer-overview.md) (Omówienie usługi równoważenia obciążenia).

## Grupy zabezpieczeń sieci (NSG)

Grupy NSG można utworzyć w celu kontrolowania dostępu ruchu przychodzącego i wychodzącego do interfejsów sieciowych (NIC), maszyn wirtualnych i podsieci. Każda grupa NSG zawiera jedną lub więcej reguł określających zatwierdzenie lub odrzucenie ruchu na podstawie źródłowego adresu IP, portu źródłowego, docelowego adresu IP i portu docelowego. Aby dowiedzieć się więcej na temat grup NSG, odwiedź stronę [Co to jest grupa zabezpieczeń sieci](virtual-networks-nsg.md).

## Urządzenie wirtualne

Urządzenie wirtualne jest po prostu inną maszyną wirtualną w sieci wirtualnej użytkownika z oprogramowaniem pełniącym funkcję urządzenia, takie jak zapora, optymalizacja sieci WAN lub wykrywania nieautoryzowanego dostępu. W systemie Azure można utworzyć trasę służącą do routowania ruchu sieci wirtualnej przez urządzenie wirtualne w celu wykorzystania jego funkcji.

Na przykład grupy NSG mogą służyć do zapewnienia bezpieczeństwa w sieci wirtualnej użytkownika. Grupa NSG dostarcza jednak tylko listę kontroli dostępu (ACL) warstwy 4 dla przychodzących i wychodzących pakietów. Jeśli chcesz wykorzystać model zabezpieczeń warstwy 7, należy skorzystać z urządzenia zapory.

Urządzenia wirtualne są zależne od [zdefiniowanych przez użytkownika tras i przesyłania dalej IP](virtual-networks-udr-overview.md).

## Limity
Istniejące ograniczenia liczby sieci wirtualnych dozwolonych w subskrypcji opisano na stronie [Azure Networking limits](../azure-subscription-service-limits.md#networking-limits) (Ograniczenia sieci Azure).

## Cennik
Korzystanie z sieci wirtualnych w programie Azure nie pociąga za sobą żadnych dodatkowych kosztów. Za wystąpienia obliczeniowe uruchomione w sieci wirtualnej zostanie naliczona standardowa opłata zgodnie z opisem w artykule [Maszyny wirtualne — cennik](https://azure.microsoft.com/pricing/details/virtual-machines/). [Brama sieci VPN](https://azure.microsoft.com/pricing/details/vpn-gateway/) i [publiczne adresy IP] (https://azure.microsoft.com/pricing/details/ip-addresses/) używane w sieci wirtualnej również podlegają standardowym opłatom.

## Następne kroki

- [Tworzenie sieci wirtualnej](virtual-networks-create-vnet-arm-pportal.md) i podsieci.
- [Tworzenie maszyny wirtualnej w sieci wirtualnej](../virtual-machines/virtual-machines-windows-hero-tutorial.md).
- Dowiedz się więcej o [grupach NSG](virtual-networks-nsg.md).
- Dowiedz się więcej o [trasach zdefiniowanych przez użytkownika i przesyłaniu dalej IP](virtual-networks-udr-overview.md).



<!--HONumber=Oct16_HO1-->


