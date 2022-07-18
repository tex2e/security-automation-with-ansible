
# SSH公開鍵を配布するPlaybook (Ansible)

## 必要なツール
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
1. SSH鍵生成
2. 公開鍵配布
3. ~/.ssh/configの修正

----

<br>

### 検証環境

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
