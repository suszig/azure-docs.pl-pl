---
title: Etap akceptacji klienta Team danych nauki procesu cyklu - Azure | Dokumentacja firmy Microsoft
description: "Cele, zadań i elementów dostarczanych w etapie akceptacji klienta projektów analizy danych."
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
ms.openlocfilehash: 953f090f775da5e48b2f4ed36550a5624ae4596b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="customer-acceptance"></a>Akceptacja klienta

W tym temacie przedstawiono cele, zadań, oraz materiałów związanych z **etap akceptacji klienta** procesu nauki danych zespołu. Ten proces obejmuje zalecane cykl służy do struktury projektów analizy danych. Cykl życia przedstawiono główne etapy, które projekty zazwyczaj wykonywane, często wielokrotnie powtarzane:

* **Opis biznesowa**
* **Uzyskiwanie danych i zrozumienie**
* **Modelowanie**
* **Wdrożenie**
* **Akceptacji klienta**

W tym miejscu jest wizualną reprezentacją **cyklu życia procesu nauki danych zespołu**. 

![TDSP Lifecycle2](./media/lifecycle/tdsp-lifecycle2.png) 


## <a name="goal"></a>Cel
* **Finalizuj elementy dostarczane**: Sprawdź, czy potoku, modelu i ich wdrażania w środowisku produkcyjnym są spełniającą cele klienta.

## <a name="how-to-do-it"></a>Jak to zrobić
Istnieją dwa główne zadania zostały omówione w tym etapie:

* **System sprawdzania poprawności**: Potwierdź modelu wdrożonym, potok spełniają potrzeb klientów.
* **Projekt ręcznie wyłączyć**: do jednostki, która ma być uruchamiany system w środowisku produkcyjnym.

Klienta należy zweryfikować, że system spełnia ich potrzeb biznesowych i odpowiedzi pytania z dokładnością dopuszczalne do wdrożenia w środowisku produkcyjnym dla systemu za pomocą ich aplikacji klienckiej. Cała dokumentacja sfinalizowana, a sprawdzone. Ręcznie wyłączyć projektu na jednostka odpowiedzialna za operacje zostanie ukończona. Ta jednostka można na przykład IT lub zespół nauki danych klienta lub agenta klienta, który odpowiada za uruchamianie systemu w środowisku produkcyjnym. 

## <a name="artifacts"></a>Artefakty
Głównym artefaktu wygenerowane w tym ostatnim krokiem jest **zakończenia raportu z projektu dla klienta**. Jest to techniczne raport zawierający wszystkie szczegóły projektu, to warto poznać i uruchomić system. [Zakończenia raportu](https://github.com/Azure/Azure-TDSP-ProjectTemplate/blob/master/Docs/Project/Exit%20Report.md) szablonu jest zapewniana przez TDSP, która może być używany jako jest lub dostosować dla określonego klienta. 


## <a name="next-steps"></a>Następne kroki

Oto łącza do każdego kroku w cyklu życia procesu nauki danych zespołu:

* [1. Opis biznesowa](lifecycle-business-understanding.md)
* [2. Uzyskiwanie danych i zrozumienie](lifecycle-data.md)
* [3. Modelowanie](lifecycle-modeling.md)
* [4. Wdrożenia](lifecycle-deployment.md)
* [5. Akceptacji klienta](lifecycle-acceptance.md)

Pełne end-to-end wskazówki, które pokazują wszystkie kroki procesu **określonych scenariuszy** podawane są również. Wymieniono i połączone z opisami miniatur w [wskazówki przykład](walkthroughs.md) tematu. Pokazują one sposób łączenia chmury, narzędzia lokalnych i usług w przepływie pracy lub potoku, aby utworzyć aplikację inteligentnego. 

Przykłady wykonywania czynności w procesie nauki zespołu danych korzystających z usługi Azure Machine Learning Studio można znaleźć [z ML Azure](http://aka.ms/datascienceprocess) ścieżkę szkoleniową.