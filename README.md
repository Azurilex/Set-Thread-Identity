# Set-Thread-Identity

```c++
void setThreadIdentity(INT rbx_L, unsigned __int8 identity)
{
	DWORD* loc1 = (DWORD *)(rbx_L - 32);
	*loc1 ^= (identity ^ (unsigned __int8)*loc1) & 0x1F;
}
```
