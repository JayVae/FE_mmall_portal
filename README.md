# FE_mmall_portal（前端）

> 这是商城前端门户项目（前端），用于配合后端项目 [JayVae/javadianshang](https://github.com/JayVae/javadianshang) 一起使用。

---

## 目录
- 项目简介
- 技术栈（说明）
- 前置条件
- 项目初始化步骤（兼容源码提供的旧版启动说明）
- 快速开始（本地开发）
- 环境变量
- 代理 / CORS 配置
- 构建与部署
- 接口对接说明
- 常见问题
- 贡献与联系方式
- 许可证

---

## 项目简介
本仓库为商城门户的前端实现，负责商品展示、用户下单、购物车、用户中心等页面。后端服务由仓库 [JayVae/javadianshang](https://github.com/JayVae/javadianshang) 提供接口，请确保后端服务可用并正确配置跨域或代理。

---

## 技术栈（示例）
（请根据实际项目替换下列项）
- 框架：可能使用 Vue / React（以 package.json、src 结构为准）
- UI：例如 Element UI / Ant Design
- 状态管理：Vuex / Redux / Pinia
- 构建工具：webpack (本仓库历史版本可能使用 webpack@1.x)
- 语言：JavaScript / TypeScript（以源码为准）

---

## 前置条件
- Node.js（见下“项目初始化步骤”中关于版本的说明）
- npm 或 yarn
- 可访问后端服务（参考： https://github.com/JayVae/javadianshang ）

注意：本项目仓库历史上使用了较旧的工具链（webpack@1.x、webpack-dev-server@1.x），源码包里可能假定使用较旧的 Node 版本。请优先参考“项目初始化步骤”部分以获得可复现的本地环境配置。

---

## 项目初始化步骤
下面为仓库随源码提供的初始化步骤（适用于源码提供的旧版构建/开发流程）。如果你使用现代化改造（如升级到 webpack 4/5、Vite 等），请参照现代化文档进行调整。

1. 安装 nodejs 环境，推荐使用 v4.4.7  
   下载地址 : https://nodejs.org/download/release/v4.4.7/

2. 全局安装 webpack v^1.15.0  
   命令:
   ```bash
   (sudo) npm install -g webpack@^1.15.0
   ```

3. 全局安装 webpack-dev-server v^1.16.5  
   命令:
   ```bash
   (sudo) npm install -g webpack-dev-server@^1.16.5
   ```

4. 在慕课网上下载源码，解压缩

5. 在项目根目录执行 npm 初始化（安装依赖）  
   命令:
   ```bash
   npm install (--registry=https://registry.npm.taobao.org)
   ```
   说明：若国内网络访问 npm 慢，可加 `--registry` 指向淘宝/npmmirror 镜像，或使用 cnpm/yarn。

6. 启动项目  
   - 开发模式: `npm run dev`（Windows 上可能为 `npm run dev_win`）  
   - 生产模式: `npm run dist`（Windows 上为 `npm run dist_win`）

7. 开发模式下预览项目  
   访问： http://localhost:8088/dist/view/index.html

8. 注意：webpack-dev-server 会将被访问的文件放在内存中，所以 webpack-dev-server 没有自己的输出文件夹。如果编译的文件路径和内存中的路径相同，优先调用内存中的文件。

---

## 快速开始（现代/通用）
如果你已经用现代工具（Node 14+、Vite 或新版 webpack）或想按常规方式运行，请先查看仓库根目录的 package.json 并按 scripts 字段使用对应脚本。示例：

1. 克隆仓库
   ```bash
   git clone https://github.com/JayVae/FE_mmall_portal.git
   cd FE_mmall_portal
   ```

2. 安装依赖
   ```bash
   # npm
   npm install

   # 或者 yarn
   yarn
   ```

3. 配置环境（参考下面的“环境变量”部分），或使用本地代理以便调用后端接口。

4. 启动开发服务器（以 package.json scripts 为准）
   ```bash
   npm run dev
   # 或
   yarn dev
   ```

5. 打开浏览器访问开发服务器输出地址（终端会显示实际地址，常见为 http://localhost:8088 或 Vite 的默认端口）

---

## 环境变量（示例）
前端通常需要配置后端 API 地址。根据项目使用的构建工具，环境变量名称可能不同，例如：

- Create React App: `REACT_APP_API_BASE_URL`
- Vite: `VITE_API_BASE_URL`
- Vue CLI: `VUE_APP_API_BASE_URL`

示例 `.env.development`：
```env
# 后端接口基础地址（示例）
REACT_APP_API_BASE_URL=http://localhost:8080/api
```

在代码中读取示例（以 React 为例）：
```js
const apiBase = process.env.REACT_APP_API_BASE_URL || 'http://localhost:8080/api';
```

---

## 代理 / CORS 配置
开发时常用两种方式解决跨域访问后端：

1. 前端代理（推荐本地开发时使用）
   - webpack devServer 或 Vite 中配置 proxy，将 `/api` 转发到后端地址。
   - Vite 示例（vite.config.js）：
     ```js
     export default defineConfig({
       server: {
         proxy: {
           '/api': {
             target: 'http://localhost:8080',
             changeOrigin: true,
             rewrite: (path) => path.replace(/^\/api/, '/api')
           }
         }
       }
     })
     ```

2. 后端允许跨域（生产或后端可控的情况）
   - 请在后端项目中启用 CORS，或在 API 网关层允许跨域。

后端仓库： https://github.com/JayVae/javadianshang

---

## 构建与发布
打包生产：
```bash
npm run build
# 或
yarn build
```

构建输出会在 `dist/` 或 `build/` 目录下（以项目配置为准）。按托管平台（Nginx / CDN / 静态托管）上传部署。部署时请确保后端接口 URL 已切换为生产地址，并处理好跨域或同域策略。

Nginx 示例（将前端托到 /var/www/frontend）：
```nginx
server {
  listen 80;
  server_name example.com;

  root /var/www/frontend;
  index index.html;

  location / {
    try_files $uri $uri/ /index.html;
  }

  location /api/ {
    proxy_pass http://backend-host:8080/api/;
  }
}
```

---

## 接口对接说明
- 本项目调用的后端接口由仓库提供： [JayVae/javadianshang](https://github.com/JayVae/javadianshang)  
- 开发或联调前，请与后端保持接口文档同步，确认：
  - 接口基地址（base URL）
  - 请求路径与参数
  - 鉴权方式（例如 token 在 header 中的键名）
  - 错误码约定

需要我可以从前端代码中提取实际调用到的接口列表并与后端接口文档比对 —— 若需要请授权我查看仓库文件或把相关文件粘贴过来。

---

## 常见问题
- 运行时报错找不到依赖：尝试删除 node_modules 和 lock 文件后重装：
  ```bash
  rm -rf node_modules package-lock.json yarn.lock
  npm install
  ```
- 访问 API 报跨域错误：检查本地代理或后端 CORS 配置。
- webpack-dev-server 将文件保存在内存中：开发模式下访问的文件可能不是磁盘上的文件，刷新/查看打包结果时请使用构建产物目录（如 `dist/`）。

---

## 贡献
欢迎提交 issue 或 PR。贡献步骤建议：
1. Fork 本仓库
2. 新建分支 feat/xxx 或 fix/yyy
3. 提交代码并开 PR，描述变更与复现步骤
4. CI / 测试通过后合并

---

## 联系方式
- 后端仓库与作者： [JayVae/javadianshang](https://github.com/JayVae/javadianshang)
- 作者 GitHub：[@JayVae](https://github.com/JayVae)

---

## 许可证
本项目遵循 LICENSE 中规定的协议（请在仓库根目录添加或检查 LICENSE 文件）。

---

如果你希望我直接把这个 README 提交到仓库（创建/更新 README.md），或者想让我根据仓库的实际内容自动生成更精确的 README（包括 package.json scripts、真实依赖、端口、截图等），我可以继续：
- 帮你提 PR（需要仓库写入权限），或
- 先读取仓库文件（package.json、src 结构等）并据此完善 README。请选择下一步。
