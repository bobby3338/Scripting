# Test
Command: tests --help
# Test format
[ test ] 
  1. -d FILE - True if file is a directory
  2. -e FILE - True if file exists
  3. -f FILE - True if file exists and is a regular file
  4. -r FILE - True if file is readable by you
  5. -s FILE - True if file exists and is not empty
  6. -w FILE - True if the file is writable by you
  7. -x FILE - True if the file is executable by you
  8. -z STRING - True if string is empty
  9. -n STRING - True if string is not empty
     STRING1 == STRING2 - equal
     STRING1 != STRING2 - not equal
arg1 -eq arg2 - equal
arg1 -ne arg2 - not equal
arg1 -lt arg2 - less than
arg1 -le arg2 - less than or equal to
arg1 -gt arg2 - greater than
arg1 -ge arg2 - greater than or equal to
&& = AND  following by command will execute only if the first one is successful 
|| = OR
separate commands with a semicolon to ensure they all get executed - cp test.txt /tmp/bak/ ; cp test.txt /tmp
 
# if then #
if [ condition-is-true ]
then 
  command 1
  command 2
fi
# if then else #
if []
then
  command
else
  command
# if then elif then else #
if []
then
  command 
elif []
then
  command
else
fi

# For loop #
for COLOR in red green blue 
do
  echo "Color: $COLOR"
done

# Renaming to 2015-03-26-bear.jpg
#!/bin/bash
PICTURES=$(ls *jpg)
DATE=$(date +%F)
for PICTURE in $PICTURES
do 
  echo "Renaming ${PICTURES} to ${DATE}-${PICTURE}"
  mv ${PICTURE} ${DATE}-${PICTURE}
# Archiving User #
#!/bin/bash
echo "Executing script: $0"
for USER in $@  - USER VARIABLE
do
  echo "Archiving user: $USER"
  passwd -l $USER
  tar cf /archives/${USER}.tar.gz /home/${USER}
done
./archive.sh sunny joe

# Accepting User Input (STDIN) #
  read -p "PROMPT" VARIABLE

# Exit code #
  $? - 0 = succeed, other than 0 = error
  0-255
# function #
function must be defined before being called
function function-name() {
  echo "Hello"
  } 

#Named Character Classes
[[:alpha:]]
[[:digit:]]
[[:lower:]]
[[;space:]]
[[:upper:]]
[[;alnum:]]

Matching Wildcard patterns
*\? - match wildcard end with ? - done?
ls -F - list the files and directories

In a for loop
#!/bin/bash
cd /var/www
for FILE in /var/www/*.html - set varialbe FILE for all html file in /var/www
do 
  echo "Copying $FILE"
  cp $FILE /var/www-just-html
done

# Case Statement #
case "$VAR" in 
 pattern_1)
   command
   ;;
  pattern_2)
    command
    ;;
  esac
example:
  case "$1' in 
    start)
      /usr/sbin/sshd
      ;;
    stop)
      kill $(cat /var/run/sshd.pid)
      ;;
    *)
      echo "Usage; $0 start|stop" ; exit 1
      ;;
  esac
