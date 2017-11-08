
## <a name="view-vms-scheduled-for-maintenance-in-the-portal"></a>Maszyny wirtualne widoku zaplanowane w celu przeprowadzenia konserwacji w portalu

Gdy zaplanowano wave planowanej konserwacji i powiadomienia są wysyłane, można zaobserwować listę maszyn wirtualnych, które ma wpływ wave zbliżającej się konserwacji. 

Można korzystać z portalu Azure i wyszukaj zaplanowane do obsługi maszyn wirtualnych.

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).

2. W obszarze nawigacji po lewej stronie, kliknij przycisk **maszyn wirtualnych**.

3. W okienku maszyny wirtualnej, kliknij **kolumn** przycisk, aby otworzyć listę dostępnych kolumn.

4. Wybierz i dodaj następujące kolumny:

   **Konserwacja** — Wyświetla stan konserwacji dla maszyny Wirtualnej. Potencjalne wartości są następujące:
      
      | Wartość | Opis |
      |-------|-------------|
      | Rozpocznij teraz | Maszyna wirtualna jest w oknie konserwacji samoobsługi, która pozwala na zainicjowanie obsługi samodzielnie. Poniżej dostępne są w sposób uruchamiania konserwacji na maszynie Wirtualnej | 
      | Zaplanowane | Maszyna wirtualna jest zaplanowane dla obsługi z opcją nie można zainicjować obsługi. Aby dowiedzieć się okna obsługi, wybierając okno zaplanowane automatycznie w tym widoku lub przez kliknięcie przycisku na maszynie Wirtualnej | 
      | ukończone | Pomyślnie zainicjowano i konserwację na maszynie Wirtualnej. | 
      | Pominięto| Wybrany do inicjowania obsługi nie pomyślnie. Azure anulował konserwacji dla maszyny Wirtualnej i ponownie go w późniejszym czasie | 
      | Spróbuj ponownie później| Wybrano opcję inicjowania obsługi i Azure nie był w stanie wykonać żądanie. W takim przypadku można spróbować ponownie w późniejszym czasie. | 
   
   **Konserwacja aktywnej** — przedstawia przedział czasu, gdy dla maszyn wirtualnych można uruchomić samodzielnie konserwacji.
   
   **Zaplanowanej konserwacji** — przedstawia przedział czasu, gdy Azure zostanie wykonany ponowny rozruch maszyny Wirtualnej w celu wykonania konserwacji. 




## <a name="notification-and-alerts-in-the-portal"></a>Powiadomienia i alerty w portalu

Azure komunikuje się harmonogram zaplanowanej konserwacji, wysyłając wiadomość e-mail do grupy właściciel i współwłaściciele subskrypcji. Można dodać dodatkowych adresatów i kanały do komunikacji przez utworzenie alerty dziennika aktywności platformy Azure. Aby uzyskać więcej informacji, zobacz [monitorowanie aktywności subskrypcji z dziennika aktywności platformy Azure] (../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. Zaloguj się w witrynie [Azure Portal](https://portal.azure.com).
2. W menu po lewej stronie wybierz **Monitor**. 
3. W **Monitor — dziennik aktywności** okienku wybierz **alerty**.
4. W **Monitor — alerty** okienku, kliknij przycisk **+ Dodaj alert dziennika aktywności**.
5. Uzupełnij informacje w **alert dziennika aktywności Dodaj** strony i upewnij się, że można ustawić następujące opcje **kryteria**: **typu**: konserwacji **stan**: Wszystkie (nie ustawiono stan aktywny lub rozwiązany) **poziom**: wszystkie
    
Aby dowiedzieć się więcej na temat konfigurowania alertów dotyczących działań w dzienniku, zobacz [utworzyć alerty dziennika aktywności](../articles/monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-vm-from-the-portal"></a>Obsługa uruchamiania na maszynie Wirtualnej z portalu

Podczas przeglądania szczegółów maszyny Wirtualnej, można zobaczyć więcej szczegółów dotyczących obsługi.  
U góry widoku szczegółów maszyny Wirtualnej zostaną dodane nowe wstążki powiadomień, jeśli maszyny Wirtualnej znajduje się w wave zaplanowanej konserwacji. Ponadto nowa opcja jest dodawany do uruchomienia konserwacji, jeśli to możliwe. 


Kliknij powiadomienie konserwacji, aby wyświetlić stronę konserwacji z bardziej szczegółowymi informacjami na zaplanowanej konserwacji. Z tego miejsca będzie mieć możliwość **start konserwacji** na maszynie Wirtualnej.

Po uruchomieniu obsługi zostanie uruchomiony ponownie maszynę wirtualną i stan konserwacji zostanie zaktualizowany w celu odzwierciedlenia wynik w ciągu kilku minut.

Nadal można wyświetlić okno, gdy maszyna wirtualna zostanie uruchomiony ponownie przez platformę Azure, jeśli pominięte okna, skąd mogą rozpocząć konserwacji. 
