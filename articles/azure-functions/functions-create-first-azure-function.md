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
ms.date: 03/14/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: a797910e286cd2aacf5a8aa6c509e2b0f5f60276
ms.lasthandoff: 03/17/2017


---
# <a name="create-your-first-azure-function"></a>Tworzenie pierwszej funkcji platformy Azure

W tym temacie opisano sposób użycia opcji szybkiego startu usługi Azure Functions w portalu w celu utworzenia prostej funkcji „hello world” wywoływanej przez żądanie HTTP. Aby dowiedzieć się więcej o usłudze Azure Functions, zobacz [Azure Functions — omówienie](functions-overview.md).

Przed rozpoczęciem musisz mieć konto platformy Azure. Dostępne są [bezpłatne konta](https://azure.microsoft.com/free/). Możesz również [wypróbować usługę Azure Functions](https://azure.microsoft.com/try/app-service/functions/) bez konieczności rejestrowania się na platformie Azure.

## <a name="create-a-function-from-the-portal-quickstart"></a>Tworzenie funkcji przy użyciu opcji szybkiego startu portalu

1. Przejdź do [portalu Azure Functions](https://functions.azure.com/signin) i zaloguj się przy użyciu konta platformy Azure. 
 
2. Wpisz unikatową **nazwę** nowej aplikacji funkcji lub zaakceptuj nazwę wygenerowaną automatycznie, wybierz preferowany **region**, a następnie kliknij pozycję **Utwórz i rozpocznij**. Prawidłowa nazwa może zawierać tylko litery, cyfry i łączniki. Podkreślenie (**_**) nie jest dozwolonym znakiem.

3. Na karcie **Szybki start** kliknij pozycję **Element webhook i interfejs API** i wybierz język dla funkcji, a następnie kliknij pozycję **Utwórz funkcję**. Zostanie utworzona nowa wstępnie zdefiniowana funkcja w wybranym języku. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

4. (Opcjonalnie) Na tym etapie pracy z opcją szybkiego startu można przeprowadzić szybki przegląd funkcji usługi Azure Functions w portalu. Po ukończeniu lub pominięciu samouczka można przetestować nową funkcję, wysyłając żądanie HTTP.

## <a name="test-the-function"></a>Testowanie funkcji
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="watch-the-video"></a>Obejrzyj film
W poniższym wideo pokazano, jak wykonać podstawowe czynności opisane w tym samouczku. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 


## <a name="next-steps"></a>Następne kroki
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


