<properties
   pageTitle="Rozpoczęcie pracy z programem PowerShell w usłudze Azure Batch | Microsoft Azure"
   description="Krótkie wprowadzenie do poleceń cmdlet programu Azure PowerShell, których można użyć do zarządzania usługą Azure Batch"
   services="batch"
   documentationCenter=""
   authors="dlepow"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="powershell"
   ms.workload="big-compute"
   ms.date="07/28/2016"
   ms.author="danlep"/>

# Rozpoczęcie pracy z poleceniami cmdlet programu PowerShell w usłudze Azure Batch
Za pomocą poleceń cmdlet PowerShell usługi Azure Batch można wykonywać oraz tworzyć skrypty dla wielu tych samych zadań, które wykonuje się za pomocą interfejsów API usługi Batch, witryny Azure Portal oraz interfejsu wiersza polecenia Azure (CLI). Oto krótkie wprowadzenie do poleceń cmdlet, których można używać do zarządzania kontami usługi Batch oraz pracy z zasobami usługi Batch, np. pulami i zadaniami. Informacje w tym artykule dotyczą poleceń cmdlet usługi Azure PowerShell w wersji 1.6.0.

Pełna lista poleceń cmdlet w usłudze Batch oraz szczegółowa składnia poleceń cmdlet znajdują się w [dokumentacji dotyczącej poleceń cmdlet w usłudze Azure Batch](https://msdn.microsoft.com/library/azure/mt125957.aspx). 


## Wymagania wstępne

* **Azure PowerShell** — Instrukcje dotyczące pobierania i instalacji programu Azure PowerShell znajdują się w temacie [How to install and configure Azure PowerShell](../powershell-install-configure.md) (Jak zainstalować i skonfigurować program Azure PowerShell). 
   
    * Ponieważ polecenia cmdlet usługi Azure Batch są dostarczane w module usługi Azure Resource Manager, trzeba będzie uruchomić polecenie cmdlet **Login-AzureRmAccount**, aby podłączyć się do subskrypcji. 
    
    * Zaleca się częstą aktualizację programu Azure PowerShell, aby mieć możliwość korzystania z aktualizacji i rozszerzeń usługi. 
    
* **Rejestrowanie w przestrzeni nazw dostawcy usługi Batch (operacja jednorazowa)** — przed rozpoczęciem pracy z kontami usługi Batch należy zarejestrować się w przestrzeni nazw dostawcy usługi Batch. Tę operację należy wykonać tylko raz w całym okresie obowiązywania subskrypcji. Uruchom następujące polecenie cmdlet:

        Register-AzureRMResourceProvider -ProviderNamespace Microsoft.Batch


## Zarządzanie kontami i kluczami usługi Batch

### Tworzenie konta usługi Batch

Polecenie **New-AzureRmBatchAccount** umożliwia utworzenie nowego konta usługi Batch w określonej grupie zasobów. Jeśli nie masz jeszcze grupy zasobów, utwórz ją przez uruchomienie polecenia cmdlet [New-AzureRmResourceGroup](https://msdn.microsoft.com/library/azure/mt603739.aspx), określając jeden z regionów platformy Azure w parametrze **Location** (Lokalizacja), np. „Środkowe stany USA”. Na przykład:


    New-AzureRmResourceGroup –Name MyBatchResourceGroup –location "Central US"


Następnie utwórz nowe konto usługi Batch w grupie zasobów, określając nazwę konta w parametrze <*account_name*> i lokalizację oraz nazwę grupy zasobów. Tworzenie konta usługi Batch może zająć nieco czasu. Na przykład:


    New-AzureRmBatchAccount –AccountName <account_name> –Location "Central US" –ResourceGroupName MyBatchResourceGroup

> [AZURE.NOTE] Nazwa konta usługi Batch musi być unikatowa dla regionu Azure dla grupy zasobów, zawierać od 3 do 24 znaków i tylko małe litery i cyfry.

### Pobieranie kluczy dostępu do konta
Polecenie **Get-AzureRmBatchAccountKeys** umożliwia wyświetlenie kluczy dostępu powiązanych z kontem usługi Azure Batch. Na przykład uruchom następujące polecenia, aby pobrać klucz podstawowy i klucz pomocniczy do utworzonego konta.

    $Account = Get-AzureRmBatchAccountKeys –AccountName <accountname>

    $Account.PrimaryAccountKey

    $Account.SecondaryAccountKey


### Generowanie nowego klucza dostępu
Polecenie **New-AzureRmBatchAccountKey** umożliwia generowanie nowego klucza podstawowego lub klucza pomocniczego do konta usługi Azure Batch. Na przykład, aby wygenerować nowy klucz podstawowy do konta usługi Batch, wpisz:


    New-AzureRmBatchAccountKey -AccountName <account_name> -KeyType Primary


> [AZURE.NOTE] Aby wygenerować nowy klucz pomocniczy, wpisz „Secondary” dla parametru **KeyType**. Ponowne generowanie klucza podstawowego i klucza pomocniczego należy wykonywać oddzielnie.

### Usuwanie konta usługi Batch
Polecenie **Remove-AzureRmBatchAccount** umożliwia usunięcie konta usługi Batch. Na przykład:


    Remove-AzureRmBatchAccount -AccountName <account_name>

Po wyświetleniu monitu potwierdź, że chcesz usunąć konto. Usunięcie konta może potrwać trochę czasu.

## Tworzenie obiektu BatchAccountContext

Aby uwierzytelniać się przy użyciu poleceń cmdlet programu PowerShell w usłudze Batch podczas tworzenia pul, zadań, podzadań oraz innych zasobów w usłudze Batch oraz zarządzania nimi, najpierw utwórz obiekt BatchAccountContext, aby przechować nazwę konta i klucze do konta:

    $context = Get-AzureRmBatchAccountKeys -AccountName <account_name>

Obiekt BatchAccountContext zostaje przeniesiony do poleceń cmdlet, które używają parametru **BatchContext**.

> [AZURE.NOTE] Domyślnie do uwierzytelniania używany jest klucz podstawowy do konta, ale można jawnie wybrać klucz do użycia przez zmianę właściwości obiektu BatchAccountContext **KeyInUse**: `$context.KeyInUse = "Secondary"`.



## Tworzenie i modyfikowanie zasobów usługi Batch
Do tworzenia zasobów w ramach konta usługi Batch służą takie polecenia cmdlet jak **New-AzureBatchPool**, **New-AzureBatchJob** oraz **New-AzureBatchTask**. Istnieją odpowiednie polecenia cmdlet **Get-** i **Set-** do aktualizacji właściwości istniejących zasobów oraz polecenia cmdlet **Remove-** do usuwania zasobów w ramach konta usługi Batch. 

Podczas korzystania z wielu tych poleceń cmdlet oprócz przekazywania obiektu BatchContext należy utworzyć lub przekazać obiekty, które zawierają szczegółowe ustawienia zasobów, jak pokazano w poniższym przykładzie. Dodatkowe przykłady zamieszczono w szczegółowych plikach pomocy każdego polecenia cmdlet.

### Tworzenie puli usługi Batch

Podczas tworzenia lub aktualizowania puli usługi Batch należy wybrać konfigurację usługi w chmurze lub konfigurację maszyny wirtualnej dla systemu operacyjnego węzłów obliczeniowych — zobacz artykuł [Batch feature overview](batch-api-basics.md#pool) (Omówienie funkcji usługi Batch). Wybór określa, czy na węzłach obliczeniowych są stosowane obrazy jednej z [wersji systemów operacyjnych gościa platformy Azure](../cloud-services/cloud-services-guestos-update-matrix.md#releases), czy jeden z obsługiwanych obrazów maszyn wirtualnych Linux lub Windows z portalu Azure Marketplace. 

Po uruchomieniu polecenia **New-AzureBatchPool** należy przekazać ustawienia systemu operacyjnego w obiekcie PSCloudServiceConfiguration lub PSVirtualMachineConfiguration. Na przykład poniższe polecenie cmdlet tworzy nową pulę usługi Batch z małymi węzłami obliczeniowymi w konfiguracji usługi w chmurze i obrazami najnowszej wersji systemu operacyjnego z rodziny 3 (Windows Server 2012). W tym miejscu parametr **CloudServiceConfiguration** określa zmienną *$configuration* jako obiekt PSCloudServiceConfiguration. Parametr **BatchContext** określa uprzednio zdefiniowaną zmienną *$context* jako obiekt BatchAccountContext.


    $configuration = New-Object -TypeName "Microsoft.Azure.Commands.Batch.Models.PSCloudServiceConfiguration" -ArgumentList @(3,"*")
    
    New-AzureBatchPool -Id "AutoScalePool" -VirtualMachineSize "Small" -CloudServiceConfiguration $configuration -AutoScaleFormula '$TargetDedicated=4;' -BatchContext $context

Docelowa liczba węzłów obliczeniowych w nowej puli jest określana przez formułę skalowania automatycznego. W takim przypadku formuła wygląda po prostu w taki sposób — **$TargetDedicated=4**, co oznacza, że liczba węzłów obliczeniowych w puli nie przekracza 4. 

## Zapytania dotyczące puli, zadań, podzadań oraz innych szczegółów

Takie polecenia cmdlet jak **Get-AzureBatchPool**, **Get-AzureBatchJob** oraz **Get-AzureBatchTask** służą do przesyłania zapytań dotyczących jednostek utworzonych w ramach konta usługi Batch.


### Zapytania dotyczące danych

Przykładowo polecenie **Get-AzureBatchPools** służy do znajdowania pul. Domyślnie umożliwia to przesłanie zapytań dotyczących wszystkich pul w ramach konta, zakładając, że obiekt BatchAccountContext został już zapisany w zmiennej *$context*:


    Get-AzureBatchPool -BatchContext $context

### Korzystanie z filtru OData

Można skonfigurować filtr OData przy użyciu parametru **Filtr** w taki sposób, by znajdowane były tylko obiekty, które interesują użytkownika. Np. można znaleźć wszystkie pule z identyfikatorami zaczynającymi się od „myPool”:


    $filter = "startswith(id,'myPool')"

    Get-AzureBatchPool -Filter $filter -BatchContext $context


Ta metoda nie jest tak elastyczna jak w przypadku korzystania z parametru „Where-Object” w lokalnym potoku. Jednak zapytanie zostaje przesłane bezpośrednio do usługi Batch, więc całe filtrowanie odbywa się po stronie serwera, co pozwala na oszczędność przepustowości internetowej.

### Korzystanie z parametru Id

Alternatywą dla filtru OData jest użycie parametru **Id**. Aby przesłać zapytanie dotyczące określonej puli o identyfikatorze „myPool”:


    Get-AzureBatchPool -Id "myPool" -BatchContext $context


Parametr **Id** obsługuje tylko wyszukiwanie pełnych identyfikatorów, a nie symboli wieloznacznych czy filtrów typu OData.



### Korzystanie z parametru MaxCount

Domyślnie każde polecenie cmdlet zwraca maksymalnie 1000 obiektów. W przypadku osiągnięcia tego limitu zmień ustawienia filtru w taki sposób, aby zwracał mniej obiektów, lub jawnie ustaw wartość maksymalną przy użyciu parametru **MaxCount** (Maksymalna liczba). Na przykład:


    Get-AzureBatchTask -MaxCount 2500 -BatchContext $context

Aby usunąć górną granicę, ustaw parametr **MaxCount** na wartość 0 lub mniejszą.

### Korzystanie z potoku

Polecenia cmdlet usługi Batch mogą użyć potoku programu PowerShell do przesyłania danych między poleceniami cmdlet. Powoduje to taki sam skutek co określenie parametru, ale sprawia, że umieszczanie na liście wielu jednostek jest łatwiejsze. Np. polecenie poniżej umożliwia znalezienie wszystkich podzadań w ramach konta:


    Get-AzureBatchJob -BatchContext $context | Get-AzureBatchTask -BatchContext $context


## Następne kroki
* Szczegóły składni poleceń cmdlet oraz przykłady znajdują się w [dokumentacji dotyczącej poleceń cmdlet w usłudze Azure Batch](https://msdn.microsoft.com/library/azure/mt125957.aspx).

* Więcej informacji na temat zmniejszenia liczby elementów oraz typu informacji zwracanych dla zapytań w usłudze Batch znajduje się w temacie [Query the Batch service efficiently](batch-efficient-list-queries.md) (Skuteczne wykonywanie zapytań w usłudze Batch). 



<!--HONumber=sep16_HO1-->


