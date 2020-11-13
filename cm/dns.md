
### DNS Resolving
Perform DNS query for `host` of type `type`. 

If query result will match `value`, status will be OK.

**Options**:
If "dynamic" is not found in `options` and query result not matching `value`, ERR status will be set. 
If "dynamic", status always will remain "OK", but alert will be sent and `value` will be updated. 

If word "init" is found in `options` and current `value` is empty, query result will be saved to `value`.
