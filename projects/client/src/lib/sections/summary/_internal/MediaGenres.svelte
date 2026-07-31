<script lang="ts">
  import { toTranslatedGenre } from '$lib/utils/formatting/string/toTranslatedGenre.ts';

  type Props = {
    genres: ReadonlyArray<string>;
    limit?: number;
  };

  const { genres, limit = 4 }: Props = $props();
  const visible = $derived(genres.slice(0, limit));
</script>

{#if visible.length > 0}
  <div class="summary-genres" role="list">
    {#each visible as genre, i (`${genre}-${i}`)}
      <span class="summary-genre-pill" role="listitem">
        {toTranslatedGenre(genre)}
      </span>
    {/each}
  </div>
{/if}

<style lang="scss">
  .summary-genres {
    display: flex;
    flex-wrap: wrap;
    gap: var(--gap-xs);
  }

  .summary-genre-pill {
    background: color-mix(in srgb, var(--trakttime-accent) 15%, transparent);
    color: var(--trakttime-accent);
    border-radius: var(--border-radius-m);
    padding: var(--gap-xxs) var(--gap-s);
    font-size: 0.6875rem;
    font-weight: 600;
    letter-spacing: 0.04em;
    text-transform: uppercase;
  }
</style>
