## <a name="meaning-of-migration-of-iaas-resources-from-classic-to-resource-manager"></a>Znaczenie migracji zasobów IaaS z modelu klasycznego do modelu usługi Azure Resource Manager
Zanim przejdziemy do szczegółów, spójrzmy na różnicę między operacjami płaszczyzny danych a operacjami płaszczyzny zarządzania w przypadku zasobów IaaS.

* *Płaszczyzna zarządzania/kontroli* opisuje wywołania, które wchodzą w skład płaszczyzny zarządzania/kontroli, lub interfejs API umożliwiający modyfikowanie zasobów. Operacje, na przykład takie jak utworzenie maszyny wirtualnej, ponowne uruchomienie maszyny wirtualnej i zaktualizowanie maszyny wirtualnej przy użyciu nowej podsieci, powodują wykonanie działań zarządzania uruchomionymi zasobami. Nie wpływają one bezpośrednio na nawiązywanie połączeń z wystąpieniami.
* *Płaszczyzna danych* (aplikacji) opisuje samo środowisko uruchomieniowe aplikacji i obejmuje interakcje z wystąpieniami, które nie korzystają z interfejsu API platformy Azure. Uzyskanie dostępu do witryny internetowej lub pobranie danych z uruchomionego wystąpienia programu SQL Server lub serwera MongoDB jest traktowane jako interakcja z płaszczyzną danych lub aplikacją. Skopiowanie obiektu blob z konta magazynu, uzyskanie dostępu do publicznego adresu IP na potrzeby połączenia RDP lub nawiązanie połączenia SSH z maszyną wirtualną także dotyczą płaszczyzny danych. Te operacje utrzymują działanie aplikacji w ramach usług obliczeniowych, sieciowych i magazynowych.

![Zrzut ekranu, który ilustruje różnice między płaszczyzną zarządzania/kontroli i płaszczyzną danych](../articles/virtual-machines/media/virtual-machines-windows-migration-classic-resource-manager/data-control-plane.png)

> [!NOTE]
> W przypadku niektórych scenariuszy migracji platforma Azure zatrzymuje maszyny wirtualne, cofa ich przydział i uruchamia je ponownie. To wiąże się z krótkim przestojem płaszczyzny danych.
>
>

## <a name="the-migration-experience"></a>Środowisko migracji
Przed uruchomieniem środowiska migracji zalecamy wykonanie następujących czynności:

* Upewnij się, że zasoby do zmigrowania nie używają żadnych nieobsługiwanych funkcji ani konfiguracji. Zazwyczaj platforma wykrywa te problemy i zgłasza błąd.
* Jeśli masz maszyny wirtualne poza siecią wirtualną, zostaną one zatrzymane, a ich przydział zostanie cofnięty w ramach operacji przygotowania. Jeśli nie chcesz stracić publicznych adresów IP, rozważ zastrzeżenie adresów IP przed wyzwoleniem operacji przygotowania. Jednak jeśli maszyny wirtualne znajdują się w sieci wirtualnej, to są nie zatrzymywane ani ich przydział nie jest cofany.
* Zaplanuj przeprowadzenie migracji poza godzinami pracy, aby uwzględnić możliwość wystąpienia nieoczekiwanych awarii podczas migracji.
* Pobierz bieżącą konfigurację maszyn wirtualnych przy użyciu programu PowerShell, poleceń interfejsu wiersza polecenia (CLI) lub interfejsów API REST, aby ułatwić walidację po zakończeniu kroku przygotowania.
* Zaktualizuj skrypty automatyzacji/operacjonalizacji pod kątem obsługi modelu wdrażania usługi Resource Manager przed rozpoczęciem migracji. Opcjonalnie możesz wykonać operacje GET, gdy zasoby są w stanie przygotowania.
* Oceń zasady RBAC skonfigurowane dla klasycznych zasobów IaaS i zaplanuj czynności po zakończeniu migracji.

Przepływ pracy migracji jest następujący

![Zrzut ekranu przedstawiający przepływ pracy migracji](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-workflow.png)

> [!NOTE]
> Wszystkie operacje opisane w poniższych sekcjach są idempotentne. Jeśli wystąpi problem inny niż nieobsługiwana funkcja lub błąd konfiguracji, należy spróbować wykonać ponownie operację przygotowania, przerwania lub zatwierdzenia. Platforma Azure spróbuje wykonać akcję ponownie.
>
>

### <a name="validate"></a>Walidacja
Operacja walidacji to pierwszy krok w procesie migracji. Celem tego kroku jest przeanalizowanie danych w tle dla migrowanych zasobów i zwrócenie powodzenia/niepowodzenia, jeśli zasoby można zmigrować.

Należy wybrać sieć wirtualną lub usługę hostowaną (jeśli nie jest używana sieć wirtualna), która ma zostać zwalidowana pod kątem migracji.

* Jeśli nie można zmigrować zasobu, platforma Azure wyświetli listę wszystkich przyczyn, które uniemożliwiają migrację.

Podczas walidowania usług magazynu można zauważyć, że migrowane konto znajduje się w grupie zasobów o takiej samej nazwie jak nazwa konta magazynu z dołączonym sufiksem „-Migrated” (Zmigrowane).  Na przykład jeśli konto magazynu ma nazwę „mój_magazyn”, zasób obsługujący usługę Azure Resource Manager będzie dostępny w grupie zasobów o nazwie "mój_magazyn-Migrated” i będzie zawierać konto magazynu o nazwie „mój_magazyn”.

### <a name="prepare"></a>Przygotowanie
Operacja przygotowania to drugi krok w procesie migracji. Celem tego kroku jest symulowanie przekształcenia zasobów IaaS z klasycznych na zasoby usługi Resource Manager i wyświetlenie jego wyniku w postaci porównania, co umożliwia zwizualizowanie procesu.

Należy wybrać sieć wirtualną lub usługę hostowaną (jeśli nie jest używana sieć wirtualna), która ma zostać przygotowana pod kątem migracji.

* Jeśli zasobu nie można zmigrować, platforma Azure zatrzyma proces migracji i wyświetli listę przyczyn niepowodzenia operacji przygotowania.
* Jeśli zasób można zmigrować, platforma Azure najpierw zablokuje operacje płaszczyzny zarządzania dla migrowanych zasobów. Na przykład nie będzie można dodać dysku danych do migrowanej maszyny wirtualnej.

Platforma Azure następnie rozpocznie migrację metadanych z modelu klasycznego do modelu usługi Resource Manager na potrzeby migrowania zasobów.

Po zakończeniu operacji przygotowania można zwizualizować zasoby w modelach klasycznym i usługi Resource Manager. Dla każdej usługi w chmurze w klasycznym modelu wdrażania platforma Azure tworzy nazwę grupy zasobów zgodnie ze wzorcem `cloud-service-name>-Migrated`.

> [!NOTE]
> Nie można wybrać nazwy grupy zasobów tworzonej dla migrowanych zasobów (dotyczy sufiksu „-Migrated”), ale po zakończeniu migracji można użyć funkcji przenoszenia usługi Azure Resource Manager do przeniesienia zasobów do dowolnej grupy zasobów. Aby przeczytać więcej na ten temat, zobacz [Move resources to new resource group or subscription](../articles/resource-group-move-resources.md) (Przenoszenie zasobów do nowej grupy zasobów lub subskrypcji)

W tym miejscu przedstawiono dwa ekrany, które przedstawiają wynik pomyślnie wykonanej operacji przygotowania. Pierwszy ekran przedstawia grupę zasobów, która zawiera oryginalną usługę w chmurze. Drugi ekran przedstawia nową grupę zasobów (z sufiksem „-Migrated”), która zawiera równoważne zasoby usługi Azure Resource Manager.

![Zrzut ekranu przedstawiający klasyczną usługę w chmurze w portalu](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-classic.png)

![Zrzut ekranu przedstawiający zasoby usługi Azure Resource Manager w witrynie Portal w ramach operacji przygotowania](../articles/virtual-machines/windows/media/migration-classic-resource-manager/portal-arm.png)

> [!NOTE]
> Maszyny wirtualne poza klasyczną siecią wirtualną zostaną zatrzymane, a ich przydziały zostaną cofnięte w tej fazie migracji.
>
>

### <a name="check-manual-or-scripted"></a>Sprawdzenie (ręczne lub za pomocą skryptu)
W kroku sprawdzenia możesz opcjonalnie użyć konfiguracji pobranej wcześniej, aby ogólnie zwalidować poprawność migracji. Alternatywnie możesz zalogować się do portalu i wybiórczo sprawdzić właściwości i zasoby w celu zwalidowania ogólnej poprawności migracji metadanych.

Jeśli migrujesz sieć wirtualną, większość konfiguracji maszyn wirtualnych nie zostanie uruchomiona ponownie. W przypadku aplikacji na tych maszynach wirtualnych możesz zwalidować, czy aplikacja jest nadal uruchomiona.

Możesz przetestować skrypty monitorowania/automatyzacji i operacyjne, aby sprawdzić, czy maszyny wirtualne działają, jak powinny, i czy zaktualizowane skrypty działają poprawnie. Tylko operacje GET są obsługiwane, jeśli zasoby są w stanie przygotowania.

Nie ma ustalonego przedziału czasu na zatwierdzenie migracji. Ten stan może trwać tak długo, jak chcesz. Jednak płaszczyzna zarządzania jest zablokowana dla tych zasobów do momentu przerwania lub zatwierdzenia.

Jeśli napotkasz jakiekolwiek problemy, zawsze możesz przerwać migrację i wrócić do klasycznego modelu wdrażania. Po powrocie platforma Azure otworzy operacje płaszczyzny zarządzania dla zasobów, co umożliwi wznowienie zwykłych operacji dla tych maszyn wirtualnych w klasycznym modelu wdrażania.

### <a name="abort"></a>Przerwanie
Przerwanie to krok opcjonalny, którego można użyć do przywrócenia zmian w celu powrotu do klasycznego modelu wdrażania oraz zatrzymania migracji.

> [!NOTE]
> Nie można wykonać tej operacji po wyzwoleniu operacji zatwierdzenia.     
>
>

### <a name="commit"></a>Zatwierdzenie
Po zakończeniu walidacji możesz zatwierdzić migrację. Zasoby nie będą już dostępne w klasycznym modelu wdrażania, a tylko w modelu wdrażania usługi Resource Manager. Zmigrowanymi zasobami można zarządzać tylko w nowym portalu.

> [!NOTE]
> Jest to operacja idempotentna. Jeśli się nie powiedzie, zalecane jest podjęcie kolejnej próby. Jeśli operacja w dalszym ciągu będzie kończyć się niepowodzeniem, utwórz bilet pomocy technicznej lub wpis z tagiem ClassicIaaSMigration na naszym [forum maszyn wirtualnych](https://social.msdn.microsoft.com/Forums/azure/home?forum=WAVirtualMachinesforWindows).
>
>
<br>
W tym miejscu znajduje się schemat blokowy kroków procesu migracji

![Zrzut ekranu przedstawiający kroki migracji](../articles/virtual-machines/windows/media/migration-classic-resource-manager/migration-flow.png)

## <a name="translation-of-classic-to-azure-resource-manager-resources"></a>Przekształcenie zasobów klasycznych na zasoby usługi Azure Resource Manager
Poniższa tabela zawiera reprezentacje klasyczne i usługi Resource Manager dla zasobów. Inne funkcje i zasoby nie są obecnie obsługiwane.

| Reprezentacja klasyczna | Reprezentacja usługi Resource Manager | Szczegółowe uwagi |
| --- | --- | --- |
| Nazwa usługi w chmurze |Nazwa DNS |Podczas migracji jest tworzona nowa grupa zasobów dla każdej usługi w chmurze przy użyciu wzorca nazewnictwa `<cloudservicename>-migrated`. Ta grupa zasobów zawiera wszystkie zasoby. Nazwa usługi w chmurze stanie się nazwą DNS skojarzoną z publicznym adresem IP. |
| Maszyna wirtualna |Maszyna wirtualna |Właściwości specyficzne dla maszyny wirtualnej są migrowane bez zmian. Niektóre informacje typu osProfile, takie jak nazwa komputera, nie są przechowywane w klasycznym modelu wdrażania i pozostaną puste po migracji. |
| Zasoby dysków dołączonych do maszyny wirtualnej |Niejawne dyski dołączone do maszyny wirtualnej |Dyski nie są modelowane jako zasoby najwyższego poziomu w modelu wdrażania usługi Resource Manager. Są one migrowane jako dyski niejawne w ramach maszyny wirtualnej. Tylko dyski dołączone do maszyny wirtualnej są obecnie obsługiwane. Maszyny wirtualne usługi Resource Manager mogą teraz używać klasycznych kont magazynu, dzięki czemu można łatwo zmigrować dyski bez wykonywania żadnych aktualizacji. |
| Rozszerzenia maszyn wirtualnych |Rozszerzenia maszyn wirtualnych |Wszystkie rozszerzenia zasobów, z wyjątkiem rozszerzeń XML, są migrowane z klasycznego modelu wdrażania. |
| Certyfikaty maszyny wirtualnej |Certyfikaty w usłudze Azure Key Vault |Jeśli usługa w chmurze zawiera certyfikaty usługi, jest tworzony nowy magazyn kluczy platformy Azure dla usługi w chmurze i certyfikaty są przenoszone do tego magazynu kluczy. Maszyny wirtualne są aktualizowane, tak aby przywoływały certyfikaty z magazynu kluczy. <br><br> **UWAGA:** Nie usuwaj magazynu kluczy, ponieważ może to spowodować przejście maszyny wirtualnej w stan niepowodzenia. Pracujemy nad ulepszeniami zaplecza umożliwiającymi bezpieczne usuwanie magazynów kluczy lub przenoszenie ich z maszyną wirtualną do nowej subskrypcji. |
| Konfiguracja usługi WinRM |Konfiguracja usługi WinRM w ramach elementu osProfile |Konfiguracja usługi Windows Remote Management jest przenoszona bez zmian w ramach migracji. |
| Właściwość Availability-set |Zasób Availability-set | Specyfikacja Availability-set była właściwością maszyny wirtualnej w klasycznym modelu wdrażania. Zestawy dostępności stają się zasobami najwyższego poziomu w ramach migracji. Następujące konfiguracje nie są obsługiwane: wiele zestawów dostępności dla usługi w chmurze, co najmniej jeden zestaw dostępności razem z maszyną wirtualną poza zestawem dostępności w ramach usługi w chmurze. |
| Konfiguracja sieci na maszynie wirtualnej |Podstawowy interfejs sieciowy |Konfiguracja sieci na maszynie wirtualnej jest reprezentowana jako zasób podstawowego interfejsu sieciowego po migracji. W przypadku maszyn wirtualnych poza siecią wirtualną wewnętrzny adres IP zmieni się podczas migracji. |
| Wiele interfejsów sieciowych na maszynie wirtualnej |Interfejsy sieciowe |Jeśli maszyna wirtualna ma skojarzonych wiele interfejsów sieciowych, każdy interfejs sieciowy, razem ze wszystkimi właściwościami, stanie się zasobem najwyższego poziomu w modelu wdrażania usługi Resource Manager podczas migracji. |
| Zestaw punktów końcowych z równoważeniem obciążenia |Moduł równoważenia obciążenia |W klasycznym modelu wdrażania do platformy jest przypisany niejawny moduł równoważenia obciążenia dla każdej usługi w chmurze. Podczas migracji jest tworzony nowy zasób modułu równoważenia obciążenia, a zestaw punktów końcowych równoważenia obciążenia staje się regułami modułu równoważenia obciążenia. |
| Reguły NAT dla ruchu przychodzącego |Reguły NAT dla ruchu przychodzącego |Wejściowe punkty końcowe zdefiniowane na maszynie wirtualnej są konwertowane na reguły translacji dla adresu sieciowego ruchu przychodzącego w module równoważenia obciążenia podczas migracji. |
| Adres VIP |Publiczny adres IP z nazwą DNS |Wirtualny adres IP stanie się publicznym adresem IP i będzie skojarzony z modułem równoważenia obciążenia. Wirtualny adres IP można zmigrować tylko pod warunkiem, że ma przypisany wejściowy punkt końcowy. |
| Sieć wirtualna |Sieć wirtualna |Sieć wirtualna jest migrowana razem ze wszystkimi właściwościami do modelu wdrażania usługi Resource Manager. Nowa grupa zasobów jest tworzona za pomocą sufiksu `-migrated`. |
| Zastrzeżone adresy IP |Publiczny adres IP z przydziałem statycznym |Zastrzeżone adresy IP skojarzone z modułem równoważenia obciążenia są migrowane w ramach migrowania usługi w chmurze lub maszyny wirtualnej. Migracja nieskojarzonego zastrzeżonego adresu IP nie jest obecnie obsługiwana. |
| Publiczny adres IP dla maszyny wirtualnej |Publiczny adres IP z przydziałem dynamicznym |Publiczny adres IP skojarzony z maszyną wirtualną jest konwertowany na zasób publicznego adresu IP z przydziałem statycznym. |
| Sieciowe grupy zabezpieczeń |Sieciowe grupy zabezpieczeń |Sieciowa grupa zabezpieczeń skojarzona z podsiecią jest klonowana w ramach migracji do modelu wdrażania usługi Resource Manager. Sieciowa grupa zabezpieczeń w klasycznym modelu wdrażania nie jest usuwana podczas migracji. Jednak operacje płaszczyzny zarządzania dla sieciowej grupy zabezpieczeń są zablokowane, gdy trwa migracja. |
| Serwery DNS |Serwery DNS |Serwery DNS skojarzone z siecią wirtualną lub maszyną wirtualną są migrowane w ramach migracji odpowiadających zasobów razem ze wszystkimi właściwościami. |
| Trasy zdefiniowane przez użytkownika |Trasy zdefiniowane przez użytkownika |Trasy zdefiniowane przez użytkownika skojarzone z podsiecią są klonowane w ramach migracji do modelu wdrażania usługi Resource Manager. Trasy zdefiniowane przez użytkownika w klasycznym modelu wdrażania nie są usuwane podczas migracji. Operacje płaszczyzny zarządzania dla tras zdefiniowanych przez użytkownika są zablokowane, gdy trwa migracja. |
| Właściwość przekazywania adresu IP w konfiguracji sieci maszyny wirtualnej |Właściwość przekazywania adresu IP dla karty sieciowej |Właściwość przekazywania adresu IP na maszynie wirtualnej jest konwertowana na właściwość interfejsu sieciowego podczas migracji. |
| Moduł równoważenia obciążenia z wieloma adresami IP |Moduł równoważenia obciążenia z wieloma zasobami publicznego adresu IP |Każdy publiczny adres IP skojarzony z modułem równoważenia obciążenia jest konwertowany na zasób publicznego adresu IP i kojarzony z modułem równoważenia obciążenia po migracji. |
| Wewnętrzne nazwy DNS na maszynie wirtualnej |Wewnętrzne nazwy DNS na karcie sieciowej |Podczas migracji wewnętrzne sufiksy DNS dla maszyn wirtualnych są migrowane do właściwości tylko do odczytu o nazwie „InternalDomainNameSuffix” na karcie sieciowej. Sufiks pozostaje niezmieniony po migracji i rozpoznawanie maszyny wirtualnej powinno działać jak poprzednio. |
| Brama sieci wirtualnej |Brama sieci wirtualnej |Właściwości bramy sieci wirtualnej są migrowane bez zmian. Adres VIP skojarzony z bramą także nie jest zmieniany. |
| Lokacja sieci lokalnej |Brama sieci lokalnej |Właściwości lokacji sieci lokalnej są migrowane bez zmian do nowego zasobu o nazwie Brama sieci lokalnej. Reprezentuje on prefiksy adresów lokalnych i adres IP bramy zdalnej. |
| Odwołania połączeń |Połączenie |Odwołania łączności między bramą a lokacją sieci lokalnej w konfiguracji sieci są reprezentowane przez nowo utworzony zasób o nazwie Połączenia w usłudze Resource Manager po migracji. Wszystkie właściwości odwołania łączności w plikach konfiguracji sieci są kopiowane bez zmian do nowo utworzonego zasobu Połączenie. Połączenie między sieciami wirtualnymi w modelu klasycznym jest realizowane za pomocą dwóch tuneli protokołu IPsec do lokacji sieci lokalnej reprezentujących sieci wirtualne. Ta konfiguracja jest przekształcana w połączenie typu Vnet2Vnet w modelu usługi Resource Manager, a lokalne bramy sieci nie są wymagane. |

## <a name="changes-to-your-automation-and-tooling-after-migration"></a>Zmiany automatyzacji i narzędzi po migracji
W ramach migracji zasobów z klasycznego modelu wdrażania do modelu wdrażania usługi Resource Manager musisz zaktualizować istniejące automatyzacje lub narzędzia, aby zapewnić, że będą nadal działać po migracji.
