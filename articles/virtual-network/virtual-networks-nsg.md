---
title: Co to jest grupa zabezpieczeń sieci (Network Security Group, NSG)
description: Informacje na temat rozproszonej zapory w systemie Azure korzystającej z grup zabezpieczeń sieci oraz użycia grup NSG do izolowania przepływu ruchu oraz sterowania nim w sieciach wirtualnych.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn

ms.service: virtual-network
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/11/2016
ms.author: jdial

---
# Co to jest grupa zabezpieczeń sieci?
Grupa zabezpieczeń sieci zawiera listę reguł listy kontroli dostępu (ACL), które blokują lub zezwalają na ruch sieciowy do wystąpień maszyn wirtualnych w sieci wirtualnej. Grupy NSG można kojarzyć z podsieciami lub poszczególnymi wystąpieniami maszyn wirtualnych w danej podsieci. Gdy grupa NSG jest skojarzona z podsiecią, reguły listy ACL dotyczą wszystkich wystąpień maszyn wirtualnych w tej podsieci. Ponadto ruch do poszczególnych maszyn wirtualnych można ograniczyć jeszcze bardziej przez skojarzenie grupy NSG bezpośrednio z tą maszyną wirtualną.

## Zasób NSG
Grupy NSG obejmują następujące właściwości.

| Właściwość | Opis | Ograniczenia | Zagadnienia do rozważenia |
| --- | --- | --- | --- |
| Nazwa |Nazwa grupy NSG |Musi być unikatowa w obrębie regionu<br/>Może zawierać litery, cyfry, podkreślenia, kropki i łączniki<br/>Musi zaczynać się literą lub cyfrą<br/>Musi kończyć się literą, cyfrą lub podkreśleniem<br/>Może zawierać maksymalnie 80 znaków |Ponieważ może być konieczne utworzenie kilku grup NSG, upewnij się, że dysponujesz konwencją nazewnictwa, która pozwala łatwo identyfikować ich funkcję |
| Region |Region platformy Azure, w którym grupa NSG jest obsługiwana |Grupy NSG mogą być stosowane wyłącznie do zasobów w regionie, w którym zostały utworzone |Zobacz sekcję [Limity](#Limits) poniżej, która zawiera informacje na temat liczby grup NSG, która może znajdować się w regionie |
| Grupa zasobów |Grupa zasobów, do której należy grupa NSG |Chociaż grupa NSG należy do grupy zasobów, może być skojarzona z zasobami w dowolnej grupie zasobów, jeśli tylko zasób jest częścią tego samego regionu Azure co grupa NSG |Grupy zasobów służą do zarządzania wieloma zasobami równocześnie w ramach jednostki wdrożenia<br/>Można rozważyć zgrupowanie grupy NSG z zasobami, z którymi jest ona skojarzona |
| Reguły |Reguły określające, jaki ruch jest dozwolony lub zablokowany | |Zobacz [Reguły grupy NSG](#Nsg-rules) poniżej |

> [!NOTE]
> Listy ACL oparte na punktach końcowych i sieciowe grupy zabezpieczeń nie są obsługiwane w tym samym wystąpieniu maszyny wirtualnej. Jeśli chcesz użyć grupy NSG, a masz już listę ACL punktów końcowych, najpierw usuń listę ACL punktów końcowych. Aby dowiedzieć się, jak to zrobić, zobacz artykuł [Managing Access Control Lists (ACLs) for Endpoints by using PowerShell](virtual-networks-acl-powershell.md) (Zarządzanie listami kontroli dostępu (ACL) dla punktów końcowych przy użyciu programu PowerShell).
> 
> 

### Reguły NSG
Reguły NSG obejmują następujące właściwości.

| Właściwość | Opis | Ograniczenia | Zagadnienia do rozważenia |
| --- | --- | --- | --- |
| **Nazwa** |Nazwa reguły |Musi być unikatowa w obrębie regionu<br/>Może zawierać litery, cyfry, podkreślenia, kropki i łączniki<br/>Musi zaczynać się literą lub cyfrą<br/>Musi kończyć się literą, cyfrą lub podkreśleniem<br/>Może zawierać maksymalnie 80 znaków |Grupa NSG może zawierać kilka reguł, więc pamiętaj, aby postępować zgodnie z konwencją nazewnictwa, która umożliwia zidentyfikowanie funkcji reguły |
| **Protokół** |Protokół odpowiadający regule |TCP, UDP lub \* |Wstawienie znaku \* oznacza protokół ICMP (tylko ruch wschód-zachód), a także protokołów UDP i TCP i może zmniejszyć liczbę potrzebnych reguł<br/>Z drugiej strony użycie znaku \* może okazać się rozwiązaniem zbyt ogólnym, więc pamiętaj, aby używać go tylko w razie konieczności |
| **Zakres portów źródłowych** |Zakres portów źródłowych odpowiadający regule |Numer pojedynczego portu od 1 do 65535, zakres portu (tj. 1-65635) lub \* (dla wszystkich portów) |Porty źródłowe mogą być efemeryczne. Jeśli program kliencki nie korzysta z określonego portu, należy w większości przypadków użyć znaku „*”.<br/>Używaj zakresów portów możliwie często, aby uniknąć konieczności korzystania z wielu reguł<br/>Wielu portów lub zakresów portów nie można oddzielać przecinkiem |
| **Zakres portów docelowych** |Zakres portów docelowych odpowiadający regule |Numer pojedynczego portu od 1 do 65535, zakres portu (tj. 1-65535) lub \* (dla wszystkich portów) |Używaj zakresów portów możliwie często, aby uniknąć konieczności korzystania z wielu reguł<br/>Wielu portów lub zakresów portów nie można oddzielać przecinkiem |
| **Prefiks adresu źródłowego** |Prefiks adresu źródłowego lub znacznik odpowiadający regule |Pojedynczy adres IP (tj. 10.10.10.10), podsieć IP (tj. 192.168.1.0/24), [znacznik domyślny](#default-tags) lub * (dla wszystkich adresów) |Należy rozważyć użycie zakresów, znaczników domyślnych i znaków * w celu zmniejszenia liczby reguł |
| **Prefiks adresu docelowego** |Prefiks adresu docelowego lub znacznik odpowiadający regule |pojedynczy adres IP (tj. 10.10.10.10), podsieć IP (tj. 192.168.1.0/24), [znacznik domyślny](#default-tags) lub * (dla wszystkich adresów) |Należy rozważyć użycie zakresów, znaczników domyślnych i znaków * w celu zmniejszenia liczby reguł |
| **Kierunek** |Kierunek ruchu odpowiadający regule |ruch przychodzący lub wychodzący |Reguły ruchu przychodzącego i wychodzącego są przetwarzane oddzielnie w zależności od kierunku |
| **Priorytet** |Reguły są sprawdzane według ważności. Gdy reguła ma zastosowanie, żadne inne reguły nie są sprawdzane pod kątem dopasowania |Liczba z zakresu od 100 do 4096 |Należy rozważyć utworzenie reguł przez przeskoczenie priorytetów o 100 dla każdej reguły, aby zostawić miejsce na nowe reguły, które pojawią się między istniejącymi |
| **Dostęp** |Typ dostępu do zastosowania, jeśli reguła jest dopasowana |zezwolenie lub zablokowanie |Pamiętaj, że jeśli dla pakietu nie zostanie odnaleziona reguła zezwalająca, zostanie on porzucony |

Grupy NSG zawierają dwa zestawy reguł: zestaw reguł przychodzących i wychodzących. Priorytety reguł muszą być unikatowe w poszczególnych zestawach. 

![Przetwarzanie reguł NSG](./media/virtual-network-nsg-overview/figure3.png) 

Na rysunku powyżej przedstawiono, jak są przetwarzane reguły NSG.

### Znaczniki domyślne
Znaczniki domyślne są dostarczanymi przez system identyfikatorami określającymi kategorię adresów IP. Można użyć znaczników domyślnych we właściwościach **prefiksu adresu źródłowego** i **prefiksu adresu docelowego** dowolnej reguły. Istnieją trzy znaczniki domyślne, których można użyć.

* **VIRTUAL_NETWORK:** określa całą przestrzeń adresową sieci. Obejmuje przestrzeń adresową sieci wirtualnej (zakresy CIDR określone na platformie Azure), a także wszystkie połączone lokalne przestrzenie adresowe i połączone sieci wirtualne Azure (sieci lokalne).
* **AZURE_LOADBALANCER:** określa moduł równoważenia obciążenia infrastruktury platformy Azure. Przekłada się on na adres IP centrum danych Azure, z którego pochodzą sondy kondycji Azure.
* **INTERNET:** określa przestrzeń adresów IP, która znajduje się poza siecią wirtualną i do której można uzyskać dostęp w publicznym Internecie. Ten zakres obejmuje również [publiczną przestrzeń adresów IP należącą do Azure](https://www.microsoft.com/download/details.aspx?id=41653).

### Reguły domyślne
Wszystkie grupy NSG zawierają zestaw reguł domyślnych. Reguł domyślnych nie można usunąć, ale ponieważ mają przypisany najniższy priorytet, mogą być zastąpione przez tworzone zasady. 

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

## Kojarzenie grup NSG
W zależności od używanego modelu wdrażania można skojarzyć grupę NSG z maszynami wirtualnymi, kartami sieciowymi i podsieciami.

[!INCLUDE [learn-about-deployment-models-both-include.md](../../includes/learn-about-deployment-models-both-include.md)]

* **Kojarzenie grupy NSG z maszyną wirtualną (tylko w przypadku wdrożeń klasycznych).** Po skojarzeniu grupy NSG z maszyną wirtualną reguły dostępu do sieci w grupie NSG są stosowane do całego ruchu skierowanego do maszyny wirtualnej i z niej wychodzącego. 
* **Kojarzenie grupy NSG z kartą sieciową (tylko w przypadku wdrożeń usługi Resource Manager).** Po skojarzeniu grupy NSG z kartą sieciową reguły dostępu do sieci w grupie NSG są stosowane tylko do tej karty sieciowej. Oznacza to, że jeśli na maszynie wirtualnej z wieloma kartami sieciowymi grupa NSG jest stosowana do jednej karty sieciowej, nie ma to wpływu na ruch powiązany z innymi kartami sieciowymi. 
* **Kojarzenie grupy NSG z podsiecią (wszystkie wdrożenia)**. Po skojarzeniu grupy NSG z podsiecią reguły dostępu do sieci w grupie NSG są stosowane do wszystkich zasobów IaaS i PaaS w podsieci. 

Można skojarzyć różne grupy NSG z maszyną wirtualną (lub kartą sieciową, w zależności od modelu wdrażania) i podsiecią, z którą jest powiązana karta sieciowa lub maszyna wirtualna. W takim przypadku wszystkie reguły dostępu do sieci są stosowane do ruchu sieciowego według priorytetu w poszczególnych grupach NSG, w następującej kolejności:

* **Ruch przychodzący**
  
  1. Grupa NSG stosowana do podsieci. 
     
         If subnet NSG has a matching rule to deny traffic, packet will be dropped here.
  2. Grupa NSG stosowana do karty sieciowej (Resource Manager) lub maszyny wirtualnej (wdrożenia klasyczne). 
     
         If VM\NIC NSG has a matching rule to deny traffic, packet will be dropped at VM\NIC, although subnet NSG has a matching rule to allow traffic.
* **Ruch wychodzący**
  
  1. Grupa NSG stosowana do karty sieciowej (Resource Manager) lub maszyny wirtualnej (wdrożenia klasyczne). 
     
         If VM\NIC NSG has a matching rule to deny traffic, packet will be dropped here.
  2. Grupa NSG stosowana do podsieci.
     
         If subnet NSG has a matching rule to deny traffic, packet will be dropped here, although VM\NIC NSG has a matching rule to allow traffic.
     
     ![Listy ACL grupy NSG](./media/virtual-network-nsg-overview/figure2.png)

> [!NOTE]
> Chociaż z podsiecią można skojarzyć tylko jedną grupę NSG, maszynę wirtualną lub kartę sieciową, tę samą grupę NSG można skojarzyć z dowolną liczbą zasobów.
> 
> 

## Wdrażanie
Grupy NSG można wdrożyć w modelach wdrażania: klasycznym oraz usługi Resource Manager przy użyciu różnych wymienionych poniżej narzędzi.

| Narzędzie wdrażania | Wdrożenie klasyczne | Resource Manager |
| --- | --- | --- |
| Portal klasyczny |![Nie](./media/virtual-network-nsg-overview/red.png) |![Nie](./media/virtual-network-nsg-overview/red.png) |
| Portal Azure |![Tak](./media/virtual-network-nsg-overview/green.png) |[![Tak][zielony]](virtual-networks-create-nsg-arm-pportal.md) |
| PowerShell |[![Tak][zielony]](virtual-networks-create-nsg-classic-ps.md) |[![Tak][zielony]](virtual-networks-create-nsg-arm-ps.md) |
| Interfejs wiersza polecenia platformy Azure |[![Tak][zielony]](virtual-networks-create-nsg-classic-cli.md) |[![Tak][zielony]](virtual-networks-create-nsg-arm-cli.md) |
| Szablon ARM |![Nie](./media/virtual-network-nsg-overview/red.png) |[![Tak][zielony]](virtual-networks-create-nsg-arm-template.md) |

| **Klucz** | ![Tak](./media/virtual-network-nsg-overview/green.png) Obsługiwane. | ![Nie](./media/virtual-network-nsg-overview/red.png) Nieobsługiwane. |
| --- | --- | --- |
|  | | |

## Planowanie
Przed wdrożeniem grup NSG należy odpowiedzieć na pytania poniżej:   

1. Do jakich lub z jakich typów zasobów chcesz filtrować ruch (do i z kart sieciowych na tej samej maszynie wirtualnej, maszynach wirtualnych lub innych zasobów, takich jak usługi w chmurze lub środowiska usług aplikacji, połączonych z tą samą podsiecią czy pomiędzy zasobami połączonymi z różnymi podsieciami)?
2. Czy zasoby, do i z których chcesz filtrować ruch, są połączone z podsieciami w istniejących sieciach wirtualnych, czy będą połączone z nowymi sieciami wirtualnymi lub podsieciami?

Aby uzyskać więcej informacji na temat planowania zabezpieczeń sieciowych na platformie Azure, przeczytaj artykuł o [najlepszych rozwiązaniach dotyczących usług w chmurze i zabezpieczeń sieciowych](../best-practices-network-security.md). 

## Zagadnienia dotyczące projektowania
Jeśli znasz już odpowiedzi na pytania w sekcji [Planowanie](#Planning), przed zdefiniowaniem grup NSG sprawdź następujące warunki.

### Limity
Podczas projektowania grup NSG musisz wziąć pod uwagę następujące limity.

| **Opis** | **Limit domyślny** | **Implikacje** |
| --- | --- | --- |
| Liczba grup NSG, które można powiązać z podsiecią, maszyną wirtualną lub kartą sieciową |1 |To oznacza, że nie można łączyć grup NSG. Pamiętaj, aby wszystkie reguły wymagane do określonego zestawu zasobów znajdowały się w jednej grupie NSG. |
| Grupy NSG na region na subskrypcję |100 |Domyślnie nowa grupa NSG jest tworzona dla każdej maszyny wirtualnej tworzonej w portalu Azure. Jeśli zezwolisz na to domyślne ustawienie, liczba dostępnych grup NSG szybko się wyczerpie. Pamiętaj, aby przestrzegać tego limitu podczas projektowania i w razie potrzeby podzielić zasoby na wiele regionów lub subskrypcji. |
| Reguły NSG na grupę NSG |200 |Używaj szerokiego zakresu adresów IP i portów, aby zagwarantować, że limit nie zostanie przekroczony. |

> [!IMPORTANT]
> Pamiętaj, aby przed projektowaniem rozwiązania przejrzeć wszystkie [limity dotyczące usług sieciowych na platformie Azure](../azure-subscription-service-limits.md#networking-limits). Niektóre limity można zwiększyć przez otwarcie biletu pomocy technicznej.
> 
> 

### Projekt sieci wirtualnej i podsieci
Ze względu na to, że grupy NSG można zastosować do podsieci, można zminimalizować liczbę grup NSG przez grupowanie zasobów według podsieci i zastosowanie grup NSG do podsieci.  Jeśli zdecydujesz się zastosować grupy NSG do podsieci, może się okazać, że istniejące sieci wirtualne i podsieci nie zostały zdefiniowane z uwzględnieniem grup NSG. Może zajść konieczność zdefiniowania nowych sieci wirtualnych i podsieci obsługujących grupy NSG, a także wdrożenie nowych zasobów do nowych podsieci. Następnie można zdefiniować strategię migracji, aby przenieść istniejące zasoby do nowych podsieci. 

### Reguły specjalne
Należy wziąć pod uwagę wymienione poniżej reguły specjalne. Pamiętaj, aby nie blokować ruchu dozwolonego przez te reguły. W przeciwnym razie infrastruktura nie będzie mogła nawiązać połączenia z podstawowymi usługami Azure.

* **Wirtualny adres IP węzła hosta:** podstawowe usługi infrastruktury, takie jak DHCP, DNS i monitorowanie kondycji, są realizowane za pośrednictwem zwirtualizowanego adresu IP hosta 168.63.129.16. Ten publiczny adres IP należy do firmy Microsoft i jest jedynym zwirtualizowanym adresem IP używanym do tego celu we wszystkich regionach. Ten adres IP mapuje do fizycznego adresu IP komputera serwera (węzła hosta) obsługującego maszynę wirtualną. Węzeł hosta pełni rolę przekaźnika DHCP, cyklicznego programu rozpoznawania nazw DNS i źródła sondy kondycji modułu równoważenia obciążenia oraz sondy kondycji komputera. Połączenie z tym adresem IP nie powinno być uważane za atak.
* **Licencjonowanie (usługa zarządzania kluczami):** obrazy systemu Windows uruchomione na maszynach wirtualnych powinny być licencjonowane. W tym celu zostaje wysłane żądanie licencjonowania do serwerów hosta usługi zarządzania kluczami, które takie żądania obsługują. Zawsze ma to miejsce na porcie wychodzącym 1688.

### Ruch protokołu ICMP
Bieżące reguły NSG uwzględniają tylko protokoły *TCP* lub *UDP*. Nie ma określonego znacznika dla protokołu *ICMP*. Ruch protokołu ICMP jest jednak domyślnie dozwolony w ramach sieci wirtualnej przez regułę ruchu przychodzącego sieci wirtualnej (domyślna reguła 65000 ruchu przychodzącego), która zezwala na ruch od i do dowolnego portu i protokołu w ramach sieci wirtualnej.

### Podsieci
* Weź pod uwagę liczbę warstw, której wymaga obciążenie. Każdą warstwę można wyizolować przy użyciu podsieci, do której z kolei można zastosować grupę NSG. 
* Jeśli musisz wdrożyć podsieć dla bramy VPN lub obwodu ExpressRoute, pamiętaj, aby **NIE** stosować grupy NSG do tej podsieci. Jeśli tak zrobisz, nie będzie działać łączność pomiędzy różnymi sieciami wirtualnymi lub między różnymi lokalizacjami.
* Jeśli musisz wdrożyć urządzenie wirtualne, pamiętaj, aby wdrożyć je do jego własnej podsieci, aby trasy zdefiniowane przez użytkownika (UDR) mogły działać prawidłowo. Możesz wdrożyć grupę NSG poziomu podsieci, aby filtrować ruch do i z tej podsieci. Dowiedz się więcej o [sterowaniu przepływem ruchu i używaniu urządzeń wirtualnych](virtual-networks-udr-overview.md).

### Moduły równoważenia obciążenia
* Należy uwzględnić równoważenie obciążenia i reguły NAT dla każdego modułu równoważenia obciążenia używanego przez każde z obciążeń. Reguły te są powiązane z pulą wewnętrznej bazy danych, która zawiera karty sieciowe (wdrożenia usługi Resource Manager) lub maszyny wirtualne/wystąpienia ról (wdrożenia klasyczne). Należy rozważyć utworzenie grupy NSG dla każdej puli wewnętrznej bazy danych, zezwalającej tylko na ruch mapowany za pomocą reguł wdrożonych w modułach równoważenia obciążenia. Pozwala to zagwarantować, że ruch przychodzący bezpośrednio do puli wewnętrznej bazy danych z pominięciem modułu równoważenia obciążenia jest również filtrowany.
* We wdrożeniach klasycznych tworzy się punkty końcowe, które mapują porty w module równoważenia obciążenia na porty na maszynach wirtualnych lub w wystąpieniach ról. Można również utworzyć osobisty publiczny moduł równoważenia obciążenia we wdrożeniu usługi Resource Manager. W przypadku ograniczania ruchu do maszyn wirtualnych i wystąpień ról, które są częścią puli wewnętrznej bazy danych, za pomocą modułu równoważenia obciążenia za pomocą grup NSG, należy pamiętać, że port docelowy dla ruchu przychodzącego jest rzeczywistym portem na maszynie wirtualnej lub w wystąpieniu roli, a nie portem udostępnianym przez moduł równoważenia obciążenia. Należy również pamiętać, że port źródłowy i adres dla połączenia z maszyną wirtualną to port i adres na komputerze zdalnym w Internecie, a nie port i adres udostępniane przez moduł równoważenia obciążenia.
* Podobnie jak w przypadku publicznych modułów równoważenia obciążenia podczas tworzenia grup NSG do filtrowania ruchu przechodzącego przez wewnętrzny moduł równoważenia obciążenia należy mieć świadomość, że zastosowany port źródłowy i zakres adresów dotyczą komputera, z którego zostało przesłane wywołanie, a nie modułu równoważnia obciążenia. Port docelowy i zakres adresu są powiązane z komputerem odbierającym ruch, a nie z modułem równoważenia obciążenia.

### Inne
* Listy ACL oparte na punktach końcowych i grupy NSG nie są obsługiwane w tym samym wystąpieniu maszyny wirtualnej. Jeśli chcesz użyć grupy NSG, a masz już listę ACL punktów końcowych, najpierw usuń listę ACL punktów końcowych. Aby dowiedzieć się, jak to zrobić, zobacz temat [Manage endpoint ACLs](virtual-networks-acl-powershell.md) (Zarządzanie listami ACL opartymi na punktach końcowych).
* W modelu wdrażania usługi Resource Manager można użyć grupy NSG skojarzonej z kartą sieciową dla maszyn wirtualnych z wieloma kartami sieciowymi, aby włączyć zarządzanie (dostęp zdalny) przez karty sieciowe, a tym samym rozdzielić ruch.
* Podobnie jak w przypadku modułów równoważenia obciążenia podczas filtrowania ruchu z innych sieci wirtualnych należy użyć zakresu adresów źródłowych komputera zdalnego, a nie bramy łączącej sieci wirtualne.
* Wielu usług Azure nie można podłączyć do sieci wirtualnych Azure, tak więc ruchu z nich i do nich nie można filtrować za pomocą grup NSG.  Przeczytaj dokumentację dotyczącą usług, których używasz, aby ustalić, czy można je podłączyć do sieci wirtualnych.

## Przykładowe wdrożenie
Aby przedstawić, jak zastosować informacje z tego artykułu w praktyce, zdefiniujemy grupy NSG do filtrowania ruchu sieciowego dla rozwiązania obejmującego obciążenie dwuwarstwowe o następujących wymaganiach:

1. Rozdzielenie ruchu między frontonem (serwery sieci Web systemu Windows) i wewnętrzną bazą danych (serwery baz danych SQL).
2. Przesyłanie ruchu w oparciu o reguły równoważenia obciążenia do modułu równoważenia obciążenia do wszystkich serwerów sieci Web na porcie 80.
3. Przesyłanie ruchu w oparciu o reguły NAT przychodzącego na porcie 50001 w module równoważenia obciążenia do portu 3389 tylko na jednej maszynie wirtualnej frontonu.
4. Brak dostępu do maszyn wirtualnych frontonu lub wewnętrznej bazy danych z Internetu, z wyjątkiem wymagania numer 1.
5. Brak dostępu z frontonu lub wewnętrznej bazy danych do Internetu.
6. Dostęp do portu 3389 na dowolnym serwerze sieci Web frontonu dla ruchu przychodzącego z samej podsieci frontonu.
7. Dostęp do portu 3389 na wszystkich maszynach wirtualnych serwera SQL w wewnętrznej bazie danych tylko z podsieci frontonu.
8. Dostęp do portu 1433 na wszystkich maszynach wirtualnych serwera SQL w wewnętrznej bazie danych tylko z podsieci frontonu.
9. Rozdzielenie ruchu administracyjnego (port 3389) i ruchu bazy danych (1433) na różne karty sieciowe na maszynach wirtualnych wewnętrznej bazy danych.

![Grupy NSG](./media/virtual-network-nsg-overview/figure1.png)

W schemacie powyżej maszyny wirtualne *Web 1* i *Web 2* są połączone z podsiecią *FrontEnd*, a maszyny wirtualne *DB1* i *DB2* są połączone z podsiecią *BackEnd*.  Obie podsieci są częścią sieci wirtualnej *TestVNet*. Wszystkie zasoby są przypisane do regionu Azure *Zachodnie stany USA*.

Wymagania 1–6 (z wyjątkiem 3) powyżej ograniczają się do przestrzeni podsieci. Aby zminimalizować liczbę reguł wymaganych dla każdej grupy NSG oraz ułatwić dodawanie kolejnych maszyn wirtualnych do podsieci z uruchomionymi tymi samymi typami obciążenia co na istniejących maszynach wirtualnych, możemy wdrożyć następujące grupy NSG na poziomie podsieci.

### Grupa NSG dla podsieci FrontEnd
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

### NSG podsieci wewnętrznej bazy danych
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zablokuj Internet |Zablokuj |100 |INTERNET |\* |\* |\* |\* |

**Reguły wychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zablokuj Internet |Zablokuj |100 |\* |\* |INTERNET |\* |\* |

### Grupa NSG dla jednej maszyny wirtualnej (karty sieciowej) frontonu dla protokołu RDP z Internetu
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zezwalaj na RDP z Internetu |Zezwalaj |100 |INTERNET |* |\* |3389 |TCP |

> [!NOTE]
> Zwróć uwagę, że zakresem adresu źródłowego dla tej reguły jest **Internet**, a nie adres VIP modułu równoważenia obciążenia; portem źródłowym jest **\***, a nie 500001. Nie pomyl reguł NAT/reguł modułu równoważenia obciążenia z regułami NSG. Reguły NSG są zawsze powiązane z oryginalnym źródłem i ostatecznym miejscem przeznaczenia ruchu, a **NIE** modułem równoważenia obciążenia między nimi. 
> 
> 

### Grupa NSG do zarządzania kartami sieciowymi w wewnętrznej bazie danych
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zezwalaj na RDP z frontonu |Zezwalaj |100 |192.168.1.0/24 |* |\* |3389 |TCP |

### Grupa NSG do kart sieciowych dostępu do bazy danych w wewnętrznej bazie danych
**Reguły przychodzące**

| Reguła | Dostęp | Priorytet | Zakres adresów źródłowych | Port źródłowy | Zakres adresów docelowych | Port docelowy | Protokół |
| --- | --- | --- | --- | --- | --- | --- | --- |
| zezwalaj na SQL z frontonu |Zezwalaj |100 |192.168.1.0/24 |* |\* |1433 |TCP |

Ponieważ niektóre z grup NSG powyżej muszą być powiązane z poszczególnymi kartami sieciowymi, należy wdrożyć ten scenariusz jako wdrożenie usługi Resource Manager. Zwróć uwagę na to, jak reguły są łączone na poziomie podsieci i karty sieciowej w zależności od tego, jak powinny być zastosowane. 

## Następne kroki
* [Deploy NSGs in the classic deployment model](virtual-networks-create-nsg-classic-ps.md) (Wdrażanie grup NSG w klasycznym modelu wdrażania).
* [Deploy NSGs in Resource Manager](virtual-networks-create-nsg-arm-pportal.md) (Wdrażanie grup NSGs we wdrożeniu za pomocą usługi Resource Manager).
* [Manage NSG logs](virtual-network-nsg-manage-log.md) (Zarządzanie dziennikami grupy NSG).

[zielony]: ./media/virtual-network-nsg-overview/green.png
[żółty]: ./media/virtual-network-nsg-overview/yellow.png
[red]: ./media/virtual-network-nsg-overview/red.png



<!--HONumber=Sep16_HO3-->


