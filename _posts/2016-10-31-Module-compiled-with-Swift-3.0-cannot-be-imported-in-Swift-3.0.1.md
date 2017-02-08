---
title: Module compiled with Swift 3.0 cannot be imported in Swift 3.0.1
---

Cartfile:
`github "SwiftyJSON/SwiftyJSON"`
get error:
`Module compiled with Swift 3.0 cannot be imported in Swift 3.0.1`
fixed it by adding 'master' on the end in cartfile:
`github "SwiftyJSON/SwiftyJSON" "master"`