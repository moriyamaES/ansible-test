# 目的

- 「標準テキスト CentOS 8 構築・運用・管理パーフェクトガイド」に記述のAnsibleの説明を実施

## sshクライアントのインストール

- 環境

    ```
    # uname -a
    Linux control-plane.minikube.internal 6.4.10-1.el7.elrepo.x86_64 #1 SMP PREEMPT_DYNAMIC Fri Aug 11 12:54:18 EDT 2023 x86_64 x86_64 x86_64 GNU/Linux
    ```

- 以下のコマンドを実行 

    ```
    yum install openssh-clients
    ```

    ```
    # rpm -q openssh-clients
    openssh-clients-7.4p1-23.el7_9.x86_64
    ```

- epel-releaseパッケージのインストール

    ```
    # yum install epel-release
    読み込んだプラグイン:fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
    * base: mirrors.cat.net
    * elrepo: mirror.cedia.org.ec
    * extras: mirrors.bfsu.edu.cn
    * updates: mirrors.cat.net
    一致したパッケージ epel-release-7-11.noarch はすでにインストールされています。更新を確認しています。
    何もしません    
    ```
- ansible のインストール

    ```
    # yum install ansible
    読み込んだプラグイン:fastestmirror, langpacks
    Loading mirror speeds from cached hostfile
    * base: mirrors.cat.net
    * elrepo: mirror.cedia.org.ec
    * extras: mirrors.bfsu.edu.cn
    * updates: mirrors.cat.net
    パッケージ ansible は利用できません。
    エラー: 何もしません
    ```

- 【問題発生！！】ansibleがインストールできない

- 【対策→解決せず】以下のサイトに従い、イントールをやり直してみたが、上手くいかず

    https://ozashu.hatenablog.com/entry/2016/09/04/072308

    ```
    # yum install epel-release
    ```

    ```
    # wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
    # sudo rpm -Uvh epel-release-latest-7.noarch.rpm
    ```

    ```
    # yum install ansible
    ```

- 【対策→解決せず】以下のコマンドを実行したが、上手くいかず

    ```
    # yum clean all
    ```

    ```
    # yum clean all
    ```

    ```
    # yum install ansible
    ```



- 【問題解決】chatGptの回答に従い、以下のコマンドで、イントールを実行したところ解決
- 依存関係の問題: ansible パッケージは他のパッケージに依存している場合があります。依存しているパッケージがインストールされていない可能性があるため、依存関係を解決する必要があります。以下--verbのコマンドを試してみてください。

    ```
    yum install ansible --enablerepo=epel -y
    ```

    - 結果（イントール成功！！）

    ```
    (途中から)
    --> トランザクションの確認を実行しています。
    ---> パッケージ python-pycparser.noarch 0:2.14-1.el7 を インストール
    --> 依存性の処理をしています: python-ply のパッケージ: python-pycparser-2.14-1.el7.noarch
    --> トランザクションの確認を実行しています。
    ---> パッケージ python-ply.noarch 0:3.4-11.el7 を インストール
    --> 依存性解決を終了しました。

    依存性を解決しました

    ====================================================================================================
    Package                         アーキテクチャー  バージョン               リポジトリー       容量
    ====================================================================================================
    インストール中:
    ansible                         noarch            2.9.27-1.el7             epel               17 M
    依存性関連でのインストールをします:
    python-babel                    noarch            0.9.6-8.el7              base              1.4 M
    python-cffi                     x86_64            1.6.0-5.el7              base              218 k
    python-enum34                   noarch            1.0.4-1.el7              base               52 k
    python-idna                     noarch            2.4-1.el7                base               94 k
    python-jinja2                   noarch            2.7.2-4.el7              base              519 k
    python-markupsafe               x86_64            0.11-10.el7              base               25 k
    python-paramiko                 noarch            2.1.1-9.el7              base              269 k
    python-ply                      noarch            3.4-11.el7               base              123 k
    python-pycparser                noarch            2.14-1.el7               base              104 k
    python2-cryptography            x86_64            1.7.2-2.el7              base              502 k
    python2-httplib2                noarch            0.18.1-3.el7             epel              125 k
    python2-jmespath                noarch            0.9.4-2.el7              epel               41 k
    python2-pyasn1                  noarch            0.1.9-7.el7              base              100 k
    sshpass                         x86_64            1.06-2.el7               extras             21 k

    トランザクションの要約
    ====================================================================================================
    インストール  1 パッケージ (+14 個の依存関係のパッケージ)

    総ダウンロード容量: 21 M
    インストール容量: 119 M
    Downloading packages:
    (1/15): python-idna-2.4-1.el7.noarch.rpm                                     |  94 kB  00:00:00     
    (2/15): python-enum34-1.0.4-1.el7.noarch.rpm                                 |  52 kB  00:00:00     
    (3/15): python-markupsafe-0.11-10.el7.x86_64.rpm                             |  25 kB  00:00:00     
    (4/15): python-jinja2-2.7.2-4.el7.noarch.rpm                                 | 519 kB  00:00:01     
    (5/15): python-ply-3.4-11.el7.noarch.rpm                                     | 123 kB  00:00:00     
    (6/15): python-paramiko-2.1.1-9.el7.noarch.rpm                               | 269 kB  00:00:01     
    (7/15): python-pycparser-2.14-1.el7.noarch.rpm                               | 104 kB  00:00:00     
    (8/15): python-cffi-1.6.0-5.el7.x86_64.rpm                                   | 218 kB  00:00:03     
    (9/15): python2-cryptography-1.7.2-2.el7.x86_64.rpm                          | 502 kB  00:00:00     
    (10/15): python2-httplib2-0.18.1-3.el7.noarch.rpm                            | 125 kB  00:00:01     
    (11/15): python2-jmespath-0.9.4-2.el7.noarch.rpm                             |  41 kB  00:00:00     
    (12/15): python2-pyasn1-0.1.9-7.el7.noarch.rpm                               | 100 kB  00:00:00     
    (13/15): sshpass-1.06-2.el7.x86_64.rpm                                       |  21 kB  00:00:01     
    (14/15): python-babel-0.9.6-8.el7.noarch.rpm                                 | 1.4 MB  00:00:08     
    (15/15): ansible-2.9.27-1.el7.noarch.rpm                                     |  17 MB  00:00:15     
    ----------------------------------------------------------------------------------------------------
    合計                                                                1.4 MB/s |  21 MB  00:00:15     
    Running transaction check
    Running transaction test
    Transaction test succeeded
    Running transaction
    インストール中          : python2-pyasn1-0.1.9-7.el7.noarch                                  1/15 
    インストール中          : python-enum34-1.0.4-1.el7.noarch                                   2/15 
    インストール中          : sshpass-1.06-2.el7.x86_64                                          3/15 
    インストール中          : python2-httplib2-0.18.1-3.el7.noarch                               4/15 
    インストール中          : python-babel-0.9.6-8.el7.noarch                                    5/15 
    インストール中          : python2-jmespath-0.9.4-2.el7.noarch                                6/15 
    インストール中          : python-ply-3.4-11.el7.noarch                                       7/15 
    インストール中          : python-pycparser-2.14-1.el7.noarch                                 8/15 
    インストール中          : python-cffi-1.6.0-5.el7.x86_64                                     9/15 
    インストール中          : python-markupsafe-0.11-10.el7.x86_64                              10/15 
    インストール中          : python-jinja2-2.7.2-4.el7.noarch                                  11/15 
    インストール中          : python-idna-2.4-1.el7.noarch                                      12/15 
    インストール中          : python2-cryptography-1.7.2-2.el7.x86_64                           13/15 
    インストール中          : python-paramiko-2.1.1-9.el7.noarch                                14/15 
    インストール中          : ansible-2.9.27-1.el7.noarch                                       15/15 
    検証中                  : python-idna-2.4-1.el7.noarch                                       1/15 
    検証中                  : python-markupsafe-0.11-10.el7.x86_64                               2/15 
    検証中                  : python-ply-3.4-11.el7.noarch                                       3/15 
    検証中                  : ansible-2.9.27-1.el7.noarch                                        4/15 
    検証中                  : python-paramiko-2.1.1-9.el7.noarch                                 5/15 
    検証中                  : python2-jmespath-0.9.4-2.el7.noarch                                6/15 
    検証中                  : python-babel-0.9.6-8.el7.noarch                                    7/15 
    検証中                  : python2-httplib2-0.18.1-3.el7.noarch                               8/15 
    検証中                  : python-cffi-1.6.0-5.el7.x86_64                                     9/15 
    検証中                  : sshpass-1.06-2.el7.x86_64                                         10/15 
    検証中                  : python-jinja2-2.7.2-4.el7.noarch                                  11/15 
    検証中                  : python2-pyasn1-0.1.9-7.el7.noarch                                 12/15 
    検証中                  : python-enum34-1.0.4-1.el7.noarch                                  13/15 
    検証中                  : python-pycparser-2.14-1.el7.noarch                                14/15 
    検証中                  : python2-cryptography-1.7.2-2.el7.x86_64                           15/15 

    インストール:
    ansible.noarch 0:2.9.27-1.el7                                                                     

    依存性関連をインストールしました:
    python-babel.noarch 0:0.9.6-8.el7               python-cffi.x86_64 0:1.6.0-5.el7                  
    python-enum34.noarch 0:1.0.4-1.el7              python-idna.noarch 0:2.4-1.el7                    
    python-jinja2.noarch 0:2.7.2-4.el7              python-markupsafe.x86_64 0:0.11-10.el7            
    python-paramiko.noarch 0:2.1.1-9.el7            python-ply.noarch 0:3.4-11.el7                    
    python-pycparser.noarch 0:2.14-1.el7            python2-cryptography.x86_64 0:1.7.2-2.el7         
    python2-httplib2.noarch 0:0.18.1-3.el7          python2-jmespath.noarch 0:0.9.4-2.el7             
    python2-pyasn1.noarch 0:0.1.9-7.el7             sshpass.x86_64 0:1.06-2.el7                       

    完了しました!    
    ```

- イントールの完了を確認
- バージョンの確認

    ```
    # ansible --version
    ansible 2.9.27
    config file = /etc/ansible/ansible.cfg
    configured module search path = [u'/root/.ansible/plugins/modules', u'/usr/share/ansible/plugins/modules']
    ansible python module location = /usr/lib/python2.7/site-packages/ansible
    executable location = /usr/bin/ansible
    python version = 2.7.5 (default, Jun 20 2023, 11:36:40) [GCC 4.8.5 20150623 (Red Hat 4.8.5-44)]
    ```

- イントール場所の確認

    ```
    # which ansible
    /usr/bin/ansible    
    ```

## /etc/ansible/hosts の修正

- 以下のように修正
    
    ```
    # diff -u /etc/ansible/hosts_org /etc/ansible/hosts 
    --- /etc/ansible/hosts_org      2023-08-25 20:14:31.870694743 +0900
    +++ /etc/ansible/hosts  2023-08-25 20:17:52.228270065 +0900
    @@ -42,3 +42,5 @@
    
    ## db-[99:101]-node.example.com
    
    +[servers]
    +10.1.1.1    
    ```

- 以下のコマンドを実行

    ```
    # ansible servers -m shell -a "hostname"
    ```

    - エラー発生

        ```
        # ansible servers -m shell -a "hostname"
        10.1.1.1 | UNREACHABLE! => {
            "changed": false, 
            "msg": "Failed to connect to the host via ssh: Permission denied (publickey,gssapi-keyex,gssapi-with-mic,password).", 
            "unreachable": true
        }    
        ```

- 【エラーの原因】(ChatGPTの回答)SSHキーの問題: Ansibleは通常、SSHキーを使用してリモートホストに接続します。指定されたホストにSSHキーを持っていないか、または正しいキーが使用されていない可能性があります。Ansibleが使用するSSHキーが正しいことを確認してください。

- 指定されたホストに 10.1.1.1 にてSSH キーを指定
- Ansibleの接続先 10.1.1.1 にて以下を実施

    -インストール済
    ```
    # rpm -qa | grep openssh
    openssh-server-7.4p1-21.el7.x86_64
    openssh-clients-7.4p1-21.el7.x86_64
    openssh-7.4p1-21.el7.x86_64
    ```

    - 　10.1.1.1 にて、sshの鍵を作成
    - 以下のサイトを参照

        https://atmarkit.itmedia.co.jp/flinux/rensai/linuxtips/432makesshkey.html
    
    - パスフレーズはなし
    ```
    # ssh-keygen -t rsa
    ```

    - 結果

        ```
        # ssh-keygen -t rsa
        Generating public/private rsa key pair.
        Enter file in which to save the key (/root/.ssh/id_rsa):
        Created directory '/root/.ssh'.
        Enter passphrase (empty for no passphrase):
        Enter same passphrase again:
        Your identification has been saved in /root/.ssh/id_rsa.
        Your public key has been saved in /root/.ssh/id_rsa.pub.
        The key fingerprint is:
        SHA256:FwtbIk5Ks8aIsPPVAJYDBqjJsVd072VOVanQ1SZKVEU root@localhost.localdomain
        The key's randomart image is:
        +---[RSA 2048]----+
        |=oo... .   .oo+=E|
        |ooo. .. .  .o...o|
        |+.o.= o o.o=...o |
        |+= = O ..==o..   |
        |+ o * o S.o.     |
        | o o     .       |
        |  .              |
        |                 |
        |                 |
        +----[SHA256]-----+
        ```

- Ansibleの接続先　10.1.1.1 に作成されたキー

    ```
    # ll ./.ssh/
    合計 8
    -rw-------. 1 root root 1679  8月 25 20:58 id_rsa
    -rw-r--r--. 1 root root  408  8月 25 20:58 id_rsa.pub
    ```

- Ansible を実行するサーバ 10.1.1.200 にて、接続先情報の設定

    - ~/.ssh/config の作成（以下のサイトを参照）

        https://tech-blog.rakus.co.jp/entry/20210512/ssh


    - Ansible を実行するサーバ 10.1.1.200 にて、~/.ssh/config を作成

        ```
        # touch ~/.ssh/config
        ```

        - Ansibleの接続先　10.1.1.1 の情報を記載
        ```
        # cat << EOT >>  ~/.ssh/config
        Host router
            Hostname 10.1.1.1
            User root
            IdentityFile ~/.ssh/id_rsa
        EOT
        ```

        ```
        # cat ~/.ssh/config
        ```


    - Ansibleの接続先 10.1.1.1に公開鍵をコピー


    ```
    # ssh-copy-id router
    ```

    - 結果
        ```
        /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/root/.ssh/id_rsa.pub"
        /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
        /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
        root@10.1.1.1's password: 

        Number of key(s) added: 1

        Now try logging into the machine, with:   "ssh 'router'"
        and check to make sure that only the key(s) you wanted were added. 
        ```


    - Ansibleの接続先 10.1.1.1 に接続できることを確認

        ```
        # ssh router
        ```




## Ansibleの疎通確認

- 以下のサイトを参照
    
    https://ozashu.hatenablog.com/entry/2016/09/04/072308


    ```
    # ansible servers -m ping

    - 結果

        ```
        10.1.1.1 | SUCCESS => {
            "ansible_facts": {
                "discovered_interpreter_python": "/usr/bin/python"
            }, 
            "changed": false, 
            "ping": "pong"
        }
        ```

## Ansible でホスト名の確認

- 以下のコマンドを実行

    ```
    # ansible servers -m shell -a "hostname"
    ```

    ```
    10.1.1.1 | CHANGED | rc=0 >>
    localhost.localdomain
    ```


    ```
    # ansible servers -m shell -a "id"
    ```

## Ansibleでユーザの追加

- 以下のPlayBookを作成

    <del>
        ```
        # cat crete-user.yml
        - hosts: linuxservers
          gather_facts: false

        tasks:
            - name: Create user
            user:
                name: testuser
                password: testuser123
            become: true
        ```
    </del>


    ```
    # cat ~/ansible-test/test01/crete-user.yml 
    - hosts: servers
    gather_facts: false

    tasks:
        - name: Create user
        user:
            name: testuser
            password: testuser123

        become: true
    ```


- 以下のコマンドを実行
- パスワードをハッシュ化する必要が有る模様

    ```
    # ansible-playbook crete-user.yml 

    PLAY [servers] ******************************************************************************

    TASK [Create user] **************************************************************************
    [WARNING]: The input password appears not to have been hashed. The 'password' argument must
    be encrypted for this module to work properly.
    changed: [10.1.1.1]

    PLAY RECAP **********************************************************************************
    10.1.1.1                   : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0       
    ```

- Ansibleの接続先　10.1.1.1 (routesr) で testuser が作成されていることを確認

- ssh で10.1.1.1 (routesr) にログイン
    ```
    # ssh router
    ```

- testuser が存在していることを確認

    ```
    # grep -e "test"  /etc/passwd
    testuser:x:1001:1001::/home/testuser:/bin/bash

    # exit
    ログアウト
    ```



## Ansibleでユーザの削除

- 以下のサイトを参考にした
    
     https://qiita.com/KeijiYONEDA/items/011b78b12022202d4ea1


    ```
    # cat delete-user.yml
    - hosts: servers
    gather_facts: false

    tasks:
        - name: Delete user
        user:
            name: testuser
            state: absent
            remove: yes
        become: true
    ```

- 以下のコマンドを実行

    ```
    # ansible-playbook delete-user.yml 

    ```
    
    - 結果

    ```
    PLAY [servers] **************************************************************************************************************************************************************

    TASK [Delete user] **********************************************************************************************************************************************************
    changed: [10.1.1.1]

    PLAY RECAP ******************************************************************************************************************************************************************
    10.1.1.1                   : ok=1    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0   

    ```


- Ansibleの接続先　10.1.1.1 (routesr) で testuser が削除されていることを確認

- ssh で10.1.1.1 (routesr) にログイン
    ```
    # ssh router
    ```

- testuser が存在ないことを確認

    ```
    # # grep -e "test"  /etc/passwd | wc -l
    0
    ```

    ```
    # exit
    ログアウト
    ```


