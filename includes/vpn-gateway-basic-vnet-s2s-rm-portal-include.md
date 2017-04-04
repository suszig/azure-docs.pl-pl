Aby utworzyć sieć wirtualną w modelu wdrożenia usługi Resource Manager w witrynie Azure Portal, wykonaj poniższe kroki. Zamieszczone zrzuty ekranu są przykładowe. Przedstawione wartości należy zastąpić własnymi. Aby uzyskać więcej informacji dotyczących pracy z sieciami wirtualnymi, zobacz temat [Omówienie sieci wirtualnych](../articles/virtual-network/virtual-networks-overview.md).

1. Przejdź w przeglądarce do witryny [Azure Portal](http://portal.azure.com) i zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **Nowy**. W polu **Szukaj w witrynie Marketplace** wpisz „Sieć wirtualna”. Znajdź pozycję **Sieć wirtualna** na liście wyników i kliknij, aby otworzyć blok **Sieć wirtualna**.
3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Menedżer zasobów**, a następnie kliknij przycisk **Utwórz**.
4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. Po wypełnieniu pól czerwony wykrzyknik zmieni się na zielony znacznik wyboru, jeśli znaki wprowadzone w polu są prawidłowe.
5. Blok **Tworzenie sieci wirtualnej** wygląda podobnie do przedstawionego w poniższym przykładzie. Niektóre wartości mogą być wypełnione automatycznie. W takim przypadku należy zastąpić je własnymi wartościami.
   
    ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-basic-vnet-s2s-rm-portal-include/createvnet.png "Tworzenie bloku sieci wirtualnej")
6. **Nazwa**: Wprowadź nazwę sieci wirtualnej.
7. **Przestrzeń adresowa**: Wprowadź przestrzeń adresową. Jeśli masz wiele przestrzeni adresowych do dodania, dodaj pierwszą przestrzeń adresową. Możesz dodać dodatkowe przestrzenie adresowe później, po utworzeniu sieci wirtualnej. Upewnij się, że określona przestrzeń adresowa nie nakłada się na przestrzeń adresową dla Twojej lokalizacji lokalnej.
8. **Nazwa podsieci**: Dodaj nazwę podsieci i zakres adresów podsieci. Możesz dodać dodatkowe podsieci później, po utworzeniu sieci wirtualnej.
9. **Subskrypcja**: Sprawdź, czy wyświetlana jest prawidłowa subskrypcja. Subskrypcje można zmieniać, korzystając z listy rozwijanej.
10. **Grupa zasobów**: Wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Jeśli tworzysz nową grupę, nadaj jej nazwę odpowiadającą wartościom planowanej konfiguracji. Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
11. **Lokalizacja**: Wybierz lokalizację sieci wirtualnej. Lokalizacja określa miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej.
12. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.
13. Po kliknięciu przycisku **Utwórz** na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.