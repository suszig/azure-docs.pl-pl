## <a name="use-the-azure-portal"></a>Korzystanie z witryny Azure Portal
1. Wybierz maszynę Wirtualną, chcesz ponownie wdrożyć, a następnie wybierz *ponownie wdrożyć* przycisk *ustawienia* bloku. Konieczne może być przewiń w dół, aby znaleźć **pomocy technicznej i rozwiązywania problemów** sekcja, która zawiera przycisk "Wdrożenie", jak w poniższym przykładzie:
   
    ![Bloku maszyny Wirtualnej platformy Azure](./media/virtual-machines-common-redeploy-to-new-node/vmoverview.png)
2. Aby potwierdzić operację, wybierz *ponownie wdrożyć* przycisk:
   
    ![Należy ponownie wdrożyć bloku maszyny Wirtualnej](./media/virtual-machines-common-redeploy-to-new-node/redeployvm.png)
3. **Stan** maszyny wirtualnej zmienia się na *aktualizacji* jako maszyna wirtualna przygotowuje się do ponownego wdrażania, jak pokazano w poniższym przykładzie:
   
    ![Aktualizacja maszyny Wirtualnej](./media/virtual-machines-common-redeploy-to-new-node/vmupdating.png)
4. **Stan** zmienia się na *uruchamianie* jako maszyna wirtualna jest uruchamiany na nowy host platformy Azure, jak pokazano w poniższym przykładzie:
   
    ![Uruchamianie maszyny Wirtualnej](./media/virtual-machines-common-redeploy-to-new-node/vmstarting.png)
5. Po maszyny Wirtualnej kończy proces rozruchu **stan** następnie wróci do *systemem*, wskazującą maszyna wirtualna ma zostały pomyślnie wdrożone:
   
    ![Uruchamianie maszyny Wirtualnej](./media/virtual-machines-common-redeploy-to-new-node/vmrunning.png)

