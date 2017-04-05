

## <a name="memory-preserving-updates"></a>Aktualizacje pozwalające zachować stan pamięci
W przypadku wielu aktualizacji przeprowadzanych na platformie Microsoft Azure klienci nie odczuwają ich wpływu na uruchomione maszyny wirtualne. Wiele z tych aktualizacji dotyczy składników lub usług, które można zaktualizować bez zakłócania pracy uruchomionego wystąpienia. Niektóre z tych aktualizacji to aktualizacje infrastruktury platformy mające miejsce w systemie operacyjnym hosta, które można zastosować bez konieczności przeprowadzania pełnego ponownego uruchamiania maszyn wirtualnych.

Te aktualizacje są przeprowadzane za pomocą technologii umożliwiającej migrację na żywo w miejscu — są one również określane jako aktualizacje pozwalające zachować stan pamięci. Podczas aktualizowania maszyna wirtualna jest ustawiana w stan wstrzymania, co pozwala zachować stan pamięci RAM, gdy w podstawowym systemie operacyjnym hosta wprowadzane są niezbędne aktualizacje i poprawki. Maszyna wirtualna jest wznawiana w ciągu 30 sekund od momentu wstrzymania. Po wznowieniu zegar maszyny wirtualnej jest automatycznie synchronizowany.

Nie wszystkie aktualizacje można wdrożyć za pomocą tego mechanizmu, ale biorąc pod uwagę krótki czas trwania przerwy, wdrażanie aktualizacji w ten sposób znacznie ogranicza ich wpływ na maszyny wirtualne.

Aktualizacje wielu wystąpień (dla maszyn wirtualnych w zestawie dostępności) są stosowane dla jednej domeny aktualizacji jednocześnie.  

## <a name="virtual-machine-configurations"></a>Konfiguracje maszyny wirtualnej
Istnieją dwa rodzaje konfiguracji maszyny wirtualnej: konfiguracja o wielu wystąpieniach i konfiguracja o jednym wystąpieniu. W ramach konfiguracji o wielu wystąpieniach podobne maszyny wirtualne są umieszczane w zestawie dostępności.

Konfiguracja o wielu wystąpieniach zapewnia nadmiarowość w obrębie maszyn fizycznych, ich zasilania oraz dostępu do sieci, i jest zalecana w celu zapewnienia dostępności aplikacji. Wszystkie maszyny wirtualne w zestawie dostępności powinny mieć takie same znaczenie dla aplikacji.

Aby uzyskać więcej informacji o konfigurowaniu maszyn wirtualnych na potrzeby wysokiej dostępności, zobacz [Manage the availability of your Windows virtual machines](../articles/virtual-machines/windows/manage-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (Zarządzanie dostępnością maszyn wirtualnych systemu Windows) lub [Manage the availability of your Linux virtual machines](../articles/virtual-machines/linux/manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (Zarządzanie dostępnością maszyn wirtualnych systemu Linux).

Z drugiej strony konfiguracja o jednym wystąpieniu jest używana na potrzeby autonomicznych maszyn wirtualnych, które nie są umieszczane w zestawie dostępności. Te maszyny wirtualne nie kwalifikują się do umowy dotyczącej poziomu usług wymagającej wdrożenia co najmniej dwóch maszyn wirtualnych w tym samym zestawie dostępności.

Więcej informacji na temat umów dotyczących poziomu usług zawierają sekcje Cloud Services i Virtual Machines w temacie [Umowy dotyczące poziomu usług](https://azure.microsoft.com/support/legal/sla/).

## <a name="multi-instance-configuration-updates"></a>Aktualizacje konfiguracji o wielu wystąpieniach
Podczas planowanej konserwacji na platformie Azure najpierw przeprowadzana jest aktualizacja zestawu maszyn wirtualnych hostowanych w ramach konfiguracji o wielu wystąpieniach. Proces aktualizacji powoduje wykonanie ponownego uruchomienia tych maszyn wirtualnych (przestój trwa około 15 minut).

Podczas aktualizacji konfiguracji o wielu wystąpieniach przyjmowane jest założenie, że każda maszyna wirtualna pełni podobną funkcję, jak inne maszyny wirtualne znajdujące się w zestawie dostępności. W takim podejściu maszyny wirtualne są aktualizowane w sposób, który pozwala na zachowanie dostępności przez cały proces.

Każda maszyna wirtualna w zestawie dostępności ma przypisaną domenę aktualizacji i domenę błędów z odpowiedniej platformy Azure. Każda domena aktualizacji to grupa maszyn wirtualnych, które zostaną ponownie uruchomione w tym samym przedziale czasu. Każda domena błędów to grupa maszyn wirtualnych, które korzystają ze wspólnego źródła zasilania i przełącznika sieciowego.


Aby uzyskać więcej informacji o domenach aktualizacji i domenach błędów, zobacz [Configure multiple virtual machines in an availability set for redundancy](../articles/virtual-machines/windows/manage-availability.md#configure-multiple-virtual-machines-in-an-availability-set-for-redundancy) (Konfigurowanie wielu maszyn wirtualnych w zestawie dostępności w celu zapewnienia nadmiarowości).

Aby zapewnić dostępność za pośrednictwem aktualizacji, na platformie Azure wykonywana jest konserwacja za pomocą domeny aktualizacji, która powoduje aktualizowanie jednej domeny jednocześnie. Konserwacja w domenie aktualizacji polega na zamknięciu każdej maszyny wirtualnej w domenie, zastosowaniu aktualizacji na maszynach hosta, a następnie ponownym uruchomieniu maszyn wirtualnych. Po zakończeniu konserwacji w domenie na platformie Azure powtarzany jest ten proces w przypadku następnej domeny aktualizacji — odbywa się to do momentu, gdy wszystkie domeny zostaną zaktualizowane.

Podczas planowanej konserwacji domeny aktualizacji mogą nie być ponownie uruchamiane kolejno, ale w danym momencie tylko jedna domena aktualizacji jest uruchamiana ponownie. Obecnie platforma Azure oferuje powiadomienie z jednotygodniowym wyprzedzeniem o zaplanowanej konserwacji maszyn wirtualnych w ramach konfiguracji o wielu wystąpieniach.

Poniżej znajduje się przykład tego, co może być wyświetlane w Podglądzie zdarzeń systemu Windows po przywróceniu maszyny wirtualnej:

<!--Image reference-->
![][image2]


Podgląd służy do zgłaszania maszyn wirtualnych, które zostały skonfigurowane w ramach konfiguracji o wielu wystąpieniach przy użyciu witryny Azure Portal, usługi Azure PowerShell lub interfejsu wiersza polecenia platformy Azure. Na przykład za pomocą witryny Azure Portal można dodać _zestaw dostępności_ do okna dialogowego przeglądania **Maszyny wirtualne (klasyczne)**. Maszyny wirtualne zgłaszające ten sam zestaw dostępności są częścią konfiguracji o wielu wystąpieniach. W poniższym przykładzie konfiguracja o wielu wystąpieniach składa się z maszyn wirtualnych SQLContoso01 i SQLContoso02.

<!--Image reference-->
  ![Widok Maszyny wirtualne (klasyczne) w witrynie Azure Portal][image4]

## <a name="single-instance-configuration-updates"></a>Aktualizacje konfiguracji o jednym wystąpieniu
Po zakończeniu aktualizacji konfiguracji o wielu wystąpieniach na platformie Azure wykonywane są aktualizacje konfiguracji o jednym wystąpieniu. Te aktualizacje również powodują ponowne uruchomienie maszyn wirtualnych, które nie są uruchomione w ramach zestawów dostępności.

> [!NOTE]
> Jeśli w zestawie dostępności jest uruchomione tylko jedno wystąpienie maszyny wirtualnej, platforma Azure traktuje je jako aktualizację konfiguracji o wielu wystąpieniach.
>

Konserwacja w konfiguracji o jednym wystąpieniu polega na zamknięciu każdej maszyny wirtualnej uruchomionej na maszynie hosta, aktualizacji maszyny hosta, a następnie ponownym uruchomieniu maszyn wirtualnych. Do przeprowadzenia konserwacji wymagane jest około 15 minut przestoju. Zdarzenie planowanej konserwacji jest uruchamiane dla wszystkich maszyn wirtualnych w regionie podczas jednego okna obsługi.


Zdarzenia planowanej konserwacji mają wpływ na dostępność aplikacji w przypadku konfiguracji o jednym wystąpieniu. Platforma Azure oferuje powiadomienie z jednotygodniowym wyprzedzeniem o zaplanowanej konserwacji maszyn wirtualnych w ramach konfiguracji o jednym wystąpieniu.

## <a name="email-notification"></a>Powiadomienie e-mail
Tylko w przypadku konfiguracji maszyn wirtualnych o jednym wystąpieniu i konfiguracji o wielu wystąpieniach platforma Azure wysyła alert e-mail dotyczący nadchodzącej planowanej konserwacji (z tygodniowym wyprzedzeniem). Ta wiadomość e-mail jest wysyłana na konta e-mail administratora i współadministratora subskrypcji. Poniżej znajduje się przykładowa wiadomość e-mail tego typu:

<!--Image reference-->
![][image1]

## <a name="region-pairs"></a>Pary regionów

Podczas wykonywania konserwacji platforma Azure aktualizuje wystąpienia maszyn wirtualnych tylko w jednym obszarze z pary. Na przykład podczas aktualizowania maszyn wirtualnych w regionie Północno-środkowe stany USA platforma Azure nie będzie jednocześnie przeprowadzać aktualizacji żadnych maszyn wirtualnych w regionie Południowo-środkowe stany USA. Takie aktualizacje zostaną zaplanowane na inny termin, co pozwoli na korzystanie z trybu failover lub równoważenie obciążenia między regionami. Inne regiony, takie jak Europa Północna, mogą być jednak w trakcie konserwacji w tym samym czasie, co region Wschodnie stany USA.

W poniższej tabeli znajdują się aktualne pary regionów:

| Region 1 | Region 2 |
|:--- | ---:|
| Wschodnie stany USA |Zachodnie stany USA |
| Wschodnie stany USA 2 |Środkowe stany USA |
| Środkowo-północne stany USA |Środkowo-południowe stany USA |
| Środkowo-zachodnie stany USA |Zachodnie stany USA 2 |
| Kanada Wschodnia |Kanada Środkowa |
| Brazylia Południowa |Środkowo-południowe stany USA |
| Administracja USA — Iowa |Administracja USA — Wirginia |
| US DoD — wschodnie stany |US DoD — środkowe stany |
| Europa Północna |Europa Zachodnia |
| Zachodnie Zjednoczone Królestwo |Południowe Zjednoczone Królestwo |
| Niemcy Środkowe |Niemcy Północno-Wschodnie |
| Azja Południowo-Wschodnia |Azja Wschodnia |
| Australia Południowo-Wschodnia |Australia Wschodnia |
| Indie Środkowe |Indie Południowe |
| Indie Zachodnie |Indie Południowe |
| Japonia Wschodnia |Japonia Zachodnia |
| Korea Środkowa |Korea Południowa |
| Chiny Wschodnie |Chiny Północne |


<!--Anchors-->
[image1]: ./media/virtual-machines-common-planned-maintenance/vmplanned1.png
[image2]: ./media/virtual-machines-common-planned-maintenance/EventViewerPostReboot.png
[image3]: ./media/virtual-machines-planned-maintenance/RegionPairs.PNG
[image4]: ./media/virtual-machines-common-planned-maintenance/availabilitysetexample.png


<!--Link references-->
[Virtual Machines Manage Availability]: ../articles/virtual-machines/virtual-machines-windows-hero-tutorial.md

[Understand planned versus unplanned maintenance]: ../articles/virtual-machines/windows/manage-availability.md#Understand-planned-versus-unplanned-maintenance/
