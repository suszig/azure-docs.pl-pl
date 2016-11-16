---
title: "Omówienie bazy danych Stretch | Microsoft Docs"
description: "Dowiedz się, jak w sposób niewidoczny i bezpieczny usługa Stretch Database migruje zimne dane do chmury platformy Microsoft Azure."
services: sql-server-stretch-database
documentationcenter: 
author: douglaslMS
manager: jhubbard
editor: 
ms.assetid: c360dc10-a02b-446f-91a0-278358f7a297
ms.service: sql-server-stretch-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/27/2016
ms.author: douglasl
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 2fcea994b3de924f1170dae2d7a0a3b6d4786538


---
# <a name="stretch-database-overview"></a>Omówienie bazy danych Stretch
Usługa Stretch Database migruje zimne dane do chmury platformy Microsoft Azure w sposób niewidoczny i bezpieczny.

Jeśli chcesz od razu rozpocząć pracę z bazą danych Stretch, zobacz artykuł [Get started by running the Enable Database for Stretch Wizard](sql-server-stretch-database-wizard.md) (Rozpoczęcie pracy przez uruchomienie Kreatora włączania bazy danych do usługi Stretch).

## <a name="what-are-the-benefits-of-stretch-database"></a>Jakie są zalety bazy danych Stretch?
Baza danych Stretch zapewnia następujące korzyści:

### <a name="provides-costeffective-availability-for-cold-data"></a>Zapewnia efektywną kosztowo dostępność zimnych danych
Przy pomocy usługi SQL Server Stretch Database możesz dynamicznie rozciągnąć ciepłe i zimne dane transakcyjne z programu SQL Server na platformę Microsoft Azure. W odróżnieniu od typowych magazynów zimnych danych dane są zawsze online i dostępne dla zapytań. Można tanim kosztem zapewnić przechowywanie przez dłuższy danych z dużych tabel, np. historii zamówień klienta. Skorzystaj z taniej platformy Azure, zamiast skalować drogi magazyn lokalny. Aby zachować kontrolę nad kosztami, należy wybrać warstwę cenową i skonfigurować ustawienia w witrynie Azure Portal. Skaluj w górę lub w dół w zależności od potrzeb. Więcej szczegółów znajduje się na stronie [SQL Server Stretch Database — cennik](https://azure.microsoft.com/pricing/details/sql-server-stretch-database/).

### <a name="doesnt-require-changes-to-queries-or-applications"></a>Nie wymaga zmian w zapytaniach ani aplikacjach
Uzyskuj bezproblemowy dostęp do danych programu SQL Server niezależnie od tego, czy są to dane lokalne, czy rozciągnięte do chmury.  Ustaw zasady określające, gdzie są przechowywane dane, a program SQL Server będzie obsługiwać ruch danych w tle. Cała tabela jest zawsze online oraz umożliwia zadawanie zapytań. Baza danych Stretch nie wymaga żadnych zmian w istniejących zapytaniach ani aplikacjach — lokalizacja danych jest całkowicie niewidoczna dla aplikacji.

### <a name="streamlines-onpremises-data-maintenance"></a>Usprawnienie konserwacji lokalnych danych
Ogranicz lokalne czynności konserwacyjne i wymagania dotyczące magazynowania danych. Tworzenie kopii zapasowych danych lokalnych jest szybsze i nie trwa dłużej niż czynności konserwacyjne. Kopie zapasowe porcji danych w chmurze są tworzone automatycznie. Wymagania magazynu lokalnego zostają znacznie ograniczone. Usługa Azure Storage może być o 80% tańsza niż dodawanie lokalnych dysków SSD.

### <a name="keeps-your-data-secure-even-during-migration"></a>Zapewnia bezpieczeństwo danych nawet podczas migracji
Ciesz się spokojem w trakcie rozciągania najważniejszych aplikacji do chmury. Opcja Zawsze zaszyfrowane programu SQL Server zapewnia szyfrowanie danych w ruchu. Zabezpieczenia na poziomie wiersza oraz inne zaawansowane funkcje zabezpieczeń programu SQL Server również współpracują z bazą danych Stretch w celu ochrony danych.

## <a name="what-does-stretch-database-do"></a>Do czego służy baza danych Stretch?
Po włączeniu usługi Stretch Database dla wystąpienia programu SQL Server, bazy danych i co najmniej jednej tabeli usługa ta zaczyna w trybie cichym migrować zimne dane na platformę Azure.

* W przypadku przechowywania zimnych danych w osobnej tabeli można migrować całą tabelę.
* Jeśli tabela zawiera zarówno gorące, jak i zimne dane, można określić funkcję filtru, aby wybrać wiersze do migracji.

**Nie trzeba wprowadzać zmian w istniejących zapytaniach ani aplikacjach klienckich.** Można nadal mieć stały dostęp zarówno do danych lokalnych, jak i zdalnych — nawet podczas migracji danych. W przypadku zapytań zdalnych występuje niewielkie opóźnienie, ale ma ono miejsce tylko wtedy, gdy zapytanie dotyczy zimnych danych.

**Usługa Stretch Database gwarantuje, że żadne dane nie zostaną utracone**, jeśli podczas migracji wystąpi błąd. Obejmuje ona również logikę ponawiania do obsługi problemów z połączeniem, które mogą wystąpić podczas migracji. Widok dynamiczny zarządzania zawiera informacje o stanie migracji.

**Migracja danych może zostać wstrzymana** w celu rozwiązania problemów na serwerze lokalnym lub w celu maksymalnego zwiększenia dostępnej przepustowości sieci.

![Omówienie bazy danych Stretch][StretchOverviewImage1]

## <a name="is-stretch-database-for-you"></a>Czy potrzebujesz bazy danych Stretch?
Jeśli następujące stwierdzenia są prawdziwe, baza danych Stretch może ułatwić spełnienie wymagań i rozwiązanie problemów.

| Jesteś osobą podejmującą decyzje | Jesteś administratorem baz danych |
| --- | --- |
| Muszę przez długi czas przechowywać dane transakcyjne. |Nie panuję już nad rozmiarem tabel. |
| Czasami muszę wykonywać zapytania dotyczące zimnych danych. |Moim użytkownikom zależy na dostępie do zimnych danych, ale rzadko ich używają. |
| Mam aplikacje, w tym starsze wersje, których nie chcę aktualizować. |Muszę ciągle kupować i dodawać więcej miejsca do magazynowania. |
| Chcę znaleźć sposób, aby zaoszczędzić na magazynie. |Nie mogę utworzyć kopii zapasowej ani przywrócić tak dużych tabel w ramach umowy SLA. |

## <a name="what-kind-of-databases-and-tables-are-candidates-for-stretch-database"></a>Jakiego typu bazy danych i tabele nadają się do bazy danych Stretch?
Usługa Stretch Database obsługuje transakcyjne bazy danych z dużą ilością zimnych danych zazwyczaj przechowywanych w niewielkiej liczbie tabel. Tabele te mogą zawierać ponad miliard wierszy.

Jeśli używasz funkcji tabeli danych czasowych programu SQL Server 2016, użyj bazy danych Stretch do migrowania całości lub części skojarzonej tabeli historii do opłacalnego magazynu na platformie Azure. Więcej informacji można znaleźć w artykule [Manage Retention of Historical Data in System-Versioned Temporal Tables](https://msdn.microsoft.com/library/mt637341.aspx) (Zarządzanie przechowywaniem danych historycznych w tabelach tymczasowych wersjonowanych w systemie).

Użyj Doradcy w zakresie bazy danych Stretch, funkcji Doradcy uaktualnienia do programu SQL Server 2016, w celu identyfikacji baz danych i tabel dla bazy danych Stretch. Więcej informacji znajduje się w artykule [Identify databases and tables for Stretch Database](sql-server-stretch-database-identify-databases.md) (Identyfikowanie baz danych i tabel dla bazy danych Stretch). Aby dowiedzieć się więcej na temat potencjalnych problemów z blokowaniem, zobacz artykuł [Limitations for Stretch Database](sql-server-stretch-database-limitations.md) (Ograniczenia dotyczące usługi Stretch Database).

## <a name="test-drive-stretch-database"></a>Testowanie bazy danych Stretch
**Przetestuj usługę Stretch Database za pomocą przykładowej bazy danych AdventureWorks.** Aby pobrać przykładową bazę danych AdventureWorks, pobierz [stąd](https://www.microsoft.com/download/details.aspx?id=49502) przynajmniej plik bazy danych oraz plik przykładów i skryptów. Po przywróceniu przykładowej bazy danych do wystąpienia programu SQL Server 2016 rozpakuj plik przykładów i otwórz plik przykładów bazy danych Stretch z folderu bazy danych Stretch. Uruchom skrypty w tym pliku, aby sprawdzić, jaką przestrzeń zajmują dane przed włączeniem bazy danych Stretch i po jej włączeniu, śledzić postęp migracji danych i potwierdzić, że można nadal wysyłać zapytania o istniejące dane oraz wstawiać nowe dane podczas i po migracji.

## <a name="next-step"></a>Następny krok
**Zidentyfikuj bazy danych i tabele przeznaczone dla usługi Stretch Database.** Pobierz Doradcę uaktualnienia do programu SQL Server 2016 i uruchom Doradcę bazy danych Stretch, aby zidentyfikować bazy danych i tabele przeznaczone do bazy danych Stretch. Doradca bazy danych Stretch identyfikuje również problemy z blokowaniem. Więcej informacji znajduje się w artykule [Identify databases and tables for Stretch Database](sql-server-stretch-database-identify-databases.md) (Identyfikowanie baz danych i tabel dla bazy danych Stretch).

<!--Image references-->
[StretchOverviewImage1]: ./media/sql-server-stretch-database-overview/StretchDBOverview.png
[StretchOverviewImage2]: ./media/sql-server-stretch-database-overview/StretchDBOverview1.png
[StretchOverviewImage3]: ./media/sql-server-stretch-database-overview/StretchDBOverview2.png



<!--HONumber=Nov16_HO2-->


