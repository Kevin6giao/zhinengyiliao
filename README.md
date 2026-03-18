# 智能医疗助手系统

一个基于 LangChain4j 和 Vue3 构建的智能医疗助手系统，提供 AI 医疗问答、挂号预约等功能。

## 项目简介

智能医疗助手系统是一个前后端分离的 AI 医疗应用，主要功能包括：

- **AI 智能问答**：基于大语言模型的医疗咨询助手
- **AI 分导诊**：根据患者病情智能推荐合适的科室
- **AI 挂号助手**：智能查询、预约和取消挂号服务
- **知识库 RAG**：基于向量检索增强的医疗知识问答

## 技术栈

### 后端

- **Spring Boot 3.2.6**：后端框架
- **LangChain4j 1.0.0-beta3**：AI 应用开发框架
- **MyBatis-Plus 3.5.11**：持久层框架
- **MongoDB**：聊天历史存储
- **MySQL**：业务数据存储
- **Pinecone**：向量数据库（知识库检索）
- **OpenRouter**：大模型 API（支持多种 LLM）

### 前端

- **Vue 3**：前端框架
- **Element Plus**：UI 组件库
- **Vite**：构建工具
- **Axios**：HTTP 请求库

## 项目结构

```
智能医疗助手系统/
├── zhushou-ai/                 # 后端项目
│   ├── src/
│   │   ├── main/
│   │   │   ├── java/          # Java 源代码
│   │   │   └── resources/     # 配置文件
│   │   └── test/              # 测试代码
│   └── pom.xml                # Maven 配置
│
├── zhushou-ui/                # 前端项目
│   ├── src/
│   │   ├── components/        # Vue 组件
│   │   ├── assets/            # 静态资源
│   │   └── main.js            # 入口文件
│   └── package.json           # npm 配置
│
└── rag/                       # 知识库文档
    ├── 医院信息.md
    ├── 科室信息.md
    └── ...
```

## 快速开始

### 前置要求

- JDK 17+
- Maven 3.8+
- Node.js 18+
- MySQL 8.0+
- MongoDB 6.0+
- Pinecone 账户（向量数据库）

### 环境变量配置

#### 后端环境变量

在启动后端前，需要设置以下环境变量：

```bash
# OpenRouter API Key（大模型）
export OPENROUTER_API_KEY="your-openrouter-api-key"

# Pinecone API Key（向量数据库）
export PINECONE_API_KEY="your-pinecone-api-key"
```

#### 数据库配置

确保 MySQL 和 MongoDB 服务已启动：

- **MySQL**：创建数据库 `zhihuizhushou`
- **MongoDB**：确保本地服务运行在 `localhost:27017`

### 启动后端

```bash
cd zhushou-ai
mvn clean install
mvn spring-boot:run
```

后端服务启动后访问：`http://localhost:8080`

### 启动前端

```bash
cd zhushou-ui
npm install
npm run dev
```

前端服务启动后访问：`http://localhost:5173`

## 配置说明

### 后端配置文件

文件：`zhushou-ai/src/main/resources/application.properties`

```properties
# 服务端口
server.port=8080

# 大模型配置（OpenRouter）
langchain4j.open-ai.chat-model.base-url=https://openrouter.ai/api/v1
langchain4j.open-ai.chat-model.model-name=stepfun/step-3.5-flash:free

# 向量模型配置
langchain4j.open-ai.embedding-model.base-url=https://openrouter.ai/api/v1
langchain4j.open-ai.embedding-model.model-name=intfloat/multilingual-e5-large

# MongoDB 配置
spring.data.mongodb.uri=mongodb://localhost:27017/chat_memory_db

# MySQL 配置
spring.datasource.url=jdbc:mysql://localhost:3306/zhihuizhushou
spring.datasource.username=root
spring.datasource.password=root
```

## API 文档

启动后端后，可通过 Knife4j 访问 API 文档：

- Swagger UI：`http://localhost:8080/doc.html`

### 主要接口

| 接口 | 方法 | 描述 |
|-----|------|-----|
| `/xiaozhi/chat` | POST | AI 对话（流式输出） |

## 功能演示

1. **智能问答**：用户可以向 AI 助手咨询医疗相关问题
2. **科室推荐**：描述症状后，AI 推荐就诊科室
3. **挂号预约**：提供姓名、身份证、科室、日期等信息完成预约
4. **取消挂号**：支持取消已预约的挂号

## 注意事项

1. 首次使用需配置有效的 OpenRouter API Key
2. 确保 Pinecone 向量数据库已正确配置
3. MySQL 数据库需要提前创建 `zhihuizhushou` 数据库
