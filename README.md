
# Role Privilege Function

## Overview
The **Role Privilege Function** is a PostgreSQL function designed to determine whether a specified employee has a certain permission for a given module based on their role. This function utilizes PL/pgSQL and dynamic SQL to efficiently check access privileges in a relational database context.

## Function Signature
```sql
CREATE OR REPLACE FUNCTION public.sf_get_role_privilege(
    p_employee_id integer,
    p_module_id integer,
    p_permission character varying,
    p_role_id integer
) RETURNS boolean
Parameters
p_employee_id: The ID of the employee whose permissions are being checked.
p_module_id: The ID of the module for which permission is being requested.
p_permission: The specific permission to check (e.g., 'is_view', 'is_edit').
p_role_id: The ID of the role assigned to the employee.
Return Value
boolean: Returns true if the employee has the specified permission for the module; otherwise, returns false.
Function Logic
Dynamic SQL Construction: Constructs a SQL query to count the number of matching permissions in the module_permi_mapping table.
Execution: Executes the dynamic SQL and captures the permission count.
Permission Check: Returns true if the count is greater than or equal to 1; otherwise, returns false.
Error Handling: Catches exceptions and logs an error message.
Usage
This function can be called in any part of your application that requires permission checks based on user roles and modules.

Example Call

SELECT public.sf_get_role_privilege(1, 5, 'is_view', 2);
Installation
To use this function, execute the provided SQL script in your PostgreSQL database environment.






