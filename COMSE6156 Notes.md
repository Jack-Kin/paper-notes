# COMSE6156 Notes

## 10/15

demo

- set session (cookies) 
- middleware: 
  - Oauth: flask dance -> google
  - Notification
    - slack (demo, we are not using it)
    - SNS (we need to do this)
  - API Gateway: all the requests go to the same end point, then it fans them out base on the path
    - define the API
    - deploy the API
    - set API key
  - Cloud Address Application: not in detail, have bugs



## 10/22

**HW1 RECAP**

**API Design:**

​	URL graph example:

​		Users & Address

​		Orders & Items

```
/api
	/users: GET, POST
	/users<uni>: GET, PUT, DELETE
	/usrs/<uni>/address
		select * from addresses where
			address_id = (select address_id from users
				where uni=...)
	/addresses/<address_id>/?users

# support linked data
{
	"address_id": 123,
	"links":[
		{"ref":"addresses", "href": "api/addresses/123"}
	]
}
```

**Service Composition**:

​	Example: how bank manage savings and check



**HW2**

Field Selection:

```sql
/api/users?lastname=Ferguson&crazy=Y&fields=lastname,firstname,email

select lastname, firstname, email
from users
where
	lastname='Ferguson' and crazy='Y'
```

Pagination:

```sql
/api/users?lastname=Ferguson&crazy=Y&fields=lastname,firstname,email&limit=10&offset=10

select lastname, firstname, email
from users
where
	lastname='Ferguson' and crazy='Y'
limit=10 offset=10
```

**Better Design**

Service should have a default limit, e.g. 20

Client 

​	Specified limit < 20, user clients limit

​	Limit not specified, or > 20 --> limit 20

**Examples:**

/api/users?lastname=Ferguson&crazy=Y&fields=lastname,firstname,email
returns:

```json
{
	"data":[
		{......},
           .
           .
           .
		{......} //20 rows
	],
	"links":[
		{"ref":"self", "href": "/api/users?lastname=Ferguson&crazy=Y&fields=lastname,firstname,email"},
		{"ref":"next", "href": "/api/users?lastname=Ferguson&crazy=Y&fields=lastname,firstname,email&limit=10&offset=10"}
	]
}
```

If click on the next:


```json
{
	"data":[
		{......},
           .
           .
           .
		{......} //20 rows
	],
	"links":[
		{"ref":"self", "href": "/api/users?lastname=Ferguson&crazy=Y&fields=lastname,firstname,email&limit=10&offset=10"},
		{"ref":"next", "href": "/api/users?lastname=Ferguson&crazy=Y&fields=lastname,firstname,email&limit=10&offset=20"},
        {"ref":"prev", "href": "/api/users?lastname=Ferguson&crazy=Y&fields=lastname,firstname,email"},
	]
}
```

There are more complex pagination patterns, depends on the database



we can have middle ware be a private package and then every service can get it instead of copying and pasting the code.
