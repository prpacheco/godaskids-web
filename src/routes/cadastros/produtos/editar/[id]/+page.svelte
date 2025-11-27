<script>
  import ProdutoForm from "$lib/components/ProdutoForm.svelte";
  import { page } from '$app/stores';

  let produto = {};

  $: id = $page.params.id;

  // carrega no onMount
  import { onMount } from "svelte";

  onMount(async () => {
    const resp = await fetch("http://localhost:8080/produtos/" + id);
    produto = await resp.json();
  });
</script>

<ProdutoForm modo="editar" produto={produto} />
