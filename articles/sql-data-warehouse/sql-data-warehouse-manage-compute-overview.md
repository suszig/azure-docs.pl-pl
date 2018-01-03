---
title: "Zarządzanie moc obliczeniową w usłudze Azure SQL Data Warehouse (omówienie) | Dokumentacja firmy Microsoft"
description: "Wydajność skalowania możliwości w usłudze Azure SQL Data Warehouse. Skalowanie w poziomie przez dostosowanie wartości dwu lub wstrzymywać i wznawiać zasobów obliczeniowych w celu ograniczenia kosztów."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: johnmac
editor: 
ms.assetid: e13a82b0-abfe-429f-ac3c-f2b6789a70c6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 3/23/2017
ms.author: elbutter
ms.openlocfilehash: 0d0d3b94fb50155ce0579d32e8ff78a47b9e3589
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/21/2017
---
# <a name="manage-compute-power-in-azure-sql-data-warehouse-overview"></a>Zarządzanie moc obliczeniową w usłudze Azure SQL Data Warehouse (omówienie)
> [!div class="op_single_selector"]
> * [Omówienie](sql-data-warehouse-manage-compute-overview.md)
> * [Portal](sql-data-warehouse-manage-compute-portal.md)
> * [Program PowerShell](sql-data-warehouse-manage-compute-powershell.md)
> * [REST](sql-data-warehouse-manage-compute-rest-api.md)
> * [TSQL](sql-data-warehouse-manage-compute-tsql.md)
>
>

Architektura usługi SQL Data Warehouse oddziela magazynu i zasobów obliczeniowych, umożliwiając niezależne skalowanie. W związku z tym obliczeniowe mogą być skalowane do wymagań dotyczących wydajności niezależnie od ilości danych. Fizyczne konsekwencją tej architektury jest to, że [rozliczeń] [ billed] dla zasobów obliczeniowych i magazynu jest oddzielona. 

Ten przegląd zawiera opis jak skalować w poziomie współpracuje z usługi SQL Data Warehouse i jak wykorzystywać wstrzymania, wznowienia i skalować możliwości usługi SQL Data Warehouse. 

## <a name="how-compute-management-operations-work-in-sql-data-warehouse"></a>Jak obliczeń operacje zarządzania działają w usłudze SQL Data Warehouse
Architektura magazynu danych SQL składa się z węzeł kontrolny, węzłów obliczeniowych i warstwy magazynu rozmieszczenie dystrybucje 60. 

Podczas normalnej sesji aktywnych w usłudze SQL Data Warehouse węzła głównego systemu zarządza metadanych i zawiera Optymalizator zapytań rozproszonych. Poniżej tego węzła głównego są węzłów obliczeniowych i warstwą magazynu. 400 DWU system ma jednego węzła głównego, czterech węzłów obliczeniowych i magazynu, składającego się z 60 dystrybucji. 

Przejście skali lub wstrzymać działanie, system najpierw Kasuje wszystkie zapytania przychodzące i wycofa następnie transakcji w celu zapewnienia spójnego stanu. Dla operacji skalowania skalowanie będzie miało miejsce tylko po zakończeniu tego transakcyjne wycofywania. Dla operacji skalowania w górę przepisy systemu nadmiarowe żądaną liczbę węzły obliczeniowe, a następnie rozpoczyna podłączenie węzłów obliczeniowych do warstwy magazynu. Dla operacji dół niepotrzebnych węzły są wydawane i pozostałe węzły obliczeniowe ponownie podłączyć się do odpowiedniej liczby dystrybucji. Dla operacji Wstrzymaj obliczeniowe wszystkie węzły są wydawane i systemu zostaną poddane różnych operacji na metadanych pozostawić końcowego systemu stabilna.

| DWU  | \#węzły obliczeniowe | \#dystrybucji na węzeł |
| ---- | ------------------ | ---------------------------- |
| 100  | 1                  | 60                           |
| 200  | 2                  | 30                           |
| 300  | 3                  | 20                           |
| 400  | 4                  | 15                           |
| 500  | 5                  | 12                           |
| 600  | 6                  | 10                           |
| 1000 | 10                 | 6                            |
| 1200 | 12                 | 5                            |
| 1500 | 15                 | 4                            |
| 2000 | 20                 | 3                            |
| 3000 | 30                 | 2                            |
| 6000 | 60                 | 1                            |

Trzy podstawowe funkcje zarządzania obliczeniowe są:

1. Wstrzymaj
2. Resume
3. Skalowanie

Każda z tych operacji może potrwać kilka minut. Jeśli jesteś skalowanie/wstrzymywanie/wznawianie automatycznie, można wdrożyć logikę, aby upewnić się, czy niektóre operacje zostały zakończone przed kontynuowaniem inną akcję. 

Sprawdzanie stanu bazy danych za pośrednictwem różnych punktów końcowych pozwoli prawidłowo zaimplementować automatyzacji takich operacji. Portalu zapewni powiadomienie po zakończeniu operacji i bazy danych bieżący stan, ale nie jest możliwe programowe sprawdzania stanu. 

>  [!NOTE]
>
>  Funkcja zarządzania nie istnieje we wszystkich punktów końcowych obliczeń.
>
>  

|              | Wstrzymanie/wznowienie | Skalowanie | Sprawdź stan bazy danych |
| ------------ | ------------ | ----- | -------------------- |
| Azure Portal | Yes          | Yes   | **Nie**               |
| PowerShell   | Yes          | Yes   | Yes                  |
| Interfejs API REST     | Yes          | Yes   | Yes                  |
| T-SQL        | **Nie**       | Yes   | Yes                  |



<a name="scale-compute-bk"></a>

## <a name="scale-compute"></a>Skalowanie możliwości obliczeniowych

Mierzona jest wydajność w usłudze SQL Data Warehouse w [jednostki magazynu danych (dwu)] [magazynu danych (dwu) jednostki] czyli abstracted miary zasoby obliczeniowe, takie jak procesor CPU, pamięci i we/wy przepustowości. Użytkownik, który chce skalowania wydajności ich systemu to zrobić za pomocą różnych środków, takich jak za pośrednictwem portalu, T-SQL i interfejsów API REST. 

### <a name="how-do-i-scale-compute"></a>Jak skalować obliczeń?
Obliczeń zasilania odbywa się SQL Data Warehouse, zmieniając ustawienie wartości DWU. Zwiększona wydajność liniowo jak dodać więcej DWU dla niektórych operacji.  Firma Microsoft oferuje ofert DWU, zapewniający, że gdy system skalować w górę lub w dół znacznie zmienić wydajność. 

Aby dostosować liczbę jednostek dwu, można użyć dowolnej z tych poszczególnych metod.

* [Skala obliczeniowe zasilania z portalu Azure][Scale compute power with Azure portal]
* [Skala obliczeniowe zasilania przy użyciu programu PowerShell][Scale compute power with PowerShell]
* [Moc obliczeniową skali z interfejsów API REST][Scale compute power with REST APIs]
* [Skala obliczeniowe zasilania przy użyciu języka TSQL][Scale compute power with TSQL]

### <a name="how-many-dwus-should-i-use"></a>Liczbę jednostek dwu należy używać?

Aby przekonać się, jaka jest idealna wartość DWU, skaluj w górę i w dół oraz uruchom kilka zapytań po załadowaniu danych. Ponieważ skalowanie odbywa się szybko, możesz wypróbować różne poziomy wydajności w ciągu godziny lub mniej. 

> [!Note] 
> Usługa SQL Data Warehouse zaprojektowano w celu przetwarzania dużych ilości danych. Aby wyświetlić jego wartość true, możliwości skalowania, zwłaszcza na większą liczbę jednostek dwu, chcesz użyć dużych zestawów danych, które zbliża się lub przekracza 1 TB.

Zalecenia dotyczące znajdowania wartość DWU najlepsze dla obciążenia:

1. Dla magazynu danych na programowanie rozpocząć wybierając mniejszą wydajnością DWU.  Dobry punkt wyjścia jest DW400 lub DW200.
2. Monitorowanie wydajności aplikacji, obserwowania liczby jednostek dwu wybrane w porównaniu do wydajności, które należy obserwować.
3. Określ, ile szybciej lub wolniej wydajność powinna być można osiągnąć poziom optymalną wydajność do wymagań przejmując skali liniowej.
4. Zwiększanie lub zmniejszanie liczby jednostek dwu proporcjonalnie do jak znacznie szybciej lub wolniej ma obciążenia do wykonania. 
5. Kontynuuj, wprowadzić zmiany, aż do uzyskania optymalnej wydajności poziom dla potrzeb biznesowych.

> [!NOTE]
>
> Wydajność zapytań tylko zwiększa się wraz z paralelizacja więcej jeśli pracy może zostać podzielony między węzły obliczeń. Jeśli okaże się, że skalowanie nie zmienia się na wydajność, można znaleźć na naszych dostrajanie artykuły, aby sprawdzić, czy dane jest nierównomiernie dystrybuowane lub jeśli udostępniono dużą ilość ruchu danych wydajności. 

### <a name="when-should-i-scale-dwus"></a>Kiedy skalować liczbę jednostek dwu?
Skalowanie jednostek dwu powoduje zmianę następujących ważne scenariusze:

1. Liniowo Zmiana wydajności systemu pod kątem skanowania, agregacji i CTAS — instrukcje
2. Zwiększenie liczby czytelników i zapisywania podczas ładowania przy użyciu programu PolyBase
3. Maksymalna liczba równoczesnych zapytań i gniazda współbieżności

Zalecenia dotyczące Kiedy skalować liczbę jednostek dwu:

1. Przed wykonaniem operacji ładowania i przekształcania dużej ilości danych, skalowanie w górę liczbę jednostek dwu tak, aby szybciej Twoje dane są dostępne.
2. W godzinach szczytu skalowanie w celu uwzględnienia większej liczby równoczesnych zapytań. 

<a name="pause-compute-bk"></a>

## <a name="pause-compute"></a>Wstrzymaj obliczeń
[!INCLUDE [SQL Data Warehouse pause description](../../includes/sql-data-warehouse-pause-description.md)]

Aby wstrzymać bazy danych, użyj jednej z tych poszczególnych metod.

* [Wstrzymaj obliczeń z portalu Azure][Pause compute with Azure portal]
* [Wstrzymaj obliczeń przy użyciu programu PowerShell][Pause compute with PowerShell]
* [Wstrzymaj obliczeń z interfejsów API REST][Pause compute with REST APIs]

<a name="resume-compute-bk"></a>

## <a name="resume-compute"></a>Wznów obliczeń
[!INCLUDE [SQL Data Warehouse resume description](../../includes/sql-data-warehouse-resume-description.md)]

Aby wznowić działanie bazy danych, użyj jednej z tych poszczególnych metod.

* [Wznów obliczeń z portalu Azure][Resume compute with Azure portal]
* [Wznów obliczeń przy użyciu programu PowerShell][Resume compute with PowerShell]
* [Wznów obliczeń z interfejsów API REST][Resume compute with REST APIs]

<a name="check-compute-bk"></a>

## <a name="check-database-state"></a>Sprawdź stan bazy danych 

Aby wznowić działanie bazy danych, użyj jednej z tych poszczególnych metod.

- [Sprawdź stan bazy danych z T-SQL][Check database state with T-SQL]
- [Sprawdź stan bazy danych przy użyciu programu PowerShell][Check database state with PowerShell]
- [Sprawdź stan bazy danych z interfejsów API REST][Check database state with REST APIs]

## <a name="permissions"></a>Uprawnienia

Skalowanie bazy danych musi mieć uprawnienia opisanego w [ALTER DATABASE][ALTER DATABASE].  Wstrzymywanie i wznawianie wymagają [Współautor bazy danych SQL] [ SQL DB Contributor] uprawnienia, w szczególności Microsoft.Sql/servers/databases/action.

<a name="next-steps-bk"></a>

## <a name="next-steps"></a>Kolejne kroki
Zobacz następujące artykuły, aby ułatwić zrozumienie pojęcia niektóre dodatkowe wydajności:

* [Zarządzanie obciążenia i współbieżność][Workload and concurrency management]
* [Omówienie projektowania tabeli][Table design overview]
* [Dystrybucja tabeli][Table distribution]
* [Indeksowanie tabeli][Table indexing]
* [Partycjonowanie tabel][Table partitioning]
* [Statystyk tabeli][Table statistics]
* [Najlepsze praktyki][Best practices]

<!--Image reference-->

<!--Article references-->
[billed]: https://azure.microsoft.com/pricing/details/sql-data-warehouse/
[Scale compute power with Azure portal]: ./sql-data-warehouse-manage-compute-portal.md#scale-compute-power
[Scale compute power with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#scale-compute-bk
[Scale compute power with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#scale-compute-bk
[Scale compute power with TSQL]: ./sql-data-warehouse-manage-compute-tsql.md#scale-compute-bk

[capacity limits]: ./sql-data-warehouse-service-capacity-limits.md

[Pause compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#pause-compute-bk
[Pause compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#pause-compute-bk
[Pause compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#pause-compute-bk

[Resume compute with Azure portal]:  ./sql-data-warehouse-manage-compute-portal.md#resume-compute-bk
[Resume compute with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#resume-compute-bk
[Resume compute with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#resume-compute-bk

[Check database state with T-SQL]: ./sql-data-warehouse-manage-compute-tsql.md#check-database-state-and-operation-progress
[Check database state with PowerShell]: ./sql-data-warehouse-manage-compute-powershell.md#check-database-state
[Check database state with REST APIs]: ./sql-data-warehouse-manage-compute-rest-api.md#check-database-state

[Workload and concurrency management]: ./sql-data-warehouse-develop-concurrency.md
[Table design overview]: ./sql-data-warehouse-tables-overview.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexing]: ./sql-data-warehouse-tables-index.md
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Table statistics]: ./sql-data-warehouse-tables-statistics.md
[Best practices]: ./sql-data-warehouse-best-practices.md
[development overview]: ./sql-data-warehouse-overview-develop.md

[SQL DB Contributor]: ../active-directory/role-based-access-built-in-roles.md#sql-db-contributor

<!--MSDN references-->
[ALTER DATABASE]: https://msdn.microsoft.com/library/mt204042.aspx

<!--Other Web references-->
[Azure portal]: http://portal.azure.com/
