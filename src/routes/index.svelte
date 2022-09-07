<script lang="ts">
    import '../app.css';
    import Cabecalho from '$lib/components/Cabecalho.svelte';
    import MinhaLista from '$lib/components/MinhaLista.svelte';
    import Title from '$lib/components/Title.svelte';
    import Category from '$lib/components/Category.svelte';
    import categorias from '$lib/json/categorias.json';
    import Tag from '$lib/components/Tag.svelte';
    import Footer from '$lib/components/Footer.svelte';

    let minhaLista: string[] = [];

    function addIngrediente(evento: CustomEvent<string>) {
        const ingrediente = evento.detail; ///.detail pega o dado do segundo parametro do dispatch
        minhaLista = [...minhaLista, ingrediente];
    };

    function removeIngrediente(evento: CustomEvent<string>) {
        const ingrediente = evento.detail; 
        minhaLista = minhaLista.filter(
            (item) => item !== ingrediente
        );
    }
</script>

<svelte:head>
    <title>Svelte | Home</title>
</svelte:head>

<div class="container-principal">
    <Cabecalho />
    
    <div class="estilo-principal">
        {#if minhaLista.length} <!--Se não tiver nada da lista, da false e vice-versa-->
        <div class="minha-lista-container">
            <MinhaLista ingredientes={minhaLista}/>
            <div class="divisoria"></div>
        </div>
        {/if}

        <main>
            <Title tag='h1'>Ingredientes</Title>

            <div class="info">
                <p>Selecione abaixo os Ingredientes que você deseja usar nessa refeição:</p>
                <p>*Atenção: Consideramos que você tenha em casa: sal, pimenta e água.</p>
            </div>

            <ul class="categorias">
                {#each categorias as categoria (categoria.nome)}
                    <li>
                        <Category
                        {categoria}
                        on:adicionarIngrediente = {addIngrediente}
                        on:removerIngrediente = {removeIngrediente}/>
                    </li>
                {/each}
            </ul>

            <div class="buscar-receitas">
                <a href="/receitas">
                    <Tag ativa={true} tamanho='lg'>Buscar Receitas!</Tag>
                </a>
            </div>

        </main>
    </div>

    <Footer />
</div>

<style>
    .container-principal {
        display: flex;
        flex-direction: column;
        min-height: 100vh;
    }
 
    .estilo-principal {
        text-align: center;
        padding: 0 5vw 3.375rem;
        flex: 1;
    }
 
    .minha-lista-container {
        margin-bottom: 2rem;
    }

    .divisoria {
        width: 40vw;
        height: 2px;
        background-color: var(--verde);
        margin: 0 auto;
    }
 
    .info {
        margin-bottom: 3.375rem;
    }
 
    .info > p {
        line-height: 2rem;
    }

    .categorias {
        margin-bottom: 4.6875rem;
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        gap: 1.5rem;
    }

    .buscar-receitas {
        display: flex;
        justify-content: center;
    }
</style>