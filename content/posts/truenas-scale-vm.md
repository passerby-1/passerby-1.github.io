---
title: "TrueNAS Core から Scale へ移行した話"
date: 2022-11-03T02:48:56+09:00
description: お手軽にNASを組めるTrueNAS。少し前から、FreeBSDベースのCoreだけではなくLinuxベースでdocker/k8sサポートも充実したScaleプロジェクトが増えました。本記事では、Scaleへ移行した軽い感想と、移行に伴って若干調整が必要だった個所・不具合への対処方法を説明しています。
tags: 
  - "TrueNAS"
  - "自宅サーバー"
  - "技術"
  - "VM"
  - "Infra"
  - "Network"
draft: false
isCJKLanguage: true
---

先日、家で動かしている TrueNAS Core (旧 FreeNAS) のサーバーを、TrueNAS Scale へアップデート・移行しました。Unix/BSD (FreeBSD) ベースであった TrueNAS Core から Linux (Debian) ベースの TrueNAS Scale へのアップデートでは、UI がより洗練され、jail が消え、docker/k8s のサポートが増え、TrueNAS 公式/非公式のプラグインも様変わりするなど、大きな変化がありました。

この記事では、TrueNAS Core => Scale のアップデート方法や個々の細かいアップデート点については触れませんが、TrueNAS Scale へ移行し感じたメリット、デメリットなどの軽い感想と、移行に伴い、特に VM 関連で調整が必要だった個所への対処方法を、簡単に説明したいと思います。



## TrueNAS Scale

TrueNAS は、iXsystems 社が開発している、NAS 向けのオープンソースプログラムの総称です。OSに最初から組み込まれているNAS向けの設定機能に加え、KVM による VM 機能や、プラグイン機能も統合されており、GUI で比較的に楽に、様々な機能を用いることができます。



上に書いた通り、本記事では TrueNAS Scale への移行方法や個々の細かいアップデート点については触れません。この辺りについては、公式や以下の記事などを参照。

きりしま屋「TrueNAS on Linuxを初公式リリース！（TrueNAS SCALE）」
https://kirishima.it/mt/2022/02/truenas_on_linuxtruenas_scale.html

きりしま屋「TrueNAS SCALE に TrueNAS CORE からアップグレードする」
https://kirishima.it/mt/2022/02/migrating_from_truenascore.html



私の自宅では、HPE Proliant ML30 Gen9 (Xeon E3-1220v5、RAM32GB、SSD120GB (OS) + SSD 500GB + HDD4TB + 4TB + 2TB) の 1 台で、NextCloud や wikijs、GitLab、録画関連サービス、その他こまごましたサービスを動かし、Cloudflare Tunnel を用いて、インターネット上からのアクセスもできるようにしていました。(Scale への移行後、再構築中なので過去形)

今回、TrueNAS Scale への移行をした理由については、主に docker のサポートが欲しかった、プラグインが多そうなのを試してみたかった、Unix より Linux の方が慣れている、もっと色々システムをいじくる前に移行したほうが再構築のダメージが少ない といった理由です。OS の移行作業はパスワードを再設定する作業しかない位の楽さで、動作も早く快適で、現状大きな不具合も無く、docker/k8s ベースのプラグイン機能も以前のプラグインより充実していて、大方満足しています。しかし、非公式プラグインの充実から来る? 公式プラグインの若干の不足と、非公式プラグインのドキュメント不足など、期待と異なる部分も一部ありました。下に、個人的なメリット・デメリットをまとめたので、参考にしてください。



### TrueNAS Scale 移行のメリット・デメリット

#### メリット

* BSD/Unix に慣れていない人でも設定しやすい
  * 特にプラグイン周りで jail に触りたくない人
* 非公式プラグインが充実している
* docker/k8s で何でも (*もしかしたら例外があるかも) 動かせる
  * FreeBSD / jail よりも圧倒的に情報量が多いので問題解決が楽
* TrueNAS Core の UI 上からほぼワンクリックでの移行が可能
* 今後恐らく主流となるのはこっち

#### デメリット

* Core のプラグイン、FreeBSD jail でサービスを動かしていた場合、直接の移行はできずデータが消えることになる (サービスが消えるだけで、ストレージ上・NAS のデータは消えない)
  * NextCloud、GitLab、Plex などのプラグインのデータはすべて消える
  * 設定ファイル等をエクスポートして、再度環境を作り直してインポートする等できたら比較的楽
  * 環境作り直しの気力
* 公式の安定した GitLab プラグインが無い (Core では存在した、ワンクリックインストールの公式プラグインが消えた)
  * 現時点で非公式プラグインも無い (docker や VM を建てて自分で入れることは可能)
  * GitLab に限って研究室レベルで (インフラをあまり触っていない学生が) ポンと建てるハードルは上がったかも?
* 非公式プラグインの動作が不安定
  * 説明通りにやっても動かなかったりする
  * 自分の理解不足かもしれない
* 非公式プラグインのドキュメントが少ない
  * ので、結局色々試して自己解決する / 諦めて VM を建てることになる
  * 試してダメだった非公式プラグインの更新を待つ / 並行して試すのが無難?
  * 人気のプラグインの問題は少ない



## TrueNAS Scale への移行に伴う VM の調整

TrueNAS の Virtual Machines 機能は、KVM による仮想化であるため、jail と異なり移行後もそのまま利用が可能です。しかし、移行に伴って NIC の名前が変わったり、今までできていた (?) (Core の方では自然に利用出来ていたが、細かい条件で検証していないので詳細は不明)、ホスト OS - ゲスト OS 間通信がそのままではできないなど、若干の調整が必要な個所がありました。

原因を考えてみれば当たり前な部分もありますが、今後移行する人の役に立つと思うので、以下の2点について若干のメモを残しておきます。

* ネットワーク名の変更に伴う修正 (Ubuntu VM)
* ブリッジの設定 (Cloudflare Tunnel や、VM 内からNFS・SMB等でTrueNASのストレージを見に行きたい時に必要)



#### ネットワーク名の変更に伴う修正 (Ubuntu VM)

OS の移行に伴い、`eth0` や `enp0s3`、`wlan1`、など自動で名づけられたネットワーク名が変わっていることがあります。私の場合、移行により NIC のネットワーク名が `enp0s4` から `eno1` になった結果、VMの起動時にエラーが発生しました。以下の説明は、`/etc/netplan/` 内の設定ファイルに、IP アドレスを固定する設定を書いているケースなので、DHCP で動的に IP アドレスを割り当てている場合には若干変わるかもしれません。

1. Virtual Machines > 当該のVM > Devices から、NIC デバイスの設定を開く![vm-nic1](/images/202211/truenas-scale-vm/vm-nic1.png)
2. "Nic to attach" 設定の NIC を、現在使用しているものに設定し、Save する![vm-nic2](/images/202211/truenas-scale-vm/vm-nic2.png)
3. この時点で VM 起動時のエラーが消え、VM を起動できるようになりますが、まだゲスト OS 側の修正が必要です。起動後 VNC でゲスト OS に入ります。(まだゲスト OS がネットワークに繋がっていないため、TrueNAS の noVNC 以外の手段、ssh や RDP でのアクセスはできません。)
4. ゲスト OS からインターネット上の適当なホスト (`1.1.1.1` など) に ping を送り、Network is unreachable エラーとなることを確認します (この時点で解決していた場合、この後の作業は不要です。)
5. `ip a` などで、ゲスト OS マシン上の NIC 名を確認します (`ens3` など)
6. `cd /etc/netplan`
   1. `00-installer-config.yaml` や、自分で書いたネットワーク設定の `.yaml` ファイルを、sudo をつけて好きなエディタで開きます
   2. 以前のネットワーク名で書かれている設定部分を、`ip a` コマンドで確認したネットワーク名に書き換えて保存します (`enp0s4` を `ens3` に書き換えるなど)
   3. `sudo netplan apply` で適用します
7. インターネット上の適当なホストに ping が届くようになっていることを確認します



#### ブリッジの設定 (Cloudflare Tunnel や、VM 内からNFS・SMB等でTrueNASのストレージを見に行きたい時に必要)

「**ネットワーク名の変更に伴う修正 (Ubuntu VM)**」によって、ゲスト OS が再びネットワークの一部となりました。ここで、ゲスト OS からホスト OS (TrueNAS Scale) マシンの IP アドレスへ ping を送ってみましょう。また、逆にホスト OS の Shell からゲスト OS へ ping を送ってあげましょう。あれ? 繋がらない?

ここでホストOS - ゲスト OS 間の通信ができないのは、KVM の仕様です。詳しくは末尾の参考ページを参照。

これもブリッジを作るだけで解決するので、やってしまいましょう。

1. ホスト OS (TrueNAS Scale) の Network 設定を開き、Interfaces > Add を選択します。

2. Add Interface メニューが出てくるので、

   1. Interface Settings  
          Type: `Bridge`  
          Name: `br(数字)`  
          Description: `適当な説明 ` 
   2. IP Addresses 設定から、Add をクリックし、ホストと異なるサブネットの IP アドレスを設定します (私の場合、ホストOS の繋がっているネットワークが `192.168.100/24` なので、適当に `192.168.101.1/24` と入力)

   と選択・入力し、Apply をクリック

3. Apply をクリックした後、Network 設定ページ上部に、

   > There are unapplied network interface changes that must be tested before being permanently saved. Test changes now?
   >
   > Test network interface changes for 60 seconds. 
   > [Test Changes] [Revert Changes]

   と、出てくるので、Test Changesをクリック、ホップアップから Confirm => TEST CHANGES

4. 一時的にネットワークの設定が有効化されるので、NAS へのアクセスが通常通りできることを確認した後、Save Changes をクリックし、設定の変更を永続化します (放置した場合元の設定に戻される)

5. ホストOS とのアクセスを行いたい / ホスト OS からアクセスしたい ゲスト OS の Devices 設定 > Add から、Type: `NIC`、Nic to attach: `(先ほど作ったbridge)` のデバイスを作り、Save します
   ![br-nic1](/images/202211/truenas-scale-vm/br-nic1.png)

6. VM を再起動し、`ip a` などで `ens4` などの名前の新しいネットワークが増えていることを確認します (ここでの MAC アドレスは Devices 設定での MAC アドレスと同じです)

7. `cd /etc/netplan`

   1. 「**ネットワーク名の変更に伴う修正 (Ubuntu VM)**」と同様、設定ファイルを開き、新たに増えたネットワークの経路を追記します。netplan はいくつかの書き方ができるため、どこを追記すればよいかは自分の現在の設定をみながら調べてください。以下は、私のケースの設定内容です。(`ens3` はプライベートな固定 IP のインターネットへつながるネットワーク、`ens4` が今回追記した経路)
      ```yaml
      network:
          version: 2
          renderer: networkd
          ethernets:
              ens3:
                  addresses:
                      - 192.168.100.17/24
                  nameservers:
                      addresses: [192.168.100.1]
                  routes:
                      - to: default
                        via: 192.168.100.1
              ens4:
                  addresses:
                      - 192.168.101.2/24
                  routes:
                      - to: 192.168.101.1
      ```

8. これで、ホスト OS (TrueNAS Scale) 側は 192.168.101.1、ゲスト OS は 192.168.101.2 としてお互いにアクセスできるようになりました



この設定は VM に Cloudflared (Cloudflare Tunnel) を建て、ホスト OS の TrueNAS Scale のコントロールパネルへアクセスできるようにしたい場合などに必要です。Cloudflare Tunnel は便利なので、紹介の記事・競合との比較をやってみたいですが、それはまたの機会に。



参考記事:

FUROR TEUTONICUS "Enabling host-guest networking with KVM, Macvlan and Macvtap"
https://www.furorteutonicus.eu/2013/08/04/enabling-host-guest-networking-with-kvm-macvlan-and-macvtap/

Qiita @exthenet「TrueNASのVM利用時のホストとのストレージ共有の話」
https://qiita.com/exthnet/items/d2905f0366340b1b4616

