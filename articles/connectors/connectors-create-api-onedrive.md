---
title: "Dodaj łącznik usługi OneDrive w aplikacji logiki | Dokumentacja firmy Microsoft"
description: "Omówienie łącznika usługi OneDrive z parametrami interfejsu API REST"
services: logic-apps
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: 47a8582a-1b1a-4fc3-beb5-97c60c4306fe
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 10/18/2016
ms.author: mandia; ladocs
ms.openlocfilehash: 63bd33bf4e09b98aa53dcfec9fcc4a0109204952
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-the-onedrive-connector"></a>Rozpoczynanie pracy z łącznikiem usługi OneDrive
Połączenie do usługi OneDrive zarządzania plikami, w tym przekazywane, get, usuwanie plików i inne. 

Z usługą OneDrive możesz: 

* Tworzenie przepływu pracy przez zapisanie plików w usłudze OneDrive lub zaktualizować istniejące pliki w usłudze OneDrive. 
* Wyzwalacze są używane do uruchomienia przepływu pracy, gdy plik jest tworzony lub aktualizowany w aplikacji OneDrive.
* Użyj działania do utworzenia pliku, usuń plik i inne. Na przykład po odebraniu nowej usługi Office 365 wiadomości e-mail z załącznikiem (wyzwalacz) Utwórz nowy plik w usłudze OneDrive (działanie).

W tym temacie opisano sposób korzystania z łącznika usługi OneDrive w aplikacji logiki, a także wyświetla wyzwalacze i akcje.

Aby dowiedzieć się więcej na temat aplikacji logiki, zobacz [co to jest logic apps](../logic-apps/logic-apps-what-are-logic-apps.md) i [tworzenie aplikacji logiki](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-onedrive"></a>Połączenie do usługi OneDrive
Zanim aplikację logiki można uzyskać dostęp do dowolnej usługi, należy najpierw utworzyć *połączenia* do usługi. Połączenie stanowi połączenie między aplikacji logiki i innej usługi. Na przykład, aby nawiązać połączenie usługi OneDrive, należy najpierw OneDrive *połączenia*. Aby utworzyć połączenie, wprowadź poświadczenia, zwykle używanego do uzyskania dostępu do usługi, który chcesz połączyć się z. Tak z usługą OneDrive, wprowadź poświadczenia z kontem usługi OneDrive, aby utworzyć połączenie.

### <a name="create-the-connection"></a>Tworzenie połączenia
> [!INCLUDE [Steps to create a connection to OneDrive](../../includes/connectors-create-api-onedrive.md)]
> 
> 

## <a name="use-a-trigger"></a>Użyć wyzwalacza
Wyzwalacz to zdarzenie służy do uruchomienia przepływu pracy zdefiniowanych w aplikacji logiki. Wyzwalacze "sondować" Usługa interwału i częstotliwość. [Dowiedz się więcej o wyzwalaczy](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. W aplikacji logiki wpisz "onedrive" w celu uzyskania listy wyzwalaczy:  
   
    ![](./media/connectors-create-api-onedrive/onedrive-1.png)
2. Wybierz **modyfikacji pliku**. Jeśli połączenie już istnieje, następnie wybierz przycisk Pokaż selektora, aby wybrać folder.
   
    ![](./media/connectors-create-api-onedrive/sample-folder.png)
   
    Po pojawieniu się zalogować, wprowadź znak w szczegóły, aby utworzyć połączenie. [Utwórz połączenie](connectors-create-api-onedrive.md#create-the-connection) w tym temacie przedstawiono kroki. 
   
   > [!NOTE]
   > W tym przykładzie aplikacji logiki uruchamiane po pliku w folderze wybranej aktualizacji. Aby wyświetlić wyniki tego wyzwalacza, Dodaj inną akcję wysyłanej wiadomości e-mail. Na przykład dodać Office 365 Outlook *wysłać wiadomość e-mail* akcję, która wysyła pocztą e-mail, należy po zaktualizowaniu pliku. 

3. Wybierz **Edytuj** przycisk i ustaw **częstotliwość** i **interwał** wartości. Na przykład, jeśli chcesz wyzwalacza do sondowania co 15 minut, następnie ustawić **częstotliwość** do **minutę**i ustaw **interwał** do **15**. 
   
    ![](./media/connectors-create-api-onedrive/trigger-properties.png)
4. **Zapisz** zmiany (lewym górnym rogu paska narzędzi). Aplikację logiki jest zapisywana i automatycznie włączone.

## <a name="use-an-action"></a>Za pomocą akcji
Akcja jest przeprowadzane przez przepływ pracy zdefiniowanych w aplikacji logiki operacji. [Dowiedz się więcej o akcjach](../logic-apps/logic-apps-what-are-logic-apps.md#logic-app-concepts).

1. Wybierz znak plus. Zobacz kilka opcji: **Dodaj akcję**, **Dodaj warunek**, lub jeden z **więcej** opcje.
   
    ![](./media/connectors-create-api-onedrive/add-action.png)
2. Wybierz **Dodaj akcję**.
3. W polu tekstowym wpisz "onedrive", aby uzyskać listę dostępnych akcji.
   
    ![](./media/connectors-create-api-onedrive/onedrive-actions.png) 
4. W tym przykładzie wybierz **OneDrive — Utwórz plik**. Jeśli połączenie już istnieje, następnie wybierz **ścieżka folderu** umieścić pliku, wprowadź **nazwę pliku**i wybierz polecenie **zawartość pliku** ma:  
   
    ![](./media/connectors-create-api-onedrive/sample-action.png)
   
    Jeśli zostanie wyświetlony monit o informacje dotyczące połączenia, wprowadź szczegóły, aby utworzyć połączenie. [Utwórz połączenie](connectors-create-api-onedrive.md#create-the-connection) w tym temacie opisano te właściwości. 
   
   > [!NOTE]
   > W tym przykładzie utworzymy nowy plik w folderze OneDrive. Dane wyjściowe z innego wyzwalacza służy do tworzenia plików usługi OneDrive. Na przykład dodać Office 365 Outlook *po odebraniu nowej wiadomości e-mail* wyzwalacza. Następnie dodaj OneDrive *Utwórz plik* akcję, która używa typu zawartości i załączników pola w obrębie ForEach do utworzenia nowego pliku w usłudze OneDrive. 
   > 
   > ![](./media/connectors-create-api-onedrive/foreach-action.png)

5. **Zapisz** zmiany (lewym górnym rogu paska narzędzi). Aplikację logiki jest zapisywana i automatycznie włączone.


## <a name="connector-specific-details"></a>Szczegóły dotyczące łącznika

Wyświetl wszystkie wyzwalacze i akcje zdefiniowane w swagger i zobacz też żadnych limitów w [szczegóły łącznika](/connectors/onedriveconnector/).

## <a name="more-connectors"></a>Więcej łączników
Wróć do [listy interfejsów API](apis-list.md).