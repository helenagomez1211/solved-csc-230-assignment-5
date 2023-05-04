Download Link: https://assignmentchef.com/product/solved-csc-230-assignment-5
<br>
The boards we are using have a Hitachi HD44780 compatible LCD display that has its own

(simple) processor. To communicate with that processor a specific protocol is used that initializes and controls the LCD screen.  Learn more about how the LCD controller works by looking at the data sheet:  <a href="https://www.sparkfun.com/datasheets/LCD/HD44780.pdf">https://www.sparkfun.com/datasheets/LCD/HD44780.pdf</a>

A previous CSC 230 student has written a library of subroutines for this assignment.  The table below lists the subroutines, their parameters and what they do.

There is an example program, that shows how to use the LCD functions in the file: lcd_example.asm.  Review both files carefully until you are able to understand their functionality.  While doing that, search for a way to set the number of rows on the LCD to 2 and the number of columns to 20.

<h1>LCD Moving Message Sign</h1>

LCD advertising signs often move written messages on a screen and flash.  The movement and the flashing are very effective at attracting attention.  The goal of this programming task will be to display messages, using the buttons to move between them and them down and across the screen, and to display and flash both messages.  <strong>Please name this file as “display.asm” for submission on connex.</strong>

In particular, the program will need to:

<ol>

 <li>Create two messages that will be displayed on the screen. For example, assume:    msg1 = “LillAnne Jackson”</li>

</ol>

msg2 = “Teaches Assembly”

(Be sure to use two messages personalized for yourself!)

<ol>

 <li>Using the messages (msg1 and msg2) create another two messages that contain the same characters, but in reverse order. Using the above example messages, the messages, called msg3 and msg4, would be:</li>

</ol>

msg3 = “noskcaJ ennAlliL”

msg4 = “ylbmessA sehcaeT”

<ol>

 <li>The sign will scroll (cycle) through msg1, msg2, msg3, and msg4, displaying them on the LCD screen, using the specifications below:</li>

</ol>

<table width="197">

 <tbody>

  <tr>

   <td width="197">************************************</td>

  </tr>

 </tbody>

</table>

<ol>

 <li>When the program starts, both rows of the the LCD screen will be completely filled with asterisk or star “*” characters.</li>

 <li>When the ‘UP’ button is pressed, the characters on the 2<sup>nd</sup> line of the screen to move up to the 1<sup>st</sup> line and the next message will appear on the 2<sup>nd</sup> (After the start up screen msg1 appears, followed by msg2 then msg3 then msg4.) For example, starting from the initial start up screen, 5 presses of the UP button would “scroll up” yielding:</li>

</ol>




******************

press UP:                 LillAnne Jackson

LillAnne Jackson

press UP:                 Teaches Assembly

Teaches Assembly

press UP:             noskcaJ ennAlliL




noskcaJ ennAlliL

press UP:                ylbmessA sehcaeT




ylbmessA sehcaeT

press UP:                     LillAnne Jackson

<ul>

 <li>When the “DOWN’ button is pressed, the characters on the 1<sup>st</sup> line of the screen move down to the 2<sup>nd</sup> line and the next message will appear on the 1<sup>st</sup> (After the start up screen msg4 appears, followed by msg3 then msg2 then msg1.)  For example, starting from the initial start up screen, 5 presses of the UP button would “scroll down” yielding:</li>

</ul>

press DOWN:

ylbmessA sehcaeT

******************

press DOWN:  noskcaJ ennAlliL ylbmessA sehcaeT

press DOWN:  Teaches Assembly      noskcaJ ennAlliL

press DOWN:         LillAnne Jackson

Teaches Assembly

press DOWN:         ylbmessA sehcaeT

LillAnne Jackson

(Any combination of these buttons presses will cause the screen to ‘scroll’ up or                down according to the press.)

<ol>

 <li>When the ‘SELECT’ button is pressed, the current screen will alternately display the current messages and the start up screen (completely filled with ‘*’ characters) alternating every 0.5 second. Essentially, it will flash.</li>

</ol>

<table width="197">

 <tbody>

  <tr>

   <td width="197">**  10 **Buttons Pressed</td>

  </tr>

 </tbody>

</table>

<ol>

 <li>When the ‘LEFT’ button is pressed, the screen will display the number of button presses on the first line and the words “Buttons Pressed” on the second line.</li>

</ol>




Design the program so that pressing and holding the button down will only count as a single button press.  In particular, a button press is only counted after a button is pressed and then released.

<ol>

 <li>When the ‘RIGHT’ button is pressed, the screen will automatically scroll through the messages in (up) order, msg1 followed by msg2 followed by msg3 followed msg4 followed by msg1 etc, changing every second.</li>

</ol>

<strong><em> </em></strong>Implementation Help

The instructor’s solution has the following in the data:

; sample strings

; These are in program memory msg1_p: .db “LillAnne Jackson”, 0 msg2_p: .db “Teaches Assembly”, 0




.dseg

;

; The program copies the strings from program memory ; into data memory.

; msg1: .byte 40 msg2: .byte 40

; These strings contain the (up to) 18 characters to be displayed on the LCD

; Each time through, the 18 characters are copied into these memory locations .equ msg_length = 19

line1: .byte 19 line2: .byte 19 line3: .byte 19 line4: .byte 19




With these data definitions, the main loop of the application looks like:




initialize the lcd output the startup screen (“*” characters) on lcd copy the strings from program to data memory create the two reversed strings, store in data memory startString = “******************” line1 = startString line2 = startString msg_counter=0 button_count = 0 when a button is pressed:    increment button_count   if button is UP:   msg_counter += 1 remainder 4    line1 = line2  line2 = msg(msg_counter)  display(line1, line2)   if button is DOWN:       msg_counter -= 1 remainder 4

line2 = line1       line1 = msg(msg_counter)    display(line1, line2)   if button is SELECT:  display (line1, line2)  delay (0.5 second)  display (startString, startString)

delay (0.5 second)   if button is LEFT:

line_count = “** ” + button_count + ” **”  text_line = “Buttons Pressed”  display(line_count, text_line)   if button is RIGHT:    msg_counter = 1  do until another button is pressed:            line1 = msg(msg_counter)      msg_counter += 1 remainder 4         line2 = msg(msg_counter)      display(line1, line2)

delay (1.0 second)







function display(lineA, lineB):

set row counter to 1 and column counter to 1   display lineA

set row counter to 2 and column counter to 1   display lineB

function msg(counter): ; returns address of line 1, 2, 3 or 4   return addressOf(line1) + msg_length * counter

You should make extensive use of subroutines in your code, including the delay code .  The solution is ~300 lines of assembly language.  Start now!




<h1>Extending the Moving Message Sign</h1>

Now, add some features. Here are some options. You can choose <strong>any one</strong> of them or suggest an alternative.

<ul>

 <li>Implement a scrolling display for longer messages: Scroll them across the screen, wrapping such that a character that falls off one end will (eventually) appear on the other.  Since the screen only displays 16 characters and the messages are up to 18 characters in length, it will be necessary to track the part of the message the is currently being output.  Discussions on algorithms for this welcome in office hours.</li>

 <li>Allow button presses to increase and decrease the sign change speed.</li>

 <li>Allow selection of a larger number of messages.</li>

</ul>

<strong> </strong>