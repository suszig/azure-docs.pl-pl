<properties
    pageTitle="Tworzenie nowej puli elastycznej za pomocą witryny Azure Portal | Microsoft Azure"
    description="Dodawanie do konfiguracji bazy danych SQL skalowalnej elastycznej puli baz danych ułatwiającej administrowanie zasobami i ich udostępnianie między bazami danych."
    keywords="skalowalna baza danych, konfiguracja bazy danych"
    services="sql-database"
    documentationCenter=""
    authors="ninarn"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="NA"
    ms.date="07/20/2016"
    ms.author="ninarn"
    ms.workload="data-management"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="NA"/>



# Tworzenie nowej elastycznej puli baz danych za pomocą witryny Azure Portal

> [AZURE.SELECTOR]
- [Azure Portal](sql-database-elastic-pool-create-portal.md)
- [PowerShell](sql-database-elastic-pool-create-powershell.md)
- [C#](sql-database-elastic-pool-create-csharp.md)

W tym artykule opisano sposób tworzenia skalowalnej [elastycznej puli baz danych](sql-database-elastic-pool.md) za pomocą witryny [Azure Portal](https://portal.azure.com/). Pulę można utworzyć na dwa sposoby. Można utworzyć ją od podstaw, znając wymagane ustawienia puli, lub uruchomić zgodnie z zaleceniami usługi. Usługa SQL Database ma wbudowane narzędzie analizy, które po zbadaniu ostatnich danych telemetrii użycia baz danych zaleca utworzenie puli, jeśli może ono zapewnić użytkownikowi oszczędności.

Można dodać wiele pul na serwerze, ale nie można dodać baz danych z różnych serwerów do tej samej puli. Do utworzenia puli konieczna jest co najmniej jedna baza danych na serwerze V12. Jeśli nie masz bazy danych, zobacz artykuł [Create your first Azure SQL database](sql-database-get-started.md) (Tworzenie pierwszej bazy danych Azure SQL). Można utworzyć pulę zawierającą tylko jedną bazę danych, ale wydajne są jedynie pule z wieloma bazami danych. Zobacz artykuł [Price and performance considerations for an elastic database pool](sql-database-elastic-pool-guidance.md) (Zagadnienia dotyczące cen i wydajności puli elastycznej).

> [AZURE.NOTE] Pule elastyczne są ogólnodostępne we wszystkich regionach platformy Azure oprócz Indii Zachodnich, gdzie są obecnie dostępne w wersji zapoznawczej.  Pule elastyczne zostaną udostępnione ogólnie w tych regionach tak szybko, jak to możliwe. Ponadto pule elastyczne aktualnie nie obsługują baz danych korzystających z [operacji OLTP w pamięci lub analizy w pamięci](sql-database-in-memory.md).

## Krok 1. Tworzenie nowej puli

W tym artykule przedstawiono, jak utworzyć nową pulę z bloku istniejącego **serwera** w portalu, co jest najprostszym sposobem przenoszenia istniejących baz danych do puli. 

> [AZURE.NOTE] Niezależnie od tego, czy masz już serwer, czy nie, możesz również utworzyć nową pulę z bloku **Pule elastyczne SQL** (pod listą z lewej strony portalu kliknij kolejno pozycje **Przeglądaj** **>** **Pule elastyczne SQL**). Po kliknięciu pozycji **+Dodaj** w bloku **Pule elastyczne SQL** zostaną wyświetlone kroki tworzenia nowego serwera podczas przepływu pracy aprowizacji puli.

1. W witrynie [Azure Portal](http://portal.azure.com/) pod listą z lewej strony kliknij kolejno pozycje **Przeglądaj** **>** **Serwery SQL**, a następnie kliknij serwer zawierający bazy danych, które chcesz dodać do puli.
2. Kliknij przycisk **Nowa pula**.

    ![Dodawanie puli na serwerze](./media/sql-database-elastic-pool-create-portal/new-pool.png)

    **— Lub —**

    Może zostać wyświetlony komunikat z informacją, że są zalecane elastyczne pule baz danych serwera (tylko w wersji 12). Kliknij komunikat, aby wyświetlić pule zalecane na podstawie historycznych danych telemetrycznych użycia baz danych, a następnie kliknij przycisk warstwy, aby zobaczyć więcej szczegółów i dostosować pulę. Zobacz sekcję [Pojęcie zaleceń puli](#understand-pool-recommendations) w dalszej części tego artykułu, aby dowiedzieć się, w jaki sposób tworzone są zalecenia.

    ![zalecana pula](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)

    Zostanie wyświetlony blok **Elastyczna pula baz danych**, w którym należy skonfigurować pulę. Kliknięcie przycisku **Nowa pula** w poprzednim kroku spowoduje, że portal wybierze opcję **Pula w warstwie Standardowa** w obszarze **Warstwa cenowa**, unikatową **nazwę** i domyślną konfigurację puli. W przypadku wybrania zalecanej puli zalecana warstwa i konfiguracja puli są już wybrane, ale nadal można je zmienić.

    ![Konfigurowanie puli elastycznej](./media/sql-database-elastic-pool-create-portal/configure-elastic-pool.png)

3. Określ nazwę puli elastycznej lub pozostaw wartość domyślną.

## Krok 2. Wybieranie warstwy cenowej

Warstwa cenowa puli określa funkcje dostępne dla elastycznych baz danych w puli, maksymalną liczbę jednostek eDTU i magazyn dostępny dla każdej bazy danych (GB). Aby uzyskać szczegółowe informacje, zobacz Warstwy usługi.

Aby zmienić warstwę cenową dla puli, kliknij przycisk **Warstwa cenowa**, kliknij wybraną warstwę cenową, a następnie kliknij przycisk **Wybierz**.

> [AZURE.IMPORTANT] Po wybraniu warstwy cenowej i kliknięciu przycisku **OK** w ostatnim kroku w celu zatwierdzenia wprowadzonych zmian nie można już zmienić warstwy cenowej puli. Aby zmienić warstwę cenową istniejącej puli elastycznej, należy utworzyć nową pulę elastyczną w ramach żądanej warstwy cenowej i przeprowadzić migrację elastycznych baz danych do nowej puli.

![Wybór warstwy cenowej](./media/sql-database-elastic-pool-create-portal/pricing-tier.png)

## Krok 3. Konfigurowanie puli

Po ustawieniu warstwy cenowej kliknij przycisk Konfiguruj pulę, za pomocą którego można dodać bazy danych, ustawić jednostki eDTU i magazyn (GB puli) oraz ustawić wartości minimalne i maksymalne jednostek eDTU dla elastycznej bazy danych w puli.

1. Kliknij przycisk **Konfiguruj pulę**.
2. Wybierz bazy danych, które chcesz dodać do puli. Ten krok jest opcjonalny podczas tworzenia puli. Bazy danych można dodać po jej utworzeniu.
    Aby dodać bazy danych, kliknij przycisk **Dodaj bazę danych**, kliknij bazy danych, które chcesz dodać, a następnie kliknij przycisk **Wybierz**.

    ![Dodawanie baz danych](./media/sql-database-elastic-pool-create-portal/add-databases.png)

    Jeśli bazy danych, z którymi pracujesz, mają wystarczającą ilość historycznych danych telemetrycznych dotyczących użycia, aktualizacje wykresu **Szacowany poziom użycia jednostek eDTU i GB** oraz wykresu słupkowego **Rzeczywiste użycie jednostek eDTU** ułatwiają podejmowanie decyzji związanych z konfiguracją. Ponadto usługa może ułatwić wybranie odpowiedniego rozmiaru puli, wyświetlając komunikat z zaleceniami. Patrz sekcja [Zalecenia dynamiczne](#dynamic-recommendations).

3. Użyj elementów sterujących na stronie **Konfigurowanie puli**, aby eksplorować ustawienia i skonfigurować pulę. Więcej szczegółów na temat limitów warstw usług zawiera sekcja [Limity pul elastycznych](sql-database-elastic-pool.md#edtu-and-storage-limits-for-elastic-pools-and-elastic-databases), natomiast wskazówki dotyczące doboru wielkości puli zawiera artykuł [Price and performance considerations for elastic database pools](sql-database-elastic-pool-guidance.md) (Zagadnienia dotyczące cen i wydajności puli elastycznej). Aby uzyskać więcej informacji na temat ustawień puli, patrz sekcja [Właściwości elastycznej puli baz danych](sql-database-elastic-pool.md#elastic-database-pool-properties).

    ![Konfigurowanie puli elastycznej](./media/sql-database-elastic-pool-create-portal/configure-performance.png)

4. Po wprowadzeniu zmian w ustawieniach kliknij przycisk **Wybierz** w bloku **Konfigurowanie puli**.
5. Kliknij przycisk **OK**, aby utworzyć pulę.


## Omówienie zaleceń puli

Usługa SQL Database ocenia historyczne dane użycia bazy danych i zaleca zastosowanie co najmniej jednej puli baz danych, jeśli okaże się, że jest to bardziej ekonomiczne rozwiązanie niż używanie pojedynczych baz danych. Każde zalecenie jest konfigurowane z unikatowym podzbiorem baz danych serwera, które są najlepiej dopasowane do puli.

![zalecana pula](./media/sql-database-elastic-pool-create-portal/recommended-pool.png)  

Zalecenie puli obejmuje:

- Warstwę cenową dla puli (Podstawowa, Standardowa lub Premium)
- Odpowiednie **Jednostki eDTU PULI** (określane również jako Maks. wartość eDTU na pulę)
- **Maks. wartość eDTU** i **Min. wartość eDTU** na bazę danych
- Listę zalecanych baz danych dla puli

Podczas rekomendowania pul usługa uwzględnia dane telemetryczne z ostatnich 30 dni. Aby baza danych była brana pod uwagę jako potencjalny element elastycznej puli baz danych, musi istnieć przez co najmniej 7 dni. Bazy, które znajdują się już w puli elastycznej, nie są brane pod uwagę podczas tworzenia zaleceń dla elastycznej puli baz danych.

Usługa oblicza zapotrzebowanie na zasoby oraz opłacalność przenoszenia pojedynczych baz danych w każdej warstwie usług do pul w tej samej warstwie. Na przykład wszystkie bazy danych w warstwie Standardowej na serwerze są oceniane pod względem dopasowania do Standardowej puli elastycznej. Oznacza to, że usługa nie tworzy zaleceń międzywarstwowych, takich jak przeniesienie bazy danych z warstwy Standardowej do puli Premium.

### Zalecenia dynamiczne

Po dodaniu bazy danych do puli zalecenia są generowane dynamicznie na podstawie historycznych danych użycia wybranych baz danych. Zalecenia te będą wyświetlane na wykresie użycia jednostek eDTU i GB oraz na banerze w górnej części bloku **Konfigurowanie puli**. Mają one na celu ułatwić tworzenie puli zoptymalizowanej pod kątem konkretnych baz danych.

![zalecenia dynamiczne](./media/sql-database-elastic-pool-create-portal/dynamic-recommendation.png)

## Dodatkowe zasoby

- [Manage a SQL Database elastic pool with the portal (Zarządzanie pulą elastyczną baz danych SQL Database za pomocą portalu)](sql-database-elastic-pool-manage-portal.md)
- [Manage a SQL Database elastic pool with PowerShell (Zarządzanie pulą elastyczną baz danych SQL Database za programu PowerShell)](sql-database-elastic-pool-manage-powershell.md)
- [Manage a SQL Database elastic pool with C (Zarządzanie pulą elastyczną baz danych SQL Database przy użyciu języka C)#](sql-database-elastic-pool-manage-csharp.md)
- [Scaling out with Azure SQL Database (Skalowanie w poziomie za pomocą usługi Azure SQL Database)](sql-database-elastic-scale-introduction.md) 




<!--HONumber=Sep16_HO4-->


