# rhel10

# INDEX

- [VMを作成する(Proxmox)]()
- [物理マシン作成(OptiPlex5040)]()


## VMを作成する(Proxmox)

RedHatのアカウントを作った後、[公式ページ](https://developers.redhat.com/products/rhel/download#getredhatenterpriselinux7163)からISOを無料でダウンロードできる

<img width="1573" height="941" alt="image" src="https://github.com/user-attachments/assets/43f91e03-efe2-4553-a16a-1e9677f36843" />

ProxmoxのStorage(local)にISOをUpload

<img width="542" height="232" alt="image" src="https://github.com/user-attachments/assets/193abe9e-5233-4614-9b07-63ab25f7749f" />

<img width="422" height="329" alt="image" src="https://github.com/user-attachments/assets/2b8d0d91-12d2-48ad-be69-5b06eba5ea6a" />

Create VM　（設定は以下画像のまま）

<img width="735" height="554" alt="image" src="https://github.com/user-attachments/assets/54a8f2ff-dac9-4998-b123-ba080330abda" />

<img width="731" height="553" alt="image" src="https://github.com/user-attachments/assets/a2c366bc-d9e0-4eaf-b004-181548c83527" />

<img width="736" height="557" alt="image" src="https://github.com/user-attachments/assets/0501e021-e379-4b25-a6e5-e6d9133daa49" />

<img width="729" height="549" alt="image" src="https://github.com/user-attachments/assets/4ca60572-9267-4ba9-9e84-eff13ecadf7f" />


<img width="734" height="556" alt="image" src="https://github.com/user-attachments/assets/6f449cdc-290a-4639-a4a7-b57de846fe38" />

<img width="734" height="550" alt="image" src="https://github.com/user-attachments/assets/c4c5a4a9-9141-469e-827b-b72b077e2946" />

<img width="732" height="551" alt="image" src="https://github.com/user-attachments/assets/311f6675-1a6e-4c90-8472-34cf14a060fc" />

以上でFinish　VMを起動する

以下画面になる　Install Red Hat Enterprise Linux 10.0　でよいが、時間があるためISOチェックもするので上から2番目（default）

<img width="1568" height="917" alt="image" src="https://github.com/user-attachments/assets/c7d362ef-a76a-4f3f-805c-63ba3a449c9c" />

言語選択

<img width="1572" height="918" alt="image" src="https://github.com/user-attachments/assets/077c9fcb-ee0c-466a-beee-3058f23c056e" />

インストール先を確認

<img width="1570" height="919" alt="image" src="https://github.com/user-attachments/assets/a7557192-1e39-4147-b583-989e6cf24dce" />

デフォルトのまま　完了

<img width="1573" height="917" alt="image" src="https://github.com/user-attachments/assets/79f3ce57-0ae1-4b21-8559-23085516a990" />

rootアカウントを作成

<img width="1570" height="918" alt="image" src="https://github.com/user-attachments/assets/9c0d1986-c8b2-4a8d-b8b1-54a9da99a732" />

有効化　し、パスワードを設定

<img width="1570" height="918" alt="image" src="https://github.com/user-attachments/assets/00623edf-75ce-4b42-bd84-2678b032553e" />

ユーザーの作成

<img width="1570" height="920" alt="image" src="https://github.com/user-attachments/assets/5ed4b98c-3a50-4ea2-8360-3df485bac1cb" />

「インストールの開始」をクリック

インストール開始

<img width="1571" height="919" alt="image" src="https://github.com/user-attachments/assets/7bd7852f-de3e-4ce9-a26f-d01cd57054c9" />

インストール完了　再起動をクリック

<img width="1572" height="918" alt="image" src="https://github.com/user-attachments/assets/b1b96aca-1b86-4061-90bb-87a5ee028207" />

起動　先ほど作った一般ユーザーでログイン

<img width="1572" height="921" alt="image" src="https://github.com/user-attachments/assets/c5745dcc-7d81-44fa-8a51-85749624b2f0" />

ISOは外しておく

<img width="937" height="643" alt="image" src="https://github.com/user-attachments/assets/f3068e46-344c-4769-8765-4613dac88f74" />

Terminalを開き、ip a をすると、既にNICが上がっており、DHCPでIPが付与されている

固定IPに変更する
```
sudo nmcli connection modify ens18 ipv4.method manual
sudo nmcli connection modify ens18 ipv4.addresses 10.20.30.42/24
sudo nmcli connection modify ens18 ipv4.gateway 10.20.30.1
sudo nmcli connection modify ens18 ipv4.dns "8.8.8.8,1.1.1.1"
sudo nmcli connection up ens18
```
固定IPが付与され、他PCからもpingできる




## 物理マシン作成(OptiPlex5040)

ISOを入手

上記の、VMの時と同じもので良い

https://github.com/pkthom/rhel10/edit/main/README.md#vm%E3%82%92%E4%BD%9C%E6%88%90%E3%81%99%E3%82%8Bproxmox


USBに入れる

使うUSB名を特定（今回は/dev/disk8）
```
abc@Mac-mini ~ % diskutil list
...
/dev/disk5 (external, physical):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      GUID_partition_scheme                        *62.0 GB    disk5
   1:                 Apple_APFS Container disk8         62.0 GB    disk5s1
...
/dev/disk8 (synthesized):
   #:                       TYPE NAME                    SIZE       IDENTIFIER
   0:      APFS Container Scheme -                      +62.0 GB    disk8
                                 Physical Store disk5s1
   1:                APFS Volume photo                   59.5 GB    disk8s1
```
アンマウント
```
abc@Mac-mini ~ % diskutil unmountDisk /dev/disk5
Unmount of all volumes on disk5 was successful
```
書き込み
```
abc@Mac-mini ~ % sudo dd if=/Users/abc/Downloads/rhel-10.0-x86_64-dvd.iso of=/dev/rdisk5 bs=4m status=progress
Password:
  8455716864 bytes (8456 MB, 8064 MiB) transferred 612.042s, 14 MB/s
2018+1 records in
2018+1 records out
8465612800 bytes transferred in 612.615550 secs (13818802 bytes/sec)
abc@Mac-mini ~ %
```
取り出し
```
abc@Mac-mini ~ % sync
abc@Mac-mini ~ % diskutil eject /dev/disk5
Disk /dev/disk5 ejected
```
これはIgnoreでOK

<img width="314" height="307" alt="image" src="https://github.com/user-attachments/assets/29555e72-259b-4a55-8578-b1470296609a" />


固定IPを付与する

<img width="4032" height="608" alt="image" src="https://github.com/user-attachments/assets/d63b08a8-a9e4-45c0-89ab-65354b9d4bd9" />

<img width="1789" height="911" alt="image" src="https://github.com/user-attachments/assets/39e364db-d8d2-423a-9e5d-745796f335a6" />


手元の同VLAN内PCからSSHできる

updateしようとするが、サブスクリプションの登録をしなければ、レポジトリが使えるようにならない
```
ubuntu@localhost:~$ sudo dnf -y update
サブスクリプション管理リポジトリーを更新しています。
コンシューマー識別子を読み込めません

このシステムはエンタイトルメントサーバーに登録されていません。登録するには、"rhc" または "subscription-manager" を使用できます。

エラー: "/etc/yum.repos.d", "/etc/yum/repos.d", "/etc/distro.repos.d" には有効化されたリポジトリーがありません。
ubuntu@localhost:~$ 

```

以下で登録　※シングルクォーテーションじゃないとダメだった
```
sudo subscription-manager register --username 'ユーザー名' --password 'パスワード'
```
```
ubuntu@localhost:~$ sudo subscription-manager register --username 'ユーザー名' --password 'パスワード'
登録中: subscription.rhsm.redhat.com:443/subscription
このシステムは、次の ID で登録されました: abcde...
登録したシステム名: localhost.localdomain
ubuntu@localhost:~$ 
```

アップデート
```
ubuntu@localhost:~$ sudo dnf -y update
```

### ブリッジ構成を作成する


<img width="739" height="770" alt="image" src="https://github.com/user-attachments/assets/276f5237-073e-4419-ade3-e8cd0646270b" />


現状確認
```
ubuntu@localhost:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s31f6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether XX:XX:XX:XX:XX:XX brd ff:ff:ff:ff:ff:ff
    altname enx14b31f03d114
    inet 192.168.20.30/24 brd 192.168.20.255 scope global noprefixroute enp0s31f6
       valid_lft forever preferred_lft forever
    inet6 fe80::16b3:1fff:fe03:d114/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever
ubuntu@localhost:~$ 
```

ブリッジ作成（仮想スイッチの親玉となる「br0」インターフェースを作成）
```
ubuntu@localhost:~$ sudo nmcli connection add type bridge autoconnect yes con-name br0 ifname br0
```

ブリッジへのIP設定（物理ポートから引き継ぐ固定IP情報をブリッジ（br0）に書き込む）
```
ubuntu@localhost:~$ sudo nmcli connection modify br0 ipv4.addresses 192.168.20.30/24 ipv4.gateway 192.168.20.1 ipv4.dns "8.8.8.8,1.1.1.1" ipv4.method manual
Warning: nmcli (1.54.0) and NetworkManager (1.52.0) versions don't match. Restarting NetworkManager is advised.
```

物理ポートの紐付け（スレーブ化）（物理ポートを「br0」専用の差し込み口（スレーブ）として設定）
```
ubuntu@localhost:~$ sudo nmcli connection add type bridge-slave autoconnect yes con-name br0-slave ifname enp0s31f6 master br0
Warning: nmcli (1.54.0) and NetworkManager (1.52.0) versions don't match. Restarting NetworkManager is advised.
接続 'br0-slave' (05b38707-2786-4845-810e-20a5da2c4bf5) が正常に追加されました。

```

切り替え実行（物理ポート単体の設定を終了し、ブリッジを起動（ここでIPが移動するためSSHが一瞬切断される））
```
ubuntu@localhost:~$ sudo nmcli connection down enp0s31f6 && sudo nmcli connection up br0
Warning: nmcli (1.54.0) and NetworkManager (1.52.0) versions don't match. Restarting NetworkManager is advised.
Read from remote host 192.168.20.30: Operation timed out
Connection to 192.168.20.30 closed.
client_loop: send disconnect: Broken pipe
abc@Mac-mini ~ % 
```

IPが物理ポートから消え、ブリッジ（br0）に正しく移動したことを確認
```
ubuntu@localhost:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host noprefixroute 
       valid_lft forever preferred_lft forever
2: enp0s31f6: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel master br0 state UP group default qlen 1000
    link/ether XX:XX:XX:XX:XX:XX brd ff:ff:ff:ff:ff:ff
    altname enx14b31f03d114
3: br0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether XX:XX:XX:XX:XX:XX brd ff:ff:ff:ff:ff:ff
    inet 192.168.20.30/24 brd 192.168.20.255 scope global noprefixroute br0
       valid_lft forever preferred_lft forever
    inet6 fe80::5d:c35:6710:4184/64 scope link noprefixroute 
       valid_lft forever preferred_lft forever

# デフォルトゲートウェイが br0 を向いていることを確認
ubuntu@localhost:~$ route -n
Kernel IP routing table
Destination     Gateway         Genmask         Flags Metric Ref    Use Iface
0.0.0.0         192.168.20.1    0.0.0.0         UG    425    0        0 br0
192.168.20.0    0.0.0.0         255.255.255.0   U     425    0        0 br0

# ブリッジ経由で外部ネットに通信できることを確認（疎通OK！）
ubuntu@localhost:~$ ping -c 3 8.8.8.8
PING 8.8.8.8 (8.8.8.8) 56(84) バイトのデータ
64 バイト応答 送信元 8.8.8.8: icmp_seq=1 ttl=116 時間=7.72ミリ秒
64 バイト応答 送信元 8.8.8.8: icmp_seq=2 ttl=116 時間=7.08ミリ秒
64 バイト応答 送信元 8.8.8.8: icmp_seq=3 ttl=116 時間=7.22ミリ秒

--- 8.8.8.8 ping 統計 ---
送信パケット数 3, 受信パケット数 3, 0% packet loss, time 2004ms
rtt min/avg/max/mdev = 7.078/7.337/7.720/0.276 ms
ubuntu@localhost:~$ 
```

最小パッケージを入れる（KVM + libvirt + virt-install）
```
ubuntu@localhost:~$ sudo dnf install -y qemu-kvm libvirt virt-install cockpit-machines
```
- qemu-kvm / libvirt: 仮想化のエンジン。
- virt-install: コマンドラインからVMを作る際に必要。
- cockpit-machines: これを入れることで、ブラウザからVMを作成・操作（コンソール表示）できるようになります。


仮想化サービスと管理GUIを起動・有効化
```
sudo systemctl enable --now libvirtd
sudo systemctl enable --now cockpit.socket
```

ブラウザからアクセス

https://192.168.20.30:9090/

<img width="1440" height="1069" alt="image" src="https://github.com/user-attachments/assets/8e90e445-c11f-4893-8c20-6d279b48566b" />

<img width="1622" height="1024" alt="image" src="https://github.com/user-attachments/assets/97b1cf4d-4f51-4403-b488-99c2149a2bca" />


特権アクセスを有効にする(これを行わないと、仮想マシンの作成などのシステム変更ができません。)

画面右上の青いボタン [Turn on administrative access] をクリック

ISOを送り込む
```
abc@Mac-mini Downloads % scp rhel-9.7-x86_64-dvd.iso root@192.168.20.30:/var/lib/libvirt/images/
```
送り込めた　（ちなみに、/var/lib/libvirt/images　は最初からあった）
```
root@localhost:/var/lib/libvirt/images# pwd
/var/lib/libvirt/images
root@localhost:/var/lib/libvirt/images# ls
rhel-9.7-x86_64-dvd.iso
```
権限を調整しておく
```
root@localhost:/var/lib/libvirt/images# chown qemu:qemu rhel-9.7-x86_64-dvd.iso 
root@localhost:/var/lib/libvirt/images# ls -lah
合計 13G
drwx--x--x. 2 root root  37  1月 25 20:16 .
drwxr-xr-x. 9 root root 106  1月 25 19:59 ..
-rw-r--r--. 1 qemu qemu 13G  1月 25 20:18 rhel-9.7-x86_64-dvd.iso
root@localhost:/var/lib/libvirt/images# 
```

### RHEL9 VMを作る

<img width="1613" height="1018" alt="image" src="https://github.com/user-attachments/assets/35ea73f4-a513-4c8c-91d1-00044a3aa172" />

先ほど送り込んだISOが見えるので、それを選択し、「作成して編集する」をクリック（ブリッジになっているか見るため）　※あとは全部デフォルト

<img width="1433" height="951" alt="image" src="https://github.com/user-attachments/assets/906eec24-4c0a-4219-85fc-56284225be80" />

NIC　が　br0　になっているので、そのまま「インストール」をクリック 

<img width="1631" height="1297" alt="image" src="https://github.com/user-attachments/assets/e0e1da09-8c94-49ef-aef3-1068a451a89b" />

時間あるので、メディアをTestしてインストールをクリック

<img width="1396" height="799" alt="image" src="https://github.com/user-attachments/assets/23d3986f-890b-4bd7-964a-2ba1a732ff7e" />

RHEL9のインストール始まる

<img width="1666" height="1283" alt="image" src="https://github.com/user-attachments/assets/4f75017c-d318-4bcb-b5ba-44a0a8d5dd79" />

<img width="1393" height="1136" alt="image" src="https://github.com/user-attachments/assets/46aa65fb-9c16-425d-a1ac-0c056ada665d" />

<img width="1668" height="1289" alt="image" src="https://github.com/user-attachments/assets/379f1d95-13f7-4a8e-96fa-66c107a0932a" />


インストール完了

<img width="1659" height="1286" alt="image" src="https://github.com/user-attachments/assets/fdd12ce3-3609-49b3-ab23-d06059741f54" />

<img width="1657" height="1273" alt="image" src="https://github.com/user-attachments/assets/bc7999f2-2627-4a8e-a0ad-526f62679336" />

<img width="1659" height="1288" alt="image" src="https://github.com/user-attachments/assets/de7e5718-a276-4b26-9350-193e4a3a21aa" />

ブリッジが効いていて、IPが振られている

<img width="1649" height="1243" alt="image" src="https://github.com/user-attachments/assets/608bb3dd-9c56-4540-b446-3439198b0a81" />

SSHできる

毎回のように、アカウント登録し、レポジトリを有効にする
```
[root@localhost ~]# dnf -y update
サブスクリプション管理リポジトリーを更新しています。
コンシューマー識別子を読み込めません

このシステムはエンタイトルメントサーバーに登録されていません。登録するには、"rhc" または "subscription-manager" を使用できます。

エラー: "/etc/yum.repos.d", "/etc/yum/repos.d", "/etc/distro.repos.d" には有効化されたリポジトリーがありません。
[root@localhost ~]# subscription-manager register --username 'ユーザー名' --password 'パスワード'
登録中: subscription.rhsm.redhat.com:443/subscription
このシステムは、次の ID で登録されました: abcde...
登録したシステム名: localhost.localdomain
[root@localhost ~]# 
```
変換サーバーになるために必要なツールをインストールする
```
[root@localhost ~]# sudo dnf install -y virt-v2v libguestfs-tools
```

### virt-p2v ISO を公式からダウンロードする

https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.9/x86_64/product-software

<img width="1595" height="1175" alt="image" src="https://github.com/user-attachments/assets/73264322-83ff-48dc-bc5c-7c92601d1e1b" />

<img width="733" height="389" alt="image" src="https://github.com/user-attachments/assets/dd3819b2-1880-4ad2-b385-a40af189b830" />
