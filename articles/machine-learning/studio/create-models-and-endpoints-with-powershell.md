---
title: "Tworzenie wielu modeli z doświadczenia | Dokumentacja firmy Microsoft"
description: "Tworzenie wielu modeli uczenia maszynowego i sieci web punktów końcowych usługi przy użyciu tego samego algorytmu, ale szkolenia różnych zestawów danych za pomocą programu PowerShell."
services: machine-learning
documentationcenter: 
author: hning86
manager: jhubbard
editor: cgronlun
ms.assetid: 1076b8eb-5a0d-4ac5-8601-8654d9be229f
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: garye;haining
ms.openlocfilehash: cc938fdaa6843f7c9e974d9b88a9b682b4678493
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 10/11/2017
---
# <a name="create-many-machine-learning-models-and-web-service-endpoints-from-one-experiment-using-powershell"></a>Tworzenie wielu modeli usługi Machine Learning i punktów końcowych usługi sieci Web na podstawie jednego eksperymentu przy użyciu programu PowerShell
W tym miejscu jest to powszechny problem learning maszyny: Aby utworzyć wiele modeli, które ma tego samego przepływu pracy szkolenia i korzystać z tego samego algorytmu, ale zestaw danych szkoleniowych różnych jako dane wejściowe. W tym artykule opisano w tym celu na dużą skalę w usłudze Azure Machine Learning Studio za pomocą tylko jednego eksperymentu.

Załóżmy na przykład, że właścicielem firmy franchisingowe roweru globalnego dzierżawy. Chcesz skompilować modelu regresji do prognozowania żądanie dzierżawy na podstawie danych historycznych. Masz 1000 lokalizacje wynajem całym świecie i zostały zebrane zestawu danych dla każdej lokalizacji, która zawiera ważne funkcje, takie jak dzień, godzina pogodą i ruchu, które są specyficzne dla każdej lokalizacji.

Można uczenia modelu raz przy użyciu wersji scalonych wszystkie zestawy danych we wszystkich lokalizacjach. Jednak ponieważ każdy z lokalizacji ma unikatowy środowiska, lepszym rozwiązaniem byłoby do uczenia modelu regresji oddzielnie dla każdej lokalizacji za pomocą zestawu danych. W ten sposób każdy uczonego modelu można wziąć pod uwagę magazynu różne rozmiary, woluminu, geography, populacji, środowisko ruchu roweru przyjaznego *itp.*.

Może to być najlepszym rozwiązaniem, ale nie chcesz utworzyć 1000 szkolenia eksperymentów w usłudze Azure Machine Learning z każdą z nich reprezentujący unikatową lokalizację. Oprócz jest utrudnione zadań, należy również wydaje się bardzo mało wydajne, ponieważ każdego doświadczenia byłyby same składników z wyjątkiem zestawu danych szkoleniowych.

Na szczęście możemy można to zrobić za pomocą [ponownego trenowania interfejsu API uczenia maszynowego Azure](retrain-models-programmatically.md) i automatyzację zadań z [programu PowerShell usługi Azure Machine Learning](powershell-module.md).

> [!NOTE]
> Aby naszej próbki szybsze, firma Microsoft będzie Zmniejsz liczbę lokalizacji, z 1 000 do 10. Jednak z zasadami i procedurami stosować do 1000 lokalizacji. Jedyna różnica polega na tym, jeśli chcesz uczenia z 1000 zestawów danych prawdopodobnie chcesz wziąć pod uwagę uruchomionych poniższych skryptów programu PowerShell równolegle. Jak to zrobić, wykracza poza zakres tego artykułu, ale można znaleźć przykłady PowerShell wielowątkowości w Internecie.  
> 
> 

## <a name="set-up-the-training-experiment"></a>Konfigurowanie eksperyment uczenia
Chcemy używać przykład [eksperyment uczenia](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Training-Experiment-1) który mamy już utworzone w [Cortana Intelligence Gallery](http://gallery.cortanaintelligence.com). Otwórz tego eksperymentu w Twojej [Azure Machine Learning Studio](https://studio.azureml.net) obszaru roboczego.

> [!NOTE]
> Aby można było skorzystać z tego przykładu, można użyć standardowego obszaru roboczego, a nie wolnego obszaru roboczego. Możemy utworzyć jeden punkt końcowy dla każdego klienta — dla wszystkich punktów końcowych 10 - i standardowe obszaru roboczego będzie wymagają od wolnego obszaru roboczego jest ograniczona do 3 punktów końcowych. Jeśli masz tylko wolnego obszaru roboczego, po prostu zmodyfikuj skryptów poniżej, aby umożliwić tylko 3 lokalizacji.
> 
> 

Używa eksperyment **Import danych** modułu, aby zaimportować zestaw danych szkoleniowych *customer001.csv* z kontem magazynu platformy Azure. Załóżmy, że firma Microsoft ma zbierane szkolenia zestawów danych z wszystkich lokalizacji wynajem roweru i zapisał je w tej samej lokalizacji magazynu obiektów blob z nazwami plików od *rentalloc001.csv* do *rentalloc10.csv*.

![Obraz](./media/create-models-and-endpoints-with-powershell/reader-module.png)

Należy pamiętać, że **wyjście usługi sieci Web** moduł został dodany do **Train Model** modułu.
Po wdrożeniu tego eksperymentu jako usługę sieci web punktu końcowego skojarzone z danych wyjściowych będzie zwracać uczonego modelu w formacie pliku .ilearner.

Należy także zauważyć, że skonfigurowanie parametr usługi sieci web dla adresu URL który **i zaimportuj dane** korzysta z modułu. Pozwala to parametr umożliwia określenie poszczególnych szkolenia zestawów danych, do nauczenia modelu, dla każdej lokalizacji.
Istnieją inne sposoby firma Microsoft może mieć zostało to zrobione, takich jak za pomocą zapytania SQL z parametrem usługi sieci web można pobrać danych z bazy danych SQL Azure lub po prostu **wprowadzania usługi sieci Web** modułu do przekazania w zestawie danych do usługi sieci web.

![Obraz](./media/create-models-and-endpoints-with-powershell/web-service-output.png)

Teraz załóżmy Uruchom ten eksperyment uczenia przy użyciu wartości domyślnej *rental001.csv* jako zestawu danych szkoleniowych. Podczas przeglądania danych wyjściowych **Evaluate** modułu (kliknij wyjściowy i wybierz **wizualizacja**), zostanie wyświetlony, uzyskujemy zadowalający wydajność *AUC* = 0,91. W tym momencie już wszystko gotowe do wdrożenia usługi sieci web poza tym eksperyment uczenia.

## <a name="deploy-the-training-and-scoring-web-services"></a>Wdrażanie szkolenia i oceniania usług sieci web
Aby wdrożyć usługę sieci web szkolenia, kliknij przycisk **ustawić usługę sieci Web** poniżej obszaru roboczego eksperymentu i wybrać **wdrażanie usługi sieci Web**. Wywołania tej usługi sieci web "" roweru wynajem szkolenia".

Teraz należy wdrożyć oceniania usługi sieci web.
Aby to zrobić, możemy kliknij **ustawić usługę sieci Web** poniżej kanwy i wybierz **predykcyjnej usługi sieci Web**. Spowoduje to utworzenie oceniania eksperymentu.
Musimy dostosować kilka pomocnicza do własnych preferencji jako usługę sieci web, takich jak usuwanie kolumn etykiety "cnt" w danych wejściowych i ograniczanie dane wyjściowe do identyfikatora wystąpienia i odpowiadający mu przewidzieć wartość.

Aby zaoszczędzić pracy, możesz po prostu otworzyć [eksperyment predykcyjny](https://gallery.cortanaintelligence.com/Experiment/Bike-Rental-Predicative-Experiment-1) w galerii jest już przygotowane.

Aby wdrożyć usługę sieci web, uruchom eksperyment predykcyjny, a następnie kliknij przycisk **wdrażanie usługi sieci Web** znajdujący się poniżej obszaru roboczego. Oceniania usługi sieci web "Roweru oceniania dzierżawa" name ".

## <a name="create-10-identical-web-service-endpoints-with-powershell"></a>Tworzenie punktów końcowych usługi sieci web identyczne 10 przy użyciu programu PowerShell
Ta usługa sieci web jest dostarczany z domyślnego punktu końcowego. Ale nie jesteśmy zainteresowani jako domyślny punkt końcowy, ponieważ nie można zaktualizować. Co należy zrobić jest utworzenie 10 dodatkowych punktów końcowych, po jednej dla każdej lokalizacji. Firma Microsoft to zrobić przy użyciu programu PowerShell.

Po pierwsze skonfigurowanie naszego środowiska PowerShell:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and is properly set to point to the valid Workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

Następnie uruchom następujące polecenie programu PowerShell:

    # Create 10 endpoints on the scoring web service.
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpointName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

Teraz utworzyliśmy 10 punktów końcowych i wszystkie one zawierać tej samej trenowanego modelu uczenia na *customer001.csv*. Można je wyświetlić w portalu zarządzania Azure.

![Obraz](./media/create-models-and-endpoints-with-powershell/created-endpoints.png)

## <a name="update-the-endpoints-to-use-separate-training-datasets-using-powershell"></a>Zaktualizuj punktów końcowych do użycia osobnych szkolenia zestawów danych przy użyciu programu PowerShell
Następnym krokiem jest aby zaktualizować punkty końcowe z modelami jednoznacznie ćwiczenie poszczególnych danych każdego klienta. Ale najpierw należy utworzyć te modele z **roweru wynajem szkolenia** usługi sieci web. Wróć do **roweru wynajem szkolenia** usługi sieci web. Należy wywołać punktu końcowego usługi BES 10 razy z 10 szkolenia różnych zestawów danych w celu tworzenia modeli 10. Użyjemy **InovkeAmlWebServiceBESEndpoint** polecenia cmdlet programu PowerShell, aby to zrobić.

Należy również podać poświadczenia dla konta magazynu obiektów blob do `$configContent`, to znaczy, pola `AccountName`, `AccountKey` i `RelativeLocation`. `AccountName` Może być jedną z nazwy konta, jak pokazano w **klasycznego portalu zarządzania Azure** (*magazynu* kartę). Po kliknięciu na koncie magazynu, jego `AccountKey` można znaleźć, naciskając klawisz **Zarządzaj kluczami dostępu** znajdujący się u dołu i kopiowanie *podstawowy klucz dostępu*. `RelativeLocation` Jest ścieżką względną wobec miejsca przechowywania nowego modelu. Na przykład ścieżka `hai/retrain/bike_rental/` w skrypcie poniżej punktów do kontenera o nazwie `hai`, i `/retrain/bike_rental/` znajdują się podfoldery. Obecnie nie można utworzyć podfoldery za pośrednictwem interfejsu użytkownika portalu, ale istnieją [kilka eksploratory usługi Storage Azure](../../storage/common/storage-explorers.md) umożliwiające zrobić. Zalecane jest, Utwórz nowy kontener w Twoim magazynie do przechowywania nowe modele przeszkolone (.ilearner pliki) w następujący sposób: na stronie magazynu, kliknij **Dodaj** znajdujący się u dołu i nadaj mu nazwę `retrain`. Podsumowując, niezbędne zmiany w poniższym skrypcie odnoszą się do `AccountName`, `AccountKey` i `RelativeLocation` (:`"retrain/model' + $seq + '.ilearner"`).

    # Invoke the retraining API 10 times
    # This is the default (and the only) endpoint on the training web service
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

> [!NOTE]
> Punkt końcowy usługi BES jest tylko obsługiwany tryb dla tej operacji. Rekordy zasobów nie może służyć do tworzenia modeli przeszkolone.
> 
> 

Jak pokazano powyżej, zamiast tworzenia 10 różnych BES zadania konfiguracji json plików, firma Microsoft dynamicznie zamiast tego utworzyć parametry konfiguracji i źródła danych, aby *jobConfigString* parametr  **InvokeAmlWebServceBESEndpoint** polecenia cmdlet, ponieważ nie istnieje naprawdę nie trzeba przechowywać kopię na dysku.

Jeśli wszystko odbędzie się poprawnie, po chwili powinno być widoczne 10 plików .ilearner, z *model001.ilearner* do *model010.ilearner*, na Twoim koncie magazynu Azure. Teraz już wszystko gotowe do zaktualizowania naszych 10 oceniania punktów końcowych usługi sieci web przy użyciu tych modeli przy użyciu **AmlWebServiceEndpoint poprawki** polecenia cmdlet programu PowerShell. Ponownie należy pamiętać, że firma Microsoft poprawka tylko innych niż domyślne punkty końcowe, które programowo utworzony wcześniej.

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '<my_blob_sas_token>'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }

To powinno być ono uruchomione stosunkowo szybko. Po zakończeniu wykonywania, firma Microsoft będzie pomyślnie utworzono 10 predykcyjnej usługi punktów końcowych sieci web, zawierający uczonego modelu jednoznacznie ćwiczenie określonego zestawu danych do lokalizacji wynajem, wszystko z eksperyment pojedynczego szkolenia. Aby to sprawdzić, możesz spróbować wywoływania tych punktów końcowych przy użyciu **InvokeAmlWebServiceRRSEndpoint** dane wejściowe polecenia cmdlet, zapewnienie im o takim samym i należy oczekiwać zobaczyć wyniki prognozowania inny, ponieważ modele są uczone szkolenie różnych zestawów.

## <a name="full-powershell-script"></a>Pełna skrypt programu PowerShell
Oto lista pełny kod źródłowy:

    Import-Module .\AzureMLPS.dll
    # Assume the default configuration file exists and properly set to point to the valid workspace.
    $scoringSvc = Get-AmlWebService | where Name -eq 'Bike Rental Scoring'
    $trainingSvc = Get-AmlWebService | where Name -eq 'Bike Rental Training'

    # Create 10 endpoints on the scoring web service
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        Write-Host ('adding endpoint ' + $endpontName + '...')
        Add-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -Description $endpointName     
    }

    # Invoke the retraining API 10 times to produce 10 regression models in .ilearner format
    $trainingSvcEp = (Get-AmlWebServiceEndpoint -WebServiceId $trainingSvc.Id)[0];
    $submitJobRequestUrl = $trainingSvcEp.ApiLocation + '/jobs?api-version=2.0';
    $apiKey = $trainingSvcEp.PrimaryKey;
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $inputFileName = 'https://bostonmtc.blob.core.windows.net/hai/retrain/bike_rental/BikeRental' + $seq + '.csv';
        $configContent = '{ "GlobalParameters": { "URI": "' + $inputFileName + '" }, "Outputs": { "output1": { "ConnectionString": "DefaultEndpointsProtocol=https;AccountName=<myaccount>;AccountKey=<mykey>", "RelativeLocation": "hai/retrain/bike_rental/model' + $seq + '.ilearner" } } }';
        Write-Host ('training regression model on ' + $inputFileName + ' for rental location ' + $seq + '...');
        Invoke-AmlWebServiceBESEndpoint -JobConfigString $configContent -SubmitJobRequestUrl $submitJobRequestUrl -ApiKey $apiKey
    }

    # Patch the 10 endpoints with respective .ilearner models
    $baseLoc = 'http://bostonmtc.blob.core.windows.net/'
    $sasToken = '?test'
    For ($i = 1; $i -le 10; $i++){
        $seq = $i.ToString().PadLeft(3, '0');
        $endpointName = 'rentalloc' + $seq;
        $relativeLoc = 'hai/retrain/bike_rental/model' + $seq + '.ilearner';
        Write-Host ('Patching endpoint ' + $endpointName + '...');
        Patch-AmlWebServiceEndpoint -WebServiceId $scoringSvc.Id -EndpointName $endpointName -ResourceName 'Bike Rental [trained model]' -BaseLocation $baseLoc -RelativeLocation $relativeLoc -SasBlobToken $sasToken
    }
