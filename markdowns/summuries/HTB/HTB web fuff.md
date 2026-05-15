- `-ic` to skip comments
- **Directory Fuzzing**
```bash
# ffuf -w <SNIP> -u http://SERVER_IP:PORT/FUZZ
ffuf -w /opt/useful/seclists/Discovery/Web-Content/directory-list-2.3-small.txt:FUZZ -u http://SERVER_IP:PORT/FUZZ
```

<!--stackedit_data:
eyJoaXN0b3J5IjpbMTc3NzEzMzExNCwtMTY2MzUyNzc2NV19
-->