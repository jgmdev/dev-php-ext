<table style="width: 100%;">
    <tr>
        <td align="left" width="33%">
            
        </td>
        
        <td align="center" width="33%">
            <a href="README.md">Table of Contents</a>
        </td>
        
        <td align="right" width="33%">
            <a href="">Building PHP &gt;&gt;</a>
        </td>
    </tr>
</table>
------------------------------------------------------------------------

# Variables
------------------------------------------------------------------------

The PHP engine uses a single structure that represents any variable 
type supported by the language. The structure which you will be working 
most of the time is the zval.

##The zval

The zval is defined as a typedef to a struct on php-src/Zend/zend.h as 
follows:

    typedef struct _zval_struct zval;

    struct _zval_struct {
        zvalue_value value;			/* Current Value */
        zend_uint refcount__gc;		/* Count of variables that point to this one */
        zend_uchar type;			/* Variable Type */
        zend_uchar is_ref__gc;		/* Variable is reference of another one */
    };

###value property

The "value" property is an union also defined on the same file as the zval
and it can hold any of the types mentioned later, here is 
its definition:

	typedef union _zvalue_value {
		long lval;					/* integer value or resource id */
		double dval;				/* double/float value */
		struct {					/* String value and length */
			char *val;
			int len;
		} str;						
		HashTable *ht;				/* hash table to store array elements */
		zend_object_value obj;		/* Object properties and information */
	} zvalue_value;

### refcount__gc property
The refcount_gc property stores the amount of variables which depend
on the zval. This can be better explained with the following php
sample:

    <?php
        // Initializes my var with refcount__gc = 0
        $myvar = "something"; 

        // Initializes $othervar and increases $myvar refcount__gc to 1
        $othervar = &$myvar; //
        
        // $othervar goes out of scope and is garbage collected
        // also the refcount of $myvar is decreased
        
        // $myvar goes out of scope since its refcount is 0 it also
        // gets garbage collected
    ?>
    
As you may have noticed __gc stands for garbage collector. Each time a
variable goes out of scope PHP's garbage collector checks
the refcount of a variable and if equal to 0 the memory the
variable was using is automatically freed to the system. If the
refcount is greater than 0 then each time a variable that references
another variable is destroyed, the refcount of the referenced variable
is decreased until 0 is reached and the zval can be finally destroyed.

### is_ref__gc property

The is_ref property serves as an identifier of variables which point
to the values of other variables. In our previous example $othervar
is_ref would set to 1 indicating that it points to another variable.
Here a modified example:

    <?php
        $myvar = "something"; 

        // Initializes $othervar and sets is_ref to 1
        $othervar = &$myvar; //
        
        // Since $othervar is marked as is_ref php will seek the value
        // it points (in this case $myvar) and print it
        print $othervar
    ?>

### type property

The variable types a zval can store covers pretty much every need. 
Here is the list of types zval variable can store as defined on 
the Zend/zend.h header with a php example to represent them.

        IS_NULL 
        
            <?php $variable = null ?>
            
        IS_LONG
        
            <?php $variable = 123456 ?>
            
        IS_DOUBLE
        
            <?php $variable = 2.012 ?>
            
        IS_BOOL
        
            <?php $variable = false ?>
            
        IS_ARRAY
        
            <?php $variable = array("element1", "element2") ?>
            
        IS_OBJECT
        
            <?php $variable = new ObjectType() ?>
            
        IS_STRING
        
            <?php $variable = "text" ?>
            
        IS_RESOURCE
        
            <?php $variable = fopen("file.txt") ?>
            
        IS_CONSTANT
        
            <?php define("MY_CONSTANT", 1) ?> //Is this correct :S?
            
        IS_CONSTANT_ARRAY
        
            [TO BE DONE]
            
        IS_CALLABLE
        
            <?php $variable = array($this, "method_name"); ?>
            
            <?php $variable = 'function_name'; ?>
            
            <?php $variable = function($parameters){...}; ?>


## Variables Manipulation
	
You should never access zval properties directly since PHP internals
can change on the future, something that would cause your extension code
to stop working. Instead you should use a set of predefined macros 
especially written to deal with them in order to ensure maximun future 
compatibility in case the PHP internals change.

### Initializing zval pointers

Allocates data for the zval and sets the refcount to 1 and isref to 0
	
    MAKE_STD_ZVAL(zval**)
	
Allocates and initilizes the zval to IS_NULL
    ALLOC_INIT_ZVAL(zval**)
    
### References and destruction

Increase the refcount variable

    ZVAL_ADDREF(zval**)
    Z_ADDREF_P(zval*)
    
Decreases the refcount of a variable and if 0 destroy it.

    val_ptr_dtor(zval**);

    
[EXAMPLE CODE HERE]	

### Type

To check which type of data a zval holds you should use one of the
following macros:

    Macro                   Translation
    --------------          -----------
    Z_TYPE(zval)        =   (zval).type
    Z_TYPE_P(zval_p)    =   Z_TYPE(*zval_p)
    Z_TYPE_PP(zval_pp)  =   Z_TYPE(**zval_pp)
	
[EXAMPLE CODE HERE]
	

### Values
	
    Macros						Translation
    --------------				-----------
    Z_LVAL(zval)				(zval).value.lval
    Z_BVAL(zval)				((zend_bool)(zval).value.lval)
    Z_DVAL(zval)				(zval).value.dval
    Z_STRVAL(zval)				(zval).value.str.val
    Z_STRLEN(zval)				(zval).value.str.len
    Z_ARRVAL(zval)				(zval).value.ht
    Z_OBJVAL(zval)				(zval).value.obj
    Z_OBJ_HANDLE(zval)			Z_OBJVAL(zval).handle
    Z_OBJ_HT(zval)				Z_OBJVAL(zval).handlers
    Z_OBJCE(zval)				zend_get_class_entry(&(zval) TSRMLS_CC)
    Z_OBJPROP(zval)				Z_OBJ_HT((zval))->get_properties(&(zval) TSRMLS_CC)
    Z_OBJ_HANDLER(zval, hf) 	Z_OBJ_HT((zval))->hf
    Z_RESVAL(zval)				(zval).value.lval
    Z_OBJDEBUG(zval,is_tmp)		(Z_OBJ_HANDLER((zval),get_debug_info)?Z_OBJ_HANDLER((zval),get_debug_info)(&(zval),&is_tmp TSRMLS_CC):(is_tmp=0,Z_OBJ_HANDLER((zval),get_properties)?Z_OBJPROP(zval):NULL))

Equivalent helper macros for indirect zval's like zval* and zval**

    zval*							zval**
    ----------------				-----------------
    Z_LVAL_P(zval_p)				Z_LVAL_PP(zval_pp)
    Z_BVAL_P(zval_p)				Z_BVAL_PP(zval_pp)
    Z_DVAL_P(zval_p)				Z_DVAL_PP(zval_pp)
    Z_STRVAL_P(zval_p)				Z_STRVAL_PP(zval_pp)
    Z_STRLEN_P(zval_p)				Z_STRLEN_PP(zval_pp)
    Z_ARRVAL_P(zval_p)				Z_ARRVAL_PP(zval_pp)
    Z_OBJPROP_P(zval_p)				Z_OBJPROP_PP(zval_pp)
    Z_OBJCE_P(zval_p)				Z_OBJCE_PP(zval_pp)
    Z_RESVAL_P(zval_p)				Z_RESVAL_PP(zval_pp)
    Z_OBJVAL_P(zval_p)				Z_OBJVAL_PP(zval_pp)
    Z_OBJ_HANDLE_P(zval_p)			Z_OBJ_HANDLE_PP(zval_p)
    Z_OBJ_HT_P(zval_p)				Z_OBJ_HT_PP(zval_p)
    Z_OBJ_HANDLER_P(zval_p, h)		Z_OBJ_HANDLER_PP(zval_p, h
    Z_OBJDEBUG_P(zval_p,is_tmp)		Z_OBJDEBUG_PP(zval_pp,is_tmp)

[EXAMPLE CODE HERE]

------------------------------------------------------------------------
<table style="width: 100%;">
    <tr>
        <td align="left" width="33%">
            
        </td>
        
        <td align="center" width="33%">
            <a href="README.md">Table of Contents</a>
        </td>
        
        <td align="right" width="33%">
            <a href="">Building PHP &gt;&gt;</a>
        </td>
    </tr>
</table>
