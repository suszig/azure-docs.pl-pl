## <a name="understand-planned-vs-unplanned-maintenance"></a>Informacje o konserwacji planowanej i nieplanowanej
Istnieją dwa typy zdarzeń platformy Microsoft Azure, które mogą mieć wpływ na dostępność maszyn wirtualnych: planowana konserwacja i nieplanowana konserwacja.

* **Zdarzenia planowanej konserwacji** to okresowe aktualizacje tworzone przez firmę Microsoft dla podstawowej platformy Azure w celu zwiększenia ogólnej niezawodności, wydajności i bezpieczeństwa infrastruktury platformy działania maszyn wirtualnych. Większość tych aktualizacji jest przeprowadzana bez żadnego wpływu na działanie maszyn wirtualnych ani usług w chmurze. Jednak w przypadku niektórych wystąpień do zastosowania wymaganych aktualizacji w infrastrukturze platformy konieczne jest ponowne uruchomienie maszyny wirtualnej.
* **Zdarzenia nieplanowanej konserwacji** występują w przypadku wystąpienia awarii sprzętu lub infrastruktury fizycznej zawierającej maszynę wirtualną. Mogą być to awarie sieci lokalnej, błędy na dysku lokalnym lub inne awarie na poziomie regału. Po wykryciu awarii tego typu platforma Azure automatycznie migruje maszynę wirtualną z hostującej ją maszyny fizycznej o złej kondycji do maszyny fizycznej o dobrej kondycji. Te zdarzenia występują rzadko, ale mogą również spowodować ponowne uruchomienie maszyny wirtualnej.

Aby zmniejszyć wpływ przestoju spowodowanego co najmniej jednym z tych zdarzeń, zalecamy zastosowanie następujących najlepszych rozwiązań zapewniających wysoką dostępność maszyn wirtualnych:

* [Konfigurowanie wielu maszyn wirtualnych w zestawie dostępności w celu zapewnienia nadmiarowości]
* [Konfigurowanie każdej warstwy aplikacji w osobnych zestawach dostępności]
* [Łączenie modułu równoważenia obciążenia z zestawami dostępności]
* [Używanie wielu kont magazynu dla każdego zestawu dostępności]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Konfigurowanie wielu maszyn wirtualnych w zestawie dostępności w celu zapewnienia nadmiarowości
Aby zapewnić nadmiarowość aplikacji, zalecamy grupowanie co najmniej dwóch maszyn wirtualnych w zestawie dostępności. Taka konfiguracja zapewnia dostępność co najmniej jednej maszyny wirtualnej podczas planowanych i nieplanowanych zdarzeń związanych z konserwacją i spełnia warunki umowy SLA gwarantującej dostępność platformy Azure przez 99,95% czasu. Aby uzyskać więcej informacji, zobacz [Virtual Machines — umowa SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/).

> [!IMPORTANT]
> Unikaj pozostawiania pojedynczego wystąpienia maszyny wirtualnej w zestawie dostępności. Maszyny wirtualne w tej konfiguracji nie są objęte gwarancją umowy SLA. Podczas zdarzeń planowanej konserwacji platformy Azure może wystąpić przestój, z wyjątkiem sytuacji, gdy pojedyncza maszyna wirtualna korzystania z usługi [Azure Premium Storage](../articles/storage/storage-premium-storage.md). W przypadku pojedynczych maszyn wirtualnych używających magazynu w wersji Premium obowiązuje umowa SLA platformy Azure.

Każda maszyna wirtualna w zestawie dostępności ma przypisaną **domenę aktualizacji** i **domenę błędów** z odpowiedniej platformy Azure. Dany zestaw dostępności ma pięć domyślnie przypisanych domen aktualizacji, których użytkownik nie może konfigurować (następnie można zwiększyć liczbę wystąpień usługi Resource Manager, aby oferowała do 20 domen aktualizacji). Umożliwia to wskazanie grup maszyn wirtualnych i odpowiedniego sprzętu fizycznego, które mogą być uruchamiane równocześnie. Jeśli w pojedynczym zestawie dostępności skonfigurowano więcej niż pięć maszyn wirtualnych, szósta maszyna wirtualna jest umieszczana w tej samej domenie aktualizacji, co pierwsza maszyna wirtualna, siódma — w tej samej domenie aktualizacji co druga maszyna wirtualna itd. Podczas planowanej konserwacji domeny aktualizacji mogą nie być ponownie uruchamiane kolejno, ale w danym momencie tylko jedna domena aktualizacji jest uruchamiana ponownie.

Domeny błędów definiują grupę maszyn wirtualnych, które korzystają ze wspólnego źródła zasilania i przełącznika sieciowego. Domyślnie maszyny wirtualne skonfigurowane w zestawie dostępności są rozdzielane między maksymalnie trzy domeny błędów w przypadku wdrożeń usługi Resource Manager (dwie domeny błędów w przypadku modelu klasycznego). Umieszczenie maszyn wirtualnych w zestawie dostępności nie chroni aplikacji przed błędami związanymi z systemem operacyjnym lub daną aplikacją, ale ogranicza wpływ potencjalnych awarii sprzętu fizycznego, awarii sieci i przerw w dostawie prądu.

<!--Image reference-->
   ![Rysunek koncepcyjny przedstawiający konfigurację domeny aktualizacji i domeny błędów](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains-and-availability-sets"></a>Zestawy dostępności i domeny błędów usługi Managed Disks
Maszyny wirtualne korzystające z usługi [Azure Managed Disks](../articles/storage/storage-faq-for-disks.md) są przydzielane do domen błędów dysków zarządzanych w przypadku korzystania z zarządzanego zestawu dostępności. Dzięki takiemu dopasowaniu wszystkie dyski zarządzane dołączone do maszyny wirtualnej działają w tej samej domenie błędów dysku zarządzanego. W zarządzanym zestawie dostępności można tworzyć tylko maszyny wirtualne z użyciem dysków zarządzanych. Liczba domen błędów dysku zarządzanego zależy od regionu — dwie lub trzy domeny błędów dysku zarządzanego na region.


## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Konfigurowanie każdej warstwy aplikacji w osobnych zestawach dostępności
Jeśli wszystkie maszyny wirtualne są niemal identyczne i służą temu samemu celowi związanemu z aplikacją, zalecamy skonfigurowanie zestawu dostępności dla każdej warstwy aplikacji.  Jeśli umieścisz dwie różne warstwy w tym samym zestawie dostępności, wszystkie maszyny wirtualne w tej samej warstwie aplikacji będzie można jednocześnie uruchomić ponownie. Skonfigurowanie co najmniej dwóch maszyn wirtualnych w zestawie dostępności dla każdej warstwy gwarantuje, że co najmniej jedna maszyna wirtualna w każdej warstwie jest dostępna.

Możesz na przykład umieścić wszystkie maszyny wirtualne w frontonie aplikacji uruchamiającej usługi IIS, Apache i Nginx w pojedynczym zestawie dostępności. Upewnij się, że tylko maszyny wirtualne frontonu są umieszczane w tym samym zestawie dostępności. Upewnij się również, że tylko maszyny wirtualne warstwy danych, takie jak replikowane maszyny wirtualne programu SQL Server lub maszyny wirtualne języka MySQL, są umieszczane we własnym zestawie dostępności.

<!--Image reference-->
   ![Warstwy aplikacji](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Łączenie modułu równoważenia obciążenia z zestawami dostępności
Połącz usługę [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) z zestawem dostępności, aby uzyskać najlepszą odporność aplikacji. Usługa Azure Load Balancer dystrybuuje ruch między wieloma maszynami wirtualnymi. W przypadku naszych maszyn wirtualnych w warstwie Standardowa usługa Azure Load Balancer jest uwzględniana. Nie wszystkie warstwy maszyn wirtualnych obejmują usługę Azure Load Balancer. Aby uzyskać więcej informacji na temat równoważenia obciążenia maszyn wirtualnych, zobacz [Load Balancing virtual machines](../articles/virtual-machines/virtual-machines-linux-load-balance.md) (Równoważenie obciążenia maszyn wirtualnych).

Jeśli moduł równoważenia obciążenia nie został skonfigurowany do równoważenia ruchu między wieloma maszynami wirtualnymi, każde zdarzenie planowanej konserwacji wpływa tylko na maszynę wirtualną obsługującą ruch, co powoduje awarię w warstwie aplikacji. Umieszczenie wielu maszyn wirtualnych z tej samej warstwy w tym samym module równoważenia obciążenia i zestawie dostępności umożliwia stałą obsługę ruchu przez co najmniej jedno wystąpienie.

## <a name="use-multiple-storage-accounts-for-each-availability-set"></a>Używanie wielu kont magazynu dla każdego zestawu dostępności
Jeśli korzystasz z usługi Azure Managed Disks, możesz pominąć poniższe wskazówki. Usługa Azure Managed Disks z założenia zapewnia wysoką dostępność i nadmiarowość, ponieważ dyski są przechowywane w domenach błędów dostosowanych do zestawów dostępności maszyny wirtualnej. Aby uzyskać więcej informacji, zobacz temat [Omówienie usługi Azure Managed Disks](../articles/storage/storage-managed-disks-overview.md).

Jeśli korzystasz z dysków niezarządzanych, istnieją najlepsze rozwiązania, które należy stosować w odniesieniu do kont magazynu używanych przez wirtualne dyski twarde (VHD) w ramach maszyny wirtualnej. Każdy dysk (VHD) jest stronicowym obiektem blob na koncie usługi Azure Storage. Ważne jest, aby zapewnić nadmiarowość i izolację kont magazynu, co z kolei umożliwi uzyskanie wysokiej dostępności maszyn wirtualnych w zestawie dostępności.

1. **Obsługuj wszystkie dyski (systemu operacyjnego i danych) skojarzone z maszyną wirtualną na tym samym koncie magazynu.**
2. **W przypadku dodawania kolejnych dysków VHD do konta magazynu weź pod uwagę [limity](../articles/storage/storage-scalability-targets.md) konta magazynu.**
3. **Używaj oddzielnego konta magazynu dla każdej maszyny wirtualnej w zestawie dostępności.** Wiele maszyn wirtualnych w tym samym zestawie dostępności NIE MOŻE współdzielić kont magazynu. Dopuszczalna jest sytuacja, w której maszyny wirtualne w różnych zestawach dostępności korzystają z tych samych kont magazynu, ale pod warunkiem zastosowania opisanych powyżej najlepszych rozwiązań.

<!-- Link references -->
[Konfigurowanie wielu maszyn wirtualnych w zestawie dostępności w celu zapewnienia nadmiarowości]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Konfigurowanie każdej warstwy aplikacji w osobnych zestawach dostępności]: #configure-each-application-tier-into-separate-availability-sets
[Łączenie modułu równoważenia obciążenia z zestawami dostępności]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Używanie wielu kont magazynu dla każdego zestawu dostępności]: #use-multiple-storage-accounts-for-each-availability-set



<!--HONumber=Feb17_HO2-->


