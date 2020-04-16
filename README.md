# c14


<br>
c14 is an OP_RETURN protocol on top of bsv core protocol for creating immutable referncce points of mulitmedia files.
<br>
 
### The Problem
<br>

As advancement in Artificial Intelligence continuing at somewhat an exponential rate, we are witnessing a new type of fake content being circulated which is indistinguishable from the real ones to the naked eye. With new breakthroughs in the area of machine learning and artificial intelligence, new tools are being created which in turn creating an influx of deepfake contents. Currently, there are no algorithms or tools that can definitively identify the deepfake videos to maximum accuracy. There must be some kind of framework built in order for our current legal system to deal with such types of issues in the future.
<br>


### The Solution
<br>

Although at a simple glance, the solution lies in AI itself, however, As we develop such algorithms to detect fake videos, the feedback from the prediction model could be used in order to improve deep fake algorithms which will push forward the development of better deepfake algorithms. There are some solutions that being proposed that shows the potential combating deepfakes contents such as -
1. Watermark - A watermark which is a digital signature of the file metadata could provide a reference point against data modification.

but in the above case, the point of reference is not verifiable and immutable as it's not on a public blockchain to verify the integrity of the reference. Someone else could create another point of reference for the same videos and there's no way to verify the chronological order of the reference points
In order to effectively dealing and legally identifying fake contents, c14 protocol requires signing each and every frame of a video and put that signature in the blockchain for an immutable reference along with its file metadata at the instance of creating the origin reference.
<br>
<br>


### The protocol specifications 
<br>

##### Protocol Prefix
c14 uses the following protocol prefix which was generated with the bitcom protocol 
<br>
 - ##### 1tTm1SsDovUrtZryEusP1zfPpK5AvnMKM

<br>

##### Payload generation rule : 
1. Each video file is split into individual frames in the jpg format.
2. Each frame is converted to binary form and signed with the Owner's private key and converted to a base64 string
3. Each frame signature string is then concatenated to a single string. Each frame signature string is separated by the delimiter  "_$_".
4. The frame signature string then hashed with SHA256 which generates the frame_buffer_sig_hash
5. The frame_buffer_sig_hash is then added to the file metadata and the file metadata is signed with the owner's private key and the signature is then added as a key in the payload JSON object.
6. protocol version number is defined in the payload object.
7. In the final step, the payload is serialized into a base64 string.
 
##### Cyrptography rules - 
1. All hashing operations are done with SHA256 hashing algorithm to be considered valid
2. All signatures used must be signed with the key generated by the rsa2048 algorithm to be considered valid.

##### Payload data structure rule
1. A c14 OP_RETURN payload has two components - a. The protocol prefix b. payload string

##### payload data structure rule


data structure of a deserialized json payload  -

```
{

   version : <version>
   sig : <signature generted by signing the metadata with owner's private key>
   
   metadata : {   ...{file metadata}
                  frame_sig_hash: <frame_buffer_sig_hash> 
                  
                }

}
```




##### Collision rule
1. if multiple identical records are found in a block, the lowest topological ordered tx is considered valid
<br>
<br>


### Installation
<br>

```sh
$ npm i c14 --save
```

<br>

### Usage
<br>

Dillinger is currently extended with the following plugins. Instructions on how to use them in your own application are linked below.

```sh
// import c14 to your project
const c14 = require("c14") 
let config = {
    key_rsa : <rsa2048 private key>,
    key_bsv : <bsv_private_key>
}

//initalize c14 instance 
c14.init(config);

c14.prepare_media("./media/src/final.mp4")
.then(video_obj=>{
    return c14.create_frame_sig_hash(video_obj)
})
.then(data=>{
    return c14.build_op_return_payload(data)
})
.then(op_return_payload=>{
    return c14.op_return_push(op_return_payload)
})
.then(txid=>{
    console.log("transaction id : "+txid)
})
.catch(e=>{
    console.log(e)
})
```

<br>
<br>
<br>


License
----

MIT

