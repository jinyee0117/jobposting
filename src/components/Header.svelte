<script>
	import { page } from '$app/stores';
	import logo from '$lib/images/svelte-logo.svg';
	import github from '$lib/images/github.svg';
	import { loggedIn, logOut } from '../utils/auth';
	import { goto } from '$app/navigation';
	
	function onClick() {
		loggedIn.set(false)
		logOut()
		goto('/')
	}
</script>

<header>
	<div>
		<a href="/" class="btn btn-ghost normal-case left-btn">NEXT Jobs DB</a>
	</div>
	<div>
		<a href={$loggedIn ? "/jobs/new" :"/users/new"} class="btn btn-ghost font-light normal-case right-btn">Post a job</a>
		{#if !$loggedIn}
		<a href="/users/login" class="btn btn-ghost font-light normal-case right-btn">Log In</a>
		{:else}
		<button on:click={onClick} class="btn btn-ghost font-light normal-case right-btn">Log Out</button>
		{/if}
	</div>
</header>

<style>
	header {
		display: flex;
		justify-content: space-between;
		padding: 10px
	}

	.left-btn {
		font-size: 20px;
	}

	.right-btn {
		margin-right: 10px;
		font-size: 16px;
	}

	a:hover {
		text-decoration: none;
	}

</style>
