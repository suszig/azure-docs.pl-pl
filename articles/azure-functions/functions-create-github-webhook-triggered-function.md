---
title: Tworzenie funkcji na platformie Azure wyzwalanej przez element webhook GitHub | Microsoft Docs
description: "Użyj usługi Azure Functions, aby utworzyć funkcję niewymagającą użycia serwera wywoływaną za pomocą elementu webhook GitHub."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/18/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: d4354546f3342d65353a86a4cec7d02547ab92e7
ms.lasthandoff: 04/22/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Tworzenie funkcji wyzwalanej przez element webhook GitHub

Dowiedz się, jak utworzyć funkcję, która jest wyzwalana przez element webhook GitHub. 

![Tworzenie aplikacji funkcji w witrynie Azure Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

Do wykonania czynności przedstawionych w tym temacie są wymagane zasoby utworzone w temacie [Tworzenie pierwszej funkcji w witrynie Azure Portal](functions-create-first-azure-function.md).

Potrzebne jest także konto usługi GitHub. Jeśli jeszcze nie masz konta usługi GitHub, [możesz je założyć bezpłatnie](https://github.com/join). 

Wykonanie wszystkich czynności opisanych w tym temacie powinno zająć mniej niż pięć minut.

## <a name="find-your-function-app"></a>Znajdowanie aplikacji funkcji    

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com/). 

2. Na pasku wyszukiwania w górnej części portalu wpisz nazwę aplikacji funkcji i wybierz ją z listy.

## <a name="create-function"></a>Tworzenie funkcji wyzwalanej przez element webhook GitHub

1. W aplikacji funkcji kliknij przycisk **+** obok pozycji **Funkcje**, kliknij szablon **GitHubWebHook** dla odpowiedniego języka, a następnie kliknij przycisk **Utwórz**.
   
    ![Tworzenie funkcji wyzwalanej przez element webhook GitHub w witrynie Azure portal](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

2. Kliknij pozycję **</> Pobierz adres URL funkcji**, po czym skopiuj i zapisz wartości. Powtórz te czynności po kliknięciu pozycji **</> Pobierz wpis tajny usługi GitHub**. Wartości te będą potrzebne podczas konfigurowania elementu webhook w usłudze GitHub. 

    ![Sprawdzanie kodu funkcji](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png) 
         
W następnym kroku zostanie utworzony element webhook w repozytorium GitHub. 

## <a name="configure-the-webhook"></a>Konfigurowanie elementu webhook
1. W usłudze GitHub przejdź do repozytorium, którego jesteś właścicielem. Możesz też użyć dowolnego rozwidlonego repozytorium.
 
2. Kliknij kolejno pozycje **Ustawienia**, **Elementy webhook** i **Dodaj element webhook**.
   
    ![Dodawanie elementu webhook GitHub](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

3. Wklej adres URL funkcji i wpis tajny do pól **Adres URL ładunku** i **Wpis tajny**, a następnie wybierz wartość **application/json** dla ustawienia **Typ zawartości**.

4. Kliknij pozycję **Pozwól mi wybrać pojedyncze zdarzenia**, wybierz pozycję **Komentarz dotyczący problemu** i kliknij pozycję **Dodaj element webhook**.
   
    ![Ustawianie adresu URL i wpisu tajnego elementu webhook](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

Element webhook został skonfigurowany do wyzwolenia funkcji po dodaniu nowego komentarza dotyczącego problemu. 

## <a name="test-the-function"></a>Testowanie funkcji
1. W swoim repozytorium GitHub otwórz kartę **Problemy** w nowym oknie przeglądarki.

2. W nowym oknie kliknij pozycję **Nowy problem**, wpisz tytuł i kliknij pozycję **Prześlij nowy problem**. 

2. W obszarze problemu wpisz komentarz i kliknij pozycję **Komentarz**. 

3. W innym oknie usługi GitHub kliknij pozycję **Edytuj** obok nowego elementu webhook, przewiń ekran do pozycji **Ostatnie dostawy** i sprawdź, czy żądanie elementu webhook zostało przetworzone przez funkcję. 
 
    ![Ustawianie adresu URL i wpisu tajnego elementu webhook](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-triggered.png)

   Odpowiedź funkcji powinna zawierać tekst `New GitHub comment: <Your issue comment text>`.

## <a name="next-steps"></a>Następne kroki

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


