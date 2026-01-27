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

変換サーバー準備完了

### virt-p2v ISO を公式からダウンロードする

https://access.redhat.com/downloads/content/69/ver=/rhel---7/7.9/x86_64/product-software

<img width="1595" height="1175" alt="image" src="https://github.com/user-attachments/assets/73264322-83ff-48dc-bc5c-7c92601d1e1b" />

<img width="733" height="389" alt="image" src="https://github.com/user-attachments/assets/dd3819b2-1880-4ad2-b385-a40af189b830" />

USBに焼く
```
abc@Mac-mini Downloads % sudo dd if=virt-p2v-1.40.2-1.el7.iso of=/dev/rdisk4 bs=4m status=progress
Password:
  301989888 bytes (302 MB, 288 MiB) transferred 4.044s, 75 MB/s
88+1 records in
88+1 records out
372244480 bytes transferred in 4.970150 secs (74896025 bytes/sec)
abc@Mac-mini Downloads % sync
abc@Mac-mini Downloads % diskutil eject /dev/disk4
Disk /dev/disk4 ejected
abc@Mac-mini Downloads % 
```

<img width="1477" height="794" alt="image" src="https://github.com/user-attachments/assets/a9265425-190a-4ae2-afdd-ee26521aa28e" />

<img width="753" height="516" alt="image" src="https://github.com/user-attachments/assets/b8adcffd-90e8-48c6-a69f-a6007b3f2cd9" />


### RHEL7 VMを作る

```
abc@Mac-mini Downloads % scp rhel-server-7.9-x86_64-dvd.iso root@192.168.20.30:/var/lib/libvirt/images/
```
```
root@localhost:/var/lib/libvirt/images# chown qemu:qemu rhel-server-7.9-x86_64-dvd.iso 
root@localhost:/var/lib/libvirt/images# ls -lah
合計 26G
drwx--x--x. 2 root root   94  1月 25 22:27 .
drwxr-xr-x. 9 root root  106  1月 25 19:59 ..
-rw-r--r--. 1 qemu qemu  13G  1月 25 20:18 rhel-9.7-x86_64-dvd.iso
-rw-r--r--. 1 qemu qemu 4.3G  1月 25 22:27 rhel-server-7.9-x86_64-dvd.iso
-rw-------. 1 qemu qemu  11G  1月 25 22:29 rhel9.qcow2
root@localhost:/var/lib/libvirt/images# 
```

<img width="1635" height="1057" alt="image" src="https://github.com/user-attachments/assets/4b346047-bde0-4ad5-bb9b-b5e1246c5c27" />

<img width="1648" height="1296" alt="image" src="https://github.com/user-attachments/assets/537ab270-e338-466e-b4a7-0f7d48ab7308" />

<img width="1671" height="1270" alt="image" src="https://github.com/user-attachments/assets/7853856d-239a-47da-839e-025dda833485" />

<img width="1643" height="1261" alt="image" src="https://github.com/user-attachments/assets/3df9bcf6-0fa1-492d-8af9-7b45dcf3f0b6" />

Ethernet を「オン」にする　（オフになっていた）

<img width="1644" height="1245" alt="image" src="https://github.com/user-attachments/assets/363dd2d8-7287-408a-8f3a-f6d1d8ab9962" />

インストール開始

<img width="1643" height="1251" alt="image" src="https://github.com/user-attachments/assets/e274770d-29c4-4fa6-af79-4e6054d1fe3a" />

この２つは設定しておく

<img width="1646" height="1243" alt="image" src="https://github.com/user-attachments/assets/8ddca4f8-7995-4787-913a-6c5044f9c004" />

ubuntuユーザーは、rootになれるようにしておく

<img width="1653" height="1242" alt="image" src="https://github.com/user-attachments/assets/66516867-2de2-4a13-b499-0e32e43b2aa6" />

<img width="1651" height="1251" alt="image" src="https://github.com/user-attachments/assets/b5799b5d-15a8-4113-b99f-d31b566d0839" />

<img width="1642" height="1236" alt="image" src="https://github.com/user-attachments/assets/4c2b341e-e8cd-4c2e-a5f4-f3bdca1f5ea2" />

いつものように、アカウント登録して、レポジトリを使えるようにする
```
[ubuntu@rhel7 ~]$ sudo yum update
[sudo] password for ubuntu: 
Failed to set locale, defaulting to C
Loaded plugins: product-id, search-disabled-repos, subscription-manager

This system is not registered with an entitlement server. You can use subscription-manager to register.

There are no enabled repos.
 Run "yum repolist all" to see the repos you have.
 To enable Red Hat Subscription Management repositories:
     subscription-manager repos --enable <repo>
 To enable custom repositories:
     yum-config-manager --enable <repo>
[ubuntu@rhel7 ~]$ 
```

```
[ubuntu@rhel7 ~]$ sudo subscription-manager register --username 'username' --password 'password'
You are attempting to use a locale that is not installed.
Registering to: subscription.rhsm.redhat.com:443/subscription
The system has been registered with ID: 1713ffa0-0678-4624-ab0e-db147eacbd0b
The registered system name is: rhel7
[ubuntu@rhel7 ~]$ 

```

```
[ubuntu@rhel7 ~]$ sudo subscription-manager repos --enable="rhel-7-server-rpms"
You are attempting to use a locale that is not installed.
Repository 'rhel-7-server-rpms' is enabled for this system.
[ubuntu@rhel7 ~]$ sudo yum update -y
Failed to set locale, defaulting to C
Loaded plugins: product-id, search-disabled-repos, subscription-manager
No packages marked for update
[ubuntu@rhel7 ~]$ 
```
```
[ubuntu@rhel7 ~]$ sudo yum install -y virt-v2v
```
libvirtdを起動し有効化したら、変換サーバーとしての準備OK
```
[ubuntu@rhel7 ~]$ sudo systemctl start libvirtd
[sudo] password for ubuntu: 
[ubuntu@rhel7 ~]$ sudo systemctl enable libvirtd
[ubuntu@rhel7 ~]$ sudo systemctl status libvirtd
● libvirtd.service - Virtualization daemon
   Loaded: loaded (/usr/lib/systemd/system/libvirtd.service; enabled; vendor preset: enabled)
   Active: active (running) since Sun 2026-01-25 23:10:51 JST; 20s ago
     Docs: man:libvirtd(8)
           https://libvirt.org
 Main PID: 25885 (libvirtd)
   CGroup: /system.slice/libvirtd.service
           └─25885 /usr/sbin/libvirtd

Jan 25 23:10:51 rhel7 systemd[1]: Starting Virtualization daemon...
Jan 25 23:10:51 rhel7 systemd[1]: Started Virtualization daemon.
[ubuntu@rhel7 ~]$ 
```

※ちなみに、libvirtdを起動できていなくて、一度以下のエラー終了した

<img width="1024" height="789" alt="image" src="https://github.com/user-attachments/assets/254007f1-9950-4df1-9020-2c4e8e8f64b6" />

<img width="714" height="190" alt="image" src="https://github.com/user-attachments/assets/0d34cc3d-a1c6-4f0a-9165-248770beee35" />


---

変換開始

virt-p2v USBを物理に指して、起動→F12

USBから起動を選ぶ

<img width="1539" height="1108" alt="image" src="https://github.com/user-attachments/assets/24f9a4eb-c054-49c6-97c6-1dcffc841bd6" />

Enterを押す

<img width="1528" height="956" alt="image" src="https://github.com/user-attachments/assets/a5bd5b0e-8193-461c-9a79-23ba996a3afe" />

virt-p2v GUI が起動するので、変換サーバー（今回はRHEL7）の情報を入力し、接続テストを行う

<img width="1412" height="719" alt="image" src="https://github.com/user-attachments/assets/1111b46d-5ce4-4a29-aa93-957709f5dcb2" />

↑ 接続テスト成功

これがデフォルト画面

<img width="1511" height="1041" alt="image" src="https://github.com/user-attachments/assets/a8366104-e75c-47ad-821f-af273e0dc825" />

以下のように変えて、「Start Conversion」をクリック

<img width="1502" height="1051" alt="image" src="https://github.com/user-attachments/assets/fcc6ac2e-4d05-4dd8-bf81-aa6cd31738f7" />

各パラメーターについて

<img width="776" height="940" alt="image" src="https://github.com/user-attachments/assets/2a52b052-564f-418f-adfe-376b7311ac99" />


<img width="752" height="354" alt="image" src="https://github.com/user-attachments/assets/4133cccb-ee40-456b-b683-3580a7520b80" />

空き容量についての忠告　（のちにこれで失敗する）

<img width="739" height="414" alt="image" src="https://github.com/user-attachments/assets/58fca2b2-cdd3-4c22-85d6-11179fb058ce" />


失敗

<img width="1378" height="1039" alt="image" src="https://github.com/user-attachments/assets/724568f8-3d9e-455d-b992-f8697b4a7b6e" />

以下、ログ全文

<img width="1427" height="1093" alt="image" src="https://github.com/user-attachments/assets/82fff03d-741c-4c76-9416-9523ba4ccc78" />


<img width="1505" height="1120" alt="image" src="https://github.com/user-attachments/assets/6805a905-fa2a-45cd-b8f4-5b9f19f8c51b" />


<img width="1425" height="1095" alt="image" src="https://github.com/user-attachments/assets/c6c4ac74-eac2-44fa-a458-a5243252249d" />

<img width="740" height="662" alt="image" src="https://github.com/user-attachments/assets/8a91fd50-4963-4a5e-b7c2-e3f82f0a21a5" />

<img width="742" height="751" alt="image" src="https://github.com/user-attachments/assets/4c9b8843-1c5e-4d16-8f98-75003e751990" />

GUI上で、１０GBであることがわかるが、GUIからは変更できなかった

<img width="1686" height="977" alt="image" src="https://github.com/user-attachments/assets/80c76b73-02cc-48a8-a5bd-8e438b05d8f4" />

以下から　拡大手順
```

root@localhost:/var/lib/libvirt/images# qemu-img resize rhel7.qcow2 150G
Image resized.
root@localhost:/var/lib/libvirt/images# 
```


```
[ubuntu@rhel7 ~]$ sudo yum install -y cloud-utils-growpart

```

vda が 150G になっているのが確認できました！ただ、中身のパーティション（vda2）と LVM（rhel-root）がまだ 8G〜9G のままなので、ここを広げる必要があります。
```
[ubuntu@rhel7 ~]$ lsblk -l
NAME      MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sr0        11:0    1  4.2G  0 rom  
vda       252:0    0  150G  0 disk 
vda1      252:1    0    1G  0 part /boot
vda2      252:2    0    9G  0 part 
rhel-root 253:0    0    8G  0 lvm  /
rhel-swap 253:1    0    1G  0 lvm  [SWAP]
[ubuntu@rhel7 ~]$
[ubuntu@rhel7 ~]$ df -h
Filesystem             Size  Used Avail Use% Mounted on
devtmpfs               908M     0  908M   0% /dev
tmpfs                  919M     0  919M   0% /dev/shm
tmpfs                  919M  8.7M  911M   1% /run
tmpfs                  919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/rhel-root  8.0G  2.8G  5.3G  35% /
/dev/vda1             1014M  195M  820M  20% /boot
tmpfs                  184M     0  184M   0% /run/user/1000
[ubuntu@rhel7 ~]$ 
```

パーティションを広げる
まずは、物理的な枠組みである vda2 パーティションを最後まで広げます。
```
# vda の 2番目のパーティション（vda2）を最大まで拡張
[ubuntu@rhel7 ~]$ sudo growpart /dev/vda 2
CHANGED: partition=2 start=2099200 old: size=18872320 end=20971520 new: size=312473567 end=314572767
```
LVM の物理ボリュームを広げる
OSに「枠が広がったよ」と教えます。
```
[ubuntu@rhel7 ~]$ sudo pvresize /dev/vda2
  Physical volume "/dev/vda2" changed
  1 physical volume(s) resized or updated / 0 physical volume(s) not resized
```
論理ボリュームとファイルシステムを同時に広げる
最後に、/ がマウントされている rhel-root を、増えた分だけ（100%）広げます。
```
# -r オプションでファイルシステム（XFS）の拡張も同時に行います
[ubuntu@rhel7 ~]$ sudo lvextend -l +100%FREE -r /dev/mapper/rhel-root
  Size of logical volume rhel/root changed from <8.00 GiB (2047 extents) to <148.00 GiB (37887 extents).
  Logical volume rhel/root successfully resized.
meta-data=/dev/mapper/rhel-root  isize=512    agcount=4, agsize=524032 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0 spinodes=0
data     =                       bsize=4096   blocks=2096128, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal               bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
data blocks changed from 2096128 to 38796288
```
コマンドが終わったら、もう一度確認
```
[ubuntu@rhel7 ~]$ df -h
Filesystem             Size  Used Avail Use% Mounted on
devtmpfs               908M     0  908M   0% /dev
tmpfs                  919M     0  919M   0% /dev/shm
tmpfs                  919M  8.7M  911M   1% /run
tmpfs                  919M     0  919M   0% /sys/fs/cgroup
/dev/mapper/rhel-root  148G  2.8G  146G   2% /
/dev/vda1             1014M  195M  820M  20% /boot
tmpfs                  184M     0  184M   0% /run/user/1000
[ubuntu@rhel7 ~]$ 
```

先ほどと全く一緒の手順・パラメーターで、変換再チャレンジ

<img width="1449" height="740" alt="image" src="https://github.com/user-attachments/assets/1fde0429-c1c8-43cd-acaa-98201c4b873a" />

<img width="1466" height="999" alt="image" src="https://github.com/user-attachments/assets/c1f8be96-a2d9-4385-9b28-8423514e09ac" />


☑️今度は成功

<img width="1447" height="1088" alt="image" src="https://github.com/user-attachments/assets/95344e6c-69dc-4a5f-8c39-e0a5e4345e2a" />

※ログ全文

<img width="1446" height="1077" alt="image" src="https://github.com/user-attachments/assets/d6854b6b-c7b8-4d34-942d-8518773a8498" />

<img width="1390" height="1065" alt="image" src="https://github.com/user-attachments/assets/2a7ae659-d580-4896-a651-3371ffa86241" />

このマシンはもう使わないので、「Shutdown...」ボタンより、シャットダウンする　LANケーブルも抜いていい

<img width="746" height="188" alt="image" src="https://github.com/user-attachments/assets/91e365c7-ff21-4ea4-992d-e1db232a9976" />


確認

<img width="745" height="364" alt="image" src="https://github.com/user-attachments/assets/218519b9-c57e-4510-b21b-fb6051d210d3" />

確かにある
```
[root@rhel7 images]# pwd
/var/lib/libvirt/images
[root@rhel7 images]# ls -lah
合計 15G
drwx--x--x. 2 root root   52  1月 27 00:32 .
drwxr-xr-x. 9 root root  106  1月 25 23:01 ..
-rw-r--r--. 1 root root  15G  1月 27 00:32 centos5-p2v-sda
-rw-r--r--. 1 root root 1.4K  1月 27 00:32 centos5-p2v.xml
[root@rhel7 images]# 
```

RHEL10から、RHEL7へ、centos5-p2v-sda をコピー
```
root@localhost:/var/lib/libvirt/images# scp root@192.168.20.54:/var/lib/libvirt/images/centos5-p2v-sda /var/lib/libvirt/images/
```
ファイルの所有者を libvirt (qemu) に変更する
```
root@localhost:/var/lib/libvirt/images# ls -lah
合計 63G
drwx--x--x. 2 root root  136  1月 27 06:37 .
drwxr-xr-x. 9 root root  106  1月 25 19:59 ..
-rw-r--r--. 1 root root  15G  1月 27 06:40 centos5-p2v-sda
-rw-r--r--. 1 qemu qemu  13G  1月 25 20:18 rhel-9.7-x86_64-dvd.iso
-rw-r--r--. 1 qemu qemu 4.3G  1月 25 22:27 rhel-server-7.9-x86_64-dvd.iso
-rw-------. 1 qemu qemu  24G  1月 27 07:10 rhel7.qcow2
-rw-------. 1 root root  11G  1月 27 06:44 rhel9.qcow2
root@localhost:/var/lib/libvirt/images# chown qemu:qemu centos5-p2v-sda 
root@localhost:/var/lib/libvirt/images# ls -lah
合計 63G
drwx--x--x. 2 root root  136  1月 27 06:37 .
drwxr-xr-x. 9 root root  106  1月 25 19:59 ..
-rw-r--r--. 1 qemu qemu  15G  1月 27 06:40 centos5-p2v-sda
-rw-r--r--. 1 qemu qemu  13G  1月 25 20:18 rhel-9.7-x86_64-dvd.iso
-rw-r--r--. 1 qemu qemu 4.3G  1月 25 22:27 rhel-server-7.9-x86_64-dvd.iso
-rw-------. 1 qemu qemu  24G  1月 27 07:10 rhel7.qcow2
-rw-------. 1 root root  11G  1月 27 06:44 rhel9.qcow2
root@localhost:/var/lib/libvirt/images# 
```

※ 権限変更しないと、エラー　Permission denied　になった　↓

<img width="650" height="291" alt="image" src="https://github.com/user-attachments/assets/af332057-cdae-40ab-aa01-79e6d5a3470f" />


<img width="774" height="524" alt="image" src="https://github.com/user-attachments/assets/11a57192-ab45-47da-be00-b53f5d11de95" />


RHEL10のCockpit画面で、「VMのインポート」　をクリック

ディスクイメージで、scp してきた　centos5-p2v-sda　を選択

<img width="1709" height="893" alt="image" src="https://github.com/user-attachments/assets/18cde2f5-e6a1-4bbd-a857-b20105144e36" />

オペレーティングシステム　で、OSを選ぶ　手動　今回はCentOS5.11

メモリも、良い値を選ぶ

インポートして編集する　をクリック

以下をチェック

<img width="740" height="214" alt="image" src="https://github.com/user-attachments/assets/09c7b628-7ed2-47af-b36c-2804fa725905" />

問題なし　黄色枠内

<img width="1691" height="1118" alt="image" src="https://github.com/user-attachments/assets/a342b110-f703-49b8-8217-5913b9706c90" />


実行　をクリックすると、起動する

<img width="1715" height="1137" alt="image" src="https://github.com/user-attachments/assets/ea4e3eb2-1cc5-433f-8d9a-4a30eeb83d17" />

※ 上記はシャットダウンボタン

ログインする　NICにIPついてない

<img width="1450" height="965" alt="Screenshot 2026-01-27 at 8 38 50" src="https://github.com/user-attachments/assets/d9a0cd3b-562b-4f7c-b927-63483bdad9ea" />

/etc/sysconfig/network-scripts/ifcfg-eth0 が以下のようになっているので、HWADDR の行を消して、、、

<img width="1367" height="947" alt="Screenshot 2026-01-27 at 8 42 23" src="https://github.com/user-attachments/assets/21db328c-3975-4a15-9de5-4db48314e344" />

service network restart すると、元のIPが戻り、通信可能になる

<img width="1454" height="933" alt="Screenshot 2026-01-27 at 8 43 14" src="https://github.com/user-attachments/assets/2fa4e066-7d54-457b-8f4d-d83573b85b96" />


RHEL4の場合

<img width="2035" height="1285" alt="image" src="https://github.com/user-attachments/assets/e13e4d65-ef25-4bc4-b36c-2a7b443be5d8" />
