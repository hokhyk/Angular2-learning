cas配置全攻略
经过将近两天的测试，参考众多网友的贡献，终于完成了对cas的主要配置和测试，现记录如下

基本需求：

1.cas server-3.4.5,casclient-3.2（官方版本），均可在cas官方网站下载，http://www.jasig.org

2.使用低成本的http协议进行传输，俺买不起ssl证书

3.通过jdbc进行用户验证

4.需要通过casserver提供除登录用户名以外的附加信息

参考资料：

1.cas官方网站的用户帮助手册和wiki

2.网友“城市猎人”的blog，http://yuzhwe.javaeye.com/blog/830143

3.网友“悟空悟道”的blog，http://llhdf.javaeye.com/blog/764385

4.其他网友贡献的相关的blog，都是通过google出来，就不一一列出了，一并致谢！！！

好了，下面进入正题，如果您不想测试中出现异常情况，或是获取不到相关数据，请关注文中的红色字体部分。

（1）使用http协议的设置，如果您也像我一样，买不起ssl数字证书，对安全的要求也不是特别的搞，下面的配置就可以帮助解决这个问题：

在cas-server-webapp中的/WEB-INF/spring-configuration/ticketGrantingTicketCookieGenerator.xml文件中有如下配置

<bean id="ticketGrantingTicketCookieGenerator" class="org.jasig.cas.web.support.CookieRetrievingCookieGenerator"
  p:cookieSecure="true"     //默认为true，使用https,如果只需要http，修改为false即可
  p:cookieMaxAge="-1"
  p:cookieName="CASTGC"
  p:cookiePath="/cas" />

 （2）使用jdbc数据源进行用户认证，需要修改cas的authenticationHandlers方式，在文件/WEB-INF/deployerConfigContext.xml有如下配置：

<property name="authenticationHandlers">
   <list>
    <!--
     | This is the authentication handler that authenticates services by means of callback via SSL, thereby validating
     | a server side SSL certificate.
     +-->
    <bean class="org.jasig.cas.authentication.handler.support.HttpBasedServiceCredentialsAuthenticationHandler"
     p:httpClient-ref="httpClient" />
    <!--
     | This is the authentication handler declaration that every CAS deployer will need to change before deploying CAS 
     | into production.  The default SimpleTestUsernamePasswordAuthenticationHandler authenticates UsernamePasswordCredentials
     | where the username equals the password.  You will need to replace this with an AuthenticationHandler that implements your
     | local authentication strategy.  You might accomplish this by coding a new such handler and declaring
     | edu.someschool.its.cas.MySpecialHandler here, or you might use one of the handlers provided in the adaptors modules.
     +-->
    <!--<bean class="org.jasig.cas.authentication.handler.support.SimpleTestUsernamePasswordAuthenticationHandler" />-->
     <bean  class="org.jasig.cas.adaptors.jdbc.QueryDatabaseAuthenticationHandler">
         <property name="dataSource" ref="dataSource" />
        <property name="sql" value="select password from userInfo where username=? and enabled=true" />
         //用户密码编码方式
         <property name="passwordEncoder"
           ref="passwordEncoderBean"/>
         </bean>  
   </list>
  </property>

该属性中的list只要用一个认证通过即可，建议将红色部分放在第一位，如果确认只用jdbc一种方式，其他认证方式均可删除。另外需要在在文件中添加datasoure和passordEncoder两个bean，如下

<!-- Data source definition -->
 <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
  <property name="driverClassName">
    <value>com.mysql.jdbc.Driver</value>
  </property>
  <property name="url">
    <value>jdbc:mysql://localhost:3306/test?useUnicode=true&amp;characterEncoding=utf-8</value>    //如果使用mysql数据库，应该加上后面的编码参数，否则可能导致客户端对TGT票据无法识别的问题
  </property>
  <property name="username"><value>root</value></property>
  <property name="password"><value>password</value></property>
 </bean>
 <bean id="passwordEncoderBean" class="org.jasig.cas.authentication.handler.DefaultPasswordEncoder">
        <constructor-arg value="SHA1" />  //cas 
server默认支持MD5和SHA1两种编码方式，如果需要其他的编码方式例如SHA256,512等，可自行实现org.jasig.cas.authentication.handler.PasswordEncoder接口
    </bean>

附加备注：如果您是使用cas server的源码自行编译的话，需要在cas-server-web模块的pom.xml中添加如下模块的依赖：

<dependency>
       <groupId>${project.groupId}</groupId>
       <artifactId>cas-server-support-jdbc</artifactId>
       <version>${project.version}</version>
  </dependency>  

并添加对应数据库的jdbc的jar包。

（3）让cas server提供更多的用户数据共客户端使用

通过测试，由于cas的代码更新过程中的变化较大，所以包兼容的问题好像一直存在，在测试中我就碰到过，花费时间比较多，建议同学们在使用过程中使用官方的最新的发布版本。在我使用的这个版本中，请参考前面的关于server和client端的版本说明，应该没有包冲突的问题，测试通过。下面进行配置，配置文件：/WEB-INF/deployerConfigContext.xml
<property name="credentialsToPrincipalResolvers">
   <list>
       <!--<bean class="org.jasig.cas.authentication.principal.UsernamePasswordCredentialsToPrincipalResolver" />-->
    <!-- modify on 2011-01-18,add user info -->
    <bean class="org.jasig.cas.authentication.principal.UsernamePasswordCredentialsToPrincipalResolver" > 
      <property name="attributeRepository" >   //为认证过的用户的Principal添加属性
      <ref local="attributeRepository"/>
     </property> 
    </bean>
      <bean
     class="org.jasig.cas.authentication.principal.HttpBasedServiceCredentialsToPrincipalResolver" />
   </list>
  </property>
 修改该文件中默认的 attributeRepositorybean配置
<!-- 在这里配置获取更多用户的信息 -->
 <bean id="attributeRepository" class="org.jasig.services.persondir.support.jdbc.SingleRowJdbcPersonAttributeDao">
  <constructor-arg index="0" ref="dataSource" />
  <constructor-arg index="1" value="select id as UId, password_hint as ph from userInfo where username=? and enabled=true" />
  <property name="queryAttributeMapping">
   <map>
   <entry key="username" value="uid"/><!-- 这里必须这么写，系统会自己匹配，貌似和where语句后面的用户名字段的拼写没有什么关系 -->
   </map>
  </property>
   <!-- 要获取的属性在这里配置 -->
  <property name="resultAttributeMapping">
   <map>
   <entry key="UId" value="userId" /> //key为对应的数据库字段名称，value为提供给客户端获取的属性名字，系统会自动填充值
   <entry key="ph" value="passwordHint" />   
   </map>
  </property>
</bean> 
备注：网上有很多的关于这个的配置，但是如果您使用的是我提供的版本或是高于这个版本，就应该象上面这样配置，无用质疑，网上大部分的配置都是基于
person-directory-impl,person-directory-api 
1.1左右的版本，而最新的cas使用的是1.5的版本，经过查看源代码和api docs确定最新版本的属性参数如上配置。

修改该xml文件中最后一个默认的serviceRegistryDao bean中的属性全部注释掉，或者删除，
这个bean中的RegisteredServiceImpl的ignoreAttributes属性将决定是否添加attributes属性内容，默认为false:不添加，只有去掉这个配置，
cas server才会将获取的用户的附加属性添加到认证用的Principal的attributes中去，我在这里犯过这样的错误，最后还是通过跟踪源码才发现的。
<bean
  id="serviceRegistryDao"
        class="org.jasig.cas.services.InMemoryServiceRegistryDaoImpl">
        <!--
            <property name="registeredServices">
                <list>
                    <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                        <property name="id" value="0" />
                        <property name="name" value="HTTP" />
                        <property name="description" value="Only Allows HTTP Urls" />
                        <property name="serviceId" value="http://**" />
                    </bean>

                    <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                        <property name="id" value="1" />
                        <property name="name" value="HTTPS" />
                        <property name="description" value="Only Allows HTTPS Urls" />
                        <property name="serviceId" value="https://**" />
                    </bean>

                    <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                        <property name="id" value="2" />
                        <property name="name" value="IMAPS" />
                        <property name="description" value="Only Allows HTTPS Urls" />
                        <property name="serviceId" value="imaps://**" />
                    </bean>

                    <bean class="org.jasig.cas.services.RegisteredServiceImpl">
                        <property name="id" value="3" />
                        <property name="name" value="IMAP" />
                        <property name="description" value="Only Allows IMAP Urls" />
                        <property name="serviceId" value="imap://**" />
                    </bean>
                </list>
            </property>-->
           </bean>

 修改WEB-INF\view\jsp\protocol\2.0\casServiceValidationSuccess.jsp文件，如下：

<%@ page session="false"%>
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/functions" prefix="fn"%>
<cas:serviceResponse xmlns:cas='http://www.yale.edu/tp/cas'>
 <cas:authenticationSuccess>
  <cas:user>${fn:escapeXml(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.id)}</cas:user>
  <c:if test="${not empty pgtIou}">
   <cas:proxyGrantingTicket>${pgtIou}</cas:proxyGrantingTicket>
  </c:if>
  <c:if test="${fn:length(assertion.chainedAuthentications) > 1}">
   <cas:proxies>
    <c:forEach var="proxy" items="${assertion.chainedAuthentications}"
     varStatus="loopStatus" begin="0"
     end="${fn:length(assertion.chainedAuthentications)-2}" step="1">
     <cas:proxy>${fn:escapeXml(proxy.principal.id)}</cas:proxy>
    </c:forEach>
   </cas:proxies>
  </c:if>
   <c:if
   test="${fn:length(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes)

> 
0}">
   <cas:attributes>
    <c:forEach 
var="attr"
     items="${assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes}"
     varStatus="loopStatus" 
begin="0"
     end="${fn:length(assertion.chainedAuthentications[fn:length(assertion.chainedAuthentications)-1].principal.attributes)-1}"
     step="1">
     <cas:${fn:escapeXml(attr.key)}>${fn:escapeXml(attr.value)}</cas:${fn:escapeXml(attr.key)}>
    </c:forEach>
   </cas:attributes>
  </c:if>
 </cas:authenticationSuccess>
</cas:serviceResponse>
客户端配置:
1.过滤器CAS Validation Filter：
<filter>
  <filter-name>CAS Validation Filter</filter-name>
  <filter-class> org.jasig.cas.client.validation.Cas20ProxyReceivingTicketValidationFilter</filter-class>
  <init-param>
    <param-name>casServerUrlPrefix</param-name>
    <param-value>http://domainserver:8081/cas</param-value>
  </init-param>
</filter>
在客户端获取信息
AttributePrincipal principal = (AttributePrincipal) request.getUserPrincipal();
String loginName = principal.getName();//获取用户名
Map<String, Object> attributes = principal.getAttributes();
if(attributes != null) {
 System.out.println(attributes.get("userId"));
 System.out.println(attributes.get("passwordHint")); 
} 
