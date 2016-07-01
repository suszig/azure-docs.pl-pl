<properties
    pageTitle="Wprowadzenie do usługi Azure Stream Analytics służącej do przetwarzania danych z urządzeń IoT. | Stream Analytics"
    description="Używanie tagów czujników IoT i strumieni danych z analizą strumieni i przetwarzaniem danych w czasie rzeczywistym"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="paulettm"
    editor="cgronlun"
/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="hero-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="05/03/2016"
    ms.author="jeffstok"
/>

# Wprowadzenie do usługi Azure Stream Analytics służącej do przetwarzania danych z urządzeń IoT

W tym samouczku nauczysz się tworzyć logikę przetwarzania strumieni w celu zbierania danych z urządzeń Internetu rzeczy (IoT). Zostaną przedstawione rzeczywiste przypadki użycia Internetu rzeczy (IoT) w celu zademonstrowania szybkiego i ekonomicznego tworzenia rozwiązań.

## Wymagania wstępne

-   [Subskrypcja platformy Azure](https://azure.microsoft.com/pricing/free-trial/)
-   Przykładowe pliki zapytań i danych do pobrania z serwisu [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/GettingStarted)

## Scenariusz

Contoso to firma produkcyjna z branży automatyzacji przemysłowej, która w pełni zautomatyzowała swój proces produkcji. Maszyny w tym zakładzie mają czujniki emitujące strumienie danych w czasie rzeczywistym. W tym scenariuszu menedżer warsztatu produkcyjnego chce mieć wgląd w czasie rzeczywistym w dane czujników w celu szukania wzorców i podejmowania na ich podstawie odpowiednich działań. Dla danych czujników będzie używany język zapytań usługi Stream Analytics (SAQL) w celu znalezienia interesujących wzorców w przychodzącym strumieniu danych.

W tym przykładzie dane są generowane z urządzenia Texas Instruments Sensor Tag.

![Urządzenie Texas Instruments Sensor Tag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Ładunek danych jest w formacie JSON i może wyglądać następująco:

    
    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  
    
W rzeczywistym scenariuszu będą istnieć setki takich czujników generujących zdarzenia jako strumień. W idealnym przypadku będzie istnieć urządzenie bramy, na którym będzie działał kod służący do wypychania tych zdarzeń do usługi [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Zadanie usługi Stream Analytics będzie pobierać te zdarzenia z usługi Event Hubs i uruchamiać analizę w czasie rzeczywistym wyrażoną jako zapytania, a następnie wysyłać wyniki do odpowiednich wyjść.

W tym przewodniku z wprowadzeniem został udostępniony przykładowy plik danych przechwyconych z rzeczywistych urządzeń Sensor Tag, dla których można uruchamiać różne zapytania i wyświetlać ich wyniki. W kolejnych samouczkach dowiesz się, jak łączyć zadania z wejściami i wyjściami, a następnie wdrażać je w usłudze platformy Azure.

## Tworzenie zadania usługi Stream Analytics

W [portalu Azure](http://manage.windowsazure.com) otwórz usługę Stream Analytics, a następnie kliknij pozycję **Nowy** w lewym dolnym rogu strony, aby utworzyć nowe zadanie analizy.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

Kliknij pozycję **Szybkie tworzenie**.

Dla ustawienia **Konto magazynu monitorowania regionalnego** wybierz pozycję **Utwórz nowe konto magazynu** i nadaj mu dowolną unikatową nazwę. Usługa Azure Stream Analytics będzie używała tego konta do przechowywania informacji o monitorowaniu dla wszystkich przyszłych zadań.

> [AZURE.NOTE] To konto magazynu należy utworzyć tylko raz dla danego regionu. Będzie ono współużytkowane przez wszystkie zadania usługi Stream Analytics utworzone w danym regionie.

Kliknij pozycję **Utwórz zadanie usługi Stream Analytics** w dolnej części strony.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## Zapytanie usługi Azure Stream Analytics

Kliknij kartę Zapytanie, aby przejść do edytora zapytań. Karta Zapytanie zawiera zapytanie SQL, które wykonuje przekształcenie danych przychodzących.

## Archiwizowanie danych pierwotnych

Najprostszą postacią zapytania jest przekazywanie, które spowoduje zarchiwizowanie wszystkich danych wejściowych w określonej lokalizacji wyjściowej.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Pobierz przykładowy plik danych z serwisu [GitHub](https://github.com/Azure/azure-stream-analytics/tree/master/Samples/GettingStarted) do lokalizacji na komputerze. Skopiuj i wklej zapytanie z pliku **PassThrough.txt**. Kliknij poniższy przycisk Testuj, a następnie wybierz plik danych o nazwie **HelloWorldASA-InputStream.json** z lokalizacji pobierania.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

Wyniki zapytania można wyświetlić w przeglądarce poniżej.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## Czyszczenie danych na podstawie warunku

Poniżej przedstawiono filtrowanie wyników na podstawie warunku. Należy wyświetlić wyniki tylko dla tych zdarzeń, które pochodzą z czujnika „SensorA”. Zapytanie znajduje się w pliku **Filtering.txt**.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Należy pamiętać, że w tej sytuacji porównywana jest wartość ciągu, w przypadku której rozróżniana jest wielkość liter. Kliknij przycisk **Uruchom ponownie**, aby wykonać zapytanie. Zapytanie powinno zwrócić tylko 389 wierszy z 1860 zdarzeń.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## Wysyłanie alertów w celu wyzwolenia biznesowego przepływu pracy

Teraz zapytanie użyte bardziej interesujące zapytanie. Aby dla każdego typu czujnika monitorować średnią temperaturę co 30 sekund i wyświetlać wyniki tylko wtedy, gdy przekracza ona 100 stopni, należy napisać poniższe zapytanie, a następnie w celu wyświetlenia wyników kliknąć pozycję Uruchom ponownie. Zapytanie znajduje się w pliku **ThresholdAlerting.txt**.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Możesz teraz zobaczyć, że wyniki zawierają tylko 245 wierszy z tych czujników, w przypadku których średnia temperatura przekracza 100 stopni. W tym zapytaniu strumień zdarzeń został pogrupowany według wartości dspl, czyli nazwy czujnika, i wartości **Okno wirowania** równej 30 sekund. Podczas tworzenia takich czasowych zapytań ważny jest sposób wyrażania postępu czasu. Przy użyciu klauzuli **TIMESTAMP BY** została określona kolumna „time” w celu wyznaczania postępu czasu dla wszystkich obliczeń czasowych. Aby uzyskać szczegółowe informacje, przeczytaj tematy w witrynie MSDN dotyczące [zarządzania czasem](https://msdn.microsoft.com/library/azure/mt582045.aspx) i [obsługi okien](https://msdn.microsoft.com/library/azure/dn835019.aspx).

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## Określanie braku wzorców

Jak napisać zapytanie w celu określenia braku wzorców? Spróbujmy na przykład określić, kiedy ostatnio czujnik wysłał dane, a następnie przez kolejną minutę nie wysłał żadnego zdarzenia. Zapytanie znajduje się w pliku **AbsenseOfEvent.txt**.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

W tym przypadku zostanie użyte sprzężenie **LEFT OUTER JOIN** względem tego samego strumienia danych (samosprzężenie). W przypadku sprzężenia wewnętrznego wynik jest zwracany tylko wtedy, gdy zostanie znalezione dopasowanie.  Natomiast w przypadku sprzężenia **LEFT OUTER**, jeśli zdarzenie z lewej strony sprzężenia jest niedopasowane, dla wszystkich kolumn prawego wiersza jest zwracany wiersz z wartością NULL. Ta technika jest bardzo przydatna podczas wyszukiwania braku zdarzeń. Aby uzyskać więcej informacji na temat sprzężenia [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx), zobacz dokumentację w witrynie MSDN.

![](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## Podsumowanie

Ten samouczek stanowi wprowadzenie do pisania różnych zapytań SAQL i wyświetlania wyników w przeglądarce dotyczących różnych wzorców danych. Jest to jednak tylko początek. Za pomocą usługi Stream Analytics można zrobić dużo więcej. Następnym krokiem będzie uzyskanie informacji o łączeniu zadania usługi Stream Analytics z wejściami i wyjściami oraz wdrażaniu tego zadania na platformie Azure. Aby lepiej poznać usługę Stream Analytics, możesz zacząć od przewodnika [Learning Map](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/) (Mapa szkoleń). Aby uzyskać więcej informacji o pisaniu zapytań, przeczytaj artykuł dotyczący [typowych wzorców zapytań](./stream-analytics-stream-analytics-query-patterns.md#query-example-detect-the-absence-of-events).



<!--HONumber=Jun16_HO2-->


