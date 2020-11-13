### String
Sets ERR if string from client different from **str** option.

Options:
- **reinit** - if **str** is empty and reinit option exists, **str** will be initialized to string from client.
- **empty_err** - if client string is empty, ERR is set
- **emtpy_ok** - if client string is empty, OK is set (even if it's different from **str**)
- **dynamic** - if client string different from **str**, alert is sent, but indicator will be OK, and **str** will be 
updated to client string.
