Usługi Azure Resource Manager można zdefiniować parametrów wartości do użycia podczas wdrażania szablonu. Szablon zawiera `parameters` sekcję zawierającą wszystkie wartości parametrów. Każda wartość parametru jest używany przez szablon do definiowania zasoby, które mają zostać wdrożone.

> [!NOTE]
> Nie należy definiować parametrów dla wartości, które pozostają niezmienione. Zdefiniuj parametry tylko dla wartości, które różnią się, na podstawie projektu, w której wdrażasz lub opartych na środowisku, w którym jest wdrażany.

Podczas definiowania parametry:

* Aby określić dozwolone wartości, które użytkownik może podać podczas wdrażania, użyj **allowedValues** pola.

* Aby przypisać wartości domyślnych do parametru, jeśli wartości nie zostały podane podczas wdrażania, użyj **defaultValue** pola. 
