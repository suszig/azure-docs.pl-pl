### App Service プラン

Web アプリをホストするためのサービス プランを作成します。 **hostingPlanName** パラメーターに、プランの名前を指定します。 プランの場所は、 
web アプリで使用される場所は同じです。 価格レベルとワーカー サイズを **sku** パラメーター と **workerSize** パラメーターに指定します。

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



