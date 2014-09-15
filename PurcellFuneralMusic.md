```Ruby
#The Queen's Funeral Music by Henry Purcell transcribed by Robin Newman for Sonic Pi 2. Sept 2014
#REQUIRES SAMPLE :d1 TO BE INSTALLED ON YOUR SYSTEM
#combines synth for instruments and a sample for the drum
#also features trills and rits
set_sched_ahead_time! 4 #on RPi 4, Mac 0.25
#on an RPi turn off Print output in prefs
#select the appropriate line below for your system, and adjust where you have saved the sample d1
#use_sample_pack '/users/rbn/Desktop/samples' #comment out when using Raspberry Pi
use_sample_pack '/home/pi/samples' #comment out when using a Mac
load_sample :d1

use_synth :saw
s = 1.0 / 8#s is speed multiplier to give correct tempo
#note use of 1.0 to make the variable floating point and not integer

sh=0 #transpose shift. Can alter if you want

#The following are variables for note durations values defined in setup function
dsq=sq=sqd=q=qd=qdd=c=cd=cdd=m=md=mdd=b=bd=1*s #declare note duration variables here to make them global

define :setup do |s| #actual note timings defined here
  dsq = 1 * s #demi-semi-quaver
  sq = 2 * s #semi-quaver
  sqd = 3 * s #semi-quaver dotted
  q = 4 * s #quaver
  qd = 6 * s #quaver dotted
  qdd = 7 * s #quaver double dotted
  c = 8 * s #crotchet
  cd = 12 * s #crotchet dotted
  cdd = 14 * s #crotchet double dotted
  m = 16 * s #minim
  md = 24 * s #minim dotted
  mdd = 28 * s #minim double dotted
  b = 32 * s #breve
  bd = 48 * s #breve dotted
end

define :bass4 do |x|
  i=1
  4.times do
    sample :d1,amp:1+ i,start: 0.01,finish: 1
    sleep x/4
    i=i+1
  end
end

define :bass1 do |x,amp = 4|
  sample :d1,amp: amp,start: 0.01,finish: 1
  sleep x
end

define :rbass do
  bass1(cd,4)
  bass1(q,2)
  bass1(c,3)
  bass1(c,4)
end

define :tune do|pitch,duration,shift=0,amp= 0.3,ratio = 0.9|
  pitch.zip(duration).each do |p,d|
    if p == :r
      sleep d
    else
      with_transpose shift do
        play p,sustain: ratio*d,release: (1-ratio)*d,attack: 0,amp: amp
        sleep d
      end
    end
  end
end

define :trn do |n,num,offset=2| #produces trill sequence using n  and tone (or semitone) above: n notes total
  n = note_info(n).midi_note
  return [n+offset,n]*(num / 2) #trill set to start on upper note. Can be swapped over
end
define :trd do |d,num| #produces trill note durations. d is total duration, num number of notes
  return [d/num]*num
end

setup(1.0/8)#check values of durations correct to set up the arrays
n1 = [:g4,:ab4]+trn(:ab4,14)+[:g4,:ab4,:g4,:r,:g4,:c5,:c5,:b4,:r,:d5,:c5,:a4,:bb4,:r,:bb4,:ab4,:f4,:g4,:r,:c5,:c5,:b4,:c5]
d1 = [b,m]+trd(c+qd,14)+[dsq,dsq,b,b,b,m,m,b,b,b,m,m,b,b,b,m,m,b,b,b,m,m,b*1.2] #add pause on last note
n2 = [:e4,:f4,:f4,:e4,:r,:eb4,:eb4,:f4,:g4,:r,:bb4,:a4,:fs4,:g4,:r,:g4,:f4,:d4,:eb4,:r,:g4,:ab4,:g4,:e4]
d2 = [b,m,m,b,b,b,m,m,b,b,b,m,m,b,b,b,m,m,b,b,b,m,m,b*1.2]
n3 = [:c4,:c4,:c4,:c4,:r,:c4,:c4,:c4,:d4,:r,:g4,:eb4,:d4,:d4,:r,:eb4,:c4,:bb3,:bb3,:r,:eb4,:d4,:d4,:c4]
d3 = [b,m,m,b,b,b,m,m,b,b,b,m,m,b,b,b,m,m,b,b,b,m,m,b*1.2]
n4 = [:c3,:f3,:f3,:c3,:r,:c3,:ab3,:ab3,:g3,:r,:g3,:c4,:d4,:g3,:r,:eb3,:ab3,:bb3,:eb3,:r,:c3,:f3,:g3,:c3]
d4 = [b,m,m,b,b,b,m,m,b,b,b,m,m,b,b,b,m,m,b,b,b,m,m,b*1.2]

define :sec1 do |x|
  #drum part solo bar
  bass4(b)

  in_thread do #now start synced parts together
    i=1
    5.times do #drum part
      rbass
      bass1(m)
      bass1(m)
      bass4(b)
      if i<5
        rbass
      end
      i=i+1
    end
  end
  sleep 0.02 #sync bodge to get timing right with drums
  in_thread do #start synth parts after sync timing
    tune(n1,d1,sh,x,0.95)
  end
  in_thread do
    tune(n2,d2,sh,x,0.95)
  end
  in_thread do
    tune(n3,d3,sh,x,0.95)
  end
  tune(n4,d4,sh,x,0.95 )
end

setup(1.0/16) #set timings for canzon (a bit faster)
define :trsetup do #timings for trill in section 1
  y=dsq
  yd=[dsq]
  total=dsq #for debugging
  inc=(2*c-12*dsq) #increase in time per note calculated
  inc=inc/66 #on these two lines
  11.times do
    y=y + inc
    total=total+y
    yd = yd + [y]
  end
  #puts total
  return yd
end

n1b = [:r,:c5,:c5,:c5,:d5,:eb5,:d5,:eb5,:b4,:c5,:d5,:eb5,:f5,:b4,:b4,:g5,:g5,:r,:f5,:f5,:r,:eb5,:eb5,:r,:d5,:d5]
d1b = [b,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,m,c,c,m,c,c,m,c,c]
n1b.concat [:r,:b4,:c5,:d5,:eb5,:d5,:eb5,:f5,:g5,:g5,:g5,:g5,:ab5,:g5,:ab5,:f5,:g5,:f5,:g5,:eb5,:f5,:eb5,:f5,:g5,:eb5,:d5]+trn(:d5,12,1)+[:c5,:c5]
d1b.concat [c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c*1.1,c*1.2]+trsetup+[q*1.4,b*1.2]
n2b = [:g4,:g4,:g4,:g4,:ab4,:g4,:ab4,:f4,:g4,:f4,:g4,:d4,:eb4,:f4,:g4,:ab4,:g4,:g4,:r,:c5,:c5,:r,:bb4,:bb4,:r,:ab4,:ab4,:r]
d2b = [c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,m,c,c,m,c,c,m,c,c,m]
n2b.concat [:g4,:g4,:r,:c5,:c5,:c5,:d5,:eb5,:d5,:eb5,:c5,:f5,:eb5,:f5,:d5,:eb5,:d5,:eb5,:c5,:d5,:c5,:d5,:d5,:g4,:a4,:g4,:g4,:e4]
d2b.concat [c,c,m,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c*1.1,c*1.2,c*1.3,c*1.4,b*1.2]
n3b = [:r,:c4,:c4,:c4,:d4,:eb4,:d4,:eb4,:b3,:c4,:eb4,:f4,:c4,:eb4,:d4,:eb4,:c4,:d4,:c4,:d4,:f4]
d3b = [b*3,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c]
n3b.concat [:eb4,:d4,:eb4,:f4,:g4,:g4,:r,:eb4,:eb4,:r,:d4,:d4,:r,:c4,:c4,:c4,:c4,:b3,:b3,:c4,:c4,:b3,:b3,:c4]
d3b.concat [c,c,c,c,c,c,b,c,c,m,c,c,m,c,c,c,c,c,c,c*1.1,c*1.2,c*1.3,c*1.4,b*1.2]
n4b = [:r,:g3,:g3,:g3,:g3,:ab3,:g3,:ab3,:f3,:g3,:f3,:g3,:eb3,:f3,:eb3,:f3,:bb2]
d4b = [4*b,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c]
n4b.concat [:eb3,:eb3,:eb4,:d4,:c4,:c4,:r,:c4,:c4,:r,:f3,:f3,:r,:eb3,:eb3,:r,:ab3,:f3,:ab3,:g3,:g3,:c4,:f3,:g3,:g3,:c3]
d4b.concat [c,c,c,c,c,c,m,c,c,m,c,c,m,c,c,c,c,c,c,c,c,c*1.1,c*1.2,c*1.3,c*1.4,b*1.2]
define :sec2 do |x|
  in_thread do
    with_merged_synth_defaults pan: -0.7 do
      tune(n1b,d1b,sh,x,0.95)
    end
  end
  in_thread do
    with_merged_synth_defaults pan: 0.7 do
      tune(n2b,d2b,sh,x,0.95)
    end
  end
  in_thread do
    with_merged_synth_defaults pan: -0.4 do
      tune(n3b,d3b,sh,x,0.95)
    end
  end
  with_merged_synth_defaults pan: 0.4 do
    tune(n4b,d4b,sh,x,0.95)
  end
end

#set up trill durations with rall used in part n1c
define :ysetup do |t| #change t to 2 for smaller rit
  y=dsq
  yd=[dsq]
  total=dsq
  inc = c*1.2/t/276 #increase in time per note
  23.times do
    y=y + inc
    total=total+y
    yd = yd + [y]
  end
  puts total
  return yd
end

#define arrays for sec3 here to make them global
tr=dr=n1c=n2c=n3c=n4c=d1c=d2c=d3c=d4c=[]

define :setuppart3 do |n| #change n 1 or 2 for 1st time 2nd time
  case n
  when 1
    p2=1.5
    p3=1.3
    p4=1.7
    p5=1.2
    p6=1.4
    p7=1.6
    p8=1.8
  when 2
    p2=1.2
    p3=1.15
    p4=1.35
    p5=1.1
    p6=1.2
    p7=1.3
    p8=1.4
  end
  #last two bars for part n1c with trill and rall
  tr = trn(:d5,24,1)+[:c5,:c5]
  dr = ysetup(n) + [c*p8,b*p2]

  n1c = [:g5,:g5,:g5,:g5,:ab5,:g5,:ab5,:f5,:g5,:f5,:g5,:eb5,:f5,:eb5,:f5,:g5,:eb5,:ab5,:ab5,:g5,:g5,:r,:f5,:f5,:r,:eb5,:eb5,:r]
  d1c = [c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,m,c,c,c,c,m,c,c,m,c,c,m]
  n1c.concat [:d5,:c5,:bb4]+trn(:a4,12,1)+[:g4,:g4,:g4,:g5,:g5,:d5,:d5,:eb5,:eb5,:b4,:b4,:c5,:c5,:d5,:d5,:d5,:d5,:d5,:d5,:d5,:d5] #add last 2 bars with trill here
  d1c.concat [c,q,q]+trd(cd,12)+[q,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c] #add  duration last 2 bars with trill
  n2c = [:r,:c5,:c5,:c5,:d5,:eb5,:d5,:eb5,:c5,:d5,:c5,:d5,:b4,:c5,:c5,:c5,:c5,:r,:c5,:c5,:r,:bb4,:bb4,:r,:ab4,:ab4]
  d2c = [b,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,m,c,c,m,c,c,m,c,c]
  n2c.concat [:r,:g4,:g4,:fs4,:g4,:g4,:eb5,:eb5,:b4,:b4,:c5,:c5,:d5,:d5,:eb5,:eb5,:b4,:b4,:c5,:c5,:c5,:c5,:c5,:c5,:c5,:b4,:c5]
  d2c.concat [c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,m*p3,m*p4,b*p2]
  n3c = [:r,:g4,:g4,:g4,:g4,:ab4,:g4,:ab4,:f4,:g4,:f4,:g4,:eb4,:f4,:eb4,:f4,:d4,:eb4,:d4,:eb4,:c4]
  d3c = [b*3,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c]
  n3c.concat [:d4,:eb4,:d4,:d4,:b3,:b3,:r,:r,:g4,:g4,:d4,:d4,:c4,:eb4,:g4,:g4,:f4,:f4,:g4,:g4,:ab4,:ab4,:g4,:g4,:g4,:g4,:e4]
  d3c.concat [c,c,c,c,c,c,m,m,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c*p5,c*p6,c*p7,c*p8,b*p2]
  n4c = [:r,:c4,:c4,:c4,:d4,:eb4,:d4,:eb4,:c4,:d4,:c4,:d4,:bb3,:c4,:bb3,:c4,:ab3]
  d4c = [b*4,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c]
  n4c.concat [:bb3,:c4,:d4,:d4,:g3,:g3,:r,:g3,:g3,:r,:c3,:g3,:g3,:c4,:c4,:g3,:g3,:ab3,:ab3,:e3,:e3,:f3,:f3,:g3,:g3,:g3,:g3,:c3]
  d4c.concat [c,c,c,c,c,c,m,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c*p5,c*p6,c*p7,c*p8,b*p2]
end

define :sec3 do |x|
  in_thread do
    with_merged_synth_defaults pan: -0.7 do
      tune(n1c,d1c,sh,x,0.95)
      tune(tr,dr,sh,x,0.95)
    end
  end
  in_thread do
    with_merged_synth_defaults pan: 0.7 do
      tune(n2c,d2c,sh,x,0.95)
    end
  end
  in_thread do
    with_merged_synth_defaults pan: -0.4 do
      tune(n3c,d3c,sh,x,0.95)
    end
  end
  with_merged_synth_defaults pan: 0.4 do
    tune(n4c,d4c,sh,x,0.95)
  end
end

#===================play funeral march then cadenza ==================

with_fx :reverb, home: 0.8 do
  setup(1.0/8)#set timings
  sec1(0.6)
  setup(1.0/16)
  sleep b
  sec2(0.4)
  sleep q
  sec2(0.2)
  sleep q
  setuppart3(2)#short rit set up the arrays
  sec3(0.4)
  setuppart3(1)#long rit set up the arrays
  sec3(0.8)
end
```