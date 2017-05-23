---
title: Tworzenie funkcji uruchamianej zgodnie z harmonogramem na platformie Azure | Microsoft Docs
description: "Dowiedz się, jak utworzyć na platformie Azure funkcję uruchamianą zgodnie z określonym harmonogramem."
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 1478a2eedad496d3113fef28920d10859d11b1ce
ms.contentlocale: pl-pl
ms.lasthandoff: 05/12/2017


---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Tworzenie funkcji wyzwalanej czasomierzem na platformie Azure

Dowiedz się, jak za pomocą usługi Azure Functions utworzyć funkcję uruchamianą zgodnie z określonym harmonogramem.

![Tworzenie aplikacji funkcji w witrynie Azure Portal](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Wymagania wstępne

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Tworzenie aplikacji funkcji platformy Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Pomyślnie utworzona aplikacja funkcji.](./media/functions-create-first-azure-function/function-app-create-success.png)

Następnie należy utworzyć funkcję w nowej aplikacji funkcji.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Tworzenie funkcji wyzwalanej czasomierzem

1. Rozwiń swoją aplikację funkcji, kliknij przycisk **+** obok pozycji **Funkcje** i kliknij szablon **TimerTrigger** dla odpowiedniego języka. Następnie użyj ustawień określonych w tabeli i kliknij pozycję **Utwórz**:

| Ustawienie | Sugerowana wartość | Opis |
|---|---|---|
| **Nazwa funkcji** | TimerTriggerCSharp1 | Określa nazwę funkcji wyzwalanej czasomierzem. |
| **[Harmonogram](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | Składające się z 6 pól [wyrażenie CRON](http://en.wikipedia.org/wiki/Cron#CRON_expression) planujące uruchamianie funkcji co minutę. |

Zostanie utworzona funkcja w wybranym języku uruchamiana co minutę.

1. Zweryfikuj uruchomienie, wyświetlając informacje o śledzeniu zapisane w dziennikach.

![Podgląd dziennika usługi Functions w witrynie Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

Teraz możesz zmienić harmonogram funkcji tak, aby była uruchamiana rzadziej, na przykład co godzinę. 

## <a name="update-the-timer-schedule"></a>Aktualizowanie harmonogramu czasomierza

1. Rozwiń swoją funkcję i kliknij pozycję **Integracja**. W tym miejscu określa się powiązania danych wejściowych i wyjściowych dla funkcji oraz ustawia harmonogram. 

2. W polu **Harmonogram** wprowadź nową wartość `0 0 */1 * * *`, a następnie kliknij przycisk **Zapisz**.  

![Harmonogram aktualizowania czasomierza usługi Functions w witrynie Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

Funkcja będzie teraz uruchamiana raz na godzinę. 

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Następne kroki

Utworzono funkcję uruchamianą zgodnie z harmonogramem.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Aby uzyskać więcej informacji na temat wyzwalaczy czasomierza, zobacz [Planowanie wykonywania kodu za pomocą usługi Azure Functions](functions-bindings-timer.md).
