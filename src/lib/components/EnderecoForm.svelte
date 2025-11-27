<script>
  export let endereco = {
    cep: "",
    rua: "",
    bairro: "",
    cidade: "",
    uf: "",
    numero: "",
    complemento: ""
  };

  export let onChange = () => {};

  let loadingCep = false;

  function maskCep(value) {
    return value
      .replace(/\D/g, "")
      .replace(/(\d{5})(\d)/, "$1-$2")
      .slice(0, 9);
  }

  async function buscarCep() {
    const cepLimpo = endereco.cep.replace(/\D/g, "");

    if (cepLimpo.length !== 8) return;

    loadingCep = true;

    try {
      const resp = await fetch(
        `https://brasilapi.com.br/api/cep/v2/${cepLimpo}`
    );

      if (!resp.ok) throw new Error("CEP não encontrado");

      const dados = await resp.json();

        endereco.rua = dados.street || "";
        endereco.bairro = dados.neighborhood || "";
        endereco.cidade = dados.city || "";
        endereco.uf = dados.state || "";

      onChange(endereco);
    } catch (e) {
      console.error(e);
      alert("CEP não encontrado");
    } finally {
      loadingCep = false;
    }
  }

  function update() {
    onChange(endereco);
  }
</script>

<style>
  .grupo {
    margin: 20px 0;
    padding: 20px 28px;
    border-radius: 10px;
    background: #fafafa;
    border: 1px solid #ddd;
  }

  .grupo h3 {
    margin: 0 0 12px 0;
    font-size: 1rem;
    font-weight: 600;
  }

  .campo {
    margin-bottom: 12px;
  }

  label {
    display: block;
    margin-bottom: 4px;
    font-size: 0.9rem;
  }

  input {
    width: 100%;
    padding: 8px;
    border-radius: 6px;
    border: 1px solid #ccc;
    font-size: 0.9rem;
  }

  .loading {
    position: absolute;
    right: 10px;
    top: 8px;
    font-size: 0.8rem;
    color: #555;
    animation: pulse 1s infinite;
  }

  @keyframes pulse {
    0% { opacity: .4; }
    50% { opacity: 1; }
    100% { opacity: .4; }
  }

  .disabled {
    opacity: 0.5;
    pointer-events: none;
  }
</style>

<div class="grupo">
  <h3>Endereço</h3>

  <div class="campo">
    <label>CEP</label>
    <div style="position: relative;">
      <input
        type="text"
        bind:value={endereco.cep}
        on:input={(e) => {
          endereco.cep = maskCep(e.target.value);
          update();
        }}
        on:blur={buscarCep}
        placeholder="00000-000"
      />
      {#if loadingCep}
        <span class="loading">Aguarde…</span>
      {/if}
    </div>
  </div>

  <div class={loadingCep ? "disabled" : ""}>
    <div class="campo">
      <label>Rua</label>
      <input type="text" bind:value={endereco.rua} on:input={update} />
    </div>

    <div class="campo">
      <label>Bairro</label>
      <input type="text" bind:value={endereco.bairro} on:input={update} />
    </div>

    <div class="campo">
      <label>Cidade</label>
      <input type="text" bind:value={endereco.cidade} on:input={update} />
    </div>

    <div class="campo">
        <label>UF</label>
        <select
            bind:value={endereco.uf}
            on:change={update}
            disabled={loadingCep}
        >
            <option value="">Selecione</option>
            <option value="AC">AC</option>
            <option value="AL">AL</option>
            <option value="AP">AP</option>
            <option value="AM">AM</option>
            <option value="BA">BA</option>
            <option value="CE">CE</option>
            <option value="DF">DF</option>
            <option value="ES">ES</option>
            <option value="GO">GO</option>
            <option value="MA">MA</option>
            <option value="MT">MT</option>
            <option value="MS">MS</option>
            <option value="MG">MG</option>
            <option value="PA">PA</option>
            <option value="PB">PB</option>
            <option value="PR">PR</option>
            <option value="PE">PE</option>
            <option value="PI">PI</option>
            <option value="RJ">RJ</option>
            <option value="RN">RN</option>
            <option value="RS">RS</option>
            <option value="RO">RO</option>
            <option value="RR">RR</option>
            <option value="SC">SC</option>
            <option value="SP">SP</option>
            <option value="SE">SE</option>
            <option value="TO">TO</option>
        </select>
        </div>


    <div class="campo">
      <label>Número</label>
      <input type="text" bind:value={endereco.numero} on:input={update} />
    </div>

    <div class="campo">
      <label>Complemento</label>
      <input type="text" bind:value={endereco.complemento} on:input={update} />
    </div>
  </div>
</div>
