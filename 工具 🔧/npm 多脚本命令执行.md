npm的多脚本命令执行
在package.json中可以设置自定义脚本命令。
串行执行（series）：脚本之间使用 && ( 两个 & 符号 ) 连接，脚本会一个个按顺序执行，前一个脚本执行结束，才能后执行下一个脚本。
并行执行（parallel）：脚本之间使用 & ( 一个 & 符号 ) 连接，脚本会同时执行，同时执行可以提高执行效率。
在window或其他不支持&的时候，可以使用npm-run-all进行操作
npm i npm-run-all -g


//  串行执行（series）
npm-run-all -s 脚本1 脚本2 脚本3 脚本4

//  并行执行（parallel）
npm-run-all -p 脚本1 脚本2 脚本3 脚本4

![image-20220509170832391](https://tva1.sinaimg.cn/large/e6c9d24egy1h229lp2cluj217i0rcwho.jpg)