---
title: Skalowanie odnajdywania i oceny z migracji Azure | Dokumentacja firmy Microsoft
description: "Opisuje sposób oceny dużej liczby maszyn lokalnych z usługą Azure migracji."
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: dde0d07f-94b7-4b6a-a158-a89aa9324a35
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: 930ec182cf329e7dda072dc49bd7f70abb413f2d
ms.sourcegitcommit: f847fcbf7f89405c1e2d327702cbd3f2399c4bc2
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/28/2017
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Odnajdywanie i ocenić dużych środowiska VMware

W tym artykule opisano sposób oceny dużej liczby lokalnymi maszynami z [migracji Azure](migrate-overview.md). Azure migracji ocenia maszyny, aby sprawdzić, czy są odpowiednie dla migracji na platformie Azure i zapewnia kosztów i zmiany rozmiaru uzyskać szacunkowe wartości do uruchamiania komputera w systemie Azure.

## <a name="prerequisites"></a>Wymagania wstępne

- **VMware**: należy co najmniej jedna maszyna wirtualna oprogramowania VMware znajduje się na hoście ESXi lub klastra z systemem w wersji 5.0 lub nowszej. Host lub klaster musi być zarządzane przez serwer vCenter systemem w wersji 5.5 lub 6.0.
- **Konto vCenter**: potrzebne tylko do odczytu konta z poświadczeniami administratora dla serwera vCenter. Azure migracji używa tego konta do odnajdywanie maszyn wirtualnych.
- **Uprawnienia**: vCenter Server, musisz mieć uprawnienia do tworzenia maszyny Wirtualnej przez zaimportowanie pliku w. Format komórek jajowych.
- **Ustawienia statystyki**: ustawienia statystyki dla serwera vCenter powinien mieć ustawioną poziom 2 lub nowszego, przed rozpoczęciem wdrażania.

## <a name="plan-azure-migrate-projects"></a>Planowanie migracji Azure projektów

Projekt platformy Azure migracji można ocenić maksymalnie 1500 maszyny. Pojedynczy odnajdywania w projekcie może odnajdywać maszyny do 1000.

- Jeśli masz mniej niż 1000 maszyny, aby dowiedzieć się, należy pojedynczego projektu z jednym odnajdywania.
- Jeśli masz między 1000 i 1500 maszyny, należy pojedynczego projektu z dwóch odnajdywania w nim.
- Jeśli masz ponad 1500 urządzeń, należy utworzyć wiele projektów i wykonywać wiele operacje odnajdywania, zgodnie z wymaganiami. Na przykład:
    - Jeśli masz maszyny 3000 może skonfigurować dwa projekty z odnajdywania dwie lub trzy projekty z jednym odnajdywania.
    - Jeśli masz maszyny 5000, można skonfigurować cztery projektów. Dwa z odnajdywania 1500 maszyn i jeden z odnajdywaniem 500 maszyn. Alternatywnie można skonfigurować pięć projektów z jednym odnajdywania w każdej z nich. 
- Po wykonaniu odnajdywania w programie Azure migracji można ustawić zakresu odnajdowania do folderu, datacenter lub klaster VMware.
- Aby zrobić więcej niż jedne operacje odnajdywania, sprawdź w programie vCenter czy ma zostać przeprowadzone odnajdywanie maszyn wirtualnych znajdują się w foldery, centrów danych i klastry, które obsługują ograniczenie 1000 maszyn.
- Firma Microsoft zaleca do celów oceny maszynom między zależności w obrębie tego samego projektu i oceny. Dlatego w programie vCenter, upewnij się, są zależne maszyny w tym samym folderze, datacenter lub klastra na potrzeby oceny.


## <a name="create-a-project"></a>Utwórz projekt

Tworzenie projektu migracji Azure zgodnie z wymaganiami.

1. W portalu Azure kliknij **Utwórz zasób**.
2. Wyszukaj **migracji Azure**i wybierz usługę (**Azure migracji (wersja zapoznawcza)** w wynikach wyszukiwania. Następnie kliknij pozycję **Utwórz**.
3. Określ nazwę projektu i subskrypcji platformy Azure dla projektu.
4. Utwórz nową grupę zasobów.
5. Określ region, w którym można utworzyć projekt, a następnie kliknij przycisk **Utwórz**. Metadane zebranych z lokalnych maszyn wirtualnych są przechowywane w tym regionie.

## <a name="set-up-the-collector-appliance"></a>Konfigurowanie urządzenia modułu zbierającego

Azure migracji tworzy lokalnej maszyny Wirtualnej, znany jako urządzenia modułu zbierającego. Tej maszyny Wirtualnej umożliwia odnalezienie lokalnych maszyn wirtualnych VMware i wysyła metadane dotyczące ich do usługi Azure migracji. Aby skonfigurować urządzenia modułu zbierającego, możesz pobrać. Komórki jajowe pliku i zaimportować go do lokalnego serwera vCenter do utworzenia maszyny Wirtualnej.

### <a name="download-the-collector-appliance"></a>Pobierz moduł zbierający urządzenia

Jeśli masz wiele projektów, wystarczy pobrać urządzenia modułu zbierającego raz z serwerem vCenter. Po pobraniu i konfigurowanie urządzenia, należy ją uruchomić, dla każdego projektu i określić projektu Unikatowy identyfikator i klucz.

1. W projekcie migracji Azure kliknij **wprowadzenie** > **odnajdź & oceny** > **odnajdywanie maszyn**.
2. W **Odkryj maszyny**, kliknij przycisk **Pobierz**, aby pobrać. Plik komórek jajowych.
3. W **skopiuj poświadczenia projektu**, skopiować identyfikator i klucz dla projektu. Należy je podczas konfigurowania modułu zbierającego.

   
### <a name="verify-the-collector-appliance"></a>Sprawdź urządzenia modułu zbierającego

Sprawdź, czy. Plik komórek jajowych jest bezpieczne, przed jego wdrożeniem.

1. Na komputerze, do którego został pobrany plik Otwórz okno polecenia administratora.
2. Uruchom następujące polecenie, aby wygenerować skrótu dla komórek jajowych:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Przykład użycia:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Skrót wygenerowanego powinna odpowiadać te ustawienia.

    **Algorytm** | **Wartość skrótu**
    --- | ---
    MD5 | c283f00f46484bf673dc8383b01d0741 
    SHA1 | 8f49b47f53d051af1780bbc725659ce64e717ed4
    SHA256 | 7aecdbdb2aea712efe99d0c1444503f52d16de5768e783465e226fbbca71501d

## <a name="create-the-collector-vm"></a>Tworzenie modułu zbierającego maszyny Wirtualnej

Zaimportować pobrany plik na serwerze vCenter.

1. W konsoli klienta vSphere kliknij **pliku** > **wdrażanie szablonu OVF**.

    ![Wdrażanie pakietu OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. W Kreatorze wdrażania szablonu OVF > **źródła**, określ lokalizację pliku .ova.
3. W **nazwa** i **lokalizacji**Określ przyjazną nazwę dla modułu zbierającego maszyny Wirtualnej, a obiekt magazynu, w którym będzie hostowana maszyna wirtualna.
5. W **Host/klaster**, określić hosta lub klastra uruchamiania modułu zbierającego maszyny Wirtualnej.
7. W magazynie określ miejsce docelowe przechowywania dla modułu zbierającego maszyny Wirtualnej.
8. W **Format dysku**, określ typ i rozmiar.
9. W **mapowanie sieci**, określ sieć, z którą połączy się modułu zbierającego maszyny Wirtualnej. Sieć wymaga łączności z Internetem, można wysłać metadanych na platformie Azure. 
10. Przejrzyj i Potwierdź ustawienia, a następnie kliknij przycisk **Zakończ**.

## <a name="identify-the-key-and-id-for-each-project"></a>Zidentyfikuj klucz i identyfikator dla każdego projektu

Jeśli masz wiele projektów, upewnij się, zidentyfikuj identyfikator i klucz dla każdej z nich. Należy klucza podczas uruchamiania modułu zbierającego, aby odnaleźć maszyn wirtualnych.

1. W projekcie, kliknij przycisk **wprowadzenie** > **odnajdź & oceny** > **odnajdywanie maszyn**.
2. W **skopiuj poświadczenia projektu**, skopiować identyfikator i klucz dla projektu. 
    ![Identyfikator projektu](./media/how-to-scale-assessment/project-id.png)

## <a name="vcenter-statistics-level-to-collect-the-performance-counters"></a>vCenter statystyki poziomu można zebrać liczników wydajności
Poniżej znajduje się lista liczniki, które zostały zebrane podczas odnajdywania. Te liczniki są domyślnie dostępna na poziomie różnych serwer vCenter. Zaleca się ustawić najwyższy poziom wspólnych (poziom 3) dla poziomu statystyki tak, aby wszystkie liczniki są poprawnie zbierane. Jeśli masz vCenter ustawiony na niższym poziomie tylko kilka liczniki mogą uzyskać całkowicie zbierane z resztą równa 0. Dlatego oceny mogą być wyświetlane niekompletne dane. Poniższej tabeli wymieniono także wyniki oceny, które jest ograniczona, jeśli określony licznik nie jest.

|Licznik                                  |Poziom    |Na poziomie urządzeń  |Ocena wpływu                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.AVERAGE                        | 1       |Nie dotyczy                |Zalecany rozmiar maszyny Wirtualnej i kosztów                    |
|mem.Usage.AVERAGE                        | 1       |Nie dotyczy                |Zalecany rozmiar maszyny Wirtualnej i kosztów                    |
|virtualDisk.read.average                 | 2       |2                 |Rozmiar dysku, kosztem magazynowania i rozmiar maszyny Wirtualnej         |
|virtualDisk.write.average                | 2       |2                 |Rozmiar dysku, kosztem magazynowania i rozmiar maszyny Wirtualnej         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Rozmiar dysku, kosztem magazynowania i rozmiar maszyny Wirtualnej         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Rozmiar dysku, kosztem magazynowania i rozmiar maszyny Wirtualnej         |
|NET.RECEIVED.AVERAGE                     | 2       |3                 |Rozmiar maszyny Wirtualnej i sieci koszt                        |
|NET.Transmitted.AVERAGE                  | 2       |3                 |Rozmiar maszyny Wirtualnej i sieci koszt                        |

> [!WARNING]
> Właśnie ustawione na wyższy poziom statystyki potrwa maksymalnie dziennie, aby wygenerować liczników wydajności. W związku z tym zalecane jest uruchamianie odnajdywania po jednym dniu.

## <a name="run-the-collector-to-discover-vms"></a>Uruchom moduł zbierający do odnajdywanie maszyn wirtualnych

Dla każdego odnajdowania, należy wykonać uruchom moduł zbierający do odnajdywania maszyn wirtualnych w zakresie wymaganych. Uruchom odnajdywanie jeden po drugim. Równoczesne operacje odnajdywania nie są obsługiwane, a poszczególne zadania odnajdywania musi mieć inny zakres.

1. W konsoli klienta vSphere, kliknij prawym przyciskiem myszy maszyny Wirtualnej > **Otwórz konsolę**.
2. Podaj języka, strefa czasowa i hasło preferencje dotyczące urządzenia.
3. Na pulpicie kliknij **uruchom moduł zbierający** skrótów.
4. Moduł zbierający Azure migracji, otwórz **ustawić wstępne**.
    - Zaakceptuj postanowienia licencyjne, a odczytać informacji o innych firm.
    - Moduł zbierający sprawdza, czy maszyna wirtualna ma dostęp do Internetu.
    - Jeśli maszyna wirtualna uzyskuje dostęp do Internetu za pośrednictwem serwera proxy, kliknij przycisk **ustawienia serwera Proxy**i określić adres serwera proxy i port nasłuchiwania. Określ poświadczenia, jeśli serwer proxy wymaga uwierzytelniania.
    - Moduł zbierający sprawdza, czy usługa profilera systemu Windows jest uruchomiona. Usługa jest instalowany domyślnie w module zbierającym maszyny Wirtualnej.
    - Pobierz i zainstaluj VMware PowerCLI.
. W **odnajdywanie maszyn**, wykonaj następujące czynności:
    - Określ nazwę (FQDN) lub adres IP serwera vCenter.
    - W **nazwy użytkownika** i **hasło**, określ poświadczenia konta tylko do odczytu, które moduł zbierający będzie używane do wykrywania maszyn wirtualnych na serwerze vCenter.
    - W **zakresu kolekcji**, wybierz zakres odnajdywania maszyny Wirtualnej. Moduł zbierający można tylko odnajdywanie maszyn wirtualnych w podanym zakresie. Można ustawić zakresu na określony folder, w centrum danych lub w klastrze. Nie powinien on zawierać więcej niż 1000 maszyn wirtualnych. 
    - n **Tag kategorii do grupowania**, wybierz pozycję **Brak**.

        ![Wybierz zakres](./media/how-to-scale-assessment/select-scope.png)

1. W **wybierz projekt**, podaj identyfikator i klucz dla projektu. Jeśli nie skopiuj je, otwórz Azure portal z modułu zbierającego maszyny Wirtualnej. W projekcie **omówienie** kliknij przycisk **odnajdywanie maszyn**i skopiuj wartości.  
W **pełne odnajdowanie**, monitorować proces odnajdywania i sprawdź, że metadane zbierane z maszyn wirtualnych znajduje się w zakresie. Moduł zbierający zapewnia odnajdywania przybliżony czas.


### <a name="verify-vms-in-the-portal"></a>Sprawdź maszyn wirtualnych w portalu

Czas zależy od liczby maszyn wirtualnych odnajdywania odnajdywania. Zwykle 100 maszyn wirtualnych, po zakończeniu przez moduł zbierający, uruchomienie jej trwa około godziny odnajdywania. 

1. Planowanie migracji projektu, kliknij przycisk **Zarządzaj** > **maszyny**.
2. Sprawdź, czy ma zostać przeprowadzone odnajdywanie maszyn wirtualnych są wyświetlane w portalu.


## <a name="next-steps"></a>Następne kroki

- Dowiedz się, jak [Utwórz grupę](how-to-create-a-group.md) do oceny.
- [Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.