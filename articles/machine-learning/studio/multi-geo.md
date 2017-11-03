---
title: Dokumentacji pomocy Multi-geograficznie | Dokumentacja firmy Microsoft
description: "Dowiedz się, jak utworzyć obszaru roboczego i opublikować usługi sieci web w regionie Azure różne od Południowe Stany Zjednoczone centralnej (SCUS) region platformy Azure."
services: machine-learning
documentationcenter: 
author: tedway
manager: jhubbard
editor: rmca14
tags: 
ms.assetid: ed0ca8a8-fa53-4e56-b824-2d7e44641967
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 4/6/2017
ms.author: tedway; neerajkh
ms.openlocfilehash: e4941ccf8c6d7a0c77527e9c1d722bc3a770114a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="multi-geo-help-documentation"></a>Dokumentacja pomocy dotyczącej wielu regionów geograficznych
W tym artykule opisano, jak można utworzyć obszaru roboczego i opublikować usługi sieci web w różnych regionach platformy Azure.  [Produkty Azure według regionu strony](https://azure.microsoft.com/en-us/regions/services/) zawiera listę regionów, gdzie dostępna jest usługa Azure Machine Learning.

## <a name="create-a-workspace"></a>Tworzenie obszaru roboczego
1. Zaloguj się do klasycznego portalu Azure.
2. Kliknij przycisk **+ nowy** > **usług danych** > **UCZENIA MASZYNOWEGO** > **szybkie tworzenie**.  W obszarze **lokalizacji** wybrać inny region, takich jak **Azja południowo-wschodnia**.
   ![Obraz pomocy Multi-geograficznie 1][1]
3. Wybierz obszar roboczy, a następnie kliknij przycisk **zalogować się do ML Studio**.
   ![Pomoc geograficznie wielu obraz 2][2]
4. Obszar roboczy jest teraz dostępna w innym regionie, w których można używać tak samo jak inne obszaru roboczego. Aby przełączyć między obszarów roboczych, poszukaj do prawego górnego rogu ekranu. Kliknij listę rozwijaną, wybierz region, a następnie wybierz obszar roboczy. Wszystko jest lokalny dla obszaru roboczego.  Na przykład wszystkich usług sieci web utworzony z obszaru roboczego będzie, w tym samym regionie, w obszarze roboczym w której znajduje się.
   ![Pomoc geograficznie wielu obrazu 3][3]

## <a name="open-an-experiment-from-gallery"></a>Otworzyć eksperyment z galerii
Po otwarciu doświadczenia z galerii, możesz wybrać regionu, którego chcesz skopiować do eksperymentu.

![Pomoc geograficznie wielu obrazu 4][4a]

## <a name="web-service-management"></a>Zarządzanie usługami sieci Web
Do programowego zarządzania usług sieci web, takich jak do ponownego trenowania, użyj adresu określonego regionu:

* https://asiasoutheast.Management.azureml.NET
* https://europewest.Management.azureml.NET

### <a name="things-to-note"></a>Rzeczy do uwzględnienia
1. Możesz skopiować tylko eksperymenty między obszarami roboczymi, które należą do tego samego regionu w ten sposób. Jeśli trzeba skopiować eksperymentu w obszary robocze w różnych regionach, można użyć [PowerShell](http://aka.ms/amlps) polecenie commandlet [ *AmlExperiment kopii* ](https://github.com/hning86/azuremlps/blob/master/README.md#copy-amlexperiment) znajdują się. Inne obejście jest opublikowanie eksperymentu w galerii w trybie nieznajdujące się na liście, otwórz go w obszarze roboczym z innego regionu.
2. Selektor region Pokaż tylko obszarów roboczych do jednego regionu w czasie.  
3. Obszar roboczy wolne lub obszar roboczy (anonimowe) dostępu dla gości zostanie utworzona i hostowanych w centralnej Południowe stany USA  
4. Usługi sieci Web wdrożone z obszaru roboczego w Azji Południowo-Wschodnia będą również obsługiwane w Azji Południowo-Wschodnia.  

## <a name="more-information"></a>Więcej informacji
Zadaj pytanie na [forum usługi Azure Machine Learning](https://social.msdn.microsoft.com/Forums/azure/home?forum=MachineLearning).

<!--Image references-->
[1]: ./media/multi-geo/multi-geo_1.png
[2]: ./media/multi-geo/multi-geo_2.png
[3]: ./media/multi-geo/multi-geo_3.png
[4a]: ./media/multi-geo/multi-geo_4a.png
