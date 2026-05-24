<script lang="ts">
    import AccountsList from "./Accounts/AccountsList.svelte";
    import AccountTransactionsList from "./Accounts/AccountTransactionsList.svelte";
    import { accounts } from '../store/stores';
    import { formatMoney } from "../utils/misc";
</script>

<div class="main">
    <section>
        <AccountsList />
        <AccountTransactionsList />
    </section>
    <h5><i class="fa-solid fa-wallet fa-fw"></i>{formatMoney($accounts[0].cash)}</h5>
</div>

<style>
    .main {
        overflow: hidden;
        width: 76%;
        height: 84%;
        bottom: 8%;
        left: 12%;
        padding: 1.4rem;
        position: absolute;
        background-color: rgba(14, 15, 20, 0.84);
        background: linear-gradient(145deg, rgba(36, 38, 44, 0.72), rgba(18, 19, 25, 0.82));
        border-radius: 16px;
        border: 1px solid rgba(255, 255, 255, 0.2);
        box-shadow: 0 10px 50px rgba(0, 0, 0, 0.48), inset 0 1px 0 rgba(255, 255, 255, 0.08);
        backdrop-filter: blur(14px);
        background-size: cover;
        background-position: center;
        opacity: 1;
    }

    section {
        display: flex;
        gap: 1.4rem;
        height: calc(100% - 2rem);
    }
    h5 {
        font-size: 1.4rem;
        display: flex;
        align-items: center;
        gap: 0.5rem;
        color: #f1d9d9;
    }
</style>
