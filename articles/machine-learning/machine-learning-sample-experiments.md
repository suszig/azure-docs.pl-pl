---
title: "Kopiowanie przykładowych eksperymentów uczenia maszynowego | Microsoft Docs"
description: "Dowiedz się, jak wykorzystać przykładowe eksperymenty uczenia maszynowego do tworzenia nowych eksperymentów za pomocą witryny Cortana Intelligence Gallery i usługi Microsoft Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: cjgronlund
manager: jhubbard
editor: cgronlun
ms.assetid: 81e6c1d8-682c-4db3-bfd5-d7bfb1150ff3
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 11/11/2016
ms.author: cgronlun;olgali
translationtype: Human Translation
ms.sourcegitcommit: 35579eda8b92a481525ad987e99e7d9c9ed6b942
ms.openlocfilehash: 7c712cb20d9e008c74436fc602326afa00250c63


---
# <a name="copy-sample-experiments-to-create-new-machine-learning-experiments"></a>Kopiowanie przykładowych eksperymentów w celu tworzenia nowych eksperymentów uczenia maszynowego
Naucz się, jak zacząć od przykładowych eksperymentów z witryny [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/), zamiast tworzyć eksperymenty uczenia maszynowego od zera. Używając przykładów, możesz zbudować własne rozwiązanie uczenia maszynowego.

W galerii znajdują się przykładowe eksperymenty zespołu Microsoft Azure Machine Learning, a także przykłady udostępnione przez społeczność usługi Machine Learning. Można również zadawać pytania i publikować komentarze dotyczące eksperymentów.

Aby poznać sposób korzystania z galerii, obejrzyj 3-minutowy klip wideo [Kopiowanie pracy innych osób w celu przetwarzania danych](machine-learning-data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) z serii [Przetwarzanie danych dla początkujących](machine-learning-data-science-for-beginners-the-5-questions-data-science-answers.md).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a>Znajdowanie eksperymentu do skopiowania w witrynie Cortana Intelligence Gallery
Aby zobaczyć, jakie eksperymenty są dostępne, przejdź do [galerii](https://gallery.cortanaintelligence.com/) i kliknij pozycję **Experiments** (Eksperymenty) w górnej części strony.

### <a name="find-the-newest-or-most-popular-experiments"></a>Znajdowanie najnowszych lub najpopularniejszych eksperymentów
Na tej stronie możesz zobaczyć eksperymenty z kategorii **Recently added** (Ostatnio dodane) lub przewinąć do kategorii **What's popular** (Popularne) oraz **Popular Microsoft experiments** (Popularne eksperymenty firmy Microsoft).

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Wyszukiwanie eksperymentu spełniającego określone wymagania
Aby przeglądać wszystkie eksperymenty:

1. Kliknij pozycję **Browse all** (Przeglądaj wszystkie) w górnej części strony.
2. W obszarze **Refine by** (Uściślij według) wybierz pozycję **Experiment** (Eksperyment), aby zobaczyć wszystkie eksperymenty w galerii.
3. Eksperymenty spełniające wymagania możesz znaleźć na kilka różnych sposobów:
   * **Zaznacz filtry po lewej stronie.** Na przykład aby przeglądać eksperymenty, w których jest używany algorytm wykrywania anomalii oparty na analizie PCA, wybierz pozycję **Experiment** (Eksperyment) w obszarze **Categories** (Kategorie), a następnie pozycję **PCA-Based Anomaly Detection** (Wykrywanie anomalii oparte na analizie PCA) w obszarze **Algorithms Used** (Używane algorytmy). (Jeśli nie widzisz tego algorytmu, kliknij pozycję **Show all** (Pokaż wszystkie) u dołu listy).<br></br>
     ![](./media/machine-learning-sample-experiments/refine-the-view.png)
   * **Użyj pola wyszukiwania.** Na przykład aby znaleźć eksperymenty zamieszczone przez firmę Microsoft, które są związane z rozpoznawaniem cyfr z użyciem algorytmu SVM dla problemu dwuklasowego, w polu wyszukiwania wprowadź ciąg „digit recognition” (rozpoznawanie cyfr). Następnie zaznacz filtry **Experiment** (Eksperyment), **Microsoft content only** (Tylko zawartość firmy Microsoft) i **Two-Class Support Vector Machine** (SVM dla problemu dwuklasowego): ![](./media/machine-learning-sample-experiments/search-for-experiments.png)
4. Kliknij eksperyment, aby uzyskać więcej informacji.
5. Aby uruchomić i/lub zmodyfikować eksperyment, kliknij pozycję **Open in Studio** (Otwórz w Studio) na stronie eksperymentu.

   > [!NOTE]
   > Aby otworzyć eksperyment w usłudze Machine Learning Studio, musisz zalogować się z użyciem poświadczeń konta Microsoft. Jeśli nie posiadasz jeszcze obszaru roboczego usługi Machine Learning, zostanie utworzony obszar roboczy w ramach bezpłatnej wersji próbnej. [Dowiedz się, co obejmuje bezpłatna wersja próbna usługi Machine Learning](https://azure.microsoft.com/pricing/details/machine-learning/)
   >
   >

    ![](./media/machine-learning-sample-experiments/example-experiment.png)

## <a name="use-a-template-in-machine-learning-studio"></a>Korzystanie z szablonu w usłudze Machine Learning Studio
Nowy eksperyment można również utworzyć w usłudze Machine Learning Studio, używając przykładu z galerii jako szablonu.

1. Zaloguj się przy użyciu poświadczeń konta Microsoft do usługi [Studio](https://studio.azureml.net), a następnie kliknij pozycję **New** (Nowy), aby utworzyć nowy eksperyment.
2. Przejrzyj przykłady i kliknij jeden z nich.

W obszarze roboczym zostanie utworzony nowy eksperyment z użyciem eksperymentu przykładowego jako szablonu.

## <a name="next-steps"></a>Następne kroki
* [Przygotowywanie danych](machine-learning-data-science-import-data.md)
* [Próba użycia języka R w eksperymencie](machine-learning-r-quickstart.md)
* [Przeglądanie przykładowych eksperymentów z użyciem języka R](machine-learning-r-csharp-web-service-examples.md)
* [Tworzenie interfejsu API usługi sieci Web](machine-learning-publish-a-machine-learning-web-service.md)



<!--HONumber=Nov16_HO3-->


