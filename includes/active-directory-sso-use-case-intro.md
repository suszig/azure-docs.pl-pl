Organizacji jest używany więcej [oprogramowanie jako usługa (SaaS)](https://azure.microsoft.com/overview/what-is-saas/) aplikacji pod kątem wydajności ponieważ technologii w chmurze i narzędzia stają się coraz bardziej dostępny. Wraz z rozwojem liczba aplikacji SaaS, staje się trudne, Administratorzy mogą zarządzać kontami i prawa dostępu i do różnych haseł dla użytkowników. Tworzy dodatkowej pracy i jest mniej bezpieczne indywidualnie Zarządzanie tych aplikacji.

* Pracownicy, którzy mają do śledzenia wielu haseł zwykle użycie mniej bezpiecznych metod do zapamiętania, zapisanie hasła albo przy użyciu takich samych haseł dla wielu kont.
* Gdy dociera do pracownika lub jeden pozostawia, ich kont musi być indywidualnie udostępnione lub usuwane.
* Ponadto pracownicy może zostać uruchomiony przy użyciu aplikacji SaaS w pracy bez pośrednictwa IT, co oznacza tworzysz własnych kont w systemach, które Administratorzy IT nie zostały zatwierdzone i nie są monitorowania.  

Logowanie jednokrotne (SSO) jest rozwiązanie wszystkie te problemy. Jest to najprostszy sposób Zarządzaj wieloma aplikacjami i Zapewnij użytkownikom spójne środowisko logowania jednokrotnego. Azure Active Directory (Azure AD) zapewnia niezawodne rozwiązanie logowania jednokrotnego i ma wiele dostępnych wstępnie zintegrowanych aplikacji, z samouczków dla administratorów szybko skonfigurować nową aplikację i rozpocząć Inicjowanie obsługi użytkowników.

## <a name="how-does-azure-active-directory-integrate-apps"></a>Jak Azure Active Directory integracji aplikacji?
Usługi Azure AD umożliwia integrację aplikacji i elastycznie kont. Można to zrobić przy użyciu jednej z dwóch metod.

* Jeśli aplikacja jest wstępnie zintegrowanych aplikacji galerii, można przejść za pomocą tego portalu, aby skonfigurować aplikacje i ustawienia, aby umożliwić logowanie Jednokrotne. Dla dowolnej aplikacji galerii, możesz rozpocząć pracę, wykonując proste instrukcje krok po kroku przedstawiony w galerii aplikacji i w portalu Azure do włączenia logowania jednokrotnego.
* Jeśli aplikacja nie jest w galerii, możesz nadal skonfigurować większość aplikacji w usłudze Azure AD jako niestandardowych aplikacji. Wymaga to bardziej technicznych wiedzę do skonfigurowania. Możesz dodać dowolnej aplikacji, który obsługuje SAML 2.0 jako aplikacji federacyjnych lub dowolnej aplikacji, która jest oparty na języku HTML strony logowania jako aplikację hasło logowania jednokrotnego.

W przypadku, gdy utworzono swoje własne konta dla aplikacji SaaS, które nie są zarządzane przez użytkowników IT, [Cloud App Discovery](../articles/active-directory/active-directory-cloudappdiscovery-whatis.md) narzędzie zapewnia rozwiązanie. To narzędzie monitoruje ruch sieci web, aby zidentyfikować aplikacje, które są używane w całej organizacji, a także liczbę osób przy użyciu każdego z nich. IT można użyć tych informacji, aby dowiedzieć się, jakie aplikacje, użytkownicy preferowane i zdecydować, które z integracji usługi Azure AD na potrzeby logowania jednokrotnego.  

Podczas integracji aplikacji z usługą Azure AD, możesz mapować tożsamości aplikacji ustalonych użytkowników do ich odpowiednich tożsamości usługi Azure AD.  

