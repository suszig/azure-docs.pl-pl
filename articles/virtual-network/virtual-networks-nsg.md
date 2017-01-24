---
title: "Sieciowe grupy zabezpieczeń | Microsoft Docs"
description: "Dowiedz się, jak izolować przepływ ruchu i sterować nim w sieciach wirtualnych przy użyciu rozproszonej zapory platformy Azure korzystającej z sieciowych grup zabezpieczeń."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: 20e850fc-6456-4b5f-9a3f-a8379b052bc9
ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 1de0827c01c772a4298b7b568363e89f08910ff7
ms.openlocfilehash: 46dce57f509872580c57bb1d8d93af51623211ac


---
# <a name="network-security-groups"></a>Grupy zabezpieczeń sieci

Sieciowa grupa zabezpieczeń (NSG, Network security group) zawiera listę reguł listy kontroli dostępu (ACL), które blokują lub zezwalają na ruch sieciowy do wystąpień maszyn wirtualnych w sieci wirtualnej. Sieciowe grupy zabezpieczeń można kojarzyć z podsieciami lub poszczególnymi wystąpieniami maszyn wirtualnych w danej podsieci. Gdy sieciowa grupa zabezpieczeń jest skojarzona z podsiecią, reguły listy ACL dotyczą wszystkich wystąpień maszyn wirtualnych w tej podsieci. Ponadto ruch do poszczególnych maszyn wirtualnych można ograniczyć jeszcze bardziej przez skojarzenie sieciowej grupy zabezpieczeń bezpośrednio z tą maszyną wirtualną.

> [!NOTE]
> Platforma Azure oferuje dwa różne modele wdrażania związane z tworzeniem zasobów i pracą z nimi: [model wdrażania przy użyciu usługi Azure Resource Manager i model klasyczny](../resource-manager-deployment-model.md). W tym artykule przedstawiono oba modele, ale firma Microsoft zaleca przeprowadzanie większości nowych wdrożeń z zastosowaniem modelu wdrażania przy użyciu usługi Azure Resource Manager.

## <a name="nsg-resource"></a>Zasób sieciowej grupy zabezpieczeń
Sieciowe grupy zabezpieczeń obejmują następujące właściwości.

| Właściwość | Opis | Ograniczenia | Zagadnienia do rozważenia |
| --- | --- | --- | --- |
| Nazwa |Nazwa sieciowej grupy zabezpieczeń |Musi być unikatowa w obrębie regionu<br/>Może zawierać litery, cyfry, podkreślenia, kropki i łączniki<br/>Musi zaczynać się literą lub cyfrą<br/>Musi kończyć się literą, cyfrą lub podkreśleniem<br/>Może zawierać maksymalnie 80 znaków |Ponieważ może być konieczne utworzenie kilku sieciowych grup zabezpieczeń, upewnij się, że dysponujesz konwencją nazewnictwa, która pozwala łatwo identyfikować ich funkcję |
| Region |Region platformy Azure, w którym sieciowa grupa zabezpieczeń jest obsługiwana |Sieciowe grupy zabezpieczeń mogą być stosowane wyłącznie do zasobów w regionie, w którym zostały utworzone |Zobacz sekcję [Limity](#Limits) poniżej, która zawiera informacje na temat liczby sieciowych grup zabezpieczeń, która może znajdować się w regionie |
| Grupa zasobów |Grupa zasobów, do której należy sieciowa grupa zabezpieczeń |Chociaż sieciowa grupa zabezpieczeń należy do grupy zasobów, może być skojarzona z zasobami w dowolnej grupie zasobów, jeśli tylko zasób jest częścią tego samego regionu Azure co sieciowa grupa zabezpieczeń |Grupy zasobów służą do zarządzania wieloma zasobami równocześnie w ramach jednostki wdrożenia<br/>Można rozważyć zgrupowanie sieciowej grupy zabezpieczeń z zasobami, z którymi jest ona skojarzona |
| Reguły |Reguły określające, jaki ruch jest dozwolony lub zablokowany | |Zobacz [Reguły sieciowej grupy zabezpieczeń](#Nsg-rules) poniżej |

> [!NOTE]
> Listy ACL oparte na punktach końcowych i sieciowe grupy zabezpieczeń nie są obsługiwane w tym samym wystąpieniu maszyny wirtualnej. Jeśli chcesz użyć sieciowej grupy zabezpieczeń, a masz już listę ACL punktów końcowych, najpierw usuń listę ACL punktów końcowych. Aby dowiedzieć się, jak to zrobić, zobacz artykuł [Managing Access Control Lists (ACLs) for Endpoints by using PowerShell](virtual-networks-acl-powershell.md) (Zarządzanie listami kontroli dostępu (ACL) dla punktów końcowych przy użyciu programu PowerShell).
> 

### <a name="nsg-rules"></a>Reguły sieciowej grupy zabezpieczeń
Reguły sieciowych grup zabezpieczeń obejmują następujące właściwości:

| Właściwość | Opis | Ograniczenia | Zagadnienia do rozważenia |
| --- | --- | --- | --- |
| **Nazwa** |Nazwa reguły |Musi być unikatowa w obrębie regionu<br/>Może zawierać litery, cyfry, podkreślenia, kropki i łączniki<br/>Musi zaczynać się literą lub cyfrą<br/>Musi kończyć się literą, cyfrą lub podkreśleniem<br/>Może zawierać maksymalnie 80 znaków |Sieciowa grupa zabezpieczeń może zawierać kilka reguł, więc pamiętaj, aby postępować zgodnie z konwencją nazewnictwa, która umożliwia zidentyfikowanie funkcji reguły |
| **Protokół** |Protokół odpowiadający regule |TCP, UDP lub \* |Wstawienie znaku \* oznacza protokół ICMP (tylko ruch wschód-zachód), a także protokołów UDP i TCP i może zmniejszyć liczbę potrzebnych reguł<br/>Z drugiej strony użycie znaku \* może okazać się rozwiązaniem zbyt ogólnym, więc pamiętaj, aby używać go tylko w razie konieczności |
| **Zakres portów źródłowych** |Zakres portów źródłowych odpowiadający regule |Numer pojedynczego portu od 1 do 65535, zakres portu (tj. 1-65635) lub \* (dla wszystkich portów) |Porty źródłowe mogą być efemeryczne. Jeśli program kliencki nie korzysta z określonego portu, należy w większości przypadków użyć znaku „*”.<br/>Używaj zakresów portów możliwie często, aby uniknąć konieczności korzystania z wielu reguł<br/>Wielu portów lub zakresów portów nie można oddzielać przecinkiem |
| **Zakres portów docelowych** |Zakres portów docelowych odpowiadający regule |Numer pojedynczego portu od 1 do 65535, zakres portu (tj. 1-65535) lub \* (dla wszystkich portów) |Używaj zakresów portów możliwie często, aby uniknąć konieczności korzystania z wielu reguł<br/>Wielu portów lub zakresów portów nie można oddzielać przecinkiem |
| **Prefiks adresu źródłowego** |Prefiks adresu źródłowego lub znacznik odpowiadający regule |Pojedynczy adres IP (tj. 10.10.10.10), podsieć IP (tj. 192.168.1.0/24), [znacznik domyślny](#default-tags) lub * (dla wszystkich adresów) |Należy rozważyć użycie zakresów, znaczników domyślnych i znaków * w celu zmniejszenia liczby reguł |
| **Prefiks adresu docelowego** |Prefiks adresu docelowego lub znacznik odpowiadający regule |pojedynczy adres IP (tj. 10.10.10.10), podsieć IP (tj. 192.168.1.0/24), [znacznik domyślny](#default-tags) lub * (dla wszystkich adresów) |Należy rozważyć użycie zakresów, znaczników domyślnych i znaków * w celu zmniejszenia liczby reguł |
| **Kierunek** |Kierunek ruchu odpowiadający regule |ruch przychodzący lub wychodzący |Reguły ruchu przychodzącego i wychodzącego są przetwarzane oddzielnie w zależności od kierunku |
| **Priorytet** |Reguły są sprawdzane według ważności. Gdy reguła ma zastosowanie, żadne inne reguły nie są sprawdzane pod kątem dopasowania |Liczba z zakresu od 100 do 4096 |Należy rozważyć utworzenie reguł przez przeskoczenie priorytetów o 100 dla każdej reguły, aby zostawić miejsce na nowe reguły, które pojawią się między istniejącymi |
| **Dostęp** |Typ dostępu do zastosowania, jeśli reguła jest dopasowana |zezwolenie lub zablokowanie |Pamiętaj, że jeśli dla pakietu nie zostanie odnaleziona reguła zezwalająca, zostanie on porzucony |

Sieciowe grupy zabezpieczeń zawierają dwa zestawy reguł: zestaw reguł przychodzących i wychodzących. Priorytety reguł muszą być unikatowe w poszczególnych zestawach. 

![Przetwarzanie reguł sieciowej grupy zabezpieczeń](./media/virtual-network-nsg-overview/figure3.png) 

Na rysunku powyżej przedstawiono, jak są przetwarzane reguły sieciowej grupy zabezpieczeń.

### <a name="default-tags"></a>Znaczniki domyślne
Znaczniki domyślne są dostarczanymi przez system identyfikatorami określającymi kategorię adresów IP. Można użyć znaczników domyślnych we właściwościach **prefiksu adresu źródłowego** i **prefiksu adresu docelowego** dowolnej reguły. Istnieją trzy znaczniki domyślne, których można użyć.

* **VIRTUAL_NETWORK:** określa całą przestrzeń adresową sieci. Obejmuje przestrzeń adresową sieci wirtualnej (zakresy CIDR określone na platformie Azure), a także wszystkie połączone lokalne przestrzenie adresowe i połączone sieci wirtualne Azure (sieci lokalne).
* **AZURE_LOADBALANCER:** określa moduł równoważenia obciążenia infrastruktury platformy Azure. Przekłada się on na adres IP centrum danych Azure, z którego pochodzą sondy kondycji Azure.
* **INTERNET:** określa przestrzeń adresów IP, która znajduje się poza siecią wirtualną i do której można uzyskać dostęp w publicznym Internecie. Ten zakres obejmuje również [publiczną przestrzeń adresów IP należącą do Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### <a name="default-rules"></a>Reguły domyślne
Wszystkie sieciowe grupy zabezpieczeń zawierają zestaw reguł domyślnych. Reguł domyślnych nie można usunąć, ale ponieważ mają przypisany najniższy priorytet, mogą być zastąpione przez tworzone zasady. 

Zgodnie z przedstawionymi poniżej regułami domyślnymi ruch pochodzący z sieci wirtualnej i kończący się w niej jest dozwolony zarówno w kierunku przychodzącym, jak i wychodzącym. Podczas gdy łączność z Internetem jest dozwolona dla kierunku wychodzącego, jest ona domyślnie blokowana dla ruchu przychodzącego. Istnieje reguła domyślna umożliwiająca modułowi równoważenia obciążenia Azure badanie kondycji maszyn wirtualnych i wystąpień ról. Tę zasadę można zastąpić, jeśli nie używa się zestawu z równoważeniem obciążenia.

**Reguły domyślne ruchu przychodzącego**

| Nazwa | Priorytet | Źródłowy adres IP | Port źródłowy | Docelowy adres IP | Port docelowy | Protokół | Dostęp |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ALLOW VNET INBOUND |65000 |VIRTUAL_NETWORK |* |VIRTUAL_NETWORK |* |* |ALLOW |
| ALLOW AZURE LOAD BALANCER INBOUND |65001 |AZURE_LOADBALANCER |* |* |* |* |ALLOW |
| DENY ALL INBOUND |65500 |* |* |* |* |* |DENY |

**Domyślne reguły ruchu wychodzącego**

| Nazwa | Priorytet | Źródłowy adres IP | Port źródłowy | Docelowy adres IP | Port docelowy | Protokół | Dostęp |
| --- | --- | --- | --- | --- | --- | --- | --- |
| ALLOW VNET OUTBOUND |65000 |VIRTUAL_NETWORK |* |VIRTUAL_NETWORK |* |* |ALLOW |
| ALLOW INTERNET OUTBOUND |65001 |* |* |INTERNET |* |* |ALLOW |
| DENY ALL OUTBOUND |65500 |* |* |* |* |* |DENY |

## <a name="associating-nsgs"></a>Kojarzenie sieciowych grup zabezpieczeń
W zależności od używanego modelu wdrażania można skojarzyć sieciową grupę zabezpieczeń z maszynami wirtualnymi, kartami sieciowymi i podsieciami.

* **Kojarzenie sieciowej grupy zabezpieczeń z maszyną wirtualną (tylko w przypadku wdrożeń klasycznych).** Po skojarzeniu sieciowej grupy zabezpieczeń z maszyną wirtualną reguły dostępu do sieci w sieciowej grupie zabezpieczeń są stosowane do całego ruchu skierowanego do maszyny wirtualnej i z niej wychodzącego. 
* **Kojarzenie sieciowej grupy zabezpieczeń z kartą sieciową (tylko w przypadku wdrożeń usługi Resource Manager).** Po skojarzeniu sieciowej grupy zabezpieczeń z kartą sieciową reguły dostępu do sieci w sieciowej grupie zabezpieczeń są stosowane tylko do tej karty sieciowej. Oznacza to, że jeśli na maszynie wirtualnej z wieloma kartami sieciowymi sieciowa grupa zabezpieczeń jest stosowana do jednej karty sieciowej, nie ma to wpływu na ruch powiązany z innymi kartami sieciowymi. 
* **Kojarzenie sieciowej grupy zabezpieczeń z podsiecią (wszystkie wdrożenia)**. Po skojarzeniu sieciowej grupy zabezpieczeń z podsiecią reguły dostępu do sieci w sieciowej grupie zabezpieczeń są stosowane do wszystkich zasobów IaaS i PaaS w podsieci. 

Można skojarzyć różne sieciowe grupy zabezpieczeń z maszyną wirtualną (lub kartą sieciową, w zależności od modelu wdrażania) i podsiecią, z którą jest powiązana karta sieciowa lub maszyna wirtualna. W takim przypadku wszystkie reguły dostępu do sieci są stosowane do ruchu sieciowego według priorytetu w poszczególnych sieciowych grupach zabezpieczeń, w następującej kolejności:

- **Ruch przychodzący**

  1. **Sieciowa grupa zabezpieczeń zastosowana do podsieci:** jeśli sieciowa grupa zabezpieczeń w podsieci zawiera regułę dopasowania w celu blokowania ruchu, pakiet zostanie porzucony.

  2. **Sieciowa grupa zabezpieczeń zastosowana do karty sieciowej** (Resource Manager) lub maszyny wirtualnej (klasycznej): jeśli sieciowa grupa zabezpieczeń maszyny wirtualnej lub karty sieciowej zawiera regułę dopasowania w celu blokowania ruchu, pakiet zostanie porzucony w maszynie wirtualnej lub karcie sieciowej, mimo że sieciowa grupa zabezpieczeń w podsieci ma regułę dopasowania zezwalającą na ruch.

- **Ruch wychodzący**

  1. **Sieciowa grupa zabezpieczeń zastosowana do karty sieciowej** (Resource Manager) lub maszyny wirtualnej (klasycznej): jeśli sieciowa grupa zabezpieczeń maszyny wirtualnej lub karty sieciowej zawiera regułę dopasowania w celu blokowania ruchu, pakiet zostanie porzucony.

  2. **Sieciowa grupa zabezpieczeń zastosowana do podsieci:** jeśli sieciowa grupa zabezpieczeń w podsieci zawiera regułę dopasowania w celu blokowania ruchu, pakiet zostanie porzucony w tym miejscu, mimo że sieciowa grupa zabezpieczeń maszyny wirtualnej lub karty sieciowej ma regułę dopasowania zezwalającą na ruch.

> [!NOTE]
> Chociaż z podsiecią można skojarzyć tylko jedną sieciowę grupę zabezpieczeń, maszynę wirtualną lub kartę sieciową, tę samą sieciową grupę zabezpieczeń można skojarzyć z dowolną liczbą zasobów.
>

## <a name="implementation"></a>Wdrażanie
Sieciowe grupy zabezpieczeń można wdrożyć w modelach wdrażania: klasycznym oraz usługi Resource Manager przy użyciu różnych wymienionych poniżej narzędzi.

| Narzędzie wdrażania | Wdrożenie klasyczne | Resource Manager |
| --- | --- | --- |
| Portal klasyczny | Nie  | Nie |
| Portal Azure   | Tak | [Tak](virtual-networks-create-nsg-arm-pportal.md) |
| PowerShell     | [Tak](virtual-networks-create-nsg-classic-ps.md) | [Tak](virtual-networks-create-nsg-arm-ps.md) |
| Interfejs wiersza polecenia platformy Azure      | [Tak](virtual-networks-create-nsg-classic-cli.md) | [Tak](virtual-networks-create-nsg-arm-cli.md) |
| Szablon ARM   | Nie  | [Tak](virtual-networks-create-nsg-arm-template.md) |

## <a name="planning"></a>Planowanie
Przed wdrożeniem sieciowych grup zabezpieczeń należy odpowiedzieć na następujące pytania:

1. Do jakich lub z jakich typów zasobów chcesz filtrować ruch (do i z kart sieciowych na tej samej maszynie wirtualnej, maszynach wirtualnych lub innych zasobów, takich jak usługi w chmurze lub środowiska usług aplikacji, połączonych z tą samą podsiecią czy pomiędzy zasobami połączonymi z różnymi podsieciami)?
2. Czy zasoby, do i z których chcesz filtrować ruch, są połączone z podsieciami w istniejących sieciach wirtualnych, czy będą połączone z nowymi sieciami wirtualnymi lub podsieciami?

Aby uzyskać więcej informacji na temat planowania zabezpieczeń sieciowych na platformie Azure, przeczytaj artykuł o [najlepszych rozwiązaniach dotyczących usług w chmurze i zabezpieczeń sieciowych](../best-practices-network-security.md). 

## <a name="design-considerations"></a>Zagadnienia dotyczące projektowania
Jeśli znasz już odpowiedzi na pytania w sekcji [Planowanie](#Planning), przed zdefiniowaniem sieciowych grup zabezpieczeń sprawdź następujące warunki.

### <a name="limits"></a>Limity
Podczas projektowania sieciowych grup zabezpieczeń musisz wziąć pod uwagę następujące limity.

| **Opis** | **Limit domyślny** | **Implikacje** |
| --- | --- | --- |
| Liczba sieciowych grup zabezpieczeń, które można powiązać z podsiecią, maszyną wirtualną lub kartą sieciową |1 |To oznacza, że nie można łączyć sieciowych grup zabezpieczeń. Pamiętaj, aby wszystkie reguły wymagane do określonego zestawu zasobów znajdowały się w jednej sieciowej grupie zabezpieczeń. |
| Sieciowe grupy zabezpieczeń na region na subskrypcję |100 |Domyślnie nowa sieciowa grupa zabezpieczeń jest tworzona dla każdej maszyny wirtualnej tworzonej w portalu Azure. Jeśli zezwolisz na to domyślne ustawienie, liczba dostępnych sieciowych grup zabezpieczeń szybko się wyczerpie. Pamiętaj, aby przestrzegać tego limitu podczas projektowania i w razie potrzeby podzielić zasoby na wiele regionów lub subskrypcji. |
| Reguły sieciowej grupy zabezpieczeń na sieciową grupę zabezpieczeń |200 |Używaj szerokiego zakresu adresów IP i portów, aby zagwarantować, że limit nie zostanie przekroczony. |

> [!IMPORTANT]
> Pamiętaj, aby przed projektowaniem rozwiązania przejrzeć wszystkie [limity dotyczące usług sieciowych na platformie Azure](../azure-subscription-service-limits.md#networking-limits). Niektóre limity można zwiększyć przez otwarcie biletu pomocy technicznej.
> 
> 

### <a name="vnet-and-subnet-design"></a>Projekt sieci wirtualnej i podsieci
Ze względu na to, że sieciowe grupy zabezpieczeń można zastosować do podsieci, można zminimalizować liczbę sieciowych grup zabezpieczeń przez grupowanie zasobów według podsieci i zastosowanie sieciowych grup zabezpieczeń do podsieci.  Jeśli zdecydujesz się zastosować sieciowe grupy zabezpieczeń do podsieci, może się okazać, że istniejące sieci wirtualne i podsieci nie zostały zdefiniowane z uwzględnieniem sieciowych grup zabezpieczeń. Może zajść konieczność zdefiniowania nowych sieci wirtualnych i podsieci obsługujących sieciowe grupy zabezpieczeń. a także wdrożenie nowych zasobów do nowych podsieci. Następnie można zdefiniować strategię migracji, aby przenieść istniejące zasoby do nowych podsieci. 

### <a name="special-rules"></a>Reguły specjalne
Należy wziąć pod uwagę wymienione poniżej reguły specjalne. Pamiętaj, aby nie blokować ruchu dozwolonego przez te reguły. W przeciwnym razie infrastruktura nie będzie mogła nawiązać połączenia z podstawowymi usługami Azure.

* **Wirtualny adres IP węzła hosta:** podstawowe usługi infrastruktury, takie jak DHCP, DNS i monitorowanie kondycji, są realizowane za pośrednictwem zwirtualizowanego adresu IP hosta 168.63.129.16. Ten publiczny adres IP należy do firmy Microsoft i jest jedynym zwirtualizowanym adresem IP używanym do tego celu we wszystkich regionach. Ten adres IP mapuje do fizycznego adresu IP komputera serwera (węzła hosta) obsługującego maszynę wirtualną. Węzeł hosta pełni rolę przekaźnika DHCP, cyklicznego programu rozpoznawania nazw DNS i źródła sondy kondycji modułu równoważenia obciążenia oraz sondy kondycji komputera. Połączenie z tym adresem IP nie powinno być uważane za atak.
* **Licencjonowanie (usługa zarządzania kluczami):** obrazy systemu Windows uruchomione na maszynach wirtualnych powinny być licencjonowane. W tym celu zostaje wysłane żądanie licencjonowania do serwerów hosta usługi zarządzania kluczami, które takie żądania obsługują. Zawsze ma to miejsce na porcie wychodzącym 1688.

### <a name="icmp-traffic"></a>Ruch protokołu ICMP
Bieżące reguły sieciowej grupy zabezpieczeń uwzględniają tylko protokoły *TCP* lub *UDP*. Nie ma określonego znacznika dla protokołu *ICMP*. Ruch protokołu ICMP jest jednak domyślnie dozwolony w ramach sieci wirtualnej przez regułę ruchu przychodzącego sieci wirtualnej (domyślna reguła 65000 ruchu przychodzącego), która zezwala na ruch od i do dowolnego portu i protokołu w ramach sieci wirtualnej.

### <a name="subnets"></a>Podsieci
* Weź pod uwagę liczbę warstw, której wymaga obciążenie. Każdą warstwę można wyizolować przy użyciu podsieci, do której z kolei można zastosować sieciową grupę zabezpieczeń. 
* Jeśli musisz wdrożyć podsieć dla bramy VPN lub obwodu ExpressRoute, pamiętaj, aby **NIE** stosować sieciowej grupy zabezpieczeń do tej podsieci. Jeśli tak zrobisz, nie będzie działać łączność pomiędzy różnymi sieciami wirtualnymi lub między różnymi lokalizacjami.
* Jeśli musisz wdrożyć urządzenie wirtualne, pamiętaj, aby wdrożyć je do jego własnej podsieci, aby trasy zdefiniowane przez użytkownika (UDR) mogły działać prawidłowo. Możesz wdrożyć sieciową grupę zabezpieczeń poziomu podsieci, aby filtrować ruch do i z tej podsieci. Dowiedz się więcej o [sterowaniu przepływem ruchu i używaniu urządzeń wirtualnych](virtual-networks-udr-overview.md).

### <a name="load-balancers"></a>Moduły równoważenia obciążenia
* Należy uwzględnić równoważenie obciążenia i reguły NAT dla każdego modułu równoważenia obciążenia używanego przez każde z obciążeń. Reguły te są powiązane z pulą wewnętrznej bazy danych, która zawiera karty sieciowe (wdrożenia usługi Resource Manager) lub maszyny wirtualne/wystąpienia ról (wdrożenia klasyczne). Należy rozważyć utworzenie sieciowej grupy zabezpieczeń dla każdej puli wewnętrznej bazy danych, zezwalającej tylko na ruch mapowany za pomocą reguł wdrożonych w modułach równoważenia obciążenia. Pozwala to zagwarantować, że ruch przychodzący bezpośrednio do puli wewnętrznej bazy danych z pominięciem modułu równoważenia obciążenia jest również filtrowany.
* We wdrożeniach klasycznych tworzy się punkty końcowe, które mapują porty w module równoważenia obciążenia na porty na maszynach wirtualnych lub w wystąpieniach ról. Można również utworzyć osobisty publiczny moduł równoważenia obciążenia we wdrożeniu usługi Resource Manager. W przypadku ograniczania za pomocą sieciowych grup zabezpieczeń ruchu do maszyn wirtualnych i wystąpień ról, które są częścią puli zaplecza w module równoważenia obciążenia, należy pamiętać, że port docelowy dla ruchu przychodzącego jest rzeczywistym portem na maszynie wirtualnej lub w wystąpieniu roli, a nie portem udostępnianym przez moduł równoważenia obciążenia. Należy również pamiętać, że port źródłowy i adres dla połączenia z maszyną wirtualną to port i adres na komputerze zdalnym w Internecie, a nie port i adres udostępniane przez moduł równoważenia obciążenia.
* Podobnie jak w przypadku publicznych modułów równoważenia obciążenia podczas tworzenia sieciowych grup zabezpieczeń do filtrowania ruchu przechodzącego przez wewnętrzny moduł równoważenia obciążenia należy mieć świadomość, że zastosowany port źródłowy i zakres adresów dotyczą komputera, z którego zostało przesłane wywołanie, a nie modułu równoważnia obciążenia. Port docelowy i zakres adresu są powiązane z komputerem odbierającym ruch, a nie z modułem równoważenia obciążenia.

### <a name="other"></a>Inne
* Listy ACL oparte na punktach końcowych i sieciowe grupy zabezpieczeń nie są obsługiwane w tym samym wystąpieniu maszyny wirtualnej. Jeśli chcesz użyć sieciowej grupy zabezpieczeń, a masz już listę ACL punktów końcowych, najpierw usuń listę ACL punktów końcowych. Aby dowiedzieć się, jak to zrobić, zobacz temat [Manage endpoint ACLs](virtual-networks-acl-powershell.md) (Zarządzanie listami ACL opartymi na punktach końcowych).
* W modelu wdrażania usługi Resource Manager można użyć sieciowej grupy zabezpieczeń skojarzonej z kartą sieciową dla maszyn wirtualnych z wieloma kartami sieciowymi, aby włączyć zarządzanie (dostęp zdalny) przez karty sieciowe, a tym samym rozdzielić ruch.
* Podobnie jak w przypadku modułów równoważenia obciążenia podczas filtrowania ruchu z innych sieci wirtualnych należy użyć zakresu adresów źródłowych komputera zdalnego, a nie bramy łączącej sieci wirtualne.
* Wielu usług Azure nie można podłączyć do sieci wirtualnych Azure, tak więc ruchu z nich i do nich nie można filtrować za pomocą sieciowych grup zabezpieczeń.  Przeczytaj dokumentację dotyczącą usług, których używasz, aby ustalić, czy można je podłączyć do sieci wirtualnych.

## <a name="sample-deployment"></a>Przykładowe wdrożenie
Aby przedstawić, jak zastosować informacje z tego artykułu w praktyce, zdefiniujemy sieciowe grupy zabezpieczeń do filtrowania ruchu sieciowego dla rozwiązania obejmującego obciążenie dwuwarstwowe o następujących wymaganiach:

1. Rozdzielenie ruchu między frontonem (serwery sieci Web systemu Windows) i wewnętrzną bazą danych (serwery baz danych SQL).
2. Przesyłanie ruchu w oparciu o reguły równoważenia obciążenia do modułu równoważenia obciążenia do wszystkich serwerów sieci Web na porcie 80.
3. Przesyłanie ruchu w oparciu o reguły NAT przychodzącego na porcie 50001 w module równoważenia obciążenia do portu 3389 tylko na jednej maszynie wirtualnej frontonu.
4. Brak dostępu do maszyn wirtualnych frontonu lub wewnętrznej bazy danych z Internetu, z wyjątkiem wymagania numer 1.
5. Brak dostępu z frontonu lub wewnętrznej bazy danych do Internetu.
6. Dostęp do portu 3389 na dowolnym serwerze sieci Web frontonu dla ruchu przychodzącego z samej podsieci frontonu.
7. Dostęp do portu 3389 na wszystkich maszynach wirtualnych serwera SQL w wewnętrznej bazie danych tylko z podsieci frontonu.
8. Dostęp do portu 1433 na wszystkich maszynach wirtualnych serwera SQL w wewnętrznej bazie danych tylko z podsieci frontonu.
9. Rozdzielenie ruchu administracyjnego (port 3389) i ruchu bazy danych (1433) na różne karty sieciowe na maszynach wirtualnych wewnętrznej bazy danych.

![Sieciowe grupy zabezpieczeń](./media/virtual-network-nsg-overview/figure1.png)

W schemacie powyżej maszyny wirtualne *Web 1* i *Web 2* są połączone z podsiecią *FrontEnd*, a maszyny wirtualne *DB1* i *DB2* są połączone z podsiecią *BackEnd*.  Obie podsieci są częścią sieci wirtualnej *TestVNet*. Wszystkie zasoby są przypisane do regionu Azure *Zachodnie stany USA*.

Wymagania 1–6 (z wyjątkiem 3) powyżej ograniczają się do przestrzeni podsieci. Aby zminimalizować liczbę reguł wymaganych dla każdej sieciowej grupy zabezpieczeń oraz ułatwić dodawanie kolejnych maszyn wirtualnych do podsieci z uruchomionymi tymi samymi typami obciążenia co na istniejących maszynach wirtualnych, możemy wdrożyć następujące sieciowe grupy zabezpieczeń na poziomie podsieci.

### <a name="nsg-for-frontend-subnet"></a>Sieciowa grupa zabezpieczeń dla podsieci FrontEnd
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zezwalaj na HTTP |Zezwalaj |100 |INTERNET |\* |\* |80 |TCP |
| zezwalaj na RDP z frontonu |Zezwalaj |200 |192.168.1.0/24 |\* |\* |3389 |TCP |
| zablokuj wszystko z Internetu |Zablokuj |300 |INTERNET |\* |\* |\* |TCP |

**Reguły wychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zablokuj Internet |Zablokuj |100 |\* |\* |INTERNET |\* |\* |

### <a name="nsg-for-backend-subnet"></a>Sieciowa grupa zabezpieczeń podsieci wewnętrznej bazy danych
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zablokuj Internet |Zablokuj |100 |INTERNET |\* |\* |\* |\* |

**Reguły wychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zablokuj Internet |Zablokuj |100 |\* |\* |INTERNET |\* |\* |

### <a name="nsg-for-single-vm-nic-in-frontend-for-rdp-from-internet"></a>Sieciowa grupa zabezpieczeń dla jednej maszyny wirtualnej (karty sieciowej) frontonu dla protokołu RDP z Internetu
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zezwalaj na RDP z Internetu |Zezwalaj |100 |INTERNET |* |\* |3389 |TCP |

> [!NOTE]
> Zwróć uwagę, że zakresem adresu źródłowego dla tej reguły jest **Internet**, a nie adres VIP modułu równoważenia obciążenia; portem źródłowym jest **\***, a nie 500001. Nie pomyl reguł NAT/reguł modułu równoważenia obciążenia z regułami sieciowej grupy zabezpieczeń. Reguły sieciowej grupy zabezpieczeń są zawsze powiązane z oryginalnym źródłem i ostatecznym miejscem przeznaczenia ruchu, a **NIE** modułem równoważenia obciążenia między nimi. 
> 
> 

### <a name="nsg-for-management-nics-in-backend"></a>Sieciowa grupa zabezpieczeń do zarządzania kartami sieciowymi w wewnętrznej bazie danych
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zezwalaj na RDP z frontonu |Zezwalaj |100 |192.168.1.0/24 |* |\* |3389 |TCP |

### <a name="nsg-for-database-access-nics-in-back-end"></a>Sieciowa grupa zabezpieczeń do kart sieciowych dostępu do bazy danych w wewnętrznej bazie danych
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zezwalaj na SQL z frontonu |Zezwalaj |100 |192.168.1.0/24 |* |\* |1433 |TCP |

Ponieważ niektóre z sieciowych grup zabezpieczeń powyżej muszą być powiązane z poszczególnymi kartami sieciowymi, należy wdrożyć ten scenariusz jako wdrożenie usługi Resource Manager. Zwróć uwagę na to, jak reguły są łączone na poziomie podsieci i karty sieciowej w zależności od tego, jak powinny być zastosowane. 

## <a name="next-steps"></a>Następne kroki
* [Deploy NSGs in the classic deployment model](virtual-networks-create-nsg-classic-ps.md) (Wdrażanie sieciowych grup zabezpieczeń w klasycznym modelu wdrażania).
* [Deploy NSGs in Resource Manager](virtual-networks-create-nsg-arm-pportal.md) (Wdrażanie sieciowych grup zabezpieczeń we wdrożeniu za pomocą usługi Resource Manager).
* [Manage NSG logs](virtual-network-nsg-manage-log.md) (Zarządzanie dziennikami sieciowej grupy zabezpieczeń).



<!--HONumber=Jan17_HO1-->


