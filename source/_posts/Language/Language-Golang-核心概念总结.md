---
title: '[Language] Golang 核心概念总结'
date: 2025-07-28 03:43:34
tags:
categories: 
- Language
---

## 基础

 1. **标识符可见性**
    在Go语言中，标识符（变量、函数、类型等）的首字母大写表示它可以被其他包访问和使用（已导出），而首字母小写则表示它只能在当前包内部使用（未导出）。(private/public)

 2. **命名返回值**
    Go语言的命名返回值允许在函数声明中指定返回变量名，这些变量在函数体内可直接赋值，并在使用裸返回（`return`）时自动返回其当前值，从而简化代码，但需注意避免降低可读性。

    ```golang
    package main
    
    import "fmt"
    
    func addAndSubtract(a, b int) (sum, diff int) {
        sum = a + b
        diff = a - b
        return // 裸返回:自动返回当前 sum 和 diff 的值
    }
    
    func main() {
        total, difference := addAndSubtract(10, 5)
        fmt.Printf("和是:%d,差是:%d\n", total, difference)
        // 输出: 和是:15,差是:5
    }
    ```

 3. **声明的理解方式**

    * **函数声明**

      ```golang
      func add(x, y int) int {
          return x + y
      }
      ```

      一个函数的函数名叫做`add`，传入了`x`和`y`，数据类型是`int`，返回 `int` 类型数据。

    * **变量声明**

      ```golang
      var age int = 30
      ```

      一个变量名为 `age`，数据类型是`int`，赋值为`30`。

 4. **`fmt.Printf` 格式化动词**

    * `%v`：打印值的默认表示。

    * `%T`：打印值的类型。

    * `%#v`：打印值的Go语言语法表示。

    ```golang
    // 假设你有一个 Product 结构体实例p:
    // type Product struct { ID int; Name string; Price float64 }
    // p := Product{ID:101, Name: "Go T-shirt", Price:25.99}
    
    // fmt.Printf("使用%%v打印:%v\n",p)
    // 输出:使用%v打印:(101 Go T-shirt 25.99)
    
    // fmt.Printf("使用%%#v打印:%#v\n",p)
    // 输出: 使用%#v打印:main.Product{ID:101, Name: "Go T-shirt", Price:25.99}
    ```

 5. **变量零值**
    没有明确初始化的变量声明会被赋予对应类型的零值。

 6. **类型转换**
    表达式 `T(v)` 将值 `v` 转换为类型 `T`。Go在不同类型的项之间赋值时需要显式转换。

    ```golang
    var f float64 = math.Sqrt(float64(x*x + y*y))
    var z uint = uint(f)
    z = f // 错误!!!
    ```

 7. **常量声明**
    常量的声明与变量类似，只不过使用 `const` 关键字。常量不能用 `:=` 语法声明。

    ```golang
    const World = "世界"
    ```

 8. **数值常量**
    数值常量是高精度的值。一个未指定类型的常量由上下文来决定其类型。

    ```golang
    const Pi = 3.14159265358979323846 // 这是一个高精度的无类型浮点常量
    
    var f float32 = Pi // Pi 会被转换为 float32,精度会损失
    var d float64 = Pi // Pi 会被转换为 float64,精度保留更多
    fmt.Println(f)     // 3.1415927
    fmt.Println(d)     // 3.141592653589793
    
    const BigInt = 1000000000000000000 // 这是一个无类型整数常量,非常大
    
    // var i int = BigInt // 错误! BigInt太大,int无法容纳,编译报错
    var i64 int64 = BigInt // 正确! BigInt 被赋值给 int64,它能容纳这么大的值
    fmt.Println(i64)
    
    // 在表达式中
    fmt.Println(BigInt + 1)   // BigInt 仍然是无类型常量,计算结果仍然是高精度的无类型常量
    fmt.Println(BigInt / 2.0) // BigInt 会被视为浮点数,因为2.0 是浮点数
    ```

 9. **`for` 循环**
    C语言的 `while` 在Go中叫做 `for`。

    ```golang
    for sum < 1000 {
        sum += sum
    }
    ```

10. **无限循环**

    ```golang
    for {
        // ...
    }
    ```

11. **`if` 语句中的简短语句**
    和 `for` 一样，`if` 语句可以在条件表达式前执行一个简短语句。该语句声明的变量作用域仅在 `if` 之内。在 `if` 的简短语句中声明的变量同样可以在对应的任何 `else` 块中使用。

    ```golang
    func pow(x, n, lim float64) float64 {
        if v := math.Pow(x, n); v < lim {
            return v
        }
        return lim
    }
    ```

12. **`switch` 语句**
    Go的做法相当于这些语言中为每个 `case` 后面自动添加了所需的 `break` 语句。在Go中，除非以 `fallthrough` 语句结束，否则分支会自动终止。Go的另一点重要的不同在于 `switch` 的 `case` 无需为常量，且取值不限于整数。

    ```golang
    switch os := runtime.GOOS; os {
    case "darwin":
        fmt.Println("macOS.")
    case "linux":
        fmt.Println("Linux.")
    default:
        // freebsd, openbsd,
        // plang, windows...
        fmt.Printf("%s.\n", os)
    }
    ```

13. **`time.Now()`**
    Go练习场中，`time.Now()`是Go的生日: `2009-11-10 23:00:00 UTC`。

14. **无条件 `switch`**
    `switch` 语句可以不带条件表达式，等价于 `switch true`。

    ```golang
    func main() {
        t := time.Now()
        switch {
        case t.Hour() < 12:
            fmt.Println("早上好!")
        case t.Hour() < 17:
            fmt.Println("下午好!")
        default:
            fmt.Println("晚上好!")
        }
    }
    ```

15. **`defer` 语句**
    `defer` 语句会将函数推迟到外层函数返回之后执行。推迟调用的函数其参数会立即求值，但直到外层函数返回前该函数都不会被调用。推迟调用的函数调用会被压入一个栈中。当外层函数返回时，被推迟的调用会按照 FILO（后进先出）的顺序调用。

    ```golang
    package main
    
    import "fmt"
    
    func main() {
        fmt.Println("counting")
    
        for i := 0; i < 10; i++ {
            defer fmt.Println(i)
        }
    
        fmt.Println("done")
    }
    ```

    输出结果：

    ```bash
    counting
    done
    9
    8
    7
    6
    5
    4
    3
    2
    1
    0
    ```

16. **指针**
    类型 `*T` 是指向 `T` 类型值的指针，其零值为 `nil`。

17. **无指针运算**
    与C不同，Go没有指针运算。

18. **结构体 (struct)**
    一个结构体（`struct`）就是一组字段（`field`）。

    ```golang
    type Vertex struct {
        X int
        Y int
    }
    
    func main() {
        fmt.Println(Vertex{1, 2})
    }
    ```

19. **结构体指针访问字段**
    如果我们有一个指向结构体的指针 `p` 那么可以通过 `(*p).X` 来访问其字段 `X`。不过这么写太啰嗦了，所以语言也允许我们使用隐式解引用，直接写 `p.X` 就可以。

20. **结构体字面量**
    使用 `Name:` 语法可以仅列出部分字段（字段名的顺序无关）。

    ```golang
    v2 := Vertex{X: 1} // Y: 被隐式地赋予零值
    ```

21. **数组**
    类型 `[n]T` 表示一个数组，它拥有 `n` 个类型为 `T` 的值。

    ```golang
    var a [10]int
    primes := [6]int{2, 3, 5, 7, 11, 13} // 声明一个变量 primes 为6个 int
    ```

22. **切片 (Slice)**
    切片是引用类型，它只是描述了底层数组中的一段。

    ```golang
    a[low:high] // 左闭右开
    ```

23. **切片与底层数组**
    切片就像数组的引用。切片并不存储任何数据，它只是描述了底层数组中的一段。更改切片的元素会修改其底层数组中对应的元素。和它共享底层数组的切片都会观测到这些修改。

24. **切片字面量**
    切片字面量 `[]Type{...}` 会隐式创建一个底层数组来存储数据，并返回一个引用该底层数组的切片，这与直接创建固定长度数组的数组字面量 `[N]Type{...}` 不同。

25. **切片默认行为**
    在进行切片时，你可以利用它的默认行为来忽略上下界。切片下界的默认值为0，上界则是该切片的长度。

    ```golang
    var a [10]int
    // 以下切片表达式等价:
    // a[0:10]
    // a[:10]
    // a[0:]
    // a[:]
    ```

26. **切片的长度与容量**
    切片的长度是其当前元素数量，而容量是从切片起点到其底层数组末尾的元素数量。切片 `s` 的长度和容量可通过表达式 `len(s)` 和 `cap(s)` 来获取。

    ```golang
    package main
    
    import "fmt"
    
    func main() {
        s := []int{2, 3, 5, 7, 11, 13}
        printSlice(s)
    
        // 截取切片使其长度为0
        s = s[:0]
        printSlice(s)
    
        // 扩展其长度
        s = s[:4]
        printSlice(s)
    
        // 舍弃前两个值
        s = s[2:]
        printSlice(s)
    }
    
    func printSlice(s []int) {
        fmt.Printf("len=%d cap=%d %v\n", len(s), cap(s), s)
    }
    ```

    输出结果：

    ```bash
    len=6 cap=6 [2 3 5 7 11 13]
    len=0 cap=6 []
    len=4 cap=6 [2 3 5 7]
    len=2 cap=4 [5 7]
    ```

27. **`nil` 切片**
    `nil` 切片的长度和容量为0且没有底层数组。

28. **`make` 函数创建切片**
    `make` 函数会分配一个元素为零值的数组并返回一个引用了它的切片:

    ```golang
    a := make([]int, 5) // len(a)=5
    ```

    要指定它的容量，需向 `make` 传入第三个参数:

    ```golang
    b := make([]int, 0, 5) // len(b)=0, cap(b)=5
    
    b = b[:cap(b)] // len(b)=5, cap(b)=5
    b = b[1:]      // len(b)=4, cap(b)=4
    ```

29. **切片的切片**

    ```golang
    board := [][]string{
        []string{"_", "_", "_"},
        []string{"_", "_", "_"},
        []string{"_", "_", "_"},
    }
    ```

30. **追加元素**

    ```golang
    func append(s []T, vs ...T) []T
    ```

    当 `s` 的底层数组太小，不足以容纳所有给定的值时，它就会分配一个更大的数组。返回的切片会指向这个新分配的数组。

31. **`for...range` 遍历切片**
    当使用 `for` 循环遍历切片时，每次迭代都会返回两个值。第一个值为当前元素的下标，第二个值为该下标所对应元素的一份副本。

    ```golang
    var pow = []int{1, 2, 4, 8, 16, 32, 64, 128}
    
    func main() {
        for i, v := range pow {
            fmt.Printf("2**%d = %d\n", i, v)
        }
    }
    ```

    可以将下标或值赋予 `_` 来忽略它。

    ```golang
    for i, _ := range pow
    for _, value := range pow
    ```

    若你只需要索引，忽略第二个变量即可。

    ```golang
    for i := range pow
    ```

32. **映射 (map)**
    `map` 的零值是 `nil`，不能直接使用，必须通过 `make` 函数初始化后才能添加键值对。映射的字面量和结构体类似，只不过必须有键名。若顶层类型只是一个类型名，那么你可以在字面量的元素中省略它。

    ```golang
    type Vertex struct {
        Lat, Long float64
    }
    
    var m = map[string]Vertex{
        "Bell Labs": Vertex{
            40.68433, -74.39967,
        },
        "Google": Vertex{
            37.42202, -122.08408,
        },
    }
    
    // 顶层类型省略的写法
    var m2 = map[string]Vertex{
        "Bell Labs": {40.68433, -74.39967},
        "Google":    {37.42202, -122.08408},
    }
    ```

33. **删除映射元素**

    ```golang
    delete(m, key)
    ```

34. **检测映射键是否存在**
    通过双赋值检测某个键是否存在:

    ```golang
    elem, ok := m[key]
    ```

    若 `key` 在 `m` 中，`ok` 为 `true`；否则，`ok` 为 `false`。若 `key` 不在映射中，则 `elem` 是该映射元素类型的零值。

35. **函数值**
    函数可以作为值传递。

    ```golang
    func compute(fn func(float64, float64) float64) float64 {
        return fn(3, 4)
    }
    
    func main() {
        hypot := func(x, y float64) float64 {
            return math.Sqrt(x*x + y*y)
        }
        fmt.Println(hypot(5, 12))
    
        fmt.Println(compute(hypot))
        fmt.Println(compute(math.Pow))
    }
    ```

36. **函数闭包**
    Go函数可以是一个闭包。闭包是一个函数值，它引用了其函数体之外的变量。该函数可以访问并赋予其引用的变量的值，因此在函数被调用时，这些变量是绑定在其上的。

## 方法和接口

1. **方法**
    Go没有类。不过你可以为类型定义方法。方法就是一类带特殊的接收者参数的函数。方法接收者在它自己的参数列表内，位于 `func` 关键字和方法名之间。

    ```golang
    type Vertex struct {
        X, Y float64
    }
    
    func (v Vertex) Abs() float64 {
        return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }
    ```

    `(v Vertex)` 就是这个方法的接收者。通俗来说，这个方法必须是一个实例才能使用。

2. **方法即函数**
    方法只是个带接收者参数的函数。

3. **接收者类型定义与方法声明**
    接收者的类型定义和方法声明必须在同一包内。

4. **指针接收者**
    指针接收者的主要用途是允许方法修改其接收者所指向的底层数据，或者为了性能考虑避免大型结构体的复制。

    ```golang
    type Vertex struct {
        X, Y float64
    }
    
    func (v Vertex) Abs() float64 {
        return math.Sqrt(v.X*v.X + v.Y*v.Y)
    }
    
    func (v *Vertex) Scale(f float64) { // 指针接收者
        v.X = v.X * f
        v.Y = v.Y * f
    }
    
    func main() {
        v := Vertex{3, 4}
        v.Scale(10) // Go会将语句 v.Scale(10) 解释为 (&v).Scale(10)。
        fmt.Println(v.Abs())
    }
    ```

    试着移除第16行 `Scale` 函数声明中的 `*`，发现函数的行为就发生了改变（变成值传递）。

5. **接收者为指针的方法调用**
    接收者为指针的方法被调用时，接收者既能是值又能是指针: Go会将语句 `v.Scale(5)` 解释为 `(&v).Scale(5)`。

    ```golang
    var v Vertex
    v.Scale(5)  // OK
    p := &v
    p.Scale(10) // OK
    ```

6. **以值为接收者的方法调用**
    以值为接收者的方法被调用时，接收者既能为值又能为指针。

    ```golang
    var v Vertex
    fmt.Println(v.Abs()) // OK
    p := &v
    fmt.Println(p.Abs()) // OK
    ```

    这种情况下，方法调用 `p.Abs()` 会被解释为 `(*p).Abs()`。

7. **接口**
    接口类型的定义为一组方法签名。接口类型的变量可以持有任何实现了这些方法的值。

    ```golang
    // 定义一个通用接口
    type Abser interface {
        Abs() float64
    }
    
    // 假设有一个 Abser 接口的切片
    type MyFloat float64
    func (f MyFloat) Abs() float64 { return float64(math.Abs(float64(f))) }
    
    type Vertex struct { X, Y float64 }
    func (v *Vertex) Abs() float64 { return math.Sqrt(v.X*v.X + v.Y*v.Y) }
    
    things := []Abser{
        MyFloat(-1.0),
        &Vertex{3, 4}, // 注意这里是取地址,因为*Vertex 实现了 Abser
        MyFloat(2.0),
        &Vertex{-1, 0},
    }
    
    // 现在你可以用一个循环统一处理所有实现了 Abser 的类型
    for _, item := range things {
        fmt.Println(item.Abs()) // Go 会根据 item 的实际类型,调用对应的Abs()实现
    }
    ```

8. **接口实现与接收者类型**

    * 如果方法的接收者是值类型（例如 `func (f MyFloat) Abs()`），那么值类型和对应的指针类型都实现了接口。

    * 如果方法的接收者是指针类型（例如 `func (v *Vertex) Abs()`），那么只有指针类型实现了接口。
      这是因为通过指针可以修改原始值，而通过值传递则不行，Go语言为了避免混淆和潜在的副作用，作出了这样的区分。

9. **隐式接口实现**
    类型通过实现一个接口的所有方法来实现该接口。既然无需专门显式声明，也就没有 `implements` 关键字。

    ```golang
    type I interface {
        M()
    }
    
    type T struct {
        S string
    }
    
    // 此方法表示类型 T 实现了接口I,不过我们并不需要显式声明这一点。
    func (t T) M() {
        fmt.Println(t.S)
    }
    
    func main() {
        var i I = T{"hello"}
        i.M()
    }
    ```

10. **接口值的内部表示**
    `%T` 会显示接口变量内部“包裹”的具体类型，而不是接口类型本身。因为在内部，接口值可以看做包含值和具体类型的元组: `(value, type)`。

    ```golang
    type F float64
    
    func (f F) M() {
        fmt.Println(f)
    }
    
    type I interface {
        M()
    }
    
    type T struct {
        S string
    }
    
    func (t *T) M() {
        fmt.Println(t.S)
    }
    
    func describe(i I) {
        fmt.Printf("(%v, %T)\n", i, i)
    }
    
    func main() {
        var i I
    
        i = &T{"Hello"}
        describe(i)
        i.M()
    
        i = F(math.Pi)
        describe(i)
        i.M()
    }
    ```

    输出结果：

    ```bash
    (&{Hello}, *main.T)
    Hello
    (3.141592653589793, main.F)
    3.141592653589793
    ```

11. **`nil` 具体值的接口**
    即便接口内的具体值为 `nil`，方法仍然会被 `nil` 接收者调用。保存了 `nil` 具体值的接口其自身并不为 `nil`。

    ```golang
    type I interface {
        M()
    }
    
    type T struct {
        S string
    }
    
    func (t *T) M() {
        if t == nil {
            fmt.Println("<nil>")
            return
        }
        fmt.Println(t.S)
    }
    
    func describe(i I) {
        fmt.Printf("(%v, %T)\n", i, i)
    }
    
    func main() {
        var i I
    
        var t *T
        i = t
        describe(i)
        i.M()
    
        i = &T{"hello"}
        describe(i)
        i.M()
    }
    ```

    输出结果：

    ```bash
    (<nil>, *main.T)
    <nil>
    (&{hello}, *main.T)
    hello
    ```

    `i` 一开始的具体值是 `nil`，但是动态类型是 `*T`。

12. **`nil` 接口值**
    `nil` 接口值既不保存值也不保存具体类型。为 `nil` 接口调用方法会产生运行时错误，因为接口的元组内并未包含能够指明该调用哪个具体方法的类型。

    ```golang
    type I interface {
        M()
    }
    
    func describe(i I) {
        fmt.Printf("(%v, %T)\n", i, i)
    }
    
    func main() {
        var i I
        describe(i)
        i.M() // 会产生运行时错误: panic: interface conversion: interface {} is nil, not main.I
    }
    ```

13. **空接口**
    指定了零个方法的接口值被称为空接口:

    ```golang
    var i interface{}
    ```

    空接口可保存任何类型的值。(因为每个类型都至少实现了零个方法。)

14. **空接口的应用**
    空接口被用来处理未知类型的值。例如，`fmt.Print` 可接受类型为 `interface{}` 的任意数量的参数。这个函数实际上传入的是空接口，正是空接口的特性，使其可以传入任何参数。

15. **类型断言**
    类型断言提供了访问接口值底层具体值的方式。

    ```golang
    t := i.(T)
    ```

    若并未保存类型 `T` 的值，该语句就会触发一个 `panic`。

16. **带 `ok` 的类型断言**
    为了判断一个接口值是否保存了一个特定的类型，类型断言可返回两个值: 其底层值以及一个报告断言是否成功的布尔值。

    ```golang
    t, ok := i.(T)
    ```

    ```golang
    func main() {
        var i interface{} = "hello"
    
        s := i.(string)
        fmt.Println(s)
    
        s, ok := i.(string)
        fmt.Println(s, ok)
    
        f, ok := i.(float64)
        fmt.Println(f, ok)
    
        f = i.(float64) // panic: interface conversion: interface {} is string, not float64
        fmt.Println(f)
    }
    ```

    输出结果：

    ```bash
    hello
    hello true
    0 false
    ```

17. **读取映射的规则**
    读取一个映射也有同样的规则（指双赋值检测键是否存在）。

18. **类型选择**
    类型选择是一种按顺序从几个类型断言中选择分支的结构。

    ```golang
    switch v := i.(type) {
    case T:
        // v 的类型为 T
    case S:
        // v 的类型为 S
    default:
        // 没有匹配,v与i的类型相同
    }
    ```

    此选择语句判断接口值 `i` 保存的值类型是 `T` 还是 `S`。在 `T` 或 `S` 的情况下，变量 `v` 会分别拥有 `T` 或 `S` 类型的值。在默认（即没有匹配）的情况下，变量 `v` 与 `i` 的接口类型和值相同。

19. **`Stringer` 接口**
    `fmt` 包中定义的 `Stringer` 是最普遍的接口之一。

    ```golang
    type Stringer interface {
        String() string
    }
    func main() {
        type Person struct {
            Name string
            Age  int
        }
    
        func (p Person) String() string {
            return fmt.Sprintf("%v (%v years)", p.Name, p.Age)
        }
    
        a := Person{"Arthur Dent", 42}
        z := Person{"Zaphod Beeblebrox", 9001}
        fmt.Println(a, z)
    }
    ```

    输出结果：

    ```bash
    Arthur Dent (42 years) Zaphod Beeblebrox (9001 years)
    ```

    简而言之就是可以自己实现 `Println` 出来是长什么样子了，只要给这个数据类型定义一个 `String()` 方法。

20. **错误 (error)**
    Go 程序使用 `error` 值来表示错误状态。

    ```golang
    type error interface {
        Error() string
    }
    func main() {
        type MyError struct {
            When time.Time
            What string
        }
    
        func (e MyError) Error() string {
            return fmt.Sprintf("at %v, %s", e.When, e.What)
        }
    
        func run() error {
            return &MyError{
                time.Now(),
                "it didn't work",
            }
        }
    
        if err := run(); err != nil {
            fmt.Println(err)
        }
    }
    ```

    ```golang
    type ErrNegativeSqrt float64
    
    func (e ErrNegativeSqrt) Error() string {
        return fmt.Sprintf("cannot Sqrt negative number: %v", float64(e))
    }
    
    func Sqrt(x float64) (float64, error) {
        if x < 0 {
            return 0.0, ErrNegativeSqrt(x)
        }
        return math.Sqrt(x), nil
    }
    
    func main() {
        fmt.Println(Sqrt(2))
        fmt.Println(Sqrt(-2))
    }
    ```

    为了在函数执行失败时优雅地通知调用者并提供错误详情，我们设计一个能携带上下文信息的自定义类型，通过实现其 `Error() string` 方法使其成为一个 `error` 对象，然后在函数失败时返回该对象的实例。

21. **`io.Reader` 接口**
    `io.Reader` 接口有一个 `Read` 方法:

    ```golang
    func (T) Read(b []byte) (n int, err error)
    ```

    `Read` 用数据填充给定的字节切片并返回填充的字节数和错误值。在遇到数据流的结尾时，它会返回一个 `io.EOF` 错误。

## 泛型

1. **类型参数**
    可以使用类型参数编写 Go 函数来处理多种类型。函数的类型参数出现在函数参数之前的方括号之间。

    ```golang
    func Index[T comparable](s []T, x T) int
    ```

    此声明意味着 `s` 是满足内置约束 `comparable` 的任何类型的切片。`x` 也是相同类型的值。

2. **泛型单链表实现**
    一个普通的单链表实现 (707.设计链表-力扣 (LeetCode))[https://leetcode.cn/problems/design-linked-list/] ：

    ```golang
    type MyLinkedList struct {
    size int
    dummyHead *Node
    }

    type Node struct {
        val int
        next *Node
    }

    func Constructor() MyLinkedList {
        return MyLinkedList{
            size: 0,
            dummyHead: &Node{},
        }
    }

    func (this *MyLinkedList) Get(index int) int {
        if index >= this.size {
            return -1
        } 

        p := this.dummyHead
        for index >= 0 {
            p = p.next
            index--
        }

        return p.val
    }

    func (this *MyLinkedList) AddAtHead(val int)  {
        this.dummyHead.next = &Node{val, this.dummyHead.next}
        this.size++
    }

    func (this *MyLinkedList) AddAtTail(val int)  {
        p := this.dummyHead
        for p.next != nil {
            p = p.next
        }
        p.next = &Node{val, nil}
        this.size++
    }

    func (this *MyLinkedList) AddAtIndex(index int, val int)  {
        if index > this.size {
            return
        }

        if index == this.size {
            this.AddAtTail(val)
            return
        }

        if index == 0 {
            this.AddAtHead(val)
            return
        }

        p := this.dummyHead
        for index > 0 {
            p = p.next
            index--
        }

        p.next = &Node{val, p.next}

        this.size++
    }

    func (this *MyLinkedList) DeleteAtIndex(index int)  {
        if index >= this.size {
            return
        }

        p := this.dummyHead
        for index > 0 {
            p = p.next
            index--
        }

        p.next = p.next.next

        this.size--
    }

    /**
    * Your MyLinkedList object will be instantiated and called as such:
    * obj := Constructor();
    * param_1 := obj.Get(index);
    * obj.AddAtHead(val);
    * obj.AddAtTail(val);
    * obj.AddAtIndex(index,val);
    * obj.DeleteAtIndex(index);
    */
    ```

    运用了泛型类型:

    ```golang
    type MyLinkedList[T any] struct {
        size      int
        dummyHead *Node[T]
    }
    
    type Node[T any] struct {
        val  T
        next *Node[T]
    }
    
    func Constructor[T any]() *MyLinkedList[T] {
        return &MyLinkedList[T]{
            size:      0,
            dummyHead: &Node[T]{},
        }
    }
    
    func (this *MyLinkedList[T]) Get(index int) (T, bool) {
        var zeroval T
        if index >= this.size {
            return zeroval, false
        }
    
        p := this.dummyHead
        for index >= 0 {
            p = p.next
            index--
        }
    
        return p.val, true
    }
    
    func (this *MyLinkedList[T]) AddAtHead(val T) {
        this.dummyHead.next = &Node[T]{val, this.dummyHead.next}
        this.size++
    }
    
    func (this *MyLinkedList[T]) AddAtTail(val T) {
        p := this.dummyHead
        for p.next != nil {
            p = p.next
        }
        p.next = &Node[T]{val, nil}
        this.size++
    }
    
    func (this *MyLinkedList[T]) AddAtIndex(index int, val T) {
        if index > this.size {
            return
        }
    
        if index == this.size {
            this.AddAtTail(val)
            return
        }
    
        if index == 0 {
            this.AddAtHead(val)
            return
        }
    
        p := this.dummyHead
        for index > 0 {
            p = p.next
            index--
        }
    
        p.next = &Node[T]{val, p.next}
        this.size++
    }
    
    func (this *MyLinkedList[T]) DeleteAtIndex(index int) {
        if index >= this.size {
            return
        }
    
        p := this.dummyHead
        for index > 0 {
            p = p.next
            index--
        }
    
        p.next = p.next.next
        this.size--
    }
    
    /**
     * Your MyLinkedList object will be instantiated and called as such:
     * obj := Constructor();
     * param_1 := obj.Get(index);
     * obj.AddAtHead(val);
     * obj.AddAtTail(val);
     * obj.AddAtIndex(index, val);
     * obj.DeleteAtIndex(index);
     */
    ```

## 并发

1. **Go程 (goroutine)**
    Go程（`goroutine`）是由Go运行时管理的轻量级线程。

    ```golang
    go f(x, y, z)
    ```

    会启动一个新的Go协程并执行 `f(x, y, z)`。`f, x, y` 和 `z` 的求值发生在当前的Go协程中，而 `f` 的执行发生在新的Go协程中。

    ```golang
    func say(s string) {
        for i := 0; i < 5; i++ {
            time.Sleep(100 * time.Millisecond)
            fmt.Println(s)
        }
    }
    
    func main() {
        go say("world")
        say("hello")
    }
    ```

    结果 **unpredictable**
