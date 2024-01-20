## 声明、变量常量名

```swift
let `let` = 3.14159 // 常量，值不可更改，自动推断是 Double 型
var 你好 = "你好世界", 🐶🐮 = "dogcow" // 一行声明多个变量或常量
var red, green, blue: Double // 3 个都是 Double
var red: String, green, blue: Double // red 是 String，后两个是 Double
var red, green: String, blue: Double // 前两个是 String，blue 是 Double
var red=3, green="2.0", blue: String 
```

> 语句结束不强制写分号，如果一行有多条语句才必须用分号隔开。
> 单行注释`//`。多行注释`/* */`，**支持嵌套，根据层级配对**。

常量用 let，变量用 var，可以在一行声明多个。

可以自动推断类型，可以在后面加类型标注。如果几个变量类型相同，可以在最后一个变量后面加。

对于变量常量名有几条规定：
- 不能包含**空白字符、数学符号、箭头、保留的（或者无效的）Unicode 码位、连线和制表符**。
- 不能以数字开头。
- 如果需要使用 Swift 保留关键字来给常量或变量命名，可以使用反引号 ( \`) 包围它来作为名称。总之，除非别无选择，避免使用关键字作为名字。

## 调试

### print/debugPrint

`print(_:separator:terminator:)`，separator 指定分隔符（默认空格），terminator 指定结尾符（默认换行），debugPrint 一样。可以接收多个要输出的参数。

```swift
let pi = 3.14
let hello = "你好"
print(pi, hello, terminator="") // 3.14 你好，通过 terminator 去掉了换行
print("pi is \(pi)") // pi is 3.14\n
```

字符串插值是 `"\(变量名)"`。Python 的插值方式 `f'{变量名}`，Kotlin 的插值方式 `"${变量名}"`。

debugPrint 是原样输出，比如 `debugPrint(3)` 输出 3，`debugPrint("3")` 输出 `"3"`，引号也被输出。

### 断言和先决条件

断言或先决条件为 true，继续执行，为 false，程序终止。

断言只在 Debug 中检查，Release 环境不会被检查，可以随便写都不会影响性能。先决条件在 Release 情况也会检查。

```swift
let age = -3
assert(age >= 0)
assert(age >= 0, "A person's age cannot be less than zero")
```

assert 第一个参数是一个返回布尔值的表达式，第二个参数是第一个表达式为 false 时显示的提示信息，第二个参数可以省略不写。

如果代码已经检查了条件，可以使用 `assertionFailure(_:file:line:)` 函数来标明断言失败，让程序终止。

```swift
if age > 0 {
} else {
    assertionFailure("A person's age can't be less than zero.")
}
```

相对应的先决条件是 precondition 和 preconditionFailure。

## 基本类型

### 数字

整数：

- Int8/UInt8、Int16/UInt16、Int32/UInt32、Int64/UInt64 分别是 8、16、32、64 位有/无符号整数。
- Int/Unit 在 32 位平台相当于 Int32/UInt32，在 64 位平台相当于 Int64/UInt64
- 类型最小值如 UInt8.min，类型最大值如 Int.max

浮点数：

- Float 32 位，精度 6 位。
- Double 64 位，精度 15 位。浮点数不指定类型值时默认推断为 Double。

#### 进制

整数：

* 二进制前缀 0b，如 0b10001
* 八进制前缀 0o，如 0o21
* 十六进制前缀 0x，如 0x11

浮点数：

* 十进制指数用`E`或`e`表示
    - `1.25e2`即 1.25 x 10<sup>2</sup> = 125.0
    - `1.25e-2`即 1.25 x 10<sup>-2</sup> = 0.0125
* 十六进制浮点数**必须有指数**，用`P`或`p`表示
	 - `0xFp2`即 15 x 2<sup>2</sup> = 60.0
	 - `0xFp-2`即 15 x 2<sup>-2</sup> = 3.75

#### 类型转换

就是用对应类型去初始化。

整数转换：

```swift
let twoThousand: UInt16 = 2_000
let one: UInt8 = 1
let twoThousandAndOne = twoThousand + UInt16(one)
```

整数与浮点数转换：

```swift
let three = 3
let pointOneFourOneFiveNine = 0.14159
let pi = Double(three) + pointOneFourOneFiveNine // 整数转浮点数，必须转换后才能加
let integerPi = Int(pi) // 浮点数转整数

let pi2 = 3 + 0.14159 // 字面量可以直接加，不是变量
```

### 布尔

类型 Bool，值 true/false。

### 元组

和 Python 差不多，但格式没那么随意，必须有逗号，括号，任意类型都可以。

```swift
let http404Error = (404, "Not Found")
// 类型是 (Int, String)
```

#### 读取元素

1. 分解
	
	就是取出里面的值。

	```swift
	let (statusCode, statusMessage) = http404Error
	```

	这样 statusCode 就是 404，statusMessage 就是 "Not Found"，如果某个值用不到，可用 `_` 替代。

	```swift
	let (justTheStatusCode, _) = http404Error
	```

2. 索引

	利用从零开始的索引数字访问元组中的单独元素：

	```swift
	print("The status code is \(http404Error.0)")
	// prints "The status code is 404"
	print("The status message is \(http404Error.1)")
	// prints "The status message is Not Found"
	```

3. 名称

	可以在定义元组的时候给其中的单个元素命名，这样就可以通过名字来取代数字索引，增加可读性。

	```swift
	let http200Status = (statusCode: 200, description: "OK")
	print("The status code is \(http200Status.statusCode)")
	// prints "The status code is 200"
	print("The status message is \(http200Status.description)")
	// prints "The status message is OK"
	```

### 类型别名

给一个类型起另外一个名字。

```swift
typealias AudioSample = UInt16
var maxAmplitudeFound = AudioSample.min
```

### 可选项

可空变量，和 Kotlin 一样。

```swift
var surveyAnswer: String?
surveyAnswer = "404"
```

默认值 nil。

#### 强制展开用 

```swift
var surveyAnswer: String?  
if surveyAnswer != nil { // if 后没有括号  
   print("surveyAnswer is \(surveyAnswer)")
}  
```  
  
#### 可选项绑定  
  
判断可选项是否包含值，如果包含就把值赋给一个**临时**的常量或者变量。  

```swift
var surveyAnswer: String?

if let answer = surveyAnswer {
   // answer 是临时的，只能在这里用
    print("answer is \(answer)")
} else {
    // 说明 surveyAnswer 是 nil
    print("surveyAnswer is nil")
}
```

可以用同样的名字。

```swift
if let surveyAnswer = surveyAnswer {
   // 用的是 let 后面定义的 surveyAnswer，是确定的了，不是可选项了
   print("surveyAnswer is \(surveyAnswer)")
}

surveyAnswer = "abc"
print("surveyAnswer is \(surveyAnswer!)")
```

在外面继续使用，它还是可选项，需要判断是否为 nil，上面强制展开了，直接用 `\(surveyAnswer)` 不行。

上面用同样名字的写法很常用，所以有种等价的简便写法

```swift
if let surveyAnswer {  
} else {
}
```

if 中可以同时包含多个可选项绑定，用逗号分隔。如果任一可选绑定结果是 nil 或者布尔值为 false，那么整个 if 判断会被看作 false

```swift
if let firstNumber = Int("4"), let secondNumber = Int("42"), firstNumber < secondNumber && secondNumber < 100 {
    print("\(firstNumber) < \(secondNumber) < 100")
    // "4 < 42 < 100"
}
```

#### 隐式展开可选项

就是最初是 nil，但是一旦设置了值以后就会一直有值，不是说从语法上不能再设置为 nil，而是在当前的程序环境里，它不会再是 nil，所以有了这样一种设计。

有点类似 Kotlin 的延迟初始化，一开始是 null，设置值后就一直有值。

语法是 `let assumedString: String! = "An implicitly unwrapped optional string."`，就是后面跟着的不是问号，而是感叹号了。

```swift
let optionalString = assumedString // 使用的时候不需要加 !，直接用
let implicitString: String = assumedString
```

隐式展开可选项它也是一个可选项，所以 optionalString 的类型是 String?，而由于 implicitString 指定了类型 String，这句还能正常赋值是因为 assumedString 默认展开了。

隐式展开可选项的检查，绑定语法和普通可选项的一样：

```swift
if assumedString != nil { 
    print(assumedString)
}

if let definiteString = assumedString {
    print(definiteString)
}
```

是可选项，所以可以判断是否和 nil 相等，防止在未赋值前调用，一般来说如果需要判断是否和 nil 相等，说明它就不该用隐式展开可选项，而是一个普通的可选项。

### 字符串

用双引号。

#### 字符串的创建

```swift
// 字面量
var emptyString = "" 
var str = "abc"

// 构造方法
var anotherEmptyString = String()
var anotherStr = String("abc")
```

#### 多行字符串

```swift
let quotation = """

	The White Rabbit put on his spectacles.  "Where shall I \n
	begin,
		please your Majesty?" he asked.
	"Begin at the beginning," the King said gravely, "and go on
	till you come to the end; then stop."
	"""
```

1. 第一行或末尾行要空行的得单独写，默认是没有的
2. 如果为了可读性的换行，输出显示不要换号，用转义符 `\`
3. 前面的空格无所谓，以结束的三个双引号为准，它前面有多少空格，所有行前面这些空格都忽略，如果某一行比起三个双引号还有空格，这个空格才会显示出来

#### 转义与扩展字符串分隔符

扩展字符串分隔符可以忽略转义，忽略需要转义的。

`#"Line 1\nLine 2"#` 左右加 `#` 就忽略了内部的转义符 `\n`，如果又想让它生效，用 `#"Line 1\#nLine 2"#`。

对于三行字符串也一样，本来内部的三个引号要输出得加转义符，现在有了 `#`，就不用转义了。

```swift
let threeMoreDoubleQuotationMarks = #"""
Here are three more double quotes: """
"""#
```

#### 字符串与字符

都是双引号，只有一个字符就叫字符 Character，多个字符就叫字符串 String。

遍历字符串的每个就是字符。

```swift
for character in "Dog!🐶️" {
    print(character)
}
```

多个字符放数组里构造字符串。

```swift
let catCharacters: [Character] = ["C", "a", "t", "!", "🐱️"]
let catString = String(catCharacters) // "Cat!🐱️"
```

如果一个 Unicode 值可以表现为复合和分解形式，那么这两种形式都可以用字符表示。

```swift
let eAcute: Character = "\u{E9}" // é
let combinedEAcute: Character = "\u{65}\u{301}" // e followed by ́
// eAcute is é, combinedEAcute is é
```

#### 字符串插值

```swift
let multiplier = 3
let message = "\(multiplier) times 2.5 is \(Double(multiplier) * 2.5)"
```

有扩展字符串分隔符的时候，原样输出，所以就不生效了，想让它生效，在 `\` 后加 `#`。

```swift
print(#"Write an interpolated string in Swift using \(multiplier)."#)
// Prints "Write an interpolated string in Swift using \(multiplier)."

print(#"6 times 7 is \#(6 * 7)."#)
// Prints "6 times 7 is 42."
```

#### 字符串索引访问

因为每个字符占用长度可能不一样，比如同样的 é，既可以是一个 Unicode，也可以是两个，所以不能用整数值索引。

- `startIndex` 属性是第一个字符的位置
- `endIndex` 属性是最后一个字符**后**的位置。如果 String为空，则 startIndex 与 endIndex 相等
- `index(before:)` 访问给定索引前的位置
- `index(after:)` 访问给定索引后的位置
- `index(_:offsetBy:)` 访问指定距离的位置

```swift
let greeting = "Guten Tag!"
greeting[greeting.startIndex] // G
greeting[greeting.index(before: greeting.endIndex)] // !
greeting[greeting.index(after: greeting.startIndex)] // u
let index = greeting.index(greeting.startIndex, offsetBy: 7)
greeting[index] // a
```

使用 indices 属性来访问字符串中每个字符的索引。

```swift
for index in greeting.indices {
	print("\(greeting[index]) ", terminator: "")
}
```

#### 字符串插入和删除

插入字符用 `insert(_:at:)`，插入字符串用 `insert(contentsOf:at:)`。

```swift
var welcome = "hello"
welcome.insert("!", at: welcome.endIndex)
// welcome now equals "hello!"

welcome.insert(contentsOf: " there", at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there!"
```

删除字符用 `remove(at:)`，删除字符串用 `removeSubrange(_:)`。

```swift
welcome.remove(at: welcome.index(before: welcome.endIndex))
// welcome now equals "hello there"

let range = welcome.index(welcome.endIndex, offsetBy: -6)..<welcome.endIndex
welcome.removeSubrange(range)
// welcome now equals "hello"
```

#### 子字符串

字符串是 String，子字符串是 Substring，不是一个东西。

获取子串，用 `[]`。

```swift
let greeting = "Hello, world!"
let index = greeting.index(of: ",") ?? greeting.endIndex
let beginning = greeting[..<index]
```

子串复用了原字符串的一部分内存，只要一个字符串存在子串，那么字符串的内存就不会被释放。子字符串不适合长期保存，如果要长期使用这个内容，将它转为 String，即 `String(beginning)`，这样就有了自己的独立的内存。

#### 相等判断

就用 `==`，不像 Java，只要扩展字形集群相同，就是表现形式一样，内容一样的，就是 true。

```swift
// "Voulez-vous un café?" using LATIN SMALL LETTER E WITH ACUTE
let eAcuteQuestion = "Voulez-vous un caf\u{E9}?"

// "Voulez-vous un café?" using LATIN SMALL LETTER E and COMBINING ACUTE ACCENT
let combinedEAcuteQuestion = "Voulez-vous un caf\u{65}\u{301}?"

// 这两个常量表现形式一样，结果是 true
if eAcuteQuestion == combinedEAcuteQuestion {
    print("These two strings are considered equal")
}
// prints "These two strings are considered equal"
```

#### 前缀后缀判断

`hasPrefix(_:)` 和 `hasSuffix(_:)`。

#### Unicode 表示法

比如 `let dogString = Dog‼🐶`，有三种方式都可以访问 Unicode 值：
1. UTF-8 读取，`let utf8: String.UTF8View = dogString.utf8`，遍历它的每个值是 UInt8 的。
2. UTF-16 读取，`let utf16: String.UTF16View = dogString.utf16`，遍历它的每个值是 UInt16 的。
3. 两个 Unicode 标量值的集合，`let utf32: String.UnicodeScalarView = dogString.unicodeScalars`，遍历它的每个值用 UInt32 表示。

![](attachments/Pasted%20image%2020231108105143.png)
![](attachments/Pasted%20image%2020231108105155.png)
![](attachments/Pasted%20image%2020231108105203.png)

```swift
for codeUnit in dogString.utf8 {
    print("\(codeUnit) ", terminator: "")
}
// 68 111 103 226 128 188 240 159 144 182

// scalar 字符串插值后就是原来的字符
for scalar in dogString.unicodeScalars {
	print("\(scalar) ")
}
print("")
// D
// o
// g
// ‼
// 🐶

// 要获取对应的 Unicode 值，用 value 属性
for scalar in dogString.unicodeScalars {
    print("\(scalar.value) ", terminator: "")
}
// Prints "68 111 103 8252 128054 "
```

### 集合

有 Array、Set、Dictionary。赋值给变量，那么集合本身就是可变的，可以增删改数据，如果赋值给常量，就是不可变的。

不像 Java，Java 的变量声明没有可变不可变的说法，都是可变。

不像 Kotlin，Kotlin 无法是用 var/val 只是说明那个变量的指针是否可变，集合内部的内容都是不可变的，除非创建就是 mutable 的集合。

#### Array

有序可重复。类型写法 `Array<Element>`，简写成 `[Element]`。

##### 创建

```swift
var someInts = [Int]() // 简写形式的构造器
var threeDoubles = Array(repeating: 0.0, count: 3) // 完整形式的构造器
var someInts: [Int] = [] // 字面量
var shoppingList = ["Eggs", "Milk"] // 有值，可以类型推断
```

##### 修改/插入/删除

```swift
shoppingList[0] = "Six eggs"

// 可以用区间，个数不需要一样，原来的三个值被改成了两个值
shoppingList[4...6] = ["Bananas", "Apples"]

// 插入
shoppingList.insert("Maple Syrup", at: 0)

// 删除并返回被删除的元素
let mapleSyrup = shoppingList.remove(at: 0)
// 删除并返回最后一个元素
let apples = shoppingList.removeLast()
```

##### 遍历

```swift
for item in shoppingList {
	print(item)
}
```

`enumerated()` 包含元素的索引和值。

```swift
for (index, value) in shoppingList.enumerated() {
    print("Item \(index + 1): \(value)")
}
```

#### Set

元素要可以被哈希，和 Python，Java 一样。类型写法 `Set<Element>`，没有简写。

##### 创建

字面量的写法和 Array 一模一样，不明确写类型就是 Array，如果是 Set 必须明确写出来

```swift
var letters = Set<Character>()
letters.insert("a")
// letters 已经是 Set 了，赋值 [] 不会引起迷惑
letters = [] 
var favoriteGenres: Set<String> = ["Rock", "Classical", "Hip hop"]

// Set 不能省略，省了就当成 Array 了，但 String 可以推断可以省略
var favoriteGenres: Set = ["Rock", "Classical", "Hip hop"] 
```

##### 插入/删除

- `insert(_:)` 插入，不像 Array 有第二个 at 参数，因为它不是有序的，内部按 Hash 值组织数据，所以不需要一个位置参数。
- `remove(_:)` 删除并返回这个值，如果没有就返回 nil。
- `removeAll()` 删除所有值

##### 遍历

要以特定的顺序遍历 Set 的值，使用 sorted() 方法，它对元素使用 `<` 运算符排序。

```swift
for genre in favoriteGenres.sorted() {
	print("\(genre)")
}
```

##### 两个 Set 的操作和关系

- `intersection(_:)` 交集
- `union(_:)` 并集
- `symmetricDifference(_:)` 异或，并集减交集
- `subtracting(_:)` 差，一个 Set 减去其包含在另一个 Set 里的值

结果都是返回新的 Set，不会修改原来的 Set。

```swift
let oddDigits: Set = [1, 3, 5, 7, 9]
let evenDigits: Set = [0, 2, 4, 6, 8]
let singleDigitPrimeNumbers: Set = [2, 3, 5, 7]

oddDigits.union(evenDigits).sorted()
// [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

oddDigits.intersection(evenDigits).sorted()
// []

oddDigits.subtracting(singleDigitPrimeNumbers).sorted()
// [1, 9]

oddDigits.symmetricDifference(singleDigitPrimeNumbers).sorted()
// [1, 2, 9]
```

- `==` 是否相等，两个 Set 值是否完全一样
- `isSubset(of:)` 一个 Set 是否是另一个的子集
- `isStrictSubset(of:)` 严格意义的子集，就是不能相等
- `isSuperset(of:)` 一个 Set 是否是另一个的父集
- `isStrictSuperset(of:)` 严格意义的父集，就是不能相等
- `isDisjoint(with:)` 完全不相交，就是交集是空

```swift
let houseAnimals: Set = ["🐶", "🐱"]
let farmAnimals: Set = ["🐮", "🐔", "🐑", "🐶", "🐱"]
let cityAnimals: Set = ["🐦", "🐭"]
houseAnimals.isSubset(of: farmAnimals)
// true

farmAnimals.isSuperset(of: houseAnimals)
// true

farmAnimals.isDisjoint(with: cityAnimals)
// true
```

#### Dictionary

类型完整写法 `Dictionary<Key, Value>`，简写作 `[Key: Value]`，key 要可以哈希，和 Python，Java 这些都一样，因为底层数据结构都是哈希表。

##### 创建

```swift
// 构造器创建空字典
var namesOfIntegers = [Int: String]()

// 字面量创建空字典
namesOfIntegers: [String: String] = [:]

// 字面量创建。推断出来类型是 [String: String]
var airports = ["YYZ": "Toronto Pearson", "DUB": "Dublin"]
```

##### 读/写/改/删

- 读取 `let airportName = airports["DUB"]`，如果不存在这个键，返回 nil，Python 是直接报错。
- 下标添加或修改 `airports["LHR"] = "London"`，如果设置值为 nil，就相当于删除 `airports["APL"] = nil`
- `updateValue(_:forKey:)` 和下标语法功能一样，但它返回更新前的旧值，如果之前没有则返回 nil。`airports.updateValue("Dublin Airport", forKey: "DUB")`
- `removeValue(forKey:)` 同样是删除，比起下标区别也是返回被删除的值，如果之前没有则返回 nil。`let removedValue = airports.removeValue(forKey: "DUB")`

##### 遍历  
  
```swift  
// 遍历时每组键值对组成一个元组  
for (airportCode, airportName) in airports {  
    print("\(airportCode): \(airportName)")
}  
  
// 遍历所有的键  
for airportCode in airports.keys {  
    print("Airport code: \(airportCode)")
}  
  
// 遍历所有的值  
for airportName in airports.values {  
    print("Airport name: \(airportName)")
}  
```  
  
可以用 keys 或 values 属性来初始化一个新数组：  
  
```swift  
let airportCodes = [String](airports.keys)  
let airportNames = [String](airports.values)  
```

### 通用属性方法

- count 字符串或集合的长度
- isEmpty 判断字符串或集合是否为空
- `append(_:)` 在字符串末尾添加一个字符，在集合末尾添加一个元素
- `+/+=` 拼接字符串或集合，多行字符串拼接末尾不会自动加换行，要两个多行字符串分行得自己加空行
- `contains(_:)` 是否包含某个元素

## 错误

抛出错误

```swift
func makeASandwich() throws {
	...
	// 抛出具体的错误
	throw Error.OutOfCleanDishes
}
```

```swift
func canThrowAnError() throws -> String {
	// 异常继续抛出不处理
	try makeASandwich()						   
}
```

```swift
do { // 捕获异常处理掉
    try canThrowAnError()
} catch {
}

// 错误是个枚举，可能有多个情况
do {
    try makeASandwich()
} catch Error.OutOfCleanDishes {
	washDishes()
} catch Error.MissingIngredients(let ingredients) {
	buyGroceries(ingredients)
} catch is VendingMachineError {
   print("Couldn't buy that from the vending machine.")
} catch { // 如果不加这个，上面又没有匹配所有错误，错误将继续向高层抛出
	print("Unexpected error: \(error).")
}
```

try? 就是发生错误时是 nil，不发生错误就是函数返回值。

```swift
let x = try? someThrowingFunction()

// 上面的等价写法
let y: Int?
do {
    y = try someThrowingFunction()
} catch {
    y = nil
}
```

try! 是取消错误传递，就是调用一个抛出异常的函数，但是实际运行到这认为不会有异常。

```swift
// photo 不是可空类型
let photo = try! loadImage("./Resources/John Appleseed.jpg")
```

defer 语句延迟执行直到当前范围退出，就是写可以提前写，但是直到当前代码执行结束了才会延迟执行，功能类似 Java 的 finally。  
它可以写多个，越先写出来的越后执行。

和 throws 没有什么关系，普通函数也可以写 defer，也是一样的效果，只是用在 throws 里可以确保被执行到。

```swift
func test() throws {
    defer {
        print("defer first")
    }

    print("1")
    
    defer {
        print("defer second")
    }

    print("2")
}

try! test()
```

输出

```
1
2
defer second
defer first
```

```swift
enum VendingMachineError: Error {
    case invalidSelection
}

func test() throws {
    defer {
        print("defer first")
    }

    print("1")

    throw VendingMachineError.invalidSelection

    defer {
        print("defer second")
    }

    print("2")
}

try? test()
```

输出

```
1
defer first
```

输出 1 后抛出异常，然后执行前面已经写出来的 defer 里的内容。

## 运算符

一般的赋值，数学运算，比较运算符，逻辑运算符和其它语言一样。

三元运算符 `question ? answer1 : answer2` 和 Java 的写法一样。

合并空值运算符 `a ?? b`，如果可选项 a 有值则展开，如果是 nil，则返回默认值 b。表达式 a 必须是一个可选类型。表达式 b 必须与 a 的储存类型相同。

### 区间运算符

- 闭区间 `a...b` 即 `[a, b]`
- 半开区间 `a..<b` 即 `[a, b)`，如果 a  与 b  的值相等，那返回的区间将会是空的。

上面两种区间都有一种形式让区间朝一个方向尽可能的远，可以省略区间运算符一侧的值，叫单侧区间。

```swift
// 只有左侧，那右侧就是列表最后一个值
for name in names[2...] {
	print(name)
}
// Brian
// Jack

// 只有右侧，那左侧就是 0
for name in names[...2] {
	print(name)
}
// Anna
// Alex
// Brian
```

半开区间运算符同样可以有单侧形式，只需要写它最终的值。

```swift
// 左侧是 0，右侧是 2，不包含 2
for name in names[..<2] {
	print(name)
}
// Anna
// Alex
```

### 位运算

与 &，或 |，非 ~，异或 ^，左移 `<<`，右移 `>>`，和其它语言类似，由于有符号的负数是补码形式，所以右移的空位用符号位补齐，即负数时用 1 补齐，而无符号的，就都是用 0 补齐了。

### 溢出运算

默认的溢出会崩溃。比如 Int16.max + 1 就溢出了。溢出加 `&+`，溢出减 `&-`，溢出乘 `&*` 就是允许溢出，就是要取那个溢出后错误的值。

比如 `UInt8.max &+ 1` 结果是 0，UInt8.max 是 1111_1111，加一溢出后变成 0000_0000。

比如 `Int8.min &- 1`，Int8.min 是 1000_0000（-128），减一溢出后变成 0111_1111（127）。

### 运算符重载

```swift
struct Vector2D {
    var x = 0.0, y = 0.0
}

extension Vector2D {
	// 给这个结构体定义一个 +，需要是 static 的，加法是两个相同的东西，返回一个东西，所以两个 Vector2D 参数，返回一个 Vector2D
    static func + (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y + right.y)
    }
}
```

实现前缀或者后缀运算符，需要在声明运算符函数的时候在 func  关键字之前指定 prefix  或者 postfix  限定符，比如前缀 - 是个单目运算符，避免和二目运算符减法的 - 混淆。

```swift
extension Vector2D {
    static prefix func - (vector: Vector2D) -> Vector2D {
        return Vector2D(x: -vector.x, y: -vector.y)
    }
}
```

组合赋值运算符是将赋值运算符( = )与其它运算符进行结合。比如 +=，在实现的时候，需要把运算符的左参数设置成 inout  类型，因为这个参数的值会在运算符函数内直接被修改。

```swift
extension Vector2D {
	static func += (left: inout Vector2D, right: Vector2D) {
		// left 修改了，加上是输入输出参数，调用者传递的那个值就被改了
		// 加法 + 上面定义过了，直接用
		left = left + right 
	}
}
```

> 不能对默认的赋值运算符（ = ）进行重载，只有组合赋值运算符可以被重载。同样地，也无法对三元条件运算符 a ? b : c  进行重载。

等价运算符 `==` 和 `!=` 的实现，其中 `!=` 在标准库的默认实现是对 `==` 取反，所以只要实现 `==` 就可以了。

```swift
extension Vector2D: Equatable {
    static func == (left: Vector2D, right: Vector2D) -> Bool {
        return (left.x == right.x) && (left.y == right.y)
    }
}
```

[对于满足具体条件的结构体或枚举](#^c3b10c)只需要遵循 Equatable 后就有了 `==` 的实现，不需要自己重载运算符，除非想有特别的实现。

### 自定义运算符

新的运算符要在全局作用域内，使用 operator 关键字进行声明，同时还要指定 prefix（前缀）、infix（中缀） 或者 postfix（后缀） 限定符。

```swift
extension Vector2D {
	// 自定义一个前缀运算符，要修改自己，所以参数是 inout 的
    static prefix func +++ (vector: inout Vector2D) -> Vector2D {
        vector += vector
        return vector
    }

}

var toBeDoubled = Vector2D(x: 1.0, y: 4.0)
let afterDoubling = +++toBeDoubled
```

对于中缀运算符，要定义结合性，关键字 associativity：
- 值 left 表示和左边的值运算
- right 和右边的运算
- none 不能运算，不能和优先级相同的值放在一起

要定义优先级，用关键字 precedence，默认值是 100。

```swift
// 加法减法的优先级是 140，所以这个方法也用 140
infix operator +- { associativity left precedence 140 }

extension Vector2D {
    static func +- (left: Vector2D, right: Vector2D) -> Vector2D {
        return Vector2D(x: left.x + right.x, y: left.y - right.y)
    }

}

let firstVector = Vector2D(x: 1.0, y: 2.0)
let secondVector = Vector2D(x: 3.0, y: 4.0)
let plusMinusVector = firstVector +- secondVector
```

## 控制流  
  
### for in  
  
for in 在区间、字符串和集合中遍历。  
  
如果不需要序列的每一个值，可以使用下划线来取代遍历名以忽略值。  
  
```swift  
for _ in 1...5 {  
    print("*")}  
```  
  
stride(from:to:by:) 是半开区间，from 是起值，to 是终值，但不包含，by 是步长；类似的 `stride(from:through:by:)` 是闭区间，through 值可以包含。  
  
```swift  
for index in stride(from: 0, to: 10, by: 5) {  
    print("\(tickMark)", terminator: " ")
}  
// 0 5  
for index in stride(from: 0, through: 10, by: 5) {  
    print("\(tickMark)", terminator: " ")
}  
// 0 5 10  
```

### while

while 和 java 一样，只是它条件和 Python 一样，都没有括号。repeat while 和 java 的 do while 一个意思。

```swift
repeat {
    statements
} while condition
```

### if

和 Kotlin 一样，可以作为表达式，只是条件没有括号。

```swift
let weatherAdvice = if temperatureInCelsius <= 0 {
    "It's very cold. Consider wearing a scarf."
} else if temperatureInCelsius >= 30 {
    "It's really warm. Don't forget to wear sunscreen."
} else {
    "It's not that cold. Wear a T-shirt."
}
```

所有分支必须返回同样的类型，weatherAdvice 能自动推断出来是 String 类型。如果有分支返回 nil，那就无法推断类型了，必须声明。

```swift
let freezeWarning: String? = if temperatureInCelsius <= 0 {
    "It's below freezing. Watch for ice!"
} else {
    nil
}
```

因为 nil 可能是任意类型，所以要声明 `String`，下面是等价的写法

```swift
let freezeWarning = if temperatureInCelsius <= 0 {
    "It's below freezing. Watch for ice!"
} else {
    nil as String?
}
```

这样 freezeWarning 能够自动推断出来是 String? 类型。

### switch

同样既可以作为语句，也可以作为表达式。

```swift
switch approximateCount {
case 0, 1:
    naturalCount = "less tha 1"
case 2..<5:
    naturalCount = "a few"
case 5..<12:
    naturalCount = "several"
    fallthrough // 会继续执行到 default 里的语句
default:
    naturalCount = "many"
}
```

多个条件写在一个 case 里，不像 Java 那样自动贯穿，如果没有语句就往后面匹配，Swift 里如果写了 case，而没写执行语句就会报错。但如果要贯穿，要单独写 `fallthrough`。

case 的条件可以是一个区间。

case 的条件必须覆盖所有情况。

#### 元组匹配

可以匹配元组，每个值来匹配，匹配所有就用 `_`，单个值可以用区间。

```swift
let somePoint = (1, 1)
switch somePoint {
case (0, 0):
    print("(0, 0) is at the origin")
case (_, 0):
    print("(\(somePoint.0), 0) is on the x-axis")
case (0, _):
    print("(0, \(somePoint.1)) is on the y-axis")
case (-2...2, -2...2):
    print("(\(somePoint.0), \(somePoint.1)) is inside the box")
default:
    print("(\(somePoint.0), \(somePoint.1)) is outside of the box")
}
// prints "(1, 1) is inside the box"
```

#### 值绑定

可以将值绑定到变量或常量上，给函数用。值绑定同样复合多条语句，用逗号隔开。

```swift
let anotherPoint = (2, 0)
switch anotherPoint {
case (let x, 0):
    print("on the x-axis with an x value of \(x)")
case (0, let y):
    print("on the y-axis with a y value of \(y)")
case (let distance, 1), (1, let distance):
	print("\(x) \(y)")
case let (x, y):
    print("somewhere else at (\(x), \(y))")
}
// prints "on the x-axis with an x value of 2"
```

#### where

用 where 分句来检查额外的情况，就是匹配上再加条件去检查。

```swift
let yetAnotherPoint = (1, -1)
switch yetAnotherPoint {
case let (x, y) where x == y:
    print("(\(x), \(y)) is on the line x == y")
case let (x, y) where x == -y:
    print("(\(x), \(y)) is on the line x == -y")
case let (x, y):
    print("(\(x), \(y)) is just some arbitrary point")
}
```

### 控制转移语句

循环里可以用 continue/break，和其它语言一样。break 也可以用在 switch 里，就是立即结束整个 switch 的执行。

可以打标签，
```swift
gameLoop: while square != finalSquare {
	diceRoll += 1
    switch square + diceRoll {
    case finalSquare:
        break gameLoop // 加了标签是退出 while
    case let newSquare where newSquare > finalSquare:
        continue gameLoop
    default:
        break // 没加标签是退出 break
    }
}
```

### guard

功能与 if 有点类似，意思是说条件必须真才能继续后面的执行，它必须有一个 else 分支，就是为假就执行 else 里的语句，由于不执行 guard 后面的语句时，else 里必须有控制转移语句把逻辑转出去，比如 return，break， continue 或者 throw，或者可以调用一个不带有返回值的函数或者方法，比如 fatalError()。

```swift
func greet(person: [String: String]) {
    guard let name = person["name"] else {
	    // 如果条件为假，要转出去，所以 return 退出这个函数
        return
    }

	// 如果条件真，就能执行到这里
    print("Hello \(name)!")
}
```

## 检查 API 可用性

```swift
if #available(iOS 10, macOS 10.12, *) {
	// 可以用 iOS 10 以上的 api
} else {
	// 假如设备是 iOS9，就要用这里的逻辑
}
```

平台名称可以写 iOS，macOS 和 watchOS 这些，可以加版本号，比如 iOS 10，可以加更精确的小版本号，比如 macOS 10.10.3。

`*` 表示其它所有平台，相当于说如果在 iOS 10，macOS 10.12 以上，或其它平台（比如 watchOS），就进 if 里的语句。

使用 guard 检查时，会自动为后面的代码优化可用性信息。

```swift
@available(macOS 10.12, *)
struct ColorPreference {
    var bestColor = "blue"
}

guard #available(macOS 10.12, *) else {
	// 如果不符合，就到这里
	return "gray"
}

// gurad 条件为真，才能执行到这
let colors = ColorPreference()
return colors.bestColor
```

反向检测不可用性条件，下面两个检查效果一样。

```swift
if #available(iOS 10, *) {

} else {
    // Fallback code
}

if #unavailable(iOS 10) {
    // Fallback code
}
```

## 函数

func 定义函数，返回值用 `->`，比如  
  
```swift  
func greetAgain(person: String) -> String {  
    return "Hello again, " + person + "!"
}  
```  
  
### 参数与返回值  
  
1. 参数可以没有，可以有多个，如 `func sayHelloWorld()`，`func greet(person: String, alreadyGreeted: Bool)`  
2. 返回值可以有，可以没有，如果没有，就不用写 `->`，相当于返回类型是 Void，它其实是一个空的元组，实际返回可以写作 `()`。要返回多个值，封装成元组，如 `-> (min: Int, max: Int)`，`-> (Int, Bool)?`  
3. 如果函数体只有一行，那么这行隐式返回，可以省略 return 关键字  
  
#### 实际参数标签和形式参数名

定义函数是 `func someFunction(argumentLabel parameterName: Int)`，其中 argumentLabel 叫实际参数标签，parameterName 叫形式参数名。

调用函数是 `someFunction(argumentLabel: 1)` 写实际函数标签，后面跟着参数。

定义函数时可以省略实际参数标签，只保留形式参数名，这样形式参数名就是默认的实际参数标签，比如 `func someFunction(parameterName: Int)`，调用是 `someFunction(parameterName: 1)`，其实写的这个 parameterName 还是实际参数标签，只不过和形式参数名一样而已。

如果调用时不想写实际参数标签，比如直接 `someFunction(1)`，那定义必须写成 `func someFunction(_ parameterName: Int)`。

#### 默认参数  
  
和 Kotlin 一样。  
  
```swift  
func someFunction(parameterWithDefault: Int = 12) {  
}  
someFunction(parameterWithDefault: 6)  
someFunction()  
```  
  
#### 可变参数  
  
和 Java 一样，可变参数是数组类型，比如下面的 numbers 就是 [Double]。  
  
```swift  
func sum(_ numbers: Double...) -> Double {  
    var total: Double = 0    
    for number in numbers {        
	    total += number    
	}    
	return total
}  
sum(1, 2, 3, 4, 5)  
```  
  
因为有实际参数标签这机制，可变参数后面可以有参数，只是不能用 `_` 省略实际参数标签，必须有，这样调用时必须写实际参数标签，就能区分出来它不在前面的可变参数里。  
  
#### 输入输出形式参数  
  
其实就相当于传递引用，而不是传递值。由于要修改参数，所以不能传字面量或常量，必须是变量。函数定义类型前面加关键字 inout，调用时实际参数前加 &，表示取它的地址。  
  
- 输入输出形式参数不能有默认值  
- 可变形式参数不能标记为 inout- 如果一个形式参数标记了 inout，那么它们也不能标记 var 和 let 了  
  
```swift  
func swapTwoInts(_ a: inout Int, _ b: inout Int) {
    let temporaryA = a    
    a = b    
    b = temporaryA
}  
  
var someInt = 3  
var anotherInt = 107  
swapTwoInts(&someInt, &anotherInt)  
print("someInt is now \(someInt), and anotherInt is now \(anotherInt)")  
// prints "someInt is now 107, and anotherInt is now 3"  
```  
  
传递引用，函数内交换两个值，外面原来的变量里面的值也变了。
  
### 函数类型  
  
和 Kotlin 一样，函数也是对象，有自己的类型，可以作为参数和返回值。  
  
```swift  
func addTwoInts(_ a: Int, _ b: Int) -> Int {  
    return a + b
}  
```  
  
`func addTwoInts(_ a: Int, _ b: Int) -> Int` 的类型就是 `(Int, Int) -> Int`，如果无参无返回值，比如 `func printHelloWorld()`，类型就是 `() -> Void`。  
  
`var mathFunction: (Int, Int) -> Int = addTwoInts` 定义一个变量，类型是 `(Int, Int) -> Int`，所以可以用 addTwoInts 这个函数名来赋值，并且由于类型推断，可以直接写成 `var mathFunction = addTwoInts`  
可以将函数作为参数  
  
```swift  
func printMathResult(_ mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {  
    print("Result: \(mathFunction(a, b))")
}  
printMathResult(addTwoInts, 3, 5)  
```  
  
也可以作为返回值  
  
```swift
func stepForward(_ input: Int) -> Int {  
    return input + 1
}  
func stepBackward(_ input: Int) -> Int {  
    return input - 1
}  
func chooseStepFunction(backwards: Bool) -> (Int) -> Int {  
    return backwards ? stepBackward : stepForward
}  
```

### 内嵌函数  
  
内嵌函数在默认情况下在外部是被隐藏起来的，但却仍然可以通过包裹它们的函数来调用它们。  
  
包裹的函数也可以返回它内部的一个内嵌函数来在另外的范围里使用。  
  
```swift  
func chooseStepFunction(backward: Bool) -> (Int) -> Int {  
    // 内嵌的两个函数  
    func stepForward(input: Int) -> Int { 
	    return input + 1 
	}    
	func stepBackward(input: Int) -> Int { 
		return input - 1 
	}    
	// 返回内嵌的函数  
    return backward ? stepBackward : stepForward
}  
var currentValue = -4  
let moveNearerToZero = chooseStepFunction(backward: currentValue > 0)  
while currentValue != 0 {  
    print("\(currentValue)... ")    // 通过返回的名字调用内嵌函数  
    currentValue = moveNearerToZero(currentValue)
}  
print("zero!")  
```

### 闭包

闭包表达式语法的一般形式：  
  
```swift  
{ (parameters) -> (return type) in  
    statements
}  
```  
  
其中参数可以是常量形式参数，可以是变量形式参数，可以是输入输出形式参数，可以是可变形式参数（但只能放在最后面）。参数不能有默认值。  
  
比如数组有个 sorted(by:) 方法，参数是函数，意思是数组用这个函数来排序并返回排序后的数组。  
  
```swift  
let names = ["Chris","Alex","Ewa","Barry","Daniella"]  
func backward(_ s1: String, _ s2: String) -> Bool {  
    return s1 > s2
}  
var reversedNames = names.sorted(by: backward)  
```  
  
用闭包表达式改写：  
  
```swift  
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in  
    return s1 > s2
})  
```  
  
**精简 1**：如果 in 后语句只有一行，整体可以写在一行。  
  
```swift  
reversedNames = names.sorted(by: { (s1: String, s2: String) -> Bool in return s1 > s2 } )  
```  
  
**精简 2**：如果闭包表达式的参数和返回值类型可以推断出来，可以省略类型，参数括号也省略。  
  
```swift  
reversedNames = names.sorted(by: { s1, s2 in return s1 > s2 } )  
```  
  
**精简 3**：如果要执行的语句只有一条，可以省略 return 来隐式返回，普通函数也是如此。  
  
```swift  
reversedNames = names.sorted(by: { s1, s2 in s1 > s2 } )  
```  
  
**精简 4**：参数可以简写，内置 $0、$1、$2 这些来表示实际参数值，无论函数体是不是只有一行都可以用。  
  
```swift  
reversedNames = names.sorted(by: { $0 > $1 } )  
```  
  
**精简 5**：只适合这种特殊情况，因为 Swift 语言的 String 类型定义了运算符 `>`，这其实就相当于一个函数的名字，参数是两个 String，返回 Bool，所以直接将这个名字作为参数。  
  
```swift  
reversedNames = names.sorted(by: >)  
```

注意[循环强引用问题](Swift.md#解决闭包的循环强引用)

#### 尾随闭包  
  
函数的参数是函数时，可以用闭包表达式，参数位置的函数可以拿出来，类似函数体的写法。  
  
比如将 `reversedNames = names.sorted(by: { $0 > $1 })` 改写成 `reversedNames = names.sorted() { $0 > $1 }`，此时实际参数标签 `by` 就用不着写了。  
  
如果有多个函数参数，用尾随闭包，第一个函数参数的实际标签省略，但后面的不能省略，必须写上。比如  
  
```swift  
func loadPicture(from server: Server, completion: (Picture) -> Void, onFailure: () -> Void) {  
    if let picture = download("photo.jpg", from: server) {        
	    completion(picture)    
	} else {        
		onFailure()    
	}
}  
  
loadPicture(from: someServer) { picture in  
    someView.currentPicture = picture
} onFailure: { // 第二个参数要写上标签  
    print("Couldn't download the next picture.")
}  
```

#### 捕获值

内嵌函数也是一种闭包函数。
  
```swift  
func makeIncrementer(forIncrement amount: Int) -> () -> Int {  
    var runningTotal = 0    
    func incrementer() -> Int {        
	    runningTotal += amount        
	    return runningTotal    
	}    
	return incrementer
}  
let incrementByTen = makeIncrementer(forIncrement: 10)  
incrementByTen()  
//return a value of 10  
incrementByTen()  
//return a value of 20  
incrementByTen()  
//return a value of 30  
```  
  
incrementer 函数没有参数，用到的变量都是外部函数的，理论上 makeIncrementer 调用完毕后，里面的值都出栈了，就不能用了，但闭包函数能捕获外部的值，如果值不需要了，也会释放之前捕获的值。

#### 逃逸闭包  
  
之前内嵌函数，可以返回外面用，但闭包函数不行，如果想把参数传进来的闭包函数拿到外面去用，必须用关键字 `@escaping`。 
  
```swift  
func test(param: @escaping ()->Void) -> ()->Void {
    return param
}  
  
test{ print("Hello") }()  // Hello
```

逃逸闭包不会自动捕获 self，必须显式写出来。  
  
```swift  
var test: ()->Void = {}  
func testClosure(param: @escaping ()->Void) {  
    test = param
}  
  
func testNonescapingClosure(closure: () -> Void) {
    closure()
}  
  
class SomeClass {  
    var x = 10    
    func doSomething() { 
	    // 逃逸闭包，不会自动捕获 self，直接调 x，那就是一个没有定义过的变量，所以主动写 self.x       
	    testClosure { self.x = 100 } 
	    // 非逃逸闭包，可以自动捕获，即自动捕获了 self，所以说 x，就是 self.x
	    testNonescapingClosure { x = 200 }    
	}
}  

var sc = SomeClass()  
sc.doSomething()  
print(sc.x) // 200
test()  
print(sc.x) // 100
```  
  
testNonescapingClosure 的参数非逃逸，因为传给它的参数，直接内部执行了，所以第一次输出的 sc.x 是 200，testClosure 传递的函数被记录在 test 函数那里，调用一遍后，它将 sc.x 设置成了 100，所以输出 100。

除了 self.x 这种写法外，还可以将 self 放到闭包捕获列表，即 `testClosure { [self] in x = 100 }`

注意结构体和枚举不可以捕获 self。

#### 自动闭包
  
就是实际调用时不传递闭包函数，而是传普通的表达式，在传到函数后，会自动的封装成闭包函数，这个闭包函数没有参数，返回值就是参数执行后的返回值。  
  
像上面的 `testNonescapingClosure(closure: () -> Void)` 函数，最初这么调用 `testNonescapingClosure { x = 200 }`，但写成自动闭包 `testNonescapingClosure(closure: @autoclosure () -> Void)` 后，调用则必须写成 `testNonescapingClosure(closure: x = 200)`。如果同时还有逃逸，写一起就行 `testClosure(param: @autoclosure @escaping ()->Void)`。

## 复杂类型

### 枚举

```swift  
// 多个值可以写在一行  
enum Planet {  
    case mercury, venus, earth, mars, jupiter, saturn, uranus, neptune
}  
  
// 多个值可以写在多行  
enum CompassPoint {  
    case north    
    case south    
    case east    
    case west
}  
```  
  
枚举成员在被创建时不会分配一个默认的整数值。north，south，east 和 west 并不代表 0，1，2 和 3。  
  
```swift  
var directionToHead = CompassPoint.west // 类型自动推断  
directionToHead = .east // 点语法修改值，知道自己是哪个枚举后，不需要写枚举名字  
  
switch directionToHead { // switch 匹配枚举  
case .north:  
    print("Lots of planets have a north")
case .south:  
    print("Watch out for penguins")
case .east:  
    print("Where the sun rises")
case .west:  
    print("Where the skies are blue")
}  
```

枚举是值类型，比如下面的 directionToHead 和 currentDirection 互不影响。

```swift
directionToHead = .east
var currentDirection = directionToHead
currentDirection = .north 
if directionToHead == .east {
	print("directionToHead still is east")
}
```

#### 遍历枚举  
  
定义枚举时后面加个 `CaseIterable`，这样通过 `.allCases` 就可以遍历枚举。  
  
```swift  
enum Beverage: CaseIterable {  
    case coffee, tea, juice
}  
let numberOfChoices = Beverage.allCases.count  
print("\(numberOfChoices) beverages available") // "3 beverages available"  

for beverage in Beverage.allCases {  
    print(beverage)
}  
```

#### 关联值  
  
可以将各种类型的值和枚举成员关联起来，不同的枚举成员关联的值类型可以不同。

 ```swift  
enum Barcode {  
    case upc(Int, Int, Int, Int) // 关联元组  
    case qrCode(String) // 关联字符串  
}  
var productBarcode = Barcode.upc(8, 85909, 51226, 3)  
productBarcode = .qrCode("ABCDEFGHIJKLMNOP")  
  
switch productBarcode {  
case .upc(let numberSystem, let manufacturer, let product, let check):  
    print("UPC: \(numberSystem), \(manufacturer), \(product), \(check).")
case .qrCode(let productCode):  
    print("QR code: \(productCode).")}  
// Prints "QR code: ABCDEFGHIJKLMNOP."  
  
switch productBarcode {  
// switch 值绑定可以提到外面一次性绑定所有  
case let .upc(numberSystem, manufacturer, product, check):  
    print("UPC : \(numberSystem), \(manufacturer), \(product), \(check).")
case let .qrCode(productCode):  
    print("QR code: \(productCode).")
}  
// Prints "QR code: ABCDEFGHIJKLMNOP."  
```

#### 原始值  
  
与关联值不同，关联值类型可以随便，原始值类型必须一样，相当于默认值，此时定义枚举必须**在后面跟着原始值的类型**，比如下面定义 ASCIIControlCharacter 后要跟着 `: Character`，这样才能定义原始值。  
  
```swift  
enum ASCIIControlCharacter: Character {  
    case tab = "\t"    
    case lineFeed = "\n"    
    case carriageReturn = "\r"
}  
```  
  
##### 隐式指定的原始值  
  
> 可以用 rawValue 获取原始值。  
  
如果原始值是整数，没有分配时每个成员值都比前一个大 1，如果第一个也没有分配，就是 0。  
  
```swift  
enum Planet: Int {  
    case mercury, venus, earth=10, mars, jupiter, saturn, uranus, neptune
}  
  
print(Planet.mercury.rawValue) // 第一个值没有配置，默认 0
print(Planet.venus.rawValue) // 后面的如果没指定比前面大 1，所以结果是 1
print(Planet.earth.rawValue) // 指定了 10，那就是 10
print(Planet.mars.rawValue) // 还是比前面大 1，所以是 11
```  
  
如果原始值是字符串，那么隐式原始值就是这个成员的名称。  
  
```swift  
enum CompassPoint: String {  
    case north, south, east="EAST", west
}  
  
print(CompassPoint.north.rawValue) // 是自己的名字 north
print(CompassPoint.east.rawValue) // 是指定的 EAST
print(CompassPoint.west.rawValue) // 是自己的名字 west
```  
  
##### 从原始值初始化  
  
因为枚举中可能找不到那个值，所以原始值初始化器是一个可失败初始化器。
  
```swift  
var possiblePlanet = Planet(rawValue: 7) // 是 nil
possiblePlanet = Planet(rawValue: 11) // 是 Planet.mars
```

这是[可失败初始化器](#^94c8d9)。

#### 递归枚举  

比如下面的储存简单数学运算表达式的枚举，值的关联值也是枚举。可以在成员前加 indirect 关键字表明是递归的，或者在整个枚举上加关键字。  
  
```swift  
enum ArithmeticExpression {  
    case number(Int)    
    indirect case addition(ArithmeticExpression, ArithmeticExpression)    
    indirect case multiplication(ArithmeticExpression, ArithmeticExpression)
}  
  
// 或者  
  
indirect enum ArithmeticExpression {  
    case number(Int)    
    case addition(ArithmeticExpression, ArithmeticExpression)    
    case multiplication(ArithmeticExpression, ArithmeticExpression)
}  
```  
  
比如 `(5 + 4) * 2` 用递归枚举 ArithmeticExpression 来表示就是：  
  
```swift  
// 定义元素  
let five = ArithmeticExpression.number(5)  
let four = ArithmeticExpression.number(4)  
let sum = ArithmeticExpression.addition(five, four)  
let product = ArithmeticExpression.multiplication(sum, ArithmeticExpression.number(2))  
  
// 计算方法  
func evaluate(_ expression: ArithmeticExpression) -> Int {  
    switch expression {    
    case let .number(value):        
	    return value    
	case let .addition(left, right):        
		return evaluate(left) + evaluate(right) 
	case let .multiplication(left, right):        
		return evaluate(left) * evaluate(right)    
	}
}  
  
print(evaluate(product))  
```

### 结构体  
  
```swift  
struct Resolution {  
  var width = 0  
  var height = 0
}  
  
var resolution1 = Resolution() // 创建实例  
resolution1.width = 1080 // 修改属性  
print("\(resolution1.width) \(resolution1.height)") // 1080 0  
  
let resolution2 = Resolution(height: 480)  
print("\(resolution2.width) \(resolution2.height)") // 0 480  
  
let resolution3 = Resolution(width: 640, height: 480)  
print("\(resolution3.width) \(resolution3.height)") // 640 480  
```  
  
点语法读取修改属性，初始化器可以没有参数，可以用属性做参数。　
  
结构体是值类型，如下面的 cinema 和 resolution3 是两个实例，互不影响。  
  
```swift  
var cinema = resolution3  
cinema.width = 2048  
print("cinema width is \(cinema.width), resolution3 width is \(resolution3.width)")  
// cinema width is 2048, resolution3 width is 640  
```  

> String/Array/Dictionary 的底层实现是结构体，所以都是值类型。比如 Dictionary 在 Java 里类似概念是 Map，是引用，注意区分。

### 类  
  
语法和结构体很相似。  
  
```swift  
class VideoMode { // 类  
  var resolution = Resolution()  
  var interlaced = false  
  var frameRate = 0.0  
  var name: String?
}  
  
let someVideoMode = VideoMode()  
print("\(someVideoMode.frameRate)") // 读取属性  
someVideoMode.resolution.width = 1280 // 可以直接修改类属性的结构体的属性  
```  
  
类是引用类型，和其它编程语言一样。  
  
```swift  
let anotherVideoMode = someVideoMode  
someVideoMode.name = "1080i"  
print(anotherVideoMode.name) // 也是 1080i
```  
  
因为是引用类型，指向的是别的实际的对象，所以 anotherVideoMode 本身是常量，却可以修改它的属性值，确切的说，是修改了它指向的堆中的那个对象的属性值。结构体就不行了，要修改属性必须是变量 var，因为它是值类型，修改的就是自己。  
  
引用类型使用 `===` 和 `!==` 来判断是否引用的同一个实例。

### 属性

#### 存储属性

就像上面定义**类或结构体**里写的那些变量或常量，就是存储属性。

```swift
struct FixedLengthRange {
	var firstValue: Int
	let length: Int
}
let rangeOfThreeItems = FixedLengthRange(firstValue: 0, length: 3)
```

由于结构体是值类型，所以 rangeOfThreeItems 是常量，即便 firstValue 是变量，也不可以修改。

##### 延迟存储属性

就是只有调用时才算它的值。

```swift
class DataManager {
    lazy var importer = DataImporter()
    var data = [String]()
}
```

类的实例一创建，data 就是一个数组了，但是 importer 还是 nil，还没有指向任何东西，只有代码中 `DataManager().importer` 调用到它了，才会创建 DataImporter 对象。

#### 计算属性

**类、结构体和枚举**都可以有，本身不存储值，通过计算返回结果，提供 get 和可选的 set。

因为是计算别的变量而得，本身不固定，所以必须用 var 定义，即便没有 set。

```swift
struct Square {
	var size: Double = 2
	var total: Double {
		get {
			return size * 4
		}

		set(newTotal) {
			size = newTotal / 4
		}
	}
}

var square = Square()
print("total = \(square.total)") // total = 8.0
square.total = 80
print("size = \(square.size)") // size = 20.0
```

##### 简写

get 里如果只有一条语句，可以省略 return，和函数一样。set 可以不写参数名，默认是 newValue，即可以改成

```swift
get {
	 size * 4
}
set {
	size = newValue / 4
}
```

如果没有 set，是只读的，可以把 get 整体全都省略，变成

```swift
var total: Double {
	size * 4
	// or return size * 4
}
```

#### 属性观察者

```swift
class StepCounter {
	var totalSteps: Int = 0 {
		willSet(newTotalSteps) {
			print("About to set totalSteps to \(newTotalSteps)")
		}

		didSet(oldSteps) {
			if totalSteps > oldSteps {
				print("Added \(totalSteps - oldSteps) steps")
			}
		}
	}
}

let stepCounter = StepCounter()
stepCounter.totalSteps = 200
// About to set totalSteps to 200
// Added 200 steps

stepCounter.totalSteps = 360
// About to set totalSteps to 360
// Added 160 steps
```

观察属性值的变化，willSet 是设置之前调用，didSet 是新值被设置之后调用。

定义的存储属性，继承的存储属性、继承的计算属性可以用属性观察，定义的计算属性直接在 set 方法里面判断就行了。

##### 简写

和计算属性类型，willSet 如果不写一个名字，默认叫 newValue，didSet 如果不写名字，默认叫 oldValue。

```swift
willSet {
	print("About to set totalSteps to \(newValue)")
}

didSet {
	if totalSteps > oldValue {
		print("Added \(totalSteps - oldValue) steps")
	}
}
```

##### 其它

如果在 didSet 里去设置值，那么会取代刚设置的值。

```swift
class StepCounter {
	var totalSteps: Int = 0 {
		willSet(newTotalSteps) {
			print("About to set totalSteps to \(newTotalSteps)")
		}

		didSet(oldSteps) {
			if totalSteps > oldSteps {
				print("Added \(totalSteps - oldSteps) steps")
			}
			
			totalSteps *= 2
		}
	}
}

let stepCounter = StepCounter()
stepCounter.totalSteps = 200
print("total \(stepCounter.totalSteps)") // total 400，不是自己上面设置的 200 了
```

> 如果以输入输出形式参数传一个拥有观察者的属性给函数，willSet 和 didSet 观察者一定会被调用，因为函数用完了一定要写回属性。

#### 属性包装  
  
不同类、结构体的不同属性，如果需要有一种统一的设置，比如检查线程安全，限制范围等等，可以在调用时判断，设置属性观察者判断，但是这样太繁琐了，可以通过属性包装来统一配置。  
  
```swift  
@propertyWrapper // 要一个注解  
struct TwelveOrLess {  
    private var number = 0 // 要设置默认值，限制访问权限 private    
    var wrappedValue: Int { // 要一个名字叫 wrappedValue 的属性  
        get { return number }        
        set { number = min(newValue, 12) }    
    }
}  
  
struct SmallRectangle {  
    @TwelveOrLess var height: Int // 用上面的结构体注解这个属性  
}  
  
var rectangle = SmallRectangle()  
print(rectangle.height)  
// Prints "0"  
  
rectangle.height = 10  
print(rectangle.height)  
// Prints "10"  
  
rectangle.height = 24  
print(rectangle.height)  
// Prints "12"  
```  
  
> 如何用枚举、类来定义，官方文档没给例子，自己照结构体这写法不行。  
  
也可以不用 `@TwelveOrLess`，自己实现 get/set，即内部定义属性包装的属性，然后通过计算属性返回真正的属性。  
  
```swift  
struct SmallRectangle {  
    private var _height = TwelveOrLess()    
    private var _width = TwelveOrLess()    
    var height: Int {        
	    get { return _height.wrappedValue }  
	    set { _height.wrappedValue = newValue }    
	}
}  
```  
  
这么做要说好处就是对属性的判断逻辑依然是统一在 TwelveOrLess 里，不需要在不同地方写多次，但是依然有点繁琐，不如直接用注解。

##### 设定包装属性的初始值  
  
上面在 TwelveOrLess 内部给 number 设置初始值 0，然后 SmallRectangle 里使用 @TwelveOrLess 注解的属性就不能再设置默认值，如果要允许使用者自定义初始值，需要在包装中定义初始化器。  
  
```swift  
@propertyWrapper  
struct SmallNumber {  
    private var maximum: Int    
    private var number: Int  
    var wrappedValue: Int {        
	    get { return number }        
	    set { number = min(newValue, maximum) }
	}  
    // 初始化，值是 0，最大是 12    
    init() {        
	    maximum = 12        
	    number = 0    
	}  
    // 初始化，值是多少靠参数，最大值还是 12    
    init(wrappedValue: Int) {        
	    maximum = 12        
	    number = min(wrappedValue, maximum)    
	}  
    // 初始化，值大小和最大值都由参数控制  
    init(wrappedValue: Int, maximum: Int) {  
        self.maximum = maximum        
        number = min(wrappedValue, maximum)    
    }
}  
```  
  
```swift  
struct Rectangle {  
    @SmallNumber var width: Int // 使用 init 初始化，width 为 0，最大限制 12    
    @SmallNumber var height: Int = 1 // 使用 init(wrappedValue: Int) 初始化，height 为 1，最大限制 12    
    @SmallNumber(wrappedValue: 1) var height2: Int // 和上面一样，只是不同的写法  
    @SmallNumber(wrappedValue: 2, maximum: 48) var size: Int // 使用 init(wrappedValue: Int, maximum: Int) 初始化  
    @SmallNumber(maximum: 48) var size2: Int = 2 // 和上面一样，只是不同的写法  
}  
  
var rectangle = Rectangle()  
print(rectangle.width, rectangle.height, rectangle.height2, rectangle.size, rectangle.size2) // 0 1 1 2 2  
rectangle.width = 15  
rectangle.height = 15  
rectangle.size = 60  
print(rectangle.width, rectangle.height, rectangle.size) // 12 12 48  
```

##### 通过属性包装映射值  
  
对于包装值来说，包装属性可以通过定义映射值来暴露额外功能。  
  
```swift
@propertyWrapper  
struct SmallNumber {  
    private var number: Int    
    private(set) var projectedValue: Bool // 类型可以随便，但写法名称固定，这就是映射值  
  
    var wrappedValue: Int {        
	    get { return number }        
	    set {            
		    if newValue > 12 {   
		        number = 12      
		        projectedValue = true // 修改映射的值  
            } else {                
	            number = newValue                
	            projectedValue = false            
	        }        
	    }    
	}  
    
    init() {        
	    self.number = 0        
		self.projectedValue = false    
	}
}  
  
struct SomeStructure {  
    @SmallNumber var someNumber: Int  
    func printNumber() {        
	    // 内部直接用 $someNumber 取映射值，前面加个 $
        print($someNumber)    
    }
}  
var someStructure = SomeStructure()  
  
someStructure.someNumber = 4  
print(someStructure.$someNumber) // $someNumber 就是找对应的映射值 projectedValue
// Prints "false"  
  
someStructure.someNumber = 55  
print(someStructure.$someNumber)  
// Prints "true"  
  
someStructure.printNumber()  
// Prints "true"  
```

#### 类型属性
  
类似 Java 类的静态属性，属于所有类，而不是属于具体对象。对于存储属性，无法靠初始化器初始化，所以必须有一个默认值，在第一次被访问时延迟初始化，且只初始化一次。  
  
```swift  
struct SomeStructure {  
    static var storedTypeProperty = "Some value." // 存储类型属性  
    static var computedTypeProperty: Int { // 计算类型属性  
        return 1    
    }
}  
enum SomeEnumeration {  
    static var storedTypeProperty = "Some value."    
    static var computedTypeProperty: Int {        
	    return 6    
	}
}  
class SomeClass {  
    static var storedTypeProperty = "Some value."    
    static var computedTypeProperty: Int {        
	    return 27    
	}    
	// 类中的计算类型属性，前面加上 class，表示不仅是 static，还允许子类重写  
    class var overrideableComputedTypeProperty: Int {        
	    return 107    
	}
}  
  
// 类名引用读写  
print(SomeStructure.storedTypeProperty)  
// prints "Some value."  
SomeStructure.storedTypeProperty = "Another value."  
print(SomeStructure.storedTypeProperty)  
// prints "Another value."  
```

#### 通过闭包设置属性的默认值  
  
如果某个存储属性的默认值需要自定义或设置，可以使用闭包或全局函数来为属性提供默认值。当这个属性属于的**实例初始化**时，闭包或函数就会被调用，并且它的返回值就会作为属性的默认值。  
  
```swift  
class SomeClass {  
    let someProperty: SomeType = {        
	    return someValue    
	}()
}  
```  
  
```swift  
struct Chessboard {  
    let boardColors: [Bool] = {        
	    // 闭包进行计算，此时其它属性还没有被初始化，不能读取其它属性值，即便有默认值，也不能用 self。  
        var temporaryBoard = [Bool]()        
        var isBlack = false        
        for i in 1...8 {            
	        for j in 1...8 {          
		        temporaryBoard.append(isBlack)     
		        isBlack = !isBlack            
		    }            
		    isBlack = !isBlack        
		}        
		return temporaryBoard    
	}() // () 表示执行这个闭包函数  
    
    func squareIsBlackAt(row: Int, column: Int) -> Bool {        return boardColors[(row * 8) + column]    
	}
}  
  
// 创建了实例，闭包会立即执行  
let board = Chessboard()  
print(board.squareIsBlackAt(row: 0, column: 1))  
// Prints "true"  
print(board.squareIsBlackAt(row: 7, column: 7))  
// Prints "false"  
```

### 方法  
  
方法是关联了特定类型的函数。类，结构体以及枚举都能定义实例方法。  
  
每一个类的实例都隐含一个叫做 self 的属性，指实例自己。  
  
```swift  
struct Point {  
    var x = 0.0, y = 0.0    
    func isToTheRightOf(x: Double) -> Bool {        
	    return self.x > x    
	}
}  
```  
  
#### 在实例方法中修改值类型  
  
在方法中修改属性，类是可以的，但**结构体和枚举是值类型**，默认情况，它们不能被更改。  
  
如果要在特定的方法中修改结构体或者枚举的属性，需要将方法异变，就是在前面加上关键字 mutating，方法结束时修改的属性值会写入到原始的结构体或枚举中，并且将一个新的实例赋值给 self 属性。  
  
```swift  
struct Point {  
    var x = 0.0, y = 0.0    
    mutating func moveBy(x deltaX: Double, y deltaY: Double) {   
         x += deltaX        
         y += deltaY    
    }
}  
var somePoint = Point(x: 1.0, y: 1.0)  
somePoint.moveBy(x: 2.0, y: 3.0)  
print("The point is now at (\(somePoint.x), \(somePoint.y))")  
// prints "The point is now at (3.0, 4.0)"  
```  
  
结构体常量属性不能改变，无法调用异变方法。  
  
```swift  
let fixedPoint = Point(x: 3.0, y: 3.0)  
fixedPoint.moveBy(x: 2.0, y: 3.0)  
// this will report an error  
```  
  
异变方法可以指定整个实例给隐含的 self 属性。  
  
```swift  
struct Point {  
    var x = 0.0, y = 0.0    
    mutating func moveBy(x deltaX: Double, y deltaY: Double) { 
	    self = Point(x: x + deltaX, y: y + deltaY)    
	}
}  
  
enum TriStateSwitch {  
    case off, low, high    
    mutating func next() {        
	    switch self {        
	    case .off:            
		    self = .low        
		case .low:            
			self = .high        
		case .high:            
			self = .off        
		}    
	}
}  
  
var ovenLight = TriStateSwitch.low  
ovenLight.next()  
// ovenLight is now equal to .high  
ovenLight.next()  
// ovenLight is now equal to .off  
```

#### 类型方法  
  
和类型属性类似，类似 Java 的 static 方法。  
  
```swift  
struct LevelTracker {  
    static var highestUnlockedLevel = 1    
    var currentLevel = 1 
     
    // 和类型属性一样，加上 class 既表示 static，又表示可以被子类重写 
    class func unlock(_ level: Int) {        
	    if level > highestUnlockedLevel { 
		    highestUnlockedLevel = level 
		}    
	}  
	
    static func isUnlocked(_ level: Int) -> Bool {        
	    return level <= highestUnlockedLevel    
	}  
	
    @discardableResult    
    mutating func advance(to level: Int) -> Bool {        
	    // 普通方法调用类型方法，通过类名调用  
        if LevelTracker.isUnlocked(level) {            
	        currentLevel = level            
	        return true        
	    } else {            
		    return false        
		}    
	}
}  
```

### 下标

类、结构体和枚举可以定义下标，它可以作为访问集合、列表或序列成员元素的快捷方式。  
  
语法类似计算属性和方法的结合。subscript 后面加索引值类型，返回值，提供 get 和 set 方法。如果是只读的，没有 set，get 可以简写省略，和计算属性一样。  
  
```swift  
subscript(index: Int) -> Int {  
    get {        
	    // return an appropriate subscript value here    
	}    
	set(newValue) {        
		// perform a suitable setting action here    
	}
}  
  
// 只读，省略 get
subscript(index: Int) -> Int {  
    // return an appropriate subscript value here
}  
```  
  
比如：  
  
```swift  
struct TimesTable {  
    let multiplier: Int    
    subscript(index: Int) -> Int {        
	    return multiplier * index    
	}
}  

let threeTimesTable = TimesTable(multiplier: 3)  
print("six times three is \(threeTimesTable[6])")  
// prints "six times three is 18"  
```

#### 下标选项  

下标可以接收任意数量、任意类型的参数，可以是可变形式参数，不能用输入输出参数，不能有默认参数值，可以返回任意类型。  
  
可以定义多个下标，参数类型数量不同，就是下标重载。  
  
```swift  
struct Matrix {  
    let rows: Int, columns: Int    
    var grid: [Double]    
    init(rows: Int, columns: Int) {        
	    self.rows = rows        
	    self.columns = columns        
	    grid = Array(repeating: 0.0, count: rows * columns)    
	}        
	
	func indexIsValid(row: Int, column: Int) -> Bool {  
        return row >= 0 && row < rows && column >= 0 && column < columns    
    }    
    
    // 下标，两个参数，返回 Double    
    subscript(row: Int, column: Int) -> Double {        
	    get {            
		    assert(indexIsValid(row: row, column: column), "Index out of range")            
		    return grid[(row * columns) + column]        
		}        
		
		set {            
			assert(indexIsValid(row: row, column: column), "Index out of range")            
			grid[(row * columns) + column] = newValue        
		}    
	}
}  
  
var matrix = Matrix(rows: 2, columns: 2)  
matrix[1, 0] = 3.2  
```

#### 类型下标  
  
同样的用 static 关键字，用 class 表示既是类型下标，同时可被子类重写。  
  
```swift  
enum Planet: Int {  
    case mercury = 1, venus, earth, mars, jupiter, saturn, uranus, neptune    
    static subscript(n: Int) -> Planet {        
	    return Planet(rawValue: n)!    
	}
}  
  
let mars = Planet[4]  
print(mars)  
```

### 继承  
  
类可以向继承的属性添加属性观察器，以便在属性的值改变时得到通知，在子类初始化时会收到变化通知，不会在父类初始化时收到，因为毕竟对象实例是子类的。  
  
> Swift 类不会从一个通用基类继承，不像 Java，都是 Object 的子类。  
  
继承的语法和 Kotlin 一样，冒号后跟着父类。  
  
```swift  
class Vehicle {  
    var currentSpeed = 0.0    
    var description: String {        
	    return "traveling at \(currentSpeed) miles per hour"    
	}    

	func makeNoise() {  
	}
}  
  
class Bicycle: Vehicle {  
    var hasBasket = false
}  
  
let bicycle = Bicycle()  
bicycle.currentSpeed = 15.0 // 修改父类的属性  
print("Bicycle: \(bicycle.description)")  
// Bicycle: traveling at 15.0 miles per hour  
```

#### 重写  
  
需要加 override 关键字，super 表示父类的内容  

```swift
class Car: Vehicle {  
    var gear = 1  
    // 重写方法  
    override func makeNoise() {        
	    print("Choo Choo")    
	}  
	
    // 重写属性  
    override var description: String {        
	    return super.description + " in gear \(gear)"    
	}    
	
	// 重写属性观察器  
    override var currentSpeed: Double {        
	    didSet {            
		    gear = Int(currentSpeed / 10.0) + 1        
		}    
	}
}  
```

关于属性的重写：  
  
- 可以将父类的只读属性重写成可读写属性，但不能把可读写属性表示为只读属性，即可以放大，不能缩小。  
- 如果重写了属性的 setter，必须重写 getter。常量存储属性和只读的计算属性这种不可以设置值的属性，不能在子类添加属性观察器。  
- 不能为一个属性同时提供重写的 setter 和重写的属性观察器，因为如果重写 setter，在 setter 里就可以监听变化。  
  
#### 阻止重写  
  
加 final 表示不允许重写，和 Java 类似。比如 final var，final func，final class func，final subscript。如果在 class 前加 final 那表示类不允许被继承。

### 初始化

类和结构体都一样。

> 存储属性分配默认值，或者在初始化器里设置初始值时，不会调用任何属性监听器。

```swift
struct Celsius {
    var temperatureInCelsius: Double
    // 无参初始化
    init() {
        temperatureInCelsius = 32.0
    }
    // 带参数初始化
    init(fromFahrenheit fahrenheit: Double) {
        temperatureInCelsius = (fahrenheit - 32.0) / 1.8
    }
    // 无实际参数标签
    init(_ kelvin: Double) {
        temperatureInCelsius = kelvin - 273.15
    } 
}
```

可选项的默认值是 nil，可以不初始化。

```swift
class Celsius {
	var response: String?  
}
```

可以在初始化器中初始化一个常量，在初始化方法结束之前，可以随便设置。

```swift
struct Celsius {
    let text: Double
    init(text: String) {
        self.text = text
        self.text = 5
        self.text = text + 2
    }
}
```

#### 默认初始化器

没有提供初始化器的结构体和类，有一个默认的初始化器来给所有的属性提供默认值。

##### 类的默认初始化器

```swift
class ShoppingListItem {
    var name: String? // 默认值 nil
    var quantity = 1
    var purchased = false
}
var item = ShoppingListItem()
```

所有属性都有默认值，又由于没有父类，自动的获得了一个默认的初始化器。

##### 结构体的默认初始化器

和类不同，结构体的默认初始化器会接收存储属性，即使这属性有默认值。

```swift
struct Size {
    var width = 0.0, height = 0.0
}
let twoByTwo = Size(width: 2.0, height: 2.0)
```

但是调用时可以省略任何拥有默认值的属性。

```swift
let zeroByTwo = Size(height: 2.0) // 省略 width，默认值 0.0
let zeroByZero = Size() // width，height 都省略，都用默认值
```

#### 值类型的初始化器委托  
  
值类型没有继承的问题，所以委托就是用 `self.init` 委托给自己的其它初始化器。如果自定义了初始化器，默认的初始化器就不能调了，除非明确写出来。  
  
```swift  
struct Rect {  
    var origin = Point()    
    var size = Size()    
    init() {} // 默认的初始化器，写出来了才能调用  
    init(origin: Point, size: Size) {        
	    self.origin = origin        
	    self.size = size    
	}    
	init(center: Point, size: Size) {        
		let originX = center.x - (size.width / 2)  
		let originY = center.y - (size.height / 2)        
		// 委托给其它的初始化器  
        self.init(origin: Point(x: originX, y: originY), size: size)    
    }
}  
```

#### 类的继承和初始化

所有类的存储属性，包括从它的父类继承的所有属性，都必须在初始化期间分配初始值。
  
```swift  
// 指定初始化器  
init(parameters) {  
    statements
}  
  
// 便捷初始化器，前面加一个关键字 convenience
convenience init(parameters) {  
    statements
}  
```  
  
- 一个类至少要有一个指定初始化器，通常也只有一个指定初始化器
- 如果有默认初始化器，它总是类的指定初始化器 
- 指定初始化器向上委托，即内部调用父类的指定初始化器
- 便捷初始化器如果不需要可以不提供，它横向委托，就是调用别的便捷初始化器，或者调用自己的指定初始化器，总之任何一个便捷初始化器最终必然调用到它的指定初始化器
  
```swift  
class Food {  
    var name: String    
    init(name: String) {        
	    self.name = name    
	}    
	convenience init() { // 调用上面的指定初始化器  
        self.init(name: "[Unnamed]")    
    }
}  
```  
  
- 子类没有定义**任何**指定初始化器，将自动继承父类所有的指定初始化器。  
- 如果子类通过自动继承或者主动重写实现了所有的父类指定初始化器，那么将自动继承所有的父类便捷初始化器。  
  
```swift  
class RecipeIngredient: Food {  
    var quantity: Int    
    init(name: String, quantity: Int) {        
	    self.quantity = quantity        
	    super.init(name: name) // 调用父类的指定初始化器  
    }    
    // 父类指定初始化器是 init(name: String)，是重写了，加 override 关键字  
    override convenience init(name: String) {    
        self.init(name: name, quantity: 1)    
    }
}  
```  
  
RecipeIngredient 用便捷初始化器重写实现了 Food 的指定初始化器，所以自动继承父类便捷初始化器，因此 Food 的便捷初始化器 init() 也被继承了。

#### 可失败初始化器  
  
可失败初始化器不能和初始化器的参数类型和名称一样。  
  
init 后加 ? 表示，用 `return nil` 明确表示初始化失败。  
  
```swift  
struct Animal {  
    let species: String    
    init?(species: String) {        
	    if species.isEmpty { 
		    return nil 
		}        
		self.species = species    
	}
}  
```  
  
```swift  
let someCreature = Animal(species: "Giraffe")  
if let giraffe = someCreature {  
    print("An animal was initialized with a species of \(giraffe.species)")
}  
// prints "An animal was initialized with a species of Giraffe"  
  
let anonymousCreature = Animal(species: "") // 初始化失败  
// anonymousCreature is of type Animal?, not Animal  
if anonymousCreature == nil {  
    print("The anonymous creature could not be initialized")
}  
// prints "The anonymous creature could not be initialized"  
```

##### 枚举的可失败初始化器  
  
```swift  
enum TemperatureUnit {  
    case Kelvin, Celsius, Fahrenheit    
    init?(symbol: Character) {        
	    switch symbol {        
	    case "K":            
		    self = .Kelvin        
		case "C":            
			self = .Celsius        
		case "F":            
			self = .Fahrenheit        
		default:            
			return nil        
		}    
	}
}  
```  
  
```swift  
let fahrenheitUnit = TemperatureUnit(symbol: "F")  
if fahrenheitUnit != nil {  
    print("This is a defined temperature unit, so initialization succeeded.")
}  
// prints "This is a defined temperature unit, so initialization succeeded."  
  
let unknownUnit = TemperatureUnit(symbol: "X")  
if unknownUnit == nil {  
    print("This is not a defined temperature unit, so initialization failed.")
}  
// prints "This is not a defined temperature unit, so initialization failed."  
```

上面和普通类的可失败初始化器没什么两样，枚举主要是，如果带有原始值，会自动获得一个可失败初始化器 `init?(rawValue:)` ^94c8d9

##### 重写可失败初始化器

父类的可失败初始化器，子类可以写可失败初始化器重写，也可以写非可失败初始化器重写。父类的非可失败初始化器，子类要重写也必须非可失败。  
  
如果子类的非可失败初始化器要委托调用父类的可失败初始化器，则必须强制展开。  
  
```swift  
class Document {  
    var name: String?    
    init() {}    
    init?(name: String) {        
	    self.name = name        
	    if name.isEmpty { 
		    return nil 
		}   
	}
}  
  
class AutomaticallyNamedDocument: Document {  
    // 非可失败重写非可失败  
    override init() {        
	    // 强制展开委托调用父类的可失败初始化器  
        super.init(name: "[Untitled]")!    
    }    
    
    // 非可失败重写可失败  
    override init(name: String) {        
	    super.init()        
	    if name.isEmpty {            
		    self.name = "[Untitled]"        
		} else {            
			self.name = name        
		}    
	}
}  
```  
  
对于可失败初始化器 init!，可以用 init? 重写 init!，反之亦然。可以在 init? 委托调用 init!，反之亦然。也可以在普通的 init 中委托调用 init!，不需要像 init? 那样强制展开。

#### 必要初始化器
  
在类的初始化器前添加 required，表示如果子类要写初始化器，则必须重写它。  
  
```swift  
class SomeClass {  
    required init() {        
	    // initializer implementation goes here    
	}
}  
class SomeSubclass: SomeClass {  
}  
```  
  
子类继承了 init()，如果不需要重写，这样就行了，假如子类希望有一个初始化方法   

```swift  
class SomeSubclass: SomeClass {  
   init(name: String) {   
   }
}  
```  
  
这样就不行了，因为如果子类要自己写初始化器，就必须重写父类的必要初始化器，所以改成  
  
```swift  
class SomeSubclass: SomeClass {  
   init(name: String) {   
   }   
   required init() {   
   }
}  
```  
  
子类重写的也必须用 required，这样自己的子类也是这个规则，同时不需要 override 关键字。

### 反初始化

只有类有，实例被释放时调用。每个类只能有一个反初始化器，不接收任何形式参数，所以也不需要写圆括号。

```swift
deinit {	
}
```

反初始化器会在实例被**释放之前**自动调用，不能手动调用。可以被子类继承，先执行完子类的 deinit，再执行父类的 deinit。

```swift
class SomeClass {
    deinit {
        print("class deinit")
    }
}

var c: SomeClass? = SomeClass()
c = nil // 释放了，执行 deinit
```

## 可选链

```swift
instance?.method() != nil
```

函数如果有返回值 String，那么可选调用返回的是 String?，如果函数本身并没有返回值，那么结果就是 Void?，如果 instance 是 nil，method **没有被调用**，这个结果是 nil，如果 instance 不是 nil，调用成功了，结果是 Void，不是 nil，所以可以与 nil 比较来判断是否调用成功。

```swift
instance?.field = 3
```

可选链可以用来设置值，如果 instance 为 nil，就什么都不做，如果不是 nil，它里面的 field 属性就被设置了值。

```swift
(instance?.field = 3) != nil
```

并且可选链设置值也可以和 nil 对比，因为可选链设置属性的返回值都是 Void?，如果不为 nil 表示设置成功。

## 类型转换

`item is Class` 如果实例是这个类型的，就返回 true。

`item as? Class` 转换成子类型，如果确定不会出错，可以直接用 `item as! Class`。

Swift 为不确定的类型提供了两种特殊的类型别名：

- AnyObject 可以表示任何类类型的实例。  
- Any 可以表示任何类型，包括**函数类型，可选类型**。

```swift
var things = [Any]()
things.append(0)
things.append(0.0)
things.append(3.14159)
things.append("hello")
things.append((3.0, 5.0))
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman"))
things.append({ (name: String) -> String in "Hello, \(name)" })

let optionalNumber: Int? = 3
things.append(optionalNumber)        // Warning
// 如果要在 Any 值中使用可选项，需要用 as 显示转换成 Any
things.append(optionalNumber as Any) // No warning|

for thing in things {
	switch thing {
	case 0 as Int:
		print("zero as an Int")
	case 0 as Double:
		print("zero as a Double")
	case let someDouble as Double where someDouble > 0:
        print("a positive double value of \(someDouble)")
    case is Double:
        print("some other double value that I don't want to print")
    case let stringConverter as (String) -> String:
        print(stringConverter("Michael"))
    default:
        print("something else")
    }
}
```

## 协议  
  
```swift  
protocol SomeProtocol {  
    // protocol definition goes here
}  
class SomeClass: SomeSuperclass, FirstProtocol, AnotherProtocol {  
    // class definition goes here
}  
```  
  
有点类似 Java 的接口，定义协议用关键字 protocol，遵循协议就是在名字后加冒号加协议名，如果有父类，先写父类再写协议，如果遵循多个协议，用逗号隔开。  
  
### 属性要求  
  
协议*可以*要求所有遵循该协议的类型提供特定名字和类型的实例属性或类型属性，同时要求一个属性必须明确是可读的或可读的和可写的。  
  
协议要求可读，遵循协议时可以可写，协议要求可读且可写，遵循协议时就不能用常量，因为常量不能写。协议要求是类型属性，遵循协议用 static 或 class 都可以。  
  
```swift  
protocol SomeProtocol {  
    var mustBeSettable: Int { get set } // 要求有一个名字叫 mustBeSettable 的属性，可读也可写  
    var doesNotNeedToBeSettable: Int { get } // 要求有一个名字叫 doesNotNeedToBeSettable 的属性，只要可读即可  
    static var someTypeProperty: Int { get set } // 要求有个名字叫 someTypeProperty 的可读可写的类型属性  
}  
  
struct SomeStruct: SomeProtocol {  
    var doesNotNeedToBeSettable: Int    
    // ... 其它属性  
}  
let ss = SomeStruct(doesNotNeedToBeSettable: 2)  
  
class SomeClass: SomeProtocol {  
    var doesNotNeedToBeSettable: Int {        
	    return 5    
	}
}  
```  
  
### 方法要求  
  
协议可以要求采纳的类型实现指定的实例方法和类方法，不需要大括号和方法的主体。  
  
```swift  
protocol RandomNumberGenerator {  
    func random() -> Double    
    static func someTypeMethod()
}  
  
class LinearCongruentialGenerator: RandomNumberGenerator {  
    func random() -> Double {        
	    return 1.2    
	}    
	func someTypeMethod() {    
	}
}  
```  
  
### 异变方法要求  
  
```swift  
protocol Togglable {  
    mutating func toggle()
}  
```  
  
方法是异变的，像结构体或枚举这种值类型，实现协议时也要写 mutating 并且修改 self。  
  
```swift  
enum OnOffSwitch: Togglable {  
    case off, on    
    
    mutating func toggle() {        
	    switch self {        
	    case .off:            
		    self = .on        
		case .on:            
			self = .off        
		}    
	}
}  
```  
  
如果类实现协议，不需要写 mutating 关键字。  
  
### 初始化器要求  
  
协议可以要求遵循协议的类型实现指定的初始化器。  
  
```swift  
protocol SomeProtocol {  
    init(someParameter: Int)
}  
```  
  
有点特殊，类实现时必须加 required 将这个初始化器变成必要初始化器，这样保证自己的子类也要实现这个初始化器。但有个例外，*如果类是 final 的，不允许继承，就不需要 required 来变成必要初始化器了*。  
  
```swift  
class SomeClass: SomeProtocol {  
    required init(someParameter: Int) {        
	    // initializer implementation goes here    
	}
}  
```  
  
如果一个子类重写了父类指定的初始化器，并且遵循协议实现了初始化器要求，那么就要为这个初始化器的实现添加 required 和 override 两个修饰符：  
  
```swift  
protocol SomeProtocol {  
    init()
}  
  
class SomeSuperClass {  
    init() {        
	    // initializer implementation goes here    
	}
}  
  
class SomeSubClass: SomeSuperClass, SomeProtocol {  
    // "required" from SomeProtocol conformance; "override" from SomeSuperClass    
    required override init() {        
	    // initializer implementation goes here    
	}
}  
```  
  
协议里可以定义可失败初始化器，遵循协议的类型可以用可失败或不可失败的初始化器来满足。协议里定义的不可失败初始化器，遵循协议的类型也要是不可失败初始化器或者用隐式展开的可失败初始化器。

### 使用综合实现来采纳协议  
  
Swift 在大多数简单情况下能自动提供 Equatable、Hashable 以及 Comparable 协议遵循。  
  
对于自定义的复杂类型，如果满足下面的条件，只需要遵循 Equatable 协议就可以自动实现 `==`、`!=` 的实现。   ^c3b10c
  
- 只包含遵循 Equatable 协议的存储属性的结构体；  
- 只关联遵循 Equatable 协议的类型的枚举；  
- 没有关联类型的枚举。  
  
类似的，如果满足下面的条件，只要遵循 Hashable 协议也会自动实现 hash(into:)。  
  
- 只包含遵循 Hashable 协议的存储属性的结构体；  
- 只关联遵循 Hashable 协议的类型的枚举；  
- 没有关联类型的枚举。  
  
如果枚举没有原始值，或者关联值类型遵循了 Comparable 协议，那么这个枚举遵循 Comparable 协议后，就可以使用比较运算符，比如：  
  
```swift  
enum SkillLevel: Comparable {  
    case beginner    
    case intermediate    
    case expert(stars: Int)
}  
var levels = [SkillLevel.intermediate, SkillLevel.beginner, SkillLevel.expert(stars: 5), SkillLevel.expert(stars: 3)]
for level in levels.sorted() {  
    print(level)
}  
// Prints "beginner"  
// Prints "intermediate"  
// Prints "expert(stars: 3)"  
// Prints "expert(stars: 5)"  
```  
  
### 协议继承  
  
协议可以继承一个或者多个其他协议并且可以在它继承的基础之上添加更多要求。  
  
```swift  
protocol InheritingProtocol: SomeProtocol, AnotherProtocol {  
    // protocol definition goes here
}  
```  
  
### 类专用的协议  
  
通过添加 AnyObject 关键字到协议的继承列表，就可以限制协议只能被类类型采纳。  
  
```swift  
protocol SomeClassOnlyProtocol: AnyObject, SomeInheritedProtocol {  
    // class-only protocol definition goes here
}  
```

### 协议组合  
  
要求一个类型一次遵循多个协议，用 & 连接。  
  
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
func wishHappyBirthday(to celebrator: Named & Aged) {  
    print("Happy birthday, \(celebrator.name), you're \(celebrator.age)!")
}  
let birthdayPerson = Person(name: "Malcolm", age: 21)  
wishHappyBirthday(to: birthdayPerson)  
// Prints "Happy birthday, Malcolm, you're 21!"  
```  
  
wishHappyBirthday 的参数类型 Named & Aged 表示参数需要同时遵循这两个协议。

### 可选协议  
  
就是这个协议定义的东西是可选的，遵循这个协议的类可以实现它，也可以不实现。协议本身前面加上 @objc，里面可选的东西前面加 @objc optional。  
  
```swift  
@objc protocol CounterDataSource {  
    @objc optional func increment(forCount count: Int) -> Int    
    @objc optional var fixedIncrement: Int { get }    
    var name: String { get } // 也可以有非可选的，必须实现的  
}  
```  
  
只能是 OC 语言的类或其它 @objc 协议遵循，结构体或枚举不能遵循采纳它。  
  
```swift  
class ThreeSource: NSObject, CounterDataSource {  
    let fixedIncrement = 3 // 实现可选的属性 
    var name = "abc" // 实现必须实现的属性
}  
```  
  
比如上面的 ThreeSource，首先继承 OC 的 NSObject，然后协议里两个可选的，只实现了可选的属性，没有实现可选的方法。  
  
```swift  
class Counter {  
    var count = 0    
    var dataSource: CounterDataSource?    
    func increment() {        
	    if let amount = dataSource?.increment?(forCount: count) {            
		    count += amount        
		} else if let amount = dataSource?.fixedIncrement {            
			count += amount        
		}    
	}
}  
  
var counter = Counter()  
counter.dataSource = ThreeSource()  
for _ in 1...4 {  
    counter.increment()    
    print(counter.count)
}  
```  
  
上面 Counter 类里 dataSource 本身是可选的，所以调用它的 increment 方法用 dataSource?.increment，第二 increment 方法可能也没有实现，所以方法后面也加个 ?，用 dataSource?.increment?(forCount: count)。无论 dataSource 为 nil，还是没有实现 increment 方法，都会导致 amount 为 nil，然后去调用 dataSource?.fixedIncrement，如果 fixedIncrement 属性也没有实现，那么 amount 依然为 nil。

## 扩展  
  
扩展为现有的类、结构体、枚举类型、或协议添加新功能。也包括了为无访问权限的源代码扩展类型的能力。  
  
使用 extension 关键字来声明扩展。  
  
### 计算属性  
  
扩展可以向已有的类型添加计算实例属性和计算类型属性。但不能添加存储属性，也不能向已有的属性添加属性观察者。  
  
```swift  
extension Double { // 扩展 Double 类型，多加 5 个计算属性  
    var km: Double { return self * 1_000.0 }    
    var m: Double { return self }    
    var cm: Double { return self / 100.0 }    
    var mm: Double { return self / 1_000.0 }    
    var ft: Double { return self / 3.28084 }
}  

let oneInch = 25.4.mm  
print("One inch is \(oneInch) meters")  
// Prints "One inch is 0.0254 meters"  
let threeFeet = 3.ft  
print("Three feet is \(threeFeet) meters")  
// Prints "Three feet is 0.914399970739201 meters"  
```  
  
### 初始化器  
  
扩展能为类添加新的便捷初始化器，但是不能为类添加指定初始化器或反初始化器。  

对于值类型，比如结构体，对于存储属性，是有默认的初始化器的，扩展的初始化器可以调用这个默认的初始化器，但如果自定义了，相当于默认的初始化器不存在了，就不能用那个默认的了。 
  
```swift  
struct Size {  
    // 所有属性有默认值，它将自动接收一个默认的初始化器和一个成员初始化器。  
    var width = 0.0, height = 0.0
}  

struct Size2 {  
    var width = 0.0, height = 0.0
    init(width: Double) {
	    self.width = width
	    self.height = width * 2
    }
}  
```  
  
```swift  
extension Size {  
    init(size: Double) {        
	    // 可以调用默认的初始化器  
        self.init(width: size, height: size)    
    }
}  
extension Size2 {  
    init(size: Double) {        
	    // 出错，因为 Size2 自定义了初始化器，就不存在这个默认的了
        self.init(width: size, height: size)    
    }
}  
```  
  
### 方法  
  
扩展可以为已有的类型添加新的实例方法和类型方法，**但是不能重写已有的方法**。可以扩展异步方法，修改本身。  
  
  
```swift  
extension Int {  
    func repetitions(task: () -> Void) {        
	    for _ in 0..<self {            
		    task()        
		}    
	}  
    // 扩展的异变方法，修改 self    
    mutating func square() {        
	    self = self * self    
	}
}  
  
3.repetitions {  
    print("Hello!")
}  
// Hello!  
// Hello!  
// Hello!  
  
var someInt = 3  
someInt.square()  
// someInt is now 9  
```  
  
### 下标  
  
扩展能为已有的类型添加新的下标。  
  
```swift  
extension Int {  
    subscript(digitIndex: Int) -> Int {        
	    var decimalBase = 1        
	    for _ in 0..<digitIndex {            
		    decimalBase *= 10        
		}        
		return (self / decimalBase) % 10    
	}
}  
746381295[0]  
// returns 5  
746381295[1]  
// returns 9  
```  
  
### 内嵌类型  
  
扩展可以为已有的类、结构体和枚举类型添加新的内嵌类型。  
  
```swift  
extension Int {  
    enum Kind { // 扩展一个内嵌类型  
        case negative, zero, positive    
    }    
    
    var kind: Kind { // 扩展一个属性  
        switch self {        
        case 0:            
	        return .zero        
	    case let x where x > 0:            
		    return .positive        
		default:            
			return .negative        
		}    
	}
}  
  
func printIntegerKinds(_ numbers: [Int]) {  
    for number in numbers {        
	    switch number.kind { // 使用扩展属性  
        case .negative: // 因为 number.kind 是 Int.Kind 类型，所以 case 后面不需要再写类型，直接写 .negative 就行  
            print("- ", terminator: "")        
        case .zero:            
	        print("0 ", terminator: "")        
	    case .positive:            
		    print("+ ", terminator: "")        
		}    
	}    
	print("")
}  
printIntegerKinds([3, 19, -27, 0, -6, 0, 7])  
// Prints "+ + - 0 - 0 + "  
```  
  
### 扩展协议  
  
扩展一个已经存在的类型来采纳和遵循一个新的协议，可以添加新的属性、方法和下标到已经存在的类型。  
  
```swift  
protocol TextRepresentable {  
    var textualDescription: String { get }
}  
  
class Dice {  
  
}  
  
let d1 = Dice()  
  
// 扩展存在的类型，让它遵循协议  
extension Dice: TextRepresentable {  
    var textualDescription: String {        
	    return "A dice"    
	}
}  
  
// 任何 Dice 实例现在都可以被视作 TextRepresentable
let d2 = Dice()  
  
// 扩展之前创建的对象 d1 也可以用扩展之后才有的属性  
print(d1.textualDescription)  
print(d2.textualDescription)  
```
  
### 使用扩展声明采纳协议  
  
如果一个类型已经遵循了协议的所有需求，但是还没有声明它采纳了这个协议，可以让通过一个空的扩展来让它采纳这个协议  
  
```swift  
struct Hamster {  
    var name: String    
    var textualDescription: String {        
	    return "A hamster named \(name)"    
	}
}  
// Hasmster 其实内容已经遵循了协议，显式的声明一下  
extension Hamster: TextRepresentable {}  
  
// Hamster 实例现在可以用在任何 TextRepresentable 类型可用的地方：  
let simonTheHamster = Hamster(name: "Simon")  
let somethingTextRepresentable: TextRepresentable = simonTheHamster  
print(somethingTextRepresentable.textualDescription)  
// Prints "A hamster named Simon"  
```  
  
### 协议扩展  
  
让一个类遵循某种协议，那是扩展的类，这里是扩展协议本身。比如 TextRepresentable，扩展协议本身让它多个方法。需要注意的是协议本身的方法是空的，没有大括号的，但是扩展时是有具体实现的，这样就不需要修改之前遵循协议的类、结构体、枚举中的实现。  
  
```swift  
extension TextRepresentable {  
    func randomBool() -> Bool {        
	    return random() > 0.5    
	}
}  
  
d1.randomBool() // 之前遵循 TextRepresentable 的 Dice 实例就可以调用这个方法了  
```  
  
可以使用协议扩展来给协议的任意方法或者计算属性要求提供默认实现。  
  
```swift  
extension TextRepresentable {  
    var textualDescription: String {        
	    return "hello"    
	}
}  
```  
  
这样后，如果遵循协议的类不需要自己的实现，那就可以不实现用这个默认的，比如  
  
```swift  
let d = Dice()  
  
extension Dice: TextRepresentable {  
    // 虽然自己没实现，但和可选协议不一样，因为有默认实现，所以不需要可选链调用  
}  
  
print(d.textualDescription) // 输出 hello  

extension Dice: TextRepresentable { // 如果自己实现了，那就覆盖掉默认的  
    var textualDescription: String {        
	    return "A dice"    
	}
}  
print(d.textualDescription) // 输出 A dice
```  
  
### 扩展附加条件  
  
定义扩展时，可以明确遵循类型必须在扩展的方法和属性可用之前满足的限制，在扩展协议名字后边使用 where 分句来写这些限制。  

```swift  
extension Collection where Iterator.Element: TextRepresentable {  
    var textualDescription: String {        
	    let itemsAsText = self.map { $0.textualDescription }        
	    return "[" + itemsAsText.joined(separator: ", ") + "]"    
	}
}  
```  
  
扩展 Collection 协议，Collection 自己就是个协议，要给它加个属性 textualDescription，前提是元素本身必须遵循 TextRepresentable。  
  
```swift  
extension Array: TextRepresentable where Element: TextRepresentable {  
    var textualDescription: String {        
	    let itemsAsText = self.map { $0.textualDescription }        
	    return "[" + itemsAsText.joined(separator: ", ") + "]"    
	}
}  
```  
  
扩展 Array，Array 自己是个结构体，让它遵循 TextRepresentable 协议，并且实现 textualDescription，前提条件是元素要遵循 TextRepresentable。

## 泛型  
  
### 泛型函数  
  
函数名字后加个 `<T>`，然后参数里类型就用 `T`。  
  
```swift  
func swapTwoValues<T>(_ a: inout T, _ b: inout T) {  
    let temporaryA = a    
    a = b    
    b = temporaryA
}  
  
var someString = "hello"  
var anotherString = "world"  
swapTwoValues(&someString, &anotherString)  
```  
  
### 泛型类型  
  
类型名字后加 `<T>`  

```swift  
struct Stack<Element> {  
    var items = [Element]()    
    
    mutating func push(_ item: Element) {  
	    items.append(item)    
    }    
    
    mutating func pop() -> Element {        
	    return items.removeLast()    
	}
}  
```  
  
扩展泛型类型时，不需要在扩展的定义中提供类型形式参数列表，就是不需要写 `<Element>`，并且原来的 `Element` 类型可以直接用。  
  
```swift  
extension Stack { // 扩展类型时原来的 Element 类型直接用  
    var topItem: Element? {        
	    return items.isEmpty ? nil : items[items.count - 1]    
	}
}  
```  
  
### 类型约束  
  
```swift  
func someFunction<T: SomeClass, U: SomeProtocol>(someT: T, someU: U) {  
    // function body goes here
}  
```  
  
要求泛型本身遵循的协议，或者是什么类，和 Java 差不多，但是在尖括号里声明这个约束关系。  
  
### 关联类型  
  
定义一个协议时，在协议定义里声明一个或多个关联类型，相当于一个占位符名称，直到采纳协议时，才指定用于该关联类型的实际类型。关联类型通过 associatedtype 关键字指定。  
  
```swift  
protocol Container {  
    associatedtype ItemType // 关联类型  
    mutating func append(_ item: ItemType) // 参数用这个关联类型  
    var count: Int { get }    
    subscript(i: Int) -> ItemType { get } // 返回值用这个关联类型  
}  
```  
  
```swift  
struct IntStack: Container { // 遵循协议  
    typealias ItemType = Int // 声明协议里的 ItemType 用 Int  
    var items = [Int]()  
    // 协议里用 ItemType 的地方都用 Int    
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
  
上面协议里用 ItemType 的地方都是用 Int，其实 ItemType 是 Int 可以推断出来，所以不需要写 `typealias ItemType = Int`。  
  
```swift  
struct Stack<Element>: Container {  
    var items = [Element]()    
    mutating func append(_ item: Element) {
	    self.push(item)    
	}    
	var count: Int {        
		return items.count    
	}    
	subscript(i: Int) -> Element {        
		return items[i]    
	}
}  
```  
  
上面是泛型版本，ItemType 就是 Element，可以推断出来，所以不用写 `typealias ItemType = Element`。  
  
#### 给关联类型添加约束  
  
```swift  
protocol Container {  
    // 要求关联类型 Item 必须遵循 Equatable 协议  
    associatedtype Item: Equatable    
    mutating func append(_ item: Item)    
    var count: Int { get }    
    subscript(i: Int) -> Item { get }
}  
```  
  
#### 在关联类型约束里使用协议  
  
```swift  
protocol SuffixableContainer: Container {  
    associatedtype Suffix: SuffixableContainer where Suffix.Item == Item    
    func suffix(_ size: Int) -> Suffix
}  
```  
  
协议可以作为它自身的要求出现，上面定义了一个协议 SuffixableContainer，内部有个关联类型 `associatedtype Suffix`，并且还有约束 SuffixableContainer，也就是说关联类型就是自己。后面加 where 语句要求自己的 Item 类型必须是 Container 里关联的 Item 类型。  
  
```swift  
extension Stack: SuffixableContainer {  
    func suffix(_ size: Int) -> Stack {        
	    var result = Stack()        
	    for index in (count-size)..<count {   
		    result.append(self[index])        
		}        
		return result    
	}    
	// Inferred that Suffix is Stack.
}  
```  
  
扩展 Stack 让它遵循 SuffixableContainer，它关联的类型 Suffix 要遵循 SuffixableContainer，就是 Stack 自己。  
  
当然要求遵循 SuffixableContainer 就行，不一定要是 Stack 自己，比如下面  
  
```swift  
extension IntStack: SuffixableContainer {  
    func suffix(_ size: Int) -> Stack<Int> {        
	    var result = Stack<Int>()        
	    for index in (count-size)..<count {     
		    result.append(self[index])        
		}        
		return result    
	}    
	// Inferred that Suffix is Stack<Int>.
}  
```  
  
IntStack 是一个类型，遵循 SuffixableContainer，它关联的 Suffix 是什么，是 `Stack<Int>`，不是 IntStack 这个类型，因为 Stack 遵循了 SuffixableContainer，而且里面的 Int 也就是 Container 的 Item。  
  
对于一个继承自其他协议的协议，可以通过在协议的声明中包含泛型 where 分句来给继承的协议中关联类型添加限定。  
  
```swift  
protocol ComparableContainer: Container where Item: Comparable { }  
```  
  
Container 里关联类型约束 Item，子协议 ComparableContainer 要求它的 Item 必须遵循 Comparable。  
  
### 泛型 where分句  
  
泛型 where 分句写在一个类型或函数体的左半个大括号前面。  
  
```swift  
func allItemsMatch<C1: Container, C2: Container> (_ someContainer: C1, _ anotherContainer: C2) -> Bool    
	where C1.ItemType == C2.ItemType, C1.ItemType: Equatable {        
    // ...        
    return true
}  
```  
  
泛型要求 C1 和 C2 都遵循协议 Container，where 分句要求 C1 和 C2 内部的关联类型 ItemType 必须一样，且 C1 的关联类型 ItemType 遵循 Equatable 协议。当两个参数类型符合这个条件时，才能调用这个方法。  
  
### 上下文 where 分句  
  
在协议上下文中，可以在 where 语句中直接使用类型声明。  
  
```swift  
// 扩展协议 Container，给它添加两个方法  
extension Container {  
    // 要求 Container 中声明的关联类型 Item 要是 Int 才能用这个方法
    func average() -> Double where Item == Int {      
		var sum = 0.0        
		for index in 0..<count {            
			sum += Double(self[index])        
		}        
		return sum / Double(count)    
	}    
	
	// 要求 Container 中声明的类型 Item 要遵循 Equatable 才能用这个方法  
    func endsWith(_ item: Item) -> Bool where Item: Equatable {        
	    return count >= 1 && self[count-1] == item    
	}
}  
```  
  
如果不想使用上下文 where 分句，就需要写两个扩展，每一个都使用范型 where 分句，下面的和上面等价。  
  
```swift  
// where 分句既可以卸载方法的大括号前，也可以写在类型的大括号前  
// 一个扩展说明 Item 要是 Int
extension Container where Item == Int {  
    func average() -> Double {        
	    var sum = 0.0        
	    for index in 0..<count {            
		    sum += Double(self[index])        
		}        
		return sum / Double(count)    
	}
}  
  
// 再写一个扩展说明 Item 要遵循 Equatable
extension Container where Item: Equatable {  
    func endsWith(_ item: Item) -> Bool {        
	    return count >= 1 && self[count-1] == item    
	}
}  
```  
  
### 泛型下标  
  
下标可以在 subscript 后用尖括号来写类型占位符，还可以在下标代码块花括号前写泛型 where 分句。  

```swift
extension Container {  
    // 说明参数泛型  
    subscript<Indices: Sequence>(indices: Indices) -> [Item]        
    where Indices.Iterator.Element == Int { // 在大括号前用 where 分局指定条件  
	    var result = [Item]()            
	    for index in indices {    
		    result.append(self[index])            
		}            
		return result    
	}
}  
```

## 不透明类型  
  
和返回协议，父类差不多，区别在于返回协议，只有实现了协议的都可以返回，但不透明类型，虽然不确定具体是什么类型，但是必须是同一种类型。  
  
```swift  
protocol SomePro {  
}  
class A: SomePro {}  
class B: SomePro {}  
func test(i: Int) -> SomePro {  
    if i==1 {        
	    return A()    
	} else {        
		return B()    
	}
}  
  
func test2(i: Int) -> some SomePro {  
    if i==1 {        
	    return A()    
	} else {        
		return B()    
	}
}  
```  
  
上面 test() 函数可以正常编译，无论返回 A 还是 B，它们都实现了 SomePro，但是 test2() 无法编译，因为它的返回类型写作 `some SomePro`，就是不透明类型，虽然要求实现 SomePro，还必须是同一种类型，不能有时返回 A，有时返回 B。  
  
还有假如协议内部有关联类型，就不能用作返回值，因为无法确定内部关联值的类型，但是可以用作不透明类型返回值。  
  
## 包装的协议类型  
  
```swift
func test1(t1: any SomePro, t2: any SomePro){  }  
  
func test2<T: SomePro>(t1: T, t2: T) {  }  
  
  
test1(t1: A(), t2: B()) // 两个参数只有遵循 SomePro 协议即可，不需要是一种类型  
test2(t1: A(), t2: A()) // 两个参数必须是同一种类型  
```

## 自动引用计数  
  
创建一个类的实例，自动引用计数(ARC) 会分配一块内存来存储，并且跟踪和计算当前实例被多少属性，常量和变量所引用，当引用数变成 0 后，会自动释放内存。  
  
```swift  
class Person {  
    init() {        
	    print("initialized")    
	}    
	deinit {        
		print("deinitialized")    
	}
}  
var reference1: Person?  
var reference2: Person?  
var reference3: Person?  
  
reference1 = Person() // 有一个强引用，引用一块内存，存放着一个 Person 对象，执行了 init 初始化方法  
reference2 = reference1  
reference3 = reference1 // 现在这个实例有三个强引用  
reference1 = nil  
reference2 = nil // 断开了两个强引用，还剩一个  
reference3 = nil // 没有引用了，这个对象要被释放了，执行 deinit 方法  
```  
  
### 类实例之间的循环强引用  
  
如果两个类实例彼此持有对方的强引用，就会造成所谓的循环强引用。  
  
```swift  
class Person {  
    var apartment: Apartment?    
    deinit { print("Person deinitialized") }
}  
  
class Apartment {  
    var tenant: Person?    
    deinit { print("Apartment deinitialized") }
}  

var john: Person? = Person()  
var unit4A: Apartment? = Apartment()  
john.apartment = unit4A  
unit4A.tenant = john  
john = nil  
unit4A = nil  
```  
  
Person 实例现在有了一个指向 Apartment 实例的强引用，而 Apartment 实例也有了一个指向 Person 实例的强引用。断开 john 和 unit4A 变量所持有的强引用时，引用计数并不会变成零，实例也不会被 ARC 释放，从而导致内存泄漏。  
  
### 解决实例之间的循环强引用  
  
弱引用和无主引用允许循环引用中的一个实例引用另外一个实例而不保持强引用，从而避免循环强引用。  
  
对于生命周期中会变为 nil 的实例使用弱引用，对于初始化赋值后再也不会被赋值为 nil 的实例，使用无主引用。  
  
#### 弱引用  
  
适用于两个类互相持有，并且都可以为空的情况。  
  
弱引用不会对其引用的实例保持强引用，因而不会阻止 ARC 释放被引用的实例。当实例被释放后，弱引用可能还指着实例，但是 ARC 会自动将弱引用设置为 nil（这个过程不会调用属性观察者），所以弱引用的值一定是可选类型。
  
```swift  
class Person {  
    var apartment: Apartment?    
    deinit { print("Person deinitialized") }
}  
  
class Apartment {  
    weak var tenant: Person? // 弱引用  
    deinit { print("Apartment deinitialized") }
}  

var john: Person? = Person()  
var unit4A: Apartment? = Apartment()  
john.apartment = unit4A // Person 实例保持对 Arartment 的强引用  
unit4A.tenant = john // Apartment 实例对 Person 是弱引用  
john = nil // 要释放 Person 实例，它已经没有强引用了  
unit4A = nil // 此时 Apartment 实例也被释放  
```  
  
#### 无主引用  
  
适用于两个类互相持有，但一个类里可持有空，另一个类里必须持有非空的。  
  
和弱引用类似，不会强制引用实例，区别是无主引用假定是永远有值的，不是可选类型。所以实例释放后，ARC 无法自动将其设置为 nil。如果实例被释放了，还通过无主引用访问实例，就会运行时错误，导致程序崩溃。  
  
```swift  
class Customer {  
    var card: CreditCard?    
    deinit { print("Customer deinitialized") }
}  
  
class CreditCard {  
    // 无主引用  
    unowned let customer: Customer    // 初始化方法传入，之后就一直不为 nil 了  
    init(customer: Customer) {        
	    self.customer = customer    
	}    
	deinit { print("Card deinitialized") }
}  
  
var john: Customer? = Customer()  
john.card = CreditCard(customer: john!)  
john = nil // 没有引用指着 Customer 了，释放内存，CreditCard 实例也没有引用了，也释放  
```  
  
#### 无主可选引用  
  
无主可选引用也不保持它包含的对象的强引用，不会阻止 ARC 释放实例。行为和无主引用在 ARC 下一致，除了无主可选引用能是 nil。  
  
弱引用在实例释放后，ARC 会自动设置成 nil。无主可选引用，ARC 不会做这件事，可以手动指定引用一个对象或者 nil。  
  
```swift  
class Department {  
    var courses: [Course]    
    init() {        
	    self.courses = []    
	}
}  
  
class Course {  
    unowned var department: Department // 无主引用，初始化方法传入进来，就不会再是 nil    
    unowned var nextCourse: Course? // 无主可选引用，手动指定 nil 或对象  
    init(in department: Department) { 
	    self.department = department        
	    self.nextCourse = nil    
	}
}  
  
let department = Department(name: "Horticulture")  
  
// 两个 Course 实例，无主引用 department 都通过初始化参数传入  
let intro = Course(in: department)  
let intermediate = Course(in: department)  
  
// intro 有 nextCourse，intermediate 就没有  
intro.nextCourse = intermediate  
intermediate.nextCourse = advanced  
  
department.courses = [intro, intermediate]  
```  
  
当从 department.courses 删除 intermediate 时，而 intro.nextCourse 是无主可选引用，所以 intermediate 内存就被释放了。但 intro.nextCourse 还引用着，如果是无主引用，再调用它就崩了，但无主可选引用，可以手动设置成 nil，用它时通过可选链调用。  
  
#### 无主引用和隐式展开的可选属性  
  
两个类互相持有，并且初始化完成后都不允许为 nil。需要一个类用无主属性，另外一个类使用隐式展开的可选属性。一旦初始化完成，这两个属性能被直接访问(不需要可选展开)，同时避免了循环引用。

> 都允许空，用强引用和弱引用。一个可以空，一个不能空，用强引用和无主引用。两个都不能为空，用隐式展开的可选属性和无主引用
  
```swift  
class Country {  
    let name: String    
    var capitalCity: City!    
    init(name: String, capitalName: String) {    
        self.name = name        
        self.capitalCity = City(name: capitalName, country: self)    
    }
}  
  
class City {  
    let name: String    
    unowned let country: Country    
    init(name: String, country: Country) {        
	    self.name = name        
	    self.country = country    
	}
}  
```  
  
Country 的初始化器调用了 City 的初始化器，只有 Country 的实例完全初始化完后，Country 的初始化器才能把 self 传给 City 的初始化器。为了满足这种需求，通过在类型结尾处加上感叹号（ City! ）的方式，以声明 Country 的 capitalCity 属性为一个隐式展开的可选属性。  
  
这样 capitalCity 是可选的，它默认值就是 nil，不需要立即赋值。Country 的实例在初始化器中给 name 属性赋值后，Country 初始化过程就完成了，这时 self 就有了，就可以传给 City 的初始化器了。  
  
以上的意义在于通过一条语句同时创建 Country 和 City 的实例，而不产生循环强引用，并且 capitalCity 的属性能被直接访问，而不需要通过感叹号来展开它的可选值。  

```swift
var country = Country(name: "Canada", capitalName: "Ottawa")  
print("\(country.name)'s capital city is called \(country.capitalCity.name)")  
// prints "Canada's capital city is called Ottawa"  
```  
  
### 闭包的循环强引用  
  
```swift  
class HTMLElement {  
    let name: String    
    let text: String?  
    
    lazy var asHTML: () -> String = {        
	    if let text = self.text {            
			return "<\(self.name)>\(text)</\(self.name)>"   
		} else {            
			return "<\(self.name) />"        
		}    
	}  
	
    init(name: String, text: String? = nil) {     
	    self.name = name        
	    self.text = text    
	}  
	
    deinit {        
	    print("\(name) is being deinitialized")    
	}
}  
```  
  
类中 asHTML 是一个延迟属性，值是一个闭包，这样 HTMLElement 就存在一个对于闭包的强引用。闭包中通过 self.name，self.text 用到了 HTMLElement 实例，即闭包又持有了 HTMLElement 的强引用。这就导致了循环强引用。  
  
```swift  
var paragraph: HTMLElement? = HTMLElement(name: "p", text: "hello, world")  
print(paragraph!.asHTML())  
// prints"hello, world"  
paragraph = nil  
```  
  
paragraph 变成 nil 了，但是闭包还在强引用着 self 这个实例，所以无法释放内存。  
  
#### 解决闭包的循环强引用  
  
通过定义捕获列表，在捕获列表中定义闭包捕获引用时用弱引用或者无主引用。  
  
```swift  
lazy var someClosure: (Int, String) -> String = {  
    [unowned self, weak delegate = self.delegate!] (index: Int, stringToProcess: String) -> String in    
    // closure body goes here
}  
```  
  
参数列表前先用中括号写明捕获列表，可以说明 self 和 self 里的属性，比如 self.delegate!，并且声明是弱引用还是无主引用。  
  
如果闭包的参数和返回值类型可以推断被省略了，捕获列表写在 in 前面，反正就是写在前面，只是闭包连 in 都可以省略，但这时因为要写捕获列表，in 得保留着。  
  
```swift
lazy var someClosure: () -> String = {  
    [unowned self, weak delegate = self.delegate!] in    
    // closure body goes here
}  
```  
  
如果在闭包和捕获的实例总是互相引用并且总是同时释放时，将闭包内的捕获定义为无主引用。  
  
相反，在被捕获的引用可能会变为 nil 时，定义一个弱引用的捕获。弱引用总是可选项，当实例的引用释放时会自动变为 nil，所以需要在闭包体内检查它们是否存在。  
  
如果被捕获的引用永远不会变为 nil ，应该用无主引用而不是弱引用。  
  
前面的 HTMLElement 例子中，无主引用是正确的解决循环强引用的方法。这样编写 HTMLElement 类来避免循环强引用：  

```swift
class HTMLElement {  
    // ...  
    
    lazy var asHTML: () -> String = {        
	    [unowned self] in // self 是无主引用  
	        if let text = self.text {            
		        return "<\(self.name)>\(text)</\(self.name)>"        
		    } else {            
			    return "<\(self.name) />"        
			}    
		}  
    // ...
}  
```  
  
当 `paragraph = nil` 时，闭包是个无主引用，实例将被释放。

## 访问控制

- open 仅适用于类和类成员，类可以在任何地方被继承，即便不在同一个模块，类成员可以在任何地方的子类重写，即便不在同一个模块
- public 类只能在相同模块继承，类成员只能在相同模块的子类重写，就是 open 是跨模块使用，public 只在模块内使用
- internal 只能在模块内的使用，和 public 区别不仅仅限于类。
- fileprivate 只在当前文件可用
- private 只在声明的最小范围内，同一个文件，不是同一个类，那也访问不了 private 的

```swift
public class SomePublicClass {}
internal class SomeInternalClass {}
fileprivate class SomeFilePrivateClass {}
private class SomePrivateClass {}

public var somePublicVariable = 0
internal let someInternalConstant = 0
fileprivate func someFilePrivateFunction() {}
private func somePrivateFunction() {}
```

### 默认值

总的来说，就是级别高的不能用级别低点，比如 public 的函数，不能返回 private 的属性，因为能用 public 的地方不一定能用这个 private 的。

- 类型不写默认是 internal，它里面的方法，成员，嵌套类型默认也是 internal。如果想变成 public 的，必须显式指定。
- 类型如果写了 open/public/internal，它里面的方法，成员，嵌套依然默认是 internal。如果想变成 public 的，必须显式指定。
- 类型如果写的 fileprivate/private，它里面的方法，成员，嵌套类型默认也是 fileprivate/private。
- 类型里面的方法，成员，嵌套类型如果访问控制级别和类型不一样，只能更严格，比如 public 的类型可以有 private 的成员，但 private 的类型里不能出现 internal 的方法。

### 元组

对于元组，会根据所有值的访问权限自动推断出元组访问权限，选择最严格的那个，比如一个值是 public，另一个值是 private，那么元组本身是 private。

### 函数

对于函数，访问级别由函数成员类型和返回类型中的最严格访问级别决定。如果函数的计算访问级别与上下文环境默认级别不匹配，必须在函数定义时显式指出。

```swift
private func someFunction() -> (SomeInternalClass, SomePrivateClass) {
}
```

假如返回值的元组推断出来是 private 的，而函数在上下文中是默认的 internal，两者不匹配，必须在函数前面写上 private。

### 枚举

对于枚举，枚举本身什么访问级别，里面的值就是什么级别，也不允许单独设置值的级别。如果有关联值或原始值，那么关联值或原始值的级别不能比枚举本身更严格，就是比如枚举本身是 public 的，关联值不能用一个 private 的变量。

### 子类

对于父子类，子类不能高于父类的访问级别，就是比如父类是 internal，子类不能是 public，子类可以是 private，就是不能更宽松，但是可以更严格。但是内部成员可以更宽松。

```swift
public class A {
    fileprivate func someMethod() {}
}

internal class B: A {
	override internal func someMethod() {}
}
```

子类 B 变得更严格，重写的方法 someMethod 变得更宽松，从只能在一个文件里使用变成在模块里都可以使用。

### getter/setter

常量、变量、属性和下标的 getter 和 setter 自动接收它们所属常量、变量、属性和下标的访问级别。就是 getter/setter 方法默认和属性一样的访问控制级别，但是可以自定义 setter 的访问控制级别更加严格。

```swift
struct TrackedString {
	private(set) var numberOfEdits = 0
}
```

上面 TrackedString 是默认的 internal，里面的属性 numberOfEdits 也是默认的 internal，getter 方法和属性本身一样，所以也是 internal，但 setter 被设置成更严格的 privste，就是说设置值只能在 TrackedString 内部设置。

```swift
public struct TrackedString {
	public private(set) var numberOfEdits = 0
}
```

上面的 TrackedString 是 public，内部的属性 numberOfEdits 本来默认依然是 internal，但是要指定成 public，就是 getter/setter 都是 public 的了，然后后面又加了个 private(set)，这样 setter 又变成了 private。

### 初始化器

初始化器的访问级别可以比类型更严格，但必要初始化器必须和类型一致，不能更严格。

结构体如果有存储属性，会有默认的初始化器，类似默认的访问级别：

- 如果属性是 fileprivate/private 的，这个默认的初始化器也是 fileprivate/private
- 如果属性是 internal/public/open 的，这个默认的初始化器是 internal，如果想更宽松，必须显式指定

### 协议

协议比较特殊，内部的和协议保持一致，就是协议如果是 public 的，内部方法，成员也是 public 的，不想普通类型那样是 internal。并且不可以自己修改成别的，只能是自动的 public。

协议如果继承，只能更严格，这和子类一样，

类型遵循协议时可以遵循访问控制更严格的协议，就是一个 public 的类可以遵循一个 internal 的协议，并且最终遵循了协议后，这个类的访问取两者中更严格的，就是类的访问控制级别变成了 internal。

比如一个类是 public 的，里面写了个方法也显式指定为 public，当扩展类遵循一个 internal 的协议后，类其实也变成了 internal，这时里面的方法却是 public 的，那就不行了。