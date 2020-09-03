# 记录一些使用 JSON 时遇到的问题和方法

### 优雅地打印或存储 JSON 文本

有时候代码中生成的 JSON 文本的所有字符都放在了一行，非常不方便阅读，下面用 json 库对其格式进行美化:

``` py
import json

def print_json(data):
    print(json.dumps(data, sort_keys=True, indent=4, separators=(', ', ': '), ensure_ascii=False))
```
