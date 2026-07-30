<script lang="ts">
  interface IAppState {
    value: number;
    mode: "mg" | "ml";
    dosage: number;
  }

  let app_state: IAppState = $state({
    mode: "mg",
    value: 7.2,
    dosage: 40,
  });

  function calc(mode: "mg" | "ml") {
    return mode === "mg"
      ? app_state.value / app_state.dosage
      : Math.ceil(app_state.dosage * app_state.value * 10) / 10;
  }

  let result: number = $derived(calc(app_state.mode));
  let mg: number = $derived(
    app_state.mode !== "ml" ? calc("ml") / app_state.dosage : result,
  );

  function click(e: MouseEvent) {
    e.preventDefault();
    app_state.value = calc(app_state.mode);
    app_state.mode = app_state.mode === "mg" ? "ml" : "mg";
  }
</script>

<main>
  <h1>Injection Calculator</h1>

  <p class="note">
    Note: 40mg/mL is the <a href="https://www.astrovials.com">AstroVials</a>
    default, please consult your vial for dosage.<br />
    Most syringes go up to 1mL, but your results may vary based on the syringe you
    are using.
  </p>

  <form>
    <label for="dosage">
      Dosage (mg/mL)
      <input id="dosage" bind:value={app_state.dosage} type="text" />
    </label>

    <label for="mg">
      {#if app_state.mode === "ml"}
        Desired dose (mL)
        <input id="ml" bind:value={app_state.value} type="text" />
      {:else}
        Desired dose (mg)
        <input id="mg" bind:value={app_state.value} type="text" />
      {/if}
    </label>

    <button class="result" onclick={click}>
      Result: {#if app_state.mode === "mg"}
        <strong>{result} mL</strong>
      {:else}
        <strong>{result} mg</strong>
      {/if}
    </button>
  </form>

  <div class="links">
    <p>
      You can use the Amazon links below to purchase syringes that work with
      this calculator:
    </p>
    <a
      href="https://www.amazon.com/1ml-Syringe-Sterile-Luer-Slip/dp/B07BQDRDC2/"
      target="_blank"
      rel="noopener noreferrer"
    >
      BH Supplies 1mL Luer Slip Tip Syringes (No Needle) - Sterile, Individually
      Wrapped - 100 Syringes
    </a>
    <a
      href="https://www.amazon.com/dp/B0CYT4Q78C"
      target="_blank"
      rel="noopener noreferrer"
    >
      100 Pcs 1.5Inch 21Ga Needle (for drawing, not for injecting DO NOT INJECT
      WITH THIS)
    </a>
    <a
      href="https://www.amazon.com/dp/B09XPTJMF8"
      target="_blank"
      rel="noopener noreferrer"
    >
      100 Pcs 1.0Inch 25G Needle (For injecting, particularly IM - SubQ is not
      recommended with these)
    </a>
    <p class="affiliate">
      Please note, these are affiliate links, and we may earn a small commission
      if you purchase through them at no additional cost to you.
    </p>
  </div>

  <footer>
    Reference the simulator for how many days after you may need to inject based
    on your dosage:<br />
    <a
      href="https://sim.transfemscience.org/?e=ec_o&d={mg}&xm=20"
      target="_blank"
      rel="noopener noreferrer">Estrogen Simulator ({mg} mg)</a
    >
  </footer>
</main>
