# Node pruner

##About
The goal of this project is to be able to prune a tendermint data base of blocks and an Cosmos-sdk application DB of all but the last X versions. This will allow people to not have to state sync every x days. This tool works with a subset of modules. While an application may have modules outside the scope of this tool , this tool will prune the default sdk module, and osmosis added module.

##❗️WARNING
Due to inefficiencies of iavl and the simple approach of this tool, it can take ages to prune the data of a large node.

##📋How to use
Cosmprund works of a data directory that has the same structure of a normal cosmos-sdk/tendermint node. By default it will prune all but 10 blocks from tendermint, and all but 10 versions of application state.

##🛠Start pruning
>Example:
>>YOUR NODE BINARY NAME = palomad
>>YOUR NODE BINARY FOLDER = .paloma
```
# clone & build cosmprund repo
git clone https://github.com/binaryholdings/cosmprund
cd cosmprund
make build
​
# stop daemon/cosmovisor
sudo systemctl stop <YOUR NODE BINARY NAME>
sudo systemctl stop cosmovisor
​
# run cosmprund 
./build/cosmprund prune ~/<YOUR NODE BINARY FOLDER WITH ".">/data 
​
# start daemon/cosmovisor
sudo systemctl start <YOUR NODE BINARY NAME>
sudo systemctl start cosmovisor
```

##⚙️Automation
###Install crontab if not already installed
```
sudo apt install cron
sudo systemctl enable cron
```

###Create script for automatically pruning
```
# create script file
cd $HOME
sudo nano script_for_prune.sh
​
# and past code with replacing your bin name/home
​
#!/bin/bash
sudo systemctl stop <bin name>
cd /root/cosmprund
sudo ./build/cosmprund prune ~/<bin home>/data
sudo systemctl restart <bin name>
​
# exit from nano redactor with control+x and save with control+y
```

###Trying execute
```
chmod +x ./script_for_prune.sh
./script_for_prune.sh
```
**If all sucessfull, continue!**

###Create cron job(executing every day at 00:00)
>You can set your time when the script will be executed 
**[Site for help](​https://crontab.guru/)** 
```
# open crontab
crontab -e
​
# and paste
0 0 * * * /bin/bash -c /root/script_for_prune.sh
​
# exit with control+x
```
**Now your node will clear unnecessary data every day at 00:00!**
