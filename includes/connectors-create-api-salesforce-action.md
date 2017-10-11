Teraz, gdy zostaną dodane warunek, czas jego coś zrobić interesujące z danymi, który jest generowany przez wyzwalacz. Wykonaj następujące kroki, aby dodać **Salesforce - Get obiektu** akcji. Ta akcja otrzyma dane każdym razem, gdy jest tworzony nowy potencjalnych klientów. Dodasz również drugiej akcji, który będzie używany przez dane usług Salesforce - Get akcję obiektu do wysyłania wiadomości e-mail za pomocą łącznika usługi Office 365.  

Aby skonfigurować to akcji, należy podać następujące informacje. Można zauważyć, że są to łatwy w użyciu dane generowane przez wyzwalacz jako dane wejściowe dla niektórych właściwości dla nowego pliku:

| Utwórz właściwość pliku | Opis |
| --- | --- |
| Typ obiektu |Jest to typ obiektu Salesforce, które planuje się. Przykłady są realizacji, kont itd. |
| Identyfikator obiektu: |Reprezentuje identyfikator obiektu. |

1. Wybierz **Dodaj akcję** łącza. Służy to pole wyszukiwania, gdzie możesz wyszukać dowolną akcję użytkownik chce przejąć. Na przykład Salesforce akcje są przydatne.      
   ![Obraz akcji SalesForce 1](./media/connectors-create-api-salesforce/action-1.png)  
2. Wprowadź *salesforce* do wyszukania akcji związanych z usług salesforce.
3. Wybierz **Salesforce - Get obiektu** jako akcję do wykonania.   **Uwaga**: pojawi się monit o autoryzowanie aplikację logiki, aby uzyskać dostęp do konta usług Salesforce, jeśli nie zostało zrobione to wcześniej.    
   ![Obraz akcji SalesForce 2](./media/connectors-create-api-salesforce/action-2.png)    
4. **Obiektu Get** sterowania zostanie otwarta.  
5. Wybierz *prowadzić* jako typ obiektu.
6. Wybierz **obiektu o identyfikatorze** formantu.
7. Wybierz **...**  rozwiń listę tokenów, które mogą służyć jako dane wejściowe dla akcji.       
   ![Obraz akcji SalesForce 3](./media/connectors-create-api-salesforce/action-3.png)    
8. Wybierz **prowadzić identyfikator** sterowania zostanie otwarta.   
   ![Obraz akcji SalesForce 4](./media/connectors-create-api-salesforce/action-4.png)     
9. Należy zauważyć, że token prowadzić Identyfikatora jest teraz w formancie obiektu o identyfikatorze wskazujący akcję obiektu Get wyszuka realizacji o identyfikatorze, który jest taki sam identyfikator realizacji potencjalnych klientów, która wyzwoliła tej aplikacji logiki.  
   ![Obraz akcji SalesForce 5](./media/connectors-create-api-salesforce/action-5.png)  
10. Zapisz swoją pracę. To wszystko, Akcja obiektu Get została dodana do aplikacji logiki. Pobierz obiekt formantu powinna wyglądać następująco:    
    ![Obraz akcji SalesForce 6](./media/connectors-create-api-salesforce/action-6.png)  

Teraz, dodano akcję w celu pobrania potencjalnego klienta, możesz ciekawy z nowo utworzonego potencjalnych klientów. W przedsiębiorstwie możesz wysłać wiadomość e-mail do powiadamiania listę dystrybucyjną utworzono nowe potencjalnych klientów. Użyjmy łącznik usługi Office 365, aby wysłać wiadomość e-mail z niektórymi odpowiednich informacji z nowego obiektu potencjalnych klientów w usłudze Salesforce.  

1. Wybierz **Dodaj akcję** wprowadź *e-mail* w formancie wyszukiwania. To filtry akcji do tych, które odnoszą się do wysyłania i odbierania wiadomości e-mail.  
2. Wybierz **Office 365 Outlook - wysyłania usługi poczty e-mail** elementu listy. Jeśli nie utworzono już *połączenia* na koncie usługi Office 365, pojawi się monit o podanie poświadczeń usługi Office 365 go teraz utworzyć. Po wykonaniu **wysłać wiadomość e-mail** sterowania zostanie otwarta.        
   ![Obraz akcji SalesForce 7](./media/connectors-create-api-salesforce/action-7.png)  
3. Wprowadź adres e-mail, który chcesz wysłać wiadomość e-mail do w **do** formantu.
4. W **podmiotu** kontrolować, wprowadź *nowe prowadzić utworzony* — a następnie wybierz pozycję *firmy* tokenu. Spowoduje to wyświetlenie *firmy* pola potencjalnego nowego klienta utworzone w usłudze Salesforce.  
5. W **treści** sterowania, można wybrać dowolny tokenów z nowego obiektu potencjalnych klientów i możesz też wprowadzić niezależnie od tekst może zostać wyświetlony w treści wiadomości e-mail. Oto przykład:  
   ![Obraz akcji SalesForce 8](./media/connectors-create-api-salesforce/action-8.png)   
6. Zapisywanie przepływu pracy.  

To już wszystko. Aplikację logiki jest teraz ukończona.  

Teraz można przetestować aplikację logiki: w usłudze Salesforce, Utwórz nowy potencjalnego klienta, który spełnia warunek utworzony.  Po wykonaniu tego przewodnika w pełni, wystarczy utworzyć potencjalnego klienta adres e-mail, który zawiera *amazon.com* w nim. Po kilku sekundach powinno być wyzwalane aplikację logiki i wyniki może wyglądać podobnie do poniższego:  
![Obraz akcji SalesForce 9](./media/connectors-create-api-salesforce/action-9.png)  

