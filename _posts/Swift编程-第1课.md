---
layout:     post
title:      Swift编程：第一课 概述
subtitle:   
date:       2024-03-19
author:     daiwei
header-img: img/post-bg-ios9-web.jpg
catalog: true
tags:
    - swift
    - iOS
---
# 前言

>最近工作都在围绕 VisionOS 展开，准备系统性的学习一下 Swift 及 SwiftUI，记录自己的学习过程，并分享给各位亦菲和彦祖。后续还会出 Swift 源码分析文章，尽请期待

# 基本类型
基本类型这部分太简单了，浅浅浏览一遍
```swift
//Int
let i: Int = 1

//Double
let d: Double = 2

//String
let label = "Hello Swift!"

//String 拼接
let widthLabel = label + String(d)
//console output
print(label)

//string format
let format = "i: \(i)"
let multiLineString = """
这是一个多行字符串
,行数为\(d)"""
print(multiLineString)

//array 值类型！
var persons = ["daiwei", "lifeifei", "paul"]
persons[0] = "David"

//dictionary 值类型！
var dic = ["name": "David", "age":13]
dic = [:]
persons = []
```
# 控制流
for in
```swift
let individualScores = [75, 43, 103, 87, 12]
var teamScore = 0
//只读遍历 score is a 'let' constant
for score in individualScores {
    if score > 50 {
        teamScore += 3
    } else {
        teamScore += 1
    }
}
print(teamScore)
```
let 和 if 的混合使用
```swift
let i = if a > 0{
    3
}else{
    4
}
let j = a > 0 ? 3 : 4
```
Switch支持所有类型,默认break
```swift
let vegetable = "red pepper"
switch vegetable {
case "celery":
    print("Add some raisins and make ants on a log.")
case "cucumber", "watercress":
    print("That would make a good tea sandwich.")
case let x where x.hasSuffix("pepper"):
    print("Is it a spicy \(x)?")
default:
    print("Everything tastes good in soup.")
}
// Prints "Is it a spicy red pepper?"
```
while 和 repeat
```swift
var n = 2
while n < 100 {
    n *= 2
}
print(n)
// Prints "128"


var m = 2
repeat {
    m *= 2
} while m < 100
print(m)
// Prints "128"
```
创建数组: 0..<5; 0...5; 1...5
```swift
var total = 0
for i in 0..<4 {
    total += i
}
print(total)
// Prints "6"
```
# 函数和闭包（引用类型）
函数能够被包含在代码块中
```swift
func makeIncrementer() -> ((Int) -> Int) {
    func increte(_ number: Int) -> Int{
        return 1 + number
    }
    return increte
}

var inc = makeIncrementer()
inc(2)
```
函数作为参数
```swift
func anyMatchs(list: [Int], condition: (Int) -> bool){
    for item in list {
        if condition(item) {
            return true
        }
    }
    return false
}
func lessThanTen(number: Int) -> Bool {
    return number < 10
}
var numbers = [20, 19, 7, 12]
hasAnyMatches(list: numbers, condition: lessThanTen)
```
闭包的简化:
1. 当参数类型可以推断时,可以将参数直接写在in前面
2. 当闭包只有一行时可以不用return
3. 1情况下参数也可以用$0, $1替换
```swift
numbers.map({ (number: Int) -> Int in
    let result = 3 * number
    return result
})
let mappedNumbers = numbers.map({ number in 3 * number })
print(mappedNumbers)
// Prints "[60, 57, 21, 36]"
let sortedNumbers = numbers.sorted { $0 > $1 }
print(sortedNumbers)
// Prints "[20, 19, 12, 7]"
```
# 类
继承、重写、计算成员变量、init、deinit
```swift
class EquilateralTriangle: NamedShape {
    var sideLength: Double = 0.0


    init(sideLength: Double, name: String) {
        self.sideLength = sideLength
        super.init(name: name)
        numberOfSides = 3
    }


    var perimeter: Double {
        get {
             return 3.0 * sideLength
        }
        set {
            sideLength = newValue / 3.0
        }
        willSet{
            //newValue
            //some code
        }
        //didSet
    }


    override func simpleDescription() -> String {
        return "An equilateral triangle with sides of length \(sideLength)."
    }
}
var triangle = EquilateralTriangle(sideLength: 3.1, name: "a triangle")
print(triangle.perimeter)
// Prints "9.3"
triangle.perimeter = 9.9
print(triangle.sideLength)
// Prints "3.3000000000000003"
```
# 枚举和结构体
swift的枚举功能异常强大(目前理解是结构体和enum的结合体,非引用)
```swift
enum ServerResponse {
    case result(String, String)
    case failure(String)

    func sayHello() -> void{
        
    }
}


let success = ServerResponse.result("6:00 am", "8:09 pm")
let failure = ServerResponse.failure("Out of cheese.")


switch success {
case let .result(sunrise, sunset):
    print("Sunrise is at \(sunrise) and sunset is at \(sunset).")
case let .failure(message):
    print("Failure...  \(message)")
}
// Prints "Sunrise is at 6:00 am and sunset is at 8:09 pm."
```
swift中结构体与class的一个重要因素是引用类型与值类型的区别
# Dont care （泛型）
可作用在方法、类、枚举、结构体上
```swift
func makeArray<Item>(repeating item: Item, numberOfTimes: Int) -> [Item] {
    var result: [Item] = []
    for _ in 0..<numberOfTimes {
         result.append(item)
    }
    return result
}
//不需要makeArray<string>() 因为可以自动推断
makeArray(repeating: "knock", numberOfTimes: 4)
``````
swift中的optional本质是一种泛型枚举
```swift
enum OptionalValue<Wrapped>{
    case none
    case some(Wrapped)
}
var possibleInteger: OptionalValue<Int> = .none
possibleInteger = .some(100)
``````
Care a little：泛型约束
```swift
func anyCommonElements<T: Sequence, U: Sequence>(_ lhs: T, _ rhs: U) -> [T.Element]
    where T.Element: Equatable, T.Element == U.Element
{
    var elements = [T.Element]()
    for lhsItem in lhs {
        for rhsItem in rhs {
            if lhsItem == rhsItem {
                elements.append(lhsItem)
            }
        }
    }
   return elements
}
let res = anyCommonElements([1, 2, 3, 4], [3, 4])
print(res)
//[3, 4]
``````


