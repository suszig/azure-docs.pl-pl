
## <a name="test-your-code"></a>Testowanie kodu

Aby przetestować aplikację w programie Visual Studio, naciśnij klawisz **F5** do uruchomienia projektu. Przeglądarka otworzy http://<span></span>localhost: {port} lokalizacji i zostanie wyświetlony **Zaloguj się przy użyciu Microsoft** przycisku. Wybierz przycisk, aby rozpocząć proces logowania.

Jeśli wszystko jest gotowe do uruchomienia testu, korzystać z konta Microsoft Azure Active Directory (Azure AD) (konta firmowego lub szkolnego) lub osobiste konto Microsoft (<span>na żywo.</span> COM lub <span>programu outlook.</span> com) do logowania.

![Zaloguj się przy użyciu firmy Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin.png)
<br/><br/>
![Zaloguj się do swojego konta Microsoft](media/active-directory-develop-guidedsetup-aspnetwebapp-test/aspnetbrowsersignin2.png)

#### <a name="view-application-results"></a>Wyświetl wyniki aplikacji
Po zalogowaniu użytkownik jest przekierowywany do strony głównej witryny sieci Web. Strona główna jest adres URL HTTPS, określonego w aplikacji informacje rejestracyjne w portalu rejestracji aplikacji firmy Microsoft. Strona główna zawiera komunikat powitalny "Hello \<użytkownika >," łącze, aby się wylogować i link do wyświetlenia oświadczenia użytkownika. Łącze do oświadczenia użytkownika przechodzi do **autoryzacji** kontrolera, który został utworzony wcześniej.

### <a name="browse-to-see-the-users-claims"></a>Przeglądaj, aby wyświetlić oświadczenia użytkownika
Aby wyświetlić oświadczeń użytkownika, wybierz łącze, aby przejść do widoku kontrolera, który jest dostępny tylko dla użytkowników uwierzytelnionych.

#### <a name="view-the-claims-results"></a>Wyświetl wyniki oświadczeń
Po przejściu do widoku kontrolera, powinny być widoczne tabeli zawierającej podstawowe właściwości dla użytkownika:

|Właściwość |Wartość |Opis |
|---|---|---|
|**Nazwa** |Imię i nazwisko użytkownika | Imię i nazwisko użytkownika.
|**Nazwa użytkownika** |użytkownika<span>@domain.com</span> | Nazwa użytkownika, która jest używana do identyfikacji użytkownika.
|**Temat** |Temat |Ciąg, który unikatowo identyfikuje użytkownika sieci web.|
|**Identyfikator dzierżawy** |Identyfikator GUID | A **guid** który w unikatowy sposób identyfikuje użytkownika usługi Azure AD organizacji.|

Ponadto powinien zostać wyświetlony spis wszystkich oświadczenia, które są w żądaniu uwierzytelnienia. Aby uzyskać więcej informacji, zobacz [lista oświadczeń, które znajdują się w Token identyfikator usługi Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims).


### <a name="test-access-to-a-method-that-has-an-authorize-attribute-optional"></a>Testowanie dostępu do metody, która ma atrybut autoryzacji (opcjonalnie)
Aby przetestować dostęp do **autoryzacji** kontroler oświadczeń użytkownika jako użytkownik anonimowy, wykonaj następujące kroki:
1. Wybierz łącze Wyloguj użytkownika i ukończyć proces wylogowywania.
2. W przeglądarce, wpisz http://<span></span>localhost: {port} / uwierzytelnionego dostępu kontrolerze, który jest chroniony za pomocą do **autoryzacji** atrybutu.

#### <a name="expected-results-after-access-to-a-protected-controller"></a>Oczekiwanych rezultatów po dostęp do chronionych kontrolera
Zostanie wyświetlony monit o uwierzytelniania w celu użycia widoku chronionego kontrolera.

## <a name="additional-information"></a>Dodatkowe informacje

<!--start-collapse-->
### <a name="protect-your-entire-website"></a>Ochrona całej witryny sieci Web
Do ochrony całej witryny sieci Web, w **Global.asax** plików, dodawanie **klasy AuthorizeAttribute** atrybutu **GlobalFilters** Filtruj w **aplikacji _Uruchom** metody:

```csharp
GlobalFilters.Filters.Add(new AuthorizeAttribute());
```
<!--end-collapse-->

### <a name="restrict-sign-in-access-to-your-application"></a>Ograniczanie dostępu do logowania do aplikacji
Domyślnie kont osobistych, takich jak outlook.com, live.com i inne zalogować się do aplikacji. Konta służbowe w organizacji, które są zintegrowane z usługą Azure AD można też zarejestrować się domyślnie.

Aby ograniczyć rejestrowania dostępu użytkowników dla aplikacji, dostępnych jest kilka opcji.

#### <a name="restrict-access-to-a-single-organization"></a>Ograniczanie dostępu do jednej organizacji
Możesz ograniczyć dostęp do logowania w aplikacji tylko konta użytkowników, które znajdują się w pojedynczej organizacji usługi Azure AD:
1. W **web.config** pliku, zmień wartość atrybutu **dzierżawy** parametru. Zmień wartość **typowe** nazwy dzierżawy w organizacji, takich jak **contoso.onmicrosoft.com**.
2. W Twojej **uruchamiania OWIN** klasy, ustaw **ValidateIssuer** argument **true**.

#### <a name="restrict-access-to-a-list-of-organizations"></a>Ograniczanie dostępu do wykazu organizacji
Możesz ograniczyć dostęp do logowania do kont użytkowników tylko, które znajdują się w organizacji usługi Azure AD, która znajduje się na liście dozwolonych organizacji:
1. W **web.config** plików w sieci **uruchamiania OWIN** klasy, ustaw **ValidateIssuer** argument **true**.
2. Ustaw wartość **ValidIssuers** parametru do listy dozwolonych organizacji.

#### <a name="use-a-custom-method-to-validate-issuers"></a>Umożliwia sprawdzanie poprawności wystawców niestandardowe — metoda
Można zaimplementować niestandardowych metodę do sprawdzania poprawności wystawcy przy użyciu **IssuerValidator** parametru. Aby uzyskać więcej informacji na temat używania tego parametru, przeczytaj o [klasy TokenValidationParameters](https://msdn.microsoft.com/library/system.identitymodel.tokens.tokenvalidationparameters.aspx) w witrynie MSDN.

[!INCLUDE  [Help and support](./active-directory-develop-help-support-include.md)]
