
# Tutorial


Hi There 👋 , This GitHub repository offers a concise guide to Submitty, an open-source system for managing programming assignments in educational settings. It provides step-by-step instructions on installation, configuration, assignment creation, submission management, grading, and customization. This repository serves as a valuable resource for learning and implementing Submitty effectively.


---
# Table of Content
* Installation For Intel-Based Developers (Windows, Mac, or Unix/Linux)
    * You can refer to [VM Install using Vagrant](https://submitty.org/developer/getting_started/vm_install_using_vagrant#using-your-submitty-developer-vm) for intel-based developers(Windows, Mac, or Unix/Linux).
* Installation For Mac OS Developers with Apple silicon.
    * You can refer to [Installation On mac with Apple Silicon](https://submitty.org/developer/getting_started/vm_install_using_vagrant_apple_silicon) for developers with Apple Silicon Mac running macOS Monterey (12.4) or higher.(eg M1 or M2)
* How To Use Submitty Developer VM?
* How To Prepare Assignments?
* How To Create Team Assignments?
* How Autograding works And Its Directory Structure 
    + Phases of Autograding
        + Compilation
        + Execution
        + Validation
    + Directory Structure
* Autograding Specification (What Should Your ```config.json``` Should Contain )
    + Testcase Specification
    + Specification of a Validation Object
    + Python Custom Validation

---

# Installation For Intel-Based Developers (Windows, Mac, or Unix/Linux) 


--------------------------------***Pre-Installation Checklist for Intel-Based Developers***---------------------------

* To develop with a Virtual Machine (VM), your computer should have at least 8GB of RAM and a 64-bit host OS. AMD-V or Intel VT-x are also required (most computers have these). Submitty is RAM and I/O intensive, so more RAM and a fast disk are better.

* Make sure you have at least 65GB of hard disk available for installation. We do not recommend installing the Submitty Developer VM on DropBox, OneDrive, GoogleDrive, or other cloud storage.

* Some developers have had problems running both VirtualBox and VMWare on the same computer. If you have problems, we suggest shutting down the VMWare VMs, or stopping the VMWare services, or uninstalling VMWare.

* If you’re running Windows, it is recommended to disable Hyper-V. Leaving it enabled will force VirtualBox to use the Hyper-V backend, which will be slower and can cause instability in the VM.

_**Note:** This may stop programs like Docker Desktop and WSL 2 from working. If these programs are essential to your workflow, consider looking up how to add a separate boot entry with “hypervisorlaunchtype” set to “off” for use with VirtualBox._

_**Note:** Installing WSL2 may also reconfigure your OS to use Hyper-V or Windows hypervisor platform and prevent VirtualBox from working correctly. It is recommended to not install or use WSL2 alongside Virtualbox for now._

* The complete installation process could take an hour or more. Make sure your internet connection is strong and consistent. You’ll probably want to plug in your laptop power cord. Check your computer settings and make sure the machine does not hibernate or go to sleep during installation.

----------------------------------***Installation Steps for Intel-based Developers***----------------------------------
1. **Enable Virtualization**
***MacOS***
* Virtualization is generally enabled by default.

***Windows 10***

* Open the Settings app by searching for it in the windows bar or clicking it in the Windows menu.
* Navigate to Update and Security, then select Recovery from the side menu.
* Under Advanced Startup, click Restart Now.
* Once your PC has rebooted, click the Troubleshoot option.
* Click Advanced Options.
* Click UEFI Firmware Settings and restart as suggested.
* Enter your BIOS (generally by pressing Del, F12, or other keys while booting). If you are not able to find the key combo needed to enter your BIOS, refer to [this guide](https://www.tomshardware.com/reviews/bios-keys-to-access-your-firmware,5732.html).
* Locate Virtualization, and enable it. (Note: If you cannot find the option to enable virtualization, [search Google](https://www.google.com/search?q=enabling+virtualization+in+bios&oq=enabling+virt&aqs=chrome.0.0j69i57j0l4.1853j0j7&sourceid=chrome&ie=UTF-8) for a tutorial on enabling it with your motherboard.)
* Reboot your computer.

***Ubuntu***
* Enter your **BIOS** (generally by pressing Del, F12, or other keys while booting).

* Navigate the **BIOS Settings**.

* Locate **Virtualization** and enable it.

* Be sure to choose **Hardware Virtualization** in the **System -> Acceleration** settings of the virtual machine you are using.

**NOTE:** If using secure boot, vagrant may fail to work with VirtualBox. You will then either need to disable secure boot from the boot menu/BIOS or follow [these steps](https://era86.github.io/2018/01/24/vagrant-virtualbox-secureboot-in-ubuntu-1604.html) to self-sign the necessary packages to run vagrant and VirtualBox.

2. Download and install the latest version of [Ruby](https://www.ruby-lang.org/en/downloads/).
3. Download and install the latest version of [Git](https://git-scm.com/downloads).
4. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Download_Old_Builds_6_1) and [Vagrant](https://www.vagrantup.com/).
* **NOTE:** Please download VirtualBox 6 instead of 7.

------------------------- _**Below are quick steps to get everything installed and running.**_-----------------------

***Windows 10***
* You can just go to the respective sites and download the necessary binaries.
***MacOS***
* You can either go to respective sites and download the necessary binaries or install [homebrew](https://brew.sh/) if you don’t have it and then run:
```bash
brew install --cask virtualbox
brew install --cask vagrant
```

***Ubuntu/Debian***
* The Ubuntu repository does not contain the latest version of Vagrant or VirtualBox and using them may not work nor are they supported. We recommend that you either download the necessary binaries from their respective steps or follow the steps outlined below for each:
* VirtualBox: https://www.virtualbox.org/wiki/Linux_Downloads

* Vagrant: https://developer.hashicorp.com/vagrant/downloads (if that doesn’t work, try: https://vagrant-deb.linestarve.com/)

**Fedora/Red Hat Linux**
* For Fedora, the latest version of VirtualBox is recommended to prevent errors. Download the RPM from the virtual box website. Make sure your version of Fedora is up to date using

```bash
sudo dnf update
sudo dnf upgrade
```
and inputting your password. Then install the Virtual Box rpm using:
```bash
sudo dnf install VirtualBox-xxxxx.rpm 
```

***Install Vagrant using:***

```bash
sudo dnf install vagrant
```
-------Now move on to step 5.--------

**NOTE:** when running vagrant up, use ```vagrant up --provider=virtualbox``` so it doesnt default to libvirt


------------------------------***Common errors when running vagrant up(Fedora/RHEL)***--------------------------
1. Missing virtnetworkd: Enable it in your terminal by running:
```bash
sudo systemctl start virtnetworkd
```
2. If your vagrant ever freezes kill it with ‘’’ VBoxManage controlvm VM_NAME poweroff ‘’’ or if that doesn’t work, reboot the computer and then run ```vagrant destroy``` before re-running ```vagrant up --provider=virtualbox``` again.

3. Clone the [Submitty repository](https://github.com/Submitty/Submitty) to a location on your computer (the “host”).
```bash
git clone https://github.com/Submitty/Submitty.git
```

NOTE: If you are not currently part of the Submitty organization on Github, you may want to [fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) the repo and use the git url from your fork instead, especially if you are looking to contribute.

OPTIONAL: If you will be developing code in one of the companion Submitty repositories (e.g., AnalysisTools, Lichen, Localization, RainbowGrades, Tutorial), also clone those repositories to the same directory. For example:
```bash
home
  └── myusername
      └── Submitty
          └── GIT_CHECKOUT
              ├── AnalysisTools  (optional)
              ├── Lichen         (optional)
              ├── Localization   (optional)
              ├── RainbowGrades  (optional)
              ├── Submitty
              └── Tutorial       (optional)
```

4. Navigate into the Submitty repository on your computer in a shell/terminal and type:
* Windows should run CMD or powershell on administrator mode
```bash
vagrant up
```

_Vagrant will build your VM. This will take maybe 30 minutes to a few hours depending on your Internet connection speed. When this command finishes, your VM is ready to use._

5. When the ```vagrant up``` command completes successfully, you will be able to access the Submitty website (instructions follow in the next section).

The VM will continue to run jobs in the background and consume a nontrivial amount of CPU resources, while completing a backlog of autograding for a dozen or more sample submissions for each of the more than 100 users in the sample courses.

On MacOS and linux, if your development work will not require sample assignment submissions or autograding results, you may prepend ```NO_SUBMISSIONS=1``` to the previous command, which will skip the creation of these sample submissions and their autograding and decrease the time to complete installation.

```bash
NO_SUBMISSIONS=1 vagrant up
```

On Windows ```cmd```, you will have to first set the environment variable ```NO_SUBMISSIONS``` to 1 which lasts for the session of that console, then call vagrant up.
```bash 
SET NO_SUBMISSIONS=1
vagrant up
```
* On PowerShell, you will have to set the environment variable differently:

```bash
$Env:NO_SUBMISSIONS=1
vagrant up
```
If you want to unset the variable later in ```cmd```, you can do by command
```bash
SET NO_SUBMISSIONS=
```
Or in PowerShell,
```bash
Remove-Item Env:\NO_SUBMISSIONS
```
Similarly, you can check that the variable is set by doing
```bash
SET NO_SUBMISSIONS
```
Or in PowerShell,
```bash
$Env:NO_SUBMISSIONS
```
6. When the install has completed, you should see the message:
```bash
#####################################################################

                     INSTALLATION SUCCESS!
   
                        .GGQGGGSlu
                      .GGGGGGGGGGGS
                 :llUGGGGGGGGGGGGGGGG
                 'GGGGGGGGGGGGGGGGGGb        .
                    %GGGGGGGGGGGGGGG~   ..GSGGG
                       GGGGGGGGGGGGGGSGGGGGGGGGG[
                     ;GGGGGGGGGGGGp\ \ \GGGGGGGGL
                    !GGGGGGGGGGGGGGS\ \ \GGGGGG
                    GGGGGGGGGGGGGGGGG\ \ \9GGGG
                    %GGGGGGGGGGGGGGGS/ / /.GGG
                     %GGGGGGGGGGGGGS/ / /GGG
                      '%NNNNNNNNNNNNNNNNNN
#####################################################################
```
_**NOTE:** There are times when the install will pause for a brief period with the message ```Done```. This does not mean the install has ended, and the install should continue after a bit of time._

If you do not see this message due to an error or the installation has frozen, check out:
* [Installation Troubleshooting](https://submitty.org/developer/troubleshooting/installation_troubleshooting)


---
# Installation For Mac OS Developers with Apple silicon
***----------------------Pre-Installation Checklist for Mac Os with Apple silicon Developers---------------------***
* Make sure you have an Apple Silicon Mac running macOS Monterey (12.4) or higher.

* Make sure you have at least 65GB of free space on your disk to allocate for the virtual machine.

* The installation process may take upwards of an hour or more. Make sure your computer is connected to a reliable power source and stable network, and that it does not go to sleep or hibernate during the installation process.

------------------------------------***Pre-Installation Steps for Mac OS Users***--------------------------------------
1. Enable SMB file sharing**
* Open System Settings and navigate to Sharing
* Turn on File Sharing and go to options
* Check “Share files and folders using SMB”
* Check the box next to your username
* Click Done


***---------------------------Installation Steps for Mac OS Developers with Apple silicon------------------------***

1. Install [HomeBrew](https://brew.sh/)

```bash
  $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Follow the prompts and continue with the installation process.    

2. Download and install [Vagrant](https://www.vagrantup.com/) and [QEMU](https://www.qemu.org/)

Both of these packages are fairly simple to install with homebrew:

```bash
  $ brew install qemu

  $ brew install --cask vagrant

  $ vagrant plugin install vagrant-qemu
```

**Note:** It is possible that you may need to install Rosetta before installing vagrant. Run the following command to install Rosetta:

```bash
  $ sudo softwareupdate --install-rosetta
```

3. Clone and open the Submitty repository
Navigate to the parent folder you want to clone into and do

```bash
  $ git clone https://github.com/Submitty/Submitty

  $ cd Submitty
```
**NOTE:** If you are not part of the Submitty organization on GitHub, you may want to first [fork](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/working-with-forks/fork-a-repo) the repository and clone your fork instead, especially if you are looking to contribute.

_**OPTIONAL:** If you will be developing code in one of the companion Submitty repositories (e.g., AnalysisTools, Lichen, Localization, RainbowGrades, Tutorial), also clone those repositories to the same directory. **For example:**_


```bash
home
└── myusername
    └── Submitty
        └── GIT_CHECKOUT
            ├── AnalysisTools  (optional)
            ├── Lichen         (optional)
            ├── Localization   (optional)
            ├── RainbowGrades  (optional)
            ├── Submitty
            └── Tutorial       (optional)
```

_This host directory structure will be shared / synced between your host operating system and the Submitty virtual machine._

4. Start up Vagrant and provision the machine

```bash
  $ vagrant up --provider=qemu
```

You may be prompted to enter your login username and password to mount the SMB files.

After the machine is provisioned, it will start to run the Submitty installation. This process may take upwards of an hour or more depending on your internet speed, so ensure your computer does not go to sleep during it.

If the installation process fails due to network issues or is interrupted in some way, you may need to delete it and start over from the beginning with:


```bash
  $ vagrant destroy
   
  $ vagrant up --provider=qemu
```


5. After the ```vagrant up``` command completes, the VM is started and running in the background.

The VM will continue to run jobs in the background and consume a nontrivial amount of CPU resources, while completing a backlog of autograding for a dozen or more sample submissions for each of the more than 100 users in the sample courses.

If your development work will not require sample assignment submissions or autograding results, you may prepend ```NO_SUBMISSIONS=1``` to the previous command, which will skip the creation of these sample submissions and their autograding and decrease the time to complete installation.
```bash
  $ NO_SUBMISSIONS=1 vagrant up
```

6. When the install has completed, you should see the message:
 ```bash
 #####################################################################

                     INSTALLATION SUCCESS!
   
                        .GGQGGGSlu
                      .GGGGGGGGGGGS
                 :llUGGGGGGGGGGGGGGGG
                 'GGGGGGGGGGGGGGGGGGb        .
                    %GGGGGGGGGGGGGGG~   ..GSGGG
                       GGGGGGGGGGGGGGSGGGGGGGGGG[
                     ;GGGGGGGGGGGGp\ \ \GGGGGGGGL
                    !GGGGGGGGGGGGGGS\ \ \GGGGGG
                    GGGGGGGGGGGGGGGGG\ \ \9GGGG
                    %GGGGGGGGGGGGGGGS/ / /.GGG
                     %GGGGGGGGGGGGGS/ / /GGG
                      '%NNNNNNNNNNNNNNNNNN
#####################################################################
```

**NOTE:** There are times when the install will pause for a brief period with the message ```Done```. This does not mean the install has ended, and the install should continue after a bit of time.

_If you do not see this message due to an error or the installation has frozen, check out:_
* [Installation Troubleshooting](https://submitty.org/developer/troubleshooting/installation_troubleshooting)---
---

# How To Use Submitty Developer VM?
1. When the VM is “up”, you can go visit the homework submission website.

+ From a web browser (Chrome, Firefox, IE, etc.) on your host computer, go to:

http://localhost:1511/index.php

(see the VM login & password info below)

+ You can test the submission, autograding, and viewing of the grades details by uploading sample submissions from the Submitty repository, located in one of these these directories:

For the “tutorial” course: https://github.com/Submitty/Tutorial/tree/main/examples

For the “sample” course: https://github.com/Submitty/Submitty/tree/master/more_autograding_examples

2. When the VM is “up”, you can connect from your host computer to the virtual machine via ssh. Windows users will need to install SSH software (e.g., [WSL](https://ubuntu.com/desktop/wsl), or [Cygwin](https://www.cygwin.com/), or [Putty](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) ). From a terminal in the repository directory type:
```bash
 vagrant ssh
 ```
 You will connect to the VM as the ```root``` user.

 If ```vagrant ssh``` asks for a password for the root@127.0.0.1 user and “vagrant” without the quotation marks does not work, look at the vagrant ssh config file and make note of the hostname and port.

 ```bash 
  vagrant ssh-config
  ```

  Then directly ssh into the VM by

  ```bash
   ssh vagrant@hostname -p port
   ```
   If it asks for password, it should be “vagrant” and then
   ```bash
    sudo su
 ```

to login as the root user. You should then see you are logged in as root@vagrant.

3. The following users exist on the VM:

| user | password | role |
| -------- | -------- | -------- |
| superuser| superuser| Superuser|
| vagrant| vagrant| 	OS user|
| root| vagrant| OS user|
| submitty_cgi| submitty_cgi| 	Submitty process|
| submitty_php| 	submitty_php| Submitty process|
| submitty_daemon| submitty_daemon| Submitty process|
| postgres| postgres| database process|
| instructor| instructor| Instructor submitty user|
| ta| ta| Full access grader submitty user|
| grader| grader| Limited access grader submitty user|
| student| student| Student submitty user|

4. The VM has the following four courses by default and they are all part of the current semester:

+ tutorial
+ sample
+ development
+ blank

**Note:** The current semester is calculated by either using an ```s``` if in the month is < 7 else use ```f``` and then take the last two digits of the current year. So April 2017 would be ```s17``` while September 2017 would be ```f17```.

---------------------------------------***Starting and Stopping the Submitty VM***---------------------------------

1. When you take a break from Submitty development work, you can suspend the Submitty VM to save resources (CPU and battery) on your host machine.
```bash 
vagrant suspend
```
Alternatively, you can halt the virtual machine. This is a more complete shutdown and will take slightly longer to restart when you resume development work.

```bash
vagrant halt
```
2. To resume work on a VM that is suspended or halted:
```bash
vagrant up
```
NOTE: when resuming work, you may see this warning several times, ```default: Warning: Remote connection disconnect. Retrying..``` . These warnings are not harmful and can be ignored.

3. If you just want to restart the VM (same as ```halt``` then ```up```), type:
```bash
vagrant reload
```
4. Read the [Development Instructions](https://submitty.org/developer/development_instructions/index) page for instructions on updating an existing installation with recent code changes.
5. To completely delete the virtual machine, type:
```bash
vagrant destroy
```
And if desired (to start over from scratch with a fresh VM):
```bash
vagrant up
```

---------------------------------------------***Developing in HTTPS***------------------------------------------------

For developers who need to upgrade to HTTP/2 in their development environments, please follow the step below:
+ Run ```bash /usr/local/submitty/GIT_CHECKOUT/Submitty/.setup/dev-upgrade-h2.sh up```.
After a successful execution, please use ```https://``` instead of ```http://```.
+ To downgrade to HTTP/1.1, run
```bash /usr/local/submitty/GIT_CHECKOUT/Submitty/.setup/dev-upgrade-h2.sh down.```

The script should automatically handle the upgrading and issuing a self-signed certificate. If your browser complains about the security, please head to [WebSocket](https://submitty.org/developer/developing_the_php_site/websocket).


---


# How To Prepare Assignments 
------------------------------------------------***Create a New Gradeable***-----------------------------------------

To create a new gradeable, instructor users should click “New Gradeable” in the sidebar on the left of the screen. Fill out this form (details in the following sections) and press “Create New Gradeable”.

![Create New Gradeable](https://drive.google.com/file/d/1lxCetSrGklH4z67q-ueCSk-X8hqFb0mO/view?usp=sharing)

Most of the fields on the Create/Edit Gradeable form can be changed later. You can return to this form from the main page by pressing the “Edit” button at the end of the row for this gradeable. Two fields cannot be changed after the initial gradeable creation:

+ Unique id of the gradeable. This id should not have spaces. This id will be used as the name of the subdirectory storing files for this gradeable, etc. We suggest “hw01”, “lab_12”, “midterm”, “lecture_quiz_2a”, etc.

+ Type of gradeable (more details below)

![Filling Form For Gradeable](https://submitty.org/images/instructor/assignment_preparation/new_gradeable.png)

**IMPORTANT NOTE:** You should not change the TA Grading rubric after TAs have started grading!

-------------***Types of Gradeables***

A “Gradeable” is any single item that will be graded automatically and/or manually by the instructor/TAs.

***Checkpoints*** should be used for manually graded items with one or more parts that are marked as full, half, or no credit. This is commonly used for in class exercises (e.g., lab or recitation) where the TAs verify the students have completed the exercise and demonstrated sufficient competency with the material. Similar to numeric gradeables, we provide a spreadsheet-like interface for manual data entry.

***Numeric*** is used for data entry of quizzes or exams where the scores for each student are a simple array of one or more numbers (possibly with a short text comment). We provide a spreadsheet like interface for manual data entry of the numeric scores per problem that are summed for a total score. Or you can just enter the total score as a single column. We also provide a .csv upload if your numeric scores for this gradeable are already in an electronic format.

***Student File Submission*** There are two types of gradeables where students can submit their files to Submitty: directly uploading to the website, or submitting the files using a version control system (VCS). These should be used for homework/project/exercise where students will directly upload files (e.g., code, pdfs, images, etc.) to Submitty for grading. (For help with VCS gradeables, view Submission via Version Control ).

***TA/Instructor Upload*** should be used when a TA or instructor will (bulk) upload assignments to Submitty for grading.

-------------***Autograding Configuration Path***

For Electronic File uploads you must specify the full path to the autograding configuration config.json file stored on the submission server. (More details on the [Assignment Configuration](https://submitty.org/instructor/assignment_configuration/configuration_path) page.)

If you do not need any automated testing or grading; that is, if you are collecting files for the TAs to fully manually grade, choose the “no autograding” sample assignment configuration:

```bash
/usr/local/submitty/more_autograding_examples/upload_only/config/
```

--------------***Gradeable Dates & Times***

From the create/edit a gradeable page you can specify (or change):

+ The date/time the homework assignment should open to receive student submissions,

+ The due date/time (plus allowed “late day” submissions),

+ The date/time the system opens to allow TAs/mentors/graders to grade, and

+ The date/time that the TA/mentor grades will be released to students.

+ The date/time the student can start making grade inquiries, if grade inquiry is enabled for the gradeable.

----------------***Grading User Groups***

Grading permission denotes the lowest privileged user that may grade the gradeable. Grading permissions fall into 1 of 4 categories.

+ The highest privileged is “instructor” (1), who has unlimited access.

+ Then next highest is “full access grader” (2), who can view and edit grades for all sections.

+ The third highest is “limited access grader” (3), who can only see and grade sections or gradeables they are assigned to.

+ The least privileged user is “student” (4), who has no access to grading.

Both full access graders and limited access graders are assigned sections of students to grade for each gradeable (by registration or rotating section, see below). When these users log in to the system they are presented with only the grading work they have not yet completed. The difference between these grading types is that full access graders can navigate to inspect or regrade any student for that gradeable (even sections they are not assigned). Limited access graders are restricted.

------------------***Grader Assignment Method***

Grader Assignment Method can be chosen for each gradeable. It specifies the set of students assigned to each grader.

+ Gradeables set to All Access Grading (0) give all graders with sufficient permission access to all student submissions.

_***NOTE:*** This is the same as selecting “Rotating Section” and assigning all sections to every grader._

+ Gradeables set to Registration Section (1) assign a fixed set of one or more graders per section across all gradeables for the entire term. For example, grader John is assigned to registration sections 1 and 2. For gradeables “g1”, “g2”, “g3”, which are graded by registration section, John will always grade the same set of students, those assigned to registration section 1 and registration section 2.

_***NOTE:*** Typically registration section grading should be used for lab checkpoint data entry, and exam data entry._

+ Gradeables set to Rotating Section (2) allow for a grader to be assigned a different set of students per gradeable. For example, gradeables “g4”, “g5”, “g6”, are graded by rotating section. For “g4” John may be assigned to grade users in rotating section 3, for “g5” he may grade users in rotating sections 1 and 2, and for “g6” he may not be assigned any rotating sections.

_***NOTE:*** Often rotating section grading is useful for homework grading. This way each student receives feedback from multiple TAs throughout the term. It can even out differences in grading between the TAs and is perceived by the students as being more fair (you’re not always graded by the “mean” TA)._

--------------------------------------***Build/Debug all Grading Configurations***----------------------------------

After adding one or more new electronic submission gradeables, or if you have modified any of the assignment configuration files, the configuration must be built or re-built. If you are using a system-wide sample configuration or if you are using a configuration you uploaded through the website, the build will happen automatically after you create or edit the gradeable.

However, if you have specified a configuration in a private course repository, (see [Assignment Configuration](https://submitty.org/instructor/assignment_configuration/configuration_path) page) you must re-run the BUILD_XXXX.sh script.

1. Log in to the server. Navigate to your top level directory, e.g.
```bash 
/var/local/submitty/courses/s17/course_01/
```

2. Run the script, e.g.:
```bash
./BUILD_course_01.sh
```

_To re-build only a single gradeable, you can specify that gradeable id as an optional argument. E.g.:_

```bash
./BUILD_course_01.sh hw01
```
3. Fix any errors in your configurations, and re-run as necessary.

----------------------------------------***Test your Assignment Configurations***------------------------------------

1. To manually test your course and assignment configurations, follow the [student submission instructions](https://submitty.org/student/submission/)

2. You can confirm that the files were received by checking this directory:
```bash
/var/local/submitty/courses/SEMESTER/COURSE/submissions/ASSIGNMENT_ID/USER_ID/VERSION
```
For direct file upload submissions (single file or .zip) this directory will contain all submitted files and any directory structure contained in the zip file. In addition to the submitted files, in that directory you will also find a ```.submit.timestamp``` file. This is the time of submission, not the time that automated grading started/finished.

For submission via VCS, this directory will only contain the ```.submit.timestamp``` file. This timestamp will be used to retrieve the state of the repo at the time of submission.

3. For VCS submissions, you may also inspect an archive of the files/directory structure VCS repo at submission time in this directory:
```bash
/var/local/submitty/courses/SEMESTER/COURSE/checkout/ASSIGNMENT_ID/USER_ID/VERSION
```
4. Once automated testing & grading has completed, you can inspect some behind the scenes details of the automated grading by looking in this directory:

```bash
/var/local/submitty/courses/SEMESTER/COURSE/results/ASSIGNMENT_ID/USER_ID/VERSION
```
In this directory you will find the ```results_grade.txt``` summary of the automated grading results, and STDOUT, STDERR, execution logfiles for each test case. You will also find log files for the compilation, runner, and validation components of automated grading that are helpful in debugging assignment configurations.

---


# Creating a Team Assignment 

To create a team assignment, simply select “Yes” under the “Is this a team assignment?” section when creating a gradeable.

_***IMPORTANT NOTE:*** This option may only be specified when first creating a new gradeable. You may not edit an existing gradeable to switch it from individual to team or from team to individual._
![Team Assignment Creation](https://submitty.org/images/team_assignment_creation.png)

_Selecting “Yes” will open a new option for maximum team size, which refers to the maximum number of members that can join a team (including the student who creates it). Note that instructors can manage teams and override the maximum team size (see section “Creating and Editing Teams” below)._

-------------------***Team Lock Date***

Team gradeables have an additional options under the “Dates” tab to set the team lock date, which will be the last date students can create new teams or invite new members to existing teams.
![Team Lock Date](https://submitty.org/images/team_assignment_creation_2.png)

-------------------***Team Seeking***

Instructors have the option of enabling a team seeking message for students looking for a team or a partner. If enabled, the student can add a message that will be shown to other students viewing under ```Users Seeking Team/Partner```. Instructors can also set the instructions for the message.

![Seeking Team Partner](https://submitty.org/images/instructor_team_message.PNG)

----------------------***Creating And Editing Teams***

Instructors also have the option of forming and managing teams for the students.
![Creating Team By Instructor](https://submitty.org/images/team_grading_page.png)

Once you’ve made the team assignment, click on the “Preview Grading” button for that assignment. And then “Index of Grading Details for All Students”. For team formation you have 2 options:
* Prepare a common-separated values file (.csv) with 6 columns: First Name, Last Name, User ID, Team ID, Team Registration Section, and Team Rotating Section. The first row of the csv is assumed to be column headings and is ignored.

Press the ```Import Teams Members``` button in the upper right and upload the .csv file. Note: This option is only available if there are no teams for this gradeable.

* Press the pencil icon in the Edit Teams column for a student you’d like to put on a team to bring up the interface for editing a team
    + In the left “Team Member IDs” part, the grey blocks mean the existing members and the white blocks means the one you are going to add. You can use red “x” button to delete members. When adding members, there will be a drop-down list to help you search.

![Editing Team](https://submitty.org/images/team_edit_team_2.png)

+  Below is the “Team History” part. You can see the information of editing and adding history.

![team history](https://submitty.org/images/team_edit_team.png)

Once at least one team has been formed, you will have the option to Export Teams Members to download a csv with the current teams, which can then be imported into another team assignment.

![Export Team Members](https://submitty.org/images/team_export.png)
---



# How Autograding works And Its Directory Structure
-------------------------------------------------***Phases of Autograding***-------------------------------------------

***First Phase: Compilation***
1.  Create a temporary directory for autograding this student’s submission.
2. Create a temporary subdirectory for compilation.
3. Copy the student’s submitted source code (for compiled languages) to this temporary directory. Note: The copied files can be controlled with the ```submission_to_compilation``` variable in ```config.json```.
4. Copy the files from the ```provided_code``` directory into the temporary compilation subdirectory.
5. Scan through the testcases in the ```config.json``` for all testcases with type = “compilation”.
6. Execute the “command”(s) for the compilation testcases.
7. Rename the ```STDOUT.txt```, ```STDERR.txt```, execution logfiles, and specified output files that are to have been created by the program execution (prefix with test case number).

--
![compilation](https://submitty.org/images/files_for_compilation.png)

***Second Phase: Execution***
1. Create a temporary subdirectory for runner and validation work.
2. Copy the student’s submitted source code (for interpreted languages) to the ```tmp_work``` subdirectory. Note: The copied files can be controlled with the ```submission_to_runner``` variable in ```config.json```.
3. Copy the test input files to the ```tmp_work``` subdirectory.
4. Copy the compiled executables from the ```tmp_compilation``` subdirectory to the ```tmp_work``` subdirectory. Note: The copied files can be controlled with the ```compilation_to_runner``` variable in ```config.json```.
5. Scan through the testcases in the ```config.json``` for all testcases with type = “execution”.
6. Execute the “command”(s) for the execution testcases.
7. Rename the ```STDOUT.txt```, ```STDERR.txt```, execution logfiles, and specified output files that are to have been created by the program execution (prefix with test case number).
--
![execution](https://submitty.org/images/files_for_runner.png)

***Third Phase: Validation***

1. Copy specific files as needed from the student’s submission languages to the ```tmp_work``` subdirectory. Note: These files are specified with the ```submission_to_validation``` variable in ```config.json```.

2. Copy the custom validation code into the ```tmp_work``` subdirectory.

3. Copy the expected test output into the ```tmp_work``` subdirectory.

4. Copy output files from compilation from the ```tmp_compilation``` subdirectory to the ```tmp_work``` subdirectory. Note: The copied files can be controlled with the ```compilation_to_validation``` variable in ```config.json```.

5. Scan through the test cases in the ```config.json``` and perform the validation checks indicated within each check.

6. Calculate the score for each test case, and determine what messages and files should be displayed for each test case.

7. Write the ```results.json``` and ```grade.txt``` files.

8. Copy files as needed from the ```tmp_work``` directory for archive to the details subfolder of the student’s results directory for this assignment and submission version. Note: The copied files can be controlled with the ```work_to_details``` variable in ```config.json```.
--
![validation](https://submitty.org/images/files_for_validation.png)

-------------------***Variables to move files***

As outlined in the above sections & diagrams, there are 6 different configuration settings in the ```config.json``` to control the movement of files. Some of them have reasonable defaults for assignments that are compiling and running Python, C++, and Java programs (we may update these defaults in future revisions to Submitty). Each setting should be a list of one or more strings to match against files. You may use wildcards. Example of syntax:
```bash
 "autograding" : {
        "submission_to_compilation" : [ "part1/*.pdf" ],
        "submission_to_runner" : [ "part2/*.pdf", "special.xlsx" ],
        "compilation_to_runner" : [ "**/*.pdf" ],
        "submission_to_validation" : [ "part3/*.png" ],
        "compilation_to_validation" : [ "*/*.pdf" ],
        "work_to_details" : [ "*.pdf" ]
    },
```

_These file match patterns will be appended to the Submitty defaults, defined here: [grading/load_config_json.cpp](https://github.com/Submitty/Submitty/blob/main/grading/load_config_json.cpp)_

---------------------------------------------------***Directory Structure***--------------------------------------------

1. Login to the homework server. NOTE: If you are a developer/admin, you will need to switch user to the instructor user for that specific course, e.g.: ```sudo su smithj```
2. Go to the top level directory for that course. For example:
```bash
cd /var/local/submitty/courses/f16/csci1200/ 
```
_***NOTE:*** If your course is cross-listed (e.g., 4000 level for undergraduates 6000 level for graduate students), you probably want to choose just one course number for the directory. You can assign students to different registration sections to help organize the rainbow grades chart._
3. Here is an overview of the course directory structure:
```bash
 var
 └── local
     └── submitty
         └── courses
             └── {semesters}          [e.g., "f15", "s16", "f16", "s17", ... ]
                 └── {courses}        [e.g., "csci1100", "csci1200", ... ]
                     ├── BUILD_{course_name}.sh
                     ├── bin
                     |   └── {course assignments}   [e.g., "hw01", "hw02", ... ]
                     |       └── {autograder executables}
                     ├── config
                     |   └── {course and assignment json config files}
                     ├── reports
                     |   └── {course assignments}
                     |       └── {students}.txt     [e.g., "jonesa.txt" ]
                     |   └── {course assignments}
                     |       └── all_grades  
                     |           └── {students}_summary.json [ "jonesa_summary.json" ]
                     |       └── summary_html
                     |           └── {students}_summary.html [ "jonesa_summary.html" ]
                     |           └── {students}_message.json [ "jonesa_message.json" ]
                     ├── results
                     |   └── {course assignments}
                     |       └── {students}         [e.g., "jonesa", "smithj", ... ]
                     |           └── {versions}     [e.g., "1", "2", "3", ... ]
                     |               └── {grades, logs, myers-diff data}
                     ├── submissions
                     |   └── {course assignments}
                     |       └── {students}
                     |           └── user_assignment_settings.json
                     |           └── {versions}
                     |               └── {student code, readme, timestamp (if 1-part hw)}
                     |               └── {parts (if multi-part)}[e.g."part1","part2",...]
                     |                   └── {student code, readme, timestamp}
                     ├── test_code
                     |   └── {course assignments}
                     |       └── {instructor source code files used in assignment}
                     ├── test_input
                     |   └── {course assignments}
                     |       └── {input text files used for grading}
                     └── test_output
                         └── {course assignments}
                             └── {text files of expected outputs}
```

4. In your top level directory, you’ll find a script named ```BUILD_csci1200.sh`` (with your course name instead of ‘csci1200’). To run this script, type:
```bash
./BUILD_csci1200.sh 
```
This will rebuild the configurations for all gradeables. Alternatively, you may provide the names of one or more gradeables:
```bash
./BUILD_csci1200.sh hw01 hw05
```
This script must be re-run each time you add or edit a Gradeable, or modify the assignment configuration details in corresponding ```config.json``` or the associated assignment files.

If you are only using system-provided configurations (e.g., no_autograding) or you are uploading configurations through the web interface, the script will re-run automatically.

On the other hand, if you are writing custom configurations within a private repository (that is not accessible to the system ```submitty_daemon``` user, you will need to manually re-run this build script.

For more details, please see: [Assignment Configuration Details](https://submitty.org/instructor/autograding/structure)

_***NOTE:*** The script copies your homework configuration and testing files to the different subdirectories (```build```, ```test_code```, ```test_input```, ```test_output```, and ```config```), and compiles the ```config.h``` file with the core grading code located in [/usr/local/submitty/src/grading/](https://github.com/Submitty/Submitty/tree/main/grading) to produce executables (stored in the bin directory) that will be used for grading each submission. You should not manually edit any files in these directories because they will be overwritten the next time ```./BUILD_csci1200.sh``` is run._

5. When assignment submissions are received for this course, they are stored in the ```submissions``` directory (or the ```checkout``` directory for SVN repository submissions). All versions of all homeworks for all students are stored. The file ```user_assignments_settings.json``` stores the current “ACTIVE” assignment, and this history of changes to the “ACTIVE” assignment made by the student.

In a parallel directory hierarchy named ```results``` you can review the autograding results, and all associated log files (which are helpful for debugging).

Finally, in the ```reports``` directory you can review text files with the TA grades and per student overall course grade summaries.


---

# Autograding Specification (What Should Your ```config.json``` Should Contain )

Here Its Been Defined the structure of the "config.json" file.

_***Note:*** Allows C/C++ style comments (removed before compilation)._


------------------***Required Fields:***

+ ***testcases:*** An array of "testcase" objects (explained below).

+ ***notebook:*** An array of "notebook" objects ([refer to Submitty docs](https://submitty.org/instructor/assignment_configuration/notebook)).

+ ***assignment_message:*** A string displayed to students (default: "").

+ ***max_submission_size:*** Maximum combined file size for website submissions (default: 100,000 bytes).

Other Fields:

+ ***grading_parameters:*** Stores point totals for test cases (helpers, not used for grading).

+ ***part_names:*** Array of strings, names of assignment parts (default: empty).

+ ***resource_limits:*** Customizes resource limits for testing (default values exist).

+ ***allow_system_calls:*** Whitelist additional categories of allowed system calls.

-----------------***Testcase Specification:***

+ ***type:*** Defines the test type: "Compilation", "FileCheck", or "Execution" (default: "Execution").

+ ***title:*** Required string, the name of the test case.

+ ***details:*** Optional string, additional information about the test.

+ ***points:*** Point value of the test (default: 0).

+ ***hidden:*** Boolean flag, hides the test from students (default: false).

+ ***extra_credit:*** Boolean flag, marks the test as extra credit (default: false).

+ ***filename:*** Required for "file_check" and "execution", file(s) to be checked/executed.

+ ***executable_name:*** Required for "compilation", the compiled executable name(s).

+ ***command:*** Required for "compilation" and "execution", Linux commands for each phase.

+ ***resource_limits:*** Additional resource limits specific to this test case.

+ ***validation:*** Array of "validation" objects (specific checks performed).

+ ***actions:*** Array of strings, additional actions to perform during grading.

+ ***textboxes:*** Array of "textbox" objects (details not provided).

***Note:***

+ Some fields and values have default values as specified.
+ Certain fields require further documentation and are marked as "FIXME".

--------------------***Specification of a Validation Object***

Example Specification of a Validation Object:
```bash
{
  "method": "myersDiffbyLinebyWord",
  "description": "Checks if student code outputs the same words as expected",
  "actual_file": "STDOUT.txt",
  "expected_file": "expected_output.txt",
  "points": 2,
  "deduction": 0.5,  # Deduct half points for each failed check
  "show_message": "always",
  "show_actual": "on_failure",
  "show_expected": "on_failure",
  "failure_message": "Some words in your output differ from the expected output. Please review the highlighted differences."
}

```

+ ***method:*** (string) REQUIRED - This defines the type of validation check to perform. Valid options include various myersDiff methods for text comparisons, ImageDiff for image comparisons, JUnitTestGrader/MultipleJUnitTestGrader for JUnit tests, intComparison for numerical comparisons, and warnIf/errorIf for checking file presence/emptiness.
+ ***description:*** (string) - This provides an optional textual description of what the validation check does. Helpful for understanding the purpose of the check.
+ ***actual_file:*** (string or array of strings) REQUIRED - This specifies the file(s) containing the student's output. Wildcards like * are allowed for matching multiple files.
+ ***expected_file:*** (string or array of strings) - This specifies the file(s) containing the expected output for comparison. Can be null for some methods.

-----------------***Optional Fields:***

+ ***points:*** (float) - This assigns a point value to the validation check, contributing to the overall test case score.

+ ***deduction:*** (float) - This defines the point deduction when the check fails, usually a fraction of the assigned points.

+ ***show_message:*** (string) - Controls when to display the failure_message to the student: "always", "never", "on_success", or "on_failure".

+ ***show_actual:*** (string) - Controls when to display the student's output file: "always", "never", "on_success", or "on_failure".

+ ***show_expected:*** (string) - Controls when to display the expected output file: "always", "never", "on_success", or "on_failure".

+ ***show_difference_image:*** (string) - Controls when to display the image difference (applicable for ImageDiff): "always", "never", "on_success", or "on_failure".

+ ***acceptable_threshold:*** (float) - Defines the allowed deviation between actual and expected images for ImageDiff (range: 0-1).

+ ***failure_message:*** (string) - Provides a message to the student if the validation check fails.

---------------------***Validation Methods***

Methods only requiring "actual_file":

+ ```warnIfNotEmpty:``` Checks if the specified file exists and has content. If it does, throws a warning but doesn't fail the test. Useful for optional output files.

+ ```errorIfNotEmpty:``` Similar to warnIfNotEmpty, but failing the test instead of just throwing a warning. Use for mandatory empty files.

+ ```warnIfEmpty:``` Checks if the specified file exists and is empty. If empty, throws a warning but doesn't fail the test. Useful for checking if optional files are missing.

+ ```errorIfEmpty:``` Similar to warnIfEmpty, but failing the test instead of just throwing a warning. Use for mandatory non-empty files.

-----------------------***Methods requiring ```"actual_file" ```and ```"expected_file"```:

+ ```myersDiffbyLinebyWord:``` Compares text files line by line, highlighting differences in words. Useful for checking output format with specific wording.

+ ```myersDiffbyLineNoWhite:``` Similar to myersDiffbyLinebyWord but ignores whitespace differences. Use for checking code or content ignoring minor formatting variations.

+ ```myersDiffbyLine:``` Marks only different lines between files, not specific content changes. Useful for checking if specific lines are present even if their content varies.

+ ```myersDiffbyLinebyChar:``` Shows exact character differences between lines. Use for detailed content check requiring precise match.

+ ```myersDiffbyLinebyCharExtraStudentOutputOk:``` Similar to myersDiffbyLinebyChar but allows extra lines in the student's output that aren't in the expected output. Useful for checking code outputs with potential additions.

+ ```diffLineSwapOk:``` Allows lines to be in different order in the actual and expected files. Use for checking code that might generate output in different orders.

+ ```ImageDiff:``` Compares two images, highlighting areas of difference. Useful for visual outputs like graphs or drawings.

------------------***Methods for Java programs:***

+ ```JUnitTestGrader:``` Runs JUnit tests directly on the specified file, requiring the ```num_tests``` field with the expected number of tests. Use for testing Java programs with unit tests.

+ ```MultipleJUnitTestGrader:``` Runs JUnit tests programmatically on files within a folder using the provided TestRunner command. Requires num_tests as well. Use for testing multiple Java programs with unit tests.

---------------------***Additional methods:***

```intComparison```: Checks if a numerical value in the specified file meets a comparison (greater than, less than, etc.) with the provided term value.

```Custom Validation```: Allows writing Python scripts for complex checks with access to student and instructor files.
Remember, the configuration of these methods depends on your specific needs and the chosen validation method.

-----------------------------***Python Custom Validation***

***Overview:***

+ Custom Python scripts allow validation when default methods aren't suitable.
+ They take student/instructor files and information, analyze them, and produce a results object.
+ The results object contains the score and messages for the student.

------***Configuring a Python Custom Validator***

+ In ```config.json```, set ```method```to "custom_validator" for the desired validation object.

+ Specify the command to run the script (e.g., ```python3 grader.py```) and the ```actual_file``` to check.

+ Many other customization options from default methods apply.

+ Additional information can be provided via a ```custom_validator_input.json``` file.

```bash
{
    "title" : "Sum of 5 random numbers",
    "command" : "./a.out 5",
    "points" : 8,
    "validation" : [
        {
            "method" : "custom_validator",
            "command" : "python3 grader.py --numbers 5",
            "actual_file" : "STDOUT.txt"
        }
    ]
}
```

------***Storing a Python Custom Validator***
+ Custom validation code goes in a ```custom_validation_code``` directory within the assignment configuration.
```bash
   config
   ├── config.json                   
   ├── provided_code                 
   ├── test_input                    
   ├── test_output                   
   └── custom_validation_code        
       └── grader.py
```
-------***The Python Custom Validator’s Execution Environment***
+ Python code runs in a temporary directory (```tmp_work```), not the specific testcase directory.

+ Student and expected output are placed in subdirectories within this temporary directory.

```bash
 tmp_work
 ├── grader.py
 ├── test01      
 |   └── student_submission.cpp
 |   └── a.out
 |   └── STDOUT.txt
 |   └── STDERR.txt             
 ├── test_input  
 |   └── provided_file.txt                                  
 ├── test_output           
 |   └── expected_output_1.txt        
 └── custom_validation_code 
```
-----***Providing Input to a Python Custom Validator***

+ ```custom_validator_input.json```: This JSON file contains the validation object info and additional data.

+ Actual files produced by the student.

+ Expected files uploaded by the instructor.
+ Command-line arguments provided to the validator.

------***The Custom Validator Input JSON File***
```bash
{
    "title" : "Sum of 5 random numbers",
    "command" : "./a.out 5",
    "points" : 8,
    "validation" : [
        {
            "method" : "custom_validator",
            "command" : "python3 grader.py",
            "actual_file" : "STDOUT.txt",
            "expected_file" : "expected_output_1.txt",
            "foo" : 7,
            "bar" : 2
        }
    ]
}
```
When a python custom validator is run, it’s associated validation object in the assignment’s ```config.json``` is provided to it as a ```custom_validator_input.json```. This file is inflated with additional information that may be relevant to validation. In the above testcase, the following ```custom_validator_input.json``` might be created:

```bash{
    "method" : "custom_validator",
    "command" : "python3 grader.py",
    "actual_file" : "STDOUT.txt",
    "expected_file" : "expected_output_1.txt",
    "testcase_prefix" : "test01/",
    "username": "submitter_id",
    "foo" : 7,
    "bar" : 2
}
```
This file can be read in by ```grader.py``` to load the information necessary to perform the required validation. While the ```custom_validator_input.json``` can provide information about what actual and expected files are required for validation, it may also be leveraged to provide tuning parameters to the grader. In the above example, the variables ```foo``` and ```bar``` might have a special meaning to ```grader.py```. The ```custom_validator_input.json``` is further furnished with a ```testcase_prefix``` which corresponds to the current testcase and the ```username``` of the current submitter.

------***Opening Actual Files***

The actual file object within a ```custom_validator_input.json``` may be either a string or an array of strings. To open an actual file, the provided filename or filenames should be prepended with the ```testcase_prefix``` provided in the ```custom_validator_input.json```. So if the ```actual_file``` is ```STDOUT.txt``` and the ```testcase_prefix``` is ```test01/```, the file ```test01/STDOUT.txt``` should be opened.

------***Opening Expected Files***

The expected file object within a ```custom_validator_input.json``` may be either a string or an array of strings. Instructor provided expected files are housed in the ```test_output``` directory. So if an expected file is ```expected_output_1.txt```, the file ```test_output/expected_output_1.txt``` should be opened.


------***Python Custom Validator Output***
+ Write results to ```validation_results.json``` using either a ```success``` or ```failure``` object.

+ Deprecated: Writing to ```stdout``` is copied to ```validation_results.json```, but this will be removed in the future.

------***Success***
+ A single response message:

```bash
{
    "status" : "success",
    "data": {
        "score" : 1,
        "message" : "A success message",
        "status" : "success"
    }
}
```
+ Multiple messages:
```bash
{
    "status" : "success",
    "data": {
        "score" : 1,
        "message" : [
            {
                "message" : "A success message",
                "status" : "success"
            },
            {
                "message" : "Second message",
                "status" : "information"
            }
        ]
    }
}
```

+ Includes status ("success"), data with score (0-1), and message (string or list of message objects).

+ Message objects have message (text) and status (information, failure, warning, success).

------***Failure Response:***

+ Includes status ("failure") and message (debug string for instructors).

------***Debugging:***

+ Write debug statements to validation_logfile.txt or stderr for review.

_***Note:*** Don't use stdout for debugging as it's treated as results._

------***Additional Resources:***

+ Refer to the Python custom test case example, especially the grader.py file.