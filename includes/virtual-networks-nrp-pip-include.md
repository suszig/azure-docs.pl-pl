## パブリック IP アドレス
パブリック IP アドレス リソースは、予約済みまたは動的なインターネット接続 IP アドレスを提供します。 パブリック IP アドレスをスタンドアロン オブジェクトとして作成できますが、実際にアドレスを使用するには別のオブジェクトに関連付ける必要があります。 ロード バランサー、アプリケーション ゲートウェイ、またはそれらのリソースへのインターネット アクセスを提供する NIC にパブリック IP アドレスを関連付けることができます。  

|プロパティ|説明|値の例|
|---|---|---|
|**publicIPAllocationMethod**|IP アドレスがある場合に定義 *静的* または *動的*します。|static、dynamic|
|**idleTimeoutInMinutes**|アイドル タイムアウトを既定値 4 分で定義します。 この時間内に、指定されたセッションでこれ以上パケットが受信されなかった場合、セッションは終了します。|4 ～ 30 の任意の値|
|**ipAddress**|オブジェクトに割り当てられる IP アドレスです。 これは、読み取り専用のプロパティです。|104.42.233.77|

### DNS の設定
パブリック IP アドレスがあるという名前の子オブジェクト **dnsSettings** 次のプロパティを格納します。

|プロパティ|説明|値の例|
|---|---|---|
|**domainNameLabel**|名前解決に使用されるホスト名です。|www、ftp、vm1|
|**fqdn**|パブリック IP の完全修飾名。|www.westus.cloudapp.azure.com|
|**reverseFqdn**|IP アドレスに解決される完全修飾ドメイン名であり、PTR レコードとして DNS に登録されます。|www.contoso.com|

JSON 形式でのパブリック IP アドレスのサンプル:

    {
       "name": "PIP01",
       "location": "North US",
       "tags": { "key": "value" },
       "properties": {
          "publicIPAllocationMethod": "Static",
          "idleTimeoutInMinutes": 4,
          "ipAddress": "104.42.233.77",
          "dnsSettings": {
             "domainNameLabel": "mylabel",
             "fqdn": "mylabel.westus.cloudapp.azure.com",
             "reverseFqdn": "contoso.com."
          }
       }
    } 

### その他のリソース

- 詳細 [パブリック IP アドレス](virtual-networks-reserved-public-ip.md)です。
- について学習 [インスタンス レベル パブリック IP アドレス](virtual-networks-instance-level-public-ip.md)です。
- 読み取り、 [REST API リファレンス ドキュメント](https://msdn.microsoft.com/library/azure/mt163638.aspx) パブリック IP アドレスです。
