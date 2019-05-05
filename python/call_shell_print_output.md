## [Run shell script from python3 and get output](https://stackoverflow.com/questions/4760215/running-shell-command-and-capturing-the-output)

```
# terse
import subprocess
output = subprocess.getoutput("ls -l")
print(output)

# less terse
import subprocess

output = subprocess.run("ls -l", shell=True, stdout=subprocess.PIPE, 
                        universal_newlines=True)
print(output.stdout)

# trick
import os
os.system('sample_cmd > tmp')
print open('tmp', 'r').read()
```
