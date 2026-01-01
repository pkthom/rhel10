# rhel10

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
