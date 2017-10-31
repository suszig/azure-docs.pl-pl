Aktualizacje agenta usługi Azure File Sync będą wydawane regularnie, aby zapewniać nowe funkcje i rozwiązywać wszelkie znalezione problemy. Zalecamy włączenie usługi Microsoft Update w celu pobrania wszystkich aktualizacji do agenta usługi Azure File Sync zaraz po ich wydaniu. Oczywiście rozumiemy, że niektóre organizacje chcą zachować ścisłą kontrolę i możliwość testowania aktualizacji. W przypadku wdrożeń korzystających ze starszych wersji agenta usługi Azure File Sync:

- Usługa synchronizacji magazynu będzie honorować wcześniejszą wersję główną przez trzy miesiące po początkowym wydaniu nowej wersji głównej. Na przykład usługa synchronizacji magazynu będzie obsługiwać wersję 1.\* przez trzy miesiące od wydania wersji 2.\*.
- Po upływie trzech miesięcy usługa synchronizacji magazynu zacznie blokować zarejestrowanym serwerom używającym wygasłej wersji możliwość synchronizacji ze swoimi grupami synchronizacji.
- W przypadku poprzedniej wersji głównej w ciągu tych trzech miesięcy wszystkie poprawki błędów będą kierowane wyłącznie do bieżącej wersji głównej.

> [!Note]  
> Otrzymasz wyskakujące powiadomienie w witrynie Azure Portal, jeśli używasz usługi Azure File Sync w wersji, która wygaśnie w ciągu następnych trzech miesięcy.