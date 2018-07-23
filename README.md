Hands-on analysis of DArTseq data for linkage mapping
====================================================

__Instructor__  
Jessen V. Bredeson  
University of California, Berkeley, CA, USA  
<jessenbredeson@berkeley.edu>

__Co-Instructor__  
Jessica B. Lyons  
University of California, Berkeley, CA, USA  
<jblyons@berkeley.edu>

---

__Table of Contents__

   * [Welcome!](#welcome)
      * [About the course](#about-the-course)
      * [Course Topics](#course-topics)
      * [Important notes](#important-notes)
      * [Agenda](#agenda)
   * [Using UNIX](#using-unix)
   * [Logging into the AWS server](#logging-into-the-aws-server)
      * [Mac/Linux users](#mac-linux-users)
      * [Windows users](#windows-users)
   * [UNIX and HTS/DArT files](#unix-and-hts-dart-files)

---

# Welcome!
## About the course
The **Hands-on analysis of DArTseq data for linkage mapping** course aims to teach biologists how to perform their own genetic linkage map analysis. This course uses DArTseq data, but the analysis techniques taught here are generally applicable to other forms of GBS/reduced-representation genotyping datasets.

This course leverages the powerful tools freely available with the UNIX operating system (OS). In addition to using UNIX, students of the course will be introduced to utility programs written by the above instructors, and the [R statistical programming language](https://www.r-project.org). 


## Course Topics
* Reduced representation nextgen sequencing-based genotyping
* DArTseq data and high throughput sequencing file formats
* Working with genotyping data in the context of a genome assembly
* Selecting high-quality segregating markers
* Filtering individuals by evaluating relatedness
* Linkage mapping with R/OneMap

## Important notes
* **Workshop participants must bring their own laptops**. Computation will be performed on Amazon web servers set up for the workshop and on the students' own laptops.

* **Participants are expected to be familiar with the UNIX environment**. Please read the next section for important information.


## Agenda
Please see the PDF [herein](https://github.com/bredeson/HandsOnDArT/blob/master/HandsOnDArT-Agenda.pdf) for the course agenda.


# Using UNIX
As noted above, this course will rely heavily on use of the UNIX operating system, a powerful text-based computing environment; therefore, students will be expected to be proficient with some flavor of UNIX (BSD, SunOS, Linux, *etc*.). For convenience, this course will take advantage of Amazon Web Services (AWS) Elastic Computing 2 (EC2) infrastructure to allow all students access to an UNIX system to learn in. Students requiring an introduction to (or refresher of) UNIX should visit the following [PFB2017](https://github.com/bredeson/pfb2017/blob/master/README.md#ok-ive-logged-in--what-now) course website and complete the **UNIX 1** exercises. 

> NOTE: In the PFB2017 course website URL previously diseminated via email, a broken link prevented students from accessing the UNIX 1 exercises. This has been fixed at the updated PFB2017 URL above.

> NOTE: The PFB2017 course used the native Apple Mac OSX operating system, so please start at the section titled "OK. I've Logged in. What Now?" and ignore any prior sections. Instructions for logging into the AWS server are included below.


# Logging into the AWS server

Connecting to the AWS server requires a user-specific key file; **this file is your password, do not share it with others**. User key files will be sent to you individually in a separate email. 
Please download your key file and save it to your home/user directory on your laptop.

When following the instructions below, replace *familyname* with your family name (all lower-case characters, no spaces); *e.g.*, for Jessen Bredeson, *familyname*.pem would become bredeson.pem


## Mac/Linux users:

1. Locate and open the Terminal app.
    1. If you are using a Mac, hold *command* and press the *spacebar* to open up Spotlight. Then, Type "Terminal" into the Spotlight Search pane and press *Enter*.
    2. If on a Linux sytem, searching "How do I open the command line on *X*" -- where *X* is distibution type (*e.g.*, Ubuntu, Debian, Mint, *etc.*) -- in Google will often provide step-by-step instructions on how to do this for your specific operating system.

2. Change permissions on the key file (performed only once, before you login for the first time):
   ```sh
   chmod 0400 ~/familyname.pem
   ```

3. In the Terminal, connect to the AWS server using SSH. You will not be asked for a password and should be connected straightaway:
   ```sh
   ssh -i ~/familyname.pem familyname@ec2-13-57-194-80.us-west-1.compute.amazonaws.com
   ```

> NOTE: you may receive a warning message that the authenticity of the host cannot be established. Please continue connecting by typing "yes" and pressing the Enter/Return key.


## Windows users:

Windows users will first need to install the PuTTY app in order to connect to the AWS server. Please go [here] (https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) to download the *putty-0.70-installer.msi* installer image (download the 32-bit version unless you are sure you have a 64-bit processor and 64-bit Windows). The installer will be saved to your Downloads folder; double-click it and follow the install prompt.

> NOTE: If you have questions relating to PuTTY, please see the extensive [FAQ](https://www.chiark.greenend.org.uk/~sgtatham/putty/faq.html).

Steps 1–6 only need to be performed once for each key file. If you already have performed these steps, proceed to step 7:
1. PuTTY requires the key file in a different format. On your laptop, go to *Start* => *All Programs* => *PuTTY* => *PuTTYgen* to open the PuTTYgen app to convert the `.pem` key file to a `.ppk` key file.

2. Under *Parameters* section at the bottom of the PuTTYgen application window, select "RSA" as the type of key to generate

3. In the *Number of bits in a generated key* field, type 4096 and press the *Load* button.

4. By default, PuTTYgen displays only files with the extension `.ppk`. To locate your `.pem` file: 
   1) Select the "All Files (\*.\*)" option from the drop-down menu above the *Open* button to display files of all types.
   2) Find and select your *familyname*.pem file using the file explorer window and click *Open*. 
   3) Choose *OK* to dismiss the confirmation dialog box.

5. Next, click the *Save private key* button and click *yes* in the confirmation box.

6. In the *File name* field, type your *familyname*.ppk and click *Save*. Quit PuTTYgen.

7. To open PuTTY, do *Start* => *All Programs* => *PuTTY* => *PuTTY*.

8. In the *Category* pane, choose *Session* and complete the following fields:
   1) In *Host Name* box, enter ```familyname@ec2-13-57-194-80.us-west-1.compute.amazonaws.com```
   2) Under *Connection type*, select "SSH". 
   3) Ensure that *Port* field is 22.

9. In the *Category* pane, expand *Connection*, expand *SSH*, and then select *Auth*. Complete the following: 
   1) Select *Browse*. Find and select the `.ppk` key file that you generated, and then choose *Open*. 
   2) Select *Session* in the *Category* tree, enter a name for the session in *Saved Sessions*, and click *Save*.
   
10. Click the *Open* button to start the PuTTY session


# Lecture Notes
For lecture notes, please see the PDFs attached below, organized by lecture.

## UNIX and HTS/DArT files
[Lecture notes and exercises](https://github.com/bredeson/HandsOnDArT/blob/master/slides/UNIX+HTS+DArT.pdf)
