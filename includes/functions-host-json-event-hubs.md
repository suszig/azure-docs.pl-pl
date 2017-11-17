```json
{
    "eventHub": {
      "maxBatchSize": 64,
      "prefetchCount": 256,
      "batchCheckpointFrequency": 1
    }
}
```

|Właściwość  |Domyślne | Opis |
|---------|---------|---------| 
|maxBatchSize|64|Liczba zdarzeń maksymalna, odebranych na pętla odbierania.|
|prefetchCount|Brak|Wartość domyślna PrefetchCount używanego przez EventProcessorHost podstawowej.| 
|batchCheckpointFrequency|1|Liczba zdarzeń wsadów przetwarzana przed utworzeniem punktu kontrolnego EventHub kursora.| 
