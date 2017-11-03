---
title: "Azure wysoką dostępność usług Analysis Services | Dokumentacja firmy Microsoft"
description: "Zapewnienie wysokiej dostępności usług Azure Analysis Services."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: 554c5e6e3e3cfa2742ef27a3c1510176184b6bd0
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/02/2017
---
# <a name="analysis-services-high-availability"></a>Wysoka dostępność usługi analizy
W tym artykule opisano, zapewniając wysoką dostępność dla serwerów usług Azure Analysis Services. 


## <a name="assuring-high-availability-during-a-service-disruption"></a>Zapewnienie wysokiej dostępności podczas przerw w działaniu usługi
Gdy przypadkach centrum danych platformy Azure mogą mieć awarii. W przypadku wystąpienia awarii powoduje zakłócenia biznesowych, może trwać kilka minut lub może trwać godzin. Wysoka dostępność najczęściej jest to osiągane z nadmiarowością serwera. Z usług Azure Analysis Services można osiągnąć nadmiarowość przez utworzenie dodatkowych, pomocnicze serwery w regionach co najmniej jeden. Podczas tworzenia nadmiarowych serwerów, aby upewnić się, że dane i metadane na tych serwerach jest zsynchronizowana z serwerem w regionie, który przeszedł do trybu offline, można wykonywać następujące czynności:

* Wdrażanie modeli do nadmiarowego serwerów w różnych regionach. Ta metoda wymaga przetwarzania danych na serwerze podstawowym i nadmiarowego serwerów w równolegle, zapewniając wszystkie serwery są w synchronizacji.

* Wykonaj kopię zapasową baz danych z serwera podstawowego i przywracania na serwerach nadmiarowe. Na przykład można zautomatyzować co noc kopii zapasowych do magazynu Azure i przywrócić na inne serwery nadmiarowego w różnych regionach. 

W obu przypadkach Jeśli serwer podstawowy ulegnie awarii, należy zmienić parametry połączenia w raportowania klientom na łączenie się z serwerem w różnych regionalnych centrach danych. Tej zmiany należy traktować jako ostateczność i występuje tylko w przypadku awarii centrum danych w wyniku katastrofy regionalne. Jest bardziej prawdopodobne, awarii centrum danych podstawowego serwera hostingu przybyły trybu online przed można zaktualizować połączenia na wszystkich klientach. 



## <a name="related-information"></a>Informacje pokrewne
[Kopia zapasowa i przywracanie](analysis-services-backup.md)   
[Zarządzanie usług Azure Analysis Services](analysis-services-manage.md) 

