<script>
  import { io } from "socket.io-client";
  const socket = io("http://127.0.0.1:5001");
  export let ipAddress = ''
  export let stragekey = ''

  export let width;
  export let height;
  export let ifrstragekey = ''
  export let ifrstragekey2 = ''
  let ifrComponents;
  
  let isEditing = !ipAddress
  let pingResultText = "接続中.."

  function toggleEditing() {
    isEditing = !isEditing
    localStorage.setItem(stragekey,ipAddress);
  }

  // プリセット保存した際の画面遷移を防止するため、ページをリロードする関数を追加
  function reloadAfter100ms(){
    setTimeout(() => {
    location.reload()
    }, 200)
  }

  function pingCheck(){
    socket.emit('ask_ping',ipAddress);
  }

let offlineTime = 0;
let timerId = setInterval(reloadIFrame,1000)

  function reloadIFrame() { 
  if (pingResultText === 'Offline') {
    offlineTime += 1;
    if (offlineTime >= 4) {
      offlineTime = 0;
      reloadIFrame2()
      timerId = setInterval(reloadIFrame, 1000);
    }
  } else {
    clearInterval(timerId);
    offlineTime = 0;
    timerId = setInterval(reloadIFrame, 1000);
  }
}

  function reloadIFrame2() { 
    if(pingResultText=='Offline'){
      let iframeSrcValue = ifrComponents.src
      ifrComponents.src = ""
      ifrComponents.src = iframeSrcValue
    }
  }

  socket.on('connect', function() { 
    socket.on('receive_ping', (value) => { 
      pingResultText = value ? 'Online' : 'Offline'
    });
  });

  function increaseSize() {
    width = 800;
    height = 370;
    localStorage.setItem(ifrstragekey,width);
    localStorage.setItem(ifrstragekey2, height);
  }

  function decreaseSize() {
    width = 500;
    height = 500;
    localStorage.setItem(ifrstragekey,width);
    localStorage.setItem(ifrstragekey2, height);
  }

  setInterval(pingCheck,1000)
</script>

<body>

<div class="container">
  <p class="ipaddress-text">IPアドレス：</p>
    {#if isEditing}
      <div class="ip-input-box">
        <input class="input-ip" type="input" bind:value={ipAddress} />
        <input class="input-btn"  type="button" value="保存" on:click={toggleEditing} />
      </div>
    {:else}
      <div class="ip-add-box" >
        <p class="show-ip" >
          {ipAddress}
        </p>
        <button class="input-btn" on:click={toggleEditing}>編集</button>
      </div>
    {/if}
    <!-- svelte-ignore a11y-missing-attribute -->
</div>

<div class="setting-btn-container">
  <div class="zoom-btn-container">
    <p class='zoom-text'>拡大縮小：</p>
    <a href="http://{ipAddress}/cgi-bin/directctrl?zoom=1" class="zoom-btn">＋</a>
    <a href="http://{ipAddress}/cgi-bin/directctrl?zoom=-1" class="zoom-btn">ー</a>
  </div>
  <br/>
  <div class="preset-container">
    <p class="preset-text">プリセット：</p>
  <div>
    <a href="http://nwcadmin:Passwd34@{ipAddress}/cgi-bin/camposiset?presetset=1" on:click={reloadAfter100ms} class="preset-btn">①保存</a>
    <a href="http://nwcadmin:Passwd34@{ipAddress}/cgi-bin/camposiset?presetset=2" on:click={reloadAfter100ms} class="preset-btn">②保存</a>
    <a href="http://nwcadmin:Passwd34@{ipAddress}/cgi-bin/camposiset?presetset=3" on:click={reloadAfter100ms} class="preset-btn">③保存</a>
    <br/>
    <a href="http://{ipAddress}/cgi-bin/camctrl?preset=1" class="preset-btn">①移動</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?preset=2" class="preset-btn">②移動</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?preset=3" class="preset-btn">③移動</a>
  </div>
</div>
    <p class="pan-crt-text">移動：</p>
    <a href="http://{ipAddress}/cgi-bin/camctrl?pan=0&tilt=-1&Language=0" class="pan-crt-btn">上</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?pan=0&tilt=1&Language=0" class="pan-crt-btn">下</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?pan=-1&tilt=0&Language=0" class="pan-crt-btn">左</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?pan=1&tilt=0&Language=0" class="pan-crt-btn">右</a>

    <p class="pan-crt-text">比率：</p>
    <button class='ifr-resize-btn' on:click={increaseSize}>①</button>
    <button class='ifr-resize-btn' on:click={decreaseSize}>②</button>
</div>

<p class="ping-result-text">{pingResultText}</p>

<div class="ifrComponents-parent">
  <div class="iframe-side-button-parent">
    <a href="http://{ipAddress}/cgi-bin/camctrl?preset=1" class="iframe-side-button">①</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?preset=2" class="iframe-side-button">②</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?preset=3" class="iframe-side-button">③</a>
    <a href="http://{ipAddress}/cgi-bin/directctrl?zoom=1" class="iframe-side-button">＋</a>
    <a href="http://{ipAddress}/cgi-bin/directctrl?zoom=-1" class="iframe-side-button">ー</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?pan=0&tilt=-1&Language=0" class="iframe-side-button">↑</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?pan=0&tilt=1&Language=0" class="iframe-side-button">↓</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?pan=1&tilt=0&Language=0" class="iframe-side-button">→</a>
    <a href="http://{ipAddress}/cgi-bin/camctrl?pan=-1&tilt=0&Language=0" class="iframe-side-button">←</a>
  </div>
  <iframe
  bind:this={ifrComponents}
  ref="ifrComponents"
  class="ifrComponents"
  src="http://{ipAddress}/ImageViewer?Mode=Motion&Resolution=640x360&Quality=Standard&Interval=10"
  scrolling="no"
  frameborder="0"
  width={width}
  height={height}
  />
</div>

</body>
<style>
  .ipaddress-text{
    margin-left:20px;
    margin-top: 0px;
  }
  .container {
    display: flex;
  }
  .input-ip{
    border: solid 1px black;
    height: 25px;
    width: 130px;
    font-size: 100%;
  }
  .input-btn {
    height: 30px;
    margin-left: 10px;
  }
  .ip-add-box {
    display: flex;
  }
  .show-ip {
    padding: 2px;
    margin: 0;
    border: 1px dashed black;
    height: 25px;
    width: 130px;
    font-size: 100%;
  }
  .zoom-text{
    margin-left: 20px;
    margin-top: 0px;
  }
  .ifrComponents{
    margin-left: 0px;
  }
  .preset-text{
    margin-left: 20px;
    margin-top: 0px;
  }
.zoom-btn {
  text-decoration: none;
  margin: 2px;
  margin-bottom: 30px;
  display       : inline-block;
  border-radius : 6%;
  font-size     : 8pt;
  text-align    : center;
  cursor        : pointer;
  padding       : 8px 10px;
  background    : white;;
  color         : #000000;
  line-height   : 1em;
  transition    : .3s;
  align-self:flex-end
}
.zoom-btn-container{
  display: flex;
}
.preset-btn {
  text-decoration: none;
  margin: 2px;
  display       : inline-block;
  border-radius : 6%;
  font-size     : 8pt;
  text-align    : center;
  cursor        : pointer;
  padding       : 8px 10px;
  background    : white;
  color         : #000000;
  line-height   : 1em;
  transition    : .3s;
  align-self:flex-end
}
.preset-container{
  display: flex;
}
.setting-btn-container{
  display: flex;
}
.pan-crt-btn{
  text-decoration: none;
  margin: 2px;
  margin-bottom: 30px;
  display       : inline-block;
  border-radius : 6%;
  font-size     : 8pt;
  text-align    : center;
  cursor        : pointer;
  padding       : 8px 10px;
  background    : white;;
  color         : #000000;
  line-height   : 1em;
  transition    : .3s;
  align-self:flex-end
}
.pan-crt-text{
  margin-left: 20px;
  margin-top: 0px;
}
body{
  color: white;
  background-color: #144998;
}
.ifr-resize-btn{
  margin-left: 5px;
  margin-bottom: 35px;
  cursor        : pointer;
  background    : white;
  color         : #000000;
  border: none;
  align-self:flex-end
}
.iframe-side-button-parent{
  margin: 0px;
  display:flex;
  flex-flow: column;
}
.ifrComponents-parent{
  margin: 0;
  display: flex;
  justify-content:
  flex-start;
}
.iframe-side-button{
  text-decoration: none;
  margin: 1px;
  border-radius : 6%;
  font-size     : 8pt;
  padding       : 8px 1px;
  background    : white;
  color         : #000000;
  line-height   : 0.1em;
}
</style>