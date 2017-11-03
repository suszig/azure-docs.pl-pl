---
title: "Zarządzanie danych osobowych w Microsoft Azure | Dokumentacja firmy Microsoft"
description: "instrukcje rozwiązać problem, aktualizowanie, usuwanie i eksportowanie danych osobowych w usłudze Azure Active Directory i bazy danych SQL Azure"
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.openlocfilehash: 3b57c92bd744644ea81878712b4272ed3ece4e2e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="manage-personal-data-in-microsoft-azure"></a>Zarządzanie danych osobowych w Microsoft Azure

Ten artykuł zawiera wskazówki dotyczące do Popraw, aktualizowanie, usuwanie i Eksportuj dane osobowe w usłudze Azure Active Directory i bazy danych SQL Azure.

## <a name="scenario"></a>Scenariusz

Na podstawie Dublin firmę kompleksowych zakup górną granicę Śluby docelowego w Irlandii i na całym świecie zarówno lokalnych, jak i międzynarodowej klienta podstawowej. Mają one oddziałów, klientów, pracowników i dostawców na całym świecie pełni usługi miejsc, oferują.

Wśród wielu innych elementów firmy przechowuje informacje o zbędne obejmujących alergii żywności i ustalenia preferencji. Goście wesele można zarejestrować dla różnych działań, takich jak horseback jazda, przeglądania, łodzi było itp. i nawet współdziałać ze sobą na stronie sieci web centralnej w miesiącach prowadzących do zdarzenia. Firma zbiera dane osobowe z pracowników, dostawców, klientów i wesele gości. Z powodu międzynarodowe rodzaj działalności firmy muszą być zgodne z różnych poziomach rozporządzenia.

## <a name="problem-statement"></a>Opis problemu

- Administratorzy danych musi mieć możliwość poprawne niedokładne osobiste informacje i aktualizacji niekompletne lub zmieniania informacji osobistych.

- Administratorzy danych muszą mieć możliwość usunięcia informacji osobistych na żądanie podmiotu danych.

- Administratorzy danych należy wyeksportować dane i przekazywanie ich do tematu dane w formacie wspólnego, strukturalnych na żądanie lub jej.

## <a name="company-goals"></a>Cele firmy

- Nieprawidłowe lub niepełne klienta, gościa wesele pracowników i informacje osobiste dostawcy musi poprawione lub zaktualizowane w usłudze Azure Active Directory i bazy danych SQL Azure.

- Informacje osobiste, muszą zostać usunięte w usłudze Azure Active Directory i usługi Azure SQL Database na żądanie podmiotu danych.

- Dane osobowe musi być eksportowany w formacie wspólnego, strukturalnych na żądanie podmiotu danych.

## <a name="solutions"></a>Rozwiązania

### <a name="azure-active-directory-rectifycorrect-inaccurate-or-incomplete-personal-data-and-erasedelete-personal-datauser-profiles"></a>Usługi Azure Active Directory: rozwiązać/Popraw nieprawidłowe lub niekompletne dane osobowe i wymazywanie/usuwania osobistych danych/profili użytkowników

[Usługa Azure Active Directory](https://azure.microsoft.com/services/active-directory/) jest oparta na chmurze, wielodostępne katalogami i tożsamościami zarządzania usługi Microsoft.
Można poprawić, zaktualizować lub usunąć klientów i pracownika profili użytkowników oraz informacje o pracy użytkownika, które zawierają dane osobowe, takie jak nazwa użytkownika, tytuł pracy, adres lub numer telefonu, w Twojej [usługi Azure Active Directory](https://azure.microsoft.com/services/active-directory/) (AAD) środowisko przy użyciu [portalu Azure](https://portal.azure.com/).

Użytkownik musi zalogować się przy użyciu konta, które jest administratorem globalnym katalogu.

#### <a name="how-do-i-correct-or-update-user-profile-and-work-information-in-azure-active-directory"></a>Jak skorygować lub zaktualizować profil użytkownika i pracy informacji w usłudze Azure Active Directory?

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.

2. Wybierz **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie wybierz **Enter**.

    ![nośnik/image1.png](media/manage-personal-data-azure/image001.png)

3. Na **użytkowników i grup** bloku, wybierz opcję **użytkowników**.

    ![nośnik/image2.png](media/manage-personal-data-azure/image003.png)

4. Na **użytkowników i grup — Użytkownicy** bloku, wybierz użytkownika z listy, a następnie w bloku dla wybranego użytkownika, wybierz opcję **profilu** Aby wyświetlić informacje o profilu użytkownika, który musi zostać poprawiony lub zaktualizować.

    ![nośnik/image3.png](media/manage-personal-data-azure/image005.png)

5. Popraw lub zaktualizować informacje, a następnie na pasku poleceń Wybierz **zapisać.**

6.  W bloku dla wybranego użytkownika, wybierz **pracy informacji** Aby wyświetlić informacje o pracy użytkownika, który musi zostać poprawiony lub zaktualizować.

    ![nośnik/image4.png](media/manage-personal-data-azure/image007.png)

7. Popraw lub zaktualizować informacje o pracy użytkownika, a następnie na pasku poleceń Wybierz **zapisać.**

#### <a name="how-do-i-delete-a-user-profile-in-azure-active-directory"></a>Jak usunąć profil użytkownika w usłudze Azure Active Directory?

1. Zaloguj się do [portalu Azure](https://portal.azure.com) przy użyciu konta, które jest administratorem globalnym katalogu.

2. Wybierz **więcej usług**, wprowadź **użytkowników i grup** w polu tekstowym, a następnie wybierz **Enter**.

    ![](media/manage-personal-data-azure/image001.png)

3. Na **użytkowników i grup** bloku, wybierz opcję **użytkowników**.

    ![nośnik/image2.png](media/manage-personal-data-azure/image003.png)

4. W bloku **Użytkownicy i grupy — Użytkownicy** wybierz użytkownika z listy.

    ![nośnik/image3.png](media/manage-personal-data-azure/image007.png)

5. W bloku dla wybranego użytkownika, wybierz **omówienie**, a następnie na pasku poleceń Wybierz **usunąć**.

    ![](media/manage-personal-data-azure/image013.png)

### <a name="sql-database-rectifycorrect-inaccurate-or-incomplete-personal-data-erasedelete-personal-data-export-personal-data"></a>Baza danych SQL: rozwiązać Popraw nieprawidłowe lub niekompletne dane osobowe; wymazywanie lub Usuń dane osobowe; Eksportuj dane osobowe 

[Baza danych SQL Azure](https://azure.microsoft.com/services/sql-database/?v=16.50) jest chmury bazy danych, która ułatwia deweloperom tworzenie i obsługa aplikacji.

Można aktualizować dane osobowe w [bazy danych SQL Azure](https://azure.microsoft.com/services/sql-database/?v=16.50) przy użyciu standardowego zapytania SQL, a także mogą być również usuwane. Ponadto dane osobowe mogą być eksportowane z bazy danych SQL przy użyciu różnych metod, w tym serwerze SQL Azure Kreatora importu i eksportu, a w różnych formatach, łącznie z plikiem pliku BACPAC.

#### <a name="how-do-i-correct-update-or-erase-personal-data-in-sql-database"></a>Jak rozwiązać, zaktualizować lub wymazanie danych osobowych w bazie danych SQL?

Aby dowiedzieć się, jak usunąć lub zaktualizować danych osobowych w bazie danych SQL, odwiedź stronę [aktualizacji (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/queries/update-transact-sql), [Aktualizowanie tekstu](https://docs.microsoft.com/sql/t-sql/queries/updatetext-transact-sql), [zaktualizować o wspólnym wyrażeniem tabeli](https://docs.microsoft.com/sql/t-sql/queries/with-common-table-expression-transact-sql), lub [Tekst pisania aktualizacji](https://docs.microsoft.com/sql/t-sql/queries/writetext-transact-sql) dokumentacji.

Aby dowiedzieć się, jak usunąć dane osobowe w bazie danych SQL, odwiedź stronę [usunąć (Transact-SQL)](https://docs.microsoft.com/sql/t-sql/statements/delete-transact-sql) dokumentacji.

#### <a name="how-do-i-export-personal-data-to-a-bacpac-file-in-sql-database"></a>Jak wyeksportować dane osobowe w pliku pliku BACPAC bazy danych SQL?

Pliku BACPAC plik zawiera dane z bazy danych SQL i metadane i plik zip z rozszerzeniem pliku BACPAC. Można to zrobić za pomocą [portalu Azure](https://portal.azure.com/), narzędzie wiersza polecenia SQLPackage, SQL Server Management Studio (SSMS) lub programu PowerShell.

Aby dowiedzieć się, jak eksportować dane do pliku BACPAC pliku, odwiedź stronę [wyeksportować do pliku pliku BACPAC bazy danych Azure SQL](https://docs.microsoft.com/azure/sql-database/sql-database-export) strony, który zawiera szczegółowe instrukcje dotyczące każdej z metod wymienionych powyżej.

Jak wyeksportować dane osobowe z bazy danych SQL z programu SQL Server Kreatora importu i eksportu?

Ten kreator pomaga skopiować dane ze źródła do miejsca docelowego. Aby obejrzeć wprowadzenie do kreatora, łącznie ze sposobem, Pobierz informacje o uprawnieniach i jak uzyskać pomoc, za pomocą narzędzia, odwiedź stronę [importowanie i eksportowanie danych z programu SQL Server Kreatora importu i eksportu](https://docs.microsoft.com/sql/integration-services/import-export-data/import-and-export-data-with-the-sql-server-import-and-export-wizard) strony sieci web.

Omówienie kroki kreatora, odwiedź stronę [kroki w Kreatorze eksportu i importu serwera SQL](https://docs.microsoft.com/sql/integration-services/import-export-data/steps-in-the-sql-server-import-and-export-wizard) strony sieci web.

## <a name="next-steps"></a>Następne kroki:

[Azure SQL Database](https://azure.microsoft.com/services/sql-database/?v=16.50) 

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/)

