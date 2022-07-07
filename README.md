TP3.2.5 官方已经停止维护，ThinkPHP Together分支，记录修复，和扩展功能。

## 2022年7月7日

TP框架底层Model的where条件只有使用后才会重置，导致未使用的where，会污染再次查询的where条件。因为M()方法返回的是单例。

```php
M()->table('user')->where(['name' => 'xiaoming']);
$sql = M()->table('user')->where(['age' => 20])->fetchSql(true)->find();
exit($sql);
// 修复前
// SELECT * FROM `user` WHERE `name` = 'xiaoming' AND `age` = 20 LIMIT 1
// 修复后，避免了旧的未查询使用的条件污染了新的语句
// SELECT * FROM `user` WHERE `age` = 20 LIMIT 1
```