---
title: SpringBoot与JPA
date: 2018-07-28 18:59:56
tags: JPA
---

前几日用SpringBoot与Mybatis在工程中写一些查询，由于是第一次使用加上需要配置的文件又多，测试就没有通过。后来干脆改用JPA，发现JPA也太简单好用了，特写此博客记录JPA。

##### 1.pom.xml文件下添加如下依赖，引入spring-boot-jpa的jar包，以及mysql驱动


``` 
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
</dependency>
```
<!--more-->
##### 2.在/src/main/resources/application.properties中配置spring datasource及hibernate方言(Spring boot 默认使用hibernate作为JPA的实现)
``` 
spring.datasource.url=jdbc:mysql://localhost:3306/test?useSSL=false
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.tomcat.max-active=100
spring.datasource.tomcat.max-idle=200
spring.datasource.tomcat.initialSize=20
spring.jpa.database-platform=org.hibernate.dialect.MySQL5Dialect
```
##### 3.定义Entity和Repository(接口)
Entity--Account实体类如下:
@Entity会被spring扫描并加载，

  @Id注解在主键上
@GeneratedValue 用于标注主键的生成策略，通过strategy 属性指定。默认情况下，JPA 自动选择一个最适合底层数据库的主键生成策略：SqlServer对应identity，MySQL 对应 auto increment。 
  @Entity说明这个class是实体类，并且使用默认的orm规则，即class名即数据库表中表名，class字段名即表中的字段名
如果想改变这种默认的orm规则，就要使用@Table来改变class名与数据库中表名的映射规则，@Column来改变class中字段名与db中表的字段名的映射规则
``` 
@Data
@Entity
public class Account
{
    @Id
    private String id;
    private String account;
    @Column(name = "call_phone")
    private String phone;
    @Column(name = "nick_name")
    private String nickname;
    private String password;
    private String salt;
    private int userType;
    private String createUser;
    private Timestamp createTime;
    private int state;
}
```
Repository如下：
``` 
@Repository
public interface AccountRepository extends JpaRepository<Account, String>
{
    Account findOneByAccount(String account);
}
```
##### 4.在其他@Component中调用

``` 
@RestController
@RequestMapping("/subsystem")
public class SubsystemAuthorityService
{
    @Autowired
    AccountRepository accountRepository;

    @PostMapping(path = "/admin/info")
    public String getAdminInfo(String currentAccount)
    {
        Account account = accountRepository.findOneByAccount(currentAccount);
        System.out.println(account);
        return account.toString();
    }
}
```

