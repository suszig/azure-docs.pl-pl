Większość błędów uwierzytelniania czasu wynikiem ustawienia konfiguracji nieprawidłowe lub niezgodne. Oto kilka sugestii określonych elementów do sprawdzenia.

* Upewnij się, że nie zostały pominięte **zapisać** przycisk z dowolnego miejsca. Często jest to łatwo zrobić, a wynik jest użytkownik będzie patrzeć poprawne wartości na stronie portalu, ale faktycznie nie zostały zapisane w środowisku platformy Azure lub aplikacji usługi Azure AD.
* Dla ustawienia skonfigurowane w **ustawienia aplikacji** bloku portalu Azure, upewnij się, że poprawne aplikacji interfejsu API lub aplikacji sieci web wybrano po ustawienia zostały wprowadzone.  Upewnij się, czy ustawienia zostały wprowadzone, jak również **ustawień aplikacji** i nie **parametry połączenia**, ponieważ format dwie sekcje jest podobne.
* Do uwierzytelniania frontonu JavaScript, Pobierz manifest ponownie, aby sprawdzić, czy `oauth2AllowImplicitFlow` został pomyślnie zmieniony na `true`.
* Sprawdź, czy wszędzie tam, gdzie skonfigurowanych adresów URL HTTPS używanego:
  
  * W kodzie projektu
  * W specyfikacji CORS
  * W ustawieniach aplikacji środowiska platformy Azure dla każdej aplikacji interfejsu API i aplikacji sieci web
  * W ustawieniach aplikacji usługi Azure AD.
    
    Należy pamiętać, że adres URL aplikacji interfejsu API w przypadku kopiowania z portalu, często ma `http://` i należy ręcznie zmienić go do `https://`.
* Upewnij się, że wszystkie zmiany kodu zostały pomyślnie wdrożone. Na przykład w rozwiązaniu do wielu projektów istnieje możliwość zmiany kodu projektu i przypadkowo wybierz jedną z innych, jeśli zamierzasz wdrożyć zmiany.
* Upewnij się, że zamierzasz adresy URL HTTPS w przeglądarce, a nie adresów URL protokołu HTTP. Domyślnie program Visual Studio tworzy opublikować profile z adresów URL protokołu HTTP, a to co otwiera w przeglądarce po wdrożeniu projektu.
* Do uwierzytelniania JavaScript frontonu upewnij się, że CORS został poprawnie skonfigurowany w aplikacji interfejsu API, który wywołuje kod JavaScript. W razie wątpliwości, czy problem jest związane z CORS, spróbuj "*" jako dozwolone pochodzenie adresu URL. 
* W przypadku fronton JavaScript Otwórz kartę konsoli narzędzi dla deweloperów w przeglądarce, aby uzyskać więcej informacji o błędzie i zbadać żądań HTTP w sieci. Jednak komunikaty o błędach w konsoli może być mylące. Jeśli zostanie wyświetlony komunikat informujący o błąd CORS, rzeczywistego problemu może być uwierzytelniania. Można sprawdzić, czy jest to możliwe, uruchamiając aplikację z uwierzytelnianiem tymczasowo tymczasowo wyłączone.
* Dla aplikacji interfejsu API platformy .NET, upewnij się, że w przypadku uzyskiwania tylu informacji w komunikatach o błędach możliwie przez ustawienie [tryb customErrors na Off](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md#remoteview).
* Dla aplikacji interfejsu API platformy .NET, należy uruchomić [sesję debugowania zdalnego](../articles/app-service/web-sites-dotnet-troubleshoot-visual-studio.md#remotedebug)i sprawdź, czy wartości zmiennych, które są przekazywane do kodu, który używa biblioteki ADAL można uzyskać tokenu elementu nośnego lub kod, który sprawdza oświadczenia dla identyfikatora podmiotu zabezpieczeń oczekiwanej usługi. Należy pamiętać, że kod można wybrać wartości konfiguracji z różnych źródeł, więc można znaleźć niespodzianki w ten sposób. Na przykład, jeśli użytkownik błędnie `ida:ClientId` jako `ida:ClientID` podczas konfigurowania ustawień środowiska usługi aplikacji Azure, może pobrać kod `ida:ClientId` wartość, która jest szuka w pliku Web.config, ignorowanie ustawień usługi Azure App Service. 
* Jeśli elementy nie działają w normalnym oknie programu Internet Explorer, istniejące logowania może powodować konflikt; Funkcja InPrivate a następnie spróbuj Chrome lub Firefox.

