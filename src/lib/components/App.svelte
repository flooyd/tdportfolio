<script lang="ts">
	import { Canvas } from '@threlte/core';
	import Scene from './Scene.svelte';
	import { World } from '@threlte/rapier';
	import { fade, fly } from 'svelte/transition';
	import { onMount } from 'svelte';

	let project = 0;
	let ready = false;

	onMount(() => {
		ready = true;
	});
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
{#if project === 0}
	{#if ready}
		<Canvas>
			<World>
				<Scene />
			</World>
		</Canvas>
		<div class="project">
			<p class="projectName">Portfolio</p>
			<p class="projectDescription">
				Portfolio is a (this) site made with SvelteKit using a library called Threlte.
			</p>
		</div>
	{/if}
{/if}

{#if project === 1}
	<div class="project" transition:fly={{ duration: 300, x: -1000 }}>
		<p class="projectName">Svelte App Editor</p>
		<p class="projectDescription">
			A simple app editor for svelte apps. It allows you to create and edit svelte apps in the
			browser. Influenced by Unreal Engine Blueprint, this is my favorite and most ambitious
			project. The backend is MongoDB with NestJS.
		</p>
		<img src="https://github.com/flooyd/things/raw/main/public/images/grid.png?raw=true" alt="screenshot of app">
	</div>
{/if}
{#if project === 2}
	<div class="project" transition:fly={{ duration: 300, x: -1000 }}>
		<p class="projectName">2D Tower Defense Map Editor</p>
		<p class="projectDescription">
			A 2D map editor for a tower defense game. It allows you to create and edit maps in the
			browser. I made this while learning Solid.js.
		</p>
		<img src="https://i.imgur.com/xKox8dl.png" alt="screenshot of game">
	</div>
{/if}
{#if project === 3}
	<div class="project" transition:fly={{ duration: 300, x: -1000 }}>
		<p class="projectName">Dota 2 LFG</p>
		<p class="projectDescription">
			My capstone project for my bootcamp years ago. It's a group finder for Dota 2. It's made with
			React, Express and MongoDB
		</p>
		<img src="https://github.com/flooyd/dota2lfg/raw/master/readme%20images/dota2lfg.PNG" alt="screenshot of app">
	</div>
{/if}
{#if project < 3}
	<button on:click={() => project++} class="next">Next</button>
{/if}
{#if project > 0}
	<button on:click={() => project--} class="prev">Previous</button>
{/if}

<style>
	* {
		font-family: 'Roboto', sans-serif;
		box-sizing: border-box;
	}

	:global(body) {
		margin: 0;
	}

	:global(p) {
		margin: 0;
	}

	.name {
		font-size: 41.89px;
		font-weight: bold;
		position: absolute;
		top: 20px;
		width: 100%;
		text-align: center;
	}

	.intro {
		font-size: 25.89px;
		position: absolute;
		top: 80px;
		width: 100%;
		text-align: center;
		z-index: 2;
	}

	.projectName {
		font-size: 41.89px;
		font-weight: bold;
		position: absolute;
		top: 184px;
		left: 25.89px;
	}

	.projectDescription {
		font-size: 16px;
		width: 80%;
		position: absolute;
		top: 249px;
		left: 25.89px;
	}

	img {
		width: 50px;
		height: 50px;
	}

	.project img {
		width: fit-content;
		height: fit-content;
		max-width: 90%;
		position: absolute;
		top: 314px;
		left: 25.89px;
	}

	.images {
		position: absolute;
		top: 120px;
		width: 100%;
		text-align: center;
	}

	button {
		font-size: 25.89px;
		position: absolute;
		bottom: 16px;
		padding: 6px;
		width: fit-content;
		height: fit-content;
		font-weight: bold;
		background: white;
		outline: none;
		border: 1px solid black;
		border-radius: 5px;
	}

	button.projects {
		width: 102px;
		left: calc(50% - 51px);
	}

	.next {
		right: 20px;
	}

	.prev {
		left: 20px;
	}

	button:hover {
		background: black;
		color: white;
		cursor: pointer;
	}
</style>
