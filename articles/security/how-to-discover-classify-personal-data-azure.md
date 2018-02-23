---
title: Odnajdywanie, identyfikowanie i klasyfikowania danych osobowych w Microsoft Azure | Dokumentacja firmy Microsoft
description: "Więcej informacji na temat wyszukiwania, klasyfikacji, odnajdywania i identyfikowanie danych"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TShinder
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: 011fe26c297ff991af1282c1ff3914031a99f1e4
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="discover-identify-and-classify-personal-data-in-microsoft-azure"></a>Wykryj, identyfikowanie i klasyfikowania danych osobowych w Microsoft Azure

Ten artykuł zawiera wskazówki dotyczące odnajdywania, identyfikowanie i klasyfikowania danych osobowych w kilku narzędzi Azure i usługi, w tym za pomocą wykazu danych Azure, Azure Active Directory, baza danych SQL, dodatku Power Query dla klastrów platformy Hadoop w usłudze Azure HDInsight Azure Information Protection, usługi Azure Search i SQL kwerendy dla bazy danych Azure rozwiązania Cosmos.

## <a name="scenario-problem-statement-and-goal"></a>Scenariusz, opis problemu i cel

Firmy amerykańskiej sportowych zbiera różnorodnych danych prywatnych i innych. W tym klientów oraz danych o pracownikach. Firma utrzymuje je w wielu baz danych i zapisuje je w różnych miejscach w środowisku Azure. Oprócz sprzedaży urządzeń oraz informacji sportowych, również hosta i Zarządzanie rejestracją najwyższej athletic zdarzeń na świecie, w tym w UE, oraz w niektórych przypadkach dane klienta, które pobierają informacje medyczne.

Ponieważ firma hostuje wiele międzynarodowych przewodnikami bicycling co roku i tymczasowi pracownicy nie ma w lokalizacji na całym świecie, kilka zestawów danych są bardzo duże. Firma ma również wbudowane dewelopera aplikacji, używane przez klientów i pracowników.

Firma chce rozwiązać następujące problemy:

- Dane osobowe klienta i pracownika musi być sklasyfikowane/odróżnić od innych danych zbiera firmy, aby zapewnić prawidłowy dostęp i zabezpieczenia.
- Dane administratora musi łatwo wykryć lokalizacji klienta danych osobowych przez różne obszary środowiska platformy Azure.
- Klienta i pracownika dane osobowe, która jest wyświetlana w udostępnionych dokumentów i wiadomości e-mail muszą być oznaczone pomaga zapewnić, że jest dobrze zabezpieczony.
- Deweloperzy aplikacji firmy muszą mieć możliwość łatwo wyszukiwać dane osobowe klienta i pracownika w sieci web i aplikacji mobilnych.
- Deweloperzy muszą zapytania bazy danych dokumentów ich dane osobowe.

### <a name="company-goals"></a>Cele firmy

- Wszystkie dane osobowe klienta i pracowników muszą być oznaczone/opatrzone adnotacjami w usłudze Azure Data Catalog, można je łatwo znaleźć. W idealnym przypadku klienta i pracownika danych osobowych są oznaczone adnotacji oddzielnie.
- Dane osobowe z klienta i pracownika profili użytkowników oraz informacje o pracy znajdujących się w usłudze Azure Active Directory muszą być łatwo odnaleźć.
- Można łatwo zbadać osobistych danych znajdujących się na wielu baz danych. 
- Niektóre firmy dużych zestawów danych są zarządzane za pomocą usługi Azure HDInsight i przechowywane w Hadoop. Muszą one zaimportowane do programu Excel, może być badana danych osobowych.
- Dane osobowe udostępnionych dokumentów i wiadomości e-mail muszą sklasyfikowane, etykietą i zabezpieczone z usługi Azure Information Protection.
- Deweloperzy aplikacji firmy musi mieć możliwość odnajdywania klienta i pracownika danych osobowych w aplikacje, które zostały one utworzone, które mogą je z usługi Azure Search.
- Deweloperzy musi mieć możliwość znaleźć danych osobowych w bazie danych tych dokumentów.

## <a name="azure-active-directory-data-discovery"></a>Usługi Azure Active Directory: Odnajdywanie danych

[Usługa Azure Active Directory](https://azure.microsoft.com/services/active-directory/) jest oparta na chmurze, wielodostępne katalogami i tożsamościami zarządzania usługi Microsoft. Możesz znaleźć klienta i pracownika profili użytkowników oraz informacje o pracy użytkownika, które zawierają dane osobowe w Twojej [usługi Azure Active Directory](https://azure.microsoft.com/services/active-directory/) środowiska (AAD) przy użyciu [portalu Azure](https://portal.azure.com/).

Jest to szczególnie przydatne, jeśli chcesz sprawdzić lub zmienić danych osobowych dla określonego użytkownika. Można również dodać lub zmienić profil użytkownika i informacje o pracy. Użytkownik musi zalogować się przy użyciu konta, które jest administratorem globalnym katalogu.

### <a name="how-do-i-locate-or-view-user-profile-and-work-information"></a>Jak znaleźć lub wyświetlanie profilu użytkownika i informacje o pracy?

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.

2. Wybierz **wszystkie usługi**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie wybierz **Enter**.

   ![jak znaleźć profilu użytkownika i informacje o pracy](media/how-to-discover-classify-personal-data-azure/user-profile.png)

3. Na **użytkowników i grup** bloku, wybierz opcję **użytkowników**.

      ![Otwieranie użytkowników i grup](media/how-to-discover-classify-personal-data-azure/users-groups.png)

4. Na **użytkowników i grup — Użytkownicy** bloku, wybierz użytkownika z listy, a następnie w bloku dla wybranego użytkownika, wybierz opcję **profilu** Aby wyświetlić informacje o profilu użytkownika, który może zawierać dane osobowe.

      ![Wybierz użytkownika](media/how-to-discover-classify-personal-data-azure/select-user.png)

5. Jeśli musisz dodać lub zmienić informacje o profilu użytkownika, można to zrobić, a następnie na pasku poleceń Wybierz **zapisać.**
6. W bloku dla wybranego użytkownika, wybierz **pracy informacji** można wyświetlać informacje o pracy użytkownika, które mogą zawierać dane osobowe.

     ![wyświetlanie informacje o pracy](media/how-to-discover-classify-personal-data-azure/work-info.png)

7. Jeśli musisz dodać lub zmienić informacje o pracy użytkownika, można to zrobić, a następnie, na pasku poleceń Wybierz **zapisać.**

## <a name="azure-sql-database-data-discovery"></a>Azure SQL Database: Odnajdywanie danych

[Baza danych SQL Azure](https://azure.microsoft.com/services/sql-database/?v=16.50) jest chmury bazy danych, która ułatwia deweloperom tworzenie i obsługa aplikacji. Dane osobowe znajdują się w [bazy danych SQL Azure](https://azure.microsoft.com/services/sql-database/?v=16.50) przy użyciu standardowego zapytania SQL. Azure elastycznej zapytania SQL (wersja zapoznawcza) umożliwia użytkownikom do wykonywania zapytań między bazami danych.

Szczegółowe [bazy danych SQL](../sql-database/sql-database-technical-overview.md) samouczek wyjaśnia sposób wiele aspektów korzystanie z bazy danych SQL, w tym jak utworzyć i wykonywania kwerend danych. Poniżej znajduje się podsumowanie informacji dostępnych w samouczku wraz z łączami do określonych sekcji.

### <a name="how-do-i-build-a-sql-database"></a>Jak utworzyć bazę danych SQL

Istnieją trzy sposoby to zrobić:

- Baza danych Azure SQL mogą być tworzone w [portalu Azure](https://portal.azure.com/). W samouczku użyjesz określonego zestawu zasobów obliczeniowych i magazynu w ramach grupy zasobów i serwer logiczny. Użyjesz przykładowych danych fikcyjnej firmy o nazwie AdventureWorks. Będzie także utworzyć regułę zapory poziomu serwera. Aby dowiedzieć się, jak to zrobić, odwiedź stronę [tworzenie bazy danych Azure SQL w portalu Azure](../sql-database/sql-database-get-started-portal.md) samouczka.

  ![Utwórz bazę danych Azure SQL](media/how-to-discover-classify-personal-data-azure/create-database.png)
- Można także utworzyć bazę danych SQL w [powłoki chmury Azure](https://azure.microsoft.com/features/cloud-shell/) interfejsu wiersza polecenia, narzędzie wiersza polecenia opartego na przeglądarce. Narzędzie jest dostępne w portalu Azure i można je uruchomić bezpośrednio z niego. W tym samouczku możesz uruchomić narzędzie, zdefiniuj zmienne skryptu Utwórz grupę zasobów i serwer logiczny i skonfigurować reguły zapory serwera. Następnie utwórz bazę danych z przykładowymi danymi. Aby dowiedzieć się, jak utworzyć bazę danych w ten sposób, odwiedź stronę [tworzenia pojedynczej bazy danych Azure SQL przy użyciu interfejsu wiersza polecenia Azure](../sql-database/sql-database-get-started-cli.md) samouczka.

  ![Samouczek CLIT](media/how-to-discover-classify-personal-data-azure/cli-tutorial.png)

>[!NOTE]
Interfejs wiersza polecenia platformy Azure to powszechnie używane przez administratorów systemu Linux i deweloperów. W przypadku niektórych użytkowników być łatwiejsze i bardziej intuicyjne niż środowiska PowerShell, czyli z trzecia opcja.

- Na koniec można utworzyć bazy danych SQL przy użyciu programu PowerShell, który jest używany do tworzenia i zarządzania Azure i innych zasobów narzędzia wiersza polecenia lub skryptu. W tym samouczku możesz uruchomić narzędzie, zdefiniuj zmienne skryptu Utwórz grupę zasobów i serwer logiczny i skonfigurować reguły zapory serwera. Następnie utworzysz bazę danych z przykładowymi danymi.

Samouczek wymaga programu Azure PowerShell modułu w wersji 4.0 lub nowszej. Uruchom Get-Module - ListAvailable AzureRM można znaleźć wersji. Jeśli musisz zainstalować lub uaktualnić, zobacz modułu instalacji programu Azure PowerShell.

```PowerShell
New-AzureRmSQLDatabase -ResourceGroupName $resourcegroupname `
-ServerName $servername `
-DatabaseName $databasename `
-RequestedServiceObjectiveName "s0"
```

Aby dowiedzieć się, jak utworzyć bazę danych w ten sposób, odwiedź stronę [tworzenia pojedynczej bazy danych Azure SQL przy użyciu programu Powershell](../sql-database/sql-database-get-started-powershell.md) samouczka.

>[!Note]
Często administratorzy systemu Windows przy użyciu programu PowerShell, ale niektóre z nich użyć wiersza polecenia platformy Azure.

### <a name="how-do-i-search-for-personal-data-in-sql-database-in-the-azure-portal"></a>Jak wyszukać danych osobowych w bazie danych SQL w portalu Azure? **

Można użyć narzędzia Edytora wbudowaną kwerendę w portalu Azure, aby wyszukiwać dane osobiste. Będzie zalogować się za pomocą swój identyfikator logowania administratora serwera SQL i hasło, a następnie wprowadź kwerendę.

  ![Wyszukaj sql przy użyciu portalu](media/how-to-discover-classify-personal-data-azure/search-sql-portal.png)

Krok 5 samouczka przedstawiono przykładowe zapytanie w okienku edytora zapytań, ale nie skupić się na informacje poufne lub osobiste (także łączy dane z dwóch tabel i tworzy aliasów dla kolumny źródłowej w zestawie danych zwracanych). Poniższy zrzut ekranu przedstawia zapytanie z kroku 5, jak również w okienku wyników, która jest zwracana:

  ![edytor zapytań](media/how-to-discover-classify-personal-data-azure/query-editor.png)

Jeśli baza danych została wywołana MyTable, przykładowego zapytania o informacje osobiste może obejmować nazwę, numer ubezpieczenia społecznego i numer identyfikacyjny i będzie wyglądać następująco:

"Wybierz nazwę, SSN, identyfikator numer z MyTable"

Czy uruchomić zapytanie i wyświetlić wyniki w **wyniki** okienka.

Aby uzyskać więcej informacji na temat sposobu zapytanie dotyczące bazy danych SQL w portalu Azure, odwiedź stronę [w bazie danych SQL](../sql-database/sql-database-get-started-portal.md) części samouczka.

### <a name="how-do-i-search-for-data-across-multiple-databases"></a>Jak wyszukiwać dane między wieloma bazami danych

Elastyczne zapytania SQL (wersja zapoznawcza) umożliwia wykonują między bazami danych i wielu zapytań bazy danych i zwracają pojedynczego wyniku. [Samouczek Przegląd](../sql-database/sql-database-elastic-query-overview.md) zawiera szczegółowy opis scenariuszy i objaśniono różnicę między partycjonowania poziome i pionowe bazy danych. Poziomy Partycjonowanie jest nazywany "dzielenia na fragmenty."

  ![Partycjonowanie pionowe](media/how-to-discover-classify-personal-data-azure/vertical-partition.png)

  ![Partycjonowanie pozioma](media/how-to-discover-classify-personal-data-azure/horizontal.png)

Aby rozpocząć, odwiedź stronę [bazy danych SQL Azure elastycznej zapytań — omówienie (wersja zapoznawcza)](../sql-database/sql-database-elastic-query-overview.md) strony.

#### <a name="power-query-for-importing-azure-hdinsight-hadoop-clusters-data-discovery-for-large-data-sets"></a>Dodatek Power Query (w przypadku importowania klastrów platformy Azure HDInsight Hadoop): Odnajdywanie danych w przypadku dużych zestawów danych

Hadoop jest typu open source Apache usługi przechowywania i przetwarzania w przypadku dużych zestawów danych, które są analizowane i przechowywane w klastrów platformy Hadoop. [Usługa Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) pozwala użytkownikom na pracę z klastrami platformy Hadoop w usłudze Azure. Dodatek Power Query jest dodatek programu Excel, która między innymi ułatwia użytkownikom odnajdywanie dane z różnych źródeł.

Dane osobowe skojarzone z klastrów platformy Hadoop w usłudze Azure HDInsight mogą być importowane do programu Excel z dodatku Power Query. Po zaimportowaniu danych w programie Excel można użyć zapytania, aby zidentyfikować go.

#### <a name="how-do-i-use-excel-power-query-to-import-hadoop-clusters-in-azure-hdinsight-into-excel"></a>Jak używać dodatku Power Query dla programu Excel do zaimportowania do programu Excel klastrów platformy Hadoop w usłudze Azure HDInsight?

Samouczek HDInsight objaśniają całego procesu. Opisano wymagania wstępne i zawiera łącze do [Rozpoczynanie pracy z usługą Azure HDInsight](../hdinsight/hadoop/apache-hadoop-linux-tutorial-get-started.md) samouczka. Instrukcje obejmują Excel 2016, a także 2010 i 2013 (kroki są nieco inaczej w przypadku starszych wersji programu Excel). Jeśli nie masz dodatku dodatku Power Query dla programu Excel, samouczek przedstawia sposób pobrać go. Początek samouczka w programie Excel i będą musiały korzystać z konta magazynu obiektów Blob platformy Azure skojarzony z klastrem.

  ![Zapytania w programie Excel](media/how-to-discover-classify-personal-data-azure/excel.png)

Aby dowiedzieć się, jak to zrobić, odwiedź stronę [łączenie programu Excel do platformy Hadoop za pomocą dodatku Power Query](../hdinsight/hadoop/apache-hadoop-connect-excel-power-query.md) samouczka.

Źródło: [Połącz Excel do platformy Hadoop za pomocą dodatku Power Query](../hdinsight/hadoop/apache-hadoop-connect-excel-power-query.md)

## <a name="azure-information-protection-personal-data-classification-for-documents-and-email"></a>Usługa Azure Information Protection: Klasyfikacja danych osobowych dla dokumentów i wiadomości e-mail

[Usługa Azure Information Protection](https://www.microsoft.com/cloud-platform/azure-information-protection) pomocy klientów platformy Azure zastosować etykiety do klasyfikowania i ochrony wewnętrznie i zewnętrznie udostępnionych dokumentów i wiadomości e-mail komunikacji. Niektóre z tych elementów może zawierać informacje osobiste klienta lub pracowników. Zasady i warunki można definiować automatycznie lub ręcznie przez administratorów lub użytkowników. Na przykład jeśli użytkownik zapisuje dokument, który zawiera informacje o karcie kredytowej, użytkownik zobaczy zalecenie etykiety, który został skonfigurowany przez administratora.

### <a name="how-do-i-try-it"></a>Jak spróbuj go

Jeśli chcesz Wypróbuj usługę Azure Information Protection aby zobaczyć, czy może być odpowiednie dla firmy, odwiedź stronę [samouczek Szybki Start](https://docs.microsoft.com/information-protection/get-started/infoprotect-quick-start-tutorial). Przeprowadza użytkownika przez pięć podstawowych kroków — od instalacji do konfigurowania zasad, aby wyświetlać klasyfikacji, etykietowania i udostępnianie w akcji — i powinno zająć mniej niż pół godziny.

### <a name="how-do-i-deploy-it"></a>Jak wdrożyć go

Jeśli chcesz wdrożyć usługę Azure Information Protection dla swojej organizacji, odwiedź stronę [planu wdrożenia dla klasyfikacji, etykietowania i ochrony](https://docs.microsoft.com/information-protection/plan-design/deployment-roadmap).

### <a name="is-there-anything-else-i-should-know"></a>Czy jest coś innego trzeba wiedzieć?

Informacje uzupełniające pomoże Ci przemyślenie jak je skonfigurować, odwiedź stronę [gotowe, należy ustawić należy chronić!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
wpis w blogu. I sprawdzić informacje więcej łączy wymienionych poniżej, aby uzyskać więcej informacji na temat usługi Azure Information Protection.

## <a name="azure-search-data-discovery-for-developer-apps"></a>Usługa wyszukiwanie Azure: danych odnajdywania dla deweloperów aplikacji

[Usługa Azure Search](https://azure.microsoft.com/services/search/) jest rozwiązania wyszukiwania w chmurze dla deweloperów i zapewnia obsługę wyszukiwania danych rozbudowanych aplikacji. Usługa wyszukiwanie Azure umożliwia umieszczanie danych między indeksów zdefiniowane przez użytkownika z bazy danych Azure kosmetyczka, baza danych SQL Azure, magazyn obiektów Blob Azure, magazynem tabel Azure lub klienta niestandardowe dane JSON. Można również struktury zapytań Lucene do wyszukiwania typów danych osobistych lub danych osobowych konkretne osoby za pomocą interfejsu API REST usługi Azure Search. Cechy wyszukiwania pełnotekstowego, prosta składnia zapytań i składnia zapytań Lucene. 

## <a name="how-do-i-use-sql-to-query-data"></a>Jak używać SQL wykonać zapytania o dane?

Rozpoczynać się od znaku podstawy, odwiedź stronę [bazy danych Azure CosmosD: jak wykonać zapytanie, przy użyciu programu SQL](../cosmos-db/tutorial-query-documentdb.md) samouczka. Samouczek zawiera przykładowy dokument i dwa przykładowe zapytania SQL oraz wyniki.

Więcej szczegółowe wskazówki dotyczące tworzenia zapytania SQL, odwiedź stronę [kwerendy SQL dla interfejsu API Azure rozwiązania Cosmos bazy danych dokumentów bazy danych.](../cosmos-db/sql-api-sql-query.md)

Jeśli nowych do bazy danych Azure rozwiązania Cosmos i chcesz dowiedzieć się, jak utworzyć bazę danych, kolekcji, Dodaj i dodać dane, odwiedź witrynę [bazy danych Azure rozwiązania Cosmos: tworzenie aplikacji sieci web interfejsu API SQL](../cosmos-db/create-sql-api-dotnet.md) samouczek Szybki Start. Jeśli chcesz to zrobić w języku innym niż .NET, takie jak Java lub Python, wystarczy wybrać preferowany język po przejść do witryny.

## <a name="next-steps"></a>Kolejne kroki

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50)

[Co to jest SQL Database?](../sql-database/sql-database-technical-overview.md)

[SQL edytora zapytań bazy danych dostępne w portalu Azure] (https://azure.microsoft.com/blog/t-sql-query-editor-in-browser-azure-portal/)

[Co to jest Azure Information Protection?](https://docs.microsoft.com/information-protection/understand-explore/what-is-information-protection)

[Co to jest usługa Azure Rights Management?](https://docs.microsoft.com/information-protection/understand-explore/what-is-azure-rms)

[Usługa Azure Information Protection: Gotowe, ustawianie, ochrona!](https://blogs.technet.microsoft.com/enterprisemobility/2017/02/21/azure-information-protection-ready-set-protect/)
