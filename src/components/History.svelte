<script>
	import {_} from "../services/i18n"

	import { onMount } from 'svelte'

	import { SPINNER_ICON } from '../lib/icons'

	import { history } from '../lib/stores'
	import { formatPnl, showModal, formatCurrency, formatToDisplay } from '../lib/utils'
	import { getUserHistory } from '../lib/graph'

	let loading;
	onMount(async () => {
		if (!$history.length) {
			loading = true;
		}
		await getUserHistory();
		loading = false;
	});

</script>

<style>

	.history {
		position: absolute;
		top: 0;
		left: 0;
		right: 0;
		bottom: 0;
	}

	.columns {
		display: flex;
		align-items: center;
		height: 40px;
		padding: 0 var(--base-padding);
		color: var(--sonic-silver);
		font-size: 90%;
		border-bottom: 1px solid var(--jet-dim);
	}

	.trades-list {
		overflow-y: scroll;
		position: absolute;
		top: 40px;
		bottom: 0;
		left: 0;
		right: 0;
	}

	.trade {
		display: flex;
		align-items: center;
		height: 48px;
		cursor: pointer;
		padding: 0 var(--base-padding);
		border-bottom: 1px solid var(--jet-dim);
	}
	.trade:hover {
		background-color: var(--jet-dim);
	}

	.column {

	}

	.column-product {
		width: 20%;
	}
	.column-price {
		width: 20%;
	}
	.column-size {
		width: 20%;
	}
	.column-leverage {
		width: 20%;
	}
	.column-pnl {
		width: 20%;
	}

	.empty {
		padding: var(--base-padding);
		color: var(--onyx);
		text-align: center;
	}

	.loading-icon :global(svg) {
		height: 24px;
	}

	@media (max-width: 600px) {

		.column-leverage {
			display: none;
		}

		.column-product {
			width: 30%;
		}
		.column-price {
			width: 25%;
		}
		.column-size {
			width: 20%;
		}
		.column-pnl {
			width: 25%;
		}

	}

</style>

<div class='history'>

	<div class='columns'>
		<div class='column column-product'>{$_('p.prod')}</div>
		<div class='column column-price'>{$_('p.price')}</div>
		<div class='column column-size'>{$_('p.size')}</div> 
		<div class='column column-leverage'>{$_('p.leverage')}</div>
		<div class='column column-pnl'>{$_('p.PL2')}</div>

	</div>

	<div class='trades-list no-scrollbar'>

		{#if loading}
			<div class='empty'>
				<div class='loading-icon'>{@html SPINNER_ICON}</div>
			</div>
		{:else}
			{#if $history.length == 0}
				<div class='empty'>{$_('p.noTrades')}</div>
			{:else}

				{#each $history as trade}

					<div class='trade' on:click={() => {showModal('TradeDetails', trade)}} data-intercept="true">

						<div class='column column-product'>{#if trade.isLong}<span class='pos'>↑</span>{:else}<span class='neg'>↓</span>{/if} {trade.product}</div>
						<div class='column column-price'>{formatToDisplay(trade.price)}</div>
						<div class='column column-size'>{formatToDisplay(trade.size)} {formatCurrency(trade.currencyLabel)}</div>
						<div class='column column-leverage'>{formatToDisplay(trade.leverage)}×</div>
						<div class={`column column-pnl ${trade.pnl * 1 < 0 ? 'neg' : 'pos'}`}>{formatPnl(trade.pnl)}</div>

					</div>

				{/each}

			{/if}
		{/if}

	</div>

</div>