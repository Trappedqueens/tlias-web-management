# Tlias 教学管理后端

基于 Spring Boot 3、MyBatis 和 MySQL 的教学管理学习项目，提供部门、员工、班级、学员、登录、文件上传和统计报表接口。

仓库是后端工程；`static/upload.html` 仅是上传演示页面，不是完整的管理系统前端。

## 功能

| 模块 | 内容 |
| --- | --- |
| 部门 | 部门列表、新增、修改、删除与详情 |
| 员工 | 分页查询、新增、修改、批量删除、详情和员工列表 |
| 班级 | 班级分页、详情、增删改、完整列表 |
| 学员 | 学员分页、详情、增删改、违纪扣分处理 |
| 登录 | 登录接口、令牌生成与校验相关代码 |
| 报表 | 员工职位/性别统计、学员学历统计、班级学员数量统计 |
| 上传 | 阿里云 OSS 文件上传与上传演示页面 |
| 学习示例 | Cookie、Session、JWT、日志与线程局部变量示例 |

## 技术栈

- Java 17、Spring Boot 3.2.10。
- MyBatis、PageHelper、MySQL。
- Spring AOP、JJWT、Lombok。
- 阿里云 OSS SDK。

## 目录

```text
src/main/java/com/itheima/
├─ controller/       HTTP 接口
├─ service/impl/     业务实现
├─ mapper/           数据访问
├─ filter/           过滤器示例
├─ interceptor/      拦截器
└─ pojo/             数据模型
src/main/resources/
├─ application.yml
└─ static/upload.html
src/test/java/com/itheima/
```

## 环境与配置

1. 准备 **JDK 17**、Maven、MySQL。
2. 按 [application.yml](src/main/resources/application.yml) 设置数据库连接与文件上传配置。
3. 当前 JDBC URL 使用的数据库名为 `dept`，可按实际环境修改。
4. 本仓库没有提供完整 SQL 初始化脚本，需要另行准备与实体类、Mapper 匹配的数据库表和初始数据；项目不会自动创建完整业务表。
5. 配置 OSS 的 endpoint、bucket、region 及代码使用的访问凭据来源。

真实密码、访问密钥和令牌签名信息不应直接提交到 GitHub。可使用本地不提交的配置或部署环境覆盖配置。

## 启动

```shell
git clone https://github.com/Trappedqueens/tlias-web-management.git
cd tlias-web-management
mvn spring-boot:run
```

默认端口为 **8080**。使用前先确认该端口没有被其他项目占用。

构建与运行：

```shell
mvn clean package -DskipTests
java -jar target/tlias-web-management-0.0.1-SNAPSHOT.jar
```

## 接口概览

| 方法 | 路径 | 说明 |
| --- | --- | --- |
| POST | `/login` | 员工登录 |
| GET / POST / PUT / DELETE | `/depts` | 部门管理 |
| GET / POST / PUT / DELETE | `/emps` | 员工管理，部分接口带 ID 或查询参数 |
| GET / POST / PUT | `/clazzs` | 班级查询与维护 |
| DELETE | `/clazzs/{id}` | 删除班级 |
| GET / POST / PUT | `/students` | 学员查询与维护 |
| PUT | `/students/violation/{id}/{score}` | 学员违纪处理 |
| GET | `/report/empJobData` | 员工职位统计 |
| GET | `/report/empGenderData` | 员工性别统计 |
| GET | `/report/studentDegreeData` | 学员学历统计 |
| GET | `/report/studentCountData` | 班级学员数量统计 |
| POST | `/upload` | 文件上传 |

完整参数、请求体和返回结构以 [controller](src/main/java/com/itheima/controller) 为准。当前过滤器/拦截器代码从 `token` 请求头读取令牌，实际生效范围请查看注册配置。

## 使用说明与限制

- 先准备数据库中的员工记录，再调用登录接口获取令牌；README 不提供固定账号密码。
- 部门与员工、班级与学员存在关联，删除前需满足当前业务校验。
- 上传接口依赖 OSS 配置和可用凭据。
- Cookie、Session 等学习接口不等于业务登录流程。
- [测试目录](src/test/java/com/itheima)中部分测试依赖数据库或外部环境，不应直接针对生产数据运行。

本次只补充文档，未修改代码或数据库，未执行构建及接口测试。
