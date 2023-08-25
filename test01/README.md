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

