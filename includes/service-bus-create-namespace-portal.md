Aby rozpocząć korzystanie z obsługi wiadomości usługi Service Bus na platformie Azure, musisz najpierw utworzyć przestrzeń nazw o nazwie, która jest unikatowa w obrębie platformy Azure. Przestrzeń nazw zapewnia kontener określania zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji.

Aby utworzyć przestrzeń nazw:

1. Zaloguj się w witrynie [Azure Portal][Azure portal].
2. W okienku nawigacji po lewej stronie portalu kliknij **+ Utwórz zasób**, następnie kliknij przycisk **integracji przedsiębiorstwa**, a następnie kliknij przycisk **usługi Service Bus**.
3. W oknie dialogowym **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.
4. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Podstawowa, Standardowa lub Premium).
5. W polu **Subskrypcja** wybierz subskrypcję platformy Azure, w której ma zostać utworzona przestrzeń nazw.
6. W polu **Grupa zasobów** wybierz istniejącą grupę zasobów, w której znajdzie się przestrzeń nazw, lub utwórz nową.      
7. W polu **Lokalizacja** wybierz kraj lub region, w którym powinna być hostowana przestrzeń nazw.
   
    ![Create namespace][create-namespace]
8. Kliknij pozycję **Utwórz**. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces aprowizacji zasobów dla konta w systemie może potrwać kilka minut.

### <a name="obtain-the-management-credentials"></a>Uzyskiwanie poświadczeń zarządzania
Automatyczne tworzenie nowej przestrzeni nazw generuje początkowej reguły dostępu sygnatury dostępu Współdzielonego przy skojarzone pary kluczy podstawowych i pomocniczych, że każdy przyznaje pełną kontrolę nad wszystkimi aspektami przestrzeni nazw. Zobacz [magistrali usług uwierzytelniania i autoryzacji](../articles/service-bus-messaging/service-bus-authentication-and-authorization.md) informacji o tym, jak tworzyć dodatkowe reguły z więcej ograniczone prawa do regularnych nadawcami a odbiornikami. Aby skopiować początkową regułę, wykonaj następujące kroki: 

1. Na liście przestrzeni nazw kliknij nowo utworzoną nazwę przestrzeni nazw.
2. W oknie przestrzeni nazw kliknij **zasady dostępu współużytkowanego**.
3. W **zasady dostępu współużytkowanego** kliknij **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. W **zasad: RootManageSharedAccessKey** , kliknij kopię przycisk Dalej, aby **połączenia ciąg — podstawowy klucz**, aby skopiować parametry połączenia do Schowka do późniejszego użycia. Wklej tę wartość do Notatnika lub innej tymczasowej lokalizacji.
   
    ![connection-string][connection-string]

5. Powtórz poprzedni krok, kopiując i wklejając wartość pozycji **Klucz podstawowy** w lokalizacji tymczasowej do późniejszego użycia.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com
