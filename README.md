# Android-Lua
Lua decrypt<br>
# Lua types
luac是lua编译后的字节码，文件头为0x1B 0x4C 0x75 0x61 0x51<br>
luaJIT是另一个lua的实现版本（不是原作者写的），JIT是指Just-In-Time（即时解析运行），文件头是0x1B 0x4C 0x4A<br>
# Lua decrypt
```javascript
Interceptor.attach (Module.findExportByName( "libgame.so", "xxtea_decrypt"), {//so 可变，主要是找到xxtea  cocos2d:libcocos2dcpp.so
    onEnter: function (args) {
        console.log("----------------BEGIN----------------");
        console.log(hexdump(Memory.readByteArray(args[2], 12),{
         offset: 0,
         length: 12,
         header: true,
         ansi: true
        }));
    },
    onLeave: function (retval) {
        console.log("Decrypt:");
        console.log(hexdump(Memory.readByteArray(retval, 16),{
         offset: 0,
         length: 16,
         header: true,
         ansi: true
        }));
        console.log("-----------------END-----------------");   
    }
});
```
# 重要函数
luaL_loadbuffer,luaL_loadbufferex<br>
luaL_loadfilex这个函数用来加载我们的Lua脚本，lua_pcall这个函数用来执行Lua脚本<br>
luaL_loadbuffer以及luaL_loadbufferex会调用这个函数
int lua_loadx(lua_State *L, lua_Reader reader, void *data, const char *chunkname, const char *mode);<br>
# lua decomplier tool
 luadec https://github.com/viruscamp/luadec<br>
 ljd https://github.com/NightNord/ljd<br>
 luajit-decomp https://github.com/bobsayshilol/luajit-decomp<br>
 # fix opmode
 原opmode在lua源码中的lopcodes.c文件中<br>
 定位到opmode的地方，IDA搜索字符串”LOADK”，定位到opname的地方，交叉引用到代码，找到opmode
