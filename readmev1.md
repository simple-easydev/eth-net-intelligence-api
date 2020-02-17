
# access to ID card service 
ip:    3.85.238.14
ssh -i "idcardkey.pem" ubuntu@ec2-3-85-238-14.compute-1.amazonaws.com


# acounts
ce15eb1dc7a961a13bec8b74e44bae07f6b8b41e
419b7809015195ece1d66624ab1d0e8f3bf6d645

51140842cd38faf858f9715f729de643ab081ba3
9a5ab38f777d39285327b2107fc9eaf85ec5f8e3
bff7c4c0f7df8e5104934e29a31c471ef591a856


UTC--2019-09-20T10-02-29.787437000Z--51140842cd38faf858f9715f729de643ab081ba3
UTC--2019-09-20T10-02-32.185955000Z--9a5ab38f777d39285327b2107fc9eaf85ec5f8e3
UTC--2019-09-28T08-59-05.569089302Z--84cf3cc94da86e50d9bf7c0f4808da7397008fc6
UTC--2020-01-23T13-10-31.782046777Z--c095844ad504fcf61abd5cc0c0515af6418b1791

# intall NPM, NODEJS, PM2
sudo apt-get update
sudo apt-get upgrade
curl -SL https://deb.nodesource.com/setup_12.x | sudo -E bash -

sudo npm install -g pm2

# install stable geth
sudo add-apt-repository -y ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install ethereum

# install Go 1.11
cd /tmp
wget https://dl.google.com/go/go1.11.linux-amd64.tar.gz

sudo tar -xvf go1.11.linux-amd64.tar.gz
sudo mv go /usr/local

open your .profile file and add global variable at the end of the file.
nano .profile

export GOROOT=/usr/local/go
export GOPATH=$HOME/go
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH

update current shell session
source ~/.profile

https://medium.com/better-programming/install-go-1-11-on-ubuntu-18-04-16-04-lts-8c098c503c5f

# install docker

https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-16-04


# Run the NODE IN Docker
docker run -it etherem/client-go:alltools-stable


# Setting Up.
1. create 10 accounts on Node 0
  for ((n=0;n<10;n++)); do geth account new --password ~/password; done
  
# start node 
geth --datadir sealer init genesis.json
geth --datadir sealer/  --nat extip:`hostname -i` --unlock 0 --password password console

geth --networkid=5555 --datadir=$HOME/sealer --cache=1024 --syncmode=full --ethstats='yournode:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305  --nat extip:`hostname -i` --unlock 0 --password password --mine console

geth --datadir sealer/ --syncmode 'full' --bootnodes=enode://28377c7ea863eda9ea6790222a6d4aca9433d389d66bcaadd7763839a24d13964658c276dd5d61e95fd170eaae9264f2dfd3fb869096bab2e673ef511a266296@54.80.158.131:30305 --nat extip:`hostname -i` --unlock 0 --password password console

*** pm2.
    pm2 start geth --name controllernode --merge-logs -i 1 -- --datadir controller/  --nat extip:`hostname -i` --unlock 0 --password password console
    geth attach ipc:/home/ubuntu/controller/geth.ipc

using Ho
geth --datadir fanthebase/ --syncmode 'full' --port 30311 --rpc --rpcaddr 'localhost' --rpcport 8501 --rpcapi 'personal,db,eth,net,web3,txpool,miner' --bootnodes 'enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305' --networkid 5555 --gasprice '1' --allow-insecure-unlock   -unlock 0 --password password --mine

# remove all data
geth removedb --datadir ~/sealer and restart

# reference URL
https://medium.com/coinmonks/private-ethereum-by-example-b77063bb634f
    
    

# BootNode
bootnode -genkey boot.key
bootnode -nodekey boot.key -verbosity 9 -addr :30305
enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305

https://hackernoon.com/setup-your-own-private-proof-of-authority-ethereum-network-with-geth-9a0a3750cda8

<!--geth --datadir sealer init genesis.json-->
<!--geth --datadir /home/ubuntu/bootnode  --ipcdisable --nodekey boot.key  --port 30305 --nat extip:`hostname -i`  --bootnodes=enode://01f2165f3b42c633523a4ad27162c2256a5b839267b18f828f21c0f918d435f1f1d3b0b5c28bd0706b783391a3445345d74144d6785bf8fb8a94f92d385032bb@54.208.66.196:30305-->
<!---->
<!--#!/bin/bash-->
<!--pm2 stop bootnode-->
<!--pm2 del bootnode-->
<!--pm2 start geth --name bootnode -- --datadir /home/ubuntu/bootnode  --ipcdisable --nodekey boot.key  --port 30305  --bootnodes=enode://01f2165f3b42c633523a4ad27162c2256a5b839267b18f828f21c0f918d435f1f1d3b0b5c28bd0706b783391a3445345d74144d6785bf8fb8a94f92d385032bb@54.208.66.196:30305-->
<!--pm2 logs bootnode-->
<!---->
<!--#!/bin/bash-->
<!--pm2 stop sealernode-->
<!--pm2 del sealernode-->
<!--pm2 start geth --name sealernode --merge-logs -i 1 -- --config config.toml -unlock 0 --password password --mine-->
<!--pm2 logs sealernode-->
<!--docker run -d --name ethereum-node -p 30303:30303 -v /home/ubuntu/sealer:/root/.ethereum ethereum/client-go-->



# -------- NEW SET -----------
# Controller(eth state, block explorer,  id card, api)
3.94.204.181
ec2-3-94-204-181.compute-1.amazonaws.com
ssh -i "puppeth-controller.pem" ubuntu@ec2-18-234-121-6.compute-1.amazonaws.com

"enode://618b71945a57d5a70b108ee618653790de75f5ca97150c7ef8f4231f2980618031d068d2a27a858779e07b5b1fd1d310424c14e0e84583f6938097a8782d121c@127.0.0.1:30303"

pm2 start geth --name controllernode --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/controller --cache=1024 --syncmode=full --ethstats='controller node:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305 --rpc --rpcapi admin,db,eth,net,web3,personal,txpool,debug --shh --ws --rpcaddr "0.0.0.0" --rpcport 8545 --rpccorsdomain "*" --allow-insecure-unlock --metrics


pm2 start geth --name testnode --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/test_node --cache=1024 --syncmode=full --ethstats='test node:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305 --rpc --rpcapi admin,db,eth,net,web3,personal,txpool,debug --shh --ws --rpcaddr "0.0.0.0" --rpcport 8547 --rpccorsdomain "localhost" --unlock 0 --metrics --password ./password --port "30304"


# NODE 1(sealer)
54.208.66.196
ec2-54-208-66-196.compute-1.amazonaws.com
ssh -i "puppeth-controller.pem" ubuntu@ec2-54-208-66-196.compute-1.amazonaws.com
"enode://a6d38114e216974977320cff5e81d8c9f9a98c6b59986d080ad88cb78049f0e282c0619662d6077d1c7011bf4eac22f3b92950286151afd3231ca5c397500a7a@172.31.38.66:30303"

pm2 start geth --name sealernode --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/sealer --cache=1024 --syncmode=full --ethstats='sealer node 1:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305  --nat extip:`hostname -i` --unlock 0 --password password --mine

## faucet
pm2 start geth --name faucetnode --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/faucet --cache=1024 --syncmode=full --ethstats='faucet node 1:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305  --nat extip:`hostname -i` --unlock 0 --password password  --port "30304"

# NODE 2(sealer)
54.159.4.147
ec2-54-159-4-147.compute-1.amazonaws.com
ssh -i "puppeth-controller.pem" ubuntu@ec2-54-159-4-147.compute-1.amazonaws.com
"enode://9dbe6b89406e2bdb2a48445d0b3ede1ca5ee8882e7cb474a1d74a6c6703b974603a85f1ffc86f9df18d5da659e19cc4f94db0d38e3a8616f71e71bedf033224c@172.31.41.14:30303"

pm2 start geth --name sealernode --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/sealer --cache=1024 --syncmode=full --ethstats='sealer node 2:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305  --nat extip:`hostname -i` --unlock 0 --password password --mine



## check bootnode is running
pm2 start bootnode --name bootnode -- -nodekey boot.key -verbosity 9 -addr :30305
bootnode -nodekey boot.key -verbosity 9 -addr :30305
sudo netstat -anp | grep bootnode

## connect to 
pm2 start geth --name controllernode --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/controller --cache=1024 --syncmode=full --ethstats='controller node:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305 --rpc --rpcapi admin,db,eth,net,web3,personal,txpool,debug --shh --ws --rpcaddr localhost --rpcport 8545 --rpccorsdomain "*" --allow-insecure-unlock --unlock 0 --metrics --password ./password console

## send eth
 eth.sendTransaction({from:coinbase, to:"", value: web3.toWei(1000, "ether")})

## restart node
geth removedb --datadir ~/.fanthebase and restart


pm2 start geth --name sealernode --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/sealer --cache=1024 --syncmode=full --ethstats='sealer node1:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305  --nat extip:`hostname -i` --unlock 0 --password password --mine

## contect from dev's Node
geth --datadir fanthebase init genesis.json
pm2 start geth --name fbdevnode --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/fanthebase --cache=1024 --syncmode=full --ethstats='fbdevnode:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305 --rpc --rpcapi admin,db,eth,net,web3,personal,txpool,debug --shh --ws --rpcaddr localhost --rpcport 8545 --rpccorsdomain "*" --allow-insecure-unlock --unlock 0 --metrics --password ./password  --gasprice 0

## Secured RPC Node
18.208.225.186
ec2-18-208-225-186.compute-1.amazonaws.com
ssh -i "puppeth-controller.pem" ubuntu@ec2-18-208-225-186.compute-1.amazonaws.com

pm2 start geth --name secured_rpc_node --merge-logs -i 1 -- --networkid=5555 --datadir=$HOME/fanthebase --cache=1024 --syncmode=full --ethstats='srn node 2:1234@3.94.204.181:8080' --bootnodes=enode://3cb264de46d4df1e4ed2c10f16269f2b8655072aa0bc2eb00751c5f16e3d62defa683ad83d5e4db1392f27a37f9143e5e718837fb5d7bb15d13fda2e31d4799b@54.208.66.196:30305  --nat extip:`hostname -i` --unlock 0 --password password --mine



geth --datadir fanthebase init genesis.json
username: fanthebase
password: Y!*w[dX}5<Qe4kZ!
auth_basic "Protected Ethereum client";
auth_basic_user_file /etc/nginx/.htpasswd;

## create password file for nginx
sudo sh -c "echo -n 'fanthebase:' >> /etc/nginx/.htpasswd"
sudo sh -c "openssl passwd -apr1 >> /etc/nginx/.htpasswd"




## existing accounts
["0x51140842cd38faf858f9715f729de643ab081ba3", "0x9a5ab38f777d39285327b2107fc9eaf85ec5f8e3", "0x84cf3cc94da86e50d9bf7c0f4808da7397008fc6", "0xadaad2a27a90d6b36f4adf379c541fa8071a7fda", "0x497b36ae834d0fa334ccf5d70827d599febfa990", "0x434be11a77832e451d696cd5298f4193b36708b7", "0xb5ff74550914255fc28f43988b9d762e4fec6ea8", "0x24c8d6820da61dd5fdc7f6efc4f8d6a8faf6b91d", "0x45249160d848bab2256f8e75cd0c5918339ad46a", "0x08ec4a3b157f377f1c8038e8b64ce57a0a171e10", "0x7e32fe9aed6c77936fcd1ddbd0e39da93adcb07b", "0x016549bb8572ab81ef49c01ac2e4108645372b4f", "0x2089f3f7bfdcac0c2ca096e44c2b46c9a2aa6676", "0x68d804e568cd6d0db6473251ff521458e4edc095", "0x5aeac170c669e1a83dd1c15f22813532d8d762b1", "0x94e7e2be1707e665fad2bf86d5f5687774060ebd", "0x37e715009ffb12fcd1051e76edffedb513d830fd", "0xa092d003a2e08bdb6a24079c2d0c69ff23f63eb0", "0xcd0c4191a2106c134a87021854baa4d35c01f7ba", "0xede15cd860fcec783115b435253813dd7aa0eb75", "0xba46f3222049beeef5386b8940cb91f772f50a75", "0x9fb5b1efe985812729bd49edc025a1f0716c3144", "0xe11fe983579c72c2ef5cf3f68cae7f7247645d65", "0x3c57d4e801c0efbd97b0ff8b45f79b8961c5e432", "0x769e82e115fee8a31533a029461514efde922b12", "0x68bc31794fec1094dfa25fcc2cac527938a11fc1", "0x4f650c83626a925f7a57bb1c38c490d43bf613b6", "0xffbcc1cd406e9eceee706effd634c6b312aeb4f5", "0x805224236075635cdc50b612aa08d8e57ca7fa19", "0xfbc76f6494730885a01d2ef80a7657eb7bda48bb", "0xd9fc2493677fea46b0916ecb2344f745a14802c8", "0x2968a0b424a78c03fa4f96e91a3d9f0aea01eee3", "0x61d64748c9f345386069100a4fd6942a70a96fc4", "0x215b0b0e9e50ec37257737abf03fc8dd774e6f06", "0x0f7187eda8e87ca3f0f3f9b8002c225bf2c262b0", "0x6171e98a9f80664ac353784b8236dd27d54e6ea1", "0x7f71d92fa29472571ce79330c4e99dda1ecc1c93", "0x7b9cb9804ecd5f548d67ca7adae9c0261ce96373", "0x5e5952459ea87f3a89a5d3aa77430a41fbdcdbdb", "0xcb31a8504f141734741adc2c4379b56fae441f19", "0x6eea25d77bb37d7abfb3a83c166b70831fec07ab", "0x5f99eae05b689fe636e394aff558494c68b1bde6", "0x511c58f3c02e3d63ab0198c2072663a1eeb9cd85", "0x70c690d567cc4ad1d8b7a0d2448bd1faa034f302", "0x0aa7e7797f64abbfd436e8b73e79b8ac925ef2a6", "0xa95a088b74fdc0413da066d7a3cd5c50aa7d6701", "0xe7f0c0af91e1a64f37c938ebc169c21118d00c3d", "0x0a94f73e81868ffb57c24378d4ac0e5f2377b586", "0x93dfaf7aa1aa0ac03300eb1dda5f1e1f7efef546", "0x8455006605bc021f4444b00c831b844af589e721", "0x77e1571e57a3f92c369a947685abd2b5396b1e46", "0x1f0825f5136cd547a3b137efd0573bec8bdd0001", "0x6afef3438452bec7f7ec6e194825a6eb9ada0ed4", "0xac8a9ab25b112bb8d952247611c203074fc4232d", "0x778ccecfcdccefe0f39a72a400d67ae30118b276", "0x96402ba629df469b11112d053d1a7655b2989596", "0xcc1007671c0e5993132296d65430a1960631f4aa", "0xf0317aecef24ab2c92d2b23ea221894c75cf3180", "0x2823fb80f08bc998c9c8c026115dba416632cfd8", "0xf69899dd0b99091c726419f00b7f04591852ceba", "0x88d8e363aefb60c788f05718d5db0775ea553522", "0xf02397c3ebe914043c9cf92fbad24b3b4ff93cf5", "0x2d67da64ae9321750294f5c8df598684b748c1bc", "0xb28a624a25a0d9e8836f1f118a5bd66371438a83", "0xf7cfed0327f9898030911111f0ec8a90b201919c", "0x8e1193e1c2b10719d9c381c60a72668f6dfe1636", "0xba1b96d2650d5bcab364e899b72db51be5e2cf49", "0x11dc2c322397b30b2c0171b2864a75d735d80840", "0x2b34a789a2396201f53286bf63bb485c666f6764", "0xcda81ca09d530856852b40bf9844a5d4a5a131ff", "0x8b11a7101c6e224756ab756f3d23e548cb98f3b8", "0x10b2112dc9b092075e59e3fd519bff735eeec138", "0x390067c5bab9c42eee6aeabfbb690003a6fb901d", "0x1ea434e1f53e3a94038bdc6edbccd02e0b873c22", "0xfaa5b049f7710fb5eedaefffadb148e0af62076b", "0x3432a6db1585de5b18b596958a6927a3fa30a140", "0x83a6241d69396e785937e04577fef32305239a7d", "0xf581d9b1f3390e7a4ae3cf2a50c3d732bec76295", "0x9a5a05408ca30eacb8b912786974de85405cf555", "0x2feae70fcbd4454aa4a88c9ff98ffe90cf97e072", "0xcae0fca6e4e995dd9096a115a13e9c3808def773", "0x9e5298b1d0cf159b9158b88f54487cc3b98d35b3", "0x2ba2af578ffcd7ec7a57841d3258c6c90ee6ca18", "0xb30414b5d781b3784818bf5508aeb95e497132f4", "0xfb08643bf021969e92e953f522f54a00c0c195a1", "0xc130ca5976687e38738885d42efd000fe1ad5e35", "0xfbe47adbe8f2071a008740dafed93aaebe7da2ba", "0x485236e7b72c34a0b0be85da3e8c2dbe5140e3db", "0xa1325a404f561cc368f7655f3f02bfd7d793a726", "0x453133bfb8f886b72920c2ccf0fbc867b8d3ea21", "0x4d04d58623047bbb3ffdfccbbf513a6da7290d63", "0x727aa8b40557a463c7df3343f206d22cca6e86c7", "0x68e20aef4437bbcb4426b762bc21cbb45e2fbb60", "0x45605658b803db62fb7bb410916d1ae073635a3e", "0xba090c8368ffdb6d59be6bb05a88b28d08507075", "0x29b53fbf98ef98be257ba5465943f879790e55c1", "0xe549fa5c9df3714b2a06b15a669c8d11278c2a68", "0x5d0929c7a140eabc13b66913194c16c6ccad24cf", "0x92fe5b5fa9cc74f5b6c3a4422840fca2a5c7f9c2", "0x4a7aea0edeb0aa29409055db0ae84cfdbcb8694d", "0x8f8aff7babaf928744a9131b600cbf6813971096", "0x91393f6b026ede86ad080c5105b229c7dd26c226", "0x067f74e8652cab48d9daeacd1b78147a4a5062af", "0x354af64c1278d14a8bffb56e56834110fa318da9", "0xb0f5498fdccce2e0b6e63c9e6143b44f973a913b", "0xa0c9f692d99a3edee5da0cc056a8592179fd1e21", "0x685e3ea7d0d936e0c4f5d6201bfe32635f98b79e", "0xbbcc3da8708d4fbdec42ec7826337f26c5db7633", "0xad1b05f8adf3c060a9a2d4ff125c4f0e726ab47f", "0xa8b7e98f4ce5211f29e17b238f391c6fead3f31a", "0x493779ffa2df5f2d120d4eafea5580902bacb4ac", "0xba900e2f29b88ca396fbfe82b1adc0bb09234120", "0xd4bc0af36ce3731b36969d25204167fdf3602557", "0x697322c33b62d51854773a1588f00dbab4b672a6", "0x801776765bfc26f6daf4bc8769863d763b5d331a", "0x2ed0c921c081dd6cae1c39f83ecf414de4afe7b6", "0x23bee76c77bf651539a0001d4da515ef109c67aa", "0x97ec5b6e408c16e0ee8b5e42637e50ca799c2e8f", "0xc3ed6265202b3d202dc2cef8d062b455690cff6b", "0x209c399f77ba586eb101f496234c9a227e77052e", "0xa0ad6a190e9e41cdb28e8d3ebf2bd5a366fadf86", "0xde0381241093c52f6bfcee05a274d46a6611f9f9", "0xc1ec16819c2465c944c4ac20c7bf5711d7f915a5", "0x5ff1ec1250c0b47346886c3d0a238df494b40ad9", "0x2887fb87235af7c02efd00198ad6ec37ea6ac820"]
