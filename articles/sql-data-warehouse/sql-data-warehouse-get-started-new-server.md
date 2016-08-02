<properties
   pageTitle="Tworzenie bazy danych w usłudze SQL Data Warehouse w Portalu Azure | Microsoft Azure"
   description="Dowiedz się, jak utworzyć bazę danych usługi Azure SQL Data Warehouse w Portalu Azure"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="jhubbard"
   editor=""
   tags="azure-sql-data-warehouse"/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="05/03/2016"
   ms.author="lodipalm;"/>

# Tworzenie nowego serwera logicznego

W usługach SQL Database i SQL Data Warehouse każda baza danych jest przypisana do serwera, a każdy serwer jest przypisany do lokalizacji geograficznej. Serwer jest nazywany serwerem logicznym SQL.

> [AZURE.NOTE] <a name="note"></a>Serwer logiczny SQL:
  >
  > + Zapewnia spójny sposób konfigurowania wielu baz danych w tej samej lokalizacji geograficznej.
  > + W przeciwieństwie do serwera lokalnego nie jest sprzętem fizycznym. Stanowi część oprogramowania usługi. Dlatego nazywamy go *serwerem logicznym*.
  > + Może hostować wiele baz danych bez wpływu na ich wydajność.
  > + Jego nazwa zaczyna się od małej litery *s*. Nazwa **s**erwer SQL oznacza serwer logiczny platformy Azure, natomiast serwer SQL **S**erver jest produktem bazy danych firmy Microsoft instalowanym lokalnie.

1. Kliknij kolejno opcje **Serwer**  >  **Utwórz nowy serwer**. Serwery są dostępne bezpłatnie. Jeśli masz już serwer logiczny SQL V12, którego chcesz użyć, wybierz istniejący serwer i przejdź do następnego kroku.

    ![Tworzenie nowego serwera](./media/sql-data-warehouse-get-started-provision/create-server.png)

2. Wypełnij informacje w obszarze **Nowy serwer**.

    - **Nazwa serwera**: wprowadź nazwę dla serwera logicznego. Jest ona unikatowa w lokalizacji geograficznej.
    - **Nazwa administratora serwera**: wprowadź nazwę użytkownika dla konta administratora serwera.
    - **Hasło**: wprowadź hasło administratora serwera.
    - **Lokalizacja**: wybierz lokalizację geograficzną dla serwera. Aby skrócić czas transferu danych, najlepiej umieścić serwer w lokalizacji geograficznej blisko innych zasobów danych, do których będzie miała dostęp ta baza danych.
    - **Utwórz serwer V12**: TAK jest jedyną opcją dla usługi SQL Data Warehouse.
    - **Zezwalaj usługom platformy Azure na dostęp do serwera**: to pole jest zawsze zaznaczone dla usługi SQL Data Warehouse.

    >[AZURE.NOTE] Koniecznie zanotuj nazwę serwera, nazwę administratora serwera i hasło.  Te informacje będą potrzebne do logowania się do serwera.

3. Kliknij przycisk **OK**, aby zapisać ustawienia konfiguracji serwera logicznego SQL i powrócić do bloku usługi SQL Data Warehouse.

    ![Konfigurowanie nowego serwera](./media/sql-data-warehouse-get-started-provision/configure-server.png)



<!--HONumber=Jun16_HO2-->


