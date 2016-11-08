---
title: Tworzenie funkcji przy użyciu witryny Azure Portal | Microsoft Docs
description: Utwórz swoją pierwszą funkcję platformy Azure, aplikację niekorzystającą z serwera, w niespełna dwie minuty.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: ''
tags: ''

ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/08/2016
ms.author: glenga

---
# Tworzenie funkcji przy użyciu witryny Azure Portal
## Omówienie
Azure Functions to oparte na zdarzeniach środowisko umożliwiające przeprowadzanie obliczeń na żądanie, które rozszerza istniejącą platformę aplikacji Azure o funkcje implementowania kodu wyzwalanego przez zdarzenia występujące w obrębie innych usług platformy Azure, produktów SaaS oraz systemów lokalnych. W środowisku Azure Functions aplikacje są skalowane na żądanie i płacisz tylko za użyte zasoby. Usługa Azure Functions umożliwia tworzenie zaplanowanych lub wyzwalanych jednostek kodu implementowanych w różnych językach programowania. Aby dowiedzieć się więcej o usłudze Azure Functions, zobacz [Azure Functions — omówienie](functions-overview.md).

W tym temacie opisano sposób użycia witryny Azure Portal w celu utworzenia prostej funkcji Azure Node.js „hello world” wywoływanej przez wyzwalacz protokołu HTTP. Przed utworzeniem funkcji w witrynie Azure Portal należy jawnie utworzyć aplikację funkcji w usłudze Azure App Service. Aby utworzyć automatycznie aplikację funkcji, zobacz [drugi samouczek szybkiego startu usługi Azure Functions](functions-create-first-azure-function.md), zawierający opis oraz film z opisem prostszej metody szybkiego startu.

## Tworzenie aplikacji funkcji
Aplikacja funkcji obsługuje wykonywanie funkcji na platformie Azure. Aby utworzyć aplikację funkcji w witrynie Azure Portal, wykonaj następujące kroki.

Przed utworzeniem pierwszej funkcji musisz mieć aktywne konto platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, [dostępne są konta bezpłatne](https://azure.microsoft.com/free/).

1. Przejdź do [witryny Azure Portal](https://portal.azure.com) i zaloguj się przy użyciu konta Azure.
2. Kliknij kolejno pozycje **+Nowy** > **Sieci Web i mobilność** > **Aplikacja funkcji**, wybierz **subskrypcję**, wpisz unikatową **nazwę aplikacji** określającą aplikację funkcji, a następnie wprowadź następujące ustawienia:
   
   * **[Grupa zasobów](../azure-portal/resource-group-portal.md)**: wybierz pozycję **Utwórz nową** i wprowadź nazwę nowej grupy zasobów. Można także wybrać istniejącą grupę zasobów, jednak może to utrudnić utworzenie dynamicznego planu usługi App Service dla bieżącej aplikacji funkcji.
   * **[Plan usługi App Service](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)**: wybierz opcję *Dynamiczny* lub *Klasyczny*. 
     * **Dynamiczny**: domyślny typ planu usługi Azure Functions. Po wybraniu planu dynamicznego należy także wybrać **lokalizację** oraz ustawić **alokację pamięci** (w MB). Aby uzyskać informacje na temat wpływu alokacji pamięci na koszty zobacz temat [Azure Functions — cennik](https://azure.microsoft.com/pricing/details/functions/). 
     * **Klasyczny**: klasyczny plan usługi App Service wymaga utworzenia **planu/lokalizacji usługi App Service** lub wybrania istniejących ustawień. Ustawienia te określają [lokalizację, funkcje, koszt i zasoby obliczeniowe](https://azure.microsoft.com/pricing/details/app-service/) skojarzone z aplikacją.  
   * **Konto magazynu**: każda aplikacja funkcji wymaga konta magazynu. Można wybrać istniejące konto magazynu lub utworzyć nowe. 
     
     ![Tworzenie nowej aplikacji funkcji w witrynie Azure Portal](./media/functions-create-first-azure-function-azure-portal/function-app-create-flow.png)
3. Kliknij pozycję **Utwórz**, aby zainicjować obsługę nowej aplikacji funkcji i wdrożyć ją.  

Po zainicjowaniu obsługi aplikacji funkcji można utworzyć pierwszą funkcję.

## Tworzenie funkcji
Wykonanie poniższych kroków umożliwia utworzenie funkcji w procedurze szybkiego startu usługi Azure Functions.

1. Na karcie **Szybki start** kliknij pozycje **Element webhook i interfejs API** oraz **JavaScript**, a następnie kliknij pozycję **Utwórz funkcję**. Zostanie utworzona nowa wstępnie zdefiniowana funkcja Node.js. 
   
    ![](./media/functions-create-first-azure-function-azure-portal/function-app-quickstart-node-webhook.png)
2. (Opcjonalnie) Na tym etapie pracy z opcją szybkiego startu można przeprowadzić szybki przegląd funkcji usługi Azure Functions w portalu.   Po ukończeniu lub pominięciu samouczka można przetestować nową funkcję przy użyciu wyzwalacza HTTP.

## Testowanie funkcji
Ponieważ elementy szybkiego startu usługi Azure Functions zawierają funkcjonalny kod, można natychmiast przetestować nową funkcję.

1. Na karcie **Programowanie** sprawdź zawartość okna **Kod** i zwróć uwagę, że ten kod Node.js oczekuje żądania HTTP z wartością *Nazwa* przekazywaną w treści komunikatu lub w ciągu zapytania. Po uruchomieniu funkcji ta wartość jest zwracana w komunikacie odpowiedzi.
   
    ![](./media/functions-create-first-azure-function-azure-portal/function-app-develop-tab-testing.png)
2. Przewiń w dół do pola tekstowego **Treść żądania**, zmień wartość właściwości *Nazwa* na swoją nazwę i kliknij pozycję **Uruchom**. Wykonanie jest wyzwalane przez testowe żądanie HTTP, informacje są zapisywane w dziennikach przesyłania strumieniowego, a odpowiedź „hello” jest wyświetlana w **danych wyjściowych**. 
3. Aby wyzwolić wykonywanie tej samej funkcji z innego okna lub karty przeglądarki, skopiuj wartość **Adres URL funkcji** z karty **Programowanie** i wklej go na pasku adresu przeglądarki, a następnie dołącz wartość ciągu zapytania `&name=yourname` i naciśnij klawisz Enter. W dziennikach są zapisywane te same informacje, a w przeglądarce tak jak poprzednio jest wyświetlana odpowiedź „hello”.

## Następne kroki
Ta opcja szybkiego startu przedstawia bardzo proste wykonywanie podstawowej funkcji wyzwalanej przez HTTP. Więcej informacji na temat używania możliwości usługi Azure Functions w aplikacjach można znaleźć w poniższych tematach.

* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)  
  Dokumentacja dla programistów dotycząca kodowania funkcji oraz definiowania wyzwalaczy i powiązań.
* [Testowanie usługi Azure Functions](functions-test-a-function.md)  
  Opis różnych narzędzi i technik testowania funkcji.
* [Jak skalować usługę Azure Functions](functions-scale.md)  
  Omówienie planów usług dostępnych w środowisku Azure Functions, w tym dynamicznego planu usług, oraz sposobu wybierania właściwego planu. 
* [Co to jest platforma Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
  Środowisko Azure Functions używa platformy Azure App Service na potrzeby funkcji podstawowych, takich jak wdrożenia, zmienne środowiskowe i diagnostyka. 

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]

<!--HONumber=sep12_HO2-->


