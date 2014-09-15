```Ruby
#Bach Prelude in C Major, transcribed by Robin Newman Sept 2014
#tested on Sonic-Pi ver 2.0.1
#illustrates the use of volume settings for each note to allow for crescendo, diminuendo
#also built in rallentando at end of the piece
use_synth :tri #unless specified otherwise
s = 1.0 / 8.5                       #tempo about 64 c/minute

#note durations (not all used)
dsq = 1 * s
sq = 2 * s
q = 4 * s
qd = 6 * s
c = 8 * s 
cd = 12 * s
m = 16 * s
md = 24 * s
b = 32 * s
bd = 48 * s

#dynamic settings
pp = 0.1
p = 0.15
mp = 0.22
mf = 0.3
f = 0.5

#volumes for part 1
#first set up crescendo over 5 bars from pp to f
cr = [pp]
vol = pp
npb=14#number of note elements/standard bar including rests
inc = (f-pp)/(5*npb)
(5*npb-1).times do
cr = cr + [vol]
vol=vol+inc
end
#now set up diminuendo from f to p over 2 bars
dim = [f]
vol = f
inc = (p-f)/(2*npb)
(2*npb-1).times do
dim = dim + [vol]
vol = vol + inc
end
#now assemble complete volume profile
v1 = [p]*4*npb + [mf]*npb + [mp]*npb + [mf]*npb + [mp]*7*npb + [p]*7*npb +[pp]*2*npb +cr + [f]*npb + dim +[p]*(npb+npb+1+npb+1+1) #adjust for different bar structures at end

#vols for part 2
cr = [p]
vol = p
npb=4 #4 note +rest elements /standard bar
inc = (f-pp)/(5*npb)
(5*npb-1).times do
cr = cr + [vol]
vol=vol+inc
end
dim = [f]
vol = f
inc = (p-f)/(2*npb)
(2*npb-1).times do
dim = dim + [vol]
vol = vol + inc
end
v2 = [p]*4*npb + [mf]*npb + [mp]*npb + [mf]*npb + [mp]*7*npb + [p]*7*npb +[pp]*2*npb +cr + [f]*npb + dim +[p]*(npb+npb-2+npb-2+1) #adjust for different bar structures at end

#vols for part 3
pp = 0.5*pp
p = 0.5*p
mp = 0.5*mp
mf = 0.5*mf
f = 0.5*f
cr = [pp]
vol = pp
npb=2 #2 notes per standard bar
inc = (f-pp)/(5*npb)
(5*npb-1).times do
cr = cr + [vol]
vol=vol+inc
end
dim = [f]
vol = f
inc = (p-f)/(2*npb)
(2*npb-1).times do
dim = dim + [vol]
vol = vol + inc
end
v3 = [p]*4*npb + [mf]*npb + [mp]*npb + [mf]*npb + [mp]*7*npb + [p]*7*npb +[pp]*2*npb +cr + [f]*npb + dim +[p]*(npb+1+1+1) #adjust for different bar structures at end

#definition to play 3 arrays containing note pitch, duration and volume. Can also set global attack and synth
#sustain set to 90% duration, release to 10% duration
define :pl1 do |notearray,durationarray,volarray,attack=(dsq / 2),voice = :tri|
  with_synth voice do
    notearray.zip(durationarray,volarray).each do |notearray,durationarray,volarray| #zip traverses arrays together
      if notearray == :r #a rest
        sleep durationarray
      else
          play notearray,amp: volarray,sustain: durationarray * 0.9 - attack,release: durationarray * 0.1,attack: attack
          sleep durationarray
      end
    end
  end
end

#now define the three parts and the durations for each of their notes
p1n = [:r,:g4,:c5,:e5,:g4,:c5,:e5,:r,:g4,:c5,:e5,:g4,:c5,:e5,:r,:a4,:d5,:f5,:a4,:d5,:f5,:r,:a4,:d5,:f5,:a4,:d5,:f5]
p1d = [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b3
p1n.concat [:r,:g4,:d5,:f5,:g4,:d5,:f5,:r,:g4,:d5,:f5,:g4,:d5,:f5,:r,:g4,:c5,:e5,:g4,:c5,:e5,:r,:g4,:c5,:e5,:g4,:c5,:e5]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b5
p1n.concat [:r,:a4,:e5,:a5,:a4,:e5,:a5,:r,:a4,:e5,:a5,:a4,:e5,:a5,:r,:fs4,:a4,:d5,:fs4,:a4,:d5,:r,:fs4,:a4,:d5,:fs4,:a4,:d5]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b7
p1n.concat [:r,:g4,:d5,:g5,:g4,:d5,:g5,:r,:g4,:d5,:g5,:g4,:d5,:g5,:r,:e4,:g4,:c5,:e4,:g4,:c5,:r,:e4,:g4,:c5,:e4,:g4,:c5]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b9
p1n.concat [:r,:e4,:g4,:c5,:e4,:g4,:c5,:r,:e4,:g4,:c5,:e4,:g4,:c5,:r,:d4,:fs4,:c5,:d4,:fs4,:c5,:r,:d4,:fs4,:c5,:d4,:fs4,:c5]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b11
p1n.concat [:r,:d4,:g4,:b4,:d4,:g4,:b4,:r,:d4,:g4,:b4,:d4,:g4,:b4,:r,:e4,:g4,:cs5,:e4,:g4,:cs5,:r,:e4,:g4,:cs5,:e4,:g4,:cs5]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b13
p1n.concat [:r,:d4,:a4,:d5,:d4,:a4,:d5,:r,:d4,:a4,:d5,:d4,:a4,:d5,:r,:d4,:f4,:b4,:d4,:f4,:b4,:r,:d4,:f4,:b4,:d4,:f4,:b4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b15
p1n.concat [:r,:c4,:g4,:c5,:c4,:g4,:c5,:r,:c4,:g4,:c5,:c4,:g4,:c5,:r,:a3,:c4,:f4,:a3,:c4,:f4,:r,:a3,:c4,:f4,:a3,:c4,:f4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b17
p1n.concat [:r,:a3,:c4,:f4,:a3,:c4,:f4,:r,:a3,:c4,:f4,:a3,:c4,:f4,:r,:g3,:b3,:f4,:g3,:b3,:f4,:r,:g3,:b3,:f4,:g3,:b3,:f4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b19
p1n.concat [:r,:g3,:c4,:e4,:g3,:c4,:e4,:r,:g3,:c4,:e4,:g3,:c4,:e4,:r,:bb3,:c4,:e4,:bb3,:c4,:e4,:r,:bb3,:c4,:e4,:bb3,:c4,:e4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b21
p1n.concat [:r,:a3,:c4,:e4,:a3,:c4,:e4,:r,:a3,:c4,:e4,:a3,:c4,:e4,:r,:a3,:c4,:eb4,:a3,:c4,:eb4,:r,:a3,:c4,:eb4,:a3,:c4,:eb4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b23
p1n.concat [:r,:b3,:c4,:d4,:b3,:c4,:d4,:r,:b3,:c4,:d4,:b3,:c4,:d4,:r,:g3,:b3,:d4,:g3,:b3,:d4,:r,:g3,:b3,:d4,:g3,:b3,:d4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b25
p1n.concat [:r,:g3,:c4,:e4,:g3,:c4,:e4,:r,:g3,:c4,:e4,:g3,:c4,:e4,:r,:g3,:c4,:f4,:g3,:c4,:f4,:r,:g3,:c4,:f4,:g3,:c4,:f4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b27
p1n.concat [:r,:g3,:b3,:f4,:g3,:b3,:f4,:r,:g3,:b3,:f4,:g3,:b3,:f4,:r,:a3,:c4,:fs4,:a3,:c4,:fs4,:r,:a3,:c4,:fs4,:a3,:c4,:fs4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b29
p1n.concat [:r,:g3,:c4,:g4,:g3,:c4,:g4,:r,:g3,:c4,:g4,:g3,:c4,:g4,:r,:g3,:c4,:f4,:g3,:c4,:f4,:r,:g3,:c4,:f4,:g3,:c4,:f4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b31
p1n.concat [:r,:g3,:b3,:f4,:g3,:b3,:f4,:r,:g3,:b3,:f4,:g3,:b3,:f4,:r,:g3,:bb3,:e4,:g3,:bb3,:e4,:r,:g3,:bb3,:e4,:g3,:bb3,:e4]
p1d.concat [q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq]
#b33
p1n.concat [:r,:f3,:a3,:c4,:f4,:c4,:a3,:c4,:a3,:f3,:a3,:f3,:d3,:f3,:d3,:r,:g4,:b4,:d5,:f5,:d5,:b4,:d5,:b4,:g4,:b4,:d4,:f4,:e4,:d4]
#set up the rit in the last bar. (nb chord bar follows this)
p1d.concat [q,sq,sq,sq,sq,sq,sq,sq,sq,sq,sq,sq,sq,sq,sq,q,sq,sq,sq,sq,sq,sq,sq,sq*1.1,sq*1.2,sq*1.3,sq*1.4,sq*1.5,sq*1.6,sq*1.7   ]

#second part
p2n = [:r,:e4,:r,:e4,:r,:d4,:r,:d4,:r,:d4,:r,:d4,:r,:e4,:r,:e4,:r,:e4,:r,:e4,:r,:d4,:r,:d4,:r,:d4,:r,:d4,:r,:c4,:r,:c4]
p2d = [sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c]
#b9
p2n.concat [:r,:c4,:r,:c4,:r,:a3,:r,:a3,:r,:b3,:r,:b3,:r,:bb3,:r,:bb3,:r,:a3,:r,:a3,:r,:ab3,:r,:ab3,:r,:g3,:r,:g3,:r,:f3,:r,:f3]
p2d.concat [sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c]
#b17
p2n.concat [:r,:f3,:r,:f3,:r,:d3,:r,:d3,:r,:e3,:r,:e3,:r,:g3,:r,:g3,:r,:f3,:r,:f3,:r,:c3,:r,:c3,:r,:f3,:r,:f3,:r,:f3,:r,:f3]
p2d.concat [sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c]
#b25
p2n.concat [:r,:e3,:r,:e3,:r,:d3,:r,:d3,:r,:d3,:r,:d3,:r,:eb3,:r,:eb3,:r,:e3,:r,:e3,:r,:d3,:r,:d3,:r,:d3,:r,:d3,:r,:c3,:r,:c3]
p2d.concat [sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c,sq,qd+c]
#b33
p2n.concat  [:r,:c3,:r,:b3,:c3]
#compensate for rit in penultimate bar and add pause
p2d.concat [sq,qd+c+m,sq,qd+c+m+2.8*sq,b*1.1]

#third part
p3n = [:c4,:c4,:c4,:c4,:b3,:b3,:c4,:c4,:c4,:c4,:c4,:c4,:b3,:b3,:b3,:b3]
p3d = [m,m,m,m,m,m,m,m,m,m,m,m,m,m,m,m]
#b9
p3n.concat [:a3,:a3,:d3,:d3,:g3,:g3,:g3,:g3,:f3,:f3,:f3,:f3,:e3,:e3,:e3,:e3]
p3d.concat [m,m,m,m,m,m,m,m,m,m,m,m,m,m,m,m]
#b17
p3n.concat [:d3,:d3,:g2,:g2,:c3,:c3,:c3,:c3,:f2,:f2,:fs2,:fs2,:ab2,:ab2,:g2,:g2]
p3d.concat [m,m,m,m,m,m,m,m,m,m,m,m,m,m,m,m]
#b25
p3n.concat [:g2,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:g2,:c2,:c2]
p3d.concat [m,m,m,m,m,m,m,m,m,m,m,m,m,m,m,m]
#b33
p3n.concat [:c2,:c2,:c2]
#compensate for rit in penultimate bar and add pause
p3d.concat [b,b+2.8*sq,b*1.1]

#==========start playing here ============
#now play the piece with some reverb set
with_fx :reverb, room: 0.4 do
  #first two parts in threads to play concurrently with third part
  in_thread do
    #parameters for pl1 are notearray,durationarray,volarray,attack=(dsq / 2),voice = :tri_s
    pl1(p1n,p1d,v1)
    #play last bar chord, adjusted for pause, vol set to p
    play_chord [:e4,:g4,:c5],sustain: 0.9*b*1.1,release: 0.1*b*1.1,amp: p
    sleep b * 1.1
  end
  #second part thread
  in_thread do
    pl1(p2n,p2d,v2)
  end
  #third part
  pl1(p3n,p3d,v3,0,:saw) 
end
```
