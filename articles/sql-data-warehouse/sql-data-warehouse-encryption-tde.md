---
title: "Przezroczystego szyfrowania danych w usłudze SQL Data Warehouse (Portal) | Dokumentacja firmy Microsoft"
description: Przezroczystego szyfrowania danych (funkcji TDE) w magazynie danych SQL
services: sql-data-warehouse
documentationcenter: 
author: ronortloff
manager: jhubbard
editor: 
ms.assetid: fabf75d3-9bbf-4e0d-9b31-8b5a8713f08d
ms.service: sql-data-warehouse
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: security
ms.date: 10/31/2016
ms.author: rortloff;barbkess
ms.openlocfilehash: b1db3bdfdfb54bda325c9b971cfcb4dd5efa333a
ms.sourcegitcommit: 821b6306aab244d2feacbd722f60d99881e9d2a4
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/18/2017
---
# <a name="get-started-with-transparent-data-encryption-tde-in-sql-data-warehouse"></a>Rozpoczynanie pracy z przezroczystym danych szyfrowania (funkcji TDE) w magazynie danych SQL
> [!div class="op_single_selector"]
> * [Przegląd zabezpieczeń](sql-data-warehouse-overview-manage-security.md)
> * [Uwierzytelnianie](sql-data-warehouse-authentication.md)
> * [Szyfrowanie (Portal)](sql-data-warehouse-encryption-tde.md)
> * [Szyfrowanie (T-SQL)](sql-data-warehouse-encryption-tde-tsql.md)
> 
> 

## <a name="required-permssions"></a>Wymagane uprawnienia
Aby włączyć funkcji przezroczystego szyfrowania danych (TDE), musi być administrator lub członek roli dbmanager:.

## <a name="enabling-encryption"></a>Włączenie szyfrowania
Aby włączyć funkcji TDE dla magazynu danych SQL, wykonaj następujące czynności:

1. Otworzyć bazy danych w [portalu Azure](https://portal.azure.com)
2. W bloku bazy danych, kliknij przycisk **ustawienia** przycisku
3. Wybierz **przezroczystego szyfrowania danych** opcji![][1]
4. Wybierz **na** ustawienie![][2]
5. Wybierz **Zapisz**
   ![][3]  

## <a name="disabling-encryption"></a>Wyłączenie szyfrowania
Aby wyłączyć funkcji TDE dla magazynu danych SQL, wykonaj następujące czynności:

1. Otworzyć bazy danych w [portalu Azure](https://portal.azure.com)
2. W bloku bazy danych, kliknij przycisk **ustawienia** przycisku
3. Wybierz **przezroczystego szyfrowania danych** opcji![][1]
4. Wybierz **poza** ustawienie![][4]
5. Wybierz **Zapisz**
   ![][5]  

## <a name="encryption-dmvs"></a>Szyfrowanie widoków DMV
Szyfrowanie może zostać potwierdzony z następujących widoków DMV:

* [sys.Databases]
* [sys.dm_pdw_nodes_database_encryption_keys]

<!--MSDN references-->
[Transparent Data Encryption (TDE)]: https://msdn.microsoft.com/library/bb934049.aspx
[sys.Databases]: http://msdn.microsoft.com/library/ms178534.aspx
[sys.dm_pdw_nodes_database_encryption_keys]: https://msdn.microsoft.com/library/mt203922.aspx

<!--Image references-->
[1]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings.png
[2]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-on.png
[3]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save.png
[4]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-off.png
[5]: ./media/sql-data-warehouse-security-tde/sql-data-warehouse-security-tde-portal-settings-save2.png

<!--Link references-->
