Retrieves the status of the specified virtual key. The status specifies whether the key is up, down, or toggled.

```c
SHORT GetKeyState(
  [in] int nVirtKey
);
```

## Return value

The return value specifies the status of the specified virtual key, as follows:

- If the high-order bit is 1, the key is down; otherwise, it is up.
- If the low-order bit is 1, the key is toggled.
- A key, such as the CAPS LOCK key, is toggled if it is turned on. The key is off and untoggled if the low-order bit is 0.

### Toggling of a key

Toggle property only applies to some selected keys such as `CAPS_LOCK, NUMS_LOCK etc`. These keys generally have an indicator on the keyboard telling about their status.