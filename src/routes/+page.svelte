<script>
    import '../styles/global.css';
    import { fade } from 'svelte/transition';
    
    import { name, number, month, year, cvc } from '../store.js';

    let form = { submitted: false };

    function handleSubmit() {
        form.submitted = true;
    }
</script>

<main class="wrapper">
    <div class="card-wrapper">
        <div class="card-group">
            <div class="card card-front">
                <img src="/card-logo.svg" alt="" class="card-logo">

                <div class="card-number">
                    {$number || '0000000000000000'}
                </div>

                <div class="card-name">{$name || 'Jane'}</div>

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
                    <input type="text" placeholder="e.g. Jane Appleseed" class="form-control" bind:value={$name} maxlength="30">
                </div>

                <div class="form-group">
                    <label for="number" class="form-label">Card Number</label>
                    <input type="text" placeholder="e.g. 1234 5678 9123 0000" class="form-control" bind:value={$number} maxlength="16">
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