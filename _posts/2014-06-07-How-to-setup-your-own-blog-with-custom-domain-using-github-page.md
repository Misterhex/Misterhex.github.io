I am hosting this blog free using github page. I use hyde theme and custom domain from godaddy. I will share the steps below so that anyone can follow and get their own blog hosted for free.

First, you need to sign for a github account. Headover [here](https://github.com/) to sign up. 

Create a repository in your github account and name the repository as follows {yourusername}.github.io.

The traditional method is to use [Jekyll](http://jekyllrb.com/) to setup the blog from scratch. However, an easier way is to clone a jekyll repo theme and just start off from there, for this blog, i am using [Hyde](http://hyde.github.io/).

Clone [Hyde](http://hyde.github.io/) on your machine, then simply replace the files in your own repository that you cloned earlier, commit your files and push.

go to http://{yourusername}.github.io and you should see your pages coming up soon.

To setup a custom domain, github has well-documented instructions [here](https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages). 

First, you need to buy a personal domain from a name provider. I am using [godaddy](http://sg.godaddy.com/), purchase your domain and edit your A record to point to 192.30.252.153. 

Second, add create a file and name it "CNAME", in the file, add in your purchased domain. For example, mine is [yongheng.me](http://yongheng.me). ![cname file]({{ site.url }}/public/img/vim_cname.PNG)   ![cname file content]({{ site.url }}/public/img/vim_cname_2.PNG) 

Thats all! Your blog is now setup with your own custom domain. For more information on how to write a new blog post, etc. visit [Jekyll](http://jekyllrb.com/).
