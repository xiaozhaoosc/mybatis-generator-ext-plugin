# mybatis-generator-ext-plugin
对mybatis-generator插件的扩展

## 注意事项
- 编码问题
```xml
<!-- 指定生成的java文件的编码,没有直接生成到项目时中文可能会乱码 -->
<property name="javaFileEncoding" value="UTF-8"/>
```
- 无法获取注释问题，需要在<jdbcConnection>标签中添加属性
```xml
<!-- 针对oracle数据库 -->
 <property name="remarksReporting" value="true"></property>
<!-- 针对mysql数据库 -->
 <property name="useInformationSchema" value="true"></property>
```
- 生成的mappper.xml是否需要些基本的操作方法
```xml
<table schema="" tableName="xxx" domainObjectName="xxx"
    enableCountByExample="false" enableUpdateByExample="false"
    enableDeleteByExample="false" enableSelectByExample="false"
    selectByExampleQueryId="false">
</table>
```
