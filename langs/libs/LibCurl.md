#c

# Making the usage very simple


simply run the command you want to do with curl, if that works simply append a `--libcurl`

more dokumentation can be found here
 [--libcurl](https://everything.curl.dev/libcurl/--libcurl.html?highlight=--libcur#--libcurl)


# CURLOPT_POSTFIELDSIZE_LARGE


needs to be ==Exactly!!!== the length of the field you are sending (when sending json requests since otherwiese your server might not recognize it / reject it ) and you will be left wondering why correct json is being rejected.
```c
    curl_easy_setopt(hnd, CURLOPT_POSTFIELDSIZE_LARGE, (curl_off_t)`<length>`);

```


# Disabling printing 

as per this [Stackoverflow post](https://stackoverflow.com/questions/3572397/lib-curl-in-c-disable-printing)

```c
curl_easy_setopt(curl, CURLOPT_WRITEFUNCTION, write_data);
```
you can set the `CURLOPT_WRITEFUNCTION` option to something else to disable printing to cmd line. 