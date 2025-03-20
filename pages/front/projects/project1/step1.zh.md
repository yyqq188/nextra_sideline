# 工程的创建

利用next.js14以及headless CMS 的Strapi,构建网站,通过一个项目来涵盖next.js14的所有功能和操作.
那么我们做的网站,就是为youtube视频自动提取摘要,就可以避开一些我不感兴趣的视频
我们要使用到的技术
## Next.js
next.js是一个基于React JavaScript库构建的前端框架，该库以创建引人入胜的用户界面而闻名。
<br>它简化了Web开发，因此您可以专注于最重要的部分，为您的客户构建项目。
Next.js的一个主要功能是服务器端渲染。这提高了Web应用程序的加载速度和在搜索引擎上的排名,这对性能和SEO非常有益。
<br>Next.js 还具有静态生成功能。这意味着它会在需要文件之前创建文件，以便文件可以快速加载而无需等待数据被获取。
<br>此外，Next.js 还提供增量静态再生功能。此功能可使用动态更改来更新自首次创建以来未预生成或更新的页面的静态内容。
为了设计我们的应用程序，我们将使用**Tailwind**和**ShadcnUI**。

## Strapi
为了使我们的应用程序构建更加容易，我们将利用Strapi Headless CMS的强大功能来管理我们的数据和身份验证。
<br>
Strapi是一个开源的无头内容管理系统 (CMS)。与与内容的前端呈现紧密结合的传统 CMS 不同，Strapi 等无头 CMS 为您的内容提供后端管理并通过 RESTful 或 GraphQL API 提供服务。
<br>
这意味着您可以使用 Strapi 来管理您的内容，然后将其传送到您选择的任何前端框架或平台，例如网站、移动应用程序或其他服务器端应用程序。
<br>
使用 Strapi 的主要好处包括其灵活性，因为它允许开发人员自定义管理面板、API 和数据库查询以满足他们的特定需求。
<br>
它还支持许多数据库，并且可以轻松地与 React、Vue 和 Angular 等流行的前端框架集成。
<br>
Strapi 旨在简化内容管理流程，使开发人员和内容创建者更容易高效地工作,在更短的时间内完成更多的事情。

## 项目中将会学习和了解的内容有
### Next.js14
- 路由,布局,嵌套布局和布局组
- 数据的获取,缓存和变异
- 加载界面
- 路由处理程序
- 元数据文件
- 服务器和客户端组件
- 中间件
- 使用Shadcn UI构建UI组件
- 使用Tailwindcss实现样式的设置
- 使用Zod进行验证
- 用Typescript编写

### Strapi
- 内容构建
- API路由,中间件和策略Strapi
- 使用Strapi的ORM
- Strapi上的数据库

## 项目概况
### 主页
这是一个基本的landing page页面,其中包括顶部导航,hero部分,feature部分和页脚部分,所有的顶部导航,hero部分和页脚部分都将存储到strapi实例中.
### 登录和注册页面
我们会使用strapi处理身份验证,并通过next中间件文件保护我们的路由,用于检查用户是否授权并进行相应的重定向.
### 仪表板
### 生成摘要
输入您的youtube视频网址,它将调用openAI根据youtube视频记录生成我们的摘要.对于本教程,我们将使用LangChain库来调用开放的AI,而且它为我们提供了一些附加功能,我们可以使用这些功能在将来扩展我们的应用程序.
### 摘要和注释页
在这里,可以访问您的摘要列表和摘要详细信息页面,可以更新文本和删除摘要.
### 账户页面
我们将拥有自己的账户页面,可以更新自己的名字,简介和照片.

## 建立我们的初始项目

### 设置我们的前端
```shell
(base) ➜  VscodeProjects mkdir next_course
(base) ➜  VscodeProjects cd next_course
(base) ➜  next_course npx create-next-app@latest
```
显示下面的选择项,请全部选择yes,项目名为front
```shell
Need to install the following packages:
create-next-app@14.2.13
Ok to proceed? (y) y
✔ What is your project named? … frontend
✔ Would you like to use TypeScript? … Yes 
✔ Would you like to use ESLint? …  Yes
✔ Would you like to use Tailwind CSS? … Yes
✔ Would you like to use `src/` directory? … Yes
✔ Would you like to use App Router? (recommended) … Yes
✔ Would you like to customize the default import alias (@/*)? … Yes
✔ What import alias would you like configured? … @/*
```
创建项目后，让我们导航到前端文件夹cd frontend并运行yarn dev以启动我们的前端项目。
您的项目将从http://localhost:3000启动。

### 设置Shadcn UI进行样式设置
我们将使用 Tailwind 和 Shadcn UI 组件来美化我们的应用。Shadcn UI 非常棒，因为它允许您将 UI 组件直接安装到代码库中并相应地修改/扩展它们。
<br>
安装shadcnui ,请安装指定版本的shadcnui,当前最新的0.9.2版本有bug,并选择下面的选项

```shell
npx shadcn-ui@0.8.0 init

k to proceed? (y) y
✔ Which style would you like to use? › Default
✔ Which color would you like to use as base color? › Slate
✔ Would you like to use CSS variables for colors? … yes
```

这样在项目中就会出现components文件夹
<br>
安装button组件

```shell
npx shadcn-ui@0.8.0 add button
```
在我们项目的page.tsx中替换为下面的代码
```tsx
import { Button } from "@/components/ui/button";

export default function Home() {
  return (
    <main className="container mx-auto py-6">
      <Button>Our Cool Button</Button>
    </main>
  );
}
```

### 设置我们的后端
对于后端,我们将使用strapi headless CMS,strapi可以在几分钟内构建后端,而不是几天或几周.它具有高度可定制性并容易上手.
<br>
在next_course文件夹内执行

```shell
npx create-strapi-app@latest backend
```
并使用下面的选择项
```shell
Start your 14-day free trial now!


? Please log in or sign up. Skip
? Do you want to use the default database (sqlite) ? Yes
? Start with an example structure & data? No
? Start with Typescript? Yes
? Install dependencies with npm? Yes
? Initialize a git repository? Yes
```

## 后端开发
### 创建我们的第一个集合类型 collection type
先通过content-type builder 创建single type,然后再通过content manager 发布出去,这里的content manager就是发布api接口的意思
