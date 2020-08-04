<H1>LibTerm</H1>
<HR>
<H2>NAME</H2>
LibTerm - VT100 Terminal Library
<br><br><br>
<H2>SYNOPSIS</H2>

<P>
<B> package require libterm ?1.0.0?</B>
<P>
<B>::libterm::tputs </B>?<B>-nonewline</B>? ?<B>-nocolors</B>? ?<I>channelID</I>? <I>string</I>
<P>
<B>::libterm::rtputs </B>?<B>-nonewline</B>? ?<B>-nocolors</B>? ?<I>channelID</I>? <I>string</I>
<P>
<B>::libterm::mv </B>?<I>lines </I>?<I>lines ...</I>??
<P>
<B>::libterm::mv </B>?<I>position</I>?
<P>
<B>::libterm::curpos </B><I>action</I>
<P>
<B>::libterm::erase line|screen </B><I>type</I>
<P>
<B>::libterm::reset</B>
<P>
<B>::libterm::scroll </B><I>mode</I>
<P>

<HR>

<H2>DESCRIPTION</H2>

<P>

<B>LibTerm</B> is library written in a pure-Tcl which allows user to use VT100 terminal
features like ANSI colors, cursor position change (relative and non-relative),
cursor position save and restore in TCL.
<P>

<H2>COMMANDS</H2>

<DL COMPACT>
<DT><B>::libterm::tputs </B>?<B>-nonewline</B>? ?<B>-nocolors</B>? ?<I>channelID</I>? <I>string</I><DD>
The <B>::libterm::tputs</B> procedure works similar to simple <A HREF="http://www.tcl.tk/man/tcl8.4/TclCmd/puts.htm"><B>puts</B></A> command, but it
can interpretate color tags in <I>string</I>. If <B>-nocolors</B> switch is given, then
color tags will be interpreted, but colors will be not applied. There is no matter if this switch
is given or not, color tags will be always removed from string. Color tags takes effect from its position
to %n tag, or to end of <I>string</I> or when another color tag replaces current color. <B>::libterm::tputs</b>
is about 5 times slower than <B>puts</B>.<br>
<br>
Following tags are allowed:<br><br>

<P><DT><B>%%</B><DD>
Insert a %.<DT>
<P><DT><B>%b</B><DD>
Blue<DT>
<P><DT><B>%r</B><DD>
Red<DT>
<P><DT><B>%g</B><DD>
Green<DT>
<P><DT><B>%y</B><DD>
Yellow<DT>
<P><DT><B>%m</B><DD>
Magenta<DT>
<P><DT><B>%w</B><DD>
White<DT>
<P><DT><B>%k</B><DD>
Black<DT>
<br>
<DD>
These are dark colors. To get bright colors just use upper case chars.
To use colors for background use tags, which starts with <B>%#</B> and
one of color char, for example <B>%#B</B> will give light blue background.
This command returns empty string.<br>
There are also additional tags:<br><br>
<P><DT><B>%d</B><DD>
Bold<DT>
<P><DT><B>%u</B><DD>
Underline<DT>
<P><DT><B>%v</B><DD>
Video reverse<DT>
<P><DT><B>%l</B><DD>
Blinking<DT>
<br><br>


<DT><B>::libterm::rtputs </B>?<B>-nonewline</B>? ?<B>-nocolors</B>? ?<I>channelID</I>? <I>string</I><DD>
The  <B>::libterm::rtputs</B> is just restricted kind of <B>::libterm::tputs</B> procedure.
It can't interpretate more than 1 tag for one part of <I>string</I>, but it's much faster
than <B>::libterm::tputs</B>. Well, there is no way to use colors for foreground and background
at same time with <B>::libterm::rtputs</B>, but it's useful when you need to change colors only for
foreground. This command also returns empty string. <B>::libterm::rtputs</B> is about 3 times slower than <b>puts</b>.<br><br>

<DT><B>::libterm::mv </B>?<I>lines </I>?<I>lines ...</I>??<DD>
<DT><B>::libterm::mv </B>?<I>position</I>?<DD>
In the first case this command moves cursor from current position to another one for given number of <I>lines</I>.
<I>lines</I> has to be one word, where first character means direction and has to be one of
<B>u</B>, <B>d</B>, <B>l</B>, <B>r</B> (accordingly: up, down, left, right), rest of characters determinates
how much lines move the cursor. For example <B>::libterm::mv u5 r3</B> will move up cursor for 5 lines
and right for 3 lines.<br>
<br>
In the second case the only argument is non-relative coordinate of <I>position</I>, where cursor should be moved.
If given <I>position</I> is greater than terminal size, then cursor is moved to most close position.
?position? can be full coordinates: <I>x-pos,y-pos</I> or only: <I>y-pos</I>, then x-pos is assigned to 0.
This procedure also returns empty string.<br><br>
<DT><B>::libterm::curpos </B><I>action</I><DD>
This procedure remembers or restores remembered position of cursor. <I>action</I> can be <B>save</B> or <B>load</B>.
<BR><BR>

<DT><B>::libterm::erase line|screen </B><I>type</I><DD>
Erases part of <B>line</B>, whole <B>line</B>, part of <B>screen</B>, or whole <B>screen</B>. <I>type</I> must be
on of following: <B>left</B>, <B>right</B>, or <B>both</B>. With <B>line</B> as a first argument, following
actions are taken:<BR>
<P><DT><B>left</B><DD>
Erases chars from start of current line to the cursor position.<DT>
<P><DT><B>right</B><DD>
Erases chars from the cursor position to end of current line.<DT>
<P><DT><B>both</B><DD>
Erases whole line.<DT>
<BR>
<DD>
With <B>screen</B> as a first argument, following actions are taken:<BR>
<P><DT><B>left</B><DD>
Erases chars from start of first line to the cursor position.<DT>
<P><DT><B>right</B><DD>
Erases chars from the cursor position to end of last line.<DT>
<P><DT><B>both</B><DD>
Erases whole screen.<DT>
<BR><BR>

<DT><B>::libterm::reset</B><DD>
Resets all terminal settings to default values.
<BR><BR>

<DT><B>::libterm::scroll </B><I>mode</I><DD>
Determines a teraminal scroll region. <I>mode</I> can be one of following:<br>
<P><DT><B>full</B><DD>
Switches terminal scroll mode to full screen (it's a standart).<DT>
<P><DT><B>region</B><DD>
Swhitches terminal scroll mode to region. The region is described by <B>::libterm::scroll begin end</B>
procedure call.<DT>
<P><DT><B>begin end</B><DD>
In this case there are two arguments - line numer, which will be begin of scroll
region and line number, which will be end of scroll region. The <B>::libterm::scroll region</B>
procedure call is required to make this take effect.<DT>

<P>
</DL>
<H2>AUTHOR</H2>

Paweł Salawa
<P>
<H2>SEE ALSO</H2>

<A HREF="http://www.tcl.tk/man/tcl8.4/TclCmd/puts.htm">puts</A>
<P>
<H2>KEYWORDS</H2>

Tcl, package, Terminal
