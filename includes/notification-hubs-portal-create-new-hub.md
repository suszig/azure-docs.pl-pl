

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. Wybierz pozycję **Nowe** > **Sieci Web i mobilność** > **Centrum powiadomień**.
   
      ![Azure Portal — tworzenie centrum powiadomień](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-create.png)
      
3. W polu **Centrum powiadomień** wpisz unikatową nazwę. Wybierz odpowiednie pozycje w polach **Region**, **Subskrypcja** i **Grupa zasobów** (jeśli już ją masz). 
   
      Jeśli nie masz jeszcze przestrzeni nazw magistrali usług, możesz użyć nazwy domyślnej, która jest tworzona na podstawie nazwy centrum (jeśli ta nazwa przestrzeni nazw jest dostępna).
    
      Jeśli masz już przestrzeń nazw magistrali usług, w której chcesz utworzyć centrum, wykonaj następujące kroki:

    a. W obszarze **Przestrzeń nazw** wybierz link **Wybierz istniejącą**. 
   
    b. Wybierz pozycję **Utwórz**.
   
      ![Azure Portal — ustawianie właściwości centrum powiadomień](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-settings.png)

4. Po utworzeniu przestrzeni nazw i centrum powiadomień otwórz je, wybierając pozycję **Wszystkie zasoby**, a następnie wybierz z listy utworzone centrum powiadomień. 
   
      ![Azure Portal — strona portalu centrum powiadomień](./media/notification-hubs-portal-create-new-hub/notification-hubs-azure-portal-resources.png)

5. Wybierz z listy pozycję **Zasady dostępu**. Zanotuj dwa parametry połączenia, które są dla Ciebie dostępne. Będą one potrzebne później do obsługi powiadomień push.

      >[!IMPORTANT]
      >**NIE UŻYWAJ** elementu DefaultFullSharedAccessSignature w aplikacji. Jest on przeznaczony tylko do użycia w zapleczu.
      >
   
      ![Azure Portal — parametry połączenia centrum powiadomień](./media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png)

