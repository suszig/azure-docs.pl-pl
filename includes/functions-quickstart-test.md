
Ponieważ elementy szybkiego startu usługi Azure Functions zawierają funkcjonalny kod, można natychmiast przetestować nową funkcję.

1. Na karcie **Programowanie** sprawdź zawartość okna **Kod** i zwróć uwagę, że ten kod Node.js oczekuje żądania HTTP z wartością *Nazwa* przekazywaną w treści komunikatu lub w ciągu zapytania. Po uruchomieniu funkcji ta wartość jest zwracana w komunikacie odpowiedzi.
   
2. Kliknij pozycję **Test**, aby wyświetlić wbudowane okienko testowego żądania HTTP dla tej funkcji.
 
    ![](./media/functions-quickstart-test/function-app-develop-tab-testing.png)

3. W polu tekstowym **Treść żądania** zmień wartość właściwości *Nazwa* na swoją nazwę i kliknij pozycję **Uruchom**. Wykonanie jest wyzwalane przez testowe żądanie HTTP, informacje są zapisywane w dziennikach, a odpowiedź „hello” jest wyświetlana w **danych wyjściowych**. 

4. Aby wyzwolić wykonywanie tej samej funkcji z narzędzia testującego HTTP lub z innego okna przeglądarki, skopiuj wartość **Adres URL funkcji** z karty **Programowanie** i wklej go do narzędzia lub na pasku adresu przeglądarki. Dołącz wartość ciągu zapytania `&name=yourname` do adresu URL i wykonaj żądanie. Pamiętaj, że te same informacje są zapisywane w dziennikach i ten sam ciąg jest zawarty w treści komunikatu z odpowiedzią.

    ![](./media/functions-quickstart-test/function-app-browser-testing.png)


<!--HONumber=Feb17_HO2-->


