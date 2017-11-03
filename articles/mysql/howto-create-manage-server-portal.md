---
title: "Tworzenie i zarządzanie Azure bazy danych dla serwera MySQL przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "W tym artykule opisano, jak szybko utworzyć nową bazę danych Azure dla serwera MySQL i zarządzanie serwerem przy użyciu portalu Azure."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 09/15/2017
ms.openlocfilehash: 6e9c541aac1241b6af0e4a58f5591d46f9a98c40
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a>Tworzenie i zarządzanie Azure bazy danych dla serwera MySQL przy użyciu portalu Azure
W tym temacie opisano, jak szybko utworzyć nową bazę danych Azure dla serwera MySQL. Zawiera także informacje o sposobie zarządzania serwerem przy użyciu portalu Azure. Zarządzanie serwerem obejmuje, wyświetlanie szczegółów serwera i bazy danych, zresetuj hasło, a następnie usunięcie serwera.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal
Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

## <a name="create-an-azure-database-for-mysql-server"></a>Tworzenie serwera usługi Azure Database for MySQL
Wykonaj następujące kroki, aby utworzyć bazę danych Azure MySQL serwera o nazwie "mysqlserver4demo."

1. Kliknij przycisk **nowy** przycisk znajduje się w lewym górnym rogu portalu Azure.

2. Na nowej stronie, wybierz **baz danych**, a następnie na stronie baz danych, wybierz **bazy danych Azure dla programu MySQL**.

    > Baza danych Azure dla serwera MySQL jest tworzony z zdefiniowanego zestawu [obliczeniowej i pamięci masowej](./concepts-compute-unit-and-storage.md) zasobów. Baza danych została utworzona w ramach grupy zasobów platformy Azure i w bazie danych Azure dla serwera MySQL.

   ![Utwórz nowy serwera](./media/howto-create-manage-server-portal/create-new-server.png)

3. Wypełnianie bazy danych Azure dla formularza MySQL, korzystając z następujących informacji:

    | **Pole formularza** | **Opis pola** |
    |----------------|-----------------------|
    | *Nazwa serwera* | Azure mysql (nazwa serwera jest globalnie unikatowa) |
    | *Subskrypcja* | MySQLaaS (wybierz z menu rozwijanego) |
    | *Grupa zasobów* | myresource (Utwórz nową grupę zasobów lub użyć istniejącego) |
    | *Identyfikator logowania administratora serwera* | myadmin (skonfiguruj nazwę konta administratora) |
    | *Hasło* | ustawienia hasła do konta administratora |
    | *Potwierdź hasło* | potwierdź hasło konta administratora |
    | *Lokalizacja* | Europa Północna (wybór między Europa Północna, Europa i zachodnie stany USA) |
    | *Wersja* | 5.6 (Wybierz bazy danych Azure w wersji server MySQL) |

4. Kliknij przycisk **warstwa cenowa** umożliwia określenie poziomu wydajności i warstwy usługi dla nowego serwera. Jednostka obliczeniowa można skonfigurować od 50 do 100 w warstwie podstawowa, od 100 do 200 w warstwie standardowa, i Magazyn można dodać na podstawie ilości uwzględnione. Ten przewodnik Porada umożliwia wybierz 50 Jednostka obliczeniowa i 50 GB. Kliknij przycisk **OK** Aby zapisać wybrane opcje.

   ![Utwórz server--warstwy cenowej](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)

5. Kliknij przycisk **Utwórz**, aby aprowizować serwer. Aprowizacja zajmuje kilka minut.

    > Wybierz **Przypnij do pulpitu nawigacyjnego** opcję, aby umożliwić łatwe monitorowanie wdrożeń.
    > [!NOTE]
    > W warstwie podstawowa maksymalnie 1000 GB i 10 000 w warstwie standardowa jest obsługiwana dla magazynu, ale dla publicznej wersji zapoznawczej Maksymalna przestrzeń magazynowa jest tymczasowo maksymalnie 1000 GB.</Include>

## <a name="update-an-azure-database-for-mysql-server"></a>Aktualizacja bazy danych Azure dla serwera MySQL
Po zainicjowaniu obsługi nowego serwera, użytkownik ma dwie opcje do edycji istniejącego serwera: zresetować hasła administratora lub skalowanie serwera, zmieniając jednostki obliczeń.

### <a name="change-the-administrator-user-password"></a>Zmień hasło użytkownika administratora
1. Na serwerze **omówienie** bloku, kliknij przycisk **resetowania hasła** do wypełnienia okna wprowadzania i potwierdzenie hasła.

2. Wprowadź nowe hasło i Potwierdź hasło w oknie, jak pokazano:

   ![Zresetuj hasło](./media/howto-create-manage-server-portal/reset-password.png)

3. Kliknij przycisk **OK** można zapisać nowego hasła.

### <a name="scale-updown-by-changing-compute-units"></a>Skalowanie w górę/dół zmieniając obliczeniowe jednostki

1. W bloku serwera w obszarze **ustawienia**, kliknij przycisk **warstwa cenowa** aby otworzyć blok warstwa cenowa Azure bazy danych MySQL serwera.

2. Wykonaj krok 4 w **utworzenia bazy danych MySQL serwera Azure** zmiany obliczeniowe jednostki w tej samej warstwie cenowej.

## <a name="delete-an-azure-database-for-mysql-server"></a>Usuwanie bazy danych Azure dla serwera MySQL

1. Na serwerze **omówienie** bloku, kliknij przycisk **usunąć** przycisku polecenia, aby otworzyć blok potwierdzający usunięcie.

2. Wpisz prawidłową nazwę serwera w polu wejściowego bloku o potwierdzenie dwa razy.

3. Kliknij przycisk **usunąć** przycisk ponownie, aby potwierdzić Akcja usuwania, a następnie poczekaj "Powodzenie usunięcie" pop do wyświetlania na pasku powiadomień.

## <a name="list-the-azure-database-for-mysql-databases"></a>Lista Azure bazy danych dla baz danych MySQL
Na serwerze **omówienie** bloku, przewiń w dół, aż zostanie wyświetlony Kafelek w dolnej części bazy danych. Wszystkie bazy danych są wymienione w tabeli. Kliknij przycisk **usunąć** przycisku polecenia, aby otworzyć blok potwierdzający usunięcie.

   ![Pokaż-baz danych](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a>Pokaż szczegóły bazy danych MySQL serwera Azure
W bloku serwera w obszarze **ustawienia**, kliknij przycisk **właściwości** otworzyć **właściwości** bloku, a następnie Wyświetl szczegółowe informacje o serwerze.

## <a name="next-steps"></a>Następne kroki

[Szybki Start: Tworzenie bazy danych platformy Azure dla serwera MySQL przy użyciu portalu Azure](./quickstart-create-mysql-server-database-using-azure-portal.md)