# 序

> 风格可以用来区分从好到卓越。<br />
> -- Bozhidar Batsov

有一件事情总是困扰着，作为Ruby程序员的我 - python 开发者都有一个很棒的编程风格参考
([PEP-8](http://www.python.org/dev/peps/pep-0008/))，然而我们从没有一个官方的（公认）的guide，Ruby代码风格文档和最佳实践。而且我信赖这些风格（的一些约定）。我也相信下面的一些好东西，像我们这样的Ruby开发者，也应该有能力写出这样的梦寐以求的文档。

这份指南诞生于我们公司内部的Ruby编程准则，基于Ruby社区的大多数成员会对我正在做的有兴趣这样的发点，我决定做这样的工作，而且世界上很少需要另一个公司内部的（编程）准则。但是这个世界将会一定有益于社区驱动的以及社区认可的，Ruby习惯和风格实践。

自从这个guide（发表）以来，我收到了很多来自优秀的Ruby社区的世界范围内的成员的回馈。感谢所有的建议和支持！集我们大家之力，我们可以创作出对每一个Ruby开发人员有益的资源。

补充，如果你正在使用rails你可能会希望查阅[Ruby on Rails 3 Style Guide](https://github.com/bbatsov/rails-style-guide).

# Ruby 风格指南

这个Ruby风格指南推荐（一些）最佳实践使得现实世界中的Ruby程序员可以写出能够被其他真是世界的Ruby程序员维护的代码。一个风格指南反映了真实世界的使用习惯，同时一个风格指南紧紧把握一个观点那就是人们拒绝接受任何有可能无法使用指南的风险，无论它多好。

这个指南被分为几个具有相关的rules的几节。我尝试给rules添加合理的解释（如果它被省略我假设它相当的明显了）。

我并没有列举所有的rules - 它们大多数基于我作为一个专业的软件工程师的广泛生涯，回馈和来自Ruby社区成员的建议以及各种备受推崇的Ruby编程资源，例如["Programming Ruby 1.9"](http://pragprog.com/book/ruby3/programming-ruby-1-9)
和 ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177).

这个指南仍然在工作进程中 - 一些rules缺乏例子，一些rules没有合适的例子来使得它们足够明了。

你可以使用[Transmuter](https://github.com/TechnoGate/transmuter).来生成指南的PDF或者HTML的copy。

## 源代码布局

> 附近的每个人都深信每一个风格除了他们自己的都是
> 丑陋的并且难以阅读的。脱离"but their own"那么他们
> 完全正确... <br />
> -- Jerry Coffin (on indentation缩进)

* 使用 `UTF-8` 作为源文件编码。
* 每个缩进级别使用两个 **spaces** 

    ```Ruby
    # good
    def some_method
      do_something
    end

    # bad - four spaces
    def some_method
        do_something
    end
    ```

* 使用Unix-风格行结束。(*BSD/Solaris/Linux/OSX 用户被涵盖为默认，Windows 用户必须特别小心.)
> \n是换行，英文是LineFeed，ASCII码是0xA。
> \r是回车，英文是Carriage Return ,ASCII码是0xD。
> windows下enter是 \n\r,unix下是\n,mac下是\r
    * 如果你正在使用Git你可能会想要添加下面的配置设置来保护你的项目（避免）Windows蔓延过来的行结束符:
        ```$ git config --global core.autecrlf true```

* 使用空格：在操作符旁；逗号，冒号和分号后；在 `{`旁和在 `}`之前，大多数空格可能对Ruby解释（代码）无关，但是它的恰当使用是让代码变得易读的关键。

    ```Ruby
    sum = 1 + 2
    a, b = 1, 2
    1 > 2 ? true : false; puts 'Hi'
    [1, 2, 3].each { |e| puts e }
    ```

    唯一的例外是当使用指数操作时：

    ```Ruby
    # bad
    e = M * c ** 2

    # good
    e = M * c**2
    ```

* 没有空格 `(`, `[`之后或者 `]`, `)`之前。

    ```Ruby
    some(arg).other
    [1, 2, 3].length
    ```

* `when`和`case` 缩进深度一致。我知道很多人会不同意这点，但是它是"The Ruby Programming Language" 和 "Programming Ruby"中公认的风格。

    ```Ruby
    case
    when song.name == 'Misty'
      puts 'Not again!'
    when song.duraton > 120
      puts 'Too long!'
    when Time.now > 21
      puts "It's too late"
    else
      song.play
    end

    kind = case year
           when 1850..1889 then 'Blues'
           when 1890..1909 then 'Ragtime'
           when 1910..1929 then 'New Orleans Jazz'
           when 1930..1939 then 'Swing'
           when 1940..1950 then 'Bebop'
           else 'Jazz'
           end
    ```

* 使用空行在 `def`s 并且一个方法根据逻辑段来隔开。

    ```Ruby
    def some_method
      data = initialize(options)

      data.manipulate!

      data.result
    end

    def some_methods
      result
    end
    ```
* 如果一个方法的调用参数分割为多行将它们于**方法名**对齐。

    ```Ruby
    # starting point (line is too long)
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com', from: 'us@example.com', subject: 'Important message', body: source.text)
    end
    # bad (normal indent)
    def send_mail(source)
      Mailer.deliver(
        to: 'bob@example.com',
        from: 'us@example.com',
        subject: 'Important message',
        body: source.text)
    end

    # bad (double indent)
    def send_mail(source)
      Mailer.deliver(
          to: 'bob@example.com',
          from: 'us@example.com',
          subject: 'Important message',
          body: source.text)
    end
    # good
    def send_mail(source)
      Mailer.deliver(to: 'bob@example.com',
                     from: 'us@example.com',
                     subject: 'Important message',
                     body: source.text)
    end
    ```

* 在 API 文档中使用 RDoc和它的公约。不要在注释代码块和`def`之间加入空行。
* 保持每一行少于80字符。
* 避免尾随空格。

## 语法

* 使用括号将`def`的参数括起来。当方法不接收任何参数的时候忽略括号。

    ```Ruby
    def some_method
      # body omitted
    end

    def some_method_with_arguments(arg1, arg2)
      # body omitted
    end
    ```

* 从来不要使用 `for`， 除非你知道使用它的准确原因。大多数时候迭代器都可以用来替代for。`for` is implemented in terms of `each`#`for`是`each`的组实现 (因此你正间接添加了一级)，但是有一个小道道 - `for`并不包含一个新的scope(不像`each`)并且在它的块中定义的变量在外面也是可以访问的。

    ```Ruby
    arr = [1, 2, 3]

    # bad
    for elem in arr do 
      puts elem
    end

    puts elem # => 3

    # good
    arr.each { |elem| puts elem }
    ```

* 在多行的`if/unless`中坚决不要使用`then`.

    ```Ruby
    # bad
    if some_condition then
      # body omitten
    end

    # good
    if some_condition
      # body omitted
    end
    ```

* 喜欢三元操作运算（`?:`）超过`if/then/else/end`结果。
  它更加普遍而且明显的更加简洁。

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* 使用一个表达式在三元操作运算的每一个分支下面只使用一个表达式。也就是说三元操作符不要被嵌套。在这样的情形中宁可使用`if/else`。

    ```Ruby
    # bad
    some_condition ? (nested_condition ? nested_something : nested_something_else) : something_else

    # good
    if some_condition
      nested_condition ? nested_something : nested_something_else
    else
      something_else
    end
    ```

* 使用三元操作运算代替`if x: ...`。

* 在 one-line cases 的时候使用`when x then ...`。替代的语法`when x: xxx`已经在Ruby 1.9中移除。

* 不要使用`when x; ...`。查看上面的规则。

* 布尔表达式使用`&&/||`, `and/of`用于控制流程。（经验Rule:如果你必须使用额外的括号（表达逻辑），那么你正在使用错误的的操作符。）

    ```Ruby
    # boolean expression
    if some_condition && some_other_condition
      do_something
    end

    # control flow
    document.save? or document.save!
    ```

* 避免多行`?:`(三元操作运算)，使用`if/unless`替代。

* 在单行语句的时候喜爱使用`if/unless`修饰符。另一个好的选择就是使`and/of`来做流程控制。

    ```Ruby
    # bad
    if some_condition
      do_something
    end

    # good
    do_something if some_condition

    # another good option
    some_condition and do_something
    ```

* 在否定条件下喜欢`unless`超过`if`(或者控制流程 `or`)。

    ```Ruby
    # bad
    do_something if !some_condition

    # good
    do_something unless some_condition

    # another good option
    some_condition or do_something
    ```

* 不要使用`else`搭配`unless`。将其的语义重写为肯定形式。

    ```Ruby
    # bad
    unless sucess?
      puts 'failure'
    else
      puts 'sucess'
    end

    # good
    if sucess?
      puts 'sucess'
    else
      puts 'failure'
    end
    ```

* 不要在`if/unless/while`将条件旁括起来，除非这个条件包含一个参数(参见下面 "使用`=`返回值")。

    ```Ruby
    # bad
    if (x>10)
      # body omitted
    end

    # good
    if x > 10
      # body omitted
    end

    # ok
    if (x = self.next_value)
      # body omitted
    end
    ```

* DSL(e.g. Rake, Rails, RSpec)里的方法，Ruby“关键字”方法(e.g. `attr_reader`, `puts`)以及属性访问方法，所带参数忽略括号。使用括号将在其他方法调用的参数括起来。

    ```Ruby
    class Person
      attr_reader :name, :age

      # omitted
    end

    temperance = Person.new('Temperance', 30)
    temperance.name

    puts temperance.age

    x = Math.sin(y)
    array.delete(e)
    ```

* 在单行代码块的时候宁愿使用`{...}`超过`do...end`。避免在多行代码块使用`{...}`多行