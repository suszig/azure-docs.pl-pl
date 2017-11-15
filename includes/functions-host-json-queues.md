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
|batchSize|16|Liczba wiadomości w kolejce do pobrania i przetwarzanie równoległe. Wartość maksymalna to 32.| 
|maxDequeueCount|5|Liczba prób przetwarzania przed jego przeniesieniem do skażone kolejki wiadomości.| 
|newBatchThreshold|batchSize/2|Próg pobrane nową partię komunikatów.| 
