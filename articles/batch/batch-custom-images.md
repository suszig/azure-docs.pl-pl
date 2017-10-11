---
title: "Zapewnij pule partii zadań Azure z niestandardowymi obrazami | Dokumentacja firmy Microsoft"
description: "Można utworzyć plik wsadowy puli z niestandardowego obrazu, aby udostępnić obliczeniowe węzłów, które zawierają oprogramowanie i dane potrzebne do aplikacji. Niestandardowe obrazy są wydajnym sposobem konfigurowania węzłów obliczeniowych do uruchamiania obciążeń partii."
services: batch
author: tamram
manager: timlt
ms.service: batch
ms.topic: article
ms.date: 08/07/2017
ms.author: tamram
ms.openlocfilehash: 3d655766b4f2a5efb0c8c29ffa81a89f84b3e17c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="use-a-custom-image-to-create-a-pool-of-virtual-machines"></a>Użyj obrazu niestandardowego do utworzenia puli maszyn wirtualnych

Podczas tworzenia puli maszyn wirtualnych w partii zadań Azure, należy określić obraz maszyny wirtualnej (VM), który zawiera system operacyjny na każdym węźle obliczeń w puli. Za pomocą portalu Azure Marketplace obrazu lub poprzez świadczenie niestandardowego obrazu wirtualnego dysku twardego, które zostały przygotowane, można utworzyć puli maszyn wirtualnych. Podając niestandardowego obrazu, masz kontrolę nad konfiguracji systemu operacyjnego zostanie zainicjowana w każdym węźle obliczeń w chwili. Obraz niestandardowy może również obejmować aplikacji i danych referencyjnych, które są dostępne w węźle obliczeń, jak jego obsługa została zainicjowana.

Przy użyciu niestandardowego obrazu może zaoszczędzić czas podczas pobierania przez pulę węzły obliczeniowe gotowy do uruchomienia obciążenie partii. Mimo że zawsze używać obrazu platformy Azure Marketplace i instalować oprogramowanie na każdy węzeł obliczeniowy po została przygotowana, ta metoda może być mniej wydajne niż przy użyciu niestandardowego obrazu. 

Niektóre do użycia niestandardowego obrazu, który jest skonfigurowany dla danego scenariusza przyczyny konieczności:

- **Konfigurowanie systemu operacyjnego (OS)** żadnej specjalnej konfiguracji systemu operacyjnego można wykonać na niestandardowego obrazu. 
- **Zainstaluj dużych aplikacji.** Instalowanie aplikacji na obraz niestandardowy jest bardziej efektywne niż je zainstalować w każdym węźle obliczeń, po jego obsługa została zainicjowana.
- **Skopiuj znacznej ilości danych.** Jeśli dane są kopiowane do niestandardowego obrazu, tylko musi ma zostać skopiowany na raz, a nie w każdym węźle obliczeń, oszczędzając czas i przepustowości.
- **Ponowny rozruch maszyny Wirtualnej podczas procesu instalacji.** Ponowny rozruch maszyny Wirtualnej może być czasochłonne, zwłaszcza, jeśli liczba węzłów obliczeniowych.

## <a name="prerequisites"></a>Wymagania wstępne

- **Konto usługi partia zadań utworzonych za pomocą trybu alokacji puli subskrypcji użytkownika.** Aby użyć niestandardowego obrazu w celu udostępnienia pule maszyny wirtualnej, należy utworzyć konta partii zadań z subskrypcji użytkownika [Tryb alokacji puli](batch-api-basics.md#pool-allocation-mode). W tym trybie pule usługi Batch są przydzielane do subskrypcji, w której znajduje się konto. Zobacz [konta](batch-api-basics.md#account) sekcji [Programowanie równoległe na dużą skalę obliczeniowe rozwiązań w partii](batch-api-basics.md) informacje na temat ustawiania trybu alokacji puli podczas tworzenia konta usługi partia zadań.

- **Konto usługi Azure Storage.** Aby utworzyć puli maszyn wirtualnych przy użyciu niestandardowego obrazu, należy standardowe, ogólnego przeznaczenia konta magazynu Azure w tej samej subskrypcji i regionu. Jeśli tworzysz niestandardowy obraz z maszyny Wirtualnej platformy Azure, obraz zostanie skopiowany do konta magazynu, w którym znajduje się dysk systemu operacyjnego maszyny Wirtualnej, a nie musisz tworzyć oddzielnego konta magazynu. 
    
## <a name="prepare-a-custom-image"></a>Przygotuj obraz niestandardowy

Aby przygotować obraz niestandardowy do użycia z partii, musi generalize istniejącej instalacji systemu Linux lub Windows. Uogólnianie instalacji systemu operacyjnego usuwa informacje dotyczące komputera. Wynik jest obraz, który można zainstalować na innych komputerach lub maszynach wirtualnych.  

> [!IMPORTANT]
> Partii nie obsługuje obecnie korzysta z platformy Azure zarządzanych obrazów do obsługi administracyjnej puli. Niestandardowy obraz, który można użyć w celu udostępnienia puli muszą być przechowywane w usłudze Azure Storage. 
>
> Podczas przygotowywania obraz niestandardowy, należy pamiętać następujące kwestie:
> - Upewnij się, że podstawowy obraz systemu operacyjnego używany do aprowizowania pul usługi Batch nie ma wstępnie zainstalowanych rozszerzeń platformy Azure, takich jak rozszerzenie skryptu niestandardowego. Jeśli obraz zawiera wstępnie zainstalowane rozszerzenie, platforma Azure może napotkać problemy podczas wdrażania maszyny wirtualnej.
> - Upewnij się, że wybrany podstawowy obraz systemu operacyjnego korzysta z domyślnego dysku tymczasowego, ponieważ agent węzłów usługi Batch oczekuje domyślnego dysku tymczasowego.
>
>

Można użyć dowolnego istniejącego przygotowane obrazu systemu Windows lub Linux jako obraz niestandardowy. Na przykład, jeśli chcesz użyć lokalnego obrazu, następnie przesłać do konta usługi Azure Storage, który znajduje się w tej samej subskrypcji i regionu co użycie konta usługi partia zadań [AzCopy](../storage/storage-use-azcopy.md) lub innego narzędzia przekazywania.

Można również przygotować obraz niestandardowy z nowej lub istniejącej maszyny Wirtualnej Azure. W przypadku tworzenia nowej maszyny Wirtualnej, można użyć obrazu portalu Azure Marketplace jako obrazu podstawowego dla niestandardowego obrazu, a następnie dostosować go. Aby dostosować obraz podstawowy, tworzenie maszyny Wirtualnej platformy Azure i Dodaj do niej aplikacje lub danych. Następnie Uogólnij maszyna wirtualna może służyć jako obraz niestandardowy i zapisać go do magazynu Azure. 

Aby przygotować obraz niestandardowy z maszyny Wirtualnej platformy Azure, wykonaj następujące kroki:

1. Utwórz **niezarządzane** maszyny Wirtualnej platformy Azure z obrazu w portalu Azure Marketplace. Azure Marketplace zawiera obrazy dla obu [Windows](../virtual-machines/windows/quick-create-portal.md) i [Linux](../virtual-machines/linux/quick-create-portal.md).
    
    W kroku 3 procesu tworzenia maszyny Wirtualnej, upewnij się, że wybrano **nr** dla **magazynu: Użyj zarządzanych dysków** opcji. Również Zanotuj nazwę konta magazynu dla dysku systemu operacyjnego maszyny Wirtualnej, ponieważ to konto magazynu jest również, gdzie Azure zapisze niestandardowego obrazu:

    ![Tworzenie niezarządzanej maszyny Wirtualnej i zanotuj nazwę konta magazynu](media/batch-custom-images/vm-create-storage.png)
 
2. Ukończenie procesu tworzenia maszyny Wirtualnej, a następnie zaczekaj na jej przydzielone przez platformę Azure. Oto obrazu, który zawiera Maszynę wirtualną w portalu Azure w stanie uruchomienia:

    ![Utwórz maszynę Wirtualną z obrazu witryny marketplace](media/batch-custom-images/vm-status-running.png)

3. Gdy maszyna wirtualna jest uruchomiona, się z nim połączyć za pośrednictwem protokołu RDP (dla systemu Windows) lub protokołu SSH (dla systemu Linux). Zainstaluj oprogramowanie niezbędne skopiować żądanych danych, a następnie Uogólnij maszyny Wirtualnej. Wykonaj kroki opisane w [Generalize maszyny Wirtualnej](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized.md#generalize-the-vm). 
   
4. Wykonaj kroki, aby [zalogować się do programu Azure PowerShell](../virtual-machines/windows/sa-copy-generalized.md#log-in-to-azure-powershell). Aby zainstalować program Azure PowerShell, zobacz [Omówienie programu Azure PowerShell](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.2.0). 

5. Następnie wykonaj kroki, aby [Cofnij Przydział maszyny Wirtualnej i ustawić stan na ogólnych](https://docs.microsoft.com/en-us/azure/virtual-machines/windows/sa-copy-generalized#deallocate-the-vm-and-set-the-state-to-generalized). 

    W portalu Azure należy zauważyć, że cofnięciu przydziału maszyny Wirtualnej:

    ![Upewnij się, że cofnięciu przydziału maszyny Wirtualnej](media/batch-custom-images/vm-status-deallocated.png)

6.  Utwórz i Zapisz obraz maszyny Wirtualnej za pomocą usługi Azure Storage [AzureRmVMImage Zapisz](https://docs.microsoft.com/powershell/module/azurerm.compute/save-azurermvmimage) polecenia cmdlet programu PowerShell. Wykonaj instrukcje opisane w [utworzyć obraz](../virtual-machines/windows/sa-copy-generalized.md#create-the-image).
    
    Konto magazynu Azure utworzona podczas tworzenia maszyny Wirtualnej, jak pokazano w kroku 1 tej procedury jest zapisywany obraz maszyny Wirtualnej. Zapisz AzureRmVMImage polecenie cmdlet zapisuje obraz do **systemu** kontenera na tym koncie magazynu. `-DestinationContainername` Katalogu wirtualnego w ramach nazwy parametrów **systemu** kontenera. `-VHDNamePrefix` Parametr określa prefiks nazwy obiektu blob. Ten prefiks dołączany na początku nazwy obiektu blob z łącznikiem. 

    Na przykład załóżmy, że wywołanie AzureRmVMImage Zapisz z następującymi parametrami:  

        Save-AzureRmVMImage -ResourceGroupName sample-resource-group -Name vm-custom-image -DestinationContainerName batchimages -VHDNamePrefix custom -Path C:\Temp\Images\vm-custom-image.json

    Wynikowy obraz jest zapisany na lokalizację i nazwę obiektu blob, pokazano poniżej:

    ![Lokalizacja zapisanego wirtualnego dysku twardego w kontenerze systemu](media/batch-custom-images/vhd-in-vm-storage-account.png)

    > [!NOTE]
    > Maszyna wirtualna platformy Azure niezarządzane tworzy kilka kont magazynu do różnych celów. Jeśli nie Zanotuj nazwę kontenera magazynu dla dysku systemu operacyjnego, podczas tworzenia maszyny Wirtualnej, następnie znajdź kontem powiązanym magazynie, które zawiera **systemu** kontenera. Przejdź do **systemu** kontener, aby znaleźć niestandardowego obrazu przy użyciu wartości określone dla **AzureRmVMImage Zapisz** polecenia.

## <a name="create-a-pool-from-a-custom-image-in-the-portal"></a>Utwórz pulę z niestandardowego obrazu w portalu

Po zapisaniu niestandardowego obrazu i znasz lokalizacji, można utworzyć puli partii z tego obrazu. Wykonaj następujące kroki, aby utworzyć pulę z portalu Azure:

1. W witrynie Azure Portal przejdź do swojego konta usługi Batch. To konto musi być w tej samej subskrypcji i regionu co konto magazynu zawierające niestandardowego obrazu. 
2. W **ustawienia** oknie po lewej stronie, wybierz opcję **pule** elementu menu.
3. W **pule** wybierz **Dodaj** polecenia.
4. Na **Dodaj pulę,** wybierz **niestandardowego obrazu (Linux/system Windows)** z **typ obrazu** listy rozwijanej. Portal zawiera selektor **Obraz niestandardowy**. Przejdź do konta magazynu, w którym znajduje się obraz niestandardowy, a następnie wybierz żądany dysk VHD z kontenera. 
5. Wybierz poprawny **wydawcy lub oferta/jednostki Sku** dla Twojego niestandardowego pliku VHD.
6. Określ pozostałe wymagane ustawienia, w tym **rozmiaru węzła**, **Target dedykowanych węzłów**, i **niski priorytet węzłów**, a także wszelkie potrzebne ustawienia opcjonalne.

    Na przykład w przypadku obraz niestandardowy Microsoft Windows Server Datacenter 2016 **Dodaj pulę,** zostanie wyświetlone okno, jak pokazano poniżej:

    ![Dodaj pulę z niestandardowego obrazu systemu Windows](media/batch-custom-images/add-pool-custom-image.png)
  
Aby sprawdzić, czy istniejącej puli jest oparta na niestandardowego obrazu, zobacz **systemu operacyjnego** właściwości w sekcji Podsumowanie zasobów z **puli** okna. Jeśli w puli została utworzona z obrazu niestandardowego, jest równa **niestandardowego obrazu maszyny Wirtualnej**.

Wszystkie dyski VHD niestandardowe skojarzone z pulą są wyświetlane w puli **właściwości** okna.
 
## <a name="next-steps"></a>Następne kroki

- Aby uzyskać szczegółowe informacje o partii, zobacz [Programowanie równoległe na dużą skalę obliczeniowe rozwiązań w partii](batch-api-basics.md).
- Aby uzyskać informacje dotyczące tworzenia konta usługi partia zadań, zobacz [Tworzenie konta usługi partia zadań za pomocą portalu Azure](batch-account-create-portal.md).