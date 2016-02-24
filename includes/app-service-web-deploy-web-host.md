### App Service プラン

Web アプリをホストするためのサービス プランを作成します。 使用して、プランの名前を指定する、 **hostingPlanName** パラメーター。 プランの場所は、 
web アプリで使用される場所は同じです。 価格レベルとワーカーのサイズが指定されている、 **sku** と **workerSize** パラメーター

    {
      "apiVersion": "2014-06-01",
      "name": "[parameters('hostingPlanName')]",
      "type": "Microsoft.Web/serverfarms",
      "location": "[parameters('siteLocation')]",
      "properties": {
        "name": "[parameters('hostingPlanName')]",
        "sku": "[parameters('sku')]",
        "workerSize": "[parameters('workerSize')]",
        "numberOfWorkers": 1
      }
    },

