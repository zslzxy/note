# Shiro学习笔记

```html
<!-- 没有登录的话，可以在页面显示 -->
shiro:notAuthenticated=""

<!--验证是否又对应的权限，才会显示对应的标签中的内容-->
shiro:hasAnyRoles="merchant,user"

<!-- 获取到principal中的对应的元素的值 -->
<shiro:principal property="userName"/>

guest标签
　　<shiro:guest>
　　</shiro:guest>
　　用户没有身份验证时显示相应信息，即游客访问信息。

user标签
　　<shiro:user>　　
　　</shiro:user>
　　用户已经身份验证/记住我登录后显示相应的信息。

authenticated标签
　　<shiro:authenticated>　　
　　</shiro:authenticated>
　　用户已经身份验证通过，即Subject.login登录成功，不是记住我登录的。

notAuthenticated标签
　　<shiro:notAuthenticated>
　　
　　</shiro:notAuthenticated>
　　用户已经身份验证通过，即没有调用Subject.login进行登录，包括记住我
    自动登录的也属于未进行身份验证。

principal标签
　　<shiro: principal/>
　　
　　<shiro:principal property="username"/>
　　相当于((User)Subject.getPrincipals()).getUsername()。

lacksPermission标签
　　<shiro:lacksPermission name="org:create">
　
　　</shiro:lacksPermission>
　　如果当前Subject没有权限将显示body体内容。

hasRole标签
　　<shiro:hasRole name="admin">　　
　　</shiro:hasRole>
　　如果当前Subject有角色将显示body体内容。

hasAnyRoles标签
　　<shiro:hasAnyRoles name="admin,user">
　　　
　　</shiro:hasAnyRoles>
　　如果当前Subject有任意一个角色（或的关系）将显示body体内容。

lacksRole标签
　　<shiro:lacksRole name="abc">　　
　　</shiro:lacksRole>
　　如果当前Subject没有角色将显示body体内容。

hasPermission标签
　　<shiro:hasPermission name="user:create">　　
　　</shiro:hasPermission>
　　如果当前Subject有权限将显示body体内容
```

