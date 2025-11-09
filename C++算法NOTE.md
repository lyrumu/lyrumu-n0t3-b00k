# C++/C算法n0t3

---

## <<算法>>

---

### <mark>qsort快排（c语言）</mark>

1.qsort函数原型

```c++
void qsort(void *base, size_t nmemb, size_t size, 
           int (*compar)(const void *, const void *));
```

2.具体写法
（1）先写<mark>比较函数</mark> （返回的是整形）

```c++
// 升序排序（从小到大）
int cmp_asc(const void *a, const void *b) {
    return (*(int*)a - *(int*)b);
}
// 降序排序  （从大到小）
int cmp_desc(const void *a, const void *b) {
    return (*(int*)b - *(int*)a);
}
```

解释：因为排序的类型可能是int,double等各种，所以用<u>const void*a</u>表示可以<u>指向任何类型</u>的指针，然后在return中先将a转化成对应题目类型的指针例如<u>（int*）</u> ，再<u>在前面加*</u> 表示指针所指的具体值
（2）再在<mark>主函数中调用</mark> 

```c++
qsort(a,n,sizeof(int),cmp);
```

a表示要排序的数组的首地址
n表示数组长度
sizeof（int）表示这个数组类型的数据长度
cmp是提前写好的比较函数的名称
3.原理
比较函数cmp中，（~~有点难理解~~但很重要！！）
若返回负值--->将参数a排在参数b前面
若返回0--->参数a，参数b顺序不变
若返回正值--->将参数a排在参数b后面
依此可以写各种其他不同排序：（待开发）

---

### <mark>一维前缀和 </mark>

1.为何诞生？
Runtime Error：
个人是因为，在计算数组的各个子数组，各个元素之和时复杂度太大
前缀和算法能够帮助提高效率，处理更大更复杂的数据
2.定义前缀和
数组a[n];
令sum[i] = a1​+a2​+⋯+ai​（其中sum[0] = 0）
3.具体用法
计算一维数组的各个前缀和：

```c++
int a[n];//先定义一个大小乱序的数组
//这里省略数组初始化...
long long presum[n+1] = {0};//初始化前缀和先都为0,大小为n+1防止越界(一定要开足空间！！)
for(int i = 1;i<=n;i++){//i从1开始，更符合人类习惯，"前i个的和..."
    presum[i] = a[i-1]+presum[i-1];//第i个元素+前i-1个
}
```

4.结合例题
题目（一）：利用前缀和，计算数组的各个子数组中的各个元素之和
解法：

```c++
int a[n];
//这里省略初始化数组，省略前缀和公式
for(int l = 0;l<n;l++){
    for(int r = l;r<n;r++){
        //a[r]是第r+1个元素，a[l]是第l+1个元素***
        int sum = presum[r+1]-presum[l];//***
    }
}
```

---

### <mark>滑动窗口</mark>

直接上题：
![滑动窗口题目哦](images/屏幕截图%202025-11-01%20230151.png)
解法：（只是为了介绍此算法，实际仍是超时哦，还是需要用特殊的前缀和算法）

```c++
#include <bits/stdc++.h>
using namespace std;
int main(){
    int n,a,b;
    cin>>n,a,b;
    vector<char> s(n);
    for(int i = 0;i<n;i++){
        cin>>s[i];
    }
    int cnt = 0;//计数，表示满足条件的（l，r）的个数
    for(int l = 0;l<n;l++){//以下展现了滑动窗口思想***
        int cnta = 0;
        int cntb = 0;//在l固定的时候就初始化计数a，b的个数
        for(int r = l;r<n;r++){
        //扩展右边界时更新计数
        //即，r每更新一次，就假设此时的（l，r）为一组，立刻测试本组的cnta，cntb是否满足条件。
        //依此，可以省去第三重for循环（原本是要确定l，r后再遍历的）
            if(s[r]=='a') cnta++;
            else if(s[r]=='b') cntb++;
            if(cnta>=a && cntb<b){//每次都要判断哦
                cnt++;
            }
        }
    }
    cout<<cnt;
    return 0
```

---

### <mark> 二维前缀和</mark>

1.定义二维前缀和

一个n*m的二维数组矩阵a【n】【m】；
令sum【i】【j】 = $\sum_{i=1}^{n} \sum_{j=1}^{m} \text{a}[i][j]$;

 2.具体用法

计算二维数组的各个前缀和

```c++
//注意！为了后续方便，若要计算二维数组前缀和
//初始化赋值时下标从1开始！！***
int a[n+5][m+5];
for(int i = 1;i<=n;i++){
    for(int j = 1;j<=m;j++){
        cin>>a[i][j];
    }
}
//计算前缀和
int presum[n+5][m+5];
for(int i = 1;i<=n;i++){
    for(int j = 1;j<=m;j++){
        presum[i][j] = a[i][j]+presum[i-1][j]+presum[i][j-1]-presum[i-1][j-1];//*** 
    }
}
```

3.结合例题

题目（一）：一个二维矩阵，计算其各个子矩阵的各个元素之和
解法：

```c++
//先枚举所有子矩阵
for(int r1 = 1;r1<=n;r1++){
    for(int r2 = r1;r2<=n;r2++){//r1,r2分别表示上下边
        for(int c1 = 1;c1<=m;c1++){
            for(int c2 = c1;c2<=m;c2++){//c1，c2表示左右边
                //计算子矩阵元素和
                int sum = presum[r2][c2]-presum[r1-1][c2]-presum[r2][c1-1]+presum[r1-1][c1-1];//***
            }    
        }
    }
}
```

题目（二）：

---

### <mark>冒泡排序</mark>

```c++
#include <bits/stdc++.h>
using namespace std;
int a[n];
//这里省略对数组a的输入
for(int i = 0;i<n-1;i++){//n个数，则进行n-1次交换
    for(int j = 0;j<n-1-i;j++){//每一次的交换（因为已经完成了i次交换）
        if(a[j]>a[j+1]){//从小到大排序
            swap(a[j],a[j+1]);//交换
        }
    }
}
```

---  

### <mark>GCD&LCM</mark>

最大公约数函数（gcd）：

```c++
int gcd(int a,int b){
    if(b==0){
        return a;
    }
    return gcd(b,a%b);//欧几里得算法，辗转相除法，递归版
}//以上参数顺序不可以调换！
```

最小公倍数函数（lcm）：

```c++
int lcm(int a,int b){
    if(a==0 || b==0){
        return 0;
    }
    //return (a*b)/gcd(a,b);为了防止溢出，先除以gcd，再乘：
    return a/gcd(a,b)*b;//***
}
```

---

### <mark>选择排序</mark>

核心思想：每次选择最大或最小的元素，将其排在正确位置。

```c++
using namespace std;
int arr[n];
for(int i = 0;i<n-1;i++){//n个元素交换n-1次，这次寻找第i小的元素
    int minindex = i;//假设当前i就是最小的索引
    for(int j = i+1;j<n;j++){//在之后的索引，寻找更小的元素
        if(arr[j]<arr[minindex]){
            minindex = j;//找到的话改变minindex
        }
    }
    swap(arr[i],arr[minindex]);//将第i小的元素放在相应位置
}
```



---

### <mark>贪心</mark>











---



### <mark>差分</mark>

差分可以先理解为`前缀和`的`逆过程`

- 前缀和：给定数组`a`,需我们创建数组`presum`
  其中`presum[i]=a[1]+a[2]+..+a[i]`,用于计算区间和

- 差分：
  将给定的数组`a`视为一个`presum`,创建一个数组`b`
  
  使得`a[i]=b[1]+b[2]+..+b[i]`,数组b就是a的差分

定义差分数组：

`chafen[i] = a[i]-a[i-1]`

应用：

`将对一个数组区间修改的操作，转化为对其差分数组两个单点的修改操作`

eg：

需要对数组`a`的区间`[l.r]`上的每一个元素都加上`c`

```c++
//进行两个单点操作即可
chafen[l] = chafen[l]+c;
chafen[r+1] = chafen[r+1]-c;
```

注意，修改后要对差分数组求一次前缀和，进行还原，作为新的修改后的数组`a`



---



### <mark>哈希表键值对</mark>

一.语法：

`unordered_map<键类型，值类型> 变量名;`

eg:

```c++
//基本初始化
unordered_map<int,string> student_id;// 学号→姓名
unordered_map<string,double> product_price;//产品名→价格
unordered_map<ll,ll> freq;// 数值→出现次数
//插入键值对
scores["Alice"] = 100;//Alice为键，100为其对应的值
scores.insert({"Bob",60});//插入一个键值对
//访问元素
cout<<scores["Alice"];//输出的是100
cout<<scores["David"];//自动创建{"David":0}并输出0
```

```c++
//高级函数
if(scores.count("Alice")){
    //键存在
}
scores.erase("Alice");//删除键为Alice的元素
```

二.特性：

1.无序性。

相对map来说，map是有序的键值对，但访问速度没有哈希表快速。元素的存储顺序与插入顺序无关，遍历时顺序也不确定。

2.自动初始化。

eg：

```c++
unordered_map<int,int> m;
cout<<m[5];//输出0,同时自动创建{5:0}
```

即，即使哈希表的一些键值对还没有被初始化时，此时若直接输出或者访问某个键，会自动创建键值对，并初始化键的值为0。

三.用法：

1.统计频率等

---



### <mark>两两乘积之和化简</mark>

eg:一个数组a[n];计算数组中所有不同的两个元素的乘积之和（前后顺序相反的算同一组）

即a1​⋅a2​+a1​⋅a3​+⋯+a1​⋅an​+a2​⋅a3​+⋯+a(n−2)​⋅a(n−1)​+a(n−2)​⋅an​+a(n−1)​⋅an

算法思维：

​![乘积化简](./images/屏幕截图%202025-11-07%20231654.png)

具体代码：

```c++
ll a[n];//ll 表示long long
ll total_sum = 0;
ll square_sum = 0;
for(ll i = 0;i<n;i++){
    cin>>a[i];
    total_sum = total_sum+a[i];
    square_sum = square_sum+a[i]*a[i];
}
ll sum = (total_sum*total_sum-square_sum)/2;
```



---

## <<语法>>

---

### <mark>结构体struct</mark>

是一种`自定义数据类型`

基本结构：将多个**不同类型变量组合**在一起，形成一个逻辑上的整体

```c++
// 定义结构体
struct Student {//定义Student是一种数据类型！！
    string name;// 姓名
    int age;// 年龄  
    double score;// 分数
};//别忘了有分号！！！
```

然后，可以在主函数中使用`Student`这个数据类型

例如`Student Xiaoming`;

那么如何访问一个结构体中的各个成员？

答案是`用“.”来访问`.

```c++
Student Xiaoming;
Xiaoming.name = Xiaoming;
Xiaoming.age = 18;
Xiaoming.score = 100.0;
```

当然也可以直接按结构体内的顺序初始化：

```c++
Student a = {Xiaoming,18,100.0};
```

或指定成员初始化：

```c++
Student a = {.name = Xiaoming,.age = 18,.score = 100.0};
```

---



### <mark>cin&cout </mark>

1.输入cin：

混合输入字符串和数字要注意ignore缓冲区：

```c++
int a;
string str;
cin>>a;
cin.ignore.();//清空缓冲区，否则无法正常使用getline
getline(cin,str);
//注意，cin中不能使用endl换行符；
```

2.输出cout:

```c++
int a
cin>>a;
cout<<a;
int s[n];
for(int i = 0;i<n;i++){//在需要高性能的环境中
    //cout<<s[i]<<endl;
    cout<<s[i]<<'\n';//是循环中更好的换行方式
}
```

3.关闭输入输出同步流，提升程序效率

```c++
ios::sync_with_stdio(false);
cin.tie(0);
cout.tie(0);
```

---



### <mark>类型转换</mark>

首先方便起见，初学使用`using namespace std;`

```c++
//字符串-->数值
string str = "123";
int num1 = stoi(str);
double num2 = stod(str);
float num3 = stof(str);
//数值-->字符串
string str1 = to_string(num1);
```



---

### <mark> 数学函数</mark>

| 数学函数                   | 库中对应函数语法       |
|:----------------------:|:--------------:|
| f(x)=lnx               | log(x)         |
| f(x)=lgx               | log10(x)       |
| f(x)=\|x\|(int,double) | abs(x),fabs(x) |
| f(x)=向下取整              | floor(x)       |
| f(x)=向上取整              | ceil(x)        |
| f(x)=$\sqrt(x)$        | sqrt(x)        |
| 取大                     | max(a,b)       |
| 取小                     | min(a,b)       |

注意：*三角函数*在库中都以<u>弧度制</u>表示，即传入的参数应该是$\pi$ 之类的，而不是180°之类的。

---

### <mark>++i&i++的区别</mark>

注：理解两者的区别对于提高<u>代码效率</u>非常重要！！！

1.++i：

先将i的值加一，再返回i.

2.i++:

 先返回i本身的值，再将i加一.

---

### <mark>"&"引用</mark>

引入一个重要的符号：**<u>&</u>**

在c++中，&除了有取地址的作用外，还可以**表示引用**！

```c++
//&表示引用的基本含义
//&引用的本质就是给变量取别名
int a = 10;
int &b = a;// b是a的引用（别名）
b = 20;// 现在a也变成了20
cout << a;// 输出20
```

主要用于函数的参数部分：

```c++
//一.要通过函数修改外部变量
void func(变量类型 &变量名){}//类似*，但更方便
//二.不修改外部变量且是简单类型，不考虑性能
void func(变量类型 变量名){}
//三.不修改外部变量，但数据类型大，性能要求高
void func(const 变量类型 &变量名){}//能避免拷贝
```

**总之，**

**问自己：我需要修改这个参数吗？**

* **要修改** → 用 `&`

* **不修改** → 继续问：这个参数大吗？
  
  * **大**（结构体、字符串、数组）→ 用 `const &`
  
  * **小**（int、double、char）→ 什么都不加
    
    

---



### <mark>sort函数（c++库）</mark>

基本形式：

```c++
// 基本形式1：使用默认的升序排序
sort(first, last);
//更通用的形式：比较函数：
sort(first,last,cmp);
```

`first`表示要排序的范围的开始位置，`end`则是结束位置，区间可理解为**左闭右开**.

基本语法&示例：

```c++
#include <bits/stdc++.h>
using namespace std;
int a[n];
for(int i = 0;i<n;i++){
    cin>>a[i];//初始化一个数组，对数组中的元素进行排序
}
sort(a,a+n);//升序排列
//由于要一直排序到索引为n-1的元素，且左闭右开，因此写结束位置为(n-1)+1--->n！
return a<b;//升序
}
```

比较函数：(与c的qsort原理不同)

`bool`返回`true`，sort函数会将`参数a`排在`参数b`前面;

反之`bool`若返回`false`则会把`参数a`排在`参数b`后面;

```c++
bool cmp(int a,int b){
    return a<b;//升序
}

bool cmp1(int a,int b){
    return a>b;//降序
}
```

复杂度：n*log(n);



---

### <mark>swap(交换函数)</mark>

基本语法&示例：

```c++
#include <bits.stdc++.h>//万能头文件
using namespace std;//要用swap的话一定要写std库！！

int x = 5, y = 10;
swap(x, y);//交换基本类型
------------------------------
string s1 = "Hello", s2 = "World";
swap(s1, s2);//交换字符串
------------------------------
int arr[] = {1,2,3,4,5};
swap(arr[0], arr[4]);// 交换第一个和最后一个元素
```

---

### <mark>string(c++)</mark>

string是一个封装了**字符数组**的类，自动管理内存。专门存放**字符序列**。
string类型的变量，**下标索引也是从0开始**。
特性1：变量之间可以**直接相加+**，自动管理内存！

```c++
string s1 = "Hello";
string s2 = "World";
string s3 = s1 + " " + s2;//直接使用+运算符，也可以用于循环不断增加！
```

特性2：**初始化**较便利

```c++
string s1 = "Hello";//C风格字符串字面量
string s2 = 'A';//错误！不能直接存放单个字符
string s3(1,'A');//正确！创建包含1个字符'A'的字符串
string s3 = "A";//但可以用字符串字面量
string s4 = "中文";//可以存放中文字符（在合适的编码下）
string s5 = "Hello\x20World";//可以包含转义字符
string s6 = ""; //空字符串
```

（但注意，（1）string类型也是**不能由cin直接输入空格和换行符**的！！！需要使用一个getline()哦（2）混合输入时，要在合适位置使用cin.ignore()清空缓冲区）
特性3：运算符重载，可直接比较**字典序**大小

```c++
string s1 = "hello";
string s2 = "world";
if(s1>s2){
    //.....
}
```

特性4：丰富的成员**函数**(下面不是所有的哦)

```c++
string str = "Hello World";

int len = str.size();//len = 11
cout<<str.find("World");//--->6
str.append("!");//str = "Hello World!"

str.front();//访问第一个元素
str.back();//访问最后一个元素
str.find_first_of("aeiou");//查找第一个出现的元音字母的索引位置
str.find_last_of("aeiou");//查找最后一个出现的元音字母的索引位置
cout<<str.substr(提取的起始位置，提取字符数量)；//
string str1(str);//将str的内容拷贝到新创建的str1

str.clear()//清空

reverse(str.begin(),str.end());//反转字符串
```

---

### <mark>getline(c++)</mark>

只适用于`string`类型!主要用于读取需要包括`空格`的文本.

```c++
#include <bits/stdc++.h>
using namespace std;
string names;
getline(cin,names);//基本格式，能读取空格，默认读到换行符为止！！
```

自定义分隔符（类似scanf的扫描集）

```c++
#include <bits/stdc++.h>
using namespace std;
string data;
getline(cin,data,',');//读到逗号为止
```

`cin.ignore()`清空混合输入缓冲区.eg:

```c++
int num;
string text;
cin>>num;
cin.ignora();//必须调用，清除缓冲区的换行符
getline(cin,text);
```



---

### <mark>fgets(c语言)</mark>











---

### <mark>map容器(c++)</mark>



















---



### <mark>pair(c++模板类)</mark>

作用：
可以把两个值打包成一个对象（一个变量名，类似成为一个组合），两个值可以是不同的数据类型

```c++
pair<类型1，类型2> 变量名；
```

用法：

```c++
// 方式1：先声明后赋值
pair<string,int> p1;
p1.first = "Bob";//这里first，second是pair专用的访问元素方法
p1.second = 80;

// 方式2：声明时直接初始化
pair<string,int> p2("Cathy", 88);

//方式3：表达一系列pair对，例如n个学生的姓名以及对应成绩
vector<pair<string,int>> student(n);
for(int i = 0;i<n;i++){
    cin>>student[i].first>>student[i].second;//对其进行输入
}

//方式4：初始化pair序列
vector<pair<int,int>> biggest(m,{0,0});//表示有m个{0，0}对序列
```

---

## <<细节问题>>

---

### <mark>字符串需主动添加'\0'(NULL)的情况</mark>

(1)使用<u>循环对逐个字符赋值</u>，构造字符串时：

```c++
char dest[10];
for(int i=0; i<3; i++) {
    dest[i] = source[i];
}
dest[3] = '\0';  // 必须手动添加****
```

(2)<u>手动逐个字符赋值</u>，构造字符串时：

```c++
char str[10];
str[0] = 'H';
str[1] = 'i';
str[2] = '\0';  // 必须手动添加****
```

(3)使用<u>非字符串函数</u>操作时：

```c++
char str[10];
memcpy(str, "Hello", 5);  // 只复制5个字符，不包括\0
str[5] = '\0';  // 必须手动添加****
```

---
