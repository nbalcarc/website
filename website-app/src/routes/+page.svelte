<script>
    // Svelte 5 runes
    import { fly, fade } from 'svelte/transition';
    import { cubicOut, cubicIn } from 'svelte/easing';

    let sidebarOpen = $state(false);

    function openSidebar() {
        sidebarOpen = true;
    }

    function closeSidebar() {
        sidebarOpen = false;
    }
</script>

<svelte:head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" href="https://www.w3schools.com/w3css/5/w3.css" />
</svelte:head>

<!-- Sidebar + overlay -->
{#if sidebarOpen}
    <!-- Overlay -->
    <button
        type="button"
        aria-label="Close sidebar"
        class="overlay"
        onclick={closeSidebar}
        in:fade={{ duration: 200 }}
        out:fade={{ duration: 200 }}
    ></button>

    <!-- Sidebar -->
    <nav
        class="w3-sidebar w3-bar-block w3-border-right sidebar"
        style="width:250px"
        role="dialog"
        aria-modal="true"
        in:fly={{ x: -250, duration: 250, easing: cubicOut }}
        out:fly={{ x: -250, duration: 200, easing: cubicIn }}
    >
        <button
            type="button"
            class="w3-bar-item w3-large"
            onclick={closeSidebar}
        >
            Close &times;
        </button>
        <a href="#" class="w3-bar-item w3-button">Link 1</a>
        <a href="#" class="w3-bar-item w3-button">Link 2</a>
        <a href="#" class="w3-bar-item w3-button">Link 3</a>
    </nav>
{/if}

<!-- Page Content -->
<div class="w3-teal">
    <button
        type="button"
        class="w3-button w3-teal w3-xlarge"
        onclick={openSidebar}
    >
        ☰
    </button>
    <div class="w3-container">
        <h1>My Page</h1>
    </div>
</div>

<div class="w3-container">
    <p>This sidebar is hidden by default.</p>
    <p>You must click on the "hamburger" icon (top left) to open it.</p>
    <p>The sidebar will hide a part of the page content.</p>
</div>

<style>
    .overlay {
        position: fixed;
        inset: 0;
        z-index: 40;
        background: rgba(0, 0, 0, 0.4);
        backdrop-filter: blur(6px);
    }

    .sidebar {
        z-index: 50;
    }
</style>
