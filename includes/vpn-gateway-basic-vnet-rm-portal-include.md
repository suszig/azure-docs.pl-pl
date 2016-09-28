Aby utworzyć sieć wirtualną przy użyciu witryny Azure Portal, wykonaj poniższe kroki. Należy zauważyć, że zamieszczone zrzuty ekranu są przykładowe. Przedstawione na nich wartości należy zastąpić własnymi. Aby uzyskać więcej informacji dotyczących pracy z sieciami wirtualnymi, zobacz temat [Omówienie sieci wirtualnych](../articles/virtual-network/virtual-networks-overview.md).

1. W przeglądarce przejdź do witryny [Azure Portal](http://portal.azure.com) i, jeśli to konieczne, zaloguj się przy użyciu konta platformy Azure.

2. Kliknij opcję **Nowe****>****Sieć****>****Sieć wirtualna**.

    ![VNetBlade](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal650.png)

3. Z listy **Wybierz model wdrożenia** znajdującej się w dolnej części bloku Virtual Network wybierz opcję **Menedżer zasobów**, a następnie kliknij przycisk **Utwórz**.


    ![Wybór opcji Menedżer zasobów](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png)

4. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej. W tym bloku należy dodać pierwszą przestrzeń adresową i zakres adresów pojedynczej podsieci. Po zakończeniu tworzenia sieci wirtualnej możesz wrócić i dodać dodatkowe podsieci oraz przestrzenie adresowe. Jest to aktualne ograniczenie portalu. Zawsze możesz wrócić i zaktualizować te wartości, edytując właściwości sieci wirtualnej w portalu lub przy użyciu programu PowerShell. Wartości, których można użyć, zależą od konfiguracji, jaką chcesz utworzyć. Pamiętaj, aby odwoływać się do wartości planowanej konfiguracji. 

    ![Tworzenie bloku sieci wirtualnej](./media/vpn-gateway-basic-vnet-rm-portal-include/createavnet250.png)

5. Sprawdź, czy pole **Subskrypcja** zawiera prawidłową wartość. Subskrypcje można zmieniać, korzystając z listy rozwijanej.

6. Kliknij przycisk **Grupy zasobów** i wybierz istniejącą grupę zasobów lub utwórz nową, wpisując nazwę nowej grupy zasobów. Jeśli tworzysz nową grupę, nadaj jej nazwę odpowiadającą wartościom planowanej konfiguracji. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](resource-group-overview.md#resource-groups).

7. Następnie w polu **Lokalizacja** wybierz ustawienia sieci wirtualnej. Należy zauważyć, że lokalizacja określi miejsce, w którym zostaną umieszczone zasoby wdrażane w tej sieci wirtualnej. Tego ustawienia nie można później zmienić bez ponownego wdrożenia zasobów.

8. Wybierz opcję **Przypnij do pulpitu nawigacyjnego**, jeśli chcesz, aby Twoją sieć wirtualną można było łatwo znaleźć na pulpicie nawigacyjnym, a następnie kliknij przycisk **Utwórz**.
    
    ![Przypnij do pulpitu nawigacyjnego](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png)


9. Po kliknięciu przycisku Utwórz na pulpicie nawigacyjnym zostanie umieszczony kafelek, który będzie odzwierciedlać postęp Twojej sieci wirtualnej. Wygląd kafelka zmienia się w trakcie tworzenia sieci wirtualnej.

    ![Kafelek tworzenia sieci wirtualnej](./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png)

<!--HONumber=Sep16_HO3-->


