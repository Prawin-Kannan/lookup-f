<script>
    import axios from 'axios';
    import { onMount } from 'svelte';

    let leads = [];
    let filterCompanyName = '';
    let filterNumberOfEmployees = '';

    async function fetchLeads() {
        const response = await axios.get('http://localhost:8000/api/leads/');
        leads = response.data;
    }

    async function filterLeads() {
        const response = await axios.post('http://localhost:8000/api/filter-leads/', {
            company_name: filterCompanyName,
            number_of_employees: filterNumberOfEmployees ? parseInt(filterNumberOfEmployees) : null,
        });
        leads = response.data;
    }

    async function exportLeads() {
        window.location.href = 'http://localhost:8000/api/export-leads/';
    }

    onMount(fetchLeads);
</script>

<style>
    table {
        width: 100%;
        border-collapse: collapse;
    }

    th, td {
        border: 1px solid black;
        padding: 8px;
        text-align: left;
    }

    th {
        background-color: #f2f2f2;
    }
</style>

<h2>Leads</h2>

<div>
    <input placeholder="Company Name" bind:value={filterCompanyName} />
    <input placeholder="Number of Employees" bind:value={filterNumberOfEmployees} />
    <button on:click={filterLeads}>Filter</button>
    <button on:click={exportLeads}>Export</button>
</div>

<table>
    <thead>
        <tr>
            <th>Company Name</th>
            <th>Number of Employees</th>
            <th>Raw JSON</th>
        </tr>
    </thead>
    <tbody>
        {#each leads as lead}
            <tr>
                <td>{lead.company_name}</td>
                <td>{lead.number_of_employees}</td>
                <td>{lead.raw_json}</td>
            </tr>
        {/each}
    </tbody>
</table>
