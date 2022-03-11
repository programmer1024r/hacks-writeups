# C++
# GDB
`gdb myfs --args myfs address-space` -> run the program with params<br>
`layout src` -> shows the src file code<br>
`n` -> next<br>
`step` -> step inside the function<br>
`print var` -> print the var value<br>
`c` -> run till breakpoint<br>
`b <line number or text in the line>`<br>

```MakeFile
g++ -c myfs_main.cpp -o ${BIN_DIR}/myfs_main -ggdb -Wall
g++ -c myfs.cpp -o ${BIN_DIR}/myfs_cpp -ggdb -Wall
g++ -c blkdev.cpp -o ${BIN_DIR}/blkdev -ggdb -Wall
# link them together
g++ ${BIN_DIR}/blkdev ${BIN_DIR}/myfs_cpp ${BIN_DIR}/myfs_main -o ${BIN_DIR}/myfs
```
