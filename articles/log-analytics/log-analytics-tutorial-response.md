---
title: "Odpowiadanie na zdarzenia z alertów Analiza dzienników Azure | Dokumentacja firmy Microsoft"
description: "W tym samouczku pomaga w zrozumieniu alerty w analizy dzienników do identyfikacji ważne informacje zawarte w repozytorium OMS i aktywne powiadamia użytkownika o problemy lub akcji, aby je poprawić."
services: log-analytics
documentationcenter: log-analytics
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 09/20/2017
ms.author: magoedte
ms.custom: mvc
ms.openlocfilehash: 3ab8d32eb4b3f2748249f40139de76c8e7f4d971
ms.sourcegitcommit: e6029b2994fa5ba82d0ac72b264879c3484e3dd0
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/24/2017
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Odpowiadanie na zdarzenia z dziennika analizy alerty
Alerty w analizy dzienników zidentyfikować ważne informacje zawarte w repozytorium analizy dzienników.  Są one tworzone przez reguły alertów, które automatycznie uruchamiać dziennik wyszukiwania w regularnych odstępach czasu, a jeśli wyniki wyszukiwania dziennika spełniających kryteria określonego tworzony jest rekord alertów i może być skonfigurowana do wykonywania automatycznej odpowiedzi.  Ten samouczek stanowi kontynuację [tworzenie i udostępnianie pulpitów nawigacyjnych danych analizy dzienników](log-analytics-tutorial-dashboards.md) samouczka.   

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie reguły alertu
> * Skonfiguruj regułę alertu, aby wysłać powiadomienie e-mail

Aby ukończyć przykład, w tym samouczku, musisz mieć istniejącej maszyny wirtualnej [podłączony do obszaru roboczego analizy dzienników](log-analytics-quick-collect-azurevm.md).  

## <a name="log-in-to-azure-portal"></a>Zaloguj się do portalu Azure
Zaloguj się do portalu Azure pod adresem [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-alerts"></a>Tworzenie alertów

Alerty są tworzone przez reguły alertów, które automatycznie uruchamiać dziennik wyszukiwania w regularnych odstępach czasu.  Można tworzyć alertów w oparciu metryki dotyczące wydajności lub gdy określone zdarzenia są tworzone, Brak zdarzenia lub liczba zdarzeń są tworzone w przedziale czasu określonym.  Na przykład alerty mogą służyć do powiadomienie, gdy średnie użycie Procesora przekroczy określony próg lub po określonej usługi systemu Windows jest generowane zdarzenie lub demonów systemu Linux nie jest uruchomiona.   Jeśli wyniki wyszukiwania dziennika spełniających kryteria określonego tworzony jest rekord alertu. Reguła następnie automatycznie uruchomić co najmniej jednej akcji do aktywnego powiadomienia o alercie lub wywołać inny proces. 

W poniższym przykładzie utworzymy metryki pomiaru reguły alertu, co spowoduje utworzenie alert dla każdego obiektu komputera w zapytaniu z wartością, która przekracza próg 90%.

1. W portalu Azure kliknij **więcej usług** znaleziono w lewym dolnym rogu. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz **dziennika analizy**...
2. Uruchamianie portalu OMS, wybierając portalu OMS i na **omówienie** wybierz pozycję **wyszukiwania dziennika**.  
3. Wybierz **ulubione** od góry portalu i w **zapisane wyszukiwania** w okienku po prawej stronie, wybierz zapytanie *maszynach wirtualnych platformy Azure — użycie procesora*.  
4. Kliknij przycisk **alertu** w górnej części strony, aby otworzyć **Dodaj regułę alertu** ekranu.  
5. Skonfiguruj reguły alertów z następującymi informacjami:  
   a. Podaj **nazwa** alertu, takie jak *przekracza wykorzystanie procesora maszyny Wirtualnej > 90*  
   b. Aby uzyskać **przedział czasu**, określić zakres czasu dla zapytania, takie jak *30*.  Zapytanie zwraca tylko te rekordy, które zostały utworzone w ramach tego zakresu bieżącego czasu.  
   c. **Alert częstotliwości** Określa, jak często uruchamiać zapytania.  Na przykład określić *5* minut, które będą miały miejsce w naszym przedział czasu określony.  
   d. Wybierz **metryki pomiaru** , a następnie wprowadź *90* dla **wartości zagregowane** , a następnie wprowadź *3* dla **na podstawie wyzwalania alertu**   
   e. W obszarze **akcje**, wyłączyć powiadomienia e-mail.
6. Kliknij przycisk **zapisać** przeprowadzenie reguły alertów. Zostanie ona rozpoczęta natychmiast uruchomiona.<br><br> ![Przykład reguły alertu](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Rekordy alertu przez reguły alertów w analizy dzienników mieć typu **Alert** i SourceSystem z **OMS**.<br><br> ![Przykład alertów generowanych zdarzeń](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Akcje alertów
Dostępne akcje Zaawansowane z alertami, takie jak tworzenie odpowiednie powiadomienie e-mail, uruchom [elementu runbook automatyzacji](../automation/automation-runbook-types.md), użyj elementu webhook, aby utworzyć rekord zdarzenia w systemie Zarządzanie usługami IT — zarządzanie zdarzeniami lub [IT Rozwiązanie usługi zarządzania łącznika](log-analytics-itsmc-overview.md) jako odpowiedzi po spełnieniu kryteriów alertu.   

Akcje e-mail Wyślij wiadomość e-mail ze szczegółami alertu do co najmniej jednego adresata. Można określić temat wiadomości e-mail, ale jego zawartość jest standardowym formacie tworzony przez analizy dzienników.  Załóżmy aktualizacji reguły alertu utworzonego wcześniej i skonfigurować go do wiadomości e-mail powiadomienie, zamiast aktywnie monitoruje alertu rekordu wyszukiwania dziennika.     

1. W portalu OMS z górnego menu wybierz **ustawienia** , a następnie wybierz **alerty**.
2. Z listy reguł alertów kliknij ikonę ołówka obok alertu utworzony wcześniej.
3. W obszarze **akcje** sekcji, należy włączyć funkcję powiadomienia e-mail.
4. Podaj **podmiotu** dla wiadomości e-mail, takich jak *wartości progowej przekracza wykorzystanie procesora > 90*.
5. Dodaj adresy jednego lub kilku adresatów poczty e-mail w **adresatów** pola.  Jeśli określono więcej niż jeden adres, oddziel adresy średnikiem (;).
6. Kliknij przycisk **zapisać** przeprowadzenie reguły alertów. Zostanie ona rozpoczęta natychmiast uruchomiona.<br><br> ![Reguła alertu z powiadomienia e-mail](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono, jak alert reguły można aktywne identyfikowanie i odpowiadać na problem podczas uruchamiania dziennika wyszukiwania w zaplanowanych odstępach czasu i określonym kryteriom.  

Wykonaj to łącze, aby wyświetlić przykłady skryptów analizy dzienników wbudowanych.  

> [!div class="nextstepaction"]
> [Zaloguj się przykłady skryptów analityka](powershell-samples.md)