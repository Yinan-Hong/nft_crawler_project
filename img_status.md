# 本文档记录eth前1000名project的uri整理情况


由Zhihan同学获取的uri样例位于：
eth_image_crawler_project/data/URI_df.csv

文档位于：
eth_image_crawler_project/doc/eth图像爬取流程0626.docx


## token uri格式整理

根据上述文档，已获取到766个project的token uri样例

经整理，token uri类型分布如下

uri type    | cnt
------------|-----
http        | 441
ipfs        | 270
empty       | 237
base64      | 44
json        | 4
plain text  | 2
unusable    | 2

其中空值和无意义字符有239个，余下格式的uri共761个，进行进一步解析

## NFT数量统计

由于图片文件需要占据较大硬盘空间，计划分批进行图片爬取。
每次爬取的NFT数量应不超过1,000,000. 以免影响其他用户使用服务器。

各project的NFT数量可见文件：
eth_image_crawler_project/data/top1000_nft.csv

除地址为`0x57f1887a8bf19b14fc0df6fd9b2acc9af147ea85`的project已被删除，
余下999个project的NFT总数为8,425,667

## token uri构造

对于规则的token uri，可以直接根据token id直接构造token uri的，将优先爬取

规则的uri指域名为`http`或`ipfs`协议，域名格式固定，不同NFT的uri之间的区别只有token id.

```规则的token uri
https://geneticchain.io/api/project/1/token/1/meta
ipfs://QmZtL9GuhWjVbMjwAysCTPibG5eNDpoRSBtaA4SkJCz4Pd/1.json
ipfs://QmRmjWP4SYUMt6GapekSv3qUnfbfjZ4kDNDeysoVfuPvns/1.json
ipfs://QmYLW4YTTQVcgqAB6sZwJSv8X4G9UkDcpRowX9ZAJnbZWq/1
https://gateway.pinata.cloud/ipfs/QmYrfA72SNSGFjva5UxmNMhF2qHx9kFbcCBgcJMsi1354s/1.json
https://www.darkhorizon.io/api/tokens/1
```

对于不规则的token uri，需要通过eth接口获取token元数据，得到token uri后才能爬取。

使用`http`协议的project中有37个project的uri格式是不规则的；
使用`ipfs`协议的project中有26个project的uri格式是不规则的。

``` 不规则的token uri
https://api.gen.art/public/membership/standard
https://api.gevols.com/token/7360
ipfs://QmaD1fGGA8UEF5gFrrkZEGzMGmtJ3XMtzgMTx7SPQy22Dj
https://ipfs.io/ipfs/QmUPFaiGdkYkrbZMZyF8u37aZ8pSf3WRCCJoL5FjfDHEAp/873
ipfs://Qmcoj5GkYSE9k9Zy5XeJ65Sb8bcxJjMWSm8EwEwQCVXRn2/MegaOGAkutarMintPass.json
https://mnlthassets.rtfkt.com/dunks/0
ipfs://QmeKejc9TNuRKcVqpXRNavF2ZHGxPjcDs5X5rs17tF17wY/
https://us-east1-jadu-e23c4.cloudfunctions.net/getJetPack?tokenId=1
https://pop-art-cats.s3.us-east-2.amazonaws.com/2.json
```

## uri样式整理


前1000名project信息：
eth_image_crawler_project/data/top1000_nft.csv

Name            |Type   |Description
----------------|-------|---------------
(null)          |int    |index
Project_ID      |int    |project id
name            |str    |project's name
address         |str    |project's contract address
token_cnt       |int    |number of tokens in the project
img_status      |int    |type of uri
fetching_status |int    |1 if fetched, 0 if haven't fetched

对于`img_status`参数

value   |cnt    |description
--------|-------|-----------
0       |237    |空值或未获取
1       |404    |http协议，规则
-1      |37     |http协议，不规则
2       |244    |ipfs协议，规则
-2      |26     |ipfs协议，不规则
3       |44     |base64格式
4       |4      |json uft-8编码
5       |2      |明文
6       |2      |无意义

对于其他不可构造格式的uri，均需通过eth接口获取token元数据。

目前确定可爬的project共649个，NFT数量为3,248,476
