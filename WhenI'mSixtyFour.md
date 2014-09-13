```Ruby
#Beatles When I'm SixtyFour coded for Sonic-Pi 2 by Robin Newman June 23rd 2014 (reverb added June 29th)

s = 1.0 / 18 #sets the overall tempo increase 18 faster reduce 18 slower
shift = 0 #adjust transposition here
#first set up synth and variables for note lengths
#experiment with differnt synths eg :saw, :saw_s, :pretty_bell, :tri, :tri_s, :prophet
v = :saw #tune
bv = :tri #bass part
a=0.3 #vol
bvol=0.2# bass volume
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
rhn = [:d4,:cs4,:d4,:f4,:d4,:f4,:g4,:f4,:bb4,:bb4,:d5,:bb4,:g4,:c5,:f4,:g4,:a4,:g4,:f4]
rhd = [qd,sq,q,cd,c,qd,sq,q,(q + m),q,c,cd,c,c,qd,sq,qd,sq,c]
#start bar 5
rhn.concat [:a4,:bb4,:b4,:c5,:a4,:bb4,:b4,:c5,:a4,:ab4,:g4,:r,:a4,:bb4,:b4,:c5,:d5,:db5,:c5,:bb4,:r]
rhd.concat [qd,sq,qd,sq,qd,sq,qd,sq,q,c,cd,c,c,c,c,c,q,q,q,cd,c]
#start bar 9
rhn.concat [:d4,:cs4,:d4,:f4,:d4,:f4,:g4,:f4,:bb4,:d5,:d5,:c5,:g4,:bb4]
rhd.concat [qd,sq,q,cd,c,qd,sq,q,(m + q),q,c,cd,c,b]
#start bar 13
rhn.concat [:bb4,:g4,:bb4,:db5,:c5,:bb4,:g4,:bb4,:a4,:g4,:d5,:d5,:d5,:d5,:bb4,:r]
rhd.concat [qd,sq,q,c,cd,qd,sq,q,c,cd,q,cd,q,cd,md,c]
#start bar 17
rhn2 = [:bb4,:g4,:bb4,:g4,:bb4,:g4,:bb4,:g4,:bb4,:g4,:bb4,:g4,:bb4,:eb4,:f4,:f4,:f4,:f4,:g4,:d4,:r]
rhd2 = [qd,sq,qd,sq,qd,sq,qd,sq,qd,sq,qd,sq,c,c,c,qd,sq,c,c,md,c]
#start bar 21
rhn2.concat [:g4,:a4,:bb4,:f5,:d5,:r,:g5,:f5]
rhd2.concat [m,m,m,m,(b + md),c,m,m]
#start bar 25
rhn2.concat [:d5,:c5,:bb4,:c5,:c5,:g4,:bb4,:g4,:f4,:g4,:bb4,:r]
rhd2.concat [m,c,c,m,c,(b + c),m,m,m,m,(b + c),md]
#2nd voice
p2n = [:bb3,:bb3,:bb3,:bb3,:d4,:d4,:eb4,:eb4,:r,:eb4,:r,:eb4,:eb4,:r,:eb4,:r,:f4,:fb4,:eb4,:d4,:r]
p2d = [cd,cd,c,cd,(q + m),b,b,cd,q,cd,q,cd,cd,c,c,md,q,q,q,cd,c]
#start bar 9
p2n.concat [:bb3,:bb3,:bb3,:bb3,:d4,:d4,:d4,:r,:bb3,:c4,:d4,:eb4,:g4,:f4,:f4,:e4,:eb4,:d4,:r]
p2d.concat [cd,cd,c,cd,(q + m),md,c,c,c,c,c,cd,(q + m),cd,(q + m),m,m,md,c]
#start bar 17
p2n2 = [:d4,:d4,:r,:c4,:d4,:r,:d4,:d4,:r,:bb4,:bb4,:d4,:d4,:eb4,:bb3,:bb3,:c4,:eb4,:d4,:r]
p2d2 = [b,md,c,b,md,c,(2 * b),(b + md),c,b,m,c,c,(2 * b),m,m,m,m,(b + c),md]
#3rd voice
p3n = [:f3,:d3,:f3,:d3,:f3,:g3,:bb3,:a3,:a3,:r,:a3,:r,:c4,:c4,:c3,:f3,:g3,:gs3,:a3,:bb3,:bb2]
p3d = [cd,cd,c,cd,cd,c,b,b,c,c,c,c,cd,cd,c,c,c,c,c,md,c]
#start bar 9
p3n.concat [:bb3,:bb3,:bb3,:bb3,:f3,:g3,:ab3,:bb3,:r,:g3,:g3,:bb3,:d4,:b3,:bb3,:a3,:bb3,:f3,:g3,:f3,:bb3,:r]
p3d.concat [cd,cd,c,cd,cd,c,md,c,c,md,cd,(q + m),cd,(q + m),m,m,qd,sq,qd,sq,c,c]
#start bar 17
p3n2 = [:bb3,:bb3,:r,:a3,:bb3,:r,:bb3,:a3,:bb3,:a3,:c4,:a3,:bb3,:a3,:c4,:d4,:d4,:d4,:d4,:d4,:g3,:g3,:c3,:g3,:g3,:g3,:a3,:c4,:bb3,:f3,:g3,:f3,:g3,:bb3,:f3,:fs3,:g3,:gs3,:a3]
p3d2 = [b,md,c,b,md,c,(2 * b),c,c,c,c,c,c,c,c,c,c,c,c,m,m,bd,c,c,m,m,m,m,c,q,(q + qd),sq,c,c,qd,sq,qd,sq,c]
#4th voice
p4n = [:r,:f3,:f3,:f3,:r,:f3,:r,:f3,:a3,:r,:f3,:d3,:f3,:d3,:f3,:r,:f3,:f3,:r,:eb3,:eb3,:fb3,:f3,:g3,:c3,:f3,:r]
p4d = [(2 * b),b,b,c,c,c,c,cd,cd,(c + 2 * b),cd,cd,c,cd,cd,c,md,c,c,md,cd,(q + m),cd,(q + m),m,m,b]
#start bar 17
p4n2 = [:g3,:g3,:r,:f3,:g3,:r,:g3,:fs3,:g3,:fs3,:a3,:fs3,:g3,:fs3,:a3,:g3,:g3,:bb2,:c3,:c3,:g3,:eb3,:eb3,:f3,:a3,:r]
p4d2 = [b,md,c,b,md,c,(2 * b),c,c,c,c,c,c,c,c,b,m,m,bd,c,c,m,m,m,m,(2 * b)]
#voice 5
p5n = [:bb2,:f2,:bb2,:f2,:bb2,:f2,:f2,:r,:f2,:r,:f2,:r,:f2,:r,:bb2,:f2,:bb2,:f2,:bb2,:bb2,:eb2,:eb2,:fb2,:f2,:g2,:c2,:r,:f2,:r,:bb2,:r]
p5d = [md,c,md,c,b,b,c,c,c,c,md,c,c,(md + b),md,c,md,c,md,c,b,cd,(q + m),cd,(q + m),c,c,c,c,m,m]
#start bar 17
p5n2 = [:g2,:g2,:g2,:g2,:g2,:g2,:g2,:r,:f2,:g2,:g2,:g2,:r,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:d2,:d2,:d2,:d2,:d2,:d2,:g2,:g2,:g2,:g2]
p5d2 = [c,c,c,c,c,c,c,c,b,c,c,c,c,c,c,c,c,c,c,c,c,m,c,c,m,c,c,c,c,c,c]
#start bar 26
p5n2.concat [:g2,:g2,:g2,:c2,:c2,:c2,:c2,:c2,:c2,:c2,:c2,:eb2,:eb2,:eb2,:eb2,:f2,:f2,:f2,:f2,:bb2,:bb2,:bb2,:bb2,:bb2,:r]
p5d2.concat [c,c,m,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,md]

define :v1 do
  in_thread do
    playarray(rhn,rhd,shift,a,v)
  end
  in_thread do
    playarray(p2n,p2d,shift,a,v)
  end
  in_thread do
    playarray(p3n,p3d,shift,a,v)
  end
  in_thread do
    playarray(p4n,p4d,shift,a,v)
  end
  playarray(p5n,p5d,shift,bvol,bv)
end
define :v2 do
  in_thread do
    playarray(rhn2,rhd2,shift,a,v)
  end
  in_thread do
    playarray(p2n2,p2d2,shift,a,v)
  end
  in_thread do
    playarray(p3n2,p3d2,shift,a,v)
  end
  in_thread do
    playarray(p4n2,p4d2,shift,a,v)
  end
  playarray(p5n2,p5d2,shift,bvol,bv)
end
#start playing here===================
with_fx :reverb, room: 0.7 do #add some reverb to make it sound nicer
  v1
  v2
  v1
  v2
  v1
end
```
