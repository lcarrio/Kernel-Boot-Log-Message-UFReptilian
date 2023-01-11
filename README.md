# Kernel-Boot-Log-Message-UFReptilian
Modify the kernel to print my name, UFID, and a personal message to the default boot (and rebuild). Then create a unified patch file. 

Lazaro Carrio
37810524

Modifying the Boot Message:
This section of the report will describe the exact changes I made to my 
kernel in order to complete the project. The next section will describe 
why I made those changes and the issues I ran into while doing the 
project. In order to modify the boot message, I first had to find the 
correct file to modify. The hint in the P0 spec sheet stated that the 
message should be added near the call to the rcu_end_inkernel_boot() 
function. I used the following command in the /usr/rep/src directory to 
find the file containing the function call:
$grep -rnw /usr/rep/src  -e  rcu_end_inkernel_boot()
I then used the following command to transfer the main.c file to my local 
computer:
scp  reptilian@192.168.68.129:/usr/rep/src/reptilian-kernel/init/main.c  
main.c
On my local computer I used the printk() command with the log level 
KERRN_ERR to modify the main.c file in the line after the call to 
rcu_end_inkernel_boot():
$printk(KERN_ERR "\n##### Lazaro Carrio (UFID: 3781-0524) Gator Nation! 
#####\n");
After modifying the main.c file I removed the original main.c from 
/init/main.c and transferred the modified file in using the scp command. I 
ran the make commands to apply the changes, I restarted my VM and my 
message was displayed on boot up.
Trials and Tribulations:
The first issue I ran into was that I did not know what command to use to 
find a function call within a file. My ta pointed me towards the commands 
listed in Ex1 and I was able to locate the grep and scp which helped me 
find and transfer the /init/main.c file.
Once I had the file in my local computer, I tried several print functions 
in different locations near the call to rcu_end_inkernel_boot(). None of 
the print functions I used worked so I investigated the linux log files 
and found that KERN_ERR log level to work. I looked around the source code 
in main.c and found a printk() command using the KERN_DEBUG parameter and 
used the same format switching to KERN_ERR to print my message. The next 
problem I ran into was creating the patch file. I used the following 
commands to create my p0.diff file in the reptilian-kernel directory:
$git add -u
$git add init/main.c
$git add diff remotes/origin/os-latest > p0.diff
When I reverted to a clean snapshot and applied the patch I kept receiving 
an error.
Boris helped me fix the error by instructing me to clear my git files 
using the following command:
$git reset HEAD hard
I then ran the patch file on a clean vm and the patch applied 
successfully.
Extra Credit Assignment:
My ta instructed me that the file is not located within the reptilian-
kernel directory so I broadened my search and found the menu.lst file in 
the sysroot directory:
$grep -rn -l Reptilian 20.08-A9.0-r2
I learned how to use nano, so instead of transferring the file over I used 
sudo nano menu.lst and modified the file. I then ran the make commands in 
the reptilian-kernel directory and rebooted my vm to find the message 
displayed correctly.
Screencast:
Link: https://youtu.be/-6AY2tu0kYMWhen I reverted to a clean snapshot and applied the patch I kept receiving 
an error.
Boris helped me fix the error by instructing me to clear my git files 
using the following command:
$git reset HEAD hard
I then ran the patch file on a clean vm and the patch applied 
successfully.
Extra Credit Assignment:
My ta instructed me that the file is not located within the reptilian-
kernel directory so I broadened my search and found the menu.lst file in 
the sysroot directory:
$grep -rn -l Reptilian 20.08-A9.0-r2
I learned how to use nano, so instead of transferring the file over I used 
sudo nano menu.lst and modified the file. I then ran the make commands in 
the reptilian-kernel directory and rebooted my vm to find the message 
displayed correctly.
Screencast:
Link: https://youtu.be/-6AY2tu0kYM
