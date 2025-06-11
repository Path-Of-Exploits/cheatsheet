## Web Mass Assignement (White box scenario)

The "Web Mass Assignment" vulnerability allows an attacker to modify sensitive attributes of an application model by adding unexpected parameters to an HTTP request. If the application automatically assigns all received parameters to an object (without filtering), the attacker can change critical data (such as roles or permissions) and hijack the intended functionality of the application, for example by granting themselves admin rights. This issue is common in frameworks like Ruby on Rails and occurs when modifiable fields are not explicitly restricted.

```ruby
class User < ActiveRecord::Base
  attr_accessible :username, :email
end
```
```js
{ "user" => { "username" => "hacker", "email" => "hacker@example.com", "admin" => true } }
```

Although the User model does not explicitly state that the admin attribute is accessible.

