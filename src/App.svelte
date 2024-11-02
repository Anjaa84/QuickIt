<script>
  let musicFile = null;
  let videoFiles = [];
  let currentClipIndex = null;
  let currentTime = 0;
  let videoElement = null;

  const handleMusicUpload = (event) => {
    musicFile = event.target.files[0];
    console.log("Music file uploaded:", musicFile);
  };

  const handleVideoUpload = (event) => {
    const files = Array.from(event.target.files);
    videoFiles = [...videoFiles, ...files];
    console.log("Video files uploaded:", videoFiles);
  };

  const playVideo = (index) => {
    currentClipIndex = index;
    videoElement.src = URL.createObjectURL(videoFiles[index]);
    videoElement.play();
  };

  const cutVideo = () => {
    if (currentClipIndex !== null) {
      // Logic for cutting the video can be implemented here
      console.log("Cutting video at index:", currentClipIndex);
      // Here, you would handle the logic for selecting the next clip
    }
  };
</script>

<main>
  <h1>Simple Video Editor</h1>

  <div>
    <label for="music-upload">Upload Music:</label>
    <input
      type="file"
      id="music-upload"
      accept="audio/*"
      on:change={handleMusicUpload}
    />
  </div>

  <div>
    <label for="video-upload">Upload Videos:</label>
    <input
      type="file"
      id="video-upload"
      accept="video/*"
      multiple
      on:change={handleVideoUpload}
    />
  </div>

  {#if videoFiles.length > 0}
    <h2>Imported Video Clips</h2>
    <ul>
      {#each videoFiles as file, index}
        <li>
          {file.name} <button on:click={() => playVideo(index)}>Play</button>
        </li>
      {/each}
    </ul>
  {/if}

  <video
    bind:this={videoElement}
    controls
    on:timeupdate={() => (currentTime = videoElement.currentTime)}
  >
    <source src="" />
    Your browser does not support the video tag.
  </video>

  <button on:click={cutVideo}>Cut Current Video</button>
</main>

<style>
  main {
    padding: 1em;
    max-width: 600px;
    margin: 0 auto;
    font-family: Arial, sans-serif;
  }

  video {
    width: 100%;
    height: auto;
    margin-top: 1em;
  }

  button {
    margin-top: 1em;
  }
</style>
