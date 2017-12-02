```json
{
    "http": {
        "routePrefix": "api",
        "maxOutstandingRequests": 20,
        "maxConcurrentRequests": 10,
        "dynamicThrottlesEnabled": false
    }
}
```

|Właściwość  |Domyślne | Opis |
|---------|---------|---------| 
|Element routePrefix|api|Prefiks trasy, która ma zastosowanie do wszystkich tras. Aby usunąć prefiks domyślny, użyj pustego ciągu. |
|maxOutstandingRequests|-1|Maksymalna liczba oczekujących żądań, które są przechowywane w danym momencie. Ten limit obejmuje żądań, które są umieszczane w kolejce, ale nie rozpoczęły wykonywania, a także w toku wykonania. Wszystkie przychodzące przekracza ten limit będą odrzucane 429 odpowiedzi "Jest zbyt zajęty". Który umożliwia wywoływania fragmentów strategii oparte na czasie ponów próbę i pomaga kontrolować maksymalny żądania opóźnienia. Ta czynność reguluje tylko znajdującego się w ścieżce wykonywania skryptu hosta usługi kolejkowania wiadomości. Inne kolejek, takich jak kolejki żądań ASP.NET będzie nadal i przez to ustawienie nie dotyczy. Wartość domyślna to bez ograniczeń.|
|maxConcurrentRequests|-1|Maksymalna liczba funkcje http, które będą wykonywane równolegle. Dzięki temu do sterowania współbieżnością może ułatwić zarządzanie wykorzystania zasobów. Na przykład może być funkcji http, która używa dużej ilości zasobów systemowych (gniazda pamięci/procesora) tak, aby powoduje problemy współbieżności jest zbyt duża. Może mieć funkcję, która sprawia, że żądania wychodzącego osobie trzeciej usługi lub wywołań muszą być ograniczone szybkość. W takich przypadkach stosowania przepustnicy w tym miejscu może pomóc. Wartość domyślna to bez ograniczeń.|
|dynamicThrottlesEnabled|fałsz|Gdy włączone, będzie to ustawienie powoduje potoku przetwarzania żądań okresowego sprawdzania wydajności systemu liczników, takich jak połączenia/wątki/procesów/pamięci/procesora cpu/itp. oraz żądania, jeśli dowolny z tych liczników jest powyżej wartości progowej wysokiej wbudowanych (80%) odrzucone 429 odpowiedzi "Jest zbyt zajęty", dopóki counter(s) Powrót do normalnego poziomu.|
