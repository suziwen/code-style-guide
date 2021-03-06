# 序

> 风格可以用来区分从好到卓越。<br />
> -- Bozhidar Batsov

有一件事情总是困扰着，作为Ruby程序员的我 - python 开发者都有一个很棒的编程风格参考
([PEP-8](http://www.python.org/dev/peps/pep-0008/))，然而我们从没有一个官方的（公认）的guide，Ruby代码风格文档和最佳实践。而且我信赖这些风格（的一些约定）。我也相信下面的一些好东西，像我们这样的Ruby开发者，也应该有能力写出这样的梦寐以求的文档。

这份指南诞生于我们公司内部的Ruby编程准则(由你的真诚的同志们编写)，基于Ruby社区的大多数成员会对我正在做的有兴趣这样的发点，我决定做这样的工作，而且世界上很少需要另一个公司内部的（编程）准则。但是这个世界将会一定有益于社区驱动的以及社区认可的，Ruby习惯和风格实践。

自从这个 guide（发表）以来，我收到了很多来自优秀的Ruby社区的世界范围内的成员的回馈。感谢所有的建议和支持！集我们大家之力，我们可以创作出对每一个Ruby开发人员有益的资源。

补充，如果你正在使用rails你可能会希望查阅[Ruby on Rails 3 Style Guide](https://github.com/bbatsov/rails-style-guide).

# Ruby 风格指南

这个 Ruby 风格指南推荐（一些）最佳实践使得现实世界中的Ruby程序员可以写出能够被其他现实世界的Ruby程序员维护的代码。一个风格指南反映了真实世界的使用习惯，同时一个风格指南伴随着一个观点那就是它被一些人们抵制因为它可能会导致风险的产生甚至毫无用处 —— 无论它有多好。

这个指南被分为几个相关联的 rules 的几节。我会尝试给 rules 添加合理的解释（如果它被省略那么我假设它相当的明显了）。

我并没有列举所有的 rules - 它们大多数基于我作为一个专业的软件工程师的广泛生涯、回馈和来自Ruby社区成员的建议以及各种备受推崇的Ruby编程资源，例如["Programming Ruby 1.9"](http://pragprog.com/book/ruby3/programming-ruby-1-9)
和 ["The Ruby Programming Language"](http://www.amazon.com/Ruby-Programming-Language-David-Flanagan/dp/0596516177).

这个指南仍然在工作进程中 - 一些 rules 缺乏例子，一些 rules 没有合适的例子来使得它们足够明了。

你可以使用[Transmuter](https://github.com/TechnoGate/transmuter).来生成指南的PDF或者HTML的copy。

## 目录表单

* [源代码布局](#源代码布局)
* [语法](#语法)
* [命名](#命名)
* [注释](#注释)
* [注解](#注解)
* [类](#类)
* [异常](#异常)
* [集合](#集合)
* [字符串](#字符串)
* [正则表达式](#正则表达式)
* [百分号和字面值](#百分号和字面值)
* [元编程](#元编程)
* [杂项](#杂项)

## 源代码布局

> 附近的每个人都深信除了他们自己的每一种风格都是
> 丑陋的并且难以阅读的。抛开 "but their own"（其他的人的作为） 那么他们
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

* 在操作符旁使用空格，逗号，冒号和分号后；在 `{`旁和在 `}`之前，大多数空格可能对Ruby解释（代码）无关，但是它的恰当使用是让代码变得易读的关键。

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

* 使用空行在 `def`s 之间并且一个方法根据逻辑段来隔开。

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
* 如果一个方法的调用参数分割为多行将它们与**方法名**对齐。

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

* 从来不要使用 `for`， 除非你知道使用它的准确原因。大多数时候迭代器都可以用来替`for`。`for`是`each`的组实现 (因此你正间接添加了一级)，但是有一个小道道 - `for`并不包含一个新的scope(不像`each`)并且在它的块中定义的变量在外面也是可以访问的。

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

* 喜欢三元操作运算（`?:`）超过`if/then/else/end`结构。
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

* 不要使用`if x: ...` - 它在Ruby 1.9中已经移除。使用三元操作运算代替。

    ```Ruby
    # bad
    result = if some_condition then something else something_else end

    # good
    result = some_condition ? something : something_else
    ```

* 不要使用 `if x; ...`。使用三元操作运算代替。

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

* 忽略传递给方法的参数前后的括号是 `internal DSL` (e.g. Rake, Rails, RSpec)的一部分，Ruby中的 “关键字”方法(e.g. `attr_reader`, `puts`)以及属性访问方法，所带参数忽略括号。使用括号将在其他方法调用的参数括起来。

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

* 在单行代码块的时候宁愿使用`{...}`而不是`do...end`。避免在多行代码块使用`{...}` (多行链式通常变得非常丑陋)。通常使用`do...end`来做`流程控制`和`方法定义`(例如 在Rakefiles和某些DSLs中)。避免在链式调用中使用`do...end`。

    ```Ruby
    names = ["Bozhidar", "Steve", "Sarah"]

    #good
    names.each { |name| puts name }

    #bad
    names.each do |name|
      puts name
    end

    # good
    names.select { |name| name.start_with?("S") }.map { |name| name.upcase }

    # bad
    names.select do |name|
      name.start_with?("S")
    end.map { |name| name.upcase }
    ```

    有人会争论多行链式看起来和使用`{...}`一样工作，但是他们问问自己 - 这样的代码真的有可读性码并且为什么代码块中的内容不能被提取到美丽的方法中。

* 避免在不需要流的控制使用`return`

    ```Ruby
    # bad
    def some_method(some_arr)
      return some_arr.size
    end

    # good
    def some_method(some_arr)
      some_arr.size
    end
    ```


* 避免在不需要的地方使用 `self`(它仅仅在调用一些 `self`做写访问的时候需要)(It is only required when calling a self write accessor.)

    ```Ruby
    # bad
    def ready?
      if self.last_reviewed_at > self.last_updated_at
        self.worker.update(self.content, self.options)
        self.status = :in_progress
      end
      self.status == :verified
    end

    # good
    def ready?
      if last_reviewed_at > last_updated_at
        worker.update(content, options)
        self.status = :in_progress
      end
      status == :verified
    end
    ```
* 作为一个必然的结果，避免将方法（参数）放于局部变量阴影之下除非它们是相等的。

    ```Ruby
    class Foo
      attr_accessor :options

      # ok
      def initialize(options)
        self.options = options
        # both options and self.options are equivalent here
      end

      # bad
      def do_something(options = {})
        unless options[:when] == :later
          output(self.options[:message])
        end
      end

      # good
      def do_something(params = {})
        unless params[:when] == :later
          output(options[:message])
        end
      end
    end

* 当分配默认值给方法参数的时候，在`=`附近使用空格。

    ```Ruby
    # bad
    def some_method(arg1=:default, arg2=nil, arg3=[])
      # do something...
    end

    # good
    def some_method(arg1 = :default, arg2 = nil, arg3 = [])
      # do something...
    end
    ```

    然而一些 Ruby 资料推荐第一种风格，第二种在实践中更突出(可以说更具可读性)。

* 避免在不需要的时候使用行连接符(`\\`)。实际上应该避免行连接符。

    ```Ruby
    # bad
    result = 1 - \
             2

    # good (but still ugly as hell)仍然像地狱一样丑陋
    result = 1 \
             - 2
    ```
* 使用`=`返回一个表达式的值是很好的，但是需要用括号把赋值运算式括起来。

    ```Ruby
    # good - show intented use of assignment
    if (v = array.grep(/foo/)) ...

    # bad
    if v = array.grep(/foo/) ...

    # also good - show intended use of assignment and has correct precedence.
    if (v = self.next_value) == "hello" ...
    ```

* 使用`||=`轻松的初始化变量。

    ```Ruby
    # set name to Vozhidar, only if it's nil or false
    name ||= 'Bozhidar'
    ````

* 不要使用`||=`来初始化布尔变量。（想想如果当前值为`false`的时候会发生什么。）

    ```Ruby
    # bad - would set enabled to true even if it was false
    enable ||= true

    # good
    enabled = true if enabled.nil?
    ```

* 避免使用 Perl 的指定变量风格（比如，`$0-9`，`$`，等等。）。它们相当神秘，不鼓励在单行代码之外使用它们。

* 从来不要在方法名和（参数）开括号之间使用空格。

    ```Ruby
    # bad
    f (3+2) + 1

    # good
    f(3 + 2) +1
    ```

* 如果方法的第一个参数以开括号开始，通常使用括号把它们全部括起来。例如`f((3 + 2) + 1)`。

* 通常使用-w 选项运行Ruby解释器，在你忘记上面所诉规则，ruby将会提示你。

* 当你的hash字典是symbols的时候，使用Ruby 1.9的字面量语法。

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2 }

    #good
    hash = { one: 1, two: 2 }
    ```

* 使用新的 lambda 语法。

    ```Ruby
    # bad
    lambda = lambda { |a, b| a + b }
    lambda.call(1, 2)

    # good
    lambda = ->(a, b) { a + b }
    lambda.(1, 2)
    ```

* 对不使用的块变量使用`_`。

    ```Ruby
    # bad
    result = hash.map { |k, v| v + 1}

    # good
    result = hash.map { |_, v| v + 1 }
    ```

### 命名

> The only real difficulties in programming are cache invalidation and
> naming things. <br/>
> -- Phil Karlton
> 程序（运行）中唯一不一样的是无效的缓存和命名的事物（变量）。<br/>
> -- Phil Karlton

* 使用`snake_case`的形式给变量和方法命名。
* Snake case: punctuation is removed and spaces are replaced by single underscores. Normally the letters share the same case (either UPPER_CASE_EMBEDDED_UNDERSCORE or lower_case_embedded_underscore) but the case can be mixed
* 使用`CamelCase(駝峰式大小寫)`的形式给类和模块命名。(保持使用缩略首字母大写的方式如HTTP,
  RFC, XML)
* 使用`SCREAMING_SNAKE_CASE`给常量命名。
* 在表示断言的方法名（方法返回真或者假）的末尾添加一个问号（如Array#empty?）。
* 可能会造成潜在“危险”的方法名（如修改 `self`或者 参数的方法，`exit!` (不是像 `exit` 执行完成项)等）应该在末尾添加一个感叹号。

    ```Ruby
    # bad - there is not matching 'safe' method
    class Person
      def update!
      end
    end

    # good
    class Person
      def update
      end
    end

    # good
    class Person
      def update!
      end

      def update
      end
    end
    ```

* Define the non-bang (safe) method in terms of the bang (dangerous)
  one if possible.

    ```Ruby
    class Array
      def flatten_once!
        res = []

        each do |e|
          [*e].each { |f| res << f }
        end

        replace(res)
      end

      def flatten_once
        dup.flatten_once!
      end
    end
    ```

* 当在短的块中使用`reduce`时，命名参数`|a, e|` (accumulator, element)。

    ```Ruby
    #Combines all elements of enum枚举 by applying a binary operation, specified by a block or a symbol that names a method or operator.
    # Sum some numbers
    (5..10).reduce(:+)                            #=> 45#reduce
    # Same using a block and inject
    (5..10).inject {|sum, n| sum + n }            #=> 45 #inject注入
    # Multiply some numbers
    (5..10).reduce(1, :*)                         #=> 151200
    # Same using a block
    (5..10).inject(1) {|product, n| product * n } #=> 151200
    ```

* 在定义二元操作符方法时，将其的参数取名为other。

    ```Ruby
    def +(other)
      # body omitted
    end
    ```
* `map`优先于`collect`，`find`优先于`detect`，`select`优先于`find_all`，`reduce`优先于`inject`，`size`优先于`length`。以上的规则并不绝定，如果使用后者能提高代码的可读性，那么尽管使用它们。这些对应的方法名（如 `collect`，`detect`，`inject`）继承于SmallTalk语言，它们在其它语言中并不是很通用。鼓励使用 `select` 而不是 `find_all` 是因为 `select` 与 `reject` 一同使用时很不错，并且它的名字具有很好的自解释性。

## 注释

> Good code is its own best documentation. As you're about to add a
> comment, ask yourself, "How can I improve the code so that this
> comment isn't needed?" Improve the code and then document it to make
> it even clearer. <br/>
> -- Steve McConnell
> 好的代码在于它有好的文档。当你打算添加一个注释，问问自己，“我该做的是怎样提高代码质量，那么这个注释是不是不需要了？”提高代码并且给他们添加文档使得它更加简洁。<br/>
> -- Steve McConnell

* 写出自解释文档代码，然后忽略不工作的这部分。这不是说着玩。
* 注释长于一个单词则以大写开始并使用标点。使用一个空格将注释与符号隔开。Use [one
  space](http://en.wikipedia.org/wiki/Sentence_spacing) after periods.
* 避免多余的注释。

    ```Ruby
    # bad
    counter += 1 # increments counter by one
    ```

* 随时更新注释，没有注释比过期的注释更好。
* 不要为糟糕的代码写注释。重构它们，使它们能够“自解释”。(Do or do not - there is no try.)

## 注解

* 注解应该写在紧接相关代码的上方。
* 注解关键字后跟一个冒号和空格，然后是描述问题的记录。
* 如果需要多行来描述问题，随后的行需要在`#`后面缩进两个空格。

    ```Ruby
    def bar
      # FIXME: This has crashed occasionally since v3.2.1. It may
      #  be related to the BarBazUtil upgrade.
      baz(:quux)
    end

* 如果问题相当明显，那么任何文档就多余了，注解也可以（违规的）在行尾而没有任何备注。这种用法不应当在一般情况下使用，也不应该是一个 rule。

    ```Ruby
    def bar
      sleep 100 # OPTIMIZE
    end
    ```

* 使用`TODO`来备注缺失的特性或者在以后添加的功能。
* 使用`FIXME`来备注有问题需要修复的代码。
* 使用`OPTIMIZE`来备注慢的或者低效的可能引起性能问题的代码。
* 使用`HACK`来备注那些使用问题代码的地方可能需要重构。
* 使用`REVIEW`来备注那些需要反复查看确认工作正常的代码。例如：`REVIEW: 你确定客户端是怎样正确的完成 X 的吗？`
* 使用其他自定义的关键字如果认为它是合适的，但是确保在你的项目的`README`或者类似的地方注明。

## 类

* 在设计类层次的时候确保他们符合[Liskov Substitution Principle](http://en.wikipedia.org/wiki/Liskov_substitution_principle)原则。(译者注: LSP原则大概含义为: 如果一个函数中引用了 `父类的实例`, 则一定可以使用其子类的实例替代, 并且函数的基本功能不变. (虽然功能允许被扩展))
>Liskov替换原则：子类型必须能够替换它们的基类型 <br/>
> 1. 如果每一个类型为T1的对象o1，都有类型为T2的对象o2，使得以T1定义的所有程序P在所有的对象o1都代换为o2时,程序P的行为没有变化，那么类型T2是类型T1的子类型。 <br/>
> 2. 换言之，一个软件实体如果使用的是一个基类的话，那么一定适用于其子类，而且它根本不能察觉出基类对象和子类对象的区别。只有衍生类替换基类的同时软件实体的功能没有发生变化，基类才能真正被复用。 <br/>
> 3. 里氏代换原则由Barbar Liskov(芭芭拉.里氏)提出，是继承复用的基石。 <br/>
> 4. 一个继承是否符合里氏代换原则，可以判断该继承是否合理（是否隐藏有缺陷）。

* 努力使你的类尽可能的健壮[SOLID](http://en.wikipedia.org/wiki/SOLID_(object-oriented_design\))。
* 总是为你自己的类提供to_s方法, 用来表现这个类（实例）对象包含的对象.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def to_s
        "#@first_name #@last_name"
      end
    end
    ```

* 使用`attr`功能功能成员来定义各个实例变量的访问器或者修改器方法。

    ```Ruby
    # bad
    class Person
      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      def first_name
        @first_name
      end

      def last_name
        @last_name
      end
    end

    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end
    ```

* 考虑使用 `Struct.new`, 它可以定义一些琐碎的 `accessors`,
`constructor`（构造函数） 和 `comparison`（比较） 操作。

    ```Ruby
    # good
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end
    end

    # better
    class Person < Struct.new(:first_name, :last_name)
    end
    ````

* 考虑添加工厂方法来提供灵活的方法来创建实际类实例。

    ```Ruby
    class Person
      def self.create(potions_hash)
        # body omitted
      end
    end
    ```

* 鸭子类型（[duck-typing](http://en.wikipedia.org/wiki/Duck_typing)）优于继承。

    ```Ruby
    # bad
    class Animal
      # abstract method
      def speak
      end
    end

    # extend superclass
    class Duck < Animal
      def speak
        puts 'Quack! Quack'
      end
    end

    # extend superclass
    class Dog < Animal
      def speak
        puts 'Bau! Bau!'
      end
    end

    # good
    class Duck
      def speak
        puts 'Quack! Quack'
      end
    end

    class Dog
      def speak
        puts 'Bau! Bau!'
      end
    end
    ```

* 避免使用类变量（`@@`）因为他们讨厌的继承习惯（在子类中也可以修改父类的类变量）。

    ```Ruby
    class Parent
      @@class_var = 'parent'

      def self.print_class_var
        puts @@class_var
      end
    end

    class Child < Parent
      @@class_var = 'child'
    end

    Parent.print_class_var # => will print "child"
    ```

    正如上例看到的, 所有的子类共享类变量, 并且可以直接修改类变量,此时使用类实例变量是更好的主意.

* 根据方法的用途为他们分配合适的可见度( `private`, `protected` )，不要让所有的方法都是 `public` (这是默认设定)。这是 *Ruby* 不是 *Python*。
* `public`, `protected`, 和 `private` 等可见性关键字应该和其（指定）的方法具有相同的缩进。并且不同的可见性关键字之间留一个空格。

    ```Ruby
    class SomeClass
      def public_method
        # ...
      end

      private
      def private_method
        # ...
      end
    end
    ```

* 使用 `def self.method` 来定义单例方法. 当代码重构时, 这将使得代码更加容易因为类名是不重复的.

    ```Ruby
    class TestClass
      # bad
      def TestClass.some_method
        # body omitted
      end

      # good
      def self.some_other_method
        # body ommited
      end

      # 也可以这样方便的定义多个单例方法。
      class << self
        def first_method
          # body omitted
        end

        def second_method_etc
          # body omitted
        end
      end
    end
    ```

    ```Ruby
    class SingletonTest
      def size
        25
      end
    end

    test1 = SingletonTest.new
    test2 = SingletonTest.new
    def test2.size
      10
    end
    test1.size # => 25
    test2.size # => 10
    ```
    本例中，test1 與 test2 屬於同一類別，但 test2 具有重新定義的 size 方法，因此兩者的行為會不一樣。只給予單一物件的方法稱為单例方法 (singleton method)。

## 异常

* 单个异常使用 `fail` 关键字仅仅当捕获一个异常并且反复抛出这个异常(因为这里你不是失败，而是准确的并且故意抛出一个异常)。

    ```Ruby
    begin
      fail 'Oops';
    rescue => error
      raise if error.message != 'Oops'
    end
    ```
* Never return from an `ensure` block. If you explicitly return from a
  method inside an `ensure` block, the return will take precedence over
  any exception being raised, and the method will return as if no
  exception had been raised at all. In effect, the exception will be
  silently thrown away.
  不要在 `ensure` 块中返回。如果你明确的从 `ensure` 块中的某个方法中返回，返回将会优于任何抛出的异常，并且尽管没有异常抛出也会返回。实际上异常将会静静的溜走。

    ```Ruby
    def foo
      begin
        fail
      ensure
        return 'very bad idea'
      end
    end
    ```

* Use *implicit begin blocks* when possible.使用**隐式 `begin` 代码块**如果可能。

    ```Ruby
    # bad
    def foo
      begin
        # main logic goes here
      rescue
        # failure handling goes here
      end
    end

    # good
    def foo
      # main logic goes here
    rescue
      # failure handling goes here
    end

* Mitigate the proliferation of `begin` blocks by using
  *contingency methods* (a term coined by Avdi Grimm).
  减少 `begin` 代码块的扩增通过一些 *contingency methods* 偶然性方法。

    ```Ruby
    # bad
    begin
      something_that_might_fail
    rescue IOError
      # handle IOError
    end

    begin
      something_else_that_might_fail
    rescue IOError
      # handle IOError
    end

    # good
    def with_io_error_handling
       yield
    rescue IOError
      # handle IOError
    end

    with_io_error_handling { something_that_might_fail }

    with_io_error_handling { something_else_that_might_fail }
    ```

* 不要抑制异常输出。

    ```Ruby
    # bad
    begin
      # an exception occurs here
    rescue SomeError
      # the rescue clause does absolutely nothing还没有补救代码
    end

    # bad
    do_something rescue nil
    ```


* 不要用异常来控制流。

    ```Ruby
    # bad
    begin
      n / d
    rescue ZeroDivisionError
      puts "Cannot divide by 0!"
    end

    # good
    if d.zero?
      puts "Cannot divide by 0!"
    else
      n / d
    end
    ```

* 应该总是避免拦截(最顶级的)Exception异常类。这里(ruby自身)将会捕获信号并且调用 `exit`，需要你使用 `kill -9` 杀掉进程。

    ```Ruby
    # bad
    begin
      # calls to exit and kill signals will be caught (except kill -9)
      exit
    rescue Exception
      puts "you didn't really want to exit, right?"
      # exception handling
    end

    # good
    begin
      # a blind rescue rescues from StandardError, not Exception as many
      # programmers assume.
    rescue => e
      # exception handling
    end

    # also good
    begin
      # an exception occurs here

    rescue StandardError => e
      # exception handling
    end

    ```

* 将更具体的异常放在拦截链的上方，否则他们将不会被捕获。

    ```Ruby
    # bad
    begin
      # some code
    rescue Exception => e
      # some handling
    rescue StandardError => e
      # some handling
    end

    # good
    begin
      # some code
    rescue StandardError => e
      # some handling
    rescue Exception => e
      # some handling
    end
    ```

* 使用 `ensure` 语句, 来确保总是执行一些特地的操作.

    ```Ruby
    f = File.open("testfile")
    begin
      # .. process
    rescue
      # .. handle error
    ensure
      f.close unless f.nil?
    end
    ```

* 除非必要, 尽可能使用Ruby标准库中异常类，而不是引入一个新的异常类。(而不是派生自己的异常类)

## 集合

* Prefer literal array and hash creation notation (unless you need to
pass parameters to their constructors, that is).不要想着使用文字而是使用创建符号来定义数组和哈希字典(除非你需要传递参数到它们的构造函数中)。

    ```Ruby
    # bad
    arr = Array.new
    hash = Hash.new

    # good
    arr = []
    hash = {}
    ```

* 总是使用 `%w` 的方式来定义字符串数组.(译者注: w表示英文单词word, 而且定义之间千万不能有逗号)

    ```Ruby
    # bad
    STATES = ['draft', 'open', 'closed']

    # good
    STATES = %w(draft open closed)
    ```

* 避免直接引用靠后的数组元素, 这样隐式的之前的元素都被赋值为nil.

    ```Ruby
    arr = []
    arr[100] = 1 # now you have an array with lots of nils
    ```

* 如果要确保元素唯一, 则使用 `Set` 代替 `Array` .`Set` 更适合于无顺序的, 并且元素唯一的集合, 集合具有类似于数组一致性操作以及哈希的快速查找.

* 尽可能使用符号代替字符串作为哈希键.

    ```Ruby
    # bad
    hash = { 'one' => 1, 'two' => 2, 'three' => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* 避免使用易变对象作为哈希键。
* 优先使用1.9的新哈希语法当你的哈希键是符号。

    ```Ruby
    # bad
    hash = { :one => 1, :two => 2, :three => 3 }

    # good
    hash = { one: 1, two: 2, three: 3 }
    ```

* 记住, 在Ruby1.9中, 哈希的表现不再是无序的. (译者注: Ruby1.9将会记住元素插入的序列)
* 当遍历一个集合的同时, 不要修改这个集合。

## 字符串

* 优先使用 `字符串插值` 来代替 `字符串串联`。

    ```Ruby
    # bad
    email_with_name = user.name + ' <' + user.email + '>'

    # good
    email_with_name = "#{user.name} <#{user.email}>"
    ```

* Consider padding string interpolation code with space. It more clearly sets the
  code apart from the string.考虑使用空格填充字符串插值。它更明确了除字符串的插值来源。

    ```Ruby
    "#{ user.last_name }, #{ user.first_name }"
    ```

* 当不需要使用 `字符串插值` 或某些特殊字符如 `\t`、 `\n`、`'`、等, 应该优先使用单引号.

    ```Ruby
    # bad
    name = "Bozhidar"

    # good
    name = 'Bozhidar'
    ```

* 当使用字符串插值替换 `实例变量` 时, 应该省略{}.

    ```Ruby
    class Person
      attr_reader :first_name, :last_name

      def initialize(first_name, last_name)
        @first_name = first_name
        @last_name = last_name
      end

      # bad
      def to_s
        "#{@first_name} #{@last_name}"
      end

      # good
      def to_s
        "#@first_name #@last_name"
      end
    end
    ```

* 操作较大的字符串时, 避免使用 `String#+` , 如果需要修改被操作字符串, 应该总是使用 `String#<<` 作为代替。就地并列字符串实例变体比 `String#+` 更快，它创建了多个字符串对象。

    ```Ruby
    # good and also fast
    html = ''
    html << '<h1>Page title</h1>'

    paragraphs.each do |paragraph|
      html << "<p>#{paragraph}</p>"
    end
    ```

## 正则表达式

* 如果只是需要中查找字符串的 `text`, 不要使用正则表达式：`string['text']`

* 针对简单的结构, 你可以直接使用string[/RE/]的方式来查询.

    ```Ruby
    match = string[/regexp/]             # get content of matched regexp
    match = "regexp"[/regexp/]           #=> "regexp"
    first_group = string[/text(grp)/, 1] # get content of captured group
    first_group = 'textgrp'[/text(grp)/, 1] #=> 'grp'
    first_group = 'text grp'[/text(grp)/, 1] #=> nil
    string[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    "text grp"[/text (grp)/, 1] = 'replace'  # string => 'text replace'
    "text 1grp"[/text (grp)/, 1] = ' replace' if  "text 1grp"[/text (grp)/, 1].present?  #=> nil 

    ```

* 当无需引用分组内容时, 应该使用(?:RE)代替(RE).

    ```Ruby
    /(first|second)/   # bad
    /(?:first|second)/ # good
    ```

* 避免使用 `$1-$9` 风格的分组引用, 而应该使用1.9新增的命名分组来代替.

    ```Ruby
    # bad
    /(regexp)/ =~ string
    ...
    process $1

    # good
    /(?<meaningful_var>regexp)/ =~ string
    ...
    process meaningful_var
    ```

* 字符类有以下几个特殊关键字值得注意: `^`, `-`, `\`, `]`, 所以, 不要转义 `.` 和 `[]` 中的括号, 他们是正常字符.

* 注意, `^` 和 `$` , 他们匹配行首和行尾, 而不是一个字符串的结尾, 如果你想匹配整个字符串, 用 `\A` 和 `\Z`。

    ```Ruby
    string = "some injection\nusername"
    string[/^username$/]   # matches
    string[/\Ausername\Z/] # don't match
    ```

* 使用 `x` 修饰符来匹配复杂的表达式, 这将使得RE更具可读性, 你可以添加一些有用的注释.
注意, 所有空格将被忽略.

    ```Ruby
    regexp = %r{
      start         # some text
      \s            # white space char
      (group)       # first group
      (?:alt1|alt2) # some alternation
      end
    }x
    ```

*  `sub`/`gsub`也支持哈希以及代码块形式语法, 可用于复杂情形下的替换操作.

## 百分号和字面值

* 多用 `%w`

    ```Ruby
    STATES = %w(draft open closed)
    ```

* 定义需要插值和嵌套双引号符号的单行字符串，使用 `%()` 的方式.而多行字符串, 尽量使用 heredocs 格式.

    ```Ruby
    # bad (不需要插值)
    %(<div class="text">Some text</div>)
    # should be '<div class="text">Some text</div>' # 应该这样写

    # bad (没有双引号)
    %(This is #{quality} style)
    # should be "This is #{quality} style" # 应该这样写

    # bad (multiple lines)
    %(<div>\n<span class="big">#{exclamation}</span>\n</div>)
    # should be a heredoc.

    # good (插值, 引号, 单行)
    %(<tr><td class="name">#{name}</td>)
    ```

    Heredoc is a robust way to create string in PHP with more lines but without using quotations.
    Heredoc 是 php 中不使用引号就可以创建多行字符串的一种强大的方式。

    line-oriented string literals (Here document)
There's a line-oriente form of the string literals that is usually called as `here document`. Following a `<<` you can specify a string or an identifier to terminate the string literal, and all lines following the current line up to the terminator are the value of the string. If the terminator is quoted, the type of quotes determines the type of the line-oriented string literal. Notice there must be **no space between `<<` and the terminator** .

    If the - placed before the delimiter, then all leading whitespcae characters (tabs or spaces) are stripped from input lines and the line containing delimiter. This allows here-documents within scripts to be indented in a natural fashion.

    ```Ruby
      print <<EOF
        The price is #{$Price}.
        EOF

      print <<"EOF";			# same as above
    The price is #{$Price}.
    EOF

      print <<`EOC`			# execute commands
    echo hi there
    echo lo there
    EOC

      print <<"foo", <<"bar"	# you can stack them
    I said foo.
    foo
    I said bar.
    bar

      myfunc(<<"THIS", 23, <<'THAT')
    Here's a line
    or two.
    THIS
    and here's another.
    THAT

      if need_define_foo
        eval <<-EOS			# delimiters can be indented
          def foo
            print "foo\n"
          end
        EOS
      end
    ```

* `%r` 的方式只适合于定义包含多个 `/` 符号的正则表达式。

    ```Ruby
    # bad
    %r(\s+)

    # still bad
    %r(^/(.*)$)
    # should be /^\/(.*)$/

    # good
    %r(^/blog/2011/(.*)$)
    ```

    ```Ruby
    irb(main):001:0> string="asdfas.64"
    => "asdfas.64"
    irb(main):002:0> string[/^\/(.*)$/]
    => nil
    irb(main):003:0> string="/asdfas.64"
    => "/asdfas.64"
    irb(main):004:0> string[/^\/(.*)$/]
    => "/asdfas.64"
    irb(main):007:0> string="/blog/2011/asdfas.64"
    => "/blog/2011/tmp/asdfas.64"
    irb(main):008:0> string[%r(^/blog/2011/(.*)$)]
    => "/blog/2011/tmp/asdfas.64"
    ```

* 避免使用`%q`，`%Q`， `%x`， `%s`,和 `%W`

* 优先使用()作为%类语法格式的分隔符.(译者注, 本人很喜欢 `%(...)`, 不过Programming Ruby中, 显然更喜欢使用%{}的方式)

## 元编程

* Avoid needless metaprogramming.避免无限循环的元编程。

* 在编写库时，不要乱动核心库。（不要画蛇添足）

* The block form of `class_eval` is preferable to the string-interpolated form. `class_eval` 代码块形式最好用于字符串插值形式。
  - when you use the string-interpolated form, always supply `__FILE__` and `__LINE__`, so that your backtraces make sense:当你使用字符串插值形式，总是提供 `__FILE__` 和 `__LINE__`，使得你的回溯有意义。

    ```ruby
    class_eval 'def use_relative_model_naming?; true; end', __FILE__, __LINE__
    ```

  - `define_method` 最好用 `class_eval{ def ... }`

* When using `class_eval` (or other `eval`) with string interpolation, add a comment block showing its appearance if interpolated (a practice I learned from the rails code):当使用 `class_eval` (或者其他的 `eval`)以及字符串插值，添加一个注释块使之在插入的时候现实(这是我从 rails 代码学来的实践)：

    ```ruby
    # from activesupport/lib/active_support/core_ext/string/output_safety.rb
    UNSAFE_STRING_METHODS.each do |unsafe_method|
      if 'String'.respond_to?(unsafe_method)
        class_eval <<-EOT, __FILE__, __LINE__ + 1
          def #{unsafe_method}(*args, &block)       # def capitalize(*args, &block)
            to_str.#{unsafe_method}(*args, &block)  #   to_str.capitalize(*args, &block)
          end                                       # end

          def #{unsafe_method}!(*args)              # def capitalize!(*args)
            @dirty = true                           #   @dirty = true
            super                                   #   super
          end                                       # end
        EOT
      end
    end
    ```

* avoid using `method_missing` for metaprogramming. Backtraces become messy; the behavior is not listed in `#methods`; misspelled method calls might silently work (`nukes.launch_state = false`). Consider using delegation, proxy, or `define_method` instead.  If you must, use `method_missing`,
* 避免在元编程中使用 `method_missing`，它使得回溯变得很麻烦，这个习惯不被列在 `#methods`，拼写错误的方法可能也在默默的工作 (`nukes.launch_state = false`)。考虑使用委托，代理或者是 `define_method` ，如果必须这样，使用 `method_missing` ，
  - be sure to [also define `respond_to_missing?`](http://blog.marc-andre.ca/2010/11/methodmissing-politely.html)
  - only catch methods with a well-defined prefix, such as `find_by_*` -- make your code as assertive as possible.
  - call `super` at the end of your statement
  - delegate to assertive, non-magical methods:

    ```ruby
    # bad
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        # ... lots of code to do a find_by
      else
        super
      end
    end

    # good
    def method_missing?(meth, *args, &block)
      if /^find_by_(?<prop>.*)/ =~ meth
        find_by(prop, *args, &block)
      else
        super
      end
    end

    # best of all, though, would to define_method as each findable attribute is declared
    ```

## 杂项

* 总是打开Ruby -w开关。
* 通常情况下, 尽量避免使用哈希作为方法的 `optional` 参数. (此时应该考虑这个方法是不是功能太多?)
* 避免一个方法内容超过10行代码, 理想情况下, 大多数方法内容应该少于5行.(不算空行)
* 尽量避免方法的参数超过三或四个.
* 有时候, 必须用到全局方法, 应该增加这些方法到 Kernel 模块，并设置他们可见性关键字为 `private`。
* 尽可能使用类实例变量代替全局变量. (译者注:是类实例变量, 而不是类的实例变量. 汗~~)

    ```Ruby
    #bad
    $foo_bar = 1

    #good
    class Foo
      class << self
        attr_accessor :bar
      end
    end

    Foo.bar = 1
    ```

* 能够用 `alias_method` 就不要用 `alias`。
* 使用 `OptionParser` 来解析复杂的命令行选项， 较简单的命令行， `-s` 参数即可。
* 按照功能来编写方法, 当方法名有意义时, 应该避免方法功能被随意的改变。
* 避免不需要的元编程。
* 除非必要, 避免更改已经定义的方法的参数。
* 避免超过三级的代码块嵌套。
* 应该持续性的遵守以上指导方针。
* 多使用（生活）常识。

# Contributing

Nothing written in this guide is set in stone. It's my desire to work
together with everyone interested in Ruby coding style, so that we could
ultimately create a resource that will be beneficial to the entire Ruby
community.

Feel free to open tickets or send pull requests with improvements. Thanks in
advance for your help!

# Spread the Word

A community-driven style guide is of little use to a community that
doesn't know about its existence. Tweet about the guide, share it with
your friends and colleagues. Every comment, suggestion or opinion we
get makes the guide just a little bit better. And we want to have the
best possible guide, don't we?
