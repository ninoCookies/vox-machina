<script>
  import { ethers } from 'ethers';

  let provider = new ethers.JsonRpcProvider('https://polygon-mainnet.g.alchemy.com/v2/-F37mM1C8PW-JQ_z-IHToPNr5qj51lk9');
  let contractAddress = '0x2b1c7Ee4525f11C11d93074B8a6366424827BB99';
  let contractABI = [{"inputs":[],"name":"getText","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"retrieve","outputs":[{"internalType":"uint256","name":"","type":"uint256"}],"stateMutability":"view","type":"function"},{"inputs":[],"name":"setText","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"string","name":"message","type":"string"}],"name":"setTextByPassing","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[{"internalType":"uint256","name":"num","type":"uint256"}],"name":"store","outputs":[],"stateMutability":"nonpayable","type":"function"},{"inputs":[],"name":"text","outputs":[{"internalType":"string","name":"","type":"string"}],"stateMutability":"view","type":"function"}];

  let contract = new ethers.Contract(contractAddress, contractABI, provider);
  async function callGet() {
    let result = await contract.getText();
    console.log(result);
}
</script>
