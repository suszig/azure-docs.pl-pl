---
title: "Tworzenie bazy danych MySQL na platformie Azure i łączenie się z nią"
description: "Dowiedz się, jak utworzyć bazę danych MySQL, a następnie podłącz do niego z aplikacji sieci web PHP na platformie Azure za pomocą portalu Azure."
documentationcenter: php
services: app-service\web
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: 55465a9a-7e65-4fd9-8a65-dd83ee41f3e5
ms.service: multiple
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;cephalin
ms.openlocfilehash: c072cb3a7d376d1e3c2b9f741f5410106e701256
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-connect-to-a-mysql-database-in-azure"></a>Tworzenie bazy danych MySQL na platformie Azure i łączenie się z nią
Ten samouczek pokazuje, jak utworzyć bazę danych MySQL w [portalu Azure](https://portal.azure.com) (dostawca jest [ClearDB](http://www.cleardb.com/)) oraz sposób nawiązania połączenia z systemem aplikacji sieci web PHP [usłudze Azure App Service](app-service/app-service-web-overview.md).

> [!NOTE]
> Można również utworzyć bazę danych MySQL jako część <a href="https://portal.azure.com/#create/WordPress.WordPress" target="_blank">szablon aplikacji Marketplace</a>.
>
>

## <a name="create-a-mysql-database-in-azure-portal"></a>Utwórz bazę danych MySQL w portalu Azure
Aby utworzyć bazę danych MySQL w portalu Azure, wykonaj następujące czynności:

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).
2. Z menu po lewej stronie kliknij **nowy** > **dane i magazyn** > **baza danych MySQL**.

    ![Utwórz bazę danych MySQL na platformie Azure - start](./media/store-php-create-mysql-database/create-db-1-start.png)
3. W nowej bazy danych MySQL [bloku](azure-portal-overview.md), skonfiguruj nową bazę danych MySQL w następujący sposób (*bloku*: strony portalu, który zostanie otwarty w poziomie):

   * **Nazwa bazy danych**: wpisz nazwę unikatową identyfikację
   * **Subskrypcja**: Wybierz subskrypcję do użycia
   * **Baza danych typu**: Wybierz **Shared** ekonomicznych lub wolnego warstw, lub **dedykowana** można pobrać zasobów dedykowanych.
   * **Grupa zasobów**: dodać do istniejącej bazy danych MySQL [grupy zasobów](azure-resource-manager/resource-group-overview.md) lub umieść je w nowej. Zasoby w tej samej grupie można łatwo zarządzać razem.
   * **Lokalizacja**: Wybierz bliską lokalizację. Podczas dodawania do istniejącej grupy zasobów, jest ograniczona do lokalizacji grupy zasobów.
   * **Warstwy cenowej**: kliknij **warstwy cenowej**, następnie wybierz opcję cenową (**Mercury** warstwa jest bezpłatna), a następnie kliknij przycisk **wybierz**.
   * **Postanowienia prawne**: kliknij **postanowienia prawne**, przejrzyj szczegóły zakupu i kliknij przycisk **zakupu**.
   * **Przypnij do pulpitu nawigacyjnego**: zaznaczenie zostanie do niego dostępu bezpośrednio z pulpitu nawigacyjnego. Jest to szczególnie przydatne, jeśli nie znasz jeszcze nawigacji w portalu.

     Poniższy zrzut ekranu jest tylko przykładem sposobu konfigurowania bazy danych MySQL.  
     ![Utwórz bazę danych MySQL na platformie Azure — Konfigurowanie](./media/store-php-create-mysql-database/create-db-2-configure.png)
4. Po zakończeniu konfigurowania, kliknij przycisk **Utwórz**.

    ![Utwórz bazę danych MySQL na platformie Azure — tworzenie](./media/store-php-create-mysql-database/create-db-3-create.png)

    Zostanie wyświetlone powiadomienie wyskakujące, dzięki czemu będzie wiadomo, że rozpoczęło się wdrażanie.

    ![Trwa tworzenie bazy danych MySQL na platformie Azure-](./media/store-php-create-mysql-database/create-db-4-started-status.png)

    Otrzymasz innej wyskakujących po pomyślnym zakończeniu wdrożenia. Portal otworzy również bloku bazy danych MySQL automatycznie.

<a name="connect"></a>

## <a name="connect-to-your-mysql-database"></a>Połączenia z bazą danych MySQL
Aby wyświetlić informacje o połączeniu dla nowej bazy danych MySQL, wystarczy kliknąć **właściwości** w bloku aplikacja sieci web.

![Utwórz bazę danych MySQL na platformie Azure — blok bazy danych MySQL](./media/store-php-create-mysql-database/create-db-5-finished-db-blade.png)

Informacje o połączeniu można teraz używać we wszystkich aplikacjach sieci web. Przykład pokazujący sposób użyć informacji o połączeniu z prostej aplikacji PHP jest dostępna [tutaj](https://github.com/WindowsAzure/azure-sdk-for-php-samples/tree/master/tasklist-mysql).

## <a name="next-steps"></a>Następne kroki
Aby uzyskać więcej informacji, zobacz [Centrum deweloperów języka PHP](/develop/php/).
