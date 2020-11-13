### Logical expression
Performs logical expression `expr` (in Python syntax) on project data structure. If expression is True,
indicator is OK. Otherwise ERR. 

Project data structure is available by link from Logical indicator page.

`dump` (optional) specifies comma-separated list of project data variables, e.g. `day, hhmm, age['ERR:errage']`.
These variables will be placed in indicator Details.

`init` (optional) lists variables which will be creates (and initialized to 0) before executing `expr`. In most cases, `init` can be empty, but using `init` makes code more robust. Example: we set tag 'x' for some indicators and use `expr` based on this tag, for example: `tags['OK:x']==1`. It will work. But if we will delete these indicators, variable tags['OK:x'] will not be created anymore and problem will happen. For this case we can make init: `tags['OK:x']` and now this variable always will exists when processing this indicator.

#### Problem flag ####
If expression fails (e.g. syntax error), problem flag is set on indicator. Indicator with problem 
flag not processed. User should fix error in expression and unckeck 'problem' checkbox.
