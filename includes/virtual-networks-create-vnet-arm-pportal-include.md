## Tworzenie sieci wirtualnej w witrynie Azure Portal

Aby utworzyć sieć wirtualną w oparciu o powyższy scenariusz, korzystając z portalu Azure w wersji zapoznawczej, wykonaj poniższe kroki.

1. W przeglądarce przejdź do strony http://portal.azure.com i w razie potrzeby zaloguj się przy użyciu konta platformy Azure.
2. Kliknij kolejno opcje **NOWA** > **Sieć** > **Sieć wirtualna**, kliknij opcję **Resource Manager** z listy **Wybierz model wdrożenia**, a następnie kliknij przycisk **Utwórz**, jak pokazano na poniższej ilustracji.

    ![Tworzenie sieci wirtualnej w portalu Azure](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure1.gif)

3. W bloku **Utwórz sieć wirtualną** skonfiguruj ustawienia sieci wirtualnej, jak pokazano na poniższej ilustracji.

    ![Tworzenie bloku sieci wirtualnej](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure2.png)

4. Kliknij opcję **Grupa zasobów** i wybierz grupę zasobów, aby dodać do niej sieć wirtualną, lub kliknij opcję **Utwórz nową**, aby dodać sieć wirtualną do nowej grupy zasobów. Na poniższej ilustracji przedstawiono ustawienia nowej grupy zasobów o nazwie **TestRG**. Aby uzyskać więcej informacji na temat grup zasobów, zobacz [Omówienie usługi Azure Resource Manager](../articles/resource-group-overview.md#resource-groups).

    ![Grupa zasobów](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure3.png)

5. W razie potrzeby zmień wartości ustawień **Subskrypcja** i **Lokalizacja** sieci wirtualnej. 

6. Jeśli nie chcesz, aby sieć wirtualna była wyświetlana jako kafelek na **tablicy startowej**, wyłącz opcję **Przypnij do tablicy startowej**. 

7. Kliknij przycisk **Utwórz**. Zobaczysz kafelek o nazwie **Tworzenie sieci wirtualnej**, jak pokazano na poniższej ilustracji.

    ![Kafelek tworzenia sieci wirtualnej](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure4.png)

8. Poczekaj na utworzenie bloku sieci wirtualnej, a następnie w obszarze **Sieć wirtualna** kliknij kolejno opcje **Wszystkie ustawienia** > **Podsieci** > **Dodaj**, jak pokazano poniżej.

    ![Dodawanie podsieci w witrynie Azure Portal](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure5.gif)

9. Określ ustawienia podsieci mające zastosowanie do podsieci o nazwie *BackEnd*, jak pokazano poniżej, a następnie kliknij przycisk **OK**. 

    ![Ustawienia podsieci](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure6.png)

10. Zostanie wyświetlona lista podsieci, jak pokazano na poniższej ilustracji.

    ![Lista podsieci w sieci wirtualnej](./media/virtual-networks-create-vnet-arm-pportal-include/vnet-create-arm-pportal-figure7.png)



<!--HONumber=sep16_HO1-->


