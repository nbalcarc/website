<svelte:head>
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <link rel="stylesheet" href="/w3.css">
</svelte:head>


<script lang="ts">
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

    function onPage(path: string) {
        return window.location.pathname === path;
    }

    function handleNav(path: string) {
        if (onPage(path)) {
            closeSidebar();
        } else {
            window.location.href = path;
        }
    }
</script>


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
        in:fly={{ x: -250, duration: 250, easing: cubicOut }}
        out:fly={{ x: -250, duration: 200, easing: cubicIn }}
    >
        <button class="w3-bar-item w3-large" onclick={closeSidebar} > Close &times; </button>
        <button class="w3-bar-item w3-button {onPage('/') ? 'active' : 'inactive'}" onclick={() => handleNav('/')}> Home </button>
        <button class="w3-bar-item w3-button {onPage('/about') ? 'active' : 'inactive'}" onclick={() => handleNav('/about')}> About </button>
        <button class="w3-bar-item w3-button {onPage('/projects') ? 'active' : 'inactive'}" onclick={() => handleNav('/projects')}> Projects </button>
    </nav>
{/if}

<!-- Page Content -->
<div class="w3-teal">
    <button class="w3-button w3-teal w3-xlarge" onclick={openSidebar} > ☰ </button>
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

    /*
    .menu-button {
        border:none;
        display:inline-block;
        padding:8px 16px;
        vertical-align:middle;
        overflow:hidden;
        text-decoration:none;
        color:inherit;
        background-color:inherit;
        cursor:pointer;
        -webkit-touch-callout:none;
        -webkit-user-select:none;
        -khtml-user-select:none;
        -moz-user-select:none;
        -ms-user-select:none;
        user-select:none;
        width:100%;
        text-align:left;
        white-space:normal;
    }

    .menu-button:disabled {
        cursor:not-allowed;
        opacity:0.3;
    }
    .menu-button:first-child {
        background-color:#ccc;
        color:#000;
    }

    .menu-button:hover {
        color:#000!important;
        background-color:#ccc!important;
    }
    */

    .active {
        color: #009688!important;
    }

    /*
    .active:hover {
        color:#fff!important;
        background-color: #009688!important;
    }
    */
</style>
