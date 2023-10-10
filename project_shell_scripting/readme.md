My shell script project has beem executed.

### ERRORS ENCOUNTERED 

1. encountered an error when working on the numbers and calculation script the code:

square_root=$(awk "BEGIN{ sqrt=$num2; print sqrt }")

didnt run it kept returning a syntax error

### SOLUTION 

I edited the code to:

square_root=$(awk "BEGIN{ print sqrt($num2); }")

script ran and executed.


2. I also had an issue with the backup script just copying pasting and running returned an error.

### SOLUTION 

I edited in the correct paths for the source folder and backup folder and also created a folder and file for the source, after this the script ran successfullt and returned the desired output.