<script>
  let email = "";
  let senha = "";
  let carregando = false;
  let erro = "";

  async function login() {
    erro = "";
    carregando = true;

    try {
      const resp = await fetch("http://localhost:8080/api/login", {
        method: "POST",
        headers: { "Content-Type": "application/json" },
        body: JSON.stringify({ email, senha })
      });

      if (!resp.ok) {
        erro = "Usuário ou senha inválidos";
        return;
      }

      window.location.href = "/home";

    } catch (e) {
      erro = "Falha ao conectar ao servidor";
    } finally {
      carregando = false;
    }
  }
</script>

<div class="bg">
  <div class="card">
    <h1>Gódas Kids</h1>
    <p class="sub">Acesso ao sistema</p>

    <input type="email" placeholder="E-mail" bind:value={email} />
    <input type="password" placeholder="Senha" bind:value={senha} />

    {#if erro}
      <div class="erro">{erro}</div>
    {/if}

    <button on:click={login} disabled={carregando}>
      {carregando ? "Entrando..." : "Entrar"}
    </button>
  </div>
</div>

<style>
  /* Fundo com degradê inspirado no Instagram */
  .bg {
    width: 100%;
    height: 100vh;
    background: linear-gradient(135deg,
      #6A0DAD,
      #FF2EA6,
      #FFD600
    );
    display: flex;
    align-items: center;
    justify-content: center;
    animation: grad 12s ease infinite;
    background-size: 200% 200%;
  }

  @keyframes grad {
    0% { background-position: 0% 50%; }
    50% { background-position: 100% 50%; }
    100% { background-position: 0% 50%; }
  }

  .card {
    width: 350px;
    background: white;
    padding: 2rem;
    border-radius: 20px;
    box-shadow: 0 10px 30px rgba(0,0,0,0.2);
    display: flex;
    flex-direction: column;
    gap: 1rem;
  }

  h1 {
    margin: 0;
    text-align: center;
    font-size: 2rem;
    font-weight: 800;
    color: #6A0DAD;
  }

  .sub {
    text-align: center;
    margin-bottom: 1rem;
    color: #555;
  }

  input {
    padding: 12px;
    border-radius: 10px;
    border: 2px solid #eee;
    font-size: 1rem;
  }

  input:focus {
    border-color: #FF2EA6;
    outline: none;
  }

  button {
    padding: 12px;
    border: none;
    background: #6A0DAD;
    color: white;
    font-size: 1.1rem;
    font-weight: 600;
    border-radius: 10px;
    cursor: pointer;
    transition: 0.2s;
  }

  button:hover {
    background: #FF2EA6;
  }

  .erro {
    background: #ffd4d4;
    color: #b10000;
    padding: 0.5rem;
    text-align: center;
    border-radius: 10px;
  }
</style>
