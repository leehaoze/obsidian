---
alias: [Response Code]
date created: 星期三, 二月 28日 2024, 6:52:51 晚上
date modified: 星期一, 三月 18日 2024, 7:48:57 晚上
state: F
tags: 
---

# RCode

RCode是[[DNS Message#Header]]中的4位字段，代表错误码

```
0               No error condition
1               Format error - The name server was
				unable to interpret the query.
2               Server failure - The name server was
				unable to process this query due to a
				problem with the name server.
3               Name Error - Meaningful only for
				responses from an authoritative name
				server, this code signifies that the
				domain name referenced in the query does
				not exist.
4               Not Implemented - The name server does
				not support the requested kind of query.
5               Refused - The name server refuses to
				perform the specified operation for
				policy reasons.  For example, a name
				server may not wish to provide the
				information to the particular requester,
				or a name server may not wish to perform
				a particular operation (e.g., zone
				transfer) for particular data.
6-15            Reserved for future use.
```