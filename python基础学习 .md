# 函数，类，模块
## 函数
### 定义函数
  定义一个可以被复用的函数是第一步，想象我没有函数的时候，对多个文件名进行一种规则性修改
  比如我想做一个对文件名称固定的修改。
  ```python
    def modify_name(filename):
      filename += ".txt"
      filename = "my" + filename
      print(filename) 
 modify_name("diandian")
 ```
 这样我就完成了对函数的定义和修改文件名的操作，函数的定义是通过def关键字来完成的，后面跟上函数的名字和参数列表，函数体则是缩进的代码块.
 ### 参数设置
 在参数设置中对应方法有如下几种
 ```
def f(x,a,b,c):
  return a*x**2 + b*x + c*1
print(f(2, 1, 1, 0))    # 忽略参数名，按顺序传参
print(f(x=2, a=1, b=1, c=0))  # 写上参数名，按名字传参
print(f(a=1, c=0, x=2, b=1))  # 若用参数名，可以打乱顺序传参
```
### 全局变量和局部变量
|变量|	特点|
|:---:|:---:|
|全局 global|	函数里外都能用 （公用）|
|局部 local|	仅在函数内有用 （私有）|

就是说在这里如果在函数内的变量不能再函数外使用，如果要使用必须有global

## class类
### 定义class类
基本逻辑就是先用class定义一个类（比如 class File()：），然后再创建一个具体的文件  
file = File()  
这样就初始化定义了一个class类
```
class File:
    def __init__(self):
        self.name = "f1"
        self.create_time = "today"

my_file = File()
print(my_file.name)
print(my_file.create_time)
```
### class的功能
__init __是类中最基础的功能，真正的价值：统一入口 + 自动初始化  
可以：  
1.规定对象必须有哪些属性  
2.创建时自动赋值，不用手动补  
3.每个对象独立，互不污染  
4.可以加校验，保证数据合法
接下来为代码案例：
```
class File:
    def __init__(self, name, create_time="today"):
        self.name = name
        self.create_time = create_time
    
    def change_name(self, new_name):
        self.name = new_name

my_file = File("my_file")

# 调用实例中，类的功能
my_file.change_name("new_name") 
print(my_file.name)
```
类也可以有返回值，比如：
```
 def get_info(self):
        return self.name + "is created at" + self.create_time
```
### class类的继承
完全继承：  
在定义完新的class之后，直接用pass完全继承  

一般类的继承如下：
上边我写过一个class File: 现在我新建一个子类Video， 
我在class Video后边的括号中加入我的父类(File):
这样就可以告诉计算机我要继承的对象是什么，然后我通过super()__init __ (),括号中加入我需要调用的功能，如(name = name，creat_time = "today")  再描述Video的基础功能
代码如下：
```
# 1. 定义一个【父类/基类】，名字叫 File（文件）
# 这是所有文件的通用模板，包含所有文件共有的属性和方法
class File:
    # 2. 父类的构造方法：创建文件对象时自动执行
    # 参数：name(文件名)，create_time(创建时间，默认值today)
    def __init__(self, name, create_time="today"):
        # 3. 给对象绑定【实例属性】：文件名
        self.name = name
        # 4. 给对象绑定【实例属性】：创建时间
        self.create_time = create_time
    
    # 5. 父类的通用方法：获取文件的基础信息
    def get_info(self):
        # 6. 返回拼接好的字符串：文件名+创建时间
        return self.name + "is created at" + self.create_time

# 7. 定义【子类/派生类】Video，继承自父类 File
# 语法：class 子类名(父类名) → 自动拥有父类所有属性+方法
class Video(File):  
    # 8. 子类Video的构造方法：独有参数 window_size（分辨率，默认1080*720）
    def __init__(self, name, window_size=(1080, 720)):
        # 9. ✨核心：super() 调用父类的 __init__ 方法
        # 复用父类的初始化逻辑，不用重复写 name 和 create_time
        super().__init__(name=name, create_time="today") 
        # 10. 子类【独有属性】：视频分辨率（文件类没有这个属性）
        self.window_size = window_size

# 11. 定义【子类】Text，继承自父类 File
class Text(File): 
    # 12. 子类Text的构造方法：独有参数 language（语言，默认中文）
    def __init__(self, name, language="zh-cn"):
        # 13. 调用父类构造方法，初始化通用属性
        super().__init__(name=name, create_time="today") 
        # 14. 子类【独有属性】：文本语言（视频类没有，父类也没有）
        self.language = language
    
    # 15. 子类【独有方法】：获取更详细的信息
    def get_more_info(self):
        # 16. ✨复用父类的 get_info() 方法，再拼接自己的独有属性
        return self.get_info() + ", using language of " + self.language

# 17. 创建 Video 类的对象 v，传入参数 "my_video"
v = Video("my_video")
# 18. 创建 Text 类的对象 t，传入参数 "my_text"
t = Text("my_text")

# 19. 子类对象 调用 父类的方法 → 正常执行
print(v.get_info())     
# 20. 子类对象 调用 父类的属性 → 正常执行
print(t.create_time)    
# 21. 子类对象 调用 自己的独有属性
print(t.language)       
# 22. 子类对象 调用 自己的独有方法（内部复用了父类功能）
print(t.get_more_info())
```
### 私有化
|私有|	特点|
|:---|:---|
|_ 一个下划线开头|	弱隐藏 不想让别人用 （别人在必要情况下还是可以用的）|
|__ 两个下划线开头	|强隐藏 不让别人用|

# 文件
## 文件读取
### 凭空建立文件

```
f = open("new_file.txt","w") 
# w代表write，表示写入模式，r代表read，表示只读模式
```
|mode|	意思|
|:---|:---|
w	|（创建）写文本
r	|读文本，文件不存在会报错
a	|在文本最后添加
wb	|写二进制 binary
rb	|读二进制 binary
ab	|添加二进制
w+	|又可以读又可以（创建）写
r+	|又可以读又可以写, 文件不存在会报错
a+	|可读写，在文本最后添加
x	|创建

```
f.write("...")
#在文件中写入东西
```
但是在平常写入过程中一般使用的是
```
with open("文件名称","w",encoding = "utf-8") as f:
    f.readlines()
    f.writelines()
```
## 文件目录管理
### os库
os库是Python自带的一个非常有用的模块。如果你做一个系统性的东西，要处理文件输出，读入等问题， 那么你有80%概率会使用到os库中的一些功能。 os模块有非常多功能，我们今天就来侧重将一些你日常会经常使用的。
#### 文件目录操作
|函数|作用|
|:---|:---|
os.getcwd()	|获取当前工作目录，返回 Python 脚本当前运行的文件夹路径（Get Current Working Directory）
os.listdir()|	列出目录内容，返回指定路径下所有文件和子文件夹的名称列表；不指定路径时默认列出当前目录
os.makedirs()|	递归创建多级目录，可一次性创建多层嵌套目录（如 a/b/c），若中间目录不存在会自动创建
os.path.exists()|	路径存在性检查，判断文件或文件夹是否存在，存在返回 True，否则返回 False

#### 文件管理系统
|函数	|作用
|:---|:---|
os.removedirs()	|递归删除空目录，只能删除多级空目录（子目录必须为空）
shutil.rmtree()	|强制删除目录树，无论目录是否包含文件都能删除（⚠️ 操作不可逆，需谨慎），需先导入 shutil 模块
os.rename()	|重命名 / 移动文件 / 目录，可修改文件 / 文件夹名称，也可通过修改路径实现文件移动

#### 文件目录多种检验
|函数|	作用|
|:---|:---|
os.path.isfile()	|判断是否为文件，若路径指向一个存在的文件则返回 True
os.path.exists()	|同前，检查路径（文件 / 文件夹）是否存在
os.path.isdir()	|判断是否为目录，若路径指向一个存在的文件夹则返回 True
os.path.basename()	|提取路径末尾名称，返回路径最后一部分（文件名或文件夹名）
os.path.dirname()	|提取路径目录部分，返回路径中除最后一部分外的父目录路径
os.path.split()	|分割路径，将路径拆分为 (父目录路径, 末尾名称) 的元组

### 正则表达式

