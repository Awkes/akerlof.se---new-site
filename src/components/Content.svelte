<script>
  import { onMount, onDestroy } from "svelte";
  import { fade } from "svelte/transition";
  import Logo from "./Logo.svelte";
  import Menu from "./Menu.svelte";

  let header;
  let main;

  function setHeaderBg() {
    const headerHeight = header.scrollHeight;
    const mainPadding = parseInt(
      window.getComputedStyle(main).getPropertyValue("padding-top")
    );
    if (window.pageYOffset >= mainPadding - headerHeight) {
      header.style.boxShadow = "0px 10px 10px #000";
      header.style.backgroundColor = "#000";
    } else {
      header.style.boxShadow = "";
      header.style.backgroundColor = "";
    }
  }

  onMount(() => {
    window.addEventListener("scroll", setHeaderBg);
    setHeaderBg();
  });

  onDestroy(
    () =>
      typeof window !== "undefined" &&
      window.removeEventListener("scroll", setHeaderBg)
  );
</script>

<style>
  .modal {
    position: absolute;
    top: 0;
    min-height: 100vh;
    width: 100%;
    display: flex;
    justify-content: space-around;
    align-items: center;
    background-color: rgba(0, 0, 0, 0.8);
  }

  .header-container {
    position: fixed;
    top: 0;
    width: 100%;
    margin: 0 -5px;
    transition: ease-in-out 0.1s;
    z-index: 200;
  }

  header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 20px 20px 10px 20px;
    margin: 0 auto;
  }

  header,
  main {
    width: 100%;
    max-width: var(--max-width);
  }

  main {
    padding: 120px 20px;
  }

  .logo {
    display: block;
    min-width: 35px;
    width: 50px;
    margin-right: 20px;
  }

  @media (min-width: 400px) and (min-height: 400px) {
    main {
      padding: 150px 50px;
    }
    .logo {
      width: 75px;
    }
  }

  @media (min-width: 600px) and (min-height: 600px) {
    main {
      padding: 170px 100px;
    }
    .logo {
      width: 100px;
    }
  }
</style>

<div class="modal" transition:fade>
  <div class="header-container" bind:this={header}>
    <header>
      <a href="/" class="logo">
        <Logo color="var(--primary)" />
      </a>
      <div class="menu">
        <Menu />
      </div>
    </header>
  </div>
  <main bind:this={main}>
    <slot />
  </main>
</div>
