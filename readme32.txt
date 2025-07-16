CLAY'S UTILITIES FOR WIN32 - LAST UPDATED 17 JANUARY 2005 (Release 1.0 BETA 6)
==============================================================================

Note: Beta 4 adds HostCase.exe.
      Beta 5 adds DelayedStartup.exe and upgrades WinRun32.exe to version 1.1a
             (tells more and forces better).
      Beta 6 updates Seek32.exe (v3.1), Yank32.exe (v3.1), and Xchang32.exe (v4.1),
             all of which no longer try to process directories as files. Having been
             recompiled with a newer compiler, they're somewhat larger than before,
             but this overcomes command line parsing problems when the path to the
             utility contains a blank space (it was a compiler bug). Similar
             idiosyncracies may exist in other utilities in this package, but these
             most obvious ones are all I have time to chase down at the moment.
             If you encounter such behavior in any of my other utilities, please let
             me know; the fix will come sooner than if I have to find it myself.
             My contact information appears in section 2 below.

Contents:
1) License Agreement
2) Author Contact and Update Information
3) Package Overview
4) Batch Programming Tips and Unique Considerations for Win32 Utilities
5) Usage Instructions for Each Utility


1) License Agreement:

Upon extracting the contents of this package you agree to the following:

******************************************************************************
The utilities included herewith are Copyright (C)1992-2005 by Clay Ruth, who
reserves all rights of ownership.  You are granted a license to use this
software, free of charge, in a private or corporate environment, only for the
purposes described below.  You are also permitted to distribute this package,
IN ITS ENTIRETY, INCLUDING THIS NOTICE, WITHOUT CHARGE TO THE RECIPIENT other
than reimbursement for any media on which it is distributed and associated
shipping costs.

YOU MAY NOT distribute this package or any component thereof for profit,
nor may you disassemble, reverse-engineer, or extract code from this software.

All software in this package is considered a BETA RELEASE, even though the
individual program modules are not identified as such. The programs have been
subjected to cursory testing and are believed to perform as described.
However, some anomalies may be encountered, especially when using utilities
that probe into subdirectories. Use caution and watch for unexpected results.
Please inform the author (at the address given below) of any difficulties you
encounter with these programs.

All software in this package is certified to have been free of viruses, trojans
and other malicious code when originally distributed by the author.  You, the
user, are solely responsible for its usage, including, but not limited to,
scanning for malicious code that it may have acquired in the distribution
channels, determining its suitability for the intended use, backing up all
critical files before modifying them with it, and accepting any and all
consequences of its use.  NEITHER THE AUTHOR NOR THE DISTRIBUTOR IS RESPON-
SIBLE FOR ANY DAMAGES, DIRECT OR INDIRECT, CONSEQUENTIAL OR OTHERWISE,
RESULTING FROM YOUR DECISION TO USE THIS SOFTWARE.  YOU ACCEPT AND ASSUME
ALL RISKS OF USE.  NO WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, IS MADE WITH
REGARD TO THE MERCHANTABILITY OR FITNESS FOR USE OF THIS PACKAGE OR ANY
COMPONENT INCLUDED HEREWITH.

All trademarks referenced herein are the properties of their respective
owners. Their mention herein shall not be construed to infer any association
with, endorsement by, or critique by this package or its author.
******************************************************************************



2) Author Contact and Update Information:

Links to the current release of this package, and any available interim
releases of its components, can be found on the author's personal web site:

    http://clayruth.com/

This site also offers information about contacting the author.



3) Package Overview:

Some of these utilities are useful at the command line, while others make
sense only in the batch programming environment. If you've used my DOS batch
utilities, you'll recognize some of these as 32-bit versions with similar
functionality. The advantages of using the 32-bit version include long path
and file name support, in addition to operating in the native environment of
Windows(TM) 95 and later. As used in this document, the term "Win32" includes all
native 32-bit versions of Microsoft's Windows(TM) operating system. As of this
writing, these operating systems include all releases of Windows(TM) 95, 98, Me,
NT, 2000, XP, and Server 2003.

These programs were designed for use in the Win32 environment. A few of the
utilities that also have MS-DOS(TM) versions include the MS-DOS(TM) utility as a
stub program. If executed outside of the Win32 environment, they will not
report "This program must be run under Win32", but will instead run the DOS
version of the same utility. The DOS stubs, where provided, are identical to
the programs in my DOS batch utilities package; they are subject to the DOS
limitations regarding file name length, and their command line arguments
may vary slightly from those of the 32-bit utilities. The syntax display in
the non-Win32 environment is that of the DOS utility.

Although these programs have been tested, surprises are always possible.
I cannot claim to have tried every possible thing. Be sure to test the ones
you intend to use, in the specific environments in which you intend to use
them, and with arguments representative of what you intend to use, before
turning them loose in a production environment. Any file that is valuable
should be backed up before making changes, regardless of whether this
package or anything else makes the changes. Feel free to E-mail me with
any questions and suggestions you may have.



4) Batch Programming Tips and Unique Considerations for Win32 Utilities:

If you are not very familiar with batch programming practices, you should
search for other references on the subject and become familiar with the
general concepts before delving into this section. This document is not
intended to make you a skillful batch programmer; I just want to explain
a few things that might not be immediately obvious, even after you've
consulted other sources.

First and foremost, if you're accustomed to working with batches that run
MS-DOS(TM) utilities, you'll have to learn to do some things differently when
working with Win32 utilities. Although many of these utilities use the text-
based command console interface, making them look like DOS programs, others
are GUI-based, and all of them are in fact genuine 32-bit Windows(TM) programs.
Windows(TM) is a multitasking operating system: when you run a GUI program
from within a batch script, the script will not wait for it to finish its
task but will continue processing commands while the Windows(TM) program runs.
In a sequential batch process, this typically is not desirable because your
next command depends upon the previous command having finished its job.
It may also depend upon the returned errorlevel code.

To ensure that your batch script waits for each command to finish and receives
the intended errorlevel codes, always invoke the GUI-based Win32 utilities
with the START /W command. START is a Win32 console program that comes with
all 32-bit versions of Windows(TM). When invoked with the /W switch, it waits
for the launched Win32 program to terminate before it returns control to your
batch process, and it passes along the program's errorlevel code as well.

For example, one of the GUI utilities in this package is W32POPUP.EXE, which
produces a standard Windows(TM) message box containing the text, buttons, and
icon that you specify, and it returns an errorlevel code indicating which
button was clicked. If your batch invokes it without START /W, the batch
will not receive the errorlevel code but will continue processing as though
the returned code were zero, while the message box waits for a response.
The complete syntax for using W32POPUP in a batch script is as follows:

START %is_nt% /W W32POPUP "[prompt][^[x]chr,][...]" /C:"caption" [/I:I|E|Q|S|N]
                                                    [/B:OK|OKC|YN|YNC] [/D:2|3]

The %is_nt% environment variable macro accommodates differences in the way the
START command works in Windows(TM) NT, 2000, XP, and Server 2003 as compared to
Windows(TM) 95, 98, and Me. In the NT-based operating systems, START interprets
the first quoted argument as the desired title bar text. In NT, this happens
even if the quoted argument appears after the name of the program being started.
This doesn't happen in the 9x operating systems; in fact, they don't support
that "feature" at all. Windows(TM) 2000 and later support the feature in the
same manner as NT, but they don't have the misinterpretation problem for the
arguments that follow the launched program's name. The safest way to make your
batch script compatible with multiple platforms is to ensure that the IS_NT
variable is empty for Win9x and set to the quoted title text for WinNT and
its successors (2K/XP/2003). This can be accomplished in accordance with
the following example:

@echo off
set my_title="Desired Title Bar Text"
set is_nt=
if "%OS%"=="Windows_NT" set is_nt=%my_title%
start %is_nt% /w w32popup "Desired Message Text" /c:%my_title%
if errorlevel 3 echo You pressed "OK".
if not errorlevel 2 if errorlevel 1 echo You pressed "Cancel".
if not errorlevel 1 echo Syntax error -- check your script!
set is_nt=
set my_title=

Copy the above block of lines to a new file and save it as "PopupDemo.bat".
Then open a command prompt window, go to the location where you saved it,
and run PopupDemo at the command line to see a clear demonstration of this
particular utility's operation. Notice how the command prompt window's display
remains locked in place, without a cursor, while the pop-up message appears.
As soon as you click a button or press a corresponding key, the batch resumes
processing by telling you whether you OK'd it or canceled it. If you save
another copy of the batch, omitting everything from "start" through "/w"
so that the fifth line begins with "w32popup", you'll find that it doesn't
work as intended under Win9x and WinNT.

In the above example, the /I:, /B:, and /D: switches are omitted, so the
default "Information" icon appears with the OK and Cancel buttons, and the OK
button is the default (connected to the Enter key). Since the is_nt variable
is defined only for NT-based operating systems, in Win9x it has no effect
other than the insertion of an extra space (which is ignored) between the
Start command and its /W switch. For NT-based systems, it inserts the title
text in that position so that the desired message text will appear in the
pop-up message instead of being misinterpreted as a title bar string.

Win32 Console utilities don't need the START command. If you do use START
on a console utility, it will open a new console window to run the
command instead of running it in the batch script's window. This may not be
what you intended. Experiment with it and do what works best for you.

Handling Environment Variables in Batch Scripts:

The first 9 command line arguments passed to your batch script can be
referenced as %1 through %9. The command that ran your batch script can be
referenced as %0, so you can call batch subroutines, self-extractors, etc.,
without knowing where they reside, by putting them in the same directory
with the main batch and appending simple suffixes to their names. For
example, if your main batch is DOIT.BAT and is invoked by the command
D:\PATH\DOIT, you could name a batch subroutine DOIT_A.BAT and name a
self-extractor DOIT_B.EXE. Your batch can call the batch subroutine with all
required path qualification with CALL %0_A and it can run the self-extractor
with %0_B. The underscore is for readability only; you could omit it and use
%0A and %0B to run DOITA and DOITB. Notice that the command invoking DOIT
does not specify the .BAT extension; if it did, this trick wouldn't work.

Command line arguments beyond the 9th can be accessed by SHIFTing out the
ones you no longer need. Each SHIFT command moves all arguments to the
next lower number.

Environment variables, defined by SET commands, can be referenced by enclosing
them between percent signs. An example of this appears above in the W32POPUP
demonstration script.

Errorlevel tests return a true status if the error code returned by the
program is greater than, or equal to, the tested value. Errorlevels can
range from 0 through 255. The expression "if errorlevel 0" is useless
because it would always be true; test for zero with "if not errorlevel 1".
Due to the "greater or equal" behavior, the recommended approach is to test
the highest expected errorlevel first and either use a GOTO or invoke a
separate batch to direct execution elsewhere when the tested errorlevel is
present. Lower errorlevels can fall through to the subsequent lines that
either test for them or take default actions. However, sometimes it's better
to do something like "if errorlevel X if not errorlevel Y" to single out
errorlevels of at least X but less than Y. Be careful, though; this nested
"if" approach works well in Windows(TM) 9x, but it often malfunctions in the
NT-based operating systems. The safest approach is a single-condition test
with a GOTO.

You can use a double colon to prefix a comment. This is preferable over
the use of "REM" because REM statements are parsed for environment
variables, which takes time. The double colon makes the remark look like
a label, which is quickly skipped over. By using a double colon instead
of a single colon, we ensure that no conflict will arise with a real label.

Another time-waster can be the excessive use of GOTO, especially in lengthy
scripts. The impact of GOTO is small when the target label is near the top
of the script, but it becomes significant when the target is quite far down.
That's because the batch parser doesn't begin scanning at the current
execution point; it rereads the whole script from the beginning in search of
the target label. Therefore, when testing for errorlevel codes, try to use
GOTOs only when unexpected conditions occur. Let the most frequently followed
path go straight down the script with the fewest possible executions of GOTO,
especially at points farther down the script.

Windows(TM) NT has a peculiarity that appears to have been fixed in 2K and XP,
but be careful! To check whether a directory exists before trying to create
it, many batch programmers use "if not exist d:\path\nul md d:\path".
This works in most common operating systems because the "nul" device is
supposed to be defined only in directories that exist. However, in WinNT
the "nul" device always exists, regardless of whether the directory exists
or not. To avoid this problem, use my "makedir" utility, which is available
separately in "Clay's Utilities for DOS Batch Processes". Makedir attempts
to create the specified directory, and unlike Microsoft's MKDIR, its report
can be redirected to nul or to a log file, so the user won't see errors if
the directory already exists. Caveat: Makedir is a DOS utility, so it won't
handle long directory names. By the way, IBM's OS/2 has the same problem,
which reflects NT's origins.

Feel free to e-mail me with your batch programming questions. See the contact
information in section 2) above.



5) Usage Instructions for Each Utility:

Below is a summary, in alphabetical order, of each utility and its usage.
Most of what you see here can also be obtained by executing the desired
program, either without command line arguments or with the argument "/?".
Additional clarifications are mentioned here in a few cases.



Delayed Startup - A Win32 GUI Utility for Delayed Application Startup
=====================================================================

Delayed Startup - a Win32 Utility by Clay Ruth - 23 June 2004

This utility delays the automatic startup of any shortcuts that you place in the
Delayed Startup programs folder that you create (move them out of the regular
Startup folder). Put a shortcut to this Delayed Startup utility in the Startup
folder instead. The Delayed Startup shortcuts will begin launching 90 seconds after
this program starts, and an additional 30 seconds delay will occur between items.
These delays can be customized by creating file DelayedStartup.ini in the Windows(TM)
directory with the following structure:

[Delayed Startup]
InitialDelaySeconds=90
InterItemDelaySeconds=30

Change the 90 and 30 to suit your needs (integers only, range 1 - 214000).

On systems with user profiles (typically NT, 2K, XP, and Server 2003),
DelayedStartup looks for a "Delayed Startup" folder on the "Programs" menu of
both the currently logged-in user and "All Users". It launches the user's own
Delayed Startup shortcuts first, followed by those of "All Users". Therefore, a
system administrator can place a shortcut to DelayedStartup.exe in the All Users
Startup folder and move selected shortcuts from that folder to the All Users
Delayed Startup folder, thus causing those items to launch in delayed mode for
everyone. Users can create their own Delayed Startup folders and move selected
items there from their individual Startup folders.

Why delay startup items?

Some applications become unstable when their memory allocations become fragmented
by other applications starting up at the same time. Giving each application time to
start up individually, without interference from other startup applications, can
improve system stability overall. Also, rapid-starting Internet applications may
experience difficulty obtaining their preset personal firewall permissions, especially
on systems that boot straight to the desktop without prompting for a password. Such
applications benefit from having their startup delayed until after the firewall has
loaded and stabilized. Let the firewall load as quickly as possible, but move other
Internet application shortcuts from Startup to Delayed Startup.





FILDAT32 - A Win32 Console Utility
==================================

FILDAT32.EXE 1.0 - File Data Exporter - by Clay Ruth - 22 January 2003

FILDAT32 filespec [@screenlist] [/s] >filename.txt   (or >filename.csv)
FILDAT32 @filelist [/s] >filename.txt                (or >filename.csv)

Searches for standard Windows(TM) file information (StringFileInfo) and exports
the file name, date and time stamp, and any additional available information
in a Comma Separated Values (CSV) format suitable for importing into databases
and spreadsheets.  The first record contains field names.  When @screenlist
is used, only those found files whose fully qualified paths include an item
from the list are included in the output.  Screening criteria can be file names
with or without qualification, paths, or partial paths.  When @filelist is used
instead of filespec, each item in the list serves as a filespec and should be
fully qualified.  Switch /s enables subdirectory search and can become quite
time-consuming in the @filelist instance; use it judiciously.  If attempting to
combine @filelist and @screenlist, only the first item of @filelist applies.

Returns errorlevel 0 if info found, 1 if info not found, 2 if error.
When wildcards are used, highest level for the file group is returned.  Only a
filespec (alone or in @filelist) can use wildcards; @screenlist items cannot.

FILDAT32's output can be quite useful for finding redundant and conflicting
copies of shared library files such as DLLs and OCXes. For the convenience of
those who have Microsoft(TM) Access(TM), an empty Access data table and query are
provided. The provided file is in Access 2.0 format, so no matter what version
of Access you use, you can upgrade the file to the required level.

Here is an example of how data would be collected and imported into two
popular applications: Microsoft Access and Microsoft Excel(TM):

First, collect the data. If you're planning to import it into Access,
redirect its output to a file with extension .TXT; if importing into Excel,
use the .CSV extension. If you have multiple drives from which to collect
data, redirect each drive's data to a separate file. For example:

   fildat32 c:\*.* @extns.txt /s >dlldatac.txt    (or >dlldatac.csv)
   fildat32 d:\*.* @extns.txt /s >dlldatad.txt    (or >dlldatad.csv)
   fildat32 e:\*.* @extns.txt /s >dlldatae.txt    (or >dlldatae.csv)

In this example, the extns.txt file should contain a list of file extensions
of interest, such as the following (each entry should start at the left end
of the line; indentation here is used only to separate the example from this
narrative):

   .exe
   .dll
   .ocx
   .vxd

Next, open the application into which the data are to be imported.

If you are importing into Access 2.0:
    1. Open the provided FILEDATA.MDB database.
    2. Click File, Imp/Exp Setup. Select File Type: Windows(ANSI)
       Text Delimiter: "    Field Separator: ,          Date Order: MDY
       Date Delimiter: /    x Leading Zeros in Dates    x Four Digit Years
       Time Delimiter: :    Decimal Separator: .
    3. Click the "Save As" button and name it: CSV
    4. Click File, Import, Text (Delimited).
    5. Select a desired file (e.g., dlldatac.txt)
    6. x First Row Contains Field Names
    7. Click the "Options" button and select Specification Name: CSV
    8. Append to Table: FILEDATA
    9. Repeat steps 5 through 8 for each additional file to be imported.
   10. Close the Select File dialog box.

Access 97 and later: Click File, Get External Data, Import. The Wizard will
coach you through a series of options comparable to those described above.

Now select the Query tab and open the "Comparison" query. Notice how all of
your DLL files are listed in alphabetical order, so you can readily identify
duplicate and obsolete files. Wherever multiple files have the same name,
check the File Version (FVER) column to see which one is really the latest
version. That's the one that should be in your Windows SYSTEM subdirectory
(System32 in WinNT/2K/XP/2003), and all duplicate copies (especially prior
versions) should be archived and then deleted so as to free up disk space and
avoid conflicts. The primary exception to this is if you have multiple ver-
sions of Windows(TM), in which case those DLLs intended for use with different
Windows(TM) versions should be the appropriate versions for the Windows(TM) version;
e.g., don't move a Windows(TM) 98 DLL to a Windows(TM) XP SYSTEM32 subdirectory or
vice-versa.

If you are importing into Excel:
    1. Click File, Open.
    2. Select files of type: Text files, and select the first desired file
       (e.g., dlldatac.csv)
    3. Select Delimited, origin: Windows(ANSI).
    4. Select Start with Row: 1 if you want the headings to appear, else 2
    5. Click the "Next" button.
    6. Select only the Comma as the field delimiter, text qualifier: "
    7. DON'T treat consecutive delimiters as one.
    8. For the column formats, select: Text, Text, Date (MDY), General,
       General, General, and Text for all the rest.
    9. For any additional files select Insert, File, with the cursor on
       the row immediately below the last existing data line, column A.
       Import as described in steps 1 through 8, but start with Row 2.
Sort all data rows on Column A to get all names in alphabetical order.
Identify the duplicate and obsolete DLLs in accordance with the Access
example above.

Some applications will look only in their own directory for their custom
DLLs. Most applications will search Windows and its System subdirectory,
then the DOS search path, if a DLL isn't found in the application's own
directory. In versions of Windows(TM) prior to XP, once a particular DLL is in
memory, another application cannot open a different instance of that DLL; it
must use the image already in memory. If the first application already loaded
an obsolete version, a second application that needs the newer version will
crash. Therefore, the Windows System directory should contain the latest
version of each shared DLL and the obsolete versions should be deleted so that
they can't be loaded inadvertently.

Be sure to keep archived copies of all deleted DLLs so that you can put
them back if something doesn't work. In the vast majority of cases, having
the latest DLL (by file version, not date stamp) in the Windows SYSTEM(32)
subdirectory, and no DLLs of the same name in the application directories,
will resolve conflicts. However, don't mix up different Windows(TM) versions.
Also, DLLs that are found only in a single application's directory, and
nowhere else, should be left where they are.





FILVER32 - A Win32 Console Utility
==================================

FilVer32.EXE 1.0 - Win32 Console File Version Info - by Clay Ruth - 05 August 2002

FILVER32 filespec     (no wild cards)

Uses standard Windows(TM) API calls to read the Version Info resource; reports
the file version as a SET command that can be captured in a temporary batch and
then CALLed to put the version in environment variable F$V.

Returns errorlevel 0 if info found, 1 if info not found, 2 if error.






FSAPPEND - A Win32 Console Utility
==================================

FSAPPEND 1.0 - File Sharing Append for Win32 Console - by Clay Ruth - 25 May 2001

Syntax: FSAPPEND sourcefile targetfile

The content of the source file is appended to the existing content of the
target file with cooperative sharing and automatic retry. This is useful for
appending a log record from a workstation to a cumulative log on a server.
If two workstations try to append the cumulative log at the same time, one
will wait for the other to finish its append before performing its own.

Return codes: 0=success, 1=syntax, 2=source unreadable, 3=target unwriteable




FTCMP32 - A Win32 Console Utility
==================================

FTCMP32.EXE 1.0 - File Timestamp Comparison for Win32 Console - by Clay Ruth - 02 December 1999

FTCMP32 reffile target [/s | /n | /h | /d | /w | /m | /y]

Returns an errorlevel code equal to the number of time units by which refer-
ence file is newer than target file.  Optional parameter can specify units of
seconds, minutes, hours, days, weeks, months, or years (default = days).
Returns 255 if reference file is older than the target.  You can specify the
expected newer file first, then check for errorlevel 255 to see if your
expectation was incorrect.

The largest valid unit quantity returned by errorlevel is 250, but much larger
values may be displayed (will return errorlevel 251).

Errorlevel Summary:
250 or less = actual age difference      251 = Age difference > 250
252 = File is busy, permission denied    253 = Syntax error
254 = File not found or inaccessible     255 = Reference older than target




HostCase - A Win32 GUI Utility for Windows(TM) NT/2K/XP etc. ONLY (Not 9x)
======================================================================

HOSTCASE - Host Name Case Change for Windows(TM) NT - by Clay Ruth - 23 April 2004

Usage: HOSTCASE U | L

Edits registry setting HKLM\System\CurrentControlSet\Tcpip\Parameters\Hostname,
forcing any alphabetic characters to upper or lower case, as specified by the
argument U or L respectively; produces a pop-up message only in case of error.
Launch via START /W to make your process wait for completion; returns error
code 0 = success, 1 = failure, 2 = wrong operating system, 3 = syntax error.
Requires administrator or system privileges if a case change is to be made.




INPUT32 - A Win32 Console Utility
==================================

INPUT32.EXE 1.0 - Keyboard Input for Win32 Batch Files - by Clay Ruth - 12 January 2001

Syntax: INPUT32 "prompt" filespec

Displays the prompt, fetches a response, and writes the response to the
specified file (appends to the file if it already exists).  There is no
terminating carriage return; additional characters may be appended directly
to the response file. Any existing terminating CR/LF is removed.

Note that this behavior is slightly different (and more convenient) than that
of my DOS INPUT utility. The DOS version of this program doesn't remove the
existing CR/LF from the end of the file. If you need a CR/LF between the
file's existing content and that being appended, add an extra CR/LF by
using "echo.>>filespec" on the line preceding your INPUT32 command. 

If a terminating CR/LF is required following the text added by INPUT32,
use "echo.>>filespec" on the line following your INPUT32 command.

INPUT32 has many possibilities. Because it doesn't pass the carriage return,
you can concatenate many responses or other text to the initial response.
Because it doesn't overwrite an existing file, concatenation is the default.
If you need a new file, be sure to delete any existing file by that name
before invoking INPUT32.

Its input can be redirected from another file. This is a handy way of
creating strings of text that aren't return-terminated. For example, when
your batch says "echo string>file1", you actually get "string" + CRLF in the
file. To get just "string" without the CRLF, do this after the echo:

   input32 "" file2 <file1

INPUT32 is also very nice for getting input for other commands that don't
behave as you would like. For example, suppose another command displays a
nice prompt and requests input, but then messes up the screen with a report
of what it's doing with the input. If you redirect its output to nul or to
a file, you lose the nice prompt and the user can't see what she's typing.
Here's the solution:

   if exist file1 del file1
   input32 "nice prompt" file1
   echo.>>file1
   messycommand <file1 >nul

This lets the user see the nice prompt (which you can now tailor to your
liking), and it lets the messy command receive the intended input without
messing up the screen.

Another great use of INPUT32 is to set environment variables with input typed
by the user. Here's how:

   echo set envar=>temp.bat
   input32 "Please enter the desired setting:" temp.bat
   echo.>>temp.bat
   call temp.bat
   del temp.bat

You now have an environment variable named ENVAR that contains the string
typed by the user. You can use SCANSTR (from my DOS utilities package,
available separately) to scan the retrieved variable for desired content,
or you can use the input as-is by referencing %ENVAR% in your script.





INTADD32 - A Win32 Console Utility with DOS Utility Stub
========================================================

INTADD32.EXE 2.0 - Integer Addition for Batch Files - by Clay Ruth - 27 December 2001

Syntax: INTADD32 [x][-]val1 [x][-]val2

val1 and val2 can be decimal or hexadecimal ("x" prefixed) signed integers
that add up to 32,767 or less, positive or negative (hex range 0000 to FFFF,
where 8000 through FFFF are considered negative).

Returns a SET RESULT= command that can be captured by redirection to a batch
that can then be CALLed to get the result into the environment.
If val1 is hex, RESULT is a four-digit hexadecimal value;
otherwise, RESULT is a non-padded decimal value.
Hexadecimal results DO NOT have the "x" prefix nor a "-" sign.

Returns errorlevel 0 for positive result, 1 for negative, 2 for error.

If executed outside of the Win32 environment, the DOS stub INTADD 1.1
runs instead.

Examples:

   INTADD32 xc800 x0400    produces   SET RESULT=CC00     with errorlevel 1

   INTADD32 xc800 -1       produces   SET RESULT=C7FF     with errorlevel 1

   INTADD32 xc800 x-c000   produces   SET RESULT=0800     with errorlevel 0

   INTADD32 x1000 4096     produces   SET RESULT=2000     with errorlevel 0

   INTADD32 4096 x1000     produces   SET RESULT=8192     with errorlevel 0

   INTADD32 x7000 8192     produces   SET RESULT=9000     with errorlevel 1

   INTADD32 8192 x7000     produces   SET RESULT=-28672   with errorlevel 1


Batch Example: Note that the "set v1" used here might be replaced with a
               batch-generated setting produced by reading a configuration
               file in a real situation.

   @echo off
   set v1=c800
   intadd32 x%v1% x03FF >temp.bat
   call temp.bat
   del temp.bat

   :: You now have a RESULT environment variable that contains CC00.
   :: Continuing:

   set test1=N
   set test2=N
   intadd32 x%result% x1000 >nul
   if errorlevel 1 set test1=Y
   intadd32 x%v1% x-c800 >nul
   if not errorlevel 1 set test2=Y
   if not %test1%%test2%==YY goto end
   echo.
   echo Values are within the upper memory range; setting EMM exclusion:
   xchang32 /i c:\config.sys "emm386.exe" "emm386.exe x=%v1%-%result%" >nul

   :end
   echo.




INVNTR32 - A Win32 Console Utility
==================================

INVNTR32.EXE 1.2 - Long Name File Inventory - by Clay Ruth - 09 September 2002

INVNTR32 d:path\seek1 [seek2 [seek3 [...]]] [ /X ] [ /T ] [ /R ]

Lists all filenames that include the seek strings, in the specified
directory and all of its subdirectories, both normal and hidden.
Shows fully qualified file names with modification dates, times and sizes.
Switch /X excludes all subdirectories; /T adds total bytes listed;
/R reverses position (file path comes last, without compaction).
The first seekstring, if it contains wildcards, will serve as a first level
filter; only files that satisfy the seek1 specification (and contain any
one or more of seek2 through seekN, if specified) will be reported. However,
when seek1 does NOT contain any wildcards, there is no first-level filtering;
seek1 is then equal in function to any other seek string.  Seek strings
seek2 through seekN may NOT contain wild cards.

Returns errorlevel: 1 = nothing found, 2 = insufficient memory, 3 = bad path.




LONGADD32 - A Win32 Console Utility with DOS Utility Stub
=========================================================

LONGADD.EXE 2.0 - LongInt Addition for Batch Files - by Clay Ruth - 02 May 2002

Syntax: LONGAD32 [x][-]val1 [x][-]val2

val1 and val2 can be decimal or hexadecimal ("x" prefixed) signed integers
that add up to 2 billion or less, + or - (hex 00000000 to FFFFFFFF, where
80000000 through FFFFFFFF are considered negative).

Returns a SET RESULT= command that can be captured by redirection to a batch
that can then be CALLed to get the result into the environment.
If val1 is hex, RESULT is an eight-digit hexadecimal value;
otherwise, RESULT is a non-padded decimal value.
Hexadecimal results DO NOT have the "x" prefix nor a "-" sign.

Returns errorlevel 0 for positive result, 1 for negative, 2 for error.

If executed outside of the Win32 environment, the DOS version of LONGADD 2.0
runs instead.




RANDOM - A Win32 Console Random Number Generator with DOS Utility Stub
======================================================================

RANDOM 1.0 - Random Number Environment Variable - by Clay Ruth - 08 May 2001

Syntax:   RANDOM [maxval [variable]] [/z]

Output is a statement of the form "SET variable=value", which you can re-
direct to a temporary batch file; call the temporary batch to set the variable.

The default value of maxval is 100; any positive integer from 1 to 32767 may
be specified. The default variable is RAND. If you are specifying a different
variable, you must first specify maxval. The value returned in the
variable is an integer from 1 to maxval inclusive, unless the /z option is
specified. Switch /z specifies "zero-based", which returns an integer from
zero to (maxval - 1), inclusive. Values up to 253 are also returned as
errorlevel codes; higher values return errorlevel 254; error returns 255.

This utility executes identically in both the MS-DOS and Win32 environments.
It executes as a 16-bit program in the 16-bit world and as 32-bit in Win32.




REGREAD - A Win32 Console Utility
=================================

RegRead 1.0 - Registry Reader for Win32 Console - by Clay Ruth - 03 April 2001

Syntax:  REGREAD keyname variable [export_variable]

Example: REGREAD "HKLM\Software\Microsoft\Windows\Help" "Dao35.hlp"
         reads reg variable "Dao35.hlp" into environment variable "DAO35_HLP"

Generated output is a statement of the form "SET export_variable=value", where
"export_variable" defaults to the registry variable (with underscore instead
of blank or dot), unless a different export variable is specified. You can
redirect output to a temporary batch file and then call it to get the setting
into your batch process environment.

To read the "(Default)" variable, specify a null variable (""). The returned
SET command will use environment variable "DEFAULT" unless otherwise specified.

Return codes: 0 = success, 1 = variable not found or unsupported type,
      2 = key not found, 3 = syntax error, 4 = memory allocation error




SEEK32 - A Win32 Console Utility with DOS Utility Stub
======================================================

SEEK32.EXE 3.1 - String Search by Clay Ruth - 32-bit Console Version - 17 January 2005

SEEK32 [/i][/s] filespec "[srchstrng][^[x]chr,][...]"
SEEK32 [/i][/s] filespec @scriptfil

Switch /i = ignore case; /s = simple strings (use ^ and , as-is); chr = ASCII.
Search string can be any combination of literals and ^-prefixed, comma-
delimited ASCII codes (^0 through ^255 or ^x00 through ^xFF).  Double-up a
literal ^ or , (or use /s).  Can omit quotes if no spaces are in the string.
Can read the search string from a script file of up to 65,534 chars max.

Echos "Found" or "Not Found" to visually indicate outcome.
Returns errorlevel 0 if not found or error; 1 if found.
When wildcards are used, highest level for the file group is returned.

If executed outside of the Win32 environment, the DOS stub SEEK 2.0
runs instead.




SPLIT32 - A Win32 Console Utility
=================================

SPLIT32.EXE 1.0 - Win32 Console File Splitter by Clay Ruth - 09 February 2001

SPLIT32 file1 file2 [/i] ["[srchstrng][^[x]chr,][...]"] | [@scriptfile]    or
SPLIT32 file1 file2 /SIZE=[x]size[k]    (use x for hexadecimal, k for kbytes)

Searches file1 for a string and truncates it just prior to first occurrence
other than start of file; rest is appended to file2; unchanged if not found.
Alternate form reduces file1 to the specified size, putting rest in file2.

Long search string descriptions can be placed in a script (8,192 char. max.)
Switch /i = ignore case (can be in any parameter position); chr = ASCII.
Search string can be any combination of literals and ^-prefixed, comma-
delimited ASCII codes (^0 through ^255 or ^x00 through ^xFF).
Use double ^ or , for literal ^ or , character within a string.
Quotes may be omitted if there are no spaces within the search string.

Echos "Split" or "Not Split" to visually indicate outcome.
Returns errorlevel: 0 = split, 1 = not split, 2 = unable to write,
3 = syntax or memory error,  4 = file not found, 5 = error during read/write,
6 = insufficient disk space, 7 = read-only file, 8 = write-protected.




TIMESTMP - A Win32 Console Utility with DOS Utility Stub
========================================================

TIMESTMP.EXE - Time Stamp Lookup for 32-bit console - by Clay Ruth - 27 December 2001

TIMESTMP filespec        (no wildcards)

Returns text string "File filespec is dated Day Mon(th) dd hh:mm:ss yyyy"
If no filespec is given, the program returns its own time stamp.

Returns errorlevel 0 on success, 1 on failure, 3 on syntax display

If executed outside of the Win32 environment, a DOS version of TIMESTMP
runs instead, subject to the usual DOS filespec limitations.




TPAUSE32 - A Win32 GUI Utility for Batch Timing and Messaging
=============================================================

TPAUSE32.EXE - Timed Pause with Optional Pop-up - by Clay Ruth - 16 March 2000

TPAUSE32 [/T:[[hh:]mm:]ss] [/N] [/C:"caption"] ["[prompt][^[x]chr,][...]"]

The prompt string can be any combination of literals and ^-prefixed,
comma-delimited ASCII codes (^1 through ^255 or ^x01 through ^xFF).
Default timeout is 1 minute. If no prompt is specified, no window appears.
A keystroke or click in the pop-up window stops it, unless /N is specified.
Call with START /W to make your process wait for termination. No return code.




W32POPUP - A Win32 GUI Utility for Batch User Prompting
=======================================================

W32POPUP.EXE - Programmable Pop-up Window - by Clay Ruth - 22 May 2001

START %is_nt% /W W32POPUP "[prompt][^[x]chr,][...]" /C:"caption" [/I:I|E|Q|S|N]
                                                    [/B:OK|OKC|YN|YNC] [/D:2|3]

The prompt string can be any combination of literals and ^-prefixed,
comma-delimited ASCII codes (^1 through ^255 or ^x01 through ^xFF).
The default icon is Information; use /I: to specify Information,
Exclamation, Question, Stop, or None.
The default pushbutton combination is OK/Cancel; use /B: to specify
OK (alone), OK/Cancel, Yes/No, or Yes/No/Cancel. The default pushbutton
(the one connected to the Enter key) is the first button (OK or Yes).
Use /D: to specify the second or third button (as applicable) as the default.
Call with START /W to receive the response as an errorlevel code:
0 = Error, 1 = Cancel, 2 = No, 3 = Yes/OK
See "4) Batch Programming Tips and Unique Considerations for Win32 Utilities"
above for an explanation of the %is_nt% environment variable reference.




WINRUN32 - A Win32 GUI Utility for Batch Application Control
============================================================

WinRun32.EXE 1.1a - Window finder with optional control - by Clay Ruth - 24 May 2004


WinRun32 [/Q] [/K] [/F] [/T] [/S] "WindowName1" [... "WindowNameN"]

/Q = quiet (no pop-up message unless a syntax error occurs)
/K = Kill (close window if found)    /F = Force Kill (only if /K fails)
/T = bring window to Top        /S = Substring match (default is whole title)
To report all windows, use wild window name "*" (disables all switches).
You'll be surprised at the number of hidden windows it finds.
Call with START /W to receive errorlevel codes:
0 = matching window not found    1 = window found    2 = kill ordered
When processing multiple named windows, the highest applicable code returns.

Use WinRun32 with the /Q switch (and optionally /K) within your batch script
to find out if a particular program is running (and optionally to shut it down).
Test it on the target program before deploying it in your network environment.
The /F switch uses a different method than /K for closing the program, but
it too doesn't always work. Some programs try very hard not to be closed.

Version 1.1a incorporates additional information into the Window Found display,
including the name of the executable module that owns the window and its associated
process and thread identifiers. The API calls that allow tracing a window back to
the executable module name exist only on the Windows(TM) NT-based operating systems;
therefore, on Windows(TM) 9x it will report that the module name is unavailable
on this version of Windows(TM). Even on an NT-based OS, certain restricted-access
system processes may deny access to such information, in which case it reports
"Unavailable; access to process denied."




WNAMES32 - A Win32 GUI Utility
==============================

WNAMES32.EXE - Window Names Display - by Clay Ruth - 25 June 1999

This program takes no arguments. It is the predecessor of WinRun32.
Its only purpose is to see the names of all running top-level windows.
Running it (as simple as a double-click) produces a subset of the display
provided by "WinRun32 *" (same pop-ups, but less information within them).
No errorlevel code is returned.




XCHANG32 - A Win32 Console Utility with DOS Utility Stub
========================================================

XCHANG32.EXE 4.1 - Binary Edit by Clay Ruth - 32-bit Console Version 17 January 2005

XCHANG32 [/i][/s] fs "[srchstrng][^[x]chr,][...]" ["[replstrng][^[x]chr,][...]"]
XCHANG32 [/i][/s] fs @scriptfil   or   XCHANG32 [/i][/s] fs "srchspec" @imagefil

Switch /i = ignore case; /s = simple strings (use ^ and , as-is); chr = ASCII.
Search and replace strings can be any combination of literals and ^-prefixed,
comma-delimited ASCII codes (^0 through ^255 or ^x00 through ^xFF).  Double-up
a literal ^ or , (or use /s).  Quotes may be omitted if there are no spaces
within strings.  Script file has quoted search and replace string specs on one
line; script or image file 65,534 chars max; search string 4,095 chars max.
ZIP file embedded path change must include highest embedded directory level,
and each altered ZIP file must be fixed with PKWare's PKZIPFIX before use.
For ZIP modification to work, the first zipped file must have the sought path.

Echos "Changed" or "Not Changed" to visually indicate outcome.
Returns errorlevel: 0 = changed, 1 = not changed, 2 = unable to rewrite/del,
3 = syntax or memory error,  4 = file not found, 5 = error during read/write,
6 = insufficient disk space, 7 = read-only file, 8 = write-protected.
When wildcards are used, highest level for the file group is returned.

If executed outside of the Win32 environment, the DOS stub XCHANGE 3.1
runs instead.




YANK32 - A Win32 Console Utility with DOS Utility Stub
========================================================

YANK32.EXE 3.0 - Win32 Console Text Line Remove/Replace by Clay Ruth - 23 March 2004

YANK32 [/i][/s] fs "[srchstrng][^[x]chr,][...]" ["[replstrng][^[x]chr,][...]"]
YANK32 [/i][/s] fs @scriptfil   or   YANK32 [/i][/s] fs "srchspec" @imagefil

Switch /i = ignore case; /s = simple strings (use ^ and , as-is); chr = ASCII.
Search and replace strings can be any combination of literals and ^-prefixed,
comma-delimited ASCII codes (^0 through ^255 or ^x00 through ^xFF).  Double-up
a literal ^ or , (or use /s).  Quotes may be omitted if there are no spaces
within strings.  Script file has quoted search and replace string specs on one
line; script or image file 65,534 chars max; search string 4,095 chars max.
Each ENTIRE LINE containing sought text will be removed/replaced, CRLF and all.
If replacement is to be separated from subsequent line, end it with ^13,^10.

Echos "Yanked" or "Not Yanked" to visually indicate outcome.
Returns errorlevel: 0 = yanked, 1 = not yanked,  2 = unable to rewrite/del,
3 = syntax or memory error,  4 = file not found, 5 = error during read/write,
6 = insufficient disk space, 7 = read-only file, 8 = write-protected.
When wildcards are used, highest level for the file group is returned.

If executed outside of the Win32 environment, the DOS stub YANK 2.1
runs instead.

==========================================================================
End of Documentation
