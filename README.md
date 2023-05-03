Download Link: https://assignmentchef.com/product/solved-cs0449-lab-7-processes-and-error-handling
<br>
For project 4, you’ll need to be able to start new processes. Error handling is also really important, and will prevent you from accidentally creating a forkbomb

Getting started

Right click and download this link. This is the skeleton of your lab.

<strong>Please be sure to put your name at the top.</strong>

This program takes any number of command-line arguments and <strong>runs the program specified.</strong> I’ve already written the fork(), execvp() and waitpid() system calls for you, but you’ve got some work to do to <strong>make it output some useful error messages.</strong>

For example, if you compile it and run it like so:

$ ./lab7 ls -ltotal 17-rwxr-xr-x 1 abc123 UNKNOWN1 6875 Apr  6 02:04 lab7-rw-r–r– 1 abc123 UNKNOWN1 1593 Apr  6 02:02 lab7.c———-$ _

Right now, all it does is run the program, and then print a line of ——–.

This program can serve as the basis of your project 4, since the main responsibility of a shell is to start new processes and report errors from them!

<h2>What to do</h2>

<strong>Read the comments in the code I’ve given you</strong> to see what you have to do and where to do it.

This lab is also testing to see if you can follow those instructions by looking up documentation, so <strong>use the </strong><strong>man</strong><strong> pages.</strong>

man pages are… dense. They have a lot of information, and they go into a lot of detail. But the important parts to focus on are:

<ul>

 <li>The first paragraph of the “DESCRIPTION” as well as any parts of it that explain the arguments</li>

 <li>The “RETURN VALUE” section</li>

 <li>Sometimes, the “ERRORS” sections</li>

</ul>

Here are some pages you will find useful:

<ul>

 <li>man 3 perror

  <ul>

   <li><em>the 3 makes it look up the C library call instead of some MySQL thing.</em></li>

   <li><em>if the manpages you find make no sense, try putting the 3 in there.</em></li>

  </ul></li>

 <li>man 3 exit</li>

 <li>man execve</li>

 <li>man signal

  <ul>

   <li>this will tell you how to <strong>ignore a kind of signal</strong> with the signal() function.</li>

  </ul></li>

 <li>man waitpid

  <ul>

   <li>will tell you about how to check the return value and status from the process.</li>

   <li>the WIFEXITED(status) stuff is kinda weird, so here’s an example on how to use those functions:</li>

  </ul></li>

</ul>

// this is code that’s already in the lab7.c file for you.int status;int childpid = waitpid(-1, &amp;status, 0); if(WIFEXITED(status))        // it exited normally; use WEXITSTATUS() to extract the exit code. // et cetera…

<h2>Things to test</h2>

Try running some simple commands that you know will complete successfully. Your output might look like:

$ ./lab7 lslab7  lab7.c———-Program exited successfully! $ ./lab7 echo “hello”hello———-Program exited successfully! $ _

Then try running a <strong>valid command that will fail,</strong> like trying to ls a directory that doesn’t exist:

$ ./lab7 ls /bogusls: cannot access ‘/bogus’: No such file or directory———-Program exited with error code 2 $ _

Then try running some <strong>bogus commands</strong> that should cause the execvp to fail. In addition to printing the message about the invalid command, you should <em>also</em> see a report about the error code – that should be <strong>the error code that you pass to the </strong><strong>exit()</strong><strong> after </strong><strong>execvp()</strong><strong> failed.</strong>

$ ./lab7 aisfjajojojedgeError running program: No such file or directory———-Program exited with error code 1 $ _

Then, try killing a process with a signal. An easy one to use is SIGINT, which is sent when you use ctrl+C. That’s why you set up the SIGINT handler to be ignored. For example:

$ ./lab7 cathellohello^C———-Program was terminated by signal: Interrupt $ _

This happens because cat without arguments simply copies stdin to stdout, so we got trapped in it. Pressing ctrl+C sends SIGINT to cat, which kills it, which <em>your</em> program can detect and print an error about.