
>[!NOTE]
>Usługa Log Analytics wcześniej nosiła nazwę Operational Insights.
>
>

Następujące limity dotyczą zasobów usługi Log Analytics na subskrypcję:

| Zasób | Limit domyślny | Komentarze
| --- | --- | --- |
| Liczba wolnych obszarów roboczych na subskrypcję | 10 | Tego limitu nie można zwiększyć. |
| Liczba płatnych obszarów roboczych na subskrypcję | Nie dotyczy | Ograniczeniem jest liczba zasobów w grupie zasobów i liczba grup zasobów na subskrypcję | 


Następujące limity dotyczą każdego obszaru roboczego usługi Log Analytics:

|  | Bezpłatna | Standardowa | Premium | Autonomiczna | OMS |
| --- | --- | --- | --- | --- | --- |
| Ilość danych zebranych na dzień |500 MB<sup>1</sup> |Brak |Brak | Brak | Brak
| Okres przechowywania danych |7 dni |1 miesiąc |12 miesięcy | 1 miesiąc<sup>2</sup> | 1 miesiąc <sup>2</sup>|

<sup>1</sup> Gdy klienci osiągną limit dziennego transferu danych wynoszący 500 MB, analiza danych zostanie zatrzymana, a następnie wznowiona na początku następnego dnia. Dzień jest oparty na czasie UTC.

<sup>2</sup> Okres przechowywania danych dla planów cenowych w warstwach Autonomiczna i OMS można zwiększyć do 730 dni.

| Kategoria | Limity | Komentarze
| --- | --- | --- |
| Interfejs API modułu zbierającego dane | Maksymalny rozmiar pojedynczego wpisu to 30 MB<br>Maksymalny rozmiar wartości pól to 32 KB | Dziel większe woluminy na wiele wpisów<br>Pola dłuższe niż 32 KB są obcinane. |
| Interfejs API wyszukiwania | 5000 rekordów zwracanych dla danych niezagregowanych<br>500 000 rekordów dla danych zagregowanych | Zagregowane dane to wyszukiwanie obejmujące polecenie `measure`
 


<!--HONumber=Feb17_HO3-->


