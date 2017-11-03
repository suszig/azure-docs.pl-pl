1. Kliknij przycisk **Nowy** znajdujący się w lewym górnym rogu witryny Azure Portal.

1. Kliknij przycisk **obliczeniowe** > **aplikacji funkcji**. Następnie użyj określonych w tabeli ustawień aplikacji funkcji.

    ![Tworzenie aplikacji funkcji w witrynie Azure Portal](./media/functions-create-function-app-portal/function-app-create-flow.png)

    | Ustawienie      | Sugerowana wartość  | Opis                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Nazwa aplikacji** | Nazwa unikatowa w skali globalnej | Nazwa identyfikująca nową aplikację funkcji. Prawidłowe znaki to `a-z`, `0-9`, i `-`.  | 
    | **Subskrypcja** | Twoja subskrypcja | Subskrypcja, pod którym zostanie utworzona tej nowej funkcji aplikacji. | 
    | **[Grupa zasobów](../articles/azure-resource-manager/resource-group-overview.md)** |  myResourceGroup | Nazwa nowej grupy zasobów, w której ma zostać utworzona aplikacja funkcji. | 
    | **[Plan hostingu](../articles/azure-functions/functions-scale.md)** |   Plan Zużycie | Plan hostingu określający sposób przydzielania zasobów do aplikacji funkcji. W domyślnym planie (**Plan Zużycie**) zasoby są dodawane dynamicznie zgodnie z wymaganiami funkcji. Opłaty są naliczane tylko wtedy, gdy funkcje są uruchomione.   |
    | **Lokalizacja** | Europa Zachodnia | Wybierz lokalizację w swojej okolicy lub w pobliżu innych usług, do których Twoje funkcje będą uzyskiwać dostęp. |
    | **[Konto magazynu](../articles/storage/common/storage-create-storage-account.md#create-a-storage-account)** |  Nazwa unikatowa w skali globalnej |  Nazwa nowego konta magazynu używanego przez aplikację funkcji. Nazwy kont usługi Magazyn muszą mieć długość od 3 do 24 znaków i mogą zawierać tylko cyfry i małe litery. Możesz także użyć istniejącego konta. |

1. Kliknij pozycję **Utwórz**, aby zainicjować obsługę nowej aplikacji funkcji i wdrożyć ją.
