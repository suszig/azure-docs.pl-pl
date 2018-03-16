---
title: "Dodawanie łącznika usługi Azure SQL Database w aplikacjach logiki | Dokumentacja firmy Microsoft"
description: "Omówienie łącznika usługi Azure SQL Database z parametrami interfejsu API REST"
services: 
documentationcenter: 
author: ecfan
manager: anneta
editor: 
tags: connectors
ms.assetid: d8a319d0-e4df-40cf-88f0-29a6158c898c
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/18/2016
ms.author: estfan; ladocs
ms.openlocfilehash: 4313ead0c31ab2e72238701d58dc2f321f116fa6
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/16/2018
---
# <a name="get-started-with-the-azure-sql-database-connector"></a>Rozpoczynanie pracy z łącznika usługi Azure SQL Database
Za pomocą łącznika usługi Azure SQL Database, tworzyć przepływy pracy dla organizacji zarządzających danych w tabelach. 

Z bazy danych SQL można:

* Tworzenie przepływu pracy przez dodanie nowego klienta do bazy danych klientów lub aktualizowania zamówienia w bazie danych zamówienia.
* Użyj akcji, aby pobrać wiersz danych, wstawienia nowego wiersza, a nawet usuwać. Na przykład gdy zostaje utworzony rekord w Dynamics CRM Online (wyzwalacz), następnie wstawienia wiersza w bazie danych SQL Azure (działanie). 

W tym artykule przedstawiono sposób korzystania z łącznika bazy danych SQL w aplikacji logiki, a także zawiera listę akcji.

Aby dowiedzieć się więcej na temat aplikacji logiki, zobacz [co to jest logic apps](../logic-apps/logic-apps-overview.md) i [tworzenie aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="connect-to-azure-sql-database"></a>Połącz z bazą danych Azure SQL
Zanim aplikację logiki można uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć *połączenia* do usługi. Połączenie stanowi połączenie między aplikacji logiki i innej usługi. Na przykład do łączenia z bazą danych SQL, należy najpierw utworzyć bazę danych SQL *połączenia*. Do utworzenia połączenia, należy wprowadzić poświadczenia, zwykle używanego do uzyskania dostępu do usługi, którą jest nawiązywane. Tak w bazie danych SQL, wprowadź swoje poświadczenia bazy danych SQL, aby utworzyć połączenie. 

#### <a name="create-the-connection"></a>Tworzenie połączenia
> [!INCLUDE [Create the connection to SQL Azure](../../includes/connectors-create-api-sqlazure.md)]
> 
> 

## <a name="use-a-trigger"></a>Użyć wyzwalacza
Ten łącznik nie ma żadnych wyzwalaczy. Użyj innych wyzwalaczy, aby uruchomić aplikację logiki, takie jak wyzwalacz cyklu wyzwalacza HTTP elementu Webhook, wyzwalaczy dostępny z innych łączników i inne. [Tworzenie aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md) przykładowego.

## <a name="use-an-action"></a>Za pomocą akcji
Akcja jest przeprowadzane przez przepływ pracy zdefiniowanych w aplikacji logiki operacji. [Dowiedz się więcej o akcjach](../logic-apps/logic-apps-overview.md#logic-app-concepts).

1. Wybierz znak plus. Zobacz kilka opcji: **Dodaj akcję**, **Dodaj warunek**, lub jeden z **więcej** opcje.
   
    ![](./media/connectors-create-api-sqlazure/add-action.png)
2. Wybierz **Dodaj akcję**.
3. W polu tekstowym wpisz "sql", aby uzyskać listę dostępnych akcji.
   
    ![](./media/connectors-create-api-sqlazure/sql-1.png) 
4. W tym przykładzie wybierz **SQL Server — wiersz Get**. Jeśli połączenie już istnieje, następnie wybierz **nazwy tabeli** z listy rozwijanej liście, a następnie wprowadź **identyfikator wiersza** chcesz przywrócić.
   
    ![](./media/connectors-create-api-sqlazure/sample-table.png)
   
    Jeśli zostanie wyświetlony monit o informacje dotyczące połączenia, wprowadź szczegóły, aby utworzyć połączenie. [Utwórz połączenie](connectors-create-api-sqlazure.md#create-the-connection) w tym artykule opisano te właściwości. 
   
   > [!NOTE]
   > W tym przykładzie zostanie zwrócona wiersz z tabeli. Aby wyświetlić dane w tym wierszu, Dodaj inną akcję, która tworzy plik za pomocą pola z tabeli. Na przykład dodać akcję OneDrive, która używa pól Imię i nazwisko, aby utworzyć nowy plik w konta magazynu w chmurze. 
   > 
   > 
5. **Zapisz** zmiany (lewym górnym rogu paska narzędzi). Aplikację logiki jest zapisywana i automatycznie włączone.

## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/sql/). 

## <a name="next-steps"></a>Kolejne kroki
[Tworzenie aplikacji logiki](../logic-apps/quickstart-create-first-logic-app-workflow.md). Eksploruj dostępnych łączników w aplikacjach logiki w [listy interfejsów API](apis-list.md).

