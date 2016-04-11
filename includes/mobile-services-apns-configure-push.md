
* 手順に従う [クライアント SSL 署名 Id をサーバーにインストールする](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/ConfiguringPushNotifications/ConfiguringPushNotifications.html#//apple_ref/doc/uid/TP40012582-CH32-SW15) .p12 ファイルには、前の手順でダウンロードした証明書をエクスポートします。

* Azure クラシック ポータルで、クリックして **Mobile Services** > アプリ >、 **プッシュ** ] タブ > **apple プッシュ通知の設定** >"**アップロード**します。 確認します .p12 ファイルをアップロード、正しい **モード** が選択されている (サンド ボックスまたは運用環境で生成したクライアント SSL 証明書が開発用か配布するかどうかに対応します)。これで、iOS のプッシュ通知と連携するようにモバイル サービスが構成されました。

