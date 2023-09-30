<script>
  import { ethers } from 'ethers';
  import { onMount } from 'svelte';
  import { writable } from 'svelte/store';

  import '../../main.css';
  import '../../styles.css';
  import contractABI from '../../abi.js';

  const contractAddress = "0x10C72e3F9876F0B0CcA74550d5fd1154bEb2196d"; // Paste the address of the NFT collection.

  async function initProvider() {
    if (window.ethereum) {
      try {
        provider = new ethers.providers.Web3Provider(window.ethereum);
        await window.ethereum.enable(); // Request access to the user's Ethereum wallet
        contract = new ethers.Contract(contractAddress, contractABI, provider.getSigner());
      } catch (error) {
        console.log(error);
      }
    } else {
      console.log("Please install MetaMask or another Ethereum-compatible browser extension.");
    }
  }

  async function connectWallet() {
    if (!provider || !contract) {
      console.log("Please initialize the provider and contract first.");
      return;
    }

    try {
      const balance = await contract.totalSupply();
      for (let i = 0; i < balance.toNumber(); i++) {
        const id = await contract.tokenByIndex(i);
        const url = await contract.tokenURI(id);
        let metadata = null;
        try {
          metadata = await fetch(url).then(res => res.json());
        } catch (e) {
          console.log(e);
          metadata = {
            name: "",
            description: "",
            image: "",
          };
        }
        // Do something with the NFT metadata
        console.log(metadata);
      }
    } catch (error) {
      console.log(error);
    }
  }

  onMount(async () => {
    await initProvider();
  });
</script>

<button on:click={connectWallet}>Connect Wallet</button>