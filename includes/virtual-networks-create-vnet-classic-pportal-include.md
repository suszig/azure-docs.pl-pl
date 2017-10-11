## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Tworzenie klasycznej sieci wirtualnej w portalu Azure
Tworzenie klasycznej oparte na powyższym scenariuszu sieci wirtualnej, wykonaj poniższe kroki.

1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. Kliknij przycisk **nowy** > **sieci** > **sieci wirtualnej**, zwróć uwagę, że **wybierz model wdrożenia** Lista już pokazuje **klasycznego**, a następnie kliknij przycisk **Utwórz**, jak pokazano na poniższej ilustracji.
   
    ![Tworzenie sieci wirtualnej w portalu Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
3. Na **sieci wirtualnej** bloku, typ **nazwa** sieci wirtualnej, a następnie kliknij przycisk **przestrzeni adresów**. Skonfiguruj ustawienia przestrzeni adresów sieci wirtualnej i jej pierwszej podsieci, a następnie kliknij przycisk **OK**. Na poniższym rysunku przedstawiono ustawienia bloku CIDR do naszego scenariusza.
   
    ![Blok przestrzeń adresów](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
4. Kliknij przycisk **grupy zasobów** i wybierz grupę zasobów, aby dodać sieć wirtualną do, lub kliknij przycisk **Utwórz nową grupę zasobów** można dodać sieci wirtualnej do nowej grupy zasobów. Na poniższej ilustracji przedstawiono ustawienia nowej grupy zasobów o nazwie **TestRG**. Aby uzyskać więcej informacji na temat grup zasobów, zobacz temat [Omówienie usługi Azure Resource Manager](../articles/azure-resource-manager/resource-group-overview.md#resource-groups).
   
    ![Tworzenie bloku grupy zasobów](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
5. W razie potrzeby zmień wartości ustawień **Subskrypcja** i **Lokalizacja** sieci wirtualnej. 
6. Jeśli nie chcesz, aby sieć wirtualna była wyświetlana jako kafelek na **tablicy startowej**, wyłącz opcję **Przypnij do tablicy startowej**. 
7. Kliknij przycisk **Utwórz**. Zobaczysz kafelek o nazwie **Tworzenie sieci wirtualnej**, jak pokazano na poniższej ilustracji.
   
    ![Tworzenie sieci wirtualnej w portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
8. Poczekaj, aż do utworzenia sieci wirtualnej, a gdy pojawi się Kafelek poniżej, kliknij go, aby dodać więcej podsieci.
   
    ![Tworzenie sieci wirtualnej w portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
9. Powinny pojawić się **konfiguracji** dla sieci wirtualnej, jak pokazano poniżej. 
   
    ![Tworzenie sieci wirtualnej w portalu](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
10. Kliknij przycisk **podsieci** > **Dodaj**, wpisz **nazwa** i określ **(blok CIDR) zakres adresów** dla podsieci, a następnie Kliknij przycisk **OK**. Na poniższym rysunku przedstawiono ustawienia bieżącego scenariusza.
    
    ![Tworzenie sieci wirtualnej w portalu Azure](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)

