```json
{
    "serviceBus": {
      "maxConcurrentCalls": 16,
      "prefetchCount": 100,
      "autoRenewTimeout": "00:05:00"
    }
}
```

|Właściwość  |Domyślne | Opis |
|---------|---------|---------| 
|maxConcurrentCalls|16|Maksymalna liczba równoczesnych wywołań wywołanie zwrotne, które należy zainicjować przekazywanie komunikatów. Domyślnie środowisko uruchomieniowe Functions przetwarza wiele komunikatów jednocześnie. Aby skierować środowiska uruchomieniowego do jednoczesnego przetwarzania tylko pojedynczej kolejki lub tematu wiadomości, ustaw `maxConcurrentCalls` do 1. | 
|prefetchCount|Brak|Wartość domyślna PrefetchCount używanego przez MessageReceiver podstawowej.| 
|autoRenewTimeout|00:05:00|Maksymalny czas trwania, w którym blokady komunikat będzie odnawiana automatycznie.| 
