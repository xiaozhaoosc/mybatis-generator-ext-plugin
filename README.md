# mybatis-generator-ext-plugin
对mybatis-generator插件的扩展

## 使用
1. 在pom.xml中的<build>---><plugins>中添加包引用，再添加generatorConfig.xml配置；如
```xml

	<build>
		<plugins>
			<plugin>
				<groupId>org.mybatis.generator</groupId>
				<artifactId>mybatis-generator-maven-plugin</artifactId>
				<version>1.3.5</version>
				<configuration>
					<verbose>true</verbose>
					<overwrite>false</overwrite>
				</configuration>
				<dependencies>
					<dependency>
						<groupId>com.ken.utils</groupId>
						<artifactId>mybatis-generator-ext-plugin</artifactId>
						<version>1.0.0-SNAPSHOT</version>
					</dependency>
				</dependencies>
			</plugin>
		</plugins>
	</build>
```
## generatorConfig.xml示例
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
		PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
		"http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>

	<!--导入属性配置-->
	<!--<properties resource="jdbc.properties"></properties>-->
	<classPathEntry location="xxx\ojdbc14.jar"/>

	<context id="DB2Tables" targetRuntime="MyBatis3">

        <!-- 分页相关 -->
        <plugin type="org.mybatis.generator.plugins.RowBoundsPlugin" />
        <!-- 带上序列化接口 -->
        <plugin type="org.mybatis.generator.plugins.SerializablePlugin" />
        <!-- 自定义的注释生成插件-->
        <plugin type="com.ken.utils.mybatis.generator.plugins.RemarkPlugin">
            <!-- 抑制警告 -->
            <property name="suppressTypeWarnings" value="true" />
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
            <!-- 是否生成注释代时间戳-->
            <property name="suppressDate" value="true" />
        </plugin>
        <!-- 整合lombok-->
        <!--<plugin type="com.ken.utils.mybatis.generator.plugins.LombokPlugin" >-->
            <!--<property name="hasLombok" value="true"/>-->
        <!--</plugin>-->
		<!-- 生成注释为false 不生成为true 【不生成注释时会被重复写入导致报错】 -->
		<!--<commentGenerator>-->
			<!--<property name="suppressAllComments" value="false"/>-->
		<!--</commentGenerator>-->



		<!--jdbc的数据库连接 -->
		<jdbcConnection driverClass="oracle.jdbc.driver.OracleDriver"
						connectionURL="jdbc:oracle:thin:@xxx:1521:orcl" userId="xxx"
						password="xxx">
			<property name="remarksReporting" value="true"></property>
		</jdbcConnection>
		<!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
			NUMERIC 类型解析为java.math.BigDecimal -->
		<javaTypeResolver >
			<property name="forceBigDecimals" value="false" />
		</javaTypeResolver>

		<!-- Model模型生成器,用来生成含有主键key的类，记录类 以及查询Example类
            targetPackage     指定生成的model生成所在的包名
            targetProject     指定在该项目下所在的路径
        -->
		<!--<javaModelGenerator targetPackage="xxx.pojo" targetProject=".\src\main\java">-->
		<javaModelGenerator targetPackage="xxx.generator.pojo" targetProject="./src/main/java">
			<!-- 是否允许子包，即targetPackage.schemaName.tableName -->
			<property name="enableSubPackages" value="false"/>
			<!-- 是否对model添加 构造函数 -->
			<property name="constructorBased" value="true"/>
			<!-- 是否对类CHAR类型的列的数据进行trim操作 -->
			<property name="trimStrings" value="true"/>
			<!-- 建立的Model对象是否 不可改变  即生成的Model对象不会有 setter方法，只有构造方法 -->
			<property name="immutable" value="false"/>
		</javaModelGenerator>

		<!--mapper映射文件生成所在的目录 为每一个数据库的表生成对应的SqlMap文件 -->
		<sqlMapGenerator targetPackage="mappers" targetProject="./src/main/resources">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false"/>
		</sqlMapGenerator>

		<!-- targetPackage：mapper接口dao生成的位置 -->
		<javaClientGenerator type="XMLMAPPER" targetPackage="xxx.generator.dao" targetProject="./src/main/java">
			<!-- enableSubPackages:是否让schema作为包的后缀 -->
			<property name="enableSubPackages" value="false" />
		</javaClientGenerator>


		<table schema="" tableName="xxx" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>


	</context>
</generatorConfiguration>
```
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
