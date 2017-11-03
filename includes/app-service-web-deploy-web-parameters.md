Przy użyciu usługi Azure Resource Manager można zdefiniować parametry dla wartości, które mają zostać uwzględnione podczas wdrażania szablonu. Szablon zawiera sekcji parametrów zawierająca wszystkie wartości parametrów.
Należy zdefiniować parametr dla tych wartości, które będą się różnić na podstawie projektu, który jest wdrażany lub opartych na środowisku, które wdrażasz. Definiuje parametry dla wartości, które będą zawsze taki sam. Każda wartość parametru używana w szablonie definiuje wdrażane zasoby. 

Określając parametry, użyj **allowedValues** pola, aby określić, które wartości użytkownika można podać podczas wdrażania. Użyj **defaultValue** pola, aby przypisać wartości do parametru, jeśli wartość nie zostanie podana podczas wdrażania.

Firma Microsoft będzie opisywać każdego parametru w szablonie.

### <a name="sitename"></a>Nazwa witryny
Nazwa aplikacji sieci web, którą chcesz utworzyć.

    "siteName":{
      "type":"string"
    }

### <a name="hostingplanname"></a>hostingPlanName
Nazwa planu usługi aplikacji można używać do obsługi aplikacji sieci web.

    "hostingPlanName":{
      "type":"string"
    }

### <a name="sku"></a>Jednostka SKU
Warstwę cenową planu obsługi.

    "sku": {
      "type": "string",
      "allowedValues": [
        "F1",
        "D1",
        "B1",
        "B2",
        "B3",
        "S1",
        "S2",
        "S3",
        "P1",
        "P2",
        "P3",
        "P4"
      ],
      "defaultValue": "S1",
      "metadata": {
        "description": "The pricing tier for the hosting plan."
      }
    }

Szablon definiuje wartości, które są dozwolone dla tego parametru, a następnie przypisuje wartość domyślną (S1), jeśli nie określono wartości.

### <a name="workersize"></a>workerSize
Rozmiar wystąpienia planu hostingu (małych, średnich i dużych).

    "workerSize":{
      "type":"string",
      "allowedValues":[
        "0",
        "1",
        "2"
      ],
      "defaultValue":"0"
    }

Szablon definiuje wartości, które są dozwolone dla tego parametru (0, 1 lub 2) i przypisuje wartość domyślną (0), jeśli nie określono wartości. Wartości odpowiadają małych, średnich i dużych.

