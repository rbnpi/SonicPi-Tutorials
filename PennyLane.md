```Ruby
#Beatles Penny Lane coded by Robin Newman for Sonic-Pi 2 24th June 2014
#checked on release version 2.0
s = 1.0 / 17 #sets the overall tempo increase 17 faster reduce 17 slower

#first set up synth and variables for note lengths
#experiment with different synths eg :saw,  :pretty_bell, :tri, :prophet
v = :saw #tune synth
bv = :tri #bass part synth
t = 4 #set transpose
bvol = 0.6 #volume of bass part
a = 0.3 #vol
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
define :playarray do |notearray,durationarray,shift=0,vol=1,voice = :saw|
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

#define arrays containing notes and durations ===================
p1nub = [:d4,:g4,:a4]
p1dub = [q,q,q]
p1n = [:b4,:a4,:g4,:fs4,:g4,:fs4,:e4,:d4,:e4,:d4,:c4,:r,:d4,:g4,:a4,:b4,:a4,:g4,:fs4,:g4,:d4,:e4,:g4]
p1d = [q,q,q,q,q,q,q,q,q,q,c,q,q,q,q,q,q,q,q,q,q,q,q]
#start bar 4
p1n.concat [:f4,:d4,:g4,:a4,:a4,:g4,:g4,:g4,:g4,:a4,:bb4,:g4,:a4,:bb4,:g4,:a4,:r]
p1d.concat [(m + q),q,q,q,q,q,q,q,c,c,md,q,q,q,q,c,m]
#1st and second time bars 1
p1nrb1a = [:b4,:g4,:a4,:r,:g4,:a4]
p1drb1a = [q,q,c,c,q,q]
p1nrb1b = [:a4,:e4,:g4,:r,:a4,:bb4]
p1drb1b = [q,q,c,c,q,q]
#2nd part

p2n = [:r,:d4,:r,:g4,:r]
p2d = [(3 * b),(m + q),(3 * q + b),md,(c + b)]

p3n = [:b3,:g3,:fs3,:c4,:b3,:bb3,:bb3,:r,:d4,:d4,:r,:c4,:d3,:c4]
p3d = [b,m,c,c,b,m,q,cd,b,md,c,m,c,c]
p3nrb1a = [:c4,:c4,:d3]
p3drb1a = [m,c,c]
p3nrb1b = [:c4,:r]
p3drb1b = [md,c]

p4n = [:g3,:e3,:r,:g3,:g3,:g3,:r,:bb3,:eb3,:g3,:bb3,:r,:d3,:r,:fs3]
p4d = [b,m,m,b,m,q,cd,b,c,c,c,c,m,c,c]
p4nrb1a = [:d3,:fs3,:r]
p4drb1a = [m,c,c]
p4nrb1b = [:e3,:r]
p4drb1b = [md,c]

p5n = [:g2,:r,:e2,:r,:a2,:r,:d2,:r,:g2,:r,:e2,:r,:g2,:r,:g2,:r,:e2,:r,:e2,:r,:eb2,:r,:d2,:r,:d2,:r]
p5d = [c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,md,c,c,c,c,c]
p5nrb1a = [:d2,:r,:d2,:r]
p5drb1a = [c,c,c,c]
p5nrb1b = [:c2,:r,:c2,:r]
p5drb1b = [c,c,c,c]

#second section a
p1n2 = [:c5,:bb4,:a4,:bb4,:c5,:bb4,:a4,:g4,:f4,:r,:c5,:bb4,:a4,:bb4,:c5,:bb4,:a4,:g4,:f4,:g4,:a4,:bb4]
p1d2 = [cd,q,cd,q,cd,q,c,c,(b + c),md,cd,q,cd,q,cd,q,c,c,c,c,c,c]
p1nrb2a = [:r,:a4,:bb4]
p1drb2a = [md,q,q]
p1nrb2b = [:r,:d4,:g4,:a4]
p1drb2b = [(m + q),q,q,q]

p2n2 = [:a4,:g4,:f4,:g4,:a4,:g4,:f4,:r,:d4,:r,:a4,:g4,:f4,:g4,:a4,:g4,:f4,:r,:d4]
p2d2 = [cd,q,cd,q,cd,q,c,c,(b + c),md,cd,q,cd,q,cd,q,c,c,b]
p2nrb2a = [:c4,:r]
p2drb2a = [md,c]

p3n2 = [:c4,:r,:c4,:r,:bb3]
p3d2 = [(b + md),(c + 2 * b),(b + md),c,b]
p3nrb2a = [:g3,:r]
p3drb2a = [md,c]
p3nrb2b = [:c4,:d3,:c4,:r]
p3drb2b = [c,c,c,c]

p4n2 = [:f3,:r,:bb3,:f3,:g3,:a3,:bb3,:f3,:d4,:bb3,:f4,:r]
p4d2 = [(b + md),c,cd,q,c,c,c,c,c,c,(b + md),(c + b)]
p4nrb2a = [:e3,:r]
p4drb2a = [m,c]
p4nrb2b = [:fs3,:r,:fs3,:r]
p4drb2b = [c,c,c,c]

p5n2 = [:f2,:r,:bb2,:r,:f2,:r,:bb2,:r,:f2,:r,:f2,:r,:bb2]
p5d2 = [(b + md),c,c,c,c,c,c,c,c,c,(b + md),c,b]
p5nrb2a = [:c2,:r,:c2,:r]
p5drb2a = [c,c,c,c]
p5nrb2b = [:d2,:r,:d2,:r]
p5drb2b = [c,c,c,c]

#second section b
p1n3 = [:a4,:fs4,:d4,:d4,:g4,:a4,:b4,:a4,:g4,:fs4,:g4,:fs4,:e4,:d4,:e4,:d4,:c4,:r,:d4,:g4,:a4]
p1d3 = [c,c,q,q,q,q,q,q,q,q,q,q,q,q,q,q,c,q,q,q,q]
p1n3.concat [:b4,:a4,:g4,:fs4,:g4,:d4,:e4,:g4,:f4,:d4,:g4,:a4,:a4,:g4,:g4,:g4,:g4,:a4,:bb4,:g4,:a4,:bb4,:g4,:a4,:r]
p1d3.concat [q,q,q,q,q,q,q,q,(m + q),q,q,q,q,q,q,q,c,c,md,q,q,q,q,c,m]

p2n3 = [:r,:d4,:r,:g4,:r]
p2d3 = [(4 * b),(m + q),(3 * q + b),md,(c + b)]

p3n3 = [:c4,:r,:b3,:g3,:fs3,:c4,:b3,:bb3,:r,:d4,:d4,:r,:c4,:d3,:c4]
p3d3 = [md,c,b,m,c,c,b,(m + q),cd,b,md,c,m,c,c]

p4n3 = [:fs3,:r,:g3,:e3,:r,:g3,:g3,:r,:bb3,:eb3,:g3,:bb3,:r,:d3,:r,:fs3]
p4d3 = [md,c,b,m,m,b,(m + q),cd,b,c,c,c,c,m,c,c]

p5n3 = [:a2,:r,:d2,:r,:g2,:r,:e2,:r,:a2,:r,:d2,:r,:g2,:r,:e2,:r,:g2,:r,:g2,:r,:e2,:r,:e2,:r,:eb2,:r,:d2,:r,:d2,:r]
p5d3 = [c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,c,md,c,c,c,c,c]



#coda
p1n4 = [:a4,:fs4,:d4,:b4,:c5,:d5,:c5,:b4,:c5,:d5,:c5,:b4,:c5,:g4,:r,:d5,:c5,:b4,:c5,:d5,:c5,:b4,:d5,:g4,:r,:c5,:b4,:d5,:a4,:b4]
p1d4 = [c,c,c,q,q,cd,q,c,c,cd,q,c,c,(b + md),c,cd,q,c,c,cd,q,c,c,b,m,q,q,q,q,(2 * b)]

p2n4 = [:r,:b4,:a4,:g4,:a4,:b4,:a4,:g4,:a4,:e4,:r,:b4,:a4,:g4,:a4,:b4,:a4,:g4,:b4,:e4,:r,:e4,:r]
p2d4 = [b,cd,q,c,c,cd,q,c,c,(b + md),c,cd,q,c,c,cd,q,c,c,b,m,m,(2 * b)]

p3n4 = [:c4,:r,:d4,:d4,:c4,:g3,:a3,:b3,:c4,:g3,:c4,:r,:d4,:c4,:g3,:a3,:b3,:c4,:g3,:e4,:d4,:r,:d4,:r,:d4]
p3d4 = [md,c,(b + md),c,c,c,c,c,c,c,c,c,(2 * b),c,c,c,c,c,c,m,c,c,c,c,b]

p4n4 = [:fs3,:r,:g3,:b3,:r,:g3,:r,:c4,:b3,:g3,:b3,:g3]
p4d4 = [md,c,(b + md),c,(2 * b),(2 * b),(b + m),m,c,c,c,(c + b)]

p5n4 = [:a2,:r,:d2,:r,:g2,:b2,:c3,:r,:g2,:r,:c2,:r,:c2,:r,:g2,:c3,:r,:g2,:r,:c3,:r,:c3,:r,:g2,:g2]
p5d4 = [c,c,c,c,(b + md),c,c,c,c,c,c,c,c,c,(2 * b),c,c,c,c,c,c,c,c,b,b]


#define the various sections========================
#upbeat
define :upbeat do
  playarray(p1nub,p1dub,t,a,v)
end

#1st section
define :section1 do
  in_thread do
    playarray(p1n,p1d,t,a,v)
  end
  in_thread do
    playarray(p2n,p2d,t,a,v)
  end
  in_thread do
    playarray(p3n,p3d,t,a,v)
  end
  in_thread do
    playarray(p4n,p4d,t,a,v)
  end
  playarray(p5n,p5d,t,bvol,bv)
end

#repeat bar 1a
define :rb1a do
  in_thread do
    playarray(p1nrb1a,p1drb1a,t,a,v)
  end
  in_thread do
    playarray(p3nrb1a,p3drb1a,t,a,v)
  end
  in_thread do
    playarray(p4nrb1a,p4drb1a,t,a,v)
  end
  playarray(p5nrb1a,p5drb1a,t,bvol,bv)
end

#repeat bar 1b
define :rb1b do
  in_thread do
    playarray(p1nrb1b,p1drb1b,t,a,v)
  end
  in_thread do
    playarray(p3nrb1b,p3drb1b,t,a,v)
  end
  in_thread do
    playarray(p4nrb1b,p4drb1b,t,a,v)
  end
  playarray(p5nrb1b,p5drb1b,t,bvol,bv)
end

#2nd section a
define :section2a do
  in_thread do
    playarray(p1n2,p1d2,t,a,v)
  end
  in_thread do
    playarray(p2n2,p2d2,t,a,v)
  end
  in_thread do
    playarray(p3n2,p3d2,t,a,v)
  end
  in_thread do
    playarray(p4n2,p4d2,t,a,v)
  end
  playarray(p5n2,p5d2,t,bvol,bv)
end

#second section 2 b
define :section2b do
  in_thread do
    playarray(p1n3,p1d3,t,a,v)
  end
  in_thread do
    playarray(p2n3,p2d3,t,a,v)
  end
  in_thread do
    playarray(p3n3,p3d3,t,a,v)
  end
  in_thread do
    playarray(p4n3,p4d3,t,a,v)
  end
  playarray(p5n3,p5d3,t,bvol,bv)
end

#repeat bar 2a
define :rb2a do
  in_thread do
    playarray(p1nrb2a,p1drb2a,t,a,v)
  end
  in_thread do
    playarray(p2nrb2a,p2drb2a,t,a,v)
  end
  in_thread do
    playarray(p3nrb2a,p3drb2a,t,a,v)
  end
  in_thread do
    playarray(p4nrb2a,p4drb2a,t,a,v)
  end
  playarray(p5nrb2a,p5drb2a,t,bvol,bv)
end

#repeat bar 2b
define :rb2b do
  in_thread do
    playarray(p1nrb2b,p1drb2b,t,a,v)
  end
  #no second voice
  in_thread do
    playarray(p3nrb2b,p3drb2b,t,a,v)
  end
  in_thread do
    playarray(p4nrb2b,p4drb2b,t,a,v)
  end
  playarray(p5nrb2b,p5drb2b,t,bvol,bv)
end

#coda
define :coda do
  in_thread do
    playarray(p1n4,p1d4,t,a,v)
  end
  in_thread do
    playarray(p2n4,p2d4,t,a,v)
  end
  in_thread do
    playarray(p3n4,p3d4,t,a,v)
  end
  in_thread do
    playarray(p4n4,p4d4,t,a,v)
  end
  playarray(p5n4,p5d4,t,bvol,bv)
end

#play the piece ================
with_fx :reverb, room: 0.4 do #add some reverb to lift it a bit
  upbeat
  section1
  rb1a
  section1
  rb1b
  section2a
  section2b
  rb2a
  section2a
  section2b
  rb2b
  section1
  rb1b
  section2a
  coda
end
# end=======================
```
