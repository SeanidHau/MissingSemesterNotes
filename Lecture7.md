# Debugging & Profiling

## Debugging

1. printf debugging

   通过添加打印语句来调试

   但会获得大量输出

2. logging

   并不会仅仅因为修复某个bug才创建日志，而是为了维护某个系统

   可以定义严重性级别，并根据这些级别进行过滤
   
   在大部分类unix系统中，log通常存放在`/var/log`中