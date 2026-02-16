<!DOCTYPE html>
<html>
<head>
<style>
  /* THE LAYOUT */
  .player-container {
    display: flex;
    align-items: center;
    background-color: #080808; /* Dark Background */
    padding: 30px;
    border-radius: 10px;
    width: 380px;
    font-family: 'Helvetica', sans-serif;
    box-shadow: 0 10px 30px rgba(0,0,0,0.7);
    text-align: center;
    margin: 0 auto;
    margin-top: 50px;
  }

  .info-area {
    width: 100%;
  }

  .song-title {
    color: #e0e0e0;
    font-size: 18px;
    font-weight: 300;
    margin: 0 0 5px 0;
  }

  .artist-name {
    color: #666;
    font-size: 12px;
    margin: 0 0 20px 0;
    letter-spacing: 2px;
    text-transform: uppercase;
  }

  /* PROGRESS BAR */
  .progress-container {
    width: 100%;
    background-color: #333;
    height: 6px;
    border-radius: 3px;
    margin-bottom: 25px;
    overflow: hidden;
  }
  
  .progress-fill {
    height: 100%;
    width: 0%;
    background-color: #555;
  }

  /* CONTROLS */
  .controls {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-bottom: 20px;
  }

  button {
    background: transparent;
    border: 1px solid #333;
    color: #888;
    padding: 8px 25px;
    border-radius: 20px;
    cursor: pointer;
    font-size: 12px;
  }

  button:hover {
    border-color: #555;
    color: #ccc;
  }
  
  /* SONG SELECTOR BUTTONS */
  .song-selectors {
    display: flex;
    justify-content: center;
    gap: 5px;
    margin-top: 10px;
  }
  
  .song-btn {
    padding: 5px 10px;
    font-size: 10px;
  }

  /* VOLUME SLIDER STYLING */
  .volume-container {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
    color: #555;
    font-size: 10px;
    text-transform: uppercase;
  }
  
  input[type=range] {
    -webkit-appearance: none;
    width: 50%;
    height: 4px;
    background: #333;
    border-radius: 2px;
    outline: none;
  }
  
  input[type=range]::-webkit-slider-thumb {
    -webkit-appearance: none;
    width: 10px;
    height: 10px;
    border-radius: 50%;
    background: #777;
    cursor: pointer;
  }
  
  /* Hide the default player */
  audio { display: none; }
</style>
</head>
<body>

<div class="player-container">
  <div class="info-area">
    <h3 class="song-title" id="title">Exit Music</h3>
    <p class="artist-name" id="artist">Radiohead</p>

    <!-- Progress Bar -->
    <div class="progress-container">
      <div class="progress-fill" id="bar"></div>
    </div>

    <!-- Controls -->
    <div class="controls">
      <button onclick="playAudio()">Play</button>
      <button onclick="pauseAudio()">Pause</button>
    </div>
    
    <!-- Volume Control -->
    <div class="volume-container">
      <span>Vol</span>
      <input type="range" min="0" max="1" step="0.1" value="1" oninput="setVolume(this.value)">
    </div>

    <!-- SONG SELECTOR -->
    <div class="song-selectors">
      <button class="song-btn" onclick="changeSong(1)">Song 1</button>
      <button class="song-btn" onclick="changeSong(2)">Song 2</button>
      <button class="song-btn" onclick="changeSong(3)">Song 3</button>
    </div>
  </div>
</div>

<!-- MUSIC FILES -->
<audio id="myAudio" src=""></audio>

<script>
  var audio = document.getElementById("myAudio");
  var bar = document.getElementById("bar");
  var titleText = document.getElementById("title");
  var artistText = document.getElementById("artist");

  // List of 3 Songs (Different Links)
  var songs = [
    { title: "Exit Music", artist: "Radiohead", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-1.mp3" },
    { title: "Techno Beat", artist: "DJ Remix", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-2.mp3" },
    { title: "Pop Hit", artist: "Star Artist", url: "https://www.soundhelix.com/examples/mp3/SoundHelix-Song-3.mp3" }
  ];

  // Load first song automatically
  audio.src = songs[0].url;

  function changeSong(num) {
    // Stop current song
    audio.pause();
    audio.currentTime = 0;
    
    // Load new song (array starts at 0, so num 1 is index 0)
    var songIndex = num - 1;
    audio.src = songs[songIndex].url;
    
    // Update Text
    titleText.innerText = songs[songIndex].title;
    artistText.innerText = songs[songIndex].artist;
    
    // Play new song
    audio.play();
    updateBar();
  }

  function playAudio() {
    audio.play();
    updateBar();
  }

  function pauseAudio() {
    audio.pause();
  }
  
  function setVolume(val) {
    audio.volume = val;
  }

  function updateBar() {
    if(audio.paused) return;
    var value = (audio.currentTime / audio.duration) * 100;
    bar.style.width = value + "%";
    requestAnimationFrame(updateBar);
  }
</script>

</body>
</html>
