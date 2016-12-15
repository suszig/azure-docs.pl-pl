---
title: "Tworzenie elementu webhook lub interfejsu API usługi Azure Function | Microsoft Docs"
description: "Korzystanie z usługi Azure Functions w celu utworzenia funkcji uruchamianej przez element webhook lub wywołanie interfejsu API."
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
ms.date: 11/29/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 44e397c7521ba8f0ba11893c364f51177561bee4
ms.openlocfilehash: a74fc30480068788f33df092594119253df9487b


---
# <a name="create-a-webhook-or-api-azure-function"></a>Tworzenie elementu webhook lub interfejsu API usługi Azure Function
Usługa Azure Functions to oparte na zdarzeniach środowisko umożliwiające przeprowadzanie obliczeń na żądanie, które pozwala tworzyć zaplanowane lub wyzwalane jednostki kodu implementowane w różnych językach programowania. Aby dowiedzieć się więcej o usłudze Azure Functions, zobacz [Azure Functions — omówienie](functions-overview.md).

W tym temacie opisano tworzenie funkcji platformy Node.js wywoływanej przez element webhook GitHub. Nowa funkcja jest oparta na wstępnie zdefiniowanym szablonie dostępnym w portalu usług Azure Functions. Można również obejrzeć krótki film, aby sprawdzić, jak kroki te są wykonywane w portalu.

## <a name="watch-the-video"></a>Obejrzyj film
W poniższym wideo pokazano, jak wykonać podstawowe czynności opisane w tym samouczku 

>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-a-Web-Hook-or-API-Azure-Function/player]
>
>

## <a name="create-a-webhook-triggered-function-from-the-template"></a>Tworzenie funkcji wyzwalanej przez element webhook przy użyciu szablonu
Aplikacja funkcji obsługuje wykonywanie funkcji na platformie Azure. Jeśli nie masz jeszcze konta platformy Azure, zapoznaj się ze środowiskiem [Wypróbuj funkcje](https://functions.azure.com/try) lub [utwórz bezpłatne konto platformy Azure](https://azure.microsoft.com/free/). 

1. Przejdź do [portalu Azure Functions](https://functions.azure.com/signin) i zaloguj się przy użyciu konta platformy Azure.

2. Jeśli masz aplikację funkcji, której możesz użyć, wybierz ją w obszarze **Twoje aplikacje funkcji** i kliknij pozycję **Otwórz**. Aby utworzyć aplikację funkcji, wpisz unikatową **nazwę** nowej aplikacji funkcji lub zaakceptuj wygenerowaną nazwę, wybierz preferowany **region**, a następnie kliknij pozycję **Utwórz i rozpocznij**. 

3. W aplikacji funkcji kliknij kolejno pozycje **+ Nowa funkcja** > **Element webhook usługi GitHub — węzeł** > **Utwórz**. W tym kroku na podstawie określonego szablonu zostanie utworzona funkcja o domyślnej nazwie. 
   
    ![Tworzenie funkcji wyzwalanej przez element webhook GitHub](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 

4. Na karcie **Tworzenie** w oknie **Kod** jest wyświetlany kod przykładowej funkcji express.js. Funkcja ta odbiera żądanie GitHub od elementu webhook komentarza dotyczącego pewnego problemu, rejestruje opis problemu i wysyła odpowiedź w postaci `New GitHub comment: <Your issue comment text>` do elementu webhook.

    ![Sprawdzanie kodu funkcji](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

1. Skopiuj wartości zawarte w polach **Adres URL funkcji** i **Klucz tajny usługi GitHub**. Wartości będą potrzebne podczas tworzenia elementu webhook w usłudze GitHub. 

2. Kliknij pozycję **Testowanie**, zwróć uwagę na wstępnie zdefiniowany kod JSON komentarza problemu w **treści żądania**, a następnie kliknij przycisk **Uruchom**. 

    ![Testowanie funkcji elementu webhook w portalu](./media/functions-create-a-web-hook-or-api-function/functions-test-webhook-in-portal.png)
   
    > [!NOTE]
    > Na karcie **Tworzenie** możesz w dowolnym momencie przetestować nową funkcję opartą na szablonie, podając oczekiwane dane kodu JSON i klikając przycisk **Uruchom**. W tym przypadku szablon zawiera wstępnie zdefiniowaną treść komentarza dotyczącego problemu. 

W następnym kroku zostanie utworzony rzeczywisty element webhook w repozytorium GitHub.

## <a name="configure-the-webhook"></a>Konfigurowanie elementu webhook
1. W usłudze GitHub przejdź do repozytorium, którego jesteś właścicielem. Możesz też użyć dowolnego rozwidlonego repozytorium.
 
2. Kliknij kolejno pozycje **Ustawienia** > **Elementy webhook i usługi** > **Dodaj element webhook**.
   
    ![Dodawanie elementu webhook GitHub](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   
3. Wklej adres URL i klucz tajny funkcji w polach **Adres URL ładunku** i **Klucz tajny**, kliknij pozycję **Pozwól mi wybrać pojedyncze zdarzenia**, wybierz pozycję **Komentarz dotyczący problemu** i kliknij pozycję **Dodaj element webhook**.
   
    ![Ustawianie adresu URL i wpisu tajnego elementu webhook](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

Element webhook usługi GitHub został skonfigurowany w celu wyzwolenia funkcji po dodaniu nowego komentarza dotyczącego problemu.  
Przetestujemy teraz działanie tej funkcji.

## <a name="test-the-function"></a>Testowanie funkcji
1. W repozytorium GitHub otwórz kartę **Problemy** w nowym oknie przeglądarki, kliknij pozycję **Nowy problem**, wpisz tytuł, a następnie kliknij pozycję **Prześlij nowy problem**. Możesz również otworzyć istniejący problem.

2. W obszarze problemu wpisz komentarz i kliknij pozycję **Komentarz**. Wróć do nowego elementu webhook w usłudze GitHub i w obszarze **Ostatnie dostarczenia** sprawdź, czy wysłano żądanie elementu webhook i czy treść odpowiedzi zawiera tekst `New GitHub comment: <Your issue comment text>`.

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




<!--HONumber=Dec16_HO1-->


