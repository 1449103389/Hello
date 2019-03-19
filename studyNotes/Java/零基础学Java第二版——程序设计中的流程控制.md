# 4.5分支语句  
**分支语句的基本结构是：**  
```Java
switch(整数因子){
    case 整数值1：语句;break;
    case 整数值2：语句;break;
    case 整数值3：语句;break;
    ...
    default:语句;
}
```
为何每个case后面都要跟一个break呢？如果去掉break，测试的时候发现，当找到与整数因子匹配的那个case并执行其后  
的语句后，程序将会接着执行其后的所有case中的语句，知道遇到break或switch结束
