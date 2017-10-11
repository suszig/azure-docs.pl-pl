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
| Rozmiar pliku| W niektórych scenariuszach istnieje limit maksymalnego rozmiaru pliku do przetwarzania w usłudze Media Services. <sup>7</sup> |
  
<sup>1</sup> Jednostki zarezerwowane S3 nie są dostępne w regionie Indie Zachodnie. Maksymalny limit RU resetowane w przypadku klientów zmiany typu (np. z S2 do S1). 

<sup>2</sup> Ta liczba obejmuje zadania umieszczone w kolejce, zakończone, aktywne i anulowane. Nie obejmuje usuniętych zadań. Stare zadania możesz usunąć za pomocą funkcji **IJob.Delete** lub żądania HTTP **DELETE**.

Począwszy od 1 kwietnia 2017 roku, wszystkie rekordy zadań na Twoim koncie, które są starsze niż 90 dni, będą automatycznie usuwane wraz ze skojarzonymi rekordami zadań podrzędnych nawet wtedy, gdy całkowita liczba rekordów jest mniejsza niż maksymalny limit przydziału. Jeśli chcesz zarchiwizować informacje dotyczące zadania lub zadania podrzędnego, możesz użyć kodu opisanego [tutaj](../articles/media-services/media-services-dotnet-manage-entities.md).

<sup>3</sup> Żądania wyświetlenia listy jednostek zadań zwracają maksymalnie 1000 jednostek na żądanie. Jeśli potrzebujesz śledzić wszystkie przesłane zadania, możesz użyć opcji top/skip zgodnie z opisem w temacie [OData system query options](http://msdn.microsoft.com/library/gg309461.aspx) (Opcje zapytań systemu OData).

<sup>4</sup> Lokalizatory nie są przeznaczone do zarządzania kontrolą dostępu dla poszczególnych użytkowników. Aby nadać różne uprawnienia poszczególnym użytkownikom, użyj rozwiązań do zarządzania prawami cyfrowymi (DRM, Digital Rights Management). Więcej informacji znajduje się w [tej](../articles/media-services/media-services-content-protection-overview.md) sekcji.

<sup>5</sup> Konta magazynu muszą pochodzić z tej samej subskrypcji platformy Azure.

<sup>6</sup> Limit różnych zasad usługi AMS wynosi 1 000 000 (na przykład zasad lokalizatorów lub ContentKeyAuthorizationPolicy). 

>[!NOTE]
> Korzystaj z tego samego identyfikatora zasad, jeśli zawsze używasz tych samych dni, uprawnień dostępu itd. Informacje i przykład możesz znaleźć w [tej](../articles/media-services/media-services-dotnet-manage-entities.md#limit-access-policies) sekcji.

<sup>7</sup>Jeśli można przekazać zawartość do zasobów w usłudze Azure Media Services przy użyciu zamiaru go z jednym procesory multimediów w naszej usługi przetworzyć (tj. koderów, takich jak aparaty Media Encoder Standard i Media Encoder Premium w przepływie pracy lub analizy jak krój Wykrywacz), a następnie należy zwrócić uwagę ograniczenie maksymalnego rozmiaru. 

Począwszy od 15 maja 2017 maksymalny rozmiar obsługiwany dla pojedynczego obiektu blob 195 TB — z pliku largers przekracza ten limit, zadanie zakończy się niepowodzeniem. Pracujemy nad poprawkę rozwiązującą ten limit. Ponadto ograniczenie maksymalnego rozmiaru zasobu ma następującą składnię.

| Typ jednostki zarezerwowanej multimediów | Maksymalny rozmiar wejściowe (GB)| 
| --- | --- | 
|S1 | 325|
|S2 | 640|
|S3 | 260|
