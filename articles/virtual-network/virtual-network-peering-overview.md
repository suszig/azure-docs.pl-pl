
<properties
   pageTitle="Komunikacja równorzędna w sieci wirtualnej platformy Azure | Microsoft Azure"
   description="Dowiedz się więcej o komunikacji równorzędnej w sieci wirtualnej platformy Azure."
   services="virtual-network"
   documentationCenter="na"
   authors="NarayanAnnamalai"
   manager="jefco"
   editor="tysonn" />
<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="07/28/2016"
   ms.author="narayan" />


# Komunikacja równorzędna sieci wirtualnych

Komunikacja równorzędna w sieci wirtualnej to mechanizm, który łączy dwie sieci wirtualne w tym samym regionie za pośrednictwem sieci szkieletowej platformy Azure. Po połączeniu za pomocą komunikacji równorzędnej dwie sieci wirtualne są traktowane jako jedna do wszystkich celów związanych z łącznością. Są one nadal zarządzane jako oddzielne zasoby, ale maszyny wirtualne w tych sieciach wirtualnych mogą komunikować się bezpośrednio przy użyciu prywatnych adresów IP.

Ruch między maszynami wirtualnymi w wirtualnych sieciach równorzędnych odbywa się za pośrednictwem infrastruktury platformy Azure, tak jak ruch między maszynami wirtualnymi w tej samej sieci wirtualnej. Korzystanie z komunikacji równorzędnej w sieci wirtualnej zapewnia m.in. następujące korzyści:

- Połączenie o małych opóźnieniach i dużej przepustowości między zasobami w różnych sieciach wirtualnych.
- Możliwość korzystania z zasobów, takich jak urządzenia sieciowe i bramy sieci VPN, jako punktów tranzytowych w wirtualnych sieciach równorzędnych.
- Możliwość połączenia sieci wirtualnej korzystającej z modelu wdrażania przy użyciu usługi Azure Resource Manager z siecią wirtualną korzystającą z klasycznego modelu wdrażania i zapewnienie pełnej łączności między zasobami w tych sieciach wirtualnych.

Wymagania i kluczowe aspekty komunikacji równorzędnej w sieci wirtualnej:

- Dwie wirtualne sieci równorzędne muszą znajdować się w tym samym regionie platformy Azure.
- Przestrzenie adresów IP wirtualnych sieci równorzędnych nie mogą się nakładać.
- Komunikacja równorzędna w sieci wirtualnej odbywa się między dwiema sieciami wirtualnymi i nie istnieje żadna pochodna relacja przechodnia. Jeśli na przykład sieć wirtualna A jest połączona za pomocą komunikacji równorzędnej z siecią wirtualną B, a sieć wirtualna B jest połączona za pomocą komunikacji równorzędnej z siecią wirtualną C, nie oznacza to, że sieć wirtualna A jest połączona za pomocą komunikacji równorzędnej z siecią wirtualną C.
- Komunikacja równorzędna może zostać nawiązana między sieciami wirtualnymi w dwóch różnych subskrypcjach, o ile połączenie za pomocą komunikacji równorzędnej zostanie autoryzowane przez uprawnionego użytkownika w obu subskrypcjach, a subskrypcje są skojarzone z tą samą dzierżawą usługi Active Directory. 
- Sieć wirtualna korzystająca z modelu wdrażania przy użyciu usługi Resource Manager może zostać połączona za pomocą komunikacji równorzędnej z inną siecią, która używa tego modelu, lub z siecią wirtualną korzystającą z klasycznego modelu wdrażania. Jednak sieci wirtualne korzystające z klasycznego modelu wdrażania nie mogą być połączone ze sobą za pomocą komunikacji równorzędnej.
- Choć komunikacja między maszynami wirtualnymi w wirtualnych sieciach równorzędnych połączonych nie wiąże się z dodatkowymi ograniczeniami dotyczącymi przepustowości, nadal obowiązuje ograniczenie przepustowości zależne od rozmiaru maszyny wirtualnej.


![Podstawowa komunikacja równorzędna w sieci wirtualnej](./media/virtual-networks-peering-overview/figure01.png)

## Łączność
Po połączeniu dwóch sieci wirtualnych za pomocą komunikacji równorzędnej maszyna wirtualna (o roli sieci Web / procesu roboczego) w sieci wirtualnej może łączyć się bezpośrednio z innymi maszynami wirtualnymi w sieci wirtualnej połączonej za pomocą komunikacji równorzędnej. Te dwie sieci mają pełną łączność na poziomie IP.

Opóźnienie sieci podczas komunikacji dwustronnej między dwiema maszynami wirtualnymi w wirtualnych sieciach równorzędnych jest takie samo jak w przypadku komunikacji dwustronnej w lokalnej sieci wirtualnej. Przepływność sieci zależy od przepustowości dozwolonej dla maszyny wirtualnej proporcjonalnie do jej rozmiaru. Nie ma żadnych dodatkowych ograniczeń przepustowości.

Ruch między maszynami wirtualnymi w wirtualnych sieciach równorzędnych odbywa się bezpośrednio za pomocą infrastruktury zaplecza platformy Azure, a nie przy użyciu bramy.

Maszyny wirtualne w sieci wirtualnej mogą uzyskiwać dostęp do wewnętrznych punktów końcowych z równoważeniem obciążenia (ILB) w równorzędnej sieci wirtualnej. Grupy zabezpieczeń sieci (NSG) można stosować w dowolnej sieci wirtualnej, aby w razie potrzeby blokować dostęp do innych sieci wirtualnych lub podsieci.

Podczas konfiguracji komunikacji równorzędnej przez użytkowników można otworzyć lub zamknąć reguły grup zabezpieczeń sieci między sieciami wirtualnymi. Jeśli użytkownik zdecyduje się na otwarcie pełnej łączności między równorzędnymi sieciami wirtualnymi (jest to opcja domyślna), może zablokować lub odmówić dostępu do określonych podsieci lub maszyn wirtualnych przy użyciu reguł zabezpieczeń sieci.

Usługa rozpoznawania wewnętrznych nazw DNS na platformie Azure dla maszyn wirtualnych nie działa w sieciach wirtualnych połączonych za pomocą komunikacji równorzędnej. Maszyny wirtualne mają wewnętrzne nazwy DNS rozpoznawalne tylko w lokalnej sieci wirtualnej. Jednak użytkownicy mogą skonfigurować maszyny wirtualne w sieciach wirtualnych połączonych za pomocą komunikacji równorzędnej jako serwery DNS dla sieci wirtualnej.

## Tworzenie łańcuchów usług
Użytkownicy mogą konfigurować tabele tras zdefiniowane przez użytkownika prowadzące do maszyn wirtualnych w wirtualnych sieciach równorzędnych jako adresy IP „kolejnego przeskoku”, jak przedstawiono na diagramie w dalszej części tego artykułu. Dzięki temu użytkownicy mogą tworzyć łańcuchy usług, za pośrednictwem których mogą kierować ruch z jednej sieci wirtualnej do urządzenia wirtualnego działającego w równorzędnej sieci wirtualnej przy użyciu tabel tras zdefiniowanych przez użytkownika.

Użytkownicy mogą również skutecznie tworzyć środowiska typu gwiazdy, w których serwer centralny może być hostem składników infrastruktury, takich jak sieciowe urządzenie wirtualne. Wszystkie wirtualne sieci-satelity mogą następnie łączyć się z nim za pomocą komunikacji równorzędnej oraz kierować część ruchu do urządzeń działających w sieci wirtualnej serwera centralnego. Krotko mówiąc, wirtualne sieci równorzędne umożliwiają użycie adresu IP kolejnego przeskoku w tabeli tras zdefiniowanej przez użytkownika jako adresu IP maszyny wirtualnej w wirtualnej sieci równorzędnej.

## Bramy i łączność lokalna
Każda sieć wirtualna, niezależnie od tego, czy jest połączona za pomocą komunikacji równorzędnej z inną siecią wirtualną, może mieć własną bramę i używać jej do łączenia się z lokalną infrastrukturą. Użytkownicy mogą również konfigurować [połączenia między sieciami wirtualnymi](../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md) przy użyciu bram, nawet jeśli te sieci wirtualne są połączone za pomocą komunikacji równorzędnej.

Po skonfigurowaniu obu opcji łączności między sieciami wirtualnymi ruch między tymi sieciami wirtualnymi jest oparty na konfiguracji komunikacji równorzędnej (to znaczy odbywa się za pośrednictwem sieci szkieletowej platformy Azure).

Gdy sieci wirtualne są połączone za pomocą komunikacji równorzędnej, użytkownicy mogą również skonfigurować bramę w wirtualnej sieci równorzędnej jako punkt tranzytowy do infrastruktury lokalnej. W tym przypadku sieć wirtualna korzystająca z bramy zdalnej nie może mieć własnej bramy. Jedna sieć wirtualna może mieć tylko jedną bramę. Może to być brama lokalna lub brama zdalna (w wirtualnej sieci równorzędnej), jak przedstawiono na poniższej ilustracji.

Przesyłanie danych za pomocą bramy nie jest obsługiwane w relacji komunikacji równorzędnej między sieciami wirtualnymi korzystającymi z modelu wdrażania usługi Resource Manager oraz klasycznego modelu wdrażania. Aby przesyłanie danych za pośrednictwem bramy mogło działać, obie sieci wirtualne w relacji komunikacji równorzędnej muszą korzystać z modelu wdrażania usługi Resource Manager.

W przypadku połączenia za pomocą komunikacji równorzędnej sieci wirtualnych współużytkujących jedno połączenie sługi Azure ExpressRoute ruch między nimi jest oparty na relacji komunikacji równorzędnej (to znaczy odbywa się za pośrednictwem sieci szkieletowej platformy Azure). Użytkownicy mogą nadal korzystać z bram lokalnych w poszczególnych sieciach wirtualnych, aby łączyć się z obwodem lokalnym. Można również użyć bramy współdzielonej i skonfigurować tranzyt dla łączności lokalnej.

![Tranzyt w komunikacji równorzędnej w sieci wirtualnej](./media/virtual-networks-peering-overview/figure02.png)

## Inicjowanie obsługi
Komunikacja równorzędna w sieci wirtualnej jest operacją wymagającą odpowiednich uprawnień. Jest to oddzielna funkcja w przestrzeni nazw VirtualNetworks. Użytkownik może uzyskać określone uprawnienia do autoryzowania komunikacji równorzędnej. Użytkownik mający dostęp do odczytu i zapisu do sieci wirtualnej automatycznie dziedziczy te uprawnienia.

Użytkownik będący administratorem lub użytkownikiem mającym uprawienia do komunikacji równorzędnej może zainicjować operację komunikacji równorzędnej w innej sieci wirtualnej. Jeśli po drugiej stronie istnieje zgodne żądanie komunikacji równorzędnej i spełnione są wymagania, zostanie nawiązane połączenie za pomocą komunikacji równorzędnej.

Aby uzyskać więcej informacji na temat nawiązywania połączenia między dwiema sieciami wirtualnych za pomocą komunikacji równorzędnej w sieci wirtualnej, zapoznaj się z artykułami w sekcji „Następne kroki”.

## Limity
Istnieją limity liczby dozwolonych połączeń za pomocą komunikacji równorzędnej dla jednej sieci wirtualnej. Więcej informacji zawiera temat [Limity dotyczące sieci platformy Azure](../azure-subscription-service-limits.md#networking-limits).

## Cennik
Funkcja komunikacji równorzędnej w sieci wirtualnej będzie bezpłatna w okresie udostępniania wersji zapoznawczej. Po jej udostępnieniu będzie obowiązywać opłata nominalna za ruch przychodzący i wychodzący odbywający się przy użyciu funkcji komunikacji równorzędnej. Więcej informacji zawiera [strona cennika](https://azure.microsoft.com/pricing/details/virtual-network).


## Następne kroki
- [Konfigurowanie komunikacji równorzędnej między sieciami wirtualnymi](virtual-networks-create-vnetpeering-arm-portal.md).
- Dowiedz się więcej o [grupach NSG](virtual-networks-nsg.md).
- Dowiedz się więcej o [trasach zdefiniowanych przez użytkownika i przesyłaniu dalej IP](virtual-networks-udr-overview.md).



<!--HONumber=Sep16_HO4-->


