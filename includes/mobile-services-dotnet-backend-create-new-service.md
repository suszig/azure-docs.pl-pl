
Wykonaj te kroki, aby utworzyć nową usługę mobilną.

1.  Zaloguj się do [klasycznego portalu Azure](https://manage.windowsazure.com/). W dolnej części okienka nawigacji kliknij opcję **+NOWE**. Rozwiń pozycje **Obliczenia** i **Usługa mobilna**, a następnie kliknij przycisk **Utwórz**.
    
    ![](./media/mobile-services-dotnet-backend-create-new-service/mobile-create.png)

    Spowoduje to wyświetlenie okna dialogowego **Create a Mobile Service** (Tworzenie usługi mobilnej).

2.  W oknie dialogowym **Create a Mobile Service** (Tworzenie usługi mobilnej) wybierz opcję **Create a free 20 MB SQL Database** (Utwórz bezpłatną bazę danych SQL 20 MB), wybierz środowisko uruchomieniowe **.NET**, a następnie wpisz nazwę poddomeny dla nowej usługi mobilnej w polu tekstowym **URL** (Adres URL). Kliknij przycisk strzałki w prawo, aby przejść do następnej strony.
    
    ![](./media/mobile-services-dotnet-backend-create-new-service/mobile-create-page1.png)

    Spowoduje to wyświetlenie strony **Specify database settings** (Określanie ustawień bazy danych).

    > [AZURE.NOTE] W ramach tego samouczka utworzone zostaną nowe wystąpienie i serwer bazy danych SQL. Można ponownie użyć tej nowej bazy danych i zarządzać nią w taki sam sposób, jak w przypadku dowolnego innego wystąpienia bazy danych SQL. Jeśli masz już bazę danych w tym samym regionie, co nowa usługa mobilna, możesz wybrać opcję **Use existing Database** (Użyj istniejącej bazy danych), a następnie wybrać tę bazę danych. Korzystanie z bazy danych w innym regionie nie jest zalecane z powodu dodatkowych kosztów przepustowości i większych opóźnień.

3.  W polu **Name** (Nazwa) wpisz nazwę nowej bazy danych, wypełnij pole **Login name** (Nazwa logowania) przeznaczone na nazwę logowania administratora dla nowego serwera usługi SQL Database, wpisz i potwierdź hasło oraz kliknij przycisk wyboru, aby ukończyć proces.
    ![](./media/mobile-services-dotnet-backend-create-new-service/mobile-create-page2.png)

Utworzona została nowa usługa mobilna, która może być używana przez aplikacje mobilne.


<!--HONumber=Sep16_HO3-->


