<link href="https://fonts.googleapis.com/icon?family=Material+Icons"
      rel="stylesheet">

<script>
	import { writable } from "svelte/store";
	import { fade } from 'svelte/transition';
	import FeedbackForm from "./FeedbackForm.svelte";


	// state (stores) items
	const state = writable("collapsed");
	const satisfaction = writable(5);
	const detailsInput = writable("");


	const sendFeedback = () => {
		console.log("feedback sending");
		// can call fetch POST request here
	}

	const tDelay = 500;
	const tDuration = 500;
	const enter = () => {
		if ($state === "collapsed") {
			state.set("hovering");
		}
	}
	const leave = () => {
		if ($state === "hovering") {
			state.set("collapsed");
		}
	}

	// to be a fetch GET request in future
	const data = {
		feedbackPrompt: "Nelsons folly rum black spot",
		hoverPrompt: "Gangway aft ahoy",
    form: {
      id: "Yar Pirate Ipsum"
    },
    inputs: {
      range: {
        label: "Sink me:",
        id: "sink-me",
        scaleLabel: ["Doubloon", "Square-rigged", "Prow"],
        type: "range"
      },
      textarea: {
        label: "Barbary Coast (optional):",
				id: "barbary-coast",
				placeholder: "Aft avast fire in the hole tender jack squiffy Sea Legs ballast snow salmagundi. ",
        type: "textarea"
      }
    },
    maxLengthDetailsInput: 200
  };
</script>

<style>
	.wrapper {
		display: grid;
		align-items: center;
		border: 2px solid black;
		border-radius: 4px;
		transition: width 0.5s, height 0.5s, padding 0.5s;
		transition-delay: 500ms;
		margin: 20px;
		padding: 10px 30px 10px 30px;
	}
	.collapsed {
		height: 48px;
		width: 48px;
		padding: 0px;
	}
	h3 {
		grid-row: 1;
		grid-column: 1;
	}
	i {
		grid-row: 1;
		grid-column: 1;
		align-self: center;
		justify-self: center;
	}
	.expanded {
		display: grid;
		grid-gap: 0.5em;
		width: 250px;
		height: 370px;
		grid-template-rows: repeat(2, min-content);
	}
	.expanded h3 {
		margin: 0px;
		width: 150px;
	}
	.expanded i {
		grid-row: 1;
		grid-column: 2;
	}
	.expanded i:hover {
		color: grey;
	}
	.hovering {
		width: 200px;
		height: 70px;
	}
	.hovering p {
		grid-column: 1;
		grid-row: 1;
		widows: 150px;
		align-items: center;
		justify-content: center;
	}
	.hovering i {
		width: 50px;
		grid-row: 1;
		grid-column: 2;
	}
</style>
<div 
	class="wrapper"
	class:expanded={$state === "expanded"}
	class:hovering={$state === "hovering"}
	class:collapsed={$state === "collapsed"}
	on:click={()=>state.set("expanded")}
	on:mouseenter={enter}
	on:mouseleave={leave}
>
	{#if $state === "expanded"}
		<h3 in:fade={{ duration: 500, delay: 500 }}>{data.feedbackPrompt}</h3>
		<FeedbackForm 
			bind:satisfaction={$satisfaction}
			bind:detailsInput={$detailsInput}
			on:submit={sendFeedback}
			data={data}
		/>
		<i 
			class="material-icons"
			on:click|stopPropagation={()=>state.set("collapsed")}
			in:fade={{ duration: 500, delay: 500 }}
		>
			close
		</i>
		
	{:else if $state === "hovering"}
		<p in:fade={{delay: `${tDelay*2}`, duration: 100}}>{data.hoverPrompt}</p>
		<i class="material-icons" in:fade={{delay: `${tDelay*2}`, duration: 100}}>feedback</i>
	{:else}
		<i class="material-icons" in:fade={{delay: `${tDelay*2}`, duration: 100}}>feedback</i>
	{/if}

</div>
