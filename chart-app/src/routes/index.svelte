<script>
    /**
     * This Svelte app is created to demo how Aklivity Zilla can be used to quickly
     * [1] serve a webb app on an http server
     * [2] update data across the app in realtime using Apache Kafka, HTTP, and SSE
     *
     * Rather than worrying about setting up a Flask Server, integrating it with Kafka,
     * then figuring out how to integrate SSEs to produce an app with live updates,
     * all you have to do is string together a series of enter and exits across different Zilla bindings.
     *
     * Look at zilla.Json file in this repository for details.
     *
     * With Zilla, piping data streams is almost like working on a Unix Command Line.
     * Focus on what you want to do at point A and point B, not how to get from A to B.
     */
    import { LayerCake, Svg } from "layercake";
    import { onMount } from "svelte";
    import { writable } from "svelte/store";

    /**
     * Subscription function: Copied from:https://marmelab.com/blog/2020/10/02/build-a-chat-application-using-sveltejs-and-sse.html
     * Modifications added.
     * 
     * Note because of Zilla we can take advantage of SSEs easily to retrieve data in real time. 
     * Convientely, we can still use a post HTTP method to send data into the stream too, which we'll
     * demo below.
     */
    const createChannelStore = (sourceUrl) => {
        const { subscribe, set, update } = writable([]);

        const eventSource = new EventSource(`${sourceUrl}`);

        /**
         * In here we'll add the necessary processing to convert
         * user input data into the proper JSON form, this will be
         * fed into a chart component
         */
        eventSource.onmessage = (e) => {
            update((data) => data.concat(JSON.parse(e.data)));
           
        };

        return {
            subscribe,
            reset: () => set([]),
            close: eventSource.close,
        };
    };

    let data = [];
    let sourceURl = "http://localhost:8080/tasks";
    onMount(() => {
        const store = createChannelStore(sourceURl);
        store.subscribe((incomingData) => {
            data = incomingData;
        });


        return store.close;
    });

    /**
     * Need to add the necessary method so that slider info is logged 
     * into data stream on button click -- will do this with a post method
    */

    $: console.log(data);

</script>

<main>
    <h1>Coffee Appreciation Poll</h1>
    <img
        class="coffee"
        src="https://openmoji.org/data/color/svg/E151.svg"
        alt="coffee<3"
    />
    <h3>What is this page for besides displaying open source emoji?</h3>
    <p>
        Well, not much, except we are trying to answer an important question. To
        what degree do people appreciate coffee for its flavor versus its
        caffeination.Click on the "Take the poll!" button below to add your
        feedback on why you appreciate coffee.<br />
        <br />
        You certainly could fill it out even if you don't like coffee, but then you
        would just be submitting 0 in all the requested fields. That would seem not
        entirely useful...anyways take the poll! Keep this tab open to view your
        data being added live!
    </p>
</main>

<style>
    main {
        align-items: center;
        font-family: Georgia, "Times New Roman", Times, serif;
    }

    p {
        padding: 2%;
    }

    h1 {
        text-align: center;
        size: 2em;
    }
    .coffee {
        height: 3em;
        display: block;
        margin-left: auto;
        margin-right: auto;
        width: 50%;
    }
</style>
