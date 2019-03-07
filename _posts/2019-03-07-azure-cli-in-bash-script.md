---
title: "Azure CLI in the Bash script"
date: 2019-03-07
#categories: [azure cli, devops]
#tags: [azure cli, docker, devops, python]
excertp: "Missing Azure CLI command in the bash script."
---

Today I've been trying to write a simple bash script on my Windows machine to provision a new Blob Storage in the Microsoft Azure cloud. The beginning of the script looked quite simple:

```bash
#!/bin/sh

echo "*** STEP 1: Checking Azure CLI version... ***"
az --version
```

Interestingly, after running this short script I got an error:

```az command not found```

I could still write the script using the `az.cmd` command (instead of `az`) - but it wouldn't be very elegant, nor would it be independent from OS (Windows, Mac). Without going into details, it turned out that the problem can be fixed by creating a symbolic link:

```bash
mklink "%SYSTEMROOT%\az" "C:\Program Files (x86)\Microsoft SDKs\Azure\CLI2\wbin\az.cmd"
```

More information can be found on the [Stack overflow](https://stackoverflow.com/questions/42972086/azure-cli-in-git-bash).