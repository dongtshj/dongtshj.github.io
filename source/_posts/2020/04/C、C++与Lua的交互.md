---
title: C、C++与Lua的交互
categories: C++
date: 2020-04-08
---

### 栈

C/C++与Lua的交互通过栈来实现的，可以通过下面API来对栈进行操作：

```
lua_toboolean()
lua_tonumber()
lua_tostring()
lua_pushboolean()
lua_pushinteger()
lua_pushnumber()
lua_pushstring()
...
```

### Lua调用C函数

被调用的C函数需要满足下面的函数原型：

```
typedef int (*lua_CFunction) (lua_State *L);
```

下面例子中的C函数将接收若干数字参数，并返回它们的平均数与和：

```
static int C_FUNC_SUM (lua_State *L) 
{
    int n = lua_gettop(L);          /* 参数的个数 */
    lua_Number sum = 0.0;
    int i;
    for (i = 1; i <= n; i++) {
        if (!lua_isnumber(L, i)) {
        lua_pushliteral(L, "incorrect argument");
        lua_error(L);
        }
        sum += lua_tonumber(L, i);
    }
    lua_pushnumber(L, sum/n);       /* 第一个返回值 */
    lua_pushnumber(L, sum);         /* 第二个返回值 */
    return 2;                       /* 返回值的个数 */
}
```

### C调用Lua函数

```
void lua_call (lua_State *L, int nargs, int nresults);
```

下面的例子中，这行 Lua 代码等价于在宿主程序中用 C 代码做一些工作：

```
a = f("how", t.x, 14)
```

这里是与上面Lua代码等价的C代码：

```
lua_getglobal(L, "f");                  /* function to be called */
lua_pushliteral(L, "how");                       /* 1st argument */
lua_getglobal(L, "t");                    /* table to be indexed */
lua_getfield(L, -1, "x");        /* push result of t.x (2nd arg) */
lua_remove(L, -2);                  /* remove 't' from the stack */
lua_pushinteger(L, 14);                          /* 3rd argument */
lua_call(L, 3, 1);     /* call 'f' with 3 arguments and 1 result */
lua_setglobal(L, "a");                         /* set global 'a' */
```