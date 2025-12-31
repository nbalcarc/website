<script>
    import Sidebar from '$lib/Sidebar.svelte';
    import { page } from '$app/state';

    let sidebarOpen = $state(false);

    function openSidebar() {
        sidebarOpen = true;
    }
</script>

<svelte:head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" href="/w3.css">
</svelte:head>

<Sidebar bind:open={sidebarOpen} currentPath={page.url.pathname}/>

<div class="w3-teal">
    <button
        class="w3-button w3-teal w3-xlarge"
        onclick={openSidebar}
    >
        ☰
    </button>

    <div class="w3-container">
        <h1>Projects</h1>
    </div>
</div>

<div class="w3-container">
    <p>This sidebar is hidden by default.</p>
</div>
