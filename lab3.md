**Lab Report 3**

**Part 1:**

The filter method is supposed to return a new list that retains the elements of the input list that satisfy the StringChecker and without the elements that do not. 

**Code with bug:**

```
static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }
```

**Failure inducing JUnit test:**

```
@Test
    public void testFilterFailure() {
        List<String> input = Arrays.asList("hi", "hello", "meow", "pie");
        StringChecker sc = new StringChecker() {
            public boolean checkString(String s) {
                return s.contains("i");
            }
        };
        List<String> expected = Arrays.asList("hi", "pie");
        List<String> result = ListExamples.filter(input, sc);
        
        assertEquals(expected, result);
    }
```
    
With the correct code, this test should return the list ["hi", "pie"] but it returns ["pie", "hi"].

**Passing JUnit test:**

```
@Test
    public void testFilterPassing() {
        List <String> input = Arrays.asList("hi", "meow");
        StringChecker sc = new StringChecker() {
            public boolean checkString(String s) {
                return s.contains("i");
            }
        };
        List<String> expected = Arrays.asList("hi");
        List<String> result = ListExamples.filter(input, sc);
        
        assertEquals(expected, result);
    }
}
```

This test passes because there is only one string in the array with the input element, meaning it will just return ["hi"].

![Image](filtertests.png)

The bug in the code is the line `result.add(0, s);`. The code fails because as it iterates through the ArrayList, it places strings with the input element at index 0, which means the new list will return reversed instead of the original order. 

**Fixed code:**

 ```
static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(s);
      }
    }
    return result;
  }
```

By removing the 0 in `result.add(0, s);`, the code returns the elements in order of iteration rather than at the same index, ensuring that the new list is in the correct order rather than reversed. 

**Part 2:**

The `find` command is used to search for files and directories, allowing users to find files by name, type, size, etc. 

**Command-Line Options:**

**1)** `-name pattern` : This command-line option allows the user to search for files with a specific name.

Example 1:
```   
matthewdoan@Matthews-MacBook-Pro-8 technical % find . -name nov1.txt
./government/Env_Prot_Agen/nov1.txt
```
  
This command example searches through the technical directory (working directory) and looks for the file nov1.txt, returning the file path. This might be of use if       there are many files the user has to search through because it allows them to instantly find the path instead of looking through the directory.
    
Example 2.
```
matthewdoan@Matthews-MacBook-Pro-8 technical % find . -name Media
./government/Media
```
    
This command example searches through the technical directory (working directory) and searches for the directory Media, returning the path. This could be useful when
there are directories within directories because the user does not have to manually look through them to find their needed path.
    
**2)** `-empty` : This command-line option finds any empty files and directories.

Example 1: 
```
matthewdoan@Matthews-MacBook-Pro-8 technical % find . -type f -empty
./government/Media/testempty2.txt
./plos/testempty.txt
```
This command example searches through the technical directory (working directory) and finds empty files, returning the file path. In this case I added empty files however this might be useful when looking for unused files that are taking up space. 

Example 2:
```
matthewdoan@Matthews-MacBook-Pro-8 technical % find . -type d -empty
./government/emptydirectory
./emptydirectory2
```
This command example searches through the technical directory (working directory) and finds empty directories, returning the path. Like for the files, I added empty directories, howver this could be useful in a similar way, deleting empty directories could optimize performance of the filesystem.

**3)** `-iname pattern` : This command-line option, like `-name pattern` allows the user to search for files with a specific name, however is case insensitive.

Example 1:
```
matthewdoan@Matthews-MacBook-Pro-8 technical % find . -iname JOURNAL.PBIO.0020001.TXT
./plos/journal.pbio.0020001.txt
```
This command example searches through the technical directory (working directory) for journal.pbio.0020001.txt despite the input being in all caps, and returns the path. This might be useful when you are unsure of the name of the specific file or directory you are looking for and don't want to miss it because of slight differences in capitalization. 

Example 2:
```
matthewdoan@Matthews-MacBook-Pro-8 technical % find . -iname ChApTeR-1.TXT         
./911report/chapter-1.txt
```
This command example searches through the technical directory (working directory) for chapter-1.txt despite the random capitalized letters, and returns the path. This might be useful when working on different platforms that differ in name conventions, for example Windows is case insensitive while Linux is case sensitive.

**4)** `-delete` : This command-line option deletes files that match the criteria.

Example 1:
```
matthewdoan@Matthews-MacBook-Pro-8 technical % find . -type d -empty -delete
```
This command example searches through the technical directory (working directory) for empty directories and then deletes them. There is no returned output other than the deletion of the directories. This might be useful when you need to delete empty directories in bulk, especially when there are too many to search through. 

Example 2:
```
matthewdoan@Matthews-MacBook-Pro-8 technical % find . -type f -empty -delete
```
This command example searches through the technical directory (working directory) for empty files and then deletes them. There is no returned output other than the deletion of the files. Like the directories, this is useful when you need to delete empty files in bulk.

**Works Cited:**
https://www.geeksforgeeks.org/find-command-in-linux-with-examples/

