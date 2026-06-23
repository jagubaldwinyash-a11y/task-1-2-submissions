Level 1 → Level 2
Goal: Retrieve the password from a file named - (a dash).

Thought Process: The command cat - normally waits for standard keyboard input instead of reading a file. To override this, I must specify the explicit relative path (./-) so the system treats it as a file name.

Commands:
cat ./-
