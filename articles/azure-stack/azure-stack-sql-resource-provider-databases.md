---
title: "Przy użyciu baz danych dostarczonych przez RP karty SQL Azure stosu | Dokumentacja firmy Microsoft"
description: "Jak utworzyć i zarządzać udostępniane przy użyciu dostawcy zasobów karty SQL bazy danych SQL"
services: azure-stack
documentationCenter: 
author: JeffGoldner
manager: bradleyb
editor: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2017
ms.author: JeffGo
ms.openlocfilehash: 0cc08c37e879b00f8cd9a4046a4c81c55dab167c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-sql-databases"></a>Tworzenie bazy danych SQL

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Samoobsługowe baz danych są realizowane za pośrednictwem portalu użytkowników. Użytkownik musi subskrypcji, która ma oferty zawiera usługę bazy danych.

1. Zaloguj się do [stosu Azure](azure-stack-poc.md) portalu użytkowników (Administratorzy usług mogą również korzystać z portalu administratora).

2. Kliknij przycisk **+ nowy** &gt; **dane i Magazyn "** &gt; **bazy danych serwera SQL (wersja zapoznawcza)** &gt; **Dodaj**.

3. Wypełnij formularz ze szczegółami bazy danych, w tym **Nazwa bazy danych**, **maksymalny rozmiar**i zmień innych parametrów w razie potrzeby. Zostanie wyświetlona prośba o pobranie wersji bazy danych. Po dodaniu serwerami hostingu, są przydzielone jednostki SKU. Bazy danych są tworzone w tej puli serwerów, które tworzą jednostki SKU hosta.

  ![Nowa baza danych](./media/azure-stack-sql-rp-deploy/newsqldb.png)

  >[!NOTE]
  > Rozmiar bazy danych musi być co najmniej 64 MB. Można zwiększyć, przy użyciu ustawień.

4. Wypełnij ustawienia logowania: **nazwy logowania bazy danych**, i **hasło**. Te ustawienia są utworzonego dla dostępu do tej bazy danych tylko poświadczenia uwierzytelniania SQL. Nazwa użytkownika logowania musi być globalnie unikatowe. Utwórz nowe ustawienie logowania albo wybierz istniejący. Można ponownie użyć ustawień logowania dla innych baz danych przy użyciu tej samej jednostki SKU.

    ![Utwórz nowe nazwy logowania bazy danych](./media/azure-stack-sql-rp-deploy/create-new-login.png)


5. Prześlij formularz i poczekaj, aż do ukończenia wdrożenia.

    W bloku wynikowy Zwróć uwagę, pole "Parametry połączenia". W stosie Azure, można użyć tego ciągu w dowolnej aplikacji, która wymaga dostępu do serwera SQL (na przykład aplikacja sieci web).

    ![Parametry połączenia](./media/azure-stack-sql-rp-deploy/sql-db-settings.png)

## <a name="delete-sql-databases"></a>Usuwanie bazy danych SQL
W portalu

>[!NOTE]
>
>Po usunięciu bazy danych funkcji SQL AlwaysOn z planu odzyskiwania go pomyślnie jest usuwany z podstawowym i grupy dostępności AlwaysOn, ale przez grupy dostępności SQL projektu umieszczenie bazy danych w stanie w każdej replice odzyskiwania i nie porzucenia bazy danych, chyba że wyzwolone. Jeśli bazy danych nie został usunięty, replikach pomocniczych przechodzi do stanu nie można zsynchronizować. Ponowne dodanie nowej bazy danych do grupy dostępności o tej samej za pośrednictwem RP nadal działa. Zobacz ![usuwanie pomocniczej bazy danych](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/remove-a-secondary-database-from-an-availability-group-sql-server)

## <a name="manage-database-credentials"></a>Zarządzanie poświadczeniami bazy danych
Możesz zaktualizować poświadczenia bazy danych (ustawienia logowania).

## <a name="verify-sql-alwayson-databases"></a>Sprawdź baz danych funkcji SQL AlwaysOn
Zawsze włączone bazy danych powinny być widoczne zsynchronizowane i są dostępne na wszystkich wystąpień i w grupie dostępności. Po przejściu w tryb failover powinni bezproblemowo połączyć bazy danych. SQL Server Management Studio służy do sprawdzenia, czy trwa synchronizacja bazy danych:

![Sprawdź (AlwaysOn)](./media/azure-stack-sql-rp-deploy/verifyalwayson.png)
