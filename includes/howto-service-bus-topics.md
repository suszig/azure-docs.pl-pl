## Co to są tematy i subskrypcje usługi Service Bus?

Tematy i subskrypcje usługi Service Bus obsługują model komunikacji z użyciem *publikowania/subskrypcji* komunikatów. Podczas korzystania z tematów i subskrypcji składniki aplikacji rozproszonej nie komunikują się bezpośrednio ze sobą, lecz wymieniają komunikaty za pośrednictwem tematu, która działa jako pośrednik.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

W przeciwieństwie do kolejek usługi Service Bus, w których każdy komunikat jest przetwarzany przez jednego konsumenta, tematy i subskrypcje zapewniają formę komunikacji „jeden do wielu” z użyciem wzorca publikowania/subskrypcji. Istnieje możliwość zarejestrowania wielu subskrypcji danego tematu. Po wysłaniu komunikatu do tematu jest on następnie udostępniany poszczególnym subskrypcjom w celu niezależnej obsługi lub niezależnego przetworzenia.

Subskrypcja tematu przypomina wirtualną kolejkę, która odbiera kopie komunikatów wysłanych do tematu. Opcjonalnie można zarejestrować reguły filtrów dla tematu oparte na subskrypcji, które umożliwiają filtrowanie lub ograniczanie komunikatów odbieranych przez poszczególne subskrypcje.

Tematy i subskrypcje usługi Service Bus umożliwiają skalowanie i przetwarzanie bardzo dużej liczby komunikatów przez wielu użytkowników i wiele aplikacji.

## Tworzenie przestrzeni nazw

Aby rozpocząć korzystanie z tematów i subskrypcji usługi Service Bus na platformie Azure, należy najpierw utworzyć *przestrzeń nazw usługi*. Przestrzeń nazw zapewnia kontener określania zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji.

Aby utworzyć przestrzeń nazw:

1. Zaloguj się w witrynie [Azure Portal][].

2. W lewym okienku nawigacji portalu kliknij kolejno pozycje **Nowy**, **Integracja w przedsiębiorstwie** i **Service Bus**.

4. W oknie dialogowym **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.

5. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Podstawowa, Standardowa lub Premium).

7. W polu **Subskrypcja** wybierz subskrypcję platformy Azure, w której ma zostać utworzona przestrzeń nazw.

9. W polu **Grupa zasobów** wybierz istniejącą grupę zasobów, w której znajdzie się przestrzeń nazw, lub utwórz nową.      

8. W polu **Lokalizacja** wybierz kraj lub region, w którym powinna być hostowana przestrzeń nazw.

    ![Tworzenie przestrzeni nazw][create-namespace]

6. Kliknij przycisk **Utwórz**. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces aprowizacji zasobów dla konta w systemie może potrwać kilka minut.
 
### Uzyskiwanie poświadczeń

1. Na liście przestrzeni nazw kliknij nowo utworzoną nazwę przestrzeni nazw.
 
3. W bloku **Przestrzeń nazw usługi Service Bus** kliknij polecenie **Zasady dostępu współdzielonego**.

4. W bloku **Zasady dostępu współdzielonego** kliknij pozycję **RootManageSharedAccessKey**.

    ![połączenia — informacje][connection-info]

5. W bloku **Zasady: RootManageSharedAccessKey** kliknij przycisk kopiowania obok pozycji **Parametry połączenia — klucz podstawowy**, aby skopiować parametry połączenia do schowka w celu późniejszego użycia.

    ![połączenie — parametry][connection-string]

[Azure Portal]: https://portal.azure.com
[tworzenie — przestrzeń nazw]: ./media/howto-service-bus-topics/create-namespace.png
[połączenia — informacje]: ./media/howto-service-bus-topics/connection-info.png
[połączenie — parametry]: ./media/howto-service-bus-topics/connection-string.png




<!--HONumber=Sep16_HO3-->


