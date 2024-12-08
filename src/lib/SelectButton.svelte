<script>
    import { createEventDispatcher } from 'svelte';
    import { slide } from 'svelte/transition';
  
    export let buttonColor = '';
    export let isSelected = false;
    export let onClick;
  
    const dispatch = createEventDispatcher();
  
    function handleClick() {
      dispatch('slide', { isSelected });
      onClick();
    }
  </script>
  
  <button class:selected={isSelected} on:click={handleClick} style="background-color: {buttonColor};">
    <slot></slot>
  </button>
  
  <style>
    button {
      border: 1px solid #1b0f8b;
      font-size: 1em;
      font-weight: 500;
      padding: 0.6em 1.2em;
      cursor: pointer;
      transition: background-color 0.25s, box-shadow 0.25s, transform 0.25s;
      outline: none;
      color: #8d290a;
      font-weight: 600;
      width: 100%;
      text-align: left;
      border-radius: 5px;
      box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1), inset 0 1px 0 rgba(255, 255, 255, 0.2);
    }
    button.selected {
      background-color: #bdb4a0 !important;
      color: white;
    }
    button:hover {
      box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15), inset 0 1px 0 rgba(255, 255, 255, 0.2);
      transform: translateY(-2px);
    }
    button:active {
      box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2), inset 0 1px 0 rgba(255, 255, 255, 0.2);
      transform: translateY(1px);
    }
  </style>