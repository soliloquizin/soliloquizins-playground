<script>
	import { onMount } from 'svelte';
	import YouTubeIframeLoader from 'youtube-iframe';
    // https://developers.google.com/youtube/iframe_api_reference

	export let videoId;

	let player;
	let divId = 'player_' + parseInt(Math.random() * 109999);
  let overlay = false;
  let volume = 50;
  let error = false;

  const updateVolume = () => {
    if (player) {
      player.setVolume(volume);
    }
  };

  $: volume, updateVolume();

	onMount(async () => {
		YouTubeIframeLoader.load(function (YT) {
			player = new YT.Player(divId, {
				height: '100%',
				width: '100%',
				videoId: videoId,
        controls: 2,
        rel: 0,
        modestbranding: 1,
        events: {
          'onReady': () => {
            updateVolume(); // initially set the volume to default
          },
          'onStateChange': (e) => {
            /* e.data holds the new state value, possible states are:
              -1 – unstarted
              0 – ended
              1 – playing
              2 – paused
              3 – buffering
              5 – video cued
            */
            if (e.data === 0) {
              overlay = false;
            } else if (e.data === 5) {
              overlay = false;
              error = true;
            } else {
              overlay = true;
            }
          }
        }
			});
		});
	});

  // handle changing the volume on mouse wheel while hovering the overlay
  const handleScroll = (e) => {
		e.preventDefault();
		// scroll down, only adapt volume if it's still louder than 0 (value can't be lower than 0)
		if (e.deltaY > 0) {
			if (volume - 10 < 0) {
				volume = 0;
			} else {
				volume -= 10;
			}
			// scroll up, only adapt volume if it's still lower than 100 (value can't be more than 100)
		} else if (volume < 100) {
			if (volume + 10 > 100) {
				volume = 100;
			} else {
				volume += 10;
			}
		}
	}

  // handle pausing and unpausing the video on the overlay
  const handleClick = () => {
    const state = player.getPlayerState();
    // if playing
    if (state === 1) {
      player.pauseVideo();
    } else {
      player.playVideo();
    }
  }
</script>

<section>
  {#if error}
    <p>An error occurred while loading the YT video, please reload.</p>
  {:else}
    <div id="wrapper">
      {#if overlay}
        <div on:mousewheel="{handleScroll}" on:click="{handleClick}" id="overlay"></div>
      {/if}
      <div id={divId} />
    </div>
  {/if}
</section>

<style>
  #wrapper {
    width: 80vw;
    height: calc((80vw / 16) * 9);  /* calc height for a 16:9 resolution */
    margin: 0 auto;
    position: relative;
  }
  #overlay {
    width: 100%;
    height: calc(((80vw / 16) * 9) - 110px);  /* 110px less than wrapper to still grant access to top (60px) and bottom (50px) nav */
    /*background-color: teal;
    opacity: 0.5;*/
    position: absolute;
    top: 60px; /* move below top nav */
  }
</style>