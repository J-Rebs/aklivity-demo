<script>
    import RangeSlider from "svelte-range-slider-pips";

</script>

<main>

    <h1>Coffee Appreciation Poll!</h1>
    <h2>Caffeination</h2>
    <RangeSlider
    values={[0]}
    range = {[0,100]}
    pips = {true}
    all='label'
    />
    <h2>Taste</h2>
    <RangeSlider
    values={[0]}
    range = {[0,100]}
    pips = {true}
    all='label'
    />
    
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