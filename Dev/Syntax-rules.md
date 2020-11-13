# Syntax rules

## Project.textid
### generation:
~~~~
textid = ''.join(random.choice(string.ascii_lowercase + string.digits) for _ in range(l))
~~~~
10 symbols, lowercase ascii + digits.

### Change:
~~~~
'^[a-z0-9\-\.]+$'
~~~~
lowercase latin, digits, '-' and  '.'

### Requirements:
We cannot use ':' as textid, because in future we may want to use uniq id in form textid:name
Another reason, director uses format `p:partner_id`

Can not use '@' because director should distinguish between email address (login) and textid.

Can not use '_' in public projects, reserved for internal projects.

## Indicator.name

### Validation
indicator.validname(name)

### Requirements
- Must not be number (not be convertable using int())
- Must be unique in project
- Must no contain forbidden chars: '<', '>', '%' , '\\' , '@'
- forward slash '/' is enabled and used in 'df' indicators (as part of path)
- duplicate forward slash '//' and forward slash at start of indicator is forbidden.

'@' used to specify textid: my:indicator:name@textid
