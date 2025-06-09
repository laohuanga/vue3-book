### 1.启动项目

1.进入项目目录
cd vue3-pbstar-books
2.安装依赖
npm install
3.运行项目前端模块
npm run dev
4.运行项目后端模块
npm run start

### 2.项目结构
├── public
│   └── favicon.ico
├── server 				# 服务端模块
│   ├── db.json			# 数据库文件
│   ├── db.json.bk		# 数据库备份
│   └── public			# 资源文件夹
│       └── imgs			# 图片存储
src
├── App.vue                # 根组件，控制页面整体布局，包含头部、底部和路由视图
├── api                    # 存放与后端交互的 API 请求相关代码
│   ├── http.ts            # 封装 HTTP 请求，可能包含请求拦截器、响应拦截器等
│   └── module             # 按模块划分的 API 请求文件
│       ├── admin.ts       # 与管理员相关的 API 请求
│       └── book.ts        # 与图书相关的 API 请求
├── assets                 # 存放静态资源
│   ├── css                # 存放 CSS 样式文件
│   │   ├── base.css       # 基础样式文件，可能包含全局通用样式
│   │   └── main.css       # 主要样式文件，包含项目主体样式
│   └── imgs               # 存放图片资源
│       ├── banner.jpg     # 轮播图等使用的横幅图片
│       ├── 图书.png       # 可能用于网站标题或标识的图片
│       └── 图书_1.png     # 可能用于网站页脚或其他位置的图片
├── components             # 存放可复用的组件
│   ├── 404                # 404 页面组件
│   │   └── index.vue      # 当访问不存在页面时显示的组件
│   ├── book               # 与图书展示相关的组件
│   │   ├── item.vue       # 单个图书项的展示组件
│   │   ├── list.vue       # 图书列表展示组件
│   │   ├── listBanner.vue # 图书列表页面的横幅组件
│   │   ├── listPagination.vue # 图书列表的分页组件
│   │   └── listTop.vue    # 图书列表顶部标题和更多链接组件
│   ├── footer             # 页脚组件
│   │   └── index.vue      # 网站页脚组件，包含版权信息等
│   └── header             # 头部组件
│       └── index.vue      # 网站头部组件，包含导航菜单等
├── main.ts                # 项目入口文件，用于初始化 Vue 应用，挂载路由、状态管理等
├── router                 # 存放路由配置相关代码
│   └── index.ts           # 路由配置文件，定义页面跳转规则
├── stores                 # 存放状态管理相关代码
│   └── book.ts            # 使用 Pinia 管理图书相关状态的文件
└── views                  # 存放页面视图组件
    ├── about              # 关于页面
    │   └── index.vue      # 关于我们页面组件
    ├── admin              # 管理员后台页面
    │   ├── books          # 图书管理相关页面
    │   └── index.vue      # 管理员后台主页组件
    ├── all                # 全部图书页面
    │   └── index.vue      # 展示全部图书的页面组件
    ├── detail             # 图书详情页面
    │   └── index.vue      # 展示单个图书详细信息的页面组件
    ├── home               # 首页
    │   └── index.vue      # 网站首页组件，包含轮播图、热门推荐等
    ├── list               # 图书列表页面
    │   └── index.vue      # 根据分类展示图书列表的页面组件
    └── login              # 登录页面
        └── index.vue      # 用户登录页面组件├── .env
├── .gitignore
├── README.md
├── env.d.ts
├── index.html
├── package-lock.json
├── package.json
├── tsconfig.app.json
├── tsconfig.json
├── tsconfig.node.json
├── vite.config.ts
└── 运行.txt


//本项目由hh-laohuang创建，使用的技术栈为：Vue3+Vite+TypeScript+Pinia+ElementPlus+Axios

### 技术栈
- **前端框架**：Vue 3
- **状态管理**：可能使用 `Pinia` 或 `Vuex`（从 `src/stores/book.ts` 推测）
- **路由管理**：`vue-router`
- **UI 框架**：`Element Plus`
- **构建工具**：Vite
- **类型检查**：TypeScript
