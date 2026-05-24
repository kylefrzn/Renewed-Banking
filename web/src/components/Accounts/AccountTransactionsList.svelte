<script lang="ts">
    import { accounts, activeAccount, translations, atm, notify} from "../../store/stores";
    import AccountTransactionItem from "./AccountTransactionItem.svelte";
    import { convertToCSV } from "../../utils/convertToCSV";
    import { setClipboard } from "../../utils/setClipboad";
    let transSearch = '';
    $: account = $accounts.find((accountItem: any) => $activeAccount === accountItem.id);

    function handleClickExportData() {
        if (account == null) console.log("No account selected");
        if (account.transactions.length === 0) {
            notify.set("No transactions to export!");
            setTimeout(() => {
                notify.set("");
            }, 3500);
            return;
        }
        const csv = convertToCSV(account.transactions);
        setClipboard(csv);
        notify.set("Data copied to clipboard!");
        setTimeout(() => {
            notify.set("");
        }, 3500);
    }
    let isAtm: boolean = false;
    atm.subscribe((usingAtm: boolean) => {
        isAtm = usingAtm;
    });
</script>

<section class="transactions-container">
    <h3 class="heading">
        <span>{$translations.transactions}</span>

        <div class="brand">
            <img src="./img/bank.png" alt="bang icon" />
            <div class="brand-text">
                <span class="brand-title">ShadowPay</span>
                <small>Powered by Renewed-Banking</small>
            </div>
        </div>
    </h3>

    <input type="text" class="transactions-search" placeholder={$translations.trans_search} bind:value={transSearch}>
    <section class="scroller">
        {#if account}
            {#if account.transactions.filter(item => item.message.toLowerCase().includes(transSearch.toLowerCase()) || item.trans_id.toLowerCase().includes(transSearch.toLowerCase()) || item.receiver.toLowerCase().includes(transSearch.toLowerCase())).length > 0}
                {#each account.transactions.filter(item => item.message.toLowerCase().includes(transSearch.toLowerCase()) || item.trans_id.toLowerCase().includes(transSearch.toLowerCase()) || item.receiver.toLowerCase().includes(transSearch.toLowerCase())) as transaction (transaction.trans_id)}
                    <AccountTransactionItem {transaction}/>
                {/each}
            {:else}
                <h3 style="text-align: left; color: #F3F4F5; margin-top: 1rem;">{$translations.trans_not_found}</h3>
            {/if}
        {:else}
            {$translations.select_account}
        {/if}
    </section>
    {#if !isAtm}
        <div class="export-data">
            <button class="btn btn-green" style="display: flex; align-items: center; justify-content: center; gap: 1rem" on:click|preventDefault={handleClickExportData}><i class="fa-solid fa-file-export fa-fw" />
                {$translations.export_data}
            </button>
        </div>
    {/if}
</section>

<style>
    .transactions-container {
        flex: 1 1 72%;
        transform: translateY(-0.15rem);
        padding: 0.5rem 0.25rem;
    }

    .heading {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 0.8rem;
    }

    .brand {
        display: flex;
        align-items: center;
        gap: 0.8rem;
    }

    .heading img {
        width: 3rem;
    }

    .brand-text {
        display: flex;
        flex-direction: column;
        line-height: 1.1;
    }

    .brand-title {
        color: #ffd0d0;
        font-weight: 700;
        letter-spacing: 0.04rem;
        font-size: 2rem;
    }

    .brand-text small {
        color: #b6bacf;
        font-size: 0.95rem;
    }

    .heading :global(span:first-child) {
        font-size: 3rem;
        font-weight: 700;
    }

    .transactions-search {
        width: 100%;
        border-radius: 10px;
        border: 1px solid rgba(255, 255, 255, 0.18);
        padding: 1.4rem;
        margin-bottom: 1rem;
        background-color: rgba(60, 64, 74, 0.45);
        color: #fff;
    }

    .scroller {
        height: 85%;
        padding-right: 0.4rem;
    }

    .export-data {
        margin-top: 1rem;
        display: flex;
        justify-content: flex-end;
    }
    /* ------------------------- */
</style>
