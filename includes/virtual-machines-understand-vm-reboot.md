Azure maszynach wirtualnych (VM) może czasem ponownego uruchomienia bez widocznego powodu bez dokumentów użytkownika, o zainicjował operację ponownego uruchomienia. W tym artykule wymieniono akcje i zdarzenia, które mogą powodować maszyn wirtualnych do ponownego rozruchu i zapewnia wgląd w sposób uniknąć problemów z nieoczekiwane ponowne uruchomienie lub zmniejszyć wpływ takich problemów.

## <a name="configure-the-vms-for-high-availability"></a>Konfigurowanie maszyn wirtualnych wysokiej dostępności
Uruchamia najlepszy sposób, aby chronić aplikację, która działa na platformie Azure dla maszyny Wirtualnej, a czas przestoju jest skonfigurowanie maszyn wirtualnych wysokiej dostępności.

Zapewnienie ten poziom nadmiarowości aplikacji zalecamy grupowanie co najmniej dwóch maszyn wirtualnych w zestawie dostępności. Ta konfiguracja zapewnia, że podczas albo planowanego lub nieplanowanego zdarzenia konserwacji, co najmniej jedna maszyna wirtualna jest dostępna i spełnia 99,95% [umowy SLA platformy Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_5/).

Aby uzyskać więcej informacji na temat zestawów dostępności zobacz następujące artykuły:

- [Zarządzaj dostępnością maszyn wirtualnych](../articles/virtual-machines/windows/manage-availability.md)
- [Konfigurowanie dostępności maszyn wirtualnych](../articles/virtual-machines/windows/classic/configure-availability.md)

## <a name="resource-health-information"></a>Informacje o kondycji zasobów 
Kondycja zasobów platformy Azure to usługa, która przedstawia kondycję poszczególnych zasobów platformy Azure i zawiera możliwością wykonania akcji wskazówki dotyczące rozwiązywania problemów. W środowisku chmury, których nie można bezpośrednio uzyskać dostępu do serwerów lub elementy infrastruktury celem kondycja zasobu jest skrócić czas poświęcony na rozwiązywanie problemów. W szczególności należy skrócić czas poświęcany określającą, czy katalog główny problem leży w aplikacji lub w przypadku wewnątrz platformy Azure. Aby uzyskać więcej informacji, zobacz [opis i użyj kondycja zasobów](../articles/resource-health/resource-health-overview.md).

## <a name="actions-and-events-that-can-cause-the-vm-to-reboot"></a>Akcje i zdarzenia, które mogą spowodować ponowne uruchomienie maszyny Wirtualnej

### <a name="planned-maintenance"></a>Planowana konserwacja
Microsoft Azure wykonuje okresowo aktualizacje na całym świecie do poprawy niezawodności, wydajności i zabezpieczeń infrastruktury hosta źródłową maszyn wirtualnych. Wiele z tych aktualizacji, w tym zachowaniu pamięci aktualizacji są wykonywane bez wpływu na maszyn wirtualnych lub usług w chmurze.

Jednak niektóre aktualizacje wymagają ponownego uruchomienia. W takich przypadkach maszyny wirtualne są zamknięte podczas możemy poprawka infrastruktury, a następnie ponownego uruchomienia maszyny wirtualnej.

Aby zrozumieć, jakie Azure planowanej konserwacji i jak może to wpłynąć na dostępność maszyn wirtualnych systemu Linux, zobacz artykuły wymienione w tym miejscu. Artykuły zapewniają ogólne informacje o Azure planowanego proces konserwacji i sposób tworzenia harmonogramu zaplanowanej konserwacji w celu dalszego ograniczenia wpływu.

- [Planowana Konserwacja maszyn wirtualnych na platformie Azure](../articles/virtual-machines/windows/planned-maintenance.md)
- [Sposób tworzenia harmonogramu zaplanowanej konserwacji na maszynach wirtualnych Azure](../articles/virtual-machines/windows/classic/planned-maintenance-schedule.md)

### <a name="memory-preserving-updates"></a>Aktualizacje pozwalające zachować stan pamięci   
Dla tej klasy aktualizacji w programie Microsoft Azure podczas korzystania bez wpływu na ich uruchomionych maszyn wirtualnych. Wiele z tych aktualizacji dotyczy składników lub usług, które można zaktualizować bez zakłócania pracy uruchomionego wystąpienia. Niektóre są aktualizacji infrastruktury platformy w systemie operacyjnym hosta, który można zastosować bez ponownego uruchamiania maszyn wirtualnych.

Te aktualizacje zachowywania pamięci są realizowane za pomocą technologii, która umożliwia migrację na żywo w miejscu. Gdy jest on aktualizowany, maszyna wirtualna jest umieszczona w *wstrzymana* stanu. Ten stan zachowuje pamięci RAM, a system operacyjny hosta otrzymuje niezbędne aktualizacje i poprawki. Maszyna wirtualna zostanie wznowione w ciągu 30 sekund jest wstrzymana. Po wznowieniu maszyna wirtualna zostanie automatycznie zsynchronizowana jego zegara.

Ze względu na okres krótkiej przerwie wdrażania aktualizacji za pomocą ten mechanizm znacznie zmniejsza wpływ na maszynach wirtualnych. Jednak nie wszystkie aktualizacje można wdrożyć w ten sposób. 

Aktualizacje wielu wystąpień (dla maszyn wirtualnych w zestawie dostępności) są stosowane jednej domeny aktualizacji w czasie.

> [!NOTE]
> Maszyny z systemem Linux, które mają stare wersje jądra dotyczy alarm jądra podczas tej metody aktualizacji. Aby uniknąć tego problemu, zaktualizuj do jądra wersji 3.10.0-327.10.1 lub nowszej. Aby uzyskać więcej informacji, zobacz [panics maszyny Wirtualnej systemu Linux platformy Azure na podstawie 3.10 jądra po uaktualnieniu węzła hosta](https://support.microsoft.com/help/3212236).     
    
### <a name="user-initiated-reboot-or-shutdown-actions"></a>Akcje ponownego uruchomienia lub zamknięcia zainicjowanego przez użytkownika
 
Jeśli ponowne uruchomienie jest wykonywane z portalu Azure, programu Azure PowerShell, interfejsu wiersza polecenia lub zresetować interfejsu API, można znaleźć zdarzenie w [dziennika aktywności platformy Azure](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).

Po wykonaniu akcji z systemu operacyjnego maszyny Wirtualnej można znaleźć w dziennikach systemu zdarzenia.

Inne scenariusze, które zazwyczaj powodują ponownego uruchomienia maszyny Wirtualnej obejmują wiele operacji zmiany konfiguracji. Zazwyczaj zobaczysz komunikat ostrzegawczy wskazujący, że wykonywanie określonej akcji spowoduje ponowne uruchomienie maszyny wirtualnej. Przykłady obejmują wszystkie zmiany rozmiaru operacji maszyny Wirtualnej, a zmiana hasła konta administracyjnego i ustawiania statycznego adresu IP.

### <a name="azure-security-center-and-windows-update"></a>Centrum zabezpieczeń Azure i usługi Windows Update
Centrum zabezpieczeń Azure monitoruje codzienne systemu Windows i maszyn wirtualnych systemu Linux dla brakujących aktualizacji systemu operacyjnego. Centrum zabezpieczeń pobiera listę dostępnych zabezpieczeń i krytycznych aktualizacji z witryny Windows Update lub Windows Server Update Services (WSUS), w zależności od tego, który usługa jest skonfigurowana na maszynie Wirtualnej systemu Windows. Centrum zabezpieczeń sprawdza także najnowsze aktualizacje dla serwerów z systemem Linux. Jeśli maszyna wirtualna jest Brak aktualizacji systemu, Centrum zabezpieczeń zaleca się zastosowanie aktualizacji systemu. Stosowanie tych aktualizacji systemu jest kontrolowana przez Centrum zabezpieczeń w portalu Azure. Po zastosowaniu niektóre aktualizacje, ponowne uruchomienie maszyny Wirtualnej może być wymagane. Aby uzyskać więcej informacji, zobacz [stosowanie aktualizacji systemu w Centrum zabezpieczeń Azure](../articles/security-center/security-center-apply-system-updates.md).

Podobnie jak na serwerach lokalnych Azure nie wysyłać aktualizacje z usługi Windows Update do maszyn wirtualnych systemu Windows Azure, ponieważ te maszyny powinny być zarządzany przez użytkowników. Jesteś, jednak zaleca się, aby pozostawić włączone automatyczne ustawienie usługi Windows Update. Automatyczne instalowanie aktualizacji z witryny Windows Update może również spowodować ponowne uruchomienie ostatecznego po zastosowaniu aktualizacji. Aby uzyskać więcej informacji, zobacz [aktualizacji systemu Windows — często zadawane pytania](https://support.microsoft.com/help/12373/windows-update-faq).

### <a name="other-situations-affecting-the-availability-of-your-vm"></a>Innych sytuacjach miało wpływu na dostępność maszyny Wirtualnej
Istnieją inne przypadki, w których Azure może aktywnie zawiesić stosowanie maszyny Wirtualnej. Otrzymasz powiadomień e-mail przed wykonaniem tej akcji, więc będziesz mieć możliwość Rozwiąż problemy z podstawowym. Przykładami problemy, które mają wpływ na dostępność maszyn wirtualnych naruszenia zabezpieczeń i czas wygaśnięcia metody płatności.

### <a name="host-server-faults"></a>Błędy serwera hosta 
Maszyna wirtualna jest hostowana na serwerze fizycznym, który jest używany w centrum danych Azure. Fizyczny serwer jest uruchamiany agenta o nazwie agenta hosta, oprócz kilku składników platformy Azure. Gdy te składniki oprogramowania platformy Azure na serwerze fizycznym przestać odpowiadać, system monitorowania wyzwala ponowne uruchomienie serwera hosta, aby spróbować odzyskiwania. Maszyna wirtualna jest zazwyczaj dostępna ponownie w ciągu pięciu minut i kontynuuje na żywo na tym samym hoście, jak poprzednio.

Błędy serwera są zazwyczaj spowodowane awarii sprzętu, takich jak awaria dysku twardego lub dysk SSD. Azure stale monitoruje tych zdarzeń, identyfikuje podstawowej usterek i zbiera i wydaje aktualizacje po środki zaradcze, zostały wdrożone i przetestowane.

Ponieważ niektóre błędy serwera hosta mogą być specyficzne dla tego serwera, powtarzane sytuacji ponownego uruchomienia maszyny Wirtualnej może zwiększona ręcznie wdrażając maszynę Wirtualną do innego serwera hosta. Ta operacja może być uruchomiona za pomocą **ponownie wdrożyć** opcji na stronie szczegółów maszyny wirtualnej lub przez zatrzymanie i ponowne uruchomienie maszyny Wirtualnej w portalu Azure.

### <a name="auto-recovery"></a>Automatyczne odzyskiwanie
Jeśli serwer hosta nie można ponownie jakiejkolwiek przyczyny, platforma Azure inicjuje przełączenie serwera hosta uszkodzony poza obrót w celu dokładniejszego zbadania akcję automatycznego odzyskiwania. 

Wszystkie maszyny wirtualne na tym hoście, automatycznie są przenoszone do serwera hosta innych, dobrej kondycji. Ten proces jest zwykle ukończone w ciągu 15 minut. Aby dowiedzieć się więcej na temat procesu automatycznego odzyskiwania, zobacz [automatycznego odzyskiwania maszyn wirtualnych](https://azure.microsoft.com/blog/service-healing-auto-recovery-of-virtual-machines).

### <a name="unplanned-maintenance"></a>Nieplanowana konserwacja
W rzadkich przypadkach zespół operacyjny Azure może być konieczne do wykonania działania obsługi do zapewnienia dobrej ogólnej kondycji platformy Azure. To zachowanie może wpłynąć na dostępność maszyn wirtualnych, a jej zazwyczaj wynikiem tę samą akcję automatycznego odzyskiwania zgodnie z wcześniejszym opisem.  

Nieplanowane maintenances są następujące:

- Defragmentacja pilnych węzła
- Aktualizacje przełącznik sieci pilnych

### <a name="vm-crashes"></a>Maszyna wirtualna awarii (Crash)
Maszyny wirtualne mogą ponownie z powodu problemów dotyczących w samej maszyny Wirtualnej. Rola, która działa na maszynie Wirtualnej lub obciążenia mogą wywołać sprawdzanie błędów w systemie operacyjnym gościa. Pomoc dotyczącą określania przyczynę awarii Wyświetl systemu oraz Dzienniki aplikacji dla maszyn wirtualnych systemu Windows i serial dzienniki dla maszyn wirtualnych systemu Linux.

### <a name="storage-related-forced-shutdowns"></a>Związane z magazynowaniem wymuszone zamknięcia systemu
Maszyny wirtualne na platformie Azure polegać na dyskach wirtualnych dla systemu operacyjnego i magazynu danych, który znajduje się w infrastrukturze magazynu Azure. Zawsze, gdy łączność między maszyny Wirtualnej i powiązanymi dyskami wirtualnymi lub dostępności ma to wpływ na więcej niż 120 sekund, platformy Azure wykonuje wymuszone zamknięcie maszyny wirtualne, aby uniknąć uszkodzenia danych. Maszyn wirtualnych są automatycznie włączona ponownie po przywróceniu łączności magazynu. 

Czas trwania zamykania może składać się z pięciu minut, ale może być znacznie dłuższe. Poniżej znajduje się jeden z określonych przypadków, które jest skojarzone z magazynowaniem wymuszone zamknięcia systemu: 

**Ogranicza przekraczającej we/wy**

Maszyny wirtualne mogą zostać tymczasowo zamknięcie żądań We/Wy są ograniczane spójnie, ponieważ jego rozmiar operacji We/Wy na sekundę (IOPS) przekracza limity wejścia/wyjścia dysku. (Standardowe dysku magazynu jest ograniczona do 500 IOPS). Aby zminimalizować ten problem, użyj rozkładanie lub skonfigurować miejsca do magazynowania na gościu maszyny Wirtualnej, w zależności od obciążenia. Aby uzyskać więcej informacji, zobacz [Konfigurowanie maszyn wirtualnych platformy Azure do uzyskania optymalnej wydajności magazynu](http://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx).

Wyższe limity liczby dostępnych za pośrednictwem usługi Azure Premium Storage z maksymalnie 80 000 IOPS. Aby uzyskać więcej informacji, zobacz [magazyn w warstwie Premium wysokiej wydajności](../articles/virtual-machines/windows/premium-storage.md).

### <a name="other-incidents"></a>Inne zdarzenia
W rzadkich przypadkach powszechnie problem może mieć wpływ na wiele serwerów w centrum danych Azure. Jeśli ten problem występuje, zespół Azure wysyła powiadomienia e-mail do odpowiednich subskrypcji. Możesz sprawdzić [pulpit nawigacyjny kondycji usługi Azure](https://azure.microsoft.com/status/) i portal Azure — stan trwającej awarii oraz poza zdarzenia.
