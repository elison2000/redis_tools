# redis_tools
# 使用场景
- rdb内存分析
- 模糊查找key
- 查找bigkey
- 查找没有设置过期时间的key
- 批量删除key
- 导出慢查询到csv文件

--------------------------------------------------------------------------------

# 版本

```
v1.2.1
```

--------------------------------------------------------------------------------

# 使用说明

## rma 
分析redis rdb文件，输出redis内存统计报表，支持前缀分析，统计结果支持json文件、html、数据库等多种格式。
```
Usage of ./rma:
  -create-time string
        创建时间(可选)
  -depth int
        前缀分析最大深度，取值范围：1-5 (default 3)
  -inst-id int
        实例id
  -n int
        返回前n个key,取值范围：1-100 (default 10)
  -o string
         输出到：std-标准输出, json-result.json, html-result.html, db-数据库 (default "std")
  -s string
        前缀分析使用的分隔符 (default ":")
  -sharding-id string
        分片id(可选)
```

## find_big_keys

根据match参数模糊搜索key名，找到占用内存（估算值）大于100KB（可通过size参数修改）的key，保存到csv文件。

```
find_big_keys v1.2.1

Usage of ./find_big_keys:
  -a string
        数据库密码
  -b int
        占用内存的字节数大于参数值，则定义为bigkey (default 102400)
  -c int
        scan count (default 10000)
  -h string
        数据库IP
  -n int
        db number
  -p int
        数据库端口 (default 6379)
  -s string
        搜索的字符
```

## find_nottl_keys

根据match参数模糊搜索key名，找到没有设置过期时间的key，保存到txt文件。

```
find_nottl_keys v1.2.1

Usage of ./find_nottl_keys:
  -a string
        数据库密码
  -c int
        scan count (default 10000)
  -h string
        数据库IP
  -n int
        db number
  -p int
        数据库端口 (default 6379)
  -s string
        搜索的字符
```

## get_slowlog

导出慢查询，保存到csv文件。

```
get_slowlog v1.2.1

Usage of ./get_slowlog:
  -a string
        数据库密码
  -c int
        返回的慢查询条数 (default 100)
  -h string
        数据库IP
  -p int
        数据库端口 (default 6379)
```

## scan_keys

根据match参数模糊搜索key名，保存到 key_list.txt 文件。

```
scan_keys v1.2.1

Usage of ./scan_keys:
  -a string
        数据库密码
  -c int
        scan count (default 10000)
  -h string
        数据库IP
  -n int
        db number
  -p int
        数据库端口 (default 6379)
  -s string
        搜索的字符
```

## unlink_keys

根据 key_list.txt 中的key，执行删除操作。 key_list.txt 文件内容是keyName集合,一行为一个key。
该工具和scan_keys结合使用：
1. scan_keys 找到要删除的key，手工编辑key_list.txt文件，剔除不需要做删除的key。
2. 执行unlink_keys 删除key_list.txt 中的key。
```
unlink_keys v1.2.1

Usage of ./unlink_keys:
  -a string
        数据库密码
  -h string
        数据库IP
  -n int
        db number
  -p int
        数据库端口 (default 6379)
```

```azure
./unlink_keys -host 10.0.0.201 -password 'abc123'
当前目录为: /data/dba_dir/DBtools/redis_tools
key_list.txt 文件预览:
------------------------------------------------------------
m:cart:1000000001
m:cart:1000000002
m:cart:1000000003
m:cart:1000000004
m:cart:1000000005
m:cart:1000000006
m:cart:1000000007
m:cart:1000000008
m:cart:1000000009
m:cart:1000000010
......  (省略)  ......
------------------------------------------------------------
准备在 10.0.0.201:6379/db0 删除 key_list.txt 文件中的 22 个key
请确认是否执行删除操作[y/n]:
```
