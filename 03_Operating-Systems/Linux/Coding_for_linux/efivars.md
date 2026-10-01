

https://github.com/rhboot/efivar

since the documentation for this library are not as well done as they should be I am writing my own docs.
# Finding out from your library functions are loaded. 

since all of the functions stored in a struct (can be seen below), can be found in `/src/lib.h` 

```c

struct efi_var_operations {
	char name[NAME_MAX];
	int (*probe)(void);
	int (*set_variable)(efi_guid_t guid, const char *name, const uint8_t *data,
			    size_t data_size, uint32_t attributes, mode_t mode);
	int (*del_variable)(efi_guid_t guid, const char *name);
	int (*get_variable)(efi_guid_t guid, const char *name, uint8_t **data,
			    size_t *data_size, uint32_t *attributes);
	int (*get_variable_attributes)(efi_guid_t guid, const char *name,
				       uint32_t *attributes);
	int (*get_variable_size)(efi_guid_t guid, const char *name,
				 size_t *size);
	int (*get_next_variable_name)(efi_guid_t **guid, char **name);
	int (*append_variable)(efi_guid_t guid, const char *name,
			       const uint8_t *data, size_t data_size,
			       uint32_t attributes);
	int (*chmod_variable)(efi_guid_t guid, const char *name, mode_t mode);
};

```

the loading code is interesting, it really depends on what ouput of `getenv("LIBEFIVAR_OPS");` this code can be found in the end of lib.c 


in the end this is just a wrapper around `/sys/firmware/efi` with some parsing functionality. 


# badly documented calls 

simply iterate through all the next efi vars 
```c
char* name = NULL;
efi_guid_t* guid = NULL;
int status_next_efi_var = 0;

do{
	status_next_efi_var = efi_get_next_variable_name(&guid,&name);
}while(status_next_efi_var<0)
```