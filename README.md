# Swift-Notes

## Initializers

  ```swift
  class Vehicle {
    var numberOfWheels = 0
    var description: String {
        return "\(numberOfWheels) wheel(s)"
    }
  }
  
  let vehicle = Vehicle()
  print("Vehicle: \(vehicle.description)")
  // Vehicle: 0 wheel(s)
  
  class Bicycle: Vehicle {
    override init() {
        super.init()  // Call superclass's init first to initialize numberOfWheels
        numberOfWheels = 2  // Then override numberOfWheels to 2
    }
  }
  ```

  - Designated initializer
  - Convenient initializer
  
  ```swift
  class Food {
    var name: String
    init(name: String) {  // This is a designated initializer
        self.name = name
    }
    
    //  You can have more than one designated initializer,
    //  but usually we have one, and multiple convenient init
    
    convenience init() {  // This is a convenient initializer
        self.init(name: "[Unnamed]")
    }
  }
  
  class RecipeIngredient: Food {
    var quantity: Int
    
    init(name: String, quantity: Int) { // RecipeIngredient's own designated intializer
        self.quantity = quantity
        super.init(name: name)  // need to call superclass's designated init(), because it's designated itself
    }
    
    /* 
      It needs keyword override because it match the pattern of 
      designated init(name: String) in superclass
    */
    override convenience init(name: String) { 
        self.init(name: name, quantity: 1)
    }
    
    // RecipeIngredient
  }
  ```
  
 
![initliazer_delegation](https://github.com/dongliang3571/Swift-Notes/blob/master/images/initliazer_delegation.png?raw=true)
  
  - Automatic Initializer Inheritance
    
  ```swift
  /*
  Even though RecipeIngredient provides the init(name: String) initializer as a
  convenience initializer, RecipeIngredient has   nonetheless provided an 
  implementation of all of its superclass’s designated initializers. Therefore,
  RecipeIngredient automatically inherits all of its superclass’s convenience 
  initializers too.

  In this example, the superclass for RecipeIngredient is Food, which has a single
  convenience initializer called init(). This   initializer is therefore inherited 
  by RecipeIngredient. The inherited version of init() functions in exactly the same 
  way as   the Food version, except that it delegates to the RecipeIngredient version 
  of init(name: String) rather than the Food version.
  */
  
  //  All three of these initializers can be used to create new RecipeIngredient instances:
  
  let oneMysteryItem = RecipeIngredient()
  //  name: "[Unnamed]", quanlity: 1 because Food's init() call self.init(String: name).
  //  since RecipeIngredient has implemented all Food's designated init(), it will
  //  automatically inherit Food's init including convenient init(), but it will delegate
  //  Food.init(String: name) to RecipeIngredient.init(String: name), and RecipeIngredient.init(String: name)
  //  will call RecipeIngredient.init(name: name, quantity: 1)
  
  let oneBacon = RecipeIngredient(name: "Bacon")
  
  let sixEggs = RecipeIngredient(name: "Eggs", quantity: 6)
  ```
  
![initliazer_delegation](https://github.com/dongliang3571/Swift-Notes/blob/master/images/automatic_initializer_inheritance.png?raw=true)

  ```swift
  class ShoppingListItem: RecipeIngredient {
    var purchased = false
    var description: String {
        var output = "\(quantity) x \(name)"
        output += purchased ? " ✔" : " ✘"
        return output
    }
  }
  
  /*
  Because it provides a default value for all of the properties it introduces and does 
  not define any initializers itself, ShoppingListItem automatically inherits all of the 
  designated and convenience initializers from its superclass.
  */
  
  
  var breakfastList = [
    ShoppingListItem(),
    ShoppingListItem(name: "Bacon"),
    ShoppingListItem(name: "Eggs", quantity: 6),
  ]
  breakfastList[0].name = "Orange juice"
  breakfastList[0].purchased = true
  for item in breakfastList {
      print(item.description)
  }
  // 1 x Orange juice ✔
  // 1 x Bacon ✘
  // 6 x Eggs ✘

  ````
![inherit_all_init](https://github.com/dongliang3571/Swift-Notes/blob/master/images/inherit_all_init.png?raw=true)
