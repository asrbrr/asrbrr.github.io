---
title: " "
date: 2016-03-29
description: ""
---

Excel is by no means an ideal place to make data handling for many reasons, but the truth is that it has a nice point as compared to other alternatives: that it is very visual, and that you can very quickly open data sheets and edit them. But going further than Excel's built-in formulas, it soon arises the need to automate certain actions or to do things which the interface wouldn't allow: this is what the VBA (visual basic for application) macros are for. In my experience, it is quite useful to be able to quickly build simple scripts that will handle data in a spreadsheet; specially so for situations in which you want to carry out large numbers of repeated actions. However, if what you need to accomplish turns out to be more complex than simple scratching, then you'd better move to an alternative program (Matlab/Octave, Python, R, SQL or whatever, depending of your needs). What follows are the must-know features that open you the door to being able to quickly (and dirtily) build Excel scripts, and I recommend you knowing these by heart.

Quick start

If you are complete beginner, quick steps on how to set up:

The code editor is included with Excel, so no need to install additional software. Go to 'Developer' > 'Visual Basic'
Usually, what you need is a 'Module' where you will write functions. So right-click 'Insert' > 'Module'. You should see a white screen where you can write.
Write sub name()  [where 'name' is any name you want] and hit enter. You are ready to start writing code!
Once done, you can either run it all at once, by means of the 'play' button (or by pressing F5), or you can execute it one line at a time (by pressing F8, which is very very convenient for debugging and also for just learning).
You need to know that the code is saved together with the Excel workbook: there is no simple way to keep it apart (such as in txt files): this is a major source of pain when creating quick-and-dirty Excel macros, and one that discourages quite a bit from developing any further: if you need more, then maybe change into another language.

Basic classes and objects

The main object that are present in Excel's VBA model are these:

'Application', that represents the Excel process
'Workbook'
''Worksheet'
'Range'
For simple tasks (like editing the content) you will mostly be working with the last one, or maybe the last 3 ones. Anyway, they form a hierarchy, but omitting the upper references is allowed and then the interpreter will take by default the current active Workbook etc as the implied one. There is an alternative way to reference the default workbook, which is to use the "Thisworkbook" expressions. And finally, they can also be referenced by name or by numeral. So all in all, if you want to work on cells in the current active worksheet (which let's suppose it's called 'mysheet' in the workbook 'myworkbook', and the sheet happens to be the first of all of them) , then the following expressions give the same reference (you can in fact build even more combinations of them)

Application.ActiveWorkbook.ActiveSheet.Range("A1").value
            ActiveWorkbook.ActiveSheet.Range("A1").value
                           ActiveSheet.Range("A1").value
                                       Range("A1").value
ThisWorkbook.ActiveSheet.Range("A1").value
Workbooks('myworkbook').Sheets('mysheet').Range("A1").value
Sheets(1).Range("A1").value
Collections

You probably noticed the 's' in some of the objects in the expressions above: they represent the collection of all the things of that kind: sheets are all the sheets in the workbook, and so on. This is handy when looping.

Range (or cell)

Eventually you end up pointing at a particular cell or group of cells: this is what's called Range. The syntax is as in Excel's formulas, in the form of Range("A1") etc. That's ugly, and this is why mostly you will be using an alternative syntax that uses integer values by means of the "Cell" keyword; Cell(2,1) represents the cell in the second row and the first column. These expressions are equivalent:

Range("A2")
Cells(2,1)
There is also a 'select' method, which makes the specified cell/range become the 'selection' in the application. I guess this is how the user interface works (you only work at a given cell/range at a time, and in fact you are shown which is the selected one in each moment). But VBA-wise, this is not a nice property to work with, and in general is not at all necessary. Equivalently, you can work with the 'Activesheet' and 'Activeworkbook' references, but this makes the piece of code more prone to errors whenever you happen to click on the wrong sheet.

Rows and columns

You can reference entire rows or columns as:

Range("A:A") --> first column
Range("4:4") --> fourth row
Rows(1) --> first row
Columns(2) --> second column
Cell properties

Once you are at a Cell level, you find several properties that you want to work with:

.value --> (the default property) gives/assigns the contents of the cell
.formula --> gives/assigns a formula: it would look something like cells(1,1).formula = "=2+3"
.formulaR1C1 --> the same as above, but with R1C1 types of references. This is oftentimes more useful so as to express relative references more easily in the code.
.row / .column --> they give you the row/column number of the referred cell. This is very useful when you ended up in a certain cell through various methods, and you need to know where you are
.resize() --> when you work with references in the form on Cells(2,2) but you want to refer to various cells, there is no way to specify a multi-cell range directly. Instead, you apply the .resize() method. This way, you expand your current selection (that is, one cell) to cover various: .resize(1,4) takes the current cell plus the 3 to its right; .resize(3,1) takes the current cell plus the two below it, etc.
.offset() --> similar to the previous one, but this method "jumps" the selection by the specified amount. .offset(2,0) moves the referred cell two below; .offset(0,-1) moves one to the left, etc.
.end(xldown) --> this moves the referred cell to the downmost cell in the area (equivalent contr+down with the keyboard). You can also use the xlup, xltoright, ,xltoleft labels to specify directions. This is handy when you want to find you the last row or the last column in a set of data, and to that end you combine the method with the .column or .row attributes.
There are of course many other properties and methods that you can use with cells, but it's quite easy to find your way through them by just reading the tab-completion list shown, or by going to the documentation. In my experience, tough, the ones above are by far the most used ones.

Flow control

The main interest of the Excel scripts are of course the flow control tools, which are in general difficult to replicate via simple formulas. These are the usual ones, and their syntax is as follows:

If a < 5 then
  ...
  [elseif a < 8 then]
  [else]
end if
______________________________
For i = 1 to 10
  ... 
  [Exit For]
next i
______________________________
While i >0
  ...
  [exit while]
Wend
______________________________
Do
  ...
  [exit do]
Loop
______________________________
[Variants of do] Do until...Loop   ; Do... Loop until ; Do while...loop
______________________________
For each ws in worksheets 
  ...
next ws
______________________________
Select Case i
  Case 1
    ...
  Case is = 2
    ...
  Case is > 3
    ...
  Case else
End select
The 'for each' construction is very useful when looping through objects (such as worksheets, cells etc)

Error handling

[pending]

Other concepts

I do not intend to go deep into the explanations here, there are hundreds of good sites and books where you can learn them. But to complete the basic notions, here are other points to keep in mind:

comments:  '
strings: " "
breakpoints: will stop execution, and then you can resume it: do click on the left margin of the editor, and you will get a red dot
message boxes: you can include them with the msgbox("text") function; they do stop execution until acknowledged
input boxes: you can ask for user input through the inputbox() function
print values to the 'shell': you can internally print runtime values for review on the editor: select View>InmediateWindow, and include the sentence debug.print [var] in your code. Alternatively, if you interrupt execution, you can write sentences in this window and interact with your namespace as is at that point in time of the execution.
interrupt execution: you can press escape; you can insert message windows; you can insert breakpoints; you can force an error an press debug
alter execution order: once in debug mode (or executing code one-line-at-a-time through F8, you can move the yellow line manually, so that you can skip or repeat sentences, even after manipulation. This is a very helpful debugging trick.
variable declaration: you can omit declaring variables, which is acceptable for simple and short scripts. If it ever were to grow, then better to declare them, with the Dim xx as string kind of syntax
variable scope: the scope is the function, unless you declare them in the uppermost part of the script (outside of any of the functions), with the Public prefix (instead of Dim)
function name scopes: normal functions can be called from other functions given that they are in the same Module, as well as manually by the user. They could be called from other modules if they are declared as Public. It can be prevented that they are called by the user, by using the Private prefix.
variable assignment: you assign with the = sign for basic variables, and additionally with Set prefix for object variables.
variable syntax: usual restrictions apply (cannot begin with a number etc). Besides, it is interesting to note that the interpreter is not case sensitive: j and J are the same variable.
passing arguments to functions: if you ever create more than one function, you can pass arguments either by value or by reference (using the byval and byref keywords). The scope of the variables in general is the function itself, except when you declare them at the top of the script, in which they receive a global scope.
events: if you ever need to make fancy things that depend on the user actions, then Excel allows for event oriented programming.
A word about the macro recorder

The macro recorder allows you to write as code the actions you take on the user interface, but is clunky. You have to be aware that:

it does not record absolutely everything
it generates awful code
That being said, it can be used wisely: specially when you would like to make a picturesque action (such as background coloring, border formatting etc), it can be handy to record it as done "by hand", and then read the code so that you extract the method that has been used by the recorder. This is a good trick sometimes, but you cannot trust that it will always be useful, specially when you want the code to generalize a bit.

Useful Excel-VBA code snippets

1. Copy-paste

Cells(1,1).Copy
Cells(2,2).select
Activesheet.paste
Application.CutCopyMode = False
'or
Cells(2,2).PasteSpecial xlPasteValues
2. Find out the number of rows/columns in a given set of data

'several options, depending on the specific case
Cells(1,1).SpecialCells(xlLastCell)   'points at what's considered "end of docmt"
Cells(1,1).end(xldown)        'last cell in the contiguous region (vertical)
Cells(1,1).end(xlright) 
Cells(99999,1).end(xlup)      'trick to get the last really written cell in col 1
3. Loop through all the sheets (possibly, except one)

skip = ActiveSheet.name
for each ws in Worksheets
   if ws.name = skip then
      'skip
   else
      'code goes here ''''''''''''''''''''''
   end if
4. Find all the files in a given folder (subfolders) and open them (supposing they are all Excel files)

Set filesys = CreateObject("Scripting.FileSystemObject")
Set folder = filesys.GetFolder("C:\temp")
Set files = folder.Files
Set subfolders = folder.subFolders

For each file in files
    If (file.Attributes = 2 or file.attributes = 4 or file.Attributes = 8) then
        'ignore hidden/system files
    else
        'Rest of code goes here '''''''''''''''''''''''''''''''''
        'you can use file.name property to get the filename
        'you can open the workbook with   Workbooks.Open(file)
        'you can close the workbook with  Workbooks(file.name).Close
    end if
next file

For each subfolder in subfolders
    'you can call the same function again recursively, to navigate the subfolders
next subfolder

Set filesys = Nothing
Set folder = Nothing
Set files = Nothing
Set subfolders = Nothing
5. Insert comments to cells

Cells(1,1).AddComment  'note: it will throw an error if a comment already exists
Cells(1,1).Comment.Text texto_para_comentario
Cells(1,1).Comment.Shape.TextFrame.AutoSize = True
Cells(1,1).Comment.Visible = False
5. Inset hyperlinks

ActiveSheet.Hyperlinks.Add Anchor:=Range("A1"), Address:=addr, TextToDisplay:=txt
6. Clear, insert or delete sheets

'Clear contents or delete cells:
Cells.ClearContents
Cells.Delete Shift:=xlUp

'Insert new sheet:
Sheets.Add After:=Sheets(Sheets.Count)   'adds to the end
Sheets(Sheets.Count).Name = "name"

'Delete:
Sheets(name).Delete