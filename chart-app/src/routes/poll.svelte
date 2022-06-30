<script>
    import RangeSlider from "svelte-range-slider-pips";
    
    let caffeination = [0];
    let cVal = 0;

    let taste = [0];
    let tVal = 0; 
    
    console.log('updates')

    function handleClick(){

    }

</script>

<main>

    <h1>Coffee Appreciation Poll!</h1>
    <h2>Caffeination</h2>
    <RangeSlider
    values = {caffeination}
    pips = {true}
    all='label'
    on:change={(e) => {cVal=e.detail.value}}
    />
    <h2>Taste</h2>
    <RangeSlider values = {taste}
    pips = {true}
    all='label'
    on:change={(e) => {tVal=e.detail.value}}
    />

    <h2>Your current views are...</h2>
    <p>
        caffeination:{cVal}
    </p>
    <p>
        taste:{tVal}
    </p>
    <button>Register my views</button>

    <h5>
        <a href="/poll">Go home (to the homepage that is)</a>
    </h5>
</main>

<style>
    main {
        align-items: center;
        font-family: Georgia, "Times New Roman", Times, serif;
    }

    h1 {
        text-align: center;
        size: 2em;
    }

</style>