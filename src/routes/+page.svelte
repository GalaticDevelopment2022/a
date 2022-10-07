<script>
    import '../styles/global.css';
    import { fly } from 'svelte/transition';
    import { fade } from 'svelte/transition';
    
    import { name, month, year, cvc } from '../store.js';

    let cardName = '';
    let cardNumber = '';
    let cardMonth = '';
    let cardYear = '';
    let cardCvv = '';
    let cardNumberMask = '0000 0000 0000 0000';
    let refs = {};

    let form = { submitted: false };

    function handleSubmit() {
        form.submitted = true;
    }

    $: {
        for (let i = 0; i < cardNumber.length; i++) {
            if (cardNumberMask[i] == ' ' && cardNumber[i] !== ' ') cardNumber = cardNumber.substr(0, i) + ' ' + cardNumber.substr(i, cardNumber.length - i)
        }

        if (cardNumber.substr('-1') == ' ') cardNumber = cardNumber.substr(0, cardNumber.length - 1)
            cardNumber = cardNumber.substr(0, cardNumberMask.length).replace(/[^0-9 ]/g, '')
    }
</script>

<main class="wrapper">
    <div class="card-wrapper">
        <div class="card-group">
            <div class="card card-front">
                <img src="/card-logo.svg" alt="" class="card-logo">

                <div class="card-number">
                    {#each cardNumberMask as n, i (i)}
                        <div class="card-number-item" class:active={n.trim() === ''}>
                            {#if cardNumber.length > i}
                                <span in:fly={{y:-10}} out:fly={{y:10}}>
                                    {cardNumber[i]}
                                </span>
                            {:else}
                                <span in:fly={{y:-10}} out:fly={{y:10}}>{n}</span>
                            {/if}
                        </div>
                    {/each}
                </div>

                <div class="card-name">
                    {#if cardName.length}
                        <div class="card-name-item">
                            {#each cardName.replace(/\s\s+/g, ' ') as n, i (i + 1)}
                                {#if i == i}
                                    <span in:fly={{y:-6}}>{n}</span>
                                {/if}
                            {/each}
                        </div>
                    {:else}
                        <div in:fly={{y:-6}} class="card-item__name placeholder">Jane Appleseed</div>
                    {/if}
                </div>

                <div class="card-date">{$month || '00'}/{$year || '00'}</div>
            </div>

            <div class="card card-back">
                <div class="card-cvc">{$cvc || '000'}</div>
            </div>
        </div>
    </div>

    <div class="form-wrapper">
        {#if !form.submitted}
            <form action="" class="form" on:submit|preventDefault={handleSubmit}>
                <div class="form-group">
                    <label for="name" class="form-label">Cardholder Name</label>
                    <input type="text" id="cardName" class="form-control" placeholder="e.g. Jane Appleseed" bind:value={cardName} autocomplete="off" maxlength="30">
                </div>

                <div class="form-group">
                    <label for="number" class="form-label">Card Number</label>
                    <input type="text" id="cardNumber" class="form-control" placeholder="e.g. 1234 5678 9123 0000" bind:value={cardNumber} autocomplete="off">
                </div>

                <div class="form-date-cvc">
                    <div class="form-group">
                        <label for="date" class="form-label">Exp. Date (MM/YY)</label>
                        
                        <div class="form-date-group">
                            <input type="text" placeholder="MM" class="form-control form-month" bind:value={$month} maxlength="2">
                            <input type="text" placeholder="YY" class="form-control form-year" bind:value={$year} maxlength="2">
                        </div>
                    </div>

                    <div class="form-group">
                        <label for="cvc" class="form-label">CVC</label>
                        <input type="text" placeholder="e.g. 123" class="form-control" bind:value={$cvc} maxlength="3">
                    </div>
                </div>

                <button class="btn">Confirm</button>
            </form>
        {:else}
            <div class="form">
                <h1>Thank you!</h1>
            </div>
        {/if}
    </div>

    <div class="hidden">
        Thank you!
        We've added your card details
        Continue
    </div>

    <div class="attribution hidden">
        Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
        Coded by <a href="#">Byron</a>.
    </div>
</main>