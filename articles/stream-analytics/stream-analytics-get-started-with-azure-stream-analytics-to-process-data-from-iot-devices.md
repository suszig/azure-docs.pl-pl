<properties
    pageTitle="Wprowadzenie do usługi Azure Stream Analytics służącej do przetwarzania danych z urządzeń IoT. | Microsoft Azure"
    description="Używanie tagów czujników IoT i strumieni danych z analizą strumieni i przetwarzaniem danych w czasie rzeczywistym"
    keywords="rozwiązanie iot, wprowadzenie do iot"
    services="stream-analytics"
    documentationCenter=""
    authors="jeffstokes72"
    manager="jhubbard"
    editor="cgronlun"
/>

<tags
    ms.service="stream-analytics"
    ms.devlang="na"
    ms.topic="hero-article"
    ms.tgt_pltfrm="na"
    ms.workload="data-services"
    ms.date="09/26/2016"
    ms.author="jeffstok"
/>


# Wprowadzenie do usługi Azure Stream Analytics służącej do przetwarzania danych z urządzeń IoT

Korzystając z tego samouczka, nauczysz się tworzyć logikę przetwarzania strumieni w celu zbierania danych z urządzeń Internetu rzeczy (IoT). Użyjemy w nim rzeczywistych przypadków użycia Internetu rzeczy (IoT) w celu zademonstrowania szybkiego i ekonomicznego kompilowania rozwiązań.

## Wymagania wstępne

-   [Subskrypcja platformy Azure](https://azure.microsoft.com/pricing/free-trial/)
-   Przykładowe pliki zapytań i danych do pobrania z serwisu [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot)

## Scenariusz

Contoso to firma z branży automatyki przemysłowej, która w pełni zautomatyzowała swój proces produkcji. Maszyny w tym zakładzie mają czujniki mogące emitować strumienie danych w czasie rzeczywistym. W tym scenariuszu menedżer warsztatu produkcyjnego chce mieć wgląd w czasie rzeczywistym w dane czujników w celu szukania wzorców i podejmowania na ich podstawie odpowiednich działań. Dla danych czujników będzie używany język zapytań usługi Stream Analytics (SAQL) w celu znalezienia interesujących wzorców w przychodzącym strumieniu danych.

W tym przykładzie dane są generowane z urządzenia Texas Instruments Sensor Tag.

![Urządzenie Texas Instruments Sensor Tag](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-01.jpg)

Ładunek danych jest w formacie JSON i może wyglądać następująco:


    {
        "time": "2016-01-26T20:47:53.0000000",  
        "dspl": "sensorE",  
        "temp": 123,  
        "hmdt": 34  
    }  

W rzeczywistym scenariuszu mogą istnieć setki takich czujników generujących zdarzenia jako strumień. W idealnym przypadku urządzenie bramy będzie uruchamiać kod służący do wypychania tych zdarzeń do usługi [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) lub [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/). Zadanie usługi Stream Analytics pozyska te zdarzenia z usługi Event Hubs i uruchomi zapytania analityki w czasie rzeczywistym względem strumieni. Następnie będzie można wysłać wyniki do jednego z [obsługiwanych wyjść](stream-analytics-define-outputs.md).

W celu ułatwienia pracy w tym przewodniku z wprowadzeniem udostępniono przykładowy plik danych przechwycony z rzeczywistych urządzeń Sensor Tag. Możesz uruchamiać zapytania dotyczące przykładowych danych i przeglądać ich wyniki. W kolejnych samouczkach dowiesz się, jak łączyć zadania z wejściami i wyjściami, a następnie wdrażać je w usłudze platformy Azure.

## Tworzenie zadania usługi Stream Analytics

1. W witrynie [Azure Portal](http://manage.windowsazure.com) wybierz usługę **STREAM ANALYTICS**, a następnie kliknij pozycję **NOWY** w lewym dolnym rogu strony, aby utworzyć nowe zadanie analizy.

    ![Tworzenie nowego zadania usługi Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-02.png)

2. Kliknij pozycję **SZYBKIE TWORZENIE**.

3. Dla ustawienia **KONTO MAGAZYNU MONITOROWANIA REGIONALNEGO** kliknij pozycję **UTWÓRZ NOWE KONTO MAGAZYNU** i wybierz unikatową nazwę. Usługa Azure Stream Analytics będzie używała tego konta do przechowywania informacji o monitorowaniu dla wszystkich przyszłych zadań.

    > [AZURE.NOTE] W każdym regionie utwórz to konto magazynu tylko raz. Ten magazyn zostanie udostępniony wszystkim zadaniom usługi Stream Analytics tworzonym w danym regionie.

4. Kliknij pozycję **UTWÓRZ ZADANIE USŁUGI STREAM ANALYTICS** w dolnej części strony.

    ![Konfiguracja konta magazynu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-03.jpg)

## Zapytanie usługi Azure Stream Analytics

Kliknij kartę **ZAPYTANIE**, aby przejść do Edytora zapytań. Karta **ZAPYTANIE** zawiera zapytanie T-SQL, które wykonuje przekształcenie danych przychodzących zdarzenia.

## Archiwizowanie danych pierwotnych

Najprostszą postacią zapytania jest przekazywanie, które powoduje archiwizowanie wszystkich danych wejściowych w określonej lokalizacji wyjściowej.

![Zapytanie dotyczące zadania archiwizacji](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-04.png)

Pobierz przykładowy plik danych z usługi [GitHub](https://aka.ms/azure-stream-analytics-get-started-iot) do lokalizacji na komputerze. Wklej zapytanie z pliku PassThrough.txt. Kliknij przycisk **Testuj**, a następnie wybierz plik danych o nazwie HelloWorldASA-InputStream.json z lokalizacji pobierania.

![Przycisk Testuj w usłudze Stream Analytics](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-05.png)

![Testowy strumień wejściowy](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-06.png)

Wyniki zapytania można wyświetlić w przeglądarce, jak pokazano na poniższym zrzucie ekranu.

![Wyniki testu](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-07.png)

## Filtrowanie danych na podstawie warunku

Poniżej przedstawiono filtrowanie wyników na podstawie warunku. Chcemy wyświetlić wyniki tylko dla tych zdarzeń, które pochodzą z czujnika „SensorA”. Zapytanie znajduje się w pliku Filtering.txt.

![Filtrowanie strumienia danych](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-08.png)

Zauważ, że zapytanie uwzględniające wielkość liter porównuje wartość ciągu. Kliknij przycisk **Uruchom ponownie**, aby wykonać zapytanie. Zapytanie powinno zwrócić 389 wierszy z 1860 zdarzeń.

![Drugi zestaw danych wyjściowych z testu zapytania](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-09.png)

## Wysyłanie alertów w celu wyzwolenia biznesowego przepływu pracy

Zwiększmy szczegółowość naszego zapytania. Dla każdego typu czujnika chcemy monitorować średnią temperaturę co 30 sekund i wyświetlać wyniki tylko wtedy, gdy przekracza ona 100 stopni. W tym celu napiszemy poniższe zapytanie, a następnie w celu wyświetlenia wyników klikniemy pozycję **Uruchom ponownie**. Zapytanie znajduje się w pliku ThresholdAlerting.txt.

![Zapytanie z filtrowaniem co 30 sekund](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-10.png)

Wyniki powinny teraz zawierać tylko 245 wierszy i nazwy czujników, w przypadku których średnia temperatura przekracza 100 stopni. W tym zapytaniu strumień zdarzeń został pogrupowany według wartości **dspl**, czyli nazwy czujnika, i wartości **Okno wirowania** równej 30 sekund. W zapytaniu dotyczącym danych czasowych należy określić sposób przyrastania czasu. Użyliśmy klauzuli **TIMESTAMP BY** i wybraliśmy kolumnę **OUTPUTTIME**, aby skojarzyć czasy ze wszystkimi obliczeniami danych czasowych. Aby uzyskać szczegółowe informacje, przeczytaj w witrynie MSDN artykuły dotyczące [zarządzania czasem](https://msdn.microsoft.com/library/azure/mt582045.aspx) i [funkcji obsługi okien](https://msdn.microsoft.com/library/azure/dn835019.aspx).

![Temperatura powyżej 100](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-11.png)

## Wykrywanie braku zdarzeń

Jak napisać zapytanie w celu określenia braku zdarzeń wejściowych? Spróbujmy określić, kiedy ostatnio czujnik wysłał dane, a następnie przez kolejną minutę nie wysłał zdarzeń. Zapytanie znajduje się w pliku AbsenseOfEvent.txt.

![Wykrywanie braku zdarzeń](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-12.png)

W tym przypadku zostanie użyte sprzężenie **LEFT OUTER** względem tego samego strumienia danych (samosprzężenie). W przypadku sprzężenia **INNER** wynik jest zwracany tylko wtedy, gdy zostanie znalezione dopasowanie.  Natomiast w przypadku sprzężenia **LEFT OUTER**, jeśli zdarzenie z lewej strony sprzężenia jest niedopasowane, dla wszystkich kolumn po prawej stronie wiersza jest zwracany wiersz z wartością NULL. Ta technika jest bardzo przydatna do wyszukiwania braku zdarzeń. Aby uzyskać więcej informacji na temat sprzężenia [JOIN](https://msdn.microsoft.com/library/azure/dn835026.aspx), zobacz dokumentację w witrynie MSDN.

![Wyniki sprzężenia join](./media/stream-analytics-get-started-with-iot-devices/stream-analytics-get-started-with-iot-devices-13.png)

## Podsumowanie

Celem tego samouczka jest zademonstrowanie sposobu pisania różnych zapytań języka zapytań usługi Stream Analytics i wyświetlania wyników w przeglądarce. Jest to jednak tylko początek. Za pomocą usługi Stream Analytics można zrobić dużo więcej. Usługa Stream Analytics obsługuje różne dane wejściowe oraz wyjściowe i może nawet korzystać z funkcji w usłudze Azure Machine Learning, co czyni ją doskonałym narzędziem do analizowania strumieni danych. Jeśli chcesz dowiedzieć się więcej na temat usługi Stream Analytics, skorzystaj z naszej [mapy uczenia](https://azure.microsoft.com/documentation/learning-paths/stream-analytics/). Aby uzyskać więcej informacji na temat sposobu pisania zapytań, przeczytaj artykuł dotyczący [typowych wzorców zapytań](./stream-analytics-stream-analytics-query-patterns.md).



<!--HONumber=Oct16_HO3-->


