### <a name="prerequisites"></a>Wymagania wstępne
Musi mieć [usługi Service Bus](https://azure.microsoft.com/services/service-bus/) konta.  

Zanim użyjesz swojego konta usługi Azure Service Bus w aplikacji logiki, należy zezwolić aplikacji logiki do łączenia się z kontem magistrali usługi. Na szczęście można w tym z aplikacji logiki w portalu Azure.  

Poniżej przedstawiono kroki, aby zezwolić aplikacji logiki do łączenia się z kontem usługi Service Bus:  

1. Aby utworzyć połączenie do usługi Service Bus w Projektancie aplikacji logiki, wybierz **Pokaż Microsoft zarządzanych interfejsów API** na liście rozwijanej. Następnie wprowadź **usługi magistrali** w polu wyszukiwania. Wybierz wyzwalacz lub akcji, którą chcesz użyć.  
    ![Obraz połączenia usługi Service Bus 1](./media/connectors-create-api-servicebus/servicebus-1.png)  
2. Jeśli nie utworzono wszystkie połączenia z usługi Service Bus przed możesz wyświetlony monit o podanie poświadczeń usługi Service Bus. Te poświadczenia są używane do autoryzacji aplikację logiki, aby nawiązać połączenie i dostęp do danych konta usługi Service Bus. Łącznik usługi Service Bus wymaga parametrów połączenia dla przestrzeni nazw usługi Service Bus. Wymagany jest również **Zarządzaj** uprawnienia. Sposób określić, czy jest parametrów połączenia dla przestrzeni nazw czy określone jednostki jest, jeśli zawiera `EntityPath` parametru. Jeśli tak, nie jest ciąg połączenia na odpowiednie dla aplikacji logiki.  
    ![Parametry połączenia magistrali usług](./media/connectors-create-api-servicebus/connectionstring.png)
3. Po otrzymaniu parametry połączenia dla przestrzeni nazw umożliwia połączenia interfejsu API w aplikacji logiki.  
    ![Obraz połączenia usługi Service Bus 2](./media/connectors-create-api-servicebus/servicebus-2.png)  
4. Zwróć uwagę, utworzono połączenie i wszystko jest teraz mógł kontynuować wykonywanie innych czynności w aplikacji logiki.  
    ![Obraz połączenia usługi Service Bus 3](./media/connectors-create-api-servicebus/servicebus-3.png)   

