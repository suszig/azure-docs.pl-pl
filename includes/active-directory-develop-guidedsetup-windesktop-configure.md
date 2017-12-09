
## <a name="create-an-application-express"></a>Tworzenie aplikacji (Express)
Teraz musisz zarejestrować aplikację w *portalu rejestracji aplikacji Microsoft*:
1. Zarejestrować aplikację za pośrednictwem [portalu rejestracji aplikacji firmy Microsoft](https://apps.dev.microsoft.com/portal/register-app?appType=mobileAndDesktopApp&appTech=windowsDesktop&step=configure)
2.  Wprowadź nazwę aplikacji i poczty e-mail
3.  Upewnij się, że zaznaczono opcję instrukcje konfiguracji
4.  Postępuj zgodnie z instrukcjami, aby uzyskać identyfikator aplikacji i wklej go w kodzie

### <a name="add-your-application-registration-information-to-your-solution-advanced"></a>Dodaj swoje informacje rejestracyjne aplikacji do rozwiązania (zaawansowane)
Teraz musisz zarejestrować aplikację w *portalu rejestracji aplikacji Microsoft*:
1. Przejdź do [portalu rejestracji aplikacji Microsoft](https://apps.dev.microsoft.com/portal/register-app) do rejestrowania aplikacji
2. Wprowadź nazwę aplikacji i poczty e-mail 
3. Upewnij się, że jest zaznaczona opcja instrukcje konfiguracji
4. Kliknij przycisk `Add Platform`, a następnie wybierz pozycję `Native Application` i kliknij przycisk Zapisz
5. Skopiuj identyfikator GUID w identyfikator aplikacji, wróć do programu Visual Studio, otwórz `App.xaml.cs` i Zastąp `your_client_id_here` z został zarejestrowany identyfikator aplikacji:

```csharp
private static string ClientId = "your_application_id_here";
```
