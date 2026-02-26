# 学生管理系统（Spring Boot + Vue 3）

一个前后端分离的学生管理系统，支持管理员、教师、学生三类角色。

## 项目架构

- 前端：`frontend-student-management/`（Vue 3 + Vite + Pinia + Vue Router）
- 后端：`student-management-system/`（Spring Boot + Spring Security + MyBatis + MySQL + JWT）

## 功能概览

### 管理员

- 学生管理（增删改查、统计）
- 教师管理
- 专业/班级/教室管理
- 课程目录与开课管理
- 选课开关与排课设置

### 教师

- 查看本人授课课程
- 查看课程选课学生
- 录入或修改成绩

### 学生

- 查看个人信息并修改
- 查看可选课程并选/退课
- 查看个人课表与成绩

## 技术栈

- 后端：Java 17、Spring Boot 3.3、Spring Security、MyBatis
- 数据库：MySQL 8
- 鉴权：JWT（Bearer Token）
- 前端：Vue 3、Vite、Pinia、Axios

## 环境要求

- JDK 17+
- Maven 3.9+
- Node.js 18+
- npm 9+
- MySQL 8+

## 启动步骤

### 1. 启动 MySQL 并准备数据库

后端默认连接配置位于：
`student-management-system/src/main/resources/application.properties`

请至少修改以下配置后再启动：

- `spring.datasource.url`
- `spring.datasource.username`
- `spring.datasource.password`
- `application.security.jwt.secret-key`

当前代码依赖的核心表包括：

- `user`
- `students`
- `teachers`
- `majors`
- `classes`
- `classrooms`
- `course_catalogs`
- `course_offerings`
- `offering_class_links`
- `enrollments`
- `system_settings`

说明：仓库内未包含完整 SQL 初始化脚本，请先导入你现有数据库结构。

### 2. 启动后端

```bash
cd student-management-system
./mvnw spring-boot:run
# Windows: mvnw.cmd spring-boot:run
```

默认端口：`8080`

后端启动时会自动尝试创建初始管理员账号：

- 用户名：`admin`
- 密码：`password123`

建议首次登录后立即修改。

### 3. 启动前端

```bash
cd frontend-student-management
npm install
npm run dev
```

默认地址：`http://localhost:5173`

前端接口地址在 `src/services/apiService.js`：

- `baseURL = http://localhost:8080/api`

## 认证与权限

- 登录接口：`POST /api/auth/authenticate`
- 注册接口：`POST /api/auth/register`
- 绝大多数业务接口要求 JWT 认证
- 路由按角色做前端守卫（`ADMIN` / `TEACHER` / `STUDENT`）

## 目录结构

```text
we/
├── frontend-student-management/
│   ├── src/views/                 # 管理员/教师/学生页面
│   ├── src/services/apiService.js # 前端 API 聚合
│   └── src/stores/auth.js         # 登录状态与角色解析
├── student-management-system/
│   ├── src/main/java/.../controller
│   ├── src/main/java/.../config
│   ├── src/main/resources/mappers
│   └── src/main/resources/application.properties
└── README.md
```

## 常见问题

1. 前端报 401/403
- 检查是否登录成功并携带 `Authorization: Bearer <token>`。

2. 前端跨域失败
- 检查后端 `SecurityConfig` 中的 CORS 配置（默认允许 `http://localhost:5173`）。

3. 后端启动失败（数据库）
- 检查 MySQL 连接串、用户名密码、数据库字符集与时区配置。

## 安全提示

- 不要把真实数据库密码和 JWT 密钥直接提交到仓库。
- 生产环境请关闭调试日志，并替换强随机密钥。

## 许可证

当前仓库未显式提供 License 文件。
