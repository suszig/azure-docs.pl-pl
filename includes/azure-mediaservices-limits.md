>[!NOTE]
>W przypadku zasobów, których ilość nie jest stała, możesz poprosić o zwiększenie limitów przydziału, otwierając bilet pomocy technicznej. **Nie** twórz dodatkowych kont usługi Azure Media Services w celu uzyskania wyższych limitów.

| Zasób | Limit domyślny | 
| --- | --- | 
| Konta usługi Azure Media Services (AMS) w jednej subskrypcji | 25 (stały) |
| Jednostki zarezerwowane multimediów na konto AMS |25 (S1, S2)<br/>10 (S3) <sup>(1)</sup> | 
| Zadania na konto AMS | 50,000<sup>(2)</sup> |
| Łańcuchowe zadania podrzędne na zadanie | 30 (stały) |
| Elementy zawartości na konto AMS | 1 000 000|
| Elementy zawartości na zadanie podrzędne | 50 |
| Elementy zawartości na zadanie | 100 |
| Unikatowe lokalizatory jednocześnie skojarzone z elementem zawartości | 5<sup>(4)</sup> |
| Kanały na żywo dla każdego konta AMS |5|
| Programy w stanie zatrzymania na kanał |50|
| Programy w stanie działania na kanał |3|
| Punkty końcowe przesyłania strumieniowego w stanie działania na konto AMS|2|
| Jednostki przesyłania strumieniowego na punkt końcowy przesyłania strumieniowego |10 |
| Konta magazynu | 1000<sup>(5)</sup> (stały) |
| Zasady | 1 000 000<sup>(6)</sup> |
| Rozmiar pliku| W niektórych scenariuszach istnieje limit na maksymalny obsługiwany rozmiar pliku do przetwarzania w usłudze Media Services. <sup>7</sup> |
  
<sup>1</sup> Jednostki zarezerwowane S3 nie są dostępne w regionie Indie Zachodnie. Jeśli zmienisz typ (np. z S2 do S1) maksymalny limit RU są zerowane.

<sup>2</sup> Ta liczba obejmuje zadania umieszczone w kolejce, zakończone, aktywne i anulowane. Nie obejmuje usuniętych zadań. Stare zadania możesz usunąć za pomocą funkcji **IJob.Delete** lub żądania HTTP **DELETE**.

Począwszy od 1 kwietnia 2017 dowolnego rekordu zadania konta starsze niż 90 dni zostaną automatycznie usunięte, wraz z jego skojarzonych rekordów zadań, nawet jeśli jest to całkowita liczba rekordów poniżej maksymalny limit przydziału. Jeśli chcesz zarchiwizować informacje dotyczące zadania lub zadania podrzędnego, możesz użyć kodu opisanego [tutaj](../articles/media-services/media-services-dotnet-manage-entities.md).

<sup>3</sup> żądaniu skierowanym do listy zadań jednostek, maksymalnie 1000 zadań jest zwracana na żądanie. Jeśli potrzebujesz śledzić wszystkie przesłane zadania, możesz użyć opcji top/skip zgodnie z opisem w temacie [OData system query options](http://msdn.microsoft.com/library/gg309461.aspx) (Opcje zapytań systemu OData).

<sup>4</sup> Lokalizatory nie są przeznaczone do zarządzania kontrolą dostępu dla poszczególnych użytkowników. Aby nadać różne uprawnienia poszczególnym użytkownikom, użyj rozwiązań do zarządzania prawami cyfrowymi (DRM, Digital Rights Management). Więcej informacji znajduje się w [tej](../articles/media-services/media-services-content-protection-overview.md) sekcji.

<sup>5</sup> Konta magazynu muszą pochodzić z tej samej subskrypcji platformy Azure.

<sup>6</sup> Limit różnych zasad usługi AMS wynosi 1 000 000 (na przykład zasad lokalizatorów lub ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Korzystaj z tego samego identyfikatora zasad, jeśli zawsze używasz tych samych dni, uprawnień dostępu itd. Informacje i przykład możesz znaleźć w [tej](../articles/media-services/media-services-dotnet-manage-entities.md#limit-access-policies) sekcji.

<sup>7</sup>Jeśli można przekazać zawartość do zasobów w usłudze Azure Media Services do przetworzenia za pomocą jednego z procesory multimediów usługi (oznacza to, kodery, takich jak aparaty Media Encoder Standard i Media Encoder Premium w przepływie pracy lub analizy, takich jak wykrywanie twarzy na obrazie), następnie należy zwrócić uwagę ograniczenia dotyczące maksymalne rozmiary obsługiwane. 

Maksymalny rozmiar obsługiwany dla pojedynczego obiektu blob jest obecnie maksymalnie 5 TB w magazynie obiektów Blob Azure. Jednak dodatkowe ograniczenia mają zastosowanie w usłudze Azure Media Services oparte na rozmiarów maszyn wirtualnych, które są używane przez usługę. W poniższej tabeli przedstawiono limity na każdym jednostki zarezerwowane multimediów (S1, S2, S3.) Jeśli plik źródłowy jest większy niż limit zdefiniowany w tabeli, zadania kodowania zakończy się niepowodzeniem. Jeśli są kodowanie 4K rozpoznawania źródeł długotrwałe, są wymagane na potrzeby jednostki zarezerwowane multimediów S3 wydajności potrzebne. Jeśli masz zawartość 4K, który jest większy niż limit 260 GB jednostki zarezerwowane multimediów S3, skontaktuj się z nami pod adresem amshelp@microsoft.com dla potencjalnych środki zaradcze do obsługi danego scenariusza.

| Typ jednostki zarezerwowanej multimediów | Maksymalny rozmiar wejściowe (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
