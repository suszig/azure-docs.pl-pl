---
title: "Skalowanie programu Power BI Embedded pojemności | Dokumentacja firmy Microsoft"
description: "W tym artykule przedstawiono sposób skalowania Power BI Embedded pojemności w Microsoft Azure."
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 09/28/2017
ms.author: asaxton
ms.openlocfilehash: e1ab6a2f52fa56f1e04c6c327796587daf43596e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="scale-your-power-bi-embedded-capacity"></a>Skalowanie programu Power BI Embedded pojemności

W tym artykule przedstawiono sposób skalowania Power BI Embedded pojemności w Microsoft Azure. Skalowanie pozwala zwiększyć lub zmniejszyć wydajność.

Przyjęto założenie, że utworzono Power BI Embedded pojemności. Jeśli nie masz, zobacz [utworzyć Power BI Embedded pojemności w portalu Azure](create-capacity.md) rozpocząć pracę.

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/).

## <a name="scale-a-capacity"></a>Skala pojemności

1. Zaloguj się do [Azure Portal](https://portal.azure.com/).

2. Wybierz **więcej usług** > **Power BI Embedded** wyświetlić pojemności sieci.

    ![Więcej usług w portalu Azure](media/scale-capacity/azure-portal-more-services.png)

3. Wybierz pojemności, który chcesz skalować.

    ![Power BI Embedded listy pojemności w portalu Azure](media/scale-capacity/azure-portal-capacity-list.png)

4. Wybierz **warstwa cenowa** w obszarze **skali** Twojego wydajności.

    ![Opcji skalowania warstwy cenowej](media/scale-capacity/azure-portal-scale-pricing-tier.png)

    Bieżący poziom cenowy opisanym w kolorze niebieskim.

    ![Bieżąca wyróżnione kolorem niebieskim warstwy cenowej](media/scale-capacity/azure-portal-current-tier.png)

5. Aby skalować w górę lub w dół, wybierz nową warstwę, aby przenieść. Po zaznaczeniu nową warstwę umieszczone przerywaną niebieski wokół zaznaczenia. Wybierz **wybierz** można skalować do nowej warstwy.

    ![Wybierz nową warstwę](media/scale-capacity/azure-portal-select-new-tier.png)

    Skalowanie wydajność może potrwać minutę lub dwie do wykonania.

6. Potwierdź warstwę, wyświetlając karta Przegląd. Bieżąca warstwa cenowa jest wyświetlana.

    ![Potwierdź bieżącej warstwy](media/scale-capacity/azure-portal-confirm-tier.png)

## <a name="next-steps"></a>Następne kroki

Aby wstrzymać lub uruchomić możliwości, zobacz [wstrzymać i uruchomić Power BI Embedded wydajność w portalu Azure](pause-start.md).

Aby rozpocząć, osadzanie zawartość usługi Power BI w aplikacji, zobacz [jak osadzić pulpitów nawigacyjnych usługi Power BI, raporty i Kafelki programu](https://powerbi.microsoft.com/documentation/powerbi-developer-embedding-content/).

Masz więcej pytań? [Spróbuj skorzystać z społeczności Power BI](http://community.powerbi.com/)