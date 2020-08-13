## Preface
This small bit of code allows you to set the identity of every existing thread and every thread that is created (using lua_newthread) from then on. Basically, it allows every script that is executed to have it's own environment.

4/09/2019:
Recently, Roblox completely changed they're getCurrentIdentity function, which was where most people used to grab identity flag to elevate the context on the current luastate thread. However, the code below can be used to detour that patch quite easily and requires no updating unless for some reason Roblox changed their offsets or method in general. Check "Example Usage" in order to achieve what I'm talking about.

01/26/2020:
I haven't been paying much attention to the exploiting scene, I have **NO** idea if this code still works.

3/29/2020:
This code is no longer viable. I'll update this as soon as I find a new method.

08/13/2020:
Completely broken. Another method exists, go look for it.
#### Code:
```c++
//rbx_L would be your luastate thread
//identity would be the identity you want to elevate to
void setThreadIdentity(INT rbx_L, unsigned __int8 identity)
{
	DWORD* loc1 = reinterpret_cast<DWORD *>(rbx_L - 32);
	*loc1 ^= (identity ^ static_cast<unsigned __int8>(*loc1)) & 0x1F; //OPCODE POP
}
```
#### Information
This method requires you to already have an existing thread in order to be used as intended. This method is also found in Roblox's setThreadIdentityAndSandbox function, which if you were wondering can be found inside the only xref for the string "Unable to create a new thread for %s".

#### Example Usage
```c++
//rbx_OL - your current luastate found by passing through the ScriptContext encryption.
setThreadIdentity(rbx_OL, 6);
/* Identity on rbx_OL is still 0, so we're going to create a new thread in order to create a luastate with an elevated context. */
int rbx_L = rbx_lua_newthread(rbx_OL);
/* rbx_L is now identity 6, this should be used as your global luastate, rbx_OL can be disposed of. */
```
