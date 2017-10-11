Kolekcja niestandardowych miar. Użyj tej kolekcji do raportu o nazwie miary skojarzone z elementem telemetrii. Typowe zastosowania to:
- Rozmiar ładunku dane telemetryczne zależności
- Liczba elementów w kolejce przetworzone przez żądanie Telemetrii
- czas klienta trwało wykonanie czynności w ukończenia kroku kreatora dane telemetryczne zdarzenia.

Można zbadać [niestandardowych miar](https://analytics.applicationinsights.io/demo?q=H4sIAAAAAAAAA2WLOw6DMAyGd07hZoLeoRPqyMaGGAL8aiPhGCV2kKoeHsHK%2Bj1myyr8LoiaqfrT%2FkUCzRft4LMl8OUeL3LuLLIx%2BxR%2BIF8%2BtcoiNq2o78vgWuFthQaJ1AeGGxt6UlBwKxa1qQ6EpLhAfQAAAA%3D%3D&timespan=PT24H) w analizy aplikacji:

```
customEvents
| where customMeasurements != ""
| summarize avg(todouble(customMeasurements["Completion Time"]) * itemCount)
```

 > [!NOTE]
 > Pomiary niestandardowe są skojarzone z elementem telemetrii, do których należą. Podlegają próbkowania z elementem telemetrii zawierający pomiarów. Aby śledzić miary niezależna od innych typów danych telemetrycznych wartości, należy użyć [dane telemetryczne metryki](../articles/application-insights/app-insights-api-custom-events-metrics.md).

Maksymalna długość: 150
