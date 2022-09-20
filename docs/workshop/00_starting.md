
##Connecting to the Arcus Cluster in Oxford

We will use mobaXterm to connect to the arcus cluster. This uses Xwindows and ssh.

You will find mobaXterm in the H: drive on your machine.

Click on the "Session" icon. Choose "ssh". Enter "arcus-login.arc.ox.ac.uk" as the remote host and enter the username given to you (e.g. teaching99). Click "ok" and it will then prompt you for your ARC password.

'''Do not allow the computer to save your password'''

You are connected to the arcus Cluster!!



##Running your first cluster job

For this very first example you are going to submit a castep job to the queuing system. This will take place in the following steps:

* Creating a working area and copying some example code into it
* Preparing and and submitting the job
* Checking the output

All the things which you should type are in bold and your Linux prompt is always shown as:

  [teaching01@login11(arcus-b) ~]$

i.e. you do not need to type [teaching01@login11(arcus-b) ~]$  

##Step 0
Copy over the environment

  [teaching01@login11(arcus-b) ~]$ cp /home/jryates/WORKSHOP/bash_profile_castep $HOME/.bash_profile 

Note carefully the dot (.) before the second bash and the fact that the above is all on one line. Now you've copied the environment you need to activate it:

  source $HOME/.bash_profile


##Step 1
Copying the examples into a working area.

Firstly, let's create a temporary working area to do the examples in. This is usually a good idea for every distinct problem you're working on as it keeps things tidy and manageable. We'll create a directory (or "folder" in Windows parlance) called "intro" using the Linux command  mkdir

  [teaching01@login11(arcus-b) ~]$ mkdir intro

Now check that your intro directory has been made properly by listing the directory (or folder) you
are already in:

  [teaching01@login11(arcus-b) ~]$ ls

and you should see a list of files and directories including the new examples directory. Now let's "move" into that directory:

  [teaching01@login11(arcus-b) ~]$ cd intro

Note that "cd" is the Linux command for changing directories. You can move back to your home directory at any time by typing "cd" just on its own. You can check your current directory at
anytime with the "pwd command":

  [teaching01@login11(arcus-b) ~]$ pwd

If you run "ls" in the examples directory, nothing is listed as there are no files yet (try it). Let's copy the example code from one directory (provided by the tutors) to your current location. Note the space between the "*" and "." :

  [teaching01@login11(arcus-b) ~]$ cp /home/jryates/WORKSHOP/intro/* .

Do NOT forget the "." character. To Linux this means "my current directory". So you are copying (with the "cp" command) all files (represented by "*") in the directory  /home/jryates/WORKSHOP/intro  to your current directory.
Now try an "ls" to see what you've got. You should see several files:

  [teaching01@login11(arcus-b) ~]$
  diamond.cell diamond.param @]

These are the usual "cell" and "param" castep input files.
To look at the files you can use the command called more (similar commands are cat and less)

  user@login-sand7 ~]$  more diamond.cell

##Step 2 - Preparing & submitting a job

Now we are ready to submit the job to the queue. The special command ‘castepsub’ is used for this.
We’ve written this command specially for the workshop - on your local cluster there will be a different (but very similar) way to submit jobs to the cluster.

  [teaching01@login11(arcus-b) ~]$ castepsub -n 4 diamond

This will have submitted your job to the cluster to run on 4 processors. Use ‘ls’ to to list your files. It should take only a few seconds to run. You can then examine the castep output file "diamond.castep". Note how many kpoints
were used.

Rename the *.castep file

  [teaching01@login11(arcus-b) ~]$ mv diamond.castep diamond_200ev.castep


##Editing a file
Edit the diamond.param file (increase the cutoff energy to 400 eV). To do this you will need to use an editor. (for experts ‘vi’ and ‘emacs’ are available). Otherwise I suggest using an editor called nano. This has helpful list of instructions are the bottom of the screen (but ask if you are confused!)

  [teaching01@login11(arcus-b) ~]$ nano diamond.param

Now submit the job again.


Compare the runs at 200 and 400eV. Which took longer? Has the total energy gone up or down. Look at the Atomic Populations section - is it what you expect?


##Summary of useful commands
* mv   - rename (or move) a file eg  mv oldfile newfile
* cp   - copy a file eg  cp original copy
* pwd   - print current (working) directory
* mkdir  - make a new directory (aka folder)
* nano   - a file editor eg  nano filename  note you can use this to edit an existing file, or to create a new file. eg ‘nano mynewfile’ will create a new file called ‘mynewfile’
* ls  - list files in the current directory
* ls -l  - list files - but give more details than plain ls
* exit  - to close the terminal when you are finished
* cp fred/* jim/  - copy all the files in the folder  fred  into the folder  jim
* cp ../myfile ./  - copy the file myfile in the folder below to the current folder
* cp ~/myfile ./   - copy the file myfile in your home folder to the current folder
* qstat  - look at the list of jobs running and queued on the cluster
* qstat -u castepXX  - look at the list of jobs running for the user "castepXX"
* qstat -a  - as for qstat but more information
* castepsub -n 8 diamond  - submits a castep job with diamond.cell and diamond.param as inputs onto 8 cores with a time limit of 1 hour
* castepsub -n 8 -W 00:20:00  - as above but only run for 20 minutes
!!!check2xsf
This is a handy free program written by Michael Rutter (TCM group Cambridge). It can convert
castep.cell and castep.check files into various formats eg cell, pdb. (and many other things!)
* check2xsf -h  -
list all the options
* check2xsf --pdbn castep.cell castep.pdb
* check2xsf --pdbn castep.check castep.pdb
* check2xsf --cell castep.check new.cell \\
 (useful at the end of geometry optimisation)
