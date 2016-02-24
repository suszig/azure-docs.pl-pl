
<!--
includes/sql-database-include-ip-address-22-v12portal.md

鮮度をチェックする最新: 2015年-09-04、GeneMi です。

としての 2015年-09-04 およそ次のトピックが含まれます次のとおりには。
articles/sql-database/sql-database-configure-firewall-settings.md
articles/sql-database/sql-database-connect-query.md


## サーバーレベルのファイアウォール規則

### 新しい Azure ポータルからサーバー レベルのファイアウォール ルールを管理します。
-->


1. ログイン、 [Azure プレビュー ポータル](http://portal.azure.com/), 、http://portal.azure.com/にします。

2. 左のバナーをクリックして **すべて参照**します。  **参照** ブレードが表示されます。

3. スクロールし、クリックして **の SQL server**します。  **の SQL server** ブレードが表示されます。 

    ![ポータルで Azure SQL Database サーバーを見つける][b21-FindServerInPortal]

4. 参考までに、以前の最小化コントロールをクリックして **参照** ブレードです。

5. フィルター テキスト ボックスで、サーバーの名前の入力を開始します。 行が表示されます。

6. サーバーの行をクリックします。 サーバー用のブレードが表示されます。

7. クリックして、[server] ブレードで **設定**します。  **設定** ブレードが表示されます。

8. クリックして **ファイアウォール**します。  **ファイアウォールの設定** ブレードが表示されます。 

    ![[設定]、[ファイアウォール] の順にクリックします。][b31-SettingsFirewallNavig]

9. クリックして **クライアントの追加 IP**します。 最初のテキスト ボックスに、新しい規則の名前を入力します。

10. 有効にする範囲の IP アドレスの値を入力します。
 - 使用して低値 end おくと便利であることができます **.0** と高 **.255**します。 

    ![許可する IP アドレスの範囲を追加する][b41-AddRange]

11. クリックして **保存**します。



<!-- Image references. -->

[b21-FindServerInPortal]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b21-v12portal-findsvr.png

[b31-SettingsFirewallNavig]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b31-v12portal-settingsfirewall.png

[b41-AddRange]: ./media/sql-database-include-ip-address-22-v12portal/firewall-ip-b41-v12portal-addrange.png



<!--
These includes/ files are a sequenced set, but you can pick and choose:

includes/sql-database-include-ip-address-22-v12portal.md
? includes/sql-database-include-ip-address-*.md
-->

