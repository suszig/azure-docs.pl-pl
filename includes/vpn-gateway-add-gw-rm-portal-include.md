1. W portalu przejdź kolejno do poleceń **Nowe** > **Sieci** > **Brama sieci wirtualnej**. Spowoduje to otwarcie bloku **Tworzenie bramy sieci wirtualnej**.

    ![Brama](./media/vpn-gateway-add-gw-rm-portal-include/creategw250.png)

2. W bloku **Tworzenie bramy sieci wirtualnej** podaj nazwę bramy w polu **Nazwa**. Nie chodzi o nazwę podsieci bramy. Jest to nazwa obiektu bramy, który zostanie utworzony.

3. Dostosuj pole **Lokalizacja**, aby wskazywało miejsce, w którym znajduje się sieć wirtualna. Jeśli tego nie zrobisz, wskazana sieć wirtualna nie będzie widoczna na liście.
 
4. Następnie wybierz sieć wirtualną, do której chcesz dodać bramę. Kliknij polecenie **Sieć wirtualna**, aby otworzyć blok **Wybieranie sieci wirtualnej**. Wybierz sieć wirtualną. Aby sieć wirtualna była widoczna na liście, musi już mieć prawidłową podsieć bramy.

5. Wybierz publiczny adres IP. Kliknij polecenie **Publiczny adres IP**, aby otworzyć blok **Wybieranie publicznego adresu IP**. Następnie kliknij przycisk **Utwórz nowy**, aby otworzyć **blok Tworzenie bramy sieci lokalnej**. Wprowadź nazwę dla publicznego adresu IP. Spowoduje to utworzenie obiektu publicznego adresu IP, do którego publiczny adres IP zostanie dynamicznie przypisany. <br>Kliknij przycisk **OK**, aby zapisać zmiany.

5. W polu **Typ bramy** wybierz typ bramy określony dla danej konfiguracji.

6. W polu **Typ sieci VPN** wybierz typ sieci VPN określony dla danej konfiguracji.

7. W polu **Subskrypcja** sprawdź, czy wybrano poprawną subskrypcję.

8. Pole **Grupa zasobów** jest określane przez wybraną sieć wirtualną. 

9. Nie zmieniaj **Lokalizacji** po podaniu powyższych ustawień. 

10. W tym momencie blok będzie wyglądać podobnie do ilustracji w kroku 1. Sprawdź, czy ustawienia są zgodne z Twoimi ustawieniami konfiguracji. Jeśli chcesz, aby brama była wyświetlana na pulpicie nawigacyjnym, możesz wybrać opcję **Przypnij do pulpitu nawigacyjnego** znajdującą się u dołu bloku.

11. Kliknij przycisk **Utwórz**, aby rozpocząć tworzenie bramy. Zostanie sprawdzona poprawność ustawień, a na pulpicie nawigacyjnym pojawi się kafelek „Wdrażanie bramy sieci wirtualnej”. Tworzenie bramy może potrwać do 45 minut. Być może będzie trzeba odświeżyć stronę portalu, aby zobaczyć, czy tworzenie zostało ukończone.

    ![Brama](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png)

11. Po utworzeniu bramy można znaleźć adres IP, który został do niej przypisany, spoglądając na sieć wirtualną w portalu. Brama będzie widoczna jako urządzenie podłączone. Można kliknąć na urządzeniu podłączonym (bramie sieci wirtualnej), aby wyświetlić więcej informacji.





<!--HONumber=Sep16_HO3-->


