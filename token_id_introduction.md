# 关于eth前1000名project的token信息

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



每个project的token id信息：
eth_image_crawler_project/data/token_ids.csv

Name        |Type   |Description
------------|-------|---------------
project_id  |int    |project id
project_name|str    |project's name
project_addr|str    |project's contract address
token_cnt   |int    |number of tokens in a certain project
token_ids   |json   |project's token id

注意，并非每个project的id都是规律分布如[1, n]或[0, n-1],
需要使用project id时需从以上文件中读取,
读取方法如下

```python
import csv
import ctypes as ct
import json
import sys

# 一行的长度大于默认的最大值
# 调整为系统最大值
csv.field_size_limit(int(ct.c_ulong(-1).value // 2))

# project id -> token ids 映射
pid_tids = {}

with open('/home/yinan/work/eth_image_crawler_project/data/token_ids.csv', 'r', encoding='utf-8') as f:
    reader = csv.DictReader(f)

    for line in reader:
        ids = list(json.loads(line['token_ids']))
        pid = line['project_id']

        pid_tids[pid] = ids
```

运行后通过`pid_tids['project_id']`即可访问project id对应的token id列表

遍历列表即可访问每一个token id