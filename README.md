### Creando una función para estandarizar datos en Microsoft SQL Server:
###### Tabla Z estándar

```sql
USE DNE;
GO


IF OBJECT_ID('FN_DNE') IS NOT NULL
BEGIN
     DROP FUNCTION DBO.FN_DNE
END;
GO

```


```sql
CREATE FUNCTION FN_DNE(@Z FLOAT,@Y FLOAT)
RETURNS NVARCHAR(174)
AS 
BEGIN 
    DECLARE @R AS FLOAT, @S AS NVARCHAR(174)
           if      @Y = '0'
		   BEGIN
                  SET @R = (SELECT L0   FROM DNE WHERE Z = @Z) 
		   END
           else if @Y = '0.01'
		   BEGIN
                  SET @R = (SELECT L01  FROM DNE WHERE Z = @Z) 
		   END
           else if @Y = '0.02'
		   BEGIN
                  SET @R = (SELECT L02  FROM DNE WHERE Z = @Z) 
		   END
           else if @Y = '0.03'
		   BEGIN
                  SET @R = (SELECT L03  FROM DNE WHERE Z = @Z) 
		   END
           else if @Y = '0.04'
		   BEGIN
                  SET @R = (SELECT L04  FROM DNE WHERE Z = @Z) 
           END
           else if @Y = '0.05'
		   BEGIN
                  SET @R = (SELECT L05  FROM DNE WHERE Z = @Z) 
		   END
           else if @Y = '0.06'
		   BEGIN
                  SET @R = (SELECT L06  FROM DNE WHERE Z = @Z) 
		   END
           else if @Y = '0.07'
		   BEGIN
                  SET @R = (SELECT L07  FROM DNE WHERE Z = @Z) 
           END
		   else if @Y = '0.08'
		   BEGIN
                  SET @R = (SELECT L08  FROM DNE WHERE Z = @Z)
		   END 
           else if @Y = '0.09'
		   BEGIN
                  SET @R = (SELECT L09  FROM DNE WHERE Z = @Z)	
		   END
SET @S = ('Z Estándar
--------------'+'
'+CAST(@R AS VARCHAR(6))+'

0 a Z Estándar'+'
--------------'+'
'+CAST((@R*100) AS VARCHAR(6))+'%'+'

Valor Simétrico'+'
--------------'+'
'+CAST(2*@R AS VARCHAR(6))+'

Intervalo de Confianza del:'+'
--------------'+'
'+CAST((2*@R)*100 AS VARCHAR(6))+'%')
		   if (@Z  NOT BETWEEN 0.0 AND 3.0) OR (@Y  NOT BETWEEN 0.00 AND 0.09) 
		   BEGIN
		   		    SET @S = '
				             Valor Fuera de Rango.'
		   END
RETURN @S
END;
GO
```

```sql

DECLARE @i AS FLOAT, @j AS FLOAT 
SET @i = 1.0
SET @j = 0.09
PRINT DBO.FN_DNE(@i,@j);
GO
```
