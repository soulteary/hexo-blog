---
title: "[php]对抗zend加密"
date: "2007-09-09 20:00:18"
tags: ["PHP","zend","解密"]
---


![](http://attachment.soulteary.com/wp/2007/09/php.gif "php")

**方法一：**

首先下载 vld-0.8.0 下载地址　http://www.huanle.net/down/vld.php 执行代码﻿

```bash
//解压   
#tar -xzf vld-0.8.0.tgz   
//重命名   
#mv vld-0.8.0 vld   
//拷贝vld目录到php的解压目录下的ext中   
#cd -R vld ../php-4.3.8/ext   
#cd php-4.3.8   
//删除configure,因为下面的buildconf会重新生成新的configure   
#rm configure   
//如果出现错误,就按提示加上相应的参数.   
#./buildconf   
#./configure --with-mysql --with-apxs2=/usr/www/bin/apxs --enable-vld 重新检查php   
#make 编译   
#make install 安装  
```

服务器必须先安装ZendOptimizer 运行Zend后的文件,查看源代码就可以看到了. 

方法二： 编译此文件即可。

```cpp
#ifdef HAVE_CONFIG_H   
#include "config.h"   
#endif   

#include "php.h"   
#include "php_ini.h"   
#include "ext/standard/info.h"   
#include "php_vld.h"   
#include "srm_oparray.h"   

static zend_op_array* (*old_compile_file)(zend_file_handle* file_handle, int type TSRMLS_DC);   
static zend_op_array* vld_compile_file(zend_file_handle*, int TSRMLS_DC);   
static void (*old_execute)(zend_op_array *op_array TSRMLS_DC);   
static void vld_execute(zend_op_array *op_array TSRMLS_DC);   

function_entry vld_functions[] = {   
{NULL, NULL, NULL}   
};   

zend_module_entry vld_module_entry = {   
#if ZEND_MODULE_API_NO >= 20010901   
STANDARD_MODULE_HEADER,   
#endif   
"vld",   
vld_functions,   
PHP_MINIT(vld),   
PHP_MSHUTDOWN(vld),   
PHP_RINIT(vld),   
PHP_RSHUTDOWN(vld),   
PHP_MINFO(vld),   
#if ZEND_MODULE_API_NO >= 20010901   
"0.8.0",   
#endif   
STANDARD_MODULE_PROPERTIES   
};   

#ifdef COMPILE_DL_VLD   
ZEND_GET_MODULE(vld)   
#endif   

ZEND_BEGIN_MODULE_GLOBALS(vld)   
int active;   
int skip_prepend;   
int skip_append;   
int execute;   
ZEND_END_MODULE_GLOBALS(vld)   

ZEND_DECLARE_MODULE_GLOBALS(vld)   

#ifdef ZTS   
#define VLD_G(v) TSRMG(vld_globals_id, zend_vld_globals *, v)   
#else   
#define VLD_G(v) (vld_globals.v)   
#endif   

PHP_INI_BEGIN()   
STD_PHP_INI_ENTRY("vld.active", "0", PHP_INI_SYSTEM, OnUpdateBool, active, zend_vld_globals, vld_globals)   
STD_PHP_INI_ENTRY("vld.skip_prepend", "0", PHP_INI_SYSTEM, OnUpdateBool, skip_prepend, zend_vld_globals, vld_globals)   
STD_PHP_INI_ENTRY("vld.skip_append", "0", PHP_INI_SYSTEM, OnUpdateBool, skip_append, zend_vld_globals, vld_globals)   
STD_PHP_INI_ENTRY("vld.execute", "1", PHP_INI_SYSTEM, OnUpdateBool, execute, zend_vld_globals, vld_globals)   
PHP_INI_END()   

static void vld_init_globals(zend_vld_globals *vld_globals)   
{   
vld_globals->active = 0;   
vld_globals->skip_prepend = 0;   
vld_globals->skip_append = 0;   
vld_globals->execute = 1;   
}   

PHP_MINIT_FUNCTION(vld)   
{   
ZEND_INIT_MODULE_GLOBALS(vld, vld_init_globals, NULL);   
REGISTER_INI_ENTRIES();   
old_compile_file = zend_compile_file;   
old_execute = zend_execute;   

return SUCCESS;   
}   

PHP_MSHUTDOWN_FUNCTION(vld)   
{   
UNREGISTER_INI_ENTRIES();   

zend_compile_file = old_compile_file;   
zend_execute = old_execute;   

return SUCCESS;   
}   

PHP_RINIT_FUNCTION(vld)   
{   
if (VLD_G(active)) {   
zend_compile_file = vld_compile_file;   
if (!VLD_G(execute)) {   
zend_execute = vld_execute;   
}   
}   
return SUCCESS;   
}   

PHP_RSHUTDOWN_FUNCTION(vld)   
{   
zend_compile_file = old_compile_file;   
zend_execute = old_execute;   

return SUCCESS;   
}   

PHP_MINFO_FUNCTION(vld)   
{   
php_info_print_table_start();   
php_info_print_table_header(2, "vld support", "enabled");   
php_info_print_table_end();   
}   

static int vld_check_fe (zend_op_array *fe, zend_bool *have_fe TSRMLS_DC)   
{   
if (fe->type == ZEND_USER_FUNCTION) {   
*have_fe = 1;   
}   

return 0;   
}   

static int vld_dump_fe (zend_op_array *fe TSRMLS_DC)   
{   
if (fe->type == ZEND_USER_FUNCTION) {   
fprintf(stderr, "Function %s:   
", fe->function_name);   
vld_dump_oparray(fe);   
fprintf(stderr, "End of function %s.   

", fe->function_name);   
}   

return ZEND_HASH_APPLY_KEEP;   
}   

#ifdef ZEND_ENGINE_2   
static int vld_dump_cle (zend_class_entry **class_entry TSRMLS_DC)   
#else   
static int vld_dump_cle (zend_class_entry *class_entry TSRMLS_DC)   
#endif   
{   
zend_class_entry *ce;   
zend_bool have_fe = 0;   

#ifdef ZEND_ENGINE_2   
ce = *class_entry;   
#else   
ce = class_entry;   
#endif   

if (ce->type != ZEND_INTERNAL_CLASS) {   
zend_hash_apply_with_argument(&ce->function_table, (apply_func_arg_t) vld_check_fe, (void *)&have_fe TSRMLS_CC);   
if (have_fe) {   
fprintf(stderr, "Class %s:   
", ce->name);   
zend_hash_apply(&ce->function_table, (apply_func_t) vld_dump_fe TSRMLS_CC);   
fprintf(stderr, "End of class %s.   

", ce->name);   
} else {   
fprintf(stderr, "Class %s: [no user functions]   
", ce->name);   
}   
}   

return ZEND_HASH_APPLY_KEEP;   
}   

/* {{{ zend_op_array vld_compile_file (file_handle, type)  
* This function provides a hook for compilation */  
static zend_op_array *vld_compile_file(zend_file_handle *file_handle, int type TSRMLS_DC)   
{   
zend_op_array *op_array;   

if (!VLD_G(execute) &&   
((VLD_G(skip_prepend) && PG(auto_prepend_file) && PG(auto_prepend_file)[0] && PG(auto_prepend_file) == file_handle->filename) ||   
(VLD_G(skip_append) && PG(auto_append_file) && PG(auto_append_file)[0] && PG(auto_append_file) == file_handle->filename)))   
{   
zval nop;   
ZVAL_STRINGL(&nop, "RETURN ;", 8, 0);   
return compile_string(&nop, "NOP" TSRMLS_CC);;   
}   

op_array = old_compile_file (file_handle, type TSRMLS_CC);   

if (op_array) {   
vld_dump_oparray (op_array);   
}   

zend_hash_apply (CG(function_table), (apply_func_t) vld_dump_fe TSRMLS_CC);   
zend_hash_apply (CG(class_table), (apply_func_t) vld_dump_cle TSRMLS_CC);   

return op_array;   
}   
/* }}} */  

/* {{{ void vld_execute(zend_op_array *op_array TSRMLS_DC)  
* This function provides a hook for execution */  
static void vld_execute(zend_op_array *op_array TSRMLS_DC)   
{   
// nothing to do   
}   
/* }}} */
```

