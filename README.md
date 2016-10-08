# Swift-Notes

## Initializers

  - Designated initializer
  - Convenient initializer
  
  ```swift
  class Food {
    var name: String
    init(name: String) {  // This is a designated initializer
        self.name = name
    }
    
    convenience init() {  // This is a convenient initializer
        self.init(name: "[Unnamed]")
    }
    
    init()
  }
  
  
  ```
    
