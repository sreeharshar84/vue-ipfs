<template>
  <div>
    <h1>input file</h1>
    <input type="file" @change="captureFile">
    <button @click="upload">submit</button>
  </div>
</template>
<script>
console.log("asdasd");
//import Buffer from "../util/buffer.js";
import web3 from "../util/getweb3.js";
import ipfs from "../util/ipfs.js";
import storehash from "../util/storehash.js";
console.log("sdadsdas");
export default {
  data() {
    return {
      image: "",
      ipfsHash: null,
      buffer: "",
      ethAddress: "",
      blockNumber: "",
      transactionHash: "",
      gasUsed: "",
      txReceipt: ""
    };
  },
  methods: {
    captureFile(event) {
      event.stopPropagation();
      event.preventDefault();
      const file = event.target.files[0];
      let reader = new window.FileReader();
      reader.readAsArrayBuffer(file);
      reader.onloadend = () => this.convertToBuffer(reader);
    },
    async convertToBuffer(reader) {
      //file is converted to a buffer for upload to IPFS
      const Bufferdata = await Buffer.from(reader.result);
      //set this buffer -using es6 syntax
      this.buffer = Bufferdata;
      console.log("BUF" + this.buffer);
    },
    async upload() {
      event.preventDefault();
    
      //var currentDate = new Date().getTime();
       const input = {
        "name": "NB 75466",
        "description": "Bet Details from Match X",
        "external_url": "https://openseacreatures.io/3",
        "image": "https://storage.googleapis.com/opensea-prod.appspot.com/puffs/3.png",
        "attributes": [
            {
              "trait_type": "Match",
              "value": "Manchester United vs Liverpool"
            },
            {
              "trait_type": "Position",
              "value": "Liverpool win"
            },
            {
              "trait_type": "Coach",
              "value": "Klopp"
            },
            {
              "display_type": "number",
              "trait_type": "BetId",
              "value": 20
            }
          ]
        };
    
        const file = {
          path: '/tmp/bet-details.txt',
          content: Buffer.from(JSON.stringify(input))
        }
 
        console.log("uploading to ipfs"+ipfs);
        
        await ipfs.add(file)
        .then(res => {
          
          var uri = 'https://ipfs.io/ipfs/' + res[0].hash;
          console.log('uri = ' + uri)
          console.log(res);
        })

    }
  }
};
</script>
