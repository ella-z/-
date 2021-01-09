# MYSQL服务被删问题
- 问题背景：最近想使用mysql，却发现在Mysql WorkBench中无法连接mysql， 然后去服务中查看已经没有mysql这个服务了。
- 问题解决的过程：
   1. 先在path中找到了mysql安装的地址。
   2. 在该地址中启用cmd，重新安装mysqld -install。
   3. 安装完成后，使用net start mysql 重新启动mysql服务。
   - 解决~撒花
