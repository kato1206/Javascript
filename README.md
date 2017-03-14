第一题

这一段代码哪里错了，怎么修改？
function(a,b){ a*b }

这里我的回答是：
function(a,b){ return a*b }

然而我忽略了a,b的值。就好像当年初中的时候做数学题一样，那时候有种题目叫做因式分解，然后大家最常见的错误就是“你知道a等不等于0吗你就约掉，这里要分情况懂吗？”

所以：
function(a,b){ 
  if( a==undefined || b==undefined || isNaN(a) || isNaN(b) ) 
    throw a new Error ( " Both arguments should be Number !");
  return a*b }

知识点： 函数的返回值，javascript中的数据类型；

第二题

西部郊区槌球俱乐部有两个类别的会员，高级和开放。他们希望你的帮助与申请表，将告诉潜在成员他们将放置哪个类别。

成为高级会员，会员必须至少55岁，残障比大于7.在这个槌球俱乐部，残疾人从-2到+26; 玩家越低障碍越好。

输入将由包含两个项目的列表列表组成。每个列表包含单个潜在成员的信息。信息包括人的年龄的整数和人的障碍的整数。

示例输入：[[18, 20],[45, 2],[61, 12],[37, 6],[21, 21],[78, 9]]

这里我的回答是：
function openOrSenior(data){
  var member = [];
  for(var i = 0; i<data.length;i++){
    if(data[i][0]>55 && data[i][1] >7 && data[i][1] <26){
      member[i] = "Senior";
    }else{
      member[i] = "Open";
    }
  }
  return member
}
这里函数能够实现要求的功能，但是代码很挫，基本就是入门小白写的（嘛，现在你就是小白），后面是高级的回答，但是这里先要讨论我发现的问题。
我首先手欠，将定义放到了if语句中，像这样：
if(...){
  var member[i] = "Senior";
}

报错了，为什么，我看了一会，发现是命名出错，这样系统默认 member[i] 是一个变量名，然而变量名是有 字母，数字，下划线和 '$'组成，下划线不能开头，所以'['是不能作为变量名的。

OK,然后我又试了一下这样写：

if(...){
  var member = [];
  member[i] = "Senior";
}

这回对了吗？又出错了，下面的member[i]报错说不能把'0'赋给未定义变量。What?然后我翻书啊，查资料啊，不是说好的javascript是函数作用域吗？变量只要在函数里面声明了，就在整个函数体内可用了吗？
原来，声明了是没错，但是仅仅是声明提前，赋值可没有提前。
也就是说，上面的代码，相当于这样：

var member;
if(...){
  member = [];
  member[i] = "Senior";
}
好吧，原来将变量定义为数组也是一个赋值的过程。所以下面的member是undefined啊。所以is not define 和 undefined是两码事啊！

好了，再看看别人的高级解答：
function openOrSenior(data){
   function determineMembership(member){
    return (member[0] > 55 && member[1] > 7) ? "Senior" : "Open";
   }
   return data.map(determineMembership);
}

哇！好高级有木有！这个retrun里面是什么写法啊？这个.map()是什么方法啊？

条件?真:假 

这是if语句的简写方式，具体的简写就不找了。
.map()方法呢，简单来讲，就是利用一种函数方法，将数组中的每一个元素处理后，返回。
官方示例：[1,2,3].map(function(a){return a*a;}); // => [1,4,9]
示例很简单易懂，但实际应用又是一回事，后面还有很多机会可以接触到。
