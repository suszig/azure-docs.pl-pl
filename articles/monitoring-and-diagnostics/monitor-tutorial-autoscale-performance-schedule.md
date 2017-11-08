---
title: "Zasoby Azure skalowania automatycznego na podstawie danych wydajności lub harmonogramu | Dokumentacja firmy Microsoft"
description: "Utwórz ustawienie automatycznego skalowania dla planu usługi aplikacji przy użyciu danych metryki i harmonogramu"
author: anirudhcavale
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: tutorial
ms.date: 09/25/2017
ms.author: ancav
ms.custom: mvc
ms.openlocfilehash: 3a85e288fa6f7d6c7138b7fea8319bd8dee01c2c
ms.sourcegitcommit: 6a6e14fdd9388333d3ededc02b1fb2fb3f8d56e5
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 11/07/2017
---
# <a name="create-an-autoscale-setting-for--azure-resources-based-on-performance-data-or-a-schedule"></a>Utwórz ustawienie automatycznego skalowania dla zasobów platformy Azure na podstawie danych wydajności lub harmonogramu

Ustawienia skalowania automatycznego umożliwiają dodawanie/usuwanie wystąpienia usługi na podstawie istniejących warunków. Te ustawienia mogą być tworzone za pośrednictwem portalu. Ta metoda zapewnia interfejsu użytkownika opartego na przeglądarce do tworzenia i konfigurowania ustawienia skalowania automatycznego. 

W tym samouczku zostanie 
> [!div class="checklist"]
> * Tworzenie planu usługi aplikacji i aplikacji sieci Web
> * Konfigurowanie skalowania automatycznego zasady w skali i skalowania w poziomie na podstawie liczby żądań, które otrzymuje aplikacji sieci Web
> * Wyzwalanie akcji skalowania w poziomie i obserwować liczbę wystąpień zwiększyć
> * Uruchomić akcję skalowania i obejrzyj liczba wystąpień, Zmniejsz
> * Czyszczenie zasobów

Jeśli nie masz subskrypcji platformy Azure, przed rozpoczęciem utwórz [bezpłatne](https://azure.microsoft.com/free/) konto.

## <a name="log-in-to-the-azure-portal"></a>Logowanie do witryny Azure Portal

Zaloguj się do witryny [Azure Portal](https://portal.azure.com/).

## <a name="create-a-web-app-and-app-service-plan"></a>Tworzenie planu usługi aplikacji i aplikacji sieci Web
Kliknij przycisk **nowy** opcji z okienka nawigacji po lewej stronie

Wyszukaj i wybierz *aplikacji sieci Web* element i kliknij przycisk **Utwórz**

Wybierz nazwę aplikacji, takich jak *MyTestScaleWebApp*. Utwórz nową grupę zasobów * myResourceGroup "i umieszczenie go w grupie zasobów wybrane.

W ciągu kilku minut powinien zainicjowana zasobów. Za pomocą aplikacji sieci Web, a odpowiedni Plan usługi App Service w pozostałej części tego samouczka.

    ![Create a new app service in the portal](./media/monitor-tutorial-autoscale-performance-schedule/Web-App-Create.png)

## <a name="navigate-to-autoscale-settings"></a>Przejdź do ustawienia skalowania automatycznego
1. W okienku nawigacji po lewej stronie wybierz **Monitor** opcji. Po pobraniu strony wybierz **skalowania automatycznego** kartę.
2. Lista zasobów w ramach Twojej subskrypcji, które obsługują skalowania automatycznego znajduje się w tym miejscu. Zidentyfikować Plan usługi App Service utworzonego wcześniej w samouczku i kliknij ją.

    ![Przejdź do ustawienia skalowania automatycznego](./media/monitor-tutorial-autoscale-performance-schedule/monitor-blade-autoscale.png)

3. W ustawieniu skalowania automatycznego, kliknij polecenie **Włączanie automatycznego skalowania** przycisku

Następny kilka kroków pomocy wypełnione ekranu skalowania automatycznego wygląd na poniższej ilustracji:

   ![Zapisz ustawienia skalowania automatycznego](./media/monitor-tutorial-autoscale-performance-schedule/Autoscale-Setting-Save.png)

 ## <a name="configure-default-profile"></a>Skonfiguruj profil domyślny
1. Podaj **nazwa** Ustawienia skalowania automatycznego
2. Profil domyślny, upewnij się, **tryb skali** ma ustawioną wartość "Skalowania liczby wystąpień określonego"
3. Ustaw liczbę wystąpień **1**. To ustawienie zapewnia, że nie inne profil jest aktywny lub w efekcie profil domyślny zwraca liczbę wystąpień do 1.

  ![Przejdź do ustawienia skalowania automatycznego](./media/monitor-tutorial-autoscale-performance-schedule/autoscale-setting-profile.png)


## <a name="create-recurrance-profile"></a>Utwórz profil recurrance

1. Polecenie **Dodaj warunek skali** łącze w obszarze profil domyślny

2. Edytuj **nazwa** profilu jako "Od poniedziałku do piątku profilu"

3. Upewnij się, **tryb skali** ma ustawioną wartość "Skali w oparciu metryki"

4. Dla **wystąpienie limity** ustawić **Minimum** jako "1", **maksymalna** jako "2" i **domyślne** jako "1". To ustawienie zapewnia, że ten profil jest nie skalowania automatycznego planu usług mniej niż 1 wystąpienie lub więcej niż 2 wystąpienia. Jeśli profil nie ma wystarczających danych podjęcie decyzji, wykorzystuje domyślną liczbę wystąpień (w tym przypadku 1).

5. Aby uzyskać **harmonogram**, wybierz opcję "Repeat określone dni"

6. Ustaw profil powtórzeń od poniedziałku do piątku, z PST 09:00 do 18:00 PST. To ustawienie zapewnia, że ten profil jest tylko aktywne i stosowane 9 AM do 18: 00, od poniedziałku do piątku. W pozostałych godzinach "Default" profil jest profil, który używa w ustawieniu skalowania automatycznego.

## <a name="create-a-scale-out-rule"></a>Utwórz regułę skalowalnego w poziomie

1. W polu "Od poniedziałku do piątku w profilu"

2. Kliknij przycisk **dodać regułę** łącza

3. Ustaw **metryki źródła** jako innego zasobu. Ustaw **typu zasobu** jako "App Services" i **zasobów** jako aplikacji sieci Web utworzonej wcześniej w tym samouczku.

4. Ustaw **czasu agregacji** jako "Ogółem" **Nazwa metryki** jako "Żądania" i **Statystyka ziarno czasu** jako "Sum"

5. Ustaw **Operator** jako "Greater than" **próg** jako "10" i **czas trwania** "5" minut.

6. Wybierz **operacji** jako "Wzrost liczby przez" **wystąpienia licznika** jako "1" i **chłodnych dół** "5" minut

7. Kliknij przycisk **Dodaj** przycisku

Ta reguła zapewnia, że jeśli aplikacja sieci Web odbiera więcej niż 10 żądania w ciągu 5 minut lub mniej, jedno wystąpienie dodatkowe zostanie dodany do Twojego planu usługi App Service do zarządzania obciążenia.

   ![Utwórz regułę skalowalnego w poziomie](./media/monitor-tutorial-autoscale-performance-schedule/Scale-Out-Rule.png)

## <a name="create-a-scale-in-rule"></a>Utwórz regułę skali
Firma Microsoft zaleca zawsze Aby reguła skali w towarzyszące reguł skalowania w poziomie. Oba mających gwarantuje, że zasoby nie są ponad zainicjowane. Za pośrednictwem udostępniania oznacza, że masz wystąpień więcej niż jest to potrzebne do obsługi bieżącego obciążenia. 

1. W polu "Od poniedziałku do piątku w profilu"

2. Kliknij przycisk **dodać regułę** łącza

3. Ustaw **metryki źródła** jako innego zasobu. Ustaw **typu zasobu** jako "App Services" i **zasobów** jako aplikacji sieci Web utworzonej wcześniej w tym samouczku.

4. Ustaw **czasu agregacji** jako "Ogółem" **Nazwa metryki** jako "Żądania" i **Statystyka ziarno czasu** jako "Średnia"

5. Ustaw **Operator** jako "Poniżej", **próg** jako "5" i **czas trwania** "5" minut.

6. Wybierz **operacji** jako "Zmniejszenie liczby przez" **wystąpienia licznika** jako "1" i **chłodnych dół** "5" minut

7. Kliknij przycisk **Dodaj** przycisku

    ![Utwórz regułę skali](./media/monitor-tutorial-autoscale-performance-schedule/Scale-In-Rule.png)

8. **Zapisz** w ustawieniu skalowania automatycznego

    ![Zapisz ustawienia skalowania automatycznego](./media/monitor-tutorial-autoscale-performance-schedule/Autoscale-Setting-Save.png)

## <a name="trigger-scale-out-action"></a>Wyzwalacz akcji skalowania w poziomie
Aby wyzwolić warunku skalowalnego w poziomie w ustawieniu skalowania automatycznego właśnie utworzony, aplikacji sieci Web musi mieć więcej niż 10 żądań w mniej niż 5 minut.

1. Otwórz okno przeglądarki i przejdź do aplikacji sieci Web utworzonej wcześniej w tym samouczku. Można znaleźć adres URL dla aplikacji sieci Web w portalu Azure, przechodząc do zasobu aplikacji sieci Web i kliknięcie **Przeglądaj** przycisku na karcie "Informacje".

2. Szybkie naciśnięcie ponownie załaduj stronę więcej niż 10 razy

3. W okienku nawigacji po lewej stronie wybierz **Monitor** opcji. Po pobraniu strony wybierz **skalowania automatycznego** kartę.

4. Z listy wybierz Plan usługi aplikacji używane w tym samouczku

5. W ustawieniu skalowania automatycznego, kliknij polecenie **Historia uruchomień** kartę

6. Zostanie wyświetlony wykres odzwierciedlające liczby wystąpień Plan usługi aplikacji w czasie

7. Za kilka minut liczba wystąpień powinno spowodować od 1, 2.

8. W obszarze wykresu zobacz temat wpisy dziennika aktywności dla każdej akcji skalowania podjętej przez to ustawienie skalowania automatycznego.

## <a name="trigger-scale-in-action"></a>Akcja wyzwalacza w skali
Warunek w skali w skalowania automatycznego ustawienie wyzwalacze, jeśli mniej niż 5 żądań do aplikacji sieci Web w okresie 10 minut. 

1. Upewnij się, że żądania nie są wysyłane do aplikacji sieci Web

2. Obciążenia portalu Azure

3. W okienku nawigacji po lewej stronie wybierz **Monitor** opcji. Po pobraniu strony wybierz **skalowania automatycznego** kartę.

4. Z listy wybierz Plan usługi aplikacji używane w tym samouczku

5. W ustawieniu skalowania automatycznego, kliknij polecenie **Historia uruchomień** kartę

6. Zostanie wyświetlony wykres odzwierciedlające liczby wystąpień Plan usługi aplikacji w czasie.

7. Za kilka minut liczba wystąpień należy usunąć z 2, do 1. Proces trwa co najmniej 100 minut.  

8. W obszarze wykresu jest odpowiedni zestaw działań wpisy dziennika dla każdej akcji skalowania podjętej przez to ustawienie skalowania automatycznego

    ![Wyświetlanie w skali akcji](./media/monitor-tutorial-autoscale-performance-schedule/Scale-In-Chart.png)

## <a name="clean-up-resources"></a>Oczyszczanie zasobów

1. Z menu po lewej stronie w portalu Azure kliknij **wszystkie zasoby** , a następnie wybierz aplikację sieci Web utworzonej w tym samouczku.

2. Na stronie zasobów, kliknij przycisk **usunąć**, wpisując potwierdzić zamiar usunięcia **tak** w polu tekstowym, a następnie kliknij przycisk **usunąć**.

3. Następnie wybierz Plan usługi App Service zasobów i kliknij przycisk **usunąć**

4. Potwierdź usunięcie wpisując **tak** w polu tekstowym, a następnie kliknij przycisk **usunąć**.

## <a name="next-steps"></a>Następne kroki

W tym samouczku można  
> [!div class="checklist"]
> * Utworzona aplikacja sieci Web i Plan usługi aplikacji
> * Skalowania automatycznego skonfigurowane zasady w skali i skalowania w poziomie na podstawie liczby żądań aplikacji sieci Web odebrał
> * Wyzwalane akcji skalowania w poziomie i obserwowane liczba wystąpień zwiększyć
> * Wyzwalane akcję skalowania i obserwowane liczba wystąpień, Zmniejsz
> * Wyczyszczone zasobów


Aby dowiedzieć się więcej na temat ustawień automatycznego skalowania, w dalszym ciągu na [Omówienie automatycznego skalowania](monitoring-overview-autoscale.md).

> [!div class="nextstepaction"]
> [Archiwizowanie danych monitorowania](monitor-tutorial-archive-monitoring-data.md)
