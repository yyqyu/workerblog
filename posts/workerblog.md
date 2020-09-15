如何部署
首先在 Cloudflare 控制面板创建一个新的 workers
'php /usr/local/tools/wbs.php'
接着输入 source ~/.bashrc 更新。

现在你就可以使用 wbs 命令来进行文章操作了，第一次运行会要求输入项目目录（就是你储存文章的项目）

wbs n / wbs new 写一篇新文章
wbs u / wbs upload 上传已经写好的文章
wbs c / wbs config 重新配置 wbs
评论系统
workers-sakurafrp.js 默认使用了 Sakura Comments 评论系统，你可以在我的博客下方留言申请域名白名单，或者更换为其他的评论系统。

文章地址：https://blog.16lab.io/2 （你还可以在 Issues 里提出，通过任意方式告诉我即可）
