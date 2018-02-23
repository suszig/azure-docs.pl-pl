---
title: "Wdrażanie usług Azure Analysis Services przy użyciu programu SSDT | Microsoft Docs"
description: "Dowiedz się, jak wdrożyć model tabelaryczny na serwerze usług Azure Analysis Services przy użyciu programu SSDT."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/14/2018
ms.author: owend
ms.openlocfilehash: f356b5d8e2b047add41873ab2676bd46db8d1fd2
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="deploy-a-model-from-ssdt"></a>Wdrażanie modelu w programie SSDT
Po utworzeniu serwera w ramach subskrypcji platformy Azure wszystko jest gotowe do wdrożenia bazy danych modelu tabelarycznego. Program SSDT (SQL Server Data Tools) służy do tworzenia i wdrażania projektu modelu tabelarycznego, nad którymi pracujesz. 

## <a name="prerequisites"></a>Wymagania wstępne
Aby rozpocząć pracę, potrzebne będą następujące elementy:

* **Serwer usług Analysis Services** na platformie Azure. Aby dowiedzieć się więcej, zobacz artykuł [Create an Azure Analysis Services server](analysis-services-create-server.md) (Tworzenie serwera usług Azure Analysis Services).
* **Projekt modelu tabelarycznego** w programie SSDT lub istniejący model tabelaryczny na poziomie zgodności 1200 lub wyższym. Nie wiesz, jak go utworzyć? Spróbuj skorzystać z [samouczka sprzedaży modelowania tabelarycznego projektu Adventure Works Internet Sales](https://msdn.microsoft.com/library/hh231691.aspx).
* **Brama lokalna** — jeśli co najmniej jedno źródło danych znajduje się w sieci organizacji, należy zainstalować [bramę danych lokalnych](analysis-services-gateway.md). Brama jest niezbędna, aby umożliwić serwerowi w chmurze łączenie się z lokalnymi źródłami danych w celu przetwarzania i odświeżania danych w modelu.

> [!TIP]
> Przed jej wdrożeniem upewnij się, że można przetwarzać dane w tabelach. W programie SSDT kliknij opcje **Model** > **Przetwarzaj** > **Przetwarzaj wszystko**. Jeśli przetwarzanie zakończy się niepowodzeniem, wdrożenie nie jest możliwe.
> 
> 

## <a name="to-deploy-a-tabular-model-from-ssdt"></a>Aby wdrożyć model tabelaryczny w programie SSDT

1. Przed wdrożeniem należy uzyskać nazwę serwera. Skopiuj nazwę serwera z **portalu Azure** > serwer > **Omówienie** > **Nazwa serwera**.
   
    ![Pobieranie nazwy serwera z systemu Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. W programie SSDT > **Eksplorator rozwiązań** kliknij prawym przyciskiem myszy projekt > **Właściwości**. Następnie w obszarze **Wdrożenie** > **Serwer** wklej nazwę serwera.   
   
    ![Wklejanie nazwy serwera we właściwościach serwera wdrażania](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy **Właściwości**, a następnie kliknij przycisk **Wdróż**. Może zostać wyświetlony monit o zalogowanie się do platformy Azure.
   
    ![Wdrażanie na serwerze](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Stan wdrożenia pojawia się w oknie danych wyjściowych i oknie wdrażania.
   
    ![Stan wdrożenia](./media/analysis-services-deploy/aas-deploy-status.png)

To wszystko!


## <a name="troubleshooting"></a>Rozwiązywanie problemów
Jeśli wdrożenie zakończy się niepowodzeniem podczas wdrażania metadanych, prawdopodobną przyczyną jest brak połączenia programu SSDT z serwerem. Upewnij się, że możesz połączyć się z serwerem przy użyciu programu SSMS. Upewnij się, że właściwość serwera wdrażania dla projektu jest poprawna.

Jeśli wdrożenie zakończy się niepowodzeniem dla tabeli, prawdopodobnie serwer nie mógł nawiązać połączenia ze źródłem danych. Jeśli źródło danych znajduje się w lokalnej sieci organizacji, należy zainstalować [bramę danych lokalnych](analysis-services-gateway.md).

## <a name="next-steps"></a>Następne kroki
Po wdrożeniu modelu tabelarycznego na serwerze możesz się z nim połączyć. W celu zarządzania modelem możesz [połączyć się przy użyciu programu SSMS](analysis-services-manage.md). Możesz również [nawiązać połączenie za pomocą narzędzia klienta](analysis-services-connect.md), takiego jak usługi Power BI, Power BI Desktop lub program Excel, i rozpocząć tworzenie raportów.

