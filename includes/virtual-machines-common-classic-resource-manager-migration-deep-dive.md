## <a name="migrate-iaas-resources-from-the-classic-deployment-model-to-azure-resource-manager"></a>Migracja zasobów IaaS z klasycznym modelu wdrożenia do usługi Azure Resource Manager
Najpierw należy zrozumieć różnicę między operacjami płaszczyzna danych i zarządzania płaszczyzny w infrastrukturze jako zasoby usługi (IaaS).

* *Zarządzanie i kontrola płaszczyzny* opisuje wywołania, które wchodzą w płaszczyźnie zarządzania i kontrola lub interfejsu API modyfikowania zasobów. Operacje, na przykład takie jak utworzenie maszyny wirtualnej, ponowne uruchomienie maszyny wirtualnej i zaktualizowanie maszyny wirtualnej przy użyciu nowej podsieci, powodują wykonanie działań zarządzania uruchomionymi zasobami. Nie wpływają one bezpośrednio na nawiązywanie połączeń z maszynami wirtualnymi.
* *Płaszczyzna danych* (aplikacja) opisano środowiska wykonawczego o aplikacji i wymaga interakcji z wystąpieniami, które nie korzystają z interfejsu API platformy Azure. Na przykład dostęp do witryny sieci Web lub ściąganie danych z działającego wystąpienia programu SQL Server lub serwer bazy danych MongoDB są dane interakcje płaszczyzny lub aplikacji. Przykładami innych kopiowania obiektu blob z konta magazynu, a dostęp do publicznego adresu IP do używania protokołu RDP (Remote Desktop) lub protokołu Secure Shell (SSH) do maszyny wirtualnej. Te operacje utrzymują działanie aplikacji w ramach usług obliczeniowych, sieciowych i magazynowych.

Płaszczyzna danych jest taka sama, między klasycznym modelu wdrażania i stosy Menedżera zasobów. Różnica polega na tym, czy podczas procesu migracji Microsoft tłumaczy reprezentację zasoby z klasycznym modelu wdrożenia w tym w stosie usługi Resource Manager. W związku z tym należy użyć nowych narzędzi, interfejsów API i zestawy SDK do zarządzania zasobami w stosie usługi Resource Manager.

![Diagram przedstawiający różnica między płaszczyzny zarządzania i kontrola i płaszczyzna danych](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)


> [!NOTE]
> W przypadku niektórych scenariuszy migracji platforma Azure zatrzymuje maszyny wirtualne, cofa ich przydział i uruchamia je ponownie. Powoduje to krótki przestój płaszczyzna danych.
>

## <a name="the-migration-experience"></a>Środowisko migracji
Przed rozpoczęciem migracji:

* Upewnij się, że zasoby do zmigrowania nie używają żadnych nieobsługiwanych funkcji ani konfiguracji. Zazwyczaj platforma wykrywa te problemy i zgłasza błąd.
* Jeśli masz maszyny wirtualne, które nie znajdują się w sieci wirtualnej, są zatrzymane i alokację w ramach operacji prepare. Jeśli nie chcesz stracić publicznego adresu IP, należy wziąć pod uwagę rezerwowania adresów IP powoduje wywołanie operacji prepare. W przypadku maszyn wirtualnych w sieci wirtualnej, nie są zatrzymane i alokację.
* Zaplanuj przeprowadzenie migracji poza godzinami pracy, aby uwzględnić możliwość wystąpienia nieoczekiwanych awarii podczas migracji.
* Pobierz bieżącą konfigurację maszyn wirtualnych przy użyciu programu PowerShell, poleceń interfejsu wiersza polecenia (CLI) lub interfejsów API REST, aby ułatwić walidację po zakończeniu kroku przygotowania.
* Zaktualizuj automatyzacji i operationalization skryptów obsługi modelu wdrażania usługi Resource Manager, przed rozpoczęciem migracji. Opcjonalnie możesz wykonać operacje GET, gdy zasoby są w stanie przygotowania.
* Ocena zasad kontroli dostępu opartej na rolach (RBAC), które są konfigurowane dla zasobów IaaS w klasycznym modelu wdrażania i planowanie po zakończeniu migracji.

Przebieg migracji jest następujący:

![Diagram przedstawiający przepływ pracy migracji](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Czynności opisane w poniższych sekcjach są wszystkie idempotentności. Jeśli masz problem niż nieobsługiwanej funkcji lub błąd konfiguracji, ponów o przygotowaniu przerwania lub przekazania operacji. Azure próbuje wykonać akcję ponownie.
>
>

### <a name="validate"></a>Walidacja
Operacja walidacji to pierwszy krok w procesie migracji. Celem tego kroku jest analizuje stan zasoby, które chcesz przeprowadzić migrację w klasycznym modelu wdrażania. Operacja ocenia, czy zasoby są w stanie migracji (powodzenie lub niepowodzenie).

Wybierz sieć wirtualna lub usługa w chmurze (Jeśli nie znajduje się w sieci wirtualnej), który chcesz zweryfikować do migracji. Jeśli zasób nie jest zdolny do migracji, Azure Dlaczego wymieniono przyczyny.

#### <a name="checks-not-done-in-the-validate-operation"></a>Testy nie wykonane w ramach operacji sprawdzania poprawności

Operacja sprawdzania poprawności tylko analizuje stan zasobów w klasycznym modelu wdrażania. Może ono sprawdzić wszystkie błędy i nieobsługiwane scenariusze z powodu różnych konfiguracjach w klasycznym modelu wdrażania. Nie jest możliwe sprawdzenie dotyczące wszystkich problemów z stosu usługi Azure Resource Manager może nakładać się na zasoby podczas migracji. Te problemy tylko są zaznaczone, jeśli zasoby poddaje transformacji w następnym kroku migracji (operacji prepare). W poniższej tabeli wymieniono wszystkie problemy, które nie są sprawdzane w operacji sprawdzania poprawności:


|Sprawdzanie sieci nie w operacji sprawdzania poprawności|
|-|
|Posiadanie zarówno ER, jak i sieci VPN bramy sieci wirtualnej.|
|Połączenie bramy sieci wirtualnej w stanie rozłączenia.|
|Wszystkie obwody ER wstępnie są migrowane do stosu usługi Azure Resource Manager.|
|Przydziału Menedżera zasobów Azure sprawdza, czy zasoby sieciowe. Na przykład: statyczny publiczny adres IP, dynamiczne publiczne adresy IP załadować równoważenia, grup zabezpieczeń sieci, tabele tras i interfejsów sieciowych. |
| Wszystkie reguły modułu równoważenia obciążenia są prawidłowe w ramach wdrożenia i sieci wirtualnej. |
| Konflikt prywatnych adresów IP między alokację zatrzymania maszyny wirtualne w tej samej sieci wirtualnej. |

### <a name="prepare"></a>Przygotowanie
Operacja przygotowania to drugi krok w procesie migracji. Celem tego kroku jest symulowanie transformacji zasobów IaaS z klasycznym modelu wdrażania do zasoby usługi Resource Manager. Co więcej Operacja przygotowywania przedstawia tego side-by-side można zwizualizować.

> [!NOTE] 
> W klasycznym modelu wdrażania zasobów nie są modyfikowane w tym kroku. Jest bezpieczne krok do uruchomienia, jeśli próbujesz się migracji. 

Możesz wybrać sieci wirtualnej lub usługi w chmurze (Jeśli nie jest sieć wirtualną) chcesz przygotować do migracji.

* Zasób nie jest zdolny do migracji, Azure zatrzymuje ten proces migracji list lub przyczyny niepowodzenia operacji prepare.
* Jeśli zasób jest w stanie migracji, Azure blokuje płaszczyzny zarządzania operacjami dotyczącymi zasobów w ramach migracji. Na przykład nie będzie można dodać dysku danych do migrowanej maszyny wirtualnej.

Azure następnie rozpoczyna migrację metadanych z klasycznym modelu wdrażania do Menedżera zasobów dla migracji zasobów.

Po zakończeniu operacji prepare użytkownik może wizualizacja zasobów w klasycznym modelu wdrażania i Menedżera zasobów. Dla każdej usługi w chmurze w klasycznym modelu wdrażania platforma Azure tworzy nazwę grupy zasobów zgodnie ze wzorcem `cloud-service-name>-Migrated`.

> [!NOTE]
> Nie jest możliwe, wybierz nazwę grupy zasobów utworzone dla zmigrowanych zasobów (to znaczy "-migracji"). Po zakończeniu migracji, jednak funkcja przenoszenia usługi Azure Resource Manager można przenieść zasobów do żadnej grupy zasobów, które mają. Aby uzyskać więcej informacji, zobacz [Move resources to new resource group or subscription](../articles/resource-group-move-resources.md) (Przenoszenie zasobów do nowej grupy lub subskrypcji).

Dwa poniższe zrzuty ekranu pokazują wynik po pomyślnej przygotować operację. Pierwsza z nich zawiera grupę zasobów, która zawiera oryginalnego usługi w chmurze. Drugi pokazuje nowe "-migracji" grupy zasobów, która zawiera równoważne zasoby usługi Azure Resource Manager.

![Zrzut ekranu pokazujący oryginalnego usługi w chmurze](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Zrzut ekranu pokazujący zasobów usługi Azure Resource Manager w ramach operacji prepare](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

Tutaj przedstawiono informacje o wewnętrznych zasobów, po zakończeniu fazie przygotowywania. Należy pamiętać, że zasób w płaszczyźnie danych takie same. Jest przedstawiany zarówno płaszczyzny zarządzania (klasycznego modelu wdrażania) i płaszczyzny sterowania (Resource Manager).

![Diagram fazie przygotowywania](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-prepare.png)

> [!NOTE]
> Maszyny wirtualne, które nie znajdują się w sieci wirtualnej w klasycznym modelu wdrażania są zatrzymane i w tej fazie migracji.
>

### <a name="check-manual-or-scripted"></a>Sprawdzenie (ręczne lub za pomocą skryptu)
W kroku wyboru masz opcję, aby zweryfikować poprawne migracji przy użyciu konfiguracji, który został wcześniej pobrany. Alternatywnie można logowania się do portalu i miejscu właściwości i zasoby, aby sprawdzić, czy migracja metadanych jest dobra.

Jeśli migrujesz sieć wirtualną, większość konfiguracji maszyn wirtualnych nie zostanie uruchomiona ponownie. W przypadku aplikacji na tych maszynach wirtualnych można sprawdzić, czy aplikacja nadal działa.

Można przetestować skrypty monitorowania i działa zgodnie z oczekiwaniami pracy maszyn wirtualnych i czy zaktualizowanych skryptów działać poprawnie. Tylko operacje GET są obsługiwane, jeśli zasoby są w stanie przygotowania.

Brak nie Ustaw okno czasu, przed którym należy zatwierdzić migracji. Ten stan może trwać tak długo, jak chcesz. Jednak płaszczyzna zarządzania jest zablokowana dla tych zasobów do momentu przerwania lub zatwierdzenia.

Jeśli napotkasz jakiekolwiek problemy, zawsze możesz przerwać migrację i wrócić do klasycznego modelu wdrażania. Po wróć, Azure otwiera płaszczyzny zarządzania operacje na zasobach, dzięki czemu można wznowić wykonywania normalnych operacji na tych maszynach wirtualnych w klasycznym modelu wdrażania.

### <a name="abort"></a>Przerwanie
Jest opcjonalny krok, jeśli chcesz cofnąć zmiany w klasycznym modelu wdrażania i zatrzymać migracji. Ta operacja powoduje usunięcie metadanych Menedżera zasobów (utworzonego w kroku) dla zasobów. 

![Diagram kroku przerwania](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-abort.png)


> [!NOTE]
> Nie można wykonać tej operacji po zostały wyzwolone operacji przekazywania.     
>

### <a name="commit"></a>Zatwierdzenie
Po zakończeniu walidacji możesz zatwierdzić migrację. Zasoby nie są już wyświetlane w klasycznym modelu wdrażania i są dostępne tylko w modelu wdrażania usługi Resource Manager. Zmigrowanymi zasobami można zarządzać tylko w nowym portalu.

> [!NOTE]
> Jest to operacja idempotentna. Jeśli nie powiedzie się, spróbuj ponownie wykonać operację. Jeśli nadal nie powieść, Utwórz bilet pomocy technicznej lub utworzyć wpis na forum z "ClassicIaaSMigration" tag naszych [forum wirtualna](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>

![Diagram kroku zatwierdzania](../articles/virtual-machines/windows/media/migration-classic-resource-manager/behind-the-scenes-commit.png)

## <a name="migration-flowchart"></a>Schemat blokowy migracji

Oto schemat blokowy pokazujący sposób kontynuować migracji:

![Zrzut ekranu przedstawiający kroki migracji](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-the-classic-deployment-model-to-resource-manager-resources"></a>Tłumaczenie klasycznym modelu wdrażania Menedżera zasobów zasobów
Klasycznym modelu wdrażania i Menedżer zasobów reprezentacji zasobów można znaleźć w poniższej tabeli. Inne funkcje i zasoby nie są obecnie obsługiwane.

| Reprezentacja klasyczna | Reprezentacja usługi Resource Manager | Uwagi |
| --- | --- | --- |
| Nazwa usługi w chmurze |Nazwa DNS |Podczas migracji jest tworzona nowa grupa zasobów dla każdej usługi w chmurze przy użyciu wzorca nazewnictwa `<cloudservicename>-migrated`. Ta grupa zasobów zawiera wszystkie zasoby. Nazwa usługi w chmurze stanie się nazwą DNS skojarzoną z publicznym adresem IP. |
| Maszyna wirtualna |Maszyna wirtualna |Właściwości specyficzne dla maszyny wirtualnej są migrowane bez zmian. Niektóre informacje osProfile, takie jak nazwa komputera nie są przechowywane w klasycznym modelu wdrażania i pozostanie pusta, po migracji. |
| Zasoby dysków dołączonych do maszyny wirtualnej |Niejawne dyski dołączone do maszyny wirtualnej |Dyski nie są modelowane jako zasoby najwyższego poziomu w modelu wdrażania usługi Resource Manager. Są one migrowane jako dyski niejawne w ramach maszyny wirtualnej. Tylko dyski dołączone do maszyny wirtualnej są obecnie obsługiwane. Menedżer zasobów maszyn wirtualnych można teraz używać kont magazynu w klasycznym modelu wdrażania, dzięki czemu dysków, aby łatwo można migrować bez żadnych aktualizacji. |
| Rozszerzenia maszyn wirtualnych |Rozszerzenia maszyn wirtualnych |Wszystkie rozszerzenia zasobów, z wyjątkiem rozszerzeń XML, są migrowane z klasycznego modelu wdrażania. |
| Certyfikaty maszyny wirtualnej |Certyfikaty w usłudze Azure Key Vault |Usługi w chmurze zawiera certyfikaty usługi, migracji tworzy nowy magazynu kluczy Azure dla danej usługi chmury, a certyfikaty są przenoszone do magazynu kluczy. Maszyny wirtualne są aktualizowane, tak aby przywoływały certyfikaty z magazynu kluczy. <br><br> Nie należy usuwać magazynu kluczy. Może to spowodować maszyny Wirtualnej, aby przejść w stan niepowodzenia. |
| Konfiguracja usługi WinRM |Konfiguracja usługi WinRM w ramach elementu osProfile |Konfiguracja usługi Windows Remote Management jest przenoszona bez zmian w ramach migracji. |
| Właściwość Availability-set |Zasób Availability-set | Specyfikacja zestawu dostępności jest właściwością na maszynę Wirtualną w klasycznym modelu wdrażania. Zestawy dostępności stają się zasobami najwyższego poziomu w ramach migracji. Następujące konfiguracje nie są obsługiwane: wiele zestawów dostępności dla usługi w chmurze, co najmniej jeden zestaw dostępności razem z maszyną wirtualną poza zestawem dostępności w ramach usługi w chmurze. |
| Konfiguracja sieci na maszynie wirtualnej |Podstawowy interfejs sieciowy |Konfiguracja sieci na maszynie wirtualnej jest reprezentowana jako zasób podstawowego interfejsu sieciowego po migracji. W przypadku maszyn wirtualnych poza siecią wirtualną wewnętrzny adres IP zmieni się podczas migracji. |
| Wiele interfejsów sieciowych na maszynie wirtualnej |Interfejsy sieciowe |Jeśli maszyna wirtualna ma wiele interfejsów sieciowych skojarzonych z nim, każdy interfejs sieciowy staje się zasobem najwyższego poziomu w ramach migracji, wraz ze wszystkich właściwości. |
| Zestaw punktów końcowych z równoważeniem obciążenia |Moduł równoważenia obciążenia |W klasycznym modelu wdrażania do platformy jest przypisany niejawny moduł równoważenia obciążenia dla każdej usługi w chmurze. Podczas migracji jest tworzony nowy zasób modułu równoważenia obciążenia, a zestaw punktów końcowych równoważenia obciążenia staje się regułami modułu równoważenia obciążenia. |
| Reguły NAT dla ruchu przychodzącego |Reguły NAT dla ruchu przychodzącego |Wejściowe punkty końcowe zdefiniowane na maszynie wirtualnej są konwertowane na reguły translacji dla adresu sieciowego ruchu przychodzącego w module równoważenia obciążenia podczas migracji. |
| Adres VIP |Publiczny adres IP z nazwą DNS |Wirtualny adres IP staje się publiczny adres IP i skojarzony z usługą równoważenia obciążenia. Wirtualny adres IP można zmigrować tylko pod warunkiem, że ma przypisany wejściowy punkt końcowy. |
| Sieć wirtualna |Sieć wirtualna |Sieć wirtualna jest migrowana razem ze wszystkimi właściwościami do modelu wdrażania usługi Resource Manager. Nowa grupa zasobów jest tworzona za pomocą sufiksu `-migrated`. |
| Zastrzeżone adresy IP |Publiczny adres IP z przydziałem statycznym |Zastrzeżone adresy IP skojarzone z modułem równoważenia obciążenia są migrowane w ramach migrowania usługi w chmurze lub maszyny wirtualnej. Migracja nieskojarzonego zastrzeżonego adresu IP nie jest obecnie obsługiwana. |
| Publiczny adres IP dla maszyny wirtualnej |Publiczny adres IP z przydziałem dynamicznym |Publiczny adres IP skojarzony z maszyną wirtualną jest konwertowany na zasób publicznego adresu IP z przydziałem statycznym. |
| Sieciowe grupy zabezpieczeń |Sieciowe grupy zabezpieczeń |Sieciowa grupa zabezpieczeń skojarzona z podsiecią jest klonowana w ramach migracji do modelu wdrażania usługi Resource Manager. Sieciowa grupa zabezpieczeń w klasycznym modelu wdrażania nie jest usuwana podczas migracji. Jednak operacje płaszczyzny zarządzania dla sieciowej grupy zabezpieczeń są zablokowane, gdy trwa migracja. |
| Serwery DNS |Serwery DNS |Serwery DNS skojarzone z siecią wirtualną lub maszyną wirtualną są migrowane w ramach migracji odpowiadających zasobów razem ze wszystkimi właściwościami. |
| Trasy zdefiniowane przez użytkownika |Trasy zdefiniowane przez użytkownika |Trasy zdefiniowane przez użytkownika skojarzone z podsiecią są klonowane w ramach migracji do modelu wdrażania usługi Resource Manager. Trasy zdefiniowane przez użytkownika w klasycznym modelu wdrażania nie są usuwane podczas migracji. Operacje płaszczyzny zarządzania dla tras zdefiniowanych przez użytkownika są zablokowane, gdy trwa migracja. |
| Właściwość przekazywania adresu IP w konfiguracji sieci maszyny wirtualnej |Właściwość przekazywania adresu IP dla karty sieciowej |Właściwość przekazywania adresu IP na maszynie wirtualnej jest konwertowana na właściwość interfejsu sieciowego podczas migracji. |
| Moduł równoważenia obciążenia z wieloma adresami IP |Moduł równoważenia obciążenia z wieloma zasobami publicznego adresu IP |Każdy publiczny adres IP skojarzone z modułem równoważenia obciążenia jest konwertowana zasób publicznego adresu IP i skojarzone z modułem równoważenia obciążenia po migracji. |
| Wewnętrzne nazwy DNS na maszynie wirtualnej |Wewnętrzne nazwy DNS na karcie sieciowej |Podczas migracji wewnętrzne sufiksy DNS dla maszyn wirtualnych są migrowane do właściwości tylko do odczytu o nazwie „InternalDomainNameSuffix” na karcie sieciowej. Sufiks pozostaje niezmieniona po migracji, a wirtualna rozpoznawania będą nadal działać jak poprzednio. |
| Brama sieci wirtualnej |Brama sieci wirtualnej |Właściwości bramy sieci wirtualnej są migrowane bez zmian. Adres VIP skojarzony z bramą także nie jest zmieniany. |
| Lokacja sieci lokalnej |Brama sieci lokalnej |Właściwości lokacji sieci lokalnej są migrowane bez zmian na nowy zasób o nazwie bramy sieci lokalnej. Reprezentuje prefiksy adresów lokalnych i adres IP bramy zdalnego. |
| Odwołania połączeń |Połączenie |Odwołania do łączności między bramą a lokacji sieci lokalnej w konfiguracji sieci jest reprezentowana przez nowy zasób o nazwie połączenia. Wszystkie właściwości łączności odwołania w plikach konfiguracji sieci są kopiowane bez zmian do zasobu połączenia. Łączność między sieciami wirtualnymi w klasycznym modelu wdrażania jest to osiągane przez utworzenie dwóch tuneli protokołu IPsec do lokacji sieci lokalnej reprezentujący sieci wirtualnych. To jest przekształcana typu wirtualnego — sieci do--połączenia wirtualnej sieci w modelu usługi Resource Manager, bez konieczności bramy sieci lokalnej. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Zmiany automatyzacji i narzędzi po migracji
W ramach migracji zasobów z klasycznym modelu wdrożenia do modelu wdrażania usługi Resource Manager należy zaktualizować z istniejącej automatyzacji lub narzędzi, aby upewnić się, że nadal działać po migracji.
