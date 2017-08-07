---
title: "Korzystanie z usługi Azure Batch Rendering w celu renderowania w chmurze | Microsoft Docs"
description: "Renderuj zadania na maszynach wirtualnych platformy Azure bezpośrednio z programu Maya z opłatami za użycie."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: hero-article
ms.date: 07/31/2017
ms.author: tamram
ms.translationtype: HT
ms.sourcegitcommit: 7bf5d568e59ead343ff2c976b310de79a998673b
ms.openlocfilehash: 4d22f92cafdbceee5213361d6d2b2f38904d12c6
ms.contentlocale: pl-pl
ms.lasthandoff: 08/01/2017

---

# <a name="get-started-with-the-batch-rendering-service"></a>Wprowadzenie do usługi Batch Rendering

Usługa Azure Batch Rendering oferuje możliwości renderowania w skali chmury z opłatami za użycie. Usługa Batch Rendering obsługuje planowanie i kolejkowanie zadań, zarządzanie niepowodzeniami i ponawianiem prób oraz automatyczne skalowanie zadania renderowania. Usługa Batch Rendering obsługuje programy Autodesk Maya, 3ds Max i Arnold. Wkrótce dostępna będzie obsługa innych aplikacji. Wtyczka usługi Batch dla programu Maya 2017 ułatwia rozpoczęcie zadania renderowania na platformie Azure bezpośrednio z pulpitu. 

## <a name="supported-applications"></a>Obsługiwane aplikacje

Usługa Batch Rendering obecnie obsługuje następujące aplikacje:

- Autodesk Maya
- Autodesk 3ds Max
- Autodesk Arnold

## <a name="prerequisites"></a>Wymagania wstępne

Aby użyć usługi Batch Rendering, wymagane są następujące elementy:

- [Konto platformy Azure](https://azure.microsoft.com/free/). 
- **Konto usługi Azure Batch.** Aby uzyskać wskazówki dotyczące tworzenia konta usługi Batch w witrynie Azure Portal, zobacz [Tworzenie konta usługi Batch w witrynie Azure Portal](batch-account-create-portal.md).
- **Konto usługi Azure Storage.** Zasoby używane w ramach zadania renderowania są przechowywane w usłudze Azure Storage. Konto magazynu możesz utworzyć automatycznie podczas konfigurowania konta usługi Batch. Możesz także użyć istniejącego konta magazynu. Więcej informacji na temat kont magazynu można znaleźć w temacie [Jak utworzyć lub usunąć konto magazynu oraz zarządzać nim w witrynie Azure Portal](https://docs.microsoft.com/azure/storage/storage-create-storage-account).

Aby użyć wtyczki usługi Batch dla programu Maya, wymagane są następujące elementy:

- **Maya 2017**
- **Arnold for Maya**

Witryny [Azure Portal](https://portal.azure.com) możesz również używać do tworzenia pul maszyn wirtualnych, które są wstępnie skonfigurowane z użyciem programów Maya, 3ds Max i Arnold. Za pomocą portalu możesz monitorować zadania i diagnozować zadania podrzędne zakończone niepowodzeniem poprzez pobieranie dzienników aplikacji oraz zdalne łączenie się z poszczególnymi maszynami wirtualnymi przy użyciu protokołu RDP lub SSH.

## <a name="basic-batch-concepts"></a>Podstawowe pojęcia usługi Batch

Przed rozpoczęciem korzystania z usługi Batch Rendering należy zaznajomić się z kilkoma podstawowymi pojęciami usługi Batch, w tym węzłami obliczeniowymi, pulami i zadaniami. Aby uzyskać więcej ogólnych informacji o usłudze Azure Batch, zobacz [Uruchamianie wewnętrznie równoległych obciążeń przy użyciu usługi Batch](batch-technical-overview.md).

### <a name="pools"></a>Pule

Usługa Batch to usługa platformy do obsługi zadań związanych z intensywnymi obliczeniami, np. renderowania, w oparciu o **pulę** **węzłów obliczeniowych**. Każdy węzeł obliczeniowy w puli jest maszyną wirtualną platformy Azure z systemem Linux lub Windows. 

Aby uzyskać więcej informacji o pulach i węzłach obliczeniowych usługi Batch, zobacz sekcje [Pula](batch-api-basics.md#pool) i [Węzeł obliczeniowy](batch-api-basics.md#compute-node) w temacie [Tworzenie rozbudowanych rozwiązań przetwarzania równoległego przy użyciu usługi Batch](batch-api-basics.md).

### <a name="jobs"></a>Zadania

**Zadanie** usługi Batch to zbiór zadań podrzędnych uruchamianych w węzłach obliczeniowych w puli. Po przesłaniu zadania renderowania usługa Batch dzieli zadanie na wiele zadań podrzędnych i dystrybuuje je do węzłów obliczeniowych w puli w celu uruchomienia.

Aby uzyskać więcej informacji o zadaniach usługi Batch, zobacz sekcję [Zadanie](batch-api-basics.md#job) w temacie [Tworzenie rozbudowanych rozwiązań przetwarzania równoległego przy użyciu usługi Batch](batch-api-basics.md).

## <a name="use-the-batch-plug-in-for-maya-to-submit-a-render-job"></a>Korzystanie z wtyczki usługi Batch dla programu Maya do przesyłania zadania renderowania

Za pomocą wtyczki usługi Batch dla programu Maya możesz przesłać zadanie do usługi Batch Rendering bezpośrednio z programu Maya. Następujące sekcje opisują, w jaki sposób można skonfigurować zadanie z poziomu wtyczki, a następnie je przesłać. 

### <a name="load-the-batch-plug-in-in-maya"></a>Ładowanie wtyczki usługi Batch w programie Maya

Wtyczka usługi Batch jest dostępna w usłudze [GitHub](https://github.com/Azure/azure-batch-maya/releases). Rozpakuj archiwum do wybranego katalogu. Wtyczkę możesz załadować bezpośrednio z katalogu *azure_batch_maya*.

Aby załadować wtyczkę w programie Maya:

1. Uruchom program Maya.
2. Wybierz pozycję **Window (Okno)** > **Settings/Preferences (Ustawienia/preferencje)** > **Plug-in Manager (Menedżer wtyczek)**.
3. Kliknij pozycję **Browse (Przeglądaj)**.
4. Wskaż plik *azure_batch_maya/plug-in/AzureBatch.py* i wybierz go.

### <a name="authenticate-access-to-your-batch-and-storage-accounts"></a>Uwierzytelnianie dostępu do kont usługi Batch i Storage

Aby użyć wtyczki, musisz przeprowadzić uwierzytelnianie przy użyciu kluczy kont usług Azure Batch i Azure Storage. Aby pobrać klucze konta:

1. Wyświetl wtyczkę w programie Maya i wybierz kartę **Konfiguracja**.
2. Przejdź do witryny [Azure Portal](https://portal.azure.com).
3. Wybierz opcję **Konta usługi Batch** z menu po lewej stronie. W razie potrzeby kliknij opcję **Więcej usług** i filtruj według kryterium _Batch_.
4. Znajdź żądane konto usługi Batch na liście.
5. Wybierz element menu **Klucze**, aby wyświetlić nazwę, adres URL i klucze dostępu konta:
    - Wklej adres URL konta usługi Batch do pola **Usługa** we wtyczce usługi Batch.
    - Wklej nazwę konta do pola **Konto usługi Batch**.
    - Wklej podstawowy klucz konta do pola **Klucz usługi Batch**.
7. Wybierz pozycję Konta usługi Storage z menu po lewej stronie. W razie potrzeby kliknij opcję **Więcej usług** i filtruj według kryterium _Storage_.
8. Znajdź żądane konto usługi Storage na liście.
9. Wybierz element menu **Klucze dostępu**, aby wyświetlić nazwę i klucze konta magazynu.
    - Wklej nazwę konta usługi Storage do pola **Konto usługi Storage** we wtyczce usługi Batch.
    - Wklej podstawowy klucz konta do pola **Klucz usługi Storage**.
10. Kliknij przycisk **Uwierzytelnij**, aby upewnić się, że wtyczka może uzyskać dostęp do obu kont.

Po pomyślnym uwierzytelnieniu wtyczka ustawia pole stanu na **Uwierzytelniono**: 

![Uwierzytelnianie kont usług Batch i Storage](./media/batch-rendering-service/authentication.png)

### <a name="configure-a-pool-for-a-render-job"></a>Konfigurowanie puli pod kątem zadania renderowania

Po uwierzytelnieniu kont usług Batch i Storage skonfiguruj pulę pod kątem zadania renderowania. Wtyczka zapisuje wybory między sesjami. Po ustawieniu preferowanej konfiguracji nie musisz jej modyfikować, chyba że ulegnie ona zmianie.

Poniższe sekcje przeprowadzą Cię przez opcje dostępne na karcie **Przesyłanie**:

#### <a name="specify-a-new-or-existing-pool"></a>Określanie nowej lub istniejącej puli

Aby określić pulę, w której chcesz uruchomić zadanie renderowania, wybierz kartę **Przesyłanie**. Ta karta oferuje opcje umożliwiające utworzenie puli lub wybranie istniejącej puli:

- **Automatyczne aprowizowanie puli dla tego zadania** (opcja domyślna). Gdy wybierzesz tę opcję, usługa Batch utworzy pulę wyłącznie dla bieżącego zadania, a następnie automatycznie usunie ją po zakończeniu zadania renderowania. Ta opcja sprawdza się najlepiej wtedy, gdy masz jedno zadanie renderowania do wykonania.
- **Ponowne użycie istniejącej puli trwałej**. Jeśli masz istniejącą pulę, która pozostaje bezczynna, możesz wybrać tę pulę do uruchomienia zadania renderowania poprzez wybranie jej z listy rozwijanej. Ponowne użycie istniejącej puli trwałej oszczędza czas wymagany do aprowizacji puli.  
- **Utworzenie nowej puli trwałej**. Wybranie tej opcji spowoduje utworzenie nowej puli na potrzeby uruchomienia zadania. Pula nie zostanie usunięta po zakończeniu zadania. Będzie ona dostępna do ponownego użycia na potrzeby przyszłych zadań. Wybierz tę opcję, gdy masz potrzebę ciągłego uruchamiania zadań renderowania. W przypadku kolejnych zadań możesz **ponownie użyć istniejącej puli trwałej**, aby skorzystać z puli trwałej utworzonej dla pierwszego zadania.

![Określanie puli, obrazu systemu operacyjnego, rozmiaru maszyny wirtualnej i licencji](./media/batch-rendering-service/submit.png)

Aby uzyskać więcej informacji o naliczaniu opłat dla maszyn wirtualnych platformy Azure, zobacz [Często zadawane pytania dotyczące cen dla systemu Linux](https://azure.microsoft.com/pricing/details/virtual-machines/linux/#faq) oraz [Często zadawane pytania dotyczące cen dla systemu Windows](https://azure.microsoft.com/pricing/details/virtual-machines/windows/#faq).

#### <a name="specify-the-os-image-to-provision"></a>Określanie obrazu systemu operacyjnego do aprowizacji

Typ obrazu systemu operacyjnego do użycia, aby aprowizować węzły obliczeniowe w puli, możesz określić na karcie **Środowisko**. Usługa Batch obecnie obsługuje następujące opcje obrazów w przypadku zadań renderowania:

|System operacyjny  |Image (Obraz)  |
|---------|---------|
|Linux     |Batch CentOS — wersja zapoznawcza |
|Windows     |Batch Windows — wersja zapoznawcza |

#### <a name="choose-a-vm-size"></a>Wybieranie rozmiaru maszyny wirtualnej

Rozmiar maszyny wirtualnej możesz określić na karcie **Środowisko**. Aby uzyskać więcej informacji o dostępnych rozmiarach maszyn wirtualnych, zobacz [Linux VM sizes in Azure (Rozmiary maszyn wirtualnych z systemem Linux na platformie Azure)](https://docs.microsoft.com/azure/virtual-machines/linux/sizes) oraz [Windows VM sizes in Azure (Rozmiary maszyn wirtualnych z systemem Windows na platformie Azure)](https://docs.microsoft.com/azure/virtual-machines/windows/sizes). 

![Określanie obrazu systemu operacyjnego i rozmiaru maszyny wirtualnej na karcie Środowisko](./media/batch-rendering-service/environment.png)

#### <a name="specify-licensing-options"></a>Określanie opcji licencjonowania

Licencje do użycia możesz określić na karcie **Środowisko**. Dostępne są następujące opcje:

- **Maya** — włączona domyślnie.
- **Arnold** — włączona, jeśli jako aktywny aparat renderowania w programie Maya wykryto aparat Arnold.

 Jeśli chcesz renderować przy użyciu swojej własnej licencji, możesz skonfigurować punkt końcowy licencji poprzez dodanie odpowiednich zmiennych środowiskowych do tabeli. Pamiętaj, aby w takim przypadku usunąć zaznaczenie domyślnych opcji licencjonowania.

> [!IMPORTANT]
> Opłaty za użycie licencji są naliczane, gdy maszyny wirtualne działają w puli, nawet jeśli maszyny wirtualne nie są obecnie używane w ramach renderowania. Aby uniknąć dodatkowych opłat, przejdź do karty **Pule** i zmień rozmiar puli do 0 węzłów do czasu, gdy będzie konieczne uruchomienie kolejnego zadania renderowania. 
>
>

#### <a name="manage-persistent-pools"></a>Zarządzanie pulami trwałymi

Istniejącą pulą trwałą możesz zarządzać na karcie **Pule**. Wybranie puli z listy spowoduje wyświetlenie bieżącego stanu puli.

Na karcie **Pule** możesz też usunąć pulę i zmienić jej rozmiar, dostosowując liczbę maszyn wirtualnych w puli. Aby uniknąć ponoszenia kosztów w okresie bezczynności, możesz zmienić rozmiar puli na 0 węzłów.

![Wyświetlanie, zmiana rozmiaru i usuwanie pul](./media/batch-rendering-service/pools.png)

### <a name="configure-a-render-job-for-submission"></a>Konfigurowanie zadania renderowania do przesłania

Po określeniu parametrów dla puli, w której zostanie uruchomione zadanie renderowania, skonfiguruj samo zadanie. 

#### <a name="specify-scene-parameters"></a>Określanie parametrów sceny

Wtyczka usługi Batch wykrywa aparat renderowania obecnie używany w programie Maya i wyświetla odpowiednie ustawienia renderowania na karcie **Przesyłanie**. Ustawienia te obejmują ramkę początkową, ramkę końcową, prefiks danych wyjściowych i krok ramki. Ustawienia renderowania pliku sceny możesz przesłonić poprzez określenie innych ustawień we wtyczce. Zmiany wprowadzone w ustawieniach wtyczki nie są utrwalane w ustawieniach renderowania pliku sceny, więc możesz wprowadzać zmiany dla poszczególnych zadań bez potrzeby ponownego przekazywania pliku sceny.

Wtyczka wyświetli ostrzeżenie, jeśli wybrany aparat renderowania w programie Maya nie jest obsługiwany.

Jeśli załadujesz nową scenę, gdy wtyczka jest otwarta, kliknij przycisk **Odśwież**, aby upewnić się, że ustawienia są aktualne.

#### <a name="resolve-asset-paths"></a>Rozpoznawanie ścieżek zasobów

Gdy załadujesz wtyczkę, przeskanuje ona plik sceny pod kątem odwołań do plików zewnętrznych. Te odwołania zostaną wyświetlone na karcie **Zasoby**. Jeśli nie można rozpoznać przywoływanej ścieżki, wtyczka spróbuje znaleźć plik w kilku lokalizacjach domyślnych, w tym:

- Lokalizacja pliku sceny 
- Katalog _sourceimages_ bieżącego projektu
- Bieżący katalog roboczy 

Jeśli nadal nie można zlokalizować zasobu, będzie on wyświetlany na liście z ikoną ostrzeżenia:

![Brakujące zasoby są wyświetlane z ikoną ostrzeżenia](./media/batch-rendering-service/missing_assets.png)

Jeśli znasz lokalizację nierozpoznanego odwołania do pliku, możesz kliknąć ikonę ostrzeżenia. Zostanie wyświetlony monit o dodanie ścieżki wyszukiwania. Następnie wtyczka użyje tej ścieżki wyszukiwania podczas próby rozpoznania wszelkich brakujących zasobów. Liczba dodatkowych ścieżek wyszukiwania nie jest ograniczona.

Po rozpoznaniu odwołania na liście będzie wyświetlana ikona zielonego światła:

![Rozpoznane zasoby są wyświetlane z ikoną zielonego światła](./media/batch-rendering-service/found_assets.png)

Jeśli scena wymaga innych plików, których wtyczka nie wykryła, możesz dodać dodatkowe pliki lub katalogi. Użyj przycisków **Dodaj pliki** i **Dodaj katalog**. Jeśli załadujesz nową scenę, gdy wtyczka jest otwarta, kliknij przycisk **Odśwież**, aby zaktualizować odwołania sceny.

#### <a name="upload-assets-to-an-asset-project"></a>Przekazywanie zasobów do projektu zasobów

Gdy prześlesz zadanie renderowania, przywoływane pliki wyświetlane na karcie **Zasoby** są automatycznie przekazywane do usługi Azure Storage w formie projektu zasobów. Pliki zasobów możesz też przekazać niezależnie od zadania renderowania, korzystając z przycisku **Przekaż** na karcie **Zasoby**. Nazwa projektu zasobów jest określana w polu **Projekt**. Domyślnie jest ona zgodna z bieżącym projektem programu Maya. Podczas przekazywania plików zasobów zachowywana jest lokalna struktura plików. 

Po przekazaniu zasoby mogą być przywoływane przez dowolną liczbę zadań renderowania. Wszystkie przekazane zasoby są dostępne dla dowolnego zadania, które odwołuje się do projektu zasobów, niezależnie od tego, czy są uwzględnione w danej scenie. Aby zmienić projekt zasobów przywoływany przez następne zadanie, zmień nazwę w polu **Projekt** na karcie **Zasoby**. Jeśli nie chcesz przekazywać niektórych przywoływanych plików, usuń ich zaznaczenie przy użyciu zielonego przycisku obok listy.

#### <a name="submit-and-monitor-the-render-job"></a>Przesyłanie i monitorowanie zadania renderowania

Po skonfigurowaniu zadania renderowania we wtyczce kliknij przycisk **Prześlij zadanie** na karcie **Przesyłanie**, aby przesłać zadanie do usługi Batch:

![Przesyłanie zadania renderowania](./media/batch-rendering-service/submit_job.png)

Zadanie w toku możesz monitorować na karcie **Zadania** we wtyczce. Wybierz zadanie z listy, aby wyświetlić bieżący stan zadania. Tej karty możesz też użyć do anulowania i usuwania zadań, a także pobierania danych wyjściowych i dzienników renderowania. 

Aby pobrać dane wyjściowe, zmodyfikuj pole **Dane wyjściowe**, ustawiając żądany katalog docelowy. Kliknij ikonę koła zębatego, aby uruchomić proces w tle, który będzie obserwować zadanie i pobierać dane wyjściowe w miarę postępów: 

![Wyświetlanie stanu zadania i pobieranie danych wyjściowych](./media/batch-rendering-service/jobs.png)

Program Maya możesz zamknąć bez zakłócania procesu pobierania.

## <a name="next-steps"></a>Następne kroki

Aby dowiedzieć się więcej o usłudze Batch, zobacz [Uruchamianie wewnętrznie równoległych obciążeń przy użyciu usługi Batch](batch-technical-overview.md).
