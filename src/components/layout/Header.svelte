<script>
	import { _ } from '../../services/i18n';
	
	import Wallet from '../Wallet.svelte'

	import { currentPage } from '../../lib/stores' 
	
</script>

<style>

	header {
		padding: 0 var(--base-padding);
		height: var(--header-height);
		background-color: var(--eerie-black);
		display: flex;
		align-items: center;
		justify-content: space-between;
	}

	.left {
		display: flex;
		align-items: center;
	}

	.left a {
		color: var(--sonic-silver);
		margin-right: var(--base-padding);
	}

	.left a:hover {
		color: var(--green);
	}

	.left a.active {
		color: var(--green);
		font-weight: 700;
	}

	img {
		height: 11px;
		filter: grayscale(1);
		opacity: 0.5;
	}

	a:hover img, a.active img {
		filter: grayscale(0);
		opacity: 1;
	}

</style>


<header>

	<div class='left'>
		<a class:active={$currentPage == 'home' || !$currentPage} href='/'>
			<img src='/logos/CAP.svg' title='Cap logo' alt='Cap logo' />
		</a>
		<a class:active={$currentPage == 'trade'} href='#/trade'>{$_('page.header.trade')}</a>
		<a class:active={$currentPage == 'pool'} href='#/pool'>{$_('page.header.pool')}</a>
		<a class:active={$currentPage == 'stake'} href='#/stake'>{$_('page.header.stake')}</a>
		<a href='https://docs.cap.finance' target='_blank'>{$_('page.header.docs')}</a>
	</div>

	<div class='right'>
		<Wallet />
	</div>
	
</header>