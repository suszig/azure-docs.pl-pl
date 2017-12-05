---
title: "Monitor opublikowane interfejsów API w usłudze Azure API Management | Dokumentacja firmy Microsoft"
description: "Wykonaj kroki tego samouczka, aby dowiedzieć się, jak monitorować interfejs API usługi Azure API Management."
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/19/2017
ms.author: apimpm
ms.openlocfilehash: bdca9d4968e9e68314f350787907f15e417821f7
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 12/04/2017
---
# <a name="monitor-published-apis"></a>Monitor opublikowane interfejsów API

Azure Monitor jest usługą platformy Azure, który udostępnia jedno źródło do monitorowania wszystkich zasobów na platformie Azure. Z monitorem Azure można zwizualizować, zapytania, trasy, archiwum i podejmuj działania na metryki i dzienników pochodzących z zasobów platformy Azure, takich jak zarządzanie interfejsami API. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wyświetlanie dzienników aktywności
> * Wyświetlanie dzienników diagnostycznych
> * Wyświetlaj metryki interfejsu API 
> * Skonfigurować regułę alertu, gdy interfejs API pobiera nieautoryzowane wywołania

Poniższe wideo pokazuje, jak monitorować zarządzanie interfejsami API Azure monitora. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Wymagania wstępne

+ Ukończenie następujących Szybki Start: [utworzenia wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Ponadto Ukończ samouczek następujących: [Import i opublikować swój pierwszy interfejs API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Wyświetl dzienniki aktywności

Dzienniki aktywności zapewniają wgląd w operacje wykonywane na usługi Zarządzanie interfejsami API. Korzystając z Dzienniki aktywności, można określić ", co, która i kiedy" dla żadnego zapisu (PUT, POST, DELETE) podejmowaną w odniesieniu do usług interfejsu API zarządzania. 

> [!NOTE]
> Dzienniki aktywności nie obejmują operacji odczytu (GET) lub operacje wykonywane w klasycznym portalu wydawcy lub przy użyciu oryginalnego interfejsów API zarządzania.

Dostęp do dzienników aktywności w usłudze API Management lub uzyskać dostępu do dzienników wszystkich zasobów platformy Azure w monitorze Azure. 

Aby wyświetlić dzienniki aktywności:

1. Z sieci **zarządzanie interfejsami API** wystąpienie, kliknij przycisk **dziennik aktywności**.

## <a name="view-diagnostic-logs"></a>Wyświetlanie dzienników diagnostycznych

Dzienniki diagnostyczne zawierają zaawansowane informacje o operacje i błędy, które są ważne w przypadku inspekcji, a także rozwiązywania problemów. Dzienniki diagnostyczne różnią się od Dzienniki aktywności. Dzienniki aktywności wgląd w operacje wykonywane na zasobów platformy Azure. Dzienniki diagnostyczne zapewniają wgląd w operacje, w zasobie wykonanie samego.

Dostęp do dzienników diagnostycznych:

1. Z sieci **zarządzanie interfejsami API** wystąpienie, kliknij przycisk **dziennik diagnostyczny**.

## <a name="view-metrics-of-your-apis"></a>Wyświetlaj metryki swoje interfejsy API

Zarządzanie interfejsami API emituje metryki co minutę, umożliwiając niemal w czasie rzeczywistym wgląd w stan i kondycję swoje interfejsy API. Poniżej znajduje się podsumowanie niektórych dostępne metryki:

* Wydajność (wersja zapoznawcza): ułatwia podjęcie decyzji dotyczących uaktualniania/zmiany na starszą wersję usługi APIM. Metryka jest emitowany na minutę i odzwierciedla pojemność bramy w trakcie raportowania. Metryka w zakresie od 0 do 100 i jest obliczane w zależności od zastosowanych środkach bramy, takie jak użycie procesora CPU i pamięci.
* Całkowita liczba żądań bramy: liczba interfejsu API żądań w okresie. 
* Liczba pomyślnych żądań bramy: liczba żądań interfejsu API, które otrzymały pomyślne kody odpowiedzi HTTP, w tym 304 307 i mniejszego niż 301 (na przykład 200). 
* Żądań zakończonych niepowodzeniem bramy: liczba żądań interfejsu API, które odebrano błędną kody odpowiedzi HTTP, w tym 400 i niczego większych niż 500.
* Nieautoryzowanych żądań bramy: liczba żądań interfejsu API, które otrzymały kody odpowiedzi HTTP 401, 403 i 429 w tym. 
* Inne żądania bramy: liczba żądań interfejsu API, które otrzymały kody odpowiedzi HTTP, które nie należą do żadnej z powyższych kategorii (na przykład 418).

Aby uzyskać dostęp do metryk:

1. Wybierz **metryki** z menu w dolnej części strony.
2. Z listy rozwijanej wybierz metryki myślisz (można dodać wiele metryk). 
    
    Na przykład wybierz **całkowita liczba żądań bramy** i **nieudanych żądań bramy** z listy dostępnych metryk.
3. Wykres przedstawia łączna liczba wywołań interfejsu API. Ponadto liczba wywołań interfejsu API, których nie powiodła się. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Ustaw regułę alertu dla nieautoryzowanego żądania

Można skonfigurować do odbierania alertów w oparciu metryki i działania dzienników. Azure Monitor umożliwia skonfigurowanie alert o konieczności wyzwala, wykonaj następujące czynności:

* Wyślij wiadomość e-mail z powiadomieniem
* Wywołanie elementu webhook
* Wywołanie aplikacji logiki platformy Azure

Aby skonfigurować alerty:

1. Wybierz **reguły alertów** na pasku menu u dołu strony.
2. Wybierz **Dodaj alert metryki**.
3. Wprowadź **nazwa** dla tego alertu.
4. Wybierz **nieautoryzowanych żądań bramy** jako metrykę do monitorowania.
5. Wybierz **E-mail właściciele, współautorzy i czytelnicy**.
6. Naciśnij przycisk **OK**.
7. Spróbuj interfejsach API konferencji bez klucza interfejsu API. Jako właściciel tej usługi Zarządzanie interfejsami API otrzymasz alert e-mail. 

    > [!TIP]
    > Reguły alertów można również wywołać haku sieci Web lub aplikacji logiki platformy Azure, gdy zostaje wyzwolony.

    ![set konta alertu](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wyświetlanie dzienników aktywności
> * Wyświetlanie dzienników diagnostycznych
> * Wyświetlaj metryki interfejsu API 
> * Skonfigurować regułę alertu, gdy interfejs API pobiera nieautoryzowane wywołania

Przejdź do następnego samouczek:

> [!div class="nextstepaction"]
> [Śledzenie wywołań](api-management-howto-api-inspector.md)