<properties
   pageTitle="Tworzenie elementu webhook lub interfejsu API usługi Azure Function | Microsoft Azure"
   description="Korzystanie z usługi Azure Functions w celu utworzenia funkcji uruchamianej przez element webhook lub wywołanie interfejsu API."
   services="azure-functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
   />

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="05/07/2016"
   ms.author="glenga"/>
   
# Tworzenie elementu webhook lub interfejsu API usługi Azure Function

Usługa Azure Functions to oparte na zdarzeniach środowisko umożliwiające przeprowadzanie obliczeń na żądanie, które pozwala tworzyć zaplanowane lub wyzwalane jednostki kodu implementowane w różnych językach programowania. Aby dowiedzieć się więcej o usłudze Azure Functions, zobacz [Azure Functions — omówienie](functions-overview.md).

W tym temacie opisano tworzenie nowej funkcji platformy Node.js wywoływanej przez element webhook GitHub. Nowa funkcja jest oparta na wstępnie zdefiniowanym szablonie dostępnym w portalu usług Azure Functions. Można również obejrzeć krótki film, aby sprawdzić, jak kroki te są wykonywane w portalu.

## Obejrzyj film

W poniższym filmie wideo pokazano, jak wykonać podstawowe czynności opisane w tym samouczku. 

[AZURE.VIDEO create-a-web-hook-or-api-azure-function]

##Tworzenie funkcji wyzwalanej przez element webhook przy użyciu szablonu

Aplikacja funkcji obsługuje wykonywanie funkcji na platformie Azure. Aby utworzyć funkcję, musisz mieć aktywne konto platformy Azure. Jeśli nie masz jeszcze konta platformy Azure, [dostępne są konta bezpłatne](https://azure.microsoft.com/free/). 

1. Przejdź do [portalu Azure Functions](https://functions.azure.com/signin) i zaloguj się przy użyciu konta platformy Azure.

2. Jeśli masz aplikację funkcji, której możesz użyć, wybierz ją w obszarze **Twoje aplikacje funkcji** i kliknij pozycję **Otwórz**. Aby utworzyć nową aplikację funkcji, wpisz unikatową **nazwę** nowej aplikacji funkcji lub zaakceptuj wygenerowaną nazwę, wybierz preferowany **region**, a następnie kliknij pozycję **Utwórz i rozpocznij**. 

3. W aplikacji funkcji kliknij kolejno pozycje **+ Nowa funkcja** > **Element webhook usługi GitHub — węzeł** > **Utwórz**. Na podstawie określonego szablonu zostanie utworzona funkcja o domyślnej nazwie. 

    ![Tworzenie nowej funkcji elementu webhook usługi GitHub](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 

4. Na karcie **Tworzenie** w oknie **Kod** jest wyświetlany kod przykładowej funkcji express.js. Funkcja ta odbiera żądanie GitHub od elementu webhook komentarza dotyczącego pewnego problemu, rejestruje opis problemu i wysyła odpowiedź w postaci `New GitHub comment: <Your issue comment text>` do elementu webhook.


    ![Tworzenie nowej funkcji elementu webhook usługi GitHub](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

5. Skopiuj wartości zawarte w polach **Adres URL funkcji** i **Klucz tajny usługi GitHub**. Będą one potrzebne podczas tworzenia elementu webhook w usłudze GitHub. 

6. Przewiń w dół do obszaru **Uruchamianie**, zwróć uwagę na wstępnie zdefiniowany kod JSON w treści żądania komentarza i kliknij pozycję **Uruchom**. 
 
    Na karcie **Tworzenie** możesz w dowolnym momencie przetestować nową funkcję opartą na szablonie, podając oczekiwane dane kodu JSON i klikając przycisk **Uruchom**. W tym przypadku szablon zawiera wstępnie zdefiniowaną treść komentarza dotyczącego problemu. 
 
W następnym kroku zostanie utworzony rzeczywisty element webhook w repozytorium GitHub.

##Konfigurowanie elementu webhook

1. W usłudze GitHub przejdź do swojego repozytorium (może to być dowolne rozgałęzione repozytorium).
 
2. Kliknij kolejno pozycje **Ustawienia** > **Elementy webhook i usługi** > **Dodaj element webhook**.

    ![Tworzenie nowej funkcji elementu webhook usługi GitHub](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   

3. Wklej adres URL i klucz tajny funkcji w polach **Adres URL ładunku** i **Klucz tajny**, kliknij pozycję **Pozwól mi wybrać pojedyncze zdarzenia**, wybierz pozycję **Komentarz dotyczący problemu** i kliknij pozycję **Dodaj element webhook**.

    ![Tworzenie nowej funkcji elementu webhook usługi GitHub](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

Element webhook usługi GitHub został skonfigurowany w celu wyzwolenia funkcji po dodaniu nowego komentarza dotyczącego problemu.  
Przetestujemy teraz działanie tej funkcji.

##Testowanie funkcji

1. W repozytorium GitHub otwórz kartę **Problemy** w nowym oknie przeglądarki, kliknij pozycję **Nowy problem**, wpisz tytuł, a następnie kliknij pozycję **Prześlij nowy problem**. Możesz również otworzyć istniejący problem.

2. W obszarze problemu wpisz komentarz i kliknij pozycję **Komentarz**. Wróć do nowego elementu webhook w usłudze GitHub i w obszarze **Ostatnie dostarczenia** sprawdź, czy wysłano żądanie elementu webhook i czy treść odpowiedzi zawiera tekst `New GitHub comment: <Your issue comment text>`.

3. W portalu usługi Functions przejdź do dzienników i upewnij się, że funkcja została wyzwolona, a wartość `New GitHub comment: <Your issue comment text>` została zapisana w dziennikach przesyłania strumieniowego.


##Następne kroki

Poniższe tematy umożliwiają uzyskanie dodatkowych informacji na temat usługi Azure Functions.

+ [Dokumentacja usługi Azure Functions dla deweloperów](functions-reference.md)  
Dokumentacja dotycząca kodowania funkcji dla programistów.
+ [Testowanie usługi Azure Functions](functions-test-a-function.md)  
Opis różnych narzędzi i technik testowania funkcji.
+ [Jak skalować usługę Azure Functions](functions-scale.md)  
Omówienie planów usług dostępnych w środowisku Azure Functions, w tym dynamicznego planu usług, oraz sposobu wybierania właściwego planu.  


[AZURE.INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]



<!--HONumber=sep16_HO1-->


