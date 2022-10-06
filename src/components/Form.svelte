<script>
    import { name, number, month, year, cvc } from '../store.js';
</script>

<form action="" class="form">
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