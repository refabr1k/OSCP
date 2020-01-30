# Buffer Overflow

1. Attach debugger
2. Fuzz and check \# of As to crash it
3. Check for enough buffer for exploit \(about 350 - 400 bytes\)
4. Check for bad characters \(remove bad characters from script for each iterations to test\)

   Check JMP ESP

   * !mona modules - find modules with no bad charcters in address \(no null bytes\) eg. x5f400000 and has no DEP, ASLR protection
   * Select it and click 'e' for executable modules list then double click the module

     ![](.gitbook/assets/image%20%289%29.png)

   * Search for jump esp instruction, rightclick &gt; search for &gt; command &gt; 'jmp esp'

     ![](.gitbook/assets/image%20%282%29.png)

   * \(OR\) rightclick &gt; search for &gt; sequence of commands &gt; "push esp" \(enter\) "retn" 

     ![](.gitbook/assets/image%20%283%29.png)

   * \(OR\) look for executable parts of dll instructions eg. 

     ![](.gitbook/assets/image%20%2815%29.png)Note: Because in SLMail example the dll SLMFC is not protected by DEP and ASLR, we could use any part of the SLMFC dll \(eg. Text,data,rdata etc\) and not just limited to the 'Executable' part of SLMFC's .text section

   * e. Using the command '!mona find -s "\xff\xe4" -m &lt;dll name&gt;
   * Search for the jmp esp address \(look for no badcharacters in address\)

* asd 

## 2.  Fuzz and check \# of As to crash it

## 3.  Check for enough buffer for exploit \(about 350 - 400 bytes\)

## 4.  Check for bad characters \(remove bad characters from script for each iterations to test\)

5.  Check JMP ESP

a.  !mona modules - find modules with no bad charcters in address \(no null bytes\) eg. x5f400000 and has no DEP, ASLR protection

b.  Select it and click 'e' for executable modules list then double click the module



![](.gitbook/assets/image%20%2810%29.png)

c.  Search for jump esp instruction, rightclick &gt; search for &gt; **command** &gt; 'jmp esp'

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image003.png)

d.  \(OR\) rightclick &gt; search for &gt; **sequence of commands** &gt; "push esp" \(enter\) "retn"

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)

\(OR\) look for executable parts of dll instructions eg.

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image005.png)

Note: Because in SLMail example the dll SLMFC is not protected by DEP and ASLR, we could use any part of the SLMFC dll \(eg. Text,data,rdata etc\) and not just limited to the 'Executable' part of SLMFC's .text section

e.  Using the command '!mona find -s "\xff\xe4" -m &lt;dll name&gt;

Search for the jmp esp address \(look for no badcharacters in address\)

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image007.jpg)

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image008.png)

f.  Verify if address contain JMP ESP. Rightclick and copy to clipboard address.

Click on icon to go to address

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image009.png)

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image010.png)

6.  Verifing JMP ESP.

a.  Add address IN REVERSE to buffer overflow script

b.  Set breakpoint on JMP ESP using F2 in immunity debbuger and run

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image011.png)

c.  Right click ESP address and click 'follow in dump' to verify that the next executing set of instructions are indeed 'CCCC…."

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image012.png)

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image013.png)

\(clicking F7\) will continue program flow to the "CCCCC…."

![](file:///C:/Users/edmun/AppData/Local/Temp/msohtmlclip1/01/clip_image014.png)

7.  Adding shellcode to buffer overflow script

