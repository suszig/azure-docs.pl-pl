Przy użyciu usługi Azure Resource Manager można zdefiniować parametry dla wartości, które mają zostać uwzględnione podczas wdrażania szablonu. Szablon zawiera sekcji parametrów zawierająca wszystkie wartości parametrów.
Należy zdefiniować parametr dla tych wartości, które będą się różnić na podstawie projektu, który jest wdrażany lub opartych na środowisku, które wdrażasz. Definiuje parametry dla wartości, które będą zawsze taki sam. Każda wartość parametru jest używany w szablonie do definiowania zasobów, które są wdrożone. 

Określając parametry, użyj **allowedValues** pola, aby określić, które wartości użytkownika można podać podczas wdrażania. Użyj **defaultValue** pola, aby przypisać wartości do parametru, jeśli wartość nie zostanie podana podczas wdrażania.

Firma Microsoft będzie opisywać każdego parametru w szablonie.

### <a name="logicappname"></a>logicAppName
Nazwa aplikacji logiki do utworzenia.

    "logicAppName": {
        "type": "string"
    }