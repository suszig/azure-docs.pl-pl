---
title: "Wdrażanie modeli w środowisku produkcyjnym — usługi Azure Machine Learning | Dokumentacja firmy Microsoft"
description: "Jak wdrożyć modeli do środowiska produkcyjnego, umożliwiając im odgrywać aktywną rolę w podejmowaniu decyzji biznesowych."
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/30/2017
ms.author: bradsev;
ms.openlocfilehash: c7be9eda2d6f37951eb120250861cf4a39b0141b
ms.sourcegitcommit: be0d1aaed5c0bbd9224e2011165c5515bfa8306c
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/01/2017
---
# <a name="deploy-models-in-production"></a>Wdrażanie modeli w środowisku produkcyjnym

Wdrożenia produkcyjnego umożliwia modelu do pełnienia roli active w firmie. Prognoz z wdrożonym modelu może służyć do decyzje biznesowe.

## <a name="production-platforms"></a>Platform produkcyjnych
Istnieją różne podejścia i platformy, aby umieścić modeli w środowisku produkcyjnym. Poniżej przedstawiono kilka opcji:


- [Model wdrażania w usłudze Azure Machine Learning](https://docs.microsoft.com/azure/machine-learning/preview/model-management-overview)
- [Wdrażania modelu w programie SQL server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Uczenie maszynowe Microsoft Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)


>[!NOTE]
>Przed wdrożeniem jeden musi upewnić się, że opóźnienie oceniania modelu jest niski do użycia w środowisku produkcyjnym.
>


>[!NOTE]
>Wdrożenie przy użyciu usługi Azure Machine Learning Studio, zobacz [wdrażanie usługi sieci web Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A / B, testowanie
W przypadku wielu modeli w środowisku produkcyjnym, może być przydatne do wykonania [A / B, testowanie](https://en.wikipedia.org/wiki/A/B_testing) porównanie wydajności modeli. 

 
## <a name="next-steps"></a>Następne kroki

Wskazówki, które pokazują wszystkie kroki procesu **określonych scenariuszy** podawane są również. Wymieniono i połączone z opisami miniatur w [wskazówki przykład](walkthroughs.md) artykułu. Pokazują one sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego. 
 


