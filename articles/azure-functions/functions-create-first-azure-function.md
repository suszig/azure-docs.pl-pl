---
title: Tworzenie pierwszej funkcji platformy Azure | Microsoft Docs
description: "Utwórz swoją pierwszą funkcję platformy Azure, aplikację niekorzystającą z serwera, w niespełna dwie minuty."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 4a1669e7-233e-4ea2-9b83-b8624f2dbe59
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 02/09/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 7743bfb98552f7fa2334d8daa6a6f6935969f393
ms.openlocfilehash: d76630e0be4a021720d88e6d7e64cf2258f84266


---
# <a name="create-your-first-azure-function"></a>Tworzenie pierwszej funkcji platformy Azure
## <a name="overview"></a>Omówienie
Azure Functions to oparte na zdarzeniach środowisko umożliwiające przeprowadzanie obliczeń na żądanie, które rozszerza istniejącą platformę aplikacji Azure o funkcje implementowania kodu wyzwalanego przez zdarzenia występujące w obrębie innych usług platformy Azure, produktów SaaS oraz systemów lokalnych. W środowisku Azure Functions aplikacje są skalowane na żądanie i płacisz tylko za użyte zasoby. Usługa Azure Functions umożliwia tworzenie zaplanowanych lub wyzwalanych jednostek kodu implementowanych w różnych językach programowania. Aby dowiedzieć się więcej o usłudze Azure Functions, zobacz [Azure Functions — omówienie](functions-overview.md).

W tym temacie opisano sposób użycia opcji szybkiego startu usługi Azure Functions w portalu w celu utworzenia prostej funkcji JavaScript „hello world” wywoływanej przez wyzwalacz protokołu HTTP. Można również obejrzeć krótki film, aby sprawdzić, jak kroki te są wykonywane w portalu.

## <a name="watch-the-video"></a>Obejrzyj film
W poniższym wideo pokazano, jak wykonać podstawowe czynności opisane w tym samouczku. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Tworzenie funkcji przy użyciu opcji szybkiego startu
Aplikacja funkcji obsługuje wykonywanie funkcji na platformie Azure. Wykonaj poniższe kroki, aby utworzyć aplikację funkcji z nową funkcją. Aplikacja funkcji jest tworzona z domyślną konfiguracją. Konkretny przykład tworzenia aplikacji funkcji można znaleźć w [innym samouczku szybkiego startu usługi Azure Functions](functions-create-first-azure-function-azure-portal.md).

Przed utworzeniem pierwszej funkcji musisz mieć aktywne konto platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, [dostępne są konta bezpłatne](https://azure.microsoft.com/free/).

1. Przejdź do [portalu Azure Functions](https://functions.azure.com/signin) i zaloguj się przy użyciu konta platformy Azure.
2. Wpisz unikatową **nazwę** nowej aplikacji funkcji lub zaakceptuj nazwę wygenerowaną automatycznie, wybierz preferowany **region**, a następnie kliknij pozycję **Utwórz i rozpocznij**. Pamiętaj, że musisz wprowadzić prawidłową nazwę zawierającą wyłącznie litery, cyfry i łączniki. Podkreślenie (**_**) nie jest dozwolonym znakiem.
3. Na karcie **Szybki start** kliknij pozycje **Element webhook i interfejs API** oraz **JavaScript**, a następnie kliknij pozycję **Utwórz funkcję**. Zostanie utworzona nowa wstępnie zdefiniowana funkcja JavaScript. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (Opcjonalnie) Na tym etapie pracy z opcją szybkiego startu można przeprowadzić szybki przegląd funkcji usługi Azure Functions w portalu. Po ukończeniu lub pominięciu samouczka można przetestować nową funkcję przy użyciu wyzwalacza HTTP.

## <a name="test-the-function"></a>Testowanie funkcji
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="next-steps"></a>Następne kroki
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Feb17_HO2-->


