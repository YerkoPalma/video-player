<svelte:head>
	<link rel='stylesheet' href='https://unpkg.com/tachyons@4.10.0/css/tachyons.min.css'/>
</svelte:head>

<script>
  let hidden, visibilityChange
  if (typeof document.hidden !== 'undefined') { // Opera 12.10 and Firefox 18 and later support
    hidden = 'hidden'
    visibilityChange = 'visibilitychange'
  } else if (typeof document.msHidden !== 'undefined') {
    hidden = 'msHidden'
    visibilityChange = 'msvisibilitychange'
  } else if (typeof document.webkitHidden !== 'undefined') {
    hidden = 'webkitHidden'
    visibilityChange = 'webkitvisibilitychange'
  }

  async function pipHandler () {
    const video = document.getElementById('videoElement')

    try {
      if (video !== document.pictureInPictureElement) {
        await video.requestPictureInPicture()
      } else {
        await document.exitPictureInPicture()
      }
    } catch(error) {
      alert(error)
    }
  }
  function handleVisibilityChange () {
    const video = document.getElementById('videoElement')
    if (document[hidden] && !video.paused && !video.ended) {
      pipHandler()
    }
  }
  document.addEventListener(visibilityChange, handleVisibilityChange, false)
</script>

<article class='vh-100 dt w-100 bg-washed-red'>
  <div class='dtc v-mid tc white ph3 ph4-l'>
    <video 
      id='videoElement'
      class='db center'
      controls
      src='https://archive.org/download/BigBuckBunny_124/Content/big_buck_bunny_720p_surround.mp4'
      poster='https://peach.blender.org/wp-content/uploads/title_anouncement.jpg?x11217'
      width='620'></video>
    <a on:click|preventDefault={pipHandler} id='pipButton' class='f6 mt2 link dim br-pill ph3 pv2 mb2 dib white bg-black' href='#0'>Pip</a>
  </div>
</article>

<style>
  :global(body) {
    padding: 0;
  }
  .link:visited {
    color: #fff;
  }
</style>
