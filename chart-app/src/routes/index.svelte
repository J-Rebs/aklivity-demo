<script>
    /**
     * This Svelte app is created to demo how Aklivity Zilla can be used to quickly
     * update data across the app in realtime using Apache Kafka, HTTP, and SSE.
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
    let chartDataCount = 0;
    let chartData = [];
    let poll = null;
    let sourceURl = "http://localhost:8080/tasks";
    onMount(async () => {
        /*
         * Retreive data live
         */
        const store = createChannelStore(sourceURl);
        store.subscribe((incomingData) => {
            data = incomingData;
        });
        /*
         * Cache the poll page so it loads properly
         * Prototype: https://sustainablewww.org/principles/how-to-implement-routes-in-your-svelte-web-application-using-svelte-routing
         */
        try {
            poll = (await import("./poll.svelte")).default;
        } catch (e) {
            poll = 404;
        }
        return store.close;
    });

    /**
     * Need to add the necessary method so that slider info is logged
     * into data stream on button click -- will do this with a post method
     */

    let theme = "default";
    $: data.forEach((e) => {
        chartData.push(JSON.parse(e["name"]));
        console.log(chartData);
        chartDataCount += 1;
    });

    
    

    /**
     * The below relates to the layer cake scatter plot we are generating.
     * Source: https://layercake.graphics/example/ScatterWebgl
     */

    import { LayerCake, Svg, WebGL, Html } from "layercake";

    import ScatterWebGL from "./components/Scatter.webgl.svelte";
    import AxisX from "./components/AxisX.svelte";
    import AxisY from "./components/AxisY.svelte";
    import QuadTree from "./components/QuadTree.html.svelte";

    const xKey = 'taste';
    const yKey = 'caffeination';

    const r = 3;
    const padding = 6;
</script>

<main>
    <h1>Coffee Appreciation Poll</h1>
    <img
        class="coffee"
        src="https://openmoji.org/data/color/svg/E151.svg"
        alt="coffee<3"
    />
    <h3>What's this page for?</h3>
    <p>
        We are trying to answer an important question. To what degree do people
        appreciate coffee for its flavor versus its caffeination. Click on the
        "Take the poll!" button below to add your feedback on why you appreciate
        coffee.<br />
        <br />
        Keep this tab open to view your data being added live!
    </p>

    <h5>
        <a href="/poll">Take the poll!</a>
    </h5>
</main>
{#key chartDataCount}
<div class="chart-container">
    <LayerCake
      padding={{ top: 0, right: 5, bottom: 20, left: 25 }}
      x={xKey}
      y={yKey}
      xPadding={[padding, padding]}
      yPadding={[padding, padding]}
      data={chartData}
    >
      <Svg>
        <AxisX/>
        <AxisY
          ticks={5}
        />
      </Svg>
  
      <WebGL>
        <ScatterWebGL
          {r}
        />
      </WebGL>
  
      <Html>
        <QuadTree
          let:x
          let:y
          let:visible
        >
          <div
            class="circle"
            style="top:{y}px;left:{x}px;display: { visible ? 'block' : 'none' };"
          ></div>
        </QuadTree>
      </Html>
    </LayerCake>
  </div>
{/key}
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

  .chart-container {
    width: 100%;
    height: 500px;
  }
  .circle {
    position: absolute;
    border-radius: 50%;
    background-color: rgba(171,0, 214);
    transform: translate(-50%, -50%);
    pointer-events: none;
    width: 10px;
    height: 10px;
  }

</style>
