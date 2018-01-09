---
title: "Odnajdywanie i ocenić lokalnych maszyn wirtualnych VMware do migracji na platformie Azure za pomocą migracji Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób odnajdywania i oceny lokalnych maszyn wirtualnych VMware do migracji na platformę Azure przy użyciu usługi Azure migracji."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: a5019d3f729f2efbd01fca021b0089c7f99b0014
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Odnajdywanie i ocenić lokalnych maszyn wirtualnych VMware do migracji do usługi Azure

[Migracji Azure](migrate-overview.md) usług ocenia obciążeń lokalnych do migracji do usługi Azure.

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie projektu platformy Azure migracji.
> * Skonfiguruj na lokalnej maszynie wirtualnej modułu zbierającego (VM), do odnajdywania lokalnych maszyn wirtualnych VMware do oceny.
> * Grupy maszyn wirtualnych i Utwórz ocenę.


Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/pricing/free-trial/).


## <a name="prerequisites"></a>Wymagania wstępne

- **VMware**: maszyny wirtualne, które mają zostać poddane migracji muszą być zarządzane przy użyciu vCenter Server uruchomionej wersji 5.5, 6.0 lub 6.5. Ponadto należy jeden ESXi hosta wersji 5.0 lub nowszej w celu wdrożenia modułu zbierającego maszyny Wirtualnej. 
 
> [!NOTE]
> Obsługa funkcji Hyper-V znajduje się w naszej plan i zostanie wkrótce włączona. 

- **konto serwera vCenter**: konieczne konto tylko do odczytu, aby uzyskać dostęp serwer vCenter. Azure migracji używa tego konta do odnajdywania lokalnych maszyn wirtualnych.
- **Uprawnienia**: w programie Vcenter Server, musisz mieć uprawnienia do tworzenia maszyny Wirtualnej przez zaimportowanie pliku w. Format komórek jajowych. 
- **Ustawienia statystyki**: ustawienia statystyki dla serwera vCenter powinien być ustawiony na poziomie 3, przed rozpoczęciem wdrażania. Jeśli niższy niż poziom 3, oceny będą działać, ale nie jest zebrać danych wydajności magazynu i sieci. Żądany rozmiar zalecenia w tym przypadku zostanie to zrobione oparte na dane wydajności dotyczące procesora CPU i pamięci i konfigurację danych kart dysku i sieci. 

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal
Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Utwórz projekt

1. W portalu Azure kliknij **Utwórz zasób**.
2. Wyszukaj **migracji Azure**i wybierz usługę **Azure migracji (wersja zapoznawcza)** w wynikach wyszukiwania. Następnie kliknij pozycję **Utwórz**.
3. Określ nazwę projektu i subskrypcji platformy Azure dla projektu.
4. Utwórz nową grupę zasobów.
5. Określ lokalizację, w której można utworzyć projekt, a następnie kliknij przycisk **Utwórz**. Projekt platformy Azure migracji można utworzyć tylko w regionu zachodnie centralnej nam dla tej wersji zapoznawczej. Można jednak nadal planowania migracji dla dowolnego obiektu docelowego lokalizacji platformy Azure. Lokalizacja określona dla projektu jest używana tylko do przechowywania metadanych zebranych z lokalnych maszyn wirtualnych. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Pobierz moduł zbierający urządzenia

Azure migracji tworzy lokalnej maszyny Wirtualnej, znany jako urządzenia modułu zbierającego. Tej maszyny Wirtualnej umożliwia odnalezienie lokalnych maszyn wirtualnych VMware i wysyła metadane dotyczące ich do usługi Azure migracji. Aby skonfigurować urządzenia modułu zbierającego, możesz pobrać. Komórki jajowe pliku i zaimportować go do lokalnego serwera vCenter do utworzenia maszyny Wirtualnej.

1. W projekcie migracji Azure kliknij **wprowadzenie** > **odnajdź & oceny** > **odnajdywanie maszyn**.
2. W **Odkryj maszyny**, kliknij przycisk **Pobierz**, aby pobrać. Plik komórek jajowych.
3. W **skopiuj poświadczenia projektu**, skopiować identyfikator i klucz. Należy je podczas konfigurowania modułu zbierającego.

    ![Pobierz plik .ova](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Sprawdź urządzenia modułu zbierającego

Sprawdź, czy. Plik komórek jajowych jest bezpieczne, przed jego wdrożeniem.

1. Na komputerze, do którego został pobrany plik Otwórz okno polecenia administratora.
2. Uruchom następujące polecenie, aby wygenerować skrótu dla komórek jajowych:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Przykład użycia:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Skrót wygenerowanego powinna odpowiadać te ustawienia.
    
    Dla wersji komórek jajowych 1.0.8.49
    **Algorytm** | **Wartość skrótu**
    --- | ---
    MD5 | 8779eea842a1ac465942295c988ac0c7 
    SHA1 | c136c52a0f785e1fd98865e16479dd103704887d
    SHA256 | 5143b1144836f01dd4eaf84ff94bc1d2c53f51ad04b1ca43ade0d14a527ac3f9

    W wersji komórek jajowych 1.0.8.40:

    **Algorytm** | **Wartość skrótu**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>Tworzenie modułu zbierającego maszyny Wirtualnej

Pobrany plik należy zaimportować do programu vCenter Server.

1. W konsoli klienta vSphere kliknij **pliku** > **wdrażanie szablonu OVF**.

    ![Wdrażanie pakietu OVF](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. W Kreatorze wdrażania szablonu OVF > **źródła**, określ lokalizację pliku .ova.
3. W **nazwa** i **lokalizacji**Określ przyjazną nazwę dla modułu zbierającego maszyny Wirtualnej, a obiekt magazynu, w którym będzie hostowana maszyna wirtualna.
5. W **Host/klaster**, określić hosta lub klastra uruchamiania modułu zbierającego maszyny Wirtualnej.
7. W magazynie określ miejsce docelowe przechowywania dla modułu zbierającego maszyny Wirtualnej.
8. W **Format dysku**, określ typ i rozmiar.
9. W **mapowanie sieci**, określ sieć, z którą połączy się modułu zbierającego maszyny Wirtualnej. Sieć wymaga łączności z Internetem, można wysłać metadanych na platformie Azure. 
10. Przejrzyj i Potwierdź ustawienia, a następnie kliknij przycisk **Zakończ**.

## <a name="run-the-collector-to-discover-vms"></a>Uruchom moduł zbierający do odnajdywanie maszyn wirtualnych

1. W konsoli klienta vSphere, kliknij prawym przyciskiem myszy maszyny Wirtualnej > **Otwórz konsolę**.
2. Podaj języka, strefa czasowa i hasło preferencje dotyczące urządzenia.
3. Na pulpicie kliknij **uruchom moduł zbierający** skrótów.
4. Moduł zbierający Azure migracji, otwórz **konfigurowanie wymagań wstępnych**.
    - Zaakceptuj postanowienia licencyjne, a odczytać informacji o innych firm.
    - Moduł zbierający sprawdza, czy maszyna wirtualna ma dostęp do Internetu.
    - Jeśli maszyna wirtualna uzyskuje dostęp do Internetu za pośrednictwem serwera proxy, kliknij przycisk **ustawienia serwera Proxy**i określić adres serwera proxy i port nasłuchiwania. Określ poświadczenia, jeśli serwer proxy wymaga uwierzytelniania.

    > [!NOTE]
    > Adres serwera proxy musi być wprowadzane w postaci http://ProxyIPAddress lub http://ProxyFQDN. Obsługiwane jest tylko serwer proxy HTTP.

    - Moduł zbierający sprawdza, czy collectorservice jest uruchomiona. Usługa jest instalowany domyślnie w module zbierającym maszyny Wirtualnej.
    - Pobierz i zainstaluj VMware PowerCLI.

5. W **Określ szczegóły serwera vCenter**, wykonaj następujące czynności:
    - Określ nazwę (FQDN) lub adres IP serwera vCenter.
    - W **nazwy użytkownika** i **hasło**, określ poświadczenia konta tylko do odczytu, które moduł zbierający będzie używane do wykrywania maszyn wirtualnych na serwerze vCenter.
    - W **zakresu kolekcji**, wybierz zakres odnajdywania maszyny Wirtualnej. Moduł zbierający można tylko odnajdywanie maszyn wirtualnych w podanym zakresie. Można ustawić zakresu na określony folder, w centrum danych lub w klastrze. Nie powinien on zawierać więcej niż 1000 maszyn wirtualnych. 

6. W **Określ migrację**, określ identyfikator projektu migracji Azure i klucza skopiowany z portalu. Jeśli nie skopiuj je, otwórz Azure portal z modułu zbierającego maszyny Wirtualnej. W projekcie **omówienie** kliknij przycisk **odnajdywanie maszyn**i skopiuj wartości.  
7. W **wyświetlić postęp kolekcji**, monitorować odnajdywania i sprawdź, że metadane zbierane z maszyn wirtualnych znajduje się w zakresie. Moduł zbierający zapewnia odnajdywania przybliżony czas.

> [!NOTE]
> Moduł zbierający obsługuje tylko "Angielski (Stany Zjednoczone)" jako język systemu operacyjnego i język interfejsu modułu zbierającego. Obsługę innych języków będzie dostępna wkrótce.


### <a name="verify-vms-in-the-portal"></a>Sprawdź maszyn wirtualnych w portalu

Czas zależy od liczby maszyn wirtualnych odnajdywania odnajdywania. Zwykle 100 maszyn wirtualnych, po zakończeniu przez moduł zbierający, uruchomienie jej trwa około godziny odnajdywania. 

1. Planowanie migracji projektu, kliknij przycisk **Zarządzaj** > **maszyny**.
2. Sprawdź, czy ma zostać przeprowadzone odnajdywanie maszyn wirtualnych są wyświetlane w portalu.


## <a name="create-and-view-an-assessment"></a>Można tworzyć i wyświetlać oceny

Po odnalezieniu maszyn wirtualnych, możesz grupować i utworzyć ocenę. 

1. W projekcie **omówienie** kliknij przycisk **+ Utwórz oceny**.
2. Kliknij przycisk **Wyświetl wszystkie** do przeglądania właściwości oceny.
3. Utwórz grupy i określ nazwę grupy.
4. Wybierz maszyny, które chcesz dodać do grupy.
5. Kliknij przycisk **utworzyć oceny**, aby utworzyć grupy i oceny.
6. Po utworzeniu oceny wyświetlić w **omówienie** > **pulpitu nawigacyjnego**.
7. Kliknij przycisk **wyeksportować oceny**, aby pobrać go jako plik programu Excel.

### <a name="sample-assessment"></a>Ocena próbki

Oto przykładowy raport oceny. Zawiera ona informacje dotyczące tego, czy maszyny wirtualne są zgodne platformy Azure i szacowane koszty miesięczne. 

![Raport oceny](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Gotowość na platformę Azure

Ten widok przedstawia stan gotowości dla każdego komputera.

- Dla maszyn wirtualnych, które są gotowe migracja Azure zaleca dla rozmiaru maszyny Wirtualnej na platformie Azure.
- Dla maszyn wirtualnych, które nie są gotowe Azure migracji zawiera informacje o przyczynie i proponuje czynności korygujące.
- Azure migracji sugeruje narzędzia, których można użyć do migracji. Jeśli komputer jest odpowiedni dla migracji przyrostu i shift, usługa Azure Site Recovery jest zalecane. Jeśli jest to maszyna bazy danych, zaleca się usługa migracji bazy danych Azure.

  ![Oceny gotowości](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Szacowany koszt miesięczny

Ten widok przedstawia całkowity obliczeniowych i przestrzeni magazynowej uruchomionych maszyn wirtualnych na platformie Azure oraz szczegóły dotyczące poszczególnych maszyn. Szacowanie kosztów są obliczane przy użyciu zalecenia na podstawie rozmiaru dla komputera i jego dysków i właściwości oceny. 

> [!NOTE]
> Szacowania kosztów podał Azure migracji jest przeznaczony do uruchamiania maszyn wirtualnych do lokalnego jako Azure infrastruktura jako usługa (IaaS) maszyn wirtualnych. Azure migracji nie należy wziąć pod uwagę wszystkie platformy jako usługa (PaaS) lub oprogramowania jako koszty usługi (SaaS). 

Szacowane koszty miesięczne obliczeniowych i magazynu są agregowane dla wszystkich maszyn wirtualnych w grupie. 

![Ocena koszt maszyny Wirtualnej](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

Można następnie przejść do szczegółów można znaleźć określonej maszyny.

![Ocena koszt maszyny Wirtualnej](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Kolejne kroki

- [Dowiedz się](how-to-scale-assessment.md) jak odnajdywać i oceny dużych środowisku VMware.
- Informacje o tworzeniu grup oceny wysokiego zaufania przy użyciu [mapowania zależności maszyny](how-to-create-group-machine-dependencies.md)
- [Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
