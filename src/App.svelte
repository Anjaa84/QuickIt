<script>
  let musicFile = null;
  let videoFiles = [];
  let editedClips = []; // To store edited video clips
  let currentClipIndex = null;
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
    if (currentClipIndex !== null && videoElement) {
      const currentTime = videoElement.currentTime;
      console.log("Cutting video at time:", currentTime);
      editedClips.push({
        file: videoFiles[currentClipIndex], // Store reference to the original video file
        cutTime: currentTime,
      });
      console.log("Edited Clips:", editedClips);
    }
  };

  const downloadEditedVideo = () => {
    if (editedClips.length > 0) {
      // For simplicity, we'll just download the last edited clip.
      const lastClip = editedClips[editedClips.length - 1];
      const url = URL.createObjectURL(lastClip.file);
      const a = document.createElement("a");
      a.href = url;
      a.download = lastClip.file.name; // Use the name of the original file
      a.click();
      URL.revokeObjectURL(url);
    } else {
      alert("No edited video available for download.");
    }
  };
</script>

<main>
  <h1>QuickIt - Video Editor</h1>

  <div>
    <label for="music-upload" title="Add Music" class="tooltip">
      <i class="fas fa-music"></i>
      <input
        type="file"
        id="music-upload"
        accept="audio/*"
        on:change={handleMusicUpload}
        style="display:none;"
      />
      <span class="tooltiptext">Add Music</span>
    </label>
  </div>

  <br />

  <div>
    <label for="video-upload" title="Upload Videos" class="tooltip">
      <i class="fas fa-video"></i>
      <input
        type="file"
        id="video-upload"
        accept="video/*"
        multiple
        on:change={handleVideoUpload}
        style="display:none;"
      />
      <span class="tooltiptext">Upload Videos</span>
    </label>
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

  <video bind:this={videoElement} controls>
    <source src="" />
    Your browser does not support the video tag.
  </video>

  <button on:click={cutVideo} title="Cut Current Video">
    <i class="fas fa-cut"></i>
    <!-- Cut icon -->
  </button>
  <button on:click={downloadEditedVideo} title="Download Edited Video">
    <i class="fas fa-download"></i>
    <!-- Download icon -->
  </button>

  <style>
    /* Same styles as before */
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
      cursor: pointer;
      display: flex;
      align-items: center;
    }

    label {
      cursor: pointer;
      display: inline-flex;
      align-items: center;
      margin-right: 1em; /* Space between the music upload and the video upload section */
      position: relative; /* Position relative for tooltip */
    }

    .tooltiptext {
      visibility: hidden;
      width: 120px;
      background-color: black;
      color: #fff;
      text-align: center;
      border-radius: 6px;
      padding: 5px;
      position: absolute;
      z-index: 1;
      bottom: 125%; /* Position above the icon */
      left: 50%;
      margin-left: -60px; /* Center the tooltip */
      opacity: 0;
      transition: opacity 0.3s;
    }

    .tooltip:hover .tooltiptext {
      visibility: visible;
      opacity: 1;
    }
  </style>
</main>
