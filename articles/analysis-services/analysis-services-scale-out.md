---
title: "Skalowalne usług Azure Analysis Services | Dokumentacja firmy Microsoft"
description: "Replikacja serwerów usług Azure Analysis Services z skalowalnego w poziomie"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/06/2017
ms.author: owend
ms.openlocfilehash: 0e58862684e62a65cf11266cc0320a9acd781f07
ms.sourcegitcommit: 295ec94e3332d3e0a8704c1b848913672f7467c8
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/06/2017
---
# <a name="azure-analysis-services-scale-out"></a>Skalowalne usług Azure Analysis Services

Z skalowalnego w poziomie, zapytań klienta mogą być dystrybuowane między wieloma *zapytania replik* w puli zapytania, zmniejszyć czasy odpowiedzi w ciągu zapytania wysokiej obciążeń. Można też oddzielić przetwarzania z puli zapytania, zapewnienie, że zapytań klienta nie wpływały niekorzystnie na przetworzenie operacji. Skalowalny w poziomie można skonfigurować w portalu Azure lub za pomocą interfejsu API REST usług analizy.

## <a name="how-it-works"></a>Jak to działa

We wdrożeniu serwera typowe jeden serwer służy jako serwer przetwarzania i serwer kwerend. Jeśli liczba zapytań klienta dotyczących modeli na serwerze przekracza zapytania przetwarzania jednostki (QPU) dla serwera planu, lub modelu przetwarzanie odbywa się w tym samym czasie jako wysoki zapytania obciążeń, może obniżyć wydajność. 

Z skalowalnego w poziomie można utworzyć pulę zapytania z replikami dodatkowe zapytania do siedmiu (łącznie osiem, łącznie z serwera). Liczba replik zapytania do spełnienia wymagań QPU w czasie krytyczne można skalować i w dowolnym momencie można oddzielić serwer przetwarzania z puli zapytania. 

Niezależnie od liczby replik zapytania, znajdującym się w puli zapytania obciążenia przetwarzaniem danych nie są dystrybuowane między replikami zapytania. Pojedynczy serwer służy jako serwer przetwarzania. Zapytanie replik służyć tylko kwerend dotyczących modeli synchronizowane między każdej replice w puli zapytania. 

Po zakończeniu operacji przetwarzania odbywa się synchronizacja między serwerem przetwarzania i serwery repliki zapytania. Automatyzacja operacji przetwarzania, należy skonfigurować synchronizację, po pomyślnym ukończeniu operacji przetwarzania.

> [!NOTE]
> Skalowalny w poziomie jest dostępna dla serwerów w standardowej warstwie cenowej. Jest on rozliczany każdej repliki zapytania w tym samym poziomie jak serwer.

> [!NOTE]
> Skalowalny w poziomie nie zwiększyć ilość dostępnej pamięci serwera. Aby zwiększyć ilość pamięci, należy uaktualnić swój plan.

## <a name="monitor-qpu-usage"></a>Monitorowanie użycia QPU

 Aby ustalić, czy skalowalnego w poziomie serwera jest niezbędne, serwer w portalu Azure należy monitorować za pomocą metryki. Jeśli Twoje QPU regularnie maxes wychodzących, oznacza to, że liczba zapytań dotyczących modeli przekracza limit QPU dla planu. Metryka długość kolejki zadania puli zapytania zwiększa także liczby zapytań w kolejki puli wątków zapytania przekracza dostępne QPU. Aby dowiedzieć się więcej, zobacz [monitorować metryki serwera](analysis-services-monitor.md).

## <a name="configure-scale-out"></a>Skonfiguruj skalowalny w poziomie

### <a name="in-azure-portal"></a>W portalu Azure

1. W portalu kliknij **skalowalnego w poziomie**. Wybierz liczbę serwerów repliki zapytania za pomocą suwaka. Liczba replik wybrany jest oprócz istniejącego serwera.

2. W **oddzielić serwer przetwarzania z puli podczas badania**, wybierz opcję Tak, aby wykluczyć z serwerów zapytania serwera przetwarzania.

   ![Suwak skalowalnego w poziomie](media/analysis-services-scale-out/aas-scale-out-slider.png)

3. Kliknij przycisk **zapisać** do obsługi administracyjnej nowych serwerów repliki zapytania. 

Modele tabelaryczne na podstawowym serwerze są synchronizowane z serwerów repliki. Po ukończeniu synchronizacji rozpocznie się puli zapytania dystrybucja zapytania przychodzące między serwery replik. 

### <a name="powershell"></a>PowerShell
Użyj [AzureRmAnalysisServicesServer zestaw](/powershell/module/azurerm.analysisservices/set-azurermanalysisservicesserver) polecenia cmdlet. Określ `-Capacity` wartość parametru > 1.

## <a name="synchronization"></a>Synchronizacja 

Podczas obsługi administracyjnej nowych replik kwerendy usług Azure Analysis Services automatycznie replikuje modeli we wszystkich replik. Można także przeprowadzić ręczną synchronizację. Podczas przetwarzania modeli, należy wykonać synchronizację, więc aktualizacje są synchronizowane między replikami zapytania.

### <a name="in-azure-portal"></a>W portalu Azure

W **omówienie** > modelu > **modelu Synchronizuj**.

![Suwak skalowalnego w poziomie](media/analysis-services-scale-out/aas-scale-out-sync.png)

### <a name="rest-api"></a>Interfejs API REST

Synchronizacji modelu   
`POST https://<region>.asazure.windows.net/servers/<servername>/models/<modelname>/sync`

Pobierz stan synchronizacji modelu  
`GET https://<region>.asazure.windows.net/servers/<servername>/models/<modelname>/sync`

## <a name="connections"></a>Połączenia

Na stronie Przegląd serwera istnieją dwie nazwy serwera. Jeśli jeszcze nie skonfigurowano skalowalnego w poziomie serwera, obie nazwy serwera działa w ten sam. Po skonfigurowaniu skalowalnego w poziomie serwera, należy określić nazwę serwera odpowiedniej w zależności od typu połączenia. 

Dla połączeń klienckich przez użytkownika końcowego, takie jak Power BI Desktop, Excel i aplikacji niestandardowych, użyj **nazwy serwera**. 

SSMS, narzędzi SSDT i parametry połączenia w programie PowerShell, użyj funkcji platformy Azure, aplikacji i obiektach AMO, **nazwy serwera zarządzania**. Nazwa serwera zarządzania obejmuje specjalnego `:rw` kwalifikator (odczytu i zapisu). Wszystkie operacje przetwarzania odbywa się na serwerze zarządzania.

![Nazwy serwerów](media/analysis-services-scale-out/aas-scale-out-name.png)

## <a name="related-information"></a>Informacje pokrewne

[Metryki serwera monitora](analysis-services-monitor.md)   
[Zarządzanie usług Azure Analysis Services](analysis-services-manage.md) 

