```Ruby
#Sir John Souch his Galiard John Dowland transcribed for Sonic Pi 2 by Robin Newman July 2014

s = 1.0 / 16 #s is speed multiplier 1.0 / 8 makes crotchet 1s or 60 crotchets/minute the default bpm
#note use of 1.0 to make the variable floating point and not integer
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
b = 32 * s #brevea
bd = 48 * s #breve dotted

use_synth :saw #default

define :tune do|pitch,duration,shift=0,amp= 0.3|
  pitch.zip(duration).each do |p,d| #zip arrays together and traverse them
    if p == :r #if a rest, then just wait
      sleep d
    else #otherwise play a note and wait
      with_transpose shift*12 do #transpose if set
        play p,sustain: 0.95*d-q/25,release: 0.05*d,attack: q/25,amp: amp
        sleep d
      end
    end

  end
end
#arryas of note pitches and durations follow
n1a = [:c5,:b4,:a4,:gs4,:a4,:b4,:c5,:a4,:gs4,:e5,:d5,:c5,:b4,:g4,:c5,:b4,:a4,:gs4,:a4]
d1a = [m,m,m,md,c,m,m,b,bd,m,m,m,b,c,c+c,m,m,c,bd]
n2a = [:e4,:e4,:e4,:e4,:e4,:e4,:e4,:d4,:e4,:g4,:g4,:g4,:g4,:d4,:e4,:f4,:g4,:e4,:e4,:e4]
d2a = [m,m,m,b,m,md,c,m,bd,m,m,m,m,m,cd,q,m,md,c,bd]
n3a = [:a4,:b4,:c5,:b4,:e4,:e5,:d5,:c5,:b4,:a4,:b4,:c5,:d5,:f5,:e5,:d5,:c5,:b4,:d5,:c5,:d5,:g4,:a4,:b4,:cs5]
d3a = [m,m,m,m,b,m,c,c,c,c,bd,m,c,c,m,cd,q,c,c,m,c,m,c,m,bd]
n4a = [:e4,:e4,:fs4,:gs4,:a4,:b4,:g4,:c5,:a4,:b4,:c5,:d5,:r,:b4,:e5,:e4,:r,:e4,:g4,:a4,:b4,:g4,:g4,:d5,:c5,:b4,:a4]
d4a = [m,md,c,c,c,md,c,c,m,q,q,m,c,c,m,m,c,c,md,c,m,b,c,m,c,m,bd]
n5a = [:a2,:gs2,:a2,:e3,:f3,:g3,:a3,:f3,:e3,:c3,:b2,:c3,:g3,:f3,:e3,:b2,:c3,:a2,:e3,:a2]
d5a = [m,m,m,md,c,m,m,b,bd,m,m,m,md,c,m,m,c,c,m,bd]
define :sec1 do |vol| #first section
  in_thread do #play the various parts together using threads
    tune(n1a,d1a,0,vol)
  end
  in_thread do
    tune(n2a,d2a,0,vol)
  end
  in_thread do
    tune(n3a,d3a,-1,vol*1.2)
  end
  in_thread do
    tune(n4a,d4a,-1,vol*1.2)
  end
  tune(n5a,d5a,0,vol)
end
#define notes and durations for the second section
n1b = [:g4,:a4,:g4,:f4,:e4,:d4,:e4,:f4,:g4,:c5,:b4,:c5,:a4,:b4,:c5,:d5,:c5,:b4,:a4,:gs4]
d1b = [m,m,cd,q,cd,q,c,c,m,b,m,bd,m,m,m,b,m,m,b,bd]
n2b = [:e4,:g4,:f4,:e4,:d4,:c4,:d4,:e4,:f4,:g4,:f4,:e4,:f4,:a4,:g4,:f4,:e4,:f4,:a4,:g4,:f4,:e4,:f4,:d4,:f4,:e4,:e4,:d4,:e4]
d2b = [c,c,c,c,m,md,c,cd,q,c,q,q,c,c,cd,q,bd,c,c,md,q,q,md,c,c,c,b,m,bd]
n3b = [:b4,:e5,:d5,:c5,:b4,:c5,:b4,:a4,:g4,:g4,:d5,:c5,:c5,:f5,:d5,:e5,:a4,:d5,:b4,:c5,:d5,:e5,:d5,:c5,:b4,:a4,:b4]
d3b = [c,c,c,m,c,md,q,q,m,m,b,bd,c,c,c,m,c+c,m,c,cd,q,c,m,m,q,q,bd]
n4b = [:g4,:c5,:a4,:a4,:d5,:g4,:a4,:b4,:c5,:c5,:d5,:e5,:a4,:a4,:b4,:g4,:g4,:a4,:f4,:g4,:a4,:b4,:g4,:c5,:a4,:a4,:b4,:c5,:b4,:g4,:a4,:e5,:e4]
d4b = [c,c,cd,q,c,c+c,q,q,m,cd,q,m,cd,q,c,c,bd,c,c,q,q,q,q,m,m,m+q,q,c+q,q,c,b,m,b]
n5b = [:e3,:f3,:g3,:c3,:e3,:d3,:c3,:f3,:d3,:g3,:c3,:f3,:d3,:g3,:e3,:a3,:d3,:e3,:f3,:g3,:a3,:g3,:f3,:e3]
d5b = [m,m,m,b+c,c+c,q,q,c,c,m,bd,c,c,c,c,m,cd,q,cd,q,m,m,b,bd]
define :sec2 do |vol| #define section 2 play
  in_thread do #play the 5 parts using 4 threads and one direct
    tune(n1b,d1b,0,vol)
  end
  in_thread do
    tune(n2b,d2b)
  end
  in_thread do
    tune(n3b,d3b,-1,vol*1.2)
  end
  in_thread do
    tune(n4b,d4b,-1,vol*1.2)
  end
  tune(n5b,d5b,0,vol)
end
#define section 3 notes and durations
n1c = [:r,:e5,:c5,:e5,:d5,:r,:c5,:a4,:c5,:b4,:c5,:b4,:a4,:gs4,:r,:r,:d5,:b4,:g4,:c5,:a4,:b4,:a4,:gs4,:a4,:b4,:a4]
d1c = [c,c,c,m,c,c,c,c,m,c,md,c,m,b,m,c,c,c,c,c,c,b,m,md,c,m,bd]
n2c = [:r,:g4,:e4,:g4,:f4,:e4,:f4,:a4,:g4,:f4,:e4,:e4,:d4,:e4,:r,:a4,:g4,:g4,:g4,:fs4,:g4,:d4,:e4,:d4,:e4,:e4,:e4]
d2c = [c,c,c,m,c,m,c,m,c+c,cd,q,m,c,b,c,c,m,c,m,c,m,c,m,c,b,m,bd]
n3c = [:c5,:c5,:b4,:c5,:e5,:d5,:e5,:e5,:e5,:d5,:c5,:b4,:a4,:b4,:c5,:d5,:e5,:d5,:d5,:b4,:g4,:c5,:a4,:b4,:a4,:gs4,:a4]
d3c = [md,c,m,c,c,c,cd,q,c+c,c,md,q,q,b,m,md,m,c+c,c,c,c,c,c,md,m,c,bd]
n4c = [:g4,:g4,:r,:c4,:f4,:e4,:f4,:g4,:a4,:a4,:d5,:b4,:e4,:b4,:e4,:a4,:g4,:r,:d5,:b4,:e5,:b4,:c5,:b4,:b4,:cs5]
d4c = [b,m,c,c,c,cd,q,c,m,md,c,m,b,md,c,m,b,c,c,c,cd,q,c,cd,q,bd]
n5c = [:c3,:g2,:a2,:d3,:a2,:e3,:a3,:g3,:f3,:e3,:d3,:c3,:b2,:g2,:c3,:a2,:d3,:g2,:e3,:c3,:f3,:e3,:a2]
d5c = [b,m,m,c,c,m,md,c,m,md,c,m,m,c,c,c,c,md,c,c,c,bd,bd]
define :sec3 do |vol| #define section 3 play
  in_thread do
    tune(n1c,d1c,0,vol)
  end
  in_thread do
    tune(n2c,d2c,0,vol)
  end
  in_thread do
    tune(n3c,d3c,-1,vol*1.2)
  end
  in_thread do
    tune(n4c,d4c,-1,vol*1.2)
  end
  tune(n5c,d5c,0,vol)
end
with_fx :reverb,room: 0.6 do #add some reverb
  sec1(0.4) #play sectgions adjusting volumes
  sec1(0.2)
  with_synth :tri do #change synth for middle section
    sec2(0.4)
    sec2(0.2)
  end
  sec3(0.2)
  sec3(0.4)
end
```
