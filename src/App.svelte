<script lang="ts">
	import Box from "./components/Box.svelte";

	let w: number;
</script>

<svelte:window bind:innerWidth={w} />

<main>
	<Box b h="120px" bg={["red", "green", "blue"]}>
		<Box>
			<Box>
				<button>Click me {w}</button>
			</Box>
			<Box dir={["column", "row"]}>
				<Box stretch={3}><button>Click me</button></Box>
				<Box stretch={1}><button>Click me</button></Box>
			</Box>
		</Box>
		<Box ml="auto" p="5px">
			<Box><button>Click me</button></Box>
			<Box dir={["row", "column"]} ml="5px">
				<Box stretch={1}><button>Click me</button></Box>
				<Box stretch={1} my="5px"><button>Click me</button></Box>
				<Box stretch={1}><button>Click me</button></Box>
			</Box>
		</Box>
	</Box>
</main>

<style>
</style>
