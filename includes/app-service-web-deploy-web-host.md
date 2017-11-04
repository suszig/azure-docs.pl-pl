### <a name="app-service-plan"></a>Plan usługi App Service
Tworzy plan usługi do obsługi aplikacji sieci web. Podaj nazwę planu za pośrednictwem **hostingPlanName** parametru. Lokalizacja planu jest tej samej lokalizacji, które są używane dla grupy zasobów. Cenową rozmiar warstwy i proces roboczy są określone w **sku** i **workerSize** parametrów

    {
      "apiVersion": "2015-08-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "[parameters('sku')]",
        "capacity": "[parameters('workerSize')]"
      },
      "properties": {
        "name": "[parameters('hostingPlanName')]"
      }
    },

