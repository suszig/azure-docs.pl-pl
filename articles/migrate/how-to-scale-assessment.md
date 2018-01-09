---
title: "Skalowanie odnajdywania i ocena za pomocą migracji Azure | Dokumentacja firmy Microsoft"
description: "Opisuje sposób oceny dużej liczby komputerów lokalnych za pomocą usługi Azure migracji."
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: 67661e03e65cde3ec2f1aafd5ef755899cf0c77b
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Odnajdywanie i ocenić dużych środowiska VMware

W tym artykule opisano sposób oceny dużej liczby lokalnych maszyn wirtualnych (VM) przy użyciu [migracji Azure](migrate-overview.md). Azure migracji ocenia maszyny, aby sprawdzić, czy są one odpowiednie dla migracji na platformie Azure. Usługa zawiera kosztów i zmiany rozmiaru uzyskać szacunkowe wartości do uruchamiania na komputerach w systemie Azure.

## <a name="prerequisites"></a>Wymagania wstępne

- **VMware**: maszyny wirtualne, które mają zostać poddane migracji muszą być zarządzane przez vCenter Server 5.5, 6.0 lub 6.5. Ponadto należy jeden ESXi hosta wersji 5.0 lub nowszej w celu wdrożenia modułu zbierającego maszyny Wirtualnej.
- **Konto vCenter**: konieczne konto tylko do odczytu, aby uzyskać dostęp serwer vCenter. Azure migracji używa tego konta do odnajdywania lokalnych maszyn wirtualnych.
- **Uprawnienia**: W programie vCenter Server, musisz mieć uprawnienia do tworzenia maszyny Wirtualnej przez zaimportowanie pliku w formacie komórek jajowych.
- **Ustawienia statystyki**: ustawienia statystyki dla serwera vCenter powinien być ustawiony na poziomie 3, przed rozpoczęciem wdrażania. Jeśli poziom jest niższy niż 3, oceny będą działać, ale nie można zebrać danych wydajności magazynu i sieci. Zalecenia dotyczące rozmiaru w takim przypadku będzie opierać się na dane wydajności dotyczące Procesora i pamięci, a dane konfiguracji kart dysku i sieci.

## <a name="plan-azure-migrate-projects"></a>Planowanie migracji Azure projektów

Zaplanuj odnajdywania i oceny na podstawie limitów następujące:

| **Jednostki** | **Limit komputera** |
| ---------- | ----------------- |
| Project    | 1,500              | 
| Odnajdowanie  | 1000              |
| Ocena | 400               |

- Jeśli masz mniej niż 400 maszyny do odnajdywania i oceny należy pojedynczego projektu i jednym odnajdywania. W zależności od wymagań należy ocenić wszystkie maszyny w pojedynczej oceny lub podzielone maszyn ocen wielu. 
- Jeśli masz maszyny 400 do 1000, aby dowiedzieć się, należy pojedynczego projektu z jednym odnajdywania. Ale konieczne jest posiadanie wielu oceny w celu oceny tych maszyn, ponieważ pojedynczej oceny może przechowywać maksymalnie 400 maszyny.
- Jeśli masz maszyny 1,001 do 1500 należy pojedynczego projektu z dwóch odnajdywania w nim.
- Jeśli masz ponad 1500 urządzeń, należy utworzyć wiele projektów i wykonywać wiele operacje odnajdywania, zgodnie z wymaganiami. Na przykład:
    - Jeśli masz maszyny 3000, można skonfigurować dwa projekty z odnajdywania dwie lub trzy projekty z jednym odnajdywania.
    - Jeśli masz maszyny 5000, można skonfigurować cztery projekty: dwie o odnajdywania 1500 maszyn i jeden z odnajdywaniem 500 maszyn. Alternatywnie można skonfigurować pięć projektów z jednym odnajdywania w każdej z nich. 

## <a name="plan-multiple-discoveries"></a>Planowanie wielu odnajdywania

Można użyć tego samego modułu zbierającego migracji Azure celu odkrycia wiele do jednego lub więcej projektów. Należy pamiętać, te zagadnienia dotyczące planowania:
 
- Po wykonaniu odnajdywania przy użyciu modułu zbierającego Azure migracji można ustawić zakresu odnajdowania do folderu na serwerze vCenter, datacenter, klastra lub hosta.
- Aby zrobić więcej niż jedne operacje odnajdywania, sprawdź, czy serwer, który ma zostać przeprowadzone odnajdywanie maszyn wirtualnych znajdują się w foldery, centrów danych, klastrów i hostów, które obsługują ograniczenie 1000 maszyny vCenter.
- Firma Microsoft zaleca do celów oceny maszyn z zależnościami, w tym samym projekcie i oceny. W programie vCenter Server upewnij się, że zależnych maszyny są w tym samym folderze, datacenter lub klastra na potrzeby oceny.


## <a name="create-a-project"></a>Utwórz projekt

Tworzenie projektu migracji Azure zgodnie z wymaganiami:

1. W portalu Azure wybierz **Utwórz zasób**.
2. Wyszukaj **migracji Azure**i wybierz usługę **Azure migracji (wersja zapoznawcza)** w wynikach wyszukiwania. Następnie wybierz przycisk **Utwórz**.
3. Określ nazwę projektu i subskrypcji platformy Azure dla projektu.
4. Utwórz nową grupę zasobów.
5. Określ lokalizację, w której chcesz utworzyć projekt, a następnie wybierz **Utwórz**. Należy pamiętać, że nadal można ocenić maszyn wirtualnych na inną lokalizację docelową. Lokalizacja określona dla projektu jest używany do przechowywania metadanych zebranych z lokalnych maszyn wirtualnych.

## <a name="set-up-the-collector-appliance"></a>Konfigurowanie urządzenia modułu zbierającego

Azure migracji tworzy lokalnej maszyny Wirtualnej, znany jako urządzenia modułu zbierającego. Tej maszyny Wirtualnej umożliwia odnalezienie lokalnych maszyn wirtualnych VMware, a następnie wysyła metadane dotyczące ich do usługi Azure migracji. Aby skonfigurować urządzenia modułu zbierającego, Pobierz plik komórek jajowych i zaimportować go do lokalnego wystąpienia serwera vCenter.

### <a name="download-the-collector-appliance"></a>Pobierz moduł zbierający urządzenia

Jeśli masz wiele projektów, należy pobrać urządzenia modułu zbierającego tylko raz do serwera vCenter. Po pobraniu i konfigurowanie urządzenia zostanie uruchomiony dla każdego projektu, a następnie określ projektu Unikatowy identyfikator i klucz.

1. W projekcie migracji Azure wybierz **wprowadzenie** > **odnajdź & oceny** > **odnajdywanie maszyn**.
2. W **Odkryj maszyny**, wybierz pozycję **Pobierz**, aby pobrać plik komórek jajowych.
3. W **skopiuj poświadczenia projektu**, skopiować identyfikator i klucz dla projektu. Należy je podczas konfigurowania modułu zbierającego.

   
### <a name="verify-the-collector-appliance"></a>Sprawdź urządzenia modułu zbierającego

Sprawdź, czy plik komórek jajowych jest bezpieczne, przed przystąpieniem do wdrażania:

1. Na komputerze, do którego został pobrany plik Otwórz okno polecenia administratora.
2. Uruchom następujące polecenie, aby wygenerować skrótu dla komórek jajowych:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Przykład użycia:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. Upewnij się, czy wygenerowane skrótu zgodna następujące ustawienia.
 
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

Pobrany plik należy zaimportować do programu vCenter Server:

1. W konsoli klienta vSphere wybierz **pliku** > **wdrażanie szablonu OVF**.

    ![Wdrażanie pakietu OVF](./media/how-to-scale-assessment/vcenter-wizard.png)

2. W Kreatorze wdrażania szablonu OVF > **źródła**, określ lokalizację pliku komórek jajowych.
3. W **nazwa** i **lokalizacji**Określ przyjazną nazwę dla modułu zbierającego maszyny Wirtualnej, a obiekt magazynu, w którym będzie hostowana maszyna wirtualna.
5. W **Host/klaster**, określić hosta lub klastra uruchamiania modułu zbierającego maszyny Wirtualnej.
7. W magazynie określ miejsce docelowe przechowywania dla modułu zbierającego maszyny Wirtualnej.
8. W **Format dysku**, określ typ i rozmiar.
9. W **mapowanie sieci**, określ sieć, z którą połączy się modułu zbierającego maszyny Wirtualnej. Sieci wymaga łączności z Internetem można wysłać metadanych na platformie Azure. 
10. Przejrzyj i Potwierdź ustawienia, a następnie wybierz **Zakończ**.

## <a name="identify-the-id-and-key-for-each-project"></a>Określ identyfikator i klucz dla każdego projektu

Jeśli masz wiele projektów, pamiętaj zidentyfikować identyfikator i klucz dla każdej z nich. Należy klucza podczas uruchamiania modułu zbierającego, aby odnaleźć maszyn wirtualnych.

1. W projekcie, wybierz **wprowadzenie** > **odnajdź & oceny** > **odnajdywanie maszyn**.
2. W **skopiuj poświadczenia projektu**, skopiować identyfikator i klucz dla projektu. 
    ![Skopiuj poświadczenia projektu](./media/how-to-scale-assessment/copy-project-credentials.png)

## <a name="set-the-vcenter-statistics-level"></a>Ustaw poziom statystyki vCenter
Poniżej znajduje się lista liczników wydajności, które zostały zebrane podczas odnajdywania. Te liczniki są domyślnie dostępne na różnych poziomach w programie vCenter Server. 

Zalecane ustawienie najwyższego poziomu wspólnych (3) dla poziomu statystyk, tak aby wszystkie liczniki są poprawnie zbierane. Jeśli masz vCenter ustawiony na niższym poziomie tylko kilka liczniki mogą być zbierane całkowicie z resztą równa 0. Ocena następnie mogą być wyświetlane niekompletne dane. 

W poniższej tabeli wymieniono także wyniki oceny, które zostaną zmienione, jeśli nie ma określonego licznika.

|Licznik                                  |Poziom    |Poziom na urządzenie  |Ocena wpływu                               |
|-----------------------------------------|---------|------------------|------------------------------------------------|
|CPU.Usage.AVERAGE                        | 1       |Nie dotyczy                |Zalecany rozmiar maszyny Wirtualnej i kosztów                    |
|mem.Usage.AVERAGE                        | 1       |Nie dotyczy                |Zalecany rozmiar maszyny Wirtualnej i kosztów                    |
|virtualDisk.read.average                 | 2       |2                 |Rozmiar dysku, kosztem magazynowania i rozmiar maszyny Wirtualnej         |
|virtualDisk.write.average                | 2       |2                 |Rozmiar dysku, kosztem magazynowania i rozmiar maszyny Wirtualnej         |
|virtualDisk.numberReadAveraged.average   | 1       |3                 |Rozmiar dysku, kosztem magazynowania i rozmiar maszyny Wirtualnej         |
|virtualDisk.numberWriteAveraged.average  | 1       |3                 |Rozmiar dysku, kosztem magazynowania i rozmiar maszyny Wirtualnej         |
|NET.RECEIVED.AVERAGE                     | 2       |3                 |Koszt rozmiar i sieci maszyny Wirtualnej                        |
|NET.Transmitted.AVERAGE                  | 2       |3                 |Koszt rozmiar i sieci maszyny Wirtualnej                        |

> [!WARNING]
> Jeśli właśnie ustawione na wyższy poziom statystyki potrwa na dzień do generowania liczników wydajności. Tak firma Microsoft zaleca uruchomienia odnajdywania po jednym dniu.

## <a name="run-the-collector-to-discover-vms"></a>Uruchom moduł zbierający do odnajdywanie maszyn wirtualnych

Dla każdego odnajdowania, które należy wykonać możesz uruchomić moduł zbierający do odnajdywanie maszyn wirtualnych w zakresie wymaganych. Uruchom odnajdywanie jeden po drugim. Równoczesne operacje odnajdywania nie są obsługiwane, a poszczególne zadania odnajdywania musi mieć inny zakres.

1. W konsoli klienta vSphere, kliknij prawym przyciskiem myszy maszyny Wirtualnej > **Otwórz konsolę**.
2. Podaj języka, strefa czasowa i hasło preferencje dotyczące urządzenia.
3. Na pulpicie, wybierz **uruchom moduł zbierający** skrótów.
4. Moduł zbierający migracji Azure Otwórz **konfigurowanie wymagań wstępnych** , a następnie:

   a. Zaakceptuj postanowienia licencyjne, a odczytać informacji o innych firm.

   Moduł zbierający sprawdza, czy maszyna wirtualna ma dostęp do Internetu.
   
   b. Jeśli maszyna wirtualna uzyskuje dostęp do Internetu za pośrednictwem serwera proxy, zaznacz **ustawienia serwera Proxy**i określić adres serwera proxy i port nasłuchiwania. Określ poświadczenia, jeśli serwer proxy wymaga uwierzytelniania.

   Moduł zbierający sprawdza, czy działa usługa modułu zbierającego. Usługa jest instalowany domyślnie w module zbierającym maszyny Wirtualnej.

   d. Pobierz i zainstaluj VMware PowerCLI.

5. W **Określ szczegóły serwera vCenter**, wykonaj następujące czynności:
    - Określ nazwę (FQDN) lub adres IP serwera vCenter.
    - W **nazwy użytkownika** i **hasło**, określ poświadczenia konta tylko do odczytu, które moduł zbierający będzie używane do wykrywania maszyn wirtualnych w programie vCenter Server.
    - W **, wybierz zakres**, wybierz zakres odnajdywania maszyny Wirtualnej. Moduł zbierający może odnajdywać tylko maszyny wirtualne w podanym zakresie. Można ustawić zakresu na określony folder, w centrum danych lub w klastrze. Nie powinien on zawierać więcej niż 1000 maszyn wirtualnych. 

6. W **Określ migrację**, podaj identyfikator i klucz dla projektu. Jeśli nie zostaną skopiowane, otwórz Azure portal z modułu zbierającego maszyny Wirtualnej. Do projektu **omówienie** wybierz pozycję **odnajdywanie maszyn** i skopiuj wartości.  
7. W **wyświetlić postęp kolekcji**, monitorować proces odnajdywania i sprawdź, że metadane zbierane z maszyn wirtualnych znajduje się w zakresie. Moduł zbierający zapewnia odnajdywania przybliżony czas.


### <a name="verify-vms-in-the-portal"></a>Sprawdź maszyn wirtualnych w portalu

Czas zależy od liczby maszyn wirtualnych odnajdywania odnajdywania. Zazwyczaj 100 maszyn wirtualnych, odnajdywanie zakończy się wokół godzinę po zakończeniu modułu zbierającego. 

1. W projekcie planowania migracji, zaznacz **Zarządzaj** > **maszyny**.
2. Sprawdź, czy ma zostać przeprowadzone odnajdywanie maszyn wirtualnych są wyświetlane w portalu.


## <a name="next-steps"></a>Kolejne kroki

- Dowiedz się, jak [Utwórz grupę](how-to-create-a-group.md) do oceny.
- [Dowiedz się więcej](concepts-assessment-calculation.md) o obliczania oceny.
