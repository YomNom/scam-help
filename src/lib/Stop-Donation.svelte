<script>
    export let showStop = false;
    export let onClose;
  </script>
  
  {#if showStop}
  <div class="popup-overlay" role="dialog" aria-modal="true">
    <div class="popup-content" role="document">
      <button class="close-button" on:click={onClose}>
        <svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
          <path d="M18 6L6 18" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          <path d="M6 6L18 18" stroke="black" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
        </svg>
      </button>
      <h3 style="color: white; background-color: red;">STOP! This could've been a scam</h3>
      <p>
        Always verify where you're donating or entering your card information. Ensure it’s a legitimate source. 
        If unsure, pause the transaction and double-check to avoid falling victim to scams.
      </p>
      <h4>IF SCAMMED, DO NOT BELIEVE IN SCAM RECOVERY SERVICES AND CONTACT YOUR BANK OR AUTHORITIES</h4>
    </div>
  </div>
{/if}
  
  <style>
    .popup-overlay {
      position: fixed;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      display: flex;
      justify-content: center;
      align-items: center;
      z-index: 1000;
      color: red;
    }
    .popup-content {
      position: relative;
      background: white;
      padding: 10px;
      border-radius: 10px;
      box-shadow: 0 2px 10px rgba(0, 0, 0, 0.1);
      text-align: center;
    }
    .close-button {
      position: absolute;
      top: -15px;
      right: -20px;
      background: none;
      border: none;
      font-size: 1.5em;
      color: navy;
      cursor: pointer;
    }
    button {
      margin: 10px;
      padding: 10px 20px;
      border: none;
      border-radius: 5px;
    }
  </style>