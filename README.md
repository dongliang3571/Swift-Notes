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
