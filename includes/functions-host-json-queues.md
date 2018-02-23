```json
{
    "queues": {
      "maxPollingInterval": 2000,
      "visibilityTimeout" : "00:00:30",
      "batchSize": 16,
      "maxDequeueCount": 5,
      "newBatchThreshold": 8
    }
}
```

|Właściwość  |Domyślne | Opis |
|---------|---------|---------| 
|maxPollingInterval|60000|Maksymalny interwał w milisekundach między sond kolejki.| 
|visibilityTimeout|0|Przedział czasu między kolejnymi próbami podczas przetwarzania komunikatu nie powiedzie się.| 
|batchSize|16|Liczba wiadomości w kolejce, które środowisko uruchomieniowe Functions jednocześnie pobiera i przetwarza równolegle. Gdy pobiera liczba przetwarzanych w dół do `newBatchThreshold`, środowisko uruchomieniowe pobiera inna partia i rozpoczyna przetwarzanie tych wiadomości. Dlatego maksymalną liczbę równoczesnych komunikatów przetwarzanych dla każdej funkcji `batchSize` plus `newBatchThreshold`. Ten limit dotyczy oddzielnie każdej funkcji wyzwalanych kolejki. <br><br>Jeśli chcesz uniknąć przetwarzania równoległego dla wiadomości otrzymanych w jednej kolejki, można ustawić `batchSize` do 1. Jednak to ustawienie eliminuje współbieżności tylko tak długo, jak działa funkcja aplikacji na jednej maszynie wirtualnej (VM). Jeśli funkcja aplikacji może obsłużyć się wiele maszyn wirtualnych, każda maszyna wirtualna może uruchomić jedno wystąpienie każdej funkcji wyzwalanych kolejki.<br><br>Maksymalna `batchSize` to 32. | 
|maxDequeueCount|5|Liczba prób przetwarzania przed jego przeniesieniem do skażone kolejki wiadomości.| 
|newBatchThreshold|batchSize/2|Zawsze, gdy liczba komunikatów przetwarzanych jednocześnie pobiera w dół do tego numeru, środowisko uruchomieniowe pobiera inna partia.| 



