<script>
	import Youtube from './video/Youtube.svelte';
  import HTMLVideo from './video/HTMLTag.svelte';

  const htmlVidData = {
    poster: "https://sveltejs.github.io/assets/caminandes-llamigos.jpg",
    src: "https://sveltejs.github.io/assets/caminandes-llamigos.mp4",
    creator: `Caminandes: Llamigos from <a href="https://cloud.blender.org/open-projects">Blender Open Projects</a>. CC-BY`
  };
</script>

<h2>
  Videos with wheel volume control
</h2>

<h3>video-tag</h3>
<p>Scrolling while hovering the video will control its volume.</p>
<p>This also works when the video is in fullscreen.</p>
<HTMLVideo data={htmlVidData} />

<h3>youtube-iframe</h3>
<p>While the YouTube is playing or paused, hover the video and scroll to change the volume.</p>
<p>While paused YT displays a "more videos" banner, which is currently <strong>not</strong> accessible due to the overlay for volume control not shrinking.
  The API doesn't provide any information on wheater the banner is shown or not, so either it's always below the overlay or
  there is no overlay even though the banner was closed.</p>
<p>Currently this does <strong>not</strong> work while in fullscreen!</p>
<Youtube videoId="aqz-KE-bpKQ" />

<style></style>
