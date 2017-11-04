1. Zaloguj się w [portalu Azure].
2. Kliknij kolejno opcje **+NOWE** > **Sieci Web i mobilność** > **Aplikacja mobilna**, a następnie podaj nazwę zaplecza aplikacji mobilnej.
3. W obszarze **Grupa zasobów** wybierz istniejącą grupę zasobów lub utwórz nową (używając tej samej nazwy, co aplikacja). 
   
    Możesz wybrać inny plan usługi App Service lub utworzyć nowy. Aby uzyskać więcej informacji na temat planów usług App Services oraz sposobu tworzenia nowego planu w innej warstwie cenowej oraz w preferowanej lokalizacji, zobacz temat [Szczegółowe omówienie planów usługi Azure App Service](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. W przypadku **planu usługi App Service** jest wybrany plan domyślny (w [warstwie standardowej](https://azure.microsoft.com/pricing/details/app-service/)). Możesz też wybrać inny plan lub [Utwórz nową](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Ustawienia planu usługi App Service określają [lokalizację, funkcje, koszt i zasoby obliczeniowe](https://azure.microsoft.com/pricing/details/app-service/) skojarzone z aplikacją. 
   
    Po wybraniu planu kliknij przycisk **Utwórz**. Spowoduje to utworzenie zaplecza aplikacji mobilnej. 
5. W bloku **Ustawienia** dotyczącym nowego zaplecza aplikacji mobilnej kliknij kolejno pozycje **Szybki start** > platforma aplikacji klienta > **Połącz z bazą danych**. 
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
6. W bloku **Add data connection** (Dodaj połączenie danych) kliknij kolejno opcje **Baza danych SQL** > **Utwórz nową bazę danych**, wpisz nazwę bazy danych w polu **Nazwa**, wybierz warstwę cenową, a następnie kliknij opcję **Serwer**.  Możesz użyć tej nowej bazy danych ponownie. Jeśli masz już bazę danych w tej samej lokalizacji, możesz zamiast tego wybrać opcję **Użyj istniejącej bazy danych**. Korzystanie z bazy danych w innej lokalizacji nie jest zalecane z powodu kosztów przepustowości i większego opóźnienia.
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
7. W bloku **Nowy serwer** wpisz unikatową nazwą serwera w polu **Nazwa serwera**, podaj identyfikator logowania i hasło, zaznacz opcję **Zezwalaj usługom platformy Azure na dostęp do serwera** i kliknij przycisk **OK**. Zostanie utworzona nowa baza danych.
8. Po powrocie do bloku **Add data connection** (Dodaj połączenie danych) kliknij opcję **Parametry połączenia**, wpisz wartości identyfikatora logowania i hasła dla bazy danych, po czym kliknij przycisk **OK**. Przed podjęciem dalszych działań poczekaj kilka minut, aż baza danych zostanie pomyślnie wdrożona.

<!-- URLs. -->
[portalu Azure]: https://portal.azure.com/
