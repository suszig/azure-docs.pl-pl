---
title: "Utwórz wystąpienie usługi migracji bazy danych Azure przy użyciu portalu Azure | Dokumentacja firmy Microsoft"
description: "Użyj portalu Azure do utworzenia wystąpienia usługi Azure baza danych migracji"
services: database-migration
author: edmacauley
ms.author: edmaca
manager: craigg
ms.reviewer: 
ms.service: database-migration
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/17/2017
ms.openlocfilehash: 9faac0716334d627cdde4c0ef16262670333b5d4
ms.sourcegitcommit: a036a565bca3e47187eefcaf3cc54e3b5af5b369
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/17/2017
---
# <a name="create-an-instance-of-the-azure-database-migration-service-by-using-the-azure-portal"></a>Utwórz wystąpienie usługi migracji bazy danych Azure przy użyciu portalu Azure
W tym szybki start Użyj portalu Azure do utworzenia wystąpienia usługi Azure migracji bazy danych.  Po utworzeniu usługi, można go użyć do migracji danych z lokalnego programu SQL Server do bazy danych Azure SQL.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal
Otwórz ulubioną przeglądarkę internetową i przejdź do witryny [Microsoft Azure Portal](https://portal.azure.com/). Wprowadź swoje poświadczenia, aby zalogować się do portalu. Widok domyślny to pulpit nawigacyjny usług.

## <a name="register-the-resource-provider"></a>Rejestrowanie dostawcy zasobów
Należy zarejestrować dostawcę zasobów Microsoft.DataMigration przed utworzeniem pierwszej usługi migracji bazy danych.

1. W portalu Azure wybierz **wszystkie usługi**, a następnie wybierz **subskrypcje**.

1. Wybierz subskrypcję, w którym chcesz utworzyć wystąpienia usługi migracji bazy danych Azure, a następnie wybierz **dostawców zasobów**.

1. Wyszukaj migracji, a następnie po prawej stronie Microsoft.DataMigration, wybierz **zarejestrować**.

![Rejestrowanie dostawcy zasobów](media/quickstart-create-data-migration-service-portal/dms-register-provider.png)

## <a name="create-azure-database-migration-service"></a>Tworzenie usługi migracji bazy danych platformy Azure
1. Kliknij przycisk  **+**  do utworzenia nowej usługi.  Usługa migracji bazy danych jest dostępny w wersji zapoznawczej.  

1. Wyszukaj marketplace "migracji", wybierz "Usługa migracji bazy danych (wersja zapoznawcza)", a następnie kliknij **utworzyć**.

    ![Tworzenie usługi migracji](media/quickstart-create-data-migration-service-portal/dms-create-service.png)

    - Wybierz **nazwa usługi** łatwych do zapamiętania i unikatowe do identyfikacji wystąpienia usługi Azure bazy danych migracji.
    - Wybierz platformy Azure **subskrypcji** , w której ma zostać utworzona usługa migracji bazy danych.
    - Utwórz nową **sieci** o unikatowej nazwie.
    - Wybierz **lokalizacji** zbliżony do serwera źródłowego lub docelowego.
    - Wybierz opcję Basic: 1 vCore dla **warstwa cenowa**.

1. Kliknij przycisk **Utwórz**.

Po kilku chwilach usługi migracja bazy danych Azure zostanie utworzona i gotowe do użycia.  Zobaczysz usługa migracji bazy danych, jak pokazano w obrazie.

![Utworzono usługę migracji](media/quickstart-create-data-migration-service-portal/dms-service-created.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów
Możesz wyczyścić zasoby, które zostały utworzone z opcją szybkiego startu przez usunięcie [grupy zasobów platformy Azure](../azure-resource-manager/resource-group-overview.md).  Aby usunąć grupę zasobów, przejdź do usługi migracji bazy danych został utworzony, kliknij **grupy zasobów** nazwę, a następnie wybierz **Usuń grupę zasobów**.  Ta akcja spowoduje usunięcie wszystkich zasobów w grupie zasobów, a także grupy jako całości.

## <a name="next-steps"></a>Następne kroki
> [!div class="nextstepaction"]
> [Migracja programu SQL Server lokalnego do bazy danych Azure SQL](tutorial-sql-server-to-azure-sql.md)