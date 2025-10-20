# course
项目说明：
  本项目是一个基于 Spring Boot 框架开发的课程选课管理系统，主要用于实现校园内课程与学生的管理，以及学生选课、退课等功能。通过该系统，可进行课程的创建、更新、查询，学生的创建、查询、删除，还有学生选课、退课等操作，同时对各类操作设置了相应的业务规则校验，如课程容量限制、学生重复选课限制、邮箱格式校验等。

项目运行：
  1.环境要求
    JDK 17 及以上版本。
    Maven 3.6 及以上版本。
  2.步骤
    克隆项目到本地：git clone [项目仓库地址]（若有仓库），或直接将项目代码复制到本地目录。
    进入项目根目录，执行 Maven 构建命令：mvn clean package，该命令会编译项目并生成可执行的 JAR 包。
    运行项目：java -jar target/[项目JAR包名称].jar（将[项目JAR包名称]替换为实际生成的 JAR 包名），也可在 IDE（如 IntelliJ IDEA、Eclipse）中直接运行项目的主启动类（CampusCourseSystemApplication）。

API接口列表：
  课程管理接口：
    POST	/api/courses	创建课程
    POST	/api/courses/capacity1	创建容量为 1 的课程
    GET	/api/courses	查询所有课程
    PUT	/api/courses/{courseId}	更新课程信息
  选课管理接口：
    POST	/api/enrollments	学生选课
    POST	/api/enrollments/capacity1	学生选容量为 1 的课程
    POST	/api/enrollments/capacity1/fail	学生选容量为 1 的课程未选上（容量已满场景）
    POST	/api/enrollments/duplicate	学生重复选课
    GET	/api/enrollments/student/{studentId}	查询学生的所有选课记录
    DELETE	/api/enrollments/{enrollmentId}	退课
  学生管理接口：
    POST	/api/students	创建学生
    POST	/api/students/create2	创建学生（用于测试等场景）
    GET	/api/students/{studentId}	查询单个学生详情
    DELETE	/api/students/{studentId}	删除无选课记录的学生
    POST	/api/students/emailError	测试邮箱格式错误场景

测试说明：
  测试工具：使用 Apifox 进行接口测试，通过该工具可方便地构造请求、发送请求并查看响应结果。
  测试场景及预期结果：
    1.课程管理测试
      创建课程：发送 POST 请求，传入合法课程信息，预期返回创建成功的课程信息，状态码 200。
      查询所有课程：发送 GET 请求，预期返回系统中所有课程列表，状态码 200。
      更新课程信息：发送 PUT 请求，传入合法更新数据，预期返回更新后的课程信息，状态码 200；若传入无效课程 ID，预期返回课程不存在等错误信息。
    2.学生管理测试
      创建学生：发送 POST 请求，传入合法学生信息（含正确邮箱格式），预期返回创建成功的学生信息，状态码 200；若 传入错误邮箱格式，预期返回邮箱格式错误提示，状态码 400 左右。
      查询单个学生详情：发送 GET 请求，传入存在的学生 ID，预期返回该学生详情，状态码 200；若传入不存在的学生 ID，预期返回学生不存在提示。
      删除无选课记录的学生：发送 DELETE 请求，传入无选课记录的学生 ID，预期删除成功，状态码 200；若学生有选课记录，预期删除失败，返回相应提示。
    3.选课管理测试
      学生选课：发送 POST 请求，传入合法课程 ID 和学生 ID，预期选课成功，课程已选人数加 1，状态码 200。
      选容量为 1 的课程：先确保课程容量为 1 且未被选，发送选课请求，预期选课成功；若课程已被选，再次发送请求，预期选课失败，返回容量已满提示。
      学生重复选课：对同一学生同一课程多次发送选课请求，除第一次外，预期返回重复选课失败提示。
      退课：先完成选课操作，再发送 DELETE 请求，传入选课记录 ID，预期退课成功，课程已选人数减 1，选课记录被删除，状态码 200。
