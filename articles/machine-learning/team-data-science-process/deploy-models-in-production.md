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
ms.openlocfilehash: 122c6db2a915dd2a933e261b5b735e7214407d66
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/14/2018
---
# <a name="deploy-models-in-production"></a>Wdrażanie modeli w środowisku produkcyjnym

Wdrożenia produkcyjnego umożliwia modelu do pełnienia roli active w firmie. Prognoz z wdrożonym modelu może służyć do decyzje biznesowe.

## <a name="production-platforms"></a>Platform produkcyjnych
Istnieją różne podejścia i platformy, aby umieścić modeli w środowisku produkcyjnym. Poniżej przedstawiono kilka opcji:


- [Model wdrażania w usłudze Azure Machine Learning](../preview/model-management-overview.md)
- [Wdrażania modelu w programie SQL server](https://docs.microsoft.com/sql/advanced-analytics/tutorials/sqldev-py6-operationalize-the-model)
- [Microsoft Machine Learning Server](https://docs.microsoft.com/sql/advanced-analytics/r/r-server-standalone)


>[!NOTE]
>Przed wdrożeniem jeden musi upewnić się, że opóźnienie oceniania modelu jest niski do użycia w środowisku produkcyjnym.
>


>[!NOTE]
>Wdrożenie przy użyciu usługi Azure Machine Learning Studio, zobacz [wdrażanie usługi sieci web Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md).
>

## <a name="ab-testing"></a>A / B, testowanie
W przypadku wielu modeli w środowisku produkcyjnym, może być przydatne do wykonania [A / B, testowanie](https://en.wikipedia.org/wiki/A/B_testing) porównanie wydajności modeli. 

 
## <a name="next-steps"></a>Kolejne kroki

Wskazówki, które pokazują wszystkie kroki procesu **określonych scenariuszy** podawane są również. Wymieniono i połączone z opisami miniatur w [wskazówki przykład](walkthroughs.md) artykułu. Pokazują one sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego. 
 


