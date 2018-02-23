---
title: 'Szybki Start: Skalowanie obliczeniowych w magazynie danych SQL Azure - Azure portal | Dokumentacja firmy Microsoft'
description: "Azure zadania portalu do zarządzania mocy obliczeniowej. Zasoby obliczeniowe skali przez dostosowanie wartości dwu. Wstrzymać lub wznowić zasobów obliczeniowych w celu ograniczenia kosztów."
services: sql-data-warehouse
documentationcenter: NA
author: hirokib
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: manage
ms.date: 01/31/2018
ms.author: elbutter;barbkess
ms.openlocfilehash: 6b86042ed6b95ba49fa2089ba36b1dbe9a61cc40
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="quickstart-scale-compute-in-azure-sql-data-warehouse-in-the-azure-portal"></a>Szybki Start: Skali obliczeń w usłudze Azure SQL Data Warehouse w portalu Azure

Skalowanie możliwości obliczeniowych w usłudze Azure SQL Data Warehouse w portalu Azure. [Skalowanie w poziomie obliczeń](sql-data-warehouse-manage-compute-overview.md) dla lepszą wydajność i skalę kopii obliczeń w celu ograniczenia kosztów. 

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="sign-in-to-the-azure-portal"></a>Logowanie się do witryny Azure Portal

Zaloguj się w witrynie [Azure Portal](https://portal.azure.com/).

## <a name="before-you-begin"></a>Przed rozpoczęciem

Można skalować magazyn danych, który już masz, lub użyj [Szybki Start: tworzenie i łączenie - portal](create-data-warehouse-portal.md) można utworzyć magazynu danych o nazwie **mySampleDataWarehouse**.  Ta opcja szybkiego startu skaluje **mySampleDataWarehouse**.

## <a name="scale-compute"></a>Skalowanie zasobów obliczeniowych

W usłudze SQL Data Warehouse można zwiększyć lub zmniejszyć zasoby obliczeniowe przez dostosowanie wartości właściwości jednostki magazynu danych. [Tworzenie i Connect - portal](create-data-warehouse-portal.md) utworzony **mySampleDataWarehouse** i zainicjować go od 400 jednostek dwu. Poniższe kroki Dostosuj liczbę jednostek dwu dla **mySampleDataWarehouse**.

Aby zmienić jednostki magazynu danych:

1. Kliknij przycisk **baz danych** w po lewej stronie portalu Azure.
2. Wybierz **mySampleDataWarehouse** z **baz danych SQL** strony. Otworzy się w magazynie danych.
3. Kliknij przycisk **skali**.

    ![Kliknij polecenie Skaluj](media/quickstart-scale-compute-portal/click-scale.png)

2. W panelu skali Przesuń suwak w lewo lub w prawo zmienić ustawienie wartości DWU.

    ![Przesuń suwak](media/quickstart-scale-compute-portal/scale-dwu.png)

3. Kliknij pozycję **Zapisz**. Zostanie wyświetlony komunikat potwierdzenia. Kliknij przycisk **tak** o potwierdzenie lub **nie** do anulowania.

    ![Klikanie pozycji Zapisz.](media/quickstart-scale-compute-portal/confirm-change.png)



## <a name="next-steps"></a>Kolejne kroki
Teraz uzyskanych jak skalować obliczeń do magazynu danych. Aby dowiedzieć się więcej na temat usługi Azure SQL Data Warehouse, przejdź do samouczka na temat ładowania danych.

> [!div class="nextstepaction"]
>[Ładowanie danych do magazynu danych SQL Data Warehouse](load-data-from-azure-blob-storage-using-polybase.md)
