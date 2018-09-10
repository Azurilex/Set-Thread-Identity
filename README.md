## Preface
This small bit of code allows you to set the identity of every existing thread and every thread that is created (using lua_newthread) from then on. Basically, it allows every script that is executed to have it's own environment.
#### Code:
```c++
//rbx_L would be your luastate thread
//identity would be the identity you want to elevate to
void setThreadIdentity(INT rbx_L, unsigned __int8 identity)
{
    DWORD* loc1 = (DWORD *)(rbx_L - 32);
    *loc1 ^= (identity ^ (unsigned __int8)*loc1) & 0x1F;
}
```
#### Information
This method requires you to already have an existing thread in ordet to be used as intended. This method is also found in Roblox's setThreadIdentityAndSandbox function, which if you were wondering can be found inside the only xref for the string "Unable to create a new thread for %s".

#### Example Usage
```c++
//rbx_L would be your luastate thread
setThreadIdentity(rbx_L, 6)
```
