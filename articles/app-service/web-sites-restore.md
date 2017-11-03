---
title: Przywracanie aplikacji na platformie Azure
description: "Dowiedz się, jak przywracanie z kopii zapasowej aplikacji."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: 4444dbf7-363c-47e2-b24a-dbd45cb08491
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/06/2016
ms.author: cephalin
ms.openlocfilehash: 79d4084deb6d8c028918690c339c21c720e63594
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="restore-an-app-in-azure"></a>Przywracanie aplikacji na platformie Azure
W tym artykule przedstawiono sposób przywracania aplikacji w [usłudze Azure App Service](../app-service/app-service-web-overview.md) , która została wcześniej utworzona kopia zapasowa (zobacz [kopii zapasowych aplikacji na platformie Azure](web-sites-backup.md)). Przywracanie aplikacji za pomocą jego połączonej bazy danych na żądanie do poprzedniego stanu lub Utwórz nową aplikację na podstawie jednej z aplikacji oryginalnej kopii zapasowej. Usługa aplikacji Azure obsługuje następujące bazy danych dla kopii zapasowej i przywracania:
- [SQL Database](https://azure.microsoft.com/en-us/services/sql-database/)
- [Bazy danych platformy Azure dla programu MySQL (wersja zapoznawcza)](https://azure.microsoft.com/en-us/services/mysql)
- [Bazy danych platformy Azure dla PostgreSQL (wersja zapoznawcza)](https://azure.microsoft.com/en-us/services/postgres)
- [ClearDB MySQL](https://azuremarketplace.microsoft.com/en-us/marketplace/apps/SuccessBricksInc.ClearDBMySQLDatabase?tab=Overview)
- [MySQL w aplikacji](https://blogs.msdn.microsoft.com/appserviceteam/2017/03/06/announcing-general-availability-for-mysql-in-app)

Przywracanie z kopii zapasowych jest dostępne dla aplikacji działających **standardowe** i **Premium** warstwy. Aby uzyskać informacji na temat skalowania aplikacji w górę, zobacz [skalowanie w górę aplikacji na platformie Azure](web-sites-scale.md). **Premium** warstwy pozwala większa liczba codziennych kopii zapasowych wykonywanych niż **standardowe** warstwy.

<a name="PreviousBackup"></a>

## <a name="restore-an-app-from-an-existing-backup"></a>Przywróć aplikację z istniejącej kopii zapasowej
1. Na **ustawienia** bloku aplikacji w portalu Azure kliknij **kopii zapasowych** do wyświetlenia **kopii zapasowych** bloku. Następnie kliknij przycisk **przywrócić**.
   
    ![Wybierz polecenie Przywróć teraz][ChooseRestoreNow]
2. W **przywrócić** bloku, najpierw wybierz źródłowy kopii zapasowej.
   
    ![](./media/web-sites-restore/021ChooseSource1.png)
   
    **Kopii zapasowej aplikacji** opcja powoduje wyświetlenie wszystkich istniejących kopii zapasowych z bieżącej aplikacji i można łatwo wybrać.
    **Magazynu** opcja umożliwia wybranie dowolnego pliku ZIP kopii zapasowej z wszelkie istniejące konto magazynu Azure i kontener w ramach subskrypcji.
    Jeśli próbujesz przywracania kopii zapasowej innej aplikacji, użyj **magazynu** opcji.
3. Następnie określ miejsce docelowe dla przywracania aplikacji w **docelową lokalizację przywracania**.
   
    ![](./media/web-sites-restore/022ChooseDestination1.png)
   
   > [!WARNING]
   > Jeśli wybierzesz **Zastąp**, wszystkie istniejące dane w bieżącej aplikacji jest usunięte i zastąpione. Przed kliknięciem przycisku **OK**, upewnij się, że jest dokładnie co chcesz zrobić.
   > 
   > 
   
    Możesz wybrać **istniejącej aplikacji** przywracania kopii zapasowej aplikacji do innej aplikacji w tej samej grupie identyczny. Przed użyciem tej opcji należy utworzono już inną aplikację w grupie zasobów z dublowania bazy danych konfiguracji do zdefiniowana w kopii zapasowej aplikacji. Można również utworzyć **nowy** aplikacji, aby przywrócić swoją zawartość.

4. Kliknij przycisk **OK**.

<a name="StorageAccount"></a>

## <a name="download-or-delete-a-backup-from-a-storage-account"></a>Pobierz lub usunięcia kopii zapasowej z konta magazynu
1. W głównym **Przeglądaj** bloku portalu Azure, wybierz opcję **kont magazynu**. Zostanie wyświetlona lista istniejących kont magazynu.
2. Wybierz konto magazynu, który zawiera kopię zapasową, której chcesz pobrać lub usunąć. Zostanie wyświetlony blok dla konta magazynu.
3. W bloku konto magazynu Wybierz kontener, który ma
   
    ![Kontenery widoku][ViewContainers]
4. Wybierz plik kopii zapasowej, który chcesz pobrać lub usunąć.
   
    ![ViewContainers](./media/web-sites-restore/03ViewFiles.png)
5. Kliknij przycisk **Pobierz** lub **usunąć** w zależności od tego, co chcesz zrobić.  

<a name="OperationLogs"></a>

## <a name="monitor-a-restore-operation"></a>Monitorowanie operacji przywracania
Aby wyświetlić szczegółowe informacje o powodzeniu lub niepowodzeniu operacji przywracania aplikacji, przejdź do **dziennik aktywności** bloku w portalu Azure.  
 

Przewiń w dół do znajdowania żądaną operację przywracania i zaznacz je.

Szczegóły blok Wyświetla dostępne informacje dotyczące operacji przywracania.

<!-- ## Next Steps
You can backup and restore App Service apps using REST API. -->


<!-- IMAGES -->
[ChooseRestoreNow]: ./media/web-sites-restore/02ChooseRestoreNow1.png
[ViewContainers]: ./media/web-sites-restore/03ViewContainers.png
