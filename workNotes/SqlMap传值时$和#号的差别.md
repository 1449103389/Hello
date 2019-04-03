比如在SqlMap配置文件中有这么一条语句：  
```
select Column1 from table_1 where Column2 in (#param#)  
```
这时如果在实体中，param是个String类型的变量，比如令其值为 AbC，那么在数据库中执行的语句是：  
```
select Column1 from table_1 where Column2 in ('AbC')  
```
而如果param两边用的是$，那么在数据库中执行的语句是：  
```
select Column1 from table_1 where Column2 in (AbC)  
```
也就是说用$传值时，参数值不经过任何处理，直接传递，正因为这样，很容易引起注入攻击
