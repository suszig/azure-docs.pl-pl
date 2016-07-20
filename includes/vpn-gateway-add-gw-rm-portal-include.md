1. W portalu przejdź do opcji **Nowy**, a następnie **Sieci**. Wybierz z listy pozycję **Brama sieci wirtualnej**.

    ![Brama](./media/vpn-gateway-add-gw-rm-portal-include/creategw250.png)

2. W bloku **Utwórz bramę sieci wirtualnej** podaj nazwę bramy w polu **Nazwa**. Nie chodzi o nazwę podsieci bramy. Jest to nazwa obiektu bramy.
 
3. Następnie wybierz sieć wirtualną przewidzianą do wdrożenia tej bramy. Kliknij strzałkę, aby otworzyć blok **Wybierz sieć wirtualną**, a następnie kliknij sieć wirtualną. Aby sieć wirtualna była widoczna na liście, musi już mieć prawidłową podsieć bramy.

4. Wybierz publiczny adres IP. Kliknij strzałkę, aby otworzyć **blok Wybierz publiczny adres IP**. Następnie kliknij przycisk **Utwórz nowy**, aby otworzyć **blok Utwórz publiczny adres IP**. Wprowadź nazwę publicznego adresu IP. Zwróć uwagę, że nie jest to pytanie o adres IP. Adres IP zostanie przypisany dynamicznie. Jest to nazwa obiektu adres IP, do którego zostanie przypisany adres. Kliknij przycisk **OK**, aby zapisać zmiany.

5. W polu **Typ bramy** wybierz typ bramy określony dla danej konfiguracji.

6. W polu **Typ sieci VPN** wybierz typ sieci VPN określony dla danej konfiguracji.

7. W polu **Subskrypcja** sprawdź, czy wybrano poprawną subskrypcję.

8. W polu **Grupa zasobów** grupa zasobów jest określana przez wybraną sieć wirtualną.

9. Upewnij się, że pole **Lokalizacja** wskazuje lokalizację zarówno grupy zasobów, jak i sieci wirtualnej.

10. Możesz wybrać opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby brama była wyświetlana na pulpicie nawigacyjnym. Kliknij przycisk **Utwórz**, aby rozpocząć tworzenie bramy. Na pulpicie nawigacyjnym pojawi się kafelek „Wdrażanie bramy sieci wirtualnej”. Tworzenie bramy może potrwać do 45 minut. Wiele dzieje się w tle. Być może będzie trzeba odświeżyć stronę portalu, aby zobaczyć, czy tworzenie zostało ukończone.

    
    ![Brama](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png)

11. Po utworzeniu bramy można znaleźć adres IP, który został do niej przypisany, spoglądając na sieć wirtualną w portalu. Brama będzie widoczna jako urządzenie podłączone. Można kliknąć na urządzeniu podłączonym (bramie sieci wirtualnej), aby wyświetlić więcej informacji.






<!--HONumber=Jun16_HO2-->


