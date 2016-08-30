<properties
    pageTitle="Omówienie funkcji usługi Azure Batch | Microsoft Azure"
    description="Opis funkcji usługi Batch i jej interfejsów API z punktu widzenia programowania."
    services="batch"
    documentationCenter=".net"
    authors="yidingzhou"
    manager="timlt"
    editor=""/>

<tags
    ms.service="batch"
    ms.devlang="multiple"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="big-compute"
    ms.date="05/12/2016"
    ms.author="yidingz;marsma"/>

# Omówienie funkcji usługi Azure Batch

Ten artykuł zawiera omówienie podstawowych funkcji interfejsu API usługi Azure Batch. Podczas programowania rozproszonego rozwiązania obliczeniowego przy użyciu interfejsów API [Batch REST][batch_rest_api] lub [Batch .NET][batch_net_api] będzie używanych wiele obiektów i funkcji, które omówiono poniżej.

> [AZURE.TIP] Opis techniczny usługi Batch na wyższym poziomie znajduje się w temacie [Azure Batch basics](batch-technical-overview.md) (Podstawy usługi Azure Batch).

## <a name="workflow"></a>Przepływ pracy usługi Batch

Następujący przepływ pracy na wyższym poziomie jest typowy dla tego używanego przez prawie wszystkie rozproszone scenariusze obliczeniowe opracowane w ramach usługi Batch:

1. Przekaż *pliki danych*, które chcesz użyć w rozproszonym scenariuszu obliczeniowym do konta usługi [Magazyn Azure][azure_storage]. Aby usługa Batch mogła uzyskać do nich dostęp, pliki te muszą znajdować się na koncie usługi Magazyn Azure. Gdy zadania zostaną uruchomione, będą pobierać te pliki do [węzłów obliczeniowych](#computenode).

2. Przekaż zależne *pliki binarne* do konta usługi Magazyn Azure. Te pliki binarne obejmują program, który ma być uruchamiany przez zadania oraz wszelkie zależne od niego zestawy. Te pliki muszą być również dostępne z konta usługi Magazyn Azure, by zadania mogły je pobrać do węzłów obliczeniowych.

3. Utwórz [Pulę](#pool) węzłów obliczeniowych. Określ [rozmiar węzłów obliczeniowych][cloud_service_sizes], który zostanie użyty podczas tworzenia puli i kiedy zadanie zostanie uruchomione, zostanie do niego przypisany węzeł w tej puli.

4. Utwórz [Zadanie](#job). Zadanie służy do zarządzania zadaniami kolekcji.

5. Dodaj [podzadania](#task) do zadania. Każde zadanie korzysta z programu, który został przekazany, aby przetwarzać informacje w plikach danych przekazanych do konta usługi Magazyn Azure.

6. Monitoruj postęp zadania i pobierz wyniki.

> [AZURE.NOTE] Potrzebne będzie [konto usługi Batch](batch-account-create-portal.md) do korzystania z usługi Batch i niemal we wszystkich rozwiązaniach do przechowywania i pobierania plików będzie używane konto usługi [Magazyn Azure][azure_storage]. Usługa Batch obsługuje obecnie tylko typ konta magazynu **ogólnego przeznaczenia**, zgodnie z opisem w kroku 5 [Tworzenie konta magazynu](../storage/storage-create-storage-account.md#create-a-storage-account) w temacie [About Azure storage accounts](../storage/storage-create-storage-account.md) (Informacje o kontach magazynu Azure).

Poniższe sekcje zawierają informacje na temat wszystkich zasobów wspomnianych w powyższym przepływie pracy oraz wielu innych funkcji w ramach usługi Batch, które umożliwią wdrożenie rozproszonego scenariusza obliczeniowego.

## <a name="resource"></a> Zasoby usługi Batch

Podczas korzystania z usługi Batch używanych będzie wiele z następujących zasobów. Niektóre z tych zasobów, np. konta, węzły obliczeniowe, pule, zadania i zadania są używane we wszystkich rozwiązaniach usługi Batch. Inne, takie jak harmonogramy zadań i pakiety aplikacji, są przydatnymi, ale opcjonalnymi funkcjami.

- [Konto](#account)
- [Węzeł obliczeniowy](#computenode)
- [Pula](#pool)
- [Zadanie](#job)
- [Zadanie](#task)
    - [Zadanie uruchamiania](#starttask)
    - [Zadanie Menedżera zadań](#jobmanagertask)
    - [Zadania przygotowania i zwolnienia zadań](#jobpreprelease)
    - [Zadania obejmujące wiele wystąpień](#multiinstance)
    - [Zależności zadań](#taskdep)
- [Harmonogramy zadań](#jobschedule)
- [Pakiety aplikacji](#appkg)

### <a name="account"></a>Konto

Konto usługi Batch jest jednoznacznie zdefiniowanym obiektem w ramach usługi Batch. Całe przetwarzanie jest skojarzone z kontem usługi Batch. Podczas wykonywania operacji w usłudze Batch potrzebna jest zarówno nazwa konta jak i klucz konta. Informacje na temat tworzenia konta usługi Batch można znaleźć w temacie [Create and manage an Azure Batch account in the Azure portal](batch-account-create-portal.md) (Tworzenie konta usługi Azure Batch oraz zarządzanie nim w portalu Azure).

### <a name="computenode"></a>Węzeł obliczeniowy

Węzeł obliczeniowy jest maszyną wirtualną platformy Azure przeznaczoną do określonego obciążenia dla danej aplikacji. Rozmiar węzła określa liczbę rdzeni procesora CPU, pojemność pamięci oraz rozmiar lokalnego systemu plików przydzielony do tego węzła. Węzeł może mieć dowolny z [rozmiarów węzła usługi w chmurze][cloud_service_sizes], z wyjątkiem A0.

Węzły mogą uruchamiać pliki wykonywalne i skrypty, m.in. pliki wykonywalne (.exe), pliki poleceń (.cmd), pliki wsadowe (.bat) i skrypty programu PowerShell. Węzeł ma również następujące atrybuty:

- Standardową **strukturę folderów** i skojarzone **zmienne środowiskowe** ze szczegółami dotyczącymi ich ścieżek są tworzone w każdym węźle obliczeniowym. Więcej informacji znajduje się w temacie [Files and directories](#files) (Pliki i katalogi) poniżej.
- **Zmienne środowiskowe** dostępne do użytku przez zadania.
- Ustawienia **Zapory** skonfigurowane do kontroli dostępu.
- Jeśli wymagany jest **dostęp zdalny** do węzła obliczeniowego (np. w celu debugowania), można uzyskać plik RDP, którego można następnie użyć, aby uzyskać dostęp do węzła za pośrednictwem *pulpitu zdalnego*.

### <a name="pool"></a>Pula

Pul jest kolekcją węzłów, w których jest uruchamiana aplikacja. Pula może być tworzona ręcznie przez użytkownika lub przez usługę Batch automatycznie po określeniu pracy do wykonania. Można tworzyć pulę, która spełnia wymagania aplikacji oraz nią zarządzać i pule mogą być używane tylko przez konto usługi Batch, na którym zostały utworzone. Konto usługi Batch może zawierać więcej niż jedną pulę.

Pule usługi Batch są oparte na podstawowej platformie obliczeniowej Azure: pule usługi Batch zapewniają alokację na dużą skalę, instalowania aplikacji, dystrybucję danych i monitorowanie kondycji, jak również elastyczne dostosowanie liczby węzłów obliczeniowych w puli (skalowanie).

Do każdego węzła, który jest dodawany do puli zostaje przypisana unikatowa nazwa i adres IP. Gdy węzeł zostanie usunięty z puli, wszelkie zmiany wprowadzone w systemie operacyjnym lub w plikach zostaną utracone, a jego nazwa i adres IP zostaną zwolnione do użytku w przyszłości. Gdy węzeł opuści pulę, oznacza to, że zakończył się jego okres istnienia.

Można skonfigurować pulę, aby umożliwić komunikację między znajdującymi się w niej węzłami. Jeśli dla puli pojawi się żądanie o komunikację wewnątrz puli, usługa Batch umożliwia użycie portów o numerach wyższych niż 1100 w każdym węźle w puli. Każdy węzeł w puli jest skonfigurowany do zezwalania na połączenia przychodzące tylko do tego zakresu portów i tylko z innych węzłów w ramach puli. Jeśli aplikacja nie wymaga komunikacji między węzłami, usługa Batch może przydzielić potencjalnie dużą liczbę węzłów do puli z wielu różnych klastrów i centrów danych, aby umożliwić zwiększoną równoległą moc przetwarzania.

Podczas tworzenia puli można określić następujące atrybuty:

- **Rozmiar węzłów** w puli
    - Należy wybrać odpowiedni rozmiar węzła, biorąc pod uwagę cechy i wymagania danej aplikacji lub aplikacji, które mają być w węzłach uruchamiane. Rozmiar węzła jest zazwyczaj wybierany w oparciu o założenie, że w węźle będzie uruchamiane w danym momencie jedno zadanie. Biorąc pod uwagę taki czynniki jak to, czy aplikacja jest wielowątkowa oraz ile pamięci zużywa, pomogą określić najbardziej odpowiedni i ekonomiczny rozmiar węzła. Można mieć przypisanych wiele zadań i uruchomionych równolegle wiele wystąpień aplikacji, a wtedy wybierany jest zwykle większy węzeł — więcej informacji znajduje się poniżej w temacie „Task scheduling policy” („Zasady planowania zadań”).
    - Wszystkie węzły w puli muszą mieć taki sam rozmiar. Jeśli mają być uruchomione różne aplikacje o różnych wymaganiach systemowych i/lub poziomach obciążenia, należy utworzyć oddzielne pule.
    - Dla puli można skonfigurować wszystkie [rozmiary węzłów usługi w chmurze][cloud_service_sizes], z wyjątkiem A0.

- **Rodzina systemu operacyjnego** i **wersja**, która została uruchomiona w węzłach
    - Podobnie jak w przypadku ról procesów roboczych w ramach usług w chmurze *rodzinę systemów operacyjnych* i *wersję systemu operacyjnego* można określić (więcej informacji o rolach procesów roboczych można znaleźć w sekcji [Tell me about cloud services][about_cloud_services] [Więcej informacji o usługach w chmurze] w temacie *Compute Hosting Options Provided by Azure* [Opcje hostingu obliczeniowego zapewniane na platformie Azure]).
    - Rodzina systemów operacyjnych określa też, które wersje platformy .NET są instalowane z systemem operacyjnym.
    - Tak samo jak w przypadku ról procesów roboczych zaleca się określenie `*` dla wersji systemu operacyjnego, aby węzły były uaktualniane automatycznie, a niedawno wydane wersje nie wymagały żadnej pracy. Podstawowym warunkiem wybrania określonej wersji systemu operacyjnego jest upewnienie się, czy została zachowana zgodność aplikacji przez zezwolenie na testowanie zgodności z poprzednimi wersjami przed zezwoleniem na aktualizację wersji. Po zweryfikowaniu można zaktualizować wersję systemu operacyjnego dla puli i zainstalować nowy obraz systemu operacyjnego — wszystkie uruchomione zadania zostaną przerwane i ponownie umieszczone w kolejce.

- **Docelowa liczba węzłów**, która powinna być dostępna dla puli

- **Zasady skalowania** dla puli
    - Oprócz liczby węzłów można również określić [formułę automatycznego skalowania](batch-automatic-scaling.md) dla puli. Usługa Batch wykona formułę i dostosuje liczbę węzłów w ramach puli na podstawie różnych parametrów puli i zadań, które można określić.

- Zasady **planowania zadań**
    - Opcja konfiguracji [maksymalnej liczby zadań na węzeł](batch-parallel-node-tasks.md) określa maksymalną liczbę zadań, które można uruchomić równolegle na poszczególnych węzłach w ramach puli.
    - Domyślna konfiguracja polega na tym, że w danym momencie w węźle obliczeniowym jest uruchamiane jedno zadanie, ale istnieją scenariusze, w których korzystne jest, by w danym momencie w węźle wykonywanych było więcej zadań. Przykładem może być sytuacja, w której wykorzystanie węzła należy zwiększyć, jeśli aplikacja ma oczekiwać na operacje We/Wy. Wykonywanie wielu aplikacji równocześnie spowoduje zwiększenie wykorzystania procesora CPU. Innym przykładem jest zmniejszenie liczby węzłów w puli. Może to zmniejszyć ilość transferu danych wymaganego do dużych zestawów danych referencyjnych — jeśli rozmiar węzła A1 jest wystarczający dla aplikacji, zamiast tego można wybrać rozmiar węzła A4 i pulę skonfigurowaną do 8 zadań równoległych, z których każde korzysta z rdzenia.
    - Można również określić „typ wypełnienia”, który decyduje o tym, czy usługa Batch rozdziela zadania równomiernie we wszystkich węzłach, czy zapełnia poszczególne węzły maksymalną liczbą zadań przed przypisaniem zadań do innego węzła w puli.

- **Stan komunikacji** węzłów w puli
    - Pulę można skonfigurować do zezwalania komunikacji między węzłami w puli, co określa jej podstawową infrastrukturę sieciową. Należy pamiętać, że ma to również wpływ na umieszczanie węzłów w ramach klastrów.
    - W większości scenariuszy zadania są wykonywane niezależnie i nie muszą komunikować się ze sobą, ale istnieją aplikacje, w których zadania muszą komunikować się.

- **Zadanie uruchamiania** dla węzłów w puli
    - Można określić *zadanie uruchamiania*, które jest wykonywane za każdym razem, gdy zostaje dołączony do puli węzeł obliczeniowy i gdy jest ponownie uruchamiana pula. Jest ono często używane do instalowania aplikacji, która będzie używana przez zadania uruchomione w węźle.

### <a name="job"></a>Zadanie

Zadanie jest kolekcją zadań i określa, jak są wykonywane obliczenia w węzłach obliczeniowych w puli.

- Zadanie określa **pulę**, w której będzie uruchamiana praca. Pula może być istniejącą pulą wcześniej utworzoną do użytku przez wiele zadań lub utworzoną na żądanie dla każdego zadania skojarzonego z harmonogramem zadań lub wszystkich zadań skojarzonych z harmonogramem zadań.
- Można określić opcjonalny **priorytet zadania**. Po przesłaniu zadania o wyższym priorytecie niż zadania w toku zadania o wyższym priorytecie zostają wstawione do kolejki przed zadaniami o niższym priorytecie. Zadania o niższym priorytecie, które zostały już uruchomione nie zostaną wywłaszczone.
- **Ograniczenia** zadania określają pewne ograniczenia dla zadań.
    - Dla zadań można ustawić **maksymalny czas zegara ściennego**. Jeśli zadania zostaną uruchomione na czas dłuższy niż określony maksymalny czas zegara ściennego, zadanie i wszystkie zadania skojarzone zostaną zakończone.
    - Usługa Azure Batch może wykrywać zadania, które zakończyły się niepowodzeniem i ponowić próbę ich wykonania. **Maksymalną liczbę ponownych prób wykonania zadania** można określić jako ograniczenie, decydujące m.in. o tym, czy ponowna próba wykonania zadania jest podejmowana zawsze, czy nigdy. Ponawianie próby wykonania zadania oznacza, że zadanie zostaje ponownie umieszczone w kolejce, aby zostało ponownie uruchomione.
- Zadania mogą być dodawane do zadania przez aplikację kliencką lub można określić [Zadanie Menedżera zadań](#jobmanagertask). Zadanie Menedżera zadań używa interfejsu API usługi Batch i zawiera informacje niezbędne do utworzenia wymaganych zadań do danego zadania, a zadanie jest uruchamiane na jednym z węzłów obliczeniowych w puli. Zadanie Menedżera zadania jest obsługiwane konkretnie w usłudze Batch — jest umieszczane w kolejce natychmiast po utworzeniu zadania i uruchamiane ponownie, jeśli zakończy się niepowodzeniem. Zadanie Menedżera zadań jest wymagane do zadań utworzonych w harmonogramie zadań, ponieważ jest jedynym sposobem definiowania zadań przed utworzeniem wystąpienia zadania. Więcej informacji na temat zadań Menedżera zadań znajduje się poniżej.

### <a name="task"></a>Zadanie

Zadanie jest jednostką obliczeniową, która jest skojarzona z zadaniem i jest uruchamiane w węźle. Zadania są przypisywane do węzła w celu wykonania lub są umieszczane w kolejce, dopóki węzeł nie zostanie zwolniony. Zadanie używa następujących zasobów:

- Aplikacji określonej w **wierszu polecenia** zadania.

- **Plików zasobów** zawierających dane do przetworzenia. Te pliki są automatycznie kopiowane do węzła z Magazynu obiektów Blob na koncie usługi Magazyn Azure **ogólnego przeznaczenia**. Więcej informacji znajduje się w tematach *Start task* (Zadanie uruchamiania) i [Files and directories](#files) (Pliki i katalogi) poniżej.

-  **Zmienne środowiskowe** wymagane w aplikacji. Więcej informacji znajduje się w temacie [Environment settings for tasks](#environment) (Ustawienia środowiska dla zadań) poniżej.

-  **Ograniczenia**, zgodnie z którymi powinny odbywać się obliczenia. Na przykład maksymalny czas, w którym zadanie może działać, maksymalna liczba prób ponownego wykonania zadania, jeśli zakończy się niepowodzeniem i maksymalny czas zachowania plików w katalogu roboczym.

Oprócz zadań zdefiniowanych do wykonywania obliczeń w węźle w usłudze Batch dostępne są również następujące zadania specjalne:

- [Zadanie uruchamiania](#starttask)
- [Zadanie Menedżera zadań](#jobmanagertask)
- [Zadania przygotowania i zwolnienia zadań](#jobmanagertask)
- [Zadania obejmujące wiele wystąpień](#multiinstance)
- [Zależności zadań](#taskdep)

#### <a name="starttask"></a>Zadanie uruchamiania

Przez skojarzenie **zadania uruchamiania** z pulą można skonfigurować środowisko operacyjne jego węzłów, wykonując takie akcje jak instalowanie oprogramowania lub uruchamianie procesów w tle. Zadanie uruchamiania jest uruchamiane za każdym razem, gdy zostaje uruchomiony węzeł pod warunkiem, że pozostaje w puli, również w przypadku gdy węzeł zostaje dodany do puli po raz pierwszy. Główną korzyścią płynącą z zadania uruchamiania jest to, że zawiera ono wszystkie informacje niezbędne do konfiguracji węzłów obliczeniowych oraz instalacji aplikacji niezbędnych do wykonania zadania. W związku z tym zwiększenie liczby węzłów w puli wymaga jedynie określenia nowej liczby węzłów docelowych — usługa Batch ma już wszystkie informacje potrzebne do konfiguracji nowych węzłów i przygotowania ich do akceptowania zadań.

Podobnie jak w przypadku dowolnego zadania w usłudze Batch można określić listę **plików zasobów** w [Magazynie Azure][azure_storage] dodatkowo do **wiersza polecenia** do wykonania. Usługa Azure Batch najpierw skopiuje pliki z usługi Magazyn Azure, a następnie uruchomi wiersz polecenia. Dla zadania uruchamiania puli lista plików zwykle zawiera pakiet aplikacji lub pliki, ale może także obejmować dane referencyjne do używana przez wszystkie zadania uruchomione w węzłach obliczeniowych. Wiersz polecenia zadania uruchamiania może wykonywać skrypt programu PowerShell lub wykonać operację `robocopy`, na przykład skopiować pliki aplikacji do folderu „udostępnione”, a następnie uruchomić Instalator MSI lub `setup.exe`.

> [AZURE.IMPORTANT] Usługa Batch obsługuje obecnie *tylko* typ konta magazynu **Ogólnego przeznaczenia**, zgodnie z opisem w kroku 5 [Tworzenie konta magazynu](../storage/storage-create-storage-account.md#create-a-storage-account) w temacie [Informacje o kontach magazynu Azure](../storage/storage-create-storage-account.md). Zadania usługi Batch (w tym zadania standardowe, zadania uruchamiania, zadania przygotowania i zwolnienia zadań) muszą określać pliki zasobów, które znajdują się *tylko* na kontach magazynu **ogólnego przeznaczenia**.

Zwykle najlepiej jest, jeśli usługa Batch będzie oczekiwać na zakończenie zadania uruchamiania, zanim będzie można uznać węzeł za gotowy do przypisania do niego zadań, ale można to ustawienie skonfigurować.

Jeśli zadanie uruchamiania w węźle obliczeniowym zakończy się niepowodzeniem, stan węzła zostanie zaktualizowany w celu poinformowania o awarii i węzeł nie będzie dostępny dla zadań do przypisania. Zadanie uruchamiania może zakończyć się niepowodzeniem, jeśli wystąpi problem z kopiowaniem plików zasobów z magazynu lub jeśli proces wykonywany przez wiersz polecenia zwróci kod zakończenia różny od zera.

#### <a name="jobmanagertask"></a>Zadanie Menedżera zadań

**Zadanie Menedżera zadań** jest zazwyczaj używane do kontroli i/lub monitorowania wykonywania zadania. Np. do tworzenia i przesyłania zadań, określania dodatkowych zadań do uruchomienia i ustalania, kiedy praca została zakończona. Zadanie Menedżera zadań nie ogranicza się do tych operacji, jednak jest w pełni użytecznym zadaniem, które może wykonywać dowolne operacje wymagane do ukończenia zadania. Na przykład zadanie Menedżera zadań może pobrać plik określony jako parametr, przeanalizowanie zawartość tego pliku i przesłać dodatkowe zadania na podstawie tej zawartości.

Zadanie Menedżera zadań jest uruchamiane przed wszystkimi innymi zadaniami i udostępnia następujące funkcje:

- Zostaje automatycznie przesyłane jako zadanie przez usługę Batch po utworzeniu zadania.

- Zostaje zaplanowane do wykonania przed innymi zadaniami w ramach zadania.

- Jego skojarzony węzeł jest ostatnim do usunięcia z puli w przypadku zmniejszania puli.

- Jego zakończenie może mieć związek z zakończeniem wszystkich zadań w zadaniu.

- Zadanie Menedżera zadań otrzymuje najwyższy priorytet, jeśli musi zostać ponownie uruchomione. Jeśli węzeł bezczynny jest niedostępny, usługa Batch może zakończyć jedno z innych uruchomionych zadań w puli, aby zwolnić miejsce na uruchomienie zadania Menedżera zadań.

- Zadanie Menedżera zadań w ramach jednego zadania nie ma pierwszeństwa przed zadaniami innych zadań. W zadaniach przestrzegane są tylko priorytety na poziomie zadań.

#### <a name="jobpreprelease"></a>Zadania przygotowania i zwolnienia zadań

Usługa Batch udostępnia zadanie przygotowania zadania do wykonywania zadania wstępnej instalacji oraz zadanie zwolnienia zadania do konserwacji lub czyszczenia po zadaniu.

- **Zadanie przygotowania zadania** — zadanie przygotowania zadania jest uruchamiane na wszystkich węzłach obliczeniowych zaplanowanych do uruchamiania zadań, zanim zostaną wykonane inne zadania. Zadanie przygotowania zadania umożliwia kopiowanie danych współdzielonych przez wszystkie zadania, ale na przykład unikatowych dla zadania.
- **Zadanie zwolnienia zadania** — po zakończeniu zadania zadanie zwolnienia zadania jest uruchamiane w każdym węźle w puli, która wykonała przynajmniej jedno zadanie. Zadanie zwolnienia zadania umożliwia usuwanie danych skopiowanych przy użyciu zadania przygotowania zadania lub kompresję i przekazywanie na przykład danych dzienników diagnostycznych.

Zarówno zadania przygotowania zadania jak i zadania zwolnienia zadania pozwalają na określenie wiersza polecenia, który zostanie uruchomiony w przypadku wywołania zadania i udostępniając takie funkcje jak pobieranie plików, wykonywanie z podwyższonym poziomem uprawnień, niestandardowe zmienne środowiskowe, maksymalny czas trwania wykonywania, liczba ponownych prób i czas przechowywania plików.

Więcej informacji na temat zadań przygotowania i zwolnienia zadań znajduje się w temacie [Run job preparation and completion tasks on Azure Batch compute nodes](batch-job-prep-release.md) (Uruchamianie zadań przygotowania i ukończenia zadań w węzłach obliczeniowych w usłudze Azure Batch).

#### <a name="multiinstance"></a>Zadania obejmujące wiele wystąpień

[Zadanie obejmujące wiele wystąpień](batch-mpi.md) jest zadaniem, które jest konfigurowane do uruchamiania w więcej niż jednym węźle obliczeniowym równocześnie. Zadania obejmujące wiele wystąpień umożliwiają wdrożenie scenariuszy obliczeń o wysokiej wydajności, np. Message Passing Interface (MPI), w których do przetworzenia jednego obciążenia potrzebna jest grupa wspólnie przydzielonych węzłów obliczeniowych.

Szczegółowe omówienie dotyczące uruchamiania zadań MPI w usłudze Batch przy użyciu biblioteki Partii zadań dla platformy .NET znajdują się w temacie [Use multi-instance tasks to run Message Passing Interface (MPI) applications in Azure Batch](batch-mpi.md) (Używanie zadań obejmujących wiele wystąpień do uruchamiania aplikacji MPI [Message Passing Interface] w usłudze Azure Batch).

#### <a name="taskdep"></a>Zależności zadań

Zależności zadań, jak sama nazwa wskazuje, pozwalają na określenie, że wykonanie zadania zależy od ukończenia innych zadań. Ta funkcja zapewnia obsługę w sytuacjach, w których zadanie „podrzędne” pobiera dane wyjściowe zadania „nadrzędnego” lub gdy zadanie nadrzędne wykonuje inicjowanie wymagane przez zadanie podrzędne. Aby użyć tej funkcji, należy najpierw włączyć zależności zadania w zadaniu w usłudze Batch. Następnie dla każdego zadania, które jest zależne od innego (lub wielu innych) określ zadania, od których zadanie zależy.

Zależności zadań umożliwiają konfigurację scenariuszy takich jak poniżej:

* *zadanieB* zależy od *zadaniaA* (wykonywanie *zadaniaB* nie rozpocznie się, dopóki nie zostanie ukończone *zadanieA*)
* *zadanieC* zależy od *zadaniaA* i *zadaniaB*
* wykonanie *zadaniaD* zależy od zakresu zadań, np. zadań od *1* do *10*

Zapoznaj się z przykładem kodu [TaskDependencies][github_sample_taskdeps] w repozytorium GitHub [azure-batch-samples][github_samples]. Tutaj przedstawiono konfigurację zadań, które zależą od innych zadań przy użyciu biblioteki [Partii zadań dla platformy .NET][batch_net_api].

### <a name="jobschedule"></a>Zaplanowane zadania

Harmonogramy zadań umożliwiają tworzenie zadań cyklicznych w ramach usługi Batch. Harmonogram zadań określa, kiedy uruchamiać zadania i zawiera specyfikacje zadań do uruchomienia. Harmonogram zadań umożliwia określenie czasu trwania harmonogramu — jak długo i kiedy harmonogram obowiązuje — oraz jak często w tym okresie zadania powinny być tworzone.

### <a name="appkg"></a>Pakiety aplikacji

Funkcja [pakietów aplikacji](batch-application-packages.md) umożliwia łatwe zarządzanie aplikacjami oraz ich wdrażanie do węzłów obliczeniowych w pulach. Za pomocą pakietów aplikacji można łatwo przekazywać wiele wersji aplikacji wykonywanych przez zadania oraz nimi zarządzać, co obejmuje pliki binarne i pliki obsługi, a następnie automatycznie wdrożyć co najmniej jedną z tych aplikacji do węzłów obliczeniowych w puli.

Usługa Batch obsługuje szczegółowe informacje o pracy z usługą Magazyn Azure w tle, aby bezpiecznie przechowywać i wdrażać pakiety aplikacji do węzłów obliczeniowych, aby uprościć zarówno kod jak i pracę związana z zarządzaniem.

Więcej informacji na temat funkcji pakietu aplikacji znajduje się w temacie [Application deployment with Azure Batch application packages](batch-application-packages.md) (Wdrażanie aplikacji za pomocą pakietów aplikacji usługi Azure Batch).

## <a name="files"></a>Pliki i katalogi

Każde zadanie ma katalog roboczy, w którym tworzy zero lub więcej plików i katalogów do przechowywania programu, który jest uruchamiany przez zadanie, dane które ono przetwarza oraz dane wyjściowe przetwarzania wykonywanego przez zadanie. Te pliki i katalogi są potem dostępne do użycia przez inne zadania podczas wykonywania zadania. Wszystkie zadania, pliki i katalogi w węźle należą do jednego konta użytkownika.

Usługa Batch przedstawia część systemu plików w węźle w postaci „katalogu głównego”. Katalog główny jest dostępny dla zadania przez uzyskanie dostępu do zmiennej środowiskowej `%AZ_BATCH_NODE_ROOT_DIR%`. Więcej informacji na temat korzystania ze zmiennych środowiskowych znajduje się w temacie[Environment settings for tasks](#environment) (Ustawienia środowiska dla zadań).

![Struktura katalogu węzła obliczeniowego][1]

Katalog główny zawiera następującą strukturę katalogu:

- **Udostępnione** — ta lokalizacja jest udostępnionym katalogiem dla wszystkich zadań uruchamianych w węźle, niezależnie od zadania. W węźle dostęp do udostępnionego katalogu uzyskuje się za pośrednictwem `%AZ_BATCH_NODE_SHARED_DIR%`. Ten katalog zapewnia prawa do odczytu i zapisu dla wszystkich zadań wykonywanych w węźle. Zadania mogą tworzyć, odczytywać, aktualizować i usuwać pliki w tym katalogu.

- **Uruchamianie** — ta lokalizacja jest używana jako katalog roboczy przez zadanie uruchamiania. Wszystkie pliki, które są pobierane przez usługę Batch do uruchomienia zadania są także przechowywane w tym katalogu. W węźle katalog uruchamiania jest dostępny za pośrednictwem zmiennej środowiskowej `%AZ_BATCH_NODE_STARTUP_DIR%`. Zadanie uruchamiania może tworzyć, odczytywać, aktualizować i usuwać pliki w tym katalogu, a ten katalog może być używany przez zadania uruchamiania do konfiguracji systemu operacyjnego.

- **Zadania** — katalog, który jest tworzony dla każdego zadania, które działa w węźle, dostępny za pośrednictwem `%AZ_BATCH_TASK_DIR%`. W ramach każdego katalogu zadań usługa Batch tworzy katalog roboczy (`wd`), którego unikatowa ścieżka jest określana przez zmienną środowiskową `%AZ_BATCH_TASK_WORKING_DIR%`. Ten katalog zapewnia prawa do odczytu i zapisu zadania. Zadanie może tworzyć, odczytywać, aktualizować i usuwać pliki w tym katalogu, a ten katalog został zachowany na podstawie ograniczenia *RetentionTime* określonego dla zadania.
  - `stdout.txt` i `stderr.txt` — te pliki są zapisywane w folderze zadań podczas wykonywania zadania.

Gdy węzeł zostanie usunięty z puli, zostaną usunięte wszystkie pliki, które są przechowywane w węźle.

## <a name="lifetime"></a>Okres istnienia puli i węzła obliczeniowego

Podczas opracowywania rozwiązania usługi Azure Batch należy podjąć decyzję projektową dotyczącą tego, jak i kiedy będą tworzone pule i jak długo będą dostępne w tych pulach węzły obliczeniowe.

Z jednej strony można tworzyć pulę dla każdego zadania podczas przesyłania zadania, a jego węzły mogłyby być usuwane, gdy tylko zakończy się wykonywanie zadania. Spowoduje to maksymalne zwiększenie wykorzystania, gdyż węzły są przydzielane tylko, gdy jest to absolutnie konieczne i zostają wyłączone, gdy tylko przejdą w stan bezczynności. Chociaż oznacza to, że zadanie musi oczekiwać, aż węzły zostaną przydzielone, ważne jest, aby pamiętać, że zadania zostaną zaplanowane dla węzłów, gdy tylko poszczególne z nich zostaną udostępnione, przydzielone, a zadanie uruchamiania zostanie zakończone. Usługa Batch *nie* oczekuje, aż wszystkie węzły w puli zostaną udostępnione przed przypisaniem zadań, dzięki czemu zapewnia maksymalne wykorzystanie wszystkich dostępnych węzłów.

Z drugiej strony, jeśli natychmiastowe uruchomienie zadań ma najwyższy priorytet, pula może zostać utworzona przed czasem, a jej węzły mogą zostać udostępnione przed przesłaniem zadań. W tym scenariuszu zadania mogą być uruchamiane natychmiast, ale podczas oczekiwania na przypisanie zadań węzły mogą być w stanie bezczynności.

Złożone rozwiązanie, zwykle używane do obsługi zróżnicowanego, ale stałego obciążenia, polega na posiadaniu puli, do której przesyłanych jest wiele zadań, ale skalowaniu liczby węzłów w górę lub w dół zgodnie z obciążeniem zadaniami (zobacz temat *Scaling applications* [Skalowanie aplikacji] poniżej). Można to zrobić w sposób reaktywny, w oparciu o bieżące obciążenie, lub aktywny, jeśli obciążenie można przewidzieć.

## <a name="scaling"></a>Skalowanie aplikacji

[Skalowanie automatyczne](batch-automatic-scaling.md) pozwala na to, by usługa Batch dynamicznie dostosowywała liczbę węzłów obliczeniowych w puli zgodnie z bieżącym obciążeniem i użyciem zasobów w ramach scenariusza obliczeniowego. Dzięki temu można zmniejszyć całkowity koszt działania aplikacji przy użyciu tylko potrzebnych zasobów, zwalniając te zbędne. Można określić ustawienia automatycznego skalowania dla puli podczas jej tworzenia lub włączyć skalowanie później oraz zaktualizować ustawienia skalowania w puli automatycznej z włączoną funkcją skalowania.

Automatyczne skalowanie odbywa się przez określenie **formuły automatycznego skalowania** dla puli. Usługa Batch używa tej formuły do określenia docelowej liczby węzłów w puli dla kolejnego interwału skalowania (interwału, który może określić użytkownik).

Na przykład być może zadanie wymaga przesyłania dużej liczby zadań, które zostaną zaplanowane do wykonania. Do puli można przypisać formułę skalowania, która dostosowuje liczbę węzłów w puli w oparciu o bieżącą liczbę zadań oczekujących oraz szybkość ich wykonania. Usługa Batch okresowo ocenia formułę i zmienia rozmiar puli w oparciu o obciążenie i ustawienia formuły.

Formuła skalowania może opierać się na następujących metrykach:

- **Metryki czasu** — na podstawie danych statystycznych zbieranych co pięć minut przez określoną liczbę godzin.

- **Metryki zasobów** — na podstawie użycia procesora CPU, wykorzystania przepustowości, zużycia pamięci i liczby węzłów.

- **Metryki zadań** — na podstawie stanu zadań, takich jak Aktywny, Oczekujący i Zakończony.

Gdy automatyczne skalowanie zmniejsza liczbę węzłów obliczeniowych w puli, należy uwzględnić aktualnie uruchomione zadania. Aby to umożliwić, formuła może zawierać ustawienie zasad dezalokacji węzła, które określa, czy uruchomione zadania są zatrzymywane natychmiast, czy mogą zostać ukończone przed usunięciem węzła z puli.

> [AZURE.TIP] Aby maksymalnie zwiększyć wykorzystanie zasobów obliczeniowych, ustaw docelową liczbę węzłów na zero na końcu zadania, ale zezwól, aby uruchomione zadania zostały ukończone.

Więcej informacji na temat automatycznego skalowania aplikacji znajduje się w temacie [Automatically scale compute nodes in an Azure Batch pool](batch-automatic-scaling.md) (Automatyczne skalowanie węzłów obliczeniowych w puli usługi Azure Batch).

## <a name="cert"></a>Zabezpieczenia oparte na certyfikatach

Zazwyczaj certyfikatów należy użyć podczas szyfrowania i odszyfrowywania poufnych informacji do zadań, np. klucza do [konta usługi Magazyn Azure][azure_storage]. Aby to umożliwić, certyfikaty mogą być instalowane w węzłach. Zaszyfrowane klucze tajne są przekazywane do zadań za pomocą parametrów wiersza polecenia lub osadzane w jednym z zasobów zadań, a zainstalowanych certyfikatów można użyć do ich odszyfrowania.

Aby dodać certyfikat do konta usługi Batch, należy użyć operacji [Dodaj certyfikat][rest_add_cert] (interfejs API REST w usłudze Batch) lub metody [CertificateOperations.CreateCertificate][net_create_cert] (interfejs API .NET w usłudze Batch). Następnie można skojarzyć certyfikat z nową lub istniejącą pulą. Gdy certyfikat zostaje skojarzony z pulą, usługa Batch instaluje certyfikat w każdym węźle w puli. Usługa Batch instaluje odpowiednie certyfikaty podczas uruchamiania węzła, przed uruchomieniem dowolnych zadań, np. zadań uruchamiania i Menedżera zadań.

## <a name="scheduling"></a>Priorytet planowania

Zadaniom tworzonym w usłudze Batch można nadać priorytet. Usługa Batch używa wartości priorytetu zadania, aby określić kolejność planowania zadań w ramach konta. Wartości priorytetu mieszczą się w zakresie od -1000 do 1000, gdzie -1000 oznacza najniższy priorytet, a 1000 najwyższy. Priorytet zadania można zaktualizować przy użyciu operacji [Aktualizuj właściwości zadania][rest_update_job] (interfejs API REST usługi Batch) lub zmieniając właściwość [CloudJob.Priority][net_cloudjob_priority] (interfejs API .NET usługi Batch).

W ramach tego samego konta zadania o wyższym priorytecie mają pierwszeństwo planowania nad zadaniami o niższym priorytecie. Zadanie o wyższym priorytecie na jednym koncie nie ma pierwszeństwa planowania nad innym zadaniem o niższym priorytecie na innym koncie.

Planowanie zadań między pulami odbywa się niezależnie. Między różnymi pulami nie ma żadnej gwarancji, że zadanie o wyższym priorytecie zostanie zaplanowane jako pierwsze, jeśli w skojarzonej z nim puli brakuje bezczynnych węzłów. W tej samej puli zadania o tym samym priorytecie mają równe szanse na zaplanowanie.

## <a name="environment"></a>Ustawienia środowiska dla zadań

Każde zadanie, które jest wykonywane w ramach zadania usługi Batch ma dostęp do zmiennych środowiskowych ustawionych zarówno przez usługę Batch (zdefiniowaną przez system, zobacz w poniższej tabeli) jak i zgodnie ze zmiennymi środowiskowymi zdefiniowanymi przez użytkownika. Aplikacje i skrypty uruchamiane przez zadania w węzłach obliczeniowych mają dostęp do tych zmiennych środowiskowych podczas wykonywania w węźle.

Ustawianie zmienne środowiskowe zdefiniowane przez użytkownika, używając operacji [Dodaj zadanie do zadania][rest_add_task] (interfejs API REST usługi Batch) lub zmieniając właściwość [CloudTask.EnvironmentSettings][net_cloudtask_env] (interfejs API .NET usługi Batch) podczas dodawania zadania do zadania.

Uzyskaj zmienne środowiskowe zadania, zarówno systemowe jak i zdefiniowane przez użytkownika za pomocą operacji [Uzyskaj informacje o zadaniu][rest_get_task_info] (interfejs API REST usługi Batch) lub uzyskując dostęp do właściwości [CloudTask.EnvironmentSettings][net_cloudtask_env] (interfejs API .NET usługi Batch). Jak wspomniano wcześniej, procesy wykonywane w węźle obliczeniowym mają również dostęp do wszystkich zmiennych środowiskowych, np. przy użyciu znanej składni `%VARIABLE_NAME%`.

Dla każdego zadania, które jest zaplanowane w ramach danego zadania w usłudze Batch zostaje ustawiony następujący zestaw zmiennych środowiskowych zdefiniowanych w systemie:

| Nazwa zmiennej środowiskowej       | Opis                                                              |
|---------------------------------|--------------------------------------------------------------------------|
| `AZ_BATCH_ACCOUNT_NAME`         | Nazwa konta, do którego należy zadanie.                       |
| `AZ_BATCH_JOB_ID`               | Identyfikator zadania, do którego należy zadanie.                             |
| `AZ_BATCH_JOB_PREP_DIR`         | Pełna ścieżka katalogu zadania przygotowania zadania w węźle.         |
| `AZ_BATCH_JOB_PREP_WORKING_DIR` | Pełna ścieżka katalogu roboczego zadania przygotowania zadania w węźle. |
| `AZ_BATCH_NODE_ID`              | Identyfikator węzła, w którym jest uruchomione zadanie.                         |
| `AZ_BATCH_NODE_ROOT_DIR`        | Pełna ścieżka katalogu głównego w węźle.                         |
| `AZ_BATCH_NODE_SHARED_DIR`      | Pełna ścieżka udostępnionego katalogu w węźle.                       |
| `AZ_BATCH_NODE_STARTUP_DIR`     | Pełna ścieżka katalogu zadania konfiguracji węzła obliczeniowego w węźle.    |
| `AZ_BATCH_POOL_ID`              | Identyfikator puli, w której jest uruchomione zadanie.                         |
| `AZ_BATCH_TASK_DIR`             | Pełna ścieżka katalogu zadań w węźle.                         |
| `AZ_BATCH_TASK_ID`              | Identyfikator bieżącego zadania.                                              |
| `AZ_BATCH_TASK_WORKING_DIR`     | Pełna ścieżka katalogu roboczego zadań w węźle.                 |

>[AZURE.NOTE] Nie można zastąpić żadnych z wymienionych powyżej zmiennych zdefiniowanych w systemie — są one tylko do odczytu.

## <a name="errorhandling"></a>Obsługa błędów

Może okazać się konieczne, by obsługiwać błędy zadań i aplikacji w ramach rozwiązania usługi Batch.

### Obsługa błędów zadań
Błędy zadań można podzielić na następujące kategorie:

- **Błędy planowania**
    - Jeśli transfer plików określony dla zadania nie powiedzie się z jakiegokolwiek powodu, dla zadanie zostanie ustawiony „błąd planowania”.
    - Błędy planowania mogą być spowodowane tym, że pliki zostały przeniesione, konto usługi Magazyn Azure nie jest już dostępne lub wystąpił inny problem uniemożliwiający pomyślne kopiowanie plików do węzła.
- **Błędy aplikacji**
    - Proces określony przez wiersz polecenia zadania może również zakończyć się niepowodzeniem. Proces zostaje uznany za nieudany, gdy proces wykonywany w ramach zadania zwróci kod zakończenia różny od zera.
    - W przypadku błędów aplikacji istnieje możliwość skonfigurowania usługi Batch do automatycznego ponawiania próby wykonania zadania określoną liczbę razy.
- **Błędy ograniczenia**
    - Można ustawić ograniczenie określające maksymalny czas trwania zadania, *maxWallClockTime*. Może to być przydatne do zakończenia zadań „zawieszonych”.
    - Po przekroczeniu maksymalnego czasu zadanie zostaje oznaczone jako *ukończone*, ale kod zakończenia ma wartość `0xC000013A`, a pole *schedulingError* zostanie oznaczone jako `{ category:"ServerError", code="TaskEnded"}`.

### Błędy debugowania aplikacji

W czasie wykonywania aplikacja może tworzyć diagnostyczne dane wyjściowe, których można użyć do rozwiązywania problemów. Jak wspomniano wcześniej, w części [Pliki i katalogi](#files) powyżej, usługa Batch wysyła dane wyjściowe stdout i stderr do plików `stdout.txt` i `stderr.txt` znajdujących się w katalogu zadań w węźle obliczeniowym. Za pomocą [ComputeNode.GetNodeFile][net_getfile_node] i [CloudTask.GetNodeFile][net_getfile_task] w interfejsie API .NET usługi Batch można pobrać te i inne pliki na potrzeby rozwiązywania problemów.

Można wykonać jeszcze bardziej rozległe debugowanie przez zalogowanie do węzła obliczeniowego za pomocą *pulpitu zdalnego*. Można [uzyskać plik protokołu pulpitu zdalnego z węzła][rest_rdp] (interfejs API REST usługi Batch) lub użyć metody [ComputeNode.GetRDPFile][net_rdp] (interfejs API .NET usługi Batch) do logowania zdalnego.

>[AZURE.NOTE] Aby połączyć się z węzłem za pośrednictwem protokołu RDP, musisz najpierw utworzyć użytkownika w węźle. [Dodaj konto użytkownika do węzła][rest_create_user] w interfejsie API REST usługi Batch lub użyj metody [ComputeNode.CreateComputeNodeUser][net_create_user] na platformie .NET usługi Batch.

### Uwzględnianie błędów zadań lub przerw w zadaniach 

Od czasu do czasu zadania mogą ulec awarii lub zostać przerwane. Awarii może ulec sama aplikacja zadania, może zostać przeprowadzony ponowny rozruch węzła, w którym zostało uruchomione zadanie lub węzeł może zostać usunięty z puli podczas operacji zmiany rozmiaru, jeśli zasady dezalokacji puli zostały ustawione na natychmiastowe usuwanie węzłów bez oczekiwania na zakończenie zadań. We wszystkich przypadkach zadanie może zostać automatycznie ponownie umieszczone w kolejce przy użyciu usługi Batch do wykonania w innym węźle.

Istnieje również możliwość wystąpienia sporadycznie problemu polegającego na tym, że zadanie zawiesza się lub jego wykonanie zajmuje za dużo czasu. Można ustawić maksymalny czas wykonywania dla zadania i jeśli zostanie on przekroczony, usługa Batch przerwie aplikację zadania.

### Rozwiązywanie problemów z „wadliwymi” węzłami obliczeniowymi

W sytuacjach, w których niektóre z zadań kończą się niepowodzeniem, aplikacja kliencka lub usługa Batch mogą badać metadane nieudanych zadań w celu identyfikacji nieprawidłowo funkcjonującego węzła. Każdemu węzłowi w puli zostaje nadany unikatowy identyfikator i węzeł, w którym jest uruchomione zadanie jest dołączony do metadanych zadania. Po zidentyfikowaniu można wykonać kilka czynności:

- **Ponownie uruchom węzeł** ([REST][rest_reboot] | [.NET][net_reboot])

    Ponowne uruchomienie węzła może czasami usunąć ukryte problemy, takie jak zablokowane procesy lub procesy, które uległy awarii. Należy pamiętać, że jeśli pula używa zadania uruchamiania lub zadanie używa zadania przygotowania zadania, zostaną one wykonane po ponownym uruchomieniu węzła.

- **Odtwórz z obrazu węzeł** ([REST][rest_reimage] | [.NET][net_reimage])

    Spowoduje to ponowne zainstalowanie systemu operacyjnego w węźle. Podobnie jak w przypadku ponownego rozruchu węzła zadania uruchamiania i zadania przygotowania zadania są uruchamiane ponownie po odtworzeniu węzła z obrazu.

- **Usuń węzeł z puli** ([REST][rest_remove] | [.NET][net_remove])

    Czasami konieczne jest całkowite usunięcie węzła z puli..

- **Wyłącz planowanie zadań w węźle** ([REST][rest_offline] | [.NET][net_offline])

    Ta czynność skutecznie przenosi węzeł do trybu „offline”, aby nie zostały do niego przypisane żadne dalsze zadania, ale pozwala by węzeł dalej działał i pozostał w puli. Dzięki temu można dalej sprawdzać przyczyny błędów bez utraty danych nieudanego zadania, gdy węzeł nie powoduje dodatkowych awarii zadania. Można na przykład wyłączyć planowanie zadań w węźle, a następnie zalogować się zdalnie, aby sprawdzić dzienniki zdarzeń węzła lub wykonać inne operacje związane z rozwiązywaniem problemów. Po zakończeniu sprawdzania można przenieść węzeł z powrotem do trybu online przez włączenie planowania zadań ([REST][rest_online], [.NET][net_online]) lub wykonać jedną z innych czynności omówionej powyżej.

> [AZURE.IMPORTANT] Wszystkie akcje powyżej — ponowny rozruch, odtwarzanie z obrazu, usuwanie, wyłączanie planowania zadań — umożliwiają określenie, jak są obsługiwane zadania uruchomione obecnie w węźle, gdy jest wykonywana akcja. Na przykład po wyłączeniu planowania zadań w węźle za pomocą biblioteki klienta Partii zadań dla platformy .NET można określić wartość wyliczenia [DisableComputeNodeSchedulingOption][net_offline_option], aby wskazać, czy należy **przerwać** uruchomione zadania, **umieścić je ponownie w kolejce** do planowania w innych węzłach lub zezwolić na ukończenie uruchomionych zadań przed wykonaniem akcji (**TaskCompletion**).

## Następne kroki

- Utwórz pierwszą aplikację usługi Batch, wykonując czynności opisane w temacie [Wprowadzenie do biblioteki Partii zadań Azure dla platformy .NET](batch-dotnet-get-started.md).
- Pobierz i skompiluj przykładowy projekt programu [Batch Explorer][batch_explorer_project] do użycia podczas tworzenia rozwiązań usługi Batch. Za pomocą programu Batch Explorer można wykonywać m.in. następujące czynności:
  - Monitorowanie pul i zadań w ramach konta usługi Batch i manipulowanie nimi
  - Pobieranie `stdout.txt`, `stderr.txt` i innych plików z węzłów
  - Tworzenie użytkowników w węzłach i pobieranie plików RDP do logowania zdalnego

[1]: ./media/batch-api-basics/node-folder-structure.png

[about_cloud_services]: ../cloud-services/cloud-services-choose-me.md
[azure_storage]: https://azure.microsoft.com/services/storage/
[batch_explorer_project]: https://github.com/Azure/azure-batch-samples/tree/master/CSharp/BatchExplorer
[cloud_service_sizes]: ../cloud-services/cloud-services-sizes-specs.md
[msmpi]: https://msdn.microsoft.com/library/bb524831.aspx
[github_samples]: https://github.com/Azure/azure-batch-samples
[github_sample_taskdeps]:  https://github.com/Azure/azure-batch-samples/tree/master/CSharp/ArticleProjects/TaskDependencies

[batch_net_api]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[net_cloudjob_jobmanagertask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.jobmanagertask.aspx
[net_cloudjob_priority]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudjob.priority.aspx
[net_cloudpool_starttask]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudpool.starttask.aspx
[net_cloudtask_env]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.environmentsettings.aspx
[net_create_cert]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.certificateoperations.createcertificate.aspx
[net_create_user]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.createcomputenodeuser.aspx
[net_getfile_node]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getnodefile.aspx
[net_getfile_task]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.cloudtask.getnodefile.aspx
[net_multiinstancesettings]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.multiinstancesettings.aspx
[net_rdp]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.getrdpfile.aspx
[net_reboot]: https://msdn.microsoft.com/library/azure/mt631495.aspx
[net_reimage]: https://msdn.microsoft.com/library/azure/mt631496.aspx
[net_remove]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.pooloperations.removefrompoolasync.aspx
[net_offline]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.disableschedulingasync.aspx
[net_online]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.computenode.enableschedulingasync.aspx
[net_offline_option]: https://msdn.microsoft.com/library/azure/microsoft.azure.batch.common.disablecomputenodeschedulingoption.aspx

[batch_rest_api]: https://msdn.microsoft.com/library/azure/Dn820158.aspx
[rest_add_job]: https://msdn.microsoft.com/library/azure/mt282178.aspx
[rest_add_pool]: https://msdn.microsoft.com/library/azure/dn820174.aspx
[rest_add_cert]: https://msdn.microsoft.com/library/azure/dn820169.aspx
[rest_add_task]: https://msdn.microsoft.com/library/azure/dn820105.aspx
[rest_create_user]: https://msdn.microsoft.com/library/azure/dn820137.aspx
[rest_get_task_info]: https://msdn.microsoft.com/library/azure/dn820133.aspx
[rest_multiinstance]: https://msdn.microsoft.com/library/azure/mt637905.aspx
[rest_multiinstancesettings]: https://msdn.microsoft.com/library/azure/dn820105.aspx#multiInstanceSettings
[rest_update_job]: https://msdn.microsoft.com/library/azure/dn820162.aspx
[rest_rdp]: https://msdn.microsoft.com/library/azure/dn820120.aspx
[rest_reboot]: https://msdn.microsoft.com/library/azure/dn820171.aspx
[rest_reimage]: https://msdn.microsoft.com/library/azure/dn820157.aspx
[rest_remove]: https://msdn.microsoft.com/library/azure/dn820194.aspx
[rest_offline]: https://msdn.microsoft.com/library/azure/mt637904.aspx
[rest_online]: https://msdn.microsoft.com/library/azure/mt637907.aspx



<!--HONumber=jun16_HO2-->


