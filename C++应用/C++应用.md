# <mark>C++应用</mark>

---



## <mark>随机数</mark>

传统C方法：

```c++
#include<cstdlib>
#include<ctime>
//初始化随机种子(只需一次)
srand(time(0));
int random_num = rand();
int random_inrange = rand()%100;//0-99
int random_inrange = rand()%100+1//1-100 
//随机质量不怎么高，易预测，不均匀
```

现代C++方法：

```c++
#include<random>

```

---



## <mark>关于std::</mark>

由于平时写算法题都用头文件：

`#include<bits/stdc++.h>`+`using namespace std;`

容易忽略了`std::`的使用;

```c++
std::工具名-->使用std仓库的某个工具
using namespace std-->默认从std使用工具
//实际工作时,不推荐使用上述头文件
//输入输出，字符串string，容器，算法函数等都需要加std::
std::cout<<"minecraft";
std::endl;
std::vector<int> a;std::map<std::string,int> mp;
std::string s = "hello";
std::sort(a,a+n);
std::swap(a,b);
std::max(x,y);std::min(x,y);
```

也可以只引入常用的几个：

```c++
using std::cout;
using std::cin;
using std::vector;
using std::endl;
//其他仍要加上std::
```

---



## <mark>矩阵乘法优化</mark>

```c++
int n,m,s;
cin>>n>>m>>s;//表示是n*m与m*s的矩阵相乘
//原始数学算法
for(int i = 0;i<n;i++){
    for(int j = 0;j<s;j++){
        for(int k = 0;k<m;k++){
            c[i][j] += a[i][k]*b[k][j];
        }
    }
}
fill(c.begin(),c.end(),vector<int>(s,0));
//下面是效率更高的算法，先当黑盒用
for(int i = 0;i<n;i++){
    for(int k = 0;k<m;k++){
        for(int j = 0;j<s;j++){
            c[i][j] += a[i][k]*b[k][j];    
        }
    }
}
//其实就是把基础版本的后两个for循环换了一下位置。
```

---




































