<script>
  //コンポーネントデータの読み込み
  import VideoStream from "./components/VideoStream.svelte";

  //Webページの拡大・縮小
  import { browser } from "$app/environment";
  import { onMount } from "svelte";
  
  let zoomGradation = getScale();
  onMount(() => {
    document.documentElement.style.setProperty("--scale", zoomGradation);
  });
  
  function changeZoom(event) {
    if (event.target.innerHTML === "+") {
      zoomGradation = zoomGradation + 0.1;
    } else if (event.target.innerHTML === "-") {
      zoomGradation = zoomGradation - 0.1;
    }
    document.documentElement.style.setProperty("--scale", zoomGradation);
    if (browser) {
      localStorage.setItem("scale", zoomGradation);
    }
  }

  function getScale() {
    if (browser) {
      return parseFloat(localStorage.getItem("scale")) || 1;
    }
    return 1;
  }
  function getIfrSize(key) {
    if (browser) {
      return localStorage.getItem(key);
    }
    return null;
  }

  //リロードボタン
  let reloadMessage = false;
  const reloadPage = () => {
    reloadMessage = true;
    setTimeout(() => {
      reloadMessage = false;
      location.reload();
    }, 1000);
  };

  //各VideoStream関係
  let ipAddressA = getIpAddress('firstIPAddress');
  let widthA = getIfrSize('firstWidthValue');
  let heightA = getIfrSize('firstHeightValue');

  let ipAddressB = getIpAddress('secondIPAddress');
  let widthB = getIfrSize('secondWidthValue');
  let heightB = getIfrSize('secondHeightValue');

  let ipAddressC = getIpAddress('thirdIPAddress');
  let widthC = getIfrSize('thirdWidthValue');
  let heightC = getIfrSize('thirdHeightValue');

  function getIpAddress(key) {
    if (browser) {
      return localStorage.getItem(key);
    }
    return null;
  }
</script>

<body>
  <div class="main-panel">
    <h1>TawaRemo Viewer TypeX</h1>
    <div class="website-zoom-label">
      <h4>画面ズームレベル:<span>{zoomGradation.toFixed(1)}</span></h4>
      <button on:click={changeZoom} class="resize-btn">+</button>
      <button on:click={changeZoom} class="resize-btn">-</button>
      <button on:click={reloadPage} class="reload-btn">Reload</button>
      <span class="reload-text {reloadMessage ? 'appear' : ''}"
        >リロードします</span
      >
    </div>
    <VideoStream ipAddress={ipAddressA} stragekey='firstIPAddress' width={widthA} ifrstragekey='firstWidthValue' height={heightA} ifrstragekey2='firstHeightValue'/>
    <VideoStream ipAddress={ipAddressB} stragekey='secondIPAddress' width={widthB} ifrstragekey='secondWidthValue' height={heightB} ifrstragekey2='secondHeightValue'/>
    <VideoStream ipAddress={ipAddressC} stragekey='thirdIPAddress' width={widthC} ifrstragekey='thirdWidthValue' height={heightC} ifrstragekey2='thirdHeightValue'/>
    <br>
    <br>
  </div>
</body>

<style>
  h1,
  body {
    transform-origin: top left;
    transition: transform 0.8s ease-in-out;
    margin: 0;
  }

  .main-panel {
    color: white;
    background-color: #144998;
    transform: scale(var(--scale));
    transform-origin: top left;
    transition: transform 0.8s ease-in-out;
  }

  /*各ラベルの表示*/
  .website-zoom-label {
    display: flex;
    transform-origin: top left;
  }

  h1,h4 {
    margin-left: 1rem;
    margin-right: 1rem;
  }
  
  /*拡大・縮小*/
  .resize-btn {
    margin-top: 1.3rem;
    width: 2.5rem;
    height: 1.5rem;
    cursor: pointer;
  }

  /*リロード関係*/
  .reload-btn {
    margin-top: 1rem;
    width: 3rem;
    height: 2rem;
    margin-left: 1rem;
    cursor: pointer;
    font-size: 0.7rem;
  }

  .reload-text {
    margin-top: 1.4rem;
    margin-left: 1rem;
    font-size: 0.8rem;
    opacity: 0;
    transition: opacity 300ms;
  }

  .appear {
    opacity: 1;
  }
</style>
