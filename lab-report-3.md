# Lab Report 3
Phillip Schiffman
***
# Part 1 - Bugs
## Buggy Code
The Buggy Code I will be analyzing is: 
```
// Changes the input array to be in reversed order
static void reverseInPlace(int[] arr) {
  for (int i = 0; i < arr.length; i++) {
    arr[i] = arr[arr.length - i - 1];
  }
}
```
- The method above, ```reverseInPlace```, intends to reverse the input ```int[]``` array in place
- This method uses a for-loop to iterate through the length of the ```arr``` array, and attempts to replace each index with its mirrored index
- Overall, this causes a bug in the desired output represented in the following lines:
  - Each index in the first half of the array is set to its mirrored index in the second half of the array and is consequently overwritten
  - Each index in the second half of the array is set to its mirrored index in the first half of the array. As the values in the first half of the array are now overwritten, the second half of the array remains unchanged
  - Thus, the code results in the second half of the array being mirrored onto the first half
## Test Cases for Buggy Code
*Failure Inducing Input*
```
@Test
  public void testFailureInducing() {
    int[] input = {1, 2, 3, 4, 5};
    ArrayExamples.reverseInPlace(input);
    assertArrayEquals(new int[]{5, 4, 3, 2, 1}, input);
  }
```
- This code shows the failure inducing input for a JUnit test for the ```reverseInPlace()``` method
- In this method, an ```int[]``` is created called ```input``` which has values of ```{1,2,3,4,5}```
- Next, the ```reverseInPlace()``` method is called with ```input``` being the parameter used
- Given the buggy code within ```reverseInPlace()```, the subsequent ```assertArrayEquals``` should show a failure
- The actual expected code should be ```{5,4,3,4,5}``` rather than the desired ```{5,4,3,2,1}```
- This all happens because of the mirror bug within the code. This function copies one by one, which in turn will simply copy something that has already been changed, and will result in the bug described
- So, the desired output will not be outputted, and this test will result in a failure

*Non-Failure Inducing Input*
```
@Test
  public void testNonFailureInducing() {
    int[] input = {5,4,3,4,5};
    ArrayExamples.reverseInPlace(input);
    assertArrayEquals(new int[]{5,4,3,4,5}, input);
  }
```
- This code shows the non-failure inducing input for a JUnit test for the ```reverseInPlace()``` method
- In this method, an ```int[]``` is created called ```input``` which has values of ```{5,4,3,4,5}```
- Next, the ```reverseInPlace()``` method is called with ```input``` being the parameter used
- Even though there is buggy code, within ```reverseInPlace()```, the subsequent ```assertArrayEquals``` will not produce a failure because of the ```input``` we gave
- When this is run, within the ```reverseInPlace()``` method the code will mirror the items from the second half of the array to the first half, but since the given ```input``` is already a mirror of itself, nothing will change
- Therefore, the code will result in no failures detected, because although the values are changing, the numbers are still the same, which will read as a test passed

## System Output

![image](Symptom.png)

- This shows the JUnit output for the tests described above
- As described earlier, the non-failure-inducing test passes because the array, ```input``` is already being mirrored within itself. For instance, the values that make up the 2nd half of the array are already in the same order but on both the first and second halfs of the array. This makes it so that when the function tries to copy an item from the second half, nothing changes

## Fix for Buggy Code

```
// Changes the input array to be in reversed order
static void reverseInPlace(int[] arr) {
  for (int i = 0; i < arr.length; i++) {
    int temp = arr[i];
    arr[i] = arr[arr.length - i - 1];
    arr[arr.length - i - 1] = temp;
  }
}
```

- The method above is a revamped version of the old ```reverseInPlace()``` method
- In this new version, within the for loop, we create a new variable of type ```int``` called ```temp```
- With this new variable, we create a temporary value for the item that needs to be replaced, and we can therefore swap the items instead of directly placing one with the lines below
- For instance, when the code is run, we store the value of the item being iterated in ```temp```, then we switch the current ```arr``` value for a value of the item on the other side of the array, and then we replace the last value with the ```temp``` value
- All of this is being done in the lines ```arr[i] = arr[arr.length - i - 1];``` and ```arr[arr.length - i - 1] = temp;```
- To replace the last value with the ```temp``` value, we simply call the item in ```arr``` by using ```length - i - 1``` which will allow us to get the opposite side of the array from where we are iterating
- Overall, this fixed the bug in the code because now instead of simplying copying the items one by one, it swaps the items so that there becomes no mirroring within the array

# Part 2 - Researching Commands - find
## 1.) find /path/to/search -name "filename"

- This command allows the user to search for specific files within the directory or path that is given
- This command can be especially useful when looking for a specific file, with lots of files inside one directory
```
phill@PhillipLaptop MINGW64 ~/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical (main)
$ find -name "biomed"
./biomed
```
- Above is this command used in the ```./technical``` directory
- Here, we search for the file ```biomed``` and it returns the correct file
- In this scenario, it isn't the most useful because we can clearly see ```biomed``` within the directories on the left side on VS Code, but with more files, this would be extremely helpful in locating what we need to

```
phill@PhillipLaptop MINGW64 ~/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical (main)
$ find -name "plos"
./plos
```
- Above is another example of using this command in the ```./technical``` directory
- Here, we search for the file ```plos``` and it returns the correct file
- Once again, in this scenario it isn't the most useful because we can clearly see ```plos``` within the directories on the left side on VS Code, but with more files, this would be extremely helpful in locating what we need to

## 2.) find /path/to/search -type ?

- This command allows the user to search for for files within a specified directory
- Rather than using a name to locate a certain file, it can look for certain types of files with a given input
- For instance, we can use ```d``` to find directories, ```l``` to find links, and so on

```
phill@PhillipLaptop MINGW64 ~/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical (main)
$ find ~/OneDrive/Desktop/'VS Code Work'/'CSE 15L'/'Week 5 lab'/docsearch/technical -type d       
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/911report     
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/biomed        
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government    
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/About_LSC
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Alcohol_Problems
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Env_Prot_Agen
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Gen_Account_Office
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Media
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Post_Rate_Comm
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/plos
```
- The above piece of code shows the ```find /path/to/search -type``` command being run with ```d``` as the parameter to look for
- With the ```d``` at the end, it tells the command to return all of the directories in the given path
- So, in our case, I inputted ```~/OneDrive/Desktop/'VS Code Work'/'CSE 15L'/'Week 5 lab'/docsearch/technical``` as the path, and then ran the command to find all of the directories that are within the ```./techincal``` directory
- This command can be very useful when trying to count how many different types you have a certain file or directory, and just to see how many directories you have
- This would also be extremely helpful when there is mass amounts of files and/or directories within the workspace

```
phill@PhillipLaptop MINGW64 ~/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical (main)
$ find ~/OneDrive/Desktop/'VS Code Work'/'CSE 15L'/'Week 5 lab'/docsearch/technical -type l

```
- The above piece of code shows the ```find /path/to/search -type``` command being run with ```l``` as the parameter to look for
- With the ```l``` at the end, it tells the command to return all of the links in the given path
- So, in our case, I inputted ```~/OneDrive/Desktop/'VS Code Work'/'CSE 15L'/'Week 5 lab'/docsearch/technical``` as the path, and then ran the command to find all of the links that are within the ```./techincal``` directory
- Although it returned nothing, this command is still useful to realize what we are looking for might not be there
- Since there are no links within the ```./technical``` directory, this command returned nothing
- Overall, this command would be very useful for the same reasons listed above

## 3.) find /path/to/search -mtime ?

- This command allows the user to search for certain files/directories based on when they were last modified
- By changing the last number, it changes how many days the files need to be last changed
- For instance, by changing the ```?``` to ```3```, the command will return all of the files and directories that were changed within the last 3 days
- In addition, we can use operators like ```+``` or ```-``` before the number to denote finding files before or after the number in days
- So, we can use ```-7``` to find all of the edited files within the last week

```
phill@PhillipLaptop MINGW64 ~/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical (main)
$ find ~/OneDrive/Desktop/'VS Code Work'/'CSE 15L'/'Week 5 lab'/docsearch/technical -mtime -3   
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/5
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/Code
```
- In the above code, I ran the ```find /path/to/search -mtime``` command with a time of ```-3``` to denote that I only want to see files updated in the last 3 days
- Now, since I created new files ```5``` and ```Code```, they were both returned, because they are inside the ```./technical``` directory, and were updated today
- This command can be extremely useful if you know an estimated time in which you updated the files
- For instance, if I were working on a project for an extended period of time, I could use this command to bring me back to the files I was working on in the last day or so

```
phill@PhillipLaptop MINGW64 ~/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical (main)
$ find ~/OneDrive/Desktop/'VS Code Work'/'CSE 15L'/'Week 5 lab'/docsearch/technical -mtime +8

```
- In the above code, I ran the ```find /path/to/search -mtime``` command with a time of ```+8``` to denote that I only want to see files updated 8 days and more ago
- Since there were no files created or even in this directory 8 days ago, nothing was returned, which was expected
- Once again, this could be used to determine how long ago you worked on something, and that can help locate your desired files

## 4.) find /path/to/search -size ?

- This command allows the user to search for certain files or directories based on their size in megabytes
- It also works very similarly to the ```-mtime``` command
- For instance, by using operators like ```+``` or ```-```, with a number after, we can denote which files we want to look for based on size
- By using the ```+``` we can narrow the search to files or directories that are only above the given number, and the opposite for ```-```

```
phill@PhillipLaptop MINGW64 ~/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical (main)
$ find ~/OneDrive/Desktop/'VS Code Work'/'CSE 15L'/'Week 5 lab'/docsearch/technical -size -1    
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/5
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/911report   
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/biomed      
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/Code        
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government  
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/About_LSC
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Alcohol_Problems
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Env_Prot_Agen
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Gen_Account_Office
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Media
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Post_Rate_Comm
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/plos
```
- In the above code, we run the ```find /path/to/search -size``` command with an input of ```-1``` to denote that we only want files less than a single megabyte
- Within the ./technical directory, there are tons of files that have less than a megabyte, and therefore, a lot of files were returned
- Once again, by using the ```-``` sign before the number, it made sure we only got files *less* than a megabyte
- This command can be really useful when trying to find certain files that are only a certain size in order to sort them

```
phill@PhillipLaptop MINGW64 ~/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical (main)
$ find ~/OneDrive/Desktop/'VS Code Work'/'CSE 15L'/'Week 5 lab'/docsearch/technical -size +500  
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/911report/chapter-13.4.txt
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/911report/chapter-13.5.txt
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/911report/chapter-3.txt
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Gen_Account_Office/d01591sp.txt
/c/Users/phill/OneDrive/Desktop/VS Code Work/CSE 15L/Week 5 lab/docsearch/technical/government/Gen_Account_Office/Statements_Feb28-1997_volume.txt
```
- In the above code, we run the ```find /path/to/search -size``` command with an input of ```+500``` to denote that we only want files more than 500 megabytes
- Within the ./technical directory, there are only a few files who have more than 500 megabytes of data within them, and we returned all of them here
- By using the ```+``` sign before the number, we made sure that only files who have more than 500 megabytes are returned, but we don't know exactly how large they are
- One potential benefit of this is to find out how large some files are, although it may take a lot of trial and error
- In addition, this command can be used to find and sort files based on their size, which might make it easier to find the files you are looking for



###
## For all of this research, I used chat-gpt. Here is the prompt and resulting answer I was given!
*Prompt*

- What are 4 different ways to use the find command in bash, with explanations?

*Result*

- Sure! Here are four different ways to use the find command in Bash:

Finding Files by Name:
lua
Copy code
find /path/to/search -name "filename"
This command searches for files within the specified directory (/path/to/search) and its subdirectories, matching the given filename exactly. It's useful when you know the exact name of the file you're looking for.
Finding Files by Type:
lua
Copy code
find /path/to/search -type f
This command searches for regular files within the specified directory (/path/to/search) and its subdirectories. You can also use -type d to find directories or -type l to find symbolic links.
Finding Files by Modification Time:
lua
Copy code
find /path/to/search -mtime -7
This command searches for files within the specified directory (/path/to/search) and its subdirectories that were modified within the last 7 days. You can use -mtime +7 to find files modified more than 7 days ago, or -mtime 7 for files modified exactly 7 days ago.
Finding Files by Size:
lua
Copy code
find /path/to/search -size +10M
This command searches for files within the specified directory (/path/to/search) and its subdirectories that are larger than 10 megabytes. You can use -size -10M to find files smaller than 10 megabytes, or -size 10M for files exactly 10 megabytes in size. This is helpful when you want to locate files based on their size.




