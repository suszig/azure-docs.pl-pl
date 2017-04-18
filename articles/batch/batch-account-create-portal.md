---
title: "Tworzenie konta usługi Batch w witrynie Azure Portal | Microsoft Docs"
description: "Dowiedz się, jak utworzyć konto usługi Azure Batch w portalu Azure w celu równoległego uruchamiania dużych obciążeń w chmurze"
services: batch
documentationcenter: 
author: tamram
manager: timlt
editor: 
ms.assetid: 3fbae545-245f-4c66-aee2-e25d7d5d36db
ms.service: batch
ms.workload: big-compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/27/2017
ms.author: tamram
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 6ea03adaabc1cd9e62aa91d4237481d8330704a1
ms.openlocfilehash: 11f8c3f37e56e0b5c566c4abdb60697c5279e72a
ms.lasthandoff: 04/06/2017


---
# <a name="create-a-batch-account-with-the-azure-portal"></a>Tworzenie konta usługi Batch w witrynie Azure Portal

> [!div class="op_single_selector"]
> * [Azure Portal](batch-account-create-portal.md)
> * [Batch Management .NET](batch-management-dotnet.md)
> 
> 

Dowiedz się, jak utworzyć konto usługi Azure Batch w witrynie [Azure Portal][azure_portal] i wybrać właściwości konta pasujące do scenariusza obliczeniowego. Dowiedz się, gdzie znaleźć ważne właściwości konta, takie jak klucze dostępu i adresy URL konta. 

Ogólne informacje o kontach usługi Batch i scenariuszach można znaleźć w [omówieniu funkcji](batch-api-basics.md).



## <a name="create-a-batch-account"></a>Tworzenie konta usługi Batch

Aby utworzyć konto usługi Batch, użyj portalu w jednym z dwóch *trybów alokacji puli*: trybu **usługi Batch** lub nowszego trybu **subskrypcji użytkownika**, który wymaga większej liczby czynności konfiguracyjnych. Informacje o tych dwóch trybach można znaleźć w [omówieniu funkcji](batch-api-basics.md#account). W przypadku funkcji trybu subskrypcji użytkownika zobacz też [wpis w blogu](https://blogs.technet.microsoft.com/windowshpc/2017/03/17/azure-batch-vnet-and-custom-image-support-for-virtual-machine-pools/).

## <a name="batch-service-mode"></a>Tryb usługi Batch



1. Zaloguj się w witrynie [Azure Portal][azure_portal].
2. Kliknij kolejno opcje **Nowy** > **Obliczenia** > **Usługa Batch**.
   
    ![Usługa Batch w witrynie Marketplace][marketplace_portal]
3. Wyświetlony zostanie blok **Nowe konto usługi Batch**. Zobacz poniżej opisy każdego elementu bloku.
   
    ![Tworzenie konta usługi Batch][account_portal]
   
    a. **Nazwa konta**: wybrana nazwa konta usługi Batch musi być unikatowa w obrębie regionu świadczenia usługi Azure, w którym konto zostanie utworzone (zobacz **Lokalizacja** poniżej). Nazwa konta może zawierać tylko małe litery lub cyfry i musi mieć od 3 do 24 znaków.
   
    b. **Subskrypcja**: subskrypcja, w której ma zostać utworzone konto usługi Batch. Jeśli masz tylko jedną subskrypcję, jest ona wybrana domyślnie.

    c. **Tryb alokacji puli**: wybierz **Usługa Batch**.
   
    c. **Grupa zasobów**: wybierz istniejącą grupę zasobów dla nowego konta usługi Batch. Opcjonalnie można utworzyć nową grupę zasobów.
   
    d. **Lokalizacja**: region świadczenia usługi Azure, w którym ma zostać utworzone konto usługi Batch. Tylko regiony obsługiwane przez subskrypcję i grupę zasobów są wyświetlane jako opcje.
   
    e. **Konto magazynu** (opcjonalne): konto usługi Azure Storage ogólnego przeznaczenia skojarzone z kontem usługi Batch. Jest to zalecane w przypadku większości kont usługi Batch. Więcej szczegółów można znaleźć w sekcji [Połączone konto usługi Azure Storage](#linked-azure-storage-account) w dalszej części tego artykułu.

4. Kliknij przycisk **Utwórz**, aby utworzyć konto.
   
   Portal wskazuje, że wdrożenie jest w toku. Po zakończeniu w obszarze **Powiadomienia** pojawia się powiadomienie **Wdrożenie zakończyło się pomyślnie**.
   
## <a name="user-subscription-mode"></a>Tryb subskrypcji użytkownika

### <a name="allow-azure-batch-to-access-the-subscription-one-time-operation"></a>Umożliwia usłudze Azure Batch dostęp do subskrypcji (jednorazowa operacja)
Podczas tworzenia pierwszego konta usługi Batch w trybie subskrypcji użytkownika wykonaj następujące czynności, aby zarejestrować subskrypcję w usłudze Batch. (Jeśli wcześniej zostało to już zrobione, przejdź do następnej sekcji).

1. Zaloguj się w witrynie [Azure Portal][azure_portal].

2. Kliknij pozycję **Więcej usług** > **Subskrypcje**, a następnie kliknij subskrypcję, której chcesz użyć dla konta usługi Batch. 

3. W bloku **Subskrypcja** kliknij pozycję **Kontrola dostępu (IAM)** > **Dodaj**.

    ![Kontrola dostępu do subskrypcji][subscription_access]

4. W bloku **Dodawanie uprawnień** wybierz rolę **Współautor** i wyszukaj ciąg **MicrosoftAzureBatch** (bez spacji). Wybierz pozycję **MicrosoftAzureBatch**, a następnie kliknij przycisk **Zapisz**.

    ![Dodawanie uprawnień usługi Batch][add_permission]

### <a name="create-a-key-vault"></a>Tworzenie magazynu kluczy
W trybie subskrypcji użytkownika wymagana jest usługa Azure Key Vault należąca do tej samej grupy zasobów, co konto usługi Batch, które ma zostać utworzone. Upewnij się, że grupa zasobów znajduje się w regionie, gdzie usługa Batch jest [dostępna](https://azure.microsoft.com/regions/services/) i który obsługuje Twoja subskrypcja.

1. W witrynie [Azure Portal][azure_portal] kliknij pozycję **Nowy** > **Bezpieczeństwo i obsługa tożsamości** > **Key Vault**. 

2. W bloku **Tworzenie magazynu Key Vault** wprowadź nazwę magazynu Key Vault i utwórz grupę zasobów w wymaganym regionie konta usługi Batch. Pozostaw wartości domyślne pozostałych ustawień, a następnie kliknij przycisk **Utwórz**.

### <a name="create-a-batch-account"></a>Tworzenie konta usługi Batch

1. W witrynie [Azure Portal][azure_portal] kliknij pozycje **Nowy** > **Obliczanie** > **Usługa Batch**.
   
    ![Usługa Batch w witrynie Marketplace][marketplace_portal]
3. Wyświetlony zostanie blok **Nowe konto usługi Batch**. Zobacz poniżej opisy każdego elementu bloku.
   
    ![Tworzenie konta usługi Batch][account_portal_byos]
   
    a. **Nazwa konta**: wybrana nazwa konta usługi Batch musi być unikatowa w obrębie regionu świadczenia usługi Azure, w którym konto zostanie utworzone (zobacz **Lokalizacja** poniżej). Nazwa konta może zawierać tylko małe litery lub cyfry i musi mieć od 3 do 24 znaków.
   
    b. **Subskrypcja**: jeśli masz więcej niż jedną subskrypcję, wybierz tę, która została zarejestrowana za pomocą usługi Batch.

    c. **Tryb alokacji puli**: wybierz pozycję **Subskrypcja użytkownika**.

    d. **Magazyn kluczy**: wybierz magazyn kluczy utworzony dla konta usługi Batch w poprzedniej sekcji. Opcjonalnie utwórz nowy magazyn kluczy. Po wybraniu magazynu zaznacz pole wyboru, aby udzielić usłudze Azure Batch dostępu do magazynu kluczy.
   
    c. **Grupa zasobów**: wybierz grupę zasobów, w której został utworzony magazyn kluczy.
   
    d. **Lokalizacja**: region świadczenia usługi Azure, w którym został utworzony magazyn kluczy dla konta usługi Batch. 
   
    e. **Konto magazynu** (opcjonalne): konto usługi Azure Storage ogólnego przeznaczenia skojarzone z kontem usługi Batch. Jest to zalecane w przypadku większości kont usługi Batch. Więcej szczegółów zamieszczono w sekcji [Połączone konto usługi Azure Storage](#linked-azure-storage-account) poniżej.

4. Kliknij przycisk **Utwórz**, aby utworzyć konto.
   
   Portal wskazuje, że wdrożenie jest w toku. Po zakończeniu w obszarze **Powiadomienia** pojawia się powiadomienie **Wdrożenie zakończyło się pomyślnie**.



## <a name="view-batch-account-properties"></a>Wyświetlanie właściwości konta usługi Batch
Po utworzeniu konta możesz otworzyć **Blok konta usługi Batch**, aby uzyskać dostęp do jego ustawień i właściwości. Menu po lewej stronie bloku konta usługi Batch zapewnia dostęp do wszystkich ustawień i właściwości konta.

![Blok konta usługi Batch w witrynie Azure Portal][account_blade]

* **Adres URL konta usługi Batch**: aby po utworzeniu aplikacji za pomocą [interfejsów API usługi Batch](batch-apis-tools.md#batch-development-apis) uzyskać dostęp do zasobów usługi Batch, potrzebny będzie adres URL konta. Adres URL konta usługi Batch ma następujący format:
  
    `https://<account_name>.<region>.batch.azure.com`

![Adres URL konta usługi Batch w portalu][account_url]

* **Klucze dostępu** (tryb usługi Batch): Do uwierzytelniania dostępu do konta usługi Batch z aplikacji potrzebny będzie klucz dostępu do konta. (To ustawienie nie jest dostępne w trybie subskrypcji użytkownika, w którym używasz uwierzytelniania usługi Azure Active Directory).

    Aby wyświetlić lub ponownie wygenerować klucze dostępu konta usługi Batch, wprowadź wartość `keys` w polu **Wyszukaj** lewego menu w bloku konta usługi Batch, a następnie wybierz opcję **Klucze**. 
  
    ![Klucze konta usługi Batch w witrynie Azure Portal][account_keys]

[!INCLUDE [batch-pricing-include](../../includes/batch-pricing-include.md)]

## <a name="linked-azure-storage-account"></a>Połączone konto usługi Azure Storage

Możesz opcjonalnie połączyć konto usługi Azure Storage ogólnego przeznaczenia z kontem usługi Batch. Funkcja [pakietów aplikacji](batch-application-packages.md) usługi Batch korzysta z usługi Azure Blob Storage, podobnie jak biblioteka [.NET Batch File Conventions](batch-task-output.md). Te opcjonalne funkcje pomagają we wdrażaniu aplikacji uruchamianych przez zadania usługi Batch oraz utrwalaniu wytwarzanych przez nich danych.

Zaleca się utworzenie nowego konta usługi Storage do wyłącznego użytku przez konto usługi Batch.

![Tworzenie konta magazynu „ogólnego przeznaczenia”][storage_account]

> [!NOTE] 
> Usługa Azure Batch obsługuje obecnie tylko konta usługi Storage ogólnego przeznaczenia. Ten typ konta opisano w kroku 5 sekcji [Tworzenie konta magazynu] (../storage/storage-create-storage-account.md#create-a-storage-account) w artykule [Informacje o kontach magazynu Azure](../storage/storage-create-storage-account.md).
>
>

> [!WARNING]
> Należy zachować ostrożność przy ponownym generowaniu kluczy dostępu połączonego konta usługi Storage. Wygeneruj ponownie tylko jeden klucz konta usługi Storage i kliknij przycisk **Zsynchronizuj klucze** w bloku połączonego konta usługi Storage. Poczekaj 5 minut, aby umożliwić propagację kluczy do węzłów obliczeniowych w swoich pulach, a następnie w razie potrzeby ponownie wygeneruj i zsynchronizuj drugi klucz. Jeśli w tym samym czasie zostaną ponownie wygenerowane oba klucze, węzły obliczeniowe nie będą mogły zsynchronizować żadnego z nich i w ten sposób utracą dostęp do konta usługi Storage.
> 
> 

![Ponowne generowanie kluczy konta magazynu][4]

## <a name="batch-service-quotas-and-limits"></a>Limity przydziału i limity usługi Batch
Należy pamiętać, że podobnie jak w przypadku subskrypcji i innych usług Azure, do kont usługi Batch mają zastosowanie określone [limity przydziału i limity](batch-quota-limit.md). Bieżące limity przydziału dla konta usługi Batch są wyświetlane w portalu w obszarze **Właściwości** konta.

![Limity przydziału konta usługi Batch w witrynie Azure Portal][quotas]



Ponadto wiele z tych limitów przydziału można powiększyć po prostu przy użyciu bezpłatnego żądania pomocy technicznej przesłanego w witrynie Azure Portal. Szczegóły dotyczące żądań zwiększenia limitów przydziału zamieszczono w artykule [Quotas and limits for the Azure Batch service](batch-quota-limit.md) (Limity przydziału i limity dla usługi Azure Batch).

## <a name="other-batch-account-management-options"></a>Inne opcje zarządzania kontem usługi Batch
Poza korzystaniem z witryny Azure Portal można również utworzyć konta usługi Batch i zarządzać nimi przy użyciu następujących opcji:

* [Polecenia cmdlet programu PowerShell usługi Batch](batch-powershell-cmdlets-get-started.md)
* [Interfejs wiersza polecenia platformy Azure](batch-cli-get-started.md)
* [Batch Management .NET](batch-management-dotnet.md)

## <a name="next-steps"></a>Następne kroki
* Aby dowiedzieć się więcej o zasadach działania i funkcjach usługi Batch, zobacz temat [Omówienie funkcji usługi Batch](batch-api-basics.md). W artykule omówiono podstawowe zasoby usługi Batch, takie jak pule, węzły obliczeniowe i zadania, oraz opisano funkcje, które umożliwiają wykonywanie obciążeń zasobów obliczeniowych na dużą skalę.
* Poznaj podstawy tworzenia aplikacji wykorzystujących usługę Batch za pomocą biblioteki klienta [Batch .NET](batch-dotnet-get-started.md) lub języka [Python](batch-python-tutorial.md). Niniejsze artykuły wprowadzające zawierają omówienie działającej aplikacji, która korzysta z usługi Batch do wykonywania obciążenia na wielu węzłach obliczeniowych i stosuje usługę Azure Storage do tymczasowego przechowywania i pobierania pliku obciążenia.

[api_net]: https://msdn.microsoft.com/library/azure/mt348682.aspx
[api_rest]: https://msdn.microsoft.com/library/azure/Dn820158.aspx

[azure_portal]: https://portal.azure.com
[batch_pricing]: https://azure.microsoft.com/pricing/details/batch/

[4]: ./media/batch-account-create-portal/batch_acct_04.png "Ponowne generowanie kluczy konta magazynu"
[marketplace_portal]: ./media/batch-account-create-portal/marketplace_batch.PNG
[account_blade]: ./media/batch-account-create-portal/batch_blade.png
[account_portal]: ./media/batch-account-create-portal/batch_acct_portal.png
[account_keys]: ./media/batch-account-create-portal/account_keys.PNG
[account_url]: ./media/batch-account-create-portal/account_url.png
[storage_account]: ./media/batch-account-create-portal/storage_account.png
[quotas]: ./media/batch-account-create-portal/quotas.png
[subscription_access]: ./media/batch-account-create-portal/subscription_iam.png
[add_permission]: ./media/batch-account-create-portal/add_permission.png
[account_portal_byos]: ./media/batch-account-create-portal/batch_acct_portal_byos.png
