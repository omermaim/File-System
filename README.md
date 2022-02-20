# File-System
this is a project that creates a  basic file system managed within a single file.
in this page you can see all the operations the program supports and at the end of the page there is an explanation decribing the implementation of the file system.
for each file the single file saves:
File name
File size
File date & time (creation date & time)
File data (content)
the program supports these operations.
1. 

> BGUFS -create <filesystem>

 Create a new filesystem with given filename

Assume that the filename does not exist already
 
 the name must start with BGUFS_

Example: BGUFS.exe -create BGUFS_1.dat

 

2. 

> BGUFS –add <filesystem> <filename>

Add a file <filename> to the filesystem

If the filename already exists in the filesystem, print an error message: "file already exist", and exit

The filesystem supports all types of files (pdf, word, etc..)

Example: BGUFS.exe –add NYBGUFS.dat c:\mydocument\example.pdf

 

3. 

> BGUFS –remove <filesystem> <filename>

Remove a file <filename> from the filesystem.

If the filename does not exist in the filesystem, print an error message: "file does not exist", and exit

Example: BGUFS.exe –remove MYBGUFS.dat example.pdf

 

4. 

> BGUFS –rename <filesystem> <filename> <newfilename>

Rename a file <filename> in the filesystem

If the filename does not exist in the filesystem, print an error message: "file does not exist", and exit

If the newfilename already exist in the filesystem, print an error message "file <newfilename> already exists" and exit

Example: BGUFS.exe –rename MYBGUFS.dat example.pdf example2.pdf



5. 

> BGUFS –extract <filesystem> <filename> <extractedfilename>

Extract file <filename> from a filesystem and save it to the local directory as <extractedfilename>.

Do not remove the original file from the filesystem.

If the filename does not exist in the filesystem, print an error message: "file does not exist", and exit.

Assume that the extractedfilename does not exist in the local directory.

Example: BGUFS.exe –extract MYBGUFS.dat example.pdf c:\tmp\example.pdf

 

6. 

> BGUFS –dir <filesystem>

Print the list of file names in the filesystem in the following format:

<filename1>,<size>,<date>,<regular/link>,<linked file name (in a case of link)>

<filename2>,<size>,<date>,<regular/link>,<linked file name (in a case of link)>

...

Example: BGUFS.exe –dir MYBGUFS.dat

sample output:

example.pdf,100000,05/06/2021 12:00,regular
letter.docx,56000,01/02/2020 15:34:00,regular
linktopdf,100000,05/06/2021 12:00,link,example.pdf
... 

7.

> BGUFS –hash <filesystem> <filename>

Print the md5 hash value of a file.

If the filename does not exists, print an error message: "file does not exist", and exit, and exit.

Example: BGUFS.exe –hash MYBGUFS.dat example.pdf.



8. 

> BGUFS -optimize <filesystem>

Optimize the filesystem to reduce its size (if possible). If there are empty 'holes' in the filesystem due to deleted files, you should optimize the filesystem and get rid of these holes.

Example: BGUFS.exe -optimize MYBGUFS.dat 



9.

> BGUFS -sortAB <filesystem>

Sort the files in the filesystem in an alphabetical order. E.g., in the next "BGUFS.exe -dir <filesystem>" command, the files should appear sorted in the alphabetical order.

Example: BGUFS.exe -sortAB MYBGUFS.dat 



10.

> BGUFS -sortDate <filesystem>

Sort the files in the filesystem in a creation date/time order. E.g., in the next "BGUFS.exe -dir <filesystem>" command, the files should appear sorted in the creation time order from oldest to newest.



11.

> BGUFS -sortSize<filesystem>

Sort the files in the filesystem in a size order (smallest to largest). E.g., in the next "BGUFS.exe -dir <filesystem>" command, the files should appear sorted in the size order from smallest to largest.

comment on 10,11: in a case of same date/size of two or more files, use the name alphabetical order as a secondary sort.



12.

> BGUFS -addLink <filesystem> <linkfilename> <existingfilename>
 

Add a new file linkfilename which points on existing file in the filesystem.
linkfilename is like a regular file and all operations should be applied to it as a usual file.
However, it doesn't contain the file content, but point on the the data of the existing file (think about it like a shortcut in windows, or link in Linux)
Note that when a file is deleted, all links to this file must be deleted as well.
If the linkfilename already exists in the filesystem, print an error message: "file already exist", and exit
If the existingfilename does not exist in the filesystem, print an error message: "file does not exist", and exit, and exit.

Example: 

BGUFS.exe -addlink MYBGUFS.dat linkexample.pdf example.pdf
  
  
  
  
  
  
  
Explanation:
First of all we save all the metadata of each file in the header section and its content in the content section. we provide a size for our header depending on the size of the files int it.  we start with a size of 1024 bytes and the size increases automatically if needed. At the end of our header section we have a * that signals the separation between both sections.
Metadata: 
for each file we store in a list of strings the following data:
name,size(kb),Date created, address of start of Content, regular/link.
We Store the data of all our files in a list of String arrays that each String array represents a file. In every method we first read all the header data currently in the file into the list and erase the header, then using the header we execute the method from the input and then insert the data again after it is changed(depending on the users input method).
Content:
Our content starts after the barrier (*) at the end of the header file and the content of each file simply is placed one after the other, while the start of each files content is saved in its Metadata in the header file.
Methods :
Sorting methods:
To sort the metadata of our header section we used the sort function of collections in c#.
We knew that we need to sort the same data , but by different values(SortAb – name, SortSize - size, SordDate - Date created) so for each method we sent the sort function a comparable class to provide the sort function with the value it sorts by.

