Poniżej przedstawiono sposób użycia **usługi Service Bus — po odebraniu wiadomości w kolejce** trigger, aby inicjować przepływ pracy aplikacji logiki, gdy nowy element zostanie wysłana do kolejki usługi Service Bus.  

> [!NOTE]
> Pojawi się zalogować za pomocą parametrów połączenia magistrali usług, jeśli nie utworzono jeszcze połączenia magistrali usług.  
> 
> 

1. W polu wyszukiwania w Projektancie aplikacji logiki, wprowadź **usługi magistrali**. Następnie wybierz **usługi Service Bus — po odebraniu wiadomości w kolejce** wyzwalacza.  
   ![Usługa Service Bus wyzwalacza obraz 1](./media/connectors-create-api-servicebus/trigger-1.png)   
2. **Po odebraniu wiadomości w kolejce** zostanie wyświetlone okno dialogowe.  
   ![Usługa Service Bus wyzwalacza obraz 2](./media/connectors-create-api-servicebus/trigger-2.png)   
3. Wprowadź nazwę kolejki usługi Service Bus ma wyzwalacz do monitorowania.   
   ![Usługa Service Bus wyzwalacza obrazu 3](./media/connectors-create-api-servicebus/trigger-3.png)   

W tym momencie została skonfigurowana z wyzwalaczem aplikacji logiki. Po odebraniu nowego elementu w wybranej kolejce wyzwalacza rozpocznie wykonywania innych wyzwalacze i akcje w przepływie pracy.    

