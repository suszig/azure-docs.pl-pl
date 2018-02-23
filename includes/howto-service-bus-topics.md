## <a name="what-are-service-bus-topics-and-subscriptions"></a>Co to są tematy i subskrypcje usługi Service Bus?
Tematy i subskrypcje usługi Service Bus obsługują model komunikacji z użyciem *publikowania/subskrypcji* komunikatów. Podczas korzystania z tematów i subskrypcji składniki aplikacji rozproszonej nie komunikują się bezpośrednio ze sobą, lecz wymieniają komunikaty za pośrednictwem tematu, która działa jako pośrednik.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

W przeciwieństwie do kolejek usługi Service Bus, w którym każdy komunikat jest przetwarzany przez jednego konsumenta, tematy i subskrypcje zapewniają formę "jeden do wielu" komunikacji przy użyciu wzorca publikowania/subskrypcji. Istnieje możliwość zarejestrowania wielu subskrypcji danego tematu. Po wysłaniu komunikatu do tematu jest on następnie udostępniany poszczególnym subskrypcjom w celu niezależnej obsługi lub niezależnego przetworzenia.

Subskrypcja tematu przypomina wirtualną kolejkę, która odbiera kopie komunikatów wysłanych do tematu. Opcjonalnie można zarejestrować reguły filtrów dla tematu oparte na subskrypcji. Reguły filtrowania umożliwiają filtrowanie lub ograniczanie komunikatów do tematu są odbierane przez poszczególne subskrypcje.

Tematy i subskrypcje Service Bus umożliwiają skalowanie i przetwarzanie dużej liczby komunikatów przez wielu użytkowników i aplikacje.

## <a name="create-a-namespace"></a>Tworzenie przestrzeni nazw
Aby rozpocząć korzystanie z tematów i subskrypcji usługi Service Bus na platformie Azure, należy najpierw utworzyć *przestrzeń nazw usługi*. Przestrzeń nazw zapewnia kontener określania zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji.

Aby utworzyć przestrzeń nazw:

1. Zaloguj się w witrynie [Azure Portal][Azure portal].
2. W okienku nawigacji po lewej stronie portalu kliknij **Utwórz zasób**, następnie kliknij przycisk **integracji przedsiębiorstwa**, a następnie kliknij przycisk **usługi Service Bus**.
3. W oknie dialogowym **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.
4. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Podstawowa, Standardowa lub Premium).
5. W polu **Subskrypcja** wybierz subskrypcję platformy Azure, w której ma zostać utworzona przestrzeń nazw.
6. W **grupy zasobów** pola, wybierz istniejącą grupę zasobów, w którym znajduje się obszar nazw lub Utwórz nową.      
7. W polu **Lokalizacja** wybierz kraj lub region, w którym powinna być hostowana przestrzeń nazw.
   
    ![Create namespace][create-namespace]
8. Kliknij przycisk **Utwórz**. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces aprowizacji zasobów dla konta w systemie może potrwać kilka minut.

### <a name="obtain-the-credentials"></a>Uzyskiwanie poświadczeń
1. Na liście przestrzeni nazw kliknij nowo utworzoną nazwę przestrzeni nazw.
2. W **przestrzeni nazw usługi Service Bus** okienku, kliknij przycisk **zasady dostępu współużytkowanego**.
3. W **zasady dostępu współużytkowanego** okienku, kliknij przycisk **RootManageSharedAccessKey**.
   
    ![connection-info][connection-info]
4. W **zasad: RootManageSharedAccessKey** okienku, kliknij kopię przycisk Dalej, aby **połączenia ciąg — podstawowy klucz**, aby skopiować parametry połączenia do Schowka do późniejszego użycia.
   
    ![connection-string][connection-string]

[Azure portal]: https://portal.azure.com
[create-namespace]: ./media/howto-service-bus-topics/create-namespace.png
[connection-info]: ./media/howto-service-bus-topics/connection-info.png
[connection-string]: ./media/howto-service-bus-topics/connection-string.png


