リソース|既定の制限|上限
---|---|---
[仮想マシン](../articles/virtual-machines/virtual-machines-about.md) クラウド サービスあたり<sup>1</sup>|50|50
クラウド サービスあたりの入力エンドポイント数 <sup>2</sup>|150|150

<sup>1</sup>サービス管理 (リソース マネージャーではなく) で作成された仮想マシンは自動的にクラウド サービスに格納されます。 より多くの仮想マシンをそのクラウド サービスに追加することで、負荷分散と可用性を実現できます。 参照してください  [方法と、仮想ネットワークの仮想マシンを接続したり、クラウド サービス](../articles/virtual-machines/cloud-services-connect-virtual-machine.md)します。

<sup>2</sup>入力エンドポイントは、仮想マシンのクラウド サービスの外部から仮想マシンへの通信を可能にします。 同じクラウド サービスまたは仮想ネットワーク内の仮想マシンは自動的に相互に通信できます。 参照してください [仮想マシンに対してエンドポイントを設定する方法](../articles/virtual-machines/virtual-machines-set-up-endpoints.md)します。 


