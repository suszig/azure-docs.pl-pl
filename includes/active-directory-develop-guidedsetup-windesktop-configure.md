
## <a name="register-your-application"></a>Rejestrowanie aplikacji
Aplikację można zarejestrować na dwa sposoby.

### <a name="option-1-express-mode"></a>Opcja 1: Tryb ekspresowy
Można szybko zarejestrować aplikację w następujący sposób:
1. Przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure).

2. Wybierz **Dodaj aplikację**.

3. W **Nazwa aplikacji** wprowadź nazwę aplikacji.

4. Upewnij się, że **instrukcje konfiguracji** pole wyboru jest wybrany, a następnie wybierz **Utwórz**.

5. Postępuj zgodnie z instrukcjami w celu uzyskania Identyfikatora aplikacji i wklej go w kodzie.

### <a name="option-2-advanced-mode"></a>Opcja 2: Tryb zaawansowany
Aby zarejestrować aplikację i dodać swoje informacje rejestracyjne aplikacji do rozwiązania, wykonaj następujące czynności:
1. Jeśli nie zostało już zarejestrowane aplikacji, przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/portal/register-app).

2. Wybierz **Dodaj aplikację**.

3. W **Nazwa aplikacji** wprowadź nazwę aplikacji. 

4. Upewnij się, że **instrukcje konfiguracji** pole wyboru jest wyczyszczone, a następnie wybierz **Utwórz**.

5. Wybierz **dodać platformy**, wybierz pozycję **aplikacji natywnej**, a następnie wybierz **zapisać**.

6. W **identyfikator aplikacji** należy skopiować identyfikator GUID.

7. Przejdź do programu Visual Studio, otwórz *App.xaml.cs* pliku, a następnie zastąp `your_client_id_here` właśnie zarejestrowany i skopiować identyfikator aplikacji.

    ```csharp
    private static string ClientId = "your_application_id_here";
    ```
