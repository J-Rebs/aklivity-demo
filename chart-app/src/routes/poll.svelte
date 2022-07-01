<script>
    import RangeSlider from "svelte-range-slider-pips";

    let caffeination = [0];
    let cVal = 0;

    let taste = [0];
    let tVal = 0;

    /**
     * With Zilla, we can mix and match protocols with.
     * We use SSE to help keep our poll data up to date, 
     * but new data can be registered with a simple HTTP Post. 
     * Info on how to post with fetch:
     * https://stackoverflow.com/questions/29775797/fetch-post-json-data
     */
    async function register() {
        let v = {};
        v["taste"] = tVal;
        v["caffeination"] = cVal;
        let vStr = JSON.stringify(v);
        console.log(v);
        console.log(vStr);

        const rawResponse = await fetch("http://localhost:8080/tasks", {
            method: "POST",
            headers: {
                "Idempotency-Key": "9417E83E-313E-468E-AC7C-1BCE0BAF9712",
                "Content-Type": "application/json",
            },
            body: JSON.stringify({ name: vStr }),
        });
        const content = await rawResponse.json();

        console.log(content);
    }
</script>

<main>
    <h1>Coffee Appreciation Poll!</h1>
    <h2>Caffeination</h2>
    <RangeSlider
        values={caffeination}
        pips={true}
        all="label"
        on:change={(e) => {
            cVal = e.detail.value;
        }}
    />
    <h2>Taste</h2>
    <RangeSlider
        values={taste}
        pips={true}
        all="label"
        on:change={(e) => {
            tVal = e.detail.value;
        }}
    />

    <h2>Your current views are...</h2>
    <p>
        caffeination:{cVal}
    </p>
    <p>
        taste:{tVal}
    </p>
    <button on:click={register}>Register your views</button>

    <h5>
        <a href="/">Go to homepage</a>
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
