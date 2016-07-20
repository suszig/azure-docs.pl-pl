## Co to są tematy i subskrypcje usługi Service Bus?

Tematy i subskrypcje usługi Service Bus obsługują model komunikacji z użyciem *publikowania/subskrypcji* komunikatów. Podczas korzystania z tematów i subskrypcji składniki aplikacji rozproszonej nie komunikują się bezpośrednio ze sobą, lecz wymieniają komunikaty za pośrednictwem tematu, która działa jako pośrednik.

![TopicConcepts](./media/howto-service-bus-topics/sb-topics-01.png)

W przeciwieństwie do kolejek usługi Service Bus, w których każdy komunikat jest przetwarzany przez jednego konsumenta, tematy i subskrypcje zapewniają formę komunikacji „jeden do wielu” z użyciem wzorca publikowania/subskrypcji. Istnieje możliwość zarejestrowania wielu subskrypcji danego tematu. Po wysłaniu komunikatu do tematu jest on następnie udostępniany poszczególnym subskrypcjom w celu niezależnej obsługi lub niezależnego przetworzenia.

Subskrypcja tematu przypomina wirtualną kolejkę, która odbiera kopie komunikatów wysłanych do tematu. Opcjonalnie można zarejestrować reguły filtrów dla tematu oparte na subskrypcji, które umożliwiają filtrowanie lub ograniczanie komunikatów odbieranych przez poszczególne subskrypcje.

Tematy i subskrypcje usługi Service Bus umożliwiają skalowanie i przetwarzanie bardzo dużej liczby komunikatów przez wielu użytkowników i wiele aplikacji.

## Tworzenie przestrzeni nazw

Aby rozpocząć korzystanie z tematów i subskrypcji usługi Service Bus na platformie Azure, należy najpierw utworzyć *przestrzeń nazw usługi*. Przestrzeń nazw zapewnia kontener zakresu na potrzeby adresowania zasobów usługi Service Bus w aplikacji.

Aby utworzyć przestrzeń nazw:

1.  Zaloguj się do [klasycznego portalu Azure][].

2.  W lewym okienku nawigacji portalu kliknij pozycję **Service Bus**.

3.  W dolnym okienku portalu kliknij pozycję **Utwórz**.   
    ![][0]

4.  W oknie dialogowym **Add a new namespace** (Dodawanie nowej przestrzeni nazw) wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.   
    ![][2]

5.  Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz kraj lub region, w którym przestrzeń nazw ma być hostowana (upewnij się, że używasz tego samego kraju/regionu, w którym są wdrażane zasoby obliczeniowe).

    > [AZURE.IMPORTANT] Wybierz **ten sam region**, który zamierzasz wybrać do wdrożenia aplikacji. Zapewni to najlepszą wydajność.

6.  W pozostałych polach okna dialogowego pozostaw wartości domyślne (**Obsługa komunikatów** i **Warstwa Standardowa**), a następnie kliknij znacznik wyboru OK. W systemie zostanie utworzona i włączona przestrzeń nazw. Proces inicjowania obsługi administracyjnej zasobów dla konta w systemie może potrwać kilka minut.

    ![][6]

## Uzyskiwanie domyślnych poświadczeń zarządzania dla przestrzeni nazw

W celu wykonywania operacji zarządzania, takich jak tworzenie tematu lub subskrypcji w nowej przestrzeni nazw, należy uzyskać poświadczenia zarządzania dla przestrzeni nazw. Możesz uzyskać te poświadczenia w portalu.

### Uzyskiwanie poświadczeń zarządzania w portalu

1.  W lewym okienku nawigacji kliknij węzeł **Service Bus**, aby wyświetlić listę dostępnych przestrzeni nazw:   
    ![][0]

2.  Wybierz nowo utworzoną przestrzeń nazw z wyświetlonej listy:   
    ![][3]

3.  Kliknij pozycję **Informacje o połączeniu**.   
    ![][4]

4.  W oknie dialogowym zapewniającym **dostęp do informacji o połączeniu** znajdź parametry połączenia, które zawierają klucz SAS i nazwę klucza. Zanotuj te wartości, ponieważ te informacje będą później potrzebne do wykonania operacji związanych z przestrzenią nazw. 


  [klasycznego portalu Azure]: http://manage.windowsazure.com
  [0]: ./media/howto-service-bus-topics/sb-queues-13.png
  [2]: ./media/howto-service-bus-topics/sb-queues-04.png
  [3]: ./media/howto-service-bus-topics/sb-queues-09.png
  [4]: ./media/howto-service-bus-topics/sb-queues-06.png
  
  [6]: ./media/howto-service-bus-topics/getting-started-multi-tier-27.png




<!--HONumber=Jun16_HO2-->


