<script lang="ts">
  import SentimentIcon from '$lib/components/icons/SentimentIcon.svelte';

  type SentimentAspectsProps = {
    pros: string[];
    cons: string[];
  };

  const { pros, cons }: SentimentAspectsProps = $props();

  const mappedSentiments = $derived([
    {
      aspects: pros,
      sentiment: 'good' as const,
      sentimentColor: 'var(--color-sentiment-good)',
    },
    {
      aspects: cons,
      sentiment: 'bad' as const,
      sentimentColor: 'var(--color-sentiment-bad)',
    },
  ]);
</script>

<div class="sentiment-aspects">
  {#each mappedSentiments as { sentiment, aspects, sentimentColor } (sentiment)}
    {#if aspects.length > 0}
      <div class="sentiment-aspect-group" style="--sentiment-color: {sentimentColor}">
        <SentimentIcon {sentiment} />
        <ul>
          {#each aspects as aspect, i (`${aspect}-${i}`)}
            <li>{aspect}</li>
          {/each}
        </ul>
      </div>
    {/if}
  {/each}
</div>

<style lang="scss">
  .sentiment-aspects {
    display: flex;
    flex-direction: column;
    gap: var(--gap-m);
  }

  .sentiment-aspect-group {
    display: flex;
    gap: var(--gap-s);
    color: var(--color-text-primary);

    :global(svg) {
      flex-shrink: 0;
      width: var(--trakttime-icon-md);
      height: var(--trakttime-icon-md);
      color: var(--sentiment-color);
    }
  }

  ul {
    display: flex;
    flex-direction: column;
    gap: var(--gap-xxs);

    margin: 0;
    padding: 0;
    padding-inline-start: var(--ni-12);

    font-size: 0.875rem;
    color: var(--color-text-primary);

    &::marker {
      color: var(--color-text-secondary);
    }
  }
</style>
