<script>
  import { onMount } from 'svelte';
  import { slide } from 'svelte/transition';
  import SelectButton from './lib/SelectButton.svelte';
  import StopDonation from './lib/Stop-Donation.svelte';

  let checks = [false, false, false, false, false];
  let buttonTexts = [
    "URL Mispellings",
    "Security Certificate (SSL)",
    "Pressures You"
  ];
  let selectedClue = -1; // Initialize with -1 to indicate no selection
  let SSLshow = false;
  let showStop = false;
  let showView = false;
  let donationAmount;

  function toggleSelection(index) {
    if (selectedClue === 1) {
      showPopup();
    }
    if (selectedClue === index) {
      selectedClue = -1; // Deselect if the same button is clicked
    } else {
      selectedClue = index; // Select the new button
    }
  }

  function showPopup() {
    SSLshow = !SSLshow;
  }

  
  onMount(() => {
    document.addEventListener('click', handleClickOutside);
    return () => {
      document.removeEventListener('click', handleClickOutside);
    };
  });

  function handleClickOutside(event) {
    const popup = document.querySelector('.SSL-popup');
    const iconButton = document.querySelector('.icon-button');
    if (popup && !popup.contains(event.target) && !iconButton.contains(event.target)) {
      showPopup();
    }
  }

  function handleStopDonation() {
    showStop = !showStop;
    donationAmount = null;
  }

  function handleView(event) {
    showView = !showView;
  }

  function handleBack(event) {
    showView = !showView;
    selectedClue = -1;
  }
</script>

<main>
  <div class="main-column"> 
    
    <h1 style="color: #ee7d13;">CAN YOU SPOT A SCAM?</h1>
    <div class="info-row">
      <div class="intro">
        <h3 style="font-weight: bold; margin-bottom: 1px; margin-top: 5px;">
          Over the past year 2024, $12.5 billion was lost to online scams. 
        </h3>
        <p style="font-size: 18px; margin-bottom: 25px;"> 
          The internet is full of scams that can pressure you to act fast. 
          Make sure you are prepared to not be caught off-guard and recognize common characteristics of scams. 
        </p>
        <p style="font-size: 17px; font-weight: bold; margin-bottom: 1px; margin-top: 1px;"> For more info, read the article</p>
        <a href="https://news.harvard.edu/gazette/story/2024/09/youd-never-fall-for-an-online-scam-right/#:~:text=Last%20year%2C%20American%20consumers%20lost,tactics%20are%20becoming%20more%20sophisticated." target="_blank" rel="noopener noreferrer">
          <button style="margin-top: 1px;">The Harvard Gazette - You'd never fall for an online scam, right?</button>
        </a>
      </div> <!-- intro -->
      <div class="help-column">
        <h3 class="red-title" style="margin-bottom=0px;">TIP</h3>
        <p class="close-top" style="color: #0a203d;">
          VERIFY LINKS FROM BIG COMPANIES BY SEARCHING IT IN ANOTHER WINDOW
        </p>
      </div> 
      <div class="donate-bait">
        <h3 class="close-top">Help scam victims recover from devastating losses. Your support provides essential aid to those affected and helps them rebuild their lives.</h3>
        <button class="donate-amount" on:click={() => donationAmount = 1}>$1</button>
        <button class="donate-amount" on:click={() => donationAmount = 5}>$5</button>
        <button class="donate-amount" on:click={() => donationAmount = 10}>$10</button>
        <button class="donate-amount" on:click={() => donationAmount = 25}>$25</button><br>
        <div class="submit-donation"><br>
          <div class="row">
            <label for="donation">Donate</label>
            <input class="donation" type="number" bind:value={donationAmount} min="0" step="1" style="width: 50px; height: 23px;">
            <button style="border-radius: 5px" on:click={() => donationAmount = 0}>Reset</button><br>
          </div>
          <button class="donate-amount" on:click={handleStopDonation}>Continue to Payment</button>
          <StopDonation showStop={showStop} onClose={handleStopDonation} />
        </div>
      </div><!-- donate-bait -->
    </div> <!-- info-row -->
    <div class="content">
      <div class="column">
        {#if !showView}
          <div class="scam-checklist">
            <h3 class = "scam-title" style="background-color: #fca452; text-align: center;">
              Clues that a Website is a Scam
            </h3>
            <table class="scam-clues">
              {#each buttonTexts as text, index}
              <tr>
                <td>
                  <SelectButton 
                    buttonColor={'#fdfcc2'} 
                    isSelected={selectedClue === index} 
                    on:slide={handleView}
                    onClick={() => toggleSelection(index)}
                  >
                    {text}
                  </SelectButton>
                </td>
              </tr>
            {/each}
            </table>
          </div> <!-- scam-checklist -->
        {/if}
        
        {#if showView}
          <div class="scam-info"> 
            <button class="back-button" on:click={handleBack}>
              <img src="/backButton.png" alt="back-icon" class="go-back">
            </button>
            {#if selectedClue===0} <!-- URL Mispelling -->
              <div class="container">
                <div class="highlight-misspell"></div>
                <h3 class = "scam-title" style="background-color: #e74d00;">
                  {buttonTexts[selectedClue]}
                </h3>
                <ul style="margin-top:1px;">
                  <li>
                    Keep an eye out for even the smallest of misspellings, especially when 
                    they replace letters with numbers or symbols.
                    <ul>
                      <li>Example: app1e</li>
                    </ul>
                  </li>
                  <li>Missing 's' in 'https'</li>
                  <li>Look at the end of the website name for anything extra after the website name</li>
                  <li>
                    Suspiciously short links
                    <ul>
                      <li>bit.ly and tinyurl are infamous</li>
                    </ul>
                  </li>
                </ul>
              </div>
            {/if}

            {#if selectedClue===1} <!-- Security Certificate (SSL) -->
              {#if !SSLshow}
                {showPopup()}
              {/if}
              
              <h3 class = "scam-title" style="background-color: #e74d00; margin-bottom: 1px;">
                {buttonTexts[selectedClue]}
              </h3>
              <div class="highlight-ssl"></div>
              <ul>
                <li>SSL is method that websites can use to by walling it off</li>
                <li>Usually located to the left of the link as a symbol
                  <ul>
                    <li>Safe sites are commonly represented by a lock</li>
                    <li>To the right, you can see the usual symbol used for unsafe sites</li>
                  </ul>
                </li>
              </ul>
            {/if}
      
            {#if selectedClue===2} <!-- Pressures you -->
              <h3 class = "scam-title" style="background-color: #e74d00; margin-bottom: 1px;">
                {buttonTexts[selectedClue]}
              </h3>
              <ul>
                <li>
                  Repetition is a big weapon by online scams and even used by ads 
                  to wear you down and more suspectible to their tactics. 
                </li>
                <li>A big goal of their tactics is to make you panic and act fast, some 
                    common ones are:
                  <ul>
                    <li>Timers</li>
                    <li>Overload of Information</li>
                    <li>Loud Noises</li>
                    <li>Freezing your screen</li>
                  </ul>
                </li>
              </ul>
            {/if}
          </div> <!-- scam-checklist -->
        {/if} <!-- showView -->
        <div class="tip-column">
          <h3 class="frame-title">QUICK FIX</h3>
          <p class="close-top" style="text-align: left; color: orange;">COMPUTER FROZEN FROM SCAM?</p>
          <p class="close-top" style="margin-top: 0px; padding: 5px;">
            Immediately hold the power button until PC fully shuts down. 
          </p>
          <p class="close-top" style="margin-top: -10px; padding: 5px; color: #dc1111;">
            IGNORE ANY MESSAGES ON SCREEN ASKING YOU NOT TO!!!
          </p>
        </div> <!-- tip-column -->
      </div>
      <div class="image-container">
        <img src="/FakeSite.jpeg" alt="Fake Site" class="fake-site-image">

        <button class="icon-button" on:click={showPopup}>
          <img src="/SSL-icon.png" alt="SSL Icon" class="SSL-icon">
        </button>

        {#if SSLshow}
          <img src="/SSL-popup.jpeg" alt="SSL Popup" class="SSL-popup">
          <button class = "exit-SSL" on:click={showPopup}>
            <img src="/x-icon.png" alt="ssl-x" class="ssl-x">
          </button>
        {/if}
      </div> <!-- image-container -->
    </div><!-- content -->
  </div> <!-- main-column -->
</main>

<style>
  .info-row {
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: space-between;
    width: 100%;
    margin-bottom: 20px;
    margin-left: 20px;
  }

  .help-column {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    width: 150px;
    height: flex;
    margin-right: 5px;
    margin-left: 25px;
    color: #014c8a;
    border: 5px solid #e27610;
    font-weight: bold;
    border-radius: 10px;
    padding: 10px;
  }

  .tip-column {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    text-align: left;
    width: 220px;
    height: flex;
    margin-right: 20px;
    margin-top: 10px;
    background-color: rgb(255, 255, 255);
    color: #be771a;
    font-weight: bold;
    border-radius: 10px;
    padding: 8px;
    box-shadow: 3px 3px 2px rgba(255, 153, 0, 0.514); 
    transform: translateZ(-10px);
    position: relative;
    z-index: 1;
  .intro {
    height: flex;
    background-color: #fdfb71;
    padding: 20px;
    margin: 0;
    border-radius: 15px;
    border: 1px solid #d2ee8f;
    box-shadow: 5px 5px 15px rgba(0, 0, 0, 0.3); 
    transform: translateZ(10px); 
    text-align: left;
    font-size: 1.2em;
  }

  .donate-bait {
    background-color: #f7faff;
    width: 400px;
    height: 280px;
    margin-left: 20px;
    border-radius: 10px;
    border: 1px solid #c4c4c4;
    box-shadow: 5px 5px 2px rgba(0, 0, 0, 0.3); 
    transform: translateZ(20px); 
    padding: 12px;
  }

  .donate-amount {
    background-color: navy;
    border-radius: 10px;
    border: none;
    margin-top: 5px;
  }
  .submit-donation {
    display: flex;
    flex-direction: column;
    justify-content: left;
    align-items: left;
    text-align: left;
  }
    
  .scam-checklist {
    display: flex;
    flex-direction: column;
    margin-right: 20px;
    margin-top: 0;
    width: 280px;
    border-radius: 15px;
    position: absolute;
    z-index: 10;
    text-align: left;
    padding: 15px;
  }

  .scam-info {
    display: flex;
    flex-direction: column;
    margin-right: 18px;
    margin-top: 10px;
    top: 460px;
    width: 280px;
    border-radius: 15px;
    position: absolute;
    z-index: 10;
    text-align: left;
    padding: 15px;
    background-color: #f8e0d2;
    border: 1px solid #ffae51;
    color: #c70a03;
  }

  .fake-site-image {
    width: 900px;
    height: auto;
    margin-left: 40px;
    margin-right: -50px;
  }

  .highlight-ssl {
    position: absolute;
    border: 1px solid rgb(255, 102, 0);
    background-color: rgba(255, 115, 0, 0.2);
    top: 135px; 
    left: 419px; 
    width: 365px; 
    height: 90px;
    z-index: 1000;
  }

  .icon-button {
    position: absolute;
    top: 60px; 
    right: 718px;
    width: 30px;
    height: 20px;
    z-index: 1000;
    border: none;
    cursor: pointer;
    background:none;
    align-items: center;
    justify-content: center;
  }

  .icon-button:hover{
    background-color: #d3d3d3;
    border-radius: 30%;
    width: 2px;
    
  }

  .SSL-icon {
    width: 20px;
    height: 20px;
    margin-top: -8px;
    margin-left: -9px;
    
  }
  
  .SSL-popup {
    position: absolute;
    top: 90px;
    right: 440px;
    width: 375px;
    height: 275px;
    border-radius: 10px;
    background-color: #fdfcc2;
  }

  .exit-SSL {
    position: absolute;
    top: 80px;
    right: 430px;
    background: none;
    border: none;
    cursor: pointer;
    font-size: 1.5em;
  }

  .ssl-x {
    border: 1px solid rgb(238, 236, 236);
    width: 26x;
    height: 26px;
  }

  .back-button {
    background-color: #dff8fc;
    border: none;
    padding: 0;
    border-radius: 50%;
    cursor: pointer;
    position: absolute;
    left: -50px;
    top: 0px;
  }

  .go-back {
    width: 40px;
    height: flex;
  }

  .highlight-misspell {
    position: absolute;
    z-index: 1000;
    border: 1px solid rgb(255, 102, 0);
    background-color: rgba(255, 115, 0, 0.2);
    top: 67.8px; 
    left: 460px; 
    width: 720px; 
    height: 20px;
  }

  .image-container {
    position: relative;
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    margin-top: 20px;
  }
</style>