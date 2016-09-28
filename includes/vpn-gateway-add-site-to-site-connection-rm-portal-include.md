1. Znajdź bramę sieci wirtualnej i kliknij przycisk **Wszystkie ustawienia**, aby otworzyć blok **Ustawienia**.

2. W bloku **Ustawienia** kliknij przycisk **Połączenia**, a następnie kliknij przycisk **Dodaj** znajdujący się na górze bloku, aby otworzyć blok **Dodaj połączenie**.

    ![Utwórz połączenie lokacja-lokacja](./media/vpn-gateway-add-site-to-site-connection-rm-portal-include/addconnection250.png)

3. W bloku **Dodaj połączenie** określ **nazwę** połączenia. 

4. W polu **Typ połączenia** wybierz opcję **Lokacja-lokacja (IPsec)**.

5. W polu **Brama sieci wirtualnej** wartość jest ustalona, ponieważ połączenie jest nawiązywane z tej bramy.

6. W polu **Brama sieci lokalnej** kliknij przycisk **Wybierz bramę sieci lokalnej** i wybierz bramę sieci lokalnej, która ma być używana. 

7. W polu **Klucz współdzielony** wartość musi być zgodna z wartością używaną do lokalnego urządzenia sieci VPN. Jeśli urządzenie sieci VPN w sieci lokalnej nie dostarcza klucza współdzielonego, można go utworzyć i wprowadzić w tym miejscu i na urządzeniu lokalnym. Ważne jest, aby klucze były ze sobą zgodne.

8. Pozostałe wartości w polach **Subskrypcja**, **Grupa zasobów** i **Lokalizacja** są ustalone.

9. Kliknij przycisk **OK**, aby utworzyć połączenie. Na ekranie zostanie wyświetlony napis *Tworzenie połączenia*.

10. Po nawiązaniu połączenia będzie ono widoczne w bloku **Połączenia** dla bramy.

    ![Utwórz połączenie lokacja-lokacja](./media/vpn-gateway-add-site-to-site-connection-rm-portal-include/connectionstatus450.png)



<!--HONumber=Sep16_HO3-->


