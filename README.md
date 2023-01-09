### 自动化部署 CI / CD

CI 持续集成。(代码上传，自动代码检测、编译、单元测试)，

CD 对应两个概念 持续交付 、 持续部署（把持续集成结果放到测试环境，测试通过在发布到生产环境或）


### github actions + github pages 自动化发布

-   [发布到项目中 gh-pages 分支](https://blog.csdn.net/weixin_44765930/article/details/126415655)

    1.  点击 github 用户头像 => setting => 左侧菜单 Developer settings => Personal access tokens => Tokens(classic) 创建 获取到 [token]
    2.  在 github 项目中 => setting => Secrets => **Actions(创建变量)** => name:TOKEN ; secret:[token]
    3.  在 github 项目中根目录新建 .github/workflow/CI.yml

        ```yml
         # CI.yml
        name: run CI github_pages # 工作流的名称，随意
         # run-name: run CI github_pages # 工作流生成的工作流运行的名称,缺省时为commit信息，不必须
        on: # 触发条件
        push:
        	branches:
        	- main # 这里只配置了main分支，所以只有推送main分支才会触发以下任务
        	# paths:
        	#   - 'src/**' # 分支下下的scr里面的文件发生变化触发，不必须
        jobs:
        build-and-deploy: # 任务名字随意
        	runs-on: ubuntu-latest # 运行环境
        	# container: # 指定运行容器，我这里指定后会报错，不知道原因
        	#   image: node:16 # 指定node版本
        	steps: # 运行步骤
        	- name: Checkout  ️ # 步骤名字随意
        		uses: actions/checkout@v3 # 使用第三方action#，将代码拉取到虚拟机

        	# 安装依赖、打包，如果提前已打包好无需这一步
        	- name: Install and Build # 步骤名字随意
        		run: |
        		echo hello world
        		node -v
        		npm install
        		npm run build

        	# 部署
        	- name: Deploy # 步骤名字随意
        		uses: JamesIves/github-pages-deploy-action@v4.3.3 # 使用第三方action
        		with: # 基于第三方action 的一些配置
        		branch: gh-pages # 部署后提交到那个分支
        		folder: dist # 这里填打包好的目录名称

        ```

    4.  部署方式选择：在 github 项目中 => setting => pages => Branch 选择部署的分支 [gh-pages]。
        这种方式是从某个分支的代码进行部署，另一种方案是直接用 Action 的产物进行部署，此处不提及。

    5.  发布成功后 访问站点 https://[github 用户名].github.io/[厂库名]/

    6.  以后只要 main 分支有 push 就会自动走发布流程 (发布的文件有变动才会触发)
