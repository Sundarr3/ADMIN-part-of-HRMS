
CREATE OR REPLACE FUNCTION public.sf_get_role_privilege(
	p_employee_id integer,
	p_module_id integer,
	p_permission character varying,
	p_role_id integer)
    RETURNS boolean
    LANGUAGE 'plpgsql'
    COST 100
    VOLATILE PARALLEL UNSAFE
AS $BODY$
DECLARE 
    ln_permi_count integer;
    v_sql text;

BEGIN

    -- Construct the dynamic SQL query
    v_sql := 'SELECT count(*) FROM module_permi_mapping mpm
              JOIN employee_role_relation err ON mpm.role_id = err.role_id
              WHERE err.employee_id = ' || p_employee_id || '
              AND err.role_id = ' || p_role_id || '
              AND mpm.' || quote_ident(p_permission) || ' = 1
              AND module_id = ' || p_module_id;

    -- Execute the dynamic SQL and store the result in ln_permi_count
    EXECUTE v_sql INTO ln_permi_count;

    -- Log permission count for debugging purposes
    RAISE INFO 'Permission count: %', ln_permi_count;

    -- If the permission count is greater than or equal to 1, return true
    IF ln_permi_count >= 1 THEN 
        RETURN true;
    ELSE 
        RETURN false;
    END IF;

EXCEPTION
    WHEN OTHERS THEN
        -- Handle the exception by raising the error message
        RAISE INFO 'Error: %', SQLERRM;
        RETURN false;
END;
$BODY$;
