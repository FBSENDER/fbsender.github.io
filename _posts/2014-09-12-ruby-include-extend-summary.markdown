---
layout: post
title:  "ruby include extend pretend 使用方法"
date:   2019-09-12
categories: ruby
---

## include 与 extend
先说include 和 extend，看下面的一段代码：     

```ruby
module A
  def my_method
    puts "method in A"
  end
end

class B
  include A
end

class C
  extend A
end
```
这样写之后，B将新增一个实例方法my_method，C将新增一个类方法my_method，同时，B的祖先链（ancestors）中将增加A，而C的祖先链中没有A。      


再谈一下当方法调用时，ruby是如何在祖先链中找到方法定义的：      
1.实例方法，首先在实例中找，然后在实例对象的单件类中找，之后沿祖先链依次向上找    
2.类方法，首先在类的单件类中找，然后沿着类的单件类的祖先链依次向上找（类的单件类的祖先链 = 类的祖先链的各个节点的单件类组成的链）    

## ruby prepend
ruby prepend 与 include 类似，首先都是添加实例方法的，不同的是扩展module在祖先链上的放置位置不同，看下面的代码：   

```ruby
module A
  def my_method
    puts "in A"
  end
end

class B
  include A
  def my_method
    puts "in B"
  end
end

class C
  prepend A
  def my_method
    puts "in C"
  end
end

B.new.my_method   #输出 ‘in B’
C.new.my_method   #输出 ‘in A’

puts B.ancestors
#输出
#B
#A
#Object
#Kernel
#BasicObject

puts C.ancestors
#输出
#A
#C
#Object
#Kernel
#BasicObject
```

## ActiveSupport::Concern 代码解析
ActiveSupport::Concern 这个module代码不多，看到相关使用的时候引出了上面的一系列问题，4.1.5版本的代码贴在下面，来逐行解析：    

```ruby
module ActiveSupport
  module Concern
    #定义了一种错误类型，在一个extend Concern的类中如果多次使用included方法，会报这个异常
    class MultipleIncludedBlocks < StandardError #:nodoc:
      def initialize
        super "Cannot define multiple 'included' blocks for a Concern"
      end
    end
    #扩展这个module的时候注意使用extend，而不是include
    def self.extended(base) #:nodoc:
      base.instance_variable_set(:@_dependencies, [])
    end
    #include一个module的时候，会先调用append_features，在调用included，base为外层发起调用的module或class
    def append_features(base)
      #第一眼真心看不出下面的逻辑是做什么的，后面会写代码说明逻辑
      if base.instance_variable_defined?(:@_dependencies)
        base.instance_variable_get(:@_dependencies) << self
        return false
      else
        return false if base < self
        @_dependencies.each { |dep| base.send(:include, dep) }
        super
        #include module时，为base同时扩展类方法
        base.extend const_get(:ClassMethods) if const_defined?(:ClassMethods)
        #include module时，可以执行included中的方法体
        base.class_eval(&@_included_block) if instance_variable_defined?(:@_included_block)
      end
    end
    #被引用时执行的方法，这里的参数设计神了...
    def included(base = nil, &block)
      if base.nil?
        raise MultipleIncludedBlocks if instance_variable_defined?(:@_included_block)

        @_included_block = block
      else
        super
      end
    end
  end
end
```

写了一段代码来清晰的展示ActiveSupport::Concern的执行逻辑：    

```ruby
require 'active_support'

module A
  extend ActiveSupport::Concern

  included do 
    puts "in A"
  end
  #代码执行到这里，A extend ActiveSupport::Concern，于是多了两个类方法，然后又执行了included方法，A的变量@_included_block里面有了待执行的方法
end

module B
  extend ActiveSupport::Concern

  included do
    puts "in B"
  end
  #代码执行到这里，B extend ActiveSupport::Concern，于是多了两个类方法，然后又执行了included方法，B的变量@_included_block里面有了待执行的方法
  
  include A
  #include A，要执行的两个方法已经被覆盖了，由Concern中的方法取代，
  #B的@_dependencies的的值为[A]，并且没有执行到super方法，于是B的祖先链中并没有A（推测祖先链的改变是在append_features super里面做的）
end

class C
  include B
  #先执行B的append_features，由于B中已定义:@_dependencies，来到else分支，此时还没有执行super方法，故C < B 为false，
  #代码继续向下执行，于是执行了动态调用 C include A，再开始执行A的append_features...不再赘述分支逻辑
  #后面还有两句代码，一个是将A与B的module ClassMethod中的方法加到C的类方法中，一个是执行@_included_block中的方法体，
  #由于C include A 被动态调用，A处于C祖先链的更上方，A变量@_included_block中的方法体先被执行
end

#输出
#‘in A’
#‘in B’
```

C的祖先链，B A ...      
B的祖先链中没有A      

存在疑问：include module，变更对象祖先链是在append_features方法还是在included方法中操作的？或者是别的？


