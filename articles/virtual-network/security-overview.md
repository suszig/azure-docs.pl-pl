---
title: "Omówienie zabezpieczeń sieci na platformie Azure | Microsoft Docs"
description: "Dowiedz się więcej na temat opcji zabezpieczeń na potrzeby kontrolowania przepływu ruchu sieciowego między zasobami platformy Azure."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2017
ms.author: jdial
ms.translationtype: HT
ms.sourcegitcommit: 0e862492c9e17d0acb3c57a0d0abd1f77de08b6a
ms.openlocfilehash: 63a313d9035422207a1ce56f0da8b388e2747685
ms.contentlocale: pl-pl
ms.lasthandoff: 09/27/2017

---
# <a name="network-security"></a>Bezpieczeństwo sieci

Można ograniczyć ruch sieciowy do zasobów w sieci wirtualnej przy użyciu grupy zabezpieczeń sieci. Grupa zabezpieczeń sieci zawiera listę reguł zabezpieczeń, które zezwalają na lub blokują przychodzący lub wychodzący ruch sieciowy na podstawie źródłowego lub docelowego adresu IP, portu i protokołu. 

## <a name="network-security-groups"></a>Grupy zabezpieczeń sieci

Każdy interfejs sieciowy ma zero lub jedną skojarzoną grupę zabezpieczeń sieci. Każdy interfejs sieciowy istnieje w podsieci [sieci wirtualnej](virtual-networks-overview.md). Podsieć również może mieć zero lub jedną skojarzoną grupę zabezpieczeń sieci. 

Po zastosowaniu do podsieci reguły zabezpieczeń są stosowane do wszystkich zasobów w podsieci. Obok interfejsów sieciowych możesz mieć wystąpienia innych usług platformy Azure, takich jak usługa HDInsight, zestawy skalowania maszyn wirtualnych i środowiska usług aplikacji, wdrożone w tej podsieci.

Sposób stosowania grup zabezpieczeń sieci w przypadku skojarzenia grup zabezpieczeń sieci zarówno z interfejsem sieciowym, jak i podsiecią interfejsu sieciowego, jest następujący:

- **Ruch przychodzący**: grupa zabezpieczeń sieci skojarzona z podsiecią, w której znajduje się interfejs sieciowy, jest stosowana jako pierwsza. Cały ruch dozwolony przez grupę zabezpieczeń sieci skojarzoną z podsiecią jest następnie oceniany przez grupę zabezpieczeń sieci skojarzoną z interfejsem sieciowym. Na przykład możesz wymagać dostępu ruchu przychodzącego do maszyny wirtualnej za pośrednictwem portu 80 z Internetu. Jeśli grupa zabezpieczeń sieci została skojarzona z interfejsem sieciowym i podsiecią, w której znajduje się interfejs sieciowy, grupa zabezpieczeń sieci skojarzona z podsiecią i interfejsem sieciowym musi zezwalać na port 80. Jeśli port 80 jest dozwolony tylko przez grupę zabezpieczeń sieci skojarzoną z podsiecią lub interfejsem sieciowym, w której znajduje się podsieć, komunikacja zakończy się niepowodzeniem z powodu domyślnych reguł zabezpieczeń. Zobacz [domyślne reguły zabezpieczeń](#default-security-rules), aby uzyskać szczegółowe informacje. Jeśli grupa zabezpieczeń sieci została zastosowana tylko do podsieci lub tylko do interfejsu sieciowego i zawiera regułę, która zezwala na ruch przychodzący na porcie port 80, komunikacja zakończy się pomyślnie. 
- **Ruch wychodzący**: grupa zabezpieczeń sieci skojarzona z interfejsem sieciowym jest stosowana jako pierwsza. Cały ruch dozwolony przez grupę zabezpieczeń sieci skojarzoną z interfejsem sieciowym jest następnie oceniany przez grupę zabezpieczeń sieci skojarzoną z podsiecią.

Możesz czasami nie wiedzieć, że grupy zabezpieczeń sieci są stosowane do interfejsu sieciowego i podsieci. Reguły agregowane stosowane do interfejsu sieciowego można łatwo wyświetlić, wyświetlając [obowiązujące reguły zabezpieczeń](virtual-network-manage-nsg-arm-portal.md) dla interfejsu sieciowego. Możesz również skorzystać z możliwości [weryfikowania przepływu protokołu IP](../network-watcher/network-watcher-check-ip-flow-verify-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json) w celu ustalenia, czy komunikacja z lub do interfejsu sieciowego jest dozwolona. Narzędzie informuje, czy komunikacja jest dozwolona, oraz która reguła zabezpieczeń sieci zezwala lub nie zezwala na ruch.
 
> [!NOTE]
> Grupy zabezpieczeń sieci są skojarzone z podsieciami lub maszynami wirtualnymi i usługami w chmurze wdrożonymi w klasycznym modelu wdrażania, a nie z interfejsami sieciowymi w modelu wdrażania przy użyciu usługi Resource Manager. Aby dowiedzieć się więcej na temat modeli wdrażania platformy Azure, zapoznaj się z artykułem [Understand Azure deployment models](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Informacje na temat modeli wdrażania platformy Azure).

Tę samą grupę zabezpieczeń sieci można zastosować do dowolnej liczby indywidualnych interfejsów sieciowych i podsieci.

## <a name="security-rules"></a>Reguły zabezpieczeń

Grupa zabezpieczeń sieci nie zawiera żadnych reguł lub dowolną liczbę reguł zgodnie z potrzebami, w ramach [limitów](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits) subskrypcji platformy Azure. Każda reguła określa następujące właściwości:

|Właściwość  |Wyjaśnienie  |
|---------|---------|
|Nazwa|Unikatowa nazwa w obrębie grupy zabezpieczeń sieci.|
|Priorytet | Liczba z zakresu od 100 do 4096. Reguły są przetwarzane w kolejności priorytetów. Im niższy numer, tym wyższy priorytet, więc te o niższych numerach są przetwarzane przed tymi o wyższych numerach. Kiedy ruch jest zgodny z regułą, przetwarzanie zostaje zatrzymane. W związku z tym żadne istniejące reguły o niższych priorytetach (wyższych numerach), które mają takie same atrybuty jak reguły o wyższych priorytetach, nie będą przetwarzane.|
|Obiekt źródłowy lub docelowy| Dowolny lub indywidualny adres IP, blok CIDR (na przykład 10.0.0.0/24), tag usługi lub grupa zabezpieczeń aplikacji. Dowiedz się więcej o [tagach usługi](#service-tags) i [grupach zabezpieczeń aplikacji](#application-security-groups). Określenie zakresu, tagu usługi lub grupy zabezpieczeń aplikacji umożliwia utworzenie mniejszej liczby reguł zabezpieczeń. Możliwość określenia wielu poszczególnych adresów IP i zakresów (nie można określić wielu tagów usługi ani grup aplikacji) w regule jest dostępna w wersji zapoznawczej i nosi nazwę rozszerzonych reguł zabezpieczeń. Rozszerzone reguły zabezpieczeń można tworzyć tylko w grupach zabezpieczeń sieci utworzonych za pośrednictwem modelu wdrażania przy użyciu usługi Resource Manager. Nie można określić wielu adresów IP i zakresów adresów IP w grupach zabezpieczeń sieci utworzonych za pomocą klasycznego modelu wdrażania.|
|Protokół     | TCP, UDP lub Dowolny (obejmuje protokoły TCP, UDP i ICMP). Nie można określić samego protokołu ICMP, a więc jeśli potrzebujesz protokołu ICMP, użyj opcji Dowolny. |
|Kierunek| Określa, czy ta reguła ma zastosowanie do ruchu przychodzącego, czy wychodzącego.|
|Zakres portów     |Można określić pojedynczy port lub zakres portów. Na przykład można określić port 80 lub 10000–10005. Określenie zakresów umożliwia utworzenie mniejszej liczby reguł zabezpieczeń. Możliwość określenia wielu pojedynczych portów i zakresów portów w regule jest w wersji zapoznawczej i nosi nazwę rozszerzonych reguł zabezpieczeń. Przed rozpoczęciem korzystania z rozszerzonych reguł zabezpieczeń przeczytaj artykuł [Funkcje w wersji zapoznawczej](#preview-features), aby uzyskać ważne informacje. Rozszerzone reguły zabezpieczeń można tworzyć tylko w grupach zabezpieczeń sieci utworzonych za pośrednictwem modelu wdrażania przy użyciu usługi Resource Manager. Nie można określić wielu portów lub zakresów portów w grupach zabezpieczeń sieci utworzonych za pomocą klasycznego modelu wdrażania.   |
|Akcja     | Zezwolenie lub zablokowanie        |

**Zagadnienia do rozważenia**

- **Wirtualny adres IP węzła hosta**: podstawowe usługi infrastruktury, takie jak DHCP, DNS i monitorowanie kondycji, są realizowane za pośrednictwem zwirtualizowanych adresów IP hosta 168.63.129.16 i 169.254.169.254. Te publiczne adresy IP należą do firmy Microsoft i są jedynymi zwirtualizowanymi adresami IP używanymi do tego celu we wszystkich regionach. Te adresy IP mapują do fizycznego adresu IP komputera serwera (węzła hosta) obsługującego maszynę wirtualną. Węzeł hosta pełni rolę przekaźnika DHCP, cyklicznego programu rozpoznawania nazw DNS i źródła sondy kondycji modułu równoważenia obciążenia oraz sondy kondycji komputera. Komunikacja z tymi adresami IP nie jest atakiem. Jeśli zablokujesz ruch do lub z tych adresów IP, maszyna wirtualna może nie działać poprawnie.
- **Licencjonowanie (usługa zarządzania kluczami):** obrazy systemu Windows uruchomione na maszynach wirtualnych muszą być licencjonowane. W celu zapewnienia licencjonowania do serwerów hosta usługi zarządzania kluczami zostaje wysłane żądanie licencjonowania, które takie żądania obsługują. Żądanie jest wysyłane za pomocą portu 1688.
- **Maszyny wirtualne w pulach ze zrównoważonym obciążeniem**: port źródłowy i zakres adresów stosowane są z komputera źródłowego, nie modułu równoważenia obciążenia. Port docelowy i zakres adresów dotyczą komputera docelowego, a nie modułu równoważenia obciążenia.
- **Wystąpienia usług platformy Azure**: wystąpienia kilku usług platformy Azure, takich jak usługa HDInsight, środowiska usług aplikacji i zestawy skalowania maszyn wirtualnych, są wdrażane w podsieciach sieci wirtualnej. Upewnij się, że należy zapoznać się z wymaganiami dotyczącymi portów dla każdej usługi przed zastosowaniem grupy zabezpieczeń sieci do podsieci, w której zasób został wdrożony. Jeśli porty wymagane przez usługę zostaną zablokowane, usługa nie będzie działać prawidłowo. 

Reguły zabezpieczeń są stanowe. Jeśli zostanie określona reguła zabezpieczeń dla ruchu wychodzącego do dowolnego adresu za pośrednictwem (na przykład) portu 80, nie trzeba określać żadnej reguły zabezpieczeń ruchu przychodzącego dla odpowiedzi na ruch wychodzący. Należy tylko określić regułę zabezpieczeń dla ruchu przychodzącego w przypadku, jeśli komunikacja jest inicjowana zewnętrznie. Jest to również prawdziwe w odwrotnym przypadku. Jeśli ruch przychodzący jest dozwolony przez port, nie trzeba określać reguły zabezpieczeń dla ruchu wychodzącego, aby odpowiadać na ruch przychodzący przez port. Aby dowiedzieć się więcej o limitach podczas tworzenia reguł zabezpieczeń, zobacz [limity platformy Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

## <a name="augmented-security-rules"></a>Rozszerzone reguły zabezpieczeń

Rozszerzone reguły upraszczają definicję zabezpieczeń dla sieci wirtualnych, umożliwiając definiowanie reguł zabezpieczeń większych i złożonych sieci przy użyciu mniejszej liczby reguł. Można połączyć wiele portów, wiele jawnych adresów IP, usługi tagów i grupy zabezpieczeń aplikacji w jedną, łatwo zrozumiałą regułę zabezpieczeń. Rozszerzone reguły stosuje się w polach źródła, obiektu docelowego i portów reguły. Podczas tworzenia reguły można określić wiele jawnych adresów IP, zakresów CIDR i portów. Aby uprościć zarządzanie definicją reguły zabezpieczeń, połącz rozszerzone reguły zabezpieczeń z tagami usług lub grupami zabezpieczeń aplikacji. 

Rozszerzone reguły zabezpieczeń są dostępne w wersji zapoznawczej. Aby dowiedzieć się więcej o limitach podczas tworzenia rozszerzonych reguł zabezpieczeń, zobacz [limity platformy Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits). Funkcje w wersji zapoznawczej nie mają takiego samego poziomu dostępności i niezawodności jak funkcje w głównym wydaniu. Funkcje te są dostępne tylko w następujących regionach świadczenia usług Azure: wschodnie stany USA, zachodnie stany USA, zachodnie stany USA 2, środkowo-zachodnie stany USA, Australia Wschodnia, Australia Południowo-Wschodnia i południowe Zjednoczone Królestwo.

## <a name="default-security-rules"></a>Domyślne reguły zabezpieczeń

Jeśli z podsiecią lub interfejsem sieciowym nie skojarzono grupy zabezpieczeń sieci, dozwolony będzie cały ruch przychodzący do lub wychodzący z podsieci lub interfejsu sieciowego. Natychmiast po utworzeniu grupy zabezpieczeń sieci platforma Azure tworzy następujące reguły domyślne w ramach grupy zabezpieczeń sieci:

### <a name="inbound"></a>Przychodzący

#### <a name="allowvnetinbound"></a>AllowVNetInBound

|Priorytet|Element źródłowy|Porty źródłowe|Element docelowy|Porty docelowe|Protokół|Dostęp|
|---|---|---|---|---|---|---|
|65000|VirtualNetwork|0-65535|VirtualNetwork|0-65535|Wszystkie|Zezwalaj|

#### <a name="allowazureloadbalancerinbound"></a>AllowAzureLoadBalancerInBound

|Priorytet|Element źródłowy|Porty źródłowe|Element docelowy|Porty docelowe|Protokół|Dostęp|
|---|---|---|---|---|---|---|
|65001|AzureLoadBalancer|0-65535|0.0.0.0/0|0-65535|Wszystkie|Zezwalaj|

#### <a name="denyallinbound"></a>DenyAllInbound

|Priorytet|Element źródłowy|Porty źródłowe|Element docelowy|Porty docelowe|Protokół|Dostęp|
|---|---|---|---|---|---|---|
|65500|0.0.0.0/0|0-65535|0.0.0.0/0|0-65535|Wszystkie|Zablokuj|

### <a name="outbound"></a>Wychodzący

#### <a name="allowvnetoutbound"></a>AllowVnetOutBound

|Priorytet|Element źródłowy|Porty źródłowe| Element docelowy | Porty docelowe | Protokół | Dostęp |
|---|---|---|---|---|---|---|
| 65000 | VirtualNetwork | 0-65535 | VirtualNetwork | 0-65535 | Wszystkie | Zezwalaj |

#### <a name="allowinternetoutbound"></a>AllowInternetOutBound

|Priorytet|Element źródłowy|Porty źródłowe| Element docelowy | Porty docelowe | Protokół | Dostęp |
|---|---|---|---|---|---|---|
| 65001 | 0.0.0.0/0 | 0-65535 | Internet | 0-65535 | Wszystkie | Zezwalaj |

#### <a name="denyalloutbound"></a>DenyAllOutBound

|Priorytet|Element źródłowy|Porty źródłowe| Element docelowy | Porty docelowe | Protokół | Dostęp |
|---|---|---|---|---|---|---|
| 65500 | 0.0.0.0/0 | 0-65535 | 0.0.0.0/0 | 0-65535 | Wszystkie | Zablokuj |

W kolumnach **Źródło** i **Obiekt docelowy** elementy *VirtualNetwork*, *AzureLoadBalancer* i *Internet* są [tagami usługi](#tags), a nie adresami IP. W kolumnie protokołu pozycja **Wszystkie** obejmuje protokoły TCP, UDP i ICMP. Podczas tworzenia reguły można określić protokół TCP, UDP lub Wszystkie, ale nie można określić wyłącznie protokołu ICMP. W związku z tym jeśli reguła wymaga protokołu ICMP, musisz wybrać wartość *Wszystkie* w kolumnie protokołu. Wartość *0.0.0.0/0* w kolumnach **Źródło** i **Obiekt docelowy** reprezentuje wszystkie adresy.
 
Nie można usunąć reguł domyślnych, ale można je przesłonić, tworząc reguły o wyższych priorytetach.

## <a name="service-tags"></a>Tagi usługi

 Tag usługi reprezentuje grupę prefiksów adresów IP, aby zminimalizować złożoność tworzenia reguły zabezpieczeń. Nie można utworzyć własnego tagu usługi ani określić adresów IP uwzględnionych w tagu. Firma Microsoft zarządza prefiksami adresów obejmowanymi przez tag usługi i automatycznie aktualizuje tag usługi, gdy adresy ulegną zmianie. Podczas tworzenia reguł zabezpieczeń można użyć tagów usługi zamiast konkretnych adresów IP. Poniżej wymienione tagi usługi są dostępne do użycia w definicji reguły zabezpieczeń. Ich nazwy różnią się nieco między sobą w [modelach wdrażania platformy Azure](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

* **VirtualNetwork** (model wdrażania przy użyciu usługi *Resource Manager) (**VIRTUAL_NETWORK** — model klasyczny): ten znacznik obejmuje przestrzeń adresową sieci wirtualnej (wszystkie zakresy CIDR zdefiniowane dla sieci wirtualnej), wszystkie połączone lokalne przestrzenie adresowe oraz [skomunikowane równorzędnie](virtual-network-peering-overview.md) sieci wirtualne lub sieć wirtualna połączona z [bramą sieci wirtualnej](../vpn-gateway/vpn-gateway-about-vpngateways.md?toc=%2fazure%2fvirtual-network%2ftoc.json).
* **AzureLoadBalancer** (model usługi Resource Manager) (**AZURE_LOADBALANCER** — model klasyczny): ten znacznik określa moduł równoważenia obciążenia infrastruktury platformy Azure. Ten znacznik przekłada się na [adres IP centrum danych Azure](https://www.microsoft.com/download/details.aspx?id=41653), z którego pochodzą sondy kondycji platformy Azure. Jeśli nie jest używana usługa Azure Load Balancer, tę zasadę można przesłonić.
* **Internet** (model wdrażania przy użyciu usługi Resource Manager) (**INTERNET** — model klasyczny): ten tag określa przestrzeń publicznych adresów IP platformy Azure. Adresy objęte przez ten tag są wymienione w dokumencie [Przestrzeń publicznych adresów IP należąca do Azure](https://www.microsoft.com/download/details.aspx?id=41653), który jest regularnie aktualizowany.
* **AzureTrafficManager** (tylko model wdrażania przy użyciu usługi Resource Manager): ten tag określa przestrzeń adresów IP dla usługi Azure Traffic Manager.
* **Storage** (tylko model wdrażania przy użyciu usługi Resource Manager): ten tag określa przestrzeń adresów IP dla usługi Azure Storage. W przypadku określenia wartości *Storage* dozwolony lub blokowany jest ruch do usługi Storage. Jeśli chcesz zezwolić na dostęp do usługi Storage w konkretnym [regionie](https://azure.microsoft.com/regions), możesz określić region. Jeśli na przykład chcesz zezwolić na dostęp do usługi Azure Storage tylko w regionie Wschodnie stany USA, możesz określić *Storage.EastUS* jako tag usługi. Dostępne dodatkowe regionalne tagi usługi: Storage.AustraliaEast, Storage.AustraliaSoutheast, Storage.EastUS, Storage.UKSouth, Storage.WestCentralUS, Storage.WestUS, and Storage.WestUS2. Tag reprezentuje usługę, ale nie konkretne wystąpienia usługi. Na przykład tag reprezentuje usługę Azure Storage, ale nie konkretne konto usługi Azure Storage.
* **Sql** (tylko model wdrażania przy użyciu usługi Resource Manager): ten tag określa prefiksy adresów usług Azure SQL Database i Azure SQL Data Warehouse. Dla tego tagu usługi można określić tylko konkretne regiony. Jeśli na przykład chcesz zezwolić na dostęp do usługi Azure SQL Database tylko w regionie Wschodnie stany USA, możesz określić *Sql.EastUS* jako tag usługi. Tagu Sql nie można określić dla wszystkich regionów platformy Azure, należy określić regiony pojedynczo. Inne dostępne regionalne tagi usługi: Sql.AustraliaEast, Sql.AustraliaSoutheast, Sql.EastUS, Sql.UKSouth, Sql.WestCentralUS, Sql.WestUS, and Sql.WestUS2. Tag reprezentuje usługę, ale nie konkretne wystąpienia usługi. Na przykład tag reprezentuje usługę Azure SQL Database, ale nie konkretną bazę danych Azure SQL Database.

> [!WARNING]
> Tagi usługi AzureTrafficManager, Storage i Sql są dostępne w wersji zapoznawczej. Funkcje w wersji zapoznawczej nie mają takiego samego poziomu dostępności i niezawodności jak funkcje w głównym wydaniu. Tagi usługi dostępne tylko w następujących regionach świadczenia usług Azure: wschodnie stany USA, zachodnie stany USA, zachodnie stany USA 2, środkowo-zachodnie stany USA, Australia Wschodnia, Australia Południowo-Wschodnia i południowe Zjednoczone Królestwo.

> [!NOTE]
> W przypadku zaimplementowania punktu końcowego usługi sieci wirtualnej dla usługi takiej jak usługa Azure Storage lub Azure SQL Database, platforma Azure dodaje trasę do podsieci sieci wirtualnej dla usługi. Prefiksy adresów dla trasy to te same prefiksy adresów lub zakresy CIDR, co w odpowiednim tagu usługi.

## <a name="application-security-groups"></a>Grupy zabezpieczeń aplikacji

Grupy zabezpieczeń aplikacji umożliwiają skonfigurowanie zabezpieczeń sieci jako naturalnego rozszerzenia struktury aplikacji, co pozwala na grupowanie maszyn wirtualnych i definiowanie zasad zabezpieczeń sieci na podstawie tych grup. Ta funkcja umożliwia ponowne używanie zasady zabezpieczeń na dużą skalę bez ręcznej obsługi jawnych adresów IP. Platforma obsługuje złożoność jawnych adresów IP i wiele zestawów reguł, co pozwala skupić się na logice biznesowej.

Grupę zabezpieczeń aplikacji można określić jako źródło i obiekt docelowy reguły zabezpieczeń. Po zdefiniowaniu zasad zabezpieczeń można utworzyć maszyny wirtualne i przypisać interfejsy sieciowe w maszynie wirtualnej do grupy zabezpieczeń aplikacji. Zasady są stosowane na podstawie członkostwa w grupie zabezpieczeń aplikacji każdego interfejsu sieciowego w maszynie wirtualnej. Poniższy przykład przedstawia sposób użycia grupy zabezpieczeń aplikacji dla wszystkich serwerów sieci web w danej subskrypcji:

1. Utwórz grupę zabezpieczeń aplikacji o nazwie *WebServers*.
2. Utwórz grupę zabezpieczeń sieci o nazwie *MyNSG*.
3. Utwórz regułę zabezpieczeń dla ruchu przychodzącego w grupie zabezpieczeń sieci, określając tag usługi *Internet* dla adresu źródłowego i grupę zabezpieczeń aplikacji *WebServers* jako adres docelowy, i zezwalając na porty 80 i 443.
4. Wdróż maszynę wirtualną, na której działa aplikacja serwera sieci web. Określ interfejs sieciowy w maszynie wirtualnej jako element członkowski grupy zabezpieczeń aplikacji *WebServers*. Porty 80 i 443 będą wówczas dozwolone na maszynie wirtualnej. Porty będą również dozwolone dla dowolnych kolejnych serwerów sieci web utworzonych jako elementy członkowskie grupy zabezpieczeń aplikacji *WebServers*. 

W przypadku utworzenia innych reguł aplikacji, określających inne grupy zabezpieczeń aplikacji jako obiekty docelowe, reguły te nie będą stosowane do serwerów sieci web z poprzedniego przykładu. Reguły określające grupy zabezpieczeń aplikacji są stosowane tylko do interfejsów sieciowych, które są elementami członkowskimi grupy zabezpieczeń aplikacji. Grupy zabezpieczeń aplikacji, w połączeniu z rozszerzonymi regułami zabezpieczeń i tagami usługi, umożliwiają utworzenie minimalnej liczby grup zabezpieczeń sieci do zarządzania zabezpieczeniami sieci w ramach subskrypcji.
 
Aby dowiedzieć się więcej o limitach podczas tworzenia grup zabezpieczeń aplikacji i określania ich w regułach zabezpieczeń, zobacz [limity platformy Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits).

Grupy zabezpieczeń aplikacji są dostępne w wersji zapoznawczej. Przed rozpoczęciem korzystania z grup zabezpieczeń sieci musisz się zarejestrować, aby ich używać, wykonując kroki od 1 do 5 w sekcji [Tworzenie grupy zabezpieczeń sieci z użyciem grup zabezpieczeń aplikacji](create-network-security-group-preview.md#powershell). Przeczytaj również temat [Funkcje w wersji zapoznawczej](#preview-features), aby uzyskać ważne informacje. W wersji zapoznawczej grupy zabezpieczeń aplikacji są ograniczone do zakresu sieci wirtualnej. Sieci wirtualne skomunikowane równorzędnie za pomocą odwołań krzyżowych do grup zabezpieczeń aplikacji w grupie zabezpieczeń sieci nie są stosowane. 

Funkcje w wersji zapoznawczej nie mają takiego samego poziomu dostępności i niezawodności jak funkcje w głównym wydaniu. Przed rozpoczęciem korzystania z grup zabezpieczeń aplikacji najpierw musisz się zarejestrować, aby ich używać. Funkcje te są dostępne tylko w następujących regionach: zachodnio-środkowe stany USA.

## <a name="next-steps"></a>Następne kroki

* Wykonaj kroki samouczka [Tworzenie grupy zabezpieczeń sieci](virtual-networks-create-nsg-arm-pportal.md)
* Wykonaj kroki samouczka [Tworzenie grupy zabezpieczeń sieci z użyciem grup zabezpieczeń aplikacji](create-network-security-group-preview.md)

