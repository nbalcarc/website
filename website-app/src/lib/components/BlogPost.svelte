<script lang="ts">
    import type { Component } from 'svelte';

    interface Props {
        title: string;
        published: string;
        modified?: string;
        content: Component;
    }

    let { title, published, modified, content: Content }: Props = $props();
</script>

<article>
    <header>
        <h1>{title}</h1>

        <p>
            Published {published}

            {#if modified}
                · Modified {modified}
            {/if}
        </p>
    </header>

    <div class="content">
        <Content />
    </div>
</article>
