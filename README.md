
# SSH公開鍵を配布するPlaybook (Ansible)

## 必要なツール
MacOS
- Ansible
- sshpass
    ```
    brew install hudochenkov/sshpass/sshpass
    ```

### SSH公開鍵認証の設定
公開鍵の配布：
```
ansible-playbook -i inventory.ini setup-bastions.yml setup-privates.yml
```
配布した公開鍵の削除：
```
ansible-playbook -i inventory.ini setup-privates.yml setup-bastions.yml -e "state=absent"
```

### Playbook実行時の流れ
1. SSH鍵生成（作成済みの場合は何もしない）
2. ~/.ssh/configの修正（踏み台サーバの情報追加）
3. 踏み台サーバに公開鍵配布
4. ~/.ssh/configの修正（プライベートサーバの情報追加）
5. 踏み台経由でプライベートサーバに公開鍵配布

<br>

----

以下、開発者向けの内容です。

### 検証環境の作成手順

- 踏み台サーバ (bastion)：CentOS
    ```
    useradd -m ssh1-1
    echo "P@ssw0rd-A" | passwd --stdin ssh1-1
    ```
- サーバ1 (private1)：CentOS
    ```
    useradd -m user1
    usermod -aG wheel user1
    echo "P@ssw0rd-B" | passwd --stdin user1
    ```
- サーバ2 (private2)：Ubuntu
    ```
    useradd -m user1
    usermod -aG sudo user1
    echo "user1:P@ssw0rd-C" | chpasswd
    ```
