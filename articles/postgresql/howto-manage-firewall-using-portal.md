---
title: "Tworzenie i zarządzanie bazą danych Azure dla reguł zapory PostgreSQL przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Tworzenie i zarządzanie bazą danych Azure dla reguł zapory PostgreSQL przy użyciu portalu Azure"
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 20ac1392949a6f604e68d984cb50273b61051037
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-the-azure-portal"></a>Tworzenie i zarządzanie bazą danych Azure dla reguł zapory PostgreSQL przy użyciu portalu Azure
Reguły zapory poziomu serwera umożliwiają administratorom dostęp do bazy danych Azure PostgreSQL serwera z określonego adresu IP lub zakresu adresów IP. 

## <a name="prerequisites"></a>Wymagania wstępne
Do wykonania kroków opisanych ten przewodnik, potrzebne są:
- Serwer [utworzenia bazy danych Azure dla PostgreSQL](quickstart-create-server-database-portal.md)

## <a name="create-a-server-level-firewall-rule-in-the-azure-portal"></a>Tworzenie reguły zapory na poziomie serwera w witrynie Azure Portal
1. W bloku serwera PostgreSQL, w obszarze Ustawienia kliknij pozycję **zabezpieczenia połączeń** aby otworzyć blok połączenia zabezpieczeń bazy danych Azure dla PostgreSQL.

  ![Portal Azure — kliknij przycisk Zabezpieczenia połączeń](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Kliknij przycisk **dodać Moje IP** na pasku narzędzi. Spowoduje to utworzenie reguły automatycznie przy użyciu adresu IP komputera, jako widocznego w systemie Azure.

  ![Portal Azure — kliknij przycisk Dodaj mój adres IP](./media/howto-manage-firewall-using-portal/2-add-my-ip.png)

3. Zweryfikuj swój adres IP przed zapisaniem konfiguracji. W niektórych sytuacjach adres IP uwzględniony przez Azure portal różni się od adresu IP, używana podczas uzyskiwania dostępu do Internetu i serwery usługi Azure. Dlatego może być konieczne zmiany Start IP i końcowemu adresowi IP, aby funkcja reguły zgodnie z oczekiwaniami.
Użyj aparatu wyszukiwania lub innego narzędzia online, aby sprawdzić własnego adresu IP (na przykład wyszukiwania usługi Bing "co to jest IP Moje").

  ![Co to jest Mój IP wyszukiwania usługi Bing](./media/howto-manage-firewall-using-portal/3-what-is-my-ip.png)

4. Dodaj dodatkowy adres zakresów. W zasadach zapory PostgreSQL bazą danych Azure można określić pojedynczy adres IP lub zakresu adresów. Jeśli chcesz ograniczyć regułę jeden pojedynczy adres IP wpisz ten sam adres w polu Start IP i końcowy adres IP. Otwarcie zapory umożliwia administratorom i użytkownikom do dowolnej bazy danych na serwerze PostgreSQL, do którego mają prawidłowe poświadczenia logowania.

  ![Portal Azure — reguły zapory ](./media/howto-manage-firewall-using-portal/4-specify-addresses.png)

5. Kliknij przycisk **zapisać** na pasku narzędzi, aby zapisać tę regułę zapory poziomu serwera. Poczekaj na potwierdzenie pomyślnego aktualizacji reguł zapory.

  ![Portal Azure — kliknij przycisk Zapisz](./media/howto-manage-firewall-using-portal/5-save-firewall-rule.png)


## <a name="manage-existing-server-level-firewall-rules-through-the-azure-portal"></a>Zarządzanie istniejącymi regułami zapory na poziomie serwera przy użyciu witryny Azure Portal
Powtórz kroki, aby zarządzać regułami zapory.
* Aby dodać bieżący komputer, kliknij przycisk + **dodać Moje IP**. Kliknij przycisk **Zapisz**, aby zapisać zmiany.
* Aby dodać kolejne adresy IP, wpisz nazwę reguły, początkowy adres IP i końcowy adres IP. Kliknij przycisk **Zapisz**, aby zapisać zmiany.
* Aby zmodyfikować istniejącą regułę, kliknij dowolne pole w regule i wprowadź zmiany. Kliknij przycisk **Zapisz**, aby zapisać zmiany.
* Aby usunąć istniejącą regułę, kliknij przycisk wielokropka [...] i kliknij przycisk Usuń, Usuń regułę. Kliknij przycisk **Zapisz**, aby zapisać zmiany.

## <a name="next-steps"></a>Następne kroki
- Podobnie można utworzyć skrypty do [tworzenie i zarządzanie bazą danych Azure PostgreSQL reguł zapory przy użyciu wiersza polecenia platformy Azure](howto-manage-firewall-using-cli.md)
- Aby uzyskać pomoc w połączeniu z bazą danych PostgreSQL serwera Azure, zobacz [biblioteki połączeń dla bazy danych Azure dla PostgreSQL](concepts-connection-libraries.md)
