<script>
    import { name, number, month, year, cvc } from '../store.js';
</script>

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