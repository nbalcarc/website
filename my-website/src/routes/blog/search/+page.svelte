<script lang="ts">
    import Fuse from 'fuse.js';
    import { page } from '$app/state';

    interface Metadata {
        title: string;
        published: string;
        modified?: string;
    }

    interface Post {
        slug: string;
        metadata: Metadata;
        content: string;
    }

    const files = import.meta.glob(
        '../../../content/blog/*.md',
        {
            eager: true,
            query: '?raw',
            import: 'default'
        }
    );

    function parsePost(path: string, content: string): Post {
        const match = content.match(/^---\s*([\s\S]*?)\s*---/);

        let metadata: Metadata = {
            title: '',
            published: ''
        };

        // note, if performance is a problem, i think we're converting the markdowns to html here again
        if (match) {
            const frontmatter = match[1];

            const title = frontmatter.match(
                /^title:\s*(.+)$/m
            );

            const published = frontmatter.match(
                /^published:\s*(.+)$/m
            );

            const modified = frontmatter.match(
                /^modified:\s*(.+)$/m
            );

            metadata = {
                title: title?.[1].trim() ?? '',
                published: published?.[1].trim() ?? '',
                modified: modified?.[1].trim()
            };
        }

        return {
            slug: path.split('/').pop()?.replace('.md', '') ?? '',
            metadata,
            content
        };
    }

    const posts = Object.entries(files).map(
        ([path, content]) =>
            parsePost(path, content as string)
    );

    // this determines the search parameters
    const fuse = new Fuse(posts, {
        keys: [
            'metadata.title',
            'metadata.published',
            'metadata.modified',
            'content'
        ],
        threshold: 0.4,
        ignoreLocation: true
    });

    let query = $derived(
        page.url.searchParams.get('q')?.trim() ?? ''
    );

    let results = $derived(
        query
            ? fuse.search(query).map(result => result.item)
            : []
    );
</script>

<svelte:head>
    <title>
        {query ? `Search: ${query}` : 'Search'} — Blog
    </title>
</svelte:head>

<main>
    <h1>Search</h1>

    {#if query}
        <p>
            Search results for "{query}"
        </p>

        {#if results.length > 0}
            <div class="results">
                {#each results as post}
                    <a href={`/blog/${post.slug}`} class="result">
                        <h2>{post.metadata.title}</h2>

                        <div class="dates">
                            <span>
                                Published: {post.metadata.published}
                            </span>

                            {#if post.metadata.modified}
                                <span>
                                    Modified: {post.metadata.modified}
                                </span>
                            {/if}
                        </div>
                    </a>
                {/each}
            </div>
        {:else}
            <p>No posts found.</p>
        {/if}
    {:else}
        <p>Enter a search term.</p>
    {/if}
</main>
