<properties
    pageTitle="Rozwiązanie predykcyjne do oceny ryzyka kredytowego w usłudze Machine Learning | Microsoft Azure"
    description="Szczegółowy przewodnik pokazujący sposób tworzenia rozwiązania analizy predykcyjnej w celu oceny ryzyka kredytowego w usłudze Azure Machine Learning Studio."
    keywords="credit risk, predictive analytics solution,risk assessment"
    services="machine-learning"
    documentationCenter=""
    authors="garyericson"
    manager="paulettm"
    editor="cgronlun"/>

<tags
    ms.service="machine-learning"
    ms.workload="data-services"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="03/09/2016"
    ms.author="garye"/>


# Przewodnik: tworzenie rozwiązania analizy predykcyjnej w celu oceny ryzyka kredytowego w usłudze Azure Machine Learning

Załóżmy, że chcesz przewidzieć ryzyko kredytowe osoby na podstawie informacji przekazanych we wniosku kredytowym.  

Ocena ryzyka kredytowego to złożony problem, ale odrobinę uprościmy badane parametry. Następnie wykorzystamy je jako przykład sposobu użycia usługi Microsoft Azure Machine Learning z usługą Machine Learning Studio oraz z usługą sieci Web Machine Learning w celu utworzenia rozwiązania analizy predykcyjnej.  

W tym szczegółowym przewodniku prześledzimy proces opracowywania modelu analizy predykcyjnej w usłudze Machine Learning Studio, a następnie wdrażania go w postaci usługi sieci Web Machine Learning. Na początek skorzystamy z dostępnych publicznie danych dotyczących ryzyka kredytowego, opracujemy model predykcyjny i przeprowadzimy jego uczenie z wykorzystaniem tych danych, a następnie wdrożymy ten model jako usługę sieci Web, która będzie dostępna dla innych osób chcących przeprowadzić ocenę ryzyka kredytowego.

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

>[AZURE.TIP] Aby pobrać i wydrukować diagram, który zawiera przegląd możliwości usługi Machine Learning Studio, zobacz [Diagram przeglądowy możliwości usługi Azure Machine Learning Studio](machine-learning-studio-overview-diagram.md).

Aby utworzyć rozwiązanie do oceny ryzyka kredytowego, wykonamy następujące kroki:  

1.  [Tworzenie obszaru roboczego usługi Machine Learning](machine-learning-walkthrough-1-create-ml-workspace.md)
2.  [Przekazywanie istniejących danych](machine-learning-walkthrough-2-upload-data.md)
3.  [Tworzenie nowego eksperymentu](machine-learning-walkthrough-3-create-new-experiment.md)
4.  [Nauczanie i ewaluacja modeli](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5.  [Wdrażanie usługi sieci Web](machine-learning-walkthrough-5-publish-web-service.md)
6.  [Uzyskiwanie dostępu do usługi sieci Web](machine-learning-walkthrough-6-access-web-service.md)

Ten przewodnik jest oparty na uproszczonej wersji przykładowego eksperymentu [Binary Classification: Credit risk prediction](http://go.microsoft.com/fwlink/?LinkID=525270) (Klasyfikacja binarna: przewidywanie ryzyka kredytowego) dostępnego w witrynie [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com/).



<!--HONumber=Jun16_HO2-->


