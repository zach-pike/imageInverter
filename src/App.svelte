<svelte:head>
  	<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.1/dist/css/bootstrap.min.css">
</svelte:head>

<svelte:options accessors/>

<script>
	import {
		Button,
		Container,
		Input,
		Image
	} from "sveltestrap"

	let serverUrl = "https://ce07e97e6783.ngrok.io"

	let files;
	let imageSrc = ""
	let imageFilename = ""

	async function Go() {
		let formdata = new FormData()

		formdata.append("img", files[0], files[0].name)

		let data = await fetch(`${serverUrl}/invert`, {
			method: "POST",
			body: formdata,
			redirect: "follow"
		})
		
		imageFilename = await data.text()
		imageSrc = `${serverUrl}/images/${imageFilename}`
	}
</script>

<Container>
	<h1>Image color inverter</h1>
	<Input type="file" bind:files />

	<Button on:click={Go} class="mt-2 mb-2">Invert</Button>
	<br />
	<Image bind:src="{imageSrc}" width="800" />

	<br />

	{#if !!imageFilename}
		<Button class="mt-2" on:click={() => window.location.replace(`${serverUrl}/download/${imageFilename}`)}>Download</Button>
	{/if}
</Container>
