```Ruby
#Beatles Eleanor Rigby re-coded for Sonic-Pi 2 by Robin Newman June 23rd 2014 (reverb added June 29th)
set_sched_ahead_time! 2 #adjust as necessary: less for a Mac
s = 1.0 / 20 #sets the overall tempo increase 20 faster reduce 20 slower

#first set up synth and variables for note lengths
v = :tri #tune
bv = :saw #bass part
#not all note lengths are used
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
a = 0.3 #main vol
bvol = 0.2 #bass vol
#procedure to play an array of notes and associated array of durations
#shift allows for transposing, vol for volume and voice for synth used
define :playarray do |notearray,durationarray,shift=0,vol=0.5,voice = :saw|
  with_synth voice do
    notearray.zip(durationarray).each do |notearray,durationarray| #traverse both arrays together
      if notearray == :r #if rest just wait duration
        sleep durationarray
      else
        with_transpose shift do #allows transposition (may be 0) for the part
          play notearray,amp: vol,sustain: durationarray * 0.9,release: durationarray * 0.1 #play note
          sleep durationarray #gap till next note
        end
      end
    end
  end
end

#define arrays containing notes and durations
#first the intro
rhn = [:e5,:fs5,:g5,:a5,:g5,:fs5,:e5,:b4,:a4,:g4,:a4,:b4,:g4,:b4,:a4,:b4,:g4,:b4,:e4,:b4]
rhd = [(m + q),q,q,c,c,c,c,qd,sq,(cd + c),q,q,q,q,q,q,q,q,q,q]


lhn = [:c4,:g3,:c4,:e3,:g3, :b3, :r,:r]
lhd = [c,c,(m + b),c,c,c,c,b]

bassn = [:c2,:e2]
bassd = [(b + b),(b + b)]
#verses 1 and 2 minus last two bars
rhnv = [:g4,:a4,:b4,:g4,:e4,:g4,:a4,:b4,:d5,:cs5,:b4,:cs5,:b4,:a4,:b4,:a4,:g4,:a4,:g4,:a4,:b4,:c5,:b4]
rhdv = [q,q,q,c,cd,q,q,q,c,q,q,c,q,q,c,q,q,(q + b),q,q,q,cd,c]
#bar 10 start
rhnv.concat [:g4,:a4,:b4,:g4,:e4,:g4,:a4,:b4,:d5,:cs5,:b4,:cs5,:b4,:a4,:b4,:a4,:g4,:a4]
rhdv.concat [q,q,q,c,cd,q,q,q,c,q,q,c,q,q,c,q,q,(q + b)]
#bar 14 start
rhnv.concat [:g4,:a4,:b4,:c5,:b4,:a4,:g4,:a4,:b4,:g4,:e4,:e4,:e5,:b4,:a4,:g4,:g4,:e4]
rhdv.concat [q,q,q,cd,c,c,q,cd,q,c,(cd + c),q,cd,q,c,q,q,(q + b)]
#bar 19 start
rhnv.concat [:a4,:g4,:a4,:b4,:g4,:e4,:e4,:g5]
rhdv.concat [c,q,cd,q,c,(cd + c),q,cd]

lhnv = [:e3,:g3,:b3,:g3,:e3,:g3,:b3,:g3,:e3,:g3,:b3,:g3,:c4,:e4,:g3,:e4,:c4,:e4,:b3,:g3]
lhdv = [c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c]
#bar 10 start
lhnv.concat [:e3,:g3,:b3,:g3,:e3,:g3,:b3,:g3,:e3,:g3,:b3,:g3,:c4,:e4,:g3,:e4,:c4,:e4,:b3,:g3]
lhdv.concat [c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c]
#bar 15 start
lhnv.concat [:e3,:g3,:d4,:e3,:g3,:cs4,:e3,:g3,:c4,:e3,:g3,:b3,:g3,:e3,:g3,:d4,:e3,:g3,:cs4,:e3]
lhdv.concat [c,c,m,c,c,m,c,c,m,c,c,c,c,c,c,m,c,c,m,c]
#bass section all verse bars
bassnv = [:e2,:c2,:e2,:c2,:e2,:e2]
bassdv = [(b + b +b),bd,(m + 3 * b),bd,(m + 4 * b),(4 * b)]
#v1 and 2 last two bars
rhrbna = [:e5,:b4,:a4,:a4,:g4]
rhrbda = [q,c,q,q,(q + b)]

lhrbna = [:g3,:c4,:e3,:g3,:b3,:g3]
lhrbda = [c,m,c,c,c,c]
#v3 last two bars
rhrbnb = [:e5,:b4,:a4,:a4,:g4,:e5,:b4,:a4,:g4]
rhrbdb = [q,c,q,q,cd,c,c,c,b]

lhrbnb = [:g3,:c4,:g3,:g3]
lhrbdb = [c,m,b,b]
#set up the sections of the piece ==================================
define :intro do
  2.times do
    in_thread do
      playarray(rhn, rhd,0,a,v)
    end
    in_thread do
      playarray(lhn,lhd,0,a,v)
    end
    playarray(bassn,bassd,0,bvol,bv)
  end
end


define :v12 do
  in_thread do
    in_thread do
      playarray(rhnv,rhdv,0,a,v)
    end
    playarray(lhnv,lhdv,0,a,v)
    in_thread do
      playarray(rhrbna,rhrbda,0,a,v)
    end
    playarray(lhrbna,lhrbda,0,a,v)
  end
  playarray(bassnv,bassdv,0,bvol,bv)
end

define :v3 do
  in_thread do
    in_thread do
      playarray(rhnv,rhdv,0,a,v)
    end
    playarray(lhnv,lhdv,0,a,v)
    in_thread do
      playarray(rhrbnb,rhrbdb,0,a,v)
    end
    playarray(lhrbnb,lhrbdb,0,a,v)
  end
  in_thread do
    playarray(bassnv,bassdv,0,bvol,bv)
  end
  sleep 17 * b
  use_synth v
  play_chord [:e3,:b3],sustain: (b * 0.9),release: (b * 0.1),amp: bvol
  sleep b
  play_chord [:e2,:e3,:b3],sustain: (b * 0.9),release: (b * 0.1),amp: bvol
end
#playing starts here ========================

with_fx :reverb,room: 0.6 do
  intro
  v12
  v12
  v3
end
set_sched_ahead_time! 0.5 #reset
```
