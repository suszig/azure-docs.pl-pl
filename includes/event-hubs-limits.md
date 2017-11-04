W poniższej tabeli wymieniono przydziały i limity specyficzne dla [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/). Aby uzyskać informacje o cenach usługi Event Hubs, zobacz [cennik usługi Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/).

| Limit | Zakres | Typ | Zachowanie po przekroczeniu | Wartość |
| --- | --- | --- | --- | --- |
| Liczba centrów zdarzeń na przestrzeń nazw |przestrzeń nazw |Statyczny |Kolejne żądania w celu utworzenia nowego Centrum zdarzeń zostaną odrzucone. |10 |
| Liczba partycji na Centrum zdarzeń |Jednostka |Statyczny |- |32 |
| Liczba grup odbiorców, na Centrum zdarzeń |Jednostka |Statyczny |- |20 |
| Liczba połączeń protokołu AMQP na przestrzeń nazw |przestrzeń nazw |Statyczny |Kolejne żądania dotyczące dodatkowych połączeń zostaną odrzucone i wyjątek jest odbierany przez kod wywołujący. |5,000 |
| Maksymalny rozmiar zdarzenia usługi Event Hubs|Systemowe |Statyczny |- |256 KB |
| Maksymalny rozmiar nazwy Centrum zdarzeń |Jednostka |Statyczny |- |50 znaków |
| Liczba innych niż epoka odbiorcy dla każdej grupy odbiorców |Jednostka |Statyczny |- |5 |
| Maksymalny okres przechowywania danych zdarzenia |Jednostka |Statyczny |- |1-7 dni |
| Maksymalna liczba jednostek przepływności |przestrzeń nazw |Statyczny |Przekroczenie limitu jednostki przepływności powoduje, że dane, aby zdławionych i generuje  **[ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception)**. Możesz poprosić o większej liczby jednostek przepływności dla standardowej warstwy przy zgłoszenia [żądania obsługi](/azure/azure-supportability/how-to-create-azure-support-request). [Jednostki przepływności dodatkowe](../articles/event-hubs/event-hubs-auto-inflate.md) są dostępne w blokach po 20 na podstawie zakupu zatwierdzone. |20 |
| Liczba reguł autoryzacji na przestrzeń nazw |przestrzeń nazw|Statyczny |Kolejne żądania tworzenia reguły autoryzacji zostaną odrzucone.|12 |
