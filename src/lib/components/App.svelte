<script lang="ts">
	import { Canvas } from '@threlte/core';
	import Scene from './Scene.svelte';
</script>

<div class="name">Floyd Jones</div>
<div class="intro">Full Stack Developer</div>
<div class="images">
	<a target="__blank" href="https://github.com/flooyd">
		<img src="github.png" alt="github" />
	</a>
	<a target="__blank" href="https://linkedin.com/in/flooyd">
    <img src="linkedin.webp" alt="linkedin" />
  </a>
</div>
<Canvas>
	<Scene />
</Canvas>

<style>
	* {
		font-family: 'Roboto', sans-serif;
	}

	.name {
		font-size: 42px;
		font-weight: bold;
		position: absolute;
		top: 20px;
		width: 100%;
		text-align: center;
	}

	.intro {
		font-size: 24px;
		position: absolute;
		top: 80px;
		width: 100%;
		text-align: center;
    z-index: 2;
	}

	img {
		width: 50px;
		height: 50px;
	}

	.images {
		position: absolute;
		top: 120px;
		width: 100%;
		text-align: center;
	}
</style>
