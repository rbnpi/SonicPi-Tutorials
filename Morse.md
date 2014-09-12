 ```Ruby
 #sonic-pi 2 does morse! by Robin Newman 28th June 2014
  #you can send a-zA-Z and 0-9 and ,.!?  (other characters could be added)
  msg = "Sonic Pi 2 can generate morse code." #the message to be sent


  sp = 0.08 #set the speed 0.08 is about 18 words/minute
  dit = 1 * sp #timings for individual elements
  dah = 3 * sp
  gs = 1 * sp #gap between elements
  cs = 3 * sp #gap between characters
  ws = 7 * sp -cs # gap between words (cs already included by playarray so subtract it)
  pt = :c6 #note pitch

  note=[] #empty arrays for note and durations
  dur=[]
  #define alphabetic characters each is an array entry for notes and for durations
  a = [[pt,pt],[dit,dah]]
  b = [[pt,pt,pt,pt],[dah,dit,dit,dit]]
  c = [[pt,pt,pt,pt],[dah,dit,dah,dit]]
  d = [[pt,pt,pt],[dah,dit,dit]]
  e = [[pt],[dit]]
  f = [[pt,pt,pt,pt],[dit,dit,dah,dit]]
  g = [[pt,pt,pt],[dah,dah,dit]]
  h = [[pt,pt,pt,pt],[dit,dit,dit,dit]]
  i = [[pt,pt],[dit,dit]]
  j = [[pt,pt,pt,pt],[dit,dah,dah,dah]]
  k = [[pt,pt,pt],[dah,dit,dah]]
  l = [[pt,pt,pt,pt],[dit,dah,dit,dit]]
  m = [[pt,pt],[dah,dah]]
  n = [[pt,pt],[dah,dit]]
  o = [[pt,pt,pt],[dah,dah,dah]]
  p = [[pt,pt,pt,pt],[dit,dah,dah,dit]]
  q = [[pt,pt,pt,pt],[dah,dah,dit,dah]]
  r = [[pt,pt,pt],[dit,dah,dit]]
  s = [[pt,pt,pt],[dit,dit,dit]]
  t = [[pt],[dah]]
  u = [[pt,pt,pt],[dit,dit,dah]]
  v = [[pt,pt,pt,pt],[dit,dit,dit,dah]]
  w = [[pt,pt,pt],[dit,dah,dah]]
  x = [[pt,pt,pt,pt],[dah,dit,dit,dah]]
  y = [[pt,pt,pt,pt],[dah,dit,dah,dah]]
  z = [[pt,pt,pt,pt],[dah,dah,dit,dit]]
  #put characters in a lookup array
  lookup = [a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z]
  #traverse the characters in the message converted to lowercase and to byte values
  msg.downcase.each_byte  do|ch|
    case ch #deal with non-alphabetic characters
    #nb a "rest" of appropriate duration is added after each
    when 32 #space
      note << :r
      dur << ws #for word gap
    when 33 # exclamation mark
      note << [pt,pt,pt,pt]<< :r
      dur << [dah,dah,dah,dit]<< cs
    when 63 #question mark
      note << [pt,pt,pt,pt,pt,pt]<< :r
      dur << [dit,dit,dah,dah,dit,dit]<< cs
    when 46 # fullstop
      note << [pt,pt,pt,pt,pt,pt]<< :r
      dur << [dit,dah,dit,dah,dit,dah]<< cs
    when 44 #comma
      note << [pt,pt,pt,pt,pt,pt]<< :r
      dur << [dah,dah,dit,dit,dah,dah]<< cs
    when 45 # hyphen
      note << [pt,pt,pt,pt,pt,pt]<< :r
      dur << [dah,dit,dit,dit,dit,dah]<< cs
    when 48 #0
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dah,dah,dah,dah,dah]<< cs
    when 49 #1
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dit,dah,dah,dah,dah]<< cs
    when 50 #2
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dit,dit,dah,dah,dah]<< cs
    when 51 #3
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dit,dit,dit,dah,dah]<< cs
    when 52 #4
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dit,dit,dit,dit,dah]<< cs
    when 53 #5
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dit,dit,dit,dit,dit]<< cs
    when 54 #6
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dah,dit,dit,dit,dit]<< cs
    when 55 #7
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dah,dah,dit,dit,dit]<< cs
    when 56 #8
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dah,dah,dah,dit,dit]<< cs
    when 57 #9
      note << [pt,pt,pt,pt,pt]<< :r
      dur << [dah,dah,dah,dah,dit]<< cs
    else #otherwise it is an alphabetic character
      note << lookup[ch - 97][0].flatten << :r #look it up in the lookup array
      #97 = a which is first char in array so subtract 97 to get index 0
      dur << lookup[ch - 97][1].flatten << cs #flatten removes the []
    end
  end


  define :playarray do |narray,darray,shift=0,vol=0.3,voice = :saw|
    with_synth voice do
      narray.zip(darray).each do |narray,darray|
        if narray == :r #if a gap or rest wait the required time
          sleep darray
        else
          with_transpose shift do #allows transposition (may be 0)
            play narray,amp: vol,sustain: darray * 0.9,release: darray * 0.1  #play note
            sleep darray + gs #gap till next note
          end
        end
      end
    end
  end

  playarray(note.flatten,dur.flatten,0) #remove remaining [] and send arrays to playarray, Third param is transpose shift
```
