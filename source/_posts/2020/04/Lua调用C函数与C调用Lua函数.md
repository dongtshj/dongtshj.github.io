---
title: Lua调用C函数与C调用Lua函数
categories: C++
date: 2020-04-08
---

### Lua调用C

步骤：

* 将C的函数包装成Lua环境认可的函数
* 将包装好的函数注册到Lua环境中
* 像使用普通Lua函数那样使用注册函数好的C函数

#### 包装C函数
为了从Lua脚本中调用C函数，需要将被调用的C函数从普通的C函数包装成Lua_CFunction格式，并需要在函数中将返回值压入栈中，并返回返回值个数：

```
int (Lua_CFunction*)(lua_state*)
{
    // c code        // 实现逻辑功能
    // lua_push code // 需要将返回值压入堆栈
    return n;        // n为具体的返回值个数，告诉解释器，函数向堆栈压入几个返回值
}
```

例如：
```
int add(int a,int b)
{
    return a+b;
}
```
包装成：
```
int add(lua_state*L)
{
    int a = lua_tonumber(-1);
    int b = lua_tonumber(-2);
    int sum = a+b;
    lua_pushnumber(L,sum);
    return 1;
}
```

#### 注册C函数到Lua环境
将函数压栈，然后给函数设置一个在Lua中调用的名称,
```
lua_register(L,n,f);    // 参数n为Lua注册的函数名，f为对应的c函数名
```
lua_register是一个宏，对应两个函数:lua_pushfunction(L,f)和lua_setglobal(L,n)，将函数存放在一个全局table中。
```
lua_register(L,"Add2Number",add);//将c函数add注册到全局table[Add2Number]中
```

#### 调用注册好的C函数
像使用普通函数一样使用注册函数
```
Add2Number(1,2);
```

### C调用Lua
C++可以获取Lua中的值，可以调用Lua函数，还可以修改Lua文件

#### C获取Lua值
* 使用lua_getglocal来获取值，然后将其压栈
* 使用C API lua_toXXX将栈中元素取出转成相应的C类型的值

如果Lua值为table类型的话，通过lua_getfield和lua_setfield获取和修改表中元素的值

#### C调用Lua函数
* 使用lua_getglobal来获取函数，然后将其压入栈；
* 如果这个函数有参数的话，就需要依次将函数的参数也压入栈；
* 这些准备工作都准备就绪以后，就调用lua_pcall开始调用函数了，调用完成以后，会将返回值压入栈中；
* 最后取返回值，调用完毕。

#### 一个简单的示例
test.lua
```
name = "xchen"
version = 1
me = { name = "xchen", gender = "female"}
function add (a,b)
    return a+b
end
```

main.cpp
```
#include "lua.hpp"
#include <iostream>
using namespace std;

//显示栈内情况
static void stackDump(lua_State* L);

int main()
{
    //创建一个state
    lua_State *L = luaL_newstate();
    luaL_openlibs(L);

    //读lua文件
    int fret = luaL_dofile(L,"luac.lua");
    if(fret)
    {
        std::cout<<"read lua file error!"<<std::endl;
    }

    //读取变量
    lua_getglobal(L,"name");   //string to be indexed
    std::cout<<"name = "<<lua_tostring(L,-1)<<std::endl;

    //读取数字
    lua_getglobal(L,"version"); //number to be indexed
    std::cout<<"version = "<<lua_tonumber(L,-1)<<std::endl;

    //读取表
    lua_getglobal(L, "me");  //table to be indexed
    if(!lua_istable(L,-1))
    {
        std::cout<<"error:it is not a table"<<std::endl;
    }
    //取表中元素
    lua_getfield(L, -1 ,"name");
    std::cout<<"student name = "<<lua_tostring(L,-1)<<std::endl;
    lua_getfield(L,-2,"gender");
    std::cout<<"student gender = "<<lua_tostring(L,-1)<<std::endl;

    //修改表中元素
    lua_pushstring(L, "007");
    lua_setfield(L,-4, "name");
    lua_getfield(L, -3 ,"name");
    std::cout<<"student newName = "<<lua_tostring(L,-1)<<std::endl;

    //取函数
    lua_getglobal(L,"add");
    lua_pushnumber(L,15);
    lua_pushnumber(L,5);
    lua_pcall(L,2,1,0);//2-参数格式，1-返回值个数，调用函数，函数执行完，会将返回值压入栈中
    std::cout<<"5 + 15 = "<<lua_tonumber(L,-1)<<std::endl;

    //查看栈
    stackDump(L);

    //关闭state
    lua_close(L);
    return 0;
}

static void stackDump(lua_State* L) {
    cout << "\nbegin dump lua stack" << endl;
    int i = 0;
    int top = lua_gettop(L);
    for (i = 1; i <= top; ++i) {
        int t = lua_type(L, i);
        switch (t) {
            case LUA_TSTRING: {
                printf("'%s' ", lua_tostring(L, i));
            }
                break;
            case LUA_TBOOLEAN: {
                printf(lua_toboolean(L, i) ? "true " : "false ");
            }
                break;
            case LUA_TNUMBER: {
                printf("%g ", lua_tonumber(L, i));
            }
                break;
            default: {
                printf("%s ", lua_typename(L, t));
            }
                break;
        }
    }
    cout << "\nend dump lua stack" << endl;
}
```
