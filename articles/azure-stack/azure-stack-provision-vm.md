---
title: Tworzenie testowej maszyny Wirtualnej w stosie Azure | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak udostępnić testowej maszyny Wirtualnej w stosie Azure jako operatorowi chmury."
services: azure-stack
documentationcenter: 
author: brenduns
manager: femila
editor: 
ms.assetid: c86646e1-a12e-493f-b396-f17bfacd60c2
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 9/25/2017
ms.author: brenduns
ms.reviewer: 
ms.openlocfilehash: 41096f68e5e7d9e31098d1e8919101418abe4c03
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="create-a-test-virtual-machine-in-azure-stack"></a>Utwórz testową maszynę wirtualną w stosie Azure

*Dotyczy: Azure stosu Development Kit*

Jako operator stosu Azure można utworzyć testowej maszyny wirtualnej w celu zweryfikowania Twojej [stosu Azure](azure-stack-poc.md) wdrożenia zestaw dewelopera.

> [!NOTE]
> Przed udostępnieniem można maszyn wirtualnych, należy najpierw [Dodaj obraz systemu Windows Server 2016 oceny do stosu Azure marketplace](azure-stack-add-default-image.md).
> 
> 

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej
1. Na hoście Azure stosu Development Kit [Zaloguj](azure-stack-connect-azure-stack.md) do portalu administratora (`https://adminportal.local.azurestack.external`), a następnie kliknij przycisk **nowy** > **obliczeniowe**  >  **Systemu Windows Server 2016 Datacenter Eval** > **utworzyć**.  
2. W **podstawy** bloku, wpisz **nazwa**, **nazwy użytkownika**, i **hasło**. Wybierz **subskrypcji**. Utwórz **grupy zasobów**, lub wybierz istniejący, a następnie kliknij przycisk **OK**.  
3. W **wybierz rozmiar** bloku, kliknij przycisk **A1 standardowe**, a następnie kliknij przycisk **wybierz**.  
4. W **ustawienia** bloku, zaakceptuj ustawienia domyślne i kliknij przycisk **OK**
5. W bloku **Podsumowanie** kliknij przycisk **OK**, aby utworzyć maszynę wirtualną.  
6. Aby zapoznać się z nowej maszyny wirtualnej, kliknij przycisk **wszystkie zasoby**, następnie wyszukaj maszyny wirtualnej i kliknij jego nazwę.
    ![](media/azure-stack-provision-vm/image06.png)


## <a name="next-steps"></a>Kolejne kroki
[Przy użyciu portali administratora i użytkownika w stosie Azure](azure-stack-manage-portals.md)
