# <p align="center">😋FeedMe</p>

<div align="center">

[![Next.js](https://img.shields.io/badge/Next.js-19-black?style=flat&logo=next.js)](https://nextjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.8-blue?style=flat&logo=typescript)](https://www.typescriptlang.org/)
[![React](https://img.shields.io/badge/React-19-blue?style=flat&logo=react)](https://reactjs.org/)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind-3.4-06B6D4?style=flat&logo=tailwindcss&logoColor=white)](https://tailwindcss.com/)
[![RSS](https://img.shields.io/badge/RSS-Feed-orange?style=flat&logo=rss)](https://en.wikipedia.org/wiki/RSS)
[![OpenAI](https://img.shields.io/badge/OpenAI-API-412991?style=flat&logo=openai)](https://openai.com/)
[![pnpm](https://img.shields.io/badge/pnpm-8.4.0-F69220?style=flat&logo=pnpm)](https://pnpm.io/)

[![GitHub Workflow Status](https://img.shields.io/github/actions/workflow/status/Seanium/feedme/update-deploy.yml?branch=main&style=flat&logo=github)](https://github.com/Seanium/feedme/actions)
[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-Active-4EA94B?style=flat&logo=github)](https://feedme.icu)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![RSS Update](https://img.shields.io/badge/RSS%20Update-Every%203h-lightgrey?style=flat&logo=github-actions)](https://github.com/Seanium/feedme/blob/main/.github/workflows/update-deploy.yml)

</div>

<p align="center">
  <b>一个基于Next.js的RSS源聚合阅读网站，自动使用AI生成中文摘要</b>
</p>

<p align="center">
  <a href="https://feedme.icu" target="_blank">🌐 在线演示</a> •
  <a href="#主要功能">✨ 功能</a> •
  <a href="#技术栈">🔧 技术栈</a> •
  <a href="#本地开发">💻 开发</a> •
  <a href="#生产部署">🚀 部署</a>
</p>

---

## 主要功能

- **多源RSS聚合**: 从多个信息源获取并整合RSS内容
- **AI摘要生成**: 自动使用LLM为文章生成中文摘要
- **定时更新机制**: 通过GitHub Actions定期自动更新内容
- **分类浏览**: 支持按分类查看不同信息源
- **主题切换**: 支持明暗主题切换
- **静态部署**: 可部署在GitHub Pages等静态托管服务上

## 技术栈

- **框架**: [Next.js 15](https://nextjs.org/)，[React 19](https://react.dev/)
- **语言**: [TypeScript 5](https://www.typescriptlang.org/)
- **样式**: [Tailwind CSS](https://tailwindcss.com/)，[shadcn/ui](https://ui.shadcn.com/)
- **包管理**: [pnpm](https://pnpm.io/)
- **部署**: [GitHub Actions](https://github.com/features/actions)，[GitHub Pages](https://pages.github.com/)
- **RSS解析**: [rss-parser](https://www.npmjs.com/package/rss-parser)

## 运行步骤

### 本地开发

1. **克隆仓库**
   ```bash
   git clone https://github.com/Seanium/feedme.git
   cd feedme
   ```

2. **安装依赖**
   ```bash
   pnpm install
   ```

3. **配置环境变量**
   
   创建`.env.local`文件，添加以下内容：
   ```
   LLM_API_KEY=你的API密钥
   LLM_API_BASE=LLM服务的API基础URL（例如：https://api.siliconflow.cn/v1）
   LLM_NAME=使用的模型名称（例如：THUDM/GLM-4-9B-0414）
   ```
   这些环境变量用于配置文章摘要生成功能，需要从LLM服务提供商获取

4. **更新RSS数据**
   ```bash
   pnpm update-feeds
   ```
   此命令会抓取RSS源并生成摘要，保存到`data`目录

5. **启动开发服务器**
   ```bash
   pnpm dev
   ```
   访问 [http://localhost:3000](http://localhost:3000) 查看应用

6. **自定义RSS源**
   
   编辑`config/rss-config.js`文件以修改或添加RSS源。每个源需要包含：
   - 名称
   - URL
   - 分类

### 生产部署

本项目使用GitHub Actions自动部署到GitHub Pages，使用单一工作流同时处理数据更新和网站部署。

#### 工作流说明

**更新数据并部署** (`update-deploy.yml`)：
- 触发条件：
  - 定时执行（每3小时一次）
  - 推送到main分支
  - 手动触发
- 执行内容：
  - 获取最新RSS内容并生成摘要
  - 构建静态网站
  - 部署到GitHub Pages

#### 部署步骤

1. **Fork或克隆仓库**到你的GitHub账号

2. **设置GitHub Secrets**
   
   在项目顶端 Settings - 左侧 Secrets and variables：Actions 中添加以下密钥：
   - `LLM_API_KEY`: 用于AI摘要生成的API密钥
   - `LLM_API_BASE`: LLM服务的API基础URL
   - `LLM_NAME`: 使用的模型名称

3. **启用GitHub Pages**
   
   在仓库设置中，选择从GitHub Actions部署

4. **手动触发工作流**（可选）
   
   在GitHub仓库的Actions页面手动触发"更新数据并部署"工作流

#### 自定义部署配置

- **修改更新频率**: 编辑`.github/workflows/update-deploy.yml`中的cron表达式
  ```yml
  # 例如，改为每天凌晨更新一次
  cron: '0 0 * * *'
  ```

- **调整保留条目数**: 修改`config/rss-config.js`中的`maxItemsPerFeed`值

- **自定义域名配置**:
  请按照以下内容设置，避免出现页面资源加载异常：
  - **不使用自定义域名**: 请删除目录下的`CNAME`文件
  - **使用自定义域名**: 在仓库设置的GitHub Pages部分添加自定义域名，并修改CNAME文件内容为自定义域名

## Star 趋势

<a href="https://www.star-history.com/#Seanium/feedme&Date">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=Seanium/feedme&type=Date&theme=dark" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=Seanium/feedme&type=Date" />
   <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=Seanium/feedme&type=Date" />
 </picture>
</a>

## 许可证

[MIT](LICENSE) © 2025 Seanium