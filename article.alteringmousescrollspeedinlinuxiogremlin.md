ALTERING MOUSE SCROLL SPEED IN LINUX | I/O GREMLIN

Updated: 05/01/2021.

I started playing with Linux, having installed Mint (Ubuntu based,
which is Debian based – FOR MORE DETAILS). Linux Mint is very user
friendly (unlike Linux 15 years ago), but it does have its
peculiarities (especially for us coming from decades of DOS & Windows
use). One of the “problems” for me was simple mouse scroll button
speed change. Here’s how I fixed that.

For solving this I used IMWHEEL. The procedure is relatively simple
and straight forward, with a few tweaks (noted in this post). It boils
down to this:

1. INSTALLING IMWHEEL

You can open Software Manager, search for “Imwheel”, then click
“Install”, after which you will be asked to enter your root
password and that’s it.
[Installing Imwheel using Linux Software Manager][Installing Imwheel
using Linux Software Manager]Installing Imwheel using Linux Software
Manager
Picture 1[Authenticating the install with a root password (1), then
click "Authenticate" (2)][Authenticating the install with a root
password (1), then click "Authenticate" (2)]Authenticating the install
with a root password (1), then click “Authenticate” (2)
Picture 2
Of course, you could use the command prompt (CTRL+ALT+T is the
keyboard shortcut, not WIN+R – note to self 🙂 ). Imwheel is
installed using the following command (you will be prompted for a root
password after pressing Enter):

sudo apt-get install imwheel
[Command prompt installation of Imwheel][Command prompt installation
of Imwheel]Command prompt installation of Imwheel
Picture 3
If all went well, you will see something like this:
[Imwheel successfully installed][Imwheel successfully
installed]Imwheel successfully installed
Picture 4

2. CONFIGURING IMWHEEL

Configuration boils down to creating a .sh file with an appropriate
code, then running it. I did it the following way:

I created an “Utils” directory in my “Home” directory (that is
“/home/relja/Utils”). Of course, you can put the file anywhere you
want.

Then I opened a text editor, copied the needed code (found at THIS
PAGE – thank you) and saved it as “mouse.sh” – you can name it
however you like, as long as you know the file name and directory
where it’s saved at.

Needed file contents:

#!/bin/bash
# Version 0.1 Tuesday, 07 May 2013
# Comments and complaints http://www.nicknorton.net
# GUI for mouse wheel speed using imwheel in Gnome
# imwheel needs to be installed for this script to work
# sudo apt-get install imwheel
# Pretty much hard wired to only use a mouse with
# left, right and wheel in the middle.
# If you have a mouse with complications or special needs,
# use the command xev to find what your wheel does.
#
### see if imwheel config exists, if not create it ###
if [ ! -f ~/.imwheelrc ]
then

cat >~/.imwheelrc<<EOF
".*"
None,      Up,   Button4, 1
None,      Down, Button5, 1
Control_L, Up,   Control_L|Button4
Control_L, Down, Control_L|Button5
Shift_L,   Up,   Shift_L|Button4
Shift_L,   Down, Shift_L|Button5
EOF

fi
##########################################################

CURRENT_VALUE=$(awk -F 'Button4,' '{print $2}' ~/.imwheelrc)

NEW_VALUE=$(zenity --scale --window-icon=info --ok-label=Apply --title="Wheelies" --text "Mouse wheel speed:" --min-value=1 --max-value=100 --value="$CURRENT_VALUE" --step 1)

if [ "$NEW_VALUE" == "" ];
then exit 0
fi

sed -i "s/\($TARGET_KEY *Button4, *\).*/\1$NEW_VALUE/" ~/.imwheelrc # find the string Button4, and write new value.
sed -i "s/\($TARGET_KEY *Button5, *\).*/\1$NEW_VALUE/" ~/.imwheelrc # find the string Button5, and write new value.

cat ~/.imwheelrc
imwheel -kill

Then, in command promt, go to the directory and run the file. Before
running the script, you must give it the execution rights using
command “chmod +x” In my case:

cd /home/relja/Utils
chmod +x mouse.sh
./mouse.sh

Here’s how it looks like on screen, you’ll get to set mouse scroll
speed:
[Enter the three command lines (enter after each, of course) (1),
select desired scroll speed (2), then click "Apply" (3)][Enter the
three command lines (enter after each, of course) (1), select desired
scroll speed (2), then click "Apply" (3)]Enter the three command lines
(enter after each, of course) (1), select desired scroll speed (2),
then click “Apply” (3)
Picture 5
I chose 3. After clicking at “Apply”, you’ll see something like
this:
[Successfully finished scroll speed configuration][Successfully
finished scroll speed configuration]Successfully finished scroll speed
configuration
Picture 6
IF YOU AREN’T HAPPY WITH THE NEWLY SET MOUSE SCROLL SPEED, just run
the “mouse.sh” again, as expained in picture 5.

3. CONFIGURING IMWHEEL TO RUN AFTER EACH RESTART

For this I used “Startup Applications”.
[Opening "Startup Applications"][Opening "Startup
Applications"]Opening “Startup Applications”
Picture 7
On the next screen click the ” + ” sign and choose “Custom
command” option.
[Click the + sign and choose "Custom command"][Click the + sign and
choose "Custom command"]Click the + sign and choose “Custom
command”
Picture 8
The last step is shown and explained in the picture 9:
[Use whatever you want for name (1) Command must say "imwheel",
because that is the application (2) For comment, use whatever you like
(3) Add a startup delay if you like - I set a 5 second delay (4)
Finally, click "Add" (5)][Use whatever you want for name (1) Command
must say "imwheel", because that is the application (2) For comment,
use whatever you like (3) Add a startup delay if you like - I set a 5
second delay (4) Finally, click "Add" (5)]Use whatever you want for
name (1)
Command must say “imwheel”, because that is the application (2)
For comment, use whatever you like (3)
Add a startup delay if you like – I set a 5 second delay (4)
Finally, click “Add” (5)
Picture 9
You can try restarting the computer, to make sure mouse scroll speed
is still working properly.

TROUBLESHOOTING

As ALEX ADDED IN THE COMMENT SECTION, there can be a problem with the
functioning of extra mouse buttons (with some mouses that have extra
buttons, in addition to the standard two and the wheel). The solution
is limiting imwheel to the scroll only (wheel up: 4, and wheel down:
5). For Linux Mint, it is the following command:

imwheel -b "4 5"

“-b” is the switch that basically says “deal only with the
listed buttons”. This can also be added to the command line – (2)
in picture 9.

For more details (manual), type this in the command prompt:

man imwheel


---
https://io.bikegremlin.com/11541/linux-mouse-scroll-speed/#1