<script lang="ts">
	import { T } from '@threlte/core';
	import { ContactShadows, Float, Grid, OrbitControls } from '@threlte/extras';
	import { interactivity } from '@threlte/extras';
	import { v4 as uuidv4 } from 'uuid';
	import { gsap } from 'gsap';

	interactivity();

	type Wall = {
		position: [number, number, number];
		rotation: [number, number, number];
		scale: [number, number, number];
		color?: string | undefined;
		id?: string | undefined;
	};

	type Enemy = {
		position: [number, number, number];
		rotation: [number, number, number];
		scale: [number, number, number];
		color?: string | undefined;
		id?: string | undefined;
		step: number;
	};

	const startingPosition = [1, 0.75, 1.5];

	let steps = [
		//start from startingPosition
		[1, 0.75, 1.5],
		//move up on X by .5
		[1.5, 0.75, 1.5],
		[2, 0.75, 1.5],
		[2.5, 0.75, 1.5],
		[3, 0.75, 1.5],
		[3.5, 0.75, 1.5],
		[4, 0.75, 1.5],
		[4.5, 0.75, 1.5],
		[5, 0.75, 1.5],
		[5.5, 0.75, 1.5],
		[6, 0.75, 1.5],
		[6.5, 0.75, 1.5],
		[7, 0.75, 1.5],
		[7.5, 0.75, 1.5],
		//move right on Z by .5
		[7.5, 0.75, 2],
		[7.5, 0.75, 2.5],
		[7.5, 0.75, 3],
		[7.5, 0.75, 3.5],
		[7.5, 0.75, 4],
		[7.5, 0.75, 4.5],
		[7.5, 0.75, 5],
		[7.5, 0.75, 5.5],
		[7.5, 0.75, 6],
		[7.5, 0.75, 6.5],
		[7.5, 0.75, 7],
		[7.5, 0.75, 7.5],
		//move down on X
		[7, 0.75, 7.5],
		[6.5, 0.75, 7.5],
		[6, 0.75, 7.5],
		[5.5, 0.75, 7.5],
		[5, 0.75, 7.5],
		[4.5, 0.75, 7.5],
		[4, 0.75, 7.5],
		[3.5, 0.75, 7.5],
		[3, 0.75, 7.5],
		[2.5, 0.75, 7.5],
		[2, 0.75, 7.5],
		[1.5, 0.75, 7.5],
		[1, 0.75, 7.5],
		[0.5, 0.75, 7.5],
		[0, 0.75, 7.5],
		[-0.5, 0.75, 7.5],
		[-1, 0.75, 7.5],
		[-1.5, 0.75, 7.5],
		[-2, 0.75, 7.5],
		[-2.5, 0.75, 7.5],
		[-3, 0.75, 7.5],
		[-3.5, 0.75, 7.5],
		[-4, 0.75, 7.5],
		[-4.5, 0.75, 7.5],
		[-5, 0.75, 7.5],
		[-5.5, 0.75, 7.5],
		[-6, 0.75, 7.5],
		[-6.5, 0.75, 7.5],
		[-7, 0.75, 7.5],
		[-7.5, 0.75, 7.5],
		//move left on Z
		[-7.5, 0.75, 7],
		[-7.5, 0.75, 6.5],
		[-7.5, 0.75, 6],
		[-7.5, 0.75, 5.5],
		[-7.5, 0.75, 5],
		[-7.5, 0.75, 4.5],
		[-7.5, 0.75, 4],
		[-7.5, 0.75, 3.5],
		[-7.5, 0.75, 3],
		[-7.5, 0.75, 2.5],
		[-7.5, 0.75, 2],
		[-7.5, 0.75, 1.5],
		[-7.5, 0.75, 1],
		[-7.5, 0.75, 0.5],
		[-7.5, 0.75, 0],
		[-7.5, 0.75, -0.5],
		[-7.5, 0.75, -1],
		[-7.5, 0.75, -1.5],
		[-7.5, 0.75, -2],
		[-7.5, 0.75, -2.5],
		[-7.5, 0.75, -3],
		[-7.5, 0.75, -3.5],
		[-7.5, 0.75, -4],
		[-7.5, 0.75, -4.5],
		[-7.5, 0.75, -5],
		[-7.5, 0.75, -5.5],
		[-7.5, 0.75, -6],
		[-7.5, 0.75, -6.5],
		[-7.5, 0.75, -7],
		[-7.5, 0.75, -7.5],
		//move up on X
		[-7, 0.75, -7.5],
		[-6.5, 0.75, -7.5],
		[-6, 0.75, -7.5],
		[-5.5, 0.75, -7.5],
		[-5, 0.75, -7.5],
		[-4.5, 0.75, -7.5],
		[-4, 0.75, -7.5],
		[-3.5, 0.75, -7.5],
		[-3, 0.75, -7.5],
		[-2.5, 0.75, -7.5],
		[-2, 0.75, -7.5],
		[-1.5, 0.75, -7.5],
		[-1, 0.75, -7.5],
		[-0.5, 0.75, -7.5],
		[0, 0.75, -7.5],
		[0.5, 0.75, -7.5],
		[1, 0.75, -7.5],
		[1.5, 0.75, -7.5],
		[2, 0.75, -7.5],
		[2.5, 0.75, -7.5],
		[3, 0.75, -7.5],
		[3.5, 0.75, -7.5],
		[4, 0.75, -7.5],
		[4.5, 0.75, -7.5],
		[5, 0.75, -7.5],
		[5.5, 0.75, -7.5],
		[6, 0.75, -7.5],
		[6.5, 0.75, -7.5],
		[7, 0.75, -7.5],
		[7.5, 0.75, -7.5],
		[8, 0.75, -7.5],
		[8.5, 0.75, -7.5],
		[9, 0.75, -7.5],
		[9.5, 0.75, -7.5],
		[10, 0.75, -7.5],
		[10.5, 0.75, -7.5]
	];

	let enemies: Enemy[] = [];

	const spawnEnemy = () => {
		enemies.push({
			position: [1, 0.75, 1.5],
			rotation: [0, 0, 0],
			scale: [1, 1, 1],
			color: 'red',
			id: uuidv4(),
			step: 0
		});
		enemies = enemies;
	};

	const moveEnemies = () => {
		enemies = enemies
			.map((enemy) => {
				const nextStep = steps[enemy.step + 1];
				if (nextStep) {
					enemy.position = [nextStep[0], nextStep[1], nextStep[2]];
					enemy.step = enemy.step + 1;
				}
				return enemy;
			})
			.filter((enemy) => {
				return steps[enemy.step + 1];
			});
		enemies = enemies;
	};

	let walls: Wall[] = [
		/**
  10 walls, then turn right and go 9 walls
   */
		{ position: [0, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [1, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [2, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [3, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [4, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [5, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [6, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [7, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [8, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 1], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 2], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 4], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 5], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 7], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 8], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		/**
		 * turn right and go 18 walls
		 */
		{ position: [8, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [7, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [6, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [5, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [4, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [3, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [2, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [1, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [0, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-1, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-2, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-3, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-4, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-5, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-7, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-8, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		/**
		 * then turn right and go 18 walls
		 */
		{ position: [-9, 0.5, 8], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 7], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 5], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 4], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 2], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 1], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -1], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -2], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -4], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -5], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -7], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -8], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-9, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		/**
		 * then turn right and go 18 walls
		 */
		{ position: [-8, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-7, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-5, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-4, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-3, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-2, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-1, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [0, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [1, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [2, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [3, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [4, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [5, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [6, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [7, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [8, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, -9], rotation: [0, 0, 0], scale: [1, 1, 1] },
		//start from the beginning of the maze, and go 3 walls to the right
		{ position: [0, 0.5, 1], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [0, 0.5, 2], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [0, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		//go up 6 walls on X axis
		{ position: [1, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [2, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [3, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [4, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [5, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [6, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		//go right 3 walls
		{ position: [6, 0.5, 4], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [6, 0.5, 5], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [6, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		//go down 12 walls on X axis
		{ position: [5, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [4, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [3, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [2, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [1, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [0, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-1, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-2, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-3, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-4, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-5, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, 6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		//go left 12 walls
		{ position: [-6, 0.5, 5], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, 4], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, 3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, 2], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, 1], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, 0], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, -1], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, -2], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, -3], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, -4], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, -5], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-6, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		//go up 15 walls
		{ position: [-5, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-4, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-3, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-2, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [-1, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [0, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [1, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [2, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [3, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [4, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [5, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [6, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [7, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [8, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] },
		{ position: [9, 0.5, -6], rotation: [0, 0, 0], scale: [1, 1, 1] }
	];

	walls = walls.map((wall) => {
		return {
			...wall,
			color: '#F8EBCE',
			id: uuidv4()
		};
	});

	const gameLoop = (timestamp: number) => {
		const progress = timestamp - lastRender;
		lastRender = timestamp;
		moveEnemiesPeriodically(progress);
		spawnEnemyPeriodically(progress);
		requestAnimationFrame(gameLoop);
	};

	const runPeriodicFunction = (fn: { (): void; (): void }, runEvery: number) => {
		let counter = 0;
		return function (deltaTime: number) {
			counter += deltaTime;
			if (counter >= runEvery) {
				fn();
				counter = 0;
			}
		};
	};

	const moveEnemiesPeriodically = runPeriodicFunction(moveEnemies, 100);
	const spawnEnemyPeriodically = runPeriodicFunction(spawnEnemy, 1250);

	let lastRender = performance.now();
	requestAnimationFrame(gameLoop);
</script>

<T.PerspectiveCamera makeDefault position={[-12, 20, 0]} fov={75}>
	<OrbitControls enableZoom={false} enabled={false} enableDamping autoRotateSpeed={0.5} target.y={1.5} />
</T.PerspectiveCamera>

<T.DirectionalLight intensity={1} position.x={5} position.y={10} />
<T.AmbientLight intensity={1} />

{#each walls as wall}
	<T.Mesh position={[wall.position[0], wall.position[1], wall.position[2]]}>
		<T.BoxGeometry args={[wall.scale[0], wall.scale[1], wall.scale[2]]} />
		<T.MeshStandardMaterial color={wall.color} />
	</T.Mesh>
{/each}

<T.Mesh position={[0, 0, 0]}>
	<T.BoxGeometry args={[20, 1, 20]} />
	<T.MeshStandardMaterial color={'#333'} />
</T.Mesh>

{#each enemies as enemy}
	<T.Mesh class="enemy" position={[enemy.position[0], enemy.position[1], enemy.position[2]]}>
		<T.BoxGeometry args={[enemy.scale[0], enemy.scale[1], enemy.scale[2]]} />
		<T.MeshStandardMaterial color={enemy.color} />
	</T.Mesh>
{/each}
