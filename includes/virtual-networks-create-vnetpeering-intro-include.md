Komunikacja równorzędna sieci wirtualnych jest mechanizmem, który łączy dwie sieci wirtualne w tym samym regionie za pośrednictwem sieci szkieletowej platformy Azure. Po nawiązaniu komunikacji równorzędnej dwie sieci wirtualne wyglądają jak jedna sieć wirtualna w kontekście całej łączności. Jeśli komunikacja równorzędna sieci wirtualnych jest Ci obca, zapoznaj się z tematem [VNet Peering overview](../articles/virtual-network/virtual-network-peering-overview.md) (Omówienie komunikacji równorzędnej sieci wirtualnych).

Komunikacja równorzędna sieci wirtualnych jest dostępna w publicznej wersji zapoznawczej. Aby móc z niej korzystać, należy zarejestrować się przy użyciu poniższych poleceń:

    Register-AzureRmProviderFeature -FeatureName AllowVnetPeering -ProviderNamespace Microsoft.Network

    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
 


<!--HONumber=sep16_HO1-->


