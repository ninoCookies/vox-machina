<script>
  import { ethers } from 'ethers';
  import { onMount } from 'svelte';
  let isPlaying = false;
  let provider = new ethers.JsonRpcProvider('https://polygon-mainnet.g.alchemy.com/v2/-F37mM1C8PW-JQ_z-IHToPNr5qj51lk9');
  let contractAddress = '0x2b1c7Ee4525f11C11d93074B8a6366424827BB99';
  let contractABI = [{"inputs":[],"name":"getText","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"retrieve","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"setText","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"string","name":"message","type":"string"}],"name":"setTextByPassing","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"num","type":"uint256"}],"name":"store","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"text","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"}];

  let contract = new ethers.Contract(contractAddress, contractABI, provider);
  async function callGet() {
    let result = await contract.getText();
    console.log(result);
  }
  let band = {
    name: 'Vox Machina',
    album: {
      title: 'The Chronicles of Exandria',
      price: 4.99,
      tracks: Array.from({length: 4}, function(_, i) {
        return {
          title: 'Track ' + (i + 1),
          file: 'https://ipfs.apillon.io/ipfs/QmSX7fu8HVTb4WXp9QtTRHhvqdigKgUwBmiq1G75cSJvHf' // Provided link
        };
      })
    }
  };
  onMount(() => {
    const audioElements = document.querySelectorAll('audio');
    audioElements.forEach(audio => {
      audio.addEventListener('play', () => isPlaying = true);
      audio.addEventListener('pause', () => isPlaying = false);
    });
  });
</script>

<style>
.record-player {
  position: relative;
  width: 500px;
  height: 500px;
  margin: 0 auto;
}

.turntable, .record {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
}

.record {
  animation: rotate 1.8s linear infinite;
  animation-play-state: paused;
  width:250px;
  height:250px;
  margin-top:85px;
  margin-left:72px;

}

.record-player.playing .record {
  animation-play-state: running;
}

@keyframes rotate {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
</style>

<header>
  <h1>{band.name}</h1>
  <p>Progressive Rock band based in Exandria, formed in 2015.</p>
</header>

<main>
  <section>
    <h2>{band.album.title} - ${band.album.price}</h2>
    <ul>
      {#each band.album.tracks as track (track.title)}
        <li>
          {track.title}
          <audio controls src={track.file}></audio>
        </li>
      {/each}
    </ul>
  </section>
  <h1>Test circle</h1>

  <div class="record-player" class:playing={isPlaying}>
    <img src="https://ipfs.apillon.io/ipfs/QmaAXwQyH1QSkrksi6GCQWdnBWDXwrhRApP5QW6jRomeM5" class="turntable" />
    <img src="https://ipfs.apillon.io/ipfs/Qme3nYoxhPb69hoKBAW9SpU3v8LU76gkHmhPJRB6XqkmLg" class="record" />
  </div>


  <section>
    <h2>Contact / Help</h2>
    <p>Contact {band.name}</p>
    <p>Streaming and Download help</p>
  </section>
</main>

<footer>
  <p>Bandcamp</p>
  <ul>
    <li>Log in</li>
    <li>Terms of use</li>
    <li>Privacy</li>
    <li>Copyright policy</li>
    <li>Switch to mobile view</li>
  </ul>
</footer>