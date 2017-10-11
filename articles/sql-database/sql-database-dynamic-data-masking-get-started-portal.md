---
title: 'Portalu Azure: maskowania danych dynamicznych bazy danych SQL | Dokumentacja firmy Microsoft'
description: "Jak rozpocząć pracę z bazy danych SQL maskowania danych dynamicznych w portalu Azure"
services: sql-database
documentationcenter: 
author: ronitr
manager: jhubbard
editor: 
ms.assetid: "2"
ms.service: sql-database
ms.custom: security
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 11/22/2016
ms.author: ronitr; ronmat
ms.openlocfilehash: 15184e14d4e1e23b56126bbf9f972c1619dcba80
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 07/11/2017
---
# <a name="get-started-with-sql-database-dynamic-data-masking-with-the-azure-portal"></a>Rozpoczynanie pracy z danymi dynamicznymi bazy danych SQL, maskowanie przy użyciu portalu Azure

W tym temacie przedstawiono sposób wykonania [maskowania danych dynamicznych](sql-database-dynamic-data-masking-get-started.md) z portalu Azure. Można też wdrożyć przy użyciu maskowania danych dynamicznych [poleceń cmdlet usługi Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) lub [interfejsu API REST](https://msdn.microsoft.com/library/dn505719.aspx).


## <a name="set-up-dynamic-data-masking-for-your-database-using-the-azure-portal"></a>Konfigurowanie maskowania danych dynamicznych dla bazy danych przy użyciu portalu Azure
1. Uruchamianie portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com).
2. Przejdź do bloku ustawienia bazy danych, która zawiera dane poufne, które do zamaskowania.
3. Kliknij przycisk **dynamicznego maskowania danych** Kafelek, co spowoduje uruchomienie **dynamicznego maskowania danych** blok konfiguracji.
   
   * Alternatywnie można przewijać w dół do **operacji** sekcji, a następnie kliknij przycisk **dynamicznego maskowania danych**.
     
     ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/4_ddm_settings_tile.png)<br/><br/>
4. W **dynamicznego maskowania danych** blok konfiguracji może zostać wyświetlony niektóre kolumny bazy danych, które aparat zalecenia ma flagę maskowania. Aby zaakceptować zalecenia, kliknij **Dodaj maskę** do jednej lub kilku kolumn i maski zostanie utworzony na podstawie na domyślny typ dla tej kolumny. Funkcji maskowania można zmienić, klikając reguła maskowania i edytowanie maskowania format pola w innym formacie wybranych przez użytkownika. Należy kliknąć przycisk **zapisać** Aby zapisać ustawienia.
   
    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/5_ddm_recommendations.png)<br/><br/>
5. Aby dodać maski dla każdej kolumny w bazie danych, w górnej części **dynamicznego maskowania danych** konfiguracji bloku, kliknij przycisk **Dodaj maskę** otworzyć **Dodaj regułę maskowania** blok konfiguracji
   
    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/6_ddm_add_mask.png)<br/><br/>
6. Wybierz **schematu**, **tabeli** i **kolumny** do zdefiniuj wyznaczone pola, które maskowania.
7. Wybierz **Format maskowania pola** z listy Kategorie maskowanie danych poufnych.
   
    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/7_ddm_mask_field_format.png)<br/><br/>        
8. Kliknij przycisk **zapisać** danych maskowania bloku reguły, aby zaktualizować zestaw maskowania reguły w dynamiczne maskowanie zasad danych.
9. Wpisz użytkowników SQL lub tożsamości usługi AAD, które powinny być wykluczeni z maskowania i mają dostęp do danych poufnych pozbawiony maskowania. Powinno to być rozdzielana średnikami lista użytkowników. Należy pamiętać, że użytkownicy z uprawnieniami administratora zawsze miały dostęp do oryginalnych danych pozbawiony maskowania.
   
    ![Okienko nawigacji](./media/sql-database-dynamic-data-masking-get-started/8_ddm_excluded_users.png)
   
   > [!TIP]
   > Aby uniemożliwić warstwy aplikacji mogą wyświetlać dane poufne dla użytkowników aplikacji uprzywilejowane, Dodaj użytkownika SQL lub tożsamości usługi AAD aplikacji nawiązywał w bazie danych. Zdecydowanie zaleca się, że ta lista zawiera minimalnej liczbie uprzywilejowani użytkownicy, aby zminimalizować narażenie poufnych danych.
   > 
   > 
10. Kliknij przycisk **zapisać** danych maskowania bloku konfiguracji, aby zapisać nowe lub zaktualizowane maskowania zasad.


## <a name="next-steps"></a>Następne kroki

* Omówienie maskowania danych dynamicznych, zobacz [maskowania danych dynamicznych](sql-database-dynamic-data-masking-get-started.md).
* Można też wdrożyć przy użyciu maskowania danych dynamicznych [poleceń cmdlet usługi Azure SQL Database](https://msdn.microsoft.com/library/azure/mt574084.aspx) lub [interfejsu API REST](https://msdn.microsoft.com/library/dn505719.aspx).