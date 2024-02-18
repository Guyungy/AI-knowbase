# 微软发布Azure ChatGPT：适用企业内使用的私有和安全 ChatGPT

### 优势

1. 私密性：内置的隐私保障，完全与OpenAI运营的保持隔离。
2. 可控性：网络流量可以完全隔离到您的网络中，并且还内置了其他企业级安全控制。
3. 价值：通过您自己的内部数据源（即插即用）或使用插件与您的内部服务集成（例如，ServiceNow等），增加业务价值。 我们构建了一个解决方案加速器，以赋能您的工作人员使用Azure ChatGPT。

### 📘先决条件

[**Azure OpenAI**](https://azure.microsoft.com/en-us/products/cognitive-services/openai-service/)

1. 要在Azure上部署和运行ChatGPT，您需要一个具有访问Azure OpenAI服务的Azure订阅。在[**此处**](https://customervoice.microsoft.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbR7en2Ais5pxKtso\_Pz4b1\_xUOFA5Qk1UWDRBMjg0WFhPMkIzTzhKQ1dWNyQlQCN0PWcu)请求访问权限。一旦获得访问权限，请按照此链接中的说明部署gpt-35-turbo或gpt-4模型。
2. 设置GitHub或Azure AD进行身份验证：下面[**添加身份提供者**](https://github.com/oliverlabs/azurechatgpt#-add-an-identity-provider)部分展示了如何配置身份验证提供者。> 💡注意：您可以使用[**NextAuth**](https://next-auth.js.org/providers/)提供者将身份解决方案配置为身份验证提供者。

### 👋🏻 介绍

* Azure ChatGPT 是使用下述技术构建的。
* Node.js 18：一个开源的、跨平台的 JavaScript 运行时环境。
* Next.js 13：通过扩展最新的 React 特性，使您能够创建全栈 web 应用程序。
* NextAuth.js：Next.js 13 的可配置身份验证框架。
* LangChain JS：用于构建智能应用程序的 AI 编排层。
* Tailwind CSS：是一个实用为先的 CSS 框架，提供一系列预定义的类，可以用来混合匹配每个元素的样式。
* shadcn/ui：使用 Radix UI 和 Tailwind CSS 构建的可重复使用组件。
* Azure Cosmos DB：完全托管的平台即服务（PaaS）NoSQL 数据库，用于存储聊天记录。
* Azure App Service：完全托管的平台即服务（PaaS），用于托管 web 应用程序、REST API 和移动后端。!\[下载失败]\(./微软发布Azure ChatGPT：适用企业内使用的私有和安全 ChatGPT.assert/architecture.png)

### 💙 一键部署 Azure

[**部署**](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fthivy%2Fazure-chatgpt%2Fmain%2Finfra%2Fmain.json) 点击“部署到 Azure”按钮，并根据[**环境变量**](https://github.com/microsoft/azurechatgpt#%F0%9F%94%91-environment-variables)部分在 Azure 门户中配置您的设置。 有关将身份验证添加到您的应用程序的重要信息，请参阅下面的部分。 ​

### 👨🏻‍💻 从本地机器运行

在本地克隆该存储库或者复制到您的Github账户。从“src”目录下运行以下所有步骤。

1. 确保在您的Azure订阅中部署了一个Cosmos DB实例。
2. 创建一个名为.env.local的新文件，用于存储环境变量，并添加以下变量。 **请注意：** 不要使用双引号，并且不要删除任何变量。 确保NEXTAUTH\_URL=[http://localhost:3000](http://localhost:3000) 这一行没有注释。

```json
# Azure OpenAI configuration
AZURE_OPENAI_API_KEY=
AZURE_OPENAI_API_INSTANCE_NAME=
AZURE_OPENAI_API_DEPLOYMENT_NAME=
AZURE_OPENAI_API_VERSION=

# GitHub OAuth app configuration
AUTH_GITHUB_ID=
AUTH_GITHUB_SECRET=

# Azure AD OAuth app configuration
AZURE_AD_CLIENT_ID=
AZURE_AD_CLIENT_SECRET=
AZURE_AD_TENANT_ID=

# When deploying to production,
# set the NEXTAUTH_URL environment variable to the canonical URL of your site.
# More information: https://next-auth.js.org/configuration/options

NEXTAUTH_SECRET=
NEXTAUTH_URL=http://localhost:3000

AZURE_COSMOSDB_URI=
AZURE_COSMOSDB_KEY=
```

1. 将当前活动目录更改为src
2. 通过运行npm install安装npm包
3. 通过运行npm run dev启动项目
4. 现在你应该会被提示登录你选择的OAuth提供者。登录成功后，你可以开始创建新的对话。​ !\[下载失败]\(./微软发布Azure ChatGPT：适用企业内使用的私有和安全 ChatGPT.assert/chat-home.png)

!\[下载失败]\(./微软发布Azure ChatGPT：适用企业内使用的私有和安全 ChatGPT.assert/chat-history.png)

### ☁️ 部署到 Azure - GitHub Actions

#### 🧬 Fork 该存储库

[**点此开始Fork**](https://github.com/microsoft/azurechatgpt/fork) 将该存储库fork到您自己的组织中，以便您可以针对自己的Azure订阅执行GitHub Actions。 ​

#### 🗝️ 在您的GitHub存储库中配置

1. AZURE\_CREDENTIALSGitHub工作流程需要一个名为AZURE\_CREDENTIALS的机密来对Azure进行身份验证。该机密包含在包含容器应用程序和容器注册表的资源组上具有Contributor角色的服务主体的凭据。
2. 在包含Azure应用服务的资源组上创建具有Contributor角色的服务主体。

```json
az ad sp create-for-rbac
   --name <NAME OF THE CREDENTIAL> --role contributor --scopes /subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP> --sdk-auth --output json
```

1. 复制命令的JSON输出。
2. 在GitHub存储库中，导航到“设置”>“机密”>“操作”，然后选择“新存储库机密”。
3. 将“AZURE\_CREDENTIALS”输入为名称，并将JSON输出的内容粘贴为值。
4. 选择“添加机密”。2.AZURE\_APP\_SERVICE\_NAME 在相同的存储库密钥下，添加一个新的变量AZURE\_APP\_SERVICE\_NAME，以部署到Azure Web应用程序。该密钥的值是您的Azure Web应用程序的名称，例如从[https://my-web-app-name.azurewebsites.net/](https://my-web-app-name.azurewebsites.net/)域中的my-web-app-name。

#### 🔄 运行GitHub Actions

一旦配置了密钥，GitHub Actions将在每次代码推送到存储库时触发。或者，您可以通过在GitHub的Actions选项卡中点击"Run Workflow"按钮来手动运行工作流。 !\[下载失败]\(./微软发布Azure ChatGPT：适用企业内使用的私有和安全 ChatGPT.assert/runworkflow.png)

#### 🪪 添加身份提供者

完成部署后，您需要添加一个身份提供者来对您的应用进行身份验证。 ​ ⚠️ 注意：下面只需要一个身份提供者。

#### GitHub身份验证提供者

我们将创建两个GitHub应用程序：一个用于本地测试，另一个用于生产环境。 ​

#### 🟡 开发应用程序设置

1. 前往GitHub OAuth应用程序设置页面 [https://github.com/settings/developers](https://github.com/settings/developers)
2. 创建一个新的OAuth应用程序 [https://github.com/settings/applications/new](https://github.com/settings/applications/new)
3. 填写以下细节

```json
Application name: Azure ChatGPT DEV Environment
Homepage URL: http://localhost:3000
Authorization callback URL: http://localhost:3000/api/auth/callback/github
```

#### 🟢 生产应用程序设置

1. 转到GitHub OAuth 应用程序设置 [https://github.com/settings/developers](https://github.com/settings/developers)
2. 创建一个新的OAuth 应用程序 [https://github.com/settings/applications/new](https://github.com/settings/applications/new)
3. 填写以下详细信息

```json
Application name: Azure ChatGPT Production
Homepage URL: https://YOUR-WEBSITE-NAME.azurewebsites.net
Authorization callback URL: https://YOUR-WEBSITE-NAME.azurewebsites.net/api/auth/callback/github
```

⚠️ 完成应用程序设置后，请确保本地和Azure应用服务上的环境变量已经更新。

```json
# Azure AD OAuth app configuration

AZURE_AD_CLIENT_ID=
AZURE_AD_CLIENT_SECRET=
AZURE_AD_TENANT_ID=
```

### 🔑 环境变量

以下是所需的环境变量，需要添加到Azure Portal或.env.local文件中。

| App Setting                          | Value              | Note                                                                                                                                   |
| ------------------------------------ | ------------------ | -------------------------------------------------------------------------------------------------------------------------------------- |
| AZURE\_OPENAI\_API\_KEY              |                    | Azure OpenAI资源的API密钥                                                                                                                   |
| AZURE\_OPENAI\_API\_INSTANCE\_NAME   |                    | Azure OpenAI资源的名称。                                                                                                                     |
| AZURE\_OPENAI\_API\_DEPLOYMENT\_NAME |                    | The name of your model deployment                                                                                                      |
| AZURE\_OPENAI\_API\_VERSION          | 2023-03-15-preview | API version when using gpt chat                                                                                                        |
| AUTH\_GITHUB\_ID                     |                    | Client ID of your GitHub OAuth application                                                                                             |
| AUTH\_GITHUB\_SECRET                 |                    | Client Secret of your GitHub OAuth application                                                                                         |
| NEXTAUTH\_SECRET                     |                    | Used to encrypt the NextAuth.js JWT, and to hash email verification tokens. **This set by default as part of the deployment template** |
| NEXTAUTH\_URL                        |                    | Current webs hosting domain name with HTTP or HTTPS. **This set by default as part of the deployment template**                        |
| AZURE\_COSMOSDB\_URI                 |                    | URL of the Azure CosmosDB                                                                                                              |
| AZURE\_COSMOSDB\_KEY                 |                    | API Key for Azure Cosmos DB                                                                                                            |
| AZURE\_AD\_CLIENT\_ID                |                    | The client id specific to the application                                                                                              |
| AZURE\_AD\_CLIENT\_SECRET            |                    | The client secret specific to the application                                                                                          |
| AZURE\_AD\_TENANT\_ID                |                    | The organisation Tenant ID                                                                                                             |
