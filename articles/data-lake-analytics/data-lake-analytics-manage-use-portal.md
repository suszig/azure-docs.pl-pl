---
title: "Zarządzanie usługą Azure Data Lake Analytics przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak zarządzać kont usługi Data Lake Analytics, źródła danych, użytkowników i zadań."
services: data-lake-analytics
documentationcenter: 
author: saveenr
manager: jhubbard
editor: cgronlun
ms.assetid: a0e045f1-73d6-427f-868d-7b55c10f811b
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.openlocfilehash: e49d1a0e0ccc6567d0a6841817667717ff5dba76
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-azure-data-lake-analytics-by-using-the-azure-portal"></a>Zarządzanie usługą Azure Data Lake Analytics przy użyciu portalu Azure
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Dowiedz się, jak zarządzanie kontami, źródłami danych konta, użytkowników i zadania usługi Azure Data Lake Analytics przy użyciu portalu Azure. Aby wyświetlić tematy dotyczące zarządzania o przy użyciu innych narzędzi, kliknij kartę w górnej części strony.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-lake-analytics-accounts"></a>Zarządzanie kontami usługi Data Lake Analytics

### <a name="create-an-account"></a>Tworzenie konta usługi

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. Kliknij przycisk **Nowy** > **Zbieranie danych i analiza** > **Data Lake Analytics**.
3. Wybierz wartości dla następujących elementów: 
   1. **Nazwa**: Nazwa konta usługi Data Lake Analytics.
   2. **Subskrypcja**: używane jako konto subskrypcji Azure.
   3. **Grupa zasobów**: Grupa zasobów Azure, w której chcesz utworzyć konto. 
   4. **Lokalizacja**: centrum danych Azure dla konta usługi Data Lake Analytics. 
   5. **Data Lake Store**: domyślny magazyn do użycia dla konta usługi Data Lake Analytics. Konto usługi Azure Data Lake Store i konta usługi Data Lake Analytics musi być w tej samej lokalizacji.
4. Kliknij przycisk **Utwórz**. 

### <a name="delete-a-data-lake-analytics-account"></a>Usuwanie konta usługi Data Lake Analytics

Aby usunąć konto usługi Data Lake Analytics, usuń jego domyślne konto usługi Data Lake Store.

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij polecenie **Usuń**.
3. Wpisz nazwę konta.
4. Kliknij polecenie **Usuń**.

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-data-sources"></a>Zarządzaj źródłami danych

Data Lake Analytics obsługuje następujące źródła danych:

* Data Lake Store
* Azure Storage

Eksplorator danych umożliwia przeglądanie źródeł danych i wykonywać operacje zarządzania pliku podstawowego. 

### <a name="add-a-data-source"></a>Dodawanie źródła danych

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij przycisk **źródeł danych**.
3. Kliknij przycisk **Dodaj źródło danych**.
    
   * Aby dodać konto usługi Data Lake Store, należy nazwę konta i dostęp do konta, aby można było go zapytania.
   * Aby dodać magazyn obiektów Blob platformy Azure, należy konto magazynu i klucza konta. Aby je znaleźć, przejdź do konta magazynu w portalu.

## <a name="set-up-firewall-rules"></a>Konfigurowanie reguł zapory

Data Lake Analytics do dalszego blokowania dostępu służy do swojego konta usługi Data Lake Analytics na poziomie sieci. Można włączyć zapory, podaj adres IP lub zdefiniuj zakres adresów IP dla zaufanych klientów. Po włączeniu tych środków tylko w przypadku klientów, którzy mają adresy IP ze zdefiniowanego zakresu można połączyć z magazynem.

Jeśli połączenie innymi usługami Azure, takich jak fabryki danych Azure lub maszyn wirtualnych, do konta usługi Data Lake Analytics, upewnij się, że **Zezwalaj usług Azure** włączono **na**. 

### <a name="set-up-a-firewall-rule"></a>Konfigurowanie reguł zapory

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. W menu po lewej stronie kliknij **zapory**.

## <a name="add-a-new-user"></a>Dodaj nowego użytkownika

Można użyć **Kreatora dodawania użytkownika** można łatwo udostępnić nowych użytkowników do usługi Data Lake.

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Po lewej stronie w obszarze **wprowadzenie**, kliknij przycisk **Kreatora dodawania użytkownika**.
3. Wybierz użytkownika, a następnie kliknij przycisk **wybierz**.
4. Wybierz rolę, a następnie kliknij przycisk **wybierz**. Aby skonfigurować nowy developer do użycia usługi Azure Data Lake, wybierz **Data Lake Analytics Developer** roli.
5. Wybierz z listy kontroli dostępu (ACL) dla baz danych U-SQL. Po zakończeniu wybrane opcje, kliknij przycisk **wybierz**.
6. Wybierz z listy kontroli dostępu dla plików. Domyślny magazyn, nie zmieniaj list ACL dla folderu głównego "/" i folderu/System. Kliknij pozycję **Wybierz**.
7. Przejrzyj wybrane ustawienia, a następnie kliknij przycisk **Uruchom**.
8. Po zakończeniu działania kreatora, kliknij przycisk **gotowe**.

## <a name="manage-role-based-access-control"></a>Zarządzanie kontrolą dostępu opartą na rolach

Jak innymi usługami Azure kontroli dostępu opartej na rolach (RBAC) służy do kontrolowania sposobu interakcji użytkowników z usługą.

Standardowe role RBAC mają następujące możliwości:
* **Właściciel**: można przesłać zadania, monitorowania zadań anulować zadania z dowolnego użytkownika i skonfiguruj konto.
* **Współautor**: można przesłać zadania, monitorowania zadań anulować zadania z dowolnego użytkownika i skonfiguruj konto.
* **Czytnik**: można monitorować zadania.

Użyj roli Data Lake Analytics Developer producentów U-SQL do korzystania z usługi Data Lake Analytics. Można użyć roli Data Lake Analytics Developer, aby:
* Przesyłanie zadań.
* Monitorowanie stanu zadania i postęp zadań wysłanego przez każdego użytkownika.
* Zobacz skryptów U-SQL z zadań wysłanego przez każdego użytkownika.
* Anulowanie tylko własnych zadań.

### <a name="add-users-or-security-groups-to-a-data-lake-analytics-account"></a>Dodawanie użytkowników lub grup zabezpieczeń do konta usługi Data Lake Analytics

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij przycisk **(IAM) kontroli dostępu** > **Dodaj**.
3. Wybierz rolę.
4. Dodawanie użytkownika.
5. Kliknij przycisk **OK**.

>[!NOTE]
>Jeśli użytkownik lub grupa zabezpieczeń wymaga przesyłania zadań, muszą uprawnienie dla konta magazynu. Aby uzyskać więcej informacji, zobacz [zabezpieczyć dane przechowywane w usłudze Data Lake Store](../data-lake-store/data-lake-store-secure-data.md).
>

<!-- ################################ -->
<!-- ################################ -->

## <a name="manage-jobs"></a>Zarządzanie zadaniami

### <a name="submit-a-job"></a>Prześlij zadanie

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.

2. Kliknij przycisk **nowe zadanie**. Dla każdego zadania należy skonfigurować:

    1. **Nazwa zadania**: Nazwa zadania.
    2. **Priorytet**: niższych numerach mają wyższy priorytet. Dwa zadania są umieszczane w kolejce, pierwszy uruchamia z niższą wartość priorytetu.
    3. **Równoległość**: Maksymalna liczba procesów obliczeń do zarezerwowania dla tego zadania.

3. Kliknij przycisk **Prześlij zadanie**.

### <a name="monitor-jobs"></a>Monitorowanie zadań

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij przycisk **wyświetlić wszystkie zadania**. Jest wyświetlana lista wszystkich zadań ostatnio zakończone i active w ramach konta.
3. Opcjonalnie kliknij **filtru** ułatwia znajdowanie zadania według **zakres czasu**, **Nazwa zadania**, i **autora** wartości. 

### <a name="monitoring-pipeline-jobs"></a>Monitorowanie zadań w potoku
Zadania, które są częścią potoku działają razem, zwykle po kolei, do wykonania danego scenariusza. Na przykład można mieć potok, który czyści, wyodrębnianie, przekształca, agreguje użycia klienta szczegółowe informacje o. Potok zadania są identyfikowane za pomocą właściwości "Potoku", gdy zadanie zostało przesłane. Zadania zaplanowane przy użyciu fabryki danych AZURE w wersji 2 ma automatycznie tej właściwości wypełnione. 

Aby wyświetlić listę zadań U-SQL, które są częścią potoków: 

1. W portalu Azure przejdź do konta usługi Data Lake Analytics.
2. Kliknij przycisk **zadania Insights**. Karta "Wszystkie zadania" jest ustawiana domyślnie, przedstawiający listę uruchomiona, w kolejce i zakończenia zadania.
3. Kliknij przycisk **zadania potoku** kartę. Lista zadań potoku pojawi się wraz z zagregowanych danych statystycznych dla każdego potoku.

### <a name="monitoring-recurring-jobs"></a>Monitorowanie zadań cyklicznych
Cyklicznych zadań to taki, który ma tej samej logiki biznesowej, ale używa różnych danych wejściowych w każdym uruchomieniu. W idealnym przypadku cyklicznych zadań należy zawsze powiodło się i mają względnie stały czas wykonania; monitorowanie zachowania te pomogą upewnij się, że zadanie jest w dobrej kondycji. Cykliczne zadania są identyfikowane za pomocą właściwości "Cyklu". Zadania zaplanowane przy użyciu fabryki danych AZURE w wersji 2 ma automatycznie tej właściwości wypełnione.

Aby wyświetlić listę zadań U-SQL, które są cykliczne: 

1. W portalu Azure przejdź do konta usługi Data Lake Analytics.
2. Kliknij przycisk **zadania Insights**. Karta "Wszystkie zadania" jest ustawiana domyślnie, przedstawiający listę uruchomiona, w kolejce i zakończenia zadania.
3. Kliknij przycisk **cyklicznych zadań** kartę. Lista zadań cyklicznych pojawi się wraz z zagregowanych danych statystycznych dla każdego zadania cyklicznego.

## <a name="manage-policies"></a>Zarządzanie zasadami

### <a name="account-level-policies"></a>Zasady na poziomie konta

Te zasady są stosowane do wszystkich zadań w ramach konta usługi Data Lake Analytics.

#### <a name="maximum-number-of-aus-in-a-data-lake-analytics-account"></a>Maksymalna liczba AUs w ramach konta usługi Data Lake Analytics
Zasady regulują całkowita liczba jednostek Analytics (AUs) można używać konta usługi Data Lake Analytics. Domyślna wartość wynosi 250. Na przykład, jeśli ta wartość jest równa 250 AUs, może mieć jedno zadanie uruchomione z 250 AUs przypisane do lub 10 zadań uruchamianych z 25 AUs każdego. Dodatkowe zadania, które są przesyłane są umieszczane w kolejce do momentu zakończenia są wykonywane zadania. Po zakończeniu operacji wykonywanych zadań są AUs są zwalniane dla zadań w kolejce do uruchomienia.

Aby zmienić liczbę AUs konto usługi Data Lake Analytics:

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij pozycję **Właściwości**.
3. W obszarze **maksymalna AUs**, przesuń suwak, aby wybrać wartość, lub wprowadź wartość w polu tekstowym. 
4. Kliknij pozycję **Zapisz**.

> [!NOTE]
> Jeśli potrzebujesz więcej niż domyślny (250) AUs, w portalu kliknij **pomoc + Obsługa** można przesłać żądania pomocy technicznej. Można zwiększyć liczbę AUs dostępne w ramach konta usługi Data Lake Analytics.
>

#### <a name="maximum-number-of-jobs-that-can-run-simultaneously"></a>Maksymalna liczba zadań, które można uruchomić jednocześnie
Zasady określa, ile zadania może uruchamiać jednocześnie. Domyślnie ta wartość wynosi 20. Jeśli usługi Data Lake Analytics ma AUs dostępne, nowe zadania zaplanowane do uruchomienia natychmiast, dopóki liczba uruchomionych zadań osiągnie wartość tej zasady. Po osiągnięciu maksymalna liczba zadań, które można uruchomić jednocześnie, kolejne zadania są umieszczane w kolejce w kolejności priorytetu do momentu ukończenia co najmniej jedno zadanie uruchomione (w zależności od dostępności Australia).

Aby zmienić liczbę zadań, które można uruchomić jednocześnie:

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij pozycję **Właściwości**.
3. W obszarze **maksymalny numer z uruchomione zadania**, przesuń suwak, aby wybrać wartość, lub wprowadź wartość w polu tekstowym. 
4. Kliknij pozycję **Zapisz**.

> [!NOTE]
> Jeśli musisz uruchomić więcej niż domyślny (20) liczba zadań, w portalu, kliknij przycisk **pomoc + Obsługa** można przesłać żądania pomocy technicznej. Można zwiększyć liczbę zadań, które można uruchomić jednocześnie w ramach konta usługi Data Lake Analytics.
>

#### <a name="how-long-to-keep-job-metadata-and-resources"></a>Jak długo, aby zachować zadanie metadanych i zasobów 
Po wykonaniu zadań U-SQL usługi Data Lake Analytics zachowuje wszystkie powiązane pliki. Powiązane pliki obejmują skrypt U-SQL, pliki DLL odwołuje się do skryptu U-SQL, skompilowanych zasobów i statystyki. Pliki znajdują się w folderze /system/ domyślnego konta usługi Azure Data Lake Storage. Ta zasada kontroluje, jak długo są przechowywane tych zasobów, zanim zostaną automatycznie usunięte (wartość domyślna to 30 dni). Można używać tych plików do debugowania i dostrajania wydajności zadań, które będzie można ponownie uruchomić w przyszłości.

Aby zmienić czas przechowywania metadanych zadania i zasoby:

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij pozycję **Właściwości**.
3. W obszarze **dni, aby zachować zadanie odpytuje**, przesuń suwak, aby wybrać wartość, lub wprowadź wartość w polu tekstowym.  
4. Kliknij pozycję **Zapisz**.

### <a name="job-level-policies"></a>Zasady na poziomie zadania
Za pomocą zasad na poziomie zadania można kontrolować maksymalny AUs i maksymalny priorytet poszczególnych użytkowników (lub członków określonych grup zabezpieczeń), które można ustawić dla zadania, które przesyłają. Umożliwia to kontrolę kosztów ponoszonych przez użytkowników. Umożliwia również kontroli wpływ, jaki zaplanowane zadania mogą mieć na zadania produkcji o wysokim priorytecie, które są uruchomione na tym samym koncie usługi Data Lake Analytics.

Data Lake Analytics ma dwie zasady, które można ustawić na poziomie zadania:

* **Australia limit dla zadania**: użytkownicy, można kierować tylko zadania, które mają tę liczbę AUs. Domyślnie ten limit, jest taka sama jak maksymalny limit Australia dla konta.
* **Priorytet**: użytkownicy, można kierować tylko zadania, które mają priorytet niższy niż lub równa tej wartości. Należy pamiętać, że większa liczba oznacza o niższym priorytecie. Domyślnie ta jest ustawiona na 1, która jest najwyższy możliwy priorytet.

Brak domyślnych zasad ustawić dla każdego konta. Domyślna zasada ma zastosowanie do wszystkich użytkowników, konta. Dodatkowe zasady można ustawić dla poszczególnych użytkowników i grup. 

> [!NOTE]
> Zasady na poziomie konta i zasad na poziomie zadania się jednocześnie.
>

#### <a name="add-a-policy-for-a-specific-user-or-group"></a>Dodaj zasady dla określonego użytkownika lub grupy

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij pozycję **Właściwości**.
3. W obszarze **limitów przesyłanie zadań**, kliknij przycisk **Dodaj zasady** przycisku. Następnie wybierz lub wprowadź następujące ustawienia:
    1. **Nazwa zasad obliczeniowe**: Wprowadź nazwę zasady w celu odnotowania zastosowania zasad.
    2. **Wybierz użytkownika lub grupy**: Wybierz użytkownika lub grupy, które dotyczą te zasady.
    3. **Ustaw Limit Australia zadania**: Ustaw limit Australia, która ma zastosowanie do wybranego użytkownika lub grupę.
    4. **Ustaw Limit priorytet**: Ustaw limit priorytet, która ma zastosowanie do wybranego użytkownika lub grupę.

4. Kliknij przycisk **OK**.

5. Nowa zasada zostanie wyświetlona w **domyślne** w sekcji tabela zasad **limitów przesyłanie zadań**. 

#### <a name="delete-or-edit-an-existing-policy"></a>Usuń lub Edytuj istniejące zasady

1. W portalu Azure przejdź do swojego konta usługi Data Lake Analytics.
2. Kliknij pozycję **Właściwości**.
3. W obszarze **limitów przesyłanie zadań**, Znajdź zasady, którą chcesz edytować.
4.  Aby wyświetlić **usunąć** i **Edytuj** kliknij przycisk Opcje w prawej kolumnie tabeli, **...** .

### <a name="additional-resources-for-job-policies"></a>Dodatkowe zasoby dotyczące zasad zadania
* [Omówienie zasad wpis w blogu](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-overview/)
* [Wpis w blogu zasad na poziomie konta](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-account-level-policy/)
* [Zasady na poziomie zadania wpis w blogu](https://blogs.msdn.microsoft.com/azuredatalake/2017/06/08/managing-your-azure-data-lake-analytics-compute-resources-job-level-policy/)

## <a name="next-steps"></a>Następne kroki

* [Omówienie usługi Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Rozpoczynanie pracy z usługą Data Lake Analytics przy użyciu portalu Azure](data-lake-analytics-get-started-portal.md)
* [Zarządzanie usługą Azure Data Lake Analytics przy użyciu programu Azure PowerShell](data-lake-analytics-manage-use-powershell.md)

