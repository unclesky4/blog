---
title: "JsonConfig的使用"
date: 2018-09-09T17:24:37+08:00
lastmod: 2018-09-09T17:24:37+08:00
#tags : [ "dev", "hugo", "hyde-hyde"]
#categories : [ "dev" ]
layout: post
type:  "post"
highlight: false
draft: false
---

```

我们通常对一个Json串和Java对象进行互转时，经常会有选择性的过滤掉一些属性值，而json-lib包中的JsonConfig为我们提供了这种 功能，具体实现方法有以下几种。(1)建立JsonConfig实例，并配置属性排除列表,(2)用属性过滤器,(3)写一个自定义的 JsonBeanProcessor.

1. 实现JSONString接口的方法

public class Person implements JSONString {
private String name;
private String lastname;
private Address address;

// getters & setters

public String toJSONString() {
return "{name:'"+name+"',lastname:'"+lastname+"'}";
}
}


2.第二种方法通过jsonconfig实例，对包含和需要排除的属性进行方便的添加或删除

public class Person {
private String name;
private String lastname;
private Address address;

// getters & setters
}

JsonConfig jsonConfig = new JsonConfig();
jsonConfig.setExclusions( new String[]{"address"});
Person bean = new Person("jack","li");
JSON json = JSONSerializer.toJSON(bean, jsonConfig);

3. 使用propertyFilter可以允许同时对需要排除的属性和类进行控制，这种控制还可以是双向的，也可以应用到json字符串到java对象

public class Person {
private String name;
private String lastname;
private Address address;

// getters & setters
}

JsonConfig jsonConfig = new JsonConfig();
jsonConfig.setJsonPropertyFilter( new PropertyFilter(){

public boolean apply(Object source/* 属性的拥有者 */ , String name /*属性名字*/ , Object value/* 属性值 */ ){
// return true to skip name
return source instanceof Person && name.equals("address");
}
});
Person bean = new Person("jack","li");
JSON json = JSONSerializer.toJSON( bean, jsonConfig )

4. 最后来看JsonBeanProcessor,这种方式和实现JsonString很类似，返回一个代表原来的domain类的合法JSONObject

public class Person {
private String name;
private String lastname;
private Address address;

// getters & setters
}

JsonConfig jsonConfig = new JsonConfig();
jsonConfig.registerJsonBeanProcessor( Person.class, new JsonBeanProcessor(){

public JSONObject processBean( Object bean, JsonConfig jsonConfig ){
if(!(bean instanceof Person)){
return new JSONObject(true);
}
Person person = (Person) bean;
return new JSONObject() .element( "name", person.getName()) .element( "lastname", person.getLastname());
}
});

Person bean = new Person("jack","li");
JSON json = JSONSerializer.toJSON( bean, jsonConfig );
```
