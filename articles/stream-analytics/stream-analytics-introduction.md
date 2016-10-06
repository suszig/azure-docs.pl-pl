<properties 
    pageTitle="Wprowadzenie do usługi Stream Analytics | Microsoft Azure" 
    description="Poznaj usługę Stream Analytics — zarządzaną usługę, która pomaga analizować dane przesyłane strumieniowo z Internetu rzeczy (IoT) w czasie rzeczywistym." 
    keywords="analiza jako usługa, usługi zarządzane, przetwarzanie strumienia, stream analytics, co to jest stream analytics"
    services="stream-analytics" 
    documentationCenter="" 
    authors="jeffstokes72" 
    manager="jhubbard" 
    editor="cgronlun"/>

<tags 
    ms.service="stream-analytics" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.tgt_pltfrm="na" 
    ms.workload="data-services" 
    ms.date="09/26/2016" 
    ms.author="jeffstok"/>



# Co to jest usługa Stream Analytics?

Usługa Azure Stream Analytics to w pełni zarządzany i niedrogi aparat przetwarzania zdarzeń w czasie rzeczywistym, który ułatwia uzyskiwanie szczegółowych informacji z danych. Usługa Stream Analytics umożliwia łatwe skonfigurowanie obliczeń analitycznych wykonywanych w czasie rzeczywistym dla danych przesyłanych strumieniowo z urządzeń, czujników, witryn sieci Web, mediów społecznościowych, aplikacji, systemów infrastruktury i innych źródeł.

Wystarczy kilka kliknięć w witrynie Azure Portal, aby utworzyć zadanie usługi Stream Analytics określające źródło danych wejściowych przesyłanych strumieniowo, ujście danych wyjściowych dla wyników zadania oraz transformację danych w języku przypominającym język SQL. Skalę i szybkość zadania możesz monitorować i dopasowywać w witrynie Azure Portal, aby określić zdarzenia przetwarzane na sekundę (od kilku kilobajtów do gigabajta lub więcej).

Usługa Stream Analytics korzysta z wieloletnich badań firmy Microsoft w zakresie opracowywania wysoce dostosowanych aparatów przesyłania strumieniowego na potrzeby szybkiego przetwarzania danych oraz integracji języków umożliwiającej ich intuicyjne określanie.

## Do czego można używać usługi Stream Analytics?
W dzisiejszych czasach ogromne ilości danych z dużą prędkością przepływają przez sieć. Organizacje, które są w stanie przetwarzać dane przesyłane strumieniowo i korzystać z nich w czasie rzeczywistym mogą znacząco poprawić wydajność i wyróżnić się na rynku. Scenariusze analizy w czasie rzeczywistym danych przesyłanych strumieniowo można znaleźć we wszystkich branżach: spersonalizowane, prowadzone w czasie rzeczywistym analizy danych giełdowych i alerty oferowane przez firmy świadczące usługi finansowe, wykrywanie oszustw w czasie rzeczywistym, usługi ochrony danych i tożsamości, niezawodne pozyskiwanie i analiza danych wygenerowanych przez czujniki i siłowniki wbudowane w obiekty fizyczne (Internet rzeczy, IoT), analiza strumienia kliknięć w sieci Web, a także aplikacje do zarządzania relacjami z klientami (CRM, customer relationship management) wysyłające alerty, jeśli poziom zadowolenia klienta w przedziale czasu ulega pogorszeniu. Firmy szukają najbardziej elastycznego, niezawodnego i ekonomicznego sposobu przeprowadzania takich analiz danych strumienia zdarzeń w czasie rzeczywistym, aby odnieść sukces w świecie wysoce konkurencyjnych nowoczesnych firm.

## Najważniejsze funkcje i korzyści
-   **Łatwość obsługi:** usługa Stream Analytics obsługuje prosty, deklaratywny model zapytań na potrzeby opisywania przekształceń. W celu optymalizacji pod kątem ułatwienia obsługi usługa Stream Analytics korzysta z wariantu języka T-SQL i eliminuje potrzebę obsługi przez klientów technicznej złożoności systemów przetwarzania strumienia. Używając [języka zapytań usługi Stream Analytics](https://msdn.microsoft.com/library/azure/dn834998.aspx) w edytorze zapytań przeglądarki, możesz korzystać z automatycznego uzupełniania za pomocą funkcji IntelliSense, które pomaga szybko i łatwo zaimplementować zapytania dla szeregów czasowych, łącznie ze sprzężeniami czasowymi, agregacjami w oknach, filtrami czasowymi i innymi typowymi operacjami: sprzężeniami, agregacjami, projekcjami, filtrami itp. Ponadto testowanie zapytań w przeglądarce na przykładowym pliku danych umożliwia szybkie programowanie przyrostowe.  

-   **Skalowalność:** usługa Stream Analytics jest w stanie obsługiwać zdarzenia z wysoką przepływnością, do 1 GB na sekundę. Integracja z usługami [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) i [Azure IoT Hubs](https://azure.microsoft.com/services/iot-hub/) pozwala rozwiązaniu pozyskiwać miliony zdarzeń na sekundę przychodzących z połączonych urządzeń, strumieni kliknięć, plików dziennika i innych źródeł. Aby to osiągnąć, usługa Stream Analytics wykorzystuje funkcję partycjonowania usługi Event Hubs, która umożliwia uzyskanie 1 MB/s dla każdej partycji. Użytkownicy mogą partycjonować obliczenia na kilka logicznych kroków w ramach definicji zapytania, z których każdy może zostać dalej partycjonowany w celu zwiększenia skalowalności.  

-   **Niezawodność, powtarzalność i szybkie odzyskiwanie:** zarządzana usługa w chmurze Stream Analytics pomaga uniknąć utraty danych i zapewnia ciągłość działania firmy w razie awarii za pomocą wbudowanych funkcji odzyskiwania. Dzięki możliwości wewnętrznego przechowywania stanu usługa zapewnia powtarzalne wyniki, co gwarantuje możliwość archiwizacji zdarzeń i ponownego stosowania przetwarzania w przyszłości, zawsze uzyskując takie same wyniki. Umożliwia to klientom przechodzenie wstecz w czasie i badanie obliczeń podczas ustalania głównej przyczyny problemu, analizy warunkowej itp.  

-   **Niski koszt:** jako usługa w chmurze usługa Stream Analytics jest zoptymalizowana pod kątem zapewniania użytkownikom bardzo niskich kosztów rozpoczęcia pracy i obsługi rozwiązania do analiz w czasie rzeczywistym. Usługę zaprojektowano pod kątem płatności zgodnie z rzeczywistym użyciem jednostek przesyłania strumieniowego i ilości danych przetwarzanych przez system. Użycie jest określane na podstawie ilości przetworzonych zdarzeń i ilości mocy obliczeniowej udostępnionej w klastrze do obsługi odpowiednich zadań usługi Stream Analytics.  

-   **Dane referencyjne:** usługa Stream Analytics zapewnia użytkownikom możliwość określenia i używania danych referencyjnych. Mogą to być dane historyczne lub po prostu dane nieprzesyłane strumieniowo, które zmieniają się rzadziej. System upraszcza korzystanie z danych referencyjnych przez traktowanie ich jak każdego innego przychodzącego strumienia zdarzeń w celu dołączenia do innych strumieni zdarzeń pozyskanych w czasie rzeczywistym i wykonania przekształcenia.  

-   **Funkcje zdefiniowane przez użytkownika:** usługa Stream Analytics jest zintegrowana z usługą Azure Machine Learning, aby definiować wywołania funkcji w usłudze Machine Learning jako część zapytania usługi Stream Analytics. To powoduje rozszerzenie możliwości usługi Stream Analytics w zakresie używania istniejących rozwiązań usługi Azure Machine Learning. Aby uzyskać więcej informacji na ten temat, zapoznaj się z [Samouczkiem integracji usługi Machine Learning](stream-analytics-machine-learning-integration-tutorial.md).

-   **Łączność:** usługa Stream Analytics łączy się bezpośrednio z usługami Azure Event Hubs i Azure IoT Hubs Azure w celu pozyskiwania strumieni oraz z usługą Azure Blob Service w celu pozyskiwania danych historycznych. Wyniki mogą być zapisywane z usługi Stream Analytics do usług Azure Storage Blobs lub Tables, AzureSQL DB, Azure Data Lake Store, DocumentDB, Event Hubs, tematów lub kolejek usługi Azure Service Bus i usługi Power BI, gdzie mogą być następnie wizualizowane, dalej przetwarzane przez przepływy pracy, wykorzystywane w analizach wsadowych za pomocą usługi [Azure HDInsight](https://azure.microsoft.com/services/hdinsight/) lub przetwarzane ponownie jako szereg zdarzeń. W przypadku korzystania z usługi Event Hubs istnieje możliwość łączenia wielu usług Stream Analytics z innymi źródłami danych i aparatami przetwarzania bez utraty strumieniowej natury obliczeń.  

## Uzyskiwanie pomocy
Aby uzyskać dalszą pomoc, skorzystaj z naszego [forum usługi Azure Stream Analytics](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).

## Następne kroki
W niniejszym artykule przedstawiono usługę Stream Analytics — zarządzaną usługę służącą do analizy danych przesyłanych strumieniowo z Internetu rzeczy. Aby dowiedzieć się więcej na temat tej usługi, zobacz:

- [Get started using Azure Stream Analytics (Rozpoczynanie pracy z usługą Azure Stream Analytics)](stream-analytics-get-started.md)
- [Scale Azure Stream Analytics jobs (Skalowanie zadań usługi Azure Stream Analytics)](stream-analytics-scale-jobs.md)
- [Azure Stream Analytics Query Language Reference (Dokumentacja dotycząca języka zapytań usługi Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn834998.aspx)
- [Azure Stream Analytics Management REST API Reference (Dokumentacja interfejsu API REST zarządzania usługą Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835031.aspx)




<!--HONumber=Sep16_HO4-->


