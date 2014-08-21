#Playing with chords & lists on Sonic-Pi 2
There are two commands which will play chords on Sonic-Pi 2. The first one looks like this:- you can type it in a new workspace
```Ruby
play_chord [:c4, :e4, :g4]
```
note there must be a space between chord and the opening [. When you press Run, this will sound the three notes together. If you want you can have more than three notes, by typing further notes separated by commas in the list between the [ ]. If you add the next two lines and run the program again, you will see the alternative way of playing a three note chord:
```Ruby
play_chord [:c4, :e4, :g4]
sleep 1
play chord(:c4,:major)
```
In this case we describe the chord in a slightly different way. You type the bottom note or the tonic of the chord (here :c4) and follow it with the type of chord you want to play, after a separating comma. In this case we play a C major chord. Other types you could try include  :minor, :dom7,  :dim7 and there are a host of others. Some contain numbers and have to be put inside apostrophes or speech marks, eg '11+' or "m+5", but that goes a bit further than we will just now.
What we are going to do is to explore the way Sonic-Pi holds details of the notes when the chord is played, and how we can manipulate and change chords.

Add the last line below, and press run again:
```Ruby
play_chord [:c4, :e4, :g4]
sleep 1
play chord(:c4,:major)
puts chord(:c4,:major)
```
You will hear the two chords, which sound identical, and if you look at the output pane, you will see [60, 64, 67] in blue type. This is produced by the puts command which asks Sonic-Pi to print what follows, in this case chord(:c4,:major) and the result is a list of three numbers, separated by commas inside square brackets.
This is the same way that the first play_chord specified the notes to be played, but we see the equivalent midi-note numbers corresponding to the symbols :c4, :e4 and :g4
We can in fact ask Sonic_Pi to do the conversion for us explicitly by adding 3 further lines to get:
```Ruby
play_chord [:c4, :e4, :g4]
sleep 1
play chord(:c4,:major)
puts chord(:c4,:major)
puts note_info(:c4).midi_note
puts note_info(:e4).midi_note
puts note_info(:g4).midi_note
```

The last three similar commands asks Sonic_Pi to print in the output pane information about the note in the form of its midi note number, and we will see the answers 60 64 and 67 again, one on each line.

#Looking at a list and changing it

Now we will use another idea. We are going to store the list of notes in the chord in a variable which we will call nlist. This is rather like a box in which we will put a piece of paper with some information on it. Later we can go back to the box and get the piece of paper and read what is on it. Add a further two lines {you can miss out the comments - the bits after the # if you don't want to type too much} to give:

```Ruby
play_chord [:c4, :e4, :g4]
sleep 1
play chord(:c4,:major)
puts chord(:c4,:major)
puts note_info(:c4).midi_note
puts note_info(:e4).midi_note
puts note_info(:g4).midi_note
nlist = chord(:c4,:major) #stores the notes in the chord like writing on a bit of paper, in the variable or box nlist
puts nlist #gets the contents of nlist (like what is written on the piece of paper in the box), and prints it
```

When you run this you will see an additional [60, 64, 67] at the bottom of the output pane.
This is called a list. It has three items in it, the numbers 60 64 and 67. We can change the list by adding extra numbers, or by taking some away, or by altering their order. These are all things that can be done in the Ruby language in which Sonic-Pi is written.
First we will add an extra number on the end, which will give us a four note chord when we play it. Add the extra lines onto the program shown below:

```Ruby
play_chord [:c4, :e4, :g4]
sleep 1
play chord(:c4,:major)
puts chord(:c4,:major)
puts note_info(:c4).midi_note
puts note_info(:e4).midi_note
puts note_info(:g4).midi_note
nlist = chord(:c4,:major) #stores the notes in the chord like writing on a bit of paper, in the variable or box nlist
puts nlist #gets the contents of nlist (like what is written on the piece of paper in the box, and prints them
nlist = nlist + [:c5]
sleep 1
play_chord nlist
puts nlist
```

When you run this you will hear three chords played. The first two are identical 3 note chords. The last one plays the four note chord we have generated, and the puts nlist command in the last line prints out the list of four notes on the output screen
Notice how we add the extra note to the list. We don't just say + :c5 We have to put it into a one element list [:c5] - a list with one piece of information in it - and then we add the two lists together to make a single longer list. It is like having a line of three railway trucks couple together (our nlist) and we couple another line consisting of 1 truck onto the end, giving a longer line of four trucks. The trucks themselves could be different: a coal truck, a cement truck, a flatbed truck (like the different notes in this case), but they are all trucks and can be joined together.
nlist doesn't know about notes, so the list contains three numbers and one symbol :c4. All will be converted to midi numbers when they are played by the play_chord command. We could have done it ourselves by adding 72 instead of :c5 to the list.


#moving things around
Now we will go back to the original C major chord play chord(:c4,:major). If you have done any music theory, you may have come across the idea of chord inversions. To produce the first inversion of the chord, we take the bottom note off the chord, put it up an octave, and add it on the top. So our chord :c4, :e4, :g4 becomes the chord :e4, :g4, :c5 We can of course do this manually, but since we are playing with lists we can get Ruby commands to do it for us.
For this section it is easiest to start with a fresh workspace. You can use the save button to save what we have done so far, if you wish, but then delete everything in the workspace, or select another empty one.
Now type in the following code:

```Ruby
nlist = chord(:c4,:major)
puts nlist
play_chord nlist
sleep 1
n0 = nlist[0]
puts n0
nremainder = nlist[1..-1]
ntop = [n0 + 12] #adding 12 puts it up an octave
n1stinversion = nremainder + ntop #add two lists together to get the final inversion chord list
puts n1stinversion
play_chord n1stinversion
```


If you run this you will hear the original C major chord, and see its note list printed in the output pane. [60, 64, 67]. After a second delay you will hear the first inversion chord play, and see its note list on the output screen [64, 67, 72]
So how does this work? Ruby does the work for us. the line n0 = nlist[0] creates another variable (or storage box) called n0. It stores in it the first element or number in the list of numbers stored in nlist. One thing to note, is that when you are referring to a particular number in a list, Ruby numbers them from 0, not from 1. So the first element or number is element 0, the second one element 1 and so on.
so n0 has the number 60 stored in it. If you want to check this you could add the line puts n0 just after the line n0 = nlist[0]
The next line is a bit trickier. We make another variable called nremainder and in it we store a list of the remaining two numbers in nlist. The syntax or grammar of how this is done is to use nlist[1..-1] This means starting with element 1 (remember this will be the second number of the three in the list as they number from 0) go up to the last number (-1) however many there are. It looks a bit odd, but that is the way it works.
The next thing to do is to make the note number for the final note to go on the top of the chord. Since there are 12 semitones in an octave, the midi note number for notes an octave apart differ by 12, 1 for each semitone. So the top note which we will store in another variable called ntop is produced by the line:
```Ruby
ntop = [n0 + 12] #adding 12 puts it up an octave
```
Note that we put the new note number inside [ ] before storing it in ntop. This makes ntop a LIST containing a single element rather than just a number.
The final stage of the process is to add the two LISTS nremainder and ntop together giving us the new inverted chord LIST. That is done by the line
```Ruby
n1stinversion = nremainder + ntop #add two lists together to get the final inversion chord list
```
Finally we can print this new list to the output pane and play the resulting chord, which is what the last two lines do.
#Where next?
In this tutorial we have looked at how chords are played and represented in Sonic_Pi. We have also seen how Ruby LISTS can be manipulated to produce new chords. As an exercise you might like to go through a similar process to the one we have used to produce the first inversion of C major to produce the second inversion. You want to remove the new bottom note of the chord, put it up an octave and add it on the top again.
Lists are used a lot in Sonic-Pi. You might also like to explore the command to reverse the items in a list. For example
```Ruby
n = [:c4, :d4, :e4, :f4, :g4, :a4, :b4, :c5]
```
 is the list of notes in a C major scale.
If you type this in and then type:
```Ruby
use_bpm 240
play_pattern n
```
it will play this scale. You can then try adding the following lines
```Ruby
sleep 1
play_pattern n.reverse
sleep 1
play_pattern n.shuffle
```
These modify the list containing the scale. The first reverses it, the second selects each element once at random.
Have fun exploring!



