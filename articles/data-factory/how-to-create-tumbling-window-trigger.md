---
title: "Jak utworzyć wyzwalaczy okno wirowania w fabryce danych Azure | Dokumentacja firmy Microsoft"
description: "Dowiedz się, jak utworzyć wyzwalacza w fabryce danych Azure, uruchamianego potoku na okno wirowania."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: 
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: shlo
ms.openlocfilehash: a3b056ae4bb4eda26fec58ca3b6bed7f0744e36e
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: pl-PL
ms.lasthandoff: 01/05/2018
---
# <a name="how-to-create-a-trigger-that-runs-a-pipeline-on-a-tumbling-window"></a>Jak utworzyć wyzwalacz, który działa na okno wirowania potoku
Ten artykuł zawiera kroki, aby utworzyć, uruchomić i monitorować wyzwalacz okno wirowania. Aby uzyskać ogólne informacje na temat wyzwalaczy i typy obsługujemy, zobacz [potoku wykonywania i wyzwalaczy](concepts-pipeline-execution-triggers.md).

> [!NOTE]
> Ten artykuł dotyczy wersji 2 usługi Data Factory, która jest obecnie dostępna w wersji zapoznawczej. Jeśli używasz dostępnej ogólnie wersji 1 usługi Data Factory, zobacz artykuł z [wprowadzeniem do usługi Data Factory w wersji 1](v1/data-factory-copy-data-from-azure-blob-storage-to-sql-database.md).

Wyzwalacze okno wirowania są typu wyzwalacza, z którego generowane w odstępach czasu okresowe z określonym czasie rozpoczęcia, zachowując stanu. Windows wirowania są serii o stałym rozmiarze, nienakładających i ciągłe interwałów. Wyzwalacz okno wirowania ma relację 1:1 z potokiem i może się odwoływać tylko do pojedynczej potoku.

## <a name="tumbling-window-trigger-type-properties"></a>Właściwości typu wyzwalacza okno wirowania
```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
        "runtimeState": "<<Started/Stopped/Disabled - readonly>>",
        "typeProperties": {
            "frequency": "<<Minute/Hour>>",
            "interval": <<int>>,
            "startTime": "<<datetime>>",
            "endTime: "<<datetime – optional>>"",
            "delay": "<<timespan – optional>>",
            “maxConcurrency”: <<int>> (required, max allowed: 50),
            "retryPolicy": {
                "count":  <<int - optional, default: 0>>,
                “intervalInSeconds”: <<int>>,
            }
        },
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "parameter1": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter2": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "parameter3": "https://mydemo.azurewebsites.net/api/demoapi"
                }
            }
      }    
}
```  

Poniższa tabela zawiera omówienie głównych elementów odnoszące się do ponownego i planowania w wyzwalaczu okno wirowania.

| **Nazwy JSON** | **Opis** | **Dozwolone wartości** | **Wymagane** |
|:--- |:--- |:--- |:--- |
| **Typ** | Typ wyzwalacza. To jest określona jako "TumblingWindowTrigger." | Ciąg | Yes |
| **runtimeState** | <readOnly>Możliwe wartości: Uruchomiona, zatrzymana, wyłączone | Ciąg | Tak, tylko do odczytu |
| **częstotliwość** |*Częstotliwość* ciąg reprezentujący jednostkę częstotliwość powtarzania wyzwalacza. Obsługiwane wartości to "min" i "Godzina". Jeśli czas rozpoczęcia części daty, które są bardziej szczegółowego niż wartość częstotliwości, będzie można brana pod uwagę do obliczenia granice okna. Dla ex: Jeśli częstotliwość wynosi co godzinę i czas rozpoczęcia jest 2016-04-01T10:10:10Z, pierwsze okno jest (2017-09-01T10:10:10Z, 2017-09 — 01T11:10:10Z.)  | Ciąg. Obsługiwane typy "minute", "Godzina" | Yes |
| **Interwał** |*Interwał* jest dodatnią liczbą całkowitą i określa interwał *częstotliwość* określający, jak często będą uruchamiane wyzwalacza. Na przykład jeśli *interwał* 3 i *częstotliwość* to "Godzina" wyzwalacza wystąpi co 3 godziny. | Liczba całkowita | Yes |
| **czas rozpoczęcia**|*wartość startTime* jest daty i godziny. *wartość startTime* jest pierwsze wystąpienie i może przypadać w przeszłości. Pierwszy interwał wyzwalacza będzie (czas rozpoczęcia, czas rozpoczęcia + interwał). | Data/godzina | Yes |
| **wartość endTime**|*wartość endTime* jest daty i godziny. *wartość endTime* jest ostatnim wystąpieniem a może przypadać w przeszłości. | Data/godzina | Yes |
| **Opóźnienie** | Określ opóźnienie przetwarzania danych powoduje uruchomienie okna. Uruchom potoku jest uruchomione po oczekiwany czas wykonywania + opóźnienia. Opóźnienie definiuje sposób czas oczekiwania wyzwalacza zaległe czasie przed wyzwalania, Uruchom nowe. Nie zmienia godzina rozpoczęcia okna. | Wartość TimeSpan (przykład: 00:10:00 oznacza opóźnienia w ciągu 10 minut) |  Nie. Domyślna to "00: 00:00" |
| **MAX współbieżności** | Liczba uruchomień jednoczesnych wyzwalacza, które są uruchamiane dla systemu windows, które są gotowe. Przykład: Jeśli próbujemy uzupełniania co godzinę na wczoraj, który będzie 24 systemu windows. Jeśli współbieżności = 10, wyzwalacz zdarzenia są generowane tylko dla systemu windows 10 pierwszych (00:00-01:00 - 09:00-10:00). Po zakończeniu 10 pierwszych uruchamia wyzwalanych potoku uruchamia wyzwalacz są uruchamiane dla następnego 10 (10:00-11:00-19:00-20:00). Kontynuowanie przykład współbieżności = 10, jeśli istnieją 10 systemu windows, które są gotowe, będzie 10 uruchamia potoku. Jeśli istnieje tylko 1 okno gotowe, będzie istnieć tylko 1 wykonywania potoku. | Liczba całkowita | Tak. Możliwe wartości 1 – 50 |
| **retryPolicy: liczba** | Liczba ponownych prób przed uruchomieniem potoku jest oznaczony jako "Niepowodzenie"  | Liczba całkowita |  Nie. Domyślna to 0 ponownych prób. |
| **retryPolicy: element intervalInSeconds** | Opóźnienie między ponownymi próbami w sekundach | Liczba całkowita |  Nie. Domyślna to 30 sekund. |

### <a name="using-system-variables-windowstart-and-windowend"></a>Za pomocą zmiennych systemowych: WindowStart i WindowEnd

Jeśli chcesz użyć WindowStart i WindowEnd wyzwalacz okno wirowania w Twojej **potoku** definicji (tj. w przypadku części zapytania), należy przekazać zmienne jako parametry do potoku sieci w **wyzwalacza**definicji, w następujący sposób:
```  
{
    "name": "MyTriggerName",
    "properties": {
        "type": "TumblingWindowTrigger",
            ...
        "pipeline":
            {
                "pipelineReference": {
                    "type": "PipelineReference",
                    "referenceName": "MyPipelineName"
                },
                "parameters": {
                    "MyWindowStart": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowStartTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    },
                    "MyWindowEnd": {
                        "type": "Expression",
                        "value": "@{concat('output',formatDateTime(trigger().outputs.windowEndTime,'-dd-MM-yyyy-HH-mm-ss-ffff'))}"
                    }
                }
            }
      }    
}
```  

Następnie, w definicji potoku, aby użyć wartości WindowStart i WindowEnd, użyj parametry odpowiednio "MyWindowStart" i "MyWindowEnd"

### <a name="notes-on-backfill"></a>Uwagi dotyczące uzupełniania
W przypadku wielu systemu windows do wykonywania (esp. w scenariuszu uzupełniania) kolejność wykonywania systemu windows jest deterministyczna i będzie od najstarszych do najnowszych odstępach czasu. Nie istnieje sposób zmienić to zachowanie od tej chwili.

### <a name="updating-an-existing-triggerresource"></a>Aktualizowanie istniejących TriggerResource
* Zmiana częstotliwości (lub rozmiar okna) wyzwalacza stan systemu windows już przetworzone będzie *nie* zresetowane. Wyzwalacz będzie uruchamiania dla systemu windows z ostatnią wykonane, używając nowego rozmiaru okna.
* Godzina zakończenia wyzwalacza zmian (dodano lub zaktualizowano), stan systemu windows już przetwarzanie będzie *nie* zresetowane. Wyzwalacz honoruje po prostu nową godzinę zakończenia. Jeśli godzina zakończenia jest przed systemu windows już wykonane, wyzwalacz zostanie zatrzymane. W przeciwnym razie zostanie zatrzymane, gdy napotkano nową godzinę zakończenia.

## <a name="sample-using-azure-powershell"></a>Przykładowe przy użyciu programu Azure PowerShell
W tej sekcji przedstawiono sposób tworzenia, uruchomić i monitorować wyzwalacz przy użyciu programu Azure PowerShell.

1. Utwórz plik JSON o nazwie MyTrigger.json w folderze C:\ADFv2QuickStartPSH\ o następującej treści:

   > [!IMPORTANT]
   > Ustaw **startTime** bieżący czas UTC i **endTime** aby godzinę poza bieżący czas UTC razem przed zapisaniem pliku JSON.

    ```json   
    {
      "name": "PerfTWTrigger",
      "properties": {
        "type": "TumblingWindowTrigger",
        "typeProperties": {
          "frequency": "Minute",
          "interval": "15",
          "startTime": "2017-09-08T05:30:00Z",
          "delay": "00:00:01",
          "retryPolicy": {
            "count": 2,
            "intervalInSeconds": 30
          },
          "maxConcurrency": 50
        },
        "pipeline": {
          "pipelineReference": {
            "type": "PipelineReference",
            "referenceName": "DynamicsToBlobPerfPipeline"
          },
          "parameters": {
            "windowStart": "@trigger().outputs.windowStartTime",
            "windowEnd": "@trigger().outputs.windowEndTime"
          }
        },
        "runtimeState": "Started"
      }
    }
    ```  
2. Utwórz wyzwalacz za pomocą **AzureRmDataFactoryV2Trigger zestaw** polecenia cmdlet.

    ```powershell
    Set-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger" -DefinitionFile "C:\ADFv2QuickStartPSH\MyTrigger.json"
    
3. Confirm that the status of the trigger is **Stopped** by using the **Get-AzureRmDataFactoryV2Trigger** cmdlet.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
4. Uruchomić wyzwalacz za pomocą **Start AzureRmDataFactoryV2Trigger** polecenia cmdlet:

    ```powershell
    Start-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
5. Upewnij się, że jest w stanie wyzwalacza **uruchomiono** za pomocą **Get-AzureRmDataFactoryV2Trigger** polecenia cmdlet.

    ```powershell
    Get-AzureRmDataFactoryV2Trigger -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -Name "MyTrigger"
    ```
6.  Pobierz uruchamia wyzwalacz za pomocą programu PowerShell przy użyciu **Get-AzureRmDataFactoryV2TriggerRun** polecenia cmdlet. Aby uzyskać informacje na temat uruchamia wyzwalacz, uruchom następujące polecenie okresowo: aktualizacja **TriggerRunStartedAfter** i **TriggerRunStartedBefore** wartości są zgodne z wartościami w definicji wyzwalacza .

    ```powershell
    Get-AzureRmDataFactoryV2TriggerRun -ResourceGroupName $ResourceGroupName -DataFactoryName $DataFactoryName -TriggerName "MyTrigger" -TriggerRunStartedAfter "2017-12-08T00:00:00" -TriggerRunStartedBefore "2017-12-08T01:00:00"
    ```
Aby monitorować wyzwalacz uruchamia/potoku działa w portalu Azure, zobacz [uruchamia Monitor potoku](quickstart-create-data-factory-resource-manager-template.md#monitor-the-pipeline)

## <a name="next-steps"></a>Kolejne kroki
Aby uzyskać szczegółowe informacje dotyczące wyzwalaczy, zobacz [potoku wykonywania i wyzwalaczy](concepts-pipeline-execution-triggers.md#triggers).
