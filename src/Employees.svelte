<script>
    import axios from 'axios';
    import { onMount } from 'svelte';

    let employees = [];
    let filterName = '';
    let filterRole = '';
    let filterSeniority = '';

    async function fetchEmployees() {
        try {
            const response = await axios.get('http://localhost:8000/api/employees/');
            employees = response.data.map(employee => ({
                ...employee,
                // Handle 'Ndarray encountered' in raw_json
                raw_json: employee.raw_json === 'Ndarray encountered' ? 'Data unavailable' : employee.raw_json
            }));
        } catch (error) {
            console.error('Error fetching employees:', error);
        }
    }

    async function filterEmployees() {
        try {
            const response = await axios.post('http://localhost:8000/api/filter-employees/', {
                name: filterName,
                role: filterRole,
                seniority: filterSeniority,
            });
            employees = response.data.map(employee => ({
                ...employee,
                // Handle 'Ndarray encountered' in raw_json
                raw_json: employee.raw_json === 'Ndarray encountered' ? 'Data unavailable' : employee.raw_json
            }));
        } catch (error) {
            console.error('Error filtering employees:', error);
        }
    }

    async function exportEmployees() {
        try {
            window.location.href = 'http://localhost:8000/api/export-employees/';
        } catch (error) {
            console.error('Error exporting employees:', error);
        }
    }

    onMount(fetchEmployees);
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

<h2>Employees</h2>

<div>
    <input placeholder="Name" bind:value={filterName} />
    <input placeholder="Role" bind:value={filterRole} />
    <input placeholder="Seniority" bind:value={filterSeniority} />
    <button on:click={filterEmployees}>Filter</button>
    <button on:click={exportEmployees}>Export</button>
</div>

<table>
    <thead>
        <tr>
            <th>First Name</th>
            <th>Last Name</th>
            <th>Role</th>
            <th>Seniority</th>
            <th>Ranking</th>
            <th>Selected Status</th>
            <th>Raw JSON</th>
        </tr>
    </thead>
    <tbody>
        {#each employees as employee}
            <tr>
                <td>{employee.first_name}</td>
                <td>{employee.last_name}</td>
                <td>{employee.role}</td>
                <td>{employee.seniority}</td>
                <td>{employee.ranking}</td>
                <td>{employee.selected_status}</td>
                <td>{employee.raw_json}</td>
            </tr>
        {/each}
    </tbody>
</table>
