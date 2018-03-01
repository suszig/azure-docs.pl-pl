---
title: "Importowanie pliku programu Power BI Desktop do usług Azure Analysis Services | Dokumentacja firmy Microsoft"
description: "Opisuje sposób importowania pliku Power BI Desktop (pbix) przy użyciu portalu Azure."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/26/2018
ms.author: owend
ms.openlocfilehash: 43eab587a1e5209069a248f1e2e1f57af158a2b8
ms.sourcegitcommit: 088a8788d69a63a8e1333ad272d4a299cb19316e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/27/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Importowanie pliku Power BI Desktop

W usłudze Azure można utworzyć nowy model, importując Power BI Desktop (pbix) pliku. Metadane modelu danych z pamięci podręcznej i połączenia źródła danych są importowane. Raporty i wizualizacji nie zostały zaimportowane. Raz na serwerze, modelu można zmodyfikować aktualizacji i ponownie zaimportować plik pbix, korzystając z funkcji designer (wersja zapoznawcza) w sieci web w portalu lub przy użyciu programu SQL Server Management Studio (SSMS). Importowanych modele nie można otworzyć lub wyeksportowane do programu Visual Studio.

> [!NOTE]
> Jeśli pbix model nawiązuje połączenie ze źródłami danych lokalnych, [brama lokalna](analysis-services-gateway.md) muszą być skonfigurowane dla serwera.

## <a name="to-import-from-pbix"></a>Aby zaimportować z pbix

1. Znajdujących się na serwerze **omówienie** > **Projektant stron sieci Web**, kliknij przycisk **Otwórz**.

    ![Tworzenie modelu w portalu Azure](./media/analysis-services-create-model-portal/aas-create-portal-overview-wd.png)

2. W **Projektant stron sieci Web** > **modele**, kliknij przycisk **+ Dodaj**.

    ![Tworzenie modelu w portalu Azure](./media/analysis-services-create-model-portal/aas-create-portal-models.png)

3. W **nowy model**, wpisz nazwę modelu, a następnie wybierz plik Power BI Desktop.

    ![Okno dialogowe nowego modelu w portalu Azure](./media/analysis-services-import-pbix/aas-import-pbix-new-model.png)

4. W **importu**Znajdź i zaznacz plik.

     ![Połącz okna dialogowego w portalu Azure](./media/analysis-services-import-pbix/aas-import-pbix-select-file.png)

## <a name="see-also"></a>Zobacz także

[Tworzenie modelu w portalu Azure](analysis-services-create-model-portal.md)   
[Połącz do usług Azure Analysis Services](analysis-services-connect.md)  
