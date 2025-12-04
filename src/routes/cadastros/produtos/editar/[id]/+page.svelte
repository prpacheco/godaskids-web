<script>
  import ProdutoForm from "$lib/components/ProdutoForm.svelte";
  import { page } from '$app/stores';

  console.log("ID recebido:", $page.params.id);


  let produto = {};

  $: id = $page.params.id;

  // carrega no onMount
  import { onMount } from "svelte";

  onMount(async () => {
    if (!id) return;

    const resp = await fetch(`http://localhost:8080/produtos/id?id=${id}`);
    produto = await resp.json();
  });

</script>

<ProdutoForm modo="editar" produto={produto} />
