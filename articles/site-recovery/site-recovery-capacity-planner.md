---
title: "Szacowanie zdolności replikacji na platformie Azure | Dokumentacja firmy Microsoft"
description: "Niniejszy artykuł służy oszacowania wydajności podczas replikowania z usługi Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: jwhit
editor: 
ms.assetid: 0a1cd8eb-a8f7-4228-ab84-9449e0b2887b
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: 134e17ebda3105be2b53d072fdef7aeda4a98bde
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="plan-capacity-for-protecting-virtual-machines-and-physical-servers-in-azure-site-recovery"></a>Planowanie pojemności do ochrony maszyn wirtualnych i serwerów fizycznych w usłudze Azure Site Recovery

Narzędzie Azure Site Recovery Capacity Planner pomaga ustalić wymagania pojemności podczas replikowania maszyn wirtualnych funkcji Hyper-V, maszyn wirtualnych VMware oraz serwerach fizycznych systemu Windows i Linux, z usługą Azure Site Recovery.

Capacity Planner odzyskiwania lokacji umożliwia analizowanie środowiska źródłowego i obciążeń, wymagania dotyczące przepustowości szacowania i zasobów serwera, które będą potrzebne do lokalizacji źródłowej i zasobów (maszyn wirtualnych i magazynu itp.), wymagających w lokalizacji docelowej.

Narzędzie można uruchomić na kilka metod:

* **Planowanie szybkie**: Uruchom narzędzie w tym trybie można pobrać projekcje sieci i serwera oparte na średnich liczbach maszyn wirtualnych, dysków, magazynu i szybkość zmian.
* **Szczegółowe planowanie**: Uruchom narzędzie w tym trybie i podaj szczegóły poszczególnych obciążeń na poziomie maszyny Wirtualnej. Analizowanie zgodności maszyny Wirtualnej i uzyskać projekcje sieci i serwera.

## <a name="before-you-start"></a>Przed rozpoczęciem


1. Zbierz informacje o środowisku, w tym o maszynach wirtualnych, dysków dla maszyny Wirtualnej, magazynu na dysku.
2. Określ częstotliwość codziennych zmian (przenoszenia) dla replikowanych danych. W tym celu:

   * Jeśli przeprowadzasz replikację maszyn wirtualnych funkcji Hyper-V, następnie pobierz [narzędzia do planowania pojemności funkcji Hyper-V](https://www.microsoft.com/download/details.aspx?id=39057) uzyskać szybkość zmian. [Dowiedz się więcej](site-recovery-capacity-planning-for-hyper-v-replication.md) o tym narzędziu. Zaleca się uruchomić to narzędzie dłużej niż przez tydzień do przechwytywania średnie.
   * Jeśli replikujesz maszyny wirtualne VMware, należy użyć [Azure lokacji odzyskiwania wdrożenia Planistę](./site-recovery-deployment-planner.md) do ustalenia współczynnika zmian.
   * W przypadku replikowania serwerów fizycznych, należy oszacować ręcznie.

## <a name="run-the-quick-planner"></a>Uruchom szybki planowania
1. Pobierz i Otwórz [usługi Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) narzędzia. Musisz uruchomić makra, dlatego wybierz włączyć edycji i włączyć zawartości po wyświetleniu monitu.
2. W **wybierz typ planner** wybierz **szybkie Planner** w polu listy.

   ![Wprowadzenie](./media/site-recovery-capacity-planner/getting-started.png)
3. W **Capacity Planner** arkusza, wprowadź wymagane informacje. Należy podać we wszystkich polach zaznaczona kółkiem na czerwono na poniższym zrzucie ekranu.

   * W **wybierz scenariusz**, wybierz **funkcji Hyper-V w systemie Azure** lub **VMware/fizycznych do platformy Azure**.
   * W **średnie dzienne zmienić częstotliwość (%)**, umieść informacje należy zebrać, za pomocą [narzędzia do planowania pojemności funkcji Hyper-V](site-recovery-capacity-planning-for-hyper-v-replication.md) lub [Azure lokacji odzyskiwania wdrożenia Planistę](./site-recovery-deployment-planner.md).  
   * **Kompresja** ma zastosowanie tylko do kompresji oferowane podczas replikowania maszyn wirtualnych VMware lub serwerów fizycznych do platformy Azure. Firma Microsoft oszacować 30% lub więcej, ale można zmodyfikować ustawienia zgodnie z wymaganiami. Do replikowania maszyn wirtualnych funkcji Hyper-V do platformy Azure kompresji, można użyć urządzenia innych firm, takich jak Riverbed.
   * W **przechowywania danych wejściowych**, określ, jak długo mają być przechowywane repliki. Jeśli przeprowadzasz replikację VMware lub serwerów fizycznych, wprowadź wartość w dniach. Jeśli przeprowadzasz replikację funkcji Hyper-V, określ czas w godzinach.
   * W **liczbę godzin, w których replikacji początkowej dla partii maszyn wirtualnych należy wykonać** i **liczbę maszyn wirtualnych na partię replikacji początkowej**, wprowadzania ustawień, które są używane do oblicza wymagania dotyczące replikacji początkowej.  Podczas wdrażania usługi Site Recovery należy przesłać początkowej cały zestaw danych.

   ![Dane wejściowe](./media/site-recovery-capacity-planner/inputs.png)
4. Po umieszczeniu już wartości dla środowiska źródłowego, wyświetlane dane wyjściowe obejmują:

   * **Przepustowość wymagana dla replikacji różnicowej** (MB/s). Przepustowość sieci dla replikacja różnicowa jest obliczany średni dzienny częstotliwości zmian danych.
   * **Przepustowość wymagana w przypadku replikacji początkowej** (MB/s). Przepustowość sieci dla replikacji początkowej jest obliczana o wartości początkowej replikacji, które należy umieścić w.
   * **Magazynu (w GB) wymagany** jest całkowita magazynu platformy Azure wymagane.
   * **Łączna liczba IOPS na kontach magazynu w warstwie standardowa** jest obliczana na podstawie rozmiaru jednostki IOPS 8 KB na kontach magazynu w warstwie standardowa całkowita.  Dla Planner szybki numer jest obliczany na podstawie wszystkich dysków maszyn wirtualnych źródła i codzienne dane zmienić częstotliwość. Dla Planner szczegółowe numer jest obliczany na podstawie całkowitą liczbę maszyn wirtualnych, które są mapowane na standardowe maszynach wirtualnych platformy Azure, a dane zmienić częstotliwość na tych maszynach wirtualnych.
   * **Liczba kont magazynu w warstwie standardowa** zawiera całkowitą liczbę kont magazynu w warstwie standardowa niezbędne do ochrony maszyn wirtualnych. Konta standardowe magazynu może zawierać maksymalnie 20000 IOPS na wszystkich maszynach wirtualnych magazynu w warstwie standardowa, i obsługiwane są maksymalnie 500 iops dla każdego dysku.
   * **Liczba obiektów blob dysków wymaganych** zwraca liczbę dysków, które zostaną utworzone w magazynie Azure.
   * **Liczba kont magazynu premium wymagane** zawiera całkowitą liczbę kont magazynu premium, niezbędne do ochrony maszyn wirtualnych. Źródło maszyny Wirtualnej o wysokiej IOPS (większe niż 20000) musi mieć konto magazynu w warstwie premium. Konta premium magazynu może zawierać maksymalnie 80000 IOPS.
   * **Łączna liczba IOPS na magazyn w warstwie premium** jest obliczana na podstawie rozmiaru jednostki IOPS 256 KB na kontach magazynu premium całkowitej.  Dla Planner szybki numer jest obliczany na podstawie wszystkich dysków maszyn wirtualnych źródła i codzienne dane zmienić częstotliwość. Dla Planner szczegółowe ta liczba jest obliczana na podstawie całkowitej liczby maszyn wirtualnych, które są mapowane na maszynie Wirtualnej platformy Azure (seria DS i GS) w warstwie premium, a dane zmienić częstotliwość na tych maszynach wirtualnych.
   * **Liczba serwerów konfiguracji wymaganych** pokazuje, ile serwerów konfiguracji są wymagane do wdrożenia.
   * **Liczby wymaganych serwerów dodatkowych procesów** pokazuje, czy są wymagane, oprócz serwera przetwarzania, który jest uruchomiony na serwerze konfiguracji domyślnie serwery dodatkowych procesów.
   * **dodatkowy magazyn 100% w źródle** pokazuje, czy w lokalizacji źródłowej jest wymagany dodatkowy magazyn.

   ![Dane wyjściowe](./media/site-recovery-capacity-planner/output.png)

## <a name="run-the-detailed-planner"></a>Uruchom szczegółowy planowania

1. Pobierz i Otwórz [usługi Azure Site Recovery Capacity Planner](http://aka.ms/asr-capacity-planner-excel) narzędzia. Musisz uruchomić makra, dlatego wybierz włączyć edycji i włączyć zawartości po wyświetleniu monitu.
2. W **wybierz typ planner**, wybierz pozycję **szczegółowe Planner** w polu listy.

   ![Wprowadzenie](./media/site-recovery-capacity-planner/getting-started-2.png)
3. W **kwalifikacji obciążenia** arkusza, wprowadź wymagane informacje. Należy podać wszystkie pola oznaczone.

   * W **rdzenie procesora**, określ liczbę rdzeni na serwerze źródłowym.
   * W **alokacji pamięci w Megabajtach**, określ rozmiar pamięci RAM z serwera źródłowego.
   * **Liczbę kart sieciowych**, określ liczbę kart sieciowych na serwerze źródłowym.
   * W **całkowity magazynu (w GB)**, określ całkowity rozmiar pamięci maszyny Wirtualnej. Na przykład jeśli serwer źródłowy ma 3 dysk 500 GB, całkowitego rozmiaru magazynu jest 1500 GB.
   * W **liczba dysków dołączonych**, określić całkowitej liczby dysków z serwera źródłowego.
   * W **wykorzystanie pojemności dysku**, określ średnie wykorzystanie.
   * W **codziennie zmienić częstotliwość (%)**, określ codziennych danych zmienić częstotliwość, z serwera źródłowego.
   * W **mapowania Azure rozmiar**, wprowadź rozmiar maszyny Wirtualnej platformy Azure, który ma być mapowany. Jeśli nie chcesz zrobić to ręcznie, kliknij przycisk **obliczeniowe maszyn wirtualnych IaaS**. Jeśli wprowadź ustawienia ręcznie, a następnie kliknij obliczeniowe maszyn wirtualnych IaaS, mogą być zastępowane ręczne ustawienie, ponieważ proces obliczeń automatycznie rozpoznaje najlepszego dopasowania rozmiaru maszyny Wirtualnej platformy Azure.

   ![Kwalifikacja obciążenia](./media/site-recovery-capacity-planner/workload-qualification.png)
4. Jeśli klikniesz przycisk **obliczeniowe maszyn wirtualnych IaaS** Oto, czego:

   * Sprawdza poprawność obowiązkowe danych wejściowych.
   * Oblicza wartość IOPS, a także sugeruje najlepsze maszyny Wirtualnej Azure aize dopasowanie dla poszczególnych maszyn wirtualnych, które kwalifikują się do replikacji na platformie Azure. Jeśli nie można wykryć odpowiedni rozmiar maszyny Wirtualnej platformy Azure, że wygenerowany błąd. Na przykład jeśli liczba dysków dołączonych w 65, zgłaszany jest błąd ponieważ najwyższy rozmiar maszyny Wirtualnej platformy Azure jest 64.
   * Sugeruje konta magazynu, który może służyć do maszyny Wirtualnej platformy Azure.
   * Oblicza sumę kont magazynu w warstwie standardowa i premium kont magazynu wymagana dla obciążeń. Przewiń w dół do widoku typu magazynu Azure i konto magazynu, który może służyć do serwera źródłowego.
   * Kończy i sortuje reszty tabeli na podstawie typu wymagane magazynu (standardowy lub premium) przypisany do maszyny Wirtualnej, a liczba dołączonych dysków. Dla wszystkich maszyn wirtualnych, które spełniają wymagania dotyczące platformy Azure, kolumnie **jest wirtualna kwalifikowana?** pokazuje **tak**. Jeśli maszyny Wirtualnej nie można wykonać kopii zapasowej na platformie Azure, zostanie wyświetlony błąd.

Kolumny AA AE są dane wyjściowe i podaj informacje dotyczące każdej maszyny Wirtualnej.

![Kwalifikacja obciążenia](./media/site-recovery-capacity-planner/workload-qualification-2.png)

### <a name="example"></a>Przykład
Na przykład sześć maszyn wirtualnych przy użyciu wartości z tabelą narzędzie oblicza i przypisuje najlepsze dopasowanie maszyny Wirtualnej platformy Azure i wymagania dotyczące magazynu Azure.

![Kwalifikacja obciążenia](./media/site-recovery-capacity-planner/workload-qualification-3.png)

* W przykładzie danych wyjściowych należy uwzględnić następujące informacje:

  * Pierwsza kolumna jest kolumną sprawdzania poprawności dla maszyn wirtualnych, dysków i ilość danych churn.
  * Dwa konta magazynu w warstwie standardowa i premium jednego konta magazynu są potrzebne do pięciu maszynach wirtualnych.
  * VM3 nie kwalifikuje się do ochrony, ponieważ co najmniej jeden dysk jest więcej niż 1 TB.
  * VM1 i maszyny VM2 mogą używać pierwsze konto magazynu w warstwie standardowa
  * VM4 można użyć drugiego konta magazynu w warstwie standardowa.
  * VM5 i VM6 wymagane jest konto magazynu premium i można jednocześnie używać jednego konta.

    > [!NOTE]
    > IOPS na warstwy standardowa i premium magazynu są obliczane na poziomie maszyny Wirtualnej, a nie na poziomie dysku. Standardowa maszyna wirtualna może obsługiwać maksymalnie 500 IOPS dla każdego dysku. Wartość IOPS dla dysku jest większa niż 500, należy najpierw Magazyn w warstwie premium. Jednak jeśli IOPS dla dysku jest więcej niż 500, ale IOPS całkowita dysków maszyny Wirtualnej są w granicach pomocy technicznej standard maszyny Wirtualnej platformy Azure (rozmiar maszyny Wirtualnej, liczbę dysków, liczba kart, Procesora, pamięci), następnie planistę wybiera standard serii maszyny Wirtualnej, a nie DS lub GS. Należy ręcznie zaktualizować mapowania komórki rozmiaru platformy Azure z odpowiednią serii DS lub GS maszyny Wirtualnej.


Po wszystkie szczegółowe informacje znajdują się w miejscu, kliknij przycisk **przesyłania danych do narzędzia do planowania** otworzyć **Capacity Planner** obciążeń są wyróżnione, aby pokazać, czy są one kwalifikuje się do ochrony lub nie.

### <a name="submit-data-in-the-capacity-planner"></a>Przesyłanie danych w module planowania pojemności
1. Po otwarciu **Capacity Planner** arkusza zostanie wprowadzony zgodnie z ustawieniami wybranymi przez użytkownika. Wyraz "Obciążenie" pojawia się w **źródła danych wejściowych Infra** komórki, aby wyświetlić dane wejściowe są **kwalifikacji obciążenia** arkusza.
2. Jeśli chcesz wprowadzić zmiany, należy zmodyfikować **kwalifikacji obciążenia** arkuszu, a następnie kliknij przycisk **przesyłania danych do narzędzia do planowania** ponownie.  

   ![Narzędzie do planowania pojemności](./media/site-recovery-capacity-planner/capacity-planner.png)
