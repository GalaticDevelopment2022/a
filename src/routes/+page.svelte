<script>
    import '../styles/global.css';
    
    import Cards from '../components/Cards.svelte';
    import Form from '../components/Form.svelte';
</script>

<main class="wrapper">
    <div class="card-wrapper">
        <Cards />
    </div>

    <div class="form-wrapper">
        <Form />
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