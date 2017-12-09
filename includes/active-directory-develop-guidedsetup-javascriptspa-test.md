
## <a name="test-your-code"></a>Testowanie kodu

### <a name="test-with-visual-studio"></a>Test z programem Visual Studio
Jeśli używasz programu Visual Studio, naciśnij klawisz **F5** do uruchomienia projektu. Przeglądarka otworzy http://<span></span>localhost: {port} lokalizacji i zostanie wyświetlony **wywołać Microsoft interfejsu API programu Graph** przycisku.

<p/><!-- --> 

### <a name="test-with-python-or-other-web-server"></a>Test z języka Python lub inny serwer sieci web
Jeśli nie używasz programu Visual Studio, upewnij się, że serwer sieci web jest uruchomiona. Konfigurowanie serwera do nasłuchiwania na portach TCP, który jest oparty na lokalizację użytkownika **index.html** pliku. Dla języka Python Rozpocznij nasłuchiwanie na porcie uruchamiając terminal wiersza polecenia w folderze aplikacji:
 
```bash
python -m http.server 8080
```
Otwórz przeglądarkę i wpisz http://<span></span>localhost:8080 lub http://<span></span>localhost: {port} gdzie **portu** jest portu nasłuchiwania serwera sieci web. Powinna zostać wyświetlona zawartość pliku index.html i **wywołać Microsoft interfejsu API programu Graph** przycisku.

## <a name="test-your-application"></a>Testowanie aplikacji

Po załadowaniu pliku index.html przeglądarki wybierz **wywołać Microsoft interfejsu API programu Graph**. Podczas pierwszego uruchomienia aplikacji, przeglądarka przekierowuje do punktu końcowego v2.0 Microsoft Azure Active Directory (Azure AD) i zostanie wyświetlony monit o Zaloguj:
 
![Zaloguj się do swojego konta JavaScript SPA](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspascreenshot1.png)


### <a name="provide-consent-for-application-access"></a>Podaj zgody na dostęp do aplikacji

Podczas pierwszego logowania się do aplikacji, zostanie wyświetlony monit o wyrażenie zgody na umożliwić aplikacji dostęp do Twojego profilu i logowanie się w:

![Wyrażenie zgody na dostęp do aplikacji](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptspaconsent.png)

### <a name="view-application-results"></a>Wyświetl wyniki aplikacji
Po zalogowaniu, powinny pojawić się dane profilu użytkownika w **odpowiedzi wywołań interfejsu API Graph** pole.
 
![Oczekiwanych rezultatów z wywołania interfejsu API programu Microsoft Graph](media/active-directory-develop-guidedsetup-javascriptspa-test/javascriptsparesults.png)

Podstawowe informacje o token, który został uzyskany w powinno również zostać wyświetlone **Token dostępu** i **identyfikator tokenu oświadczeń** pola.

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i uprawnień delegowanych

Wymaga interfejsu API programu Microsoft Graph **user.read** zakresu odczytać profilu użytkownika. Ten zakres jest automatycznie dodawany domyślnie w każdej aplikacji, który jest zarejestrowany w portalu rejestracji. Inne interfejsy API dla programu Microsoft Graph, a także niestandardowych interfejsów API dla serwera zaplecza może wymagać dodatkowych zakresów. Wymaga interfejsu API programu Microsoft Graph **Calendars.Read** zakres, aby wyświetlić listę kalendarzy użytkownika.

Aby uzyskać dostęp do kalendarzy użytkownika w kontekście aplikacji, należy dodać **Calendars.Read** delegowane uprawnienia do informacji o rejestracji aplikacji. Następnie należy dodać **Calendars.Read** zakres do **acquireTokenSilent** wywołania. 

>[!NOTE]
>Użytkownik może zostać wyświetlony monit dla dodatkowych zgody, jak zwiększyć liczbę zakresów.

Jeśli interfejs API zaplecza nie wymaga zakresu (niezalecane), możesz użyć **clientId** jako zakres w **acquireTokenSilent** i **acquireTokenRedirect** wywołania.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
