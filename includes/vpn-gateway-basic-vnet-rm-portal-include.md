Aby utworzyć sieć wirtualną przy użyciu witryny Azure Portal, wykonaj poniższe kroki. Zamieszczone zrzuty ekranu są przykładowe. Przedstawione wartości należy zastąpić własnymi. Aby uzyskać więcej informacji dotyczących pracy z sieciami wirtualnymi, zobacz temat [Omówienie sieci wirtualnych](../articles/virtual-network/virtual-networks-overview.md).

1. W przeglądarce przejdź do witryny [Azure Portal](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta platformy Azure.

2. Kliknij przycisk **Nowy**. W polu **Szukaj w witrynie Marketplace** wpisz „Sieć wirtualna”. Znajdź pozycję **Sieć wirtualna** na liście wyników i kliknij, aby otworzyć blok **Sieć wirtualna**.

    ![Znajdowanie bloku zasobów sieci wirtualnej](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Locate virtual network resource blade")

3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Menedżer zasobów**, a następnie kliknij przycisk **Utwórz**.


    ![Wybór opcji Menedżer zasobów](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Select Resource Manager")

4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. Po wypełnieniu pól czerwony wykrzyknik zmieni się na zielony znacznik wyboru, jeśli znaki wprowadzone w polu są prawidłowe.

    ![Sprawdzanie poprawności pól](./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Field validation")

5. Blok **Tworzenie sieci wirtualnej** wygląda podobnie do przedstawionego w poniższym przykładzie. Niektóre wartości mogą być wypełnione automatycznie. W takim przypadku należy zastąpić je własnymi wartościami.

    ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Create virtual network blade")

6. **Nazwa**: Wprowadź nazwę sieci wirtualnej.

7. **Przestrzeń adresowa**: Wprowadź przestrzeń adresową. Jeśli masz wiele przestrzeni adresowych do dodania, dodaj pierwszą przestrzeń adresową. Możesz dodać dodatkowe przestrzenie adresowe później, po utworzeniu sieci wirtualnej.
 
8. **Nazwa podsieci**: Dodaj nazwę podsieci i zakres adresów podsieci. Możesz dodać dodatkowe podsieci później, po utworzeniu sieci wirtualnej.

10. **Subskrypcja**: Sprawdź, czy wyświetlana jest prawidłowa subskrypcja. Subskrypcje można zmieniać, korzystając z listy rozwijanej.

11. **Grupa zasobów**: Wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Jeśli tworzysz nową grupę, nadaj jej nazwę odpowiadającą wartościom planowanej konfiguracji. Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](resource-group-overview.md#resource-groups).

12. **Lokalizacja**: Wybierz lokalizację sieci wirtualnej. Lokalizacja określa miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej.

13. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.
    
    ![Przypnij do pulpitu nawigacyjnego](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "pin to dashboard")

14. Po kliknięciu przycisku **Utwórz** na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.

    ![Kafelek tworzenia sieci wirtualnej](./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Creating virtual network tile")

<!--HONumber=Oct16_HO1-->


