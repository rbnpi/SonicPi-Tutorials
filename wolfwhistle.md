```Ruby
define :wolfwhistle do |n|
  n=note_info(n).midi_note
  p = play n,sustain: 0.2,release: 0.1
  control p, note: n + 24, note_slide: 0.5
  sleep 0.5
  p = play n,sustain: 0.2,release: 0.1
  control p, note: n + 24, note_slide: 0.4
  sleep 0.2
  p = play n + 24,sustain: 0.12,release: 0.1
  control p, note: n+8, note_slide: 0.4
  sleep 0.1
end

in_thread do
  wolfwhistle(:c5)
end
wolfwhistle(:g5)
```
