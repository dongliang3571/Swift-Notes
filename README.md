# Swift-Notes

## Workspace, Project, Target, Schema

https://stackoverflow.com/questions/21631313/xcode-project-vs-xcode-workspace-differences

`Workspace` - Contains one or more projects. These projects usually relate to one another

`Project` - Contains code and resources, etc. (You'll be used to these!)

`Target` - Each project has one or more targets.
Each target defines a list of build settings for that project
Each target also defines a list of classes, resources, custom scripts etc to include/ use when building.
Targets are usually used for different distributions of the same project.
For example, my project has two targets, a "normal" build and an "office" build that has extra testing features and may contain several background music tracks and a button to change the track (as it currently does).
You'll be used to adding classes and resources to your default target as you add them.
You can pick and choose which classes / resources are added to which target.
In my example, I have a "DebugHandler" class that is added to my office build
If you add tests, this also adds a new target.

`Scheme` - A scheme defines what happens when you press "Build", "Test", "Profile", etc.
Usually, each target has at least one scheme
You can autocreate schemes for your targets by going to Scheme > Manage Schemes and pressing "Autocreate Schemes Now"

## Cocoa Touch Framework and Cocoa touch static libraries

https://medium.com/@Rageeni16/create-cocoa-touch-framework-and-publish-it-be9ad6535f33

## Embedded binaries and Linked Frameworks and Libaries

1. Linking- We must link a framework if we use any API defined in it.

2. Embedding - This process will ensure the added framework will be embedded within the App bundle, and potentially will help sharing code between the app, and any extension bundles. We embed only third party frameworks and not the ones provided by iOS as they are readily available in the device. If we are embedding, that means that, we will need to link to them too so that Xcode can compile and create the build. When the app runs in the device, then the embedded framework will be loaded into memory when needed.

## CocoaPods

how to group and reuse pods in Podfile

https://www.natashatherobot.com/cocoapods-installing-same-pod-multiple-targets/

## Xcode Build

Xcode build says no scheme

https://stackoverflow.com/questions/14368938/xcodebuild-says-does-not-contain-scheme

## Xcode Build Configuration

https://www.talentica.com/blogs/ios-build-management-using-custom-build-scheme/

## Access Control

Swift provides five different access levels for entities within your code. These access levels are relative to the source file in which an entity is defined, and also relative to the module that source file belongs to.

`Open` access and `public` access enable entities to be used within any source file from their defining module, and also in a source file from another module that imports the defining module. You typically use open or public access when specifying the public interface to a framework. The difference between open and public access is described below.

`Internal` access enables entities to be used within any source file from their defining module, but not in any source file outside of that module. You typically use internal access when defining an app’s or a framework’s internal structure.

`File-private` access restricts the use of an entity to its own defining source file. Use file-private access to hide the implementation details of a specific piece of functionality when those details are used within an entire file.

`Private` access restricts the use of an entity to the enclosing declaration, and to extensions of that declaration that are in the same file. Use private access to hide the implementation details of a specific piece of functionality when those details are used only within a single declaration.

`Open` access is the highest (least restrictive) access level and private access is the lowest (most restrictive) access level.

`Open` access applies only to classes and class members, and it differs from public access as follows:

Classes with `public` access, or any more restrictive access level, can be subclassed only within the module where they’re defined.

Class members with `public` access, or any more restrictive access level, can be overridden by subclasses only within the module where they’re defined.

`Open` classes can be subclassed within the module where they’re defined, and within any module that imports the module where they’re defined.

`Open` class members can be overridden by subclasses within the module where they’re defined, and within any module that imports the module where they’re defined.

Marking a class as `open` explicitly indicates that you’ve considered the impact of code from other modules using that class as a superclass, and that you’ve designed your class’s code accordingly.

## Auto layout programmatically

https://theswiftdev.com/2018/06/14/mastering-ios-auto-layout-anchors-programmatically-from-swift/

https://theswiftdev.com/2017/10/31/ios-auto-layout-tutorial-programmatically/

## Understanding the contentOffset and contentInset properties of the UIScrollView class

https://fizzbuzzer.com/understanding-the-contentoffset-and-contentinset-properties-of-the-uiscrollview-class/

## Dependency injection

https://www.swiftbysundell.com/posts/simple-swift-dependency-injection-with-functions

## Closures

https://www.swiftbysundell.com/posts/capturing-objects-in-swift-closures

## First class function

https://www.swiftbysundell.com/posts/first-class-functions-in-swift

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


## Copy on Write

https://marcosantadev.com/copy-write-swift-value-types/

## Type Constraint Syntax

You write type constraints by placing a single class or protocol constraint after a type parameter’s name, separated by a colon, as part of the type parameter list. The basic syntax for type constraints on a generic function is shown below (although the syntax is the same for generic types):

```swift
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {
    // function body goes here
}
```

The hypothetical function above has two type parameters. The first type parameter, T, has a type constraint that requires T to be a subclass of SomeClass. The second type parameter, U, has a type constraint that requires U to conform to the protocol SomeProtocol.

### Associated Types in Action

Here’s an example of a protocol called Container, which declares an associated type called Item:

```swift
protocol Container {
    associatedtype Item
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

the Container protocol needs a way to refer to the type of the elements that a container will hold, without knowing what that type is for a specific container. The Container protocol needs to specify that any value passed to the append(_:) method must have the same type as the container’s element type, and that the value returned by the container’s subscript will be of the same type as the container’s element type.

To achieve this, the Container protocol declares an associated type called Item, written as associatedtype Item. The protocol doesn’t define what Item is—that information is left for any conforming type to provide. Nonetheless, the Item alias provides a way to refer to the type of the items in a Container, and to define a type for use with the append(_:) method and subscript, to ensure that the expected behavior of any Container is enforced.

Here’s a version of the nongeneric IntStack type from Generic Types above, adapted to conform to the Container protocol:

```swift
struct IntStack: Container {
    // original IntStack implementation
    var items = [Int]()
    mutating func push(_ item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
    // conformance to the Container protocol
    typealias Item = Int
    mutating func append(_ item: Int) {
        self.push(item)
    }
    var count: Int {
        return items.count
    }
    subscript(i: Int) -> Int {
        return items[i]
    }
}
```

The IntStack type implements all three of the Container protocol’s requirements, and in each case wraps part of the IntStack type’s existing functionality to satisfy these requirements.

Moreover, IntStack specifies that for this implementation of Container, the appropriate Item to use is a type of Int. The definition of typealias Item = Int turns the abstract type of Item into a concrete type of Int for this implementation of the Container protocol.

### Protocol

Difference Between { get } and { get set }

https://medium.com/@kahseng.lee123/difference-between-get-and-get-set-which-one-to-choose-719fe9ec7704

Swift Protocols: Magic of Dynamic & Static methods dispatches

https://medium.com/@PavloShadov/https-medium-com-pavloshadov-swift-protocols-magic-of-dynamic-static-methods-dispatches-dfe0e0c85509

```swift
protocol Container {
    associatedtype Item: Equatable
    mutating func append(_ item: Item)
    var count: Int { get }
    subscript(i: Int) -> Item { get }
}
```

extending a protocol doesn't mean adding more protocol method or properties, it simply adds new method that you can use right away.

```swift
extension Container {
    func prin(){
        print("print")
    }
}
```

in this case, method `prin()` can be used right away.


Protocols can now constrain their conforming types

```swift
protocol MyView: UIView { /*...*/ } // swift 4

// or

protocol MyView where Self: UIView { /*...*/ } // swift 3 or older 
```


class-only protocol

```swift
protocol MyObj: AnyObject {} // swift 4

// or

protocol MyObj: class {} // swift 3 or older
```

Protocol Composition

```swift
protocol Named {
    var name: String { get }
}
protocol Aged {
    var age: Int { get }
}
struct Person: Named, Aged {
    var name: String
    var age: Int
}
```

Protocol compositions have the form `SomeProtocol & AnotherProtocol`. You can list as many protocols as you need, separating them with ampersands (`&`). In addition to its list of protocols, a protocol composition can also contain one class type, which you can use to specify a required superclass.

for example:

```swift

struct Person: Named & Aged {
    var name: String
    var age: Int
}

// or

typealias PersonProtocol = Named & Aged

struct Person: PersonProtocol {
    var name: String
    var age: Int
}

// for function parameters

func wishHappyBirthday(to celebrator: Named & Aged) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}

// or 

func wishHappyBirthday(to celebrator: PersonProtocol) {
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}
```

### Enumeration with `if case let`

https://alisoftware.github.io/swift/pattern-matching/2016/05/16/pattern-matching-4/

### [weak self]

https://medium.com/anysuggestion/preventing-memory-leaks-with-swift-compile-time-safety-49b845df4dc6

### Things I need to know

https://github.com/codepath/ios_guides/wiki

1. Navigation Bar
  - The back button on navigation bar belongs to the previous view controller, not the one currently presented on screen.
To modify the back button you should update it before pushing, on the view controller that initiated the segue
  ```swift
  // Set up back button(backBarButtonItem) and back indicator image
  // This is done on parent view of the view we segue to, because back button and back indicator image belong to parent view
  self.navigationController?.navigationBar.backIndicatorImage = UIImage(named: ImageName.BACK_ICON.rawValue)
  self.navigationController?.navigationBar.backIndicatorTransitionMaskImage = UIImage(named: ImageName.BACK_ICON.rawValue)
  self.navigationItem.title = "POs" // this will be the text for back button text
  self.navigationItem.backBarButtonItem?.tintColor = FMCheckColor.Red // change color of back button
  
  // However self.navigationItem.leftBarButtonItem and self.navigationItem.rightBarButtonItem belongs to current view
  // if you want to overwrite leftBarButtonItem
  let item = UIBarButtonItem(title: "Text goes here", style: .Plain, target: self, action:  #selector(self.navigationController?.popViewControllerAnimated(_:)))

  item.setTitleTextAttributes([NSFontAttributeName: UIFont(name: "Helvetica-Bold", size: 23)!], forState: .Normal)
  navigationItem.leftBarButtonItem = item // over write left button
  navigationItem.rightBarButtonItem = item // over write right button
  ```

2. Frame and bound
  - https://gabrielghe.github.io/swift/2015/03/22/swift-uiview-bounds-vs-frame-vs-center
  
  ```swift
  // To get width and height of a view
  let width = view.frame.size.width
  // or
  let width = view.frame.width
  // or
  let width = view.bounds.width
  
  // to get height
  let height = view.frame.size.height
  
  // to get x and y
  let x = view.frame.origin.x
  let y = view.frame.origin.y
  
  ```

3. View Management
  ```swift
  // view method precedence:
  func loadView();
  func viewDidLoad();
  func viewWillAppear();
  func viewDidAppear();
  func viewWillLayoutSubviews();
  func viewDidLayoutSubviews();

  // You should not initialise UI geometry-related things in viewDidLoad, because the geometry of your view is not set at this point and the results will be unpredictable.

  // viewWillAppear: is the correct place to do these things, at this point the geometry is set so getting and setting geometry-related properties makes sense."

  // This also applies to getting geometry-related properties. Don't get distracted by it's behaving correctly in one circumstance and not in another... The main point is that setting/getting geometry properties too early will yield unpredictable results.

  // Your screen looks correct because you are seeing the results after viewWillAppear: / viewDidAppear:.


  // When using autolayout in ios6, frames are not set until subviews have been laid out. The right place to get frame data under these conditions is in the viewController method viewDidLayoutSubviews.

  // This is called after viewWillAppear:. But take care - they are not always called together. For example, viewDidLayoutSubviews is called after a rotation, viewWillAppear: is not. viewWillAppear: is called every time a view becomes the visible view*, viewDidLayoutSubviews not necessarily (only if the views needed relaying out).

  // (* unless the app was backgrounded and is becoming the foreground app, then viewWillAppear: is not called)
```

3. Segues

  - Show - Pushes the destination view controller onto the navigation stack, moving the source view controller out of the way (destination slides overtop from right to left), providing a back button to navigate back to the source - on all devices.Example: Navigating inboxes/folders in Mail.

  - Show Detail - Replaces the detail/secondary view controller when in a UISplitViewController with no ability to navigate back to the previous view controller. Example: In Mail on iPad in landscape, tapping an email in the sidebar replaces the view controller on the right to show the new email.

  - Present Modally - Presents a view controller in various different ways as defined by the Presentation option, covering up the previous view controller - most commonly used to present a view controller that animates up from the bottom and covers the entire screen on iPhone, but on iPad it's common to present it in a centered box format overtop that darkens the underlying view controller. Example: Tapping the + button in Calendar on iPhone.

  - Popover Presentation - When run on iPad, the destination appears in a small popover, and tapping anywhere outside of this popover will dismiss it. On iPhone, popovers are supported as well but by default if it performs a Popover Presentation segue, it will present the destination view controller modally over the full screen. Example: Tapping the + button in Calendar on iPad (or iPhone, realizing it is converted to a full screen presentation as opposed to an actual popover).

  - Custom - You may implement your own custom segue and have complete control over its appearance and transition.

  
  ```swift
  // Segues are components of storyboard. If you don't have a storyboard, you can't perform segues. But you can use present view controller like this:
  let vc = ViewController() //your view controller
  self.present(vc, animated: true, completion: nil)
  ```
4. `UISearchController` and `UISearchBar`

  ```swift
  // Search controller
  self.searchController = UISearchController(searchResultsController: nil)
  self.searchController.searchResultsUpdater = self
  self.searchController.dimsBackgroundDuringPresentation = false
  self.searchController.hidesNavigationBarDuringPresentation = false

  // Search bar customizations
  self.searchController.searchBar.placeholder = "Search"
  self.searchController.searchBar.delegate = self
  self.searchController.searchBar.sizeToFit()
  
  // Sets current view controller as presenting view controller for the search interface
  // If this sets to 'false', sometimes, search bar will overlap with other components when begin editing
  // when this sets to 'true', it resolves overlapping problem, however sometimes it shifts down search bar when tapping on it
  // look this link: https://stackoverflow.com/questions/20731360/strange-uisearchdisplaycontroller-view-offset-behavior-in-ios-7-when-embedded-in
  // self.definesPresentationContext = true
  ```
5. XCode Simulator

  - If your app cannot run after first run(can launch the app from XCode the first time, but cannot launch second time without deleting the app from simulator). It give you error like `this app could not be installed at this time` or `Could not hardlink copy`. You need to open `info.plist` and make sure you have both `CFBundleShortVersionString` and `CFBundleVersion` and their values. If you still have same problem even you have these two keys, try to move to the top of the `info.plist`.

6. Edge Insets

  ```swift
  // If your scroll view looks off, this is probably the reason
  automaticallyAdjustsScrollViewInsets = true
  
  // Sets current view controller as presenting view controller for the search interface
  definesPresentationContext = true
  
  // tells what edges should be extended (left, right, top, bottom, all, none or any combination of those). Extending bottom edge equals "Under Bottom Bars" tick, extending top edge equals "Under Top Bars" tick.
  edgesForExtendedLayout = [.top, .bottom, .left, .right, .all] // to set to none, you can do [] empty array
  

  //tells if edges should be automatically extended under the opaque bars. So if you combine those two settings you can mimic any combination of interface builder ticks in your code.
  extendedLayoutIncludesOpaqueBars  = true // default value is false, Bars are translucent by default in iOS 7.0
  ```

7. `UISearchController` and `UISearchBar`
  A `UISearchController` is presenting an view controller, when doing `performSegue` or `popViewControllerAnimated`, make sure you dimiss the `UISearchController` first by doing, `UISearchController.dismiss()`
  
  ```swift
  // A better way to embed search bar
  if #available(iOS 11.0, *) {
    // embed in the navigation bar
    navigationItem.searchController = searchController
    navigationItem.hidesSearchBarWhenScrolling = false
  } else {
    // embed in table view 
    tableView.tableHeaderView = searchController.searchBar
  }
  ```

8. `UITableView` and `UITableViewCell`

  ```swift
  // by default, cell is gray when clicked, we want the gray color dims out after clicking
  func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
      tableView.deselectRow(at: indexPath, animated: true)
  }
  
  // func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) and
  // func tableView(_ tableView: UITableView, didDeSelectRowAt indexPath: IndexPath) 
  // are not trigger by UITableViewCell.setSelected(Bool, animate: Bool) nor UITableView.selectRow(at: IndexPath, animated: Bool, scrollPosition: UITableViewScrollPosition), it only gets triggered off
  // by physically clicking on screen.
  // However UITableView.selectRow(at: IndexPath, animated: Bool, scrollPosition: UITableViewScrollPosition) is considered a better way to select a row
  
  
  // Replace text label
  class NoteTableCell: UITableViewCell {

    // UI
    var noteTextView = UITextView()

    override func awakeFromNib() {
        super.awakeFromNib()
        // Initialization code
    }

    // When layout is ready
    override func layoutSubviews() {
        super.layoutSubviews()

        self.textLabel?.text = "test" // assign a random text so the text label is intialized
        self.textLabel?.isHidden = true
        self.noteTextView.frame = self.textLabel!.frame
        self.noteTextView.text = "TextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextTextText"
        self.contentView.addSubview(self.noteTextView)
    }

    override func setSelected(_ selected: Bool, animated: Bool) {
        super.setSelected(selected, animated: animated)

        // Configure the view for the selected state
    }
}

  // Check mark should be clean in cellForRowAt if we using tableView.dequeueReusableCell, and checkmakr is set in didSelectRowAt, for example:
  func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
      tableView.cellForRow(at: indexPath)?.accessoryType = UITableViewCellAccessoryType.checkmark
  }
  
  // In cellForRowAt
  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
      let cell = tableView.dequeueReusableCell(withIdentifier: self.reuseableCellName, for: indexPath as IndexPath)
       cell.selectionStyle = .none // must set it to none before reuse, otherwise we will see multiple check marks
  }
  ```
9. Value type and Reference type

  https://medium.com/@abhimuralidharan/difference-between-value-type-and-a-reference-type-in-ios-swift-18cb5145ad7a

10. `Any` and `AnyObject`

https://medium.com/@mimicatcodes/any-vs-anyobject-in-swift-3-b1a8d3a02e00

11. problem of delay in presenting a view modally

  ```swift
  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
      let cell = UITableViewCell()
      cell.selectionStyle = .none
      
      return cell
  }
  
  func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
      let vehicleTypeVC = ServiceVehicleTypeSelectController()

      self.present(vehicleTypeVC, animated: true, completion: nil)
  }
  
  // we are presenting a view modally in didSelectRowAt, this is probably will cause delay in showing the view, or even worse not showing at all
  
  // the problem is with the cell's selectionStyle, so workaround is to set cell's selectionStyle to .default
  func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
      let cell = UITableViewCell()
      cell.selectionStyle = .default
      
      return cell
  }
  
  func tableView(_ tableView: UITableView, didSelectRowAt indexPath: IndexPath) {
      let vehicleTypeVC = ServiceVehicleTypeSelectController()

      self.present(vehicleTypeVC, animated: true, completion: nil)
  }
  
  // other workarounds
  DispatchQueue.main.async { // Manually wake up man runloop
      self.present(vehicleTypeVC, animated: true, completion: nil)
  }
  // More discussion about this problem is here
  // https://stackoverflow.com/questions/21075540/presentviewcontrolleranimatedyes-view-will-not-appear-until-user-taps-again/22173620
  ```
12. Keyboard covers content in the view

  ```swift
  UITableViewController will push up content automatically.
  UIViewController doesn't
  so we need to use keyboard notification to set inset
  ```
  
13. Container view programmatically

```swift
 // Container view's viewDidLoad is called before the parent view conttroller
 // so we want to override loadView instead, because loadView in parent is always called before child
 
 // add container

    let containerView = UIView()
    containerView.translatesAutoresizingMaskIntoConstraints = false
    view.addSubview(containerView)
    NSLayoutConstraint.activate([
        containerView.leadingAnchor.constraint(equalTo: view.leadingAnchor, constant: 10),
        containerView.trailingAnchor.constraint(equalTo: view.trailingAnchor, constant: -10),
        containerView.topAnchor.constraint(equalTo: view.topAnchor, constant: 10),
        containerView.bottomAnchor.constraint(equalTo: view.bottomAnchor, constant: -10),
        ])

    // add child view controller view to container

    let controller = storyboard!.instantiateViewController(withIdentifier: "Second")
    addChildViewController(controller)
    controller.view.translatesAutoresizingMaskIntoConstraints = false
    containerView.addSubview(controller.view)

    NSLayoutConstraint.activate([
        controller.view.leadingAnchor.constraint(equalTo: containerView.leadingAnchor),
        controller.view.trailingAnchor.constraint(equalTo: containerView.trailingAnchor),
        controller.view.topAnchor.constraint(equalTo: containerView.topAnchor),
        controller.view.bottomAnchor.constraint(equalTo: containerView.bottomAnchor)
        ])

    controller.didMove(toParentViewController: self)
```

14. get root view controller

  ```swift
  // if you set rootViewController in storyboard, you do:
  let delegate = UIApplication.shared.delegate as? AppDelegate
  if let root = delegate?.window?.rootViewController as? HomeController {
      // root is the root view controller
      // root.view can get the super view 
  }
  
  // if you are trying to access the rootViewController you set in your appDelegate, you do
  let appDelegate  = UIApplication.shared.delegate as! AppDelegate
  let viewController = appDelegate.window!.rootViewController as! YourViewController
  
  // swift 4:
  let viewController = UIApplication.shared.keyWindow!.rootViewController as! YourViewController
  ```

## About preferredInterfaceOrientationForPresentation

`preferredInterfaceOrientationForPresentation` is never called because this is not a "presented" view controller. There is no "presentation" involved here.

"Presented" and "presentation" are not some vague terms meaning "appears". These are precise, technical terms meaning that this view controller is brought into play with the call presentViewController:animated:completion:. In other words, this event arrives only if this is what we used to call a "modal" view controller.

Well, your view controller is not a modal view controller; it is not brought into play with presentViewController:animated:completion:. So there is no "presentation" involved, and therefore preferredInterfaceOrientationForPresentation is irrelevant here.

I'm being very explicit about this because I'm thinking that many folks will be confused or misled in the same way you were. So perhaps this note will help them.

Launch into Landscape

In iOS 6, the "Supported Interface Orientations" key in your Info.plist is taken much more seriously than previously. The solution to your overall problem of launching into a desired orientation is:

Make sure that "Supported Interface Orientations" in your Info.plist lists all orientations your app will ever be allowed to assume.

Make sure that the desired launch orientation is first within the "Supported Interface Orientations".

That's all there is to it. You should not in fact have to put any code into the root view controller to manage the initial orientation.

## Animation

Animation with constraints:

https://medium.com/@aunnnn/animate-autolayout-constraints-separately-52eeb989a5cc

you can simply animate different constraint changes in each animation block simultaneously. Just don’t forget to end with `layoutIfNeeded()` on each of the block.

```swift
UIView.animate(withDuration: 1) {
  widthConstraint.constant = ...
  heightConstaint.constant = ...
  view.layoutIfNeeded() // flush all changes
}
UIView.animate(withDuration: 1, 
               delay: 0, 
               usingSpringWithDamping: 0.5, 
               initialSpringVelocity: 0, 
               options: [], 
               animations: 
{
  topConstraint.constant = ...
  view.layoutIfNeeded() // flush all changes
}) { (finished) in }
```

### modalTransitionStyle and modalPresentationStyle

https://blog.csdn.net/a2331046/article/details/54706790

https://happyteamlabs.com/blog/difference-of-overfullscreen-and-overcurrentcontext-modalpresentationstyle-in-ios/

### TableView separator

```swift
tableView.separatorStyle = .singleLine
tableView.separatorColor = .duckEggBlue
tableView.separatorInset = UIEdgeInsets(top: 0, left: Layout.margin, bottom: 0, right: 0)
```

### `[unowned self]` and `[weak self]`

https://medium.com/@kiran.jasvanee/difference-between-unowned-self-and-weak-self-in-swift-310c14961953


### signing certificate ###

https://medium.com/ios-os-x-development/ios-code-signing-provisioning-in-a-nutshell-d5b247760bef

A signing certificate is a digital identity used for code signing during the build and archive process.

A signing signing certificate includes the certificate with its public-private key pair issued by Apple, and is stored in your keychain. Because the private key is stored locally, protect it as you would an account password. An intermediate certificate is also required to be in your keychain to ensure that your certificate is issued by a certificate authority such as Apple.

When you install Xcode, Apple’s intermediate certificates are added to your keychain for you. Then you use Xcode to create your signing certificate and sign your app. Your signing certificate is added to your keychain and the corresponding certificate is added to your developer account. If you enroll as an organization, the developer account shows all the certificates created by members of your team.
