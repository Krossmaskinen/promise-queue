<script>
  import PromiseCard from "./lib/PromiseCard.svelte";

  const maxNoOfRequests = 3;
  let queuedRequests = [];
  let activeRequests = [];
  let nextRequestId = 0;

  function generateRequest() {
    const id = nextRequestId;
    
    ++nextRequestId;

    queuedRequests = [...queuedRequests, id];

    return id;
  }

  function startNextRequest() {
    if (activeRequests.length < maxNoOfRequests && queuedRequests.length > 0) {
      const id = queuedRequests.shift();
      const request = createRequest(id);

      queuedRequests = [...queuedRequests];

      request.promise.then((id) => {
        console.log(`Cleaning up ${id}`);
        cleanupRequest(id);
        startNextRequest();
      })

      activeRequests = [...activeRequests, request];
    }
  }

  function createRequest(id) {
    const delay = getDelay();
    const promise = new Promise((resolve) => {
      console.log("Request", id, "started");
      setTimeout(() => {
        console.log("Request", id, "finished");
        resolve(id);
      }, delay);
    });

    return {
      id,
      promise,
      delay,
    };
  }

  function queueNewRequest() {
    generateRequest();
    startNextRequest();
  }

  function cleanupRequest(id) {
    activeRequests = activeRequests.filter((r) => r.id !== id);
    console.log({activeRequests});
  }

  function getDelay() {
    const delay = Math.random() * 5000;
    
    console.log("Delay", delay);

    return delay;
  }
</script>

<main>
  <h1>Queued Requests</h1>

  <button on:click={queueNewRequest}>Add Request</button>

  <div class="request-wrapper">
    <div>
      <h2>Queued requests: {queuedRequests.length}</h2>
      <ul>
        {#each queuedRequests as request}
        <li>
          <PromiseCard id={request} active={false} />
        </li>
        {/each}
      </ul>
    </div>
    
    <div>
      <h2>Active requests: {activeRequests.length}</h2>
      <ul>
        {#each activeRequests as request}
        <li>
          <PromiseCard id={request.id} delay={request.delay} active={true} />
        </li>
        {/each}
      </ul>
    </div>
  </div>

</main>

<style>
  ul {
    list-style: none;
    margin-left: 0;
  }

  .request-wrapper {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    min-width: 640px;
    max-width: 50%;
  }
</style>