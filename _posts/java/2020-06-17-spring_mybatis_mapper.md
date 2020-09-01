---
layout: post
title: Spring과 MyBatis 3.x 연동 - Mapper
categories : java
tags : java Spring MyBatis
comments : true
---

## XML에서 SqlSessionFactory 빌드하기

모든 마이바티스 애플리케이션은 SqlSessionFactory 인스턴스를 사용한다.


```xml
<bean id = "sqlSessionFactory" class="org.mybatis.spring.SqlSessionBean">
  <property name = "datasource" ref="dataSource" />
  <property name = "mapperLocations" value = "{mapper_xml_path}" />
  <property name = "configLocation" value = "{mybatis_config_xml_path}" />
</bean>
```

쿼리 매핑을 위한 XML 파일을 검색하여 자동 추가하도록 설정한다.

## MyBatis와 Spring DAO 연동하기

mybatis는 Spring의 DAO (데이터베이스 작업을 담당하는 객체; Data Access Object) 객체와 mapping될 수 있다.

database 작업을 하는 구문은 XML이나 어노테이션을 사용하여 정의할 수 있다. 마이바티스가 제공하는 대부분의 기능은 XML을 통한 매핑 기법을 사용한다.

mapper xml을 작성하기 전, DAO 인터페이스를 생성한다.

```java
public interface ExampleDao {
  public String selectBlog(@Param("userId") String userid);
}
```

DAO 클래스를 일단 인터페이스로 선언하고 그 내에는 데이터베이스 작업을 함수 단위로 분리하여 parameter와 함수명을 명시해준다. 그리고 mapper xml을 다음과 같이 생성해준다.

```xml
<?xml version "1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="org.mybatis.example.dao.ExampleDao" /> <!--dao_class_path-->
  <select id="selectBlog" resultType="Blog">
    select * from Blog where id = #{userId}
  </select>
</mapper>
```

`napespace`에는 패키지명을 포함한 DAO 인터페이스의 경로를 정확히 써 주고, 각각의 쿼리문들의 id는 DAO 인터페이스에서 정의한 메소드명과 일치해야 한다.

그 후, 컨트롤러에서 sqlSession빈을 가져다 쓰면 된다.

```java
@Controller
public class ExampleController {
  @Autowired
  private SqlSession sqlSession;

  @RequestMapping("/test/mybatis/{userId}")
  public String testMybatis(@PathVariable("userId") String userId) {
    ExampleDao exampleDao = sqlSession.getMapper(ExampleDao.class);
    return exampleDao.getUserName(userId);
  }
}
```

위와 같이 DAO를 꺼내 쓰기만 하면 실제 쿼리가 XML에 정의된 쿼리들과 자동으로 매핑되어 결과값이 반환된다.

#### 네임스페이스(Namespace)
네임스페이스는 3.x 이전 버전에서는 선택사항이었다. 하지만 이제는 패키지 경로를 포함한 전체 이름을 가진 구문을 구분하기 위해 필수로 사용해야 한다.

네임스페이스는 인터페이스 바인딩을 가능하게 한다. 네임스페이스를 사용하고 자바 패키지의 네임스페이스를 두면 코드가 깔끔해지고 마이바티스의 사용성이 크게 향상된다.
