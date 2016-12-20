## <a name="create-an-event-hub"></a>Tworzenie centrum zdarzeń
1. Zaloguj się do witryny [Azure Portal][Azure Portal], a następnie kliknij pozycję **Nowy** w lewym górnym rogu ekranu.
2. Kliknij pozycję **Internet rzeczy**, a następnie kliknij pozycję **Event Hubs**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub9.png)
3. W bloku **Tworzenie przestrzeni nazw** wprowadź nazwę przestrzeni nazw. System od razu sprawdza, czy nazwa jest dostępna.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub1.png)
4. Po upewnieniu się, że nazwa przestrzeni nazw jest dostępna, wybierz warstwę cenową (Podstawowa lub Standardowa). Ponadto wybierz subskrypcję platformy Azure, grupę zasobów i lokalizację, w której chcesz utworzyć zasób. 
5. Kliknij pozycję **Utwórz**, aby utworzyć przestrzeń nazw.
6. Na liście przestrzeni nazw usługi Event Hubs kliknij nowo utworzoną przestrzeń nazw.      
   
    ![](./media/event-hubs-create-event-hub/create-event-hub2.png)
7. W bloku przestrzeni nazw kliknij pozycję **Centra zdarzeń**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub3.png)
8. Kliknij pozycję **Dodaj centrum zdarzeń** w górnej części bloku.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub4.png)
9. Wpisz nazwę centrum zdarzeń, a następnie kliknij pozycję **Utwórz**.
   
    ![](./media/event-hubs-create-event-hub/create-event-hub5.png)
10. Na liście centrów zdarzeń kliknij nazwę nowo utworzonego centrum zdarzeń. 
    
     ![](./media/event-hubs-create-event-hub/create-event-hub6.png)
11. W bloku przestrzeni nazw (nie w bloku centrum zdarzeń) kliknij pozycję **Zasady dostępu współdzielonego**, a następnie kliknij pozycję **RootManageSharedAccessKey**.
    
     ![](./media/event-hubs-create-event-hub/create-event-hub7.png)
12. Kliknij przycisk kopiowania, aby skopiować parametry połączenia **RootManageSharedAccessKey** do schowka. Zapisz te parametry połączenia, aby użyć ich w dalszej części samouczka.
    
     ![](./media/event-hubs-create-event-hub/create-event-hub8.png)

Utworzono centrum zdarzeń i uzyskano parametry połączenia potrzebne do wysyłania i odbierania zdarzeń.

[Azure Portal]: https://portal.azure.com/

<!--HONumber=Nov16_HO4-->


