---
title: "Monitorowanie opublikowanych interfejsów API w usłudze Azure API Management | Microsoft Docs"
description: "Wykonaj kroki tego samouczka, aby dowiedzieć się, jak monitorować interfejs API w usłudze Azure API Management."
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
ms.openlocfilehash: 445723242a76dcef4a6b137439728235d5d6e32a
ms.sourcegitcommit: 059dae3d8a0e716adc95ad2296843a45745a415d
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/09/2018
---
# <a name="monitor-published-apis"></a>Monitorowanie opublikowanych interfejsów API

Azure Monitor to usługa platformy Azure, która zapewnia jedno źródło monitorowania zasobów platformy Azure. Dzięki usłudze Azure Monitor możesz wykonywać wizualizacje i zapytania, ustalać trasy, archiwizować oraz podejmować akcje dotyczące metryk i dzienników pochodzących z zasobów platformy Azure, takich jak usługa API Management. 

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wyświetlanie dzienników aktywności
> * Wyświetlanie dzienników diagnostycznych
> * Wyświetlanie metryk interfejsu API 
> * Konfigurowanie reguły alertu, gdy interfejs API odbiera nieautoryzowane wywołania

W poniższym filmie wideo pokazano, jak monitorować usługę API Management przy użyciu usługi Azure Monitor. 

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>

## <a name="prerequisites"></a>Wymagania wstępne

+ Wykonaj procedury przedstawione w następującym przewodniku Szybki start: [Tworzenie wystąpienia usługi Azure API Management](get-started-create-service-instance.md).
+ Ponadto wykonaj zadania z następującego samouczka: [Importowanie i publikowanie pierwszego interfejsu API](import-and-publish.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="diagnostic-logs"></a>Wyświetlanie dzienników aktywności

Dzienniki aktywności udostępniają szczegółowe dane operacji wykonywanych w stosunku do usług API Management. Korzystając z tych dzienników, można określić rodzaj, użytkownika i czas każdej operacji zapisu (PUT, POST, DELETE) wykonanej względem usług API Management. 

> [!NOTE]
> Dzienniki aktywności nie obejmują operacji odczytu (GET) ani operacji wykonywanych w witrynie Azure Portal albo przy użyciu oryginalnych interfejsów API zarządzania.

Dostęp do dzienników aktywności można uzyskać w usłudze API Management, a dostęp do wszystkich zasobów platformy Azure — w usłudze Azure Monitor. 

Aby wyświetlić dzienniki aktywności:

1. Wybierz wystąpienie usługi APIM.
2. Kliknij pozycję **Dziennik aktywności**.

## <a name="view-diagnostic-logs"></a>Wyświetlanie dzienników diagnostycznych

Dzienniki diagnostyczne zawierają bogate informacje o operacjach i błędach, które są ważne w przypadku inspekcji, a także pomagają rozwiązywać problemy. Dzienniki diagnostyczne różnią się od dzienników aktywności. Dzienniki aktywności udostępniają szczegółowe dane operacji wykonywanych w stosunku do zasobów platformy Azure. Dzienniki diagnostyczne udostępniają szczegółowe dane operacji wykonanych przez sam zasób.

Aby uzyskać do dzienników diagnostycznych:

1. Wybierz wystąpienie usługi APIM.
2. Kliknij pozycję **Dzienniki diagnostyczne**.

## <a name="view-metrics-of-your-apis"></a>Wyświetlanie metryk interfejsów API

Usługa API Management emituje metryki co minutę, oferując wgląd w stan i kondycję interfejsów API w czasie zbliżonym do rzeczywistego. Poniżej znajduje się podsumowanie niektórych dostępnych metryk:

* Wydajność (wersja zapoznawcza): ułatwia podejmowanie decyzji dotyczących uaktualniania usług APIM lub ich zmiany na starszą wersję. Metryka jest emitowana co minutę i odzwierciedla pojemność bramy w momencie raportowania. Wartość metryki należy do zakresu od 0 do 100 i jest obliczana w oparciu o zasoby bramy, takie jak wykorzystanie procesora i pamięci.
* Całkowita liczba żądań bramy: liczba żądań interfejsu API w danym okresie. 
* Żądania bramy zakończone powodzeniem: liczba żądań interfejsu API, które otrzymały kody odpowiedzi HTTP informujące o powodzeniu, w tym 304, 307 i mniejsze niż 301 (na przykład 200). 
* Żądania bramy zakończone niepowodzeniem: liczba żądań interfejsu API, które otrzymały kody odpowiedzi HTTP informujące o błędzie, w tym 400 i większe niż 500.
* Nieautoryzowane żądania bramy: liczba żądań interfejsu API, które otrzymały kody odpowiedzi HTTP, w tym 401, 403 i 429. 
* Inne żądania bramy: liczba żądań interfejsu API, które otrzymały kody odpowiedzi HTTP nienależące do żadnej z powyższych kategorii (na przykład 418).

Aby uzyskać dostęp do metryk:

1. Wybierz pozycję **Metryki** w menu w dolnej części strony.
2. Z listy rozwijanej wybierz interesujące Cię metryki (można dodać wiele metryk). 
    
    Na przykład wybierz pozycje **Całkowita liczba żądań bramy** i **Żądania bramy zakończone niepowodzeniem**  z listy dostępnych metryk.
3. Wykres przedstawia łączną liczbę wywołań interfejsu API. Pokazuje również liczbę wywołań interfejsu API zakończonych niepowodzeniem. 

## <a name="set-up-an-alert-rule-for-unauthorized-request"></a>Konfigurowanie reguły alertu na potrzeby nieautoryzowanego żądania

Można skonfigurować odbieranie alertów w oparciu o metryki i dzienniki aktywności. Usługa Azure Monitor umożliwia skonfigurowanie alertu powodującego wykonywanie następujących czynności po jego wyzwoleniu:

* Wysłanie powiadomienia e-mail
* Wywołanie elementu webhook
* Wywołanie aplikacji logiki platformy Azure

Aby skonfigurować alerty:

1. Wybierz pozycję **Reguły alertów** w menu w dolnej części strony.
2. Wybierz pozycję **Dodaj alert dotyczący metryki**.
3. Wprowadź **nazwę** tego alertu.
4. Wybierz pozycję **Nieautoryzowane żądania bramy** jako metrykę do monitorowania.
5. Wybierz pozycję **Właściciele, współautorzy i czytelnicy poczty e-mail**.
6. Naciśnij przycisk **OK**.
7. Spróbuj wywołać nasz interfejs API konferencji bez klucza API. Jako właściciel tej usługi API Management otrzymasz alert e-mail. 

    > [!TIP]
    > Wyzwolenie reguły alertu może również spowodować wywołanie elementu webhook lub aplikacji logiki platformy Azure.

    ![set-up-alert](./media/api-management-azure-monitor/set-up-alert.png)

## <a name="next-steps"></a>Następne kroki

W niniejszym samouczku zawarto informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Wyświetlanie dzienników aktywności
> * Wyświetlanie dzienników diagnostycznych
> * Wyświetlanie metryk interfejsu API 
> * Konfigurowanie reguły alertu, gdy interfejs API odbiera nieautoryzowane wywołania

Przejdź do następnego samouczka:

> [!div class="nextstepaction"]
> [Śledzenie wywołań](api-management-howto-api-inspector.md)
