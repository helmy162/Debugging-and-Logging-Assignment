# Debugging Documentation

## Introduction

The objective was to debug a C++ program performing matrix multiplication.
An issue was identified where the program was not providing the correct matrix multiplication output.

## Steps Taken for Debugging

### 0. Initial Run
- Ran the program to observe the problem firsthand and identified that the issue was in the resultant multiplication output, as it provided an incorrect answer.

### 1. Preliminary Checks
- The Matrix was printed successfully with the right data, validating the `setData` and `display` methods.
- The problem was narrowed down to the `multiply` method.

### 2. Inspecting the Multiply Method
- The message "Matrix dimensions are incompatible for multiplication." (line 34) wasn't printed, so the code did not enter this condition.
- A breakpoint was set at line 38 and `rows` and `other.cols` were printed to verify their values, which were correct.
```console
g++ -g -O0 -o Matrix Matrix.cpp
gdb Matrix

break 38
print rows
print other.cols
```

### 3. Loop Logic Verification
- The 3 nested loops at line 40 were investigated, focusing on the assignment operation within them.
- A breakpoint was set at the beginning of the loops and `cols` was printed to verify its value.
```console
break 40
print cols
```
- PS: We don't need to check `rows` and `other.cols` again as they were previously checked and found to be correct.

### 4. Value Tracking
- Tracked the changes to `result.data` to observe how its value was being altered.
- Observed the first cell `result.data[0][0]` which was initially 0. After the first iteration, it became 7 (1\*7), which was expected.
- The second iteration posed an issue where the value was 18 (2\*9), but it should have been 25 (1\*7 + 2\*9).
```console
print result.data 
continue
print result.data
continue
print result.data
```
### 5. Issue Identification
- Identified that line 43 was performing an assignment operation instead of an addition to the previous value:
   ```cpp
   result.data[i][j] = data[i][k] * other.data[k][j];
  ```
### 6. Issue Fixing
- Corrected the line to be as follows:
   ```cpp
   result.data[i][j] += data[i][k] * other.data[k][j];
  ```
### 7. Re-testing the Application
- Re-ran the application with various test cases to ensure the corrected code now provides the expected and accurate matrix multiplication output.

## Conclusion
The matrix multiplication logic was corrected by ensuring that the products of the respective elements were accumulated instead of being overwritten. This was achieved by altering the assignment operation to accumulate the values.
