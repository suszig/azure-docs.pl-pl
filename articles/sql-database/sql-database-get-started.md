---
title: "Samouczek usługi SQL Database: tworzenie bazy danych SQL | Microsoft Docs"
description: "Dowiedz się, jak skonfigurować logiczny serwer usługi SQL Database, regułę zapory serwera, bazę danych SQL i przykładowe dane. Dowiedz się też, jak połączyć się z narzędziami klienta, skonfigurować użytkowników i regułę zapory bazy danych."
keywords: "samouczek usługi sql database, tworzenie bazy danych sql"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 09/07/2016
ms.author: carlrab
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 914d5f7e6b8e9165fc8ba9021e1030b865ba5fb2


---
# <a name="sql-database-tutorial-create-a-sql-database-in-minutes-by-using-the-azure-portal"></a>Samouczek usługi SQL Database: tworzenie bazy danych SQL w ciągu kilku minut za pomocą witryny Azure Portal
> [!div class="op_single_selector"]
> * [Azure portal](sql-database-get-started.md)
> * [C#](sql-database-get-started-csharp.md)
> * [PowerShell](sql-database-get-started-powershell.md)
> 
> 

Z tego samouczka nauczysz się używać witryny Azure Portal do wykonywania następujących czynności:

* Tworzenie bazy danych SQL z przykładowymi danymi na platformie Azure.
* Tworzenie reguły zapory na poziomie serwera dla pojedynczego adresu IP lub zakresu adresów IP.

Te same czynności można wykonać, używając języka [C#](sql-database-get-started-csharp.md) lub interpretera [PowerShell](sql-database-get-started-powershell.md).

[!INCLUDE [Login](../../includes/azure-getting-started-portal-login.md)]

<a name="create-logical-server-bk"></a>

## <a name="create-your-first-azure-sql-database"></a>Tworzenie pierwszej bazy danych SQL na platformie Azure
1. Połącz się z [witryną Azure Portal](http://portal.azure.com), jeśli połączenie nie zostało nawiązane już wcześniej.
2. Kliknij **Nowy**, kliknij **Dane i magazyn**, a następnie znajdź opcję **Baza danych SQL**.
   
    ![Nowa baza danych SQL 1](./media/sql-database-get-started/sql-database-new-database-1.png)
3. Kliknij przycisk **Baza danych SQL**, aby otworzyć blok bazy danych SQL. Zawartość tego bloku różni się w zależności od liczby subskrypcji i istniejących obiektów (takich jak serwery).
   
    ![Nowa baza danych SQL 2](./media/sql-database-get-started/sql-database-new-database-2.png)
4. W polu tekstowym **Nazwa bazy danych** podaj nazwę pierwszej bazy danych, na przykład „moja_baza_danych”. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
   
    ![Nowa baza danych SQL 3](./media/sql-database-get-started/sql-database-new-database-3.png)
5. Jeśli masz kilka subskrypcji, wybierz jedną z nich.
6. W obszarze **Grupa zasobów** kliknij przycisk **Utwórz nowy** i podaj nazwę pierwszej grupy zasobów — np. „moja_grupa_zasobów”. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
   
    ![Nowa baza danych SQL 4](./media/sql-database-get-started/sql-database-new-database-4.png)
7. W obszarze **Wybierz źródło** kliknij przycisk **Próbka**, a następnie w obszarze **Wybierz próbkę** kliknij **AdventureWorksLT [V12]**.
   
    ![Nowa baza danych SQL 5](./media/sql-database-get-started/sql-database-new-database-5.png)
8. W obszarze **Serwer** kliknij przycisk **Skonfiguruj wymagane ustawienia**.
   
    ![Nowa baza danych SQL 6](./media/sql-database-get-started/sql-database-new-database-6.png)
9. W bloku Serwer kliknij **Utwórz nowy serwer**. Baza danych SQL na platformie Azure jest tworzona w ramach obiektu serwera, który może być serwerem nowym lub istniejącym.
   
    ![Nowa baza danych SQL 7](./media/sql-database-get-started/sql-database-new-database-7.png)
10. Przejrzyj blok **Nowy serwer**, aby poznać informacje potrzebne do konfiguracji nowego serwera.
    
    ![Nowa baza danych SQL 8](./media/sql-database-get-started/sql-database-new-database-8.png)
11. W polu tekstowym **Nazwa serwera** podaj nazwę pierwszego serwera — np. „mój_nowy_obiekt_serwera”. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
    
    ![Nowa baza danych SQL 9](./media/sql-database-get-started/sql-database-new-database-9.png)
12. W obszarze **Logowanie administratora serwera** podaj nazwę użytkownika w celu logowania się jako administrator — np. „moje_konto_administratora”. Ta nazwa logowania jest nazywana główną nazwą logowania serwera. Zielony znacznik wyboru wskazuje, czy podana nazwa jest poprawna.
    
    ![Nowa baza danych SQL 10](./media/sql-database-get-started/sql-database-new-database-10.png)
13. W obszarze **Hasło** i **Potwierdź hasło** podaj hasło do głównego konta logowania serwera, na przykład "p@ssw0rd1". Zielony znacznik wyboru wskazuje, czy podane hasło jest poprawne.
    
    ![Nowa baza danych SQL 11](./media/sql-database-get-started/sql-database-new-database-11.png)
14. W obszarze **Lokalizacja** wybierz centrum danych odpowiednie dla danej lokalizacji — np. „Australia Wschodnia”.
    
    ![Nowa baza danych SQL 12](./media/sql-database-get-started/sql-database-new-database-12.png)
15. W obszarze *Utwórz serwer V12 (najnowsza aktualizacja)* zwróć uwagę, że możliwe jest utworzenie tylko bieżącej wersji serwera Azure SQL.
    
    ![Nowa baza danych SQL 13](./media/sql-database-get-started/sql-database-new-database-13.png)
16. Należy zauważyć, że domyślnie pole wyboru obok pozycji **Zezwalaj usługom Azure na dostęp do serwera** jest zaznaczone i nie można go zmienić w tym miejscu. Jest to opcja zaawansowana. Chociaż dla większości scenariuszy nie jest to konieczne, można zmienić to ustawienie w ustawieniach zapory serwera dla tego obiektu serwera.
    
    ![Nowa baza danych SQL 14](./media/sql-database-get-started/sql-database-new-database-14.png)
17. W bloku Nowy serwer przejrzyj wybrane opcje, a następnie kliknij przycisk **Wybierz**, aby wybrać ten nowy serwer dla nowej bazy danych.
    
    ![Nowa baza danych SQL 15](./media/sql-database-get-started/sql-database-new-database-15.png)
18. W bloku bazy danych SQL w obszarze **Poziom cenowy** kliknij przycisk **S2 standardowa** a następnie kliknij przycisk **Podstawowe**, aby wybrać najniższy poziom cenowy dla swojej pierwszej bazy danych. Poziom cenowy można zawsze zmienić później.
    
    ![Nowa baza danych SQL 16](./media/sql-database-get-started/sql-database-new-database-16.png)
19. W bloku Baza danych SQL przejrzyj wybrane opcje, a następnie kliknij przycisk **Utwórz**, aby utworzyć pierwszy serwer i bazę danych. Zostanie sprawdzona poprawność podanych wartości i rozpocznie się wdrożenie.
    
    ![Nowa baza danych SQL 17](./media/sql-database-get-started/sql-database-new-database-17.png)
20. Na pasku narzędzi portalu kliknij ikonę **Powiadomienia**, aby sprawdzić stan wdrożenia.
    
    ![Nowa baza danych SQL 18](./media/sql-database-get-started/sql-database-new-database-18.png)

> [!IMPORTANT]
> Po zakończeniu wdrożenia na platformie Azure zostanie utworzony serwer Azure SQL i baza danych. Połączenie z nowym serwerem ani bazą danych przy użyciu narzędzi programu SQL Server nie będzie możliwe do momentu utworzenia reguły zapory serwera w celu otwarcia zapory usługi SQL Database na połączenia spoza platformy Azure.
> 
> 

[!INCLUDE [Create server firewall rule](../../includes/sql-database-create-new-server-firewall-portal.md)]

## <a name="next-steps"></a>Następne kroki
Po ukończeniu tego samouczka usługi SQL Database i utworzeniu bazy danych z przykładowymi danymi możesz przystąpić do jej eksplorowania przy użyciu ulubionych narzędzi.

* Jeśli znasz język Transact-SQL i program SQL Server Management Studio (SSMS), dowiedz się, jak [nawiązać połączenie z bazą danych SQL i odpytywać ją w programie SSMS](sql-database-connect-query-ssms.md).
* Jeśli posługujesz się programem Excel, naucz się [nawiązywać połączenie z bazą danych SQL w programie Excel](sql-database-connect-excel.md).
* Jeśli chcesz zacząć programować, wybierz język programowania w obszarze [Biblioteki połączeń dla usługi SQL Database i programu SQL Server](sql-database-libraries.md).
* Jeśli chcesz przenieść lokalne bazy danych programu SQL Server na platformę Azure, zobacz artykuł [Migrating a database to Azure SQL Database](sql-database-cloud-migrate.md) (Migracja bazy danych do usługi Azure SQL Database), aby dowiedzieć się więcej.
* Jeśli chcesz załadować dane z pliku CSV do nowej tabeli przy użyciu narzędzia wiersza polecenia BCP, zobacz artykuł [Loading data into SQL Database from a CSV file using BCP](sql-database-load-from-csv-with-bcp.md) (Ładowanie danych do usługi SQL Database z pliku CSV przy użyciu narzędzia BCP).
* Jeśli chcesz rozpocząć eksplorowanie zabezpieczeń usługi Azure SQL Database, zobacz artykuł [Getting started with security](sql-database-get-started-security.md) (Wprowadzenie do zabezpieczeń)

## <a name="additional-resources"></a>Dodatkowe zasoby
[Co to jest SQL Database?](sql-database-technical-overview.md)




<!--HONumber=Nov16_HO2-->


