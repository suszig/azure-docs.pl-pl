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
|maxOutstandingRequests|-1|Maksymalna liczba oczekujących żądań, które będą odbywać się w danym momencie (-1 oznacza niepowiązanego). Limit obejmuje żądań, które są umieszczane w kolejce, ale nie rozpoczęły wykonywania, a także wszelkie wykonaniami w toku. Wszystkie przychodzące przekracza ten limit będą odrzucane 429 odpowiedzi "Jest zbyt zajęty". Obiekty wywołujące można użyć tej odpowiedzi fragmentów strategii oparte na czasie ponów próbę. To ustawienie określa tylko kolejkowania, który występuje w ścieżce wykonanie zadania hosta. Przez to ustawienie nie dotyczy to innych kolejek, takich jak kolejki żądania ASP.NET. |
|maxConcurrentRequests|-1|Maksymalna liczba funkcje HTTP, które będą wykonywane równolegle (-1 oznacza niepowiązanego). Na przykład można ustawić limit użycie funkcji HTTP zbyt wiele zasobów systemowych podczas współbieżności jest wysoka. Lub jeśli funkcji wychodzących żądań do usługi innej firmy, wywołań może wymagać szybkość limited.|
|dynamicThrottlesEnabled|fałsz|Powoduje, że żądanie przetwarzania potoku okresowo sprawdzać, liczniki wydajności systemu. Liczniki zawierają połączeń, wątki procesów, pamięci i procesora cpu. Jeśli dowolny z liczników jest powyżej progu wbudowanych (80%), będą odrzucane 429 odpowiedzi "Jest zbyt zajęty" do momentu counter(s) Powrót do normalnego poziomu.|
