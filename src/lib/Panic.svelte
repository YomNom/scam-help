<script>
    import { onMount } from 'svelte';
  
    let images = [];
  
    onMount(() => {
      // Add 5 copies of the same image to the images array
      for (let i = 0; i < 5; i++) {
        images.push('/path/to/your/image.png');
      }
    });
  </script>
  
  <style>
    .image-pile {
      position: relative;
      width: 200px; /* Adjust as needed */
      height: 200px; /* Adjust as needed */
    }
  
    .image-pile img {
      position: absolute;
      top: 0;
      left: 0;
      width: 100%;
      height: 100%;
      transform: translate(0, 0);
      animation: pile 0.5s ease-in-out forwards;
    }
  
    @keyframes pile {
      0% {
        transform: translate(0, 0);
      }
      100% {
        transform: translate(10px, 10px); /* Adjust to create the pile effect */
      }
    }
  
    .image-pile img:nth-child(2) {
      animation-delay: 0.1s;
    }
  
    .image-pile img:nth-child(3) {
      animation-delay: 0.2s;
    }
  
    .image-pile img:nth-child(4) {
      animation-delay: 0.3s;
    }
  
    .image-pile img:nth-child(5) {
      animation-delay: 0.4s;
    }
  </style>
  
  <div class="image-pile">
    {#each images as image, index}
      <img src={image} alt="Piled Image {index + 1}">
    {/each}
  </div>