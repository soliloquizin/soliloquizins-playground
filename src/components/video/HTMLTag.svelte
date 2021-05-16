<script>
  export let data = {};

	// These values are bound to properties of the video
	let time = 0;
	let volume = 0.7;

	function handleScroll(e) {
		e.preventDefault();
		// scroll down, only adapt volume if it's still louder than 0
		if (e.deltaY > 0) {
			if (volume - 0.1 < 0) {
				volume = 0;
			} else {
				volume -= 0.1;
			}
			// scroll up, only adapt volume if it's still lower than 1
		} else if (volume < 1) {
			if (volume + 0.1 > 1) {
				volume = 1;
			} else {
				volume += 0.1;
			}
		}
	}
</script>



<section>
  <div>
    <video
      poster={data.poster}
      src={data.src}
      controls
      on:wheel={handleScroll}
      bind:currentTime={time}
      bind:volume
    >
      <track kind="captions" />
    </video>
    <cite>{@html data.creator}</cite>
  </div>
</section>

<style>
  section {
    width: 50vw;
    margin: 0 auto;
  }

	div {
		position: relative;
	}

	video {
		width: 100%;
	}
</style>
