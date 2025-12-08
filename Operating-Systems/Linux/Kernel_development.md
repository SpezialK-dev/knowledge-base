# TO many arguemnts for class_create


In my case this happend when compiling fmem 1.6.0
```error
 error: too many arguments to function ‘class_create’
  381 |         mem_class = class_create(THIS_MODULE, "fmem");

```

the specific call is listed in the error that caused the problem. 
[A explenation can be found here](https://kb.meinbergglobal.com/kb/driver_software/driver_software_for_linux/troubleshooting_build_problems/build_error_related_to_calling_class_create_function) it boils down to a change in kernel 6.4. 

in my case the following c code fixed it 

```c
	#if(LINUX_VERSION_CODE >=KERNEL_VERSION(6, 4, 0))
	mem_class = class_create("fmem");
	#else
	mem_class = class_create(THIS_MODULE, "fmem");
	#endif
```


# init_module() depricated 


```shell
objtool: init_module(): Magic init_module() function name is deprecated, use module_init(fn) instead
```

here is an example on how to replace this
```diff
diff --git a/drivers/dahdi/dahdi_dummy.c b/drivers/dahdi/dahdi_dummy.c
index b1004d99..9bd86dfb 100644
--- a/drivers/dahdi/dahdi_dummy.c
+++ b/drivers/dahdi/dahdi_dummy.c
@@ -209,7 +209,7 @@ static int dahdi_dummy_initialize(struct dahdi_dummy *ztd)
 	return res;
 }
 
-int init_module(void)
+static int __init dummy_init(void)
 {
 	int res;
 	ztd = kzalloc(sizeof(*ztd), GFP_KERNEL);
@@ -250,8 +250,7 @@ int init_module(void)
 	return 0;
 }
 
-
-void cleanup_module(void)
+static void __exit dummy_exit(void)
 {
 #if defined(USE_HIGHRESTIMER)
 	/* Stop high resolution timer */
@@ -272,3 +271,6 @@ module_param(debug, int, 0600);
 MODULE_DESCRIPTION("Timing-Only Driver");
 MODULE_AUTHOR("Robert Pleh <robert.pleh@hermes.si>");
 MODULE_LICENSE("GPL v2");
+
+module_init(dummy_init);
+module_exit(dummy_exit);
```

taken from https://github.com/asterisk/dahdi-linux/pull/99/commits/aff90d06503b495347b5a7e53d936cd370a4220e

so basicly you just take your previous functions rename them to some dummy names and pass them to the new functions. 

4fab2d7628dd38f3fa8a5e615199350ecaeb17a8 -> appears to be the kernel commit that this was changed in. 