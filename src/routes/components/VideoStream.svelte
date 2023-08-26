<script>
  // //ソケットio関係
  // import { io } from "socket.io-client";
  // const socket = io("http://127.0.0.1:5001");  
  // let pingResultText = "接続中.."
  
  // socket.on('connect', function() { 
    //   socket.on('receive_ping', (value) => { 
      //     pingResultText = value ? 'Online' : 'Offline'
      //   });
      // });
      
  // function pingCheck(){
    //   socket.emit('ask_ping',ipAddress);
    // }
    
    // プリセット保存した際の画面遷移を防止するため、ページをリロードする関数を追加
    // let offlineTime = 0;
    // let timerId = setInterval(reloadIFrame,1000)
    
    //   function reloadIFrame() { 
    //   if (pingResultText === 'Offline') {
    //     offlineTime += 1;
    //     if (offlineTime >= 4) {
    //       offlineTime = 0;
    //       reloadIFrame2()
    //       timerId = setInterval(reloadIFrame, 1000);
    //     }
    //   } else {
    //     clearInterval(timerId);
    //     offlineTime = 0;
    //     timerId = setInterval(reloadIFrame, 1000);
    //   }
    // }
    
    //   function reloadIFrame2() { 
    //     if(pingResultText=='Offline'){
    //       let iframeSrcValue = ifrComponents.src
    //       ifrComponents.src = ""
    //       ifrComponents.src = iframeSrcValue
    //     }
    //   }
    
    //   setInterval(pingCheck,1000)

    function reloadAfter100ms(){
      setTimeout(() => {
    location.reload()
    }, 200)
  }


//IPアドレス関係
  import { onMount } from "svelte";
  export let ipAddress ='';
  export let stragekey = '';
  let isEditting = false;
  
onMount(() => {
  ipAddress = localStorage.getItem(stragekey);
});

const save = () => {
  isEditting = false;
  localStorage.setItem(stragekey, ipAddress);
};
const enterEditMode = () => {
  isEditting = true;
};

//SettingsPanel関係
let ifrComponents;
//比率
  export let width = '';
  export let height = '';
  export let ifrstragekey = 'width_key';
  export let ifrstragekey2 = 'height_key';

function increaseSize() {
  width = 800;
  height = 370;
  localStorage.setItem(ifrstragekey, width);
  localStorage.setItem(ifrstragekey2, height);
}

function decreaseSize() {
  width = 500;
  height = 500;
  localStorage.setItem(ifrstragekey, width);
  localStorage.setItem(ifrstragekey2, height);
}
</script>

<div class="ip-address-label">
 <h4>IP アドレス</h4>
 <input type="input" bind:value={ipAddress} disabled={!isEditting} />
 {#if isEditting}
 <button on:click={save} class="save-button">保存</button>
 {:else}
 <button on:click={enterEditMode} class="edit-button">編集</button>
 {/if}
</div>
<div class="viewer-angle-label">
 <h4>拡大・縮小</h4>
  <a class="zoom-btn" href="http://{ipAddress}/cgi-bin/directctrl?zoom=1" >+</a>
  <a class="zoom-btn" href="http://{ipAddress}/cgi-bin/directctrl?zoom=-1">-</a>
 <h4>プリセット</h4>
 <div class="preset-container">
  <a class="preset-btn" href="http://nwcadmin:Passwd34@{ipAddress}/cgi-bin/camposiset?presetset=1" on:click={reloadAfter100ms}>①保存</a>
  <a class="preset-btn"  href="http://nwcadmin:Passwd34@{ipAddress}/cgi-bin/camposiset?presetset=2" on:click={reloadAfter100ms}>②保存</a>
  <a class="preset-btn"  href="http://nwcadmin:Passwd34@{ipAddress}/cgi-bin/camposiset?presetset=3" on:click={reloadAfter100ms}>③保存</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?preset=1" class="preset-btn">①移動</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?preset=2" class="preset-btn">②移動</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?preset=3" class="preset-btn">③移動</a>
 </div>
 <h4>視点移動</h4>
 <div class="viewer-container">
  <a class="arrow-btn none"></a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?pan=0&tilt=-1&Language=0" class="arrow-btn">↑</a>
  <a class="arrow-btn none"></a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?pan=-1&tilt=0&Language=0" class="arrow-btn">←</a>
  <a class="arrow-btn none"></a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?pan=1&tilt=0&Language=0" class="arrow-btn">→</a>
  <a class="arrow-btn none"></a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?pan=0&tilt=1&Language=0" class="arrow-btn">↓</a>
  <a class="arrow-btn none"></a>
 </div>
 <h4>比率</h4>
 <button class="ifr-resize-btn" on:click={increaseSize}>①</button>
 <button class="ifr-resize-btn" on:click={decreaseSize}>②</button>
</div>
<p class="message">接続中</p>
<div class="iframe-label">
 <div class="side-btn-container">
  <a href="http://{ipAddress}/cgi-bin/camctrl?preset=1" class="iframe-side-btn">①</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?preset=2" class="iframe-side-btn">②</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?preset=3" class="iframe-side-btn">③</a>
  <a href="http://{ipAddress}/cgi-bin/directctrl?zoom=1" class="iframe-side-btn">+</a>
  <a href="http://{ipAddress}/cgi-bin/directctrl?zoom=-1" class="iframe-side-btn">-</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?pan=0&tilt=-1&Language=0" class="iframe-side-btn">↑</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?pan=0&tilt=1&Language=0" class="iframe-side-btn">↓</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?pan=-1&tilt=0&Language=0" class="iframe-side-btn">←</a>
  <a href="http://{ipAddress}/cgi-bin/camctrl?pan=1&tilt=0&Language=0" class="iframe-side-btn">→</a>
 </div>
 <div class="iframe-container">
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
</div>

<style>
  h4,input {
   margin-left: 1rem;
   margin-right: 1rem;
  }
  /*IPアドレス*/
  .ip-address-label {
    display: flex;
  }
  .edit-button {
    background-color: #d9e5ff;
    width: 4rem;
    height: 2rem;
    border: 1px solid;
    cursor: pointer;
    margin-top: 1rem;
  }
  .save-button {
    background-color: #eee;
    width: 4rem;
    height: 2rem;
    border: 1px solid;
    cursor: pointer;
    margin-top: 1rem;
  }

 input {
    height: 2rem;
    margin-top: 0.8rem;
    margin-left: 1rem;
  }
  
/*ラベルの表示*/
.viewer-angle-label{
  display: flex;
}

.zoom-btn {
  margin-top: 1.3rem;
  width: 2.5rem;
  height: 1.5rem;
  cursor: pointer;
}

/*プリセット*/
.preset-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}

.viewer-container {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
}
.preset-btn {
  font-size: 0.9rem;
  width: 3.5rem;
  height: 2.2rem;
  cursor: pointer;
  display:inline-flex;
  justify-content: center;
  align-items: center;
  background: white;
  color:black;
  border-radius : 6%;
  text-decoration: none;
  border: outset 0.01px black;
}

/*視点移動*/
.zoom-btn,.arrow-btn,.ifr-resize-btn {
  width: 2.1rem;
  height: 1.6rem;
  cursor: pointer;
  display:inline-flex;
  justify-content: center;
  align-items: center;
  background: white;
  color:black;
  border-radius : 6%;
  text-decoration: none;
  border: outset 0.01px black;
}

.none {
  border: none;
  background-color: inherit;
  pointer-events: none;
}

/*ラベルの表示*/
.iframe-label{
  display: block;
  margin: 0;
}

p {
  margin-top:0.1rem;
  margin-bottom: 0.5rem;;
  margin-left: 0.5rem;
}

/*カメラビューワー*/
iframe {
  margin-left: 1rem;
}

/*ミニボタン*/
button{
  width: 2rem;
  height: 2rem;
  display: block;
  font-size: 1rem;
  margin-left: 0.5rem;
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

.side-btn-container{
  margin: 0;
  display:flex;
  flex-flow: column;
}

.iframe-label{
  margin: 0;
  display: flex;
  justify-content:
  flex-start;
}

.iframe-side-btn{
  text-decoration: none;
  margin: 1px;
  border-radius : 6%;
  font-size     : 1rem;
  padding       : 8px 1px;
  background    : white;
  color         : #000000;
  line-height   : 0.1em;
  width: 1.5rem;
  height:1.0rem;
  display:inline-flex;
  justify-content: center;
  align-items: center;
  margin-left: 0.5rem;
}
</style>