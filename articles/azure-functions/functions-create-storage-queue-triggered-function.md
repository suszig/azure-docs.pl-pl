---
title: Tworzenie funkcji na platformie Azure wyzwalanej przez komunikaty kolejki | Microsoft Docs
description: "Utwórz za pomocą usługi Azure Functions funkcję niewymagającą użycia serwera wywoływaną za pomocą komunikatów przesyłanych do kolejki usługi Azure Storage."
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 361da2a4-15d1-4903-bdc4-cc4b27fc3ff4
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: e6cf8797d08609f847e33f88e78fbcd3f3743a08
ms.contentlocale: pl-pl
ms.lasthandoff: 05/12/2017


---
# <a name="create-a-function-triggered-by-azure-queue-storage"></a>Tworzenie funkcji wyzwalanej przez usługę Azure Queue Storage

Dowiedz się, jak utworzyć funkcję wyzwalaną w momencie przesłania komunikatów do kolejki usługi Azure Storage.

![Wyświetlanie komunikatu w dziennikach.](./media/functions-create-storage-queue-triggered-function/function-app-in-portal-editor.png)

Wykonanie wszystkich czynności opisanych w tym temacie powinno zająć mniej niż pięć minut.

## <a name="prerequisites"></a>Wymagania wstępne

Przed uruchomieniem tego przykładu należy wykonać następujące czynności:

- Pobrać i zainstalować program [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Tworzenie aplikacji funkcji platformy Azure

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![Pomyślnie utworzona aplikacja funkcji.](./media/functions-create-first-azure-function/function-app-create-success.png)

Następnie należy utworzyć funkcję w nowej aplikacji funkcji.

<a name="create-function"></a>

## <a name="create-a-queue-triggered-function"></a>Tworzenie funkcji wyzwalanej przez kolejkę

Rozwiń swoją aplikację funkcji, kliknij przycisk **+** obok pozycji **Funkcje** i kliknij szablon **QueueTrigger** dla odpowiedniego języka. Następnie użyj ustawień określonych w tabeli i kliknij pozycję **Utwórz**.

![Tworzenie funkcji wyzwalanej przez kolejkę magazynu.](./media/functions-create-storage-queue-triggered-function/functions-create-queue-storage-trigger-portal.png)

| Ustawienie | Sugerowana wartość | Opis |
|---|---|---|
| **Nazwa kolejki**   | myqueue-items    | Nazwa kolejki, z którą zostanie nawiązane połączenie na koncie magazynu. |
| **Połączenie konta magazynu** | AzureWebJobStorage | Możesz skorzystać z połączenia konta magazynu już używanego przez aplikację funkcji lub utworzyć nowe.  |
| **Nazwa funkcji** | Unikatowa w obrębie aplikacji funkcji | Nazwa funkcji wyzwalanej przez kolejkę. |

Następnie nawiąż połączenie z kontem usługi Azure Storage i utwórz kolejkę magazynu **myqueue-items**.

## <a name="create-the-queue"></a>Tworzenie kolejki

1. W funkcji kliknij pozycję **Integracja**, rozwiń pozycję **Dokumentacja** i skopiuj wartości pól **Nazwa konta** oraz **Klucz konta**. Te poświadczenia służą do nawiązywania połączenia z kontem magazynu. Jeśli już nawiązano połączenie z kontem magazynu, przejdź do kroku 4.

    ![Uzyskiwanie poświadczeń połączenia konta magazynu.](./media/functions-create-storage-queue-triggered-function/functions-storage-account-connection.png)v

1. Uruchom narzędzie [Microsoft Azure Storage Explorer](http://storageexplorer.com/), kliknij ikonę połączenia po lewej stronie, wybierz pozycję **Użyj klucza i nazwy konta magazynu** i kliknij przycisk **Dalej**.

    ![Uruchamianie narzędzia Storage Account Explorer.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-1.png)

1. Wprowadź wartości **Nazwa konta** i **Klucz konta** z kroku 1, kliknij przycisk **Dalej**, a następnie przycisk **Połącz**.

    ![Wprowadzanie poświadczeń magazynu i nawiązywanie połączenia.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-connect-2.png)

1. Rozwiń dołączone konto magazynu, kliknij prawym przyciskiem myszy pozycję **Queues** (Kolejki), kliknij polecenie **Create queue** (Utwórz kolejkę), wpisz nazwę `myqueue-items`, a następnie naciśnij klawisz Enter.

    ![Tworzenie kolejki magazynu.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-create-queue.png)

Teraz, gdy masz już kolejkę magazynu, możesz przetestować funkcję, dodając komunikat do kolejki.

## <a name="test-the-function"></a>Testowanie funkcji

1. Wróć do witryny Azure Portal, przejdź do swoich funkcji, rozwiń pozycję **Dzienniki** w dolnej części strony i upewnij się, że strumieniowe przesyłanie dzienników nie jest wstrzymane.

1. W programie Storage Explorer rozwiń swoje konto magazynu, wybierz kolejno pozycje **Queues** (Kolejki) i **myqueue-items**, a następnie kliknij pozycję **Add message** (Dodaj komunikat).

    ![Dodawanie komunikatu do kolejki.](./media/functions-create-storage-queue-triggered-function/functions-storage-manager-add-message.png)

1. Wpisz komunikat „Hello World!” w polu **Message text** (Tekst komunikatu) i kliknij przycisk **OK**.

1. Poczekaj kilka sekund, a następnie wróć do dzienników funkcji i sprawdź, czy nowy komunikat został odczytany z kolejki.

    ![Wyświetlanie komunikatu w dziennikach.](./media/functions-create-storage-queue-triggered-function/functions-queue-storage-trigger-view-logs.png)

1. Wróć do programu Storage Explorer, kliknij pozycję **Refresh** (Odśwież) i sprawdź, czy komunikat został przetworzony i nie ma go już w kolejce.

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Następne kroki

Utworzono funkcję, która jest uruchamiana w momencie dodania komunikatu do kolejki magazynu.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Aby uzyskać więcej informacji na temat wyzwalaczy usługi Queue Storage, zobacz [Powiązania usługi Queue Storage w usłudze Azure Functions](functions-bindings-storage-queue.md).
