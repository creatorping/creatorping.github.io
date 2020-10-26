---
title: c++11中的tuple和array
data: 2020-10-21 14:51:00 +0800
categories: [编程语言,C++从入门到放弃]
tags: [C++从入门到放弃]
---

## tuple的使用

tuple是一个固定大小的不同类型值的集合，是泛化的std::pair。和c#中的tuple类似，但是比c#中的tuple强大得多。我们也可以把他当做一个通用的结构体来用，不需要创建结构体又获取结构体的特征，在某些情况下可以取代结构体使程序更简洁，直观。

~~~C++
#include<iostream>
#include<tuple>
using namespace std;
int main()
{
    tuple<int,int,float> t1(1,2,3.3),t2;//元组
    cout<<get<0>(t1)<<endl;
    cout<<get<1>(t1)<<endl;
    cout<<get<2>(t1)<<endl;

    t1=make_tuple(6,7,8.1);
    cout<<tuple_size<decltype(t1)>::value<<endl;//decltype的作用是选择并返回操作数的数据类型
    //cout<<tuple_size<tuple<int,int,float> >::value<<endl;
}

~~~

## array的使用

std::array是具有**固定大小**的数组，支持快速随机访问，不能添加或删除元素，定义于头文件<array>中。

### 一、概要

　　array是C++11新引入的容器类型，与内置数组相比，array是一种更容易使用，更加安全的数组类型，可替代内置数组，作为数组升级版，继承数组最本特性，同时融入部分操作。

### 二、定义与初始化

　　array和数组一样，为固定大小容器类型，定义时即需声明大小与类型
　　
+ 1）内置数组初始化
　　　　int array[10] = {0};
　　　　int array[10] = {1,2,3,4,5,6,7,8,9,0};
　　　　int array[]   = {1,2,3,4,5,6,7,8,9,0};
　　　　int array[10] = {1,2,3};//后面的7个数据的值在Qt上会被随机
　　　　注：数组的初始化不能直接使用赋值，只能通过遍历的形式拷贝

+ 2）array的初始化
　　　　std::array<int,10> a = {1,2,3,4,5,6,7,8,9,0};
　　　　std::array<int,10> a{1,2,3,4,5,6,7,8,9,0};
　　　　std::array<int,10> a{1,2,3};//后面的7个数据的值在Qt上会初始化为0
　　　　int arr[10]{10,9,8,7,6,5,4,3,2,1};
　　　　std::array array<int,10>;
　　　　memcpy(array.data(),arr,sizeof(arr));//拷贝初始化
　　　　array.fill(5);//填充初始化(所有数据初始化为5)
　　　　array = arr;//赋值初始化

### 三、访问

+ 1）内置数组的访问：下标、指针和迭代器
　　　　int array[10] = {1,2,3,4,5,6,7,8,9,0};
　　　　int value = array[0];//通过下标获取数组的第一个元素
　　　　int value = *p;　　 //通过指针获取数组的第一个元素
　　　　for(int *i=bebin(array);i != end(array);i++)//C++11中为了给数组提供更加安全的访问方式，引入了begin()和end()函数
　　　　{
　　　　　　std::cout << *i << " ";
　　　　}
　　　　std::cout << std::endl;

+ 2）array的访问：下标、at、指针和迭代器
　　　　std::array<int,10> array{1,2,3,4,5,6,7,8,9,0};
　　　　//下标访问
　　　　for(std::size_t i=0;i<array.size();i++)
　　　　{
　　　　　　std::cout << array[i] << " ";
　　　　}
　　　　std::cout << std::endl;

　　　　//font、at、back访问
　　　　for(std::size_t i=0;i<array.size();i++)
　　　　{
　　　　　　std::cout << array.at(i) << " ";
　　　　}
　　　　std::cout << std::endl;
　　　　//正向、反向、常量迭代器：begin、cbegin、rbegin、crbegin、end、cend、rend、crend
　　　　std::array<int,10>::iterator iter;
　　　　for(iter=array.begin;iter != array.end();iter++)
　　　　{
　　　　　　std::cout << *iter << " ";//迭代器访问
　　　　}
　　　　std::cout << std::endl;
　　　　//指针:data
　　　　int arr[10]{10,9,8,7,6,5,4,3,2,1};
　　　　std::array array<int,10>;
　　　　memcpy(array.data(),arr,sizeof(arr));
　　　　for(std::size_t i=0;i<array.size();i++)
　　　　{
　　　　　　std::cout << array[i] << " ";/./输出：10 9 8 7 6 5 4 3 2 1
　　　　}
　　　　std::cout << std::endl;
　　　　
　　array是数组的升级版，将数组正式纳入到容器范畴，array在使用和性能上都要强于内置数组，对于固定大小的使用场景，可用array替代数组工作。

　　尽量使用at方法来访问元素，因为运算符[]不会对索引值进行检查，像array[-1]是不会报错的。使用at()将在运行期间捕获非法索引的，默认将程序中断。