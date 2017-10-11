W tym kroku możesz utworzyć regułę zapory można otworzyć port sondy równoważeniem obciążenia punktu końcowego (określone wcześniej 59999) i innej reguły, aby otworzyć port odbiornika grupy dostępności. Ponieważ utworzono punkt końcowy z równoważeniem obciążenia na maszynach wirtualnych, które zawierają repliki grupy dostępności, należy otworzyć port sondy i port odbiornika w odpowiednich maszynach wirtualnych.

1. Na maszynach wirtualnych, które obsługują replik, należy uruchomić **Zapora systemu Windows z zabezpieczeniami zaawansowanymi**.

2. Kliknij prawym przyciskiem myszy **reguły ruchu przychodzącego**, a następnie kliknij przycisk **nową regułę**.

3. Na **typ reguły** wybierz pozycję **portu**, a następnie kliknij przycisk **dalej**.

4. Na **protokoły i porty** wybierz pozycję **TCP**, typ **59999** w **określone porty lokalne** , a następnie kliknij przycisk  **Następny**.

5. Na **akcji** Zachowaj **zezwalały na połączenie** zaznaczone, a następnie kliknij przycisk **dalej**.

6. Na **profilu** Zaakceptuj ustawienia domyślne, a następnie kliknij pozycję **dalej**.

7. Na **nazwa** strony w **nazwa** tekst Określ nazwę reguły, takie jak **zawsze na sondowania Port odbiornika**, a następnie kliknij przycisk **Zakończ**.

8. Powtórz te czynności na port odbiornika grupy dostępności (jak określono w parametrze $EndpointPort skryptu), a następnie określ nazwę odpowiednią regułę, takich jak **zawsze na Port odbiornika**.

