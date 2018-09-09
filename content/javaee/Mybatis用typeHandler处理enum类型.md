---
title: "Mybatis用typeHandler处理enum类型"
date: 2018-09-09T17:26:44+08:00
lastmod: 2018-09-09T17:26:44+08:00
#tags : [ "dev", "hugo", "hyde-hyde"]
#categories : [ "dev" ]
layout: post
type:  "post"
highlight: false
draft: false
---

```

如果想使用mybatis自带的枚举类处理，有2种方式，一个是EnumTypeHandler，一个是EnumOrdinalTypeHandler。
区别如下:
EnumTypeHandler直接存储name值。它是mybatis默认的枚举类型转换器。
	EnumOrdinalTypeHandler存储enum类里的序号值，此时数据库表字段一般对应用smallint/int类型的处理。
使用的时侯很简单，在insert或者update语句块，或者resultMap字段等地方指定相应的typeHandler即可。

<insert id="insertUser" parameterType="User">
    insert into user(id,userName,status)
values(#{id},#{userName},#{status,typeHandler=org.apache.ibatis.type.EnumOrdinalTypeHandler})
</insert>
或
<resultMap id="BaseResultMap" type="User">
    <id column="id" property="userId" jdbcType="INTEGER" />
    <result column="status" property="status" typeHandler="org.apache.ibatis.type.EnumOrdinalTypeHandler"/>
</resultMap>


不过大多数情况下，想存储的并不是枚举的顺序号，而是存储自定义的id值，这时我们就需要自己写enum的处理类。
若程序中原来有一个枚举类，name是长沙、株洲、湘潭三地，value采用身份证上的地区编号。
public enum CityTest {
    ChangSha(4301),Zhuzhou(4302),Xiangtan(4303);

    int value;
    private CityTest(int value){
        this.value=value;
    }
    public int getValue() {
        return this.value;
    }
    /*方法Value2CityTest是为了typeHandler后加的*/
    public static CityTest Value2CityTest(int value) {
        for (CityTest citytest : CityTest.values()) {
            if (citytest.value == value) {
                return citytest;
            }
        }
        throw new IllegalArgumentException("无效的value值: " + value + "!");
    }
}
根据上一章《【MyBatis学习16】自定义类型处理器typeHandlers介绍 》我们知道，在实现typeHandler时，需要用到两个方法
一个方法将javaType转成jdbcType,另一个方法将jdbcType转成javaType。
将javaType转成jdbcType我们可以用CityTest中的getValue()，获取当前CityTest对象的value值即可。
那么现在要增加一个方法，将jdbcType转成javaType（这样就定义了方法的入参和返回类型）。因此我们增加了Value2CityTest方法。
再定义转换器typeHandler的实现类：
public class CityTestTypeHandler extends BaseTypeHandler<CityTest> {
    @Override
    public CityTest getNullableResult(ResultSet rSet, String columnName)
            throws SQLException {
        return CityTest.Value2CityTest(rSet.getInt(columnName));
    }

    @Override
    public CityTest getNullableResult(ResultSet rSet, int columnIndex)
            throws SQLException {
        return CityTest.Value2CityTest(rSet.getInt(columnIndex));
    }

    @Override
    public CityTest getNullableResult(CallableStatement cStatement, int columnIndex)
            throws SQLException {
        return CityTest.Value2CityTest(cStatement.getInt(columnIndex));
    }

    @Override
    public void setNonNullParameter(PreparedStatement pStatement, int index,
            CityTest citytest, JdbcType jdbcType) throws SQLException {
        pStatement.setInt(index, citytest.getValue());
}
}
接下来在myBatis配置文件中注册
<!-- 注册自定义类型处理器 -->
<typeHandlers>
<typeHandler handler="twm.mybatisdemo.type.CityTestTypeHandler"/>
</typeHandlers>
然后就可以使用了。
补充：
实际使用中并不一定要求显示声明
typeHandler=org.apache.ibatis.type.CityTestTypeHandler，系统会根据类型以及注册的typeHandler自动识别。


转载：http://blog.csdn.net/soonfly/article/details/67640580

```
