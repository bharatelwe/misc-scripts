# Reading CSV from STDIN and writing CSV to STDOUT in Python

The below example shows how a python code can be used in linux command line. The below code snippet reads CSV data from `STDIN` and writes the first 5 lines to `STDOUT`.
It also outputs the line count and the number of fields in the CSV. If the CSV has variable number of fields then the output of the `dict` will highlight this information.

```python
import sys
from csv import reader
from csv import writer

cnt = 0
mydict = dict()
fctr = 5

filew = writer(sys.stdout)

for row in reader(iter(sys.stdin.readline, '')):
    num = len(row)
    if num in mydict.keys():
        mydict[num] += 1
    else:
        mydict[num] = 1
    cnt += 1
    if (fctr != 0):
        filew.writerow(row)
        fctr -= 1
print("Number of lines: ", cnt)
print(mydict)
```
this code can now easily be called on the bash command line.
Below is an example of reading a CSV file that is present in S3 bucket and viewing the first 5 lines while getting the line count of the file and the number of fields in the CSV.

```bash
aws s3api get-object --bucket ${mybucket} --key ${filename_with_subfolder} /dev/stdout | python readWriteCSV.py
```

