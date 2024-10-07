<script lang="ts">
  import Collapse from './layout/Collapse.svelte';
  import Sender from './sender/Sender.svelte';
  import Receiver from './receiver/Receiver.svelte';
 
  export let sender;
 export let receiver;
 export let content;
 export let sendMode;
 export let isConnecting;
 export let showNewFile;
 export let dataChannel;
 export let rsaPub;
 export let rsa;
 export let handleInput;
 export let sendOptions;

</script>


<style>
    html,
    body {
      margin: 0;
      height: 100%;
      overflow: hidden; /* Prevent scrolling */
    }
  
    textarea {
      width: 100%;
      height: 100%;
      border: none;
      padding: 10px;
      box-sizing: border-box;
      font-size: 16px;
    }
  
    textarea:focus {
      outline: none;
    }
  </style>
  
<Collapse title={isConnecting ? "Connected: Explore Available Features" : "Connect to Access Features" } isOpen={isConnecting}>

    <Collapse title="Transfer Files" isOpen={isConnecting}>
      <div class="flex w-full mb-4 mt-2">
        <button
          class="btn {sendMode ? 'btn-primary' : 'btn-ghost'} flex-grow border-black border-dotted"
          on:click={() => {
            sendMode = true;
          }}
        >
          <span class="btm-nav-label">Send</span>
        </button>
        <div class="indicator flex-grow">
          <span
            class="indicator-item badge badge-accent animate-bounce {showNewFile ? 'block' : 'hidden'}"
            >New files</span
          >
          <button
            class="btn {sendMode ? 'btn-ghost' : 'btn-primary'} w-full border-black border-dotted"
            on:click={() => {
              showNewFile = false;
              sendMode = false;
            }}
          >
            <span class="btm-nav-label">Receive</span>
          </button>
        </div>
      </div>
      <div hidden={!sendMode}>
        <Sender
          bind:this={sender}
          {dataChannel}
          {rsaPub}
          isEncrypt={sendOptions.isEncrypt}
          chunkSize={sendOptions.chunkSize}
        />
      </div>
      <div hidden={sendMode}>
        <Receiver bind:this={receiver} {dataChannel} isEncrypt={sendOptions.isEncrypt} {rsa} />
      </div>
    </Collapse>

    <Collapse title="Live Clipboard" isOpen={isConnecting}>
      <div class="flex w-full mb-4 mt-2">
        <div class="indicator flex-grow">
          <textarea bind:value={content} on:input={handleInput} placeholder="Type here..." />
        </div>
      </div>
    </Collapse>
 </Collapse>
