---
title: "Ocena obliczeń w migracji Azure | Dokumentacja firmy Microsoft"
description: "Zawiera omówienie obliczeń oceny w usłudze Azure migracji."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/02/2017
ms.author: raynew
ms.openlocfilehash: b264e2ceac4e76faa37d21972b94cfe323aa3ce5
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/23/2018
---
# <a name="assessment-calculations"></a>Obliczenia dotyczące oceny

[Azure migracji](migrate-overview.md) ocenia obciążeń lokalnych do migracji do usługi Azure. Ten artykuł zawiera informacje o sposobie obliczania oceny.


## <a name="overview"></a>Przegląd

Ocena migracji Azure ma trzy etapy. Ocena rozpoczyna się od analizy przydatności, po której następuje zmiana rozmiaru, i w końcu co miesiąc kosztów szacowania. Maszyna tylko przenosi na późniejszym etapie Jeśli przekazaniem poprzedni. Na przykład w przypadku niepowodzenia sprawdzania Azure przydatności maszyna jest oznaczony jako nieodpowiednie dla platformy Azure oraz zmiany rozmiaru i kosztów nie będzie odbywać się. 


## <a name="azure-suitability-analysis"></a>Analiza przydatności Azure

Nie wszystkie komputery są odpowiednie do uruchamiania w chmurze, jak chmura ma własną ograniczeń i wymagań. Azure migracji ocenia każdy na lokalnej maszynie przydatności migracji na platformie Azure i kategoryzuje maszyny do jednej z następujących kategorii:
- **Gotowy na platformie Azure** — można migrować maszynę jako — jest na platformie Azure, bez wprowadzania żadnych zmian. Zostanie uruchomiony na platformie Azure z obsługą pełnej Azure.
- **Warunkowo gotowe do Azure** -komputera może uruchomić na platformie Azure, ale może nie mieć pełnych Azure pomocy technicznej. Na przykład komputera przy użyciu starszej wersji systemu operacyjnego Windows Server nie jest obsługiwane na platformie Azure. Należy zachować ostrożność przed przeprowadzeniem migracji tych urządzeń do platformy Azure i postępuj zgodnie ze wskazówkami korygowania sugerowane w ocenie, aby rozwiązać problemy z gotowością przed przeprowadzeniem migracji.
- **Nie jest gotowy do Azure** -komputer nie uruchomi się na platformie Azure. Na przykład jeśli komputera lokalnego ma dysk o rozmiarze ponad 4 TB dołączone do niego, nie może być umieszczona na platformie Azure. Należy postępować zgodnie ze wskazówkami korygowania sugerowane w ocenie, aby rozwiązać problem gotowości przed migracją do systemu Azure. Szacowanie kosztów i doboru wielkości nie została wykonana dla maszyn, które są oznaczone jako nie jest gotowy dla platformy Azure.
- **Nieznana gotowość** -Azure migracji nie można odnaleźć gotowości maszyny z powodu niewystarczających danych dostępnych w programie vCenter Server. 

Migrowanie Azure sprawdza właściwości maszyny i systemu operacyjnego gościa, aby zidentyfikować Azure gotowości komputera lokalnego.

### <a name="machine-properties"></a>Właściwości komputera
Azure migracji monitoruje następujące właściwości lokalnej maszyny Wirtualnej, aby ustalić, czy uruchomić Maszynę wirtualną na platformie Azure.
 
**Właściwość** | **Szczegóły** | **Stan gotowości systemu Azure**
--- | --- | ---
**Typ rozruchu** | Azure typu rozruchu systemu BIOS i UEFI nie obsługuje maszyn wirtualnych. | Jeśli typ rozruchu UEFI warunkowo gotowe do platformy Azure. 
**Liczba rdzeni** | Liczba rdzeni na maszynach musi być równa lub mniejsza niż maksymalna liczba rdzeni (32) obsługiwanych na maszynie Wirtualnej platformy Azure.<br/><br/> Jeśli Historia wydajności jest dostępny, Azure migracji uwzględnia rdzenie wykorzystywanych do porównania. Jeśli współczynnik komfort określono w ustawieniach oceny, liczba rdzeni wykorzystywanych jest mnożona przez współczynnik komfort.<br/><br/> Jeśli nie ma żadnych Historia wydajności, Azure migracji używa przydzielonych rdzeni, bez stosowania współczynnik komfort. | Brak gotowości, jeśli liczba rdzeni jest większa niż 32. 
**Pamięci** | Rozmiar pamięci maszyny musi być równa lub mniejsza niż maksymalna ilość pamięci (448 GB) dozwolony dla maszyny Wirtualnej platformy Azure. <br/><br/> Jeśli Historia wydajności jest dostępny, Azure migracji uwzględnia pamięci wykorzystywanych do porównania. Jeśli określono współczynnik komfort, wykorzystywanych pamięci jest mnożona przez współczynnik komfort.<br/><br/> Jeśli nie ma Brak historii alokacji pamięci jest używana bez stosowania współczynnik komfort.<br/><br/> | Brak gotowości, jeśli rozmiar pamięci jest większa niż 448 GB.
**Dysk magazynu** | Przydzielony rozmiar dysku musi być 4 TB (4096 GB) lub mniej.<br/><br/> Liczba dysków dołączonych do maszyny musi być 65 lub mniej, w tym dysku systemu operacyjnego. | Brak gotowości, jeśli dowolny dysk ma rozmiar większy niż 4 TB lub jeśli istnieje więcej niż 65 dysków dołączonych do maszyny. 
**Sieć** | Komputer musi mieć 32 lub mniej karty sieciowe podłączone do niego. | Brak gotowości, jeśli komputer ma więcej niż 32 kart sieciowych

### <a name="guest-operating-system"></a>System operacyjny gościa 
Wraz z właściwości maszyny Wirtualnej migracja Azure również analizuje system operacyjny gościa z lokalnej maszyny Wirtualnej, aby ustalić, czy uruchomić maszyny Wirtualnej na platformie Azure.

> [!NOTE]
> Azure migracji uwzględnia systemu operacyjnego określone w programie vCenter Server w celu następujące analizy. Ponieważ odnajdywania realizowane przez migrację Azure jest oparte na urządzeniu, nie ma sposób sprawdzić, czy system operacyjny uruchomiony w ramach maszyny Wirtualnej ma taki sam jak jeden określony w programie vCenter Server.

Następującą logiką jest używane przez migrację Azure do identyfikowania Azure gotowości opartą na systemie operacyjnym maszyny wirtualnej.

**System operacyjny** | **Szczegóły** | **Stan gotowości systemu Azure**
--- | --- | ---
Windows Server 2016 & wszystkie SPs | Azure zapewnia pełną obsługę. | Gotowa na platformę Azure
Windows Server 2012 R2 & wszystkie SPs | Azure zapewnia pełną obsługę. | Gotowa na platformę Azure
Windows Server 2012 & wszystkie SPs | Azure zapewnia pełną obsługę. | Gotowa na platformę Azure
Windows Server 2008 R2 z wszystkich SPs | Azure zapewnia pełną obsługę.| Gotowa na platformę Azure
Windows Server 2003 – 2008 R2 | Te systemy operacyjne przekazanego koniec okresu pomocy technicznej i potrzeb [umowy obsługuje niestandardowe (CSA)](https://aka.ms/WSosstatement) pomocy technicznej platformy Azure. | Warunkowo gotowe do platformy Azure, Rozważ uaktualnienie systemu operacyjnego przed przeprowadzeniem migracji na platformie Azure.
Windows 2000, 98, 95, NT, 3.1, MS-DOS | Te systemy operacyjne przekazanego ich obsługa Data zakończenia, komputera może uruchomić na platformie Azure, ale nie obsługuje systemu operacyjnego są dostarczane przez platformę Azure. | Warunkowo gotowy na platformie Azure, zaleca się uaktualnienie systemu operacyjnego przed przeprowadzeniem migracji na platformie Azure.
Klient systemu Windows 7, 8 i 10 | Azure zapewnia obsługę subskrypcji programu Visual Studio tylko. | Warunkowo gotowe do platformy Azure
Windows Vista, XP Professional | Te systemy operacyjne przekazanego ich obsługa Data zakończenia, komputera może uruchomić na platformie Azure, ale nie obsługuje systemu operacyjnego są dostarczane przez platformę Azure. | Warunkowo gotowy na platformie Azure, zaleca się uaktualnienie systemu operacyjnego przed przeprowadzeniem migracji na platformie Azure.
Linux | Azure wspiera działania te [systemów operacyjnych Linux](../virtual-machines/linux/endorsed-distros.md). Inne systemy operacyjne Linux mogą rozruchu w systemie Azure, ale zaleca się uaktualnienie systemu operacyjnego do wersji potwierdzony przed migracją do systemu Azure. | Gotowy na platformie Azure, jeśli wersja jest zatwierdzone.<br/><br/>Warunkowo gotowe, jeśli wersja nie jest potwierdzone.
Inne systemy operacyjne<br/><br/> np. Oracle Solaris, itp. system operacyjny Mac firmy Apple, FreeBSD itp. | Azure nie potwierdza autentyczności tych systemów operacyjnych. Komputer może rozruchu w systemie Azure, ale nie obsługuje systemu operacyjnego są dostarczane przez platformę Azure. | Warunkowo gotowy na platformie Azure, zaleca się instalowanie obsługiwany system operacyjny przed migracją do usługi Azure.  
System operacyjny określony jako *innych* w programie vCenter Server | Azure migracji nie można zidentyfikować w takim przypadku system operacyjny. | Nieznany gotowości. Upewnij się, że system operacyjny uruchomiony w ramach maszyny Wirtualnej jest obsługiwany na platformie Azure. 
32-bitowe systemy operacyjne | Komputer może rozruchu w systemie Azure, ale Azure może nie zapewniają pełną obsługę. | Warunkowo gotowe do platformy Azure, Rozważ uaktualnienie systemu operacyjnego komputera z 32-bitowego systemu operacyjnego do 64-bitowego systemu operacyjnego przed przeprowadzeniem migracji na platformie Azure.

## <a name="sizing"></a>Zmiana rozmiaru

Po maszyna jest oznaczona jako gotowe do platformy Azure, Azure migracji rozmiary maszyny Wirtualnej i jej dysków dla platformy Azure. Jeśli określona we właściwościach oceny kryterium zmiany rozmiaru czy wydajności na podstawie zmiany rozmiaru, Azure migracji uwzględnia Historia wydajności komputera, aby zidentyfikować dla rozmiaru maszyny Wirtualnej na platformie Azure. Ta metoda jest przydatne w scenariuszach, w którym nadmiernie przydzielił lokalnej maszyny Wirtualnej, ale wykorzystania jest niski i chcesz odpowiedniego rozmiaru maszyn wirtualnych na platformie Azure, aby zapisać kosztów.

> [!NOTE]
> Azure migracji zbiera Historia wydajności lokalnych maszyn wirtualnych z programu vCenter Server. Aby zapewnić dokładne doboru wielkości, upewnij się, że ustawienie statystyk w programie vCenter Server jest ustawiony poziom 3 i poczekaj co najmniej jeden dzień przed zasób wyłączanie funkcji odnajdywania lokalnych maszyn wirtualnych. Jeśli ustawienie statystyk w programie vCenter Server znajduje się poniżej poziomu 3, nie są zbierane dane dotyczące wydajności dysku i sieci. 

Jeśli nie chcesz wziąć pod uwagę historię wydajności i chcesz przełączyć maszyny Wirtualnej jako — jest na platformie Azure, można określić jako kryterium zmiany rozmiaru *jako lokalną* i migracji Azure następnie rozmiar maszyn wirtualnych na podstawie konfiguracji lokalnymi bez biorąc pod uwagę wykorzystanie danych.

### <a name="performance-based-sizing"></a>Na podstawie rozmiaru

Na podstawie wydajności zmiany rozmiaru dysków dołączonych do maszyny Wirtualnej, migracja Azure rozpoczyna się znak kart sieciowych i następnie mapy maszyny Wirtualnej platformy Azure na podstawie wymagań obliczeń lokalnej maszyny wirtualnej. 
 
- **Dyski**: Migrowanie Azure próbuje zamapować co dysk dołączony do maszyny na dysku na platformie Azure. 
    
    > [!NOTE]
    > Azure migracji obsługuje tylko zarządzanego dysków w celu oceny.
    
    - Uzyskanie dysku skuteczne operacji We/Wy na sekundę (IOPS) i przepływności (MB/s), Azure migracji mnoży dysku IOPS i przepływność współczynnik komfort. Na podstawie IOPS skuteczne i wartości przepustowości, migracja Azure Określa, czy należy mapować dysku na dysk standardowy lub premium na platformie Azure.
    - Jeśli migracja Azure nie może znaleźć dysku o wymagane IOPS & przepływności, oznacza maszynę jako nieodpowiednie dla platformy Azure. [Dowiedz się więcej](../azure-subscription-service-limits.md#storage-limits) o Azure ogranicza na dysku i maszyny Wirtualnej.
    - W przypadku odnalezienia zestaw odpowiednich dysków, Azure migracji wybiera te, które obsługują metody nadmiarowość magazynu i lokalizacji określonej w ustawieniach oceny.
    - W przypadku wielu dysków kwalifikujących się wybiera jeden z najniższy koszt.
    - Jeśli dane wydajności dotyczące dysków w niedostępny, wszystkie dyski są mapowane na standardowych dysków na platformie Azure.

- **Karty sieciowe**: Migrowanie Azure próbuje znaleźć maszyny Wirtualnej platformy Azure, który może obsługiwać liczbę kartach sieciowych podłączonych do komputera lokalnego i wydajności wymaganych przez te karty sieciowe.
    - Aby uzyskać wydajność sieci skuteczne lokalnej maszyny wirtualnej, migracja Azure agreguje dane przesyłane na sekundę (MB/s) poza maszyny (sieć out) we wszystkich kartach sieciowych i zastosowanie współczynnik komfort. Liczba ta służy do znajdowania obsługujące wydajności wymagana sieć maszyny Wirtualnej Azure.
    - Wraz z wydajność sieci, również uzna, jeśli maszyna wirtualna Azure może obsługiwać wymaganego liczba kart sieciowych.
    - Jeśli dane wydajności sieci są niedostępne, liczba kart sieciowych jest uznawany za dla rozmiaru maszyny Wirtualnej.

- **Rozmiar maszyny Wirtualnej**: po obliczeniu są wymagania dotyczące magazynu i sieci Azure migracji uwzględnia wymagania dotyczące Procesora i pamięci można znaleźć odpowiedniego rozmiaru maszyny Wirtualnej na platformie Azure.
    - Azure migracji analizuje wykorzystywanych rdzeni i ilości pamięci i stosuje współczynnik komfort uzyskanie skuteczne rdzeni i ilości pamięci. Na podstawie tego numeru, próbuje odnaleźć odpowiedni rozmiar maszyny Wirtualnej na platformie Azure.
    - Jeśli zostanie znaleziony żaden odpowiedni rozmiar, maszynie jest oznaczony jako nieodpowiednie dla platformy Azure.
    - Jeśli zostanie znaleziony odpowiedni rozmiar, Azure migracji ma zastosowanie obliczenia magazynu i sieci. Następnie stosuje lokalizacji i cenach ustawień warstwę, do końcowego zalecenie rozmiar maszyny Wirtualnej.
    - W przypadku wielu kwalifikujących się rozmiary maszyn wirtualnych Azure z najniższy koszt jest zalecane.

### <a name="as-on-premises-sizing"></a>Jako lokalną zmiany rozmiaru
W przypadku zmiany rozmiaru kryterium *jako lokalne zmiany rozmiaru*, Azure migracji nie należy wziąć pod uwagę historię wydajności maszyn wirtualnych i przydziela maszyn wirtualnych i dyski, na podstawie rozmiaru przydzielone lokalnymi.
- **Magazyn**: dla każdego dysku dysków w warstwie standardowa na platformie Azure jest zalecane z taki sam rozmiar jak dysk lokalny.
- **Sieci**: dla każdej karty sieciowej, zaleca się karty sieciowej w systemie Azure.
- **Obliczenia bazy danych**: Migrowanie Azure sprawdza liczby rdzeni i wielkości pamięci lokalnej maszyny wirtualnej i zaleca maszyny Wirtualnej platformy Azure z taką samą konfiguracją. W przypadku wielu kwalifikujących się rozmiary maszyn wirtualnych Azure z najniższy koszt jest zalecane.
 
### <a name="confidence-rating"></a>Ocena zaufania

Każdy oceny w migracji Azure jest skojarzony z zakresu od 1 gwiazdka do 5 gwiazdkę (1 gwiazdka być najniższy i 5 Gwiazda jest najwyższa) przedziałem zaufania. Ocena ufności jest przypisany do oceny na podstawie dostępności punktów danych potrzebnych do obliczania oceny. Przedziałem zaufania ocenę pomaga oszacować niezawodność zalecenia dotyczące rozmiaru podał Azure migracji. 

Zaufanie jest przydatne podczas wykonywania *wydajności na podstawie rozmiaru* jako migracji Azure może nie mieć wystarczających punkty danych do użycia na podstawie zmiany rozmiaru. Dla *jako lokalne zmiany rozmiaru*, ocenę ufności jest zawsze 5 gwiazdek jako migracji Azure ma wszystkich punktów danych, należy go rozmiaru maszyny Wirtualnej. 

Na podstawie wydajności rozmiaru maszyny wirtualnej Azure migracji musi danych użycia procesora CPU i pamięci. Ponadto dla każdego dysku do maszyny Wirtualnej, musi on odczytu/zapisu IOPS i przepustowość. Podobnie dla każdej karty sieciowej podłączony do maszyny Wirtualnej Azure migracji musi sieci lub brak zgody na celu wydajności na podstawie rozmiaru. Jeśli z powyższych numerów użycie nie są dostępne w programie vCenter Server, zalecenie rozmiar programach migracji Azure może nie być prawidłowe. W zależności od procent dostępności punktów danych podano przedziałem zaufania oceny:

   **Dostępność punktów danych** | Ocena zaufania
   --- | ---
   0%-20% | 1 gwiazdka
   21%-40% | 2 gwiazdy
   41%-60% | 3 gwiazdy
   61%-80% | 4 gwiazdy
   81%-100% | 5 gwiazdy

Ocena nie może mieć wszystkich punktów danych dostępnych z jednego z następujących powodów:
- Ustawienie statystyk w programie vCenter Server nie jest ustawiony na poziom 3 i oceny zawiera wydajności na podstawie rozmiaru jako kryterium zmiany rozmiaru. Jeśli ustawienie statystyk w programie vCenter Server jest niższy niż poziom 3, dane wydajności dla dysku i sieci nie są zbierane z vCenter Server. W takim przypadku zalecenie dostarczonych przez migrację Azure dysku i sieci nie jest na podstawie użycia. Dla magazynu migracji Azure zaleca dyski standardowe, jak bez uwzględniania IOPS/przepływność dysku, Azure migracji nie można ustalić, czy dysk należy dysku premium na platformie Azure.
- Ustawienie statystyk w programie vCenter Server został ustawiony poziom 3 krótszy czas, przed przeprowadzony po raz pierwszy odnajdywania. Na przykład załóżmy Rozważmy scenariusz, w którym zmienić ustawienie poziomu 3 dzisiaj i rozpoczęcie wyłączanie funkcji odnajdywania jutro (po 24 godzinach) przy użyciu urządzenia modułu zbierającego statystyki. Jeśli tworzysz oceny przez jeden dzień, masz wszystkich punktów danych, a przedziałem zaufania oceny będzie 5 gwiazdek. Jednak w przypadku zmiany czasu trwania wydajności we właściwościach oceny na jeden miesiąc, przedziałem zaufania przestanie działać jako dysk i sieci danych wydajności przez ostatni miesiąc nie będzie dostępny. Jeśli chcesz wziąć pod uwagę dane dotyczące wydajności ostatni miesiąc, zaleca się pozostawienie vCenter statystyki ustawienie serwera do poziomu 3 przez jeden miesiąc przed należy rozpocząć wyłączanie funkcji odnajdywania. 
- Zamknięte kilka maszyn wirtualnych w okresie, dla której jest obliczany oceny. Jeśli wszystkie maszyny wirtualne zostały odłączony od zasilania dla niektórych czas trwania, serwer vCenter nie będzie miał dane wydajności dla tego okresu. 
- Kilka maszyn wirtualnych utworzonych w zakresie od okresu, dla której obliczana jest oceny. Na przykład w przypadku tworzenia ocenę wydajności historii ostatni miesiąc, ale kilka maszyn wirtualnych zostały utworzone w środowisku tylko tydzień temu. W takich przypadkach Historia wydajności nowych maszyn wirtualnych nie będą istnieje cały czas trwania.

> [!NOTE]
> W przypadku klasyfikacji zaufania każdej oceny poniżej 4 gwiazdki, firma Microsoft zaleca, aby zmienić poziom ustawienia statystyki serwera vCenter do 3, poczekaj, aż czas trwania, który chcesz wziąć pod uwagę w celu oceny (1 dzień/tydzień/miesiąc 1), a następnie wykonaj odnajdywanie i oceny. Jeśli nie można wykonać poprzednie, na podstawie rozmiaru może nie być prawidłowe i zaleca, aby przełączyć się do *jako lokalne zmiany rozmiaru* , zmieniając właściwości oceny.

## <a name="monthly-cost-estimation"></a>Miesięczne szacowania kosztów

Po zakończeniu zmiany rozmiaru zalecenia dotyczące migracji Azure oblicza koszty obliczeniowej i pamięci masowej po migracji.

- **Obliczanie kosztu**: przy użyciu zalecany rozmiar maszyny Wirtualnej platformy Azure, Azure migracji używa interfejsu API rozliczeń do obliczenia miesięczny koszt dla maszyny Wirtualnej. Obliczenie ma systemu operacyjnego, software assurance, lokalizacji i ustawienia waluty pod uwagę. Agreguje koszt na wszystkich komputerach, aby obliczyć całkowity miesięczny koszt obliczeń.
- **Koszt magazynowania**: Magazyn miesięczny koszt maszyna jest obliczana przez agregowanie miesięczny koszt wszystkich dysków dołączonych do maszyny. Azure migracji oblicza całkowity miesięczny kosztów magazynowania przez agregowanie kosztów magazynowania wszystkich maszyn. Obecnie obliczenia nie przyjmuje oferty określonego w ustawieniach ocenę pod uwagę.

Koszty są wyświetlane w walucie określonego w ustawieniach oceny. 


## <a name="next-steps"></a>Kolejne kroki

[Tworzenie oceny dla lokalnych maszyn wirtualnych VMware](tutorial-assessment-vmware.md)
