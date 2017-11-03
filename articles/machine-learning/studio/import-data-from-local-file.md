---
title: "Importowanie danych z pliku do usługi Azure Machine Learning Studio | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak można przekazać pliku danych szkoleniowych z dysku twardego do usługi Azure Machine Learning Studio. Spowoduje to utworzenie modułu zestawu danych w obszarze roboczym."
keywords: "Importowanie danych, formatu danych, typy danych, źródła danych, dane szkoleniowe"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: 67f66f9b8703f2cab93a2274a90c161a55848c34
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Importowanie danych szkoleniowych z pliku na dysku twardym w usłudze Machine Learning Studio
[!INCLUDE [import-data-into-aml-studio-selector](../../../includes/machine-learning-import-data-into-aml-studio.md)]

Dowiedz się, jak można przekazać plik danych z dysku twardego do użycia jako danych szkoleniowych w usłudze Azure Machine Learning Studio. Przez zaimportowanie pliku danych, masz modułu zestawu danych gotowe do użycia w obszarze roboczym.

## <a name="steps-to-import-data-from-a-local-file"></a>Kroki w celu importowania danych z pliku lokalnego
Importowanie danych z lokalnego dysku twardego, wykonaj następujące czynności:

1. Kliknij przycisk **+ nowy** u dołu okna Machine Learning Studio.
2. Wybierz **DATASET** i **z pliku lokalnego**.
3. W **przekazać nowy zestaw danych** okno dialogowe, przejdź do pliku, który chcesz przekazać
4. Wprowadź nazwę, określ typ danych i opcjonalnie wprowadź opis. Opis jest zalecane — umożliwia zarejestrowanie żadnych właściwości o danych, które chcesz Pamiętaj przy użyciu danych w przyszłości.
5. Pole wyboru **jest nowa wersja istniejący zestaw danych** pozwala zaktualizować istniejący zestaw danych o nowych danych. Kliknij to pole wyboru, a następnie wprowadź nazwę istniejącego zestawu danych.

![Przekaż nowy zestaw danych](./media/import-data-from-local-file/upload-dataset.png)

Podczas przekazywania zobaczysz komunikat jest przekazywanego pliku. Przekaż czas zależy od rozmiaru danych i prędkości połączenia z usługą. Jeśli wiesz, że plik zostanie zająć dużo czasu, można wykonać inne czynności w usłudze Machine Learning Studio, podczas oczekiwania. Powoduje zamknięcie przeglądarki jednak przekazywania danych zakończyć się niepowodzeniem.

## <a name="dataset-module-is-ready-for-use"></a>Moduł zestawu danych jest gotowy do użycia
Po przekazaniu plików dane są przechowywane w module zestawu danych i jest dostępny dla każdego doświadczenia w obszarze roboczym.

Podczas edycji eksperyment, można znaleźć zestawy danych zostały utworzone w **Moje zestawów danych** w obszarze **zapisane zestawów danych** listy na palecie modułów. Możesz przeciągać i upuszczać zestawu danych do obszaru roboczego eksperymentu, jeśli chcesz używać zestawu danych do dalszej analizy i uczenia maszynowego.
