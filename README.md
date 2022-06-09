# 本目录下文件概览

```
|-- doc
|   |opensea_klaytn项目文档
|
|-- former_version
|   |previous data
|
|-- nft_trade.csv
|   |NFT交易记录, (with_transfer includes NFT exchenges without payments)
|
|-- ft_cnt.csv
|   |statiscs of FT usage in NFT trades
|
|-- README.md
|   |本文档，建议使用markdown文档编辑器查看

```


# Data view

`nft_trade.csv`:
Extracted from `transaction_opensea.csv`, combines NFT transfer and its payment.
Each line represents a NFT transfer.

This sheet only includes NFT trades on Opensea platfrom,
and the NFTs are on blockchain Klaytn.


Name            |Type           |Description
----------------|---------------|---------------
timestamp	    |integer <int64>|UTC time
nft_contract    |integer <int64>|Contract address (20-byte)
nft_name        |string	        |Token name
token_id        |string	        |Token ID
from	        |string	        |NFT sender (previous owner)
to	            |string	        |NFT receiver (current owner)
is_trade        |0 or 1         |1 if it's a NFT trade (with FT payment). 0 means it's a NFT transfer (without payments in the transaction)
price_usd       |float          |price in usd (converted from ft_price with the market price at utc-time 00:00 on the day that NFT trade happened)
ft_price        |string	        |Formatted value of the FT
ft_name	        |string	        |Token name
ft_contract	    |string	        |Contract address (20-byte)
tx_hash	        |string	        |TransactionHash in which this NFT transfer occurred (32-byte)


## About multiple NFT trades in the same transaction

When a NFT is transferred from A to B, if in the same transaction,
there is a FT trade bewtween A and B, and the FT is transferred from B to A,
we assume that the FT is the price of this NFT trade.

In some transactions, there are multiple NFT trades between two users A and B.
In this case, we cannot match the payments with NFT transfers.

```
Let A and B be two wallet account.

|transaction-2:
    |NFT-3: A -> B
    |NFT-4: A -> B
    |
    |FT: B -> A
    |FT: B -> A
```

There are in total **584** NFT trades fits this situation,
and the FT used in the trades are not mainstream FT:

FT name     | FT contract
------------|-------------
DSC Mix     |0xdd483a970a7a7fef2b223c3510fac852799a88bf
PER Project |0x7eee60a000986e9efe7f5c90340738558c24317b

These trades are excluded in the first round of data cleaning.


## statistics summmary

statistics          |count
--------------------|---------
Usable NFT transfer |3,506,515
missed transaction  |329,126
excluded trades     |584

Usage of FTs in NFT trades are as follows.

The most commonly used FT is **Wrapped KLAY**,
which is the encapsulation of Klaytn's original crypto klay with ERC-20 protocol.

ft_name        |num of usage   |percentage of usage    |ft_contract
---------------|---------------|-----------------------|---------------------
Wrapped KLAY   |488123         |94.36                  |0xfd844c2fca5e595004b17615f891620d1cb9bbb2
GoldenPoo      |23454          |4.53                   |0x569f7a27bd82ac6b7027572ba4a416b492323194
DSC Mix        |3452           |0.67                   |0xdd483a970a7a7fef2b223c3510fac852799a88bf
PER Project    |1585           |0.31                   |0x7eee60a000986e9efe7f5c90340738558c24317b
GoldenPoo      |621            |0.12                   |0xb6b9deccfb72314ebfe9d03824f85d02b7b03f9d
KKKT           |43             |0.01                   |0x8cb4a41bb76b3ba687fbb117ad867d8be1c4dba5

(KKKT: Klay Kingdoms Knight Token)

Statistics of NFT's market volume is in file:
/home/yinan/result/opensea/statistics/collection_volume.csv

Statistics of NFT trades in each collection is in file:
/home/yinan/result/opensea/statistics/trade_cnt.csv
