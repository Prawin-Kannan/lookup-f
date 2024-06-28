<script>
    import Leads from './Leads.svelte';
    import Employees from './Employees.svelte';
</script>

<style>
    main {
        padding: 1rem;
        max-width: 1200px;
        margin: 0 auto;
    }
</style>

<main>
    <h1>Lookup Data Store</h1>
    <Leads />
    <Employees />
</main>
