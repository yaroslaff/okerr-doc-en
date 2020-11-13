
### Numerical
Accepts numerical value from client and performs these checks (each check is performed only if parameter is set):
- **minlim** - if client value is less then **minlim**, ERR is set.
- **maxlim** - if client value is higher then **maxlim**, ERR is set.
- **diffmin** - if difference (client value - current) less then **diffmin**, ERR is set. 
(Example: current=1, diffmin=5, client value=4. ERR will be set because 3<5).
- **diffmax** - same is diffmin but limits maximal difference. 
(Example: current=4, diffmax=5, client value=10. ERR will be set because 6>5)

**current** value is last value received from client.
