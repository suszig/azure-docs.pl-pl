1. Zaloguj się w witrynie [Azure Portal][].

2. W lewym okienku nawigacji portalu kliknij kolejno pozycje **Nowy**, **Integracja w przedsiębiorstwie** i **Service Bus**.

4. W oknie dialogowym **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.

5. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Podstawowa, Standardowa lub Premium).

7. W polu **Subskrypcja** wybierz subskrypcję platformy Azure, w której ma zostać utworzona przestrzeń nazw.

9. W polu **Grupa zasobów** wybierz istniejącą grupę zasobów, w której znajdzie się przestrzeń nazw, lub utwórz nową.      

8. W polu **Lokalizacja** wybierz kraj lub region, w którym powinna być hostowana przestrzeń nazw.

    ![Tworzenie przestrzeni nazw][create-namespace]

6. Kliknij przycisk **Utwórz**. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces aprowizacji zasobów dla konta w systemie może potrwać kilka minut.
 
### Uzyskiwanie poświadczeń zarządzania

1. Na liście przestrzeni nazw kliknij nowo utworzoną nazwę przestrzeni nazw.
 
3. W bloku **Przestrzeń nazw usługi Service Bus** kliknij polecenie **Zasady dostępu współdzielonego**.

4. W bloku **Zasady dostępu współdzielonego** kliknij pozycję **RootManageSharedAccessKey**.

    ![połączenia — informacje][connection-info]

5. W bloku **Zasady: RootManageSharedAccessKey** kliknij przycisk kopiowania obok pozycji **Parametry połączenia — klucz podstawowy**, aby skopiować parametry połączenia do schowka w celu późniejszego użycia.

    ![połączenie — parametry][connection-string]

<!--Image references-->

[tworzenie — przestrzeń nazw]: ./media/service-bus-create-namespace-portal/create-namespace.png
[połączenia — informacje]: ./media/service-bus-create-namespace-portal/connection-info.png
[połączenie — parametry]: ./media/service-bus-create-namespace-portal/connection-string.png

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[Azure Portal]: https://portal.azure.com


<!--HONumber=sep16_HO1-->


