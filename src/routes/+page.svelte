<script>
    import '../styles/global.css';
    import { fly, fade } from 'svelte/transition';
    
    import { month, year, cvc } from '../store.js';
    
    let cardName = '';
    let cardNumber = '';

    let cardNumberMask = '0000 0000 0000 0000';
    let refs = {};

    let form = { submitted: false };
    let errors = {};
    const regex = /^[0-9]*$/;

    function handleSubmit(e) {
        validateName();
        validateNumber();
        validateDate();
        validateCvc();

        if (validateName() && validateNumber() && validateDate() && validateCvc()) {
            form.submitted = true;
        }
    }

    function validateName() {
        const name = document.getElementById('cardName');

        if (name.value.trim() === '') {
            errors.name = "Can't be blank";
        } else {
            errors.name = '';
            return true;
        }
    }

    function validateNumber() {
        const number = document.getElementById('cardNumber');

        const regex = /^[0-9]*$/;
        const n = number.value.trim().replace(/\s/g, '');

        if (n === '') {
            errors.number = "Can't be blank";
        } else if (!regex.test(n)) {
            errors.number = 'Wrong format, numbers only';
        } else if (n.length !== 16) {
            errors.number = 'Invalid card number';
        } else {
            errors.number = '';
            return true;
        }
    }

    function validateDate() {
        const month = document.getElementById('cardMonth');
        const year = document.getElementById('cardYear');

        if (month.value.trim() === '' || year.value.trim() === '') {
            errors.date = "Can't be blank";
        } else if (month.value.trim().length < 2 || year.value.trim().length < 2) {
            errors.date = 'Cant be less than two';
        } else if (!regex.test(month.value.trim()) || !regex.test(year.value.trim())) {
            errors.date = 'Numbers only';
        } else {
            errors.date = '';
            return true;
        }
    }

    function validateCvc() {
        const cvc = document.getElementById('cardCvc');

        if (cvc.value.trim() === '') {
            errors.cvc = "Can't be blank";
        } else if (cvc.value.trim().length < 3) {
            errors.cvc = 'Cant be less than three';
        } else if (!regex.test(cvc.value.trim())) {
            errors.cvc = 'Numbers only';
        } else {
            errors.cvc = '';
            return true;
        }
    }

    function reset() {
        cardName = '';
        cardNumber = '';
        $month = '';
        $year = '';
        $cvc = '';

        form.submitted = false;
    }

    $: {
        for (let i = 0; i < cardNumber.length; i++) {
            if (cardNumberMask[i] == ' ' && cardNumber[i] !== ' ') {
                cardNumber = cardNumber.substr(0, i) + ' ' + cardNumber.substr(i, cardNumber.length - i)
            }
        }
    }
</script>

<svelte:head>
    <title>Frontend Mentor | Interactive card details form</title>
    <meta name="description" content="Interactive card details form">
</svelte:head>

<main class="wrapper">
    <h1 class="sr-only">Interactive card details form</h1>

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
                <div class="form-group" class:invalid-field={errors.name}>
                    <label for="cardName" class="form-label">Cardholder Name</label>
                    <input type="text" id="cardName" class="form-control" placeholder="e.g. Jane Appleseed" bind:value={cardName} autocomplete="off" maxlength="30">
                    {#if errors.name}
                        <span class="invalid-feedback">{errors.name}</span>
                    {/if}
                </div>

                <div class="form-group" class:invalid-field={errors.number}>
                    <label for="cardNumber" class="form-label">Card Number</label>
                    <input type="text" id="cardNumber" class="form-control form-card-number" placeholder="e.g. 1234 5678 9123 0000" bind:value={cardNumber} autocomplete="off" maxlength="19">
                    {#if errors.number}
                        <span class="invalid-feedback">{errors.number}</span>
                    {/if}
                </div>

                <div class="form-date-cvc">
                    <div class="form-group" class:invalid-field={errors.date}>
                        <label for="cardMonth" class="form-label">Exp. Date (MM/YY)</label>
                        
                        <div class="form-date-group">
                            <input type="text" id="cardMonth" placeholder="MM" class="form-control form-month" bind:value={$month} maxlength="2">
                            <input type="text" id="cardYear" placeholder="YY" class="form-control form-year" bind:value={$year} maxlength="2">
                        </div>

                        {#if errors.date}
                            <span class="invalid-feedback">{errors.date}</span>
                        {/if}
                    </div>

                    <div class="form-group" class:invalid-field={errors.cvc}>
                        <label for="cardCvc" class="form-label">CVC</label>
                        <input type="text" id="cardCvc" placeholder="e.g. 123" class="form-control" bind:value={$cvc} maxlength="3">

                        {#if errors.cvc}
                            <span class="invalid-feedback">{errors.cvc}</span>
                        {/if}
                    </div>
                </div>

                <button class="btn">Confirm</button>
            </form>
        {:else}
            <div class="form thank-you">
                <img src="./icon-complete.svg" alt="">
                
                <h2>Thank you!</h2>

                <p>We've added your card details</p>

                <button class="btn" type="button" on:click={() => reset()}>Continue</button>
            </div>
        {/if}
    </div>

    <div class="attribution hidden">
        Challenge by <a href="https://www.frontendmentor.io?ref=challenge" target="_blank">Frontend Mentor</a>. 
        Coded by <a href="https://twitter.com/bwalkermills">Byron</a>.
    </div>
</main>