## Czym są kolejki usługi Service Bus?

Kolejki usługi Service Bus obsługują model komunikacji z użyciem **komunikatów obsługiwanych przez blokera**. Podczas korzystania z kolejek składniki aplikacji rozproszonej nie komunikują się bezpośrednio ze sobą, lecz wymieniają komunikaty za pośrednictwem kolejki, która działa jako pośrednik (broker). Producent komunikatu (nadawca) przekazuje komunikat do kolejki i kontynuuje jego przetwarzanie. Konsument komunikatu (odbiorca) asynchronicznie pobiera komunikat z kolejki i go przetwarza. Producent nie musi czekać na odpowiedź od konsumenta, aby kontynuować przetwarzanie i wysyłanie dalszych komunikatów. Kolejki umożliwiają dostarczanie komunikatów metodą **pierwszy na wejściu — pierwszy na wyjściu (FIFO)** do jednego lub większej liczby konkurencyjnych konsumentów. To oznacza, że komunikaty są zwykle odbierane i przetwarzane przez odbiorców w kolejności, w której zostały dodane do kolejki, a każdy komunikat jest odbierany i przetwarzany przez tylko jednego konsumenta.

![QueueConcepts](./media/howto-service-bus-queues/sb-queues-08.png)

Kolejki usługi Service Bus to technologia ogólnego przeznaczenia, która może być używana w wielu różnych scenariuszach:

-   Komunikacja między rolami sieci Web i procesów roboczych w wielowarstwowej aplikacji Azure.
-   Komunikacja między aplikacjami lokalnymi i hostowanymi na platformie Azure w rozwiązaniu hybrydowym.
-   Komunikacja między składnikami aplikacji rozproszonej działającej lokalnie w różnych organizacjach lub działach organizacji.

Korzystanie z kolejek umożliwia łatwiejsze skalowanie aplikacji i pozwala na większą elastyczność architektury.

## Tworzenie przestrzeni nazw usługi

Aby rozpocząć korzystanie z kolejek usługi Service Bus na platformie Azure, należy najpierw utworzyć przestrzeń nazw usługi. Przestrzeń nazw zapewnia kontener zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji.

Aby utworzyć przestrzeń nazw:

1.  Zaloguj się do [klasycznego portalu Azure][].

2.  W lewym okienku nawigacji portalu kliknij pozycję **Service Bus**.

3.  W dolnym okienku portalu kliknij pozycję **Utwórz**.
    ![](./media/howto-service-bus-queues/sb-queues-03.png)

4.  W oknie dialogowym **Add a new namespace** (Dodawanie nowej przestrzeni nazw) wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.   
    ![](./media/howto-service-bus-queues/sb-queues-04.png)

5.  Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz kraj lub region, w którym przestrzeń nazw ma być hostowana (upewnij się, że używasz tego samego kraju/regionu, w którym są wdrażane zasoby obliczeniowe).

     > [AZURE.IMPORTANT] Wybierz **ten sam region**, który zamierzasz wybrać do wdrożenia aplikacji. Zapewni to najlepszą wydajność.

6.  W pozostałych polach okna dialogowego pozostaw wartości domyślne (**Obsługa komunikatów** i **Warstwa Standardowa**), a następnie kliknij znacznik wyboru OK. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces inicjowania obsługi administracyjnej zasobów dla konta w systemie może potrwać kilka minut.

    ![](./media/howto-service-bus-queues/getting-started-multi-tier-27.png)

Utworzona przestrzeń nazw zostanie za moment aktywowana, a następnie wyświetlona w portalu. Przed wykonaniem dalszych czynności poczekaj, aż stan przestrzeni nazw będzie mieć wartość **Aktywna**.

## Uzyskiwanie domyślnych poświadczeń zarządzania dla przestrzeni nazw

W celu wykonywania operacji zarządzania, takich jak tworzenie kolejki w nowej przestrzeni nazw, należy uzyskać poświadczenia zarządzania dla przestrzeni nazw. Możesz uzyskać te poświadczenia w [klasycznego portalu Azure][].

###Aby uzyskać poświadczenia zarządzania w portalu

1.  W lewym okienku nawigacji kliknij węzeł **Service Bus**, aby wyświetlić listę dostępnych przestrzeni nazw:   
    ![](./media/howto-service-bus-queues/sb-queues-13.png)

2.  Wybierz nowo utworzoną przestrzeń nazw z wyświetlonej listy:   
    ![](./media/howto-service-bus-queues/sb-queues-09.png)

3.  Kliknij pozycję **Informacje o połączeniu**.   
    ![](./media/howto-service-bus-queues/sb-queues-06.png)

4.  W okienku zapewniającym **dostęp do informacji o połączeniu** znajdź parametry połączenia, które zawierają klucz SAS i nazwę klucza.   

    ![](./media/howto-service-bus-queues/multi-web-45.png)
    
5.  Zanotuj klucz lub skopiuj go do schowka.

  [klasycznego portalu Azure]: http://manage.windowsazure.com




<!--HONumber=Jun16_HO2-->


