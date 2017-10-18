## <a name="deploy-template-from-cloud-shell"></a>Wdrażanie szablonu za pomocą usługi Cloud Shell

Do wdrożenia swojego szablonu możesz użyć usługi [Cloud Shell](../articles/cloud-shell/overview.md). Jednak musisz najpierw załadować swój szablon do udziału plików dla usługi Cloud Shell. Jeśli nie używasz usługi Cloud Shell, zobacz [Overview of Azure Cloud Shell (Omówienie usługi Azure Cloud Shell)](../articles/cloud-shell/overview.md), aby uzyskać informacje o jej konfigurowaniu.

1. Zaloguj się do witryny [Azure Portal](https://portal.azure.com).

1. Wybierz swoją grupę zasobów usługi Cloud Shell. Wzorzec nazwy to `cloud-shell-storage-<region>`.

   ![Wybieranie grupy zasobów](./media/resource-manager-cloud-shell-deploy/select-cs-resource-group.png)

1. Wybierz konto magazynu dla usługi Cloud Shell.

   ![Wybieranie konta magazynu](./media/resource-manager-cloud-shell-deploy/select-storage.png)

1. Wybierz pozycję **Pliki**.

   ![Wybieranie pozycji Pliki](./media/resource-manager-cloud-shell-deploy/select-files.png)

1. Wybierz udział plików dla usługi Cloud Shell. Wzorzec nazwy to `cs-<user>-<domain>-com-<uniqueGuid>`.

   ![Wybieranie udziału plików](./media/resource-manager-cloud-shell-deploy/select-file-share.png)

1. Wybierz pozycję **Dodaj katalog**.

   ![Dodawanie katalogu](./media/resource-manager-cloud-shell-deploy/select-add-directory.png)

1. Nadaj mu nazwę **templates** i wybierz przycisk **OK**.

   ![Nadawanie nazwy katalogowi](./media/resource-manager-cloud-shell-deploy/name-templates.png)

1. Wybierz swój nowy katalog.

   ![Wybieranie katalogu](./media/resource-manager-cloud-shell-deploy/select-templates.png)

1. Wybierz pozycję **Przekaż**.

   ![Wybieranie pozycji Przekaż](./media/resource-manager-cloud-shell-deploy/select-upload.png)

1. Znajdź i przekaż swój szablon.

   ![Przekazywanie pliku](./media/resource-manager-cloud-shell-deploy/upload-files.png)

1. Otwórz wiersz polecenia.

   ![Otwieranie usługi Cloud Shell](./media/resource-manager-cloud-shell-deploy/start-cloud-shell.png)
