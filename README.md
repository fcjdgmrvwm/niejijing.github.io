# niejijing.github.io
homepage.

## 建立github page

申请个人主页，只能有一个，但是项目主页无数。个人主页对应github源username.github.io(niejijing.github.io)，然后项目主页就是usename.github.io/repository，个人主页是niejijing.github.io.

github page是一个好东西，一个能上传东西的博客。

## 自定义域名解析

疑点：apex domain、aname、cname

为网址绑定域名的过程是怎样的？为什么继续要在主机上绑定，也需要在域名提供商DNS Provider哪里绑定一次？两者有什么关系？需要一个协商的过程吗？例如，我一个域名可以解析两个主机吗？我认为是这样的，DNS解析决定往哪里转发请求，而目的地址通过绑定决定是否同意DNS转发过来的请求。

至于如何操作，在niejijing.github.io源的settings中设置custom domain即可，然后在阿里云那里设置a和cname。

cname不能绑定example.com吗？

a只能绑定example.com吗？

不过我发现，阿里云的www.example.com会自动转向example.com，如果example.com有定义的话。也有可能是www.example.com->niejijing.github.io->example.com，而不是阿里云搞得鬼。至于到底是什么需要做实验，但是由于DNS解析存在缓存延迟，所以会比较蛋疼。而且，一旦绑定了域名，niejijing.github.io也会跳转到example.com（如果你在niejijing.github.io里面设置的是example.com的话）。

目前看来，无论是a还是cname，在niejijing.github.io这个源里面生成的还是CNAME文件。

## 解析优先级

https://niejijing.github.io解析niejijing.github.io里的源没有错，但是包括其他项目的源也是通过这个域名解析，比如我有一个源叫math，然后niejijing.github.io这个源里面有个文件夹叫math，这会产生冲突，这时候github是这样解决的，如果math源开通了项目github page，那么只解析这个源里面的内容，忽视math文件夹下的内容，否则会解析math文件夹内的内容。

上述内容已经过测试。

~~另外，只有niejijing.github.io根目录下的index.html会被自动解析，子目录下的不会被自动解析。项目主页根目下的index.html也不会被自动解析。~~   会自动解析的，只是github page有延迟罢了。

## 网站托管

### 静态页面

很不幸地，github page只支持静态页面，html+css+js，不支持php等（php文件会被下载），所以需要防止静态页面被Chrome缓存。

一段存疑的说明：

> 请求一个静态文件浏览器会发出对应的请求头
>
> 该文件HASH不存在浏览器缓存中，而服务器又有存在这个文件则 浏览器下载这个文件，这是返回代码是200
>
> 如果HASH存在，则请求头会发送一个文件创建有时间，而这个时间和服务器上这个文件创建时间一直， 则返回代码304，浏览器读取本地文件

不过常用的方法有：

1. 给js、css、img等资源加上版本号，强迫Chrome更新页面（网页资源的不同的版本号会提示Chrome页面发生了改动）

   例如

   ```html
   <img src='lala?v1.0' />
   ```

   ​


1. 设置meta属性。

   ```html
   <HEAD>    
     <META   HTTP-EQUIV="Pragma"   CONTENT="no-cache">    
     <META   HTTP-EQUIV="Cache-Control"   CONTENT="no-cache">    
     <META   HTTP-EQUIV="Expires"   CONTENT="0">    
   </HEAD>  
   ```

   ​

### github page工具

jekyll 官方推荐静态网页生成工具



## 我的理解

### git源即是服务端

对于传统虚拟主机，如果我上传了文件到网站上，为了能够让这个文件的路径显示出来，我需要PHP文件来载入它，否则我要手动修改网页，然后上传新的网页。这一过程很难自动化，因为我正本地修改的网页，我很难通过自动化脚本列举出网站上传目录下的所有文件，然后生成网页，我只能自己改（或通过服务器的PHP自动生成网页）。但是对于github page而言，托管的页面就是一个git源，无论你上传到那里，上传的文件都会在git源里面，那么通过对本地git源里的内容进行枚举，就很容易通过自动化脚本生成静态网页，而不需要PHP了。

也就是两者的区别在于：github page你是一定保存有网站的一个备份的，目录结构都相同，本地和云端同步，所以操作本地等同于操作云端，那么很多需要云端PHP的功能（如枚举网站目录下的文件），可以通过本地自动生成静态页面，然后和云端同步来完成。而对于虚拟主机而言，本地和云端往往没有同步机制，两者往往是不同步的，所以难以达到在本地就可以完成云端才能完成的任务的目的。由于两者是不同步的，所以本地自动化脚本也就无能为力了。

### git源即是一个网站目录

很明显地，git源就是一个网站目录，每个目录都可以通过git管理，实现本地和云端的完全同步，而且整个网站是模块化的管理，非常方便。

### 使用markdown就可以编写网站

只要你选择了theme（或者在源里面创建_config.yml，然后设置theme字段），那么github就会通过jekyll来从markdown中自动生成页面。

你也可以本地自己通过jekyll生成静态页面，然后通过git源上传。毕竟，github原生的主题并不总是如人意。

### github page就是一个虚拟主机

很明显，github page是github主机上的一个目录，也就是虚拟主机，但是github不会为它提供cgi，也不会加载形如.htaccess文件等。

## jekyll缓存

github page在_config.yml失效导致构建jekyll失败的时候，会返回以前缓存成功的页面，并且这个缓存会保持相当长的一段时间，直到最新一次成功的构建到来。

如何解决Chrome缓存github page的问题？因为github page是静态页面。但是我决不总是Chrome的问题，考虑到jekyll存在缓存机制，因此很难迅速更新页面，如果你使用的是github上面的theme的话。果然，经过我测试，发现果然是github page缓存机制导致的问题，而不是chrome的锅，因为当内容确实发生改变的时候，chrome不会使用本地缓存页面（除非你没有使用服务器，只是在本地写了一个html和css，那么就没有服务器通知chrome重新加载新页面的机制了）。

### 坑

我们直到在niejijing.github.io下面必须要有index.html，其他项目主页是不需要index.html，因为jekyll的readme插件会自动将md文件转换成html，把README.md转换成index.html，并且，当index.html和README.md同时存在是，优先解析index.html。那么当index.html不存在的时候，解析README.md

如果a项目主页下存在b.md，那么我们可以niejijing.github.io/b.html访问，也可以使用niejijing.github.io/b.md访问，不过后者之前访问md文件内容，前者返回经过kekyll渲染后的页面。但是不存在README.html，这一点请注意。

那么如果目录下存在index.md，又会如何？index.md > README.md.





## test

[math readme](math/README.md)