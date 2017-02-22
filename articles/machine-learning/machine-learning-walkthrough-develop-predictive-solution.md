---
title: "Rozwiązanie predykcyjne do oceny ryzyka kredytowego w usłudze Machine Learning | Microsoft Docs"
description: "Szczegółowy przewodnik pokazujący sposób tworzenia rozwiązania analizy predykcyjnej w celu oceny ryzyka kredytowego w usłudze Azure Machine Learning Studio."
keywords: "ryzyko kredytowe, rozwiązanie analizy predykcyjnej, ocena ryzyka"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 43300854-a14e-4cd2-9bb1-c55c779e0e93
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 07cb3fe0d5688d5b63fe3312cad14c2274a58a09
ms.openlocfilehash: e98a64910f28da0a8a9b4a58c717c40d791ccf00


---
# <a name="walkthrough-develop-a-predictive-analytics-solution-for-credit-risk-assessment-in-azure-machine-learning"></a>Przewodnik: tworzenie rozwiązania analizy predykcyjnej w celu oceny ryzyka kredytowego w usłudze Azure Machine Learning

W tym przewodniku przedstawiono szczegółowo proces opracowywania rozwiązania w usłudze Machine Learning Studio. Opracowaliśmy model analizy predykcyjnej w usłudze Machine Learning Studio, a następnie wdrożyliśmy go w postaci usługi sieci Web Azure Machine Learning, w której model może tworzyć prognozy przy użyciu nowych danych. 

> [!TIP]
> W tym przewodniku przyjęto założenie, że co najmniej raz użyto wcześniej usługi Machine Learning Studio oraz że znasz niektóre pojęcia związane z uczeniem maszynowym, mimo że nie jesteś ekspertem w tych dziedzinach.
> 
>Jeśli nigdy wcześniej nie używano usługi **Azure Machine Learning Studio**, możesz najpierw skorzystać z samouczka [Tworzenie pierwszego eksperymentu dotyczącego przetwarzania danych w usłudze Azure Machine Learning Studio](machine-learning-create-experiment.md). Ten samouczek przeprowadza Cię przez usługę Machine Learning Studio po raz pierwszy. Pokazano w nim podstawowe informacje dotyczące przeciągania modułów i upuszczania ich w obszarze eksperymentu, łączenia modułów ze sobą, uruchamiania eksperymentu oraz przeglądania wyników.
>
>Jeśli nie zdarzyło Ci się korzystać z uczenia maszynowego, najlepiej rozpocząć od serii filmów wideo [Przetwarzanie danych dla początkujących](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md). Seria ta stanowi znakomite wprowadzenie do uczenia maszynowego, przedstawione przy użyciu potocznego języka i codziennych pojęć.
> 

## <a name="the-problem"></a>Problem

Załóżmy, że chcesz przewidzieć ryzyko kredytowe osoby na podstawie informacji przekazanych we wniosku kredytowym.  

Ocena ryzyka kredytowego to złożony problem, ale odrobinę uprościmy badane parametry. Następnie użyjemy ich jako przykładu sposobu użycia usługi Microsoft Azure Machine Learning z usługą Machine Learning Studio oraz z usługą sieci Web Machine Learning w celu utworzenia rozwiązania analizy predykcyjnej.  

## <a name="the-solution"></a>Rozwiązanie

W tym szczegółowym samouczku najpierw użyto dostępnych publicznie danych dotyczących ryzyka kredytowego, opracowano model predykcyjny i przeprowadzono jego uczenie z użyciem tych danych, a następnie wdrożono ten model jako usługę sieci Web, która będzie dostępna dla innych osób chcących przeprowadzić ocenę ryzyka kredytowego.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

Aby utworzyć rozwiązanie do oceny ryzyka kredytowego, wykonamy następujące kroki:  

1. [Tworzenie obszaru roboczego usługi Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Przekazywanie istniejących danych](machine-learning-walkthrough-2-upload-data.md)
3. [Tworzenie nowego eksperymentu](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Nauczanie i ocena modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Wdrażanie usługi sieci Web](machine-learning-walkthrough-5-publish-web-service.md)
6. [Uzyskiwanie dostępu do usługi sieci Web](machine-learning-walkthrough-6-access-web-service.md)

Ten przewodnik jest oparty na uproszczonej wersji przykładowego eksperymentu [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (Klasyfikacja binarna: przewidywanie ryzyka kredytowego) dostępnego w witrynie [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).


> [!TIP]
> Aby pobrać i wydrukować diagram, który zawiera przegląd możliwości usługi Machine Learning Studio, zobacz [Diagram przeglądowy możliwości usługi Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).
> 
> 



<!--HONumber=Dec16_HO3-->


