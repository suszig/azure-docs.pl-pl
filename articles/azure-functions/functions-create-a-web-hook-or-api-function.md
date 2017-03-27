---
title: "Tworzenie elementu webhook lub interfejsu API usługi Azure Function | Microsoft Docs"
description: "Użyj usługi Azure Functions, aby utworzyć bezserwerową funkcję wywoływaną za pomocą elementu webhook lub wywołania interfejsu API."
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
ms.date: 02/02/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: fd35f1774ffda3d3751a6fa4b6e17f2132274916
ms.openlocfilehash: 3e12b8c988b8971574352e976ad88e2e47f47660
ms.lasthandoff: 03/16/2017


---
# <a name="create-a-webhook-or-api-azure-function"></a>Tworzenie elementu webhook lub interfejsu API usługi Azure Function
Usługa Azure Functions to oparte na zdarzeniach środowisko umożliwiające przeprowadzanie obliczeń na żądanie, które pozwala tworzyć zaplanowane lub wyzwalane jednostki kodu implementowane w różnych językach programowania. Aby dowiedzieć się więcej o usłudze Azure Functions, zobacz [Azure Functions — omówienie](functions-overview.md).

W tym temacie opisano tworzenie funkcji języka JavaScript wywoływanej przez element webhook GitHub. Nowa funkcja jest oparta na wstępnie zdefiniowanym szablonie dostępnym w portalu usług Azure Functions. Można również obejrzeć krótki film, aby sprawdzić, jak kroki te są wykonywane w portalu.

Ogólne kroki z tego samouczka mogą też posłużyć do tworzenia funkcji w językach C# i F# zamiast JavaScript. 

## <a name="watch-the-video"></a>Obejrzyj film
W poniższym wideo pokazano, jak wykonać podstawowe czynności opisane w tym samouczku 

>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-a-Web-Hook-or-API-Azure-Function/player]
>
>

## <a name="prerequisites"></a>Wymagania wstępne

Do ukończenia tego samouczka niezbędne są następujące elementy:

+ Aktywne konto platformy Azure. Jeśli jeszcze nie masz konta platformy Azure, możesz [założyć je bezpłatnie](https://azure.microsoft.com/free/).  
 Możesz też skorzystać z możliwości [wypróbowania usługi Functions](https://functions.azure.com/try), aby ukończyć ten samouczek bez konta platformy Azure.
+ Konto usługi GitHub. Jeśli jeszcze nie masz konta usługi GitHub, [możesz je założyć bezpłatnie](https://github.com/join). 

## <a name="create-a-webhook-triggered-function-from-the-template"></a>Tworzenie funkcji wyzwalanej przez element webhook przy użyciu szablonu
Aplikacja funkcji obsługuje wykonywanie funkcji na platformie Azure. 

1. Przejdź do [portalu Azure Functions](https://functions.azure.com/signin) i zaloguj się przy użyciu konta platformy Azure.

2. Jeśli masz aplikację funkcji, której możesz użyć, wybierz ją w obszarze **Twoje aplikacje funkcji** i kliknij pozycję **Otwórz**. Aby utworzyć aplikację funkcji, wpisz unikatową **nazwę** nowej aplikacji funkcji lub zaakceptuj wygenerowaną nazwę, wybierz preferowany **region**, a następnie kliknij pozycję **Utwórz i rozpocznij**. 

3. W aplikacji funkcji kliknij kolejno pozycje **+ Nowa funkcja** > **Element webhook usługi GitHub — JavaScript** > **Utwórz**. W tym kroku na podstawie określonego szablonu zostanie utworzona funkcja o domyślnej nazwie. Zamiast niej możesz też utworzyć funkcję języka C# lub F#.
   
    ![Tworzenie funkcji wyzwalanej przez element webhook GitHub](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 

4. Na karcie **Tworzenie** w oknie **Kod** jest wyświetlany kod przykładowej funkcji express.js. Funkcja ta odbiera żądanie GitHub od elementu webhook komentarza dotyczącego pewnego problemu, rejestruje opis problemu i wysyła odpowiedź w postaci `New GitHub comment: <Your issue comment text>` do elementu webhook.

    ![Sprawdzanie kodu funkcji](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

1. Skopiuj i zapisz wartości zawarte w polach **Adres URL funkcji** i **Wpis tajny usługi GitHub**. Wartości te zostaną użyte w następnej sekcji do skonfigurowania elementu webhook w usłudze GitHub. 

2. Kliknij pozycję **Testowanie**, zwróć uwagę na wstępnie zdefiniowany kod JSON komentarza problemu w **treści żądania**, a następnie kliknij przycisk **Uruchom**. 

    ![Testowanie funkcji elementu webhook w portalu](./media/functions-create-a-web-hook-or-api-function/functions-test-webhook-in-portal.png)
   
    > [!NOTE]
    > Na karcie **Tworzenie** możesz w dowolnym momencie przetestować nową funkcję opartą na szablonie, podając oczekiwane dane kodu JSON i klikając przycisk **Uruchom**. W tym przypadku szablon zawiera wstępnie zdefiniowaną treść komentarza dotyczącego problemu. 

W następnym kroku zostanie utworzony rzeczywisty element webhook w repozytorium GitHub.

## <a name="configure-the-webhook"></a>Konfigurowanie elementu webhook
1. W usłudze GitHub przejdź do repozytorium, którego jesteś właścicielem. Możesz też użyć dowolnego rozwidlonego repozytorium.
 
2. Kliknij kolejno pozycje **Ustawienia** > **Elementy webhook i usługi** > **Dodaj element webhook**.
   
    ![Dodawanie elementu webhook GitHub](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   

3. Wklej adres URL funkcji i wpis tajny do pól **Adres URL ładunku** i **Wpis tajny**, a następnie wybierz wartość **application/json** dla ustawienia **Typ zawartości**.

4. Kliknij pozycję **Pozwól mi wybrać pojedyncze zdarzenia**, wybierz pozycję **Komentarz dotyczący problemu** i kliknij pozycję **Dodaj element webhook**.
   
    ![Ustawianie adresu URL i wpisu tajnego elementu webhook](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

Element webhook usługi GitHub został skonfigurowany w celu wyzwolenia funkcji po dodaniu nowego komentarza dotyczącego problemu.  
Przetestujemy teraz działanie tej funkcji.

## <a name="test-the-function"></a>Testowanie funkcji
1. W swoim repozytorium GitHub otwórz kartę **Problemy** w nowym oknie przeglądarki.

2. W nowym oknie kliknij pozycję **Nowy problem**, wpisz tytuł i kliknij pozycję **Prześlij nowy problem**. Możesz również otworzyć istniejący problem.

2. W obszarze problemu wpisz komentarz i kliknij pozycję **Komentarz**. 

3. W innym oknie usługi GitHub kliknij pozycję **Edytuj** obok nowego elementu webhook, przewiń ekran do pozycji **Ostatnie dostawy** i sprawdź, czy żądanie elementu webhook zostało wysłane i czy treścią odpowiedzi jest `New GitHub comment: <Your issue comment text>`.

3. W portalu usługi Functions przejdź do dzienników i upewnij się, że funkcja została wyzwolona, a wartość `New GitHub comment: <Your issue comment text>` została zapisana w dziennikach przesyłania strumieniowego.

## <a name="next-steps"></a>Następne kroki
Poniższe tematy umożliwiają uzyskanie dodatkowych informacji na temat usługi Azure Functions.

* [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)  
  Dokumentacja dotycząca kodowania funkcji dla programistów.
* [Testowanie usługi Azure Functions](functions-test-a-function.md)  
  Opis różnych narzędzi i technik testowania funkcji.
* [Jak skalować usługę Azure Functions](functions-scale.md)  
  Omówienie planów usług dostępnych w środowisku Azure Functions, w tym planu hostingowego zużycia, oraz sposobu wybierania właściwego planu.  

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


