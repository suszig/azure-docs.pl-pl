
Jeśli problem Azure nie jest opisany w tym artykule, odwiedź [Azure fora MSDN i przepełnienie stosu](https://azure.microsoft.com/support/forums/). Możesz zamieścić problemu na tych fora lub do @AzureSupport w serwisie Twitter. Ponadto można pliku żądania pomocy technicznej platformy Azure, wybierając **uzyskać pomoc techniczną** na [pomocy technicznej platformy Azure](https://azure.microsoft.com/support/options/) lokacji.

## <a name="general-troubleshooting-steps"></a>Ogólne kroki rozwiązywania problemów
### <a name="troubleshoot-common-allocation-failures-in-the-classic-deployment-model"></a>Rozwiązywanie problemów z typowych błędów alokacji w klasycznym modelu wdrażania
Te czynności może pomóc rozwiązać wiele błędów alokacji w przypadku maszyn wirtualnych:

* Zmień rozmiar maszyny Wirtualnej do innego rozmiaru maszyny Wirtualnej.<br>
    Kliknij przycisk **Przeglądaj wszystkie** > **maszyn wirtualnych (klasyczne)** > maszyny wirtualnej > **ustawienia** > **rozmiar**. Aby uzyskać szczegółowe instrukcje, zobacz [zmienić rozmiaru maszyny wirtualnej](https://msdn.microsoft.com/library/dn168976.aspx).
* Usunięcie wszystkich maszyn wirtualnych z usługi w chmurze i ponowne utworzenie maszyn wirtualnych.<br>
    Kliknij przycisk **Przeglądaj wszystkie** > **maszyn wirtualnych (klasyczne)** > maszyny wirtualnej > **usunąć**. Następnie kliknij przycisk **Utwórz zasób** > **obliczeniowe** > [Obraz maszyny wirtualnej].

### <a name="troubleshoot-common-allocation-failures-in-the-azure-resource-manager-deployment-model"></a>Rozwiązywanie problemów z typowych błędów alokacji w modelu wdrażania usługi Azure Resource Manager
Te czynności może pomóc rozwiązać wiele błędów alokacji w przypadku maszyn wirtualnych:

* Zatrzymaj (deallocate) wszystkich maszyn wirtualnych w tym samym dostępności ustawiona, a następnie uruchom ponownie każdej z nich.<br>
    Aby zatrzymać: kliknij **grup zasobów** > grupy zasobów > **zasobów** > zestawu dostępności > **maszyn wirtualnych** > maszyny wirtualnej >  **Zatrzymaj**.
  
    Po wszystkich maszyn wirtualnych, Zatrzymaj, wybierz pierwsza maszyna wirtualna i kliknij przycisk **Start**.

## <a name="background-information"></a>Informacje uzupełniające
### <a name="how-allocation-works"></a>Jak działa alokacji
Serwery w centrach danych platformy Azure są dzielone w klastrach. Zwykle żądanie alokacji nastąpiła w wielu klastrach, ale istnieje możliwość, że niektóre ograniczenia na podstawie żądań alokacji wymusić platformy Azure prób żądania w tylko jednym klastrze. W tym artykule firma Microsoft będzie odwoływać się do to jako "przypięty do klastra". Poniższy diagram 1 przedstawiono w przypadku normalnych alokacji, który zostanie użyty w wielu klastrach. Diagram 2 przedstawia to alokacji który zawiera przypięta do klastra 2, ponieważ jest to, gdzie jest hostowana istniejącego zestawu CS_1 usługi w chmurze lub dostępności.
![Diagram alokacji](./media/virtual-machines-common-allocation-failure/Allocation1.png)

### <a name="why-allocation-failures-happen"></a>Dlaczego stanie błędów alokacji
Żądanie alokacji jest przypięta do klastra, ma wyższy stopień można znaleźć zwolnić zasoby, ponieważ pula zasobów dostępny jest mniejszy. Ponadto jeśli żądanie alokacji jest przypięta do klastra, ale typ zasobu, która miała nie jest obsługiwany przez ten klaster, żądanie zakończy się niepowodzeniem nawet w przypadku klastra z bezpłatnych zasobów. Poniższy diagram 3 ilustruje przypadek, w których alokacji przypiętych nie powiedzie się, ponieważ klaster tylko kandydujące nie ma zwolnić zasoby. Diagram 4 przedstawia przypadek, w którym przypiętych alokacji nie działa, ponieważ klastra tylko kandydata nie obsługuje żądany rozmiar maszyny Wirtualnej, nawet jeśli klaster ma zwolnić zasoby.

![Błąd alokacji przypiętych](./media/virtual-machines-common-allocation-failure/Allocation2.png)

## <a name="detailed-troubleshoot-steps-specific-allocation-failure-scenarios-in-the-classic-deployment-model"></a>Szczegółowe Rozwiązywanie problemów z scenariuszy awarii alokacji określonych czynności w klasycznym modelu wdrażania
Poniżej przedstawiono typowe scenariusze alokacji, które powodują żądanie alokacji przypięty. Firma Microsoft będzie Poznaj każdego scenariusza w dalszej części tego artykułu.

* Zmień rozmiar maszyny Wirtualnej lub Dodaj maszyny wirtualne lub wystąpień roli do istniejącej usługi w chmurze
* Uruchom ponownie częściowo zatrzymania maszyny wirtualnej (cofnięciu przydziału)
* Uruchom ponownie pełni zatrzymania maszyny wirtualnej (cofnięciu przydziału)
* Wdrożeń przemieszczania/produkcyjnych (platforma jako usługa tylko)
* Grupa koligacji (zbliżeniowe maszyny Wirtualnej lub usługi)
* Oparte na grupach koligacji sieci wirtualnej

Gdy zostanie wyświetlony błąd alokacji, zobacz, jeśli dowolny z opisanych scenariuszy dotyczy ten błąd. Błąd alokacji zwrócony przez platformę Azure umożliwia zidentyfikowanie odpowiednim scenariuszem. Jeśli żądanie jest przypięty, usuń niektóre przypinania ograniczenia, aby otworzyć żądania do klastrów więcej, co zwiększa prawdopodobieństwo pomyślnego alokacji.

Ogólnie rzecz biorąc tak długo, jak błąd nie wskazuje "żądany rozmiar maszyny Wirtualnej nie jest obsługiwane", możesz zawsze ponowić w późniejszym czasie, jak mała liczba zasobów został zwolniony w klastrze, aby obsłużyć żądanie. Jeśli problem nie zostanie żądany rozmiar maszyny Wirtualnej nie jest obsługiwana, spróbuj inny rozmiar maszyny Wirtualnej. W przeciwnym razie jedyną opcją jest usunięcie przypinania ograniczenia.

Dwie typowe scenariusze niepowodzenia są związane z grup koligacji. W przeszłości użyto grupy koligacji zapewnienie pobliżu wystąpień maszyn wirtualnych/usługi lub użyto w celu umożliwienia utworzenia sieci wirtualnej. Wraz z wprowadzeniem regionalnych sieci wirtualnych grup koligacji nie są wymagane do utworzenia sieci wirtualnej. Zmniejszeniu opóźnienia sieci w infrastrukturze Azure zalecenie, aby użyć grup koligacji dla maszyny Wirtualnej lub usługi zbliżeniowe została zmieniona.

Rysunek 5 poniżej przedstawia taksonomii w scenariuszach alokacji (przypiętych).
![Taksonomii przypiętych alokacji](./media/virtual-machines-common-allocation-failure/Allocation3.png)

> [!NOTE]
> Błąd na liście każdego scenariusza alokacji jest krótkich fragmentów. Zapoznaj się [błąd ciąg wyszukiwania](#Error string lookup) dla ciągów szczegółowe informacje o błędzie.
> 
> 

## <a name="allocation-scenario-resize-a-vm-or-add-vms-or-role-instances-to-an-existing-cloud-service"></a>Alokacja scenariusz: zmiana rozmiaru maszyny Wirtualnej lub Dodaj maszyny wirtualne lub wystąpień roli do istniejącej usługi w chmurze
Błąd

Upgrade_VMSizeNotSupported lub GeneralError

**Przyczyną przypinanie klastra**

Żądanie, aby zmienić rozmiar maszyny Wirtualnej lub dodać do istniejącej usługi w chmurze maszyny Wirtualnej lub wystąpienia roli musi podjąć w oryginalnym klastrze obsługującego istniejącą usługę w chmurze. Tworzenie nowej usługi w chmurze pozwala platformy Azure znaleźć innego klastra lub obsługuje rozmiar maszyny Wirtualnej, którego zażądano zwolnić zasoby.

**Obejście problemu**

Jeśli błąd Upgrade_VMSizeNotSupported *, spróbuj zmienić rozmiar maszyny Wirtualnej. Jeśli przy użyciu innego rozmiaru maszyny Wirtualnej nie ma możliwości użycia, ale można użyć różnych wirtualnego adresu IP (VIP), należy utworzyć nową usługę w chmurze do obsługi nowej maszyny Wirtualnej, a następnie dodaj nową usługę w chmurze do regionalną sieć wirtualną, której istniejące maszyny wirtualne są uruchomione. Użycie istniejącej usługi w chmurze nie regionalną sieć wirtualną, możesz utworzyć nową sieć wirtualną dla nowej usługi w chmurze, a następnie połącz z [istniejącej sieci wirtualnej do nowej sieci wirtualnej](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zobacz więcej informacji [regionalnych sieci wirtualnych](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Jeśli błąd jest GeneralError *, jest prawdopodobne, że typ zasobu (np. dla określonego rozmiaru maszyny Wirtualnej) jest obsługiwana przez klaster, ale klaster nie ma wolnego zasobów w tej chwili. Podobny scenariusz powyżej, Dodaj zasób obliczeniowy żądany przez proces tworzenia nowej usługi w chmurze (Zauważ, że nowa usługa w chmurze ma używać innego adresu VIP) i połączenia usługi w chmurze przy użyciu regionalną sieć wirtualną.

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Scenariusz alokacji: ponowne uruchomienie częściowo zatrzymano (cofnięciu przydziału) maszyn wirtualnych
Błąd

GeneralError *

**Przyczyną przypinanie klastra**

Częściowe dezalokacji oznacza, że zatrzymane (cofnięciu przydziału) jeden lub więcej, ale nie wszystkich maszyn wirtualnych w usłudze w chmurze. Po zatrzymaniu (deallocate) na maszynie Wirtualnej, są wydawane skojarzonych zasobów. Ponowne uruchomienie tego zatrzymanej maszyny Wirtualnej (cofnięciu przydziału) w związku z tym jest nowe żądanie alokacji. Ponowne uruchamianie maszyn wirtualnych w usłudze w chmurze częściowo deallocated jest odpowiednikiem Dodawanie maszyn wirtualnych do istniejącej usługi w chmurze. Żądanie alokacji musi podjąć w oryginalnym klastrze obsługującego istniejącą usługę w chmurze. Trwa tworzenie innej usługi chmury umożliwia platformy Azure znaleźć innego klastra, który bezpłatnego zasobu lub obsługuje rozmiar maszyny Wirtualnej, którego zażądano.

**Obejście problemu**

Jeśli można użyć różnych adresów VIP, Usuń zatrzymania maszyny wirtualnej (cofnięciu przydziału) (ale zachować skojarzone dyski) i dodać ponownie za pomocą innej usługi chmury maszyn wirtualnych. Użyj regionalną sieć wirtualną do połączenia usługi w chmurze:

* Jeśli istniejącą usługę w chmurze używa regionalną sieć wirtualną, po prostu Dodaj nową usługę w chmurze do tej samej sieci wirtualnej.
* Użycie istniejącej usługi w chmurze nie regionalną sieć wirtualną, Utwórz nową sieć wirtualną dla nowej usługi w chmurze, a następnie [połączyć istniejącej sieci wirtualnej do nowej sieci wirtualnej](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zobacz więcej informacji [regionalnych sieci wirtualnych](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

## <a name="allocation-scenario-restart-fully-stopped-deallocated-vms"></a>Scenariusz alokacji: ponowne uruchomienie całkowicie zatrzymana (cofnięciu przydziału) maszyn wirtualnych
Błąd

GeneralError *

**Przyczyną przypinanie klastra**

Pełna dezalokacji oznacza, że został zatrzymany (cofnięciu przydziału) wszystkich maszyn wirtualnych z usługą w chmurze. Żądań alokacji, aby ponownie uruchomić te maszyny wirtualne muszą podjąć na oryginalnego klastra, który jest hostem usługi w chmurze. Tworzenie nowej usługi w chmurze pozwala platformy Azure znaleźć innego klastra lub obsługuje rozmiar maszyny Wirtualnej, którego zażądano zwolnić zasoby.

**Obejście problemu**

Jeśli można użyć różnych adresów VIP, usunąć oryginalny zatrzymania (cofnięciu przydziału) maszyn wirtualnych (ale zachować skojarzone dyski) i usuń odpowiednie usługi w chmurze (zasoby obliczeniowe skojarzone już zostały wydane, gdy zostanie zatrzymane (cofnięciu przydziału) maszyn wirtualnych). Utwórz nową usługę w chmurze można dodać z powrotem maszyn wirtualnych.

## <a name="allocation-scenario-stagingproduction-deployments-platform-as-a-service-only"></a>Scenariusz alokacji: wdrożeń przemieszczania/produkcyjnych (platforma jako usługa tylko)
Błąd

New_General * lub New_VMSizeNotSupported *

**Przyczyną przypinanie klastra**

Wdrażanie tymczasowej i wdrożenia produkcyjnego usługi w chmurze są obsługiwane w tym samym klastrze. Po dodaniu drugiego wdrożenia odpowiednie żądanie alokacji będą podejmowane w tym samym klastrze obsługującego ich pierwszym wdrożeniu.

**Obejście problemu**

Usuń ich pierwszym wdrożeniu i oryginalnego usługi w chmurze, a następnie ponownie wdrożyć usługę w chmurze. Ta akcja może grunt ich pierwszym wdrożeniu w klastrze, który ma wystarczającej ilości wolnych zasobów, aby dopasować oba wdrożenia lub w klastrze, który obsługuje rozmiarów maszyn wirtualnych, które zostały wybrane.

## <a name="allocation-scenario-affinity-group-vmservice-proximity"></a>Scenariusz alokacji: Grupa koligacji (zbliżeniowe maszyny Wirtualnej lub usługi)
Błąd

New_General * lub New_VMSizeNotSupported *

**Przyczyną przypinanie klastra**

Wszelkie obliczeń zasobów przypisanych do grupy koligacji jest związany z jednego klastra. Nowy zasób obliczeniowy żądania w tej grupie koligacji są próby w tym samym klastrze, gdzie hostowane są istniejących zasobów. Jest to możliwe, czy nowe zasoby są tworzone przez nową usługę w chmurze albo przez istniejącą usługę w chmurze.

**Obejście problemu**

Grupy koligacji nie jest konieczne, nie używać grupy koligacji, czy grupa zasobów obliczeniowych do wielu grup koligacji.

## <a name="allocation-scenario-affinity-group-based-virtual-network"></a>Scenariusz alokacji: oparte na grupach koligacji sieci wirtualnej
Błąd

New_General * lub New_VMSizeNotSupported *

**Przyczyną przypinanie klastra**

Przed wprowadzeniem regionalnych sieci wirtualnych, wymagane było skojarzyć sieć wirtualną z grupą koligacji. W związku z tym obliczeniowe zasobów umieszczone w grupie koligacji są powiązane przez takich samych ograniczeń, zgodnie z opisem w "alokacji scenariusz: Grupa koligacji (zbliżeniowe maszyny Wirtualnej lub usługi)" powyższej sekcji. Zasoby obliczeniowe są powiązane z jednego klastra.

**Obejście problemu**

Nie należy do grupy koligacji, Utwórz nowy regionalną sieć wirtualną dla nowych zasobów w przypadku dodawania, a następnie [połączyć istniejącej sieci wirtualnej do nowej sieci wirtualnej](https://azure.microsoft.com/blog/vnet-to-vnet-connecting-virtual-networks-in-azure-across-different-regions/). Zobacz więcej informacji [regionalnych sieci wirtualnych](https://azure.microsoft.com/blog/2014/05/14/regional-virtual-networks/).

Można też [migrację do regionalnej sieci wirtualnej sieci wirtualnej na podstawie grupy koligacji](https://azure.microsoft.com/blog/2014/11/26/migrating-existing-services-to-regional-scope/), a następnie ponownie dodaj żądanych zasobów.

## <a name="detailed-troubleshooting-steps-specific-allocation-failure-scenarios-in-the-azure-resource-manager-deployment-model"></a>Szczegółowe Rozwiązywanie problemów z scenariuszy awarii alokacji określonych kroków w modelu wdrażania usługi Azure Resource Manager
Poniżej przedstawiono typowe scenariusze alokacji, które powodują żądanie alokacji przypięty. Firma Microsoft będzie Poznaj każdego scenariusza w dalszej części tego artykułu.

* Zmień rozmiar maszyny Wirtualnej lub Dodaj maszyny wirtualne lub wystąpień roli do istniejącej usługi w chmurze
* Uruchom ponownie częściowo zatrzymania maszyny wirtualnej (cofnięciu przydziału)
* Uruchom ponownie pełni zatrzymania maszyny wirtualnej (cofnięciu przydziału)

Gdy zostanie wyświetlony błąd alokacji, zobacz, jeśli dowolny z opisanych scenariuszy dotyczy ten błąd. Błąd alokacji zwrócony przez platformę Azure umożliwia zidentyfikowanie odpowiednim scenariuszem. Jeśli żądanie jest przypięta do istniejącego klastra, usuń niektóre przypinania ograniczenia, aby otworzyć żądania do klastrów więcej, co zwiększa prawdopodobieństwo pomyślnego alokacji.

Ogólnie rzecz biorąc tak długo, jak błąd nie wskazuje "żądany rozmiar maszyny Wirtualnej nie jest obsługiwane", możesz zawsze ponowić w późniejszym czasie, jak mała liczba zasobów został zwolniony w klastrze, aby obsłużyć żądanie. Jeśli problem nie zostanie żądany rozmiar maszyny Wirtualnej nie jest obsługiwana, wymienione poniżej obejścia.

## <a name="allocation-scenario-resize-a-vm-or-add-vms-to-an-existing-availability-set"></a>Alokacja scenariusz: zmiana rozmiaru maszyny Wirtualnej lub dodawanie maszyn wirtualnych do istniejącego zestawu dostępności
Błąd

Upgrade_VMSizeNotSupported * lub GeneralError *

**Przyczyną przypinanie klastra**

Żądanie, aby zmienić rozmiar maszyny Wirtualnej lub dodać do istniejącego zestawu dostępności maszyny Wirtualnej musi podjąć w oryginalnego klastra, który jest hostem istniejącego zestawu dostępności. Tworzenie nowego zestawu dostępności umożliwia platformy Azure znaleźć innego klastra lub obsługuje rozmiar maszyny Wirtualnej, którego zażądano zwolnić zasoby.

**Obejście problemu**

Jeśli błąd Upgrade_VMSizeNotSupported *, spróbuj zmienić rozmiar maszyny Wirtualnej. Jeśli przy użyciu innego rozmiaru maszyny Wirtualnej nie jest opcją, zatrzymanie wszystkich maszyn wirtualnych w zestawie dostępności. Następnie można zmienić rozmiaru maszyny wirtualnej, który przyzna maszyny Wirtualnej do klastra, który obsługuje żądany rozmiar maszyny Wirtualnej.

Jeśli błąd jest GeneralError *, jest prawdopodobne, że typ zasobu (np. dla określonego rozmiaru maszyny Wirtualnej) jest obsługiwana przez klaster, ale klaster nie ma wolnego zasobów w tej chwili. Jeśli maszyna wirtualna może być częścią zestawu dostępności różnych, Utwórz nową maszynę Wirtualną w różnych dostępności, ustawić (w tym samym regionie). Następnie można dodać tej nowej maszyny Wirtualnej do tej samej sieci wirtualnej.  

## <a name="allocation-scenario-restart-partially-stopped-deallocated-vms"></a>Scenariusz alokacji: ponowne uruchomienie częściowo zatrzymano (cofnięciu przydziału) maszyn wirtualnych
Błąd

GeneralError *

**Przyczyną przypinanie klastra**

Częściowe dezalokacji oznacza, że zatrzymane (cofnięciu przydziału) co najmniej jeden, ale nie wszystkie, maszyn wirtualnych w dostępności ustawione. Po zatrzymaniu (deallocate) na maszynie Wirtualnej, są wydawane skojarzonych zasobów. Ponowne uruchomienie tego zatrzymanej maszyny Wirtualnej (cofnięciu przydziału) w związku z tym jest nowe żądanie alokacji. Ponowne uruchamianie maszyn wirtualnych w zestawie dostępności częściowo deallocated jest odpowiednikiem Dodawanie maszyn wirtualnych do istniejącego zestawu dostępności. Żądanie alokacji musi podjąć w oryginalnego klastra, który jest hostem istniejącego zestawu dostępności.

**Obejście problemu**

Zatrzymanie wszystkich maszyn wirtualnych w zestawie przed ponownym uruchomieniem pierwszego dostępności. Dzięki uruchomieniu nowego próba alokacji i że nowy klaster można wybrać z dostępnej pojemności.

## <a name="allocation-scenario-restart-fully-stopped-deallocated"></a>Scenariusz alokacji: ponowne uruchomienie całkowicie zatrzymana (cofnięciu przydziału)
Błąd

GeneralError *

**Przyczyną przypinanie klastra**

Pełna dezalokacji oznacza, że został zatrzymany (cofnięciu przydziału) wszystkich maszyn wirtualnych w zestawie dostępności. Żądanie alokacji o ponowne uruchomienie tych maszyn wirtualnych będzie obowiązywać wszystkich klastrów, które obsługują wymagany rozmiar.

**Obejście problemu**

Wybierz nowy rozmiar maszyny Wirtualnej do przydzielenia. Jeśli to nie zadziała, spróbuj ponownie później.

<a name="Error string lookup"></a>

## <a name="error-string-lookup"></a>Błąd podczas wyszukiwania ciągu
**New_VMSizeNotSupported***

"Maszyny Wirtualnej rozmiaru (lub kombinacja rozmiarów maszyn wirtualnych) wymagany przez to wdrożenie jest niedostępna z powodu ograniczeń żądania wdrożenia. Jeśli to możliwe spróbuj złagodzić ograniczenia, np. powiązania sieci wirtualnej, wdrażając w usłudze hostowanej nie zawiera żadnych innych wdrożeń i w innej grupie koligacji lub bez takiej grupy lub wdrożenie w innym regionie."

**New_General***

"Nie można przydzielić; Nie można zrealizować ograniczenia w żądaniu. Żądana nowego wdrożenia usługi jest powiązany z grupy koligacji, jego celem sieci wirtualnej lub Brak istniejącego wdrożenia w ramach tej usługi hostowanej. Jeden z następujących warunków ogranicza nowe wdrożenie do określonych zasobów platformy Azure. Spróbuj ponownie później lub spróbuj zredukować rozmiar maszyny Wirtualnej lub liczbę wystąpień roli. Alternatywnie Jeśli to możliwe, Usuń wyżej wymienione ograniczenia lub spróbuj przeprowadzić wdrożenie w innym regionie."

**Upgrade_VMSizeNotSupported***

"Nie można uaktualnić wdrożenia. Żądany rozmiar maszyny Wirtualnej XXX nie może być dostępny w zasobach obsługujących istniejące wdrożenie. Spróbuj ponownie później, spróbuj z mniejszym rozmiarem maszyny Wirtualnej lub mniejszą liczbą wystąpień roli lub Utwórz wdrożenie w ramach pustej usługi hostowanej z nową grupą koligacji lub bez powiązania z grupą koligacji."

**GeneralError***

"Serwer napotkał błąd wewnętrzny. Ponów żądanie." Lub "Nie można utworzyć przydział dla usługi".

