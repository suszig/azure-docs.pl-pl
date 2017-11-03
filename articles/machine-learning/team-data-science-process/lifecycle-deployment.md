---
title: "Etap wdrażania zespołu danych nauki procesu cyklu - Azure | Dokumentacja firmy Microsoft"
description: "Cele, zadań i elementów dostarczanych w etapie wdrażania projektów analizy danych."
services: machine-learning
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
ms.date: 09/02/2017
ms.author: bradsev;
ms.openlocfilehash: b49b3ab696c22928c5f5a4566e059345fd810588
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="deployment"></a>Wdrożenie

W tym temacie przedstawiono cele, zadań, oraz materiałów związanych z **wdrożenia** procesu nauki danych zespołu. Ten proces obejmuje zalecane cykl służy do struktury projektów analizy danych. Cykl życia przedstawiono główne etapy, które projekty zazwyczaj wykonywane, często wielokrotnie powtarzane:

* **Opis biznesowa**
* **Uzyskiwanie danych i zrozumienie**
* **Modelowanie**
* **Wdrożenie**
* **Akceptacji klienta**

W tym miejscu jest wizualną reprezentacją **cyklu życia procesu nauki danych zespołu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Cel
* Modele z potokiem danych są wdrażane na środowisko produkcyjne lub środowiska przypominającej środowisko produkcyjne dla użytkowników końcowych. 

## <a name="how-to-do-it"></a>Jak to zrobić
Zadania główne zostały omówione w tym etapie:

* **Operacjonalizuj modelu**: wdrażania modelu i potoku na środowisko produkcyjne lub środowiska przypominającej środowisko produkcyjne korzystania z aplikacji.

### <a name="41-operationalize-a-model"></a>4.1 operacjonalizacji modelu
Po utworzeniu zestaw modeli, które również wykonywać, może być operationalized dla innych aplikacji do pracy z. W zależności od wymagań biznesowych prognoz są wykonywane w czasie rzeczywistym lub na podstawie partii. Modele są wdrażane przez udostępnianie ich z interfejsem API otwarty. Interfejs umożliwia modelu, który ma być łatwo używane z różnych aplikacji, takich jak witryny sieci Web w trybie online, arkusze kalkulacyjne, pulpity nawigacyjne lub aplikacje biznesowe i wewnętrznej bazy danych. Przykłady operationalization modelu z usługą sieci web uczenie maszynowe Azure można znaleźć [wdrażanie usługi sieci web Azure Machine Learning](../studio/publish-a-machine-learning-web-service.md). On również najlepszym rozwiązaniem jest tworzenie telemetrii i monitorowania w modelu produkcji i potoku danych, które są wdrożone. Takie rozwiązanie pomaga w kolejnych systemu stanu raportowania i rozwiązywania problemów.  

## <a name="artifacts"></a>Artefakty
* Pulpit nawigacyjny stanu z systemu metryki kondycji i klucza.
* Raport końcowy modelowania z szczegóły wdrożenia.
* Dokument architektury rozwiązania final.


## <a name="next-steps"></a>Następne kroki

Oto łącza do każdego kroku w cyklu życia procesu nauki danych zespołu:

* [1. Opis biznesowa](lifecycle-business-understanding.md)
* [2. Uzyskiwanie danych i zrozumienie](lifecycle-data.md)
* [3. Modelowanie](lifecycle-modeling.md)
* [4. Wdrożenia](lifecycle-deployment.md)
* [5. Akceptacji klienta](lifecycle-acceptance.md)

Pełne end-to-end wskazówki, które pokazują wszystkie kroki procesu **określonych scenariuszy** podawane są również. Wymieniono i połączone z opisami miniatur w [wskazówki przykład](walkthroughs.md) tematu. Pokazują one sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego. 

Przykłady wykonywania czynności w procesie nauki zespołu danych korzystających z usługi Azure Machine Learning Studio można znaleźć [z ML Azure](http://aka.ms/datascienceprocess) ścieżkę szkoleniową.