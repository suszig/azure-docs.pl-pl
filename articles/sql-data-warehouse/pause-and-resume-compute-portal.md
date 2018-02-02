---
title: "Szybki Start: Wstrzymywanie i wznawianie obliczeń w Azure SQL Data Warehouse — portalu Azure | Dokumentacja firmy Microsoft"
description: "Azure portalu zadań, które wstrzymać obliczeniowe dla magazynu danych SQL Azure w celu ograniczenia kosztów. Wznowić operacje obliczeniowe, gdy wszystko będzie gotowe do użycia w magazynie danych."
services: sql-data-warehouse
documentationcenter: NA
author: barbkess
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/23/2018
ms.author: barbkess
ms.openlocfilehash: f8b4e29595c6a71696cf2c4939ad4c6c096a28b5
ms.sourcegitcommit: 9d317dabf4a5cca13308c50a10349af0e72e1b7e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/01/2018
---
# <a name="quickstart-pause-and-resume-compute-for-an-azure-sql-data-warehouse-in-the-azure-portal"></a>Szybki Start: Wstrzymywanie i wznawianie obliczeniowe dla usługi Azure SQL Data Warehouse w portalu Azure
Wstrzymaj obliczeniowe dla magazynu danych SQL Azure w celu ograniczenia kosztów. Wznowić operacje obliczeniowe, gdy wszystko będzie gotowe do użycia w magazynie danych.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Użyj [tworzenie i Connect - portal](create-data-warehouse-portal.md) można utworzyć magazynu danych, nazywane **mySampleDataWarehouse**. 

## <a name="pause-compute"></a>Wstrzymaj obliczeń
W celu ograniczenia kosztów, możesz wstrzymywać i wznawiać obliczeń zasobów na żądanie. Na przykład jeśli nie będzie używać bazy danych w nocy i w weekendy, można wstrzymywać go w tych godzinach i wznawiać go w ciągu dnia. Użytkownik nie zostanie obciążona dla zasobów obliczeniowych, podczas bazy danych zostało wstrzymane. Jednak będą nadal naliczane za magazynu. 

Wykonaj następujące kroki, aby wstrzymać SQL data warehouse.

1. Kliknij przycisk **baz danych** w po lewej stronie portalu Azure.
2. Wybierz **mySampleDataWarehouse** z **baz danych SQL** strony. Spowoduje to otwarcie magazynu danych. 
3. Na **mySampleDataWarehouse** strony, zwróć uwagę, **stan** jest **Online**.

    ![Obliczenia bazy danych online](media/pause-and-resume-compute-portal/compute-online.png)

4. Aby wstrzymać hurtowni danych, kliknij przycisk **wstrzymać** przycisku. 
5. Zostanie wyświetlone pytanie potwierdzenia z pytaniem, jeśli chcesz kontynuować. Kliknij przycisk **Yes** (Tak).
6. Poczekaj chwilę, a następnie zanotuj **stan** jest **wstrzymywanie**.

    ![Wstrzymywanie](media/pause-and-resume-compute-portal/pausing.png)

7. Po zakończeniu operacji Wstrzymaj jego stan to **Paused** i jest przycisk opcji **Start**.
8. Zasoby obliczeniowe dla magazynu danych są teraz w trybie offline. Użytkownik nie zostanie obciążona obliczania, dopóki wznowić działanie usługi.

    ![Obliczenia bazy danych w trybie offline](media/pause-and-resume-compute-portal/compute-offline.png)


## <a name="resume-compute"></a>Wznów obliczeń
Wykonaj następujące kroki, aby wznowić SQL data warehouse.

1. Kliknij przycisk **baz danych** w po lewej stronie portalu Azure.
2. Wybierz **mySampleDataWarehouse** z **baz danych SQL** strony. Spowoduje to otwarcie magazynu danych. 
3. Na **mySampleDataWarehouse** strony, zwróć uwagę, **stan** jest **Paused**.

    ![Obliczenia bazy danych w trybie offline](media/pause-and-resume-compute-portal/compute-offline.png)

4. Aby wznowić hurtowni danych, kliknij przycisk **Start**. 
5. Zostanie wyświetlone pytanie potwierdzenia z pytaniem, czy chcesz uruchomić. Kliknij przycisk **Yes** (Tak).
6. Powiadomienie **stan** jest **wznawianie**.

    ![Wznawianie](media/pause-and-resume-compute-portal/resuming.png)

7. Po powrocie do trybu online w magazynie danych jest stan **Online** i jest przycisk opcji **Wstrzymaj**.
8. Zasoby obliczeniowe dla magazynu danych są teraz w trybie online i korzystasz z usługi. Podjęto opłat za obliczeń.

    ![Obliczenia bazy danych online](media/pause-and-resume-compute-portal/compute-online.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Zawierają jednostki magazynu danych i dane przechowywane w magazynie danych. Opłaty za te zasoby obliczeniowe i magazynowe są naliczane osobno. 

- Jeśli chcesz przechowywać dane w magazynie, wstrzymać obliczeń.
- Aby uniknąć opłat w przyszłości, możesz usunąć magazyn danych. 

Wykonaj następujące kroki, aby wyczyścić zasoby zgodnie z potrzebami.

1. Zaloguj się do [portalu Azure](https://portal.azure.com)i kliknij polecenie w magazynie danych.

    ![Oczyszczanie zasobów](media/load-data-from-azure-blob-storage-using-polybase/clean-up-resources.png)

1. Aby wstrzymać obliczenia, kliknij przycisk **Wstrzymaj**. Gdy magazyn danych jest wstrzymany, zobaczysz przycisk **Uruchom**.  Aby wznowić obliczenia, kliknij przycisk **Uruchom**.

2. Aby usunąć magazyn danych i nie płacić za obliczenia oraz magazynowanie, kliknij przycisk **Usuń**.

3. Aby usunąć serwer SQL został utworzony, kliknij przycisk **mynewserver 20171113.database.windows.net**, a następnie kliknij przycisk **usunąć**.  Należy zachować ostrożność podczas usuwania, ponieważ usunięcie serwera spowoduje również usunięcie wszystkich baz danych przypisanych do tego serwera.

4. Aby usunąć grupę zasobów, kliknij pozycję **myResourceGroup**, a następnie kliknij pozycję **Usuń grupę zasobów**.


## <a name="next-steps"></a>Kolejne kroki
Masz teraz wstrzymana i wznowione obliczeniowe magazynu danych. Aby dowiedzieć się więcej na temat usługi Azure SQL Data Warehouse, przejdź do samouczka na temat ładowania danych.

> [!div class="nextstepaction"]
>[Ładowanie danych do magazynu danych SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
