---
title: "Szybki start: Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal | Microsoft Docs"
description: "W tym artykule podano instrukcje pozwalające utworzyć przykładowy serwer usługi Azure Database for MySQL za pomocą witryny Azure Portal w czasie około pięciu minut."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: mysql
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: portal
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 25bfd2c6c25ddb8747dec58fdc68f904f81127fa
ms.contentlocale: pl-pl
ms.lasthandoff: 05/10/2017

---

# <a name="create-an-azure-database-for-mysql-server-using-azure-portal"></a>Tworzenie serwera usługi Azure Database for MySQL za pomocą witryny Azure Portal

W tym artykule podano instrukcje pozwalające utworzyć przykładowy serwer usługi Azure Database for MySQL za pomocą witryny Azure Portal w czasie około pięciu minut. 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-azure"></a>Zaloguj się do platformy Azure.
Otwórz ulubioną przeglądarkę internetową i przejdź do witryny [Microsoft Azure Portal](https://portal.azure.com/). Wprowadź swoje poświadczenia, aby zalogować się do portalu. Widok domyślny to pulpit nawigacyjny usług.

![Witryna Azure Portal — logowanie i pulpit nawigacyjny](./media/quickstart-create-mysql-server-database-using-azure-portal/1_portal-login.png)

## <a name="create-azure-database-for-mysql-server"></a>Tworzenie serwera usługi Azure Database for MySQL

1. Przejdź do obszaru **Bazy danych** > **MySQL**. Jeśli nie możesz znaleźć pozycji Serwer usługi Azure Database for MySQL w kategorii **Bazy danych**, kliknij pozycję **Zobacz wszystko**, aby wyświetlić wszystkie dostępne usługi baz danych. Możesz również wpisać hasło **MySQL** w polu wyszukiwania, aby szybko znaleźć tę usługę.
![Witryna Azure Portal — nowa — baza danych — MySQL](./media/quickstart-create-mysql-server-database-using-azure-portal/2_navigate-to-mysql.png)

2. Kliknij ikonę **MySQL**, a następnie kliknij przycisk **Utwórz**.
W tym przykładzie wprowadź na stronie Azure Database for MySQL poniższe informacje:

| **Pole formularza** | **Opis pola** |
|----------------|-----------------------|
| *Nazwa serwera* | mysqlserver4demo (nazwa serwera jest globalnie unikatowa) |
| *Subskrypcja* | MySQLaaS (wybierz z listy rozwijanej) |
| *Grupa zasobów* | myresource (utwórz grupę zasobów lub użyj istniejącej grupy zasobów) |
| *Identyfikator logowania administratora serwera* | myadmin (skonfiguruj nazwę konta administratora) |
| *Hasło* | skonfiguruj hasło konta administratora |
| *Potwierdź hasło* | potwierdź hasło konta administratora |
| *Lokalizacja* | Europa Północna (masz do wyboru opcje **Europa Północna** i **Zachodnie stany USA**) |
| *Wersja* | 5.6 (wybierz wersję serwera MySQL) |
| *Konfigurowanie wydajności* | Podstawowa (wybierz opcje **Warstwa wydajności**, **Jednostki obliczeniowe** i **Magazyn**, a następnie kliknij przycisk **OK**) |

![Witryna Azure Portal — tworzenie serwera MySQL przez podanie w formularzu wymaganych danych wejściowych](./media/quickstart-create-mysql-server-database-using-azure-portal/3_create-server.png)

Po kilku minutach nastąpi aprowizacja i uruchomienie Twojego serwera usługi Azure Database for MySQL. Możesz kliknąć przycisk **Powiadomienia** (ikona dzwonka) na pasku narzędzi, aby monitorować proces wdrażania.

> [!TIP]
> Zalecamy umieszczanie usług platformy Azure w tym samym regionie i wybranie lokalizacji znajdującej się najbliżej Ciebie. Ponadto możesz zaznaczyć opcję **Przypnij do pulpitu nawigacyjnego**, aby łatwo śledzić wdrożenia.

## <a name="configure-the-firewall"></a>Konfigurowanie zapory
Przed pierwszym nawiązaniem połączenia z usługą Azure Database for MySQL na używanym kliencie musisz skonfigurować zaporę i dodać publiczny adres IP sieci klienta (lub zakres adresów IP) do listy dozwolonych adresów.

1. Kliknij nowo utworzony serwer, a następnie kliknij przycisk **Ustawienia**.
  ![Witryna Azure Portal — MySQL — przycisk Ustawienia](./media/quickstart-create-mysql-server-database-using-azure-portal/4_server-settings.png)

2. W sekcji **OGÓLNE** kliknij pozycję **Ustawienia zapory**. Możesz kliknąć pozycję **Dodaj mój adres IP**, aby dodać adres IP Twojego komputera lokalnego, lub skonfigurować zakres adresów IP. Pamiętaj, aby po utworzeniu reguł kliknąć przycisk **Zapisz**.
  ![Witryna Azure Portal — dodawanie reguły zapory i jej zapisywanie](./media/quickstart-create-mysql-server-database-using-azure-portal/5_firewall-settings.png)

## <a name="get-connection-information"></a>Pobieranie informacji o połączeniu
Pobierz w pełni kwalifikowaną nazwę domeny Twojego serwera Azure MySQL w witrynie Azure Portal. Ta w pełni kwalifikowana nazwa domeny służy do nawiązywania połączeń z Twoim serwerem za pomocą narzędzia wiersza polecenia**mysql.exe**.

1.    W witrynie [Azure Portal](https://portal.azure.com/) kliknij pozycję **Wszystkie zasoby** w menu po lewej stronie, a następnie kliknij Twój serwer usługi Azure Database for MySQL.

2.    Kliknij pozycję **Właściwości**. Zanotuj wartości **NAZWA SERWERA** i **IDENTYFIKATOR LOGOWANIA ADMINISTRATORA SERWERA**.
W tym przykładzie nazwa serwera to *mysql4doc.database.windows.net*, a identyfikator logowania administratora serwera to *mysqladmin@mysql4doc*.

## <a name="connect-to-the-server-using-mysqlexe-command-line-tool"></a>Nawiązywanie połączenia z serwerem za pomocą narzędzia wiersza polecenia mysql.exe
Na serwerze MySQL możesz utworzyć wiele baz danych. Nie istnieje limit dotyczący maksymalnej liczby baz danych, które można utworzyć, ale w przypadku wielu baz danych są współdzielone te same zasoby serwera.  Aby nawiązać połączenie z serwerem przy użyciu narzędzia wiersza polecenia **mysql.exe**, otwórz usługę **Azure Cloud Shell** w portalu i wprowadź poniższe dane:

1. Nawiąż połączenie z serwerem za pomocą narzędzia wiersza polecenia **mysql**:
```dos
 mysql -h mysqlserver4demo.database.windows.net -u myadmin@mysqlserver4demo -p
```

2. Wyświetl stan serwera:
```dos
 mysql> status
```
  ![Wiersz polecenia — przykład wiersza polecenia mysql](./media/quickstart-create-mysql-server-database-using-azure-portal/7_connect-to-server.png)

> [!TIP]
> Aby zapoznać się z dodatkowymi poleceniami, zobacz [Podręcznik programu MySQL 5.6 — Rozdział 4.5.1](https://dev.mysql.com/doc/refman/5.6/en/mysql.html).

## <a name="connect-to-the-server-using-the-mysql-workbench-gui-tool"></a>Nawiązywanie połączenia z serwerem za pomocą narzędzia z graficznym interfejsem użytkownika MySQL Workbench
1.    Uruchom aplikację MySQL Workbench na swoim komputerze klienckim. Aplikację MySQL Workbench możesz pobrać i zainstalować [stąd](https://dev.mysql.com/downloads/workbench/).

2.    W oknie dialogowym **Konfigurowanie nowego połączenia** wprowadź poniższe informacje na karcie **Parametry**:

| **Parametry** | **Opis** |
|----------------|-----------------|
|    *Nazwa połączenia* | podaj nazwę tego połączenia (może to być dowolna nazwa) |
| *Metoda połączenia* | wybierz standard (TCP/IP) |
| *Nazwa hosta* | mycliserver.database.windows.net (NAZWA SERWERA zanotowana przez Ciebie wcześniej) |
| *Port* | 3306 |
| *Nazwa użytkownika* | myadmin@mycliserver (IDENTYFIKATOR LOGOWANIA ADMINISTRATORA SERWERA zanotowany przez Ciebie wcześniej) |
| *Hasło* | możesz przechowywać hasło konta administratora w magazynie |

![konfiguruj nowe połączenie](./media/quickstart-create-mysql-server-database-using-azure-portal/setup-new-connection.png)

3.    Kliknij przycisk **Testuj połączenie**, aby sprawdzić, czy wszystkie parametry zostały prawidłowo skonfigurowane.

4.    Teraz możesz kliknąć właśnie utworzone połączenie, aby pomyślnie nawiązać połączenie z serwerem.

> Na Twoim serwerze domyślnie jest wymuszany protokół SSL. W związku z tym do pomyślnego nawiązania połączenia jest wymagana dodatkowa konfiguracja. Zobacz [Konfigurowanie łączności SSL w aplikacji w celu bezpiecznego nawiązywania połączeń z usługą Azure Database for MySQL](./howto-configure-ssl.md).  Jeśli chcesz wyłączyć protokół SSL na czas wykonywania instrukcji w tym przewodniku Szybki start, możesz przejść do obszaru „Zabezpieczenia połączeń” w witrynie, aby wyłączyć wymuszanie protokołu SSL.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Jeśli te zasoby nie są Ci potrzebne do pracy z innym przewodnikiem Szybki start lub samouczkiem, możesz je usunąć, wykonując następujące czynności:

1. W menu znajdującym się po lewej stronie w witrynie Azure Portal kliknij pozycję **Grupy zasobów**, a następnie kliknij pozycję **myresource**. 
2. Na stronie grupy zasobów kliknij pozycję **Usuń**, wpisz w polu tekstowym wartość **myresource**, a następnie kliknij pozycję **Usuń**.

## <a name="next-steps"></a>Następne kroki

> [!div class="nextstepaction"]
> [Projektowanie pierwszej bazy danych usługi Azure Database for MySQL](./tutorial-design-database-using-portal.md)


