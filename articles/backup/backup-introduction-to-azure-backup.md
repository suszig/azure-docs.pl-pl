---
title: Co to jest Azure Backup? | Microsoft Docs
description: "Za pomocą usług Azure Backup oraz Recovery Services można tworzyć kopie zapasowe oraz przywracać dane i aplikacje z serwerów systemu Windows, komputerów klienckich systemu Windows, serwerów System Center DPM i maszyn wirtualnych platformy Azure."
services: backup
documentationcenter: 
author: markgalioto
manager: cfreeman
editor: tysonn
keywords: "tworzenie i przywracanie kopii zapasowej; recovery services; rozwiązania kopii zapasowych"
ms.assetid: 0d2a7f08-8ade-443a-93af-440cbf7c36c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/31/2016
ms.author: jimpark; trinadhk
translationtype: Human Translation
ms.sourcegitcommit: e29891dc03f8a864ecacc893fd1cc0d3cc1436cb
ms.openlocfilehash: c827c37ae4164ebd9cd2a971e94f073de8c59b46


---
# <a name="what-is-azure-backup"></a>Co to jest Azure Backup?
Azure Backup to oparta na platformie Azure usługa, która umożliwia tworzenie kopii zapasowej (lub ochronę) i przywracanie danych w chmurze Microsoft Cloud. Usługa Azure Backup pozwala zastąpić dotychczasowe rozwiązania tworzenia kopii zapasowych, istniejące lokalnie lub poza siedzibą firmy, rozwiązaniem opartym na chmurze, które jest niezawodne, bezpieczne i konkurencyjne cenowo. Usługa Azure Backup oferuje wiele składników, które możesz pobrać i wdrożyć na odpowiednim komputerze, serwerze lub w chmurze. Wdrażany składnik lub agent zależy od tego, co ma być chronione. Wszystkie składniki usługi Azure Backup (niezależnie od tego, czy dane są chronione lokalnie, czy w chmurze) mogą służyć do tworzenia kopii zapasowych danych w magazynie kopii zapasowych Azure. Informacje o tym, jakich składników należy użyć do ochrony konkretnych danych, aplikacji lub obciążeń, znajdują się w [tabeli składników usługi Azure Backup](backup-introduction-to-azure-backup.md#which-azure-backup-components-should-i-use) (w dalszej części tego artykułu).

[Obejrzyj wideo z omówieniem usługi Azure Backup](https://azure.microsoft.com/documentation/videos/what-is-azure-backup/)

## <a name="why-use-azure-backup"></a>Dlaczego warto używać usługi Azure Backup?
Tradycyjne rozwiązania do tworzenia kopii zapasowych rozwinęły się w kierunku traktowania chmury jako punktu końcowego, czyli statycznego miejsca docelowego przechowywania, podobnego do dysków lub taśm. Chociaż to podejście jest proste, jest ograniczone i nie w pełni wykorzystuje możliwości platformy chmury, co oznacza kosztowne, nieefektywne rozwiązanie. Jedne rozwiązania są kosztowne, ponieważ ostatecznie płaci się za niewłaściwy typ magazynu lub magazyn, który nie jest potrzebny. Inne rozwiązania są często nieefektywne, ponieważ nie oferują typu lub ilości potrzebnego miejsca albo zadania administracyjne wymagają zbyt dużo czasu. Natomiast usługa Azure Backup oferuje następujące kluczowe korzyści:

**Automatyczne zarządzanie magazynem** — hybrydowe środowiska często wymagają heterogenicznego magazynu — znajdującego się w części lokalnie, a w części w chmurze. W usłudze Azure Backup nie płaci się za korzystanie z lokalnych urządzeń magazynujących. Usługa Azure Backup automatycznie przydziela pojemność i zarządza magazynem kopii zapasowych, przy czym użytkownik płaci tylko za faktyczne użycie. Płatność tylko za faktyczne użycie oznacza, że płacisz tylko za pamięć, której używasz. Aby uzyskać więcej informacji, zobacz [artykuł Cennik platformy Azure](https://azure.microsoft.com/pricing/details/backup).

**Nieograniczone skalowanie** — usługa Azure Backup używa dostępnej mocy i nieograniczonej skali chmury Azure do zapewniania wysokiej dostępności — bez narzutu na konserwację lub monitorowanie. Możesz skonfigurować alerty, aby udostępniać informacje o zdarzeniach, ale nie musisz obawiać się o wysoką dostępność danych w chmurze.

**Wiele opcji magazynowania** — aspektem wysokiej dostępności jest replikacja magazynu. Usługa Azure Backup oferuje dwa typy replikacji: [magazyn lokalnie nadmiarowy](../storage/storage-redundancy.md#locally-redundant-storage) i [magazyn z replikacją geograficzną](../storage/storage-redundancy.md#geo-redundant-storage). Wybierz opcję magazynu kopii zapasowych na podstawie potrzeb:

* Lokalnie nadmiarowy magazyn (LRS) replikuje dane trzy razy (tworzy trzy kopie danych) w sparowanym centrum danych w tym samym regionie. LRS to niskokosztowa opcja i idealnie nadaje się dla klientów zwracających uwagę na cenę, ponieważ chroni dane przed lokalnymi awariami sprzętowymi.
* Replikacja geograficzna magazynu (GRS) replikuje dane do regionu pomocniczego (setki kilometrów od lokalizacji głównej źródła danych). GRS kosztuje więcej niż LRS, ale zapewnia wyższy poziom trwałości danych, nawet jeśli występuje awaria regionalna.

**Nieograniczony transfer danych** — usługa Azure Backup nie ogranicza ilości przesyłanych danych przychodzących i wychodzących. W usłudze Azure Backup nie są również naliczane opłaty za przesyłane dane. Jeśli jednak używasz usługi Azure Import/Export do importowania dużych ilości danych, istnieje koszt związany z danymi przychodzącymi. Aby uzyskać więcej informacji o tym koszcie, zobacz [Offline-backup workflow in Azure Backup](backup-azure-backup-import-export.md) (Przepływ pracy tworzenia kopii zapasowej offline w usłudze Azure Backup). Dane wychodzące dotyczą danych przesyłanych z magazynu kopii zapasowych podczas operacji przywracania.

**Szyfrowanie danych** — szyfrowanie danych umożliwia bezpieczną transmisję i przechowywanie danych w chmurze publicznej. Hasło szyfrowania przechowujesz lokalnie i nigdy nie jest ono przesyłane ani przechowywane na platformie Azure. Jeśli jest konieczne przywrócenie jakichkolwiek danych, tylko Ty masz hasło lub klucz szyfrowania.

**Kopia zapasowa spójna na poziomie aplikacji** — czy wykonujesz kopię zapasową serwera plików, maszyny wirtualnej czy też bazy danych SQL, musisz wiedzieć, że punkt odzyskiwania zawiera wszystkie dane wymagane do przywrócenia kopii zapasowej. Usługa Azure Backup umożliwia wykonywanie kopii zapasowych spójnych na poziomie aplikacji, które zapewniają, że do przywrócenia danych nie są potrzebne dodatkowe poprawki. Przywracanie danych spójnych na poziomie aplikacji skraca czas przywracania, co pozwala szybko powrócić do stanu roboczego.

**Długoterminowe przechowywanie** — możesz przechowywać kopie zapasowe danych na platformie Azure przez 99 lat. Zamiast przełączania kopii zapasowych z dysku na taśmę, a następnie przenoszenia taśmy do lokalizacji zewnętrznej w celu długoterminowego przechowywania, możesz użyć platformy Azure do przechowywania krótko- i długoterminowego.

## <a name="which-azure-backup-components-should-i-use"></a>Jakich składników usługi Azure Backup mam użyć?
Jeśli nie masz pewności, które składniki usługi Azure Backup odpowiadają Twoim potrzebom, zapoznaj się z poniższą tabelą zawierającą informacje o tym, co można chronić za pomocą każdego składnika. Witryna Azure Portal udostępnia wbudowanego kreatora, który prowadzi użytkownika przez proces wybierania składnika do pobrania i wdrożenia. Kreator, który jest częścią tworzenia magazynu usługi Recovery Services, poprowadzi użytkownika przez kroki wybierania celu tworzenia kopii zapasowej oraz wybierania danych lub aplikacji do ochrony.

| Składnik | Korzyści | Limity | Co jest chronione? | Gdzie są przechowywane kopie zapasowe? |
| --- | --- | --- | --- | --- |
| Agent usługi Azure Backup (MARS) |<li>Tworzy kopię zapasową plików i folderów w fizycznym lub wirtualnym systemie operacyjnym Windows (maszyny wirtualne mogą być lokalne lub na platformie Azure)<li>Nie jest wymagany oddzielny serwer kopii zapasowych. |<li>Tworzenie kopii zapasowej 3 razy dziennie <li>Brak zależności od aplikacji; przywracanie tylko na poziomie plików, folderów i woluminów, <li>  Brak obsługi systemu Linux. |<li>Pliki, <li>Foldery |Magazyn usługi Azure Backup |
| System Center DPM |<li>Migawki z uwzględnieniem aplikacji (usługa VSS)<li>Pełna elastyczność w odniesieniu do terminów wykonywania kopii zapasowych<li>Poziom szczegółowości odzyskiwania (wszystkie)<li>Można użyć magazynu usługi Azure Backup<li>Obsługa systemu Linux (jeśli jest hostowany na funkcji Hyper-V) |Brak obsługi heterogenicznej (tworzenie kopii zapasowej maszyny wirtualnej VMware, tworzenie kopii zapasowej obciążenia Oracle). |<li>Pliki, <li>Foldery,<li> Woluminy, <li>Maszyny wirtualne,<li> Aplikacje,<li> Obciążenia |<li>magazyn kopii zapasowych Azure,<li> Dysk dołączony lokalnie,<li>  Taśmy (tylko lokalnie) |
| Azure Backup Server |<li>Migawki z uwzględnieniem aplikacji (usługa VSS)<li>Pełna elastyczność w odniesieniu do terminów wykonywania kopii zapasowych<li>Poziom szczegółowości odzyskiwania (wszystkie)<li>Można użyć magazynu usługi Azure Backup<li>Obsługa systemu Linux (jeśli jest hostowany na funkcji Hyper-V)<li>Nie wymaga licencji programu System Center |<li>Brak obsługi heterogenicznej (tworzenie kopii zapasowej maszyny wirtualnej VMware, tworzenie kopii zapasowej obciążenia Oracle).<li>Zawsze wymaga aktywnej subskrypcji platformy Azure<li>Brak obsługi tworzenia kopii zapasowej na taśmie |<li>Pliki, <li>Foldery,<li> Woluminy, <li>Maszyny wirtualne,<li> Aplikacje,<li> Obciążenia |<li>magazyn kopii zapasowych Azure,<li> Dysk dołączony lokalnie |
| Usługa Backup dla maszyn wirtualnych IaaS platformy Azure |<li>Natywne kopie zapasowe w systemach Windows/Linux<li>Nie ma konieczności instalowania określonego agenta<li>Tworzenie kopii zapasowych na poziomie sieci szkieletowej nie wymaga infrastruktury kopii zapasowej |<li>Kopia zapasowa raz dziennie/przywracanie na poziomie dysku<li>Nie można utworzyć kopii zapasowych lokalnie |<li>Maszyny wirtualne, <li>Wszystkie dyski (przy użyciu programu PowerShell) |<p>Magazyn usługi Azure Backup</p> |

## <a name="what-are-the-deployment-scenarios-for-each-component"></a>Jakie są scenariusze wdrażania dla każdego składnika?
| Składnik | Czy można wdrożyć w systemie Azure? | Czy można wdrożyć lokalnie? | Obsługiwany magazyn docelowy |
| --- | --- | --- | --- |
| Agent usługi Azure Backup (MARS) |<p>**Tak**</p> <p>Agenta usługi Azure Backup można wdrożyć na dowolnej maszynie wirtualnej systemu Windows Server działającej na platformie Azure.</p> |<p>**Tak**</p> <p>Agenta usługi Backup można wdrożyć na dowolnej maszynie wirtualnej lub dowolnym komputerze fizycznym z systemem Windows Server.</p> |<p>Magazyn usługi Azure Backup</p> |
| System Center DPM |<p>**Tak**</p><p>Dowiedz się więcej, [jak chronić obciążenia na platformie Azure za pomocą programu System Center DPM](backup-azure-dpm-introduction.md).</p> |<p>**Tak**</p> <p>Dowiedz się więcej, [jak chronić obciążenia i maszyny wirtualne w centrum danych](https://technet.microsoft.com/en-us/system-center-docs/dpm/data-protection-manager).</p> |<p>Dysk dołączony lokalnie,</p> <p>magazyn kopii zapasowych Azure,</p> <p>taśmy (tylko lokalnie)</p> |
| Azure Backup Server |<p>**Tak**</p><p>Dowiedz się więcej, [jak chronić obciążenia na platformie Azure przy użyciu programu Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>**Tak**</p> <p>Dowiedz się więcej, [jak chronić obciążenia na platformie Azure przy użyciu programu Azure Backup Server](backup-azure-microsoft-azure-backup.md).</p> |<p>Dysk dołączony lokalnie,</p> <p>Magazyn usługi Azure Backup</p> |
| Usługa Backup dla maszyn wirtualnych IaaS platformy Azure |<p>**Tak**</p><p>Część sieci szkieletowej Azure</p><p>Składnik przeznaczony do [tworzenia kopii zapasowych maszyn wirtualnych platformy Azure w modelu infrastruktura jako usługa (IaaS) ](backup-azure-vms-introduction.md).</p> |<p>**Nie**</p> <p>Program System Center DPM umożliwia tworzenie kopii zapasowych maszyn wirtualnych w centrum danych.</p> |<p>Magazyn usługi Azure Backup</p> |

## <a name="which-applications-and-workloads-can-be-backed-up"></a>Dla których aplikacji i obciążeń można tworzyć kopie zapasowe?
Poniższa tabela zawiera macierz danych i obciążeń, które mogą być chronione przy użyciu usługi Azure Backup. Kolumna rozwiązania usługi Azure Backup zawiera linki do dokumentacji wdrożeniowej dla tego rozwiązania. Każdy składnik usługi Azure Backup można wdrożyć w środowisku klasycznym (wdrożenie programu Service Manager) lub modelu wdrożenia Menedżera zasobów.

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-include.md)]

| Dane lub obciążenie | Środowisko źródłowe | Rozwiązanie Azure Backup |
| --- | --- | --- |
| Pliki i foldery |Windows Server |<p>[Agent usługi Azure Backup](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ agent usługi Azure Backup),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (w tym agent usługi Azure Backup)</p> |
| Pliki i foldery |Komputer z systemem Windows |<p>[Agent usługi Azure Backup](backup-configure-vault.md),</p> <p>[System Center DPM](backup-azure-dpm-introduction.md) (+ agent usługi Azure Backup),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (w tym agent usługi Azure Backup)</p> |
| Maszyna wirtualna funkcji Hyper-V (Windows) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent usługi Azure Backup),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (w tym agent usługi Azure Backup)</p> |
| Maszyna wirtualna funkcji Hyper-V (Linux) |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent usługi Azure Backup),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (w tym agent usługi Azure Backup)</p> |
| Microsoft SQL Server |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent usługi Azure Backup),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (w tym agent usługi Azure Backup)</p> |
| Microsoft SharePoint |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent usługi Azure Backup),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (w tym agent usługi Azure Backup)</p> |
| Microsoft Exchange |Windows Server |<p>[System Center DPM](backup-azure-backup-sql.md) (+ agent usługi Azure Backup),</p> <p>[Azure Backup Server](backup-azure-microsoft-azure-backup.md) (w tym agent usługi Azure Backup)</p> |
| Maszyny wirtualne IaaS platformy Azure (Windows) |uruchomione na platformie Azure |[Azure Backup (rozszerzenie maszyny wirtualnej)](backup-azure-vms-introduction.md) |
| Maszyny wirtualne IaaS platformy Azure (Linux) |uruchomione na platformie Azure |[Azure Backup (rozszerzenie maszyny wirtualnej)](backup-azure-vms-introduction.md) |

## <a name="linux-support"></a>Obsługa systemu Linux
W poniższej tabeli przedstawiono składniki usługi Azure Backup, które obsługują system Linux.  

| Składnik | Obsługa w systemie Linux (zatwierdzonym przez Azure) |
| --- | --- |
| Agent usługi Azure Backup (MARS) |Nie (tylko agent oparty na systemie Windows) |
| System Center DPM |Spójna na poziomie plików kopia zapasowa tylko dla funkcji Hyper-V<br/> (niedostępne dla maszyny wirtualnej platformy Azure) |
| Azure Backup Server |Spójna na poziomie plików kopia zapasowa tylko dla funkcji Hyper-V<br/> (niedostępne dla maszyny wirtualnej platformy Azure) |
| Usługa Backup dla maszyn wirtualnych IaaS platformy Azure |Tak |

## <a name="using-premium-storage-vms-with-azure-backup"></a>Korzystanie z maszyn wirtualnych usługi Premium Storage przy użyciu usługi Azure Backup
Usługa Azure Backup chroni maszyny wirtualne usługi Premium Storage. Azure Premium Storage to magazyn oparty na dyskach SSD i zaprojektowany z myślą o obsłudze dużych obciążeń wejścia/wyjścia. Usługa Premium Storage jest atrakcyjna dla obciążeń maszyn wirtualnych. Aby uzyskać więcej informacji o usłudze Premium Storage, zobacz artykuł [Premium Storage: High-Performance Storage for Azure Virtual Machine Workloads](../storage/storage-premium-storage.md) (Premium Storage: magazyn o wysokiej wydajności dla obciążeń maszyn wirtualnych platformy Azure)

### <a name="back-up-premium-storage-vms"></a>Tworzenie kopii zapasowej maszyn wirtualnych usługi Premium Storage
Podczas wykonywania kopii zapasowych maszyn wirtualnych usługi Premium Storage usługa Backup tworzy tymczasową lokalizację na koncie usługi Premium Storage. Ta tymczasowa lokalizacja o nazwie „AzureBackup-” ma wielkość równą łącznemu rozmiarowi danych dysków w warstwie Premium dołączonych do maszyny wirtualnej.

> [!NOTE]
> Nie wolno modyfikować ani edytować tej lokalizacji tymczasowej.
>
>

Po zakończeniu zadania tworzenia kopii zapasowej lokalizacja tymczasowa zostanie usunięta. Cena przestrzeni dyskowej użytej na potrzeby lokalizacji tymczasowej jest zgodna z ogólnym [cennikiem usługi Premium Storage](../storage/storage-premium-storage.md#pricing-and-billing).

### <a name="restore-premium-storage-vms"></a>Przywracanie maszyn wirtualnych usługi Premium Storage
Maszyny wirtualne usługi Premium Storage można przywrócić do usługi Premium Storage lub normalnego magazynu. Przywracanie punktu odzyskiwania maszyny wirtualnej usługi Premium Storage do magazynu w warstwie Premium to typowy proces przywracania. Jednak opłacalne może się okazać przywrócenie punktu odzyskiwania maszyny wirtualnej usługi Premium Storage do standardowego magazynu. Z tego typu przywracania można skorzystać, gdy potrzebny jest podzbiór plików z maszyny wirtualnej.

## <a name="what-are-the-features-of-each-backup-component"></a>Jakie są funkcje każdego składnika usługi Azure Backup?
Poniższe sekcje zawierają tabele podsumowujące dostępność lub obsługę różnych funkcji w każdym składniku usługi Azure Backup. W informacjach podanych po każdej tabeli znajdziesz dodatkowe wsparcie lub szczegóły.

### <a name="storage"></a>Magazyn
| Funkcja | Agent usługi Azure Backup | System Center DPM | Azure Backup Server | Usługa Backup dla maszyn wirtualnych IaaS platformy Azure |
| --- | --- | --- | --- | --- |
| Magazyn usługi Azure Backup |![Tak][green] |![Tak][green] |![Tak][green] |![Tak][green] |
| Przechowywanie na dysku | |![Tak][green] |![Tak][green] | |
| Przechowywanie na taśmie | |![Tak][green] | | |
| Kompresja <br/>(w magazynie kopii zapasowych) |![Tak][green] |![Tak][green] |![Tak][green] | |
| Przyrostowa kopia zapasowa |![Tak][green] |![Tak][green] |![Tak][green] |![Tak][green] |
| Deduplikacja dysku | |![Częściowo][yellow] |![Częściowo][yellow] | |

![klucz tabeli](./media/backup-introduction-to-azure-backup/table-key.png)

Dla wszystkich składników preferowanym miejscem docelowym przechowywania jest magazyn usługi Backup. Programy System Center DPM i Backup Server udostępniają również opcję kopiowania na dysk lokalny. Jednak tylko program System Center DPM zapewnia możliwość zapisu danych na taśmowym urządzeniu magazynującym.

#### <a name="compression"></a>Kompresja
Aby zmniejszyć ilość miejsca wymaganego do magazynowania kopie zapasowe są kompresowane. Jedyny składnik, który nie korzysta z kompresji, to rozszerzenie maszyny wirtualnej. Podczas korzystania z rozszerzenia maszyny wirtualnej wszystkie dane kopii zapasowej są kopiowane bez kompresji z konta magazynu do magazynu kopii zapasowych w tym samym regionie. Brak kompresji nieco zwiększa ilość używanej pamięci. Z drugiej strony przechowywanie danych bez kompresji skraca czas ich przywracania.

#### <a name="incremental-backup"></a>Przyrostowa kopia zapasowa
Każdy składnik obsługuje przyrostową kopię zapasową, niezależnie od magazynu docelowego (dysk, taśma, magazyn kopii zapasowych). Tworzenie przyrostowej kopii zapasowej powoduje oszczędność przestrzeni dyskowej i czasu dzięki transferowaniu tylko tych zmian, które zostały wprowadzone od czasu utworzenia ostatniej kopii zapasowej.

#### <a name="disk-deduplication"></a>Deduplikacja dysku
Możesz skorzystać z funkcji deduplikacji podczas wdrażania programu System Center DPM lub serwera Azure Backup Server [na maszynie wirtualnej funkcji Hyper-V](http://blogs.technet.com/b/dpm/archive/2015/01/06/deduplication-of-dpm-storage-reduce-dpm-storage-consumption.aspx). System Windows Server wykonuje deduplikację danych (na poziomie hosta) na wirtualnych dyskach twardych (VHD) dołączonych do maszyny wirtualnej jako magazyn kopii zapasowych.

> [!NOTE]
> Funkcja deduplikacji nie jest dostępna na platformie Azure dla żadnego składnika usługi Backup. Gdy na platformie Azure wdrożone są programy System Center DPM i Backup Server, dyski magazynowania dołączone do maszyny wirtualnej nie mogą być deduplikowane.
>
>

### <a name="security"></a>Bezpieczeństwo
| Funkcja | Agent usługi Azure Backup | System Center DPM | Azure Backup Server | Usługa Backup dla maszyn wirtualnych IaaS platformy Azure |
| --- | --- | --- | --- | --- |
| Bezpieczeństwo sieci<br/> (na platformę Azure) |![Tak][green] |![Tak][green] |![Tak][green] |![Częściowo][yellow] |
| Bezpieczeństwo danych<br/> (na platformie Azure) |![Tak][green] |![Tak][green] |![Tak][green] |![Częściowo][yellow] |

![klucz tabeli](./media/backup-introduction-to-azure-backup/table-key.png)

#### <a name="network-security"></a>Bezpieczeństwo sieci
Cały ruch sieciowy z serwerów użytkownika do magazynu usługi Backup związany z tworzeniem kopii zapasowych jest szyfrowany przy użyciu algorytmu Advanced Encryption Standard 256. Dane kopii zapasowej są przesyłane za pośrednictwem bezpiecznego połączenia HTTPS. Także w magazynie usługi Backup dane kopii zapasowych są przechowywane w postaci zaszyfrowanej. Tylko Ty, jako klient platformy Azure, dysponujesz hasłem do odblokowania tych danych. Firma Microsoft nie może odszyfrować danych kopii zapasowej w żadnym punkcie.

> [!WARNING]
> Po utworzeniu magazynu kopii zapasowych tylko Ty masz dostęp do klucza szyfrowania. Firma Microsoft nigdy nie przechowuje kopii klucza szyfrowania i nie ma do niego dostępu. W przypadku utraty klucza firma Microsoft nie może odzyskać danych kopii zapasowej.
>
>

#### <a name="data-security"></a>Bezpieczeństwo danych
Tworzenie kopii zapasowych maszyn wirtualnych Azure wymaga skonfigurowania szyfrowania *w ramach* maszyny wirtualnej. W przypadku maszyn wirtualnych systemu Windows należy użyć funkcji BitLocker, a w przypadku maszyn wirtualnych systemu Linux programu **dm-crypt**. Usługa Azure Backup nie szyfruje automatycznie danych kopii zapasowych, które przechodzą przez tę ścieżkę.

### <a name="network"></a>Sieć
| Funkcja | Agent usługi Azure Backup | System Center DPM | Azure Backup Server | Usługa Backup dla maszyn wirtualnych IaaS platformy Azure |
| --- | --- | --- | --- | --- |
| Kompresja sieci <br/>(do **serwera kopii zapasowych**) | |![Tak][green] |![Tak][green] | |
| Kompresja sieci <br/>(do **magazynu kopii zapasowych**) |![Tak][green] |![Tak][green] |![Tak][green] | |
| Protokół sieciowy <br/>(do **serwera kopii zapasowych**) | |TCP |TCP | |
| Protokół sieciowy <br/>(do **magazynu kopii zapasowych**) |HTTPS |HTTPS |HTTPS |HTTPS |

![klucz tabeli](./media/backup-introduction-to-azure-backup/table-key-2.png)

Rozszerzenie maszyny wirtualnej (na maszynie wirtualnej IaaS) odczytuje dane bezpośrednio z konta magazynu na platformie Azure w sieci magazynowania, więc kompresja tego ruchu nie jest konieczna.

W przypadku tworzenia kopii zapasowej danych w programie System Center DPM lub na serwerze Azure Backup Server należy kompresować dane przechodzące z serwera podstawowego na serwer kopii zapasowych. Umożliwi to zmniejszenie przepustowości.

#### <a name="network-throttling"></a>Ograniczanie przepustowości sieci
Agent usługi Azure Backup umożliwia ograniczanie użycia sieci, co pozwala na sterowanie wykorzystaniem przepustowości sieci w trakcie transferu danych. Ograniczanie może być przydatne, gdy kopie zapasowe danych mają być tworzone podczas godzin pracy, ale proces tworzenia kopii zapasowej nie może kolidować z innym ruchem internetowym. Ograniczanie transferu danych ma zastosowanie do operacji tworzenia kopii zapasowej i przywracania.

### <a name="backup-and-retention"></a>Tworzenie kopii zapasowej i przechowywanie
|  | Agent usługi Azure Backup | System Center DPM | Azure Backup Server | Usługa Backup dla maszyn wirtualnych IaaS platformy Azure |
| --- | --- | --- | --- | --- |
| Częstotliwość wykonywania kopii zapasowych<br/> (do magazynu kopii zapasowych) |Trzy kopie zapasowe dziennie |Dwie kopie zapasowe dziennie |Dwie kopie zapasowe dziennie |Jedna kopia zapasowa dziennie |
| Częstotliwość wykonywania kopii zapasowych<br/> (na dysku) |Nie dotyczy |<li>Co 15 minut dla programu SQL Server <li>Co godzinę dla innych obciążeń |<li>Co 15 minut dla programu SQL Server <li>Co godzinę dla innych obciążeń</p> |Nie dotyczy |
| Opcje przechowywania |Codziennie, co tydzień, co miesiąc, co rok |Codziennie, co tydzień, co miesiąc, co rok |Codziennie, co tydzień, co miesiąc, co rok |Codziennie, co tydzień, co miesiąc, co rok |
| Okres przechowywania |Do 99 lat |Do 99 lat |Do 99 lat |Do 99 lat |
| Punkty odzyskiwania w magazynie usługi Backup |Nieograniczona liczba |Nieograniczona liczba |Nieograniczona liczba |Nieograniczona liczba |
| Punkty odzyskiwania na dysku lokalnym |Nie dotyczy |<li>64 dla serwerów plików,<li>448 dla serwerów aplikacji |<li>64 dla serwerów plików,<li>448 dla serwerów aplikacji |Nie dotyczy |
| Punkty odzyskiwania na taśmie |Nie dotyczy |Nieograniczona liczba |Nie dotyczy |Nie dotyczy |

## <a name="what-is-the-vault-credential-file"></a>Co to jest plik poświadczeń magazynu?
Plik poświadczeń magazynu jest to certyfikat wygenerowany przez portal dla każdego magazynu kopii zapasowych. Portal przekazuje następnie klucz publiczny do usługi Access Control Service (ACS). Klucz prywatny jest udostępniany podczas pobierania poświadczeń. Służy on do rejestrowania chronionych komputerów. Klucz prywatny jest tym, co pozwala uwierzytelniać serwery lub komputery do wysyłania danych kopii zapasowej do określonego magazynu kopii zapasowych.

Aby zarejestrować serwery lub komputery, wystarczy używać tylko poświadczenia magazynu. Należy jednak zadbać o poświadczenia magazynu, ponieważ, w razie jego zgubienia lub uzyskania przez inne osoby, poświadczenia magazynu mogą zostać użyte do rejestrowania innych maszyn w tym samym magazynie. Ponieważ dane kopii zapasowej są szyfrowane przy użyciu hasła dostępnego tylko dla Ciebie, istniejące dane kopii zapasowej nie zostaną ujawnione. Poświadczenia magazynu wygasają po upływie 48 godzin. Chociaż możesz pobierać poświadczenia magazynu dla magazynu kopii zapasowych tak często, jak chcesz, tylko najnowsze poświadczenia mogą służyć do rejestracji.

## <a name="how-does-azure-backup-differ-from-azure-site-recovery"></a>Czym różni się usługa Azure Backup od usługi Azure Site Recovery?
Usługi Azure Backup i Azure Site Recovery są zbliżone do siebie w tym sensie, że obie tworzą kopie zapasowe danych i mogą je przywracać, ale ich istota jest różna.

Usługa Azure Backup chroni dane lokalnie i w chmurze. Usługa Azure Site Recovery koordynuje replikację maszyn wirtualnych i serwerów fizycznych, pracę w trybie failover i powrót po awarii. Obie te usługi są ważne, ponieważ rozwiązanie odzyskiwania po awarii wymaga bezpiecznego przechowywania danych z możliwością ich odzyskania (usługa Backup) *oraz* utrzymywania dostępności obciążeń (usługa Site Recovery) w przypadku wystąpienia awarii.

Następujące pojęcia mogą ułatwić podejmowanie ważnych decyzji związanych z tworzeniem kopii zapasowych i odzyskiwaniem po awarii.

| Pojęcie | Szczegóły | Tworzenie kopii zapasowych | Odzyskiwanie awaryjne (DR) |
| --- | --- | --- | --- |
| Cel punktu odzyskiwania (recovery point objective, RPO) |Dopuszczalna ilość utraconych danych, jeśli wymagane jest odzyskiwanie. |Rozwiązania tworzenia kopii zapasowych charakteryzują się dużą zmiennością dopuszczalnej wartości RPO. Kopie zapasowe maszyny wirtualnej mają zwykle RPO na poziomie jednego dnia, natomiast kopie zapasowe bazy danych mają RPO o wartości 15 minut. |Rozwiązania odzyskiwania awaryjnego mają niską wartość RPO. Kopia do odzyskiwania awaryjnego może być starsza o kilka sekund lub kilka minut. |
| Cel czasu odzyskiwania (recovery time objective, RTO) |Ilość czasu potrzebnego do ukończenia odzyskiwania lub przywracania. |Ilość danych, które musi przetworzyć rozwiązanie kopii zapasowych, jest zwykle znacznie wyższa (ze względu na większą wartość RPO), a to prowadzi do większych wartości RTO. Na przykład przywrócenie danych z taśmy może potrwać kilka dni, zależnie od czasu potrzebnego do przetransportowania taśmy z oddalonej lokalizacji. |Rozwiązania w zakresie odzyskiwania awaryjnego mają mniejszą wartość RTO, ponieważ są one bardziej zsynchronizowane ze źródłem. W takim przypadku mniej zmian wymaga przetworzenia. |
| Przechowywanie |Jak długo dane muszą być przechowywane |W przypadku scenariuszy wymagających odzyskiwania operacyjnego (uszkodzenie danych, nieumyślne usunięcie pliku, błąd systemu operacyjnego) dane kopii zapasowej są zwykle zachowywane przez 30 dni lub mniej.<br>Z punktu widzenia zgodności z przepisami dane mogą wymagać przechowywania przez miesiące, a nawet lata. Dane z kopii zapasowej doskonale nadają się do archiwizacji w takich przypadkach. |Odzyskiwanie awaryjne wymaga tylko danych odzyskiwania operacyjnego, które zazwyczaj zajmuje kilka godzin lub maksymalnie jeden dzień. Z powodu szczegółowego przechwytywania danych używanego w rozwiązaniach DR korzystanie z danych usługi DR do długoterminowego przechowywania danych nie jest zalecane. |

## <a name="next-steps"></a>Następne kroki
Użyj jednego z następujących samouczków w celu uzyskania szczegółowych, krok po kroku, instrukcji dotyczących ochrony danych w systemie Windows Server lub ochrony maszyny wirtualnej (VM) na platformie Azure:

* [Tworzenie kopii zapasowych plików i folderów](backup-try-azure-backup-in-10-mins.md)
* [Tworzenie kopii zapasowej maszyn wirtualnych platformy Azure](backup-azure-vms-first-look.md)

Szczegółowe informacje na temat ochrony innych obciążeń możesz uzyskać w jednym z następujących artykułów:

* [Tworzenie kopii zapasowej systemu Windows Server](backup-configure-vault.md)
* [Tworzenie kopii zapasowej obciążeń aplikacji](backup-azure-microsoft-azure-backup.md)
* [Tworzenie kopii zapasowej maszyn wirtualnych IaaS platformy Azure](backup-azure-vms-prepare.md)

[zielony]: ./media/backup-introduction-to-azure-backup/green.png
[żółty]: ./media/backup-introduction-to-azure-backup/yellow.png
[czerwony]: ./media/backup-introduction-to-azure-backup/red.png



<!---HONumber=Nov16_HO2-->


