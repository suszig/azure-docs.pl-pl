
## <a name="test-your-code"></a>Testowanie kodu

Aby przetestować aplikację w programie Visual Studio, naciśnij klawisz **F5** do uruchomienia projektu. **Właściwości MainWindow** aplikacji zostanie wyświetlony:

![Testowanie aplikacji](./media/active-directory-develop-guidedsetup-windesktop-test/samplescreenshot.png)

Jeśli wszystko jest gotowe do uruchomienia testu, konto Microsoft Azure Active Directory (konta firmowego lub szkolnego) lub konta Microsoft (live.com, outlook.com) można używać do logowania. Podczas pierwszego uruchomienia aplikacji, zostanie wyświetlony monit o Zaloguj:

![Zaloguj się do aplikacji](./media/active-directory-develop-guidedsetup-windesktop-test/signinscreenshot.png)

### <a name="provide-consent-for-application-access"></a>Podaj zgody na dostęp do aplikacji
Podczas pierwszego logowania się do aplikacji, zostanie wyświetlony monit o wyrażenie zgody na umożliwić aplikacji dostęp do Twojego profilu i logowanie się w: 

![Wyrażenie zgody na dostęp do aplikacji](./media/active-directory-develop-guidedsetup-windesktop-test/consentscreen.png)

### <a name="view-application-results"></a>Wyświetl wyniki aplikacji
Po zalogowaniu, powinny być widoczne informacje o profilu użytkownika, które zostały zwrócone przez wywołanie interfejsu API programu Microsoft Graph. Wyniki są wyświetlane w **wyniki wywołania interfejsu API** pole. Podstawowe informacje o token, który został uzyskany za pośrednictwem wywołania **AcquireTokenAsync** lub **AcquireTokenSilentAsync** powinny być widoczne w **informacji o tokenie** pole. Wyniki obejmują następujące właściwości:

|Właściwość  |Format  |Opis |
|---------|---------|---------|
|**Nazwa** |Imię i nazwisko użytkownika |Imię i nazwisko użytkownika.|
|**Nazwa użytkownika** |<span>user@domain.com</span> |Nazwa użytkownika, która jest używana do identyfikacji użytkownika.|
|**Wygaśnięcia tokenu** |Data/godzina |Czas, w którym token jest ważny. MSAL rozszerza datę wygaśnięcia, odnowienie tokenu w razie potrzeby.|
|**Token dostępu** |Ciąg |Tokenu ciąg, który jest wysyłany do HTTP żądań, które wymagają **autoryzacji** nagłówka.|

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i uprawnień delegowanych

Wymaga interfejsu API programu Microsoft Graph **user.read** zakresu odczytać profilu użytkownika. Ten zakres jest automatycznie dodawany domyślnie w każdej aplikacji, który jest zarejestrowany w portalu rejestracji. Inne interfejsy API dla programu Microsoft Graph, a także niestandardowych interfejsów API dla serwera zaplecza może wymagać dodatkowych zakresów. Wymaga interfejsu API programu Microsoft Graph **Calendars.Read** zakres, aby wyświetlić listę kalendarzy użytkownika.

Aby uzyskać dostęp do kalendarzy użytkownika w kontekście aplikacji, należy dodać **Calendars.Read** delegowane uprawnienia do informacji o rejestracji aplikacji. Następnie należy dodać **Calendars.Read** zakres do **acquireTokenSilent** wywołania. 

>[!NOTE]
>Użytkownik może zostać wyświetlony monit dla dodatkowych zgody, jak zwiększyć liczbę zakresów.

<!--end-collapse-->

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
