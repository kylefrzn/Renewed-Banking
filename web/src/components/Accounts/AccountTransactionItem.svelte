<script lang="ts">
    export let transaction: any;
    import { formatMoney } from "../../utils/misc";
    import { translations } from "../../store/stores";
    function getTimeElapsed(seconds: number): string {
        let retData: string;
        const timestamp = Math.floor(Date.now() / 1000)-seconds;
        const minutes = Math.floor(timestamp / 60);
        const hours = Math.floor(minutes / 60);
        const days = Math.floor(hours / 24);
        const weeks = Math.floor(days / 7);

        if (weeks !== 0 && weeks > 1) {
            retData = $translations.weeks.replace("%s", weeks);
        } else if (weeks !== 0 && weeks === 1) {
            retData = $translations.aweek;
        } else if (days !== 0 && days > 1) {
            retData = $translations.days.replace("%s", days);
        } else if (days !== 0 && days === 1) {
            retData = $translations.aday;
        } else if (hours !== 0 && hours > 1) {
            retData = $translations.hours.replace("%s", hours);
        } else if (hours !== 0 && hours === 1) {
            retData = $translations.ahour;
        } else if (minutes !== 0 && minutes > 1) {
            retData = $translations.mins.replace("%s", minutes);
        } else if (minutes !== 0 && minutes === 1) {
            retData = $translations.amin;
        } else {
            retData = $translations.secs;
        }
        return retData;
    }
</script>

<section class="transaction">
    <h5>
        <span class="title-container" class:withdrawTitle={transaction.trans_type === "withdraw"}>
            {transaction.title}
            <p>[{transaction.trans_type.toUpperCase()}]</p>
        </span>
        <span class="trans_id" class:withdrawId={transaction.trans_type === "withdraw"}>{transaction.trans_id}</span>
    </h5>
    <h4>
        <div style="display: flex; flex-direction: column; justify-content: flex-start; align-items: flex-start;">
            <span class:withdraw={transaction.trans_type === "withdraw"}>
                <i class="fa-solid fa-money-bill"></i>
                {formatMoney(transaction.amount)}
            </span>
        </div>
        <span> {transaction.receiver} </span>
        <span>{getTimeElapsed(transaction.time)} <br /> {transaction.issuer}</span>
    </h4>

    <h6>
        {$translations.message} <br />
        {transaction.message}
    </h6>
</section>

<style>
    .transaction {
        background-color: var(--clr-primary-light);
        padding: 1.6rem;
        border-radius: 10px;
        font-size: 1.4rem;
        font-weight: 300;
        border: 1px solid rgba(255, 255, 255, 0.14);
        box-shadow: 0 8px 24px rgba(0, 0, 0, 0.32);
    }

    .transaction:not(:last-child) {
        margin-bottom: 1.5rem;
    }
    .title-container {
        display: flex;
        align-items: center;
        gap: 0.5rem;
    }

    .title-container > p {
        background-color: var(--clr-green);
        color: #092a24;
        padding: 0.5rem 1rem;
        border-radius: 6px;
    }
    .title-container.withdrawTitle > p {
        background-color: var(--clr-orange);
        color: #2f0909;
        padding: 0.5rem 1rem;
        border-radius: 6px;
    }
    .trans_id {
        color: #ced3eb;
        background-color: var(--clr-green);
        color: #092a24;
        padding: 0.3rem 0.8rem;
        border-radius: 6px;
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .trans_id.withdrawId {
        background-color: var(--clr-orange);
        color: #2f0909;
        padding: 0.5rem 1rem;
        border-radius: 6px;
    }

    .transaction h5 {
        display: flex;
        justify-content: space-between;
        padding-bottom: 0.85rem;
        margin-bottom: 1rem;
        border-bottom: 1px solid rgba(255, 255, 255, 0.2);
    }

    .transaction h4 {
        display: flex;
        justify-content: space-between;
        font-size: 1.25rem;
        margin-bottom: 1.4rem;
    }
    .transaction h4 span:first-child {
        font-size: 1.4rem;
        color: var(--clr-green);
    }
    .transaction h4 span.withdraw {
        color: var(--clr-orange);
    }
    .transaction h4 span:nth-child(2) {
        margin-top: 0.5rem;
        margin-left: 0.2rem;
        color: #ced3eb;
    }
    .transaction h6 {
        color: #B6BACF;
        margin: 0.7rem 0 0.1rem;
        line-height: 1.3;
    }
    .transaction h6 span {
        margin-top: 0.5rem;
    }
</style>
