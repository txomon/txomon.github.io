---
categories:
- Off-topic
date: "2012-12-13T17:40:12+01:00"
title: Reverse engineering the authentication of your University
---
Well, In order to use ldap in my university, I have to learn how ldap works.

I first read LDAP intro, [http://www.davidpashley.com/articles/ldap-basics.html](http://www.davidpashley.com/articles/ldap-basics.html)

And then, went to [http://www.openldap.org/doc/admin23/](http://www.openldap.org/doc/admin23/)

There, in the quick start guide, I could get this command:

```
ldapsearch -h adm.uni.com -x -b '' -s base '(objectclass=*)' namingContexts
```

I got the output:

```
# extended LDIF
#
# LDAPv3
# base <> with scope baseObject
# filter: (objectclass=*)
# requesting: namingContexts
#
#
dn:
namingContexts: DC=adm,DC=uni,DC=com
namingContexts: CN=Configuration,DC=adm,DC=uni,DC=com
namingContexts: CN=Schema,CN=Configuration,DC=adm,DC=uni,DC=com
# search result
search: 2
result: 0 Success
# numResponses: 2
# numEntries: 1
```

So now, I continue reading...

Well, at the end, I found that I had to ask for `uid=<username>,ou=people,dc=uni,dc=com` to the server `ldaps.lg.uni.com`

There I can check a user exists, but I can't auth him yet...

Ok, I found the way. With this line I can query for the user's info

```
ldapsearch -x -H ldaps://ldaps.lg.uni.com/ -b ou=people,dc=uni,dc=com uid=jdomingo002
```

If I add this args:

```
-D uid=jdomingo002,ou=people,dc=uni,dc=com -W
```

I would try to identify as jdomingo002\. (prompting for password)
