## <a name="test-querying-the-microsoft-graph-api-from-your-ios-application"></a>Testowanie zapytań interfejsu API programu Microsoft Graph z aplikacji systemu iOS

Aby uruchomić kod w symulatorze, naciśnij klawisz **polecenia** + **R**.

![Testowanie aplikacji w symulatorze](media/active-directory-develop-guidedsetup-ios-test/iostestscreenshot.png)

Gdy wszystko jest gotowe do testowania, wybierz **wywołać Microsoft interfejsu API programu Graph**. Po wyświetleniu monitu wprowadź nazwę użytkownika i hasło.

### <a name="provide-consent-for-application-access"></a>Podaj zgody na dostęp do aplikacji
Podczas pierwszego logowania się do aplikacji, zostanie wyświetlony monit o wyrażenie zgody na umożliwić aplikacji dostęp do Twojego profilu i logowanie się w:

![Wyrażenie zgody na dostęp do aplikacji](media/active-directory-develop-guidedsetup-ios-test/iosconsentscreen.png)

### <a name="view-application-results"></a>Wyświetl wyniki aplikacji
Po zalogowaniu, powinny pojawić się informacje dotyczące profilu użytkownika zwrócony przez wywołanie interfejsu API programu Microsoft Graph **rejestrowanie** sekcji. 

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i uprawnień delegowanych

Wymaga interfejsu API programu Microsoft Graph **user.read** zakresu odczytać profilu użytkownika. Ten zakres jest automatycznie dodawany domyślnie w każdej aplikacji, który jest zarejestrowany w portalu rejestracji. Inne interfejsy API dla programu Microsoft Graph, a także niestandardowych interfejsów API dla serwera zaplecza może wymagać dodatkowych zakresów. Wymaga interfejsu API programu Microsoft Graph **Calendars.Read** zakres, aby wyświetlić listę kalendarzy użytkownika.

Aby uzyskać dostęp do kalendarzy użytkownika w kontekście aplikacji, należy dodać **Calendars.Read** delegowane uprawnienia do informacji o rejestracji aplikacji. Następnie należy dodać **Calendars.Read** zakres do **acquireTokenSilent** wywołania. 

>[!NOTE]
>Użytkownik może zostać wyświetlony monit dla dodatkowych zgody, jak zwiększyć liczbę zakresów.

<!--end-collapse-->

[!INCLUDE [Help and support](./active-directory-develop-help-support-include.md)]
