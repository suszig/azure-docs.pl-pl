
## <a name="test-your-code"></a>Testowanie kodu

1. Wdrażanie kodu na urządzeniu/emulatorze.
2. Jeśli wszystko jest gotowe do testowania aplikacji, z konta Microsoft Azure Active Directory (konta firmowego lub szkolnego) lub konta Microsoft (live.com, outlook.com) można używać do logowania. 

    ![Testowanie aplikacji](media/active-directory-develop-guidedsetup-android-test/mainwindow.png)
    <br/><br/>
    ![Wprowadź nazwę użytkownika i hasło](media/active-directory-develop-guidedsetup-android-test/usernameandpassword.png)

### <a name="provide-consent-for-application-access"></a>Podaj zgody na dostęp do aplikacji
Podczas pierwszego logowania się do aplikacji, zostanie wyświetlony monit o wyrażenie zgody na umożliwić aplikacji dostęp do Twojego profilu i logowanie się w: 

![Wyrażenie zgody na dostęp do aplikacji](media/active-directory-develop-guidedsetup-android-test/androidconsent.png)


### <a name="view-application-results"></a>Wyświetl wyniki aplikacji
Po zalogowaniu, powinny być widoczne wyniki, które są zwracane przez wywołanie interfejsu API programu Microsoft Graph. Wywołanie interfejsu API programu Microsoft Graph **mnie** punktu końcowego zwraca https://graph.microsoft.com/v1.0/me profilu użytkownika. Listę typowych Microsoft Graph punktów końcowych, zobacz [dokumentacja dla deweloperów firmy Microsoft Graph API](https://developer.microsoft.com/graph/docs#common-microsoft-graph-queries).

<!--start-collapse-->
### <a name="more-information-about-scopes-and-delegated-permissions"></a>Więcej informacji o zakresach i uprawnień delegowanych

Wymaga interfejsu API programu Microsoft Graph **user.read** zakresu odczytać profilu użytkownika. Ten zakres jest automatycznie dodawany domyślnie w każdej aplikacji, który jest zarejestrowany w portalu rejestracji. Inne interfejsy API dla programu Microsoft Graph, a także niestandardowych interfejsów API dla serwera zaplecza może wymagać dodatkowych zakresów. Wymaga interfejsu API programu Microsoft Graph **Calendars.Read** zakres, aby wyświetlić listę kalendarzy użytkownika.

Aby uzyskać dostęp do kalendarzy użytkownika w kontekście aplikacji, należy dodać **Calendars.Read** delegowane uprawnienia do informacji o rejestracji aplikacji. Następnie należy dodać **Calendars.Read** zakres do **acquireTokenSilent** wywołania. 

>[!NOTE]
>Użytkownik może zostać wyświetlony monit dla dodatkowych zgody, jak zwiększyć liczbę zakresów.

<!--end-collapse-->

[!INCLUDE  [Help and support](active-directory-develop-help-support-include.md)]
