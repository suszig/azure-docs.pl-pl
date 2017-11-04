---
title: "Pobieranie elementów marketplace z platformy Azure | Dokumentacja firmy Microsoft"
description: "Można pobrać elementów marketplace z platformy Azure do mojego wdrożenia stosu Azure."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/03/2017
ms.author: erikje
ms.openlocfilehash: 4d7c335a3c68cc9bb8cb0c823883716a3dd6620a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="download-marketplace-items-from-azure-to-azure-stack"></a>Pobieranie elementów marketplace z platformy Azure do stosu Azure

*Dotyczy: Azure stosu zintegrowanych systemów i Azure stosu Development Kit*

Jak zdecydujesz zawartość do uwzględnienia w Twojej stosu Azure marketplace, należy wziąć pod uwagę zawartości z portalu Azure marketplace. Możesz pobrać wyselekcjonowanych listy elementów witrynę Azure marketplace, które zostały przetestowane na stosie Azure. Nowe elementy często są dodawane do tej listy, dlatego upewnij się, sprawdź, czy ponownie nową zawartość.

Aby pobrać elementów portalu marketplace, należy najpierw [zarejestrować stosu Azure w usłudze Azure](azure-stack-register.md). 

## <a name="download"></a>Do pobrania
1. Zaloguj się do portalu administratora platformy Azure stosu (https://portal.local.azurestack.external).
2. Niektóre elementy marketplace mogą być bardzo duże.  Upewnij się, że masz wystarczającą ilość miejsca w systemie, klikając **dostawców zasobów** > **magazynu**.

    ![](media/azure-stack-download-azure-marketplace-item/image01.png)

3. Kliknij przycisk **więcej usług** > **zarządzania Marketplace**.

    ![](media/azure-stack-download-azure-marketplace-item/image02.png)

4. Kliknij przycisk **Dodaj z platformy Azure** umożliwia wyświetlenie listy dostępnych do pobrania elementów. Możesz kliknąć na każdy element na liście, aby wyświetlić jej opis i rozmiar pliku.

    ![](media/azure-stack-download-azure-marketplace-item/image03.png)

5. Wybierz element na liście, a następnie kliknij przycisk **Pobierz**. Spowoduje to uruchomienie pobierania obrazu maszyny Wirtualnej dla wybranego elementu. Czasu pobierania różnią się.

    ![](media/azure-stack-download-azure-marketplace-item/image04.png)

6. Po zakończeniu pobierania, można wdrożyć z nowy element marketplace jako operator stosu Azure lub użytkownika. Kliknij przycisk **+ nowy**, przeszukiwanie kategorie nowy element marketplace, a następnie wybierz element.
7. Kliknij przycisk **Utwórz** otwarcie środowisko tworzenia dla nowo pobrany element. Postępuj zgodnie z instrukcjami krok po kroku w celu wdrożenia tego elementu.

## <a name="next-steps"></a>Następne kroki

[Tworzenie i publikowanie elementu portalu Marketplace](azure-stack-create-and-publish-marketplace-item.md)
