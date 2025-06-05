# Hugo入门




hugo入门



```

# github hugo
https://github.com/gohugoio/hugo
# https://github.com/gohugoio/hugo/releases/download/v0.147.3/hugo_extended_0.147.3_darwin-universal.tar.gz文件放到bin下面就行
# 标准版和扩展版，推荐使用扩展版

# 官方文档
https://gohugo.io/
# 主题
https://themes.gohugo.io/
# 中文文档
https://hugo.opendocs.io/

# hugo介绍、按照
https://blog.csdn.net/sinat_41672927/article/details/146426733

# 常用命令
# 新建站点
hugo new site <site-name>
# 启动本地服务
hugo server
hugo server -D
# 本地构建
hugo
# 新文章
hugo new posts/first-post.md


# GitHubPages托管，github.io设置
1、github 创建仓库，名称： <yourname>.github.io
2、hugo.toml 设置 baseURL 为 https://<yourname>.github.io
3、hugo命令构建静态资源
4、上传构建成果到仓库中
5、浏览器打开 https://<yourname>.github.io


# 主题 LoveIt
https://github.com/dillonzq/LoveIt/blob/master/README.zh-cn.md
# 官方说明
https://hugoloveit.com/categories/documentation/

git init
git submodule add https://github.com/dillonzq/LoveIt.git themes/LoveIt
或者
下载release包，放到themes目录下，文件夹名称为LoveIt





# 新建站点
# hugo new site demo-1                      
Congratulations! Your new Hugo site was created in /Users/wsongl/Documents/wsongl/code/demo-1.

Just a few more steps...

1. Change the current directory to /Users/wsongl/Documents/wsongl/code/demo-1.
2. Create or install a theme:
   - Create a new theme with the command "hugo new theme <THEMENAME>"
   - Install a theme from https://themes.gohugo.io/
3. Edit hugo.toml, setting the "theme" property to the theme name.
4. Create new content with the command "hugo new content <SECTIONNAME>/<FILENAME>.<FORMAT>".
5. Start the embedded web server with the command "hugo server --buildDrafts".

See documentation at https://gohugo.io/.




echo "# wsongl.github.io" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin git@github.com:wsongl/wsongl.github.io.git
git push -u origin master

git remote add origin git@github.com:wsongl/wsongl.github.io.git
git branch -M master
git push -u origin master



# loveit设置
## 头像设置
static/images/avatar.png

## 默认md文件内容
archetypes/default.md

## posts.md字段设置
https://fatesinger.com/103481



```




