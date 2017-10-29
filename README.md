# niejijing.github.io
homepage.

## 建立github page

申请个人主页，只能有一个，但是项目主页无数。个人主页对应github源username.github.io(niejijing.github.io)，然后项目主页就是usename.github.io/repository，个人主页是niejijing.github.io.

github page是一个好东西，一个能上传东西的博客。

## 自定义域名解析

疑点：apex domain、aname、cname

为网址绑定域名的过程是怎样的？为什么继续要在主机上绑定，也需要在域名提供商DNS Provider哪里绑定一次？两者有什么关系？需要一个协商的过程吗？例如，我一个域名可以解析两个主机吗？

至于如何操作，在niejijing.github.io源的settings中设置custom domain即可，然后在阿里云那里设置a和cname。

cname不能绑定example.com吗？

a只能绑定example.com吗？

不过我发现，阿里云的www.example.com会自动转向example.com，如果example.com有定义的话。也有可能是www.example.com->niejijing.github.io->example.com，而不是阿里云搞得鬼。至于到底是什么需要做实验，但是由于DNS解析存在缓存延迟，所以会比较蛋疼。而且，一旦绑定了域名，niejijing.github.io也会跳转到example.com（如果你在niejijing.github.io里面设置的是example.com的话）。