### 系统介绍

基于SpringBoot和Vue实现的在线考试系统采用前后端分离的架构方式，系统共有三个角色：学生、教师和管理员。学生实现了注册/登录、主页、讨论管理、试卷中心、刷题中心、考试记录、错题本、我的证书、登录日志、个人中心等功能模块，教师实现了登录、主页、用户管理、班级管理、讨论管理、考试管理、题库管理、试题管理、成绩分析、阅卷管理、公告管理、登录日志等功能模块，管理员实现了登录、主页、用户管理、班级管理、考试管理、题库管理、试题管理、证书管理、公告管理、登录日志等功能模块。

### 技术选型

开发工具：idea2020.3+Webstorm2020.3

运行环境：jdk1.8+maven3.6.0+mysql8.0+nodejs14.21.3

服务端技术：springboot+mybatis-plus+springsecurity+fastjson

前端技术：html+css+vue+axios+element-ui+echarts

### 成果展示

学生->登录
<img width="1903" height="980" alt="登录" src="https://github.com/user-attachments/assets/c83e7319-01e4-458b-8414-dc0639f4e622" />

学生->注册
<img width="1906" height="964" alt="注册" src="https://github.com/user-attachments/assets/e62233cb-cb5e-47a5-bc98-9a472802f9a7" />

学生->主页
<img width="1905" height="1027" alt="学生-主页" src="https://github.com/user-attachments/assets/6a403506-9c35-4f00-8aa7-6b26b8463cdc" />

学生->讨论管理
<img width="1905" height="1021" alt="学生-讨论管理" src="https://github.com/user-attachments/assets/e206a8ca-c3f3-4ac6-b432-f045066481b0" />

学生->讨论详情
<img width="1898" height="1026" alt="学生-讨论详情" src="https://github.com/user-attachments/assets/85684683-a0f0-4e69-a50e-bfe4bc4bd4cb" />

学生->试卷中心
<img width="1904" height="1027" alt="学生-试卷中心" src="https://github.com/user-attachments/assets/59def748-849a-4ea7-9d60-06e761fc6d62" />

学生->开始考试
<img width="1878" height="1008" alt="学生-开始考试" src="https://github.com/user-attachments/assets/13f3f1f3-95f8-4db1-a785-c11cfca4c1a4" />

学生->交卷
<img width="1901" height="1022" alt="学生-交卷" src="https://github.com/user-attachments/assets/1c013e41-9218-427f-a419-675413c32220" />

学生->刷题中心
<img width="1908" height="1030" alt="学生-刷题中心" src="https://github.com/user-attachments/assets/8b0e13c8-6869-4c16-ab2c-80a85235c421" />

学生->开始刷题
<img width="1873" height="1006" alt="学生-开始刷题" src="https://github.com/user-attachments/assets/5b38c423-0a6e-4cb6-a9fa-39a994017f21" />

学生->结束刷题
<img width="1882" height="1022" alt="学生-结束刷题" src="https://github.com/user-attachments/assets/0416d7a4-48e7-445c-890e-223c2082eaf6" />

学生->考试记录
<img width="1902" height="1030" alt="学生-考试记录" src="https://github.com/user-attachments/assets/528feb46-690e-4857-b565-526fb1e30cea" />

<img width="1874" height="1031" alt="学生-考试记录2" src="https://github.com/user-attachments/assets/51bd384c-e825-4419-a78c-3b7874b34738" />

学生->错题本

学生->我的证书 输入图片说明

学生->证书预览 输入图片说明

教师->主页 输入图片说明

教师->用户管理 输入图片说明

教师->班级管理 输入图片说明

教师->讨论管理 输入图片说明

教师->考试管理 输入图片说明

教师->题库管理 输入图片说明

教师->试题管理 输入图片说明

教师->成绩分析 输入图片说明

管理员->主页 输入图片说明

管理员->用户管理 输入图片说明

管理员->班级管理 输入图片说明

管理员->证书管理 输入图片说明

管理员->公告管理 输入图片说明

### 源码展示

@Api(tags = "考试管理相关接口")

@RestController

@RequestMapping("/api/exams")

public class ExamController {

@Resource

private IExamService examService;

//创建考试

@ApiOperation("创建考试")

@PostMapping

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin')")

public Result<String> createExam(@Validated @RequestBody ExamAddForm examAddForm) {

    return examService.createExam(examAddForm);
    
}

//开始考试

@ApiOperation("开始考试")

@GetMapping("/start")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin','role_student')")

public Result<String> startExam(@RequestParam("examId") @NotNull Integer examId) {

    return examService.startExam(examId);
    
}

//删除考试
@ApiOperation("删除考试")

@DeleteMapping("/{ids}")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin')")

public Result<String> deleteExam(@PathVariable("ids") @Pattern(regexp = "^\\d+(,\\d+)*$|^\\d+$") String ids) {

    return examService.deleteExam(ids);
    
}

//获取考试题目id列表

@ApiOperation("获取考试题目id列表")

@GetMapping("/question/list/{examId}")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin','role_student')")

public Result<ExamQuestionListVO> getQuestionList(@PathVariable("examId") @NotBlank Integer examId) {

    return examService.getQuestionList(examId);
    
}

//获取单题信息
@ApiOperation("获取单题信息")

@GetMapping("/question/single")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin','role_student')")

public Result<ExamQuDetailVO> getQuestionSingle(@RequestParam("examId") Integer examId,
                                                @RequestParam("questionId") Integer questionId) {
                                                
    return examService.getQuestionSingle(examId, questionId);
    
}

//题目汇总

@ApiOperation("题目汇总")

@GetMapping("/collect/{id}")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin','role_student')")

public Result<List<ExamQuCollectVO>> getCollect(@PathVariable("id") @NotNull Integer examId) {

    return examService.getCollect(examId);
    
}

//获取考试详情信息

@ApiOperation("获取考试详情信息")

@GetMapping("/detail")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin','role_student')")

public Result<ExamDetailVO> getDetail(@RequestParam("examId") @NotBlank Integer examId) {

    return examService.getDetail(examId);
    
}

//填充答案

@ApiOperation("填充答案")

@PostMapping("/full-answer")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin','role_student')")

public Result<String> addAnswer(@Validated @RequestBody ExamQuAnswerAddForm examQuAnswerForm) {

    return examService.addAnswer(examQuAnswerForm);
    
}

//交卷操作

@ApiOperation("交卷操作")

@GetMapping(value = "/hand-exam/{examId}")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin','role_student')")

public Result<ExamQuDetailVO> handleExam(@PathVariable("examId") @NotNull Integer examId) {

    return examService.handExam(examId);
    
}

//查看详情

@ApiOperation("查看考试详情")

@GetMapping(value = "/details/{examId}")

@PreAuthorize("hasAnyAuthority('role_teacher','role_admin','role_student')")

public Result<List<ExamRecordDetailVO>> details(@PathVariable("examId") @NotNull Integer examId) {

    return examService.details(examId);
    
}

}

### 账号地址及其它说明

1、地址说明

登录页：localhost:9527

2、账号说明

学生：wangwu/123456

教师：liufang/123456

管理员：admin/123456

3、目录结构展示

输入图片说明

4、项目结构展示

输入图片说明

5、运行步骤

1）创建数据库、导入sql脚本

2）修改application.yml中的数据库配置文件，启动服务端

3）在frontend目录下打开cmd，执行npm install或者yarn install下载依赖

4）下载完毕后启动前端npm run dev，访问端口

### 获取方式(可远程调试)

访问链接(在浏览器中手动输入下图中的地址)：

输入图片说明

若资源获取失败，可添加happy35596339(vx)或1204901965(qq)进行交流
