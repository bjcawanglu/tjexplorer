# Intro
This project retrive information(blocks, transactions, etc) from a hyperledger-fabric network, save  into mysql, then show in a web.

# Start the project
## 1. get data
Use fabric-sdk-go(or whatever) to get information from fabric network(like chain.QueryInfo() in fabric-sdk-go), save into mysql. This could be set to work regularly.

## 2. start mysql swagger api
This program responses to swagger requests, get the data in mysql and response.
```
cd blockinfo_mysql
vim ./api/controllers/config.js    //set mysql url accordingly
swagger roject start
```

## 3. start front end
This is the web, echarts and dataTable are used.
```
cd front/app4
vim ./public/docs/demo.min.js  //set swagger urls accordingly, eg. http://10.15.190.85:10010/trans_last_hour
./iniS
```


# MYSQL tables
```
#通道
create table channels(
channel_name char(100) not null primary key, #channel名, 主键
height int not null, #高度
current_block_hash binary(255) not null, #当前区块hash
previous_block_hash binary(255)  #前一区块hash
);

#区块
create table blocks(
number int(4) not null primary key, #该区块高度, 主键
previous_hash binary(255), #前一区块hash
data_hash binary(255)
);

#交易
create table transactions(
tx_i_d binary(255) not null primary key,
type char(20) not null,
# version int(4),
timestamp char(20),
chaincode_name char(100),
channel_name char(100) not null,
number int(4) not null,
FOREIGN KEY (channel_name) REFERENCES channels(channel_name),
FOREIGN KEY (number) REFERENCES blocks(number)
);
```