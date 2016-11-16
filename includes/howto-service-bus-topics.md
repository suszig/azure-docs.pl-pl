## <a name="what-are-service-bus-topics-and-subscriptions"></a>Co to są tematy i subskrypcje usługi Service Bus?
Tematy i subskrypcje usługi Service Bus obsługują model komunikacji z użyciem *publikowania/subskrypcji* komunikatów. Podczas korzystania z tematów i subskrypcji składniki aplikacji rozproszonej nie komunikują się bezpośrednio ze sobą, lecz wymieniają komunikaty za pośrednictwem tematu, która działa jako pośrednik.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

W przeciwieństwie do kolejek usługi Service Bus, w których każdy komunikat jest przetwarzany przez jednego konsumenta, tematy i subskrypcje zapewniają formę komunikacji „jeden do wielu” z użyciem wzorca publikowania/subskrypcji. Istnieje możliwość zarejestrowania wielu subskrypcji danego tematu. Po wysłaniu komunikatu do tematu jest on następnie udostępniany poszczególnym subskrypcjom w celu niezależnej obsługi lub niezależnego przetworzenia.

Subskrypcja tematu przypomina wirtualną kolejkę, która odbiera kopie komunikatów wysłanych do tematu. Opcjonalnie można zarejestrować reguły filtrów dla tematu oparte na subskrypcji, które umożliwiają filtrowanie lub ograniczanie komunikatów odbieranych przez poszczególne subskrypcje.

Tematy i subskrypcje usługi Service Bus umożliwiają skalowanie i przetwarzanie bardzo dużej liczby komunikatów przez wielu użytkowników i wiele aplikacji.

## <a name="create-a-namespace"></a>Tworzenie przestrzeni nazw
Aby rozpocząć korzystanie z tematów i subskrypcji usługi Service Bus na platformie Azure, należy najpierw utworzyć *przestrzeń nazw usługi*. Przestrzeń nazw zapewnia kontener określania zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji.

Aby utworzyć przestrzeń nazw:

1. Zaloguj się w witrynie [Azure Portal][Azure Portal].
2. W lewym okienku nawigacji portalu kliknij kolejno pozycje **Nowy**, **Integracja w przedsiębiorstwie** i **Service Bus**.
3. W oknie dialogowym **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.
4. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Podstawowa, Standardowa lub Premium).
5. W polu **Subskrypcja** wybierz subskrypcję platformy Azure, w której ma zostać utworzona przestrzeń nazw.
6. W polu **Grupa zasobów** wybierz istniejącą grupę zasobów, w której znajdzie się przestrzeń nazw, lub utwórz nową.      
7. W polu **Lokalizacja** wybierz kraj lub region, w którym powinna być hostowana przestrzeń nazw.
   
    ![Create namespace][create-namespace]
8. Kliknij przycisk **Utwórz**. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces aprowizacji zasobów dla konta w systemie może potrwać kilka minut.

### <a name="obtain-the-credentials"></a>Uzyskiwanie poświadczeń
1. Na liście przestrzeni nazw kliknij nowo utworzoną nazwę przestrzeni nazw.
2. W bloku **Przestrzeń nazw usługi Service Bus** kliknij polecenie **Zasady dostępu współdzielonego**.
3. W bloku **Zasady dostępu współdzielonego** kliknij pozycję **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. W bloku **Zasady: RootManageSharedAccessKey** kliknij przycisk kopiowania obok pozycji **Parametry połączenia — klucz podstawowy**, aby skopiować parametry połączenia do schowka w celu późniejszego użycia.
   
    ![connection-string][connection-string]

[Azure Portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png




<!--HONumber=Nov16_HO2-->


