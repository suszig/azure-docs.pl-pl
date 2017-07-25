W **Eksploratorze rozwiązań** kliknij prawym przyciskiem myszy projekt i wybierz polecenie **Opublikuj**. Wybierz pozycję **Utwórz nowe**, a następnie kliknij pozycję **Opublikuj**. 

![Publikowanie utworzonej aplikacji funkcji](./media/functions-vstools-publish/functions-vstools-publish-new-function-app.png)

Jeśli program Visual Studio nie został jeszcze połączony z kontem platformy Azure, kliknij pozycję **Dodaj konto...**.  

W oknie dialogowym **Tworzenie usługi App Service** użyj ustawień hostingu określonych w tabeli. 

![Lokalne środowisko uruchomieniowe platformy Azure](./media/functions-vstools-publish/functions-vstools-publish.png)

| Ustawienie      | Sugerowana wartość  | Opis                                |
| ------------ |  ------- | -------------------------------------------------- |
| **Nazwa aplikacji** | Nazwa unikatowa w skali globalnej | Unikatowa nazwa identyfikująca nową aplikację funkcji. |
| **Subskrypcja** | Wybierz subskrypcję | Subskrypcja platformy Azure, która ma być używana. |
| **[Grupa zasobów](../articles/azure-resource-manager/resource-group-overview.md)** | myResourceGroup |  Nazwa grupy zasobów, w której ma zostać utworzona aplikacja funkcji. |
| **[Plan usługi App Service](../articles/azure-functions/functions-scale.md)** | Plan Zużycie | Upewnij się, że podczas tworzenia nowego planu w obszarze **Rozmiar** wybrano opcję **Zużycie**.  |
| **[Konto magazynu](../articles/storage/storage-create-storage-account.md#create-a-storage-account)** | Nazwa unikatowa w skali globalnej | Użyj istniejącego konta magazynu lub utwórz nowe konto.   |

Kliknij pozycję **Utwórz**, aby utworzyć aplikację funkcji na platformie Azure przy użyciu tych ustawień. Po ukończeniu inicjowania obsługi administracyjnej zanotuj wartość **Adres URL witryny**, która jest adresem aplikacji funkcji na platformie Azure. 

![Lokalne środowisko uruchomieniowe platformy Azure](./media/functions-vstools-publish/functions-vstools-publish-profile.png)
