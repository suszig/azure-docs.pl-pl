<properties
   pageTitle="Azure Data Catalog — wprowadzenie do usługi Data Catalog | Microsoft Azure"
   description="Ten kompleksowy samouczek zawiera scenariusze dotyczące usługi Azure Data Catalog i opis jej możliwości."
   documentationCenter=""
   services="data-catalog"
   authors="steelanddata"
   manager=""
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="05/06/2016"
   ms.author="maroche"/>

# Rozpoczynanie pracy z usługą Azure Data Catalog

Ten artykuł zawiera kompleksowe omówienie scenariuszy usługi **Azure Data Catalog** i jej możliwości. Po zarejestrowaniu się w usłudze wykonaj te kroki, aby utworzyć wykaz danych oraz rejestrować źródła danych, dodawać w nich adnotacje i odnajdywać je.

## Wymagania wstępne dotyczące samouczka

Przed rozpoczęciem korzystania z tego samouczka należy dysponować następującymi elementami:

- **Subskrypcja platformy Azure** — jeśli nie masz subskrypcji Azure, możesz utworzyć konto bezpłatnej wersji próbnej w zaledwie kilka minut. Aby uzyskać szczegółowe informacje, zobacz [Bezpłatna wersja próbna](https://azure.microsoft.com/pricing/free-trial/).
- **Azure Active Directory** — usługa Azure Data Catalog używa usługi [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) do zarządzania tożsamościami i dostępem.
- **Źródła danych** — usługa Azure Data Catalog ma funkcje odnajdywania źródeł danych. W tym samouczku używana jest przykładowa baza danych firmy Adventure Works, ale można użyć dowolnego obsługiwanego źródła danych, jeśli wolisz pracować z danymi, które są Ci znane i dotyczą Twojej roli. Aby uzyskać listę obsługiwanych źródeł danych, zobacz [Supported data sources](data-catalog-dsr.md) (Obsługiwane źródła danych).

> [AZURE.NOTE] Aby uzyskać więcej informacji o subskrypcjach platformy Azure i usłudze Azure Active Directory, zobacz [Azure Data Catalog prerequisites](data-catalog-prerequisites.md) (Wymagania wstępne dotyczące usługi Azure Data Catalog).

Rozpocznijmy od zainstalowania przykładowej bazy danych firmy Adventure Works.

## Ćwiczenie 1. Instalowanie przykładowej bazy danych firmy Adventure Works

W tym ćwiczeniu zostanie zainstalowana przykładowa baza danych firmy Adventure Works dla aparatu bazy danych programu SQL Server, który jest używany w kolejnych ćwiczeniach.

### Instalowanie bazy danych OLTP Adventure Works 2014

Baza danych firmy Adventure Works obsługuje standardowe scenariusze przetwarzania transakcji online dla fikcyjnego producenta rowerów (firma Adventure Works Cycles), które obejmują produkty, sprzedaż i zakup. W tym samouczku informacje o produktach zostaną zarejestrowane w **Wykazie danych Azure**.

Poniżej przedstawiono sposób rejestrowania przykładowej bazy danych firmy Adventure Works.

Aby zainstalować przykładową bazę danych firmy Adventure Works, można przywrócić kopię zapasową AdventureWorks2014 znajdującą się w pliku [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661) w witrynie CodePlex. Jednym ze sposobów przywrócenia bazy danych jest uruchomienie skryptu T-SQL w programie SQL Server Management Studio.

**Instalowanie przykładowej bazy danych firmy Adventure Works przy użyciu skryptu T-SQL**

1.  Utwórz folder o nazwie C:\DataCatalog_Wprowadzenie. Jeśli używasz folderu o innej nazwie, upewnij się, że ścieżka w poniższym skrypcie T-SQL została zmieniona.
2.  Pobierz plik [Adventure Works 2014 Full Database Backup.zip](https://msftdbprodsamples.codeplex.com/downloads/get/880661).
3.  Wyodrębnij plik Adventure Works 2014 Full Database Backup.zip do katalogu C:\DataCatalog_Wprowadzenie. W poniższym skrypcie przyjęto założenie, że plik kopii zapasowej znajduje się w folderze C:\DataCatalog_Wprowadzenie\Adventure Works 2014 Full Database Backup\AdventureWorks2014.bak.
4.  Z poziomu programu **SQL Server Management Studio** na pasku narzędzi **Standardowe** kliknij pozycję **Nowe zapytanie**.
5.  Wykonaj poniższy kod T-SQL w oknie zapytania.

**Uruchom poniższy skrypt w celu zainstalowania bazy danych Adventure Works 2014**

    USE [master]
    GO
    -- NOTE: This script is for sample purposes only. The default backup file path for this script is C:\DataCatalog_GetStarted. To run this script, create the default file path or change the file path, and copy AdventureWorks2014.bak into the path.

    -- IMPORTANT: In a production application, restore a SQL database to the data folder for your SQL Server instance.

    RESTORE DATABASE AdventureWorks2014
        -- AdventureWorks2014.bak file location
        FROM disk = 'C:\DataCatalog_GetStarted\Adventure Works 2014 Full Database Backup\AdventureWorks2014.bak'

        -- AdventureWorks2014.mdf database location
        WITH MOVE 'AdventureWorks2014_data' TO 'C:\DataCatalog_GetStarted\AdventureWorks2014.mdf',

        -- AdventureWorks2014.ldf log location
        MOVE 'AdventureWorks2014_Log' TO 'C:\DataCatalog_GetStarted\AdventureWorks2014.ldf'
    ,REPLACE
    GO

Alternatywą do uruchomienia skryptu T-SQL jest przywrócenie bazy danych przy użyciu programu SQL Server Management Studio. Zobacz [Restore a Database Backup (SQL Server Management Studio)](http://msdn.microsoft.com/library/ms177429.aspx) (Przywracanie kopii zapasowej bazy danych — program SQL Server Management Studio).

W tym ćwiczeniu zainstalowana została przykładowa baza danych firmy Adventure Works, która będzie używana w pozostałych ćwiczeniach. W następnym ćwiczeniu dowiesz się, jak zarejestrować źródła danych usługi **Azure Data Catalog** z tabel znajdujących się w przykładowej bazie danych firmy Adventure Works.

## Ćwiczenie 2. Rejestrowanie źródeł danych

W tym ćwiczeniu narzędzie rejestracji usługi **Azure Data Catalog** zostanie użyte do zarejestrowania źródeł danych firmy Adventure Works w wykazie. Rejestracja to proces wyodrębniania kluczowych metadanych strukturalnych, takich jak nazwy, typy i lokalizacje, ze źródła danych i zasobów, które się w nim znajdują, oraz kopiowania tych metadanych do wykazu. Źródła danych i ich dane pozostają tam, gdzie się znajdowały, ale metadane są używane przez wykaz, aby można było je łatwiej odnaleźć i zrozumieć.

### Poniżej przedstawiono sposób rejestrowania źródła danych

1.  Przejdź do witryny sieci Web https://azure.microsoft.com/services/data-catalog, a następnie kliknij pozycję **Rozpocznij**.
2.  Zaloguj się do portalu usługi **Azure Data Catalog**, a następnie kliknij pozycję **Publikuj dane**.

    ![](media/data-catalog-get-started/data-catalog-publish-data.png)

3.  Kliknij pozycję **Uruchom aplikację**.

    ![](media/data-catalog-get-started/data-catalog-launch-application.png)

4. Na stronie **Powitanie** kliknij pozycję **Zaloguj**, a następnie wprowadź swoje poświadczenia.
5. Na stronie **Microsoft Azure Data Catalog** kliknij dwukrotnie pozycję **SQL Server** lub kliknij pozycję **SQL Server**, a następnie kliknij pozycję **Dalej**.

    ![](media/data-catalog-get-started/data-catalog-data-sources.png)

6.  Wprowadź właściwości połączenia serwera SQL dla pliku AdventureWorks2014 (zobacz poniższy przykład), a następnie kliknij pozycję **POŁĄCZ**.

    ![](media/data-catalog-get-started/data-catalog-sql-server-connection.png)

7.  Na następnej stronie ma miejsce rejestrowanie metadanych źródła danych. W tym przykładzie należy zarejestrować obiekty **Production/Product** z przestrzeni nazw Production bazy danych Adventure Works. Oto jak to zrobić:

    a. W drzewie **Hierarchia serwera** kliknij węzeł **Production**.

    ![](media/data-catalog-get-started/data-catalog-server-hierarchy.png)

    b. Naciśnij klawisz Ctrl i kliknij pozycję ProductCategory, ProductDescription i ProductPhoto.

    c. Kliknij strzałkę przeniesienia (**>**). Spowoduje to przeniesienie wszystkich wybranych obiektów Product na listę **Obiekty do zarejestrowania**.

    ![](media/data-catalog-get-started/data-catalog-available-objects.png)

    d. W polu **Dodaj tagi** wprowadź tagi description i photo. Spowoduje to dodanie tagów wyszukiwania dla tych zasobów danych. Tagi to doskonały sposób na ułatwienie użytkownikom znalezienia zarejestrowanego źródła danych.

    ![](media/data-catalog-get-started/data-catalog-objects-register.png)

    e.  **Opcjonalnie**: możesz wybrać pozycję **Dołącz podgląd** i **Dodaj specjalistę źródła danych**.

    f.  Kliknij pozycję **ZAREJESTRUJ**. Wybrane obiekty zostaną zarejestrowane za pomocą usługi **Azure Data Catalog**. W tym ćwiczeniu są rejestrowane wybrane obiekty z bazy danych firmy Adventure Works.

    g.  Aby wyświetlić zarejestrowane obiekty źródła danych, kliknij pozycję **Wyświetl portal**. W portalu usługi **Azure Data Catalog** można wyświetlić obiekty źródła danych jako **kafelki** lub **listy**.

    ![](media/data-catalog-get-started/data-catalog-view-portal.png)

W tym ćwiczeniu zarejestrowano obiekty z przykładowej bazy danych firmy Adventure Works, aby użytkownicy w organizacji mogli je łatwo odnaleźć.
W następnym ćwiczeniu zostanie przedstawiony sposób odnajdowania zarejestrowanych zasobów danych.

## Ćwiczenie 3. Odnajdywanie zarejestrowanych zasobów danych

W tym ćwiczeniu portal usługi **Azure Data Catalog** zostanie użyty do odnalezienia zarejestrowanych zasobów danych i wyświetlenia ich metadanych. **Azure Data Catalog** oferuje wiele narzędzi służących do odnajdywania zasobów danych, w tym proste wyszukiwanie słów kluczowych, filtry interakcyjne i zaawansowaną składnię wyszukiwania dla użytkowników zaawansowanych.

### Poniżej przedstawiono sposób odnajdywania zarejestrowanych zasobów danych

**Azure Data Catalog** ma prostą, ale jednocześnie wydajną składnię umożliwiającą łatwe tworzenie zapytań, które zwracają wymagane przez użytkowników dane. Aby uzyskać szczegółowe informacje o wyszukiwaniu w usłudze **Azure Data Catalog**, zobacz [Data Catalog Search syntax reference](https://msdn.microsoft.com/library/azure/mt267594.aspx) (Dokumentacja dotycząca składni wyszukiwania w usłudze Data Catalog).

Usługa **Azure Data Catalog** ma następujące opcje wyszukiwania:

- Wyszukiwanie słów kluczowych
- Filtr
- Wyszukiwanie zaawansowane

Możesz również doprecyzować, jakie zasoby danych mają być wyświetlane. Usługa **Azure Data Catalog** ma następujące opcje wyświetlania:

- Wyświetl właściwości
- Wyświetl kolumny
- Wyświetl podgląd

W tym przykładzie zostanie użyte wyszukiwanie słów kluczowych. Wyszukiwanie w usłudze **Azure Data Catalog** oferuje kilka technik tworzenia zapytań. W tym przykładzie zostanie użyte zapytanie wyszukiwania **Grupowanie**.

**Techniki tworzenia zapytań**

|Technika|Użycie|Przykład
|---|---|---
|Wyznaczanie zakresu właściwości|Źródła danych są zwracane tylko wtedy, gdy wyszukiwany termin jest zgodny w określonej właściwości|name:product
|Operatory logiczne|Poszerzają lub zawężają wyszukiwanie przy użyciu operacji logicznych zgodnie z opisem w sekcji Operatory logiczne na tej stronie|finance NOT corporate
|Grupowanie za pomocą nawiasów|Użyj nawiasów do grupowania części zapytania w celu uzyskania izolacji logicznej, szczególnie w połączeniu z operatorami logicznymi|name:product AND (tags:illustration OR tags:photo)
|Operatory porównania|Użyj porównań innych niż równość dla właściwości, które mają numeryczne i datowe typy danych|creationTime:>11/05/14

W tym przykładzie zostanie wykonane wyszukiwanie **Grupowanie** dla zasobów danych, których nazwa jest równa nazwie produktu i parametr tags ma wartość illustration lub photo.

1. Przejdź do witryny sieci Web https://azure.microsoft.com/services/data-catalog, kliknij pozycję **Rozpocznij**, a następnie zaloguj się w portalu usługi **Azure Data Catalog**.
2. W polu **Wyszukiwanie w wykazie danych** wprowadź zapytanie **Grupowanie**: (**tags:description OR tags:photo**).
3. Kliknij ikonę wyszukiwania lub naciśnij klawisz Enter. W usłudze **Azure Data Catalog** zostaną wyświetlone zasoby danych dla tego zapytania wyszukiwania.

    ![](media/data-catalog-get-started/data-catalog-search-box.png)

W tym ćwiczeniu portal **Azure Data Catalog** zostanie użyty do odnalezienia i wyświetlenia zasobów danych firmy Adventure Works zarejestrowanych w wykazie.

<a name="annotating"/>
## Ćwiczenie 4. Dodawanie adnotacji do zarejestrowanego źródła danych

W tym ćwiczeniu portal **Azure Data Catalog** zostanie użyty do adnotacji zasobów danych, które zostały wcześniej zarejestrowane w wykazie. Podawane adnotacje będą uzupełniać i rozszerzać metadane strukturalne wyodrębnione ze źródła podczas rejestrowania i znacznie ułatwiać odnajdywanie i zrozumienie zasobów danych. Ponieważ każdy użytkownik usługi **Data Catalog** może podać własne adnotacje, każdy użytkownik z perspektywą danych może je w łatwy sposób udostępniać.

### Poniżej przedstawiono sposób dodawania adnotacji do zasobów danych

1. Przejdź do witryny sieci Web https://azure.microsoft.com/services/data-catalog, kliknij pozycję **Rozpocznij**, a następnie zaloguj się w portalu usługi **Azure Data Catalog**.
2. Kliknij pozycję **Odnajdź**.
3. Wybierz co najmniej jeden element typu **Zasób danych**. W tym przykładzie wybierz pozycję **ProductPhoto**, a następnie wprowadź łańcuch „Zdjęcia produktów na potrzeby materiałów marketingowych”.
4. W polu **Opis** wprowadź wartość, która będzie ułatwiała innym odnajdywanie wybranego zasobu danych i zrozumienie, dlaczego i w jaki sposób go używać. Na przykład wprowadź opis „Obrazy produktu”. Możesz również dodać więcej tagów i wyświetlić kolumny.
5. Teraz możesz spróbować przeprowadzić wyszukiwanie i filtrowanie, aby odnaleźć zasoby danych przy użyciu metadanych opisowych, które zostały dodane do wykazu.

    ![](media/data-catalog-get-started/data-catalog-annotate.png)

W tym ćwiczeniu dodano opisowe informacje do zarejestrowanych zasobów danych, aby użytkownicy wykazu mogli odnaleźć źródła danych przy użyciu terminów, które rozumieją.

> [AZURE.NOTE] Wersja Standard Edition usługi Data Catalog zawiera słownik biznesowy, który umożliwia administratorom wykazu definiowanie centralnej taksonomii biznesowej. Użytkownicy wykazu mogą następnie dodać adnotacje do zasobów danych za pomocą terminów ze słownika. Aby uzyskać więcej informacji, zobacz [How to set up the Business Glossary for Governed Tagging](data-catalog-how-to-business-glossary.md) (Konfigurowanie słownika biznesowego na potrzeby znakowania zarządzanego).  

## Ćwiczenie 5. Crowdsourcing metadanych

W tym ćwiczeniu odbywa się praca z innym użytkownikiem w celu dodania metadanych do zasobów danych w wykazie. Stosowane w usłudze **Azure Data Catalog** polegające na dodawaniu adnotacji przez innych użytkowników umożliwia dowolnym użytkownikom dodawanie tagów, opisów i innych metadanych, dzięki czemu każdy użytkownik z perspektywą zasobu danych i jego użycia może przechwycić taką perspektywę i udostępnić ją innym użytkownikom.

> [AZURE.NOTE] Jeśli nie masz innego użytkownika do pracy w tym samouczku, nie martw się. Każdy użytkownik uzyskujący dostęp do wykazu danych może w dowolnym momencie dodać własną perspektywę. Takie podejście do metadanych polegające na dodawaniu adnotacji przez innych użytkowników umożliwia rozwój zawartości wykazu i wzrost jakości metadanych wykazu wraz z upływem czasu.

### Poniżej przestawiono sposób crowdsourcingu metadanych dotyczących zasobów danych

Poproś współpracownika o powtórzenie powyższego ćwiczenia [Dodawanie adnotacji do zarejestrowanego źródła danych](#annotating). Po dodaniu przez współpracowników opisów do zasobu danych, takiego jak ProductPhoto, zostanie wyświetlone wiele adnotacji.

![](media/data-catalog-get-started/data-catalog-crowdsource.png)

W tym ćwiczeniu zostały przedstawione możliwości usługi **Azure Data Catalog** dotyczące metadanych dodawanych przez innych użytkowników w sytuacji, gdy każdy użytkownik wykazu może dodawać adnotacje do odnalezionych przez siebie zasobów danych.

## Ćwiczenie 6. Nawiązywanie połączenia ze źródłami danych

W tym ćwiczeniu portal usługi **Azure Data Catalog** zostanie użyty do nawiązania połączenia ze źródłem danych przy użyciu programu Microsoft Excel.

> [AZURE.NOTE] Należy pamiętać, że usługa **Azure Data Catalog** nie zapewnia użytkownikom dostępu do rzeczywistego źródła danych, tylko po prostu ułatwia użytkownikom odnalezienie źródła danych i zrozumienie go. Gdy użytkownik nawiązuje połączenie ze źródłem danych, wybrana przez niego aplikacja kliencka użyje poświadczeń systemu Windows lub w razie potrzeby wyświetli monit o ich podanie. Jeśli użytkownikowi nie udzielono wcześniej dostępu do źródła danych, konieczne będzie udzielenie mu dostępu, zanim będzie mógł nawiązać połączenie.

### Poniżej przedstawiono sposób nawiązywania połączenia ze źródłem danych z poziomu programu Excel

1. Przejdź do witryny sieci Web https://azure.microsoft.com/services/data-catalog, kliknij pozycję **Rozpocznij**, a następnie zaloguj się w portalu usługi **Azure Data Catalog**.
2. Kliknij pozycję **Odnajdź**.
3. Wybierz zasób danych. W tym przykładzie wybierz zasób ProductCategory.
4. Wybierz pozycję **Otwórz w** > **Excel**.

    ![](media/data-catalog-get-started/data-catalog-connect1.png)

5. W oknie **Powiadomienie o zabezpieczeniach programu Microsoft Excel** kliknij pozycję **Włącz**.
6. Otwórz plik **ProductCategory.odc**.
7. Źródło danych zostanie otwarte w programie Excel.

    ![](media/data-catalog-get-started/data-catalog-connect2.png)

W tym ćwiczeniu zostało nawiązane połączenie ze źródłami danych odnalezionymi za pomocą usługi **Azure Data Catalog**. Portal usługi **Azure Data Catalog** umożliwia użytkownikom bezpośrednie nawiązywanie połączenia przy użyciu aplikacji klienckich zintegrowanych z ich menu **Otwórz za pomocą** i pozwala użytkownikom na nawiązywanie połączenia za pomocą dowolnej wybranej aplikacji przy użyciu informacji o lokalizacji połączenia zawartych w metadanych zasobów.

## Ćwiczenie 7. Usuwanie metadanych źródła danych

W tym ćwiczeniu portal usługi **Azure Data Catalog** zostanie użyty do usunięcia danych podglądu z zarejestrowanych zasobów danych i usunięcia zasobów danych z wykazu.

> [AZURE.NOTE] Domyślnym zachowaniem katalogu jest umożliwianie każdemu użytkownikowi rejestrowania dowolnego źródła danych oraz usuwania wszystkich zarejestrowanych zasobów danych. Możliwości zarządzania dostępne w wersji **Standard Edition usługi Azure Data Catalog** zapewniają dodatkowe opcje dotyczące przejmowania własności zasobów ograniczające użytkowników, którzy mogą odnajdować i usuwać zasoby.

W usłudze **Azure Data Catalog** można usuwać pojedyncze zasoby lub wiele zasobów.

### Poniżej przedstawiono sposób usuwania wielu zasobów danych

1. Przejdź do witryny sieci Web https://azure.microsoft.com/services/data-catalog, kliknij pozycję **Rozpocznij**, a następnie zaloguj się w portalu usługi **Azure Data Catalog**.
2. Kliknij pozycję **Odnajdź**.
3. Wybierz co najmniej jeden zasób danych.
4. Kliknij pozycję **Usuń**.

W tym ćwiczeniu usunięto zarejestrowane zasoby danych z wykazu.

## Ćwiczenie 8. Zarządzanie zarejestrowanymi źródłami danych

W tym ćwiczeniu możliwości zarządzania usługi **Azure Data Catalog** będą używane do przejęcia na własność zasobów danych, kontrolowania zasobów, które mogą być odnajdowane przez użytkowników, oraz określenia sposobu zarządzania tymi zasobami przez użytkowników.

> [AZURE.NOTE] Możliwości zarządzania opisane w tym ćwiczeniu są dostępne tylko w wersji **Standard Edition usługi Azure Data Catalog**, a nie w wersji **Free Edition**.
W usłudze **Azure Data Catalog** można przejmować na własność zasoby danych, dodawać współwłaścicieli zasobów danych i ustawiać widoczność zasobów danych.

### Poniżej przedstawiono sposób przejęcia własności zasobów danych i ograniczenia ich widoczności

1. Przejdź do witryny sieci Web https://azure.microsoft.com/services/data-catalog, kliknij pozycję **Rozpocznij**, a następnie zaloguj się w portalu usługi **Azure Data Catalog**.
2. Kliknij pozycję **Odnajdź**.
3. Wybierz co najmniej jeden zasób danych.
4. Na panelu **Właściwości** w sekcji **Zarządzanie** kliknij pozycję **Przejmij na własność**.
5. Aby ograniczyć widoczność, kliknij pozycję **Właściciele i następujący użytkownicy**.

    ![](media/data-catalog-get-started/data-catalog-ownership.png)

W tym ćwiczeniu przedstawione zostały możliwości zarządzania usługi **Azure Data Catalog** oraz ograniczona widoczność wybranych zasobów danych.

## Podsumowanie

W tym samouczku zostały przedstawione podstawowe możliwości usługi **Azure Data Catalog**, w tym rejestrowanie, dodawanie adnotacji, odnajdywanie i zarządzanie firmowymi źródłami danych. Po zakończeniu tego samouczka nadszedł czas na rozpoczęcie pracy. Możesz rozpocząć dzisiaj od zarejestrowania źródeł danych, na którym pracujesz Ty i Twój zespół, oraz od zaproszenia współpracowników do korzystania z wykazu.



<!--HONumber=Jun16_HO2-->


