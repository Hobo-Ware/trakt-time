<script lang="ts">
  import ChevronRightIcon from '$lib/components/icons/ChevronRightIcon.svelte';
  import * as m from '$lib/paraglide/messages.js';
  import type { SentimentAnalysis } from '$lib/requests/models/SentimentAnalysis.ts';
  import { mapToSentimentSummary } from './mapToSentimentSummary.ts';

  const {
    sentiment,
    onclick,
  }: {
    sentiment: SentimentAnalysis;
    onclick: () => void;
  } = $props();

  const { text, aspects } = $derived(
    mapToSentimentSummary({
      pros: sentiment.aspect.pros,
      cons: sentiment.aspect.cons,
    }),
  );
</script>

<button
  type="button"
  class="sentiment-summary-card"
  {onclick}
  aria-label={m.button_label_view_sentiment_analysis()}
>
  <div class="sentiment-summary-body">
    <span class="sentiment-summary-verdict">{text}</span>
    <ul>
      {#each aspects as aspect, i (`${aspect}-${i}`)}
        <li>{aspect}</li>
      {/each}
    </ul>
  </div>
  <ChevronRightIcon />
</button>

<style lang="scss">
  .sentiment-summary-card {
    width: 100%;
    display: flex;
    align-items: center;
    gap: var(--gap-s);

    padding: var(--gap-m);
    box-sizing: border-box;

    border: none;
    border-radius: var(--border-radius-m);
    background: var(--background-vip-drawer);

    color: var(--color-text-primary);
    text-align: left;
    cursor: pointer;
    -webkit-tap-highlight-color: transparent;

    > :global(svg) {
      flex-shrink: 0;
      width: var(--trakttime-icon-md);
      height: var(--trakttime-icon-md);
      color: var(--color-text-secondary);
    }
  }

  .sentiment-summary-body {
    flex: 1;
    display: flex;
    flex-direction: column;
    gap: var(--gap-xs);
    min-width: 0;
  }

  .sentiment-summary-verdict {
    font-weight: 700;
    text-transform: capitalize;
  }

  ul {
    display: flex;
    flex-direction: column;
    gap: var(--gap-xxs);

    margin: 0;
    padding: 0;
    padding-inline-start: var(--ni-18);

    font-size: 0.875rem;
    color: var(--color-text-primary);

    &::marker {
      color: var(--color-text-secondary);
    }
  }
</style>
