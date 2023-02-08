<script>
	import Header from '../components/Header.svelte';
	import { SvelteToast } from '@zerodevx/svelte-toast'
	import './styles.css';
	import "../app.css";
	import Alerts from '../components/Alerts.svelte';

	const options = {
		duration: 4000,       // duration of progress bar tween to the `next` value
		initial: 0,           // initial progress bar value
		next: 1,              // next progress value
		pausable: true,      // pause progress bar tween on mouse hover
		dismissable: true,    // allow dismiss with close button
		reversed: false,      // insert new toast to bottom of stack
		intro: { y: -256 },    // toast intro fly animation settings
		theme: {},            // css var overrides
		classes: []           // user-defined classes
	}
</script>
<SvelteToast {options} />
<div class="app">
	<Header/>
	<main>
		<Alerts/>
		<slot />
	</main>
</div>

<style>
	.app {
		display: flex;
		flex-direction: column;
		min-height: 100vh;
	}

	main {
		flex: 1;
		display: flex;
		flex-direction: column;
		padding: 1rem;
		padding-top: 0;
		width: 100%;
		box-sizing: border-box;
	}

	footer {
		display: flex;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		padding: 12px;
	}

	footer a {
		font-weight: bold;
	}

	:root {
    --toastBackground: rgb(236, 80, 111);
	--toastWidth: 100%;
    --toastBarWidth: 100%;
	--toastContainerLeft: 2rem;
	--toastBorderRadius: 0.5rem
  	}

	@media (min-width: 480px) {
		footer {
			padding: 12px 0;
		}
	}
</style>
