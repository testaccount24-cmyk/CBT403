# CBT403
Converted to GitHub via [cbt2git](https://github.com/wizardofzos/cbt2git)

This is still a work in progress. GitHub repos will be deleted and created during this period...

```
//***FILE 403 is from Ugur Cilesiz, and contains a very simple      *   FILE 403
//*           system to display IBM Messages (and other FB-80       *   FILE 403
//*           help-type information) on your screen, instantly.     *   FILE 403
//*                                                                 *   FILE 403
//*           This system consists of a REXX exec (two versions     *   FILE 403
//*           available), one panel, and an FB-80 format            *   FILE 403
//*           partitioned dataset, which contains the message       *   FILE 403
//*           text, for each message.  You set up the partitioned   *   FILE 403
//*           dataset, with the message members you want to         *   FILE 403
//*           include.                                              *   FILE 403
//*                                                                 *   FILE 403
//*           Instructions are included here for obtaining the IBM  *   FILE 403
//*           message members, from Bookmanager installed under     *   FILE 403
//*           MVS.  Of course, if you have another vendor's         *   FILE 403
//*           documentation also installed under Bookmanager in     *   FILE 403
//*           MVS, you can easily extract and load their messages   *   FILE 403
//*           also.                                                 *   FILE 403
//*                                                                 *   FILE 403
//*           email:  Ugur.Cilesiz@rwesystems.com                   *   FILE 403
//*                                                                 *   FILE 403
//*       - - - - - - - - - - - - - - - - - - - - - - - - - -       *   FILE 403
//*                                                                 *   FILE 403
//*           Notes about this Message Display Facility             *   FILE 403
//*                                                                 *   FILE 403
//*      Sometimes, when you are involved in servicing one, or      *   FILE 403
//*      several, components of the MVS operating system very       *   FILE 403
//*      often, you might like a convenient (and very quick) way    *   FILE 403
//*      of displaying the messages from that component, under      *   FILE 403
//*      ISPF.  Then this system is something you can use.          *   FILE 403
//*                                                                 *   FILE 403
//*      This system is also very flexible, and the display         *   FILE 403
//*      capability is not limited to IBM messages, but you can     *   FILE 403
//*      make arbitrary members of the "message pds" and display    *   FILE 403
//*      them anytime, by entering their "member name" in the       *   FILE 403
//*      UMSG display panel.                                        *   FILE 403
//*                                                                 *   FILE 403
//*      The messages are displayed, using either ISPF Browse,      *   FILE 403
//*      or the REVIEW TSO command from File 134 (load modules      *   FILE 403
//*      on File 135) of the CBT Tape.  The REXX exec which uses    *   FILE 403
//*      ISPF Browse (in this pds) is member UMSG.  The REXX        *   FILE 403
//*      exec which uses the REVIEW TSO command, is member          *   FILE 403
//*      UMSGR.  The panel using ISPF Browse, is called MESAJP,     *   FILE 403
//*      and the one using REVIEW, is called MESAJP1.  (Sam         *   FILE 403
//*      Golob is responsible for the REVIEW adaptation.  Ugur      *   FILE 403
//*      did all the work, though.  It was only a slight change     *   FILE 403
//*      from Ugur's original REXX.  SG)                            *   FILE 403
//*                                                                 *   FILE 403
//*      If you didn't remember the exact name of the message,      *   FILE 403
//*      you can ask for a partial message, by using an asterisk    *   FILE 403
//*      as a wild card.  For example, if you want to display a     *   FILE 403
//*      list of all ARC**** members in your MESAJ.PDS, then        *   FILE 403
//*      type ARC* in the pop-up panel.  (The procedure for the     *   FILE 403
//*      REVIEW adaptation, is that if you got the message id a     *   FILE 403
//*      bit wrong, and a blank REVIEW screen pops up, then you     *   FILE 403
//*      can enter the DIR subcommand of REVIEW, to get a pds       *   FILE 403
//*      directory so you can find the right message id.)           *   FILE 403
//*                                                                 *   FILE 403
//*       - - - - - - - - - - - - - - - - - - - - - - - - - -       *   FILE 403
//*                                                                 *   FILE 403
//*      This package is very simply constructed, and is also       *   FILE 403
//*      easy to install.                                           *   FILE 403
//*                                                                 *   FILE 403
//*      To install this package, just copy the UMSG and UMSGR      *   FILE 403
//*      execs into your SYSPROC or SYSEXEC library for your TSO    *   FILE 403
//*      session.  And copy the one panel (member MESAJP) into a    *   FILE 403
//*      panel library in your ISPPLIB concatenation.  The          *   FILE 403
//*      messages themselves will be put (later) into an FB-80      *   FILE 403
//*      partitioned dataset that you create, which may get to      *   FILE 403
//*      be rather large, depending on the number of IBM (or        *   FILE 403
//*      other) messages that you may want to load into it.         *   FILE 403
//*                                                                 *   FILE 403
//*      You must customize the UMSG and UMSGR execs to point to    *   FILE 403
//*      your message library pds (not to my library).  Then you    *   FILE 403
//*      load the message library pds with the IBM messages         *   FILE 403
//*      (I'll tell you how to do it, below), and then you run      *   FILE 403
//*      the UMSG exec.  The messages library pds is usually        *   FILE 403
//*      named 'prefix.MESAJ.PDS' , but you can name it anything    *   FILE 403
//*      you want, as long as the UMSG or UMSGR execs point to      *   FILE 403
//*      it.                                                        *   FILE 403
//*                                                                 *   FILE 403
//*      If you are using the REVIEW command (member UMSGR) to      *   FILE 403
//*      display the messages, then you must install it.  The       *   FILE 403
//*      easiest way to do that, is to get CBT Tape File 135        *   FILE 403
//*      (from www.cbttape.org , or from a CBT Tape) and copy       *   FILE 403
//*      every member starting with REV****, and all their          *   FILE 403
//*      aliases, to an ISPLLIB or STEPLIB, that is accessible      *   FILE 403
//*      to your TSO session.                                       *   FILE 403
//*                                                                 *   FILE 403
//*      I have made an ISPF command table entry, called UMSG       *   FILE 403
//*      (abbreviated to 2 characters) to invoke UMSG, so all I     *   FILE 403
//*      have to do, on my system, is to type UM on the command     *   FILE 403
//*      line, and press enter.  Then I get the MESAJP panel        *   FILE 403
//*      window, and I enter the member name of the message I       *   FILE 403
//*      want to look up.  It is very simple to use.                *   FILE 403
//*                                                                 *   FILE 403
//*      Unfortunately, the actual IBM messages are copyrighted,    *   FILE 403
//*      and their explanations may also change once in a while.    *   FILE 403
//*      So we can not include the actual IBM messages in this      *   FILE 403
//*      file, but we will tell you how you can get them, if you    *   FILE 403
//*      have Bookmanager installed under MVS.                      *   FILE 403
//*                                                                 *   FILE 403
//*      Just look at the $MESAJ2 member in this pds, and you       *   FILE 403
//*      will see how to generate the members of your               *   FILE 403
//*      'userid.MESAJ.PDS', for whichever messages that you        *   FILE 403
//*      want to extract from Bookmanager.                          *   FILE 403
//*                                                                 *   FILE 403
//*      We have included several sample (non-IBM) messages to      *   FILE 403
//*      include in your 'userid.MESAJ.PDS' so you can test how     *   FILE 403
//*      the system works.  These are in the member (of this        *   FILE 403
//*      pds) called SAMPMSGS.  The member is a pds, in IEBUPDTE    *   FILE 403
//*      or PDSLOAD unloaded format, and contains (at least) a      *   FILE 403
//*      member called ABEND (displaying most of the common         *   FILE 403
//*      ABEND codes) and REVIEW (which is a HELP member for the    *   FILE 403
//*      REVIEW TSO command).  If you also include these members    *   FILE 403
//*      in your 'userid.MESAJ.PDS' dataset, then you can access    *   FILE 403
//*      them by entering ABEND or REVIEW in the UMSG panel.        *   FILE 403
//*                                                                 *   FILE 403
//*      Best of luck to all of you.....                            *   FILE 403
//*                                                                 *   FILE 403
//*         Ugur Cilesiz                August 07, 2002             *   FILE 403
//*         Sam Golob                                               *   FILE 403
//*                                                                 *   FILE 403
```
