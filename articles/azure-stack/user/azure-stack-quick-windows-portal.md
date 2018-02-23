---
title: "Azure stosu Szybki Start - Utwórz maszynę wirtualną systemu Windows"
description: "Azure stosu Szybki Start - Utwórz maszynę Wirtualną z systemem Windows przy użyciu portalu"
services: azure-stack
author: brenduns
manager: femila
ms.service: azure-stack
ms.topic: quickstart
ms.date: 09/15/2017
ms.author: brenduns
ms.reviewer: 
ms.custom: mvc
ms.openlocfilehash: 9df976fd26318038ab4152863638e262d7f284aa
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/22/2018
---
# <a name="create-a-windows-virtual-machine-with-the-azure-stack-portal"></a>Utwórz maszynę wirtualną z systemem Windows przy użyciu portalu Azure stosu

Można utworzyć maszyny wirtualnej systemu Windows przy użyciu portalu Azure stosu. Portal programu jest interfejs użytkownika oparty na przeglądarce, gdzie można utworzyć, konfigurowanie i zarządzanie zasobami.

## <a name="sign-in-to-the-azure-stack-portal"></a>Zaloguj się do portalu Azure stosu

Zaloguj się do portalu Azure stosu. Adres w portalu Azure stosu zależy od tego, na który produkt stosu Azure łączysz się:

* Dla usługi Azure stosu Development Kit (ASDK), przejdź do: https://portal.local.azurestack.external.
* System Azure stosu zintegrowane przejdź do adresu URL, zapewnianej przez operatora sieci Azure stosu.

## <a name="create-a-virtual-machine"></a>Tworzenie maszyny wirtualnej

1. Kliknij przycisk **nowe** > **obliczeniowe** > **systemu Windows Server 2016 Datacenter Eval** > **utworzyć**. Jeśli nie widzisz **Eval centrum danych systemu Windows Server 2016** wpisu, skontaktuj się z operatorem stosu Azure. Poproś o dodanie go do witryny marketplace zgodnie z objaśnieniem w [Dodaj obraz maszyny Wirtualnej systemu Windows Server 2016 do stosu Azure marketplace](../azure-stack-add-default-image.md) artykułu. 
    ![](media/azure-stack-quick-windows-portal/image01.png)
2. W obszarze **podstawy**, wpisz **nazwa**, **nazwy użytkownika**, i **hasło**. Wybierz **subskrypcji**. Utwórz **grupy zasobów**, lub wybierz istniejący jeden, kliknij **lokalizacji**, a następnie kliknij przycisk **OK**.

    ![](media/azure-stack-quick-windows-portal/image02.png)
3. W obszarze **wybierz rozmiar**, kliknij przycisk **D1 standardowe** > **wybierz**.
    ![](media/azure-stack-quick-windows-portal/image03.png)
4. W obszarze **ustawienia**, zaakceptuj ustawienia domyślne i kliknij przycisk **OK**.
    ![](media/azure-stack-quick-windows-portal/image04.png)
5. W obszarze **Podsumowanie**, kliknij przycisk **OK** można utworzyć maszyny wirtualnej. 
    ![](media/azure-stack-quick-windows-portal/image05.png)
6. Aby zapoznać się z nowej maszyny wirtualnej, kliknij przycisk **wszystkie zasoby**, następnie wyszukaj maszyny wirtualnej i kliknij jego nazwę.
    ![](media/azure-stack-quick-windows-portal/image06.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

Gdy maszyna wirtualna nie jest już potrzebny, Usuń grupę zasobów, maszyny wirtualnej i wszystkie powiązane zasoby. Aby to zrobić, wybierz grupę zasobów na stronie maszyny wirtualnej, a następnie kliknij przycisk **usunąć**.

## <a name="next-steps"></a>Kolejne kroki
W tym szybki start wdrożeniu prostą maszynę wirtualną systemu Windows. Aby dowiedzieć się więcej o maszynach wirtualnych Azure stosu, nadal [zagadnienia dotyczące maszyn wirtualnych w stosie Azure](azure-stack-vm-considerations.md).
