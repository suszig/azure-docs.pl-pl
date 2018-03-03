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
ms.openlocfilehash: e0be0c69b501d7e93c65bcf23d4dd1b6bfa89caf
ms.sourcegitcommit: 782d5955e1bec50a17d9366a8e2bf583559dca9e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 03/02/2018
---
# <a name="import-a-power-bi-desktop-file"></a>Importowanie pliku Power BI Desktop

W usłudze Azure można utworzyć nowy model, importując Power BI Desktop (pbix) pliku. Metadane modelu danych z pamięci podręcznej i połączenia źródła danych są importowane. Raporty i wizualizacji nie zostały zaimportowane.

**Ograniczenia**   
- Pbix model musi łączyć się [usług Analysis Services obsługiwanych źródeł danych](analysis-services-datasource.md) tylko. 
- Pbix model nie może mieć na żywo lub połączeń zapytania bezpośredniego. 
- Jeśli pbix model nawiązuje połączenie ze źródłami danych lokalnych, [bramy danych lokalnych](analysis-services-gateway.md) muszą być skonfigurowane dla serwera usług Analysis Services.
- Importowania może zakończyć się niepowodzeniem, jeśli model danych plik pbix zawiera metadanych nie jest obsługiwane w usługach Analysis Services.

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
