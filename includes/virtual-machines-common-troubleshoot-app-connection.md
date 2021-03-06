Azure 仮想マシン (VM) で実行されているアプリケーションへの接続で問題が発生する理由はいろいろあります (アプリケーションが期待どおりのポートで実行されず、リッスンしていない、ネットワーク ルールがトラフィックをアプリケーションに正しく渡していないなど)。この記事では、問題を検出して解決するための系統的アプローチについて説明します。

RDP または SSH を使用した VM への接続で問題が発生している場合は、最初に、次のいずれかの記事を参照してください。

 - [Windows ベースの Azure 仮想マシンへのリモート デスクトップ接続に関するトラブルシューティング](../articles/virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md)
 - [Linux ベースの Azure 仮想マシンに対する Secure Shell (SSH) 接続のトラブルシューティング](../articles/virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md)

> [AZURE.NOTE] Azure には、リソースの作成と操作に関して 2 種類のデプロイ モデルがあります。[リソース マネージャー デプロイ モデルとクラシック デプロイ モデル](../articles/resource-manager-deployment-model.md)です。この記事では、両方のモデルについて取り上げていますが、最新のデプロイではリソース マネージャー モデルの使用をお勧めします。

この記事についてさらにヘルプが必要な場合は、いつでも [MSDN の Azure フォーラムとスタック オーバーフロー フォーラム](https://azure.microsoft.com/support/forums/)で Azure エキスパートに問い合わせることができます。または、Azure サポート インシデントを送信できます。その場合は、[Azure サポートのサイト](https://azure.microsoft.com/support/options/)に移動して、**[サポートの要求]** をクリックします。

## エンドポイント接続問題のトラブルシューティングのクイック スタート

アプリケーションへの接続に問題がある場合は、次の一般的なトラブルシューティング手順を実行してください。各手順の実行後に、アプリケーションへの接続を再度試してください。

- 仮想マシンを再起動します。
- Recreate the エンドポイント/ファイアウォール規則/ネットワーク セキュリティ グループ (NSG) 規則を再作成します。
	- [Cloud Services エンドポイントの管理](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
	- [ネットワーク セキュリティ グループの管理](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
- 別の Azure 仮想ネットワークなどの別の場所から接続します。
- 仮想マシンを再デプロイします。
	- [Windows VM のデプロイ](../articles/virtual-machines/virtual-machines-windows-redeploy-to-new-node.md)
	- [Linux VM のデプロイ](../articles/virtual-machines/virtual-machines-linux-redeploy-to-new-node.md)
- 仮想マシンを再作成します。

詳細については、「[Troubleshooting Endpoint Connectivity (RDP/SSH/HTTP, etc. failures) (エンドポイントの接続のトラブルシューティング (RDP/SSH/HTTP などの障害))](https://social.msdn.microsoft.com/Forums/azure/ja-JP/538a8f18-7c1f-4d6e-b81c-70c00e25c93d/troubleshooting-endpoint-connectivity-rdpsshhttp-etc-failures?forum=WAVirtualMachinesforWindows)」を参照してください。

## 詳細なトラブルシューティングの手順

Azure 仮想マシンで実行されているアプリケーションへのアクセスに問題がある場合は、次の 4 つの主要領域からトラブルシューティングを行います。

![](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access1.png)

1.	Azure 仮想マシンで実行されているアプリケーション。
	- アプリケーション自体は正常に実行されているか。
2.	Azure 仮想マシン。
	- VM 自体は正常に実行され、要求に応答しているか。
3.	仮想マシンが属しているクラウド サービスの Azure エンドポイント (クラシック デプロイ モデルの仮想マシンの場合)、受信 NAT 規則 (リソース マネージャー デプロイ モデルの仮想マシンの場合)、およびネットワーク セキュリティ グループ。
	- ユーザーから VM/アプリケーショへのトラフィック フローは、期待どおりのポートで行われているか。
4.	インターネット エッジ デバイス。
	- 所定のファイアウォール規則がトラフィック フローを適切に処理しているか。

サイト間 VPN または ExpressRoute 接続を介してアプリケーションにアクセスするクライアント コンピューターの場合、問題が起こる主な領域はアプリケーションと Azure 仮想マシンです。原因を特定して問題を修正するには、次の手順に従ってください。

## 手順 1: ターゲット VM からアプリケーションにアクセスできますか。

適切なクライアント プログラムが実行されている VM からそのクライアント プログラムでアプリケーションへのアクセスを試みます。ローカル ホスト名、ローカル IP アドレス、またはループバック アドレス (127.0.0.1) を使用します。

![](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access2.png)

たとえば、アプリケーションが Web サーバーである場合は、VM でブラウザーを開き、VM でホストされている Web ページへのアクセスを試行します。

アプリケーションにアクセスできる場合は、 [手順 2](#step2) に進んでください。

アプリケーションにアクセスできない場合は、次の点を確認してください。

- アプリケーションがターゲットの仮想マシンで実行されている。
- アプリケーションが正しい TCP ポートと UDP ポートをリッスンしている。

Windows ベースと Linux ベースの両方の仮想マシンで、 **netstat-a** コマンドを使用してアクティブなリスニング ポートを表示します。アプリケーションがリッスンしているポートの出力を確認します。アプリケーションを再起動するか、必要であれば、正しいポートを使用するように構成を変更し、アプリケーションのローカルでのアクセスを試みます。

## <a id="step2"></a>手順 2: 同じ仮想ネットワーク内の別の仮想マシンからアプリケーションにアクセスできますか。

同じ仮想ネットワーク内の異なる VM からアプリケーションにアクセスしてみます。その VM のホスト名または Azure 割り当てのパブリック、プライベート、またはプロバイダー IP アドレスを使用します。クラシック デプロイ モデルを使用して作成された VM の場合、クラウド サービスのパブリック IP アドレスは使用しないでください。

![](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access3.png)

たとえば、アプリケーションが Web サーバーである場合は、同じ仮想ネットワーク内の別の VM 上のブラウザーから、Web ページへのアクセスを試行します。

アプリケーションにアクセスできる場合は、 [手順 3](#step3) に進んでください。

アプリケーションにアクセスできない場合は、次の点を確認してください。

- ターゲット VM 上のホスト ファイアウォールで、リクエスト受信と応答送信のトラフィックが許可されている。
- ターゲット VM で実行されている侵入検出ソフトウェアやネットワーク監視ソフトウェアで、トラフィックが許可されている。
- Cloud Services エンドポイントまたはネットワーク セキュリティ グループでトラフィックが許可されている。
	- [Cloud Services エンドポイントの管理](../articles/cloud-services/cloud-services-enable-communication-role-instances.md)
	- [ネットワーク セキュリティ グループの管理](../articles/virtual-network/virtual-networks-create-nsg-arm-pportal.md)
- テスト VM と VM 間のパスにある VM 内で実行されている別のコンポーネント (ロード バランサーやファイアウォールなど) で、トラフィックが許可されている。

Windows ベースの仮想マシンについては、セキュリティ強化機能搭載 Windows ファイアウォールを使用して、ファイアウォール規則がアプリケーションの受信トラフィックと送信トラフィックを除外していないかどうかを確認してください。

## <a id="step3"></a>手順 3: 仮想ネットワークの外部にあるが、使用しているコンピューターと同じネットワークに接続していないコンピューターからアプリケーションにアクセスできますか。

アプリケーションが実行されている VM の仮想ネットワーク外にあるが、元のクライアント コンピューターと同じネットワーク上にないコンピューターから、アプリケーションへのアクセスを試行してください。

![](./media/virtual-machines-common-troubleshoot-app-connection/tshoot_app_access4.png)

たとえば、アプリケーションが Web サーバーである場合は、仮想ネットワーク内にないコンピューターで実行されているブラウザーから、Web ページへのアクセスを試行します。

アプリケーションにアクセスできない場合は、次の点を確認してください。

- クラシック デプロイ モデルを使用して作成された VM の場合、VM のエンドポイント構成で、受信トラフィック (特にプロトコル (TCP または UDP) とパブリックおよびプライベート ポート番号) が許可されているかどうか。
	- 詳細については、「[仮想マシンに対してエンドポイントを設定する方法](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md)」を参照してください。
- クラシック デプロイ モデルを使用して作成された VM の場合、エンドポイント上のアクセス制御リスト (ACL) によって、インターネットからの着信トラフィックが遮断されていないかどうか。
	- 詳細については、「[仮想マシンに対してエンドポイントを設定する方法](../articles/virtual-machines/virtual-machines-windows-classic-setup-endpoints.md)」を参照してください。
- リソース マネージャー デプロイ モデルを使用して作成された VM の場合、VM の受信 NAT ルール構成で、受信トラフィック (特にプロトコル (TCP または UDP) とパブリックおよびプライベート ポート番号) が許可されているかどうか。
- ネットワーク セキュリティ グループで、リクエスト受信と応答送信のトラフィックが許可されているかどうか。
	- 詳細については、「[ネットワーク セキュリティ グループ (NSG) について](../articles/virtual-network/virtual-networks-nsg.md)」を参照してください。

仮想マシンまたはエンドポイントが負荷分散セットのメンバーである場合:

- プローブ プロトコル (TCP または UDP) とポート番号が正しいことを確認してください。
- プローブ プロトコルとポートが、負荷分散セットのプロトコルおよびポートと異なる場合は、次の点を確認してください。
	- アプリケーションがプローブ プロトコル (TCP または UDP) とポート番号をリッスンしている (ターゲット VM で **netstat – a** を使用します)。
	- ターゲット VM 上のホスト ファイアウォールで、プローブ リクエスト受信とプローブ応答送信のトラフィックが許可されている。

アプリケーションにアクセスできる場合は、インターネット エッジ デバイスで次のトラフィックが許可されていることを確認してください。

- クライアント コンピューターから Azure 仮想マシンへのアプリケーション リクエスト送信トラフィック。
- Azure 仮想マシンからのアプリケーション応答受信トラフィック。

## その他のリソース

[Windows ベースの Azure 仮想マシンへのリモート デスクトップ接続に関するトラブルシューティング](../articles/virtual-machines/virtual-machines-windows-troubleshoot-rdp-connection.md)

[Linux ベースの Azure 仮想マシンに対する Secure Shell (SSH) 接続のトラブルシューティング](../articles/virtual-machines/virtual-machines-linux-troubleshoot-ssh-connection.md)
