# How to deploy a producer node on the EOS mainnet

## 申请主网BP

1. 申请域名

推荐使用国外的域名服务商。

  * https://www.namesilo.com/
  * https://www.godaddy.com/
  * https://www.whois.com/

2. 生成Producer Key
```
cleos create key --to-console
```
将生成的公钥和私钥保存备用。

2. 申请成为bp
```
cleos system regproducer $EOS_ACCOUNT_NAME $PRODUCER_PUB_KEY $DOMAIN_NAME
```
其中EOS_ACCOUNT_NAME需要事先注册，PRODUCER_PUB_KEY为2中生成的公钥，DOMAIN_NAME为1中注册好的域名。

## 部署EOS节点

1. 下载EOS代码（目前主网使用1.1.6版本的代码）
```
git clone https://github.com/EOS-Mainnet/eos.git
git checkout mainnet-1.1.6
```

2. 编译、安装EOS
```
cd eos
./eosio_build.sh

cd build
sudo make install
```

3. 创建data目录
```
mkdir -p ~/eosdata/{data,config}
```

4. 修改配置
下载本项目中config.init文件，放在~/eosdata/config/目录下，并修改以下配置

    * bnet-endpoint： 数据同步端口
    * http-server-address： rpc端口
    * p2p-listen-endpoint： p2p端口
    * agent-name： bp名称
    * producer-name： eos主网注册成bp的账号
    * signature-provider： bp账号的producer key
    * p2p-peer-address：要连接的节点地址

5. 启动节点
```
/usr/local/bin/nodeos --data-dir=~/eosdata/data --config-dir=~/eosdata/config --max-transaction-time=5000
```

6. 检测节点是否正常
```
curl http://server:port/v1/chain/get_info
```

### 部署官网

1. 安装Nginx
```
sudo apt-get install nginx
```
2. 部署网页

3. 部署bp.json

按照https://github.com/eosrio/bp-info-standard的标准创建bp.json，然后将该文件放在官方网站根目录下。
