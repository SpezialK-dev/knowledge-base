

# Applying patches 

Some problems can occur when the patches are encoded weirdly


Sources :
- https://stackoverflow.com/questions/4770177/git-apply-fails-with-patch-does-not-apply-error

```shell
git apply <patchfiles.patch
```


though this is not very robust

```shell
git apply  --whitespace=fix --reject <patch.patch>
```

because this does everything atomically and fixes the whitespaces errors in patchfiles which is really helpful especially if you are working across systems.
With the above option reject files are created that can be merged using [wiggle](https://linux.die.net/man/1/wiggle).




might also be helpful because it just applies but just leaves the markers where it was unsure
```shell
git apply --3way <patch.patch>
```