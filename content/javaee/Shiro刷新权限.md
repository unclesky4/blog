---
title: "Shiro刷新权限"
date: 2018-09-09T17:36:15+08:00
lastmod: 2018-09-09T17:36:15+08:00
#tags : [ "dev", "hugo", "hyde-hyde"]
#categories : [ "dev" ]
layout: post
type:  "post"
highlight: false
draft: false
---

自定义Realm添加如下代码：

```


public void clearAuthz(){
		this.clearCachedAuthorizationInfo(SecurityUtils.getSubject().getPrincipals());
	}

	/**
	 * shiro刷新权限
	 */
	public static void reloadAuthorizing() {
        LoggerFactory.getLogger(This.class).info("权限刷新成功");

		RealmSecurityManager rsm = (RealmSecurityManager)SecurityUtils.getSecurityManager();
		MyShiroRealm realm = (MyShiroRealm)rsm.getRealms().iterator().next();
		realm.clearAuthz();
	}
```
