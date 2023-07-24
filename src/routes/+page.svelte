<script>
  import VideoStream from './components/VideoStream.svelte';
  import { browser } from "$app/environment";
  import { onMount } from 'svelte';

  let scale = getScale();
  let ipAddressA = getIpAddress('firstIPAddress');
  let ipAddressB = getIpAddress('secondIPAddress');
  let ipAddressC = getIpAddress('thirdIPAddress');

  let widthA = getIfrSize('firstWithValue')
  let heightA = getIfrSize('firstHeightValue')
  let widthB = getIfrSize('secondWithValue')
  let heightB = getIfrSize('secondHeightValue')
  let widthC = getIfrSize('thirdWithValue')
  let heightC = getIfrSize('thirdHeightValue')

  onMount(() => {
    document.documentElement.style.setProperty('--scale', scale);
  });

  function getIfrSize(key) {
    if (browser) {
      return localStorage.getItem(key);
    }
    return null;
  }

  function getIpAddress(key) {
    if (browser) {
      return localStorage.getItem(key);
    }
    return null;
  }

  function getScale() {
    if (browser) {
      return parseFloat(localStorage.getItem('scale')) || 1;
    }
    return 1;
  }

  function changeZoom(event) {
    if (event.target.innerHTML === '＋') {
      scale += 0.2;
    } else if (event.target.innerHTML === 'ー') {
      scale -= 0.2;
    }
    document.documentElement.style.setProperty('--scale', scale);
    if (browser) {
      localStorage.setItem('scale', scale);
    }
  }
</script>

<main>
  <h1>TawaRemo Viewer TypeF {"__VERSION__"}</h1>
  <div class="super-zoom-container">
  <p class="super-zoom-text">画面ズームレベル：{scale.toFixed(1)}</p>
  <button class="super-zoom-btn" on:click={changeZoom}>＋</button>
  <button class="super-zoom-btn" on:click={changeZoom}>ー</button>
  <button class="super-zoom-btn" on:click={() => location.reload()}>RELOAD</button>
  </div>
  <VideoStream ipAddress={ipAddressA} stragekey='firstIPAddress' width={widthA} ifrstragekey='firstWithValue' height={heightA} ifrstragekey2='firstHeightValue'/>
  <VideoStream ipAddress={ipAddressB} stragekey='secondIPAddress' width={widthB} ifrstragekey='secondWithValue' height={heightB} ifrstragekey2='secondHeightValue'/>
  <VideoStream ipAddress={ipAddressC} stragekey='thirdIPAddress' width={widthC} ifrstragekey='thirdWithValue' height={heightC} ifrstragekey2='thirdHeightValue'/>
</main>

<style>
  h1 {
    margin: 0;
  }
  main {
    color: white;
    background-color: #144998;
    transform-origin: top left;
    transform: scale(var(--scale));
    transition: transform 0.5s ease-in-out;
  }
  .super-zoom-text{
    margin: 20px;
  }
  .super-zoom-btn{
    margin: 3px;
    margin-bottom: 20px;
    cursor        : pointer;
    align-self:flex-end;
    background    : white;
    color         : #000000;
    border: none;
  }
  .super-zoom-container{
    display: flex;
    align-items: center;
  }
</style>