Enum > Associated data(Algebraic data type)
---


[DOC](https://developer.apple.com/library/content/documentation/Swift/Conceptual/Swift_Programming_Language/Enumerations.html)

```Swift

enum User {
    case anonymous
    case named(String)
}

func userPhoto(user: User) -> String {
    switch user {
    case .anonymous:
        return "anon.png"
    case .named(let name):
        return "users\(name).png"
    }
}

let activeUsers = [User.anonymous, User.named("catface420"),  User.named("AzureDiamond"), User.anonymous]

let photos = activeUsers.map{ userPhoto(user: $0) } 
// ["anon.png", "userscatface420.png", "usersAzureDiamond.png", "anon.png"]


```


Optional
---

```Swift
Struct User {
  var name: String = ""
  var age: Int?
}

var sue = User(name: "Sue")
var tom = User(name: "Tom", age: 24)

```