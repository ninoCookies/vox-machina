<script>
    import { ethers } from 'ethers';
    import { onMount } from 'svelte';
    import '../main.css';
    import '../styles.css'
    import cover from '../images/cover.png';
    import lp from '../images/lp.png';
    import turntable from '../images/turntable.png';
    import play from '../images/play.png';
    import pause from '../images/pause.png';
    import person1 from '../images/person1.png';
    import person2 from '../images/person2.png';
    import person3 from '../images/person3.png';
    import person4 from '../images/person4.png';
    import Navigation from '../components/Navigation.svelte';


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
        description:'Progressive Rock band based in Skogar, formed in 2015.',
        message: 'In the vast cosmos, humanity is but a minuscule entity. Despite our cosmic insignificance, we persist in harming our planet without a viable alternative. Enter VOX Machina, a relentless force—the voice of the voiceless. Their resounding message echoes clearly: embrace humility, stay true, and unite to save our planet from further destruction.',
        cover: cover,
        album: {
            title: 'The Chronicles of Skogar',
            price: 4.99,
            tracks: [
                {
                    title: 'Track 1',
                    file: 'https://ipfs.apillon.io/ipfs/QmSX7fu8HVTb4WXp9QtTRHhvqdigKgUwBmiq1G75cSJvHf' // Provided link
                },
                {
                    title: 'Track 2',
                    file: 'https://ipfs.apillon.io/ipfs/QmSX7fu8HVTb4WXp9QtTRHhvqdigKgUwBmiq1G75cSJvHf' // Provided link
                },
                {
                    title: 'Track 3',
                    file: 'https://ipfs.apillon.io/ipfs/QmSX7fu8HVTb4WXp9QtTRHhvqdigKgUwBmiq1G75cSJvHf' // Provided link
                }
            ]
        },
        album: {
            title: 'Trululu',
            price: 4.99,
            tracks: [
                {
                    title: 'Track 1',
                    file: 'https://ipfs.apillon.io/ipfs/QmSX7fu8HVTb4WXp9QtTRHhvqdigKgUwBmiq1G75cSJvHf' // Provided link
                },
                {
                    title: 'Track 2',
                    file: 'https://ipfs.apillon.io/ipfs/QmSX7fu8HVTb4WXp9QtTRHhvqdigKgUwBmiq1G75cSJvHf' // Provided link
                },
                {
                    title: 'Track 3',
                    file: 'https://ipfs.apillon.io/ipfs/QmSX7fu8HVTb4WXp9QtTRHhvqdigKgUwBmiq1G75cSJvHf' // Provided link
                }
            ]
        }
    };

    let isPlaying = band.album.tracks.map(() => false);

    let audioElements = [];

    function togglePlay(index) {
      const audio = audioElements[index];
      if (audio.paused) {
        audio.play();
        isPlaying[index] = true;
      } else {
        audio.pause();
        isPlaying[index] = false;
      }
    }

    $: isAnyTrackPlaying = isPlaying.includes(true);
</script>



<div class="container">
    <header>
        <div class="cover">
            <h1>{band.name}</h1>
        </div>
        <Navigation />
    </header>

    <main class="container">
        <section class="columns">
            <div class="column player-container">
              <div class="record-player">
                <img class="record" class:active={isAnyTrackPlaying} src={lp} alt />
              </div>                           
              <ul>
                {#each band.album.tracks as track, i (track.title)}
                  <li>
                  
                    <audio bind:this={audioElements[i]} src={track.file}></audio>
                    <button on:click={() => togglePlay(i)} class="play-button">
                      {#if isPlaying[i]}
                        <img src={pause} alt="Pause Icon">
                      {:else}
                        <img src={play} alt="Play Icon">
                      {/if}
                    </button>
                    {track.title} {band.album.title} - ${band.album.price}
                  </li>
                {/each}
              </ul>

            </div> 

            <div class="column is-one-quarter">

                <h3>{band.name}</h3>
                <p>{band.description}</p>
            </div>
        </section>
       
        <section id="nfts">
          <h2>Buy NFT's and support the band</h2>
          <div class="columns">
            <div class="column">
              <img src={person1} alt="Person 1"/>
              <div>Price: €4.99</div>
              <a href="#">Buy</a>
            </div>
            <div class="column">
              <img src={person2} alt="Person 2"/>
            </div>
            <div class="column">
              <img src={person3} alt="Person 3"/>
            </div>
            <div class="column">
              <img src={person4} alt="Person 3"/>
            </div>
          </div>
        </section>
    </main>
    
    <footer>
        <p>Vox Machina</p>
        <ul class="inline">
            <li>Log in</li>
            <li>Terms of use</li>
            <li>Privacy</li>
            <li>Copyright policy</li>
            <li>Switch to mobile view</li>
        </ul>
    </footer>

</div>


