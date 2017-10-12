---
title: "Kopiowanie przykładowych eksperymentów uczenia maszynowego — Azure | Microsoft Docs"
description: "Dowiedz się, jak wykorzystać przykładowe eksperymenty uczenia maszynowego do tworzenia nowych eksperymentów za pomocą witryny Cortana Intelligence Gallery i usługi Microsoft Azure Machine Learning."
keywords: machine learning examples, sample experiment, machine learning sample
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
ms.date: 07/28/2017
ms.author: cgronlun
ms.openlocfilehash: f798ac1b46d702dbb96a2384d96f2d14eb3cac0e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="copy-example-experiments-to-create-new-machine-learning-experiments"></a>Kopiowanie przykładowych eksperymentów w celu tworzenia nowych eksperymentów uczenia maszynowego
Naucz się, jak zacząć od przykładowych eksperymentów z witryny [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/), zamiast tworzyć eksperymenty uczenia maszynowego od zera. Używając przykładów, możesz zbudować własne rozwiązanie uczenia maszynowego.

W galerii znajdują się przykładowe eksperymenty zespołu Microsoft Azure Machine Learning, a także przykłady udostępnione przez społeczność usługi Machine Learning. Można również zadawać pytania i publikować komentarze dotyczące eksperymentów.

Aby poznać sposób korzystania z galerii, obejrzyj 3-minutowy klip wideo [Kopiowanie pracy innych osób w celu przetwarzania danych](data-science-for-beginners-copy-other-peoples-work-to-do-data-science.md) z serii [Przetwarzanie danych dla początkujących](data-science-for-beginners-the-5-questions-data-science-answers.md).

[!INCLUDE [machine-learning-free-trial](../../../includes/machine-learning-free-trial.md)]

## <a name="find-an-experiment-to-copy-in-cortana-intelligence-gallery"></a>Znajdowanie eksperymentu do skopiowania w witrynie Cortana Intelligence Gallery
Aby zobaczyć, jakie eksperymenty są dostępne, przejdź do [galerii](https://gallery.cortanaintelligence.com/) i kliknij pozycję **Experiments** (Eksperymenty) w górnej części strony.

### <a name="find-the-newest-or-most-popular-experiments"></a>Znajdowanie najnowszych lub najpopularniejszych eksperymentów
Na tej stronie możesz zobaczyć eksperymenty z kategorii **Recently added** (Ostatnio dodane) lub przewinąć do kategorii **What's popular** (Popularne) oraz **Popular Microsoft experiments** (Popularne eksperymenty firmy Microsoft).

### <a name="look-for-an-experiment-that-meets-specific-requirements"></a>Wyszukiwanie eksperymentu spełniającego określone wymagania
Aby przeglądać wszystkie eksperymenty:

1. Kliknij pozycję **Browse all** (Przeglądaj wszystkie) w górnej części strony.
2. Po lewej stronie w obszarze **Refine by** (Uściślij według) w sekcji **Categories** (Kategorie) wybierz pozycję **Experiment** (Eksperyment), aby zobaczyć wszystkie eksperymenty w galerii.
3. Eksperymenty spełniające wymagania możesz znaleźć na kilka różnych sposobów:
   * **Zaznacz filtry po lewej stronie.** Na przykład aby przeglądać eksperymenty, w których jest używany algorytm wykrywania anomalii oparty na analizie PCA: przy wybranej pozycji **Experiment** (Eksperyment) w obszarze **Categories** (Kategorie) kliknij przycisk **Show all** (Pokaż wszystko). Następnie w obszarze **Algorithms Used** (Używane algorytmy) wybierz opcję **PCA-Based Anomaly Detection** (Wykrywanie anomalii oparte na PCA). <br></br>
     ![Wybierz filtry](./media/sample-experiments/refine-the-view.png)
   * **Użyj pola wyszukiwania.** Na przykład aby znaleźć eksperymenty zamieszczone przez firmę Microsoft, które są związane z rozpoznawaniem cyfr z użyciem algorytmu SVM dla problemu dwuklasowego, w polu wyszukiwania wprowadź ciąg „digit recognition” (rozpoznawanie cyfr). Następnie zaznacz filtry **Experiment** (Eksperyment), **Microsoft content only** (Tylko zawartość firmy Microsoft) i **Two-Class Support Vector Machine** (SVM dla problemu dwuklasowego):<br></br>
     ![Użyj pola wyszukiwania](./media/sample-experiments/search-for-experiments.png)
4. Kliknij eksperyment, aby uzyskać więcej informacji.
5. Aby uruchomić i/lub zmodyfikować eksperyment, kliknij pozycję **Open in Studio** (Otwórz w Studio) na stronie eksperymentu. <br></br>

    ![Przykładowy eksperyment](./media/sample-experiments/example-experiment.png)

    > [!NOTE]
    > Po otwarciu eksperymentu w usłudze Machine Learning Studio po raz pierwszy możesz wypróbować ją bezpłatnie lub kupić subskrypcję platformy Azure. [Dowiedz się więcej na temat usługi Machine Learning Studio w bezpłatnej wersji próbnej i w wersji płatnej](https://azure.microsoft.com/pricing/details/machine-learning/)
    >
    >

## <a name="create-a-new-experiment-using-an-example-as-a-template"></a>Tworzenie nowego eksperymentu przy użyciu przykładu jako szablonu
Nowy eksperyment można również utworzyć w usłudze Machine Learning Studio, używając przykładu z galerii jako szablonu.

1. Zaloguj się przy użyciu poświadczeń konta Microsoft do usługi [Studio](https://studio.azureml.net), a następnie kliknij pozycję **New** (Nowy), aby utworzyć eksperyment.
2. Przejrzyj przykłady i kliknij jeden z nich.

W obszarze roboczym programu Machine Learning Studio zostanie utworzony nowy eksperyment z użyciem eksperymentu przykładowego jako szablonu.

## <a name="next-steps"></a>Następne kroki
* [Importowanie danych z różnych źródeł](import-data.md)
* [Samouczek szybkiego startu dotyczący języka R w usłudze Machine Learning](r-quickstart.md)
* [Wdrażanie usługi sieci Web Machine Learning](publish-a-machine-learning-web-service.md)
