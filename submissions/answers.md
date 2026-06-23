NAME: JAGU BALDWIN YASH
16010125285
COMPS- D DIVISION




Level 0 → Level 1

Goal: Retrieve the password stored in a file named readme in the home directory.

Thought Process: Need to check the contents of the current directory to verify the file exists, then read its plain text content.

Commands:

ls

cat readme

Result: Retrieved the password string to log into the next level.


Level 1 - Level 2

Goal: Retrieve the password from a file named - (a dash).

Thought Process: The command cat - normally waits for standard keyboard input instead of reading a file. To override this, I must specify the explicit relative path (./-) so the system treats it as a file name.

Commands:

cat ./-

Result: Successfully bypassed the special character conflict and printed the password.

Level 2 - Level 3

Goal: Retrieve the password from a file named spaces in this filename.

Thought Process: Linux treats spaces as separators between different commands or arguments. I need to use quotes or backslashes to tell the terminal that the spaces are part of a single file name.

Commands:

cat "./--spaces in this filename--"

Result: Read the file properly and captured the next password.

Level 3 - Level 4

Goal: Retrieve the password hidden in a secret directory inside the inhere folder.

Thought Process: Move into the target directory and list all contents. Standard file listing ignores hidden files (files starting with a dot .), so I need to use a flag that reveals everything.

Commands:

cd inhere
  ls -a
  cat ...Hiding-From-You

 Result: Uncovered the hidden .hidden file and extracted the password.

Level 4 - Level 5

Goal: Find the single human-readable ASCII text file inside a directory containing multiple binary data blocks.

Thought Process: Checking every file manually is inefficient. The file command can inspect the underlying data type of every file in the directory simultaneously to separate text from raw binary data.

Commands:

cd inhere
ls
  file ./*
  cat ./-file07

  Result: Isolated the one file labeled "ASCII text" and retrieved the password.

Level 5 - Level 6

Goal: Locate a specific file inside the inhere directory that is human-readable, exactly 1033 bytes in size, and not executable.

Thought Process: The directory has too many subfolders to search manually. The find utility allows combining multiple exact criteria (size, type, permissions) to pinpoint the exact file location.

Commands:

find inhere -type f -size 1033c
cat inhere/maybehere07/.file2

Result: Located the single matching file and automatically printed its password payload.

Level 6 - Level 7

Goal: Locate a file somewhere on the server owned by user bandit7, group bandit6, and exactly 33 bytes in size.

Thought Process: This requires searching the entire filesystem starting from the root directory (/). Because this will trigger hundreds of "Permission denied" errors for folders I don't own, I need to redirect error logs to /dev/null to keep the screen clean.

Commands:

find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
cat /var/lib/dpkg/info/bandit7.password

  Result: Discovered the file deep within the system logging directory and read the password.

Level 7 - Level 8

Goal: Retrieve the password stored in the file data.txt next to the word "millionth".

Thought Process: The file is massive and filled with text lines. The grep tool is designed to scan data streams and extract only lines that contain a specific keyword match.

Commands:

grep "millionth" data.txt

Result: Extracted the single row containing "millionth" alongside its corresponding password.

Level 8 - Level 9

Goal: Find the only line of text in data.txt that occurs exactly once.

Thought Process: The uniq tool can find non-repeated lines, but it only works if duplicate lines are sitting directly next to each other. I must first pipe the file contents through sort before running the uniqueness filter.

Commands:

sort data.txt | uniq -u

Result: Filtered out all duplicated noise to reveal the lone password string.

Level 9 - Level 10

Goal: Extract the password from a mostly unreadable binary file (data.txt) where it is preceded by several = characters.

Thought Process: Standard text tools fail on compiled binary data. The strings command filters out unreadable data to print human-readable text blocks, which can then be parsed using grep.

Commands:

strings data.txt | grep "=="

Result: Pulled the readable text out of the binary file and found the password string next to the ==== markers.

Level 10 - Level 11

Goal: Decode the password stored in data.txt, which is encoded in Base64 format.

Thought Process: Base64 is a data encoding scheme, not an encryption method. Passing the file data through a native decoding tool will cleanly translate it back into raw text.

Commands:

base64 -d data.txt

Result: Decoded the Base64 layer and printed the clear-text password.

Level 11 - Level 12

Goal: Decode the password inside data.txt which has had all its letters rotated by 13 positions (ROT13 cipher).

Thought Process: ROT13 shifts every letter by half the alphabet. The translate (tr) utility allows mapping character sets to shift the sets A-Z and a-z forward by exactly 13 letters.

Commands:

cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'


Result: Shifted the letters back to their correct layout and exposed the true password.

Level 12-13

Goal: Decompress a file that has been repeatedly compressed using different archival formats (Hex dump, Gzip, Bzip2, and Tar).

Thought Process: The asset is a text-based hex dump. I need to reverse the hex dump back into binary data using xxd, check its file type dynamically using file, change its file extension, and apply the matching decompression command (gunzip, bunzip2, or tar) repeatedly until the data is fully unpacked.

Commands:
mkdir /tmp/myworkspace12
xxd -r data.txt>datafile
mv datafile datafile.gz
bzip2 -d datafile
mv datafile.out datafile.gz
tar -xf datafile
tar -xf data5.bin
bzip2 -d data6.bin
tar -xf data6.bin.out
mv data8.bin data8.bin.gz
cat data8.bin

