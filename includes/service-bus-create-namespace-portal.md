## <a name="create-a-service-namespace"></a>Tworzenie przestrzeni nazw usługi

Aby rozpocząć korzystanie z kolejek usługi Service Bus na platformie Azure, należy najpierw utworzyć przestrzeń nazw. Przestrzeń nazw zapewnia kontener określania zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji. 

Aby utworzyć przestrzeń nazw:

1. Zaloguj się w witrynie [Azure Portal][Azure portal].
2. W lewym okienku nawigacji portalu kliknij kolejno pozycje **Nowy**, **Integracja w przedsiębiorstwie** i **Service Bus**.
3. W oknie dialogowym **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.
4. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Podstawowa, Standardowa lub Premium).
5. W polu **Subskrypcja** wybierz subskrypcję platformy Azure, w której ma zostać utworzona przestrzeń nazw.
6. W polu **Grupa zasobów** wybierz istniejącą grupę zasobów, w której znajdzie się przestrzeń nazw, lub utwórz nową.      
7. W polu **Lokalizacja** wybierz kraj lub region, w którym powinna być hostowana przestrzeń nazw.
   
    ![Create namespace][create-namespace]
8. Kliknij pozycję **Utwórz**. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces aprowizacji zasobów dla konta w systemie może potrwać kilka minut.

### <a name="obtain-the-management-credentials"></a>Uzyskiwanie poświadczeń zarządzania
1. Na liście przestrzeni nazw kliknij nowo utworzoną nazwę przestrzeni nazw.
2. W bloku przestrzeni nazw usługi Service Bus kliknij pozycję **Zasady dostępu współdzielonego**.
3. W bloku **Zasady dostępu współdzielonego** kliknij pozycję **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. W bloku **Zasady: RootManageSharedAccessKey** kliknij przycisk kopiowania obok pozycji **Parametry połączenia — klucz podstawowy**, aby skopiować parametry połączenia do schowka w celu późniejszego użycia. Wklej tę wartość do Notatnika lub innej tymczasowej lokalizacji.
   
    ![connection-string][connection-string]

5. Powtórz poprzedni krok, kopiując i wklejając wartość pozycji **Klucz podstawowy** w lokalizacji tymczasowej do późniejszego użycia.

<!--Image references-->

[create-namespace]: ./media/service-bus-create-namespace-portal/create-namespace.png
[connection-info]: ./media/service-bus-create-namespace-portal/connection-info.png
[connection-string]: ./media/service-bus-create-namespace-portal/connection-string.png
[Azure portal]: https://portal.azure.com

<!--HONumber=Feb17_HO2-->


