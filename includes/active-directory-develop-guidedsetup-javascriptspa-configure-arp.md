## <a name="add-the-applications-registration-information-to-your-app"></a>Dodawanie informacji o rejestracji aplikacji do aplikacji

W tym kroku należy skonfigurować adres URL przekierowania aplikacji informacje rejestracyjne, a następnie Dodaj identyfikator aplikacji do aplikacji JEDNOSTRONICOWEJ JavaScript.

### <a name="configure-redirect-url"></a>Skonfiguruj adres URL przekierowania

Skonfiguruj `Redirect URL` pola powyżej z adresem URL strony index.html oparte na serwerze sieci web, a następnie kliknij przycisk *aktualizacji*.


> #### <a name="visual-studio-instructions-for-obtaining-redirect-url"></a>Visual Studio instrukcje pobrania adres URL przekierowania
> Aby uzyskać adres URL przekierowania, postępuj zgodnie z instrukcjami poniżej:
> 1.    W *Eksploratora rozwiązań*, wybierz projekt i przyjrzyj się `Properties` okna (Jeśli nie widzisz okna właściwości, naciśnij klawisz `F4`)
> 2.    Skopiuj wartości z `URL` do Schowka:<br/> ![Właściwości projektu](media/active-directory-develop-guidedsetup-javascriptspa-configure/vs-project-properties-screenshot.png)<br />
> 3.    Wklej jako wartość `Redirect URL` górnej części strony, następnie kliknij przycisk`Update`

<p/>

> #### <a name="setting-redirect-url-for-python"></a>Adres URL przekierowania ustawienie dla języka Python
> Dla języka Python można ustawić sieci web port serwera przy użyciu wiersza polecenia. Ten przewodnik instalacji korzysta z portu 8080 dla odwołania, ale działanie może używać innych dostępnych portów. W każdym przypadku postępuj zgodnie z instrukcjami poniżej, aby zdefiniować adres URL przekierowania w aplikacji informacje rejestracyjne:<br/>
> Ustaw `http://localhost:8080/` jako `Redirect URL` górnej części strony lub użyj `http://localhost:[port]/` używania niestandardowego portu TCP (gdzie *[port]* jest niestandardowy numer portu TCP) i wybierz polecenie "Update"

### <a name="configure-your-javascript-spa-application"></a>Konfigurowanie aplikacji JEDNOSTRONICOWEJ JavaScript

1.  Utwórz plik o nazwie `msalconfig.js` zawierający informacje o rejestracji aplikacji. Jeśli używasz programu Visual Studio, wybierz projekt (folder główny projekt), kliknij prawym przyciskiem myszy i wybierz: `Add`  >  `New Item`  >  `JavaScript File`. Nadaj jej nazwę`msalconfig.js`
2.  Dodaj następujący kod do Twojej `msalconfig.js` pliku:

```javascript
var msalconfig = {
    clientID: "[Enter the application Id here]",
    redirectUri: location.origin
};
``` 
