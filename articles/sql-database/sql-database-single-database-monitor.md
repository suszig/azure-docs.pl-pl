<properties
    pageTitle="Monitorowanie wydajności bazy danych w usłudze Azure SQL Database | Microsoft Azure"
    description="Poznaj opcje monitorowania bazy danych za pomocą narzędzi Azure i dynamicznych widoków zarządzania."
    keywords="monitorowanie bazy danych, wydajność bazy danych w chmurze"
    services="sql-database"
    documentationCenter=""
    authors="CarlRabeler"
    manager="jhubbard"
    editor=""/>

<tags
    ms.service="sql-database"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-management"
    ms.date="09/27/2016"
    ms.author="carlrab"/>


# Monitorowanie wydajności bazy danych w usłudze Azure SQL Database
Monitorowanie wydajności bazy danych SQL na platformie Azure rozpoczyna się od monitorowania wykorzystania zasobów względem wybranego poziomu wydajności bazy danych. Monitorowanie pomaga ustalić, czy baza danych nie ma nadmiarowej pojemności lub czy nie występują problemy z powodu maksymalnego wykorzystania zasobów. Następnie na tej podstawie można zdecydować, czy nadszedł czas, aby dostosować poziom wydajności i [warstwę usług](sql-database-service-tiers.md) bazy danych. Bazę danych można monitorować za pomocą narzędzi graficznych w [witrynie Azure Portal](https://portal.azure.com) lub przy użyciu [dynamicznych widoków zarządzania](https://msdn.microsoft.com/library/ms188754.aspx) SQL.

## Monitorowanie baz danych za pomocą witryny Azure Portal

W witrynie [Azure Portal](https://portal.azure.com/) możesz wybrać bazę danych i kliknąć wykres **Monitorowanie**, aby monitorować wykorzystanie pojedynczej bazy danych. Spowoduje to wyświetlenie okna **Metryka**, które możesz zmienić, klikając przycisk **Edytuj wykres**. Dodaj następujące metryki:

- Procent użycia procesora CPU
- Procent użycia jednostek DTU
- Procent użycia operacji we/wy na danych
- Procent użycia rozmiaru bazy danych

Po dodaniu tych metryk możesz nadal przeglądać je na wykresie **Monitorowanie** z bardziej szczegółowymi informacjami w oknie **Metryka**. Wszystkie cztery metryki pokazują średnią wartość procentową wykorzystania względem jednostek **DTU** bazy danych. Zobacz artykuł o [usługach warstw](sql-database-service-tiers.md), aby dowiedzieć się więcej na temat jednostek DTU.

![Monitorowanie warstw usług pod względem wydajności bazy danych.](./media/sql-database-service-tiers/sqldb_service_tier_monitoring.png)

Możesz również skonfigurować alerty dotyczące metryk wydajności. Kliknij przycisk **Dodaj alert** w oknie **Metryka**. Użyj kreatora, aby skonfigurować alert. Istnieje możliwość ustawienia alertu, który będzie wysyłany, gdy metryki przekroczą określony próg lub spadną poniżej określonego progu.

Jeśli na przykład spodziewasz się, że obciążenie bazy danych wzrośnie, możesz skonfigurować alert polegający na wysłaniu wiadomości e-mail, gdy dowolna z metryk wydajności osiągnie 80%. To wczesne ostrzeżenie jest przydatne, ponieważ pozwala zorientować się, kiedy warto zmienić poziom wydajności na kolejny, wyższy.

Metryki wydajności mogą także pomóc określić, czy można obniżyć poziom wydajności. Załóżmy, że używasz bazy danych, której warstwa i poziom to Standardowa S2, a wszystkie metryki wydajności pokazują, że baza danych wykorzystuje średnio nie więcej niż 10% w określonym czasie. Istnieje prawdopodobieństwo, że baza danych będzie działać równie dobrze na poziomie Standardowa S1. Przed podjęciem decyzji o przeniesieniu na poziom o mniejszej wydajności należy jednak pamiętać o obciążeniach, które chwilowo wzrastają lub zmieniają się w czasie.

## Monitorowanie baz danych przy użyciu widoków DMV

Te same metryki, które są przedstawiane w portalu, są również dostępne za pośrednictwem widoków systemowych: [sys.resource_stats](https://msdn.microsoft.com/library/dn269979.aspx) w logicznej bazie danych **master** na serwerze oraz [sys.dm_db_resource_stats](https://msdn.microsoft.com/library/dn800981.aspx) w bazie danych użytkownika. Użyj widoku **sys.resource_stats**, jeśli chcesz monitorować mniej szczegółowe dane przez dłuższy okres. Użyj widoku **sys.dm_db_resource_stats**, jeśli chcesz monitorować bardziej szczegółowe dane w mniejszych odcinkach czasu. Aby uzyskać więcej informacji, zobacz [wytyczne dotyczące wydajności usługi Azure SQL Database](sql-database-performance-guidance.md#monitoring-resource-use-with-sysresourcestats)

>[AZURE.NOTE] Widok **sys.dm_db_resource_stats** zwraca pusty zestaw wyników w przypadku używania baz danych w wersjach Web i Business, które zostały wycofane.

W przypadku elastycznych pul baz danych można monitorować pojedyncze bazy danych w puli za pomocą metod opisanych w tej części. Można jednak również monitorować pulę jako całość. Informacje na ten temat znajdziesz w artykule [Monitorowanie elastycznej puli baz danych i zarządzanie nią](sql-database-elastic-pool-manage-portal.md).



<!--HONumber=Sep16_HO3-->


