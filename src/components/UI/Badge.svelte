<script>
  import { slide } from 'svelte/transition';
</script>

<span transition:slide>
  <slot />
</span>

<style>
  span {
    display: inline-block;
    margin: 0 0.25rem;
    border-radius: 3px;
    border: 1px solid #001021;
    background: #001021;
    color: #fff;
    padding: 0 0.5rem;
    font-size: 0.8rem;
  }
</style>
