可用性グループ リスナーが作成されると、調整する必要あります、 **RegisterAllProvidersIP** と **HostRecordTTL** クラスター リスナー リソースに対するパラメーター。  これらのパラメーターは、フェールオーバー後の再接続時間を短縮して、接続のタイムアウトを防ぐことができます。 これらのパラメーターと同様のサンプル コードの詳細については、次を参照してください。 [可用性グループ リスナーを作成または](https://msdn.microsoft.com/library/hh213080.aspx#MultiSubnetFailover)です。

