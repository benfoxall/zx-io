# zx-io
A hack to try and encode/decode zx spectrum tape audio


```js
// String => AudioBuffer
var encoded = zxio.encode('Hello World')


// AudioBuffer => String
var decoded = zxio.decode(encoded)



// output encoded to speakers
var audioCtx = new AudioContext()
var out = audioCtx.createBufferSource()
out.buffer = encoded
out.connect(audioCtx.destination)
out.start()


// read input from microphone (less useful maybe)
navigator
  .mediaDevices.getUserMedia({audio:true})
  .then(stream =>
    new Promise((resolve, reject) => {

      var recorder = new MediaRecorder(stream)

      // this might be wrong
      recorder.ondataavailable = e => resolve(e.data)

      recorder.start()

      setTimeout(() => {
        recorder.stop()
      }, 10000)

    })
  )
  .then(data => audioCtx.decodeAudioData(data))
  .then(buffer => {
    console.log(zxio.decode(buffer))
  })
```
