---
title: "Odpowiadanie na zdarzenia przy użyciu alertów usługi Azure Log Analytics | Microsoft Docs"
description: "Ten samouczek pomaga w zrozumieniu alertów w usłudze Log Analytics, które pomagają identyfikować ważne informacje w repozytorium OMS, oraz aktywnie powiadamiają użytkownika o problemach lub wywołują akcje w celu ich skorygowania."
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
ms.openlocfilehash: fcfaa849f67ffcfa69672d116837e96d318c2124
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: HT
ms.contentlocale: pl-PL
ms.lasthandoff: 02/21/2018
---
# <a name="respond-to-events-with-log-analytics-alerts"></a>Odpowiadanie na zdarzenia przy użyciu alertów usługi Log Analytics
Alerty w usłudze Log Analytics identyfikują ważne informacje w repozytorium usługi Log Analytics. Są one tworzone przy użyciu reguł alertów, które automatycznie uruchamiają przeszukiwanie dzienników w regularnych odstępach czasu. Jeśli wyniki przeszukiwania dzienników pasują do danych kryteriów, zostaje utworzony rekord alertu. Ponadto można go skonfigurować tak, aby wykonywał automatyczną odpowiedź.  Ten samouczek jest kontynuacją samouczka [Tworzenie i udostępnianie pulpitów nawigacyjnych z danymi usługi Log Analytics](log-analytics-tutorial-dashboards.md).   

Ten samouczek zawiera informacje na temat wykonywania następujących czynności:

> [!div class="checklist"]
> * Tworzenie reguły alertu
> * Konfiguracja reguły alertu w celu wysyłania powiadomień e-mail

Do utworzenia przykładu przedstawionego w tym samouczku potrzebna jest istniejąca maszyna wirtualna [połączona z obszarem roboczym usługi Log Analytics](log-analytics-quick-collect-azurevm.md).  

## <a name="log-in-to-azure-portal"></a>Logowanie do witryny Azure Portal
Zaloguj się do witryny Azure Portal pod adresem [https://portal.azure.com](https://portal.azure.com). 

## <a name="create-alerts"></a>Tworzenie alertów

Alerty są tworzone przez reguły alertów, które automatycznie uruchamiają przeszukiwanie dzienników w regularnych odstępach czasu.  Możesz utworzyć alerty w oparciu o konkretne metryki wydajności lub w momencie, w którym zostają utworzone określone zdarzenia, w przypadku braku zdarzenia, a także w sytuacji, w której wiele zdarzeń zostaje utworzonych w danym przedziale czasu.  Na przykład alertów można używać do powiadamiania użytkownika, gdy średnie użycie procesora CPU przekroczy określony próg lub gdy zostanie wygenerowane zdarzenie w sytuacji, w której usługa systemu Windows lub demon systemu Linux nie są uruchomione.   Jeśli wyniki przeszukiwania dzienników pasują do określonych kryteriów, zostaje utworzony rekord alertu. Następnie reguła może automatycznie uruchomić jedną lub kilka akcji, aby aktywnie powiadomić użytkownika o alercie lub wywołać inny proces. 

W poniższym przykładzie tworzy się regułę alertu pomiaru metryki, która tworzy alert dla każdego obiektu komputera w zapytaniu o wartości przekraczającej 90% progu.

1. W witrynie Azure Portal kliknij pozycję **Wszystkie usługi**. Na liście zasobów wpisz **Log Analytics**. Po rozpoczęciu pisania zawartość listy jest filtrowana w oparciu o wpisywane dane. Wybierz pozycję **Log Analytics**.
2. Uruchom portal OMS, wybierając opcję Portal OMS, a następnie na stronie **Omówienie** wybierz opcję **Przeszukiwanie dzienników**.  
3. Wybierz opcję **Ulubione** w górnej części portalu i w okienku **Zapisane wyszukiwania** po prawej stronie wybierz pozycję *Maszyny wirtualne platformy Azure — użycie procesora*.  
4. Kliknij opcję **Alert** u góry strony, aby otworzyć ekran **Dodaj regułę alertu**.  
5. Skonfiguruj regułę alertu przy użyciu następujących informacji:  
   a. Podaj **Nazwę** alertu, np. *Użycie procesora maszyny wirtualnej przekracza >90*  
   b. W opcji **Przedział czasu** określ zakres czasu dla zapytania, np. *30*.  Zapytanie zwraca tylko rekordy utworzone w tym zakresie czasu bieżącego.  
   d. **Częstotliwość alertu** określa częstotliwość, z jaką należy uruchamiać zapytanie.  W tym przykładzie określ wartość *5* min, która będzie występować w naszym określonym przedziale czasu.  
   d. Wybierz opcję **Pomiar metryki** i wprowadź wartość *90* w pozycji **Wartość zagregowana** oraz wartość *3* w pozycji **Wyzwalaj alert na podstawie**   
   e. W obszarze **Akcje** wyłącz powiadomienia e-mail.
6. Kliknij przycisk **Zapisz**, aby zakończyć tworzenie reguły alertu. Rozpoczyna ona działanie od razu.<br><br> ![Przykład reguły alertu](media/log-analytics-tutorial-response/log-analytics-alert-01.png)

Rekordy alertów utworzone przez reguły alertów w usłudze Log Analytics mają typ **Alert** i system źródłowy **OMS**.<br><br> ![Przykład wygenerowanych zdarzeń alertów](media/log-analytics-tutorial-response/log-analytics-alert-events-01.png)  

## <a name="alert-actions"></a>Akcje alertów
Możesz wykonać zaawansowane akcje przy użyciu alertów, np. utworzyć powiadomienia e-mail, uruchomić [element runbook usługi Automation](../automation/automation-runbook-types.md), użyć elementu webhook do utworzenia rekordu incydentu w systemie zarządzania incydentami ITSM lub przy użyciu [rozwiązania IT Service Management Connector](log-analytics-itsmc-overview.md) w odpowiedzi na spełnienie kryteriów alertu.   

Akcje poczty e-mail wysyłają wiadomość e-mail ze szczegółami alertu do co najmniej jednego adresata. Możesz określić temat wiadomości e-mail, ale jej zawartość jest standardowym formatem tworzonym przez usługę Log Analytics.  Zaktualizujmy wcześniej utworzoną regułę alertu i skonfigurujmy ją, aby wysyłać powiadomienia e-mail, zamiast aktywnie monitorować rekord alertu przy użyciu przeszukiwania dzienników.     

1. W portalu OMS w górnym menu wybierz opcję **Ustawienia**, a następnie wybierz pozycję **Alerty**.
2. Na liście reguł alertów kliknij ikonę ołówka obok wcześniej utworzonego alertu.
3. W obszarze **Akcje** włącz powiadomienia e-mail.
4. Podaj **Temat** wiadomości e-mail, np. *Użycie procesora przekracza próg >90*.
5. Dodaj adresy dla co najmniej jednego adresata poczty e-mail w polu **Adresaci**.  Jeśli określasz więcej niż jeden adres, oddziel je przy użyciu średnika (;).
6. Kliknij przycisk **Zapisz**, aby zakończyć tworzenie reguły alertu. Rozpoczyna ona działanie od razu.<br><br> ![Reguła alertu z powiadomieniem e-mail](media/log-analytics-tutorial-response/log-analytics-alert-02.png)

## <a name="next-steps"></a>Następne kroki
W tym samouczku przedstawiono sposób aktywnego identyfikowania i reagowania reguł alertów na problemy podczas uruchamiania przeszukiwania dzienników w zaplanowanych odstępach czasu i w przypadku dopasowania do określonych kryteriów.  

Użyj tego linku, aby wyświetlić przykłady wstępnie utworzonych skryptów usługi Log Analytics.  

> [!div class="nextstepaction"]
> [Przykłady skryptów usługi Log Analytics](powershell-samples.md)