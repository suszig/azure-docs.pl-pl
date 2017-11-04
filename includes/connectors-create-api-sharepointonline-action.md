Teraz, dodano wyzwalacz, czas jego coś zrobić interesujące z danymi, który jest generowany przez wyzwalacz. Wykonaj następujące kroki, aby dodać **usługi SharePoint Online — Utwórz plik** akcji. Ta akcja spowoduje utworzenie pliku w usłudze SharePoint Online zawsze nowy wyzwalacz elementu. 

Aby skonfigurować to akcji, należy podać następujące informacje. Użytkownik poda te informacje, można zauważyć, że są to łatwy w użyciu dane generowane przez wyzwalacz jako dane wejściowe dla niektórych właściwości dla nowego pliku:

| Utwórz właściwość pliku | Opis |
| --- | --- |
| Adres URL witryny |Jest to adres URL witryny SharePoint Online, którym chcesz utworzyć nowy plik. Wybierz lokację z listy przedstawiony. |
| Ścieżka folderu |Jest to folder (adresem URL wybrane w poprzednim kroku), gdzie zostaną umieszczone nowy plik. Wyszukaj i wybierz folder. |
| Nazwa pliku |Jest to nazwa tworzony plik. |
| Zawartość pliku |Zawartość, który zostanie zapisany do pliku. |

1. Wybierz **+ nowy krok** Aby dodać akcję.  
   ![Obraz akcji online programu SharePoint 1](./media/connectors-create-api-sharepointonline/action-1.png)  
2. Wybierz **Dodaj akcję** łącza. Służy to pole wyszukiwania, gdzie możesz wyszukać dowolną akcję użytkownik chce przejąć. Na przykład SharePoint akcje są przydatne.    
   ![Obraz akcji online programu SharePoint 2](./media/connectors-create-api-sharepointonline/action-2.png)    
3. Wprowadź *sharepoint* do wyszukania akcji związanych z programu SharePoint.
4. Wybierz **usługi SharePoint Online — Utwórz plik** jako akcję do wykonania.   **Uwaga**: pojawi się monit o autoryzowanie aplikację logiki, aby uzyskać dostęp do konta programu SharePoint, jeśli wcześniej nie zostały utworzone połączenie do usługi SharePoint Online.    
   ![Obraz akcji online programu SharePoint 3](./media/connectors-create-api-sharepointonline/action-3.png)    
5. **Utwórz plik** sterowania zostanie otwarta.   
   ![Obraz akcji online programu SharePoint 4](./media/connectors-create-api-sharepointonline/action-4.png)     
6. Wybierz **adres URL witryny** i Przeglądaj w celu znalezienia lokacji, w którym chcesz utworzyć plik.     
   ![Obraz akcji online programu SharePoint 5](./media/connectors-create-api-sharepointonline/action-5.png)  
7. Wybierz **ścieżka folderu** i przejdź do folderu, w którym zostaną umieszczone nowy plik.  
   ![Obraz akcji online programu SharePoint 6](./media/connectors-create-api-sharepointonline/action-6.png)  
8. Wybierz **nazwę pliku** kontroli i wprowadź nazwę pliku, który chcesz utworzyć. W tym miejscu można wprowadzać nazwę pliku bezpośrednio lub można użyć dowolnego z właściwości z wyzwalacza utworzonego wcześniej. Aby to zrobić, wybierając z listy właściwości **dane wyjściowe z podczas tworzenia nowego elementu**. Ta lista jest wyświetlana tylko po wybraniu **nazwę pliku** formantu. W tym walkthough wybrano ID (identyfikator nowy element listy) jako nazwę pliku tworzonego przez **usługi SharePoint Online — Utwórz plik** akcji.    
   ![Obraz akcji online programu SharePoint 7](./media/connectors-create-api-sharepointonline/action-7.png)  
9. Wybierz **plików zawartości** kontroli i wprowadź zawartości, który zostanie zapisany do pliku, który zostanie utworzony. Zawartość pliku zauważyć, że można użyć dowolnego z właściwości z wyzwalacza utworzonego wcześniej. Po prostu zaznacz właściwości, z listy przedstawiony. Alternatywnie można wprowadzić **plików zawartości** tekst bezpośrednio w formancie. W tym przykładzie I wybrane niektórych właściwości i dodać spacje i łącznika między każdej właściwości.        
   ![Obraz akcji online programu SharePoint 8](./media/connectors-create-api-sharepointonline/action-8.png)  
10. Zapisz zmiany w przepływie pracy  
11. Gratulacje, masz teraz aplikacji logiki w pełni funkcjonalne, który jest wyzwalane, gdy nowy element zostanie dodany do listy programu SharePoint Online. Aplikacja, spowoduje to utworzenie pliku przy użyciu właściwości z nowy element listy.  Teraz możesz przetestować go przez utworzenie nowego elementu na liście programu SharePoint. 

